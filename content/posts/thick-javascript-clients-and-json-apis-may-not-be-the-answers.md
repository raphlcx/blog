---
title: "Thick JavaScript clients and JSON APIs may not be the answers"
date: 2024-02-28T00:05:14+08:00
---

Modern front-end web development doesn't shy away from JavaScript. When tasked with creating web applications or websites, developers often turn to back-end microservices for JSON APIs and UI libraries like Angular, React, Vue, and Svelte. These libraries offer a familiar model, enabling developers to interact with object models on the frontend without manually handling the DOM's mutation and updating.

Clearly, having a programming model that developers are comfortable with enables this web development method to become the most widely adopted. It's apparent that the field of web development has made significant advancements and research over the years, representing commendable progress for the entire web community.

However, despite the widespread adoption and success, there are nuances and misconceptions to consider.

1. **Simply returning responses in JSON format from an endpoint does not make it a REST API**. In fact, it is just an endpoint that returns JSON data. True RESTful APIs adhere to specific constraints, including the use of self-describing messages and treating hypermedia as the application's state engine. An endpoint can return responses in HTML format and be perfectly RESTful.

1. During the Web 1.0 era, HTML content was transmitted from remote servers to browser clients. Although these clients lacked a contextual understanding of the HTML's content, they knew how to interpret it and display it as a graphical user interface (GUI). This is akin to Java applets, where bytecodes are sent to applets to be interpreted and rendered as GUI. Both the browser and Java applets were considered thin clients because they could render UI representations without understanding their significance. In contrast, thick clients have their own contextual knowledge, meaning they only require basic message exchanges with remote servers to operate. **By employing reactive UI libraries for front-end UI and communicating with remote servers using JSON APIs, developers essentially create a thick client that resides in the browser**.

1. Over the past decade, there has been a surge in the adoption of microservices, with many companies, including Facebook, Amazon, Google, and Netflix, embracing this approach. This widespread adoption led to the belief that all monolithic architectures should transition to microservices. However, this notion is an example of argumentum ad populum. **It's essential to recognize that while reactive UI libraries have their uses in creating web applications with complex interactions and state changes (such as spreadsheet programs, prototyping software, and trading/charting tools), they may not be necessary for every web UI situation**.

A discerning developer understands that there are numerous approaches to constructing a web application or website. It is often said, "Choose the right tool for the right job." If developers are only familiar with a single tool, such as a reactive UI library paired with JSON APIs, they are likely to rely on that tool for every single task.
