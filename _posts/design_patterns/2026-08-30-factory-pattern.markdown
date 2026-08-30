---
layout: post
title: "Factory Pattern - Creational Pattern"
categories: design-pattern
permalink: :categories/factory.html
---
# Creational Pattern - Factory  
  
It provides various **object creation mechanism** to increase the flexibility and reusability of existing code base.  
  
It includes the following patterns   
  
* Factory Pattern  
* Abstract Factory Pattern  
* Builder   
* Singleton   
* Prototype  
  
## Factory Pattern  
  
Factory pattern provides an interface for object creation in superclass and also allows the subclass to alter the type of object created by it.   
  
## The problem  
  
Consider a payment module in your app which uses Google Pay for payment. In future if you want to add Apple Pay, Line Pay, MerPay functionality to the payment module you need to directly add the payment methods for them inside the payment module, modifying the existing payment code. This problem can be avoided by using Factory Pattern.  
  
## Structure   
  
Factory pattern has the following components  
  
* **Product Interface** - A swift protocol which has a method representing what is the actual functionality of concrete products (the actual class or struct which implements this interface). It represents what the concrete product does but not how it does.  
  
For example consider a Payable interface:  
  
{% highlight swift %}
protocol Payable {
    func pay()
}
{% endhighlight %}
  
* **Products (Actual classes and structs)** - This represents the actual classes and structs which implement the Product interface.   
  
For example consider our payment module needs following payment features:  
  
{% highlight swift %}
class LinePay: Payable {
    func pay() {
        print("Paying with line pay")
    }
}
class MerPay: Payable {
    func pay() {
        print("Paying with Mer pay")
    }
}
class GooglePay: Payable {
    func pay() {
        print("Paying with google pay")
    }
}
{% endhighlight %}
  
* **The Creator or the factory** - This is the actual class which declares a factory method that returns a product object. It is important that the return type of the factory method should be of type Product Interface, which facilitates polymorphism.   
  
{% highlight swift %}
enum PaymentOptions {
    case googlePay
    case linePay
    case merPay
}
class PaymentCreatorFactory {
    class func makePaymentObject(type: PaymentOptions) -> Payable {
        switch type {
        case .googlePay:
            return GooglePay()
        case .linePay:
            return LinePay()
        case .merPay:
            return MerPay()
        }
    }
}
let linePay: Payable = PaymentCreatorFactory.makePaymentObject(type: .linePay)
linePay.pay()
let googlePay: Payable = PaymentCreatorFactory.makePaymentObject(type: .googlePay)
googlePay.pay()
let merPay: Payable = PaymentCreatorFactory.makePaymentObject(type: .merPay)
merPay.pay()
{% endhighlight %}
