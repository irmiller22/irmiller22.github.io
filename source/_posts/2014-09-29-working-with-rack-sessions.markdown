---
layout: post
title: "Working with Rack Sessions"
date: 2014-09-29 14:46:57 -0400
comments: true
categories: rack sessions rspec testing access
published: true
---

I spent the last few days building a [Flatiron School](https://flatironschool.com) lab that specifically focused on setting sessions within a small Sinatra app. The idea was that this lab would help students better understand the concept of a `session`, and how they allow web applications to maintain state. By going through several RSpec tests and making them pass, the students would iteratively see what the `session` variable values would be in each step, and would understand the process from start to finish. The problem that I ran into was that I was not able to directly access the `session` variable in Sinatra.

### The Root Problem?

The fundamental issue was that in Rack (on top of which Sinatra is built) does not allow you to directly access the `sessions` variable. I could not access it when I was writing acceptance tests, and so I set out to resolve that issue.

### Troubleshoot, take 1

The first step that I took to troubleshoot this was to read through [this StackOverflow post](http://stackoverflow.com/questions/5175854/rack-session-cookie-and-sinatra-setting-and-accessing-data) to see how I could directly access and manipulate session data. So one underlying thorn in the side was that it's hard to directly access `sessions` for acceptance testing purposes. I found this [gem](https://github.com/railsware/rack_session_access) that supposedly makes it easier for a developer to access and manipulate `sessions` variables.

I went down that road, and quickly realized that for the purposes of this lab, I was overcomplicating things a little bit. What the `Rack Session Access` gem does is that it provides middleware for your Sinatra app that allows you to set and obtain data for your application's current session. I didn't need to set anything - I just needed to be able to access the `sessions` hash and confirm that the values met the specifications set forth in my RSpec tests.

Next option.

### Troubleshoot, take 2

The second step I took was to read this [post](http://stackoverflow.com/questions/9113162/accessing-session-in-sinatra-middleware). This gave a better in depth explanation of the issue I was having, which was that I couldn't directly access the `sessions` hash method in Rack middleware. To remedy this, the top-voted answer suggested that I try to access the `sessions` hash through the `env` hash. It was through this post that I realized what I was doing incorrectly all along. I didn't have to implement the solution that was suggested in this post, but I had an idea what I could do to resolve the problem.

Let's dig a little further.

### Identifying the Solution

So how did I address this? So remember in the previous section that you can access the `sessions` hash through `env`. Before I address the solution, let's briefly run through the anatomy of a Rack `sessions` hash. When a new session is created, it creates a new `Rack::Request` object, as the example below shows.

```ruby
def call(env)
  request = Rack::Request.new(env)
end
```

The `request` object, which is assigned to a new instance of `Rack::Request`, has a method called `session`. When you call this method on the `request` object, it will return the `sessions` hash.

Now, let's apply this in terms of acceptance testing. Using the `rack/test` module, I wrote the following method in my `spec_helper.rb` file:

```ruby
def session
  last_request.env['rack.session']
end
```

The `last_request` attribute is actually a `Rack::MockRequest` object that is used to generate a request, such as a `get '/'` request method. You can access the `sessions` hash of `last_request` by accessing the `rack.session` key inside of the `env` method. Once I was able to access this, then I was able to write tests that could be used to test the values inside my `sessions` variable.

It's a very simple way to resolve the original problem, but it definitely threw me for a loop while I was in the process of problem solving. Isn't programming fun?
