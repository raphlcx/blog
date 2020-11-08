---
title: "Lamenting the web dominated by JavaScript"
date: 2020-11-08T14:43:58+08:00
---
More and more websites, especially commercial landing pages, are transitioning to adopting Single Page Application (SPA) model in building their sites. To be honest, I used to hyped with the idea of having server acting only as a pure data source for serving data via its API, with JavaScript client living inside the browser receiving the data over network requests, constructing the page dynamically at runtime. It felt clean. The client, who lives inside the browser, has the full context of what needs to be built, and how to build it appropriately by introspecting the DOM elements.

But implementation is not perfect. For most of the sites that I have visited, visiting links on the site just zap us around with showing any indicator that the site is loading the next page. Also, while on a page, elements appear and disappear all of the sudden, sometimes shifting contents on the page around, making users losing focus on the content being read. Google web vitals even include a metric for measuring the impact of these shifting, known as [Cumulative Layout Shift (CLS)](https://web.dev/cls/).

On the other end, traditional server-rendered sites rely on:

  - Browser showing a page loading indicator when it is fetching and loading a page, informing users that new contents are appearing soon.

  - Completeness of server-side rendered pages. Each page is mostly templated and served to the browser requesting the page, having potentially more guarantee that all components on the page are complete, with less components shifting around even after the page is loaded.

It's not to say an SPA site is terrible because it's lacking these features, it's just that most SPA frameworks do not provide them out of the box, especially when using bare React, Angular, or Vue.js. The developers have to ensure that they are implemented in.

So, dear product managers and developers, ensure that pages on your website or web application do not zap around without transition or loading indicator, and contents do not shift or flash around, as if they are demanding attention by doing so.
