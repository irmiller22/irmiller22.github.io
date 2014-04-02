---
layout: post
title: "Foray into Cucumber Feature Testing"
date: 2014-02-23 14:45:33 -0500
comments: true
categories: Rails Cucumber Testing Feature Scenario
published: true
---

For the past 2 weeks, I have been getting more involved in Cucumber feature testing as a result of my day-to-day job. When I first started, I found the prospect of Rails feature testing daunting, because it seemed like a cumbersome task to implement on top of building actual application features. 

We had built out some features for a client project that I'm currently working on, and we had decided to implement testing for features via Cucumber, rather than solely relying on RSpec testing. There was a reason for taking this approach: when designing a feature, we didn't want to overtest our feature to the point where we were overemphasizing testing of what could be a very basic feature. Cucumber allows for all-around integration testing without being overly verbose and cumbersome. 

The first feature test I wrote was for a "Forgotten Password" feature. I had to test a User that had forgotten his/her password, clicked on the "Forgot Password?" option at the login screen, and expect to recieve an email with instructions to reset the password. So you would set up the feature test as follows:

```ruby Feature Story
Feature: Partner Password Reset
  As a Partner who forgot the password
  I need to be able to reset the password
  So that I can log in
```

The feature description describes the storyline in very simple, straightforward terms. You need three things when describing a feature: who, what, and why. As in English literature, those three questions are essential to building up any kind of story. Then I would describe the first scenario:

```ruby Feature Scenario
Scenario: Send Reset Password Email
  Given I am a Partner who has forgotten the password
  When I am at the 'Forgot your password?' page
  And I type in my admin email address
  Then it should send me a 'Reset password instructions' email
  And it should come from the default administrator email address
```

The scenario describes step-by-step how the feature is  going to be tested, what the conditions are, and how the expectations should be met. You describe scenarios with three major keywords: `Given`, `When`, and `Then`. `Given` implies an implicit condition that is required for the scenario. `When` specifies an action or verb that should take place. `Then` explicitly states the result that should occur as a result of the action that took place under the `When` clause. 

Once you run the cucumber command in Terminal, the Terminal will generate a list of steps, or tests, that correspond to the scenario that you have written out. The following is a such output of the reset password scenario discussed earlier.

```ruby Reset Password Scenario Steps
Given(/^I am a Partner who has forgotten the password$/) do
  #pending 
end

When(/^I am at the 'Forgot your password\?' page$/) do
  #pending
end

When(/^I type in my admin email address$/) do
  #pending
end

Then(/^it should send me a 'Reset password instructions' email$/) do
  #pending
end

Then(/^it should come from the default NYTM address$/) do
  #pending
end

Then(/^it should result in a 'not found' error message$/) do
  #pending
end
```

Once the Cucumber test steps are generated and incorporated into a step definition script, then you can start filling in the test code as it corresponds to your application. In this situation, I need to generate a user object (via FactoryGirl-Rails gem that has been included in the Gemfile), manipulate the user interface via Capybara methods. The finalized step definitions are shown below.

```ruby Reset Password Scenario Steps
Given(/^I am a Partner who has forgotten the password$/) do
  @user = FactoryGirl.create(:admin_user)
end

When(/^I am at the 'Forgot your password\?' page$/) do
  visit new_admin_user_password_path
end

When(/^I type in my admin email address$/) do
  fill_in "Email", with: @user.email
  click_on 'Reset My Password'
end

Then(/^it should send me a 'Reset password instructions' email$/) do
  @email = ActionMailer::Base.deliveries.first
  expect(@email.to.first).to eq(@user.email)
  expect(@email.subject).to eq('Reset password instructions')
end

Then(/^it should come from the default mailing address$/) do
  expect(@email.from.first).to eq('original@mailing.com')
end

Given(/^I am not a Partner$/) do
  @user = FactoryGirl.build(:admin_user)
end

When(/^I type in a non\-admin email address$/) do
  fill_in "Email", with: @user.email
  click_on 'Reset My Password'
end

Then(/^it should result in a 'not found' error message$/) do
  expect(page).to have_css('.inline-errors', :text => "not found")
end
```

Hopefully this gives you a general idea of how Cucumber feature tests are implemented. Each scenario that you generate must be short, succinct, and straightforward in regards to the feature that you're testing. It took me a while to figure out best practices for implementing such tests, but I was able to get the hang of it over the course of two weeks. Just keep practicing, and see if the feature story makes sense to you. 
