---
layout: default
status: publish
published: true
title: What is DevOps not?
author: Michael Brunton-Spall
author_login: bruntonspall
author_email: michael@brunton-spall.co.uk
author_url: http://www.brunton-spall.co.uk
excerpt: ! "I've spent the last two weeks at conferences, and for some reason people
  keep assuming that I work in operations.  I can kind of understand why, but it's
  also started a number of conversations about DevOps, and the complete misunderstanding
  of the term.  It seems that DevOps is a confusing movement for people, and lots
  of people are assuming that some of the practices that might come with organisations
  embracing DevOps are themselves what make you DevOps.\r\n\r\nDefining what devops
  is can be hard, so instead I thought I'd feature a few of the things that devops
  isn't."
wordpress_id: 2153
wordpress_url: http://www.brunton-spall.co.uk/?p=2153
date: '2012-03-13 12:13:14 +0000'
date_gmt: '2012-03-13 12:13:14 +0000'
categories:
- featured
tags:
- devops
comments: true
---
I've spent the last two weeks at conferences, and for some reason people keep assuming that I work in operations.  I can kind of understand why, but it's also started a number of conversations about DevOps, and the complete misunderstanding of the term.  It seems that DevOps is a confusing movement for people, and lots of people are assuming that some of the practices that might come with organisations embracing DevOps are themselves what make you DevOps.

Defining what devops is can be hard, so instead I thought I'd feature a few of the things that devops isn't.

<!--more-->

##  "I've renamed our Ops team the DevOps team"
This is not DevOps, you've renamed your operations team, but that is all!  You may as well print a million pound note or command the sea to stop for all the good it will do.

##  "But our devops team run puppet/chef/CFEngine to manage their systems"
This is not devops, this is just ops using new tools.  You Just configure your tool in a ruby based DSL (or use CFengine), so now you have two problems instead of just the one.

##  "We split our ops team into Infrastructure and DevOps.  Our infrastructure team rack and stack, our devops team manage puppet/chef..."
Congratulations! You've recognised that some operations people don't want to do development tasks, how happy for you. But all you've done is split them out of the Ops team, but your ops team still sounds like an ops team to me.

##  "Ok, so we got rid of our ops team, and our devs have root now"
This is not just not devops, but it's pretty damn stupid.  It turns out that people have specialisations.  I wouldn't expect a java developer to be as productive with backbone.js as one of our javascript engineers, and I wouldn't expect every developer to be able to setup and configure apache or nginx.  Full stack engineers are great, and if you've got a full team of them then you have a great resource there, but even then everybody will have things that they prefer doing and things they are more efficient at doing.

##  "Ok, so we got rid of our ops team, our devs have root now, and we hired a DevOp (or DevOps) to manage configuration management. Better?"
So now you got rid of a team, replaced them with a new team with a different name, less authority and expect your team to be functional?  Get out of here!

##  "I see DevOps for what I think it is, a power grab. Revenge for not giving 'devs' root when they wanted it"
I think you are assuming that DevOps is a developer movement, and that it's all about power.  You are so far wrong that I'm pretty confused right now.  However this is also an accurate view of how some people view the devops movement, sometimes those people are grumpy sysadmins who don't want to do that configuration management thing, but that might be exactly how senior business decision makers are viewing devops.

My experience of DevOps is that the general trend is not of developers wanting to do sysadmin work or have root, but of existing sysadmins wanting to make their lives easier by working more closely with developers.  All of the people who I would consider to be doing "Devops" are brilliant sysadmins who understand something bigger than the inner workings of IPv4, BGP, Apache and nginx.

So lets make a simple list, DevOps is not about:

 * Less racking and stacking hardware<br />
 * Developers having root<br />
 * Using configuration management tools (like Puppet or Chef)<br />
 * Continuous Deployment (10 times a day)<br />
 * Getting rid of the Ops team

But surely I hear you ask, that's what all the blogosphere has been talking about, how DevOps will revolutionise Ops, and that all of these things are possible?<br />
Well that is kind of true, it looks like organisations that embrace DevOps might do some of those things, they also might not, it depends on the requirements of the organisation.

Ok, so I've talked a lot about misconceptions of DevOps, so what do I think DevOps actually is?

##  "You are DevOps when your operations team and your developer team are measured and rewarded by the same metric, did you help the business deliver value today".
The traditional view that pits devs versus ops is that ops teams are measured on reliability and availability (essentially uptime), whereas devs are measured on number of stories delivered.  These two goals are in direct opposition and it leads to developers wanting root access to get around the pesky ops team, and very grumpy sysadmins at 4am fixing bugs that devs didn't see.

Both are bad measurements, they are proxy metrics for a real metric which is business value.  Downtime is probably bad for your business, except that there are cases where it isn't.  The Apple store is taken down during apple keynotes.  Do you think that the Apple ops team looses their bonus over this?  Downtime is a proxy for business value, so greater downtime probably means lower business value.

Delivering new stories is a great metric if every story delivers value, but deploying 20 times a day is completely wasted if what you are deploying are minor ideas that one internal stakeholder wants that have no impact.  Again, deploying stories is a proxy for business value, and generally the higher the rate, the greater the business value.

See that's the problem with DevOps, it's so hard to get right because you need a team that is willing to focus around what the business actually needs, be willing to measure themselves and each other and work out what the business value is.

To do DevOps successfully you need to be able to measure and estimate the business value of a new story, and the business risk to each deployment.  Teams that do that are likely to start deploying more often, use configuration management and may even move towards removing the ops team, but in each case it will only happen if doing that action creates more business value than risk.

Of course with DevOps there is the eternal truth that if you get 5 DevOps in a room to debate "What is DevOps" you'd get 7 answers out at the end, and this is one of mine, so you can take it with a pinch of salt but my main point is that DevOps is not a set of practices, it's not *what* you do, it's *why* you do it.

[Updated for spelling and clarity in a few sentences, thanks to @mart_brooks and @tomnomnom]

