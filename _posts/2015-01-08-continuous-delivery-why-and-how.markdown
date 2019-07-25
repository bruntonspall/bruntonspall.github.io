---
layout: default
title: "Continuous Delivery - Why and How"
date: 2015-01-08 08:49:31 +0000
comments: true
published: false
featured:
  preview: /images/uploads/agile-vs-prince2-300x199.jpg
  large: /images/uploads/agile-vs-prince2.jpg
  caption: Agile vs. Prince2 by Matthew Hutchinson
  url: https://www.flickr.com/photos/hiddenloop/429289122

comments: true
categories:
 - technical
 - featured
---
Why on earth should I release software often and regularly, and how do I go about doing it?  
People talk about Continuous Delivery, Continuous Deployment, Continuous Integration and many other terms, and I'd like to demystify what these mean.

We also see a lot of technologists talking about how to setup the tools, like Jenkins or Teamcity or whatever to actually do it, but very few people appear to be talking about the benefits of doing regular release or weighing up the pros and cons of this kind of approach.
<!--more-->


I recently tweeted a simple summary of the language, and I think it might be worth repeating

<blockquote> Integration - Building on every commit, Deployment - Deploy on every commit, Delivery - deploy as fast as needed by the team</blockquote>

The language can get people very confused, and in particular I see a lot of people confusing continuous deployment with continuous delivery, and assuming that what we want is an automatic, unattended deploy into production on every developers commit.  In fact what we want is to be able to deploy as often as needed, and we want the culture of the organisation and development team to be that they want to deploy as often as possible.

Before we go into how do we implement continuous delivery, I'd like to assume that you aren't convinced of the benefits.

##  Why should we do Continuous Delivery?

What possible benefit does it have to attempt to deploy as often as this?

Criticisms I've heard tend to fall into a couple of different categories

1. Deploying our software is risky, doing it more often reduces the number of checks we can make so is even riskier
1. Deploying our software impacts people who aren't directly in the team, and they have to approve each release
1. Deploying our software is sufficiently complicated that it requires time, resources or planning to pull off
1. The cost of automating our deployment is greater than the benefit we'd get from addressing the previous 3 points

##  Regular releases reduces risk

Counterintuitively, if your deploys are risky, then performing them more often will reduce the risk not increase the risk.

I say this is counterintuitive because in my experience, nobody just understands this unless they've thought it through or been guided through it.  The problem is once you've understood and accepted that regular releases reduces risk, you tend to forget how counterintuitive this idea actually is.

The reason that increasing the number of releases reduces the risk of any individual release is becuase of the effect of batch size on production flow.  What does that mean?  Let's think about an example.

At the Guardian, when I started we did releases every 2 weeks.  This was a massively fast release process compared to many competitors, but still had it's issues.  We had a team of nearly 100 working on the software, and we divided the features being added to the system into stories that would take 2 people on average 2 - 3 days to complete.  That means in 10 days of work, we were writing around 100 - 150 different stories, which probably represents around 300 "features".  At any given release we were batching up 300 changes of behaviour to a complex system and releasing them all at once.  In order to have any confidence in the release, we needed around 4-5 days of quality assurance checking of the release candidate, which was mostly spent checking that the whole ystsem behaved as expected in the new feature areas.
