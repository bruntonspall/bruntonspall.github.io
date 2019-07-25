---
layout: default
status: publish
published: true
title: Some new features
author: Michael Brunton-Spall
author_login: bruntonspall
author_email: michael@brunton-spall.co.uk
author_url: http://www.brunton-spall.co.uk
wordpress_id: 67
wordpress_url: http://blog.brunton-spall.co.uk/2009/10/some-new-features/
date: '2009-10-10 01:07:18 +0000'
date_gmt: '2009-10-10 01:07:18 +0000'
categories:
- technical
tags:
- web development
- twitter
- the future
- discus
- diary
comments: true
---
So I&#39;ve finally added a couple of new features, so thought I&#39;d pop up a quick explanation of what I did and why.

<!--more-->

Firstly, you should hopefully have noticed that everything got bigger, a lot bigger. &nbsp;Looking around at a number of websites, and then at mine, I realised that on any screen other than my netbook, I had to squint. &nbsp;For a site that is designed around readable content, that&#39;s a bad thing. &nbsp;Not sure I&#39;m totally happy with how big it is, so let me know if you think it&#39;s ugly big, or nice big.

Let me know, I&#39;ve said that before, on the basis that the 4 people who&#39;ve looked at this site so far are friends of mine and will let me know somehow. &nbsp;But the first comment I got on the site was &quot;You asked for feedback, but I can&#39;t give it&quot;. &nbsp;I tried, actually quite hard, to add a facebook comment box, using the fancy new <a href="http://developers.facebook.com/connect">facebook connect</a> API. &nbsp;And while adding a comment box itself was easy, the scripts make a lot of weird calls that I didn&#39;t expect, Adding the connect button only if you&#39;re not already logged in requires some very strange javascript foo that I don&#39;t appear to possess, and I could not get comment counts out at all. &nbsp;Instead <a href="http://www.paulcarvill.com/">Paul Carvill</a> suggested I try <a href="http://disqus.com">Disqus</a>, which took me all of 25 minutes to add to the post page. &nbsp;Since Disqus also supports facebook, twitter, openId and so forth as a login system, I figure it&#39;s probably a win all round. &nbsp;

Final little change, the tags are now applied to bookmarks. &nbsp;I&#39;ve not gone back through the bookmarks yet to add tags, but I&#39;ll try to get through them soon. &nbsp;At the moment it&#39;s pretty useless, but I&#39;ll add a link so you can see articles by tag, which once there is more than 5 or 6 articles will be useful.

Next steps, I need to work on pagination, since I&#39;m reaching the limit of articles and bookmarks that can fit on the front page. &nbsp;I&#39;ve added an RSS and ATOM feed, but I need to add some logo&#39;s or links of some sort, and I guess a picture of me, or some form of picture would be nice. &nbsp;The most technical next thing is the desire to add an automatic tweet when i post a bookmark or article, and linking bookmarks into delicious.com to create a delicious bookmark.

So you can now leave comments on my pages to let me know that you hate the new big fonts.

