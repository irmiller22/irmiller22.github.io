---
layout: post
title: "Explanation of NGinx and Passenger"
date: 2013-11-04 20:19
comments: true
categories: NGinx Passenger Apache
---

NGinx is a open-source proxy server that primarily serves HTTP and HTTPS protocols.  Nginx was created to achieve three main goals: concurrency (process in which computations are executed simultaneously across several systems), high performance, and low memory usage.  It utilizes an event-driven approach to server requests, and allocates responses based upon server traffic and handling.  It has become a viable alternative to the Apache server model, which uses a process-driven methodology for handling large server requests.  
NGinx has emerged largely as a result of increased demand for online services and the rise of the web as a platform.  Unlike Apache, NGinx has a master process that delegates work to worker processes, and it is designed handle a large number of server connections with a small amount of memory.  

A number of companies, according to Wired, have run into scalability issues with Apache, and have transitioned to NGinx.  Facebook, Dropbox, and Wordpress are sites that utilize the NGinx server platform.  

{% img center http://www.wired.com/wiredenterprise/wp-content/uploads//2012/02/Screen-shot-2012-02-09-at-5.16.17-PM.png %}

NGinx does not have a module load feature like Apache does, but that is where Passenger steps in, as it automates the module loading process.  

Passenger is an Rack application server software that is often used in conjunction with NGinx.  Similarly with mod_php for hosting PHP apps on Apache, Passenger simplifies the process of hosting Ruby apps on NGinx.  Passenger automates the process of starting/stopping a server, restarting after a server crash, and dynamically adjusts the number of processes on a server based on traffic.  It also allows the server to run more than 1 instance of the app, allowing greater use of server resources.  