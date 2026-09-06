---
layout: post
title: "Class variables in Ruby"
categories: ruby
date: 2026-09-05
permalink: :categories/rubyclass-variables.html
---

# Class variables in Ruby 

They are similar to static variables in other languages. They are shared by all the instances of the class in other words all the instances receive same copy of the class variable. Changes made to the instance variable by one object will be reflected in the another object. Just like instance variables they cannot be accessed outside the class. Hence we use instance methods or class methods to access the class variables outside the class. Class variaables does not support attr_accessors, attr_reader and att_writer.

Class variables are denoted by @@ infront of them.

{% highlight ruby %}
class Car
	@@car_count = 0 #global variable
	attr_reader :model, :year #getters of instance variables

	def initialize(model,year)
		@model = model
		@year = year
		@@car_count += 1
	end

	# getter for class variable
	def get_car_count 
		return @@car_count
	end
end

c1 = Car.new("R35",2015)
c2 = Car.new("Leaf",2014)

puts c1.get_car_count # prints 2
puts c2.get_car_count # prints 2
{% endhighlight %}

Here we use ```get_car_count()``` instance method as a getter for class variable. Since car_count is shared by all the instances the updated count is shared by all the object. 

Common approach is to use class methods to access the class variables. Class methods have self before them in defination. And they are accessed by using the class name instead of the class instance 

{% highlight ruby %}
def self.get_car_count 
	return @@car_count
end
c1 = Car.new("R35",2015)
c2 = Car.new("Leaf",2014)

puts Car.get_car_count # prints 2
puts Car.get_car_count # prints 2
{% endhighlight %}

