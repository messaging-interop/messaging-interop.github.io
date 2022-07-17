---
title: Overview
description: Overview of the series
date: 2022-06-30
tags:
layout: layouts/post.njk
---
1. [Messaging as it exists today](/posts/1-Messaging-Today)
2. [A vision for a future of messaging](/posts/2-Vision)
3. [Core Concepts - Relationships](/posts/3-Relationships)
  - [3.1 Identity](/posts/3.1-Identity)
  - [3.2 Conversations](/posts/3.2-Conversations)
  - [3.3 Groups and Introductions](/posts/3.3-Groups)
  - [3.4 Multiple Devices](/posts/3.4-Devices)
4. [Ideas](/posts/4-Ideas)
  - [4.1 Interoperability](/posts/4.1-Interoperability)
  - [4.2 Trust](/posts/4.2-Trust)
  - [4.3 Unsolicited Messages](/posts/4.3-Unsolicited-Messages)
  - [4.4 Message privacy in transit](/posts/4.4-Transit-Privacy)
5. Other parts of the ecosystem
  - [5.1 Backup](/posts/5.1-Backup)
  - [5.2 Self-introductions](/posts/5.2-Self-introductions)
  - [5.3 Organizational Identity](/posts/5.3-Organizations)
6. Sample Implementations
  - [6.1 Relationship Manager](/posts/6.1-Relationship-Manager)
  - [6.2 Receiving Service](/posts/6.2-Receiving-Service)
  - [6.3 Sending Service](/posts/6.3-Sending-Service)
7. [Related Work](/posts/7-Related-Work)

Today, secure messaging at scale (Signal, WhatsApp, and iMessage) sits in centralized ecosystems, each of them layering different crytographic identities atop the identity of a person's phone number.

In practice, the fracturing of people’s social relationships among those ecosystems results in the frustrating experience of using multiple messaging apps simultaneously - and worse, fracturing conversations with the same person across multiple apps. There’s been a renewed interest in interoperability, spurred by the EU’s requirement for such in the DMA. [^1] [^2]

Rather than simply guling these systems together, I propose a vision of a modern messaging system that provides:
- Unified identity across modes of conversation
  - I should be able to use a single identity to exchange messages or email - my identity shouldn't depend on the app I use or the underlying transport technology.
- Diversified identity across different conversations
  - My ability to separate conversations and role-based identities is not limited to how many phone numbers or email addresses I have.
- Interoperability and Portability
  - People can communicate without having to agree on an app or service.
  - People can change apps and service providers and seamlessly continue a conversation.
- Security and Privacy
  - Messages are end to end encrypted between identities
  - Messages are transmitted with ephemeral addresses
- Protection against unsolicited messages

### Motivation

I am not my phone number or email address(es). But these digital addresses have become our de facto online identities. And while they have become so for very [good reasons](/posts/1-Messaging-Today), singular delivery addresses make it cumbersome to manage the many different identities we have in our lives - with family, friends, classmates, coworkers, and all the other social circles we inhabit. People go to great lengths and costs to acquire additional phone numbers or email addresses to keep those social circles separate.

This assumption that users have a single or primary identity is baked into much of our software and services, including cryptographic identity proposals and services.

Virtual addresses like email aliases are a start to a world where addresses no longer define our identity, but are in service of the many roles we inhabit. But in practice, they also demonstrate the difficulty of managing partitioned identities in an ecosystem that assumes that your address is your identity.

What if, instead of starting from the notion of a single identity, we think about a pairwise relationship as the fundamental unit? I have a relationship with another person, though which we have a conversation in many different mediums - messaging, email, or real-time audio and video. My contextual identity is the role I inhabit in this relationship. The technologies and addresses of the different mediums are just implementation details - details that I have to manage today manually in a Contacts app.

I have many such relationships, possibly with the same person. Alex and Blair could be coworkers, and competitors in a game, and may want to keep those conversations separate.

We can build a new communications architecture that more naturally maps to our relationships. 

[^1]: [End-to-End Encryption and Messaging Interoperability](https://educatedguesswork.org/posts/messaging-e2e/) https://educatedguesswork.org/posts/messaging-e2e/

[^2]: [How do you implement interoperability in a DMA world? | Matrix.org](https://matrix.org/blog/2022/03/29/how-do-you-implement-interoperability-in-a-dma-world) https://matrix.org/blog/2022/03/29/how-do-you-implement-interoperability-in-a-dma-world

