---
layout: default
title: "Dev, Ops and Business Value"
date: 2014-05-03 10:00:00 +0100
author: Michael Brunton-Spall
featured:
  preview: /images/uploads/2014/05/2681686220_1f39f404f4_b-300x199.jpg
  large: /images/uploads/2014/05/2681686220_1f39f404f4_b.jpg
  caption: Thinkin' about the code by Ed Yourden
  url: https://www.flickr.com/photos/yourdon/2681686220

comments: true
published: true
categories:
 - business
 - featured
---
I find myself increasingly being worried about the way that us technologists view our value to the organisations that we work in.
Part of that is a strong lack of understanding of the purpose of the business and an over identification of technology and technology choices to the value of an organisation.

As an example, the other day, one of my friends tweeted the following mini rant on twitter:

<blockquote> @gdb_ https://twitter.com/gdb_/status/462104612724817920
Instead of Code Club, we should have Don't Write New Code Until You've Fixed the Existing Code Club.
</blockquote>

So what's the problem with this quote?

Apart from coming across as a grumpy old system operator yelling “don't change my system, you'll break it” (which was a bit unfair, but Graham [took it well](http://www.gdb.me/computing/sorry-code-club.html)), it reminds me that as developers and operators we think that the primary value we deliver is writing or maintaining code.
<!-- more -->

As technologists, we have to remember that in most organisations (companies, governments, charities), we are there to serve the business needs.  That might be to help the business turn a profit, to help it run efficiently, or to make savings to an existing costly system of operation.  Therefore it is a business decision about which of our actions creates more value.

For example, let's imagine you are a large publisher. The tech team are aware of some very dodgy code in a core component of the content management system.  There is a new microblogging company on these rise and your publisher would like to engage with it by sending microblog updates when a piece of content is published.  The arguments could go like this:

* OPS - you can't create a new microblogging component until you've fixed the core component.
* DEV - building the new component will help the organisation be on the bleeding edge.

In this case, the business decision should be about weighing up the opportunity cost against the support costs.
If you are paying for several system operations staff, whose jobs mostly consist of desperately maintaining the core component, then you can allocate a cost to that cores component, in terms of 2 or 3 FTE roles, and a risk based cost on the risk of a new system making it worse.
Comparing that cost against the business case of the expected return on investment for the new microblogging framework, the opportunity cost of delay,the PR cost of not being first to integrate and so forth.

It might be that the organisation weighs up a £250k a year operating cost, vs a multi million pound opportunity and decides to build the new thing.  It might also be that the benefits of the new thing give only an approx 50k per year benefit, and time to market doesn't noticeably reduce that benefit, in which case fixing the existing code is more valuable.

Of course, as technologists we tend to value the new and shiny over fixing the old, a problem that is reinforced to business decision makers since they don't tend to know how to value the ongoing support costs of code and so building new opportunities always look more valuable.
<blockquote> Jeff Atwood http://blog.codinghorror.com/the-best-code-is-no-code-at-all/ The best code is no code at all %}
the best code is no code at all. Every new line of code you willingly bring into the world is code that has to be debugged, code that has to be read and understood, code that has to be supported.
</blockquote>

One of the problems that I see in operations and dev teams is that they are very bad at actually presenting those real operating costs in financial terms, and they tend to get emotionally attached to their solution to the problem rather than the wider business context.
This means that decision makers are making decisions without enough data, which gives rise to the assumption that business people don't make rational decisions either.

I think we should aim to be more financially literate and make those costs and benefits clearer to the decision makers.  Only once we can do that will we start making sensible decisions about fixing old code instead of creating new stuff.
