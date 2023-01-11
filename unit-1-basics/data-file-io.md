# Data Input and Output

## Introduction
Working with data is one of the most common tasks in any computational project. For example, loading data into a program, doing some calculations using that data, and then saving the results to a file (either locally or in the cloud) describes the majority of programs you will write in this course, and likely in your future career. 

There are various tools and approaches to reading and writing data files, and for accessing data from online resources. This lecture will introduce a few common patterns, and data types and introduce some patterns for working with data from online data sources.

---

## File Input and Output (File I/O)
File Input/Output (File I/O) are operations for reading and writing data to and from files on your computer system. File I/O is a common task in programming, as it allows you to store and retrieve data from persistent storage, such as a hard drive or a cloud service, e.g., [Box](https://en.wikipedia.org/wiki/Box_(company)), [Google Drive](https://en.wikipedia.org/wiki/Google_Drive) or [One Drive](https://en.wikipedia.org/wiki/OneDrive).

In most programming languages, file I/O operations are encoded using a set of built-in functions or methods that allow you to open, read, write, and close files of standard types such as [comma separated value (CSV)](https://en.wikipedia.org/wiki/Comma-separated_values) files, or modern file types such as the [Tom's Obvious Minimal Language (TOML) format](https://toml.io/en/), [JavaScript Object Notation (JSON) format](https://en.wikipedia.org/wiki/JSON) or [YAML files](https://yaml.org), YAML is a human-friendly data serialization
language for all programming languages. 

### CSV files
[Comma-separated value (CSV)](https://en.wikipedia.org/wiki/Comma-separated_values) files are delimited text files that use a comma to separate values. A CSV file typically stores tabular data (numbers and text) in plain text, where each line has the same number of fields. Each line of the file is a record, and each record consists of one or more fields, separated by commas (or other characters such as a `tab` or `space` character).

In engineering or other quantitative applications, [comma-separated value files](https://en.wikipedia.org/wiki/Comma-separated_values) are typically used to store, transmit and work with numerical data. Consider a comma-separated value file holding interest rate data for the last year:

```CSV
Date,T=20-year-percentage,T=30-year-percentage
2021-09-17,1.82,1.88
2021-09-24,1.84,1.89
2021-10-01,2.00,2.05
2021-10-08,2.05,2.10
....
```

This data file has a header row that contains column labels (which are text), while the other rows contain numerical data. Each row holds a data record, that is composed of fields that hold a type of data, e.g., the date or values for [the spot rate](https://www.investopedia.com/terms/s/spot_rate.asp) for [fixed income United States treasury debt securities](https://www.investopedia.com/ask/answers/033115/what-are-differences-between-treasury-bond-and-treasury-note-and-treasury-bill-tbill.asp).

#### Program: Read and write a CSV file

Let's develop a [Julia](https://docs.julialang.org) function that loads a [comma-separated value file](https://en.wikipedia.org/wiki/Comma-separated_values) from the local filesystem; first, let's use the [CSV.jl](https://github.com/JuliaData/CSV.jl) package, which is a 
specialized package to work with [comma-separated value files](https://en.wikipedia.org/wiki/Comma-separated_values):

```julia
using DataFrames
using CSV

function loadcsvfile(path::String)::DataFrame
    
    # check: is the path arg legit?
    # ...
    return CSV.read(path, DataFrame)
end

# set the path -
path_to_file = "Treasury-HistoricalData-09-09-22.csv";

# load csv file -
df = loadcsvfile(path_to_file);
```

This first example, which takes advantage of the [CSV.jl](https://github.com/JuliaData/CSV.jl) package, returns a table-like data structure called a `DataFrame` (which is implemented by the [DataFrames.jl](https://github.com/JuliaData/DataFrames.jl) package). The `DataFrame` data structure (which we'll explore later) offers several standard and advanced features for working with tabular data. 

One possible criticism of the first `loadcsvfile()` implementation is that all the details of what is going on are hidden in the `CSV.read()` call. However, there may be cases or applications where we may want more control when reading (or writing) [comma-separated value files](https://en.wikipedia.org/wiki/Comma-separated_values). 


#### Program: Read a CSV file refactored
Let's [refactor](https://en.wikipedia.org/wiki/Code_refactoring) the previous `loadcsvfile()` function so that we have access to each record as it is beging loaded:

```julia
function loadcsvfile(path::String; delim::Char=',', keyindex::Int64 = 1)::Tuple{Array{String,1}, Dict{String,Array{Number,1}}}
    
    # check: is the path arg legit?
    # ....

    # initialize
    counter = 1
    header = Array{String,1}()
    data = Dict{String,Array{Float64,1}}()

    # main -
    open(path, "r") do io # open a stream to the file
        for line in eachline(io) # read each line from the stream
            
            # split the line around the delim -
            fields = split(line, delim);
            if (counter == 1)
                
                # package the hedaer -
                for value in fields
                    push!(header, value)
                end

                # update the counter -
                counter = counter + 1
            else

                # package -
                tmp = Array{Float64,1}()
                keyfield = fields[keyindex]
                for (i,value) in enumerate(fields)
                    if (i != keyindex)
                        push!(tmp, parse(Float64,value))
                    end
                end
                data[keyfield] = tmp;
            end
        end
    end

    # return -
    return (header, data)
end

# set the path -
path_to_file = "Treasury-HistoricalData-09-09-22.csv";

# load file -
(h,d) = loadcsvfile(path_to_file);
```


### TOML files
TOML (Tom's Obvious, Minimal Language) is a configuration file format that is intended to be easy to read and write and is also easy to parse. It is used to store configuration data for applications. TOML files consist of key-value pairs, similar to a dictionary in Julia or Python, and can also include nested groups of keys. TOML files often have a `.toml` file extension.

```toml
# This is a TOML configuration file for a database

# section: holds connection information
[connection]
host = "localhost"      # The database hostname
port = 5432             # The port to connect to the database on
database = "mydatabase" # The name of the database
user = "myuser"         # The username to connect to the database with
password = "mypassword" # The password for the user
max_connections = 10    # The maximum number of connections to allow at once
connection_timeout = 30 # The amount of time to wait before timing out a connection

# section: holds a group of database options
[options]
ssl = true              # Whether to enable SSL connections to the database
ssl_mode = "require"    # The preferred SSL mode to use
```

TOML files are widely used for storing configuration information. For example, in [Julia](https://docs.julialang.org), the [package manager Pkg.jl](https://pkgdocs.julialang.org/v1/) stores information about the packages required for a project in a `Project.toml` file (which is automatically created when a project is activated). Because of its central role in [Julia](https://docs.julialang.org), `TOML.jl`, the package to read and write TOML files, is included in the [Julia standard library](https://docs.julialang.org/en/v1/stdlib/TOML/). Thus, we don't need to install it and can access it by placing the `using TOML` file at the start of our program.

```julia
# load packages 
using TOML

"""
    readtomlfile(path::String)::Dict{String,Any}

Load the TOML file at the path arg. Returns a Dict{String,Any} 
containing the TOML data.
"""
function readtomlfile(path::String)::Dict{String,Any}

    # check: does path point to a toml file?
    # ...

    return TOML.parsefile(path)
end

# setup path -
path_to_toml_file = "Database.toml"

# load -
d = readtomlfile(path_to_toml_file);
```

#### Additional resources
* The [TOML specification](https://toml.io/en/) describes the format and the different types of data that can be stored in a TOML file.


### JSON files
[JavaScript Object Notation (JSON)](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Objects/JSON) is a lightweight, text-based, language-independent data interchange format that is easy for humans to read and write, and easy for machines to parse and generate. JSON is based on a subset of the JavaScript programming language, and is used to represent simple data structures and associative arrays. 

JSON is composed of two data structures:

* A collection of name/value pairs typically realized as a struct, dictionary, keyed list, or associative array. 
* An ordered list of values. In most languages, this is realized as an array, vector, list, or sequence.

Here is an example of a JSON file that stores contact information:

```json
{
  "people": [
    {
      "name": "John Smith",
      "email": "john@example.com",
      "phone": "555-555-5555"
    },
    {
      "name": "Jane Smith",
      "email": "jane@example.com",
      "phone": "444-444-4444"
    }
  ]
}
```

This JSON file defines an object with a single key, `people`; the `people` key has a value that is a list of objects, each representing a person. Each person object has three keys, the `name`, `email`, and `phone` key. However, unlike TOML, the JSON format is not included in the [Julia standard library](https://docs.julialang.org/en/v1/stdlib/TOML/). Instead, there a variety of third-party packages available for reading and writing JSON files. 

```julia
# load required packages
using JSON

"""
    readjsonfile(path::String)::Dict{String,Any}

Load the JSON file at the path arg. Returns a Dict{String,Any} containing the JSON data.
"""
function readjsonfile(path::String)::Dict{String,Any}

    # check: does path point to a json file?
    # ...

    return JSON.parsefile(path)
end

# setup path -
path_to_json_file = "Contacts.json"

# load -
d = readjsonfile(path_to_json_file);
```

#### Additional resources
* The [TOML specification](https://toml.io/en/) describes the format and the different types of data that can be stored in a JSON file.

### YAML files
YAML (YAML Ain't Markup Language) is a human-readable data serialization language used to transmit data between systems. It is often used as a configuration file format for applications and is similar to JSON and TOML, but it is generally considered more readable and easier to write. YAML files use a simple syntax that consists of key-value pairs, and can also include nested groups of keys. They use indentation to denote structure, similar to Python and use various data types, including strings, numbers, booleans, and lists. YAML files often have a `.yaml` or `.yml` file extension.

Here is an example of a YAML file that could be used to store configuration data for an application:

```yaml
# This is a YAML configuration file for an application

# The application's name
name: My App

# The version of the application
version: 1.0.0

# The hostname to bind the application to
host: localhost

# The port to bind the application to
port: 8080

# A group of database options
database:
  # The database hostname
  host: localhost
  # The port to connect to the database on
  port: 5432
  # The name of the database
  name: mydatabase
  # The username to connect to the database with
  user: myuser
  # The password for the user
  password: mypassword
```


## Web services and APIs
A web service is a software system that enables machine-to-machine interaction over a network. Web services are often used to make the functionality of one application available to other applications or to provide data from one application to other. Web services can be accessed through a specified set of rules called an [application programming interfaces (APIs)](https://en.wikipedia.org/wiki/API). 

* An API is a set of programming instructions for accessing a web-based software application. It specifies how software components should interact and allows communication between different systems. Thus, an API defines how a developer writes a program that requests services, e.g., data over the internet.
* A web service is a specific type of API that uses the [HTTP protocol](https://en.wikipedia.org/wiki/Hypertext_Transfer_Protocol) to exchange data over the internet. Web services can transfer data in various formats, such as JSON or CSV.
* Finally, APIs are often associated with a specific implementation of a web service, although the terms are used interchangeably. For example, a website may have a public API and various internal APIs to manage its different components and features.

### RESTful APIs
Here we explore a particular type of API called a RESTful API. A RESTful API follows the Representational State Transfer (REST) architectural style, a widely used design pattern for web services.

RESTful APIs are designed to be lightweight, flexible, and scalable. They are based on the REST principles:
* __Client-server architecture__: In a RESTful API, the client, e.g., a web browser or a mobile application, and the server, e.g., a web server or a database in the cloud, are separated and communicate through a network, usually the Internet. This allows the client and the server to be developed and maintained independently, making the system more flexible and scalable.
* __Statelessness__: In a RESTful API, the server does not maintain information about interactions with the client. This means that each request from the client must contain all the information required to understand and process the request; the server does not store any information about the client between requests. This makes the system easier to scale and maintain, as the server does not need to keep track of information about the client’s state.
* __Cacheability__: RESTful APIs are designed to be cacheable, meaning that the server’s responses can be stored and reused by the client or a cache in the network. This increases the system’s efficiency; it reduces the need to make unnecessary requests to the server.
* __Layered system__: A RESTful API can be used over a network of interconnected servers, where each server does a specific task. This makes the system scalable and maintainable, as it can be built and deployed modularly.

RESTful APIs are often used to expose the functionality of a web service or a database over the Internet, allowing clients to interact with the service using the [HTTP request-response model](https://en.wikipedia.org/wiki/Hypertext_Transfer_Protocol). RESTful APIs are widely used in web and mobile development and are essential tools for building modern applications.

### What is HTTP?
The [Hypertext Transfer Protocol (HTTP)](https://en.wikipedia.org/wiki/Hypertext_Transfer_Protocol) is a network protocol used for the transmission of data on the World Wide Web. HTTP allows for communication between clients (such as web browsers) and servers on the web. [HTTP](https://en.wikipedia.org/wiki/Hypertext_Transfer_Protocol) is based on a request-response model, where the client sends a request to the server, and the server responds with the requested resource or an error message if the request cannot be fulfilled. 

For example, when looking for our class notes website:

* __Request__: When you enter a URL into your web browser, the browser sends an [HTTP request message](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods) to the server hosting the website. Thus, an HTTP request message is a question to a server written in a particular format that the server understands. 
* __Response__: Upon receiving the request, a server responds with an [HTTP response message](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status), which includes the website’s content and information about how to display it in the browser. 

Requesting data from an application programming interface works similarly; we make an HTTP request to the server, and we get back an HTTP response object. However, in this case, the HTTP response object is not a webpage; instead, we get data, typically organized in some text format such as [JavaScript Object Notation (JSON)](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Objects/JSON) that we can read in our program. Alternatively, we get an error message indicating that something went wrong with our request. 

Let's develop a function that makes a `GET` request from a [dummy application programming interface](http://dummy.restapiexample.com) shown in {prf:ref}`example-get-API-example`:

````{prf:example} GET Request
:class: dropdown
:label: example-get-API-example

Develop a function that calls an application programming interface (API) to retrive a list of employees. 

__Solution__: The executable Julia script below makes an API call and transforms the JSON data returned from the server into a Julia dictionary. The `makeapicallget(url::String)::Dict{String,Any}` calls the server and transforms the data into the final format.

```julia
# load external packages
using HTTP
using JSON

function makeapicallget(url::String)::Dict{String,Any}

    # ok, so we are going to make a HTTP GET call with the 
    # URL that was passed in -
    response = HTTP.request("GET", url; status_exception = false)

    # let's get the data contained in the response 
    # the data is conatined in body section of the response message
    data = String(response.body);

    # convert the JSON response body to a dictionary -
    response_body_dictionary = JSON.parse(data)

    # return -
    return response_body_dictionary;
end

# We are going to use a "fake" API to show the steps of making an API call
# http://dummy.restapiexample.com

# setup the url string -
url = "https://dummy.restapiexample.com/api/v1/employees"

# make the api call - data gets returned as JSON, 
# we then turn JSON into a Dict{String,Any} type
response_dictionary = makeapicallget(url);
```

To execute this program in [Julia](https://docs.julialang.org), save it to a file called `myapiexample.jl` and then use the [include](https://docs.julialang.org/en/v1/base/base/#Base.include) function from the [Julia REPL](https://docs.julialang.org/en/v1/stdlib/REPL/) to execute it:

```julia
include("myapiexample.jl")
```
````

---

## Summary

In this lecture we introduced topcs in data input and output.

* File I/O is an essential concept in programming, as it allows you to store and retrieve data that your programs can use. It is a fundamental building block of many applications and is used to create and manipulate files on the file system.