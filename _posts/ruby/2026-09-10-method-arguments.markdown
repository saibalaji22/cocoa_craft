---
layout: post
title: "Method Arguments"
categories: ruby
date: 2026-09-10
permalink: :categories/ruby-method-arguments.html
---

# Method Arguments

Consider the following code

{% highlight ruby %}
def greet(name,is_admin)
    if is_admin
        puts "Hello #{name}"
    else
        puts "Hello #{name} you are not admin"
    end
end
greet "Tom", true
#parameter order matters. Ruby does not enforce strict parameter order check.
greet false, "Tome"  #prints "Hello false"
{% endhighlight %}

In Ruby ordinary methods will not check the position of the argument so you must make sure you pass correct value for each argument to prevent binding them in wrong order. In the second one you are passing the arguments in wrong order so the bool gets bonded to the name parameter and the string to is_admin parameter. Since string is a truthy it will execute wrong condition.


## Default Arguments

We can also assign default value for the parameters so that when the calling the function if we do not pass an argument for that parameter it will take the default value assigned to it.

{% highlight ruby %}
def greet(name,message = "Hello")
    puts "#{message}, name"
end
greet "Tom" #uses the default value of message paramter
greet "Sam", "Good morning"
{% endhighlight %}


## Splat operator

The  * is called as splat operator. It is used with arrays. It has two uses

* To combine multiple arguments passed to a function as single array parameter
* To expand an array argument to individual params


## To combine multiple arguments passed to a function as single array parameter

Consider the following code

{% highlight ruby %}
def sum(*numbers)
    puts "The numbers are"
    puts numbers.inspect
    puts "The sum is #{numbers.sum}"
end
sum(1,2,3,4,5,6)
{% endhighlight %}

Here we are passing several arguments to the sum. They all get grouped into a single array.

What will happen if the method has two parameters a splat and a normal one. In that case the last argument get assigned to the extra parameter while all others get grouped into an array

{% highlight ruby %}
def sum(*numbers,extra)
    puts "The numbers are"
    puts numbers.inspect
    puts "The extra is"
    puts extra
    puts "The sum is #{numbers.sum}"
end
sum(1,2,3,4,5,6)
{% endhighlight %}

It will give following output

{% highlight plaintext %}
The numbers are
[1, 2, 3, 4, 5]
The extra is
6
The sum is 15
{% endhighlight %}

## To expand an array argument to individual params

We can also use splat operator when calling the method which has fixed number of parameters but we pass an array. In that case each array element will get assigned to the each parameter of the method


{% highlight ruby %}
def sum(one,two,three)
    print one, two, three
end
sum(*[1,2,3])
{% endhighlight %}

Here each element of the array gets assigned to one, two and three parameter of the the sum.

What will happen if the array we passed has four elements while the method has only 3 params .

{% highlight ruby %}
def sum(one,two,three)
    print one, two, three
end
sum(*[1,2,3,4])
{% endhighlight %}

This will throw an error

{% highlight plaintext %}
test.rb:1:in 'Object#sum': wrong number of arguments (given 4, expected 3) (ArgumentError)
caller: test.rb:4
    | sum(*[1,2,3,4])
      ^^^
    callee: test.rb:1
    | def sum(one,two,three)
          ^^^
	from test.rb:4:in '<main>'
{% endhighlight %}

The number of array elements should exactly match with the number of params in the method.


## Keyword Arguments or Named Arguments

In ruby we can also use keyword arguments. It has the following advantages

* Keyword arguments ensures we can pass the arguments in any order. Since each argument is explicitly labeled, you can't accidentally swap two same-typed values into the wrong slots.

* It explicitly states what value should be passed as an argument the call becomes self documenting


{% highlight ruby %}
def check_access(user_name:,is_admin:)
    if is_admin
        puts "The given user #{user_name} is an admin, Welcome"
    else
        puts "#{user_name} you are not an admin, the incident will be recorded"
    end
end

check_access user_name: "Tom", is_admin: true

check_access is_admin:false, user_name: "Ben" #order swapped still it will work properly
{% endhighlight %}

Here in second call the order of the arguments are swapped since the arguments are explicitly labelled it will work without any problem.
The number of arguments should match exactly with the number of parameters if not it will raise an error



We can also provide default values to the keyword arguments. If we omit the value for the argument the default value will be used.

{% highlight ruby %}
def check_access(user_name:,is_admin: false)
    if is_admin
        puts "The given user #{user_name} is an admin, Welcome"
    else
        puts "#{user_name} you are not an admin, the incident will be recorded"
    end
end

check_access user_name: "Tom", is_admin: true

check_access user_name: "Ben" #default value false will be used here
{% endhighlight %}

In all of the above cases unless a parameter has a default value we cannot omit it when calling the function.