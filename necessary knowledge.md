Design Patterns
====

system design
===

Ajax:
===

Update a web page without reloading the page
Request data from a server - after the page has loaded
Receive data from a server - after the page has loaded
Send data to a server - in the background


cornor cases
===


Data Structure Comparasion
===

https://medium.com/omarelgabrys-blog/data-structures-a-quick-comparison-6689d725b3b0

Binary Search Tree VS HashMap

HashMap: use Entry array and linkedlist to implement
call hashCode() -> calculate hash -> calculate index in the Entry array
if multiple keys get the same index at last, cur.next = head to insert new key value pair. if the list is too long, time complexity for searching reachs O(n), so when it's more than 8, convert the linked list to red-black tree.
when the length of the Entry array reaches its capacity, need to have a 2 times new Entry array and call transfer() to copy all items from the original array to the new one.
when add a key = null pair, will call putForNullKey()to go through table[0]Entry linked list, looking for the Entry where e.key = null, if found, e.value = newValue and return oldValue; if not found, call addEntry to add a new Entry where e.key = null.

process in hashMap is not safe. when two threads want to add a key value pair to the same Entry index, they will get the same head at first, then the previous added one will be lost. if you want to achieve safe process in hashMap, call static method collections.syncronizeMap()

concurrency and safe process, ConCurrentHashMap use segmentation lock, divide data into several segments and each segment has a lock.when one thread is writing the segment, other threads cannot read or write the segment. 
volitile promise current write operation will be finished before all read or write operations comes later.

hashTable has low efficiency because whole hashTable has one lock

hashMap VS hashTable
hashTable is syncronized, hashMap is not
hashTable doesn't allow null key or value, hashMap allows both

both of them use Entry array and linkedList, some detailed difference when implemented, like how to calculate the hash or expand the capacity...

connection pool
===

a connection pool is a cache of database connections maintained so that the connections can be reused when future requests to the database are required. Connection pools are used to enhance the performance of executing commands on a database. Opening and maintaining a database connection for each user, especially requests made to a dynamic database-driven website application, is costly and wastes resources. In connection pooling, after a connection is created, it is placed in the pool and it is used again so that a new connection does not have to be established. If all the connections are being used, a new connection is made and is added to the pool. Connection pooling also cuts down on the amount of time a user must wait to establish a connection to the database.

Dynamic web pages without connection pooling open connections to database services as required and close them when the page is done servicing a particular request. Pages that use connection pooling, on the other hand, maintain open connections in a pool. When the page requires access to the database, it simply uses an existing connection from the pool, and establishes a new connection only if no pooled connections are available. This reduces the overhead associated with connecting to the database to service individual requests.

Local applications that need frequent access to databases can also benefit from connection pooling. Open connections can be maintained in local applications that don't need to service separate remote requests like application servers, but implementations of connection pooling can become complicated. A number of available libraries implement connection pooling and related SQL query pooling, simplifying the implementation of connection pools in database-intensive applications.

Administrators can configure connection pools with restrictions on the numbers of minimum connections, maximum connections and idle connections to optimize the performance of pooling in specific problem contexts and in specific environments

Java basics
===
* OOP principles
* scope of private, protect, public

python VS Java
===

Java and Python have many similarities. Both languages have strong cross-platform support and extensive standard libraries. They both treat (nearly) everything as objects. Both languages compile to bytecode, but Python is (usually) compiled at runtime. They are both members of the Algol family, although Python deviates further from C/C++ than Java does.

Python and Java are both object-oriented languages, but Java uses static types, while Python is dynamic. This is the most significant difference and affects how you design, write, and troubleshoot programs in a fundamental way

We can’t mix types in a Java array. The code won’t compile.

In Python, we don’t have to provide a type when we declare the array and can put whatever we want in it. It’s up to us to make sure we don’t try to misuse the contents.

Static typing catches type errors at compile time.

Whether static typing prevents errors or not, it does make code run faster. A compiler working on statically-typed code can optimize better for the target platform. Also, you avoid runtime type errors, adding another performance boost.

Code that’s written with dynamic types tends to be less verbose than static languages. Variables aren’t declared with types, and the type can change. 

