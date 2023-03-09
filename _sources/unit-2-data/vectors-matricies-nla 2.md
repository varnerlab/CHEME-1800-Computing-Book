# Vectors, Matrices, Measurements and Distances

## Introduction
Vectors and matrices are widely used in computer science, engineering, and other fields where mathematical modeling is essential. 
This lecture will introduce Vectors, Matrices, and some operations defined on these objects:

### Objects
* {ref}`content:references:matrix-vector` are one- and two-dimensional arrays of objects, typically numbers. Vectors often describe points in space or encode coefficients of linear equations. On the other hand, matrices typically represented data sets or grids of numbers, with each element represents a different cell in the grid. Matrices also describe linear transformations, such as rotations and scaling operations, and represent systems of linear equations. 

### Solution of systems of linear algebraic equations




---

(content:references:matrix-vector)=
## Matricies and vectors
Matrices are two-dimensional rectangular arrays of numbers, widgets, etc with $m$ rows and $n$ columns:

$$\mathbf{A} = 
\begin{pmatrix}
a_{1,1} & a_{1,2} & \cdots & a_{1,n} \\
a_{2,1} & a_{2,2} & \cdots & a_{2,n} \\
\vdots  & \vdots  & \ddots & \vdots  \\
a_{m,1} & a_{m,2} & \cdots & a_{m,n} 
\end{pmatrix}$$

where $a_{ij}$ denotes the _element_ of the matrix $\mathbf{A}$ that lives on the $i$th row and $j$th col. By convention, the row index is always the first subscript while the column index is always listed second. 

