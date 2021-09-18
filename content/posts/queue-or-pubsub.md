---
title: "Queue or Pub/Sub"
date: 2021-09-18T13:57:44+08:00
---
At one point in my career, we had to decide whether a system should be using queue or Pub/Sub as a message broker. Back then, I still couldn't differentiate between the both. Queue seems like a data structure that is used in both types of message broker, and Pub/Sub is a mechanism of push and pull, or push and push, between two or more systems.

After spending more time reading up on event-driven architecture (EDA), and coming across different sets of nomenclature that describe message broker, I think I have a clearer picture on both. Let's first talk about the message broker itself.

## Message broker, the middleperson

A message broker is generally used to encourage decoupling between multiple systems, among other myriad benefits. Instead of systems communicating to each other directly, they each publish their message into a message broker, and the other party would receive the message via the broker.

The broker plays an instrumental role here, since it serves as the only interface between the systems. And, I think, it is the configuration and purpose of this broker that matters, not whether it is a queue or Pub/Sub.

## Broker - Command bus

If a broker is configured to deliver a message exactly once to each interested party, then it is a **command bus**.

A command bus usually serves as a buffer, with the intention to prevent one system from overloading the other system with its commands. The consuming side of the command bus is usually known.

Example use case would be a system attempting to send 10,000 emails. It would first place the email messages on the command bus, which the email dispatcher system can process the requests at its own pace from the bus.

## Broker - Event bus

If a broker is configured to store a history of messages, i.e. having a longer retention, then it is an **event bus**.

This type of broker acts as a ledger for events that have occured. For instance, an order is created, a user has signed up, and etc. As a ledger, it's main purpose is to store the records. It does not know who would consume it. Some systems could consume from the broker to react to the event, but a sink could also be consuming from it, persisting the event logs for Event Sourcing.

## Is it queue, or Pub/Sub, or?

Most of the time, a command bus is referred to as a queue, while an event bus as a Pub/Sub system. So when someone says to use a queue, they mostly mean to use a command bus.

I feel the terms *command bus* and *event bus* are much more suitable to describe a message broker. Regardless of the type of bus, they all have some sort of queue implementation to buffer and order the messages that they receive.

In a Pub/Sub system, a sender publishes message to the broker, and a receiver subscribes to the broker to receive the message, and this is usually the mechanism behind an event bus. But it is possible for a command bus to implement Pub/Sub pattern as well, right?
