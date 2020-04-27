---
title: The Inverse Conway Manoeuvre and Security
description: >-
  If you want security to be taken seriously by your development team, then you
  need to deliberately adjust your organisational structure to…
date: '2015-11-02T12:42:46.257Z'
---

If you want security to be taken seriously by your development team, then you need to deliberately adjust your organisational structure to ensure that security not in a silo by itself, but instead considered part of the team.

#### Conway’s law

Teams adopting microservices have been talking about Conway’s law a lot recently. Conway’s law tells us that the architecture and structure of software produced by large organisations will represent the communication structures of the organisation.

Put more simple, Eric S. Raymond put it:

> “If you have four groups working on a compiler, you will get a four pass compiler”.

Teams that start to adopt devops and microservices tend to notice this quite fast. If you claim to be doing devops, but your “infrastructure team” still sits in a separate location, then the system will tend to have a dividing line between the infrastructure and the system software. If instead you embed infrastructure skills into the team, either with a dedicated infrastructure engineer or by requiring the engineering team itself to run its own infrastructure, you tend to find a more cohesive system with less separation.

#### Inverse Conway Manouvre

So what is the inverse Conway manoeuvre? Teams who intend to implement microservices often find that they have difficulty at first getting small business unit products, owned by the developers and concentrating on a single small bounded context.

The reason for this is that many organisations view microservices as a technical architecture. That the engineering team can remain a single large team (or however you structure it today), and that you can simply mandate via technical guidelines that the team start building these new architectures.

That may work sometimes, but more often it doesn’t, because Conway’s law tells us that the architecture of the system will represent the communication structure of the team building it, if you don’t change the team you can’t change the software.

The inverse Conway manoeuvre is a mechanism for changing the architecture of your system, by first changing the organisational structure of your team.

If you take an existing engineering department that has a small DBA team, an infrastructure team and a development team. These teams sit only with their peers. If you ask the development team to build a microservices structure, they will probably outsource the database to the DBA to run as a platform, and treat the infrastructure as something managed by work tickets.

If instead you take one of the DBAs, one of the infrastructure engineers, and two of the developers and tell them they are now responsible for the new shopping basket system, chances are they will come up with something integrated. If you also split the other teams into teams that manage the product catalog, and the payments service, then you’ll probably end up with a system with defined interfaces between those three components.

This is the inverse Conway manoeuvre, adjusting the organisational model of communication as a means to encourage a new architecture. Even in the above situation if you have a single codebase that can only be deployed by the entire team, given the current levels of autonomy, the teams will drift towards a separated architecture with defined interfaces between the components anyway.

#### The problem with security

The problem with this is that security hasn’t entered the picture yet. Our teams consist of database experts, infrastructure experts and developer experts. It might include designers, content writers, business experts of course, but very rarely does it include anybody from security.

In many organisations, the security team is not setup to do proactive security. It is there to do auditing, compliance and incident response, and therefore they are often not included or involved in the development decisions.

Secondly, in many organisations, for legal reasons, the audit and compliance functions are required to be completely separate from the development team. It varies who they report to, but often they report directly up to the CEO (Chief Executive Officer) or the CISO (Chief Information Security Officer) or sometimes the CFO (Chief Financial Officer). This means they are not responsible to the CTO (Chief Technology Officer) who often runs the engineering team, and they are not responsible for delivery.

#### What can we do about this?

As you may have already worked out, I believe that the most important thing you can do to improve security in the development team is not to address security in the development team, but to perform an inverse Conway manoeuvre and change the organisational structure so that security is an enabler for delivery.

This might be hard for you. Some security people may need to be separate. If you critically need audit or compliance staff who are legally separate from the development team, then you may need to separate the security team.

Furthermore, many security people are not used to being part of a development team. The team probably works in a very different way. You can either encourage them to learn new ways of working, or you can encourage the team to understand and work with the security representative in special ways.

Finally, security people often like to have some form of community themselves. Security is a wide ranging field and no security person can possibly know everything. You will need to ensure that the security team still feels like a team and is able to work as a community at times.

This isn’t a perfect solution, there are a variety of issues that might occur, but as a concept, I think it’s a reasonably good first approach that you should consider and adjust as appropriate for your circumstances.
