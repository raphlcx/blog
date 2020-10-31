---
title: "Prematurely optimising website performance by almost 5x"
date: 2020-10-31T21:28:27+08:00
---
Call it obsession, I have a very keen desire to strip a website of its JavaScript capabilities, using purely semantic HTML and just enough CSS to style the content for readability. And I have done it, again, to this blog website.

Despite the fact that this site is not gaining global traffic with thousands of requests per second, nor is the site specifically written to cater for audiences from regions with developing internet infrastructure, I find it a joy to develop and produce websites as how it was intended in the past, a time where browsers were merely serving as a HTML document viewer. Of course, this is possible for blog websites such as the one that you are reading right now, which doesn't require much real time user interactions.

This site is generated using Hugo. I was using a simple and clean-looking Hugo theme from the community, the [kiss theme](https://github.com/ribice/kiss). For starter it worked out very well. It brought me up to speed with producing content and bringing the site up with Hugo, and I greatly appreciate that. As I use Hugo more and more, my urge to further simplify the site's theme builds up gradually, I want to be able to:

  - Use a standard font that each operating system has, without loading additional font files.

  - Achieve zero JavaScript.

  - Have more control on styling of each HTML element.

[I have done this in the past]({{< relref "a-new-tech-blog" >}} "A new tech blog"), by writing my content in pure HTML. Together with partial header and footer, I wrote Bash scripts that take all the pieces, perform the templating, and wire them up together to produce the final HTML for each content posts. I have since offloaded the templating and site building to Hugo, but I want to retain the ability to define what gets served to my readers, which is less cruft, and less bloat.

Let's see some comparison before and after the switch to a simpler theme for this site.

Before:

![network-status-before-switch](/network-status-before-switch.png)

After:

![network-status-after-switch](/network-status-after-switch.png)

As shown, we see:

  - The number of HTTP request is reduced from 5 to 3, 1.6x improvement.

  - Total data transferred is reduced from 25.5 kB to 9.48 kB, 2.7x improvement.

  - Time to finish preparing the site, reduced from 971 ms to 225 ms, 4.3x improvement.

Most of the what makes up the 9.48 kB after the switch is a larger favicon file (7 kB), which could be further reduced by switching back to use the previous PNG file, and that is only 1 kB in size. But I'm happy with the improvement in overall, and sticking to using an ICO file feels more standard.

Needless to say, this is a premature optimisation. There isn't actually a need to optimise the theme that was used before. 25.5 kB of total asset transfer? Shouldn't hog up the network THAT much.

I like simple sites, less bloat, less distractions to readers, and I enjoy producing sites like this. :)
