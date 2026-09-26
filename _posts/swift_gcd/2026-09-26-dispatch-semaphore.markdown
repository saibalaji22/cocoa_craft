---
layout: post
title: "GCD - Dispatch Semaphores"
categories: swift
date: 2026-09-26
permalink: :categories/dispatch-semaphores.html
---


# Dispatch Semaphores  
Dispatch semaphore is an object which is used to control the access to a resource which is shared my multiple execution context. In other words it is used to control the access to a resource which is shared by multiple threads. It is an integer counter. The counter value shows the number of processes which can access the shared resource in a given time.   
  
* When a thread access the resource it will decrement the counter value.  
* When the counter value becomes 0 no other thread can access the shared resource  
* When the thread finishes accessing the shared resource it will increment the counter value  
  
Semaphores provide the following methods   
* Signal - Decrements the counter value   
* Wait  - Increments the counter value   
  
Sempahores can be used to avoid race conditions. Race condition occurs when two or more process access a given shared resource concurrently with at least on write operation.   
  
For example consider the following code. Here it causes race condition  as there are three objects accessing the shared resource concurrently. It results in irregular balance update and output changes each time causing data inconsistency. This can be solved using DispatchSemaphores.  
   
{% highlight swift%}
class BankAccount{
    private var balance: Double
    init(balance: Double) {
        self.balance = balance
    }
    func deposit(value: Double){
        if value <= 0{
            print("Invalid amount")
            return
        }
        
        balance += value
        print("DEPOSITING \(value) UPDATED BALANCE IS \(balance)")
    }
    func withDraw(amount: Double){
        if amount > balance{
            print("Invalid amount")
            return
        }
        balance -= amount
        print("WITH DRAWING \(amount) UPDATED BALANCE IS \(balance)")
       
    }
    func getBalance() -> Double{
        return balance
    }
}


let a1 = BankAccount(balance: 2000)
let a2 = a1
let a3 = a1
print("CURRENT BALANCE IS \(a1.getBalance())")

DispatchQueue.global(qos: .background).async {
    a1.deposit(value: 10)
}

DispatchQueue.global(qos: .background).async {
    
    a2.withDraw(amount: 100)
}

DispatchQueue.global(qos: .background).async {
    a3.deposit(value: 10)
}

{% endhighlight %}
  
We can fix this problem by using Dispatch Semaphores. Here we create a dispatch semaphore with a counter value of 1. Which means only one process can access the shared resource in give time. All other process should wait till it completes its work. The process gets hold of the resource by calling wait() which will decrement the semaphore counter so that no other process can access the resource. Then it calls signal() after completing its work which will increment the semaphore counter value so that next process can access the shared resource.  
  
  
{% highlight swift %}
import Foundation


class BankAccount{
    private var balance: Double
    let sempahore = DispatchSemaphore(value: 1)
    init(balance: Double) {
        self.balance = balance
    }
    func deposit(value: Double){
        sempahore.wait()
        defer{
            sempahore.signal()
        }
        if value <= 0{
            print("Invalid amount")
            return
        }
       
        balance += value
        print("DEPOSITING \(value) UPDATED BALANCE IS \(balance)")
       
    }
    func withDraw(amount: Double){
        sempahore.wait()
        defer{
            sempahore.signal()
        }
        if amount > balance{
            print("Invalid amount")
            return
        }
        
        balance -= amount
        print("WITH DRAWING \(amount) UPDATED BALANCE IS \(balance)")
        
       
    }
    func getBalance() -> Double{
        return balance
    }
}


let a1 = BankAccount(balance: 2000)
let a2 = a1
let a3 = a1
DispatchQueue.global(qos: .background).async {
    a1.deposit(value: 10)
}

DispatchQueue.global(qos: .background).async {
    
    a2.withDraw(amount: 100)
}

DispatchQueue.global(qos: .background).async {
    a3.deposit(value: 10)
}

{% endhighlight %}
