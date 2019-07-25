---
layout: default
status: publish
published: true
title: ! 'Prism and NSA Spying: why I don''t (entirely) believe it.'
author: Michael Brunton-Spall
author_login: bruntonspall
author_email: michael@brunton-spall.co.uk
author_url: http://www.brunton-spall.co.uk
wordpress_id: 7286
wordpress_url: http://www.brunton-spall.co.uk/?p=7286
featured:
  preview: /images/uploads/2013/06/2321658066_d6108e4d77_o-300x212.jpg
  caption: Prism by Tim Cummins
  url: [TO BE MIGRATED]
  large: /images/uploads/2013/06/2321658066_d6108e4d77_o.jpg
date: '2013-06-07 12:10:02 +0000'
date_gmt: '2013-06-07 12:10:02 +0000'
categories:
- technical
- rants
- featured
tags:
- prism
- nsa
comments: true
---

[EDIT: Note, I have absolutely no inside knowledge here whatsoever. I haven't seen anything except via the stuff the Guardian has published publically.]
You may have read this morning that the Guardian and the Washington Post announced that they had an authenticated NSA training presentation on PRISM which claimed that they had access to multiple large companies servers and were able to spy on any and all communications.

In essence the presentation says that Gmail, Apple iMessage, Facebook and most of the other internet services are all actively monitored by the NSA.

<!--more-->
The thing is, I don't believe it. I have a number of issues with it, but it boils down to two possibilities: that the presentation itself is fake; that the presentation and project are real, but the details are wrong.

I thought the presentation itself was fake at first, for a whole number of reasons, but I'm confident that the verification process at the Guardian is thorough enough to assuage my worries.

If the slides are real, then there's a number of claims that bother me, but the main two are the claims that they have "direct access to the servers" of the corporate partners, and the estimated cost of $20 million.

What does direct access to the servers mean? If we take it at face value, it means that the NSA's system is able to log into the servers of Facebook, Google, Microsoft and Skype and do anything they want.

The problem is that I don't think that's a terribly useful ability, nor possible to do in such a way as to be kept confidential only at the highest levels.

Servers in a data center aren't just like your desktop machines. They're complex things, and for companies at that scale there are literally millions of them.

Let me start with an analogy, and use a data center I know about, the one at the Guardian. Let's say that you've managed to convince somebody suitably high up at the Guardian that you need direct access to the servers, and they legally have to keep that knowledge to a minimum.

Our servers are subdived into a number of networks. This is part of our security program, and is done at a very low level. If you can get onto one of our webservers, then you are not able to get from there to our database, the network wont allow you.

We subdivide the webservers from the application servers, which are kept away from the content publishing system servers. The databases are divided depending on the kind of data stored, so our users data is kept on different servers to our content publishing system, and no server can access both.

Secondly, assuming you are able to access all the servers and databases that you need, everytime we build a new server you'd need us to provision the server with the appropriate user accounts and details.

For most servers we use a configuration management tool that defines the users and permissions, and that configuration is stored in a team accessible version control repository. So if we added the nsa user to that repository everybody would know who did it.

Furthermore there are some systems that can't be configured in that way, they need passwords set, and those passwords need to be securely stored. In our system we have a passwords file that contains those passwords, and it is securely encrypted with public key encryption. We therefore have a record of each user who can access those passwords and what their key is.<br />
In keeping with good password hygiene, those passwords are suitably long and complex and are changed on a regular basis, so you'd need access to that file to access some of those systems.

Finally, even assuming you can manage to put your details in all of those systems without anybody knowing, and without alerting the entire team of operations staff that this top-secret system exists, you've got another big problem: change.

Even with a small team of operations staff and only around 150 odd people in our digital development team, keeping track of what servers are storing what data and doing which things is hard work.<br />
We have to have regular emails, meetings and internal documentation systems to help us know what the guardian systems look like today, and we still mess up.

Yesterday we had an issue where we had migrated one of our applications from one server to another and it was no longer able to access one of the remote servers that we normally talk to. This isn't a million pound project to change anything, this was some essential maintenence work that was happening just via the bugs work queue.

We have automated systems to allow us to change our database structure as developers change the code, and we've done projects to migrate data from one database to another.<br />
In essence I think that by the time your project to read the guardians database is written the data it's reading will be in an incompatible format, and won't be able to read it.

You'd need to continuously monitor the changes going on in our databases and datacenter and be able to premptively update your system to handle those changes.

Finally, we actively monitor the load on our databases and our application servers so that we can plan for increasing the capacity. If there was some weird NSA box in the corner that was doing strange queries on our database then we would notice it in the monitoring.

Now think about doing that for a company the size of Google, then multiplying it by 11 to follow Microsoft, Yahoo, Facebook and more. It stretches credulouity to breaking point.

That brings us back, if they can't possibly have direct access to the servers then what are they talking about?

There's two possibilities, both of which are entirely real and possible.

The first is that each of those corporate companies have come on board to allow access to a law enforcement portal that allows individual requests to be made directly by the NSA, without needing a warrant and manual form<br />
processing.<br />
This portal would have to be maintained and built by the company themselves, and it would probably allow database searching for specific terms.

We know that Google, Twitter, Yahoo, Microsoft and others have had to comply with requests with a valid warrant from law enforcement authorities already. The Yahoo and Facebook guideline documents have been leaked on Cryptome, and you can see the manual processes that Law Enforcement has to go through.

However their existence means that these companies must have systems for doing legally mandated searches of their database.

I'd certainly believe that the NSA had tried to force these companies to make those portals directly available to law enforcement, but I'd have expected more noise about it, since these companies have long resisted the existing laws in this area.

The other possibility is that the NSA has network taps into the backbone of the internet itself, and is able to send decrypt SSL style encryption and provision a dragnet style net to catch, store and forward packets or streams that are interesting.

So if the NSA had hooks into the backbone and pulled Skype/Google Video streams that were between people of interest and archived them or sent them to Fort Meade for analysis then they'd have something able to get all of that information. But it wouldn't involve direct access to servers, and it would imply that almost all modern encryption had been broken.

There's a strong suspicion that the NSA has root level certificates for SSL that allows Man In The Middle attacks, but to do that to all internet traffic would I suspect be too easy to notice.

If on the other hand they can intercept all communications and crack basic encryption via some unknown method, then they could have something that could archive more or less all of the internet traffic.

If I was building such a thing it would keep a fairly short archive of all traffic, and have a process that extracts streams that are of interest and forwards them for further processing.

The combination of those two capabilities would add up to a PRISM like capability.<br />
Now imagein that someone in sales wrote the slides, would they explain all of that or just explain it as "direct access to their servers".

I wonder if we'll ever know the full story?

