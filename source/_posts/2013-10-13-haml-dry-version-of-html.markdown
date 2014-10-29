---
layout: post
title: "HAML: DRY version of ERB?"
date: 2013-10-13 22:42
comments: true
categories: 
---
Embedded Ruby, or ERB for short, is a Ruby feature that allows a programmer to generate text from a template.  Its aim is to simplify flow control, and utilizes back-end web data to maintain repetitive front-end tasks, such as generating personalized web pages, emails, scripts, and any sort of text file.  In programming speak, ERB is Ruby's built-in template engine.

HAML, an alternative to ERB, stands for HTML Abstraction Markup Language.  HAML was created with Ruby's key principles in mind: markup should be legible, implement DRY, and be well-indented.  ERB uses HTML, which is a verbose markup language, and has proved problematic in meeting Ruby's standards for a seamless experience, and has seemingly felt incompatible with Ruby's language due to the large differences in syntactical structure.  HAML attempts to bridge that divide by creating a more seamless experience. 

Let's use an example to compare HAML and ERB.

<strong>ERB</strong>

```
<div class='dogs' id='sadie<%= dogs.sadie %>'>
  <%= dogs.body %>
</div>
```

<strong>HAML</strong>

```
.dogs{:sadie => "dogs#{dogs.sadie}"}= dogs.body
```

Look at the example above.  If you've used Nokogiri before, then the HAML example may remind you of something.  If you've ever used Nokogiri or an XML/HTML parsing gem before, you'll notice that the syntax is very similar to that of CSS queries for selectors.  Additionally, white space is used to denote indentation, as is the case with the "dogs.body" component in the ERB example.  Interpolation is very abundant in HAML as well, and there is no need to close tags in HAML.  Already, HAML has reduced verbosity significantly with just a simple example.

Let's move on to a more complex example.

<strong>ERB</strong>

```
<div class="cat">
	<div class="name"><%= name %></div>
	<% img_tag img %>
	<div class="kittens">
		<% kittens.each |kitten| %>
			<div class="kitten">
				<div class="name"><% kitten.name %></div>
			</div>
		<% end %>
	</div>
</div>
```

That was frustrating typing out all of the tags.  Now let's compare that to the HAML version.

<strong>HAML</strong>

```
.cat
	.name= name
	= img_tag img
	.kittens
		- kittens.each |kitten|
			.kitten
				.name= kitten.name
```

It's amazing how much HAML simplifies the markup template.  I think that I've displayed the benefits to using HAML.  However, there are two sides to this story.  Because browsers are designed to only understand HTML, there are those that believe abstracting away the key elements of the HTML language via HAML (or other alternatives) makes the front-end component a little more difficult.  There are also compatibility issues with CSS, particularly when it comes to web templates.  WIth that said, it is important to understand ERB simply because it is a universal language, and HAML is not.

I would love to use HAML, but it doesn't seem intuitive to use it when it is not widely utilized in the Ruby community.  I looked up some simple statistics on Stackoverflow.  There were 210,574 tags for HTML discussions alone, and that didn't include more specific HTML queries (HTML5, HTML parsing, XHTML, etc).  HAML had a whopping 1,831 tags for discussions.  I'm not a statistician, but I believe that there's statistical significance in that HAML is not widely accepted, and isn't quite a complete alternative to HTML.

With that said, it's nice to know that there are other options out there that are trying to bring Ruby's straightforward syntax to markup languages.




