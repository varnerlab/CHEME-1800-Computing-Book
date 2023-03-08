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

# Vectors, Matrices and Linear Algebraic Equations

## Introduction

This lecture will introduce Vectors, Matrices, and operations defined on these objects. Vectors and matrices are widely used in computer science, engineering, and other fields where mathematical modeling is essential. In chemical engineering, vectors and matrcies are used to represent physical laws such as the conservation of mass, moles or energy. 

Thus, we begin our discussion of vectors, matrcies and their associated operations by quickly reviewing mass and mole balance equations:

* {ref}`content:references:matrix-vector-mass-mol-balances`. Chemical Engineers use balance equations to describe, design, and troubleshoot products and processes. Balance equations describe the amount of _stuff_ (e.g., mass, moles, energy, etc.) in a _system_ that interacts with its _surroundings_. Balance equations can be represented as a system of matrices and vectors.

Next, we transition to defining the structure of matrices and vectors, and thier associated operations:

* {ref}`content:references:matrix-vector` are abstract mathematical objects, typically arrays of numbers, in engineering applications. Vectors (one-dimensional arrays) often describe points in space or encode coefficients of linear equations. On the other hand, matrices (two- or more dimensional objects) typically represent data sets or a grid of numbers. However, matrices also describe linear transformations, such as rotations and scaling operations, and define systems of linear equations. 

Finally, we consider a crucial application area, namely, the formulation and solution of systems of linear algebraic equations:

* {ref}`content:references:soln-laes-start` arise in many different Engineering fields. In Chemical Engineering, these equations naturally arise from steady-state balance equations. We'll introduce two approaches to solving these systems of equations. 

---

(content:references:matrix-vector-mass-mol-balances)=
## Review of balances
Streams of _stuff_ (e.g., material, energy, money, information, etc.) enter (exit) a system from the surroundings; streams cross the system boundary, transporting _stuff_ into or from the system. A system is said to be _open_  if _material stuff_ can enter or exit the system; otherwise, a system is _closed_. For physical systems, streams can be pipes in which material flows into or from the system at some rate. In other cases, streams into (from) a system can carry other types of _stuff_ such as information (e.g., bits) or money (e.g., $\$$, $\def\euro{\unicode{x20AC}} \euro$, bitcoin, etc).

Regardless if a system is open or closed and whatever _stuff_ is, within the system, 
_stuff_ can accumulate and be generated, e.g., via a chemical reaction or by an interest process such as a savings account or treasury bond in the case of money. Whatever the system is, and whatever _stuff_ is, the time behavior of the system can always be described by these _four_ types of terms: inputs, outputs, generation, and accumulation. 

### Open species mass balances
If we are interested in the mass of each chemical species, we can let _stuff_ equal the mass of each of the chemical components; this choice gives the _open species mass balance_ ({prf:ref}`defn-open-species-mass-balance`):

````{prf:definition} Open Species Mass Balance
:label: defn-open-species-mass-balance

Let $m_{i}$ denote the mass of chemical component $i$ in a system consisting of the species set $\mathcal{M}$ (units: mass, e.g., grams g). Further, represent the set of streams flowing into (or from) the system as $\mathcal{S}$. Each stream $s\in\mathcal{S}$ has a direction parameter $\nu_{s}\in\left[-1,1\right]$: If stream $s$ _enters_ the system $\nu_{s} = +1$, however, if stream $s$ _exits_ the system then $\nu_{s} = -1$.

Then, the mass of chemical component $i$ in the system as a function of time (units: g) is described by an _open species mass balance equation_:

```{math}
:label: eqn-species-mass-i-dynamic
\sum_{s\in\mathcal{S}}\nu_{s}\dot{m}_{i,s} + \dot{m}_{G,i} = \frac{dm_{i}}{dt}
\qquad\forall{i}\in\mathcal{M}
```

The quantity $\dot{m}_{i,s}$ denotes the mass flow rate of component $i$ in stream $s$ (units: g $i$/time),
$\dot{m}_{G,i}$ denote the rate of generation of component $i$ in the system 
(units: g $i$/time), and $dm_{i}/dt$ denotes the rate of accumlation of mass of component $i$ in the system (units: mass of $i$ per time).

