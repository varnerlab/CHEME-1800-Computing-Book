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

### Linear data structures
* {ref}`content:references:lda-arrays`: An array is a collection of items stored at contiguous memory locations. The items can be of any data type, and each item has a unique index that can be used to access it. 
* {ref}`content:references:lda-stacks` and {ref}`content:references:lda-queues` are array like data structues that control how elements are added and accessed. {ref}`content:references:lda-stacks` follows the 
[last-in, first-out (LIFO) principle](https://en.wikipedia.org/wiki/Stack_(abstract_data_type)). This means that the last element added to the stack will be the first one to be removed. Stacks are often used to store data temporarily while a program is executing. {ref}`content:references:lda-queues` follow the [first-in, first-out (FIFO) principle](https://en.wikipedia.org/wiki/FIFO_(computing_and_electronics)), the first element added to the queue will be the first one to be removed. Queues are often used to store data that needs to be processed in a specific order or to store data that is being transferred from one place to another.
* {ref}`content:references:lda-linked-lists`: A linked list is a linear data structure in which each element is a separate object, connected to the next element by a pointer; linked lists store sequences of items, which can be expanded or contracted as needed.

### Non-linear data structures
* {ref}`content:references:data-structure-hashmap-and-sets`: A hashmap is a data structure that stores key-value pairs, where each key value is unique. Sets are similar to hashmaps but only store keys and do not have values associated with them.
* {ref}`content:references:data-structure-tree`: A tree is a hierarchical data structure in which each node has one or more child nodes. Trees are used to store data that has a natural hierarchical structure, such as the structure of a file system or the organization of a company.
* {ref}`content:references:data-structure-graphs`: A graph is a data structure that consists of a set of vertices (also called nodes) and a set of edges connecting the vertices. Graphs are used to represent relationships between objects, and they are often used to model networks.

---

## Linear data structures
Linear data structures store elements linearly, meaning that they are arranged in a sequence one after the other. This contrasts with non-linear data structures, which do not have a strict sequential order.

(content:references:lda-arrays)=
### Arrays
An array is a data structure that stores a collection of items of the same type in a _contiguous_ block of memory ({numref}`fig-array-schematic`). The name of an array, e.g., $a$ points to the first element, then each subsequent element in the array adds `+1` units to the memory address of the previous element.   

```{figure} ./figs/Fig-Array-Schematic.pdf
---
height: 90px
name: fig-array-schematic
---
Schematic of a 1-based array six element array with name a.
```

The items in an array are accessed using an index, an integer representing the item’s position in the collection. In most programming languages, arrays are zero-indexed, i.e., the first element in the array has an index of `0`, the second element has an index of `1`, and so on. However, arrays in [Julia](https://julialang.org) are one-based, i.e., the first element has an index `1`, the second element has an index `2`, etc. 

Arrays can store large amounts of data because they allow fast access to any element using the index. Elements of an array are typically indexed using `bracket notation` in most languages such as [Julia](https://julialang.org) or [Python](https://www.python.org):

```{code-cell} julia
# Define an array -
a = [2,4,8,16,32,64]

# What is the 3rd element of the array a?
i = 3
println("The third element of a = $(a[i])")
```

However, array access uses parenthesis in niche systems, such as [Matlab/Octave](https://www.mathworks.com/products/matlab.html). 

Accessing individual elements or contiguous ranges of elements of an array is fast, but inserting or deleting elements from the middle of an array can be slow, as it requires shifting the elements to make room for the new element or closing the gap left by the deleted element.

#### Types of arrays: Vectors, Matrices and d-dimensional arrays
There are different sizes (types) of arrays, such as one-dimensional arrays (vectors), which store a linear sequence of elements, and multi-dimensional arrays, e.g., two-dimensional arrays (matricies) or arrays of arrays that store data in more complex structures.

In [Julia](https://julialang.org), the size of an array and the type of data that will be stored in the array are declared when you initialize the array. For example, a 1-dimensional array of indefinite length that holds integers is declared as: 

```{code-cell} julia
# Define an array arr
arr = Array{Int64,1}() # the array is empty

# When an array has undefined length, we use the push! operation to add values
push!(arr,1); # adds a 1 to index 1
push!(arr,2); # adds a 2 to index 2
push!(arr,4); # adds a 4 to index 3

# print elements of the array -
println("Elements of arr = $(arr)")
```

Of course, if you know how many elements you need the array to store beforehand, you can declare that when the array is constructed:

```{code-cell} julia
# Define an array arr
arr = Array{Int64,1}(undef, 10) # the array has 10 elements, each initialized to undefined

# When an array has defined length, we use the = operation to add values
for i in 1:10
  arr[i] = 2^i # fills the array with powers of 2
end

# print elements of the array -
println("Elements of arr = $(arr)")
```

Two-dimensional arrays follow a similar pattern when we declare the array; however, there is no direct equivalent to the `push!` operation for two- (or higher) dimensional arrays. Thus, when dealing with multi-dimensional arrays, we typically need to know the size of each dimension when we declare the array:

```{code-cell} julia
# Define an 2-dimensional array A
A = Array{Int64, 2}(undef, 4, 4) # array of Int's has 4 rows, 4 cols, initialized to undefined

# When an array has defined length, we use the = operation to add values
# This creates an upper-triangular matrix with powers 2
for i in 1:4
  for j in 1:4
    if (i<=j)
      A[i,j] = 2^j # fills the array with powers of 2 along the col index
    else
      A[i,j] = 0 # adds a zero
    end
  end
end

# print elements of the array
A
```

#### Other array like data structures: Stacks and Queues

(content:references:lda-stacks)=
##### Stacks
A `stack` data structure follows the [last-in, first-out (LIFO) principle](https://en.wikipedia.org/wiki/Stack_(abstract_data_type)), the last element added to a `stack` will be the first element removed ({numref}`fig-stack-schematic`). Unlike an array, you cannot access the elements of a stack by thier index. Instead, elements are added to the top of the stack (also known as `pushing`) and removed from the top of the stack (also known as `popping`). 


```{figure} ./figs/Fig-Stack-Schematic.pdf
---
height: 200px
name: fig-stack-schematic
---
Schematic of the `push!` and `pop!` operations for a `stack s`. Stacks follow the last-in, first-out paradigm. Thus, new elements are continually added to the top of the stack, while elements are drawn from the top (depicted with the dashed arrow).
```
Stacks in [Julia](https://docs.julialang.org) are implemented in the [DataStructures.jl](https://github.com/JuliaCollections/DataStructures.jl) package:

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

In this example, the elements `1`, `2`, and `3` are added to the `stack s` using the `push!` operation. The `pop!` operation removes the top element of the `stack`, i.e., the value 3 (the last item added); thus, `pop!` removes elements from the `stack` in the _opposite order_ they were added. 

###### Common uses of Stacks:
* __Function call stack__: In programming languages, the function [call stack](https://en.wikipedia.org/wiki/Call_stack) is typically implemented as a stack data structure. Each time a function is called, its information (such as local variables, parameters, and return address) is pushed onto the stack. When the function returns, its information is popped off the stack, allowing the program to resume execution from where it left off.
* __Undo/redo functionality__: Many applications that allow users to undo and redo actions use a stack data structure to keep track of the actions. Each time a user acts, such as typing a letter or moving an object, the action is added to the stack. The most recent activity is popped off the stack and undone when the user chooses to undo an action. Similarly, the redo functionality pushes undone actions back onto the stack.
* __Expression evaluation__: Stack data structures can also be used to evaluate mathematical expressions, such as those in postfix notation. Operands are pushed onto the stack in this case, and operators are applied to the most recently pushed operands. The result is then pushed back onto the stack until the entire expression has been evaluated.
 
(content:references:lda-queues)=
##### Queues
The `queue` data structure follows the [first-in, first-out (FIFO) principle](https://en.wikipedia.org/wiki/FIFO_(computing_and_electronics)), the first element added to the `queue` will be the first element to be removed (Fig. {numref}`fig-queue-schematic`). In a `queue`, elements are added to the bottom (also known as _enqueuing_) and removed from the top (also known as _dequeuing_). 

```{figure} ./figs/Fig-Queue-Schematic.pdf
---
height: 200px
name: fig-queue-schematic
---
Schematic of the `enqueue!` and `dequeue!` operations for a `queue q`. Quues follow the first-in, first-out paradigm. Thus, new elements are continually added to the bottom of the queue, while elements are drawn from the top (depicted with the dashed arrow).
```

Queues in [Julia](https://docs.julialang.org) are implemented by the [DataStructures.jl](https://github.com/JuliaCollections/DataStructures.jl) package:

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

In this example, the elements `1`, `2`, and `3` are added to the `queue q` in that order using the `enqueue!` operation. The `dequeue!` operation removes the top element from the `queue`, i.e., the value `1` (the first item added); `dequeue!` removes elements from the `queue` in the order they were added. 

###### Common uses of Queues
* __Job scheduling__: Queues are often used in operating systems to manage tasks, such as running programs or printing documents. Each task is placed in a queue, and the operating system schedules tasks in the order they were added to the queue, allowing tasks to be processed in the order they were received.
* __Breadth-first search__: In graph theory, the breadth-first search algorithm explores a graph by visiting all the vertices at a given level before moving on to the next level. This can be implemented using a queue data structure, where the vertices at each level are added to the queue in order and processed in that exact order.
* __Message passing__: Queues are often used to pass messages between different parts of a system, such as between a producer and a consumer in a messaging system. Messages are added to the back of the queue by the producer and consumed from the front of the queue by the consumer. This allows the producer and consumer to operate at different speeds without the risk of messages being lost or overwritten.


<!-- 
Queues are often used to store data that needs to be processed in a specific order or to store data that is being transferred from one place to another. For example, a printer `queue` might store print jobs that need to be printed in the order they were received, or a task `queue` might store tasks that need to be completed by a group of workers. -->

(content:references:lda-linked-lists)=
### Linked lists
Linked lists are a common alternative to arrays because they can be resized easily; ndoes can be inserted or removed without moving the other nodes.
A linked list is a linear data structure where each element, called a node, is a separate object that stores a data value and a reference (link) to the next node in the list ({numref}`fig-LinkedList-schematic`). 


```{figure} ./figs/Fig-LinkedList-Schematic.pdf
---
height: 180px
name: fig-LinkedList-schematic
---
Schematic of a singly linked list. Each node in the list has data, in this case a queue data structure, and a reference (link) to the next node in the list. 
```

There are two main types of linked lists: singly linked lists and doubly linked lists. In a singly linked list, each node has a reference to the next node in the list but not the previous one. On the other hand, each node connects to the next and previous nodes in a doubly linked list.

#### Common uses of linked lists
Linked lists are often used when the data structure size is not known in advance or when the data needs to be inserted or removed frequently, as the time complexity for these operations is constant for a linked list.

* __Dynamic data structures__: Linked lists are helpful when working with data structures that can change in size during runtime. Unlike arrays, linked lists can grow or shrink as needed without complex memory management or copying operations. This makes them helpful in implementing stacks, queues, and other dynamic data structures.
* __Memory allocation__: In computer memory management, linked lists are used to keep track of free and allocated memory blocks. Each memory block is represented as a node in the list, with a pointer to the next block. This allows the system to find a suitable block for a new allocation quickly and to free up blocks when they are no longer needed efficiently.
* __Modeling hierarchical data__: Linked lists can represent hierarchical data structures, such as trees and graphs. Each node in the list represents a tree node or graph vertex and contains a pointer to its children or neighbors. This allows the data to be stored and manipulated flexibly and efficiently while maintaining the hierarchical structure.

## Non-linear data structures
Non-linear data structures do not store data in a linear sequence. Instead, non-linear data structures store and manage data in more complex ways, allowing for faster access and manipulation of data, especially data with hierarchical relationships. However, while non-linear data structures can be more efficient than linear data structures, such as arrays and linked lists, for certain types of operations, they are typically more complex to construct and require more memory.

<!-- Let's consider a few handy non-linear data structures, {ref}`content:references:data-structure-hashmap-and-sets`, {ref}`content:references:data-structure-tree`, and {ref}`content:references:data-structure-graphs`.  -->

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
The exciting thing about a hashmap implementation, e.g., the `Dict` type in [Julia](https://docs.julialang.org), is the fast lookup enabled by mapping an arbitrary key to an array index. This mapping is enabled using a [hash function](https://en.wikipedia.org/wiki/Hash_function). 

A [hash function](https://en.wikipedia.org/wiki/Hash_function) takes an input value, e.g., a key, and returns a fixed-size string or number. The same information will always produce the same output, but even a small change to the input will have a very different result. In the case of a hashmap, when a key is passed to the hash function, it calculates a hash value, which is then used as the index at which the corresponding data value is stored in an array.

Let's look at some psuedo code for a simple hash function ({prf:ref}`algo-hash-function-code`):

````{prf:algorithm} Simple Hash Function
:label: algo-hash-function-code
:class: dropdown

**Inputs** String `key`, a `size` parameter, a $\beta$ factor

**Outputs** hash

**Initialize**
1. set hash $\leftarrow{0}$
1. set $L\leftarrow\text{length}(\text{key})$

**Main**
1. for $i\in{1,\dots,L}$
      1. hash $\leftarrow\left(\text{hash}\times\beta+\text{key}[i]\right)$ % `size`

**Return** hash
````

{prf:ref}`algo-hash-function-code` is a variant of Horner's hashing method when $\beta$ = 31. The [modulo operator](https://en.wikipedia.org/wiki/Modulo_operation) `%` computes the remainder, e.g., the expression `7 % 3` evaluates to `1`. The modulo `%` operation with `size` ensures that the output `hash` value is a valid index within the array. 

````{prf:example} Hashing example
:label: example-hashing-example-impl
:class: dropdown

Compute the hash value for the key = `CHEME-1800` for an array of `size` of `1000` and expansion factor $\beta$ = 31. 

__Solution__: We implemented {prf:ref}`algo-hash-function-code` in [Julia](https://docs.julialang.org)

```julia
"""
    myhash(key::String, β::Int64, size::Int64)::Int64

Convert a String `key` to `Int` for an array of type `size`:
"""
function myhash(key::String, β::Int64, size::Int64)::Int64

    # initialize -
    hash = 0
    L = length(key)

    # main loop -
    for i ∈ 1:L
        hash = (hash*β + convert(Int, key[i])) % size
    end

    # return -
    return hash
end
```
````

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