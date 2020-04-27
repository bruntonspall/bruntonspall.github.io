---
title: Brexit and data transfers
description: >-
  Post brexit, there may or may not be a problem with data being transferred
  across borders. But all of the guidance and people talking…
date: '2019-02-21T16:10:15.448Z'
---

Post brexit, there may or may not be a problem with data being transferred across borders. But all of the guidance and people talking about this seem to have some very confused concepts of terms and the processes involved, making it really hard to get clear guidance for organisations.

### An example organisation

To make this clearer, let us think about a simple example of an organisational problem and see what comes out.

Imagine that we are running MBS Ltd, a news company. We scrape the internet for news articles and then put together topic pages with recent articles. As a user, you can browse the website for free, or you can create an account with your email address, and we will track what articles you read, and create you personalised pages that recommend articles that we think you’ll like.

In this case, the personal data that we collect (and we put in our privacy policy) is:

*   Your email address
*   A list of articles you clicked through to, and “thumbs up and thumbs down” ratings you give to articles

We also use cookies to track anonymous users and build lists of articles that they view or like, so we can data mine that information to determine that “people who liked article X on topic A also liked article Y on topic B”.

We built the system as a simple Ruby on Rails web application, hosted on AWS in the EU-West-1 region. We have an AWS RDS instance that stores all of the data that we use.

### What is a data controller and data processor?

If you are running a company that accepts personal data, such as email addresses, from end users, and you store that information, then you are a data controller.

The data controller is the person or organisation responsible for the data, and responsible for caring for the data and doing to it what they told the user that they would do to it.

In this case, we collect personal data about the majority of users, the cookie information and logs keep IP address and preferences, as well as the details of our signed up users. We are therefore the data controller for this data.

A data processor is anybody who stores data or processes data on behalf of the data controller. In this case, for our customers, we put the data into AWS, who acts as a data processor. We remain responsible for the data while it is in AWS, but AWS processes it on our behalf according to the standard contract that we have with AWS (those Ts&Cs that our CTO clicked through without reading when we first setup the startup).

### Offshoring

