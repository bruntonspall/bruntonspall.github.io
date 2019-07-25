---
layout: default
title: "Adopting a new paradigm"
date: 2015-03-25 09:05:57 +0000
comments: true
published: false
categories:
---

New technologies come along that change the way that we work, operate and build things.  Cloud computing, Containers, Continuous Integration, Continous Deployment, Microservices (see they don't all start with a C) are all examples of this.

As I look around companies and technical staff adopting these technologies I notice that many people looking at the technologies and failing to understand that paradigmatic shift required to truly adopt them.  I can understand organisations choosing not to adopt a technology, maybe it's not core to the business, maybe it's too risky, maybe they are waiting until the hype cycle has it tested and past the trough of disillusion.

But what I find hard to understand is otherwise clever and well read technologists half heartedly adopting a technology and then declaring that it is not suitable, sufficient or doesn't deliver on it's promises.

# Cloud Computing

The emergence of cloud providers as utility providers of computing resource has been one of the biggest changes to hosting in the last decade, and it's been going for some time.

Many organisations approach cloud computing providers as a replacement data center.  They may claim it's cheaper (it's not), and want to migrate to using it.  But they bring their old data center expertise with them.

I see architectures that have specially configured individual servers with years of uptime.  I see arhcitectures that rely on network configuration that doesn't really exist, and I see architects asking questions at conferences that clearly demonstrate that they think of the cloud provider as an extension of their data center.

The worst behaviour that I see is the increase in the desire to run your own internal cloud platform.

Let me be clear, I see and understand the drawbacks to the cloud.  You have to build systems that are resilient and dsitributed in ways you didn't before.  You can have noisy neighbours and it can be significantly harder to predict the operational characteristics of the system.

So what value is a private cloud?  Well you are now dealing with all the drawbacks of running your own data center, managing power consumption, multiweek delays on ordering hardware, networking and dealing with failures.  You are still going to need to pay your infrastructure bills, multiple redundant network connectivity and so forth, but now you've added all the drawbacks of cloud computing to the pot.

This behaviour tends be driven, in my opinion, by a desire not to let go of the past.  Architects and business owners feel comfortable in their existing world, and when they try using the cloud in this hybrid basterdised way they naturally limit the benefits that they can reap.  This ends up with them claiming that cloud hosting is more expensive (it is), and that it delivers very little value (not true).

Architecting systems for the cloud is hard.  The value of the cloud is not that it's cheaper, the value is that you can increase time to market, you can decrease overhead costs in infrastructure, that you can reduce failure costs and you can reduce beauracracy around capacity planning, resource allocation and so forth.  These cost savings don't come out of the organisations CapEx, in fact they may not be on the budget at all, but if you can complete prjects faster, and get feedback faster, you are increasing your potential for revenue streams and that offsets the OpEx/CapEx expenditure that might look higher on paper.

# Continuous Integration and Delivery

Unit testing and testing tools have been around for a long time, but when we started to see tools like CruiseControl come around that can take your code, watch for changes and automatically kick off a build we see a massive change in the way that development can be done.

Firstly we see a huge value increase in the ability to run consistent builds, no more does the software compiled on Anna's machine differ to software compiled on Bob's machine.  Instead all the deployable software is run on a single machine.

Secondly we see an improvement to collaboration in teams.  If I can write code and when I commit it to the central repository we get a build straight away, then we can get faster feedback.