```{prf:observation} Matrix shape
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

while a $n\times{1}$ dimensional _row_ vector $\mathbf{a}$ is given by:

$$\mathbf{a} = 
\begin{pmatrix}
a_{1} & a_{2} & \cdots & a_{n}
\end{pmatrix}$$

Just like numbers, vectors and matrices can participate in typical mathematical operations, such as addition, subtraction and multiplication, with some small differences. For example, the division operation has a much different meaning for Matrices when compared to numbers. 

### Special matrices
Special matrices have specific properties or characteristics that make them useful in certain mathematical operations or applications. Some examples of special matrices include identity matrices, diagonal matrices, triangular matrices, and orthogonal matrices. These matrices have specific properties that make them useful in linear algebra, optimization, and other fields. 

Let’s discuss a few of these special matrices: {ref}`content:references:diag-id-matrix`, {ref}`content:references:triag-matrix` and {ref}`content:references:orth-matrix`

(content:references:diag-id-matrix)=
#### Diagonal and identity matrices 
A diagonal matrix is a square matrix in which all entries outside the main diagonal are zero. Diagonal matrices are used in matrix algebra to simplify calculations, and they can be created by multiplying an identity matrix by a scalar. The identity matrix is a square matrix with `1` on the diagonal and `0` everywhere else. It represents the identity transformation in linear algebra and is often denoted by the symbol $\mathbf{I}$.


(content:references:triag-matrix)=
#### Triangular matrices
A triangular matrix is a square matrix that is either upper or lower triangular ({prf:ref}`defn-triangular-matrices`):

````{prf:definition} Triangular matrices
:label: defn-triangular-matrices
Triangular matrices are square arrays with zero entries below or above the main diagonal. An $n\times{n}$ lower triangular matrix $\mathbf{L}$:

```{math}
\mathbf{L} = 
\begin{pmatrix}
l_{11} & 0 & \dots & 0 \\
l_{21} & l_{22} & \dots & 0 \\
\vdots & \vdots & & \vdots \\
l_{n1} & l_{n2} & \dots & l_{nn}
\end{pmatrix}
```

has entries $l_{ij} = 0$ for $i<j$ (above the diagonal). On the other hand, an $n\times{n}$ upper triangular matrix $\mathbf{U}$:

```{math}
\mathbf{U} = 
\begin{pmatrix}
u_{11} & u_{12} & \dots & u_{nn} \\
0 & u_{22} & \dots & u_{2n} \\
\vdots & \vdots & & \vdots \\
0 & 0 & \dots & u_{nn}
\end{pmatrix}
```

has entries $u_{ij} = 0$ for $i>j$ (below the diagonal).
````

Triangular matrices are useful in various mathematical and computational contexts. For example, they are easy to invert, can be used to represent linear transformations conveniently, and can be used to solve systems of linear algebraic equations efficiently.

(content:references:orth-matrix)=
#### Orthogonal matrices
Orthogonal matrices are square matrices whose rows and columns are mutually orthogonal and have unit lengths ({prf:ref}`defn-orthogonal-matrix`): 

````{prf:definition} Orthogonal matrices
:label: defn-orthogonal-matrix

An $n\times{n}$ real matrix $\mathbf{A}$ is an orthogonal matrix if:

```{math}
:label: eqn-orthogonal-matrices
\mathbf{A}^{T}\mathbf{A} = \mathbf{A}\mathbf{A}^{T} = \mathbf{I}
```

where $\mathbf{A}^{T}$ denotes the [transpose](https://en.wikipedia.org/wiki/Transpose) of the matrix $\mathbf{A}$, and $\mathbf{I}$ denotes the $n\times{n}$ identity matrix. 
````
Orthogonal matrices are used in various fields, such as linear algebra, physics, computer graphics, and statistics.

(content:determinant-trace)=
## Determinant and Trace
The [determinant of a matrix](https://en.wikipedia.org/wiki/Determinant) is a scalar value that can be computed from a square matrix. [Determinants](https://en.wikipedia.org/wiki/Determinant) can be used to determine whether a matrix is invertible, and they also have applications in solving systems of linear equations and calculating volume changes in linear transformations. The [determinant of square matrix](https://en.wikipedia.org/wiki/Determinant) $\mathbf{A}$ is defined as ({prf:ref}`defn-det-A`):

````{prf:definition} Determinant
:label: defn-det-A

Consider an $n\times{n}$ matrix $\mathbf{A}$, where $a_{ij}$ is the entry of $\mathbf{A}$ on row $i$ and column $j$. 
Then, the determinant of the square matrix $\mathbf{A}$, denoted as $\det\left(\mathbf{A}\right)$, is given by:

```{math}
:label: eqn-det-A
\det\left(\mathbf{A}\right) = \sum_{\sigma\in{S_{n}}}\text{sign}\left(\sigma\right)\prod_{i=1}^{n}a_{i\sigma_{i}}
```

where $S_{n}$ denotes the Symmtery group of dimension $n$, i.e., the set of all possible permutations of the set ${1,2,\dots,n}$,
$\text{sign}\left(\sigma\right)$ equals `+1` if the permutation can be obtained with an even number of exchanges; otherwise `-1`. 
Finally, $a_{i\sigma_{i}}$ denotes the entry of the matrix $\mathbf{A}$ on row $i$, and column $\sigma_{i}$.
````

Although the determinant in the general case, as described in {prf:ref}`defn-det-A`, is difficult to calculate, the determinat of triangular matrices is easy to compute:

````{prf:observation} Determinant triangular matrix
:label: obs-determinant-triangular-matrix

The determinant of a triangular matrix is equal to the product of its diagonal elements. This is true for both upper triangular $\mathbf{U}$ and lower $\mathbf{L}$ triangular matrices. For the upper triangular $n\times{n}$ matrix $\mathbf{U}$, the determinant is equal to:

```{math}
\det\left(\mathbf{U}\right) = \prod_{i=1}^{n}u_{ii}
```

while the determinant of the $n\times{n}$ lower triangular matrix $\mathbf{L}$  is given by:

```{math}
\det\left(\mathbf{L}\right) = \prod_{i=1}^{n}l_{ii}
```

__Idea__: One potential strategy to efficiently compute a determinant is perhaps to convert the general $n\times{n}$ matrix $\mathbf{A}$ to a triangular form (by some theoretical approach). However, will the determinant of the original matrix $\mathbf{A}$ and its triangular be the same? We'll need to wait and see. 
````
 
### Trace
On the other hand, the trace of a matrix is the sum of the diagonal elements of a square matrix ({prf:ref}`defn-trace-A`):

````{prf:definition} Trace
:label: defn-trace-A

Consider an $n\times{n}$ square matrix $\mathbf{A}$, where we denote the $a_{ij}$ is the entry of $\mathbf{A}$ on row $i$ and column $j$. Then, the trace of the square matrix $\mathbf{A}$, denoted as $\text{tr}\left(\mathbf{A}\right)$, is given by:

```{math}
:label: eqn-trace-A
\text{tr}\left(\mathbf{A}\right) = \sum_{i=1}^{n}a_{ii}
```
````

## Rank and kernel
The [rank of a matrix](https://en.wikipedia.org/wiki/Rank_(linear_algebra)) is a measure of the number of linearly independent rows or columns. The rank of a matrix _r_ of a $m\times{n}$ matrix $\mathbf{A}$ is always less than or equal to the minimum of its number of rows and columns:

```{math}
:label: eqn-rank-inequality
r\leq\min\left(m,n\right)
```

Rank can also be considered a measure of the unique information in a matrix; thus, if there is redundant information (rows or columns that are not linearly independent), a matrix will be less than full rank. 

The kernel of a matrix is the set of all solutions to the homogeneous equation $\mathbf{A}\mathbf{x} = \mathbf{0}$, where $\mathbf{A}$ is the matrix, and $\mathbf{x}$ is a column vector. The dimension of the kernel is equal to the number of columns in the matrix minus its rank. The kernel is also known as the null space of the matrix.

(content:matrix-vector-operations)=
## Matrix and vector operations
Matrix and vector operations are mathematical procedures that include addition, subtraction, scalar multiplication, and matrix multiplication, which can be used to represent and manipulate linear transformations. Understanding these operations is crucial for many areas of science, engineering, and computer science.

### Addition and subtraction
Matrices and vectors can be added and subtracted just like scalars quantities with one important caveat, namely, they need to be compatible. Vectors and matrices must be the _same dimension_ to be compatible with addition or subtraction operations. 

The addition (or subtraction) operations for matrices or vectors, as well as multiplying these objects by a scalar constant (as we shall see), is done element-wise. Thus, if these objects don't have the same number of elements, then addition and subtraction operations don't make sense. 

````{prf:definition} Vector addition
:label: obs-same-dimension
Suppose we have two $m\times{1}$ vectors $\mathbf{a}$ and $\mathbf{b}$. The sum of these vectors $\mathbf{y} = \mathbf{a} + \mathbf{b}$ is the $m\times{1}$ vector $\mathbf{y}$ given by:

```{math}
:label: eqn-vector-addition
y_{i} = a_{i} + b_{i}\qquad{i=1,\dots,m}
```

The vectors $\mathbf{a}$ and $\mathbf{b}$ are compatible if they have the same number of elements. 
````

Vector addition is strarighforward to implement, consider {prf:ref}`algo-vector-addition`:

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

{prf:ref}`algo-vector-addition` may not be the _best_ way to implement vector (or matrix) addition or subtract operations.v Many modern programming languages and libraries, e.g., [the Numpy library in Python](https://numpy.org) or [Julia](https://julialang.org), support _vectorization_, i.e., special operators that encode element-wise addition, subtraction or other types of element-wise operations without the need to write `for` loops. 

Vectorized code typically executes faster than naive implementations such as {prf:ref}`algo-vector-addition` because the _vectorization_ takes advantage of advanced techniques to improve performance. 

You can use the `.+` operator for element-wise addition, while element-wise subtraction can be encoded with the `.-` operator.

### Scalar multiplication
Multiplying a vector (or a matrix) by a constant is also done element wise. 

````{prf:definition} Scalar multiplication
:label: defn-scalar-multiplication
Suppose we have a $n\times{1}$ vector $\mathbf{v}$ and a constant $c$. Then the scalar product between a constant $c$ and the vector $\mathbf{v}$, denoted by $\mathbf{y} = c\mathbf{v}$, is given by:

```{math}
y_{i} = cv_{i}\qquad{i=1,2,\dots,n}
```

where $y_{i}$ and $v_{i}$ denote the ith component of the product vector $\mathbf{y}$, and the input vector $\mathbf{v}$. 

Likewise, suppose we have an $m\times{n}$ matrix $\mathbf{X}$ and constant $c$. Then, the scalar product between a constant $c$ and the matrix $\mathbf{X}$, denoted by $\mathbf{Y} = c\mathbf{X}$, is given by:

```{math}
y_{ij} = cx_{ij}\qquad{i=1,2,\dots,m,~j=1,2,\dots,n}
```

where $y_{ij}$ and $x_{ij}$ denote the ijth component of the product matrix $\mathbf{Y}$, and the input matrix $\mathbf{X}$. 
````

### Vector-vector, matrix-vector and matrix-matrix multiplication

#### Vector-vector multiplication
Inner products: Two compatible vectors $\mathbf{a}$ and $\mathbf{b}$ can be multiplied together to produce a _scalar_ in an operation called an [inner product](https://en.wikipedia.org/wiki/Inner_product_space): 

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

This operation is possible if the vectors $\mathbf{a}$ and $\mathbf{b}$ have the same number of elements.
````

Outer product: Suppose you have an $m\times{1}$ vector $\mathbf{a}$, and an $n\times{1}$ vector $\mathbf{b}$. The vectors $\mathbf{a}$ and $\mathbf{b}$ can be multipled together to form a $m\times{n}$ matrix through an [outer product](https://en.wikipedia.org/wiki/Outer_product):


````{prf:definition} Outer product
:label: defn-vector-vector-multiplication-op

Let $\mathbf{a}\in\mathbb{R}^{m\times{1}}$ and $\mathbf{b}\in\mathbb{R}^{n\times{1}}$. Then, the _vector-vector outer product_ given by:

```{math}
:label: eqn-vector-vector-outer-product
\mathbf{Y} = \mathbf{a}\otimes\mathbf{b}
```

produces the $m\times{n}$ matrix $\mathbf{Y}\in\mathbb{R}^{m\times{n}}$ with elements:

```{math}
:label: eqn-vector-outer-product-index-form
y_{ij} = a_{i}b_{j}\qquad{i=1,2,\dots,m~\text{and}~j=1,2,\dots,n}
```

The outer product operation is equivalent to $\mathbf{Y} = \mathbf{a}\mathbf{b}^{T}$.

````

#### Right multiplication of a matrix by a vector
A common operation is _right matrix-vector multiplication_ of a matrix $\mathbf{A}$ by a vector $\mathbf{x}$. If the matrix $\mathbf{A}$ and the vector $\mathbf{x}$ are compatible,  _right matrix-vector multiplication_ will produce a vector with the same number of rows as the original matrix $\mathbf{A}$:

````{prf:definition} Right matrix-vector multiplication
:label: defn-right-matrix-vector-multiplication

Let $\mathbf{A}\in\mathbb{R}^{m\times{n}}$ and $\mathbf{x}\in\mathbb{R}^{n\times{1}}$. Then, the _right matrix-vector product_ given by:

$$\mathbf{y} = \mathbf{A}\mathbf{x}$$

generates $\mathbf{y}\in\mathbb{R}^{m\times{1}}$ where the $i$th element is given by:

$$y_{i} = \sum_{j=1}^{n}a_{ij}x_{j}\qquad{i=1,2,\cdots,m}$$

This operation is possible if the number of columns of the matrix $\mathbf{A}$ equals the number of rows of the vector $\mathbf{x}$.
````

The _right multiplication operation_ can be represented graphically as a series of scalar$\times$vector multiplication and vector-vector summation operations ({numref}`fig-right-multiplication-matrix-vector`):

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
We could also consider the _left multiplication_ of a matrix by a vector. Suppose $\mathbf{A}$ is an $m\times{n}$ matrix, and $\mathbf{x}$ is a $m\times{1}$ vector, then the _left matrix-vector product_ is a row vector:

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
Many of the important uses of matrices in engineering practice depend upon the definition of matrix multiplication.
 
````{prf:definition} Matrix-matrix product
:label: defn-matrix-matrix-product

Let $\mathbf{A}\in\mathbb{R}^{m\times{n}}$ and $\mathbf{B}\in\mathbb{R}^{n\times{p}}$. The matrix-matrix product $\mathbf{C} = \mathbf{A}\times\mathbf{B}$ produces the matrix $\mathbf{C}\in\mathbb{R}^{m\times{p}}$ with elements:

```{math}
:label: eqn-matrix-matrix-product-elements
c_{ij} = \sum_{k=1}^{n}a_{ik}b_{kj}\qquad{i=1,2,\cdots,m~\text{and}~j=1,2,\cdots,p}
```

The matrices $\mathbf{A}\in\mathbb{R}^{m\times{n}}$ and $\mathbf{B}\in\mathbb{R}^{n\times{p}}$ are compatible if the number of columns of $\mathbf{A}\in\mathbb{R}^{m\times{n}}$ equals the number of rows of $\mathbf{B}\in\mathbb{R}^{n\times{p}}$. Otherwise, the matrices are incompatible and cannot be multiplied. 

The runtime of matrix multiplication is $\mathcal{O}(n^3)$, where $n$ is the dimension of the matrices being multiplied. 
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
* Non-commutativity: Matrix multiplication is typically not commutative, e.g., $\mathbf{A}\mathbf{B}\neq\mathbf{B}\mathbf{A}$.
* Distributivity: Matrix products are distributive, i.e., $\mathbf{A}\left(\mathbf{B}+\mathbf{C}\right) = \mathbf{A}\mathbf{B}+\mathbf{A}\mathbf{C}$.
* Associative: Matrix products are associative, i.e., $\mathbf{A}\left(\mathbf{B}\mathbf{C}\right) = \left(\mathbf{A}\mathbf{B}\right)\mathbf{C}$.
* Transpose: The transpose of a matrix product is the product of transposes, i.e., $\left(\mathbf{A}\mathbf{B}\right)^{T} = \mathbf{B}^{T}\mathbf{A}^{T}$.

---

## Solution of systems or linear algebraic equations
Systems of Linear Algebraic Equations (LAEs) arise in many different Engineering fields. In Chemical Engineering, these equations naturally arise from steady-state materials balances, as we have seen. In this lecture, we'll explore both _direct_ and _iterative_ methods to solve the generic system of LAEs:

```{math}
:label: eqn-general-system-laes
\mathbf{A}\mathbf{x} = \mathbf{b}
```

where $\mathbf{A}$ denotes a $m\times{n}$ matrix, $\mathbf{x}$ denotes a $n\times{1}$ column vector of unknowns (what we want to solve for), and $\mathbf{b}$ represents a $m\times{1}$ vector. Let's consider two cases:

* __Homogenous system__: A homogeneous system of linear algebraic equations is a system in which the right-hand side vector $\mathbf{b}=\mathbf{0}$; every entry in the $\mathbf{b}$-vector is equal to zero. The solution set for a homogeneous system is a subspace in linear algebra, which contains all the solutions that can be added to get new solutions.

* __Non-homogeneous system__: A non-homogeneous system of linear algebraic equations is a system in which at least one entry of the right-hand side vector $\mathbf{b}$ is non-zero.

## Existence
The existence of a solution to a system of linear equations depends on the number of equations and the number of variables in the system. Square systems will have at least one solution, which means that there will be values of the variables that can be found that will make all of the equations in the system true simultaneously.

### Homogeneous square systems
For a homogeneous system of linear equations, the trivial solution $\mathbf{x}=\mathbf{0}$ will always exist, but non-trivial solutions may not be unique. For example, there could be infinitely many $\mathbf{x}$ values that simultaneously make the system of equations true.

A homogeneous square system of linear algebraic equations with $n\times{n}$ coefficient matrix $\mathbf{A}$ and unknown vector $\mathbf{x}$:

```{math}
:label: eqn-homogenous-laes
\mathbf{A}\mathbf{x} = \mathbf{0}
```

may or may not have a unique solution. However, there is an easy check to determine the existence of a solution to Eqn. {eq}`eqn-homogenous-laes`:

````{prf:definition} Homogenous solution existence
:label: defn-homogenous-soln-existence

A homogeneous square system of linear algebraic equations with $n\times{n}$ coefficient matrix $\mathbf{A}$ and unknown vector $\mathbf{x}$
has a unique solution if and only if the coefficient matrix $\mathbf{A}$ has determinant:

```{math}
:label: eqn-det-homogenous-cond
\det\left(\mathbf{A}\right) = 0
```

The determinant condition is an easy theoretical test to check for the existence of a unique solution to a homogenous system of linear algebraic equations. 
````

In practice computing the determinant directly can be computationally expensive. Alternatively, the determinant condition can be also checked by computing the [rank](https://en.wikipedia.org/wiki/Rank_(linear_algebra)) of matrix $\mathbf{A}$.  If a matrix is less than full rank, then $\det{\left(\mathbf{A}\right)}=0$. 

There are many different ways to compute rank, however, we'll use the [rank](https://docs.julialang.org/en/v1/stdlib/LinearAlgebra/#LinearAlgebra.rank) function in [Julia](https://julialang.org).

### Non-homogeneous square systems
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

where the $\mathbf{A}^{-1}$ is called the inverse of the matrix $\mathbf{A}$. However, the inverse may not always exist ({prf:ref}`defn-matrix-inverse`):


````{prf:definition} Matrix inverse existence
:label: defn-matrix-inverse

An $n\times{n}$ square matrix $\mathbf{A}$ has an unique inverse $\mathbf{A}^{-1}$ such that:

```{math}
:label: eqn-matrix-A
\mathbf{A}^{-1}\mathbf{A} = \mathbf{A}\mathbf{A}^{-1} = \mathbf{I}
```

if $\det\left(\mathbf{A}\right)\neq{0}$. If the inverse exists, the matrix $\mathbf{A}$ is called non-singular, otherwise $\mathbf{A}$ is singular. 
````

(content:references:solution-approaches)=
## Solutions
Several methods exist to find the solution of a square system of linear equations:

* {ref}`content:references:gaussian-elimination` solves linear equations by using a sequence of operations to reduce the system to an upper triangular form and then back substitution to find the solution. 
* {ref}`content:references:iterative-methods` are another class of algorithms used to find approximate solutions for large and sparse linear systems of equations. These methods work by starting with an initial guess for the solution and then repeatedly updating the guess until it converges to the actual solution.

(content:references:gaussian-elimination)=
### Gaussian elimination 
[Gaussian elimination](https://en.wikipedia.org/wiki/Gaussian_elimination) is an efficient method for solving large square systems of linear algebraic equations. [Gaussian elimination](https://en.wikipedia.org/wiki/Gaussian_elimination) is based on "eliminating" variables by adding or subtracting equations (rows) so that the coefficients of one variable are eliminated in subsequent equations. This allows us to solve for the remaining variables one at a time until you have a solution for the entire system.

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

Solving a lower or upper triangular system can easily be done with a substitution approach. However, upper (or lower) triangular systems rarely arise naturally in applications. So why do we care about triangular systems? 

#### Row reduction approaches
Converting a matrix $\mathbf{A}$ to an upper triangular form involves performing a sequence of elementary row operations to transform the matrix into a form where all entries below the main diagonal are zero. This is typically done by row swapping, scaling, and adding rows. The goal is to create a matrix where each row’s leading coefficient (the first non-zero element) is `1`, and all the other entries in that column are `0`. This is achieved by using the leading coefficient of one row to eliminate the corresponding entries in the other rows.

Let's walkthrough a simple example to illustrate the steps involved with [row reduction](https://en.wikipedia.org/wiki/Row_echelon_form); consider the solution of the 3$\times$3 system:

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

While the logic underlying the row reduction of the matrix $\mathbf{A}$ to the upper triangular row-echelon form $\mathbf{U}$ seems simple, the implementation of a general, efficient row reduction function can be complicated; let's develop a naive implementation of a row reduction 
procedure ({prf:ref}`algo-ge-basic`):

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
### Iterative methods
Iterative methods are algorithms to estimate approximate solutions to linear algebraic equations. These methods work by starting with an initial guess for the solution and then iteratively improving the guess until it converges on the actual answer. Several types of iterative methods can be used to solve linear algebraic equations, including Jacobi’s and the Gauss-Seidel methods. These methods all involve iteratively updating the estimates of the variables in the system of equations until the solution is found.

One of the advantages of iterative methods is that they can be more efficient than direct methods for solving large, sparse systems of linear equations. However, they can also be more sensitive to the initial guess and may require more iterations to converge on the solution. Let's outline the basic idea of an interative solution method in {prf:ref}`obs-basic-iterative-method-outline`:

<!-- 
An iterative method takes an initial solution guess, and refines it by substituting
this guess back into into the linear algebraic equations. The guess is then updated over and over again until either we exhaust the number of iterations we can take, or we converge to a solution. Two key iterative methods are: [Jacobi Iteration](https://en.wikipedia.org/wiki/Jacobi_method) and
[Gauss-Seidel](https://en.wikipedia.org/wiki/Gauss–Seidel_method). The central difference between these two methods is how they update the best estimate of a solution at any given iteration.  -->

````{prf:observation} Basic iterative method
:label: obs-basic-iterative-method-outline
Suppose we have an $n\times{n}$ system of linear algebraic equations of the form:

```{math}
\mathbf{A}\mathbf{x} = \mathbf{b}
```

where $\mathbf{A}$ denotes a $n\times{n}$ matrix of coefficients, $\mathbf{x}$ denotes the $n\times{1}$ vector of unknowns that we are trying to estimate, and $\mathbf{b}$ denotes the $n\times{1}$ vector of right-hand-side values. 

We start with the ith equation in a system of $n$ equations, written in index form as:

$$\sum_{j = 1}^{n}a_{ij}x_{j} = b_{i}\qquad{i=1,2,\cdots{n}}$$

and solve it for _an estimate_ of the ith unknown:

$$\hat{x}_{i}=\frac{1}{a_{ii}}\bigl(b_{i}-\sum_{j=1,i}^{n}a_{ij}x_{j}\bigr)\qquad{i=1,2,\cdots{n}}$$

denoted by $\hat{x}_{i}$, where $\sum_{j=1,i}^{n}$ does not include index $i$. What we do next, with the estimated value of $\hat{x}_{i}$, is the key difference between Jacobi's method and the Gauss-Seidel method.
````

#### Jacobi's method
Jacobi's method __batch updates__ the estimate of $x_{i}$ at the _end_ of each iteration. Suppose we define the estimate of the value of $x_{i}$ at iteration k as $\hat{x}_{i,k}$. Then, the value of $x_{i}$ at iteration $k+1$ is given by:

$$\hat{x}_{i,k+1}=\frac{1}{a_{ii}}\bigl(b_{i}-\sum_{j=1,i}^{n}a_{ij}\hat{x}_{j,k}\bigr)\qquad{i=1,2,\cdots,n}$$

In the Jacobi method, the estimate for all variables from the previous iteration is used, and we do not update the guess until
we have processed all $i=1,2,\cdots,n$ equations. We continue to iterate until the change in the estimated solution does not change, i.e., the _distance_ between the solution estimated at $k$ and $k+1$ is below some specified tolerance. 

Let's look at psuedo code for Jacobi's method in {prf:ref}`algo-jacobi-iteration`:

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

#### Gauss-Seidel method
The Gauss-Seidel method is an iterative method for solving linear equations. It is a variant of the Gaussian elimination; the Gauss-Seidel method works by iteratively improving an initial guess for the solution to the system of equations. At each iteration, the method updates the values of the variables using the current estimates for the other variables. 

Gauss-Seidel __live updates__ the best estimate of $\hat{x}_{i}$ _during_ the processing of equations $i=1,\cdots,n$. The update procedure of Gauss-Seidel generally leads to better convergence properties than the Jacobi method. Suppose we define the best estimate for variable $i$ at iteration $k$ as $\hat{x}_{i,k}$. Then the value of $x_{i}$ at iteration $k+1$ is given by:

$$\hat{x}_{i,k+1}=\frac{1}{a_{ii}}\bigl(b_{i}-\sum_{j=1}^{i-1}a_{ij}\hat{x}_{j,k+1}-\sum_{j=i+1}^{n}a_{ij}\hat{x}_{j,k}\bigr)\qquad{i=1,2,\cdots,n}$$

We continue to iterate until the change in the estimated solution does not change, i.e., the _distance_ between the solution estimated at $k{\rightarrow}k+1$ is below some specified tolerance $\epsilon$.

Let's look at a psuedo code for the Gauss Seidel method in {prf:ref}`algo-gauss-seidel-method`:

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