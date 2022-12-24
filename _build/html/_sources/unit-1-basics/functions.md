# Functions, Control Statements, and Recursion

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

## Control statements

### If-else conditional statements
A common programming task is to check whether a condition is `true` or `false` and then execute tasks depending on this condition; this is a called conditional evaluation. Conditional evaluation, which is a important structure in almost all programming languages, is encoded with `if-else` statements.

Conditional evaluation allows portions of code to be evaluated or not evaluated depending on the value of a boolean expression. Let's consider the anatomy of the `if-elseif-else` conditional syntax in [Julia](https://docs.julialang.org), [Python](https://www.python.org) and the [C-programming language](https://en.wikipedia.org/wiki/C_(programming_language)):

`````{tab-set}
````{tab-item} julia
```julia
if condition_1
    # code statememts executed if condition_1 == true
elseif condition_2
    # code statememts executed if condition_2 == true
else
    # code statememts executed if condition_1 == false AND condition_2 == false
end
```
````

````{tab-item} python
```python
if condition_1:
    # code statememts executed if condition_1 == true
elif condition_2:
    # code statememts executed if condition_2 == true
else: 
    # code statememts executed if condition_1 == false AND condition_2 == false
````

````{tab-item} C
```c
if (condition_1) {
  /* block of code to be executed if condition_1 is true */
} else if (condition_2) {
  /* block of code to be executed if condition_2 is true */
} else {
  /* code statememts executed if condition_1 == false AND condition_2 == false */
}
```
````
`````

### Iteration
Another common programming tasks you'll encounter is iterating over a list of items, and perhaps performning a task using each item. For example, finding the sum of a list experimental values so that you can estimate a mean value, or translating words in an article from language to another, etc. Consider two ways to iterate over a collection of items, a `for-loop` and a `while-loop`. 

#### For-loops
[For-loops](https://en.wikipedia.org/wiki/For_loop) execute a block of code a fixed number of times. 
[For-loops](https://en.wikipedia.org/wiki/For_loop) have a long history in computing dating back to the late 1950s. For-loops are key language constructs in all modern programming languages. For-loops have two parts: a header and a body. 

The header of a `for-loop` defines the iteration count while body holds the code that is executed once per iteration. The header of a `for-loop` typically declares an explicit loop counter or loop variable which tells the body which iteration is being executed. Thus, `for-loops` are used when the number of iterations is known before entering the loop. 

Let's look at the structure and syntax of a `for-loop` in [Julia](https://docs.julialang.org), [Python](https://www.python.org) and the [C-programming language](https://en.wikipedia.org/wiki/C_(programming_language)) where we are iterating over a fixed range of values:

`````{tab-set}
````{tab-item} julia
```julia
for i in 1:10 # gives i values from 1 to 10 inclusive
    # loop body: holds statements to be executed at each iteration
end
```
````

````{tab-item} python
```python
for i in range(1, 10):  # gives i values from 1 to 9 inclusive (but not 10)
    # loop body: holds statements to be executed at each iteration
````

````{tab-item} C
```c
/* Using for-loop iteration range 0 -> 9 */
int sum = 0;
for (int i = 0; i < 10; i++) {
    /* body: holds statements to be executed at each iteration */
}
```
````
`````

Let's illustrate the `for-loop` and functions by developing a function that iteratively computes [Fibonacci numbers](https://en.wikipedia.org/wiki/Fibonacci_number) ({prf:ref}`example-iteration-fibonacci-numbers`):

````{prf:example} Compute the Fibonacci sequence
:class: dropdown
:label: example-iteration-fibonacci-numbers

Develop a `fibonacci` function which takes an integer $n$ as an argument and returns the Fibonacci sequence.

__Solution__: The Fibonacci numbers, denoted as $F_{n}$, form a sequence, the Fibonacci sequence, in which each number is the sum of the previous two numbers:

```{math}
F_{n} = F_{n-1}+F_{n-2}\qquad{n\geq{2}}
```

where $F_{0} = 0$ and $F_{1} = 1$. A [Julia](https://docs.julialang.org) implementation of the `fibonacci` function is given by:

```julia
function fibonacci(n::Int64)::Array{Int64,1}

    # check: is n legit? n>=1
    # ...

    # initialize -
    sequence = zeros(n+1); # why n+1 (and not n)?

    # we know the first two elements -
    sequence[1] = 0;
    sequence[2] = 1;

    # main loop -
    for i in 3:(n+1)
        sequence[i] = sequence[i-1] + sequence[i-2]
    end

    # return -
    return sequence
end 
```
````



#### While-loops
A [while-loop](https://en.wikipedia.org/wiki/While_loop) is a control structure that allows a program to repeat a block of code in the loop’s body as long as a particular condition, which is encoded in the loop's header, is true.

The header of a `while-loop` evaluates a Boolean control expression which determines if the body is executed or the loop terminates; if the control expression evaluates to `true`, the code in the body is executed; otherwise, the loop terminates. Thus, unlike a `for-loop` that iterates over a fixed (specfied) range of values, a `while-loop` can repeat both for a set number of iterations or indefinitely, depending upon the result of the evaluation of the control condition in the header of the loop. 

Let's look at the structure and syntax of a `while-loop` in [Julia](https://docs.julialang.org), [Python](https://www.python.org) and the [C-programming language](https://en.wikipedia.org/wiki/C_(programming_language)) where we are iterating over a fixed range of values:


`````{tab-set}
````{tab-item} julia
```julia

# initialize 
counter = 5;   # counter variable
factorial = 1  # initial value

# while loop -
while (counter > 0)

    # compute 
    factorial *= counter # short hand for: factorial = counter*factorial

    # update the counter -
    counter -= 1 # short hand for: counter = counter - 1
end

```
````

````{tab-item} python
```python
# initialize
counter = 5                           # Set the value to 5 
factorial = 1                         # Set the value to 1

# while loop
while counter > 0:                    # While counter(5) is greater than 0  
    factorial *= counter              # Set new value of factorial to counter.
    counter -= 1                      # Set the counter to counter - 1.

````

````{tab-item} C
```c
/* declare and initialize */
int count = 5;
int factorial = 1;

/* while loop */
while (count > 1) {

    /* compute */
    factorial *= count;

    /* update the counter */
    counter--;
}
```
````
`````


## Recursion
Recursion is a programming technique in which a function calls itself with a modified version of its own input. This allows the function to repeat a process on a smaller scale, and the results of these smaller-scale processes can be combined to solve the original problem.

The body of a recursive function typically has two parts:

* __Base case__: This is the condition that stops the recursion. When the function reaches the base case, it returns a result without calling itself again. This is necessary to prevent the function from entering an infinite loop.
* __Recursive case__: This is the part of the function that calls itself with a modified version of its input. The recursive case is responsible for breaking the problem down into smaller subproblems and solving them using the function itself.

Let's consider an example where we compute $n!$ using recursion in [Julia](https://docs.julialang.org):

```julia

function factorial(n::Int64)::Int64

    # check: is n legit? if n < 0 through an error
    # ...

    if (n == 0 || n == 1) # base case -
        return 1;
    else # recursive case
        return n*factorial(n-1); 
    end
end

```
When the `factorial` function is called, it checks if the input $n$ equals 0 or 1 (the base case). If yes, the function returns 1 without calling itself again. However, if $n\neq{0}$ or $n\neq{1}$, the process enters the recursive case; the function calls itself with an input of $n-1$ and returns the result of $n$ multiplied by the result of the recursive call. This process continues until the base case is reached, the recursion stops, and the final result is returned.

---

## Summary
Fill me in. 