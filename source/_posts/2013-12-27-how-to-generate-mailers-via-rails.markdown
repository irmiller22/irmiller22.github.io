---
layout: post
title: "How to Generate Mailers via Rails"
date: 2013-12-27 11:08
comments: true
categories: Rails Ruby Mailer
published: true
---

One topic that we never covered in regards to Rails was how to properly generate mailers (automatically generated emails from a web application).

I'm covering this because I have not used mailers before, and I think it's a pretty pertinent feature of Rails that everyone should know.

I'll be using the example of a distributed solar generation product.  Whenever a consumer is experiencing unusually low solar energy production or energy generation patterns consistent with a malfunctioning module, it will send out a mailer notifying them of these issues.  In order to generate a mailer, you type in the following command:

```ruby Mailer Generation
rails g mailer SolarMailer error_report
```

The next step is to set up the default settings of the mailer.  

```ruby Mailer Settings
class SolarMailer < ActionMailer::Base
	default from: "administrator@solartech.com"

	def error_report(panel, customer)
		mail to: customer.email, subject: "Panel #{panel.location} #{panel.id} is experiencing some technical issues.""
	end
end
```

After we have set up the default settings for the mailer, then we go to the Panel model in order to specify when the mailer should be sent out.

```ruby Panel model
class Panel < ActiveRecord::Base
	belongs_to :module
	before_save :check_power_yield

	def check_power_yield
		if power_yield <= 1200 kWh #base power generation
			SolarMailer.error_report(self, self.customer).deliver
		end
	end
end
```

That's pretty much it for a basic mailer.  You can style the mailer using HTML/CSS, and you can also add front-end features via Javascript (AJAX, jQuery).  

