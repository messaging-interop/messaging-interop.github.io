---
title: 4. Ideas
description: 
date: 2022-07-05
tags:
layout: layouts/post.njk
---
We've now described enough of the architecture and mechanics to discuss the core goals and ideas

## Goals
The core goal of this architecture to support the store and forward delivery of discrete messages, end to end encrypted between people as represented by cryptographic identities.

These messages should have a size limit sufficient to allow for structured objects conveying Unicode encoded text, cryptographic keys, and URI's, but should not be a mechanism to transmit arbitrary packetized data. Each message should correspond to a human action (modulo automated state handling -e.g. an ack- of equal but no greater magnitude) - this forms a natural rate limit.

The idea is rooted in messaging, but extends to things like emails, sharing invitations, moves in a turn-based game. It is not, for example, a real-time streaming format for audio, video, or movements in a real-time game. However, it can be used to exchange data needed to bootstrap a streaming session between members of a proton relationship.

There is no guarantee of ordering, nor should clients attempt to fit large payloads in multiple messages - messages that exceed the base size (pictures, long emails, documents) should be handled as [attachments](/posts/5.3-Attachments) as they commonly are today.

Organizations with one to many relationships can fit into this framework, discussed further in [Organizational Identity](/posts/5.3-Organizations)

## Deconstructed Messaging

For simplicity, a messaging service combines several different pieces of functionality:
- Authentication - each service manages its own set of identities, tied to an external identity (phone number)
- Message transport 
- Client application that stores the user's messages after receipt, and has some variation in custom message types.

Traditionally, identity has also been bound up with transport. To issue phone numbers, you must be a carrier. The issuer of an email identity are the parties that route the email. This both places a high bar on issuing identities, and binds identity to the service. There's been mixed success in the ecosystem of mail client applications - recent entrants have depended on the integration of client app and the email service.

For conversations to be portable, they should handle identity independently from transport. For conversations to be interoperable, users need to be able to make independent choices about service providers and client applications. 

So instead of an architecture where members of different ecosystems can message each other, in the way that SMS gateways glued SMS to other messaging systems,[^1] let's deconstruct the functionality. For Alex and Bob to communicate, they need each of the following pieces:

### Authentication
Alex and Blair need to exchange identities. We [previously](/posts(3.1-Identity)) outlined the ways that people can exchange cryptographic identities like they do phone numbers. 

### Transport
Alex, from a device, wants to send a message to Blair using that identity. Individual devices are imperfectly online - we want to offload the store and forward to online services that act as online agents for our devices. 

Alex's device, then, should engage a persistently online sending service to offload outgoing messages from their device. That sending service can then attempt & retry delivery to the remote party's online agent for receiving messages.

These agents have asymmetric roles. Sending services are relatively simple - their role is to take a message from a device and faithfully deliver it to another server endpoint. The [reference implementation] discusses how sending services can authenticate users without authenticating each message.

Receiving services have a more complex role, since they need to distinguish among incoming messages, which users to route those messages to. Their responsibiity is to keep that knowledge private, by suppring a [virtual address scheme]((/posts/4.4-Transit-Privacy)) that allows senders to use different addresses for each message.
They may engage in strategies to reduce their knowledge about this mapping - a maximalist strategy would be to deliver every message to every user.

## Reconstructing

Systems that allow users to make complex choices should not force everyone to - progressive disclsoure is essential to accomodate everyone. How do we make this architecture easy for people to use?

### Services

The core assumption that the architecture is sized for human-initiated, text and code payloads ensures that it should be economical to provide at scale. Both Apple and Google provide a free receiving service for their mobile OS's in the form of push notification services[^2] - which end to end messaging systems already run on top of for the last leg of delivery to mobile devices. 

Operating System vendors and app developers should be able offer to a free messaging tier of sending and receiving services. For redundancy and recoverabiity, users should have multiple ones - and the architecture allows people to use and change them interchangeably and invisibly to the conduct of their conversation.

Attachments have more variable resource usage, and different economic models might support greater capacity to send attachments.

### Software

A relationship manager is the core software application that people will interact with. It combines functions that people today perform in contacts apps, password managers. To reduce complexity, it should come with a default sending and receiving service, and implement basic functionality to exchange short messages and long mails.

Given that relationship managers are responsible for safeguarding a user's cryptographic identity secrets, and synchronizing them between trusted devices, it logically follows that the relationship manager should also be responsible for storing and synchronizes the decrypted messages. This allows for a consistent expectation about the security of a user's communications, across modes, and across devices.


[^1]: SMS Gateway https://en.wikipedia.org/wiki/SMS_gateway
[^2]: APNS (iOS) and Android both have a payload limit of 4KBhttps://developer.apple.com/library/archive/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/CreatingtheNotificationPayload.html. https://firebase.google.com/docs/cloud-messaging/concept-options
