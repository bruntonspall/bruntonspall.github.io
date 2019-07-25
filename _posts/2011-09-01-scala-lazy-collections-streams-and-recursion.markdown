---
layout: default
status: publish
published: true
title: Scala, lazy collections, streams and recursion
author: Michael Brunton-Spall
author_login: bruntonspall
author_email: michael@brunton-spall.co.uk
author_url: http://www.brunton-spall.co.uk
wordpress_id: 912
wordpress_url: http://www.brunton-spall.co.uk/?p=912
date: '2011-09-01 17:08:22 +0000'
date_gmt: '2011-09-01 17:08:22 +0000'
categories:
- technical
tags:
- scala
- learning
comments: true
---
I'm currently rewriting the deployment system at the guardian in Scala, and although I'd say I know Scala, I'm learning lots of things as we go.  I'm lucky enough to be pairing with <a href="http://blog.tackley.net/" target="_blank">Graham Tackley</a>, our platform team lead and someone who knows Scala far better than I do, and this means that we often write a bit of code, then go back and improve it and so forth.

The bit of Scala code I'm particularly proud of is around waiting for a port to be opened.  During our deployment scripts we restart our application servers, then we want the scripts to wait until the server is up and able to serve requests before we move on.

<!--more-->

Writing this kind of code is not terribly difficult, but testing that it works can be very tricky.  I decided to take the blackbox kind of approach to this, and my test spins up a second thread that listens on the port in question so that we can execute the real code that attempts to connect to the port.  In java or most other languages this would be something fairly long winded, but in my Scala test it looks like this:

<script type="text/javascript" src="https://gist.github.com/1187013.js?file=spawn_test.scala"></script>The spawn function is provided by concurrent.ops and is built into Scala and represents a throwaway thread that we aren't terribly interested in.  We don't want to join on it, we just want it to start up and execute. The code inside creates a new java.net.ServerSocket, calls the blocking accept method which blocks until it gets a connection, closes the connection and then closes the original listening connection. We can then create and execute our port connection code and it should validly connect to the port 9998 on localhost and work beautifully.

The next test needed to be the failure case, if the waitForPort task cannot connect to a port after a specified timeout it should error, preferably by throwing an exception of some form. Testing this was also fairly easy, I wrote the traditional try { blah(); fail()} catch... stuff, then checked with Graham if there was a better way, and of course there was:<script type="text/javascript" src="https://gist.github.com/1187013.js?file=test_evaluating.scala"></script>

ScalaTest with the ShouldMatchers makes for a simply beautiful test that is clear to read and was easy to write.

However when it came to implementing the method, I struggled a little bit.  I've never learnt functional programming, so closures and maps and so forth are still a bit alien to me, so I ended up writing some very java style code:

<script type="text/javascript" src="https://gist.github.com/1187013.js?file=javaish.scala"></script>The isSocketOpen simply tries to open a socket and catches any exceptions to return false, and true if it opened successfully. Having written this, knowing there was a better solution, I asked again, and this prompted Phil Wills to upload his blogpost about the <a href="http://blog.phil-wills.com/streams-of-pleasure" target="_blank">Stream class and lazy evaluation</a>. I won't cover the details here, but essentially using the return statement in Scala is frowned upon because of the way it works, and it's more idiomatic Scala to use Options and flatMap, here's the updated code:<script type="text/javascript" src="https://gist.github.com/1187013.js?file=range.scala"></script>

Here we've changed isSocketOpen to checkSocketOpen and made it return an option.  At this point I also made checkSocketOpen do the sleep, which we could have done before, but is necessary now.

To explain, this code takes the range 1 to 10, which is a Seq of ints (1,2,3...) and we flatMap across it with a function that returns an option.  If you haven't seen this before, here's a quick overview of what the flatMap actually does:

<p style="padding-left: 30px;">map applies a function on each item in a collection and returns a transformed collection, essentially [a,b].map(f) is the same as calling [f(a), f(b)].

<p style="padding-left: 30px;">If our function f returns an Option, this is either Some(true) or None.  An Option can really be thought of as a sequence that either contains 1 item or is empty, so Some(true) is the same as [true] and None is the same as [].

