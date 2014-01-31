---
layout: post
title: "Microhaus App: Part I"
date: 2013-12-17 12:42
comments: true
categories: 
published: true
---

Today I decided to build a microblog app.  It is like a traditional blog, but it is private to a specific group of friends.  There are other options out there for a microblog, but I wanted to create the experience for myself, and how I would customize it for my group of friends.  After I finish the app, I plan to release it to my friends from college, and see what their feedback is.  
I started out by creating a new app via rails.  For the moment, I am calling the app "microhaus."  I came up with this name particularly because it's a microblog, and it is accessible only to a small number of people (a circle of friends, if you will).  

```ruby Generating the new app
rails g new microhaus
```

I then made changes to the Gemfile in order to reflect the gem and database preferences.  This is what my gemfile currently looks like:

```ruby Gemfile
source 'https://rubygems.org'

gem 'rails', '4.0.1'

group :development do
	gem 'sqlite3'
end

gem 'sass-rails', '~> 4.0.0'
gem 'uglifier', '>= 1.3.0'
gem 'coffee-rails', '~> 4.0.0'
gem 'jquery-rails'
gem 'turbolinks'
gem 'jbuilder', '~> 1.2'

group :test do
	gem 'better_errors'
	gem 'binding_of_caller'
	gem 'rspec-rails'
	gem 'capybara'
	gem 'cucumber-rails'
end

group :production do
	gem 'pg'
	gem 'rails_12factor'
end

group :doc do
  gem 'sdoc', require: false
end
```

I've set up my test, development, and production environments.  Later on, I will add in other gems in order to enhance microhaus, but for the initial buildout process, these gems will do.  

I also created a User scaffold, which instantiates a migration for User, a controller, resources, erb templates, stylesheets, and tests.  After creating the scaffold, I ran the ```rake db:migrate``` command in order to migrate the Users table into the development database.

Secondly, I added a Post scaffold via the ```rails g scaffold Post content:string user_id:integer group_id:integer``` command.  The idea is to have a post that belongs to a user, and can be viewed by a group in which the users have the corresponding group id.  We want each post to be viewable by users of the same group.  

At this point, I committed and pushed my changes up to Github.  

The next step is to build out a secure token generator that will be used to verify cookies when a new login session is initialized.  Here's the code I used to set this up:

```ruby Secure Token Generator
require 'securerandom'

def secure_token
	token_file = Rails.root.join('.secret')
	if File.exist?(token_file)
		File.read(token_file).chomp
	else
		token = SecureRandom.hex(64)
		File.write(token_file, token)
		token
	end
end

Microhaus::Application.config.secret_key_base = secure_token
```

My approach to building an application closely mirrors that of Michael Hartl's approach, so as he recommends, this would be a good time to start writing out some tests.  I wrote out RSpec tests to make sure the "About, Home, and Contact" pages properly render as specified in my ERB templates.  

```ruby RSpec Tests
describe "Static pages" do
	let(:base_title) { 'microhaus' }

	describe "Home page" do
		it "should have the content 'microhaus'" do
			visit '/static_pages/home'
			expect(page).to have_content("#{base_title}")
		end

		it "should not have the custom title 'Home'" do
			visit '/static_pages/home'
			expect(page).to_not have_title("| Home")
		end

		it "should have the base title" do
			visit '/static_pages/home'
			expect(page).to have_title("microhaus")
  	end
  end

	describe "About page" do
		it "should have the title 'About'" do
			visit '/static_pages/about'
			expect(page).to have_title("#{base_title} | About")
		end
	end

	describe "Contact page" do
		it "should have the title 'Contact'" do
			visit '/static_pages/contact'
			expect(page).to have_title("#{base_title} | Contact")
		end
	end
end
```

At this point, we have the basic building blocks in place that are necessary to horizontally plan out our application structure.  Once that structure is in place, then we can start building the application vertically, and make changes/additions as necessary.

The next step is to start building out the functionality for the ERB templates and also to build out the corresponding RSpec tests to ensure that the functionality is there.  These concepts will be covered in the next "Microhaus" post.




 

