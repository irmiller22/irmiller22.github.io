---
layout: post
title: "ActiveRecord: Validations"
date: 2013-11-10 22:20
comments: true
categories: ActiveRecord, Validation, Rails, Ruby
---

Validations are necessary to prevent invalid data from being persisted into the database.  They are necessary to allow for efficient data entry and processing, and prevent uncessary situations such as empty user-name fields pushing empty strings into a database.  

ActiveRecord has a number of simple declarative validations, such as `validates_absence_of`, `validates_acceptance_of`, and `validates_confirmation_of`.  The one validation that I think would come in quite handy is the `validates_format_of` validation.  According to the "Rails 4 Way", it's heavily dependent upon regular expressions, but it's an invaluable way to check the formats of email addresses, web URIs, etc.  

You can also place constraints upon your validations.  For example, if you're using `validates_length_of` for usernames, you can specify a range that the length must fall between.  For example:

```ruby Validation - Username Length
class Account < ActiveRecord::Base
	validates_length_of :username, within: 5..20
end
```

If it exceeds the length, then you can provide an error message option that follows the validation.

```ruby Validation - Username Length, Error
class Account < ActiveRecord::Base
	validates_length_of :username, within: 5..20, 				wrong_length: "should be %{count} characters 			long"
end
```

An important aspect of validations is enforcing uniqueness among join models in the database.  We have used this recently (in our Flatiron-Kitchen-Ruby-003 assignment).  In order to enforce uniqueness, we have to define a scope constraint.  For example, if we had three classes, `Registration`, `Course` and `Student`, how could we make sure that a student isn't registered more than once for a particular class?  We would define the scope of the `Registration` class, as shown below:

```ruby Registration - Scope
class Registration < ActiveRecord::Base
	belongs_to :student
	belongs_to :course

	validates_uniqueness_of :student_id, scope: :course_id, message: "can only register once per course"
```

Another one that you run into all the time:

```ruby Email Validation
class Email < ActiveRecord::Base
	validates_uniqueness_of :address, message: "already taken"
end
```

In short, validations help you place constraints upon your code to make it less error-prone in regards to data persistence.  Think of validations as a gatekeeper for your application data -- it lets the good pieces in, and keeps the bad ones out, assuming that you have the proper constraints in place.  




