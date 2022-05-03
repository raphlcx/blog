---
title: "Prematurely optimising website performance by almost 5x"
date: 2020-10-31T21:28:27+08:00
---
Call it an obsession. I enjoy stripping a website of its JavaScript capabilities, using purely semantic HTML and just enough CSS to style the content for readability. And I have done it, again, to this blog website.

This site is not gaining global traffic with thousands of requests per second, nor is the site specifically written to cater for audiences from regions with developing internet infrastructure. Even so, I like producing simple websites, websites that treat browsers only as simple HTML document viewers. And fortunately, this is possible for the blog website you are reading right now, which doesn’t require many real-time user interactions.

I use Hugo to generate this site. In the beginning, I used a simple and clean-looking Hugo theme from the community, [the kiss theme](https://github.com/ribice/kiss). For starters, it worked out very well. I was productive in producing content, and I greatly appreciated that. As I use Hugo more and more, my urge to further simplify the site’s theme builds up gradually, I want to be able to:

  - Use web-safe font.
  - Achieve zero JavaScript.
  - Have more control over the styling of each HTML element.

[I have done this previously]({{< relref "a-new-tech-blog" >}} "A new tech blog") by writing my content in pure HTML. Together with partial headers and footers, I wrote Bash scripts that take all the pieces, perform the templating, and wire them up together to produce the final HTML for each content post. I have since offloaded the templating and site-building to Hugo, but I want to retain the ability to define what gets served to my readers, which is less cruft, and less bloat.

Here is a comparison before and after the switch to a more minimal theme for this site.

Before:

{{< figure src="/network-request-before-switch.png" caption="Network request before the switch">}}

After:

{{< figure src="/network-request-after-switch.png" caption="Network request after the switch">}}

We see:

  - The number of HTTP requests decreased from 5 to 3, 1.6x improvement.
  - Total data transferred decreased from 25.5 kB to 9.48 kB, 2.7x improvement.
  - Time to finish preparing the site reduced from 971 ms to 225 ms, 4.3x improvement.

What makes up the 9.48 kB after the switch is a larger favicon file, sized at seven kB, in contrast with the previous favicon of one kB in size. But I’m happy with the improvement overall, and sticking to using an ICO file feels more standard.

Yes, this is premature optimisation. There wasn’t a need to optimise the Hugo theme that I was using before. 25.5 kB of total asset transfer? It shouldn’t hog up the network THAT much.

I like simple sites, less bloat, fewer distractions to readers, and I enjoy producing sites like this.
