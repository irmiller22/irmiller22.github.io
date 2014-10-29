---
layout: post
title: "Controller Testing Via RSpec"
date: 2014-02-27 10:01:37 -0500
comments: true
categories: rails controller rspec testing TDD
published: true
---

I've been testing controllers pretty often over the last few weeks, and wanted to write a few reminders to my future self. I've been testing an app at work with RSpec, and have gleaned a few best practices to integrate into my unit tests for controllers.

First off, the purpose of controller testing is to test the controller actions directly and see what they do. For example, if we are testing a redirect, we want to make sure that the controller action is redirecting to the right path. It's important to follow the AAA pattern: Arrange, Act, and Assert. You set up the test by arranging a `before(:each)` statement, and setting up the data needed to execute a test via let blocks. Then you act upon the arranged data by manipulating it to the test's specifications. Then you assert that the arranged data should result in some specific action or result. Arrange, act, and assert.

Another tip to keep in mind. Avoid before(:all) statements whenever possible, because you hardly will ever need it, and it sort of goes against Sandi Metz's Single Responsibility Principle (SRP). If you ever actually do need it, you are most likely dealing with a very extraordinary circumstance. The before(:all) block can affect test-data stability due to unwanted side effects.

For BDD/Rspec language, prefer to use active language instead of passive language. Of course the system should do something. Let's write tests where the language asserts definitively that it does or does not do something, thereby documenting not what our system should do but what it does do. It's important to be as clear and succinct in our language about what our expectations are.

Finally, a small but important distinction between `let()` and `let!()`. An object defined in a `let()` statement is lazily evaluated, meaning that it won't instantiate that object until it has been called in a RSpec test. However, an object defined in a `let!()` statement is forcefully evaluated, and the object will be instantiated once the statement has been invoked.

Hopefully these thoughts and insights will prove useful to someone, as they will for me in the near future.
