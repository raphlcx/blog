---
title: "Queue or Pub/Sub"
date: 2021-09-18T13:57:44+08:00
---
While working as a backend software engineer, my team had to decide whether a system should be using a queue or Pub/Sub as the message broker. Back then, I still couldn't differentiate between them both. A queue seems like a data structure used in message brokers, while Pub/Sub is a mechanism of push and pull, or push and push, between two or more systems. How can you only choose one of them?

After reading more on event-driven architecture (EDA) and coming across different sets of terms that describe message brokers, I think I have a clearer picture of both. Let's first talk about the message broker itself.

## Message broker, the middle person

A message broker is generally used to encourage decoupling between multiple systems, among other myriad benefits. Instead of systems communicating to each other directly, they each publish their message into a message broker, and the other party would receive it via the broker.

The broker plays an instrumental role here since it serves as the only interface between the systems. In my opinion, it is the configuration and purpose of this broker that matters, not whether it should be a queue or Pub/Sub.

## Broker - Command bus

A broker that delivers a message exactly once to each interested party is a **command bus**.

A command bus serves as a buffer to prevent one system from overloading the other system with its commands. The consuming side of the command bus is usually known.

An example use case would be a system attempting to send 10,000 emails. It would first place the email messages on the command bus, which the email dispatcher system can process the requests at its own pace from the bus.

## Broker - Event bus

A broker that stores a history of messages for longer retention is an **event bus**.

This type of broker acts as a ledger for events that have occurred. For instance, order creation, user sign up, etc. As a ledger, its primary purpose is to store the records. It does not know who would consume it. Some systems might consume from the broker to react to the event, but a sink will subscribe to the broker to persist the event transactions for Event Sourcing.

## Is it a queue, or Pub/Sub, or?

From my observation, we usually refer to a *command bus* as a queue while an *event bus* as a Pub/Sub system. So when someone says to use a queue system, they mostly mean to use a command bus.

These terms are much more suitable to describe a message broker. Regardless of the type of bus, they all have some form of queue implementation to buffer and order the messages received.

In a Pub/Sub system, a sender publishes messages to the broker while a receiver subscribes to receive them. An event bus usually operates via this mechanism. But a command bus, commonly known as a queue, could work similarly as well, right?
