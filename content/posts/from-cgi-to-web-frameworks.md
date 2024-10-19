---
title: "From CGI to web frameworks"
date: 2024-10-19T22:41:32+08:00
---

In the beginning, there was CGI. The way it functioned was that when a web server, such as Apache or Nginx, received a request, it would pass this request to a CGI program. This program could be a PHP script, Python script, console application, or any other program, as long as it adhered to the interfaces defined by CGI.

**Illustration:**

```
request --[HTTP]--> web server --[CGI]--> program
```

However, this method is resource-intensive because the program or script interpreter must start and terminate for every single request. This led to the introduction of web server modules, like mod_php and mod_python, which were used in the early days of the Apache web server. These modules are integrated directly into the web server, allowing the program or interpreter process to start just once and be reused for each subsequent request.

**Illustration:**

```
request --[HTTP]--> web server
```

Yet again, this approach can bloat the web server with numerous modules. What if we could achieve both:

  - Start the program or interpreter process only once
  - Avoid bloating the web server

This is where application servers come into play. Application servers, such as PHP Zend Server, Python Gunicorn, and Ruby Puma, initiate their long-lived processes only once. They operate between the web server and the application logic that processes incoming requests. Every request received by the web server is forwarded to the application server, where each request is managed by custom logic created by developers. This custom logic is often packaged with a web framework like Ruby on Rails or Django.

**Illustration:**

```
request --[HTTP]--> web server --[HTTP forward]--> application server
```
