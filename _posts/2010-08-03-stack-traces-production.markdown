---
layout: default
status: publish
published: true
title: Stack traces in production
author: Michael Brunton-Spall
author_login: bruntonspall
author_email: michael@brunton-spall.co.uk
author_url: http://www.brunton-spall.co.uk
wordpress_id: 57
wordpress_url: http://blog.brunton-spall.co.uk/2010/08/stack-traces-production/
date: '2010-08-03 18:19:02 +0000'
date_gmt: '2010-08-03 18:19:02 +0000'
categories:
- technical
tags:
- web development
- security
comments: true
---
There have been a number of incidents recently where a public website I&#39;ve been using has gone wrong shown me a nice server provided stack trace on the screen. &nbsp;The most recent of these examples was the <a href="http://www.cineworld.co.uk">Cineworld website</a>.

This is really really bad for a number of reasons.

<!--more-->
## 	1. It&#39;s a bad user experience
My wife was using the cineworld website when it returned the error page. &nbsp;Since she doesn&#39;t use the Apple Mac a lot she didn&#39;t know what was wrong, she told me that something was wrong with internet connection. &nbsp;Since Chrome does display a strange error page when the internet is not available this is a perfectly valid assumption. &nbsp;In fact it appears to be everybody&#39;s assumption that when you hit a url that if you see any technical text it&#39;s coming from your local computer rather than the website. &nbsp;Normal non geek people don&#39;t think that websites can serve one page fine and fail when you follow a link.

## 	2. It reveals that you are having problems
In fact because it&#39;s a technical error statement it reveals that you are having problems because of something you actually did. &nbsp;i.e. you&#39;ve deployed a bad bit of software or that your website is buggy. &nbsp;The fact that you are revealing the error information shows that you have done something seriously wrong, and the implication is that you can&#39;t provide a good service.

## 	3. It reveals important technical information
This one is important. &nbsp;You might choose to reveal that your website is powered by Java, using the tomcat server, but you probably wont explain much more detail than that. &nbsp;In this case it revealed a number of technical things that you probably didn&#39;t want revealed.

The stack trace consisted of two traces, this is fairly common in Java and indicates that your web application uses a template system that defers to another thread to build up some portion of the website, and then composites the results. &nbsp;Not a particularly big issue, but it tells any hostile attackers that you are using threads, and therefore multithreading code which might be susceptible to a number of issues. &nbsp;There are worse things you could reveal however, and the rest of the stack trace contains most of them.

The thread for rendering the HTML has a stack trace that goes through pieces of code that have the following packages,&nbsp;com.opensymphony.sitemesh,&nbsp;carbonfive.spring,&nbsp;org.springframework,&nbsp;com.cineworld,&nbsp;org.springframework.aop,&nbsp;net.sf.cglib.<br />	For those of you not in the java world this indicates that they are using the <a href="http://www.opensymphony.com/sitemesh/" target="_blank">sitemesh</a> and the <a href="http://www.springsource.org/" target="_blank">spring framework</a>. &nbsp;This is fairly common, but there&#39;s some interesting things to know about the features that they use. &nbsp;<a href="http://www.carbonfive.com" target="_blank">CarbonFive</a> is far more interesting, since it&#39;s a creative web development agency that doesn&#39;t list Cineworld as one of <a href="http://www.carbonfive.com/view/page.basic/clients" target="_blank">it&#39;s client</a>s, so this error message is giving out information that could be considered commercially sensitive.

Finally the bottom of the page says it was rendered by <a href="http://tomcat.apache.org" target="_blank">Tomcat</a> 6.0.18 - This is probably the worst thing that we could do. &nbsp;For a start, knowing which specific version of a server you use lets people know what potential vulnerabilities there might be, but also in this case we can <a href="http://tomcat.apache.org/tomcat-6.0-doc/changelog.html" target="_blank">lookup</a> and find out that Tomcat 6.0.18 was released in July 2008, over 2 years ago. &nbsp;If this company hasn&#39;t upgraded their server software in over 2 years, you can imagine that the libraries and other servers might also not have been upgraded in that long.

## 	So what can you do?
In this case, because the server was returning a stack trace (and a 500 result) the java application servers knew that something had gone wrong and there are a number of things that you can do. &nbsp;At the guardian we catch these things with 3 different systems.

When we render a component on our page, we pass through a piece of middleware that detects errors (essentially a large try... catch block) and in the case of an error, we replace the content with an empty div. &nbsp;This means that if just a single component plays up we don&#39;t get a good looking page with a stack trace in a single block.

Sometimes something higher than the component renderer throws an exception, for example the url processors, or the component renderer itself or any of the other supporting code then the component renderer wont catch it. &nbsp;We therefore have a J2EE filter than does a similar thing to our error detecting component renderer and catches any site wide exceptions and replaces them with a generic error body. &nbsp;It also ensures that we are returning the correct 500 HTTP error code.

Finally as the final barrier, our frontend servers (apache in our case) detects 500 status codes from the application servers and replaces the entire page with a generic &quot;There has been an error, the page you are looking for can&#39;t be served&quot; type page.

&nbsp;

The error page that we server up looks like a guardian served page because of it&#39;s branding and it makes it clear that there has been an error at the guardians end and that we are investigating it.

This means that it is almost impossible for our java application to display this kind of information back to the user scaring them or revealing important information. &nbsp;Obviously there is almost certainly some weird combination of errors that we have forgotten, and when we stumble upon it (as it is inevitable that we will) we will add a fourth barrier to showing these pages to the end users.

Following these conventions wont necessarily save you, but it will help you stop leaking information to attackers and scaring your customers.