Offshoring is a tricky subject, and [Joel has written more about this](https://medium.com/@joelgsamuel/offshoring-the-8th-frontier-b1ce6cf38461), based on some comments of mine. But essentially, if we are a British company, then we have the following legal agreements.

We have a contract with AWS EMEA SARL, a company registered in Luxembourg that says that we allow them to process our data on our behalf. AWS says that [they are processor who will only act on our instructions](https://d1.awsstatic.com/legal/aws-gdpr/AWS_GDPR_DPA.pdf)

In essence, AWS says that it’s our responsibility to ensure that we have a legal right to give the data to AWS, and that they may reproduce the data as needed to move it around the internals of AWS.

We’ve selected the EU region, which means that our data is being held in servers in Ireland. AWS says that it has staff all round the world who potentially help administer their services on their behalf.

### Current legal status

Currently we have a privacy policy and consent from our users to hold their data. Because the UK is within the EEA, the data is being held in the EEA, but an EEA company and not being transferred in or out of the EEA, so everything is fairly simple.

What happens if we leave without a deal?

In the event of the UK leaving the EU without a deal, things start to get a little tricky.

Firstly, [the UK has already said that they will create a “finding of adequacy”](https://www.gov.uk/government/publications/data-protection-law-eu-exit/amendments-to-uk-data-protection-law-in-the-event-the-uk-leaves-the-eu-without-a-deal-on-29-march-2019) for the EEA that says that it is safe and acceptable for UK companies to transfer UK citizen data to an EEA company to be processed. This means that our basic system is fine, we can still transfer data into AWS Europe under the same process as we did before.

The EU has not said that they will necessarily create a finding of adequacy in the other direction, so it may not be possible for an EU company to transfer data into the UK without consent from the users. In our case this is probably fine, because we don’t receive information as a data processor from another company in europe.

End users are still allowed to make their own decisions about transferring data, so an EU citizen who choses to sign up to our service is allowed to hand their data to a UK company, providing our privacy policy and consent clauses are clear to them that they are doing so. This is a lawful basis of processing based on consent.

The problematic part is that data transfers from the EU to the UK might be forbidden by the EU GDPR since the UK will be outside the EEA and won’t have a finding of adequacy.

### What is a data transfer?

The key question here is about what constitutes a data transfer. The [guidance on international transfers](https://ico.org.uk/for-organisations/guide-to-data-protection/guide-to-the-general-data-protection-regulation-gdpr/international-transfers/) is still quite vague, and has holes you can drive a truck through, for example

> Personal data is transferred from a controller in France to a controller in Ireland (both countries in the EEA) via a server in Australia. There is no intention that the personal data will be accessed or manipulated while it is in Australia. Therefore the transfer is only to Ireland.

This use of the words “server” or “intention” don’t make any sense to me. How one measures the intent for data to be accessed or manipulated by a “server”, given that any computer data transfer must involve some level of packet technology that will manipulate the data, store it on a drive, and then access the data to serve it back up. Additionally, storing data on a server outside the EEA that you “don’t intend to be accessed” doesn’t sound like a good defense if the said server is hacked somehow! It certainly doesn’t match up with the ICO’s definition of “processing”.

It’s also worth noting that you can transfer data internationally under a number of different arrangements. What we are worrying about here is the specific transfer of data without consent, without a “legal instrument” and without any of the other exemptions to international transfers. Since using a cloud service provider is commonly perceived to be one of these restricted transfers, that’s what we are worrying about here.

Anyway, if your data is stored in AWS Europe, and your staff in the UK are accessing that data to do their jobs, it’s slightly questionable whether this is a transfer. Reading the guidance, this is clearly not the intention, transfers are clearly meant to be “a transfer for the purposes of processing by a third party”, and access by the original data controller is not a transfer. However, this isn’t made clear and nobody is making it any clearer.

Secondly, it’s entirely unclear whether data provided by EU citizens, who thought it would be stored by a company based inside the EU, are content that when the UK leaves the EU without a deal, that their data can continue to be accessed by the company. If your original privacy policy and consent made clear that this was stored by a UK company, you might be fine, but if you had general terms that just stated that it was a EU organisation that kept data inside the EEA, it _might_ be arguable that the UK should no longer have access post brexit.

### What about the US?

The EU made a finding of adequacy around US based companies providing:

1.  They are certified under the EU-US privacy shield
2.  They provide a method of legal redress

The first is pretty simple, since Privacy Shield is a self certification process from 2016 onwards, almost any US company that wants to do business with EU citizens or process data on behalf of EU companies has self certified.

The legal redress is a bit harder, because privacy shield says that the organisations [must comply with the EU data protection authorities](https://www.privacyshield.gov/servlet/servlet.FileDownload?file=015t0000000QJdg), and if the UK leaves the EU without a deal, then there may be no access for UK citizens to the EU data protection authorities.

Arguably, the terms at the time clearly meant that US companies should comply with the UK ICO (who is the UK’s data protection authority), but this still is legally unclear, and legally unclear is not a very happy place to be.

### So known unknowns?

As it stands right now, there are two major unknowns that we don’t have clarity on.

1.  Can a UK company access data held on EU citizens, that was until now held in the EU once the UK leaves the EU? Will it be different if that data is transfered to the UK now vs if you access the data across the border post brexit?
2.  Can a UK company use the EU-US Privacy Shield agreement post brexit without additional modification?

### What if I move my data to AWS UK region?

Insourcing your data to a different processor based on UK soil before the brexit date would currently be legal, it’s a transfer within the EEA, and a perfectly acceptable form of restricted transfer. Once it’s in the UK, it seems to remain legally acceptable to access it after a no deal exit, and while transfers might be forbidden after an exit, EU citizens can still choose to give their data to you. So if you buy data from an EU company that contains personal data, you can’t conduct a restricted transfer post-brexit (although as before there are other clauses such as consent, contracts and others that you may be able to transfer it under).

However, using AWS UK (or other major cloud providers for that matter) may not solve the problem. Why? Because your contract is with AWS EMEA SARL, a Luxembourg, and thus European country, not with a UK company. While the data might rest on servers in the UK, it’s still definitely under the control of an EU company, and therefore accessing the data could still constitute a transfer from an EU company to a UK company.

The only way you could do this successfully right now would be to host the data with a UK based company who owns servers in the UK, rather than a US/EU company with servers in the UK.

I suspect that a UK company could manage servers in the EU, and this would be far more acceptable than the other way round, as per the transfer example, providing the data was never intended to be accessed or processed in the EU, it would not have been transferred.

This really does tell you that where the data is physically located is of almost no relevance in the ICO’s eyes, it’s entirely about where the organisation is legally based who does the processing, and what the intent is.
