---
layout: default
status: publish
published: true
title: Failure at scale
author: Michael Brunton-Spall
author_login: bruntonspall
author_email: michael@brunton-spall.co.uk
author_url: http://www.brunton-spall.co.uk
wordpress_id: 158
wordpress_url: http://www.brunton-spall.co.uk/?p=158
date: '2011-02-04 14:01:16 +0000'
date_gmt: '2011-02-04 14:01:16 +0000'
categories:
- featured
tags:
- rant
- web development
- performance
- scalability
comments: true
---
<img class="alignnone size-full wp-image-159" title="Police.uk website" src="http://www.brunton-spall.co.uk/wp-content/uploads/2011/02/Dashboard-‹-Michael-Brunton-Spall-—-WordPress.png" alt="Snap of the front page of police.uk" width="588" height="302" /><br />
When you launch a <a href="http://www.police.uk" target="_blank">high profile website</a>, it sometimes will <a href="http://www.guardian.co.uk/uk/2011/feb/01/crime-map-website-crashes" target="_blank">spectacularly fail</a> for reasons of scale.  Since this is an area of professional interest I thought I'd have a look to see whether there was anything obvious, and it was apparent that the developers didn't appear to think at scale (and still haven't fixed the issues).

When I visit the crime maps website, the very first thing I see is a landing page that I can type my postcode into.  The HTML generated for this page must be the same for every user, so this page should be cached right?  Unfortunately, not only is this page uncached but it doesn't return any cache headers, last modified or expires headers of any form.  The static files for this page do return a Last-Modified header, but I suspect that's because they are hosted on S3.  Of slight note is that the developers didn't obey any of the website performance rules, there is no gzip and there is multiple CSS/JS files rather than a combined file.

<!--more-->
Once I enter a postcode I get redirected to a dynamically generated page based on my post code.  Again there are no cache headers here, and since it's a dynamically generated page you might think it doesn't matter, but since the page is entirely javascript driven, this HTML is could be cached easily.  There are a few links that have the same query parameter passed to them, but again once you've decided to ban all non-javascript browsers you may as well rewrite those urls in the clients browser using javascript rather than server side.

Furthermore the data that is loaded by the javascript from /crime/radius which contains the crime data for my area, in monthly groups doesn't have any new data since 2010-12, not a huge suprise I'd assume these are quartaly data sets, but that means that json response should also have cache headers, and of course it doesn't.

In this case a cache wouldn't help with scale for multiple users, but would for returning users, because the developer has specified the area they are interested in by a long/lat that is accurate to 7 decimal places - an accuracy that should be down to the millimeter of so I believe (4 places gives you accuracy to 6-10 meters or so).  Since they've merely geolocated my postcode, accuracy that close is completely wasted and would destroy any cache that is there.  My postcode is probably a hundred meters or so in size, so having a long/lat that is more accurate than 2 or 3 decimal places is probably wasted.  Decreasing the accuracy of the center point could potentially give you less accurate data, but since what you are interested in is crimes within a 0.5 mile radius, you could simply increase the radius a bit and drop some of the outlying results in the clients browser, making for a much more scalable backend server.

At this point of the analysis I got just to depressed to even continue, it was clearly obvious that the developers had given very little thought as to how this website would scale.  I suspect that since it is hosted on Amazon's EC2 they figured they would scale by firing up new servers, but I contend that even at that 18m hits an hour (approx 4k a second) about 4 or 5 caches with good hit rates would have been able to serve most of the traffic without too much issue (I'd probably have suggested a cache warming for data for the major metropolises as well).

Remember kids, EC2 allows you to add new machines easily, but you still need to think about scale and performance when writing your application.  EC2 doesn't solve your scaling problems, it solves your capacity planning problems, you still need to build a site that scales without needed 1000's of servers.

