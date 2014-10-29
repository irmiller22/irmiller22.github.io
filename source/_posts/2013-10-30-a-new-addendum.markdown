---
layout: post
title: "Abstraction: Path to Simplicity"
date: 2013-10-30 22:15
comments: true
categories: Thoughts Programming Ruby Abstraction ActiveRecord ActionPack
sources: http://www.etymonline.com/index.php?term=abstract
---

Abstraction is a concept that I have been thinking quite a lot about lately.  It is a concept that has largely been foreign to me, and it is slowly dawning on me that abstraction lies all around us.  

The Webster Dictionary defines abstraction as a "general idea or quality rather than an actual person, object, or event."  If we took the literal translation of the root abstractus (past participle of abstrahere), we would get "to draw away; to detach or divert."  This term was first used by Oswald Herzog to describe the artistic attitudes and implementations of the Dadaists (in 1519, 'Der Abstrakte Expressionismus').  He wrote, "it is pure creation.  It does not borrow objects from the real world; it creates its own objects ... the abstract reveals the will of the artist; it becomes expression."  Abstraction, in the general sense of the word, is the process by which we generate semantic meaning with a concept.  

In Ruby, abstraction is key to making your code achieve two things: more readable and reusable.  At this point, we've learned to make our code more abstract via many concepts, such as MVC, ReST (Representational State Transfer), migrations, database abstraction via ActiveRecord, and routing.

{%img center http://betweengo.com/docs/intro_rails/img/request_cycle.png %}

A major component of Ruby on Rails has really nailed down the abstract aspect of coding for me: ActiveRecord and its corresponding ActionPacks.  ActiveRecord serves as the "Model" component relational database for Rails.  It comes with a set of query methods used for creating, retrieving, updating, or destroying data in the database.  The model also is used for establishing association between classes.  For example, a king could have a "has_many" relationship with his subjects, while a subject has a "belongs_to" relationship with his king.  

```ruby Monarchy Association
class King < ActiveRecord::Base
	has_many :subjects
	has_many :lords
end

class Subject < ActiveRecord::Base
	belongs_to :king
	belongs_to :lord
	has_many :children
end
```

ActionController serves as the "Controller" engine of Rails.  By definition, a controller acts as the intermediary between the views and the models, and shuttles along HTTP responses, view renders, and redirects between the two.  The controller logic is encapsulated into each individual method, and each method should really have only one action in order to simplify the logic behind the controller.  The controller is designed to encapsulate the logic behind the app into specific CRUD methods, as shown below.    

```ruby Subjects Controller
class SubjectsController < ApplicationController
	before_action :set_subject, only: [:show, :edit, :update, :destroy]

  def index
  	@subjects = Subject.all
  end

  def create
  	Subject.create(retrieve_subject_params)
  	redirect_to subjects_path
  end

  def new
  	@subject = Subject.new
  	@lords = Lord.all
  end

  def edit
  	@subject
  end

  def show
  	@subject
  end

  def update
  	@subject.update(retrieve_subject_params)
  	redirect_to subjects_path
  end

  def destroy
  	@subject.destroy
  	redirect_to subjects_path
  end

  
  private
  def set_subject
  	@subject = Subject.find(params[:id])
  end

  def retrieve_subject_params
  	params.require(:subject).permit(:name, :region, :lord_id, :class)
  end
end
```

ActionDispatch serves as a routing engine.  It is responsible for recognizing a path as specified in the model logic and dispatching it according to that route's specific action.  A route action should do only one thing, while keeping in convention via the "fat model, skinny controller" paradigm.  ActionDispatch takes care of the majority of your routes through resource routing.  With Rails, we can declare our `index, show, new, edit, create, update, and destroy` routes with one single line of code.  For example, if we wanted to have a resource for our king, subject, and lord classes, you'd simply do the following:

```ruby Resources for King
KingdomOfValyria::Application.routes.draw do
  resources :kings
  resources :subjects
  resources :lords

  root 'front#index'
end
```

ActionView is the Rails engine responsible for maintaining the HTML views that are rendered to the browser.  For each controller that's present in the app, there is a corresponding `app/views` directory that stores ERB templates used for HTML browser rendering.  

```ruby Subject View
<h1><%= @subject.name %> - Subject of King <%= @subject.king.name %></h1>
<p>Lord: <%= @subject.lord.name %></p>
<p>Lives: <%= link_to @subject.region, @subject.region %></a></p>
<% unless @subject.children.empty? %>
  <h2>Children</h2>
  <ol>
    <% @subject.children.each do |child| %>
      <li><%= link_to child.name, child %></li>
    <% end %>
  </ol><br>
<% end %>
<%= link_to "Back", subjects_path %>
<%= link_to "Edit", edit_subject_path %> 
<%= link_to "Delete", @subject, method: :delete %>
```

Being able to effectively encapsulate your logic into separate logic categories is such a necessary skill-set to understand in order to become a proficient Ruby on Rails developer.  I'm still in the process of learning how to whittle down my logic into singular methods and actions.  Ruby is a language that was built for abstraction.  It is meant to simplify logic in your code, and as Matz said once, "I hope to see Ruby help every programmer in the world to be productive, and to enjoy programming, and to be happy."


