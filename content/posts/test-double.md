---
title: "Test double"
date: 2024-11-12T22:13:22+08:00
---

A test double is a broad term for various techniques used to simulate objects in software testing. It includes the following types:

  - Dummy
  - Fake (e.g., using an in-memory database for testing)
  - Stub
  - Test spy
  - Mock

## Mock

A *mock* is a dummy object with pre-defined behaviors, such as expecting specific method calls and returning particular responses when called. Its verification is done through behavior verification.

Example: If a person needs food, you create food, expect it to be eaten, and confirm when the person eats it.

## Stub

A *stub* is a dummy object that resembles the actual implementation but is used as a substitute. It is verified through state verification.

Example: If a person needs food, you create food, pass it to the person, let them eat it, and verify afterward that there is no remaining food.

## Test spy

When behavior verification is applied to a stub, it becomes a *test spy*.

Example: If a person needs food, you create food, pass it to the person, let them eat it, and verify both that there is no remaining food and that the food has been eaten.

## On mocking frameworks

Behavior verification requires a mocking framework, while state verification can be done without one. Mocks can also serve as stubs, and with the added features provided by a mocking framework, they offer greater functionality than stubs.
