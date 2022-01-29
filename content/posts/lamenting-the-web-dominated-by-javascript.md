---
title: "Lamenting the web dominated by JavaScript"
date: 2020-11-08T14:43:58+08:00
---
More and more websites, especially commercial landing pages, are transitioning to adopting the Single Page Application (SPA) model in building their sites. I have been guilty of riding the cargo cult too. I used to preach enthusiastically the idea of having a server acting only as a pure data source for serving data via its API, with a JavaScript client living in the browser receiving the data over network requests, constructing the page dynamically at runtime. It felt clean. The client has the full context of what to build and how to build it appropriately by interoperating with the DOM elements.

But implementation is not perfect. For most sites I have visited, clicking on the page's links zaps me around without indicating that the site is loading the next page. Also, while on a page, elements appear and disappear like peek-a-boo, sometimes shifting contents around, making users lose focus while reading the content. Google web vitals even include a metric for measuring the impact of these shifting, known as [Cumulative Layout Shift (CLS)](https://web.dev/cls/).

On the other end, traditional server-rendered sites have this figured out.

  - Browser shows a page loading indicator when fetching and loading a page, informing users that new contents are appearing soon.
  - Server-side renders each page as a complete whole and delivers it to the browser. Discounting JavaScript interventions, page components are less likely to shift around after the page is completely loaded.

It is not to say a SPA site is terrible for lacking these features, but most SPA frameworks do not provide them out of the box, especially when using bare React, Angular, or Vue.js. The developers have to do the extra work to ensure a good user experience.

So, dear product managers and developers, ensure that pages on your website or web application do not zap around without transition or loading indicator, and contents do not shift or flash around, as if they are demanding attention by doing so.
