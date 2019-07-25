---
layout: default
status: publish
published: true
title: Javascript libraries and offline support
author: Michael Brunton-Spall
author_login: bruntonspall
author_email: michael@brunton-spall.co.uk
author_url: http://www.brunton-spall.co.uk
wordpress_id: 64
wordpress_url: http://blog.brunton-spall.co.uk/2009/10/javascript-libraries-and-offline-support/
date: '2009-10-26 19:49:17 +0000'
date_gmt: '2009-10-26 19:49:17 +0000'
categories:
- technical
tags:
- jquery
- javascript
comments: true
---
A quick one here. &nbsp;I develop most of the functionality to this website when I am offline on the train. &nbsp;I wanted to use the jQuery library on my website, and the most performant way of doing so is to use Googles javascript mirror. (Yes I know about the privacy implications). &nbsp;However that doesn&#39;t work offline, rendering my website into non-jquery mode and making it a bugger to implement jquery features.

<!--more-->

After removing that call and checking it in a few times by accident I did something I&#39;ve never seen anybody else do.

<pre>	&lt;script src=&quot;http://ajax.googleapis.com/ajax/libs/jquery/1.2/jquery.min.js&quot; type=&quot;text/javascript&quot;&gt;&lt;/script&gt;	&lt;script type=&quot;text/javascript&quot;&gt;	if (window.jQuery == null) {	document.write(&#39;&lt;scrip&#39;+&#39;t src=&quot;/static/js/jquery.js&quot;&gt;&lt;/scr&#39;+&#39;ipt&gt;&#39;);	}	&lt;/script&gt;</pre>
&nbsp;

This is absolutely no use to anybody browsing my website in it&#39;s deployed form, and I should put a wrapper around that to only use it in developer mode, but what it does is simple and perfect for me.

First it gets jquery from googles servers.

Then it checks to see if jquery is actually loaded. &nbsp;If not it adds a script tag that loads jQuery from a relative URL.

&nbsp;

That is all

