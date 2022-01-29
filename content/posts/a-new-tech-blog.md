---
title: "A new tech blog"
date: 2020-08-06T23:58:29+08:00
---
I'm back to writing a tech blog.

Over the past few years, I have tried numerous ways to publish my ideas, notes, and writings on technical matters.

My first attempt was creating a custom static site generator, using plain HTML and shell script for templating. A VPS instance on Digital Ocean then served the built pages. Why build a custom static site generator, you ask? I wanted to have more control over the contents and assets of the site. It was part of my strive for minimal bloat and fully utilising semantic HTML5 for the contents.

Hosting wise, Digital Ocean VPS costs money. So I thought of using a free provider to host the static site. After all, they are simple web pages, and the host doesn't require intensive computing power to serve a few HTML pages and CSS style sheets. That was when I turned to host the site using GitHub Pages. GitHub Pages supported the use of Jekyll, and that piqued my interest.

Naturally, the next step was to migrate the contents from my custom static site generator to Jekyll. That allowed me to focus more on the blog content and less on the technical wiring of building the site.

Slowly, I started to publish my notes on the site as well. Some documents were private, but they were all published in the same space as the public notes, so everything was accessible publicly.

Some form of privacy was required. Once again, I moved the contents, this time to a WordPress site. I published the blogs and public notes while keeping the private ones private. The privacy control was supported natively using the visibility controls on WordPress.

It worked, but it didn't feel right. It isn't how a blog should look like, with notes, articles, and secluded hidden notes around it. That was when I thought what I needed was a note-taking app, in addition to a blog.

That was when I took up Evernote. The blog wasn't necessary at this time, so I moved the posts and notes into Evernote, creating them all as individual notes. Still, Evernote didn't support Markdown, and that meant vendor-locking.

Finally, I took the contents back to my local machine, editing them in plain Markdown. Initially, I used Notable as the Markdown editor. But since I was already an avid user of the vim text editor and had been getting productive with vim after all these years, I switched to using vim for editing the notes in Markdown.

That concludes my past journey of venturing into different solutions for organising content and ideas. Even after all the hurdles, I still feel that writing a tech blog is beneficial to help reinforce my learning in the technology domain and publicise them in a public space.

This latest blog uses Hugo static site generator, with content editing in plain Markdown using vim. I'm thinking of deploying the site using Netlify.

As for note-taking, I will keep it simple by retaining the editing in plain Markdown using vim and keeping it on the local machine.
