# Vectors, Matrices, Measurements and Distances

## Introduction
This lecture will introduce Vectors, Matrices, and some operations defined on these objects. Vectors and matrices are an essential part of [linear algebra](https://en.wikipedia.org/wiki/Linear_algebra), a branch of mathematics that deals with linear systems of equations and transformations.  Vectors and matrices are widely used in computer science, engineering, and other fields where mathematical modeling is important.

* __Vectors__: Vectors often represent quantities with both magnitude and direction, such as displacement, velocity, and acceleration. They can also describe points in space or as coefficients in linear equations.
* __Matrix__: A matrix is a two-dimensional array of numbers. Matrices are typically represented as a grid of numbers, with each matrix element represented by a different cell in the grid. Matrices are often used to represent linear transformations, such as rotations and scaling operations, as well as to represent systems of linear equations. They can also represent and describe data sets, with each row representing a different data point and each column representing a distinct feature.

---

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


## Determinant and Trace
The [determinant of a matrix](https://en.wikipedia.org/wiki/Determinant) is a scalar value that can be computed from a square matrix. [Determinants](https://en.wikipedia.org/wiki/Determinant) can be used to determine whether a [matrix is invertible](./laes.md), and they also have applications in solving systems of linear equations and calculating volume changes in linear transformations. The [determinant of square matrix](https://en.wikipedia.org/wiki/Determinant) $\mathbf{A}$ is defined as ({prf:ref}`defn-det-A`):

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
````

The _matrix-matrix multiplication_ operation can be represented graphically ({numref}`fig-multiplication-matrix-matrix`):

```{figure} ./figs/Fig-AB-Matrix-Matrix-Multiplication.pdf
---
height: 360px
name: fig-multiplication-matrix-matrix
---
Caption goes here
```

A psuedo code implemetation of {prf:ref}`defn-matrix-matrix-product` is given in {prf:ref}`algo-matrix-matrix-code`:

````{prf:algorithm} Naive Matrix $\times$ Matrix multiplication
:class: dropdown
:label: algo-matrix-matrix-code

**Inputs** Matrix $\mathbf{A}$, and matrix $\mathbf{B}$

**Outputs** Matrix $\mathbf{C} = \mathbf{A}\times\mathbf{B}$.

**Initialize**

1. initialize (rowsA, colsA) $\leftarrow$ size($\mathbf{A}$)
2. initialize (rowsB, colsB) $\leftarrow$ size($\mathbf{B}$)
3. initialize matrix $\mathbf{C}~\leftarrow$ zeros(rowsA, colsB)

**Check**
1. if colsA $\neq$ rowsB
    1. throw error $\leftarrow$ Matrix $\mathbf{A}$ and Matrix $\mathbf{B}$ cannot be multiplied

**Main**
1. for i $\in$ 1 to rowsA:
    1. for j $\in$ 1 to colsB:
        1. for k $\in$ 1 to colsA:
            1. $C[i,j]~\leftarrow~C[i,j] + A[i,k]\times{B[k,j]}$

**Return** matrix $\mathbf{C}$
````

##### Matrix-Matrix properties
Matrix-matrix products have different properties compared with the product of two scalar numbers.

* Non-commutativity: In general, matrix multiplication is not commutative, e.g., $\mathbf{A}\mathbf{B}\neq\mathbf{B}\mathbf{A}$; thus the order of multiplication matters (unlike multiplying two scalar numbers together). 
* Distributivity: Matrix products are distributive, i.e., $\mathbf{A}\left(\mathbf{B}+\mathbf{C}\right) = \mathbf{A}\mathbf{B}+\mathbf{A}\mathbf{C}$.
* Associative: Matrix products are associative, i.e., $\mathbf{A}\left(\mathbf{B}\mathbf{C}\right) = \left(\mathbf{A}\mathbf{B}\right)\mathbf{C}$.
* Transpose: The transpose of a matrix product is the product of transposes, i.e., $\left(\mathbf{A}\mathbf{B}\right)^{T} = \mathbf{B}^{T}\mathbf{A}^{T}$.

---

## Measurements and Distances

### Vector and matrix norms
A [norm](https://en.wikipedia.org/wiki/Norm_(mathematics)) is a function that measures the length of vectors or matrices. The notion of length is handy because it enables us to define distance, i.e., similarity between vectors (or matrices) in applications such as machine learning. 

#### Properties of vector norms
A vector norm is any function $||\star||:\mathbb{R}^{n}\rightarrow\mathbb{R}$ such that following properties are true:

* Non-negativity: $||\mathbf{x}||\geq{0}$ for any vector $\mathbf{x}\in\mathbb{R}^{n}$
* Multiplication by a scalar: $||\alpha\mathbf{x}|| = \alpha{||\mathbf{x}||}$ for any vector $\mathbf{x}\in\mathbb{R}^{n}$ and $\alpha\in\mathbb{R}$.
* Triangle inequality: $||\mathbf{x}+\mathbf{x}||\leq||\mathbf{x}||+||\mathbf{x}||$ for any vectors $\mathbf{x},\mathbf{y}\in\mathbb{R}^{n}$

````{prf:definition} p-norm
:label: defn-vector-p-norm
Arguably, the most commonly used vector norms belong to the family of $p$-norms (also called $l_{p}$-norms) which is defined as:

```{math}
:label: eqn-p-norm-defn
||\mathbf{x}||_{p} = \left(\sum_{i=1}^{n}|x_{i}|^{p}\right)^{1/p}
```

for any $p>0$.

````

#### Matrix norms
A matrix norm is any function $||\star||:\mathbb{R}^{m\times{n}}\rightarrow\mathbb{R}$ such that following properties are true:

* Non-negativity: $||\mathbf{A}||\geq{0}$ for any matrix $\mathbf{A}\in\mathbb{R}^{m\times{n}}$ and $||\mathbf{A}||=0$ if and only if $\mathbf{A}=0$.
* Multiplication by a scalar: $||\alpha\mathbf{A}|| = \alpha{||\mathbf{A}||}$ for any $m\times{n}$ matrix $\mathbf{A}\in\mathbb{R}^{m\times{n}}$ and $\alpha\in\mathbb{R}$.
* Triangle inequality: $||\mathbf{A}+\mathbf{B}||\leq||\mathbf{A}||+||\mathbf{B}||$ for any matrices $\mathbf{A},\mathbf{B}\in\mathbb{R}^{m\times{n}}$

````{prf:definition} Properties of Matrix Norms
:label: defn-compatible-matrix-vector-norm

A matrix norm $||\cdot||$ is _consistent_ with a vector norm $||\cdot||$ if:

```{math}
:label: eqn-consistent-matrix-norm
||\mathbf{A}\mathbf{x}||\leq||\mathbf{A}||\cdot||\mathbf{x}||
```

Further, a matrix norm $||\cdot||$ is _sub-multiplicative_ if $\forall\mathbf{A}\in\mathbb{R}^{m\times{n}}$ and 
$\forall\mathbf{B}\in\mathbb{R}^{n\times{q}}$ the inequality holds:

```{math}
:label: eqn-submul-norm
||\mathbf{A}\mathbf{B}||\leq||\mathbf{A}||\cdot||\mathbf{B}||
```

````

## Dimensionality reduction
Dimensionality reduction systematically reduces the number of variables in a dataset while preserving as much of the information in the data as possible. It simplifies data, removes noise, and makes patterns in the data more visible. Dimensionality reduction can help visualize data, improve machine learning algorithms’ performance, and reduce the storage and computational requirements of working with large datasets. 

There are many techniques for dimensionality reduction, including [principal component analysis](https://en.wikipedia.org/wiki/Principal_component_analysis) and [singular value decomposition](https://en.wikipedia.org/wiki/Singular_value_decomposition), which are both based on computing eigenvalues and eigenvectors, and other approaches, such as clustering and [t-distributed stochastic neighbor embedding (t-SNE)](https://en.wikipedia.org/wiki/T-distributed_stochastic_neighbor_embedding), which are based upon minimizing some distance measure.  


### Eigenvalue-eigenvector problems
Eigenvalue-eigenvector problems are a type of mathematical problem that involves finding a set of eigenvalues and eigenvectors for a matrix. An eigenvalue is a scalar value that satisfies the equation:

```{math}
:label: eqn-eigenvalue-eigenvector-problem
\mathbf{A}\mathbf{v} = \lambda\mathbf{v}
```

where $\mathbf{A}$ is a $n\times{n}$ square matrix, $\mathbf{v}$ is a $n\times{1}$ column vector (also called an eigenvector), and $\lambda$ is a scalar (also called an eigenvalue). Eigenvectors of a matrix are the vectors that, when multiplied by the matrix, are scaled by a factor of the eigenvalue. Eigenvalues and eigenvectors are used to represent the behavior of linear transformations, such as rotation, scaling, and reflection. 

Eigenvalues and eigenvectors are used in many areas of mathematics and physics, including image compression, and data reduction approaches such as [principal component analysis](https://en.wikipedia.org/wiki/Principal_component_analysis), and [singular value decomposition](https://en.wikipedia.org/wiki/Singular_value_decomposition).


---

## Summary
Fill me in. 

## Additonal resources
* The matrix vector and matrix $\times$ matrix product figures were inspired [Visualizing Matrix Multiplication as a Linear Combination, Eli Bendersky, Apr. 12, 15 · Big Data Zone](https://dzone.com/articles/visualizing-matrix)