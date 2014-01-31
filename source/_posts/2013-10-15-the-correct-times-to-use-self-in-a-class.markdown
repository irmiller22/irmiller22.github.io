---
layout: post
title: "The Correct Usage of 'self' in a Class"
date: 2013-10-15 08:44
comments: true
categories: 
---
One of the more confusing aspect of dealing with classes is knowing when to use 'self' appropriately.  In the context of a Class, 'self' refers to the current class, and is in itself an instance of the class Class.  I know this sounds confusing, but I'll break it down for you momentarily.  

```
class Dog
  attr_accessor :name, :breed

  def self.fake_bark
    "Awooooo!"
  end  
 
  def self.real_bark
    " Woof!"
  end  
end
```

In this example, self is referring to the class 'Dog'.  Basically, if the class Dog were to encompass all dogs alive in the world, any time that I used Dog.fake_bark, every single dog would give out a fake bark that sounds like "Awoooo!"  That's a husky or wolf attempting a feeble howl.  If I were to use Dog.real_bark, then every single dog in the world would give out a resounding "Woof!"  

Now let's look at the following example.

```
class Dog
	attr_accessor :name, :breed

  def fake_bark
    "Awooooo!"
  end  
 
  def real_bark
    " Woof!"
  end  
end
```

When self is not included in the method, that just means that it is a method that refers to an instance of the class 'Dog'.  So if I were to create a new dog named Fido that is a labrador, then I would do the following:

```
fido = Dog.new
fido.name = "Fido"
fido.breed = "Labrador"
```

Fido is an instance of the class Dog.  That is an importance distinction to make.  Now, if I call the fake_bark method on Fido without self prefixed, then only Fido will give out that weak attempt of a bark.  Every other dog in the world will be silent, because instances of every other dog hasn't been created in the Dog class.

It took me a while before I could distinguish between the different uses of self.  It's important to note the differences, because it can be come very problematic when you start to write your own programs or applications.  Self in the wrong place and lead to a lot of brain pain -- I learned this the hard way.  Do yourself a favor and learn all of the little nuances of the self keyword.  

