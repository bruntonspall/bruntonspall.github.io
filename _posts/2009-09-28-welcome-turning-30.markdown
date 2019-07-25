---
layout: default
status: publish
published: true
title: Welcome to turning 30
author: Michael Brunton-Spall
author_login: bruntonspall
author_email: michael@brunton-spall.co.uk
author_url: http://www.brunton-spall.co.uk
wordpress_id: 71
wordpress_url: http://blog.brunton-spall.co.uk/2009/09/welcome-turning-30/
date: '2009-09-28 19:52:30 +0000'
date_gmt: '2009-09-28 19:52:30 +0000'
categories:
- technical
tags:
- django
- the guardian
- jquery
- diary
- fabric
- apache
- nginx
- wsgi
comments: true
---
Most people for their 30th birthday do something to recapture their youth. I went paintballing and created this site.

This site is going to be my 'blog' as much as I hate the word and term, detailing my thoughts and allowing me to play with some cool technologies.

<!--more-->
This site was developed over about 5 hours of programming, using a number of cool technologies. Firstly the site is written in the <a href="http://www.djangoproject.net">django framework</a>, and <a href="http://www.python.org">python</a> based web framework. I know the first thing one should create with a web framework is a blogging application, but luckily this is not the first thing I've written in Django so I'm saved from that embarrassment at least

The blog itself is hosted on <a href="http://httpd.apache.org">Apache</a> and <a href="http://code.google.com/p/modwsgi/">mod_wsgi</a>, and goes through an <a href="http://nginx.net/">nginx</a> proxy. The site is deployed using <a href="http://fabfile.org">Fabric</a>.

I've also added a little javascript magic using the <a href="http://jquery.com">jQuery</a> library.

You'll notice on the right is a list of things I've recently bookmarked, that's powered with a nice little <a href="http://gist.github.com/191220">bookmarklet</a> that I borrowed from <a href="http://www.simonwillison.net">Simon Willison</a>, and it will soon be replicating into delicious and twitter.

There is plenty more to come, including some developments around the tagging of the content, and I'll riff a little on these subjects over the coming weeks.

The main thing that this blog is intending on focusing on is the various areas that I deal with in my work at <a href="http://www.guardian.co.uk">the guardian</a> as a web developer. This will vary from Java to Python and Ruby, from performance and scalability to test driven design, and of course may well meander of course occasionally into personal subjects close to my heart.

Happy reading and I hope to hear from you soon.

