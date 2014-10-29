---
layout: post
title: "RSpec Testing for a JSON API"
date: 2014-04-18 10:31:45 -0400
comments: true
categories: json api rspec rails ruby
published: true
---

I am in the process of building out a JSON API for a registry application, that, when pinged by Salesforce, will fire off emails to prospective Flatiron students that notify them of their acceptance into the Flatiron School program. One thing that I noticed is that there aren't many resources out there for setting up a testing environment and eventually building out a JSON API via Ruby on Rails. Hopefully this resource will shed a little more light on how to accomplish that.

The first step is to integrate a Rails serializer in order to encapsulate the JSON serialization of objects. We first install the `active_model_serializers` gem into the Gemfile, and then bundle. Now, for each model that we want to serialize for JSON serialization, we need to create a serializer: `rails g serializer student`.

Inside of the serializer file that has just been generated, we need to add in student attributes that will be defined and visible in the JSON API.

```ruby app/serializers/student_serializer.rb
class StudentSerializer < ActiveModel::Serializer
  attributes :id, :first_name, :last_name, :email
end
```

For example, this is how the JSON data will be represented on the API tree, based upon the order of attributes in the `student_serializer.rb` file above:

```ruby JSON data for student
  students:
    [
      {
        id: 4,
        first_name: "Doctor",
        last_name: "Who",
        email: "doctor_who@whoville.com"
      }
    ]
```

There is a key called students, and inside of its value store, it holds a collection of student objects. Inside of the array collection, there is 1 student with an id of 4, first name of "Doctor", last name of "Who", and an email "doctor_who@whoville.com". This is a demonstrative example of how the JSON object data will be rendered when the API is pinged.

In the `app/controllers` directory, I created a new folder system that allows for semantic versioning of the API. Currently my directory looks like this: `app/controllers/api/v1`. Inside of that folder structure, I have one file: `students_controller.rb`. We will come back to these files once we start building out our controller actions.

Next, I set up the routes for the `students` resource. My `config/routes.rb` file looks like this currently:

```ruby config/routes.rb
namespace :api do
  namespace :v1 do
    resources :students, only: [:index, :show, :create]
  end
end  
```

This is the basic setup for the API itself. At this point in the API development, we're only concerned with the `index`, `show`, and `create` actions for the API Students Controller. Now, we'll go ahead and set up the RSpec tests for the controller.

```ruby spec/controllers/api/v1/students_controller_spec.rb
require 'spec_helper'

describe API::V1::StudentsController do
  describe "GET 'index' " do
    it "returns a successful 200 response" do
      pending
    end

    it "returns all the students" do
      pending
    end
  end
```

This describe block refers to the index action of the controller. In this block, we are testing for two expectations: for the JSON response for the index action to return a 200 status code, and for the JSON response to return the correct number of students that exist in the test database.

```ruby spec/controllers/api/v1/students_controller_spec.rb
require 'spec_helper'

describe API::V1::StudentsController do
  describe "GET 'index' " do
    it "returns a successful 200 response" do
       get :index, format: :json
      expect(response).to be_success
    end

    it "returns all the students" do
      FactoryGirl.create_list(:student, 5)
      get :index, format: :json
      parsed_response = JSON.parse(response.body)
      expect(parsed_response['students'].length).to eq(5)
    end
  end
```

In order to get these tests passing, we first need to submit a get request for the `index` action in a JSON format. We first expect the response to be successful, or more specifically, to result in a 200 status code.

We also need to test whether or not it returns all of the existing students in the test database. I've used the FactoryGirl gem to mock out the student list `create_list(:student, 5)`. I've also set up the `parsed_response` variable, which will translate the response body in JSON format into a more readable format. Then I've set the expectation that the **length** of the `parsed_response[students]` should be equal to 5, as specified in the `FactoryGirl.create_list(:student, 5)` line.

```ruby spec/controllers/api/v1/students_controller_spec.rb
  describe "GET 'show' " do
    it "returns a successful 200 response" do
      pending
    end

    it "returns data of an single student" do
      pending
    end

    it "returns an error if the student does not exist" do
      pending
    end
  end
```

This block refers to the show action. I am testing for three specific expectations: a successful JSON response to return a 200 status code, a successful response to return the correct student JSON object, and for the JSON response to return an error message for a student JSON object that doesn't exist.

