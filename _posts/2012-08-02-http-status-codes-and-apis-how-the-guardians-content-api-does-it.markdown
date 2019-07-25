---
layout: default
status: publish
published: true
title: ! 'HTTP Status Codes and APIs: how the Guardian''s Content API does it'
author: Michael Brunton-Spall
author_login: bruntonspall
author_email: michael@brunton-spall.co.uk
author_url: http://www.brunton-spall.co.uk
excerpt: We've managed to build up a certain amount of experience over the last few
  years with building API's.<br />During the building of our latest Content, Identity
  and Discussion systems, we realised that we have learnt some things that are worth
  sharing, especially since the reasoning behind these common practices might not
  be as well understood.<br /><br />Today's story is about why calling our Content
  API in JSONP format results in a 200 OK response for invalid urls, and why we littered
  our json response with a seemingly pointless status field.
wordpress_id: 3445
wordpress_url: http://www.brunton-spall.co.uk/?p=3445
date: '2012-08-02 13:31:35 +0000'
date_gmt: '2012-08-02 13:31:35 +0000'
categories:
- from the guardian
tags:
- michael brunton-spall
- developer blog
- article
- technology
- internet
- web browsers
comments: true
---
I wrote on JSON and API's on the guardian developer blog recently.

<hr />
<!-- GUARDIAN WATERMARK -->
<strong>The content previously published here has been withdrawn.  We apologise for any inconvenience.</strong>

<!-- END GUARDIAN WATERMARK -->

