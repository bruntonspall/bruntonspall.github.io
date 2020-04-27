---
title: The basics are sometimes the hardest things
description: >-
  If only we could apply patches, then we could do more interesting security
  work.
date: '2018-11-07T23:06:44.740Z'
---

If only we could apply patches, then we could do more interesting security work.

I’ve as guilty as the next person of over-simplifying how easy the basics actually are. We exhort and condemn organisations and people for not getting the basics right. “Having a company vision is just table stakes”, but we forget how how hard it can actually be to execute the basics well.

If we take patching, it kind of sounds easy to just apply patches to our software. But to do that we need to know what software we have deployed, and the moment you scale up from one team to 5 teams, you are going to lose that visibility.

Even if I can keep some visibility of what systems I have, I also need to pay attention to what software I’m running, what version it is, and what patches are actually available.

With multiple service delivery teams, each using a slightly different technology stack, we now need to track whether we are patching ruby, node.js, linux, windows, vmWare, Xen and a whole host of other software.

If we add in the complexity of a typical corporate estate, desktops, email, productivity tools, mail clients, instant messenger tools, collaboration tools as well as whatever else has been installed to help our service teams deliver, patching turns out to be a lot harder in practice than it sounds as a trite soundbite.

We should patch early, and patch often, and we should aim to get the basics right before we start investing in the machine learning quantum advanced threat protection that the vendor wants to sell us, but we shouldn’t deride people for finding it hard.

_\[This blogpost is part of an attempt to blog once a day for the entirety of November (#NaBloPoMo), inspired by_ [_Terence Eden_](https://medium.com/u/1b3280d2beeb)_. This series of blogposts are therefore unedited and written on the day. Errors and Omissions Excepted\]_
