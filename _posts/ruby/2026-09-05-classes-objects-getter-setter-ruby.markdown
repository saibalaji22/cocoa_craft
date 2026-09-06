---
layout: post
title: "Classes, Objects, Getters, Setters and Accessors in Ruby"
categories: ruby
date: 2026-09-05
permalink: :categories/rubyclasses.html
---

# RUBY OOPS  
  
 
  
## Classes, Objects, Getters, Setters and Accessors  
  
Classes are building blocks of Object Oriented Programming. They are used to define a blue print based on which objects can be created for the class.  
  
The class syntax in ruby is   
  
{% highlight ruby %}
class CLASS_NAME
  // class methods and properties
end
{% endhighlight %}

  
* Here CLASS_NAME is the name of the class and in ruby the class name should always start with a capital letter   
* A class can have variables, methods, initializers, getters and setters.  
  
## Objects   
We can create object for the class by using the following syntax   
  
{% highlight ruby %}
object_name = CLASS_NAME.new
{% endhighlight %}
  
  
## Variables   
  
Classes can have variables inside them. The variables inside the class can come into the following categories   
  
* Instance variables  
* Class variables   
* Constants   
  
## Instance Variables  
  
* The instance variables are the variables which belong to the object of the class.  
* Each object will receive its own copy of instance variables   
* In Ruby instance variables are declared with @ symbol infront of them.  
* The values for the instance variables are assigned by using initializer and setters.  
* We can access the instance variable values within the class and outside the class. For accessing outside the class we use getters.  
  
{% highlight ruby %}
class Car
  def initialize(name, model, year)
    @name = name
    @model = model
    @year = year
  end

  # getters
  def name
    return @name
  end

  def model
    return @model
  end

  def year
    return @year
  end
end

c1 = Car.new("Nissan", "R35", 2015)
puts c1.model
puts c1.name
puts c1.year
{% endhighlight %}
  
###  Setters  
  
* Another approach to assign values to instance variables is by setters. The setters have the following syntax   
  
{% highlight ruby %}
def SETTER_NAME=(value)
  @INSTANCE_VARIABLE = value
end
{% endhighlight %}

Here SETTER_NAME can be anything but it is a common practice to keep it same as the INSTANCE_VARIABLE  
  
* We can access the setter via the object of the class  
  
### Getters  
  
* Getters are used to read the values of the instance variables outside of the class   
* Without getters we cannot access an instace variable outside of the class through an object    
  
  
  
{% highlight ruby %}
class Car
  # setters
  def name=(name)
    @name = name
  end

  def model=(model)
    @model = model
  end

  def year=(year)
    @year = year
  end

  # getters
  def name
    return @name
  end

  def model
    return @model
  end

  def year
    return @year
  end
end

# object creation
c1 = Car.new
# accessing the setters
c1.name = "Nissan"
c1.model = "R35 GTR"
c1.year = 2015
# accessing the getters
puts c1.model
puts c1.name
puts c1.year
{% endhighlight %}
  
### Accessors  

Accessors are shorter way to represents getter and setters of a class  

### Getters using Accessors  
  
We can use attr_reader to read the values of the instance variable. It is similar to getter.  
  
{% highlight ruby %}
attr_reader :INSTANCE_VARIABLE1, :INSTANCE_VARIABLE2, . . .
{% endhighlight %}
  
Here INSTANCE_VARIABLE  symbol name  should be same as the instance variable present inside the class.  
  
For example the in the above class we can replace the getter methods with attr_reader accessor   
  
  
{% highlight ruby %}
class Car
  attr_reader :name, :year, :model

  def name=(name)
    @name = name
  end

  def model=(model)
    @model = model
  end

  def year=(year)
    @year = year
  end
end

c1 = Car.new
c1.name = "Nissan"
c1.model = "R35 GTR"
c1.year = 2015
puts c1.model
puts c1.name
puts c1.year
{% endhighlight %}

  
### Setters using Accessors  
  
We can use attr_writer to write the values to the instance variable. It is similar to setter.  
  
{% highlight ruby %}
attr_writer :INSTANCE_VARIABLE1, :INSTANCE_VARIABLE2, . . .
{% endhighlight %}

  
Here INSTANCE_VARIABLE  symbol name  should be same as the instance variable present inside the class.  
  
For example the in the above class we can replace the setter methods with attr_writer accessor   
  
  
{% highlight ruby %}
class Car
  attr_reader :name, :year, :model
  attr_writer :name, :year, :model

  def printDetails
    @model = "test"
    puts "The model is #{@model} and make is #{:year}"
  end
end

c1 = Car.new
c1.name = "Nissan"
c1.model = "R35 GTR"
c1.year = 2015
puts c1.model
puts c1.name
puts c1.year
c1.printDetails
{% endhighlight %}

  
We can also access the instance variables inside the instance method.

### Combining both getter and setter using accessor

{% highlight ruby %}
class Car
	attr_accessor :name, :year, :model
	def printDetails
		@model = "test"
		puts "The model is #{@model} and make is #{:year}"
	end
end
c1 = Car.new
c1.name = "Nissan"
c1.model = "R35 GTR"
c1.year = 2015
puts c1.model
puts c1.name
puts c1.year
c1.printDetails
{% endhighlight %}


### Encapsulation in Ruby

Encapsulation means binding of data and method together inside a class and controlling access to what type of data can be accessed outside the class. The core idea is that objet should hide its internal states and expose only required details. In ruby encapsulation can be provided by using getter and setters either via methods or attribute accessors. 



Example
Consider the following task

Task: Build a BankAccount class
Requirements:
1. Create a BankAccount class with:
    * account_number — should be readable from outside, but never changeable after creation
    * balance — should be readable from outside, but should NOT have a public setter (no one should do account.balance = 10000 directly)
2. Add these public methods:
    * deposit(amount) — adds to balance, but should raise an error if amount <= 0
    * withdraw(amount) — subtracts from balance, but should raise an error if:
        * amount <= 0
        * the withdrawal would make balance go negative
3. Add a private helper method called sufficient_funds?(amount) that withdraw uses internally to check if there's enough balance. It should not be callable from outside the object.

{% highlight ruby %}
class BankAccount
  attr_reader :account_number #read only from outside 
  attr_reader :balance #read only from outside 

  def initialize(account_number,balance)
    @account_number = account_number
    @balance = balance
  end

  def deposit(amount)
    if amount <= 0
      raise "Invalid amount"
    end
    @balance += amount
  end

  def withdraw(amount)
    if amount <= 0 
      raise "Invalid amount"
    end
    if sufficient_funds?(amount) == false 
      raise "Insufficient balance"
    end
    @balance -= amount
  end



  private 
  def sufficient_funds?(amount)
    return amount <= @balance
  end

end

a1 = BankAccount.new(2342523,500)
a1.deposit 200
puts "Account number: #{a1.account_number} \nBalance: #{a1.balance}"
a1.withdraw 100
puts "Account number: #{a1.account_number} \nBalance: #{a1.balance}"
{% endhighlight %}
