---
layout: post
title: "Routes in Rails"
date: 2013-11-11 08:33
comments: true
published: false
categories: 
---

There are two primary purposes for routing: one, it serves as the mapper for controller action methods, and two, it provides dynamic URL generation that will be used as arguments for linking methods (like `link_to` and `redirect_to`).

Routes are defined in the `config/routes.rb` file.  In here, you will see all of your routes defined either through the shorthand form, `'events/:id' => 'events#show'`, or via resources that generate all of your routes for you.  There are two reasons why we define our routes in this way: recognition and generation.  For example, if we had an event with an ID of 1, it would be equivalent to the following:

```ruby Config Routes
link_to "Events",
	controller: "events".
	action: "show",
	id: 1
```