Putting together a Python program tends to be faster and easier than in Java. This is especially true of utility programs for manipulating files or retrieving data from web resources.

Both Java and Python compile to bytecode and run in virtual machines. This isolates code from differences between operating systems, making the languages cross-platform. But there’s a critical difference. Python usually compiles code at runtime, while Java compiles it in advance, and distributes the bytecode.

Most JVMs perform just-in-time compilation to all or part of programs to native code, which significantly improves performance. Mainstream Python doesn’t do this, but a few variants such as PyPy do.

The difference in performance between Java and Python is sometimes significant in some cases. A simple binary tree test runs ten times faster in Java than in Python.

 Java’s just-in-time compilation gives it an advantage over Python’s interpreted performance. While neither language is suitable for latency-sensitive applications, Java is still a great deal faster than Python.

compile or inerprete language
===

An interpreted language is a type of programming language for which most of its implementations execute instructions directly and freely, without previously compiling a program into machine-language instructions. The interpreter executes the program directly, translating each statement into a sequence of one or more subroutines, and then into another language (often machine code).

Many languages have been implemented using both compilers and interpreters, including BASIC, C, Lisp, and Pascal. Java and C# are compiled into bytecode, the virtual-machine-friendly interpreted language. Lisp implementations can freely mix interpreted and compiled code.

A compiled language is a programming language whose implementations are typically compilers (translators that generate machine code from source code), and not interpreters (step-by-step executors of source code, where no pre-runtime translation takes place).

The term is somewhat vague. In principle, any language can be implemented with a compiler or with an interpreter.[1] A combination of both solutions is also common: a compiler can translate the source code into some intermediate form (often called p-code or bytecode), which is then passed to an interpreter which executes it.

Java is commonly considered to be compiled

Initially, interpreted languages were compiled line-by-line; that is, each line was compiled as it was about to be executed, and if a loop or subroutine caused certain lines to be executed multiple times, they would be recompiled every time. This has become much less common. Most so-called interpreted languages use an intermediate representation, which combines compiling and interpreting.
Examples include:
JavaScript
Python

The intermediate representation can be compiled once and for all (as in Java), each time before execution (as in Ruby), or each time a change in the source is detected before execution (as in Python).

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



REST technology is generally preferred to the more robust Simple Object Access Protocol (SOAP) technology because REST uses the less bandwidth, simple and flexible making it more suitable for internet usage. It’s used to fetch or give some information from a web services. All communication done via REST API used only HTTP request. 

* commonly used HTTP methods

GET: Retrieves one or more resources identified by the request URI and it can cache the information receive.

POST: Create a resource from the submission of a request and response is not cacheable in this case. This method is unsafe if no security is applied to the endpoint as it would allow anyone to create a random resource by submission.

PUT: Update an existing resource on the server specified by the request URI.

DELETE: Delete an existing resource on the server specified by the request URI. It always return an appropriate HTTP status for every request.

Regular expression
===

framwork https://www.netguru.com/blog/python-frameworks-comparison
===
* Djongo VS SpringMVC
* Tornado VS Flask

What is Flask? a microframework for Python based on Werkzeug, Jinja 2 and good intentions. Flask is intended for getting started very quickly and was developed with best intentions in mind.

What is Tornado? A Python web framework and asynchronous networking library, originally developed at FriendFeed. By using non-blocking network I/O, Tornado can scale to tens of thousands of open connections, making it ideal for long polling, WebSockets, and other applications that require a long-lived connection to each user.

Tornado:
The integration with other frameworks and libraries is also possible: Twisted, asyncio or even WSGI applications. Tornado’s features:

offers a lot of generic classes that can be used for creating the application, e.g. Router, or SocketHandler for websockets,

custom HTML template engine,

clear and easy-to-read documentation,

functions and classes can be used for defining actions and handling requests,

custom routing handling – offers generic classes than can be used for route creation,

it supports WSGI, but it’s not recommended – the user should use Tornado’s own interfaces instead,

out-of-the-box websockets support, authentication (e.g. via Google), and security features (like cookie signing or XSRF protection),

