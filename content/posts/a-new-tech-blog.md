---
title: "A new tech blog"
date: 2020-08-06T23:58:29+08:00
---
I'm back to writing tech blog.

Over the span of the past few years, I tried out various ways to publish my ideas, notes, and writings on technical matters.

My first attempt was creating a custom static site generator, using plain HTML and bash script for templating. The built pages were then hosted on a VPS instance on Digital Ocean. Why build a custom static site generator, you ask? I wanted to have a full control on the contents, libraries, scripts, and styles that are displayed on the site. This was part of my strive for extreme minimal bloat, and fully utilising semantic HTML5 for the contents.

Digital Ocean VPS costs money though. So I thought of using a free provider to host the static site. After all, it's static, the host doesn't require intensive computing power to serve a few HTML pages and CSS style sheets. That was when I turned to hosting the site using GitHub Pages. GitHub Pages supported the use of Jekyll, and that piqued my interest.

The next destination of journey, is naturally migrating the contents from my custom static site generator, to Jekyll. That allowed me to not worry about how the site is built, and I could focus more on the content itself.

Slowly, I started to publish some of my notes as well on the site. Some notes were private, but regardless I preferred to keep them together with the public notes, so everything was public.

Some form of privacy was required. I then moved the contents once again into a Wordpress site, publicising blogs and public notes, while keeping the private notes private, which is supported natively using the visibility controls on Wordpress.

It worked, but it didn't felt right. This isn't how a blog should look like, with notes, articles, and a secluded hidden notes around it. That was when I thought, what I needed was, a note-taking app, in additional to a blog.

That was when I took up Evernote. Blog wasn't necessary at this time, so I moved the posts and notes into Evernote, creating them all as individual notes. Still, Evernote didn't support Markdown, and that meant vendor-locking.

Finally, I took the contents back to my local machine, editing them in plain Markdown. Initially I used Notable as the Markdown editor, but since I was already an avid vim user, and having been getting productive with vim after all these years, I converted to using vim for editing the notes in Markdown.

With that, concludes my past journey of venturing into different solutions for organising contents and ideas. After all these experience, I do still, feel that publishing a tech blog is beneficial, both for reinforcing my learning in the technology domain, and publicising my learning in a public space.

This latest blog is built with Hugo, with content editing in plain Markdown using vim. I'm thinking of deploying the site using Netlify.

As for note taking, I will keep it simple, by retaining the editing in plain Markdown using vim, and keeping it on local machine.
