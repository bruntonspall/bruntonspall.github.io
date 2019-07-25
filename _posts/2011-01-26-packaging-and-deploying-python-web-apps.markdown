---
layout: default
status: publish
published: true
title: ! 'Packaging and deploying python web apps '
author: Michael Brunton-Spall
author_login: bruntonspall
author_email: michael@brunton-spall.co.uk
author_url: http://www.brunton-spall.co.uk
wordpress_id: 125
wordpress_url: http://www.brunton-spall.co.uk/?p=125
date: '2011-01-26 21:30:51 +0000'
date_gmt: '2011-01-26 21:30:51 +0000'
categories:
- technical
- rants
tags:
- python
- wsgi
- deployment
comments: true
---
<!-- p, li { white-space: pre-wrap; } --><!--StartFragment-->I love python.  I have really started to get into python in a big way since I was a beta tester for <a href="http://code.google.com/appengine/">Google's App Engine</a>, and I've used it for a number of production projects now.  It is probably my go to quick language.

Unfortunately I started my career programming C and C++ and moved onto Java, which is the primary language where I work.

I could say a number of bad things about Java, from it's "Enterprise" mentality, to its love affair with XML in all of it's most horrid incarnations, but the best thing about Java is the ease of deployment of a web application.

<!--more-->

You see I am a fairly simple guy, I like my life easy and uncomplicated (or pragmatic if you prefer), and one of the things that Java gives me is a simple standard for a web application.  I put all of my compiled class files into the classes directory, add the binary library files to the lib directory, add a web.xml file in the WEB-INF directory and zip it up and rename it to a war file (or I use a tool that does that for me, like <a href="http://maven.apache.org/">Maven</a>, <a href="http://ant.apache.org/">Ant</a> or <a href="http://code.google.com/p/simple-build-tool/">sbt</a>).

This process is so simple that I can do it reliably, repeatably and I can even configure my <a href="http://www.jetbrains.com/teamcity/">continuous integration</a> server to do it for me.  Now I don't even have to worry about the process, I just commit my files and I get a war file uploaded into my artifact repository.

Tonight I struggled for hours to get a simple python web application uploaded and running on a web server.

Part of the problem is that I'm (thanks to an awesome web systems team) stringent about the security of our frontend servers.  I have three simple rules for them,

<ol>
<li>they cannot access the internet</li>
<li>they cannot access internal services that are for development</li>
<li>they cannot have compilers / utilities on them</li>
</ol>
This means that god forbid that somebody gets shell access on one of our servers, they are unable to download a rootkit (no internet access), they can't springboard out to internal services (no ssh or http access), and they can't compile tools that could help bypass our security (no C compiler).

I don't really think these rules are unreasonable, they are somewhat strict, but they have perfectly reasonable foundations and I'm very loath to break them.

So how does this work for Java web apps?

When it comes to my java webapps, my deployment process is fairly simple.

We run our deployment from a springboard machine, and the script does the following:

<ol>
<li>Downloads the build artifact from our continueos integration machine and unzips it into a temporary directory</li>
<li>Finds SQL scripts, pre-run scripts etc and run them (against the database say)</li>
<li>copies the war file from the temp directory onto the app server</li>
<li>restarts the app server (assuming we're not hot-deploying which I've never got to work successfully)</li>
</ol>
When I came to my nice little WSGI python app, I wanted to do essentially the same thing, but the problem was that every tool in my python toolbox wants to break one or more of my security rules.

On my local dev box I use <a href="http://pip.openplans.org/#">pip</a> and <a href="http://virtualenv.openplans.org/">virtualenv</a> and <a href="http://morethanseven.net/2009/07/27/fabric-django-git-apache-mod/wsgi-virtualenv-and-p.html">so</a> <a href="http://www.saltycrane.com/blog/2009/05/notes-using-pip-and-virtualenv-django/">should</a> <a href="http://www.b-list.org/weblog/2008/dec/15/pip/">you</a>.  This means I have a handy requirements.txt that details everything that is needed to run the program.  Pip by default downloads all of it's files from the global pypi, or with some magic options from a local pypi, and there appears to be no way to package up the files that it downloaded (but see <a href="http://pip.openplans.org/#bundles">pip bundles</a> - note the not stable yet).

