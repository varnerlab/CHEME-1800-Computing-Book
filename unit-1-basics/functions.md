# Functions, Iteration and Control Statements

## Introduction 
Fill me in.

---

## Functions

### Mathematical functions
Most of us are familiar with the idea of a function from mathematics ({numref}`fig-mathematical-function`). A mathematical function takes input from a domain (the set of possible inputs) and converts this input to an output that lives in the codomain. The codomain is the set of all possible output values, while the range (which is a subset of the codomain) is the set of values we actually observe. 

```{figure} ./figs/Fig-Mathematical-Function.pdf
---
height: 240px
name: fig-mathematical-function
---
Schematic of a function from mathematics. 
```

Formally a mathematical function is defined as ({prf:ref}`defn-mathematical-function`):

````{prf:definition} Mathematical function
:label: defn-mathematical-function

In mathematics, a function from a set $X$ to a set $Y$ assigns to each element of $X$ exactly one element of $Y$. The set $X$ is the domain of the function while the set $Y$ is called the codomain of the function. A function, its domain, and its codomain, are declared using the notation:

```{math}
f:X\rightarrow{Y}
```

where the value of a function $f$ at an element $x\in{X}$, denoted by $f(x)$, is called the image of $x$ under $f$, or the value of $f$ applied to the argument $x$.
````

The function $f$ is the rule that describes how the inputs are transformed into results, while $y = f(x)$ denotes the output value in the range of the function $f$.


### Computer functions
Functions on the computer share many of the features of mathematical functions, but there are a few crucial differences. For example, on the computer, a function is an object that maps a tuple of arguments to a tuple of return values. However, unlike mathematical functions, computer functions in languages such as [Julia](https://docs.julialang.org), [Python](https://www.python.org), or the [C-programming language](https://en.wikipedia.org/wiki/C_(programming_language)) can alter and be affected by the global state of your program.

The basic syntax for defining functions in [Julia](https://docs.julialang.org), [Python](https://www.python.org) or the [C-programming language](https://en.wikipedia.org/wiki/C_(programming_language)) involves defining a function name, the set of input arguments, the return value types, and finally, the logic required to transform the input arguments to the return values. 

Let's compare and contrast the structure and syntax of a simple function named `linear` which computes the value $y = mx+b$ written in [Julia](https://docs.julialang.org), [Python](https://www.python.org) and the [C-programming language](https://en.wikipedia.org/wiki/C_(programming_language)):


`````{tab-set}
````{tab-item} julia
```julia
function linear(x::Number, m::Number, b::Number)::Number
    
    # linear transform the x-value
    y = m*x+b

    # return y 
    return y
end
```
````

````{tab-item} python
```python
def linear(x,m,b):

    # linear transform the x-value
    y = m*x+b

    # return y 
    return y
```
````

````{tab-item} C
```c
double linear(double x, double m, double b) {

    /* define and initialize y */
    double y = 0.0;
    
    /* Linear transform the x-value to y */
    y = m*x + b;

    /* return y */
    return y;
}
```
````
`````

The different implementations of the `linear` function share common features:
* First, each implementation has a `return y` statement at the end of the function body. A `return` statement tells the program (which is calling your function) that the work being done in the function is finished, and here is the finished value.
* Next, the implementation of $y = mx+b$ looks the same in each language; there is a conserved set of operations, e.g., the addition `+` and multiplication `*` operators defined in each language which are similar across many languages. 

However, many structural and syntactic features are different between the languages:
* Functions in [Julia](https://docs.julialang.org) start with the function keyword and stop with an enclosing end keyword. Functions in the [C-programming language](https://en.wikipedia.org/wiki/C_(programming_language)) begin with the return type, and the function body is enclosed in braces `{...}`. Functions in [Python](https://www.python.org) start with the `def` keyword followed by the name, args, and a colon, while the return statement marks the end of a [Python](https://www.python.org) function; there is no other closing keyword or character.
* Functions in the [C-programming language](https://en.wikipedia.org/wiki/C_(programming_language)) _strictly require_ type information about the arguments, but neither [Julia](https://docs.julialang.org) or [Python](https://www.python.org) requires this information; it is recommended in [Julia](https://docs.julialang.org) but not required, and while newer [Python](https://www.python.org) versions have support for typing, the [Python](https://www.python.org) runtime does not enforce function and variable type annotations.
* Finally, the [C-programming language](https://en.wikipedia.org/wiki/C_(programming_language)) requires that variables be defined before they are used, while both [Julia](https://docs.julialang.org) and [Python](https://www.python.org) do not require this step.

## Iteration
One of the most common programming tasks you will do is iterating over a list of items. For example, suppose you wanted to see all the items on a To Do list for a day. 

## Control
Fill me in.

---

## Summary
Fill me in. 