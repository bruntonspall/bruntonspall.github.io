---
layout: default
status: publish
published: true
title: PyCharm – First Impressions
author: Michael Brunton-Spall
author_login: bruntonspall
author_email: michael@brunton-spall.co.uk
author_url: http://www.brunton-spall.co.uk
wordpress_id: 59
wordpress_url: http://blog.brunton-spall.co.uk/2010/02/pycharm-first-impressions/
date: '2010-02-03 17:48:56 +0000'
date_gmt: '2010-02-03 17:48:56 +0000'
categories:
- technical
tags:
- django
- python
- pycharm
comments: true
---
Did you see my link a few days ago, about <a href="http://www.jetbrains.net/confluence/display/PYH/JetBrains+PyCharm+Preview" target="_blank">PyCharm</a> being released by JetBrains? &nbsp;I hope so because it is a very interesting IDE for python and <a href="http://www.djangoproject.com" target="_blank">django</a> developers.

## 	Introduction
PyCharm attempts to up the Python IDE stakes, by bringing the expertise of the rather brilliant IntelliJ Java IDE to the python world. The idea is nice, and the execution is good, especially with the Django integration.

<!--more-->

## 	Installation
Installation was as simple as downloading the linux tar.gz file and extracting it.

My first issue was that on a 64 bit system, it fails to start up with the message&nbsp;

<div class="code">	Error occurred during initialization of VM</div>
<div class="code">	Could not find agent library on the library path or in the local directory: yjpagent</div>
This is because it attempts to load a library for java profiling that is 32bit compatible only. &nbsp;The easy workaround is to edit bin/pycharm.vmoptions and delete the line &quot;-agentlib:yjpagent=disablej2ee,sessionname=PyCharm&quot;. &nbsp;I think you could also download the 64 bit version of yjpagent and put it in the lib or bin directory, but haven&#39;t tried it yet.

## 	Google App Engine
I did open directory and opened an existing Google App Engine project, which opened quite nicely. &nbsp;I immediately got some nice warnings to let me know that I was not overriding some methods correctly along with a spurious error that self.request and self.response are unresolved attributes. &nbsp;I guess that&#39;s a slight problem, the IDE can&#39;t possibly know that webapp.RequestHandler.initialise will be called before the get method, and since the __init__ method doesn&#39;t create those attributes the IDE can&#39;t be sure that they exist. &nbsp;

This kind of bug is the exact reason that python IDE&#39;s are hard, and not as helpful as in Java. &nbsp;In Java the fact that RequestHandler has a request attribute would have to be declared in the class. &nbsp;Since in python it doesn&#39;t, the IDE can&#39;t tell what type that attribute is, and therefore can&#39;t autocomplete the methods on it. &nbsp;So using this for the App Engine webapp framework is no better than any other application.

## 	Django
I next opened the pure django implementation for this website. &nbsp;I had a weird issue here. &nbsp;I use pip and virtualenv to ensure I have the correct version of django installed for each python project I build. &nbsp;However PyCharm doesn&#39;t support a per project set of library dependencies, and my work machine doesn&#39;t actually have django installed system-wide. &nbsp;I installed Django 1.1 system wide, and using Settings -&gt; Python Interpreter -&gt; Reload I was able to get PyCharm to reload.

I was struck by some issues immediately. &nbsp;Where I was previously doing import home.models I now need to include the project name, so import bruntonspall.home.models. &nbsp;I also have a funky bit of python path manipulation in my settings.py, unfortunately PyCharm doesn&#39;t seem to handle them properly. &nbsp;So several of my imports did not work anymore, which caused weird warnings everywhere.

## 	Final Thoughts
This is only an early access program, and I will admit that for creating a new project from scratch, for general python programming this is as good an IDE as I&#39;ve used. &nbsp;It doesn&#39;t handle all of the really weird behaviours that I&#39;ve started to use in my django projects, but for non-advanced systems it&#39;s probably highly useful.

The biggest issue as far as I am concerned is the lack of good support for virtualenv, pip and pythonpath modification in settings.py. &nbsp;It&#39;s possible that I&#39;ve missed something, and the next few days I&#39;ll be using it heavily to test it out and find out what it does well, and what it does poorly.

