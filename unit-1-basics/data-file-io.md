# Data Input and Output

## Introduction
Working with data is one of the most common tasks in any computational project. For example, loading data into a program, doing some calculations using that data, and then saving the results to a file (either locally or in the cloud) describes the majority of programs you will write in this course, and likely in your future career. 

There are various tools and approaches to reading and writing data files, and for accessing data from online resources. This lecture will introduce a few common patterns, and data types and introduce some patterns for working with data from online data sources.

---

## File Input and Output (File I/O)
File Input/Output (File I/O) are operations for reading and writing data to and from files on your computer system. File I/O is a common task in programming, as it allows you to store and retrieve data from persistent storage, such as a hard drive or a cloud service, e.g., [Box](https://en.wikipedia.org/wiki/Box_(company)), [Google Drive](https://en.wikipedia.org/wiki/Google_Drive) or [One Drive](https://en.wikipedia.org/wiki/OneDrive).

In most programming languages, file I/O operations are encoded using a set of built-in functions or methods that allow you to open, read, write, and close files. 


File I/O is an essential concept in programming, as it allows you to store and retrieve data that your programs can use. It is a fundamental building block of many applications and is used to create and manipulate files on the file system.

## Application Programming Interfaces (APIs)

An [application programming interface (API)](https://en.wikipedia.org/wiki/API) is a set of rules, protocols, and tools for building and interacting with software applications. A RESTful API follows the Representational State Transfer (REST) architectural style, a widely used design pattern for web services.

RESTful APIs are designed to be lightweight, flexible, and scalable. They are based on the REST principles:
* __Client-server architecture__: In a RESTful API, the client, e.g., a web browser or a mobile application, and the server, e.g., a web server or a database in the cloud, are separated and communicate through a network, usually the Internet. This allows the client and the server to be developed and maintained independently, making the system more flexible and scalable.
* __Statelessness__: In a RESTful API, the server does not maintain information about interactions with the client. This means that each request from the client must contain all the necessary information to understand and process the request; the server does not store any information about the client between requests. This makes the system easier to scale and maintain, as the server does not need to keep track of information about the client’s state.
* __Cacheability__: RESTful APIs are designed to be cacheable, meaning that the server’s responses can be stored and reused by the client or a cache in the network. This increases the system’s efficiency; it reduces the need to make unnecessary requests to the server.
* __Layered system__: A RESTful API can be used over a network of interconnected servers, where each server does a specific task. This makes the system scalable and maintainable, as it can be built and deployed modularly.

RESTful APIs are often used to expose the functionality of a web service or a database over the Internet, allowing clients to interact with the service using the [HTTP request-response model](https://en.wikipedia.org/wiki/Hypertext_Transfer_Protocol). RESTful APIs are widely used in web and mobile development and are essential tools for building modern applications. 

### What is HTTP?
The [Hypertext Transfer Protocol (HTTP)](https://en.wikipedia.org/wiki/Hypertext_Transfer_Protocol) is a network protocol used for the transmission of data on the World Wide Web. HTTP allows for communication between clients (such as web browsers) and servers on the web. [HTTP](https://en.wikipedia.org/wiki/Hypertext_Transfer_Protocol) is based on a request-response model, where the client sends a request to the server, and the server responds with the requested resource or an error message if the request cannot be fulfilled. 

For example, when looking for our class notes website:

* __Request__: When you enter a URL into your web browser, the browser sends an [HTTP request message](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods) to the server hosting the website. Thus, an HTTP request message is a question to a server written in a particular format that the server understands. 
* __Response__: Upon receiving the request, a server responds with an [HTTP response message](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status), which includes the website’s content and information about how to display it in the browser. 

Requesting data from an application programming interface works similarly; we make an HTTP request to the server, and we get back data as an HTTP response object. However, in this case, the HTTP response object is not a webpage; instead, we get data, typically organized in some text format such as [JavaScript Object Notation (JSON)](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Objects/JSON) that we can read in our program. Alternatively, we get an error message indicating that something went wrong with our request. 

---

## Summary
Fill me in