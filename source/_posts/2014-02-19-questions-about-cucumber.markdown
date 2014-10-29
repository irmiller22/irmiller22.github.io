---
layout: post
title: "Questions and Thoughts About Cucumber for Myself"
date: 2014-02-19 09:58:47 -0500
comments: true
categories: cucumber warden routing
published: true
---
Questions:

* Why does a `ActionController::RoutingError` pop up even though all of the scenario steps are passing? This has been a recurring issue.
* This was breaking because of 'visit admin_partner_path(@user)' that was in the final step of `/step_definitions/admin_user_change_password.rb`
* How explicit must I be when testing a feature? What's the balance between being too explicit in my feature testing, and not covering everything that should be tested?

Tips regarding Cucumber:

* Backgrounds should only be used when you have shared context, but should be used sparingly
* Want to frame our scenarios with user-driven language, not task-specific language
* Given is used to identify your starting point; if you're testing a password feature, then you should be on the 'Edit' page for the User
* When is used to specify an Action of some kind; never use an assertion in a When clause
* Then is where you make assertions for the test

Warden:

* Warden test helpers for Cucumber will automatically log you in for a test without actually walking through the login steps
```ruby features/support/warden.rb
Warden.test_mode!
World Warden::Test::Helpers
After { Warden.test_reset! }
```
* Only time, from this point forward, that you should visit and fill-in a login form is if you're explicitly testing the login feature
* If you need to be logged in as a Partner, use the `Given I am logged in as a Partner` step. Same for other AdminUser roles as well.
* FactoryGirl helpers in cucumber will enable you to implicitly imply FactoryGirl object 
```ruby features/support/env.rb
World(FactoryGirl::Syntax:Methods) 
```
