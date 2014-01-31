---
layout: post
title: "SCALA Basics: An Introduction"
date: 2014-01-21 14:55
comments: true
categories: scala introduction basics function
published: true
---

Scala is a programming language meant to address scalability issues that confront a large number of developers today. It is a statically typed language, meaning that it binds a type to a variable for the variable's lifetime, whereas dynamically typed languages bind the type to the value referenced by a variable. Simply, statically typed variables are immutable, whereas dynamically typed variables are mutable. Scala also supports both functional and object-oriented programming. It seems that these attributes would conflict with one another, but the argument is that both attributes in conjunction lead to synergies in both performance and architecture. 

Let's get started on the introduction.

```scala Print Out a String
val book = "Let's start learning about Scala"
// java.lang.String = Let's start learning about Scala
println(book)
// Let's start learning about Scala
```

`val` is used to declare a read-only variable named `book`. In the next exercise, let's create a class that will take a number of parameters and then upcase them. We'll also refactor this function to make it more efficient and readable. Let's do it in the space below:

```scala Upper Case Function
	class Upper {
		def upper(strings: String*): Seq[String] = {
			strings.map((s:String) => s.toUpperCase())
		}
	}
``` 

We've defined the class `Upper`, and specified its parameter `strings` as an undefined number of string values, evidenced by the splat in `String*`. Then we specify the return type `Seq[String]` as a collection (or array) of strings. Then inside of the method's body, where we call the `map` method on the strings array, and passed in `(s:String) => s.toUpperCase()` as a function literal. In other words, it executes that function for each mapped parameter of the array `strings`.  

Now let's initialize the Upper Case Function below:

```scala Initializing Upper Case Function
val up = new Upper
Console.println(up.upper("run","ian","Fly","hero"))
// Array(RUN, IAN, FLY, HERO)
```

The `upper` method in the `Upper` class converts each parameter into an uppercase string, and returns them in an array. 

Now let's refactor.  

```scala Upper Case Function Refactored #1
object Upper {
	def upper(strings: String*) = strings.map(_.toUpperCase())
}
println(Upper.upper("run","ian","Fly","hero"))
// Array(RUN, IAN, FLY, HERO)
```

We've now declared an `object`, which in Scala is a singleton. In this situation, we only ever need one instance of Upper run at a time, so a singleton works appropriately. The `_` element inside of the `map` method is a placeholder variable that each string element will be assigned to before `toUpperCase()` is applied. 

This is the first day I've used Scala, and my first impression is that it's a lot like a hybrid of Ruby and Java/JavaScript. I still need to get better at understanding how collections are referenced to and organized inside of functions, but I'm looking forward to learning more Scala. 

