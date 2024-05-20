---
title: "Obsessing over tooling"
date: 2024-05-21T02:14:12+08:00
---

*(This post was originally written on April 6, 2019. I discovered this youthful rant while cleaning up my documents.)*

How often have you encountered a new tool, X, that promises to improve your workflow efficiency and cut task completion time in half? But it's not just X—there’s also Y to bootstrap and configure X, so you don’t have to manage it directly. And of course, Z is there to monitor the progress and status of Y as it sets up X.

Anyone in the software development domain, whether a developer, system administrator, or DevOps practitioner, is prone to following this chain of tools to the end, hoping to gain all the benefits touted by a random blog post on Medium.

Consider a fresh programmer setting up a basic website for a blog. What's trendy these days? JavaScript? Let’s use an Express server to serve the static pages. But wait, there are enticing features in ES2015, ES2016, and ES2017 JavaScript that the current Node.js runtime doesn't support natively. Enter Babel! What about static assets for the blog? Some web traffic is still on HTTP/1.1, so assets should be concatenated into a single bundle. No problem, just configure Webpack. And CSS needs vendor prefixes, so we'll use PostCSS and its plugins.

Now that the development work is done, let’s deploy the blog! What’s the trendy deployment tool? Docker sounds cool, but it doesn't offer auto-scaling and self-healing features—Kubernetes does. Setting up Kubernetes manually is hard, but Amazon and Google cloud vendors offer streamlined setup. Still, writing Kubernetes configurations isn't ideal, so we use Helm to package the Kubernetes "app" for easy installation.

After all this setup, the blog is finally live. But at what cost? The real value of the project is to present a blog to a global audience, and to captivate readers, more attention should be given to typography, page layout, and content quality. Chasing the latest tools and the setup process can push down the priority of making the blog engaging.

The main purpose of tools is to help creators deliver products and services that benefit society. Obsessing over the latest tools offers little business value. There are times when these tools are applicable, and it’s fine for engineers to develop and release them. However, as decision-makers, we should choose our tools wisely based on our current environment, rather than blindly following trends.

A known concept related to this is the hype cycle. At its peak, everything seems perfect—unicorns, rainbows, and nyan cats. The unicorn eats a magic mushroom and believes it has become a cat. But then reality sets in, and the unicorn’s fantasies lead to disaster. This completes one hype cycle, and people return to reliable tools that worked in the past.

Eventually, the next hype cycle begins, and the age of hipsterisation returns. We learn from past mistakes, so this time, we feed the mushroom to the cat instead.
