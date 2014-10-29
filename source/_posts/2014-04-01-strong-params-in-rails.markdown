---
layout: post
title: "Strong Params in Rails"
date: 2014-04-01 23:10:39 -0400
comments: true
categories: rails strong params ruby
published: true
---

Recently, I ran into issues trying to pass a new variable into my params hash after integrating Devise into my app, so I decided to do a refresher of strong params. Strong params was implemented in Rails so that users could not maliciously manipulate form submissions via form fields.

Strong parameters was implemented in Rails 3 via `whitelisting`, which is the act of permitting specific attributes that can be passed into the params hash via the model. In Rails 4, the responsibility of `whitelisting` has now been passed to the controller.

Typically, we have a private method inside of the controller that delegates the `whitelisting` that should take place.

For example:

```ruby UsersController
private

def resource_params
  params.require(:user).permit(:name, :age, :email)
end
```

The `require` statement inside the `resource_params` method performs the parameter validation for the user parameter. If the user parameter exists, then it will go on and validate each of the attributes. If the user parameter does not exist, then it will throw an `ActionController::ParameterMissing` error and return a 400 status code response.

Additionally, the `permit` method will strip out any attributes that do not belong inside of the params. For example, if we tried to include a `:admin` attribute inside of the `permit` method, it will not be passed into the params.
