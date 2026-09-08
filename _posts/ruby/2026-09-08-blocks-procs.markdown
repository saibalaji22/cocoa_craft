---
layout: post
title: "Blocs and Procs in Ruby"
categories: ruby
date: 2026-09-08
permalink: :categories/ruby-procs-blocks.html
---
# Blocks, Procs   
  
## Blocks  
  
* A block represents a piece of code which is attached to the end of the function  
* It is represented by do end block or {}  
* It is not an object. It is just a method syntax   
* The block can be triggered  from inside the method by using yield   
* The block can also takes parameters whose values are passed by the yield inside the method.  
*  We can check if a method has a block attached to its call by using block_given? method   
* A block can be triggered multiple times by calling yield multiple times  
* When a block is triggered using yield the method continues its execution it is not completely exited from its scope  
  
Example   
  
{% highlight ruby %}
def brackets
    puts "Starting the work"
    yield
    puts "Finished the work"
    yield
end

#calling the method 
brackets do
  puts "Logging..."
end
{% endhighlight %}

Here we have a brackets method. When calling the method we attach a block to it by using do…end block. We trigger the block inside the method by using yield.  
  
It will print the following output   
{% highlight ruby %}
Starting the work
Logging...
Finished the work
Logging...
{% endhighlight %}
  
  
We can also pass arguments to the yield which will be captured by the block attached to the method when calling it  
  
{% highlight ruby %}
def double_it(n)
    yield(n * 2)
end

#calling the method
double_it(4) do |n|
   puts "The double value is #{n}"
end
{% endhighlight %}


  
Here we trigger the outside block attached to double_it method with a parameter whose value is passed by the yield inside the method.  
  
  
We can check inside the method if a block is given or not by calling  block_given?   
  
{% highlight ruby %}
def describe(n)
    if block_given?
        yield(n)
        return
    end
    puts "Block not given so printing inside the function #{n}"
end
{% endhighlight %}

#calling the method with a block
describe(10) do |n|
   puts "Block given printing in the block outside the method #{n}"
end
#calling the method without a block
describe(20) 


  
yield also evaluates to the return value of the block — i.e., whatever the last expression inside the block computes.  
  
{% highlight ruby %}
def my_map(arr)
    result = []
    arr.each do |e|
        result << yield(e)
    end
    return result
end

double_array = my_map([2,4,6,8]){|e| e * 2}
puts double_array.inspect
#[4, 8, 12, 16]
{% endhighlight %}

Here we create a custom map higher order function using Block. It takes an array, doubles each element of the array and returns a new array.    
  
  
# Proc   
  
* A Proc is a block but it is turned into an object of type Proc   
* Unlike block Proc can be stored in a variable and passed as parameter to a function   
* Proc is created using Proc.new{} or Proc.new do..end   
* A proc can be triggered using call() method of the Proc object   
* A block attached to a method is converted to Proc inside the method  by adding a parameter to the method prefixed with &   
  
{% highlight ruby %}
greeting = Proc.new do |name|
    puts "Hello, #{name}"
end

def my_map(names,&greeting_block)
    names.each do |name|
        greeting_block.call(name)
    end
end

my_map ["Bruce","Clark","Barry"], &greeting

{% endhighlight %}
   
  
Another example where we at  
  
{% highlight ruby %}
def apply_twice(value, &action)
   result = action.call(value) #calls the attached block and returns the expression result 
   result = action.call(result) #calls the attached block and returns the expression result 
   return result
end

ans = apply_twice 5 do |x|
   x + 1
end

puts ans  # prints 7 
{% endhighlight %}

  
Here you can wonder why there is no coma(,) separated  between the  th 5 and do…end block. Here we are not passing a Proc object to the method. It is just a normal Block attached to the method. Inside the method the &action is a special parameter which converts any block attached to the method into a Proc object. Since the block is now a Proc it can be triggered using call method.   
  
* The thumb rule is if use pass a proc as an argument then add coma(,) between the arguments   
* If you directly attach a block to the method call then don’t add coma(,) between them  
  
If you try to do both like this   
  
{% highlight ruby %}
greeting = Proc.new do |name|
    puts "Hello, #{name}"
end

def my_map(names,&greeting_block)
    names.each do |name|
        greeting_block.call(name)
    end
end

my_map ["Bruce","Clark","Barry"], &greeting do |n|
  puts "TEST #{n}"
end
{% endhighlight %}

This will give an error  
  
{% highlight ruby %}
both block arg and actual block given; only one block is allowed

> 11  my_map ["Bruce","Clark","Barry"], &greeting do |n|
> 13  end

greeting.rb:11: syntax error found (SyntaxError)
{% endhighlight %}

