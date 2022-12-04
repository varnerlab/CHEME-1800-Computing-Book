# Vectors, matrices, and the introduction of numerical linear algebra

## Introduction
Fill me in. 

## Matricies and vectors
Matrices are two-dimensional rectangular arrays of numbers, widgets, etc with $m$ rows and $n$ columns:

$$\mathbf{A} = 
\begin{pmatrix}
a_{1,1} & a_{1,2} & \cdots & a_{1,n} \\
a_{2,1} & a_{2,2} & \cdots & a_{2,n} \\
\vdots  & \vdots  & \ddots & \vdots  \\
a_{m,1} & a_{m,2} & \cdots & a_{m,n} 
\end{pmatrix}$$

where $a_{ij}$ denotes the _element_ of the matrix $\mathbf{A}$ that lives on the $i$th row and $j$th col. By convention, the row index is always the first subscript while the column index is always listed second. If $m=n$ the matrix is called a _square_ matrix; square Matrices have some special properties (as we shall see later). 

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

## Operations

### Addition and subtraction
Vectors and matrices must be the _same dimension_ to be compatible for addition or subtraction operations. 
This is because adding (or subtracting) Matrices or vectors, as well as multiplying these objects by a scalar constant, is done element wise. 
For example, suppose we have two $n\times{1}$ vectors $\mathbf{v}_{1}$ and $\mathbf{v}_{2}$, then the sum $\mathbf{v}$ is given by:

$$\mathbf{v} = \mathbf{v}_{1} + \mathbf{v}_{2}$$

where the ith element of $\mathbf{v}$ is given by: $v_{i} = v_{i,1}+ v_{i,2}$

### Multiplication

#### Multiplication of a matrix or vector by a constant
Multiplying a matrix (or vector) by a constant is also done element wise. For example, suppose we have a $n\times{1}$ vector $\mathbf{v}$ and a contants _c_. Then the product: 

$$\mathbf{y} = c\mathbf{v}$$

has elements: $y_{i} = cv_{i}$.

#### Right multiplication of a matrix by a vector
A common operation is the _right multiplication_ of a matrix $\mathbf{A}$ by a vector $\mathbf{x}$. 
Suppose $\mathbf{A}$ is a $m\times{n}$ matrix, and $\mathbf{x}$ is a $n\times{1}$ column vector. The _right product_ given by:

$$\mathbf{y} = 
\mathbf{A}\mathbf{x}$$

generates a $m\times{1}$ column vector $\mathbf{y}$, where the $i$th element is given by:

$$y_{i} =
\sum_{j=1}^{n}a_{ij}x_{j}\qquad{i=1,2,\cdots,m}$$

The _right multiplication operation_ can be represented graphically ({numref}`fig-right-multiplication-matrix-vector`):

```{figure} ./figs/Fig-Ab-Multiplication.pdf
---
height: 140px
name: fig-right-multiplication-matrix-vector
---
Caption goes here
```

#### Left multiplication of a matrix by a vector
We could also consider the _left multiplication_ of a matrix by a vector. 
Suppose $\mathbf{A}$ is a $m\times{n}$ matrix, and $\mathbf{x}$ is a $m\times{1}$ col vector, then the _left product_ is given by:

$$\mathbf{y} = 
\mathbf{x}^{T}\mathbf{A}$$

where $\mathbf{x}^{T}$ denotes the [transpose](https://en.wikipedia.org/wiki/Transpose) of the vector $\mathbf{x}$.
The _left product_ generates a $1\times{n}$ row vector with elements:

$$y_{i} = 
\sum_{j=1}^{m}a_{ji}x_{j}\qquad{i=1,2,\cdots,n}$$

The _left multiplication operation_ can be represented graphically ({numref}`fig-left-multiplication-matrix-vector`):

```{figure} ./figs/Fig-bA-Left-Multiplication.pdf
---
height: 160px
name: fig-left-multiplication-matrix-vector
---
Caption goes here
```

#### Matrix-Matrix multiplication
Many of the important uses of matrices in engineering practice depend upon the definition of matrix multiplication.
Matrix-matrix products have different properties compared with the product of two scalar numbers. First, only _compatible_ matrices can be 
multiplied together. For example, consider two matrices $\mathbf{A}$ and $\mathbf{B}$. For $\mathbf{A}$ and $\mathbf{B}$ to be compatible, 
meaning we can compute the matrix product $\mathbf{C} = \mathbf{A}\mathbf{B}$, the number of columns of $\mathbf{A}$ must be same as the number of rows of $\mathbf{B}$. 

Given a matrix $\mathbf{A}$ with _m_ rows and _n_ columns, and a matrix $\mathbf{B}$ with _n_ rows and _p_ columns, the 
product matrix $\mathbf{C}$ is a matrix with _m_ rows and _p_ columns in which the (i,j)th element of $\mathbf{C}$ is given by:

$$c_{ij} = \sum_{k=1}^{n}a_{ik}b_{kj}\qquad{i=1,2,\cdots,m;~j=1,2,\cdots,p}$$

The _matrix-matrix multiplication_ operation can be represented graphically ({numref}`fig-multiplication-matrix-matrix`):

```{figure} ./figs/Fig-AB-Matrix-Matrix-Multiplication.pdf
---
height: 360px
name: fig-multiplication-matrix-matrix
---
Caption goes here
```

In general, matrix multiplication is not communinative e.g., $\mathbf{A}\mathbf{B}\neq\mathbf{B}\mathbf{A}$, thus the order of multiplication matters (unlike multiplying two scalar numbers together). However, while matrix multiplication is not communinative, it is distributive:

$$\mathbf{A}\left(\mathbf{B}+\mathbf{C}\right) = \mathbf{A}\mathbf{B}+\mathbf{A}\mathbf{C}$$

and associative:

$$\mathbf{A}\left(\mathbf{B}\mathbf{C}\right) = \left(\mathbf{A}\mathbf{B}\right)\mathbf{C}$$

## Summary
Fill me in. 

## Additonal resources
* The matrix vector and matrix $\times$ matrix product figures were inspired [Visualizing Matrix Multiplication as a Linear Combination, Eli Bendersky, Apr. 12, 15 · Big Data Zone](https://dzone.com/articles/visualizing-matrix)