---
layout: post
title: "The Art of a Question"
date: 2013-11-17 22:27
comments: true
categories: 
---
For once, I want to take a break from exploring programming topics, and explore the rationale behind asking the right question.  Or rather, is it insomuch asking the right question, or guiding yourself in the right direction?  I think it is a little bit of both, with more emphasis on guidance.  It's not so much the silver-bullet question that will find you the answers, but it's a series of small, seemingly mundane questions that will ultimately lead to the epiphany where all of the previously perplexing concepts suddenly click together into a cohesive abstraction.

From time to time, I have found myself unable to ask the right questions.  It's as if there is a mental block in my head, unable to move forward beyond an obstacle, a metaphorical wrench stuck in the gears of my neocortex.  When that process unfolds, it is easy to doubt yourself and your ability to lead yourself out of the quagmire.  There is a reason why many individuals are daunted by the prospect of learning how to program.  It is a seemingly endless black hole that manifests itself as a negative feedback loop.  Despite the organized chaos that arises, most often overlook the key decision making component of programming: knowing where to start.  

Knowing where to start isn't even the be-all-end-all solution to asking the right question.  It is simply a starting place.  Just because a runner starts at mile 0.0 after a starter pistol has fired, it does not mean that failure is imminent.  It is a mindset.  A runner tackles a marathon one step at a time.  Step by step, two feet turns into 10.  10 feet turns into a mile.  A mile turns into 10 miles.  10 miles turn into 20.  It's about breaking down the obstacle into manageable parts, and moving on from segment to segment.  This works because of two reasons: one, it keeps the logical decision making process flowing, and two, it pushes doubt and negative reinforcement to the periphery of human consciousness.

I read an article the other day discussing a concept called cognitive load.  According to George Miller, an eminent psychologist who taught at Princeton in the 1950's, our brain can realistically hold about seven pieces of information simultaneously at one point in time.  That number has since been revised to three or four.  This is a significant problem in the information age.  WIth historically recent advents such as the Internet, we are overly stimulated by the free flow of information, made possible largely by electronic devices.  Information enters and exits our cognitive consciousness that we don't grasp a basic level of understanding. We are unable to transfer it from our short-term to long-term memory stores, and consequently our ability to think critically and conceptually weakens.  Ergo, one step at a time.

So it seems that there are valid reasons, backed by science, psychology, and other studies focused on the human brain, that call for the fragmentation of a complete problem into parts.  If someone were to ask the question, "how does a route ultimately render a view in a Rails project?"  This is my thinking process below.

First off, there are multiple steps that between the route path and the view being rendered.  In MVC, there are typically eight steps in the chain of events. 

	First, a browser sends a request to the Rails router.  
	Second, a router sends the request to the respective controller.  
	Third, the controller asks the model to retrieve the respective data needed for the request from the database.  
	Fourth, the database sends that data back to the model.  
	Fifth, the model sends the retrieved data back to the controller.  
	Sixth, the data, encapsulated within the logic defined in the controller, is sent to the view.  Seventh, the view renders the data into the HTML page and is sent back to the controller.
	Eighth, and finally, the controller sends the view action back to the browser where it is rendered.

This is an oversimplified example, but the question went from a generic question about a route and respective view rendering to a multifaceted question made up of smaller implicit questions about the 8 steps of MVC progression.  Each step can also be broken down further to better understand the process.  Each little question is a foundation that can be built upon, and will ultimately to a deep understanding of how a simple Rails application works.

So perhaps the best question is not simply a targeted question that seeks a one-dimensional answer, but rather a series of smaller, more basic questions derived from a larger question.  Simplicity is the best form of policy.  Start somewhere.  Plant your flag in the sand.  Let your initial question, and the subsequent chain of questions, be your guide.  