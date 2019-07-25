---
layout: default
status: publish
published: true
title: Using Twitter @Anywhere – An introduction
author: Michael Brunton-Spall
author_login: bruntonspall
author_email: michael@brunton-spall.co.uk
author_url: http://www.brunton-spall.co.uk
wordpress_id: 58
wordpress_url: http://blog.brunton-spall.co.uk/2010/04/using-twitter-anywhere-introduction/
date: '2010-04-14 22:40:54 +0000'
date_gmt: '2010-04-14 22:40:54 +0000'
categories:
- technical
- featured
tags:
- twitter
- ! '@anywhere'
- tutorial
- coding
comments: true
---
<em>Note: This post was written when this blog was hosted on a custom written blog engine.  I've since moved back to wordpress so some details refering to this site may no longer be accurate. - MBS</em>

I've been lucky enough over the last couple of weeks to have early access to the twitter <a href="http://dev.twitter.com">anywhere</a> platform and with the eminent <a href="http://jaggeree.com">Chris Thorpe</a> (@jaggeree) build a few prototype that we could use with twitter anywhere.

I thought it might be a nice start to write a simple blog post detailing an introduction to the first few features of twitter anywhere.

<!--more-->
Twitter anywhere is a service, provided by twitter that allows you to get access to twitter details in the webpage.  Written in Javascript, and hosted via IFrame's in your site, it allows frictionless access to twitter.

## Getting started
First you need to sign up at http://dev.twitter.com/anywhere/apps/new and create a twitter client application.

In your website code you make a javascript call to initialise the @anywhere library.  I'm using jquery here to include the script and make a callback once it has rendered.  This ensures that the loading of this javascript library does not hold up the document rendering.

<pre>jQuery(function($) {</pre>
<pre>	$.getScript('http://platform.twitter.com/anywhere.js?id=KEY&amp;v=VERSION', function() {</pre>
<pre>	/* Your Code */</pre>
<pre>	});</pre>
<pre>});</pre>
At this point you need to provide a key to the anywhere javascript.  This key is the key that you were assigned when you signed up to write an @anywhere application using the twitter API.  you also need to specify the version, for example 1.

Once you have loaded the twitter anywhere library you can initialise it. Importing the twitter library will create a global javascript variable called twttr. The recommended method to initialise the library is to call the anywhere constructor.

<pre>twttr.anywhere(function(twitter) {	});</pre>
The anywhere library acts a lot like jQuery, so your function is passed a reference to the twitter variable, allowing you to name it anything you want, but twitter is probably the best name for easiest code reading.

## Tweet from your website
Once initialised the twitter library allows you a few commands.  The simplest is that you want to allow people to post a tweet automatically.  Here is a simple version that does that:

<pre>twitter('#tweetbox').tweetBox({</pre>
<pre>	defaultContent: 'Just read a great article by @bruntonspall at '+window.location,</pre>
<pre>	height: 100,</pre>
<pre>	width: 250,</pre>
<pre>	label: 'Tweet how awesome @bruntonspall is'</pre>
<pre>});</pre>
It's really that simple.  The text in the twitter box is a standard jquery selector, and of course the tweetbox has a number of defaults, I've only overridden a couple here.  You can checkout the API docs to see the other options.

The important bit about this is that it is a frictionless design.  In the past if you wanted to add this kind of tweet this functionality, you had to either run your own backend that did OAuth and users would have to authenticate your application, or you had to create a link that went offsite, direct to twitters website.  For a small blog like mine, that is not a big deal, but for most larger organisations, it's a big deal.

The first time a user clicks the tweet button, a popup will be created to authorize your website to post to their tweet stream.  If the webpage that the user came from does not have the same document domain as the OAuth callback url that you specified then you will get an OAuth failure and the tweet will fail.  That can be a bit of a pain for testing, but you could create a second app, and provide your test environment with a seperate key to get around that.

Once a user has authorised your website once, they will never need to authorise it again unless they explicitly deauthorise your website first.  That means that once a user has authorised you, tweets become a single click, that does not navigate away from the page.  nice huh?  That's what twitter means when it says frictionless tweeting.

## The follow button
The next feature is the follow button.  For my website I might want to put a follow button on that allows you to follow me.  That's as easy as you would expect, simply find the div and call the followButton method, like so:

<pre>twitter('#followbutton').followButton('@bruntonspall');</pre>
This inserts a follow button into the element you specify.  Again if the user has not authenticated it will ask for authorisation, but once authorised it's a single click frictionless follow for your website.

If your blog had multiple authors, you could easily pull the authors twitter name into the follow button to ensure that you can follow the author, or you could hardcode it to your site wide news feed twitter account.

## Hovercards and Linkify
I've left this feature late because I'm not a huge fan of the hovercards.  You've seen these on the twitter site, if you hover your mouse pointer over a tweeters name, you get a little card pop up with some info about the tweeter.  Clicking on the more button gives you even more information, and you can follow and deal with lists inline.  Linkify is even simpler, it finds @bruntonspall in the text and wraps it with an A tag to link it to the relevant twitter page.

You can enable twitter hovercards by calling the hovercards method.  It scans through the text on your page and translates @bruntonspall style twitter names that it finds.  You probably don't want it to parse your entire page though, so I'd recomend using it jQuery style:

<pre>twitter('.content').hovercards();</pre>
Which will only do the hovercard parse on the text in elements with the class 'content'.

There is something to note, both hovercards and linkifyusers do not parse text inside A, PRE, IFRAME, SCRIPT or STYLE tags by default.

It's also worth noting that calling hovercards will implicitly call linkifyUsers, so don't bother linkifying if you are also going to enable the hovercards.

What if you want to enable a hovercard for something non-text?  An example is that in our prototype we wanted to hovercard twitter avatars.  You can pass a function to the hovercards method as the username parameter which is called on each element to find out if it has a username.  Simply return a username as a string and it will hovercard it correctly.  If you pass the username parameter hovercards wont linkify, so you need to do that by hand.  Our example might be

<pre>&lt;img src='...' id='bruntonspall&gt;&lt;img src='...' id='icklecat'&gt;</pre>
<pre>twitter('img').hovercards({</pre>
<pre>	username: function(e){</pre>
<pre>	return e.id;</pre>
<pre>	}</pre>
<pre>});</pre>
The final bit is the most exciting, twitter anywhere provides you with access to the twitter account details if the user is signed into twitter and has authorised your website.  So once somebody has tweeted via your website, you can use those details for customisation or authentication purposes.  I'm sure that you can think of plenty of options here, but I've not had a chance to get them working, so you'll have to expect a follow up post once all the final details are posted

I think that's enough for a brief overview, you can checkout the official docs for more and better information, and I hope you enjoy playing with twitter Anywhere.  My experience so far has shown it to be a rather beautiful API to work with.

As you can see I've updated my site to use these features too, so feel free to tweet me using the box on the right, and checkout my twitter hovercards and information in the article.

