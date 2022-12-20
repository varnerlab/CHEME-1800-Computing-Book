# Types and Expressions 

## Introduction
Fill me in. 

---

## Types

### Numerical and logical types
Integers, floating-point, and logical values are the basic building blocks of arithmetic and computation. Built-in representations of these values, i.e., the structure that the computer understands, are called `numeric primitives.` On the other hand, the models of integers, floating-point numbers, etc., as immediate values that humans understand in code are called numeric literals, e.g., `1` is an integer literal, and `1.0` is a floating-point literal. Modern programming languages such as [Julia](https://docs.julialang.org) and [Python](https://www.python.org) provide a broad range of primitive numeric types. Further, many standard mathematical operations are defined over them. 

At the smallest scale in the computer, information is stored as bits and bytes. A `bit` is the smallest unit of storage on a computer; a `bit` is a `0` or a `1`. However, a `bit` is too tiny for practical computing tasks. Instead, `bits` are grouped into `bytes`; a group of 8 bits equals  1 $\times$ `byte`.  

The in-memory representation of integers, floating-point, and logical values is referred to as `numeric primitives`; numeric primitives, which the computer understands, are binary numbers (numbers written to the `base 2`). However, there are also other interesting number systems that you may see, e.g., numbers written in the `base 8` (octal) or `base 16` (hexadecimal) system:

````{prf:definition} Base $b$ numbers
:label: defn-number-system

The base $b$ representation of a number is a way of writing numbers using the digits:

```{math}
\left\{0, 1, \dots, (b − 1)\right\}
```

For any $n\geq{0}$ and $b\geq{2}$, there is a string of digits $\left(a_{k}a_{k-1},\dots,a_{2}a_{1}a_{0}\right)_{b}$ such that:

```{math}
:label: eqn-base-b-number
n = \sum_{j=0}^{k-1}a_{j}b^{j}
```

The quantity $k$ denotes the number of bits; $k$ depends upon the computing hardware and the type of data being represented.
````


#### Integers
Integers, represented by the set $\mathbb{Z}$, are the positive and negative natural numbers along with zero, e.g., ($\dots$, -3,-2, -1, 0, 1, 2, 3, $\dots$). The in-memory representation of integers, i.e., their `numeric primitive` representation, is typically a 4 $\times$ byte (32-bit) or 8 $\times$ byte (64-bit) binary number.

````{prf:example} 64-bit integer
:label: example-binary-128

Show that the 64-bit representation of the integer $x=1800$ is given by:

```{math}
(0000000000000000000000000000000000000000000000000000011100001000)_{2}
```

__Solution__:
First, this is a binary number, i.e., a number to the base `2`; thus, it is composed of the digit set $\left\{0,1\right\}$. Next, we know that k = 64-bits; thus, the summation in {prf:ref}`defn-number-system` will run from $0\rightarrow{63}$. However, all the digits are zero _except_ for a few positions; thus, the summation in {prf:ref}`defn-number-system` is given by:

```{math}
1800 = 2^{3}+2^{8}+2^{9}+2^{10}
```

__Tip__: The [bitstring](https://docs.julialang.org/en/v1/base/numbers/#Base.bitstring) functioin in [Julia](https://docs.julialang.org/en/v1/) displays the binary representation of different types of data, e.g., numerical data types as well as strings and characters.
````

##### What about negative integers?
{prf:ref}`defn-number-system` shows the representation of numbers in different bases, e.g., integers written in `base 2` (binary numbers). However, the set of integers $\mathbb{Z}$ also contains negative numbers; how can we represent this type of number in a `base 2` system? When representing integers using a fixed number of bits, negative integers are typically represented using [two's complement](https://en.wikipedia.org/wiki/Two%27s_complement), a mathematical operation to reversibly convert a positive binary number into a negative binary number with equivalent (but negative) value.

[Two's complement](https://en.wikipedia.org/wiki/Two%27s_complement) is executed by first inverting all bits, i.e., flipping `0` to `1` and vice-versa, and next adding a place value of `1` to the inverted number. Let's consider an example:

````{prf:example} Two's complement
:label: example-twos-complement

Develop the 64-bit pattern for $\bar{x}=-8$ using twos complement. 

__Solution__: The 64-bit patterm for $x=8$ is given by:

```{math}
:label: eqn-64-bit-value-positive-8
8 = \left(0000000000000000000000000000000000000000000000000000000000001000\right)_{2}
```

_Step 1_: Flip all the bits:

```{math}
\left(1111111111111111111111111111111111111111111111111111111111110111\right)_{2}
```

_Step 2_: What is going here? Finish me.

````

#### Floating point values
Scalar floating numbers, represented by the set $\mathbb{R}$, are stored using 4$\times$bytes (32-bits) following the [IEEE-754 standard](https://en.wikipedia.org/wiki/IEEE_754).

#### Boolean values
Fill me in.

### Collection types
Fill me in.

### Character and string types
Textual data on a computer is represented as the `String` type. Strings are modeled as a sequence of characters. 

#### Characters
Characters on the computer, e.g., the letter `A` are of type `Char`. Traditionnally, characters were represented via the [American Standard Code for Information Interchange (ASCII)](https://en.wikipedia.org/wiki/ASCII) system, as 1 $\times$ byte (8-bit) integers; thus, there were $0,\dots,255$ possible character values. For example, 
the character `A` is index 65 in the ACSII system. However, all modern computer systems use the [Unicode](https://en.wikipedia.org/wiki/Unicode) standard, which enodes approximately 150,000 characters, where the first 128 of these are the same as the ASCII set; [Unicode](https://en.wikipedia.org/wiki/Unicode) characters typically user 2-bytes per character.


#### Strings
Older languages such as the [C-programming language](https://en.wikipedia.org/wiki/C_(programming_language)) didn't have a `String` type; instead, strings were arrays of characters, i.e., strings were of type `Char[]`. However, modern languages, such as 
[Julia](https://docs.julialang.org) or [Python](https://www.python.org) have much more sophisticated `String` types that incorporate many different types of characters. However, in many ways, they still share features of the older representation of text.

### Custom types
Fill me in

## Expressions
Fill me in.

---

# Summary
Fill me in.