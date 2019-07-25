---
layout: default
status: publish
published: true
title: Adding Google Plus redirect to your Nginx powered site
author: Michael Brunton-Spall
author_login: bruntonspall
author_email: michael@brunton-spall.co.uk
author_url: http://www.brunton-spall.co.uk
wordpress_id: 1230
wordpress_url: http://www.brunton-spall.co.uk/?p=1230
date: '2011-11-12 12:55:02 +0000'
date_gmt: '2011-11-12 12:55:02 +0000'
categories:
- technical
- featured
tags:
- nginx
- google
- plus
comments: true
---
A quick one, this morning I've added the plus url to my website, so <a href="http://www.brunton-spall.co.uk/+" target="_blank">http://www.brunton-spall.co.uk/+</a> now redirects to my Google+ profile.

This, it turns out, is really easy to implement if you run Nginx as the front to your website.

<!--more-->
You simply need to update your server configuration to include the following snippet:

<pre>
location ~ ^/?$ {
  rewrite ^ https://plus.google.com/YOUR_NUMBER permanent;
}
</pre>
This will ensure that your domain.tld/+ issues a redirect to your Google+ profile.

Hope this helps

