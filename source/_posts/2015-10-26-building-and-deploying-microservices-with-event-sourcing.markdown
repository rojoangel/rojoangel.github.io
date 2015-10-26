---
layout: post
title: "Building and Deploying Microservices with Event Sourcing - by Chris Richardson @ InfoQ"
date: 2015-10-26 22:53:00 +0100
comments: true
categories: [reading, presentation, event, event sourcing, cqrs, docker]
hidden: true
---
I've just watched this presentation by Chris Richardson about [Building and Deploying Microservices with Event Sourcing](http://www.infoq.com/presentations/microservices-docker-cqrs).

These are notes to myself taken while watching the presentation:

Monolithic apps lock you in the technology you choose at the beginning of the project.

Events used to achieve eventual consistency across distributed services / datastores:
- Microservices publish events when state changes
- Microservices subscribe to events
  - maintain eventual consistency (multiple aggregates in multiple datastores)
  - synchronize replicated data
- An event needs to be published atomically every time a domain entity changes its state

Event sourcing: for each aggregate persist the events that lead to a particular state instead of the state itself

Persisting events: json is a good choice because of its loose mapping mechanism

Optimize by using snapshots
- serialize a memento of the aggregate
- load latest snapshot + subsequent events

Business benefits of event sourcing:
- built-in audit log
- enables temporal queries
- preserved history

Technical benefits:
- No more O/R mapping - we are just persisting events

Drawbacks:
- Handling duplicate events / out-of-order

Think of a microservice as a DDD aggregate

For view requests use CQRS & denormalized views

Use an Event Archiver that subscribes to all events: enables analytics.
