---
jupytext:
  formats: md:myst
  text_representation:
    extension: .md
    format_name: myst
kernelspec:
  display_name: Julia
  language: julia
  name: julia-1.8
---

# Data Structures

## Introduction
Data structures are ways of organizing and storing data in a computer so that it can be accessed and modified efficiently. Different data structures are suited to various applications; some are highly specialized for specific tasks. Some common examples of data structures that we will discuss include:

* {ref}`content:references:lda-arrays`: An array is a collection of items stored at contiguous memory locations. The items can be of any data type, and each item has a unique index that can be used to access it.
* {ref}`content:references:lda-linked-lists`: A linked list is a linear data structure in which each element is a separate object, connected to the next element by a pointer; linked lists store sequences of items, which can be expanded or contracted as needed.
* {ref}`content:references:lda-stacks`: A stack is a linear data structure that follows the "last-in, first-out" (LIFO) principle. This means that the last element added to the stack will be the first one to be removed. Stacks are often used to store data temporarily while a program is executing.
* {ref}`content:references:lda-queues`: A queue is a linear data structure that follows the "first-in, first-out" (FIFO) principle. This means that the first element added to the queue will be the first one to be removed. Queues are often used to store data that needs to be processed in a specific order or to store data that is being transferred from one place to another.
* {ref}`content:references:data-structure-hashmap-and-sets`: A hashmap is a data structure that stores key-value pairs, where each key value is unique. Sets are similar to hashmaps but only store keys and do not have values associated with them.
* {ref}`content:references:data-structure-tree`: A tree is a hierarchical data structure in which each node has one or more child nodes. Trees are used to store data that has a natural hierarchical structure, such as the structure of a file system or the organization of a company.
* {ref}`content:references:data-structure-graphs`: A graph is a data structure that consists of a set of vertices (also called nodes) and a set of edges connecting the vertices. Graphs are used to represent relationships between objects, and they are often used to model networks.

---

## Linear data structures
Linear data structures store elements linearly, meaning that they are arranged in a sequence one after the other. This contrasts non-linear data structures, which do not have a strict sequential order.

(content:references:lda-arrays)=
### Arrays
An array is a data structure that stores a collection of items of the same type in a contiguous block of memory. The items in an array are accessed using an index, an integer representing the item’s position in the collection.

In most programming languages, arrays are zero-indexed, which means that the first element in the array has an index of 0, the second element has an index of 1, and so on. 

Arrays can store large amounts of data because they allow fast access to any element using the index. However, inserting or deleting elements from the middle of an array can be slow, as it requires shifting the elements to make room for the new element or closing the gap left by the deleted element.

There are also different types of arrays, such as one-dimensional arrays, which store a linear sequence of elements, and multi-dimensional arrays, which are arrays of arrays and can be used to store data in a more complex structure.

(content:references:lda-linked-lists)=
### Linked lists
A linked list is a linear data structure where each element, called a node, is a separate object that stores a data value and a reference (link) to the next node in the list. Linked lists are used to store data sequences, and they are a common alternative to arrays because they can be resized easily; llements can be inserted or removed without moving the other elements.

There are two main types of linked lists: singly linked lists and doubly linked lists. In a singly linked list, each node has a reference to the next node in the list but not the previous one. On the other hand, each node connects to the next and previous nodes in a doubly linked list.

Linked lists can be used to implement various data structures, such as stacks, queues, and associative arrays. They are often used when the data structure size is not known in advance or when the data needs to be inserted or removed frequently, as the time complexity for these operations is O(1) for a linked list.

(content:references:lda-stacks)=
### Stacks
A `stack` is a linear data structure that follows the "last-in, first-out" (LIFO) principle. Thus, the last element added to the `stack` will be the first one to be removed.

Stacks are often used to store data temporarily while a program is executing, and they are a common way to implement function calls in many programming languages. When a function is called, the arguments and local variables are stored on a `stack` (referred to as the `call stack`) until the function returns.

In a `stack`, elements can be added to the top (also known as `pushing`) and removed from the top (also known as `popping`). This means that new elements are continually added to the top of the stack, and elements are always drawn from the top.

