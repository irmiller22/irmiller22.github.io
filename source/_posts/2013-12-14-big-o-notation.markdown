---
layout: post
title: "Big O Notation"
date: 2013-12-14 12:32
comments: true
categories: 
---

Big O Notation is simply a measure of an algorithm's performance.  It focuses on two primary components: speed and size.  It also presents a best-case scenario of an algorithm as well as a worst-case scenario.  

There are four primary scenarios that the majority of individuals will deal with in Big O Notation: O(1), O(n), O(n^2), and O(log n).

The O(1) case simply refers to an algorithm in which the performance result is the same no matter the size of the input.  For example, a change in the value of a pre-existing array element will not change the time it takes to execute the algorithm.  

```ruby O(1)
array = [1,2,3,4,5]

def is_1?(array)
	if array[0] == 1
		return true
	else
		return false
	end
end
```

The O(n) case is an algorithm that iterates over a range of elements, and the performance result increases for each additional element in the collection.  If it took 1 second to iterate through each element and I had a total of 10 elements in my collection, it would take 10 seconds (10/1).  If I had 500 elements in my collection, it would take 50 seconds (500/10).

The example below is a worst case scenario.  Since 10 is the last element in array, the each method won't break until it has run through and compared every value in the method.  If the comparator value were 1, then it would result in the best case scenario of O(1).  With every each statement that is used for comparison, it is always assumed that the worst case scenario is O(n), or the total size of the array.

```ruby O(n)
array = [1,2,3,4,5,6,7,8,9,10]

def comparison?(array)

array.each do |x|
	break if x == 10
end
```

The O(n^2) case refers to a function whose performance increases in proportion to the square of the input size.  When an additional element is added to a collection, then the performance result will increase by the square of n.  This is particularly common in cases where you are iterating with nested loops.  You can take this even further: a nested loop with deeper iterations will result in a higher power value for n.  A loop with 2 nested loops inside of it will result in O(n^3).  A well known example of this is the bubble sort.

```ruby O(n^2)
array = [8,1,9,7,5,3,2]

def bubble_sort(array)
	array.each_index do |x|
		(array.length - x - 1).times do |element|
			if array[element] > array[element + 1]
				array[element], array[element + 1] = array[element + 1], array[element]
			end
		end
	end
end
```

The O(log n) case is an algorithm that selects the middle element of the collection, and compares it against a specific value.  If the value is higher than the middle element, it will perform the same operation against the upper half of the collection.  If the value is lower than the middle element, it'll perform the same operation against the lower half.  It keeps splitting each respective collection until it finds the element it's looking for, or until all possibilities have been exhausted.  An example of this is the merge sort.

```ruby O(log n)
array = [8,7,3,2,5,1,4,9,6]

def merge_sort(array)
	return array if array.size < 2
	left = array[0, array.length/2]
	right = array[array.length/2, array.length-1]

	merge(merge_sort(left), merge_sort(right))
end

def merge(left, right)
	sorted_array = []
	until left.empty? || right.empty?
		sorted_array << (left[0] <= right[0] ? left.shift : right.shift)
	end
	sorted_array.concat(left).concat(right)
	p sorted_array
end
```