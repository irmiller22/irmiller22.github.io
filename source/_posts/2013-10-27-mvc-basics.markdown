---
layout: post
title: "CRUD Applications: Importance of MVC"
date: 2013-10-27 23:03
comments: true
categories: RSpec, Tips
---

This past weekend, we had an assignment to build a CRUD application from scratch (well, almost).  A CRUD application is short for the four basic applications of relational database management and storage: create, read, update, and delete.  The CRUD application allows interfacing between a database, a user interface, and HTTP protocols.  I thought that I'd go ahead and describe the main components of our CRUD application that we built this weekend.  There are thee specific ones to take note of: the controllers, models, and views.  

Models are Ruby-specific classes that talk to the database, store, validate, and execute logic.  In other terms, they do all of the heavy lifting for our application.  They're basically the overworked accountant in the back room during tax season, churning out numbers while eating stale donuts at an absurd rate.  

Views are elements that the user sees and interface with.  In short, they will provide the user experience through HTML, CSS, Javascript, and other front-end aspects.  They're the responsible ladies and gentlemen specifically positioned near the front of the office to provide a sense of culture and value to the outside user looking in.  

{% img center http://static2.fjcdn.com/thumbnails/comments/ah+alright+cheers.+yeah+doge+is+great+_56f6675509eadf36993a7e5b53c37f6d.jpg %}

Controllers deal with the back-end requests from users, data requests, data submissions, sessions, and so on.  Any time you enter data into a form on a website, such as a login and password, you are interacting with a controller after you have hit the submit button.  After the request is sent, the controller sends the response to the server, and the server then combines that data into a HTTP response that is sent back to the user.

{% img center http://i37.tinypic.com/2wrn985.gif 500 %}

One term that I've read about describes how you should structure your models and controllers: "fat models, skinny controllers."  The idea is that your models are not just a simple form of abstration in your database layer, but they represent the entirety of the logic in your application.  It should be able to stand alone from the rest of the application.  By convention, it should not interact with other components of your application.  Each controller method also should represent one action.  The controllers simply just shuttle requests between your views and your models (or your user interface and the application logic).  Another primary reason why this is an important programming practice is that you have enabled 2 things: one, you've made it easy to reuse your code due to its abstraction and simplicity, and two, you've also made it simpler for testing your code.

{% img center http://www.mikebernat.com/images/cake/layercake.png 500 %}

That's what I wanted to review in this blog post.  I'll also share some of my favorite, but terrible, puns with you.  Some of you might have already heard some of these.

A truck transporting fruit in California overturned the other day.  It created quite a... jam.  That driver's career is also... toast.

The other day, I couldn't quite remember how to throw a boomerang, but eventually it came back.

Hello everyone (in Ruby 003 class on Monday, Oct 28), what is Avi's background?  Well, it's a white wall.

{% img center https://i.chzbgr.com/maxW500/5870736384/hB08BCDAD/ %}



