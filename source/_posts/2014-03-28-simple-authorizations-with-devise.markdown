---
layout: post
title: "Simple Authorizations with Devise"
date: 2014-03-28 14:42:30 -0400
comments: true
categories: cancan devise authorization
published: true
---

We are in the process of building an application that requires several levels of permissions for a User model. Because we do not quite fully understand the scope of what permissions will entail in future iterations of the application, we wanted to leave open the opportunity to expand the permissions scheme further. We made the decision to use CanCan and integrate with Devise. CanCan is an authorization library that defines permissions for different resources. In addition, Devise is a large authentication gem that's used for Rails, and has modules that allow for a large degree of customization.

We added in the CanCan gem, and, while building out our login features, quickly ran into problems associated with the strong params component of Rails. For whatever reason, Rails was not able to properly create a new object because the authorization level integer (via bitmask attribute) was not able to pass through strong params. I quickly found out that we were not properly overriding the Devise params method that whitelists each of the attributes.

As a result, we decided to do away with CanCan and integrate our own permissions/authorization layer for our app.

Here's how it is set up currently in our User model:

```ruby User Model
ROLES = {
   10 => 'super_teacher',
   7 => 'teacher',
   5 => 'student',
   0 => 'user' #default role set on creation
  }

  def self.roles
    ROLES
  end

  def super_teacher?
    self.role == 10
  end

  def teacher?
    self.role == 7
  end

  def student?
    self.role == 5
  end

  def user?
    self.role == 0
  end
```

We've also set up our Registrations controller to override the default that's provided by Devise:

```ruby Registrations Controller
class RegistrationsController < Devise::RegistrationsController

  def new
    super
  end

  def create
    user = User.create!(resource_params)
    sign_in(user)
    redirect_to root_path
  end

  def update
    super
  end

  def resource_params
    params.require(:user).permit(:role, :email, :password)
  end
end
```

As a result, we can scope the view throughout our app by referencing the `role` attribute of each User. The app is very young at this point, but already a major piece of our app has been implemented. Looking forward to seeing where this will be going.
