---
layout: default
status: publish
published: true
title: Regular Expressions
author: Michael Brunton-Spall
author_login: bruntonspall
author_email: michael@brunton-spall.co.uk
author_url: http://www.brunton-spall.co.uk
wordpress_id: 61
wordpress_url: http://blog.brunton-spall.co.uk/2009/12/regular-expressions/
date: '2009-12-18 22:57:30 +0000'
date_gmt: '2009-12-18 22:57:30 +0000'
categories:
- technical
tags:
- python
- open source
- regular expressions
- regex-builder
comments: true
---
I&#39;m not a big fan of regular expressions. &nbsp;They can be powerful, but for anything remotely complicated they can be a nightmare to maintain and re-read. &nbsp;I had an idea recently for an easy to use chaining regular expression building library but I can&#39;t find anybody doing it, so I&#39;ve created one myself.

I&#39;ve borrowed the concept of chaining from the jquery library, so each function on the Regular Expression Builder object returns the modified object. &nbsp;This makes the interface easier to read, and makes constructing a complex object pretty simple.

<!--more-->

The code can be found at <a href="http://github.com/bruntonspall/regex-builder/" target="_blank">github</a>.&nbsp;

Using it in your code is pretty simple, import the library, and start building the regular expression

&nbsp;

<div class="code">	from regex_builder import *<br />	regex = str(literal(&#39;abc&#39;).one_or_more(literal(&#39;ef&#39;)))</div>
&nbsp;

So why is this useful? Regular expressions can start to get pretty large and funky.&nbsp;So for example, we might want to match a set or urls something like/travel/france and /travel/france+skiing and also /travel/france+science/nanotechnology

&nbsp;

<div class="code">	/[a-zA-Z0-9]+/[a-zA-Z0-9]+(?:+(?:[a-zA-Z0-9]+/[a-zA-Z0-9]+)|[a-zA-Z0-9]+)?</div>
&nbsp;

Writing this in the first place without making a mistake is painstaking and fiddly. &nbsp;Coming back to it 6 months later and having to change it is even worse. &nbsp;Here is the equivalent using my library.

&nbsp;

<div class="code">	slugword = one_or_more(range(&#39;a-zA-Z0-9&#39;))<br />	section_and_keyword = literal(str(slugword)+&#39;/&#39;+str(slugword))<br />	combiner = literal(&#39;/&#39;+str(section_and_keyword)).optional(<br />	&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; literal(&#39;\+&#39;).alternate(section_and_keyword, slugword))</div>
&nbsp;

Now it&#39;s not perfect by any stretch of the imagination. &nbsp;I&#39;d love to not have to use str and literal to repeat a defined regex, but the current architecture means that executing &quot;slugword.literal(&#39;a&#39;)&quot; modifies every instance of slugword.&nbsp;

Other Todo&#39;s includes adding word, whitespace and digit methods, adding an any_character method and finding bugs by actually using it. &nbsp;I&#39;ll also be extending the framework to automatically match using the re module, so you won&#39;t have to manually compile and match by hand.

I also think it would be fairly easy to port to Java, so thats on the cards

Let me know what you think, use it and tell me what regex functions you use that I&#39;m missing. &nbsp;I only implemented the simplest functions, so there is a lot of lazy flags, special repeat types and stuff that I&#39;ve never personally used, and so didn&#39;t implement.

