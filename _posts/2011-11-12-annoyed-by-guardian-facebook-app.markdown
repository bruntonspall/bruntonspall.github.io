---
layout: default
status: publish
published: true
title: Annoyed by Guardian Facebook app?
author: Michael Brunton-Spall
author_login: bruntonspall
author_email: michael@brunton-spall.co.uk
author_url: http://www.brunton-spall.co.uk
wordpress_id: 1238
wordpress_url: http://www.brunton-spall.co.uk/?p=1238
date: '2011-11-12 15:26:26 +0000'
date_gmt: '2011-11-12 15:26:26 +0000'
categories:
- technical
tags:
- facebook
- bookmarklet
- guardian
comments: true
---
Are your friends sharing links to the Guardian Facebook app in their twitter feeds but you don't use Facebook and want to see the original guardian page?

Unfortunately due to the way the web (and Facebook) works, if somebody copies the URL in their browser and posts it to twitter you'll be looking at the same thing as them, and if that was a guardian Facebook app, but you haven't logged into Facebook, Facebook wont tell us anything so we can't redirect you back to the guardian.

<!--more-->
However, most modern browsers support special bookmarks with javascript in them, called Bookmarklets, and since the original Facebook url has the important path data needed for the guardian site, it's possible to manipulate the current url to take you back to the guardian site without logging into Facebook.

Basically, drag this link: <iframe style="width: 100%; height: 100px;" src="http://jsfiddle.net/bruntonspall/La9yG/embedded/result/" frameborder="0" width="320" height="240"></iframe><br />
to your bookmarks toolbar up above this page, then click this link: <a href="http://apps.facebook.com/theguardian/music/musicblog/2011/nov/10/10-heaviest-albums-all-time?fb_ref=U-204nodQNlQBg4kqpI36BV6-CFCONX01FRS-339eqXXX,U-1aSIB2sgy2SC4xtjLBhpeM-CFCONX01FRS-339eqXXX,U-aN5g4adecUQD4urFI4BbEC-CFCONX01FRS-339eqXXX,U-2kdBzVKet5QY4dqoIr7TOu-CFCONX01FRS-33992XXX,U-EozmILdgd7yv4xUCJhdt3F-CFCONX01FRS-339nqXXX&amp;fb_source=home_multiline&amp;fb_action_types=news.reads" target="_blank">Are these the 10 heaviest albums ever made?</a> and then click the new fb-&gt;g link on your toolbar and you should go to the guardian site.

This will only work if you are not logged into facebook.

## How does it work?
The Bookmarklet is a link with a javascript protocol (before the colon).  Your browser will execute the code when you click the button, and it will do so within the context of your current page.

It gets the current url, and looks for a parameter called cancel_url.  That's where the url for the original apps.facebook.com is.  Next we replace apps.facebook.com/theguardian with www.guardian.co.uk, then we update window.location with the result.  This causes your browser to load the new url, and you should be on the guardian site.

Note that it does no error checking, it does not handle unusual guardian urls (that might not start www.guardian.co.uk) nor does it check whether there is a cancel_url before acting.

You can view the source and repurpose for your own uses at <a title="jsfiddle" href="http://jsfiddle.net/bruntonspall/La9yG" target="_blank">jsfiddle</a>.

