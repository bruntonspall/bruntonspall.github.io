---
title: Dealing with major security issues
description: >-
  So a few days ago, a new vulnerability, yet again with interesting names,
  Meltdown (pdf) and Spectre (pdf) was announced.
date: '2018-01-09T18:14:36.743Z'
---

So a few days ago, [a new](https://googleprojectzero.blogspot.co.uk/2018/01/reading-privileged-memory-with-side.html) vulnerability, yet again with interesting names, [Meltdown](https://meltdownattack.com/) ([pdf](https://meltdownattack.com/meltdown.pdf)) and [Spectre](https://spectreattack.com/) ([pdf](https://spectreattack.com/spectre.pdf)) was announced.

I’ve seen a lot of panic, news articles and paranoia around, and of course people are asking whether they are affected and starting to panic about how to patch all their systems and deal with the problem.

In this article, I’m going to outline a brief response process that you should go through to understand how to deal with such an issue, and I’ll show a worked example at the end for the Spectre/Meltdown vulnerabilities, for what I would recommend for some organizations.

This is primarily written for the CISO level, security management type people. I’m assuming that you have some technical knowledge, or a security architecture person you can ask to help with the technical understanding.

### Understand the bug and the impact

Before doing anything else, you need to understand what’s behind the vulnerability, and what the impact might be.

### How easy is it to exploit?

First we need to consider how easy the vulnerability is to exploit. That is to say, we know it’s a thing, but how easily can someone use it to actually steal data from our systems or networks.

We normally consider the exploitability of a vulnerability as a product of the access needed, the complexity required, the level of privileges needed and whether user interactions are needed.

#### Access Required

For example, certain vulnerabilities, such as SQL injection, can be conducted remotely, by someone from home, without needing any special privileges. Others, like stealing passwords from disk, might need the user to execute a program on the computer on front of them by say double-clicking an attachment in their email.

In term of access vector, the most common view is that we differentiate between something that can be done remotely, or something that has to be done locally. A remote flaw can be exploited by someone over the network, (or physically distant in certain cases), whereas a local vulnerability requires either the attacker to be physically present, or more commonly, to use the user as a proxy.

In particular, local vulnerabilities require someone to run something on the computer that contains the vulnerability. That normally means they can’t be executed by people arbitrarily over the internet. However, the increasing move to cloud computing means that sometimes you are allowing people to run binaries from remote locations, for example build servers, PaaS offerings, docker containers and so forth.

#### Complexity

Complexity considers how hard it is to get the vulnerability to happen, and how much work is needed to exploit it. Some vulnerabilities only happen in certain specific circumstances, for example, only when certain packets are processed by the network controller. If the attacker is unable to control these circumstances, then the vulnerability is much harder to exploit.

If the attacker requires the user to interact with the vulnerability, to approve system permissions or type commands or something, that is also additional complexity.

It’s also true that many of the attackers interested in your systems are probably not capable of fully understanding the vulnerability. So they might be able to run a script that automates the attack, but the vulnerability requires knowledge of how it works to exploit, then it might not be as serious, as the number of attackers who have that knowledge is much lower.

However, the capability of attackers toolkit’s is increasing over time. What used to be the sorts of attacks that only expert criminal or nation state level attackers could carry out a decade or so ago are now easily scriptable by independent hackers or interested amateurs.

#### Privileges required

Finally, we need to consider what privileges are required. Some systems (most modern operating systems, Linux, Windows 7+, OSX) separate out what actions a local administrator can do, and normal interactions with the computer are prevented by normal users.

For certain remote attacks, such as CSRF, XSS, SQL Injection and so forth, sometimes the specific area of the system that can be attacked might only be accessible by people with privileged accounts, such as administrators. That might reduce the impact of the vulnerability

#### Interaction required

Some vulnerabilities require the user to click something, or interact with the system in some specific way. This tends to mean it’s less likely for an attacker to succeed at their attack. However computers are user hostile and we train users to click through security warnings all the time, and so this is scant protection in most cases

### CVSS Scores

Luckily, we’ve been thinking about this for a long time, and generally security researchers use something called a CVSS score to rank vulnerabilities. This score is based on reviewing the vulnerability against the above 4 criteria, as well as considering the impact of the vulnerability on the Confidentiality, Integrity and Availability of data within the system.

These scores can be much misused, since just looking at a vulnerability and determining that it’s a 5.3 and therefore doesn’t need to be patched, is a bad way to handle vulnerabilities. But the scoring system itself can tell you something about the vulnerability.

### Assessing the impact

So now we know what kind of vulnerability it is (say remote, complex, low privileges required).

We now need to understand the impact. To do this, we need to understand our own IT and digital estate well enough to make reasoned educated guesses about the impact. You don’t necessarily need an asset register, but you do need to be aware of what systems you are running, and how likely they are to be vulnerable to determine whether there is a risk.

Our systems tends to fall into a few categories

1.  Internal IT Estate (Desktops, mobile phones, printers, file shares, smart desk phones etc)
2.  Internal IT Applications (Caseworking tools, room booking systems, primary displays, conference facilities, HR systems, document management systems, intranets)
3.  External public digital services (Websites, digital services, extranets)
4.  External private communications networks (Intranets, VPN’s, legacy data feeds, news systems)
5.  Other miscellaneous IT systems (BYOD devices, HVAC environmental control systems, fire alarms and security systems, CCTV systems)

As a CISO, you need a set of priorities, you need to know which of these systems are business critical, and which are simply important. Anything business critical should have some form of asset register, so you can clearly and easily identify whether they will be affected and what the impact would be.

Systems that are merely important, it’s useful to be aware of, so that if you suspect they might be affected, you can direct people to look into them

Everything else should be handled by routine maintenance and security patching (say on weekly or monthly basis, see patching regimes)

#### What about outsourced systems?

What if you don’t know about your systems, because they are managed by an outsourced partner? This could be because your digital services are managed by a solutions integrator, or because you purchased an enterprise IT system from a Software as a Service provider.

Either way, you are either dependent on your supplier knowing and patching your system, or you need to alert your supplier and request the change via your normal change control process.

For SaaS providers, you should be expecting that they will be patching and staying alert of issues like this, and you should expect to see proactive engagement from them.

For outsourced contracts with an systems integrator, you should either construct the contract such that they have to proactively catch and inform you of relevant vulnerabilities, or you need to be able to inform them that you are concerned and ask them to conduct the change for you.

### Patching regimes

We know that the number one thing that prevents security breaches is patching systems. The primary task of a security team is ensuring that there is an effective patching regime in place, and ensuring that services are patched appropriately.

One thing to be aware of, is that high profile incidents can either distract from the normal patching regime, and create more insecurity that way, or it can be used to sell the patching regime.

As a CISO, you will need to be prepared to know how long regular patches take, and be able to explain that high profile vulnerabilities will be patched within the normal time frames, and isn’t that great value for money and just proving how good your patching regime is!

More importantly, if you get pressure to patch out of process for a vulnerabiity that has low impact, but has a branding campaign and a logo and BBC news article about it, then you need to be able to explain what the impact of the disruption will be on other security activities.

### So how would this work in practice? A look at Spectre/Meltdown

So, the Spectre/Meltdown vulnerabilities are extremely technical and low level. They fundamentally affect the CPU’s inside the hardware in many computers in use today.

The attacks in this case are in a class of attack called a “side-channel attack”. That is to say, instead of attacking something and getting data straight out, you do something and observe the effects of what you do. Side channel attacks are notoriously difficult to predict or protect against, but they are generally also the hardest to exploit.

The two attacks are slightly different, although both are the result of the same underlying vulnerability in processor architecture.

Meltdown is an attack that can be run, as a binary program, on a computer and is able to dump the contents of the physical memory of a machine. Since that memory could contain the secrets or other data from other processes on the machine, it enables the attacker to steal any confidential data from the machine (maybe… see the section on data exfiltration). It however is slightly more easily defended against with a simple operating system patch.

Spectre is a much more complicated attack that targets a specific other running program on your computer, and is able to access data from it while running. However, it needs to be customised for each binary and operating system that you want to attack.

There is a theoretical attack for Spectre included that can execute in javascript, which can be executed by someones browser, but the paper is slightly unclear of the impact of this. It implies that it can read all the data in the browsers memory, but not that it can read any memory from anything outside the browser.

Note that although source code is included for both attacks, neither is truly weaponised to allow easy running on a target system, and tweaking it to work requires a high level of capability.

#### Assessment

Luckily, the assessors have already done some of this work for us. Spectre is at [https://nvd.nist.gov/vuln/detail/CVE-2017-5753](https://nvd.nist.gov/vuln/detail/CVE-2017-5753) and [https://nvd.nist.gov/vuln/detail/CVE-2017-5715](https://nvd.nist.gov/vuln/detail/CVE-2017-5715) whereas Meltdown is at [https://nvd.nist.gov/vuln/detail/CVE-2017-5754](https://nvd.nist.gov/vuln/detail/CVE-2017-5754)

The key thing we are looking at is the string [CVSS:3.0/AV:L/AC:H/PR:L/UI:N/S:C/C:H/I:N/A:N](https://nvd.nist.gov/vuln-metrics/cvss/v3-calculator?name=CVE-2017-5753&vector=AV:L/AC:H/PR:L/UI:N/S:C/C:H/I:N/A:N)

This tells us that the access vector is local, the attack complexity is high, the privileges required are low and the user interaction required is None.

This string is the same for both vulnerabilities, as far as I can tell.

Across our estate, we now know that we should be concerned about any IT system that runs on an X86 processor (and the vulnerabilities show which ones), we probably don’t even need an asset register to know that it’s almost certainly our entire estate :(

However, the access vector is local, meaning that the computer has to be told to run a program. We can’t exploit these vulnerabilities remotely by default.

This means that any IT systems where the running of arbitrary programs is not possible, or hard, are lower priority than ones where running programs is easy.

Looking at our estate, that probably means that the majority of our digital cloud estate, our SaaS products, and our main servers are low priority.

Users run programs all the time, and since the privileges requires are low, a user might not even realise that they have run a vulnerable program (it won’t pop up and ask for administrator rights the way some programs will). So our desktop and mobile estate is probably the highest priority for us.

If we run services on behalf of clients, if we run a PaaS, a container solution or anything where we allow untrusted users to run programs, then those services are high priority as well. But everything else can probably wait a bit.

Since there were patches out quite quickly, we should initiate emergency patching of our desktop estate as soon as possible. That’s pretty invasive for our users.

It’s worth remembering that Spectre is a much harder attack to expoit, and needs to be targetted against the specific binary that it’s leaking memory from. Sadly, there are a bunch of interesting standard binaries on many computers that could be targetted, but if you are running anything unusual you are only at risk from the most capable attackers (or at least in the short term).

#### Attack Chains

Attack chains are a set of different attacks, each of which gives the privileges to execute the next attack. For example, an arbitrary file upload vulnerability in a web system, combined with a command injection vulnerability allows the attacker to upload a vulnerable binary and to then execute it.

While we might say that our services don’t run arbitrary binaries, if someone can find a separate vulnerability to allow exactly that, then they can chain the two vulnerabiliites together, making you remotely vulnerable to this attack. In the case of a chain, breaking any one link should break the chain, so keep an eye on those digital service penetration tests and make sure that you close any file upload or command execution vulnerabilities.

#### Data exfiltration is still a thing

Lastly, it’s worth remembering that just because an attack has managed to execute code on your machine and dump the memory, doesn’t mean that they have all your data. It’s still on your machine, and they need to get it off. That means that they’ll need to put the data somewhere and then transmit it to themselves, either over the internet or via some physical device. Good network security and sensible policies should mean that machines with especially sensitive information cannot talk to the internet, and this attack wont therefore leak data. Of course, many computers need to talk to the internet for the users to do their jobs, so you need to decide how proportionate this defense is.

Even if they get the data off, they still need to look through the data looking for interesting things. Because they have a side channel attack, they probably can’t guarantee that they’ll get any specific data, and they might not know where it is in the data dump, unless you were targetted specifically and the attacker knows what to look for.

So cheer up, it could be worse. Keep patching your machines, in priority order and it’ll all be ok.
