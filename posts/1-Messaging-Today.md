---
title: 1. Messaging Today
description: What can we improve on in messaging today?
date: 2022-07-01
tags:
layout: layouts/post.njk
---
Today, when I talk to people on the internet, I use static digital addresses - phone numbers and email addresses - to do so, even if I’m not actually using PSTN or email.  And when I create accounts in systems, that digital address is usually a recovery factor - or in some cases is the only authentication factor.

We’ve bootstrapped accounts and messaging system off these addresses because they have these useful properties:
- **Human-rememberable strings** - I can remember and give out my own addresses.
- **Global, federated namespace** - There are many competing providers of phone and email service. Both email and SMS provide a resolution mechanism where people can message each other without having to understand the other’s choice of service provider.
- **Robust Identity Recovery** I can use a phone number or email address to recover an account. I can generally also recover access to my phone number or email address if I destroy my SIM card and forget my passwords, by convincing a  service provider that I am still the same person.

But they have significant issues:
- These systems share a basic assumption that anyone who has an address, has permission to message that user.
  - This is rooted in a lack of prearrangement about the identity I will receive messages from when, e.g., I enter an email address or phone number on a form. Socially, people make that arrangement when exchanging phone numbers.
  - So while systems attempt to filter unsolicited messages by an allow-list that doesn’t work for email, and is an incomplete answer as people will continue to receive solicited messages and calls from unknown addresses.
- Weakly portable
  - Many countries require that phone numbers be portable - this helps enable competition so that people aren’t locked into a particular provider for their identity
    - Yet numbers are not portable across countries
  - Emails are by design not portable. Often they confer some association with the institution. If one can manage holding on to an address, forwarding allows some weak portability.
  - When people move internationally, graduate from school, or change their ISP, they often discover how many relationships are based on these addresses, and how much work it is to transition to new addresses.
- A single, global identity allows people to be tracked unexpectedly across different contexts
  - Virtual email addresses are one approach to mitigating email-based tracking.

With years of experience building and using these systems, we can imagine an [evolved messaging architecture](/posts/2-Vision), that centers on people’s relationships.

[^1]: [The Double Ratchet Algorithm](https://signal.org/docs/specifications/doubleratchet/) https://signal.org/docs/specifications/doubleratchet/