```ruby spec/controllers/api/v1/students_controller_spec.rb
  describe "GET 'show' " do
    let(:student) { create(:student) }

    it "returns a successful 200 response" do
      get :student, id: student, format: :json
      expect(response).to be_success
    end

    it "returns data of an single student" do
      get :student, id: student, format: :json
      parsed_response = JSON.parse(response.body)
      expect(parsed_response['student']).to_not be_nil
    end

    it "returns an error if the student does not exist" do
      get :student, id: 10 , format: :json
      parsed_response = JSON.parse(response.body)
      expect(parsed_response['error']).to eq("Student does not exist")
      expect(response).to be_not_found
    end
  end
```

We've built out a student mock using FactoryGirl, and will be using this to test whether or not the `show` method returns the correct student from the test database based on the student's ID. For each test, we are submitting a get request for the student we mocked out earlier, and we should expect a 200 response for the first test, and for `parsed_response['student']` to essentially be a valid student object returned from the test database. For the third test, we are asking to return a student with an ID of 10, which doesn't exist in our database. We should expect an error messaged from `parsed_response['error']`, and we should also expect the response to return a message saying that the object was not found.

```ruby spec/controllers/api/v1/students_controller_spec.rb
  describe "POST 'create' " do
    context "correct email format" do
      it "returns a successful json string with success message" do
        pending
      end
    end

    context "incorrect email format" do
      it "returns an error if an incorrect email format is submitted" do
        pending
      end  
    end
  end
end
```

This block is describing the create method, which will take in an email address parameter. If the email address is valid, then it will fire off an email to that email address, and then fire off a **success** message within a JSON response. If the email address is invalid, then it won't fire off the email, and will render an **invalid** message within a JSON response.

```ruby spec/controllers/api/v1/students_controller_spec.rb
  describe "POST 'create' " do
    context "correct email format" do
      it "returns a successful json string with success message" do
        post :create, { email: "newstudent@example.com" }
        expect(response).to be_success
        parsed_response = JSON.parse(response.body)
        expect(parsed_response['success']).to eq("Accepted email format.")
      end
    end

    context "incorrect email format" do
      it "returns an error if an incorrect email format is submitted" do
        post :create, { email: "new@studentexample" }
        parsed_response = JSON.parse(response.body)
        expect(response).to be_bad_request
        expect(parsed_response['invalid']).to eq("Invalid email format.")
      end  
    end
  end
end
```

In the first test, we're passing in a valid email address inside of the post request for the `create` action. If it's valid, then we expect the JSON response to have a 200 status code, and we also expect `parsed_response` to have a **success** message as well. The second test passes in an invalid email address. We expect the JSON response to return a bad request status, more specifically a 400 status code, as well as a **invalid** message inside of the `parsed_response`.

In order to make all of these tests pass, here's how the corresponding `app/controllers/api/v1/students_controller.rb` file looks:

```ruby controllers/api/v1/students_controller.rb
module API::V1
  class StudentsController < ApplicationController
    before_action :find_student, only: [:student]

    def index
      @students = Student.all
      render json: @students
    end

    def show
      render json: @student
    end

    def create
      if valid_email?(params[:email])
        send_acceptance_email(params[:email])
        render json: { success: "Accepted email format." }
      else
        render json: { invalid: "Invalid email format." }, status: :bad_request
      end
    end

    private

    def find_student
      @student = Student.find(params[:id])
      rescue ActiveRecord::RecordNotFound
        render json: { error: "Student does not exist" }, status: :not_found
    end

    def valid_email?(email_address)
      !!(email_address =~ /.+\@.+\..+/)
    end

    def send_acceptance_email(email)
      NewStudentMailer.acceptance_email(email).deliver
    end
  end
end
```

The `index`, `show`, and `create` methods should be pretty straightforward, but perhaps I should elaborate more on the private methods. Within the context of building an API, we only need to focus on two: `find_student` and `valid_email?(email_address)`.

The `find_student` method will query the Student model and its corresponding ActiveRecord database in order to find the student object with the ID attribute specified in params. In the event that it cannot find that corresponding student and Rails throws a `ActiveRecord::RecordNotFound` error, then it will execute a rescue clause that will render a JSON response with two components: the message "Student does not exist" and a 404 status code ("Not Found").

The 'valid_email?(email_address)' method is simply a regex that will parse a parameter passed in, and determine whether or not it is a valid email address. If it is valid, it will fire off an email in the `send_acceptance_email(email)` method, but if it is not valid, then it will render a JSON response with an invalid format error message.