<p style="padding-left: 30px;">This means our assuming that our function f returns None for everything but c, the map on [a, b, c, d] will return something like [ [], [], [true], [] ].  That's often not a lot of use for us, and what we are interested in is stuff that returned something.

<p style="padding-left: 30px;">flatMap is a function that essentially flattens the returned sequences catenated together, so if f(x) = [1,2] and f(y) = [4,5], then [x, y] map f == [ [1,2], [4,5] ] but [x, y] flatMap f == [ 1, 2, 3, 4 ].  This also has the handy shortcut that [] flattens into nothing, so essentially gets skipped, so our flatMap on [a, b, c, d] should return [true].

We then use headOption, since it's possible that we returned either [true], [true, true...] or [].  head would return us the first true, but on an empty list would die horribly, headOption returns an option that is Some(h) where the list is [h, t] and None where the list is [].  We then match on the option and throw an error if we got None (and therefore not a single call returned a true).

Our function checkSocketOpen has a side effect of pausing for an amount of time though, and since flatMap executes the function once for each item in the original list, this means that we will always pause for the entire connection timeout no matter whether we successfully connect the first time or not. (kind of, actually if we only pause on a failed connection, and if we successfully connect each time, we wont pause, but we will create 10 connections rather than just the one we need to confirm the port is open).

Phil in his blog has a very simple fix for this, which is to wrap our original sequence in a Stream class.  Stream is another Scala provided class, and in this case it is a sequence that is lazily evaluated.  Essentially this means that the flatMap doesn't call the checkSocketOpen on all items immediately but instead only calls it when you try to get to the first item.  This means that headOption executes each function one after another until one of them returns true or it gets to the end of the sequence, it then returns the option based on what happened.

This means that if the first call returned none, it moved on and called f again over and over until it returned true, then the headOption stopped and never called the remaining calls, making this system stop as soon as the first connection was made.

Success! We've removed the return statement and made the code significantly harder to read and understand. :-(  I've just taken several hundred words explaining why it works and I'm still not entirely clear on the details.

So we started wondering if there was an alternative way of doing this, something that would still be idiomatic Scala but that would be obvious to junior or learning Scala programmers like myself. Graham and I wondered if what a recursive solution would look like, so we came up with this code:

<script src="https://gist.github.com/1187013.js?file=recursive.scala"></script>&nbsp;

We decided very early on to inline the checkSocketOpen function as recursive itself, so although this looks like more lines of code, it now combines both functions.

This function, when called at the bottom with checkOpen(0) initialises currentTry to 0, then tries to open a socket, if it is successful it just returns, if attempting to open a socket throws an exception it sleeps for a bit, then calls checkOpen with currentTry incremented by one.

All recursive functions have to have a guard, which is to say some statement that prevents infinite recursion (unless that's what you want anyway), this function checks to find out if currentTry is greater than MAX<em>CONNECTION</em>ATTEMPTS and if it is, sys.error throws an exception which bubbles all the way back up through 10 copies of checkOpen before exiting properly.

If you know that your range is bounded fairly low, (such as 10 retry attempts) this is a perfectly acceptable way of writing this function.  We suspect because of the way we've written the try catch that it's not possible to make this function tail recursive, which would effectively allow infinite (or lots) recursive calls, but I'm sure it's possible to adjust the try catch into a subfunction that might make it tail recursive if you really wanted to.

&nbsp;

So there you go, that's been a day of Scala learning for me, I now know the issues with the return statement, the Stream classes which might be useful later, and I've written my first truly recursive program in anger.  I'm feeling a little bit proud.

Of course there are improvements to make, making the function tail recursive would be education as I'm aware of tail recursion but am not entirely sure how to actually make a function tail recursive.  I'd also love to make that ServerSocket stuff into one line again, originally I had ServerSocket(port).accept().close() as one line, but I discovered in testing the stream stuff that the ServerSocket hangs around and keeps accepting connections if you don't close it, which I can't do in a single line without keeping a reference to it.

If you are interested in our new deploy system Magenta and the upcoming RifRaf web application for it, then I suggest you keep an eye out on our <a href="http://github.com/guardian" target="_blank">github repository</a> and feel free to send patches, advice and help our way.

