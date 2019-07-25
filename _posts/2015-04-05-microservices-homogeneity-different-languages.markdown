---
layout: default
title: "Should I allow different languages or runtimes in my organisation?"
date: 2015-04-05 00:09:00 +0100
comments: true
featured:
  preview: /images/uploads/networking-300x199.jpg
  large: /images/uploads/networking.jpg
  caption: Networking by jairoagua
  url: https://www.flickr.com/photos/31065898@N08/8220970905

categories:
 - technical
 - featured
---

One of the much vaunted benefits of microservices is the claim of heterogeneous development environments.  Because we agree that microservices should interact via well known or standardised protocols (like HTTP, Thrift, RPC), it means that different microservices can be written in completely different technology stacks.

This often seems to scare technology managers, heads of engineering and operations teams.  If we diversify our technology stack it will make people rotation harder, it makes operating the services harder as there is less uniformity, and it means that monitoring, alerting, diagnostic tools and the various other tools and processes that the organisation has built up over time wont work any more.

I want to tell you about an experience I had at the Guardian, and how having a homogeneous environment hurt us as a development team and how moving towards heterogeneous environments improved the team culture and technology options.
<!-- more -->

##  Moving back in time
A number of years ago, the Guardian had a problem.  We had successfully built a large new modern content management system, and it was doing exactly what we had intended, it was attracting more readers from around the web to some of our best content.

We had built the system using the most modern agile techniques, and at the time a very modern Java, hibernate, spring based system.  It was pretty, it returned dynamic content on demand, and when an editor launched a new piece of content it would update all across the site instantly which solved the major complaint about the old system.

The problem was that we had a scalability problem.  The CPU load on our database server was directly proportional to the number of page views, so at high traffic times, the load on the database would spike, the system would slow down and we would start to suffer outages.

We had applied all the standard java scalability solutions, we had a pair of in memory caches that were designed to cache content and queries respectively to reduce exactly this kind of behavior.  Our initial diagnosis was that the cache hit rate was far too low, that the breadth of content accessed across the guardian site meant that we were increasingly unlikely to still have the details in the cache, especially as traffic increased.

We looked at scaling our database system, but the estimated cost was multiple millions, mostly due to the arcane licensing contracts for multi-instance licenses for our database server.  We looked at separating and dividing the content onto dedicated machines, say one machine looking after sport, and another for UK News.  This didn't match the model we had built, and we suspected that the load on the database wouldn't be ameliorated by this solution, only load on the application servers.

What could we do?  We investigated distributed caches.  ehCache at the time did not have any distributed cache functionality, and the options available to us in the javaworld either involved some severe unexplainable black magic or some very J2EE style patterns which we were trying to avoid.  This looked like an insoluble problem.

At about that time, we also bought in [Simon Willison](https://twitter.com/simonw) to work with us.  Simon had a news background and wanted to work on some of the more exciting news projects that the organisation was doing.  He was bought in because he had experience that the rest of the team did not have, in building simple web applications, front and back, in a very short space of time combined with a news background that would help understand the stories and projects.

Simon bought with him a new way of approaching problems.  Simon was a python programmer, in fact one of the original team that created the [Django](http://www.djangoproject.com) project, he was an expert in using a very easy to use, quick framework to create very presentable websites in a short period of time.  That meant that to us, his code was inexplicable, and he used lots of tools that we had never heard of.

I was lucky enough to have met Simon a few years prior, as I was a python enthusiast and thus went to the Python UK conferences.  Since I'd never written anything in production in python or attempted to scale it, most of my knowledge was just in the interest of the language, but it meant that I got to chat to him on a regular basis and compare notes for what we were doing.

One time, some of my coworkers and I were still struggling with the cache issue and trying to explain it to him, and he suddenly asked "You tried memcached I presume and it didn't meet your needs".  When we explained that we'd never heard of memcached, he span it up and showed us.  A distributed cache with a astonishingly simple interface, support for time to live, and wonderfully scalable when using a sensible client library?  This was exactly the tool for the job, but somehow we had never heard of it.

You see, memcached was the solution to these sorts of problems in a python or ruby world, where you had a shared nothing architecture and lots of small application servers.  In particular where your application module, mod_python, actively kills the interpreter after a few hundred requests to deal with memory leaks, and so any inmemory cache is totally ineffective.  Where sharing memory between the processes is hard enough that out of process caches are a thing.

In comparison, in the Java world, our application servers didn't restart for 2 weeks, and java controlled the entire process memory space.  This sort of solution was just unneccesary for us, and so we had never heard of it.

We built a [simple cache wrapper](https://github.com/guardian/simplecache) which we used as an interface for our existing cache layer, and connected to the memcached protocol.  We rolled it out and to our surprise it was a massive success.  All of the application servers could share the same cache, getting massively increased hit rates across the machines.  We still suffered from the long tail problem, but the solution to that is a story for another day.

##  Team Culture and other ecosystems

So what is my rather heavy handed point here?

As a team of java developers, the context in which we worked, the tools we worked with on a regular basis made it very hard for us to see a solution to our problem.  We had begun to suffer from groupthink, where nobody in the team could envision a different way of approaching the problem.

The value that other languages and other systems bring to us in many cases is not just the direct values of that language, but when we can bring those lessons back to our existing systems and apply the understanding gained.

Each time I learn a new language I learn more about how to solve more generic programming problems, and I learn the new cultures way of attacking a problem.  Sometimes my old language deals with this better, but other times, although the new culture deals with it differently, I can reapply that learning.

Encouraging the adoption of other languages, of a heterogeneous ecosystem from which ideas can be pooled, compared and improved can only lead to better teams solving problems in better ways.
