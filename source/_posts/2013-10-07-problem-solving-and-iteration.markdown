---
layout: post
title: "Problem Solving and Iteration - Week 3"
date: 2013-10-07 08:48
comments: true
categories: 
---

Last week, we dove right into database iteration and management.  We focused on the following: SQL, hash iteration, web scraping (via Nokogiri, Open-URI), and logic approaches.  

SQL and hash iteration are pretty straightforward as concepts, but they are difficult (for the time being) for me to effectively implement.  We had to implement these concepts in the web-scraping exercise we did last week, and I still have to go back over and do it again, just to make sure I get the concepts down pat.  

So, to explain: SQL is a database tool that can be used to integrate data from hashes and arrays into organized tables.  You can also use SQL to join tables together where they share common information (usually something like an ID or user ID -- keys that can tie two charts together).  the exercise that we did last week focused on Reddit.  We used Open-URI to open a connection to Reddit's front page, and Nokogiri pulled down the HTML data into a readable, concise format.  We then used Nokogiri's syntax to sort out the data that we needed for the exercise (Top 100 posts: name of post, URL of post origin, upvotes, downvotes).

In order to create SQL tables, you use the following command:
				CREATE TABLE users 
						(
						var1 TYPE,
						var2 TYPE,
						var3, TYPE
						); 

This just creates the tables with the variables var1, var2, var3, and they are of data type TYPE (which can be INTEGER, TEXT, DATE, etc).  

To insert values into the table, you use the following command:
				INSERT INTO users VALUES (var1, var2, var3)
						("John", 25, 2010),
						("James", 21, 2011),
						("Steve", 19, 2010);

To follow up with that, there are JOINS statements: INNER JOIN, LEFT OUTER JOIN, RIGHT OUTER JOIN, AND FULL OUTER JOIN.  
		
		INNER JOIN - Produce only records for which there is a match from table A and table B

		LEFT OUTER JOIN - Produces complete set of records for table A, along with matching records from table B if they exist; otherwise the value is NULL.

		RIGHT OUTER JOIN - Produces complete set of records for table B, along with matching records from A if they exist; otherwise the value is NULL.

		FULL OUTER JOIN - Produces complete set of records from both tables; when there isn't a match, the value is NULL.

Moving on to Hashes.  Hashes are one of the most fundamental concepts in the Ruby language, and enable a wide array of possibilities if they are properly utilized.  In other languages, the hash structure is referred to as a dictionary.  It has a key-value pair, so for each respective key, there is a value, just as in a dictionary, there is a definition for each word.  

The way to visualize a hash is via the following:
					hash = { :dog => 'canine', :cat => 'feline'}

How do you access the value for 'dog'?  By simply typing in hash[:dog].  This will return the string 'canine'.  How do you add a new element to the hash?  By simply typing in the key you want into the key field, and setting it equal to its value.  For example, hash[:elephant] = 'mammal' will insert :elephant => 'mammal' into the hash.  Finally, the default return value of a hash is nil.

My objective for this week is to work on further solidifying my understanding of hashes, how to iterate through them effectively, and utimately get a web-scraping objective finalized.  

