---
layout: post
title: "ActiveRecord Basics"
date: 2013-11-10 18:16
comments: true
published: false
categories: ActiveRecord
---

ActiveRecord serves as the M, or model, in the MVC paradigm.  It enables the creation and the use of objects in which the data must be persisted to a database.  ActiveRecord makes it possible to do a number of mechanisms within an ORM framework: represent models and their respective data, represent associations between those models, validate models, and perform database operations.

```ruby User Model

class User < ActiveRecord::Base
end
```

This will create a User model that is mapped to a `users` table in the database.  The attributes of the User model would be specified in the Rails migration, and each attribute is specified as a column inside of the database.  

In order to allow us to read and manipulate the data stored in the database, ActiveRecord gives us four primary verbs: `Create, Read, Update, and Destroy`.

To create a User:
```ruby Create User
user = User.create(name: "Ian", hometown: "Charlotte, NC")
user.name = "Ian"
user.hometown = "Charlotte, NC"
user.save
```

To read, or access, a User instance in the database:
```ruby Read User
# all users
users = User.all

# find first user named Ian
ian = User.find_by(:name => "Ian")
```

To update a User instance in the database:
```ruby Update User
# find user to update
user = User.find_by(:name => "Ian")

# update name from "Ian" to "Killian"
user.update(:name => "Killian")
```

To delete a User instance from the database:
```ruby Delete User
user = User.find_by(:name => "Ian")
user.destroy
```

There are three other important concepts to understand: validations, callbacks, and migrations.  Validation enables you to check on the state of a model in order to check whether an attribute value isn't empty, whether it's unique, follows a specific format, has a specific data type, etc.  If a validation results in false, the action is cancelled and the action will not run unless a validation passes and results in a true value.  A callback allows you to attach code to certain events.  For example, if you need a name to be capitalized before it is persisted into the database, you can run a callback that will capitalize each name.  A callback can be run before or after validation, before or after save, before or after a create action, before or after an update action, and before or after a destroy action.  

ActiveRecord also has what are called macro-style methods; they are invocations that delegate to Rails how to manage class instances (data validation, callbacks, associations).  

```ruby User Relationship Declarations
class User < ActiveRecord::Base
	has_one :username
end
```

For example, an instance of the class User will have one username.  You could access it in the same way you access a method of an instance:

```
User.username 
# => "irmiller22"
```

Username is essentially an attribute of the class User.