__Law of mass conservation__:  The [Law of Conservation of Mass](https://en.wikipedia.org/wiki/Conservation_of_mass) says that mass can neither be created nor destroyed, just rearranged or transformed. Thus, if we consume species $j$, the amount of mass-consumed must show up in the other species, i.e., the sum of the individual species mass generation terms must be zero:

```{math}
\sum_{i\in\mathcal{M}}\dot{m}_{G,i} = 0
```

__Steady state__: At steady state, the accumulation term vanishes; thus, the steady state species mass balances are given by:

```{math}
:label: eqn-species-mass-i-steady-state
\sum_{s\in\mathcal{S}}\nu_{s}\dot{m}_{i,s} + \dot{m}_{G,i} = 0
\qquad\forall{i}\in\mathcal{M}
```
````

### Open species mole balances
The principles of mole-based balance equations are similar to their mass-based equivalents, i.e., we can write species mole balances or total mole balances, and the terms play an equivalent role as their mass-based equivalents. However, mass-based units are generally convenient for systems that do not involve chemical reactions, and it is often easier to use mole-based units when chemical reactions occur. 

Before we define the open species mole balance, let's discuss the generation terms that appear in mole balances. Generation terms describe the impact of chemical reactions. For mole based systems we'll use the [open extent of reaction](https://en.wikipedia.org/wiki/Extent_of_reaction) to describe how far a chemical reaction has proceeded toward completion in an open system ({prf:ref}`defn-open-extent-of-rxn`):

````{prf:definition} Open Extent of Reaction
:label: defn-open-extent-of-rxn

Let chemical species set $\mathcal{M}$ participate in the chemical reaction set $\mathcal{R}$. Then, the species generation rate $\dot{n}_{G, i}$ (units: mol per time) can be written in terms of the open extent of reaction $\dot{\epsilon}_{r}$ (units: mol per time):

```{math}
:label: eqn-open-extent-species
\dot{n}_{G,i} = \sum_{r\in\mathcal{R}}\sigma_{ir}\dot{\epsilon}_{r}
```

where $\dot{\epsilon}_{r}$ denotes the open extent of reaction $r$. The quantity $\sigma_{ir}$ denotes the stoichiometric coefficient for species $i$ in reaction $r$:
* If $\sigma_{ir}>0$ then species $i$ is _produced_ by reaction $r$, i.e., species $i$ is a product of reaction $r$ 
* If $\sigma_{ir}=0$ then species $i$ is _not connected to_ reaction $r$
* If $\sigma_{ir}<0$ then species $i$ is _consumed_ by reaction $r$, i.e., species $i$ is a reactant of reaction $r$.

__Connection with kinetic rate__: In a concentration-based system, we often use concentration balances, and the kinetic rate laws, to describe the system. The open extent of reaction $r$ is related to kinetic rate law per unit volume $\hat{r}_{r}$ (units: mol V$^{-1}$ t$^{-1}$):

```{math}
:label: eqn-open-extent-kinetics
\dot{\epsilon}_{r} = \hat{r}_{r}V
```

where $V$ denotes the system volume (units: V). 
````

Now that we understand how to describe the reaction terms, we can write the _open species mole balance_ 
({prf:ref}`defn-open-species-mole-balance`):

````{prf:definition} Open Species Mole Balance
:label: defn-open-species-mole-balance


Let $n_{i}\in\mathcal{M}$ denote the number of moles of chemical species $i$ in a well-mixed system with species set $\mathcal{M}$ which participates in the chemical reaction set $\mathcal{R}$. Further, let $\mathcal{S}$ denote the streams flowing into (or from) the system. Each stream $s\in\mathcal{S}$ has a direction parameter $\nu_{s}\in\left[-1,1\right]$: If stream $s$ _enters_ the system $\nu_{s} = +1$, however, is stream $s$ _exits_ the system then $\nu_{s} = -1$.

Then, the time evolution of $n_{i}\in\mathcal{M}$ is described by an _open species mole balance equation_ of the form:

```{math}
:label: eqn-species-mol-balance
\sum_{s\in\mathcal{S}}\nu_{s}\dot{n}_{i,s} + \sum_{r\in\mathcal{R}}\sigma_{ir}\dot{\epsilon}_{r} = \frac{dn_{i}}{dt}
\qquad\forall{i}\in\mathcal{M}
```

The quantity $\dot{n}_{i,s}$ denotes the mole flow rate of component $i$ in stream $s$ (units: mol per time), the generation terms are written using the open extent (units: mol per time), and $dn_{i}/dt$ denotes the rate of accumulation of the number of moles of component $i$ in the system (units: mol per time). 

__Steady-state__: At steady state, all the accumulation terms vanish and the system of balances becomes:

```{math}
:label: eqn-species-mol-balance-ss
\sum_{s\in\mathcal{S}}\nu_{s}\dot{n}_{i,s} + \sum_{r\in\mathcal{R}}\sigma_{ir}\dot{\epsilon}_{r} = 0
\qquad\forall{i}\in\mathcal{M}
```
````

### Motivation: Matrix-vector notation mole balances
To motivate the study of matrices and vectors, let's rethink the open species mole balances given in 
{prf:ref}`defn-open-species-mole-balance` in terms of a system of equations. Toward this, consider a simple reaction $A\longrightarrow{B}$ occurring in a well-mixed chemical reactor with single input and a single output ({numref}`fig-chemical-reactor-schematic`):


```{figure} ./figs/Fig-Reactor-Schematic.pdf
---
height: 240px
name: fig-chemical-reactor-schematic
---
Schematic of an open well-mixed chemical reactor with a single input and output stream. 
```

The species set in this case is $\mathcal{M}=\left\{A,B\right\}$ and the stream set $\mathcal{S}=\left\{s_{1},s_{2}\right\}$, and there is a single chemical reaction; let $A$ be species `1` and $B$ be species `2`. {prf:ref}`defn-open-species-mole-balance` tells us that we'll have a system of equations of the form (at steady-state):

$$
\begin{eqnarray}
\dot{n}_{A,1} - \dot{n}_{A,2} - \dot{\epsilon}_{1} & = & 0 \\
\dot{n}_{B,1} - \dot{n}_{B,2} + \dot{\epsilon}_{1} & = & 0 \\
\end{eqnarray}
$$

However, this system can be re-written in a much more compact (and general) form:

```{math}
:label: eqn-matrix-vector-system-mol-bal
\mathbf{T}\dot{\mathbf{n}} + \mathbf{S}\dot{\mathbf{\epsilon}} = \mathbf{0}
```

The `boldface` capital letters denote matrices, while small letters denote vectors. In this particular case, $\mathbf{T}$ denotes the transport matrix:

```{math}
:label: eqn-T-matrix-example
\mathbf{T} = \begin{bmatrix}
1 & -1 & 0 & 0\\
0 & 0 & 1 & -1 \\
\end{bmatrix}
```

and $\mathbf{S}$ denotes the stoichiometric matrix:


```{math}
:label: eqn-S-matrix-example
\mathbf{S} = \begin{bmatrix}
-1 \\
1 \\
\end{bmatrix}
```

while $\dot{\mathbf{n}} = \left(\dot{n}_{A,1}, \dot{n}_{A,2}, \dot{n}_{B,1}, \dot{n}_{B,2}\right)^{T}$ and 
$\dot{\mathbf{\epsilon}} = \dot{\epsilon}_{1}$. The superscript $\star^{T}$ denotes the transpose operation (turns a row into a column, or vice-versa). As it turns out, Eqn. {eq}`eqn-matrix-vector-system-mol-bal` is a particular instance of a general system 
({prf:ref}`defn-reaction-system-matrix-vector`):

````{prf:definition} General open species mole balances
:label: defn-reaction-system-matrix-vector

The steady-state species mole balances for any well-mixed reacting system with a species set $\mathcal{M}$, a reaction set $\mathcal{R}$, and stream set $\mathcal{S}$ can be modeled by the matrix-vector system:

```{math}
\mathbf{T}\dot{\mathbf{n}} + \mathbf{S}\dot{\mathbf{\epsilon}} = \mathbf{0}
```

The matrix $\mathbf{T}$ is the $|\mathcal{M}| \times |\mathcal{M}||\mathcal{S}|$ _transport matrix_, the matrix $\mathbf{S}$ is the $|\mathcal{M}| \times |\mathcal{R}|$ _stoichiometric matrix_, $\dot{\mathbf{n}}$ denotes the $|\mathcal{M}||\mathcal{S}| \times {1}$ species flow vector and $\mathbf{\epsilon}$ denotes the $|\mathcal{R}| \times {1}$ reaction vector.

The notation $|\star|$ denotes the dimension (number of elements) of set $\star$.

````

<!-- Matrices and vectors are encoded in computer programs as {ref}`content:references:lda-arrays` -->

Matrices and vectors naturally arise in even simple chemical engineering systems like the one shown in {numref}`fig-chemical-reactor-schematic`. Let's dig deeper into their definition, structure, and operations.

(content:references:matrix-vector)=
## Matricies and Vectors
Matrices are two- (or more) dimensional rectangular arrays of numbers, widgets, etc that consist of $m$ rows and $n$ columns:

$$\mathbf{A} = 
\begin{pmatrix}
a_{1,1} & a_{1,2} & \cdots & a_{1,n} \\
a_{2,1} & a_{2,2} & \cdots & a_{2,n} \\
\vdots  & \vdots  & \ddots & \vdots  \\
a_{m,1} & a_{m,2} & \cdots & a_{m,n} 
\end{pmatrix}$$

The quantity $a_{ij}\in\mathbf{A}$ denotes an _element_ of the matrix $\mathbf{A}$, where $a_{ij}$ lives on the $i$th row and $j$th column of $\mathbf{A}$. By convention, the row index is always the first subscript, while the column index is always listed second. Matrices can have different shapes ({prf:ref}`obs-matrix-shape`):

```{prf:observation} Matrix shape
:label: obs-matrix-shape
Let the matrix $\mathbf{A}$ be an $m\times{n}$ array. Then, the matrix $\mathbf{A}$ is called:

* __Square__: If $m=n$, the matrix is called a _square_ matrix; square matrices have some unique properties (as we shall see later). 
* __Overdetermined__: If $m>n$, the matrix is called an _overdetermined_ matrix; overdetermined matrices have more rows than columns.
* __Underdetermined__: If $m<n$, the matrix is called an _underdetermined_ matrix; underdetermined matrices have more columns than rows.
```

Vectors are a specal type of matrix that is one-dimensional, where _elements_ are arranged as either a single row or single column. For example, a $m\times{1}$ _column_ vector $\mathbf{a}$ is given by:

$$\mathbf{a} = 
\begin{pmatrix}
a_{1} \\
a_{2} \\
\vdots \\
a_{m}
\end{pmatrix}$$

while a ${1}\times{n}$ dimensional _row_ vector $\mathbf{a}$ is given by:

$$\mathbf{a} = 
\begin{pmatrix}
a_{1} & a_{2} & \cdots & a_{n}
\end{pmatrix}$$

Just like numbers, vectors and matrices can participate in mathematical operations, such as addition, subtraction and multiplication, with some small differences. However, before we explore the mathematical operations of matrices and vectors, we'll discuss a few special matricies. 

````{prf:remark} Restriction to real numbers
:label: rem-real-values
While the matrix $\mathbf{A}$ or vector $\mathbf{v}$ can theoretically hold any objects, e.g., strings, dictionaries, or trees, etc., in this section, we'll only consider numbers, and in particular real numbers. Thus, we'll assume all matrices are $\mathbf{A}\in\mathbb{R}^{n\times{m}}$ and vectors $\mathbf{v}\in\mathbb{R}^{n}$; no complex numbers will be considered.  
````

### Special matrices and matrix properties
Special matrices have specific properties or characteristics that make them useful in certain mathematical operations or applications. 
Some examples of special matrices include: 

* __Diagonal and identity matrices__: A diagonal matrix is a square matrix in which all entries outside the main diagonal are zero. The special diagonal matrix with `1` on the diagonal and `0` everywhere else is called the [Identity matrix](https://en.wikipedia.org/wiki/Identity_matrix), and is often denoted by the symbol $\mathbf{I}$.
* __Triangular matrices__: [Triangular matrices](https://en.wikipedia.org/wiki/Triangular_matrix) are square arrays with zero entries below or above the main diagonal. A square matrix is called _lower triangular_ if all the entries above the main diagonal are zero. Similarly, a square matrix is called _upper triangular_ if all the entries below the main diagonal are zero.
* __Orthogonal matrices__: [Orthogonal matrices](https://en.wikipedia.org/wiki/Orthogonal_matrix)  are square matrices whose rows and columns are mutually orthogonal and have unit lengths.

(content:determinant-trace)=
#### Trace, Determinant, LU decomposition and Rank

##### Trace
The trace of a square matrix is the sum of the diagonal elements ({prf:ref}`defn-trace-A`):

````{prf:definition} Trace
:label: defn-trace-A

The trace of a square matrix is defined as the sum of its diagonal elements. Consider an $n\times{n}$ square matrix $\mathbf{A}\in\mathbb{R}^{n\times{n}}$, where $a_{ij}$ denotes the element on row $i$ and column $j$ of the matrix $\mathbf{A}$. Then, the trace, denoted as $\text{tr}\left(\mathbf{A}\right)$, is given by:

```{math}
:label: eqn-trace-A
\text{tr}\left(\mathbf{A}\right) = \sum_{i=1}^{n}a_{ii}
```
````

In [Julia](https://julialang.org), the [trace function](https://docs.julialang.org/en/v1/stdlib/LinearAlgebra/#LinearAlgebra.tr) is encoded in the [LinearAlgebra](https://docs.julialang.org/en/v1/stdlib/LinearAlgebra/) package included with the [Julia](https://julialang.org) distribution. However, the [LinearAlgebra](https://docs.julialang.org/en/v1/stdlib/LinearAlgebra/) package is not loaded by default (you'll need to issue the `using LinearAlgebra` command to access its functions):

```{code-cell} julia
# load the LinearAlgebra package
using LinearAlgebra

# Define array
A = [1 2 ; 3 4]; # the semicolon forces a newline 

# compute the trace, store in the variable value
value = tr(A);

# What is the tr(A)
println("The trace of the matrix tr(A) = $(value)")
```

[NumPy also has a trace function](https://numpy.org/doc/stable/reference/generated/numpy.trace.html) if your are doing linear algebra calculations in [Python](https://www.python.org).

##### Determinant
The [determinant of a matrix](https://en.wikipedia.org/wiki/Determinant) is a scalar value that can be computed from a square matrix ({prf:ref}`defn-det-A`):

````{prf:definition} Leibniz determinant formula
:label: defn-det-A

The Leibniz determinant formula involves summing the products of elements in a square matrix, each taken from a different row and column, with a sign factor that alternates between positive and negative.

Let $\mathbf{A}\in\mathbb{R}^{n\times{n}}$ and $a_{ij}\in\mathbf{A}$ be the entry of row $i$ and column $j$ of $\mathbf{A}$.
Then, the determinant $\det\left(\mathbf{A}\right)$ is given by:

```{math}
:label: eqn-det-A
\det\left(\mathbf{A}\right) = \sum_{\sigma\in{S_{n}}}\text{sign}\left(\sigma\right)\prod_{i=1}^{n}a_{i\sigma_{i}}
```

where $S_{n}$ denotes the Symmtery group of dimension $n$, i.e., the set of all possible permutations of the set ${1,2,\dots,n}$,
the quantity $\text{sign}\left(\sigma\right)$ equals `+1` if the permutation can be obtained with an even number of exchanges; otherwise `-1`. Finally, $a_{i\sigma_{i}}$ denotes the entry of the matrix $\mathbf{A}$ on row $i$, and column $\sigma_{i}$.
````

[Determinants](https://en.wikipedia.org/wiki/Determinant) can be used to determine whether a system of linear equations has a solution, but they also have other applications. However, the Leibniz determinant shown in {prf:ref}`defn-det-A` can be computationally expensive for large matrices since the number of permutations grows factorially with the matrix size. However, it provides a theoretical basis for understanding the determinant and can be helpful for small matrices or conceptual purposes.

In [Julia](https://julialang.org), the [determinant function](https://docs.julialang.org/en/v1/stdlib/LinearAlgebra/#LinearAlgebra.det) is encoded in the [LinearAlgebra](https://docs.julialang.org/en/v1/stdlib/LinearAlgebra/) package included with the [Julia](https://julialang.org) distribution. However, the [LinearAlgebra](https://docs.julialang.org/en/v1/stdlib/LinearAlgebra/) package is not loaded by default (you'll need to issue the `using LinearAlgebra` command to access its functions):

```{code-cell} julia
# load the LinearAlgebra package
using LinearAlgebra

# Define array
A = [1 2 ; 3 4]; # the semicolon forces a newline 

# compute the determinant, store in the variable value
value = det(A);

# What is the det(A)
println("The determinant of the matrix det(A) = $(value)")
```

Although the determinant of a general square matrix is expensive to calculate, determinants of triangular matrices are easy to compute:

````{prf:observation} Determinant triangular matrix
:label: obs-determinant-triangular-matrix

The determinant of a triangular matrix is equal to the product of its diagonal elements. This is true for both upper triangular $\mathbf{U}$ and lower $\mathbf{L}$ triangular matrices. For example, the determinant of the upper triangular $n\times{n}$ matrix $\mathbf{U}$ is equal to:

```{math}
\det\left(\mathbf{U}\right) = \prod_{i=1}^{n}u_{ii}
```

A similar expression could be written for a lower triangular matrix $\mathbf{L}$.
````

```{code-cell} julia
# load the LinearAlgebra package
using LinearAlgebra

# Define array
A = [1 0 ; 2 3]; # the semicolon forces a newline 

# compute the determinant, store in the variable value
value = det(A);

# What is the det(A)
println("The determinant of the matrix det(A) = $(value)")
```

##### LU decomposition
LU decomposition, also known as LU factorization, is a method of decomposing a square matrix $\mathbf{A}\in\mathbb{R}^{n\times{n}}$ into the product of a permutation matrix $\mathbf{P}$, and lower and upper triangular matrices $\mathbf{L}$ and $\mathbf{U}$, respectively, such that $\mathbf{A} = \mathbf{P}\mathbf{L}\mathbf{U}$. 

LU decomposition is a widely used numerical linear algebra technique with many applications. For example, the LU decomposition simplifies the computation of determinants ({prf:ref}`defn-LU-determinant`):

````{prf:definition} LU decomposition
:label: defn-LU-determinant

Let the $\mathbf{A}\in\mathbb{R}^{n\times{n}}$ be LU factoriable, such that $\mathbf{A} = \mathbf{P}\mathbf{L}\mathbf{U}$. Then, the determinant $\det\left(\mathbf{A}\right)$ is given by:

```{math}
:label: eqn-LU-det
\det\left(\mathbf{A}\right) = \det\left(\mathbf{P}\right)\cdot\det\left(\mathbf{L}\right)\cdot\det\left(\mathbf{U}\right)
```
````

In [Julia](https://julialang.org), the [LU factorization](https://docs.julialang.org/en/v1/stdlib/LinearAlgebra/#LinearAlgebra.lu) is encoded in the [LinearAlgebra](https://docs.julialang.org/en/v1/stdlib/LinearAlgebra/) package included with the [Julia](https://julialang.org) standard library:


```{code-cell} julia
# load the LinearAlgebra package
using LinearAlgebra

# Define array
A = [1 2 ; 3 4]; # the semicolon forces a newline 

# compute the determinant, store in the variable value
F = lu(A);
L = F.L
U = F.U
P = F.P

# Test -
AT = P*L*U
```


##### Rank
Finally, the [rank of a matrix](https://en.wikipedia.org/wiki/Rank_(linear_algebra)) is a measure of the number of linearly independent rows or columns ({prf:ref}`defn-rank-A`): 

````{prf:definition} Rank
:label: defn-rank-A
The rank of a matrix _r_ of a $m\times{n}$ matrix $\mathbf{A}$ is always less than or equal to the minimum of its number of rows and columns:

```{math}
:label: eqn-rank-inequality
r\leq\min\left(m,n\right)
```

Rank can also be considered a measure of the unique information in a matrix; if there is redundant information (rows or columns that are not linearly independent), a matrix will have less than full rank. 
````

```{code-cell} julia
# load the LinearAlgebra package
using LinearAlgebra

# Define array
A = [1 2 ; 3 4]; # the semicolon forces a newline 

# compute the rank, store in the variable value
value = rank(A);

# What is the rank(A)
println("The rank of the matrix rank(A) = $(value)")
```


<!-- The kernel of a matrix is the set of all solutions to the homogeneous equation $\mathbf{A}\mathbf{x} = \mathbf{0}$, where $\mathbf{A}$ is the matrix, and $\mathbf{x}$ is a column vector. The dimension of the kernel is equal to the number of columns in the matrix minus its rank. The kernel is also known as the null space of the matrix. -->

(content:matrix-vector-operations)=
## Mathematical matrix and vector operations
Matrix and vector operations are mathematical procedures to add, subtract and multiple matrices and vectors.  Matrix and vector operations are similar in some wats to scalar numbers, with some crucial differences and one important caveat: _they must be compatible_.

Many (if not all) modern programming languages implement a version of the [Basic Linear Algebra Subprograms (BLAS)](https://en.wikipedia.org/wiki/Basic_Linear_Algebra_Subprograms) library. [BLAS](https://en.wikipedia.org/wiki/Basic_Linear_Algebra_Subprograms) is a low-level library that efficiently implements basic linear algebra operations, such as vector and matrix multiplication, matrix-vector multiplication, and solving linear systems of equations. The library is designed to provide high-performance implementations of these routines that can be used as building blocks for more complex algorithms. 

However, while you will not have to implement most matrix and vector operations on your own, it is still helpful to understand these operations and how broadly they work.  

### Addition and subtraction
Compatible matrices and vectors can be added and subtracted just like scalar quantities. The addition (or subtraction) operations for matrices or vectors are done element-wise. Thus, if these objects don't have the same number of elements, then addition and subtraction operations don't make sense.  

````{prf:definition} Vector addition
:label: obs-same-dimension
Suppose we have two vectors $\mathbf{a}\in\mathbb{R}^{m}$ and $\mathbf{b}\in\mathbb{R}^{m}$. The sum of these vectors $\mathbf{y} = \mathbf{a} + \mathbf{b}$ is the vector $\mathbf{y}\in\mathbb{R}^{m}$ given by:

```{math}
:label: eqn-vector-addition
y_{i} = a_{i} + b_{i}\qquad{i=1,\dots,m}
```

__Compatibility__: The vectors $\mathbf{a}$ and $\mathbf{b}$ are compatible if they have the same number of elements. 
````

Vector addition is straightforward to implement ({prf:ref}`algo-vector-addition`):

````{prf:algorithm} Naive vector addition
:class: dropdown
:label: algo-vector-addition

**Inputs** Compatible vectors $\mathbf{a}$ and $\mathbf{b}$

**Outputs** Vector sum $\mathbf{y}$

**Initialize**
1. set $n\leftarrow\text{length}(\mathbf{a})$
1. set $\mathbf{y}\leftarrow\text{zeros}(n)$

**Main**
1. for $i\in{1,\dots,n}$
    1. $y_{i}\leftarrow~a_{i} + b_{i}$

**Return** sum vector $\mathbf{y}$
````

#### Is the naive approach really the best?
{prf:ref}`algo-vector-addition` may not be the _best_ way to implement vector (or matrix) addition or subtraction operations. Many modern programming languages and libraries, e.g., [the Numpy library in Python](https://numpy.org) or [Julia](https://julialang.org), support _vectorization_, i.e., special operators that encode element-wise addition, subtraction or other types of element-wise operations without the need to write `for` loops. 

In [Julia](https://julialang.org), you can use the vectorized `.+` operator for element-wise addition, while element-wise subtraction can be encoded with the `.-` operator. Vectorized code typically executes faster than naive implementations such as {prf:ref}`algo-vector-addition` because the _vectorization_ takes advantage of advanced techniques to improve performance. 

##### Additional information on vectorization
* [Vectorized operations in NumPy](https://www.geeksforgeeks.org/vectorized-operations-in-numpy/). [NumPy](https://numpy.org) is a Python scientific computing package. It is a Python library that provides multidimensional array objects and routines for fast operations on arrays.
* [Vectorized operations in Julia](https://docs.julialang.org/en/v1/manual/functions/#man-vectorized) are included in the standard library. The standard library already defines common operators such as addition `.+`, subtraction `.-`, multiplication `.*`, and power `.^`. However, the [broadcasting](https://docs.julialang.org/en/v1/manual/arrays/#Broadcasting) paradigm allows you to write your own vectorized operations.

### Multiplication operations

#### Scalar multiplication
The most straightforward multiplication operation is between a scalar and a vector (or matrix). Multiplying a vector (or a matrix) by a scalar constant is done element-wise ({prf:ref}`defn-scalar-multiplication`):

````{prf:definition} Scalar multiplication of a matrix or vector
:label: defn-scalar-multiplication

__Vector__: Suppose we have a vector $\mathbf{v}\in\mathbb{R}^{n}$ and a real scalar constant $c\in\mathbb{R}$. Then the scalar product, denoted by $\mathbf{y} = c\mathbf{v}$, is given by:

```{math}
y_{i} = cv_{i}\qquad{i=1,2,\dots,n}
```

where $y_{i}$ and $v_{i}$ denote the ith component of the product vector $\mathbf{y}\in\mathbb{R}^{n}$, and the input vector $\mathbf{v}\in\mathbb{R}^{n}$. 

__Matrix__: Likewise, suppose we have the matrix $\mathbf{X}\in\mathbb{R}^{m\times{n}}$ and a real scalar constant $c\in\mathbb{R}$. Then, the scalar product, denoted by $\mathbf{Y} = c\mathbf{X}$, is given by:

```{math}
y_{ij} = cx_{ij}\qquad{i=1,2,\dots,m,~j=1,2,\dots,n}
```

where $y_{ij}$ and $x_{ij}$ denote components of the product matrix $\mathbf{Y}\in\mathbb{R}^{m\times{n}}$, and the input matrix $\mathbf{X}\in\mathbb{R}^{m\times{n}}$, respectively. 
````

#### Vector-vector multiplication
__Inner product__: Two compatible vectors $\mathbf{a}$ and $\mathbf{b}$ can be multiplied together to produce a _scalar_ in an operation called an [inner product operation](https://en.wikipedia.org/wiki/Inner_product_space) ({prf:ref}`defn-vector-vector-multiplication`):

````{prf:definition} Inner product
:label: defn-vector-vector-multiplication

Let $\mathbf{a}\in\mathbb{R}^{m\times{1}}$ and $\mathbf{b}\in\mathbb{R}^{m\times{1}}$. Then, the _vector-vector inner product_ is given by:

```{math}
:label: eqn-vector-vector-inner-product
y = \mathbf{a}^{T}\mathbf{b}
```

where $\mathbf{a}^{T}$ denotes the transpose of the vector $\mathbf{a}$ and the scalar $y$ equals:

```{math}
:label: eqn-vector-inner-product-index-form
y = \sum_{i=1}^{m}a_{i}b_{i}
```

__Compatibility__: This operation is possible if the vectors $\mathbf{a}$ and $\mathbf{b}$ have the same number of elements.
````

__Outer product__: Suppose you have an $m\times{1}$ vector $\mathbf{a}$, and an $n\times{1}$ vector $\mathbf{b}$. The vectors $\mathbf{a}$ and $\mathbf{b}$ can be multipled together to form a $m\times{n}$ matrix through an [outer product operation](https://en.wikipedia.org/wiki/Outer_product) ({prf:ref}`defn-vector-vector-multiplication-op`):


````{prf:definition} Outer product
:label: defn-vector-vector-multiplication-op

Let $\mathbf{a}\in\mathbb{R}^{m\times{1}}$ and $\mathbf{b}\in\mathbb{R}^{n\times{1}}$. Then, the _vector-vector outer product_ given by:

```{math}
:label: eqn-vector-vector-outer-product
\mathbf{Y} = \mathbf{a}\otimes\mathbf{b}
```

produces the matrix $\mathbf{Y}\in\mathbb{R}^{m\times{n}}$ with elements:

```{math}
:label: eqn-vector-outer-product-index-form
y_{ij} = a_{i}b_{j}\qquad{i=1,2,\dots,m~\text{and}~j=1,2,\dots,n}
```
The outer product operation is equivalent to $\mathbf{Y} = \mathbf{a}\mathbf{b}^{T}$.

__Compatibility__: This operation is possible if the vectors $\mathbf{a}$ and $\mathbf{b}$ have the same number of elements.

````

#### Right multiplication of a matrix by a vector
A common operation is _right matrix-vector multiplication_ of a matrix $\mathbf{A}$ by a vector $\mathbf{x}$. If the matrix $\mathbf{A}$ and the vector $\mathbf{x}$ are compatible,  _right matrix-vector multiplication_ will produce a vector with the same number of rows as the original matrix $\mathbf{A}$ ({prf:ref}`defn-right-matrix-vector-multiplication`):

````{prf:definition} Right matrix-vector multiplication
:label: defn-right-matrix-vector-multiplication

Let $\mathbf{A}\in\mathbb{R}^{m\times{n}}$ and $\mathbf{x}\in\mathbb{R}^{n\times{1}}$. Then, the right matrix-vector product given by:

$$\mathbf{y} = \mathbf{A}\mathbf{x}$$

generates the $\mathbf{y}\in\mathbb{R}^{m\times{1}}$ vector where the $i$th element is given by:

$$y_{i} = \sum_{j=1}^{n}a_{ij}x_{j}\qquad{i=1,2,\cdots,m}$$

__Compatibility__: This operation is possible if the number of columns of the matrix $\mathbf{A}$ equals the number of rows of the vector $\mathbf{x}$.
````

The right multiplication operation can be represented graphically as a series of scalar$\times$vector multiplication and vector-vector summation operations ({numref}`fig-right-multiplication-matrix-vector`):

```{figure} ./figs/Fig-Ab-Multiplication.pdf
---
height: 140px
name: fig-right-multiplication-matrix-vector
---
Schematic of the right matrix-vector product. The right product, which produces a column vector with the same number of rows as the matrix, can be modeled as a series of scalar$\times$vector multiplication and vector-vector summation operations. 
```

A psuedo code implemetation of {prf:ref}`defn-right-matrix-vector-multiplication` is given in {prf:ref}`algo-right-multiplication-matrix-vector`:

```{prf:algorithm} Naive right multiplication of a matrix by a vector
:label: algo-right-multiplication-matrix-vector
:class: dropdown

**Inputs:** Matrix $\mathbf{A}\in\mathbb{R}^{m\times{n}}$ and vector $\mathbf{x}\in\mathbb{R}^{n\times{1}}$

**Outputs:** Product vector $\mathbf{y}\in\mathbb{R}^{m\times{1}}$.

**Initialize**
1. set  $(m, n)\leftarrow\text{size}(\mathbf{A})$
1. initialize $\mathbf{y}\leftarrow\text{zeros}(m)$
  
**Main**
1. for $i\in{1,\dots,m}$:
    1. for $j\in{1,\dots,n}$:
        1. $y_{i}~\leftarrow y_{i} + a_{ij}\times{x_{j}}$

**Return** vector $\mathbf{y}$
```

#### Left multiplication of a matrix by a vector
We could also consider the _left multiplication_ of a matrix by a vector. Suppose $\mathbf{A}$ is an $m\times{n}$ matrix, and $\mathbf{x}$ is a $m\times{1}$ vector, then the _left matrix-vector product_ is a row vector ({prf:ref}`defn-left-multiply-matrix-vector`):

````{prf:definition} Left matrix-vector product
:label: defn-left-multiply-matrix-vector

Let $\mathbf{A}\in\mathbb{R}^{m\times{n}}$ and $\mathbf{x}\in\mathbb{R}^{m\times{1}}$. Then, the _left matrix-vector product_:

```{math}
:label: eqn-left-matrix-vector-product-matrix
\mathbf{y} = \mathbf{x}^{T}\mathbf{A}
```

produces the vector $\mathbf{y}\in\mathbb{R}^{1\times{n}}$ such that:

```{math}
:label: eqn-left-matrix-vector-product-index
y_{i} = \sum_{j=1}^{m}a_{ji}x_{j}\qquad{i=1,2,\dots,n}
```

where $\mathbf{x}^{T}$ denotes the [transpose](https://en.wikipedia.org/wiki/Transpose) of the vector $\mathbf{x}$.

__Compatibility__: This operation is possible if the number of rows of the matrix $\mathbf{A}$ equals the number of columns of the transpose of the vector $\mathbf{x}$.

````

The _left multiplication operation_ can be represented graphically as a series of scalar$\times$vector multiplication and vector-vector summation operations ({numref}`fig-left-multiplication-matrix-vector`):

```{figure} ./figs/Fig-bA-Left-Multiplication.pdf
---
height: 160px
name: fig-left-multiplication-matrix-vector
---
Schematic of the left matrix-vector product. The left product, which produces a row vector with the same number of columns as the matrix, can be modeled as a series of scalar$\times$vector multiplication and vector-vector summation operations. 
```

A psuedo code implemetation of {prf:ref}`defn-left-multiply-matrix-vector` is given in {prf:ref}`algo-left-multiplication-matrix-vector`:

```{prf:algorithm} Naive left multiplication of a matrix by a vector
:class: dropdown
:label: algo-left-multiplication-matrix-vector

**Inputs:** Matrix $\mathbf{A}\in\mathbb{R}^{m\times{n}}$ and vector $\mathbf{x}\in\mathbb{R}^{m\times{1}}$

**Outputs:** Product vector $\mathbf{y}\in\mathbb{R}^{1\times{n}}$.

**Initialize**
1. set  $(m, n)\leftarrow\text{size}(\mathbf{A})$
1. initialize $\mathbf{y}\leftarrow\text{zeros}(n)$
  
**Main**
1. for $i\in{1,\dots,n}$:
    1. for $j\in{1,\dots,m}$:
        1. $y_{i}~\leftarrow y_{i} + a_{ji}\times{x_{j}}$

**Return** vector $\mathbf{y}$
```

#### Matrix-Matrix products
Many of the important uses of matrices in engineering practice depend upon the definition of matrix multiplication ({prf:ref}`defn-matrix-matrix-product`):
 
````{prf:definition} Matrix-matrix product
:label: defn-matrix-matrix-product

Let $\mathbf{A}\in\mathbb{R}^{m\times{n}}$ and $\mathbf{B}\in\mathbb{R}^{n\times{p}}$. The matrix-matrix product $\mathbf{C} = \mathbf{A}\times\mathbf{B}$ produces the matrix $\mathbf{C}\in\mathbb{R}^{m\times{p}}$ with elements:

```{math}
:label: eqn-matrix-matrix-product-elements
c_{ij} = \sum_{k=1}^{n}a_{ik}b_{kj}\qquad{i=1,2,\cdots,m~\text{and}~j=1,2,\cdots,p}
```

__Compatibility__: The matrices $\mathbf{A}\in\mathbb{R}^{m\times{n}}$ and $\mathbf{B}\in\mathbb{R}^{n\times{p}}$ are compatible if the number of columns of $\mathbf{A}\in\mathbb{R}^{m\times{n}}$ equals the number of rows of $\mathbf{B}\in\mathbb{R}^{n\times{p}}$. Otherwise, the matrices are incompatible and cannot be multiplied. 
````

The _matrix-matrix multiplication_ operation can be represented graphically as a series of right matrix-vector products ({numref}`fig-multiplication-matrix-matrix`):

```{figure} ./figs/Fig-AB-Matrix-Matrix-Multiplication.pdf
---
height: 380px
name: fig-multiplication-matrix-matrix
---
Schematic of matrix-matrix multiplication. This operation, which produces a matrix product, can be modeled as a series of right matrix-vector products. 
```

A psuedo code implemetation of {prf:ref}`defn-matrix-matrix-product` is given in {prf:ref}`algo-matrix-matrix-code`:

````{prf:algorithm} Naive Matrix $\times$ Matrix multiplication
:class: dropdown
:label: algo-matrix-matrix-code

**Inputs** Matrix $\mathbf{A}\in\mathbb{R}^{m\times{n}}$, and matrix $\mathbf{B}\in\mathbb{R}^{n\times{p}}$

**Outputs** Product matrix $\mathbf{C}\in\mathbb{R}^{m\times{p}}$.

**Initialize**

1. initialize $(m, n)\leftarrow\text{size}(\mathbf{A})$
2. initialize $(n, p)\leftarrow\text{size}(\mathbf{B})$
3. initialize matrix $\mathbf{C}\leftarrow\text{zeros}(m, p)$

**Main**
1. for $i\in{1,\dots, m}$
    1. for $j\in{1,\dots,p}$
        1. for $k\in{1,\dots,n}$
            1. $c_{ij}~\leftarrow~c_{ij} + a_{ik}\times{b_{kj}}$

**Return** matrix $\mathbf{C}$
````



Finally, matrix-matrix products have different properties compared with the product of two scalar numbers:
* __Non-commutativity__: Matrix multiplication is typically _not_ commutative, e.g., $\mathbf{A}\mathbf{B}\neq\mathbf{B}\mathbf{A}$.
* __Distributivity__: Matrix products are distributive, i.e., $\mathbf{A}\left(\mathbf{B}+\mathbf{C}\right) = \mathbf{A}\mathbf{B}+\mathbf{A}\mathbf{C}$.
* __Associative__: Matrix products are associative, i.e., $\mathbf{A}\left(\mathbf{B}\mathbf{C}\right) = \left(\mathbf{A}\mathbf{B}\right)\mathbf{C}$.
* __Transpose__: The transpose of a matrix product is the product of transposes, i.e., $\left(\mathbf{A}\mathbf{B}\right)^{T} = \mathbf{B}^{T}\mathbf{A}^{T}$.

---

(content:references:soln-laes-start)=
## Linear algebraic equations
Linear Algebraic Equations (LAEs) arise in many different engineering fields. In Chemical Engineering, these types of equations naturally arise from steady-state (or time-discretized) balances. Let's start by asking a simple question, when does a solution exist to the generic system of LAEs of the form:

```{math}
:label: eqn-general-system-laes
\mathbf{A}\mathbf{x} = \mathbf{b}
```

where $\mathbf{A}\in\mathbb{R}^{m\times{n}}$ is the system matrix, $\mathbf{x}\in\mathbb{R}^{n\times{1}}$ denotes a column vector of unknowns (what we want to solve for), and $\mathbf{b}\in\mathbb{R}^{m\times{1}}$ is the right-hand side vector: 

* __Homogenous system__: A homogeneous system of linear algebraic equations is a system in which the right-hand side vector $\mathbf{b}= \mathbf{0}$; every entry in the $\mathbf{b}$-vector is equal to zero. The solution set for a homogeneous system is a subspace in linear algebra called the [kernel or nullpsace](https://en.wikipedia.org/wiki/Kernel_(linear_algebra)).

* __Non-homogeneous system__: A non-homogeneous system of linear algebraic equations is a system in which at least one entry of the right-hand side vector $\mathbf{b}$ is non-zero.

### Existence
The existence of a solution to a system of linear equations depends on the righ-hand side vector, the number of equations and the number of variables in the system. 

#### Homogeneous square systems
For a homogeneous square system of linear equations, the trivial solution $\mathbf{x}=\mathbf{0}$ will always exist, but non-trivial solutions may not be unique. Thus, a homogeneous square system of linear algebraic equations with $n\times{n}$ system matrix $\mathbf{A}$ and unknown vector $\mathbf{x}$:

```{math}
:label: eqn-homogenous-laes
\mathbf{A}\mathbf{x} = \mathbf{0}
```

may or may not have a unique solution. However, there is an easy check to determine the existence of a solution for a square homogenous system ({prf:ref}`defn-homogenous-soln-existence`):

````{prf:definition} Homogenous solution existence
:label: defn-homogenous-soln-existence

A square homogeneous system of linear algebraic equations with $n\times{n}$ system matrix $\mathbf{A}$ and unknown vector $\mathbf{x}$
has a unique solution if and only if the system matrix $\mathbf{A}$ has a zero determinant:

```{math}
:label: eqn-det-homogenous-cond
\det\left(\mathbf{A}\right) = 0
```

The determinant condition is an easy theoretical test to check for a unique solution to a homogenous system of linear algebraic equations. 

However, in practice, the determinant directly can be computationally expensive to compute. Alternatively, existence can also be checked by computing the [rank](https://en.wikipedia.org/wiki/Rank_(linear_algebra)) of matrix $\mathbf{A}$.  If a matrix is less than full rank, then $\det{\left(\mathbf{A}\right)}=0$.
````

<!-- There are many different ways to compute rank, however, we'll use the [rank](https://docs.julialang.org/en/v1/stdlib/LinearAlgebra/#LinearAlgebra.rank) function in [Julia](https://julialang.org). -->

#### Non-homogeneous square systems
A non-homogeneous square system of linear algebraic equations with $n\times{n}$ coefficient matrix $\mathbf{A}$, $n\times{1}$ unknown vector $\mathbf{x}$ and the $n\times{1}$ right-hand-side vector $\mathbf{b}$:

```{math}
:label: eqn-non-homogenous-laes
\mathbf{A}\mathbf{x} = \mathbf{b}
```

will have a unique solution if there exists a matrix $\mathbf{A}^{-1}$ such that:

```{math}
:label: eqn-non-homogenous-laes-inverse
\mathbf{x} = \mathbf{A}^{-1}\mathbf{b}
```

where the $\mathbf{A}^{-1}$ is called the inverse of the matrix $\mathbf{A}$ ({prf:ref}`defn-matrix-inverse`):


````{prf:definition} Matrix inverse existence
:label: defn-matrix-inverse

An $n\times{n}$ square matrix $\mathbf{A}\in\mathbb{R}^{n\times{n}}$ has an unique inverse $\mathbf{A}^{-1}$ such that:

```{math}
:label: eqn-matrix-A
\mathbf{A}^{-1}\mathbf{A} = \mathbf{A}\mathbf{A}^{-1} = \mathbf{I}
```

if $\det\left(\mathbf{A}\right)\neq{0}$. If an inverse exists, the matrix $\mathbf{A}$ is called non-singular, otherwise $\mathbf{A}$ is singular. 
````

(content:references:solution-approaches)=
### Solutions approaches
Several methods exist to find the solution of square systems of linear equations:

* {ref}`content:references:gaussian-elimination` solves linear equations by using a sequence of operations to reduce the system to an upper triangular form and then back substitution to find the solution. 
* {ref}`content:references:iterative-methods` are another class of algorithms used to find approximate solutions for large and sparse linear systems of equations. These methods work by starting with an initial guess for the solution and then repeatedly updating the guess until it converges to the actual answer.

(content:references:gaussian-elimination)=
### Gaussian elimination 
[Gaussian elimination](https://en.wikipedia.org/wiki/Gaussian_elimination) is an efficient method for solving large square systems of linear algebraic equations. [Gaussian elimination](https://en.wikipedia.org/wiki/Gaussian_elimination) is based on "eliminating" variables by adding or subtracting equations (rows) so that the coefficients of one variable are eliminated in subsequent equations. This allows us to solve the remaining variables one at a time until we have a solution for the entire system.

Let's start exploring this approach by looking at solving a triangular system of equations.


#### Triangular systems
Let's consider a non-singular $3\times{3}$ lower triangular system of equations:

```{math}
:label: eqn-triangular-system
\begin{pmatrix}
l_{11} & 0 & 0 \\
l_{21} & l_{22} & 0 \\
l_{31} & l_{32} & l_{33}
\end{pmatrix}
\begin{pmatrix}
x_{1} \\
x_{2} \\
x_{3}
\end{pmatrix} = 
\begin{pmatrix}
b_{1} \\
b_{2} \\
b_{3}
\end{pmatrix}
```

Since the matrix is non-singular, the diagonal elements $l_{ii},~i=1,2,3$ are non-zero. This allows us to take advantage of the triangular structure to solve for the unknown $x_{i}$ values:

$$
\begin{eqnarray}
x_{1} & = & b_{1}/l_{11} \\
x_{2} & = & \left(b_{2} - l_{21}x_{1}\right) / l_{22} \\
x_{3} & = & \left(b_{3} - l_{31}x_{1} - l_{32}x_{2}\right) / l_{33}
\end{eqnarray}
$$

This idea, called _forward substitution_, can be extended to any $n\times{n}$ non-singular lower triangular system ({prf:ref}`defn-general-forward-sub`):

````{prf:definition} Forward substitution
:label: defn-general-forward-sub

Suppose we have an $n\times{n}$ system ($n\geq{2}$) of equations which is lower triangular, and non-singular of the form:

```{math}
:label: eqn-lower-triag-system
\mathbf{L}\mathbf{x} = \mathbf{b}
```

Then, the solution of Eqn. {eq}`eqn-lower-triag-system` is given by:

$$
\begin{eqnarray}
x_{1} & = & \frac{b_{1}}{l_{11}} \\
x_{i} & = & \frac{1}{l_{ii}}\left(b_{i} - \sum_{j=1}^{i-1}l_{ij}x_{j}\right)\qquad{i=2,\dots,n}
\end{eqnarray}
$$

where $l_{ii}\neq{0}$. The global operation count for this appraoch is $n^{2}$ floating point operations (flops).

````

Alternatively, we can take a similar apprach with an upper triangular system,  called _backward substituion_, to solve for 
unknow solution vector $\mathbf{x}$ ({prf:ref}`defn-general-backward-sub`):


````{prf:definition} Backward substitution
:label: defn-general-backward-sub

Suppose we have an $n\times{n}$ system ($n\geq{2}$) of equations which is upper triangular, and non-singular of the form:

```{math}
:label: eqn-upper-triag-system
\mathbf{U}\mathbf{x} = \mathbf{b}
```

Then, the solution of Eqn. {eq}`eqn-upper-triag-system` is given by:

$$
\begin{eqnarray}
x_{n} & = & \frac{b_{n}}{u_{nn}} \\
x_{i} & = & \frac{1}{u_{ii}}\left(b_{i} - \sum_{j=i+1}^{n}u_{ij}x_{j}\right)\qquad{i=n-1,\dots,1}
\end{eqnarray}
$$

where $u_{ii}\neq{0}$. The global operation count for this appraoch is $n^{2}$ floating point operations (flops).

````

<!-- Since we talking about [Gaussian elimination](https://en.wikipedia.org/wiki/Gaussian_elimination), which produces an upper triangular matrix $\mathbf{U}$, let's review a _backward substitution_ algorithm to estimate the unknown vector $\mathbf{x}$ given a non-singular upper triangular $\mathbf{U}$ and right-hand-side vector $\mathbf{b}$ based on {prf:ref}`defn-general-backward-sub`: -->

Solving a lower or upper triangular system can quickly be done with a substitution approach. However, upper (or lower) triangular systems rarely arise naturally in applications. So why do we care about triangular systems? 

#### Row reduction approaches
Converting a matrix $\mathbf{A}$ to an upper triangular form involves performing a sequence of elementary row operations to transform the matrix into a form where all entries below the main diagonal are zero. This is typically done by row swapping, scaling, and adding rows. The goal is to create a matrix where each row’s leading coefficient (the first non-zero element) is `1`, and all the other entries in that column are `0`. This is achieved by using the leading coefficient of one row to eliminate the corresponding entries in the other rows.

Let's walk through a simple example to illustrate the steps involved with [row reduction](https://en.wikipedia.org/wiki/Row_echelon_form); consider the solution of the 3$\times$3 system:

```{math}
:label: eqn-ge-test-system

\begin{bmatrix}
2 & -1 & 0 \\
-1 & 2 & -1 \\
0 & -1 & 2
\end{bmatrix}
\begin{pmatrix}
x_{1} \\
x_{2} \\
x_{3}
\end{pmatrix} =
\begin{pmatrix}
0 \\
1 \\
0
\end{pmatrix}
```

__Step 1__: Form the augemented matrix $\bar{\mathbf{A}}$ which is defined as the matrix $\mathbf{A}$ with the righ-hand-side vector $\mathbf{b}$ appended as the last column:

```{math}
:label: eqn-augmented-array-A
\bar{\mathbf{A}} = \begin{bmatrix}
2 & -1 & 0 &\bigm| & 0 \\
-1 & 2 & -1 &\bigm| & 1 \\
0 & -1 & 2 &\bigm| & 0
\end{bmatrix}
```

__Step 2__: Perform row operations to reduce the augmented matrix $\bar{\mathbf{A}}$ to [the upper triangular row echelon form](https://en.wikipedia.org/wiki/Row_echelon_form). We can perform row operations:

* _Swapping_: We can interchange the order of the rows to get zeros below the main diagonal.
* _Scaling_:  If the first nonzero entry of row $R_{i}$ is $\lambda$, we can convert to $1$ through the operation: $R_{i}\leftarrow{1/\lambda}R_{i}$
* _Addition and subtraction_: For any nonzero entry below the top one, use an elementary row operation to change it to zero; If two rows $R_{i}$ and $R_{j}$ have nonzero entries in column $k$, we can transform the (j,k) entry into a zero using $R_{j}\leftarrow{R}_{j} - (a_{jk}/a_{ik})R_{i}$. 

In Eqn {eq}`eqn-augmented-array-A`, the first row and column entry are non-zero, and interchanging rows does not improve our progress. However, the leading non-zero coefficient is not `1`; thus, let's use a scaling row operation:

```{math}
:label: eqn-augmented-array-A-step3
\bar{\mathbf{A}} = \begin{bmatrix}
2 & -1 & 0 &\bigm| & 0 \\
-1 & 2 & -1 &\bigm| & 1 \\
0 & -1 & 2 &\bigm| & 0
\end{bmatrix} \stackrel{R_{1}\leftarrow(1/2)R_{1}}{\longrightarrow}
\begin{bmatrix}
1 & -0.5 & 0 &\bigm| & 0 \\
-1 & 2 & -1 &\bigm| & 1 \\
0 & -1 & 2 &\bigm| & 0
\end{bmatrix}
```

We now eliminate the coefficient below the top row; in this case $i=1$, $j=1$ and $k=1$ which gives the operation: $R_{2}\leftarrow{R}_{2} + R_{1}$:

```{math}
:label: eqn-augmented-array-A-step4
\begin{bmatrix}
1 & -0.5 & 0 &\bigm| & 0 \\
-1 & 2 & -1 &\bigm| & 1 \\
0 & -1 & 2 &\bigm| & 0
\end{bmatrix}
\stackrel{R_{2}\leftarrow{R_{2}+R_{1}}}{\longrightarrow}
\begin{bmatrix}
1 & -0.5 & 0 &\bigm| & 0 \\
0 & 1.5 & -1 &\bigm| & 1 \\
0 & -1 & 2 &\bigm| & 0
\end{bmatrix}
```

The leading coefficient of row 3 was already zero; thus, we have completed the row reduction operations for row 1. Moving to row 2, the first non-zero coefficient is in column 2. Scale row 2, and then subtract from row 3, etc. 

While the logic underlying the row reduction of the matrix $\mathbf{A}$ to the upper triangular row-echelon form $\mathbf{U}$ seems simple, the implementation of a general, efficient row reduction function can be complicated; let's develop a naive implementation of a row reduction procedure ({prf:ref}`algo-ge-basic`):

````{prf:algorithm} Naive Row Reduction
:class: dropdown
:label: algo-ge-basic

**Input**: 
Matrix $\mathbf{A}$, the vector $\mathbf{b}$

**Output**: solution $\hat{\mathbf{x}}$

**Initialize**:
1. set $(n,m)\leftarrow\text{size}(\mathbf{A})$
1. set $\bar{\mathbf{A}}\leftarrow\text{augment}(\mathbf{A},\mathbf{b})$

**Main**
1. for $i\in{1}\dots{n-1}$
    1. set $\text{pivot}\leftarrow{a_{ii}}$
    2. set $\text{pivotRow}\leftarrow{i}$

    1. for $j\in{i+1}\dots{n}$
        1. if $\text{abs}(\bar{a}_{ji}) > \text{abs}(\text{pivot})$
            1. set $\text{pivot}\leftarrow\bar{a}_{ji}$
            2. set $\text{pivotRow}\leftarrow{j}$
    
    1. set $\bar{\mathbf{A}}\leftarrow\text{swap}(\bar{\mathbf{A}},i,\text{pivotRow})$
    
    1. for $j\in{i+1}\dots{n}$
        1. set $\text{factor}\leftarrow{a_{ji}}/\text{pivot}$
        1. for $k\in{1,\dots,m}$
            1. set $a_{jk}\leftarrow{a_{jk}} - \text{factor}\times{a_{ik}}$

**Return** $\text{row reduced}~\bar{\mathbf{A}}$
````

__Step 3__: Solve for the unknown values $x_{1},\dots,x_{n}$ using _back substitution_:

```{math}
:label: eqn-back-sub-matrix-A
\begin{bmatrix}
1 & -0.5 & 0 &\bigm| & 0 \\
0 & 1 & -0.66 &\bigm| & 0.66 \\
0 & 0 & 1 &\bigm| & 0.49
\end{bmatrix}
```

This system in Eqn. {eq}`eqn-back-sub-matrix-A` is in row reduced form; thus, it can now be solved using _back substitution_. Based on the {prf:ref}`defn-general-backward-sub`, we implemented a back substitution routine {prf:ref}`algo-backward-substituion`:

````{prf:algorithm} Backward substituion
:label: algo-backward-substituion
:class: dropdown

**Inputs** Upper triangular matrix $\mathbf{U}$, column vector $\mathbf{b}$

**Outputs** solution vector $\mathbf{x}$

**Initialize**
1. set $n\leftarrow\text{nrows}\left(\mathbf{U}\right)$
1. set $\mathbf{x}\leftarrow\text{zeros}\left(n\right)$
1. set $x_n\leftarrow{b_n/u_{nn}}$

**Main**
1. for $i\in{n-1\dots,1}$
    1. set $\text{sum}\leftarrow{0}$
    1. for $j\in{i+1,\dots,n}$
        1. $\text{sum}\leftarrow\text{sum} + u_{ij}\times{x_{j}}$

    1. $x_{i}\leftarrow\left(1/u_{ii}\right)\times\left(b_{i} - \text{sum}\right)$

**Return** solution vector $\mathbf{x}$

````

Now that we have both row reduction and back substitytion algorithms, let's look at a few examples. First,  consider the solution of a square system of equations that arise from mole balance with a single first-order decay reaction ({prf:ref}`example-time-discretized-decay`):

````{prf:example} First-order decay
:label: example-time-discretized-decay
:class: dropdown

Setup a system of linear algebraic equations whose solution describes the concentration as a function of time for a compound $A$ that undergoes first-order decay in a well-mixed batch reactor. The concentration balance for compound $A$ is given by:

```{math}
:label: eqn-balance-concentration
\frac{dC_{A}}{dt} = -\kappa{C_{A}}
```

where $\kappa$ denotes the first-order rate constant governing the rate of decay (units: 1/time), and the initial condition is given by $C_{A,0}$.
Let $C_{A,0} = 10~\text{mmol/L}$, $\kappa = 0.5~\text{hr}^{-1}$ and $h = 0.1$. 

__Solution__: Let's discretize the concentration balance using a [forward finte difference](https://en.wikipedia.org/wiki/Finite_difference) approximation of the time derivatrive:

```{math}
:label: eqn-CA-recursion
C_{A,j+1} = C_{A,j} - h\kappa{C_{A,j}}
```

where $h$ denotes the time step-size, and $C_{A,\star}$ denotes the concentration of $A$ at time-step $\star$. Starting with $j=0$, 
Eqn {eq}`eqn-CA-recursion` can be used to construct a $T{\times}T$ matrix where each rows is a seperate time-step:

```{math}
:label: eqn-CA-system-TxT
\begin{pmatrix}
1 & 0 & \dots & 0 \\
(\kappa{h} - 1) & 1 & \dots & 0 \\
\vdots & \vdots & \vdots & \vdots \\
0 & \dots & (\kappa{h} - 1) & 1
\end{pmatrix}
\begin{pmatrix}
C_{A,1} \\
C_{A,2} \\
\vdots \\
C_{A,T}
\end{pmatrix} = 
\begin{pmatrix}
C_{A,0}\left(1-h\kappa\right) \\
0 \\
\vdots \\
0 
\end{pmatrix}
```

````



(content:references:iterative-methods)=
### Iterative solution methods
Iterative methods work by improving an initial guess of the solution until a desired level of accuracy is achieved. Commonly used iterative methods include {ref}`content:references:jacobi-iterative-method` and the 
{ref}`content:references:gauss-seidel-method`.

* __At each iteration__: an iterative method uses the current estimate of the solution to generate a new estimate closer to the actual answer. This process continues until the solution converges to a desired accuracy level, typically measured by a tolerance parameter.

The basic strategy of an iterative method is shown in {prf:ref}`obs-basic-iterative-method-outline`:

<!-- 
These methods work by starting with an initial guess for the solution and then iteratively improving the guess until it converges on the actual answer. Several types of iterative methods can be used to solve linear algebraic equations, including Jacobi’s and the Gauss-Seidel methods. These methods all involve iteratively updating the estimates of the variables in the system of equations until the solution is found.

One of the advantages of iterative methods is that they can be more efficient than direct methods for solving large, sparse systems of linear equations. However, they can also be more sensitive to the initial guess and may require more iterations to converge on the solution. -->

<!-- 
An iterative method takes an initial solution guess, and refines it by substituting
this guess back into into the linear algebraic equations. The guess is then updated over and over again until either we exhaust the number of iterations we can take, or we converge to a solution. Two key iterative methods are: [Jacobi Iteration](https://en.wikipedia.org/wiki/Jacobi_method) and
[Gauss-Seidel](https://en.wikipedia.org/wiki/Gauss–Seidel_method). The central difference between these two methods is how they update the best estimate of a solution at any given iteration.  -->

````{prf:observation} Basic iterative strategy
:label: obs-basic-iterative-method-outline
Suppose we have a square system of linear algebraic equations:

```{math}
\mathbf{A}\mathbf{x} = \mathbf{b}
```

where $\mathbf{A}\in\mathbb{R}^{n\times{n}}$ is the system matrix, $\mathbf{x}\in\mathbb{R}^{n\times{1}}$ is a vector of unknowns, and $\mathbf{b}\in\mathbb{R}^{n\times{1}}$ is the right-hand-side vector. Starting with equation $i$:

$$\sum_{j = 1}^{n}a_{ij}x_{j} = b_{i}\qquad{i=1,2,\cdots{n}}$$

we solve for an estimate of the ith unknown value $\hat{x}_{i}$:

$$\hat{x}_{i}=\frac{1}{a_{ii}}\bigl(b_{i}-\sum_{j=1,i}^{n}a_{ij}x_{j}\bigr)\qquad{i=1,2,\cdots{n}}$$

where $\sum_{j=1,i}^{n}$ does not include index $i$. What we do with the estimated value of $\hat{x}_{i}$ is the difference between Jacobi's and Gauss-Seidel’s methods.
````

Iterative solution methods are used to solve large systems of linear equations where direct methods (such as Gaussian elimination) are impractical due to their computational cost.


(content:references:jacobi-iterative-method)=
#### Jacobi's method
Jacobi's method __batch updates__ the estimate of $x_{i}$ at the _end_ of each iteration. Let the estimate of the value of $x_{i}$ at iteration k be $\hat{x}_{i,k}$. Then, the solution estimate at the next iteration $\hat{x}_{i,k+1}$ is given by:

```{math}
:label: eqn-jacobi-iteration
\hat{x}_{i,k+1}=\frac{1}{a_{ii}}\bigl(b_{i}-\sum_{j=1,i}^{n}a_{ij}\hat{x}_{j,k}\bigr)\qquad{i=1,2,\cdots,n}
```

In the Jacobi method, the estimate for all variables from the previous iteration is used, and we wait to update the solution until we have processed all $i=1,2,\cdots,n$ equations. We continue to iterate until the change in the estimated solution does not change, i.e., the _distance_ between the solution estimated at $k$ and $k+1$ is below some specified tolerance. 

Let's look at the pseudo-code for Jacobi's method ({prf:ref}`algo-jacobi-iteration`):

````{prf:algorithm} Jacobi iteration
:class: dropdown
:label: algo-jacobi-iteration

**Input**: 
Matrix $\mathbf{A}$, the vector $\mathbf{b}$, guess $\mathbf{x}_{o}$, tolerance $\epsilon$, maximum iterations $\mathcal{M}_{\infty}$.

**Output**: solution $\hat{\mathbf{x}}$

**Initialize**:
1. set $n\leftarrow$length($\mathbf{b}$)
1. set $\hat{\mathbf{x}}\leftarrow\mathbf{x}_{o}$

**Main**
1. for $i\in{1}\dots\mathcal{M}_{\infty}$
    1. set $\mathbf{x}^{\prime}\leftarrow\text{zeros}(n,1)$
    1. for $j\in{1}\dots{n}$
        1. set $s\leftarrow{0}$
        1. for $k\in{1}\dots{n}$
            1. if $k\neq{j}$
                1. set $s\leftarrow{s} + a_{ik}\times\hat{x}_{k}$
        1. set $x^{\prime}_{j}\leftarrow{a^{-1}_{jj}}\times\left(b_{j} - s\right)$
    1. if $||\mathbf{x}^{\prime} - \hat{\mathbf{x}}|| < \epsilon$
        1. return $\hat{\mathbf{x}}$
    1. else 
        1. set $\hat{\mathbf{x}}\leftarrow\mathbf{x}^{\prime}$
1. return $\hat{\mathbf{x}}$
````

<!-- the Gauss-Seidel method works by iteratively improving an initial guess for the solution to the system of equations. At each iteration, the method updates the values of the variables using the current estimates for the other variables.  -->

(content:references:gauss-seidel-method)=
#### Gauss-Seidel method
The Gauss-Seidel method __live updates__ the best estimate of $\hat{x}_{i}$ _during_ the processing of equations $i=1,\cdots,n$. The Gauss-Seidel update procedure generally leads to better convergence properties than the Jacobi method. Let the estimate for variable $i$ at iteration $k$ be $\hat{x}_{i,k}$. Then, the solution estimate at the next iteration $\hat{x}_{i,k+1}$ is given by:

```{math}
:label: eqn-gauss-seidel-iteration
\hat{x}_{i,k+1}=\frac{1}{a_{ii}}\bigl(b_{i}-\sum_{j=1}^{i-1}a_{ij}\hat{x}_{j,k+1}-\sum_{j=i+1}^{n}a_{ij}\hat{x}_{j,k}\bigr)\qquad{i=1,2,\cdots,n}
```

We continue to iterate until the change in the estimated solution does not change, i.e., the _distance_ between the solution estimated at $k$ and $k+1$ is below some specified tolerance. 

Let's look at a pseudo-code for the Gauss-Seidel method ({prf:ref}`algo-gauss-seidel-method`):

````{prf:algorithm} Gauss-Seidel method
:class: dropdown
:label: algo-gauss-seidel-method

**Input**: 
Matrix $\mathbf{A}$, the vector $\mathbf{b}$, guess $\mathbf{x}_{o}$, tolerance $\epsilon$, maximum iterations $\mathcal{M}_{\infty}$.

**Output**: solution $\hat{\mathbf{x}}$, converged flag

**Initialize**:
1. set $n\leftarrow$length($\mathbf{b}$)
1. set $\hat{\mathbf{x}}\leftarrow\mathbf{x}_{o}$
1. set converged $\leftarrow$ false

**Main**
1. for $i\in{1}\dots\mathcal{M}_{\infty}$
    1. set $\mathbf{x}^{\prime}\leftarrow\text{zeros}(n,1)$
    1. for $j\in{1}\dots{n}$
        1. compute new value $x_{j}^{\prime}$
    
    1. if $||\mathbf{x}^{\prime} - \hat{\mathbf{x}}|| < \epsilon$
        1. set converged $\leftarrow$ true
        1. break
    
    1. set $\hat{\mathbf{x}}\leftarrow\mathbf{x}^{\prime}$

1. return $\hat{\mathbf{x}}$, converged
````

#### Successive Overrelaxation
Successive Overrelaxation methods (SORMs) are modified versions of Gauss-Seidel, where the best estimate of $x_{i}$ is further modified
before proceeding to the evaluation of the next equations. Suppose we define the best estimate
for the value of $x_{i}$ at iteration k as $\hat{x}_{i,k}$. Then _before_ processing the next Gauss-Seidel type step we update the best guess for $x_{i}$ using the rule:

$$\hat{x}_{i,k}=\lambda\hat{x}_{i,k}+\left(1-\lambda\right)\hat{x}_{i,k-1}\qquad{i=1,2,\cdots,n}$$

where $\lambda$ is an adjustable parameter governed by $0<\lambda\leq{2}$. The value of $\lambda$ can dramatically speed-up *or*
slow-down the convergence to the correct solution, so care should be taken when choosing it. 
The value of $\lambda$ can be in one of three regimes:

* Underrelaxation $0<\lambda<1$: Can help solve systems that have problematic convergence
* Nominal $\lambda=1$: Regular Gauss-Seidel algorithm
* Overrelaxation $1<\lambda\leq{2}$: This regime will increase the rate of convergence provided the algorithm can converge

#### Convergence of iterative methods
A sufficient condition for the convergence of Jacobi or Gauss-Seidel Iteration is the well known diagonal dominance condition:

$$\sum_{j=1,i}^{n}\lvert{a_{ij}}\rvert<\lvert{a_{ii}}\rvert\qquad\forall{i}$$

Diagonal dominance is a sufficient (but not necessary) condition for convergence of an iterative method. Of course, this condition says
nothing regarding the rate of convergence. Convergence, which is a measure of the distance between the current
best solution and the true solution, can be calculated using any one of several metrics. Two common techniques are the calculation of the
percentage change in the solution as well as the measurement in a least-squares sense of the distance between the solutions. 
While the true solution is not explicitly known, you can calculate the squared difference between the known right-hand side vector, $\mathbf{b}$, and your current best estimate, $\mathbf{\hat{b}}_{k}$:

$$e_{k}=\left(\mathbf{b}-\mathbf{\hat{b}}_{k}\right)\left(\mathbf{b}-\mathbf{\hat{b}}_{k}\right)^{T}$$

where $e_{k}$ denotes the squared error for solution at iteration k of the algorithm. 

---

## Summary
This lecture introduced vectors, matrices, and operations defined on these objects:
* {ref}`content:references:matrix-vector` are one- and two-dimensional arrays of numbers. Vectors represent quantities with both magnitude and direction, such as displacement, velocity, and acceleration. They can also describe points in space or as coefficients in linear equations. Matrices are represented as a grid of numbers, with each matrix element represented by a different cell in the grid. Matrices are often used to describe linear transformations, such as rotations and scaling operations, as well as to represent systems of linear equations. They can also represent data sets, with each row representing a different data point and each column representing a distinct feature.

## Additonal resources
* The matrix vector and matrix $\times$ matrix product figures were inspired [Visualizing Matrix Multiplication as a Linear Combination, Eli Bendersky, Apr. 12, 15 · Big Data Zone](https://dzone.com/articles/visualizing-matrix)