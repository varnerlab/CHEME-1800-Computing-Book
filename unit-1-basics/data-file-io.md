# Data Input and Output

## Introduction
Working with data is one of the most common tasks in any computational project. For example, loading data into a program, doing some calculations using that data, and then saving the results to a file (either locally or in the cloud) describes the majority of programs you will write in this course, and likely in your future career. 

There are various tools and approaches to reading and writing data files, and for accessing data from online resources. This lecture will introduce a few common patterns, and data types and introduce some patterns for working with data from online data sources.

---

## Files
Fill me in.

## Application Programming Interfaces (APIs)

An [application programming interface (API)](https://en.wikipedia.org/wiki/API) is a set of rules, protocols, and tools for building and interacting with software applications. A RESTful API is an API that follows the architectural style of REST (Representational State Transfer), which is a design pattern for creating web services.

RESTful APIs are designed to be lightweight, flexible, and scalable, and they are based on the principles of REST, which include:
* __Client-server architecture__: In a RESTful API, the client, e.g., a web browser or a mobile application, and the server, e.g., a web server or a database in the cloud are separated and communicate through a network, usually the Internet. This allows the client and the server to be developed and maintained independently, making the API more flexible and scalable.
* __Statelessness__: In a RESTful API, the server does not maintain any state about interactions with the client. This means that each request from the client to the server must contain all the information required to understand and process the request; the server does not store any information about the client between requests. This makes the API easier to scale and maintain, as the server does not need to keep track of information about the client state.
* __Cacheability__: RESTful APIs are designed to be cacheable, which means that the responses from the server can be stored and reused by the client or a cache in the network. This makes the API more efficient, as it reduces the need to make unnecessary requests to the server.
* __Layered system__: A RESTful API can be used over a network of interconnected servers, where each server serves a specific purpose. This makes the API more scalable and maintainable, as it can be built and deployed modularly.

RESTful APIs are often used to expose the functionality of a web service or a database over the Internet, allowing clients to interact with the service using the [HTTP request-response model](https://en.wikipedia.org/wiki/Hypertext_Transfer_Protocol). RESTful APIs are widely used in modern web and mobile development and are essential tools for building modern applications.

---

## Summary
Fill me in