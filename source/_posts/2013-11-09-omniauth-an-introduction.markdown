---
layout: post
title: "OmniAuth: An Introduction"
date: 2013-11-09 14:52
comments: true
publicshed: false
categories: 
---

OmniAuth is simply middleware for authenticating users in applications.  Middleware is a way to filter requests and responses  that are coming into your application.  Each strategy that is used in the OmniAuth middleware is a class, and each strategy has two primary phases: a request phase and a callback phase.  

Let me take a step back, and explain what middleware is.  Middleware is the interface that communicates between the application and the web server.  In a way, it is generally a software library that assists with, but is not directly involved, in the execution of a specific task.  Some examples include authentication, data logging, performance monitoring, caching, and the like.  In this particular case, OmniAuth is dealing with authentication.  

```ruby config/initializers/omniauth.rb

Rails.application.config.middleware.use OmniAuth::Builder do
	provider :twitter, ENV['TWITTER_KEY'], ENV['TWITTER_SECRET']
	provider :facebook, ENV['FACEBOOK_ID'], ENV['FACEBOOK_SECRET']
end
```

The two providers in the initializer above are called strategies.  A strategy, in OmniAuth's case, is a specific authentication class within the OmniAuth middleware structure.  OmniAuth has a large number of strategies, available at its Github documentation wiki, that provide authentication services for a wide number of websites, such as Twitter, Facebook, Github, and others (SOURCE - https://github.com/intridea/omniauth/wiki).  

Let's walk through an example with Github.  In the request stage, a client sends a request to the app.  The client is redirected to the `/auth/Github` route, and it points to the OmniAuth Github strategy specified in the config/initializer directory within the Rails project structure.  In order to properly authenticate the user, an authentication request is fired off to Github through the provider strategy.  Once the user is properly authenticated, the callback method, specified through `/auth/github/callback`, creates a user information hash that can now be accessed through the app (SOURCE - https://speakerdeck.com/xfernandox/the-anatomy-of-omniauth).





