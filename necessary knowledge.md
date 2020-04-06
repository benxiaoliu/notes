Design Patterns
====

system design
===

Data Structure Comparasion
===
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
* Djongo VS Flask

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




