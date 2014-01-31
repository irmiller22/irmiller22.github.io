---
layout: post
title: "RSpec: A Misunderstood Complement to Ruby"
date: 2013-10-20 21:37
comments: true
categories: [RSpec, Ruby, Testing]
sharing: true
---

For the past few weeks, I have been dreading the moment when I would have to utilize RSpec on a regular basis.  Because RSpec is the testing framework for the Ruby language, I viewed it as an obstacle to my learning development.  After studying more about RSpec in the past few days, I'm starting to realize that I have been viewing its fundamental purpose incorrectly.  RSpec is a complement to the Ruby developer's toolkit -- it prevents unneccesary work by testing the behaviors of a framework.  In short, it is the blueprint of any application, and represents a guided set of directions that leads you to a finished product.

In this blog post, I'll discuss the basic premise of RSpec and also discuss a few ways to refactor RSpec code.

``` ruby RSpec Basic Syntax https://speakerdeck.com/pat/rspec-introduction
describe 'basic RSpec syntax' do 
	it 'describes how the code should behave' do
		User.code.should be(clear)
		User.code.should be(concise)
	end
end
```

Here's a simple example.

``` ruby Calculator
describe Calculator do 
	describe '#multiply' do
		it 'returns the product of its parameters' do
			calc = Calculator.new
			calc.multiply(4,5).should eq(20)
		end
	end
end
```

This example is to just show what RSpec was intended for in regards to test-driven development.  See how RSpec determines how the calculator should behave when the multiply method is called?  When given the parameters 4 and 5, the multiply method should return an integer value of 20.  

``` ruby Refactor a name spec
describe Person do
	it 'responds to own name' do
		James = Person.new
		James.should respond_to(:name)
	end
end

describe Person do
	it 'responds to own name' do
		subject.should respond_to(:name)
	end
end
```

"Subject" always refers to an instance of a class.  In this case, James is an instance of the Person class, and the step of initializing a Person instance with the name James has been refactored by using "subject."  

``` ruby Using 'Expect' instead of 'Should'
describe Person do
	it 'responds to own name' do
		expect(subject).to respond_to(:name)
	end
end
```

This is a big change that I've started to implement in my RSpec tests, largely because of issues that have to do with delegation.  There was a blog post (http://myronmars.to/n/dev-blog/2012/06/rspecs-new-expectation-syntax) that discussed the differences between 'expect' and 'should.'  My understanding of the Kernel library and the rspec-expectations are not very concrete yet, but it is partly a result of a library load.  If one library is loaded before the other, it can sometimes override syntax delegations for the same word.

Another reason is that it seems a little more clear to me in terms of understanding what's going on with the syntax, particularly when I am starting a new test from scratch.  Should semantically makes sense if you're writing tests for existing code, but not necessarily if you haven't written a line of code yet.  






