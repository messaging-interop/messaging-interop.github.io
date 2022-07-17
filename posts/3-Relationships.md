---
title: 3. Relationships
description: Describing the relationship, the fundamental unit of this architecture
date: 2022-07-03
tags:
layout: layouts/post.njk
---
As hinted at, we make relationships the fundamental unit. I don’t have one identity, I have contextual identities defined by my role in a relationship.

A **relationship** $$R$$ between Alex and Blair is a pair of data structures $$P_a$$ and $$P_b$$, stored by Alex and Blair respectively, that represent each end (role) of the relationship, and contain the data needed to communicate with the other party. Let’s call $$P_a$$ and $$P_b$$ **Protons**, to reflect the intuition that they are entangled (through message exchange) and reflect shared state.

*Protons are a conceptual extension of an idea from Double Ratchet’s ratchet keys, that people exchange and automatically update complementary cryptographic state to encrypt and authenticate asynchronous messages. In addition to cryptographic state, Protons also update their delivery addresses and other identity metadata through message exchange.*

$$P_a$$ consists of:
- A human-readable identity for $$P_b$$:
  - A string representing a preferred name
  - Optionally, pronouns
  - Optionally, a profile picture
- An Identity Public Key ($$IPK_B$$) for $$P_b$$
  - Optionally, an external resource that attests to a binding between $$IPK_B$$ and some external identity (e.g. a scoped username - \@B on Twitter)
- An Identity private key for Pa corresponding to the $$IPK_A$$ held by $$P_b$$
- Message security state variables
  - This would be Double Ratchet chain keys for 1:1 conversations and MLS for group conversations
- A list of transport mechanisms (minimum 2), that each contains
  - A URL representing the service identity and a means to obtain the service's configuration:
    -  E.g. example.com/.well-known/Proton.json
    (analogous to an MX record)
  - A long-term delivery address for $$P_b$$ (this facilitates recovery)
    - With a guaranteed valid until date
  - An ephemeral delivery address for $$P_b$$
    - With a (closer) guaranteed valid until date
- A set of associations with other related protons (relationships)defined by cryptographic attestation - see [Identity](/posts/3.1-Identity).

## Architecture
For Alex and Blair to exchange messages through $$R$$, the key functions required are:
- A relationship manager client running on the user’s device(s) that:
  - Exposes user interfaces to manage relationships through their lifetime (create, update, introduce, merge, end)
  - Decrypts incoming messages, and encrypts outgoing messages for relationships it manages
  - Stores a user’s message history
  - Optionally,
    - Synchronizes these relationships across multiple devices
    - Provides backup and recovery of a user’s relationships
- A message receiving service that accepts services on behalf of a user and routs them to the user's devices.
- A message sending service that serves as an agent for outgoing messages from devices running a relationship manager.

Alex and Blair do not have to agree on relationship managers, or message sending and receiving services. Different users's software and services must only [interoperate](/posts/4.1-Interoperability) in these ways:
- Message receiving services must accept arbitrary messages from message sending services and deliver them to the addressee
- Relationship managers must be able to encrypt and decrypt messages with other relationship managers and process header information intended for the relationship manager.

### Optional Components
These are not required, but extend the functionality of the system to match how people commonly exchange messages:
- Attestation Service
  - This asserts authority over identities within a namespace (E.g. users on Twitter, or members of a school), and attests to bindings between those namespace identities, and a cryptographic identity ( an Identity Public Key)
    - E.g.: ("This is the IPK for @example on Twitter")
  - This should also provide addresses and pre-key bundles to facilitate asynchronously starting conversations with these users.
  - The service may use Key Transparency to prove its honest issuance of a consistent key directory.
- Client Application
  - People may use the relationship architecture to exchange messages of a particular application type. For example - payments, or invitations to a real-time end to end encrypted video call. The Relationship Manager does not need to itself implement the rich functionality such applications provide, but it can provide a secure, authenticated mechanism to exchange those payloads across existing relationships.
  - In a commercial relationship, where the remote party has an application of their own, the user can choose to route payloads in that relationship to the corresponding app on their device. E.g. a push notification for a bank that is delivered to my banking app so that I can permit or deny a transaction.
  - People may also wish to use a different client application to read and write messages. Operating systems should help protect users from message exfiltration by malicious or compromised client apps.

A relationship manager may have default sending and receiving services, but standardized interfaces would allow users to configure a receiving and sending service of their choice.