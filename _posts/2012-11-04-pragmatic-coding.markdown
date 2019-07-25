---
layout: default
status: publish
published: true
title: Pragmatic Coding
featured:
  preview: /images/uploads/2012/11/pragmatic-300x199.jpg
  large: /images/uploads/2012/11/pragmatic.jpg
  caption: YIP day 231 by auntiep
  url: http://www.flickr.com/photos/auntiep/3860676374/
author: Michael Brunton-Spall
author_login: bruntonspall
author_email: michael@brunton-spall.co.uk
author_url: http://www.brunton-spall.co.uk
wordpress_id: 4540
wordpress_url: http://www.brunton-spall.co.uk/?p=4540
date: '2012-11-04 22:21:33 +0000'
date_gmt: '2012-11-04 22:21:33 +0000'
categories:
- technical
- featured
tags:
- pragmatism
- appengine
comments: true
---
>At its core, pragmatic development is about getting code written, getting it deployed and getting it out there.  Pragmatism should lead us towards minimum viable products, and releasing the minimum that we do have as early as possible to garner the quickest and best feedback.

What do I think pragmatic code actually looks like though?  I figured I'd talk about a very simple app that I built recently to explain why I wrote that code and what it does.

<!--more-->

Firstly the background, we've just done a major rewrite of the <a href="http://www.guardian.co.uk/open-platform" target="_blank">Content Api</a> at the Guardian, and one of the main architectural changes is that our indexer that notices changes in the CMS now sends a notification out via Amazon's Simple Notification System.  The Content Api boxes in EC2 subscribe to these notifications and insert the document into their local copy of Solr.  This means that each content api box is individually separate from the others, which helps to minimise dependencies between the machines.

But to know that things are working, we wanted to see what messages were coming through the SNS system.  After an abortive attempt within the team to build something complex, I threw together the <a href="http://github.com/guardian/content-api-monitor" target="_blank">"Content-Api-Monitor"</a>.

It sat on AppEngine, subscribed to the SNS stream and simply inserted each message into the datastore.  It rendered the most recent 20 or so messages when you hit it on AppEngine, and let me see what was coming through.

This solved my original problem, but after a few weeks it became clear that it had another a problem, the datastore was filling up with old data that I was never using.  I don't want a historical view of messages sent 2 weeks ago, anything more than 5 minutes ago is probably wasted. The simplest way to do this was to use a document in memcached to store the set of messages.

At it's <a href="https://github.com/guardian/content-api-monitor/tree/f12d17385742c845bbe957da13b8cf3a7c476124" target="_blank">release</a>, that's all it did, and it did it pretty badly.  All of the major code is in one file, main.py, and the code is mostly focused around parsing the json notification into a very simple form that can be stored.

The idea of storing the data in memcached and just hoping that it won't disappear would be completely unacceptable for many projects, but for this project, in 40 seconds or so we'll have completely refilled the cache anyway, so I'm not as concerned.

It's this kind of development that I consider pragmatic, it's about looking at the tradeoffs available to you and picking the tradeoff that enables you to get something built fast.  Once built you'll get feedback about whether it's useful or not.  In this case I traded off long-term storage options for a simpler data storage management system.

