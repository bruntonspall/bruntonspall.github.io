---
title: 'Discovery, Alpha, Beta, Live… Part 2'
description: >-
  This seems to have come up again, with discussions about what the purpose of a
  discovery, alpha beta actually is, and when you should…
date: '2018-12-09T12:00:42.212Z'
---

This seems to have come up again, [with discussions about what the purpose of a discovery, alpha beta actually is, and when you should build your MVP](https://medium.com/notbinary/an-alpha-alpha-approach-5d62fb68429a).

#### **TLDR**

Discoveries help you identify problems. There are no products, no MVP’s, and almost nothing resembling software development or delivery as typically imagined in this phase.

Once you have a known problem, you need to understand the potential solution space. GDS calls this an Alpha, I prefer to call it prototyping. There are no real products, in fact in many cases, you are rapidly experimenting. Development if it happens at all is rapid hypothesis driven development design purely to get the answer to a single hypothesis. “Does a user know their identification number?”, “If we ask for monthly salary, does that make sense to people paid weekly”. You iterate on hypothesis here, trying to derisk the final product and have a real proposal for what to build

Once you have a desired and tested solution (even if lightly), you can move onto starting to build a product. GDS calls this a Beta, but I think that covers a swathe of development. I’d say the first 2–6 weeks are building the alpha MVP. At this stage you are building a team, building the production capability (hosting, security, build pipelines etc). You are also planning what features are minimum, and it’s the chance to work out what order you’ll approach those in. I’d have called this an “Alpha” phase, but that concept is taken, so I think of this as MVP phase. For very large projects (Universal Credit size), this could be months, but I find you can’t keep teams in this early place for long, they want to get on a deliver, so next you move onto

Building and releasing your MVP. This for me is the now descoped “Private Beta”. This is the phase where you work out just how minimal your product can be to launch, and whether your users will consider that minimal feature set to be viable. Once you have that MVP, you need to get it in front of real users and start getting feedback, so you should start with 1 to 5 invited users, and then maybe roll up to maybe a few hundred.

Once this phase is done and you have users who are using your MVP, you are now into the more complex phase of product delivery, because you have two streams of work, a set of findings from your real users on the ways that they use your product or service, and a set of epics or further features that you always planned to build. You are now building and delivering features to a live audience, and probably growing that audience as well. Your team needs to change the way it operates because breaking the system will bother users. Note you might be in a private beta phase of your product, but live to real users (If only GDS had used different words to avoid this confusion 😭)

Finally you are ready for any number of users to join your service, so you throw the doors open. You are now in a public beta, you still may not have the entire feature set, but anyone can use this service. You are still in active development, and you are still building features from the backlog and gathering user feedback.

At some point, the feedback starts to dry up, you have built all the features you originally promised, and the need for an active development team starts to go away. You need to continue to operate the service, and you need to continue to make changes. There will be security patches, there will still be points of user frustration and of course, there will always be stakeholders with opinions on their favourite feature that they insist on for no discernable data backed reason. Your team is going to look very different now, but your service is Live, the beta banner can go away and the rate of change should drop considerably.

Finally, something else comes along to replace your service, and you migrate users over to it and tear down your service, having a requirements document of backlog burning ceremony and all going on to new and better things.

### The Phases, Team and Budget structures

#### Discovery

During a discovery, the aim for the team is to understand whether a problem hypothesis is actually a problem. What are the problems that users face, who already exists to solve that problem, and why are they not meeting user needs.

A critical question during the discovery should be “Is this a problem that Government should solve?” because there are many problems that exist, that Government is not the right place to solve the problem directly. Instead recommendations might be made to change policies, adjust funding, or share research and data with the charitable sector

**Funding**

A discovery pot for _n_ discoveries per year

**Team Structure**

Small fast moving teams of around 3 -5 people. Skills will include delivery management, business analysis, user research and traditional research and analysis at least. Data science may come in handy here depending on the data available to you.

**How does work come in**

A backlog of potential problems, prioritised by the business in terms of where user needs are not being met, number of complaints and identified waste. Discoveries are selected from the backlog and completed

**What happens after**

Discovery outputs should be put into a discovery library. A recommendation should be written about whether there are potential solutions and placed into the Prototyping backlog

#### Prototyping

Carrying out a set of prototypes to explore the potential solution space for problems identified at discovery. You could combine Discovery and Prototyping, especially if you have only a small team, which would increase learning, speed and efficiency, but make it far more likely that people get invested in the problem/solution. Fresh eyes will take a more critical look at the problem space and might determine solutions that the discovery team didn’t recognise

**Funding**

Via the discovery pot or a prototyping pot, funding for _x_ prototypes per year

**Team Structure**

Small fast moving teams that can explore the solution space. A product manager, an interaction designer, a user researcher are all key to attempting to come up with prototypes. A developer or two at this stage can help build more high fidelity prototypes, but must be careful not to over complicate the prototype.

**How does work come in**

Outputs from discoveries should recommend prototyping and testing some solutions with users. These should be prioritised by the organisation based on the identified need, the ability of the organisation to meet the need and the capacity of the organisation to build something after. There’s no point starting a prototype if the organisation just cannot possibly make that prototype a reality later

**What happens after**

Prototypes should prove whether an idea is possible and whether it can meet the user need. Do we have the data, the technical capability to build this, and if we do will it meet the user need.

At the end of the prototyping phase, the collected learnings should be documented and stored in an archive. A rough outline of an MVP type program could be created, along with estimates for time, money and skills needed. This should be enough to build an outline business case for an MVP

#### MVP

The MVP is the first attempt to actually build a production version of a prototype. Some teams may want to get started fast by taking the prototype and modifying it, but it’s important to remember that to be viable, the code built in this phase has to meet all the robustness and reliability requirements of production software. That might not be possible with the prototype code, and I personally encourage throwing it away and starting again.

Teams will need to consider technical details like hosting, code sharing, developer access, analytics, logging, failure detection, as well as getting designers, user researchers and product managers to produce a working product.

**Funding**

The outline business case from the prototype phase should recommend a project funding structure. This should include the breakdown of whats needed for the MVP to get to a private beta.

**How does work come in**

The prototypes generated at the end of prototyping should be demo’d in show and tells and the outline business case considered by the organisations funding models. If the business case is approved, then the project will start.

Internally the team should have an “iteration-0” or “inception” process whereby they discuss the features and use a process such as MoSCoW to find the minimum feature set. This initial work should produce a backlog of “epics”, and the team can start estimating those, and breaking them down into actionable stories for the team.

**What happens after**

An MVP could be found to be unviable. When put in front of users during user testing, one of the fundamental hypothesis of the discovery and prototype could be found to be fundamentally flawed. If this is the case, the project should be stopped and the discovery or prototype documentation should be updated with this information.

If not, if the MVP is proven successful, then the full business case to roll the MVP out as a full service should be built and approved. This should happen while the MVP is being built, and if prioritised, the team should continue on from the MVP.

#### Build

This is about building the full service. The MVP may well have entered a private beta, but if not, it should at this point, so that the team can gather feedback on what is working.

The team will continue building and iterating on the product while simultaneously operating the product and ensuring that it is stable, secure and successful

**Funding**

This should be project funding, for larger projects over several years, all covered with a full business case.

**Team Structure**

This is your typical delivery team, so all the skills are probably necessary

**How does work come in**

The Epics that were deprioritised during the MVP should all be on the backlog and for the primary product delivery process. However the team should be gathering feedback from the users using the service and creating new work to iterate on what they have built. That learning should be shared with the team and the backlog should be regularly re-examined and prioritised.

**What happens after**

The service goes live, the crowd erupts with applause and everybody gets a drink…

No, seriously, the service is live, so the estimate is that the bulk of the delivery is done. The project should end and leave in place a service and a service management team of some form (more next)

#### Live

During live, the service is running, and you need to be able continue to support it during operational incidents, but also to maintain it, improve it and continue to review feedback

**Funding**

Continual operation fund and money to pay for the continual improvement

**Team Structure**

This is the real “it depends”. It really depends on the size of your service, and the size of your organisation.

There are two primary models for in-house maintenance teams.

Firstly you can have a team in existence to continually maintain the service. This is similar to the GOV.UK model, the CMS is never done, there will always be more than enough work to fund an entire team to maintain the system. What you save on RFQ’s and change request charges with your outsourced model you replace with salary costs for keeping this team in place.

If your service is small and the user need doesn’t change very much, then your service might be “done”. It will need maintenance, and there will be small feature requests, but probably not enough to require an entire team. Instead in this structure, you build a maintenance team who maintain a number of services in their portfolio. This team will need to be able to review the analytics, respond to user requests and patch and maintain the software, infrastructure and system in perpetuity.

**How does work come in**

The service will continue to produce analytics, user feedback and should be subject to user research still to ensure that it is meeting the users needs. Their context changes over time, and the service may need to change with it.

Additionally, the software will begin to age immediately, and will need continuous refreshing to ensure that it is maintained and healthy

**What happens after**

If the service is no longer needed, you can kill it, salt the ground and start all over again 😂
