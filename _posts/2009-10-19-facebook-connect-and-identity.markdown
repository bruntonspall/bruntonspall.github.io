---
layout: default
status: publish
published: true
title: Facebook Connect and Identity
author: Michael Brunton-Spall
author_login: bruntonspall
author_email: michael@brunton-spall.co.uk
author_url: http://www.brunton-spall.co.uk
wordpress_id: 66
wordpress_url: http://blog.brunton-spall.co.uk/2009/10/facebook-connect-and-identity/
date: '2009-10-19 23:45:48 +0000'
date_gmt: '2009-10-19 23:45:48 +0000'
categories:
- technical
tags:
- identity
- facebook
comments: true
---
The more I think about Facebook Connect and identity the more worried I get. &nbsp;Lets start with my basic premise, your online identity is much too valuable to be controlled by a single company. &nbsp;We&#39;ve been there before, we&#39;ve seen what happens to the internet when a core technology is controlled by a single company, and Internet Explorer 6 was the result.

<!--more-->

The problem really lies in that as a website developer I can understand exactly why Facebook Connect is so appealing, and in fact why I am likely to end up supporting it on my website for commenting and social web stuff. &nbsp;It&#39;s because facebook have put a lot of effort into ensuring that developing social knowledge applications for your website as easy and rewarding as possible.

If I want to support you logging in to write a comment on something I write, I have a number of possible solutions. &nbsp;Firstly i can outsource all the complexity to another site, like Disqus or Pluck. &nbsp;Secondly I can program it all myself, using OpenID, OAuth, Open Contacts and probably some other API&#39;s that I&#39;m &nbsp;not a specialist in, or finally I can use the nice Facebook shim.

If I use someone like Pluck or Disqus, I still get no real control over my own users. &nbsp;I&#39;ve still given up control to a third party, just not the identity behemoth that is Facebook. &nbsp;Other than through their management and reporting systems, I&#39;ve got no visibility of who users are, what they are doing, or any more information about who they are than I started with. &nbsp;I might know that happy_boy_206 has commented twice, but I know nothing about who that user is, nor do I know when she/he just browses the website but doesn&#39;t comment.

With Facebook Connect, I can get information about users, and because users tend to stay logged into facebook at all times, as a web developer I can see when they come to my site, and what they do providing they are logged into facebook.

Even more importantly from a user experience point of view, Facebook is very easy to use. My mother is on facebook, my aunts and uncles use facebook, even most of my church and other non-technical freinds use facebook. &nbsp;They might wish that I wrote in English on occasion instead of technicalease, but they understand facebook. &nbsp;They do not have an OpenId account, or if they do (because of a yahoo account, or some other open id provider) they don&#39;t understand it as OpenId. &nbsp;I could create a login box that contains a list of possible accounts, including google accounts, yahoo accounts, other open id providers etc, but lets face it, the average user is likely to have a facebook account and know what it is.

Finally, the information that facebook gives to me as a website is amazing. &nbsp;You the user can log in, and grant my website permission to know who you are, and I get access to a large amount of your personal data. &nbsp;As a Web Developer, turning down that offer of access to your personal data and social graph is foolish.

So Facebook appears to be winning the identity wars, and because it&#39;s winning, it&#39;s becoming the defacto standard that can be used, and because it&#39;s a defacto standard, even people like myself who are wary of the power of the monolithic corporation need to use it&#39;s technologies.

