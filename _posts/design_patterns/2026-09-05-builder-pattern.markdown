---
layout: post
title: "Builder Pattern - Creational Pattern"
categories: design-pattern
permalink: :categories/builder.html
---


# Creational Design Pattern - Builder   
* Builder is a creational design pattern which is used to construct complex objects step by step 

* It provides a way to create objects with different configurations using same construction code or pattern   
  
# The problem  
Consider a class or struct which has several properites in which some of them are optional and some are not and some properties are assigend values based on certain logic. We create object for that class or struct by passing the required values to the constructor and the values are assigned based on the logic inside the initializer. It creates a giant initializer problem with a dozens of required  paramters and optional parameters   
  
# The soulution   
We can solve this problem by using Builder Pattern. The builder pattern allows you to construct complex objects step by step with different configurations as per our requirement instead of passing all of them in one giant constructor.  
  
# Practical Example in iOS   
Consider a Network Service class which uses a factory pattern to create the ```URLRequest``` step by step.  
  
* First we define an enum for the Http Methods   
  
{% highlight swift %}
enum HttpMethod: String{
    case get = "GET"
    case post = "POST"
    case put = "PUT"
    case delete = "DELETE"
    case patch = "PATCH"
}
{% endhighlight %}

  
* Then we have a model struct representing various parameters needed for the construct the URLRequest   
  
{% highlight swift %}
struct NetworkRequest{
    let url: URL
    let httpMethod: HttpMethod
    let headers: [String:String]
    let body: Data
    let timeOutInterval: Double = 120.0 //2 minutes
}
{% endhighlight %}

  
* Then we have a network builder class which build the URL Request as per our requirements step by step using the builder methods. Note that here each builder  method returns ```self``` which represents the current instance and can be used to chain the method calls   
  
{% highlight swift %}
class NetworkRequestBuilder{
    private let url: URL
    private var httpMethod: HttpMethod = .get
    private var body: Data?
    private var headers: [String:String]  = [String:String]()
    private var timeOutInterval: Double = 120.0
    init(url: URL) {
        self.url = url
    }
    //builder methods
    func setHttpMethod(method: HttpMethod) -> NetworkRequestBuilder{
        self.httpMethod = method
        return self
    }
    func setBody(requestBodyData: Data) -> NetworkRequestBuilder{
        self.body = requestBodyData
        return self
    }
    func setHeaders(headers: [String:String]) -> NetworkRequestBuilder{
        self.headers = headers
        return self
    }
    func build()throws -> NetworkRequest{
        let methodWithBodyRequired: Set<HttpMethod> = [.put,.patch,.post]
        if methodWithBodyRequired.contains(self.httpMethod){
            throw NSError(domain: "Body should not be nil", code: -1)
        }
        return NetworkRequest(url: self.url, httpMethod: self.httpMethod, headers: self.headers, body: self.body!)
    }
    
}
{% endhighlight %}
  
* In ```URLSession``` we represent the network request as an instance URLRequest object. We can add an extension method to convert our NetworkRequest instance to URLRequest instance   
  
{% highlight swift %}
extension NetworkRequestBuilder{
    func asURLRequest() -> URLRequest{
        var urlRequest = URLRequest(url: self.url)
        urlRequest.httpMethod = self.httpMethod.rawValue
        self.headers.forEach { (key,value) in
            urlRequest.setValue(value, forHTTPHeaderField: key )
        }
        urlRequest.timeoutInterval = timeOutInterval
        urlRequest.httpBody = self.body
        return urlRequest
    }
}
{% endhighlight %}
  
Finally we can use our Builder to construct Network request object as per our need.  
  
{% highlight swift %}
let networkRequest = try NetworkRequestBuilder(url: URL(string: "https://jsonplaceholder.typicode.com/todos/1")!)
       .setHttpMethod(method: .get)
       .setHeaders(headers: ["Content-Type": "application/json"])
       .build()
{% endhighlight %}

  
{% highlight swift %}
let urlRequest = try! NetworkRequestBuilder(url: url)
    .setHttpMethod(method: .post)
    .setHeaders(headers: ["Content-Type": "application/json"])
    .setBody(requestBodyData: bodyData)
    .build()
    .asURLRequest()

{% endhighlight %}

