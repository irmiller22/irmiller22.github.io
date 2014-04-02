---
layout: post
title: "Rails Link Shortener App"
date: 2014-02-02 15:05:05 -0500
comments: true
categories: rails link shortener mongoid mongo 
published: true
---

Over the last few days, there were primarily two things that I wanted to delve into conceptually and understand: MongoDB and the logic behind a link shortener. I decided to combine the two of these and create a simple Rails app in which you could shorten links. 

So, what is MongoDB? MongoDB is what's called a `NoSQL` database technology. A `NoSQL` database doesn't rely on a series of rows and columns within tables like a `SQL`, or relational, database does. `NoSQL` databases can store information in a variety of ways, but in the case of MongoDB, it stores it via a document database. Each respective document that you enter into the Mongo database is very similar to a JSON object in that they have a key-value store. However, the primary difference is that you do not access the documents via the key (think of accessing elements in a hash), but rather you query the database for these document elements.

I started out the app by generating a new Rails app via `rails g new link_shortener`. Once the app is generated, you can also use the Mongoid gem to generate the config file for the MongoDB database via `rails g mongoid:config`. Once you generate the config file, you can read through the file, available at `mongoid.yml` within the app, for an in-depth explanation of Mongoid's capabilities.

Next step is to create the model. Since we have an app t hat is going to shorten URLs, we need to create a URL model, so that each URL can be represented within Ruby/Rails as an object. 

In Rails, querying the database is very much like that of ActiveRecord. The same model commands, such as `where, find, find_or_create_by, save` are very much in place. Look at the Mongoid documentation to find out more.

Let's test it out.

```ruby Url Model
u = Url.new
u.url = "http://irmiller22.github.io"
#=> #<Url _id: 52eeacee49524d08ce010000, url: "http://irmiller22.github.io">

u.save
Url.where(:url => "http://irmiller22.github.io")
#=> #<Mongoid::Criteria
#=>  selector: {"url"=>"http://irmiller22.github.io"}
#=>  options:  {}
#=>  class:    Url
#=>  embedded: false>
```

Now that you've set up a model to take care of persisting data to the database, we need a controller to eventually take the input, generate the right output, and getting the data to the appropriate routing destination. Let's generate the new controller: `rails g controller urls new`. Typically when you generate a controller, you generate all 7 of the RESTful actions, but in this case we're only interested in testing one action for the moment: new. This generates the following:

```ruby UrlsController
class UrlsController < ApplicationController
  def new
  end
end
```

```ruby routes.rb
get "urls/new" 
# we will modify this to the following:
# resources :urls, only: [:new, :show, :create]
```

Now let's fill in the logic for each of the actions in the Url controller. 

```ruby UrlsController
class UrlsController < ApplicationController
  def new
  	@short_url = Url.new
  end

  def create 
  	@short_url = Url.new(url_params)
  	if @short_url.save
  		flash[:short_id] = @short_url.id
  		redirect_to new_url_url
  	else
  		render :action => "new"
  	end
  end

  def show
  	@short_url = Url.find(params[:id])
  	redirect_to @short_url.url
  end

  private

  def url_params
  	params.require(:url).permit(:url)
  end
end
```

In this simple case, I'll be resetting the link to the domain and the URL id, so it would look like this while running locally: `localhost:3000/f3e200091`. In my next refactor, I'll end up generating a random string, but this will do for now.

Let's move on to the views:

```ruby application.html.erb
<!DOCTYPE html>
<html>
<head>
  <title>LinkShortener</title>
  <%= stylesheet_link_tag    "application", media: "all", "data-turbolinks-track" => true %>
  <%= javascript_include_tag "application", "data-turbolinks-track" => true %>
  <%= csrf_meta_tags %>
</head>
<body>

<%= yield %>

<% if flash[:short_id].present? %>
	<p class='shortened_link'>
		The shortened URL is available <%= link_to "here", url_url(flash[:short_id]) %>
		(Right click and copy link to share it).
	</p>
<% end %>

</body>
</html>
```

```ruby new.html.erb
<%= form_for @short_url do |f| %>
	
	<p>
		<%= f.label :url, "Your URL:" %>
		<%= f.text_field :url %>
	</p>

	<% if @short_url.errors[:url].any? %>
		<p class="error_messages">
			The given URL <%= @short_url.errors[:url].to_sentence %>
		</p>
	<% end %>

	<p class="button">
		<%= f.submit "Shorten my URL" %>
	</p>

<% end %>
```

These two views are all you need to enable the link shortener. Fire up the rails server, and see your new link shortener in action! 
<br />
EDIT: I incorrectly asserted that a MongoDB document element is similar to that of a key-value pair, and the appropriate revisions have been made. Many thanks to Myles Recny for catching that error.