no additional tools are needed for REST API creation.

The framework should work well in cases where there are a lot of incoming connections that can be handled quickly or in real-time solutions, e.g. chats. Tornado tries to solve the c10k problem so high processing speed is a priority. Another advantage of Tornado is its native support for social services. This framework won’t be a good choice for creating standard CRUD sites or big business applications, as it wasn’t designed to be used that way. For bigger projects, it can be integrated with WSGI applications as a part of their bigger structure and take care of tasks that require high handling speeds.



* Djongo VS Flask (https://www.netguru.com/blog/flask-vs-django-comparison)
Djongo https://www.netguru.com/blog/pros-and-cons-of-django
they are quite different in terms of what they implement in the framework, and what they leave for the developer to write.

Flask is a microframework that implements the bare minimum, leaving the developer with complete freedom of choice in terms of modules and add-ons.

Django on the other hand, adopts an all inclusive approach, providing an admin panel, ORM (Object Relational Mapping), database interfaces and so on.

Development speed
Django was created for fast development of complex web apps. developers have all the tools they need to implement and develop easily scalable, reliable, and maintainable web apps, in record time. Flask’s simplicity also allows experienced developers to create smaller apps in short timeframes.

Learning curve
For those not already familiar with Python or web frameworks, it is generally accepted that Django has a slightly steeper learning curve than Flask. Django does provide benefits if you need to change your development team mid-way through the process, or scale the app with a new team after project completion. New, experienced developers should be able to understand the project’s architecture and conventions more easily in Django, rather than taking over a project created in Flask.

Flexibility and control
One of Flask’s greatest strengths, is its minimalism and simplicity. No restrictions means that the developer can implement everything exactly as they want it, using a huge range of external libraries and add-ons, making it flexible and extensible. Django on the other hand, with its built-in features and modules, offers far less freedom and control.

Community
One of the main advantages of Django, is that it has a huge active developer community. This means that if you need help, or when it’s time to scale your app, you will have an easier job finding other developers to join in and start contributing, plus a wealth of useful content already in the public domain. The Flask community is currently not as big, and so information may be harder to come by.

Maturity
Django is a very mature framework, having seen its first release in 2005. This means that it has gathered numerous extensions, plugins and third party apps covering a wide range of needs. Flask by comparison, is much younger, having been introduced in 2010, so doesn’t have quite the same range of options available to it.

needs of your project:

Size of the project
The size of your project is a good starting point when selecting a framework. Flask is more suited to smaller, less complicated applications, while Django is designed for larger, more complex, and high-load applications. The future growth plans of your project should also be factored in. While Flask is flexible and highly extensible, you will need to be prepared to create most of the functionality yourself. Django, on the other hand will be easier to scale as it is fully featured out of the box.

Structure
The structure of your app may dictate which framework you should use. If you have a need for particular tools and libraries, or want a highly customisable framework, Flask will be the best choice. If, however, you don’t require the granular tuning that Flask allows, Django will save you a lot of time and money in development hours with its myriad of built-in features and reasonable defaults.

database
===
* ACID
* Transaction

how do you do test
===

my projects
===
* OFE pipeline
* Scope vs sql

SCOPE is a declarative language with extensibility, closely resembling SQL, providing the expressibility necessary for advanced analytics tasks and operating seamlessly on cloud scale data. One of the unique features of SCOPE is the combination of the SQL-like declarative language with the extensibility and programmability that's provided by C#, Python or R. For complex data mining and analysis applications, it may sometimes be complicated or impossible to express a desired operation with SQL-like commands. Examples include complex data manipulation, customized aggregates, or complex correlation etc. SCOPE provides five highly extensible operators that manipulate rowsets: Extractor, Processor, Applier, Reducer, and Combiner. SCOPE provides the flexibility to extend one’s analysis with custom algorithms written in C# or Python (Java in future) by extending these built-in operators. The code can also be easily reused in other SCOPE scripts. User code extensions operators in SCOPE enable developers to perform massively parallel execution of C#, Python, code for analytics scenarios covering: merging various data files, applying various analytical, sequential and temporal transformations, feature engineering (FE), partitioned data model building, massively parallel FE and scoring etc. In this guide, we concentrate on the extensibility and programmability of the SCOPE language that's enabled by Python.

Note: If a SCOPE script contains User Defined Operators (UDOs), the query execution plan generated by the SCOPE optimizer is suboptimal because UDOs by default are blackbox to the SCOPE optimizer. Furthermore, as the optimizer cannot determine what is being done by the code inside these objects, it cannot determine the characteristics of UDO.
you can write C# in Scope language, more easily to implement complex logic
you can even use python
* COSMOS

https://microsoft.sharepoint.com/:p:/r/teams/stca/ipe/_layouts/15/Doc.aspx?sourcedoc=%7B32C3A3BA-8E48-4AA9-851A-ECCDA5450056%7D&file=Introduction%20to%20COSMOS%20.pptx&action=edit&mobileredirect=true&DefaultItemOpen=1

Exporting Data From Cosmos Programmatically
https://microsoft.sharepoint.com/teams/Cosmos/Wiki/Exporting%20Data%20From%20Cosmos%20Programmatically.aspx
I want to download data from Cosmos, and then push to SQL or Azure DB. What are the best practices to keep in mind?
While exporting data from Cosmos, and then pushing it to another destination, we suggest that you handle the data by partition. We also suggest you implement a caching layer to reduce the chances of connection failures. Here is some Sample Code​



* EAther

Æther Platform

Overview

Æther system architecture consists of several entities which interact through Æther Service. This acts as a hub for all Æther functionality.

https://aetherwiki.azurewebsites.net/articles/FeatureAreas/images/Architecture_Diagram.png
Architecture Diagram

An Experiment in Æther is a collection of modules and datasources that form an executable. Experiments are represented as a graph of nodes where the output of one node can be used as the input for the next. 


Æther Service

Æther Service is what makes up the core functionality of Æther. It is a RESTful API that handles requests from the user to do things like submit experiments, get resource info, search queries, etc. It also manages routing data to and from various machines during the execution process.

Æther Service also manages data references and can clean up unused data automatically. See Æther Data Deletion Policy for more info.

For scenarios that require stricter security, a local instance of Æther Service can be run on your PC or machine cluster. See Æther OneBox for details.

User Clients

There are a number of ways in which the user can interact with Æther Service: 



Æther Client

A standalone application with a graphical user interface. (Æther Client is an application that provides Æther Services to users via a graphical user interface. )

Graph parameters are used to quickly replace parameter values in one or more modules in the execution graph before re-running the graph. However, graph parameter values could only take static values until now.

Æther now has Macros that can be used as values in graph parameters to dynamically populate parameter values. Parameter value can be specified used Macros as ##MACRO## in Æther.


Æther Programmatic

A Visual Studio plugin that wraps Æther Library with some of the GUI elements of Æther Client.


Æther Library

A C# Library that provides functionality for interfacing with Æther from code.

Compute Clusters

When executing a module, Æther Service will package up the neccessary files (binaries, scripts, inputs, etc.) and send them off to a machine or group of machines known as a compute cluster that will perform the module execution.

By default, Æther will use its proprietary "EXE Pool" cluster. However, it's often advantageous to use other cloud systems for computation, depending on the situation. For example, deep learning tasks are best done on machines that can handle a massive amount of multi-processing, hence the large-scale GPU cluster of the Philly cloud system would be better suited for this task.

See Cloud Systems Overview for a list of available cloud systems and their corresponding use case scenarios.

Storage

Naturally, datasources and module output need to be stored somewhere. Æther can make use of a number of storage options. By default, Æther stores this data on Cosmos, in Virtual Clusters designed to store public Æther data.

You can of course use your own VC instead, see Add a VC in Æther for instructions. If you would like your data to remain private, rather than accessible to all Microsoft employees, see Controlled Access Execution. 

You can also store data locally on your PC through the use of Æther OneBox.

Additionally, it's also possible to use Azure Blobs for storage. See Using Azure Blobs for details.


* visualizetion website
* plugin

why use d3.js

* NLP course
* ML course




