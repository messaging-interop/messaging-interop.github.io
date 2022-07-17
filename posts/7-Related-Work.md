---
title: 7. Related Work
description: 
date: 2022-07-07
tags:
layout: layouts/post.njk
---
This sketch draws on a wealth of prior work that has has gotten us where we are today. This architecture draws inspiration from (and uses as a security primitive) ratcheting message security keys from Double Ratchet and MLS.

Message transmission architecture has inspiration from onion routing, and is adapted for the different nature of store and forward messaging from client-server sessions. Address diversification is structurally similar to onion routing, and I expect rate limiting between receiving and sending services will draw on operational experience from Tor nodes as well as email domain reputation heuristics.

Indeed, a conceptual ancestor to onion routing is David Chaum’s proposal for anonymous communication using mixnets:
https://dl.acm.org/doi/pdf/10.1145/358549.358563

I’ve also found helpful (if time-shifted) discussion from Trevor Perrin’s Modern Messaging mailing list archives and the many active participants there who have raised ideas and feedback relevant to different aspects of this sketch.
[The Messaging Archives](https://moderncrypto.org/mail-archive/messaging/)
