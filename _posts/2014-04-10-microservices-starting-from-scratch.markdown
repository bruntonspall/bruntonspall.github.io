---
layout: default
title: "Microservices - starting from scratch"
date: 2014-04-10 20:51:29 +0100
author: Michael Brunton-Spall
featured:
  preview: /images/uploads/2014/01/4159325618_892e9c2e16_b-300x199.jpg
  large: /images/uploads/2014/01/4159325618_892e9c2e16_b.jpg
  caption: Scale Camp Attendees by Adewale Oshineye
  url: http://www.flickr.com/photos/adewale_oshineye/4159325618

comments: true
published: false
categories:
 - technical
 - featured
---
Everyone is talking about microservices.  It becoming clear that microservices
are not well understood or agreed by the technical community yet.
I'm going to explain my understanding of microservices over a series of posts
so that I can add to the discussion.

In this first post, I'm going to talk about whether you should start your next
project with microservices.
<!-- more -->
I don't think you should start _most_ projects with a microservices architecture.

The primary reason for this is that in most of the projects I've worked on, I
don't think the domain that the system is dealing with is understood well enough
to actually divide the system into microservices along the correct boundaries.

Instead I see groups attempting to divide their system into microservices either
along the tiers of the their web application (a data service, a business service,
and a presentation service), or dividing it according to the organisational
structure of the organisation.

In many situations, I would be advising that you start by building a simple
monolithic application, using Flask, Sinatra or Scalatra perhaps, to explore
what the user interactions and responsibilities within the domain actually are.

I would recommend that you think about identifying and containing the services
whilst building that program, and ensure that the system is easy to divide up,
but accept that moving to microservices may not be the right approach, and you
might never split it up.

It feels like a lot of architects and developers are assuming that the
microservices movement in architecture matches up with the Rails/Django movement
that caught on a few years ago.  In that architectural system you tend to build
a single monolithic application, based around modules, with CRUD operations and
with the help of some construction kits to create the framework.

The perception around microservices is that you start by decomposing your problem
into the relevant microservices and build a set of microservices from scratch.

I don't think is the right approach in most cases.  In particular I think this
is a bad approach in the situations where you don't know your domain very well.


Once you've prototyped your application, you can then start to find areas of the
system that are appropriate for splitting into microservices, but until then you
probably don't understand the interactions between the bits of the system well
enough to build good interface definitions, API's or data descriptors.

The point of a microservice is that it should have a single responsibility, as
a service.  That might be just to store, query and retrieve a type of data, or
it might be to manage a user transaction across several datastores.

There are a variety of ways of identify what the the independent services are,
and prototyping is just one of them, you could also being doing domain driven
design, or you could do workshops and build CRC cards.  I think you should be
doing all of these things to help understand what the user needs are within the
system that you are building, but in my experience nothing will help you
understand the domain than talking with users and trying to build something
simple.
