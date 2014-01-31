---
layout: post
title: "Create vs. Build in Rails"
date: 2013-11-19 17:03
comments: true
published: false
categories: 
---

There are two primary methods that are used in the controllers for generating new instances of objects.  The first one is the Create method, and the other is the Build method.  

The Create method creates an object and automatically saves it to the database, considering that validations in the controller have passed.  
