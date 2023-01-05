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

# Programs and Modules

## Introduction
This lecture introduces computer programs and the concept of modules. A program is a set of instructions that a computer follows to perform a specific set of tasks. Program usually consist of one or more modules that are used to solve specific problems or accomplish particular tasks. On the other hand, module are a collection of related functions, variables, and other types of data organized with some intent, e.g., a module that provides types and functions to work with probability; modules organize code and make it easier to reuse and maintain.

Modules can be imported and used in many programs, which makes it easier to reuse code and avoid repeating the same functionality in multiple programs. This helps to make programs more efficient and easier to maintain. For example, imagine you have a program that needs to perform mathematical calculations. Instead of writing all of the necessary code from scratch, you could create a module containing functions for performing the calculations and importing that into your program. This way, you can reuse the code in the module in multiple programs, saving time and making it easier to maintain the code.

---

(content:references:programs-structure)=
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

### Anatomy of an executable program
The structure of programs in modern languages such as [Julia](https://docs.julialang.org) and [Python](https://www.python.org), and older foundational languages such as the [C-programming language](https://en.wikipedia.org/wiki/C_(programming_language)) are remarkably conserved. Let's look at a skeleton of an _executable_ program in various languages and compare and contrast the structure:

`````{tab-set}

````{tab-item} julia
```julia
# 0. copyright/licensing information
# 1. import functions from external modules here
# ...

# 2. include your custom functions here
# ...

# main
function main()
    # 3. do stuff here ...
end

# call main 
main()
```
````

````{tab-item} python
```python
# 0. copyright/licensing information
# 1. import functions from external modules here
# ...

# 2. include your custom functions here
# ...

# main
def main():
    # do stuff here ...

if __name__ == "__main__":
    main()
```
````

````{tab-item} C
```c
/* 0 copyright/licensing */
/* 1 includes */
/* 2 defines */
/* 3 global variable declarations */
/* 4 function prototypes */

/* main */
int main(int argc, char *argv[]) {
    /* 5 command-line parsing */
    /* do stuff ... */
    return 0;
}

/* 6 function declarations */
```
````
`````

Let's develop an _executable_ script in [Julia](https://docs.julialang.org) to compute the factorial $n!$ ({prf:ref}`example-exe-julia-script`):

````{prf:example} Execute a Julia program
:class: dropdown
:label: example-exe-julia-script

Build an example program in [Julia](https://docs.julialang.org) and execute that program; consider the recursive `factorial` function that we developed in the {ref}`content:references:recursion-functions` section:

```julia

# load external modules
# ...

# define the recursive factorial function
function factorial(n)
    
    # check: is n legit? if n < 0 through an error
    # ...

    if (n == 0) # base case -
        return 1;
    else # recursive case
        return n*factorial(n-1); 
    end
end

# Get the number from the user
println("Enter a number:")
n = readline()

# Convert the input to an integer
n = parse(Int64, n)

# Calculate and print the factorial
result = factorial(n)
println("The factorial of ", n, " is ", result)
```

To execute this program in [Julia](https://docs.julialang.org), you can save it to a file called `myfactorial.jl` and then use the [include](https://docs.julialang.org/en/v1/base/base/#Base.include) function from the [Julia REPL](https://docs.julialang.org/en/v1/stdlib/REPL/) to execute it:

```julia
julia> include("myfactorial.jl")
```
````

(content:references:module-structure)=
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

    # module body: include files, define types or functions, etc.
    # export: make data, e.g., types or function visible using the export keyword

end
```


---

## Summary
This lecture introduced computer programs and modules. A program is a set of instructions that a computer follows to perform a specific set of tasks. Program usually consist of one or more modules that are used to solve specific problems or accomplish particular tasks. On the other hand, module are a collection of related functions, variables, and other types of data organized with some intent, e.g., a module that provides types and functions to work with probability; modules organize code and make it easier to reuse and maintain.

In summary, programs are sets of instructions that are used to solve specific problems or accomplish specific tasks while modules are used to organize code and make it easier to reuse.