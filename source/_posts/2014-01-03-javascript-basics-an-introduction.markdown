---
layout: post
title: "JavaScript Basics: An Introduction"
date: 2014-01-03 23:29
comments: true
categories: Javascript Loops Variables
published: true
---
Variable Declaration and Constant

```javascript Variables
var x = 10,
		y = 30, 
		myMath = x + y;
```
myMath in this case is the sum of variables x and y, which would be equal to 40.

```javascript Constant
//const is a keyword that will declare a constant
const CONSTANT = 22;
```
To find out what data type a variable is, you use the `typeof` method, as shown below:
```javascript Typeof method
const CAR = 2;
console.log(typeof CAR);
//--> number
var string = "hello";
console.log(typeof string);
//--> string
```

Conditional Statements

You can also use an if/else statement to execute a given statement if a certain condition is true.  For example, if you wanted to check and see if a variable was considered a type of string, you'd do the following:

```javascript Conditional
var name = "Ian";
if(typeof(name) == "string"){
	console.log("It is a string.");
}else{
	console.log("It ain't a string.");
}
```

Local vs. Global Scope

```javascript Local vs. Global Scope
var x = 5;
var y = 10;

function add(a,b){
	var x = a + b;
	return x;
}

function subtract(a,b){
	var y = a - b;
	return y;
}
```
The x and y variables that are outside of the add and subtract functions are called `global` variables.  They are declared outside of a function in the global scope, and are essentially accessible from anywhere inside of the program.  

Functions create a new scope.  The x variable inside the add function and the y variable inside of the subtract function are known as `local` variables.  They exist only in the context of the respective function, and are not accessible outside of the function.  Think of it as the Vegas motto; what happens here, stays here.

Arrays in Javascript

Making a list inside of our app would involve the following:
```javascript List in JS
var students = ["Ian Miller", "James Wood", "Saron Yitbarek", "John Cafferty"];
```

Looping Through Arrays 

```javascript Looping Through Array
function trackStudents(name, roster){
	if (roster.length == 0){
		roster.push(name);
	} else{
		for(var i=0; i < roster.length; i++) {
			if(roster[i] == undefined){
				roster[i] = name;
				return roster;
			} else if (i == roster.length - 1) {
				roster.push(name);
			}
		}
  }
  return roster;
}
```

This is just a simple looping function that takes a roster, and adds a name to the roster array.  If the array does not have any elements in it, then it will push that element onto the end of the array, which will be the first element.  Otherwise, for each element in the array, if an array position is undefined, then it will set that element equal to the name value.  Also, if it is the last element in the array, it will also push the name value to the end of the array.  Finally, once the whole array has been iterated through, it will return the array.

These are mostly for me to remember and repeat.  I'm currently branching out into other languages at the moment, and will be focusing on JS, jQuery, and Ajax.  
