Design Patterns
====

system design
===

Data Structure Comparasion
===
https://medium.com/omarelgabrys-blog/data-structures-a-quick-comparison-6689d725b3b0

Java basics
===
* OOP principles
* scope of private, protect, public

python VS Java
===

compile or inerprete language
===

UUID
===

一、概述
UUID是128位的全局唯一标识符，通常由32字节的字符串表示。它可以保证时间和空间的唯一性，也称为GUID，全称为：
UUID —— Universally Unique IDentifier  Python 中叫 UUID
GUID —— Globally Unique IDentifier     C#  中叫 GUID
复制代码它通过MAC地址、时间戳、命名空间、随机数、伪随机数来保证生成ID的唯一性。
UUID主要有五个算法，也就是五种方法来实现：
二、方法
1、uuid1()——基于时间戳
由MAC地址、当前时间戳、随机数生成。可以保证全球范围内的唯一性，但MAC的使用同时带来安全性问题，局域网中可以使用IP来代替MAC。
3、uuid3()——基于名字的MD5散列值
通过计算名字和命名空间的MD5散列值得到，保证了同一命名空间中不同名字的唯一性，和不同命名空间的唯一性，但同一命名空间的同一名字生成相同的uuid。
5、uuid5()——基于名字的SHA-1散列值
算法与uuid3相同，不同的是使用 Secure Hash Algorithm 1 算法。

再次，若在Global的分布式计算环境下，最好用uuid1；
最后，若有名字的唯一性要求，最好用uuid3或uuid5。


Restful API
===
* REpresentational State Transfer (REST) is an architectural style that defines a set of constraints to be used for creating web services. REST API is a way of accessing the web services in a simple and flexible way without having any processing.

* Architectural Constraints of RESTful API: There are six architectural constraints which makes any web service are listed below:

Uniform Interface
Stateless
Cacheable
Client-Server
Layered System
Code on Demand (The only optional constraint of REST architecture)

    1. Uniform Interface: It is a key constraint that differentiate between a REST API and Non-REST API. It suggests that there should be an uniform way of interacting with a given website.
    There are four guidelines principle of Uniform Interface are:

          Resource-Based: Individual resources are identified in requests. For example: API/users.

          Manipulation of Resources Through Representations: Client has representation of resource and it contains enough information to modify or delete the resource on the server, provided it has permission to do so. Example: Usually user get a user id when user request for a list of users and then use that id to delete or modify that particular user.

          Self-descriptive Messages: Each message includes enough information to describe how to process the message so that server can easily analyses the request.

          Hypermedia as the Engine of Application State (HATEOAS): It need to include links for each response so that client can discover other resources easily.

    2. Stateless: It means that the necessary state to handle the request is contained within the request itself and server would not store anything related to the session. In REST, the client must include all information for the server to fulfill the request whether as a part of query params, headers or URI. Statelessness enables greater availability since the server does not have to maintain, update or communicate that session state. There is a drawback when the client need to send too much data to the server so it reduces the scope of network optimization and requires more bandwidth.

    3. Cacheable: Every response should include whether the response is cacheable or not and for how much duration responses can be cached at the client side. Client will return the data from its cache for any subsequent request and there would be no need to send the request again to the server. A well-managed caching partially or completely eliminates some client–server interactions, further improving availability and performance. But sometime there are chances that user may receive stale data.

    4. Client-Server: REST application should have a client-server architecture. A Client is someone who is requesting resources and are not concerned with data storage, which remains internal to each server, and server is someone who holds the resources and are not concerned with the user interface or user state. They can evolve independently. Client doesn’t need to know anything about business logic and server doesn’t need to know anything about frontend UI.

    5. Layered system: An application architecture needs to be composed of multiple layers. Each layer doesn’t know any thing about any layer other than that of immediate layer and there can be lot of intermediate servers between client and the end server. Intermediary servers may improve system availability by enabling load-balancing and by providing shared caches.

    6. Code on demand: It is an optional feature. According to this, servers can also provide executable code to the client. The examples of code on demand may include the compiled components such as Java applets and client-side scripts such as JavaScript.

 * Rules of REST API: There are certain rules which should be kept in mind while creating REST API endpoints.

          1. REST is based on the resource or noun instead of action or verb based. It means that a URI of a REST API should always end with a noun. Example: /api/users is a good example, but /api?type=users is a bad example of creating a REST API.
          2. HTTP verbs are used to identify the action. Some of the HTTP verbs are – GET, PUT, POST, DELETE, UPDATE, PATCH.
          3. A web application should be organized into resources like users and then uses HTTP verbs like – GET, PUT, POST, DELETE to modify those resources. And as a developer it should be clear that what needs to be done just by looking at the endpoint and HTTP method used.
          4. Always use plurals in URL to keep an API URI consistent throughout the application.
          5. Send a proper HTTP code to indicate a success or error status.


* REST VS SOAP
Since SOAP is a protocol, it follows a strict standard to allow communication between the client and the server whereas REST is an architectural style that doesn’t follow any strict standard but follows six constraints 
SOAP uses only XML for exchanging information in its message format whereas REST is not restricted to XML
Benefits of SOAP over REST as SOAP has ACID complaints transaction. Some of the applications require transaction ability which is accepted by SOAP whereas REST lacks in it.
Benefits of SOAP over REST as SOAP has ACID complaints transaction. Some of the applications require transaction ability which is accepted by SOAP whereas REST lacks in it.
On the basis of Security, SOAP has SSL( Secure Socket Layer) and WS-security whereas REST has SSL and HTTPS. In the case of Bank Account Password, Card Number, etc. SOAP is preferred over REST. The security issue is all about your application requirement, you have to build security on your own. It’s about what type of protocol you use.
REST can use SOAP protocol but SOAP cannot use REST.
REST technology is generally preferred to the more robust Simple Object Access Protocol (SOAP) technology because REST uses the less bandwidth, simple and flexible making it more suitable for internet usage. It’s used to fetch or give some information from a web services. All communication done via REST API used only HTTP request. 

* commonly used HTTP methods

GET: Retrieves one or more resources identified by the request URI and it can cache the information receive.

POST: Create a resource from the submission of a request and response is not cacheable in this case. This method is unsafe if no security is applied to the endpoint as it would allow anyone to create a random resource by submission.

PUT: Update an existing resource on the server specified by the request URI.

DELETE: Delete an existing resource on the server specified by the request URI. It always return an appropriate HTTP status for every request.

Regular expression
===

framwork
===
* Djongo VS SpringMVC
* Tornado VS Djongo

database
===
* ACID
* Transaction


my projects
===
* OFE pipeline
* Scope vs sql
* COSMOS
* EAther
* visualizetion website
* plugin
* NLP course
* ML course




