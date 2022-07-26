---
title: 4. Ideas
description: 
date: 2022-07-05
tags:
layout: layouts/post.njk
---
We've now described enough of the architecture and mechanics to discuss the core goals and ideas.

## Goals
The core goal of this architecture is to support the store and forward delivery of discrete messages, end to end encrypted between people as represented by cryptographic identities.

These messages should have a size limit sufficient to allow for structured objects conveying Unicode encoded text, cryptographic keys, and URI's, but should not be a mechanism to transmit arbitrary packetized data. Each message should correspond to a human action (modulo automated state handling -e.g. an ack- of equal but no greater magnitude) - this forms a natural rate limit.

The idea is rooted in messaging, but extends to other data that people can exchange like emails, sharing invitations, call invitations, or moves in a turn-based game. It is not a real-time streaming format for audio, video, or movements in a real-time game, which should utilize a streaming transport. However, it can be used to exchange data needed to bootstrap a streaming session between members of a proton relationship.

There is no guarantee of ordering, nor should clients attempt to fit large payloads in multiple messages - messages that exceed the base size (pictures, long emails, documents) should be handled as [attachments](/posts/5.3-Attachments) as they commonly are today.

Organizations with one to many relationships can fit into this framework, discussed further in [Organizational Identity](/posts/5.3-Organizations)

## Deconstructed Messaging

For simplicity, a messaging service combines several different pieces of functionality:
- Authentication - each service manages its own set of identities, tied to an external identity (phone number)
- Message transport 
- Client application that stores the user's messages after receipt, and implements a variety of message payloads - standard (images, links) and custom (payments)

Traditionally, identity has also been bound up with transport. To issue phone numbers, you must be a carrier. The issuer of an email identity are the parties that route the email. This both places a high bar on issuing identities, and binds identity to the service. There's been mixed success in the ecosystem of mail client applications - recent entrants have depended on the integration of client app and the email service.

For conversations to be portable, they should handle identity independently from transport. For conversations to be interoperable, users need to be able to make independent choices about service providers and client applications. 

So instead of an architecture where members of different ecosystems can message each other, in the way that SMS gateways glued SMS to other messaging systems,[^1] let's deconstruct the functionality. For Alex and Bob to communicate, they need each of the following pieces:

### Authentication
Alex and Blair need to exchange identities. We [previously](/posts(3.1-Identity)) outlined the ways that people can exchange cryptographic identities like they do phone numbers. 

### Transport
Alex, from a device, wants to send a message to Blair using that identity. Individual devices are imperfectly online - we want to offload the store and forward to online services that act as online agents for our devices. 

Alex's device, then, should engage a persistently online sending service to offload outgoing messages from their device. That sending service can then attempt & retry delivery to the remote party's online agent for receiving messages.

These agents have asymmetric roles. Sending services are relatively simple - their role is to take a message from a device and faithfully deliver it to another server endpoint. The [reference implementation](posts/6.3-Sending-Service) discusses how sending services can authenticate users without authenticating each message. They have related functionality 

Receiving services have a more complex role, since they need to distinguish among incoming messages, which users to route those messages to. Their responsibiity is to keep that knowledge private, by suppring a [virtual address scheme]((/posts/4.4-Transit-Privacy)) that allows senders to use different addresses for each message.
They may engage in strategies to reduce their knowledge about this mapping - a maximalist strategy would be to deliver every message to every user.

## Reconstructing

This is a flexible system that allows users to make complex choices - but it is essential to have progressive complexity that doesn't require everyone to make these choices to use the system.

People's primary or initial interaction with this architecture will be through the relationship manager app, as the way that they start relationships and communicate within them. It serves a set of functions that combine what people typically do within their messaging, contacts, and password manager apps. For simplicity, these apps should come with default receiving, sending, and identity attestation services out of the box, just as messaging apps do today with their backing service.

### Services

Sending and receiving services should be cheap to run - they are scaled for human-initiated text and code payloads. Both Apple and Google provide a free receiving service for their mobile OS's in the form of push notification services[^2] - which end to end encrypted messaging systems today already run on top of for the last leg of delivery to mobile devices. A free tier might include an attachment service sufficient to allow users to send text attachments and images as existing services provide today. Just as with cloud hosting, a variety of economic models might support higher tiers of utilization.

Key to this assumption is the distinction that sending and receiving services are responsible for message transport (and optionally hosting attachments), not the storage of the resulting plaintext.

Because the relationship manager is responsible for the security of a person's private keys for their cryptographic identities, and for the backup and recovery of the corresponding relationships, it should also be the default agent for cloud storage and backup of a user's decrypted messages.


[^1]: SMS Gateway https://en.wikipedia.org/wiki/SMS_gateway
[^2]: APNS (iOS) and Android both have a payload limit of 4KB: https://developer.apple.com/library/archive/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/CreatingtheNotificationPayload.html  https://firebase.google.com/docs/cloud-messaging/concept-options
APNS provides a rudimentary form of virtual addressing, in which different developers get different addresses for the same device, to prevent tracking of user activity across apps.