Here is an example of an integer `stack` in [Julia](https://docs.julialang.org) implemented by the [DataStructures.jl](https://github.com/JuliaCollections/DataStructures.jl) package:

```julia
using DataStructures # import the DataStructures package

# create an empty Stack that holds Int64 values
s = Stack{Int64}()

# add some values to the Stack using the push! operation
push!(s, 1);
push!(s, 2);
push!(s, 3);

# Grab a value from the Stack using the pop! operation
x = pop!(s);

# print x
println("What value did we get from the Stack = $(x)")

```

In this example, the elements 1, 2, and 3 are added to the `stack` in that order using the `push!` function. The call to the `pop!` function removes the top element in the `stack`, i.e., the value 3 (the last item added); `pop!` removes elements from the `stack` in the _opposite order_ they were added. 

(content:references:lda-queues)=
### Queues
A `queue` is a linear data structure that follows the "first-in, first-out" (FIFO) principle. This means that the first element added to the `queue` will be the first one to be removed.

Queues are often used to store data that needs to be processed in a specific order or to store data that is being transferred from one place to another. For example, a printer `queue` might store print jobs that need to be printed in the order they were received, or a task `queue` might store tasks that need to be completed by a group of workers.

In a `queue`, elements are added to the end (also known as _enqueuing_) and removed from the front of the queue (also known as _dequeuing_). This means that new elements are continually added to the back of the queue, and removed elements are always taken from the front.

Here is an example of an integer `queue` in [Julia](https://docs.julialang.org) implemented by the [DataStructures.jl](https://github.com/JuliaCollections/DataStructures.jl) package:

```julia
using DataStructures # import the DataStructures package

# create an empty Queue that holds Int64 values
q = Queue{Int64}()

# add some values to the Queue using the enqueue! operation
enqueue!(q, 1);
enqueue!(q, 2);
enqueue!(q, 3);

# Grab a value from the Queue using the dequeue! operation
x = dequeue!(q);

# print x
println("What value did we get from the Queue = $(x)")

```

In this example, the elements 1, 2, and 3 are added to the `queue` in that order using the `enqueue!` function. The call to the `dequeue!` function removes the top element in the `queue`, i.e., the value 1 (the first item added); `dequeue!` removes elements from the `queue` in the order they were added. 

## Non-linear data structures
Non-linear data structures do not store data in a linear sequence. In other words, the data is not linearly organized in a list where each element is connected to the next one. Instead, non-linear data structures store and manage data in more complex ways, allowing for faster access and manipulation of the data.

Non-linear data structures can be more efficient than linear data structures, such as arrays and linked lists, for certain types of operations. However, they are also (typically) more complex and require more memory to store the data.

Let's consider a few handy non-linear data structures, {ref}`content:references:data-structure-hashmap-and-sets`, {ref}`content:references:data-structure-tree`, and {ref}`content:references:data-structure-graphs`. 

(content:references:data-structure-hashmap-and-sets)=
### Hashmaps and sets
A hashmap is a data structure that stores key-value pairs, where each key value is unique. Hashmaps uses a [hash function](https://en.wikipedia.org/wiki/Hash_function) to map keys to array indexes, thus, allowing for efficient lookups and insertions. Sets are similar to hashmaps but only store keys and do not have values associated with them.

#### Hashmaps
Hashmaps are implemented in both [Julia](https://docs.julialang.org) and [Python](https://www.python.org) as a `dictionary` type; we've already seen several examples of [Julia](https://docs.julialang.org) dictionaries, which are type `Dict`, when working with [JSON, TOML and YAML files](../unit-1-basics/data-file-io.md). [Python](https://www.python.org) dictionaries behave in a similar way as thier [Julia](https://docs.julialang.org) counterparts, with one important exception: in [Python > 3.7](https://www.python.org) dictionaries are _ordered_, while [Julia](https://docs.julialang.org) dictionaries are always _unordered_. 

Dictionaries are included in the standard libraries of Julia and Python; thus, they are available without importing external modules. Let's look at an example of how to use a dictionary in [Julia](https://docs.julialang.org) and [Python](https://www.python.org):

`````{tab-set}
````{tab-item} julia
```julia

# Initialize and empty dictionary, keys are strings, values can be anything
car = Dict{String,Any}()

# load key=value pairs into the dictionary
car["brand"] = "Ford"
car["model"] = "Mustang"
car["year"] = 1964
```
````

````{tab-item} python
```python
# Initialize and empty dictionary
car = {}

# Add data to car dictionary
car["brand"] = "Ford"
car["model"] = "Mustang"
car["year"] = 1964
```
````
`````

Data can be accessed by passing the key value into the dictionary, e.g., `car["brand"]` would return `Ford`.

#### Sets
Like dictionaries, both [Julia](https://docs.julialang.org) and [Python](https://www.python.org) implement a `Set` type in their respective standard libraries. The `Set` type holds a unique collection of keys but does not associate these keys with any data:

`````{tab-set}
````{tab-item} julia
```julia

# Initialize and empty set that stores strings
fruit = Set{String}()

# add items to a set using push!
push!(fruit, "Apple")
push!(fruit, "Pear")
push!(fruit, "Mango")
```
````

````{tab-item} python
```python
# Initialize an empty set
fruit = set()

# Add data to fruit set
fruit.add("Apple")
fruit.add("Pear")
fruit.add("Mango")
```
````
`````

In both [Julia](https://docs.julialang.org) and [Python](https://www.python.org), the `Set` type is _unordered_; thus, unlike Arrays, Stacks or Queues, the order in which items are added to the set is not maintained. 

##### Hash functions
The exciting thing about a hashmap implementation, e.g., the `Dict` type in [Julia](https://docs.julialang.org), is the fast lookup enabled by mapping an arbitrary key type to an array index. This mapping is enabled using a [hash function](https://en.wikipedia.org/wiki/Hash_function). 

A hash function takes an input value, e.g., a key value, and returns a fixed-size string of characters or a number. The same information will always produce the same output, but even a small change to the input will have a very different result. In the case of a hashmap, when a key is passed to the hash function, it calculates a hash value, which is then used as the index at which the corresponding data value is stored in an array.

Let's look at some psuedo code for a simple hash function ():

````{prf:algorithm} Simple Hash Function
:label: algo-hash-function-code
:class: dropdown

**Inputs** String `key`, a `size` parameter, a $\beta$ factor

**Outputs** hash_value 

**Initialize**
1. set hash $\leftarrow{0}$
1. set $L\leftarrow\text{length}(\text{key})$

**Main**
1. for $i\in{0,\dots,L-1}$
      1. hash $\leftarrow\left(\text{hash}\times\beta+\text{key}[i]\right)$ % `size`

**Return** hash
````

If $\beta=31$, then {prf:ref}`algo-hash-function-code` is a variant of the Horner's hashing method. The modulo `%` operation with `size` ensures that the output `hash` value is a valid index within the array.

(content:references:data-structure-tree)=
### Trees
Trees are widely-used non-linear data structures that simulate a hierarchical tree structure, with nodes representing the hierarchy. In a tree, each node has one or more child nodes, each child node has one or more sub-children, and so on. The topmost node, which has no parent, is called the root node. The nodes that do not have any children are called leaf nodes. The edges connecting the nodes represent the relationships between the nodes.

Trees are often used to represent hierarchical relationships, such as in file systems, where each folder (node) can contain multiple files and subfolders (child nodes). Trees can also be used to model a decision, where the root node represents a decision, and the child nodes represent the possible outcomes of that decision.

There are several types of trees, including binary trees, which have at most two children per node, and n-ary trees, which have any number of children per node. Trees are commonly used to implement data structures such as binary search trees and heap data structures, which allow for efficient insertion, deletion, and search operations.

#### Revisit: Recursive Fibonacci and Memoization
One tool to diagram how a recursive function works is by develiping a call tree. Previously, we constructed a [recursive implementation of the `Fibonacci` function](../unit-1-basics/functions.md) which computed the [Fibonacci numbers](https://en.wikipedia.org/wiki/Fibonacci_number) numbers. The call tree for recursive `fibonacci(4)` is shown in ({numref}`fig-recursive-fib-4-call-tree`)

```{figure} ./figs/Fig-Fib-4-Recursive-Tree.pdf
---
height: 260px
name: fig-recursive-fib-4-call-tree
---
Schematic of the function call tree for the recursive implementation of the `fibonacci` function with $n=4$. 
```


(content:references:data-structure-graphs)=
### Graphs
A graph is a data structure in computer science that consists of a finite set of vertices (also called nodes) and a set of edges connecting these vertices. The edges can be directed (also called arcs) or undirected.

In a directed graph, the edges have a direction and connect one vertex to another. The edges are often used to represent relationships or dependencies between the vertices. For example, in a social network, the vertices might represent people, and the directed edges might represent friendships, with the edge pointing from the person to their friend. In an undirected graph, the edges have no direction and connect pairs of vertices. These edges represent connections or relationships between symmetrical vertices, such as friendships.

Graphs represent real-world situations like networks, maps, and social relationships. They are commonly used to describe relationships between data and to solve problems such as finding the shortest path between two nodes or determining whether a graph is connected

---

## Summary 
Fill me in. 