---
title: Event Sourcing is not a messaging integration pattern
author: Carlos Buenosvinos
type: post
date: 2016-08-11T15:46:58+00:00
url: /event-sourcing-is-not-a-messaging-integration-pattern/
featured_image: /posts/images/2016/08/Screen-Shot-2016-08-11-at-13.51.19.png
dsq_thread_id:
  - "5058160717"
categories:
  - DDD
  - PHP
tags:
  - architectural style
  - ddd
  - domain events
  - event sourcing
  - messaging

---
**tl;dr: If you are sending messages to RabbitMQ to communicate one application to another, you are not doing Event Sourcing. You&#8217;re just doing messaging ;)**

In the last two months, I&#8217;ve seen different nice talks using DDD, CQRS or Event Sourcing in the talk title. However, after watching the content, I think there are some general misunderstandings about some of the concepts. The most important one, in my honest opinion, is calling Event Sourcing to sending messaging to other applications using a broker. Event Sourcing is not about that. Let me explain a bit more.

<!--more-->

**Event Sourcing** is an architectural style for building applications. You can implement a web application, a game or a database server using Event Sourcing. When implementing a Bounded Context, you can use different architectural styles:

  1. **Spaghetti Architecture**: I&#8217;m a Rasmus Fanboy (aka fuck phptherightway)
  2. **Framework Fanboy**: I&#8217;m a Fabpot Fanboy (aka coupled to my framework)
  3. **Hexagonal Architecture**: I&#8217;m an Alaistar Cokburn Fanboy (aka decoupled from my framework)
  4. **Hexagonal Architecture with CQRS**: I&#8217;m an Alaistar Cokburn and Martin Fowler Fanboy (aka decoupled with performance tunnings)
  5. **Event Sourcing with CQRS**: I&#8217;m a Greg Young Fanboy (aka I do not know what I&#8217;m doing)

Enough jokes! Event Sourcing is about representing the state of your Domain computing all the Domain Events that have happened in the system. There are tons of benefits: audit, tracking, go back to a specific time, debugging, performance, etc. If you choose to use Event Sourcing, you&#8217;re also forced to use CQRS.

With Event Sourcing, your Aggregates fire Domain Events. These Domain Events are appended to an Event Store. Each Domain Event are processed and generates a denormalizations for each read model (feature that needs to read information). These denormalizations can be done in the main Database, Cache system, secondary NoSQL Database, etc. As many as needed. With this approach, our applications don&#8217;t do the work when reading (lots of queries, caching, invalidation, etc.) but when writing (denormalize). When you need to fetch an Aggregate, you load all the Domain Events for that specific Aggregate and apply them to it. If you&#8217;re not doing this, you&#8217;re not doing Event Sourcing.

Apart of the architectural style you choose, you can decide to use messaging for integrating different applications. There are other alternatives: REST, SOAP, SOA, etc.

To sum up, you can implement a single web application using Event Sourcing without publishing any message to any other application. On the other way around, you can integrate two different web applications written with Spaghetti Architecture using messaging.

Hope it helps!