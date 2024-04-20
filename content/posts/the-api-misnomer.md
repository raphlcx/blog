---
title: "The API misnomer: What it has got to do with web development?"
date: 2024-04-20T15:28:46+08:00
---

## API

When you hear the term "API" in web development, what comes to mind?

Well, for most of us in modern web development, we would associate it with HTTP endpoints on a back-end application that return response data in JSON format. A front-end application (web/mobile) would then consume these JSON data and perform the necessary processing, independent of the back-end application.

However, that is also the misnomer of "API" of the generation.

An API (application programming interface) is any endpoint that gives out data for others to use. JSON is one way the data can be formatted. There's also HTML, XML, and even binary data. An API is really just a middle-person, helping two interacting parties talk to each other. Its name doesn't come from the data format it uses.

Think of it this way, lots of things are interfaces:

- Like when you use a function from a library (that's an interface between your program and the library).
- Or when you make a call to your computer's operating system (that's an interface between your userland and the kernel).
- Even the receptionist at a clinic is an interface between you and the doctor.
- And human speech itself is an interface when you're talking to someone.

The concept of human speech (interface) remains the same, regardless of the topic, language, and tone of the conversation (data format).

## Asynchronous content loading on the web

Refocusing our attention on web development.

When we're loading web pages, sometimes certain sections take a longer time to load, and if the time-to-first-byte is too long, it can seriously mess with the user experience. That's where asynchronous content loading comes in handy. It allows the entire page to load first and then holds off on loading those slow sections until later.

Through a twist of events on SOAP, XML-RPC, REST, and JSON, the web development settled on using endpoints that return JSON data to serve up this asynchronous content. These endpoints are commonly referred to as "APIs" or "REST APIs," but calling them "REST APIs" just because they return JSON data, isn't quite accurate.

However, to make sense of this JSON data, the front end must parse and process it using custom logic. And that's where a whole ecosystem of front-end libraries and tools (like jQuery, React, Vue, and so on) comes into play to handle this scenario. Before we knew it, front-end development became a whole workflow and ecosystem of its own, complete with package management, application state management, build processes, deployment, and distribution. These add a bunch of extra layers of complexity to building and shipping software.

Now, taking a couple of steps back and looking at this in hindsight, did it really have to be this way?

## Pain points of asynchronous loading

One of the reasons why front-end intensive development gained momentum is the absence of a convenient method to manage asynchronous content loading within standard browser tooling and platforms. By embracing front-end intensive development, the process of dispatching network requests, handling responses, and executing partial page updates can be orchestrated and executed with a good level of abstraction and modularity.

But what if there was a solution that could do all that without the complexities of a modern front-end development?

That is where libraries such as htmx comes in. It is a JavaScript library built on the following principles:

- It lets any element and event fire off network requests.
- It supports HTTP methods beyond just GET and POST.
- And it allows any element to get updated with new content.

With tools like htmx, the back end can now offer its API as a hypermedia API instead of a JSON API. Remember, an API is just a way to get data, no matter what format it is in. If using hypermedia could make front-end development easier, maybe we don't have to stick to returning data as JSON.
