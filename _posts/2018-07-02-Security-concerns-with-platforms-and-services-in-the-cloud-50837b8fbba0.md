---
title: Security concerns with platforms and services in the cloud
description: >-
  When we talk about using SaaS or PaaS (or IaaS or even the new Serverless or
  FunctionAsAService, FaaS) it’s important that we understand…
date: '2018-07-02T16:30:55.944Z'
---

When we talk about using SaaS or PaaS (or IaaS or even the new Serverless or FunctionAsAService, FaaS) it’s important that we understand that security concerns change.

Security concerns don’t change very much based on whether you are using a platform, or infrastructure or just services. They change based on the maturity of the solution and the maturity of the organisation you are using. There is some difference in where the shared responsibility line is drawn, but that’s not necessarily a primacy in security, as there are other concerns that matter.

To use an example, Amazon is a multi billion dollar company, and has a set of robust and audited security processes in place. Honest Bob’s Cloud is not, and probably does not. If we were to compare an AWS SaaS product vs Honest Bob’s IaaS offering, we wouldn’t naturally say that Honest Bob’s IaaS is more secure than the AWS SaaS just because it’s infrastructure and we have more control! To be honest, when we have more to control, sometimes we make security worse because there are many more ways to shoot our own foot.

### What even is SaaS?

The SaaS bucket is something into which we lump a variety of offering from a variety of vendors with massively different security implications. Does Office-365, a large productivity suite, have the same security implications as using a small service like MailChimp for our mailing list delivery? How about using Trello to track our cards? What about taking payments via Stripe?

Security assurance models haven’t kept up with this changing world. It used to be the case that when you outsourced to a vendor, it was a major deal, you could either get an integrator to manage it for you, or you did a lot of work to assess the security of the vendor. Security assurance is often still in this world, of assuming that you need to always ask questions about the physical location of the data center for every little service you manage.

With todays Azure marketplace or AWS marketplace, you can install a database as a service from a third party vendor with the click of a button, and the typical questions about “is the data encrypted”, “where are the admins located” etc are either too slow or not always appropriate for this new world

So what am I saying in essence?

1.  Not all SaaS is equal, so lets talk about specific solutions rather than SaaS vs PaaS
2.  It’s clear that the security implications of relying on a provider for your mailing list solution is different to your decision for a productivity suite, so context is important
3.  Small suppliers are far more likely to suffer issues than large ones, and supplier selection matters. So the conversation must include supplier context too
4.  Where we adopt mature, market leading, capable small services, without customisation and complexity, we save money and effort. (i.e. building our own mailing list provider compared to buying mailchimp).
5.  We may attract some information risk in doing so, but that’s a risk that the SRO/TA etc need to understand and decide to take if appropriate. Generic rules of “you must use SaaS” or “No personal data on SaaS” make no sense, because the context is key

### So how do we decide if a SaaS product is secure?

If a service can be built on a market leading SaaS product, without customization or special relationships or need to maintain it, then we should use it where possible. Sending emails via Amazon SES, arranging video conferences via Appear.In, storing documents in Office 365 etc.

Where there is personal data involved, we need to be confident that the provider has robust security practices to protect that data. It is our liability if we select a bad provider, so we need to be more careful with those, but that confidence can come from someone with a data protection/security hat on looking at the service and agreeing “yes this is common, meets the [NCSC’s SaaS Security Principles](https://www.ncsc.gov.uk/guidance/saas-security-principles) and we think it’s acceptable”.

The more sensitive the data, or the more of it, the stronger a look we need to take. So for something like Trello, which holds very little personal data, sure, we’ll check that they meet those principles above, and get on. For something hosting a database as a service, we might want a technical person working with a security person to give it a good looking at before we approve it, and we might want specific conditions on that, such as configuring it a certain way, or backing it up a specific way.

But at it’s essence, there is no fundamental security reason that you’d not use a SaaS if you’ve already decided that you are willing to outsource your IT to the cloud for anything else. And since you should be using the cloud as your outsourced infrastructure provider, I’d argue you should always be preferring a SaaS solution where it’s possible.

### Myth busting

When you use \***any**\* outsourced service provision that is multi-tenanted, you have the possibility of the law enforcement seizing/viewing/intercepting your data because they are looking for one of the other tenants. This is worse with smaller providers, or providers who don’t protect against this with good client data segregation.

I’ve never heard of it actually happening to a major service, and people like Amazon, Apple, Google, Microsoft and other major cloud providers tend to distribute content to devices in such a way that it’s mirrored (so the removal of a single server doesn’t reduce the availability), and additionally encrypt the contents so the possession of a single server won’t get them any data on it.

Secondly, even in the miniscule worst case, that the law enforcement have seized a server under a warrant because they believe that the information on it is relevant to a case, they are only authorised by the law to search the relevant parts of the machine, and they are not allowed to reveal contents that isn’t covered by the warrant. So we would not consider the device “lost” or the data “made public” in most cases. We might have to report to the authorities that the data is no longer under our control, but it is under the control of somebody who is responsible to the same authorities.

#### What about keeping stuff in our network

I’ve seen people suggesting that running your own PaaS on your own network keeps it safe, because it means that the data stays on the corporate network

The concept of networks being the “security boundary” is no longer the right model. When we are talking about data living within the cloud environment, the networking is all virtual, and considering it an extension of the core corporate network simply adds a lot of additional risks to the core network, and reduces all of the security features of the cloud networking stacks that we could use in future. This Armadillo or Castle metaphor for networks hasn’t been appropriate since network stacks got significantly more capable.

I recommend building separate networks for as small a “zone” as is possible, and use “identity ” to create appropriately encrypted connections between the services as they need to talk to one another. This concept of “identity as the backplane” ensures that we can identify each server, identify the users of the server, and build a stack of proofs that are all signed from the base up. This means that we don’t simply trust that nobody is in the network, but we can assert and verify at all times that the connections between services have high authenticity, as well as use encryption to protect the data in transit.

When it comes to service style solutions, we can authenticate to the service, confirm that the connection is encrypted, and then we need to rely on their assertions about tenant separation. The best SaaS services will be doing the above, and searching for whitepapers or conference presentations from some of the SaaS providers out there will show how seriously they take security