Instead the recommendation for pip and virtualenv everywhere that I can find is to run virtualenv and pip on your production servers.

In all cases that requires either  breaking rule 1 or 2 as pip needs to download the files from either the internet (the pypi server), or from an internal pypi server (which is for development).  Now I'd be willing to shift a little on rule 2, it's the one with the least significant security implications, but no, pip seems to insist on breaking rule 3 as well.

When my project requires a python library that happens to be implemented natively (which a significant number do), it needs a C compiler on the machine... on my production web server machine, and I absolutely wont budge on that requirement.

So now I start to look at other options, remember that my requirement here is essentially that I have some code that depends on some libraries and I'd like to build a single deployable artifact that my servers can pick up which has everything on the same python path.

I'd really rather not install those libraries system wide because we install multiple applications on a single machine and I don't want to deal with the inevitable conflicts when an application needs a newer version of the library, thats the whole point of virtualenv right?

So my next step is to start trying to get an egg for each of my dependencies.  At this point I've already got quite cross and I've managed to get my Java head on.  "All I want to to put each egg in a lib directory and add lib to the python path and it should all work" I exclaim with a little bit of extra swearing.  Unfortunately this turns out to be a big mistake since Egg files are a nasty hack that easy_install created and has stuck around.  The very patient people on #pip politely remind me that it is in fact simple when I exclaim "This seems very complicated, I just want some damn egg's on my path", "It isn't complicated, don't use eggs".  Damn, that's me scuppered then.

So I finally decide on the hackiest of the hack solutions, on our build server, after I have checked out our code, created my virtualenv, pip installed the requirements and run the tests I am going to zip up the ve directory and use that as part of the deployable artifact.  Everything in my body is telling me this is wrong, I've got a zip file that contains bin/python2.6 and lib/python2.6/types.py as well as my dependencies, but shockingly it seems to work, I deploy my artifact, log onto the app server and run bin/python2.6 and try importing my requirements and sure enough it works, except of course that it doesn't as a web app.

It turns out that when the server starts up my wsgi file, the virtualenv is not on the python path.  A significant amount of hacking and swearing later and I've got the wsgi script manually using the site module to add the virtualenv directories to the path, and we're off, except of course we aren't.

At this point I may have elapsed into incoherent swearing for a bit (I wish I was more like <a href="http://en.wikipedia.org/wiki/The_Thick_of_It">Malcom Tucker</a> whose swearing is always <a href="http://www.guardian.co.uk/tv-and-radio/tvandradioblog/2009/oct/15/thick-of-it-malcolm-tucker">coherent, eloquent and funny</a>, whereas mine tends to be more like "Damn you mother son of a stupid crappy buggery tit wank", neither big nor clever, funny nor eloquent.

After I've calmed down and start trying painfully to debug the wsgi file (why does printing to stderr or stdout not seem to log any errors, in the end file('/tmp/f','w').write(str(sys.path)) was the only solution) I discover that absolutely nothing I can do is convincing the server to start up python2.6 instead of python2.5, which is utterly screwing up everything.  Then I find out that mod_wsgi has a compiled interpreter in it, and find out that the original builder of my machine didn't install our rpm of mod_wsgi but compiled his own, against his own python 2.5 install (in /usr/local as well).  A quick yum install mod_wsgi later and I start seeing error pages from my app that are actually generated by my app (no database, not the right directories available).  Hot diggity, I got it to work. (At this point I should point out that actually Ben Firshman and our web systems team at the guardian suffered the brunt of my swearing and ranting and gave me all the pieces needed to fix this, they deserve the actual credit).

It shouldn't be this painful, it really shouldn't.  I know java gets a bad rep for it's weirdness with the classpath on occasion, and the less said about the .NET GAC the better, but when I use a library in my application, I want to be able to package a binary version of that library in my deployable.  It should be that damn simple.

I had some paragraphs here that were my suggestions for the "python community", but actually that's lame.  I shouldn't just preach from the sidelines, "You thing isn't very good, make it better please", so I'm going to instead save that for later if I get enough time to actually come up with a sensible solution to this problem that I can implement and get working.

If you have a solution, please let me know.  I'd love to know that I'm wrong and that there is a simple solution to this that I missed.<!--EndFragment-->

