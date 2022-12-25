# Programs and Modules

## Introduction
Fill me in

---

## Programs
A computer program is a set of instructions that tells a computer what to do. It is written in a programming language, such as [Julia](https://docs.julialang.org), [Python](https://www.python.org), and the [C-programming language](https://en.wikipedia.org/wiki/C_(programming_language)), and can be compiled into machine code that a computer can execute.

The structure of a computer program typically includes the following elements:

* Input: This is the data that the program processes. It can come from various sources, such as a user inputting data through a keyboard or the program reading data from a file.
* Output: This is the result of the program's processing. It can be displayed to the user on a screen or written in a file.
* Algorithm: This is the set of steps that the program follows to solve a problem or accomplish a task. The algorithm defines how the input is transformed into the output.
* Control flow: This is the order in which the program executes its instructions. It can include loops and conditional statements, allowing the program to make decisions based on certain conditions.
* Data structures are the structures used to store and organize data within the program. Examples include arrays, lists, and dictionaries
* Functions: These are blocks of code that perform a specific task and can be called from other parts of the program. Functions can take arguments (input) and return a value (output).

Overall, the structure of a computer program is designed to take in data, process it in some way, and produce output. The specific details of the structure will depend on the problem or task the program is trying to solve.

### How do we execute a program?
Fill me in

## Modules
A module is a collection of related functions, variables, and other types of data that can be imported and used in other programs. Modules are used to organize code and make it easier to reuse and maintain.

### Julia
Modules in [Julia](https://docs.julialang.org) are defined using the `module` keyword, followed by the name of the module and a block of code that defines the functions and variables that make up the module. To use a module in a [Julia](https://docs.julialang.org) program, you can use the [using keyword](https://docs.julialang.org/en/v1/base/base/#using) followed by the name of the module. 

Modules in [Julia](https://docs.julialang.org) are delimited syntactically inside `module NameOfModule ... end`, and have the following features:

* Modules are separate namespaces, each introducing a new global scope. This is useful, because it allows the same name to be used for different functions or global variables without conflict, as long as they are in separate modules.
* Modules have facilities for detailed namespace management: each defines a set of names it exports, and can import names from other modules with using and import (we explain these below).
* Modules can be precompiled for faster loading, and contain code for runtime initialization.

Let's look at a [Julia](https://docs.julialang.org) module:

```julia
module MyExampleModule

    # module body: you include other files, define types or functions, etc.
    # export: we make data, e.g., types or function visible using the export keyword

end
```


---

## Summary
Fill me in.