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
Matrices and vectors can be added and subtracted just like scalars quantities with one important caveat, namely, they need to be compatible. Vectors and matrices must be the _same dimension_ to be compatible with addition or subtraction operations. 

The addition (or subtraction) operations for matrices or vectors, as well as multiplying these objects by a scalar constant (as we shall see), is done element-wise. Thus, if these objects don't have the same number of elements, then addition and subtraction operations don't make sense. 

````{prf:observation} Vector addition
:label: obs-same-dimension
Suppose we have two $n\times{1}$ vectors $\mathbf{v}_{1}$ and $\mathbf{v}_{2}$. The sum of these vectors is given by:

$$\mathbf{y} = \mathbf{v}_{1} + \mathbf{v}_{2}$$

where the ith element of the sum $\mathbf{y}$ is given by: $v_{i} = v_{i,1}+ v_{i,2}$. If however, $\mathbf{v}_{1}$ had $m>n$ elements, then the vectors $\mathbf{v}_{1}$ and $\mathbf{v}_{2}$ are not compatible. A similar arguement can be made about matrices.
````

Vector addition is strarighforward to implement; for example, consider {prf:ref}`algo-vector-addition`:

````{prf:algorithm} Naive vector addition
:class: dropdown
:label: algo-vector-addition

**Inputs** Compatible vectors $\mathbf{v}_{1}$ and $\mathbf{v}_{2}$

**Outputs** Vector sum $\mathbf{y}$

**Initialize**
1. N $\leftarrow$ length($\mathbf{v}_{1}$)
1. $\mathbf{y}\leftarrow$ zeros(N)

**Main**
1. for i $\in$ 1 to N
    1. $y[i]\leftarrow~v_{1}[i] + v_{2}[i]$

**Return**
$\mathbf{y}$
````

where the braket notation $y[\star]$ denotes element $\star$ of the vector $\mathbf{y}$. However, {prf:ref}`algo-vector-addition` may not be the _best_ way to implement vector (or matrix) addition or subtract operations.

#### Vectorized addition and subtraction operators
Many modern programming languages and libraries, e.g., [the Numpy library in Python](https://numpy.org) or [Julia](https://julialang.org), support _vectorization_, i.e., special operators that encode element-wise addition, subtraction or other types of element-wise operations without the need to write `for` loops. Vectorized code often executes faster than Naive implementation such as {prf:ref}`algo-vector-addition` because the _vectorization_ can take advantage of advanced techniques to improve performance. 

You can use the `.+` operator for element-wise addition, while element-wise subtraction can be encoded with the `.-` operator.

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

```{prf:algorithm} Naive right multiplication of a matrix by a vector
:label: algo-right-multiplication-matrix-vector
:class: dropdown

**Inputs:** Matrix $\mathbf{A}$, and vector $\mathbf{x}$

**Outputs:** Vector $\mathbf{y} = \mathbf{A}\times\mathbf{x}$.

**Initialize**
1. initialize (rowsA, colsA) $\leftarrow$ size($\mathbf{A}$)
1. initialize (rowsx) $\leftarrow$ length($\mathbf{x}$)
1. initialize vector $\mathbf{y}~\leftarrow$ zeros(rowsA)

**Check**
1. if colsA $\neq$ rowsx
    1. throw error $\leftarrow$ Matrix $\mathbf{A}$ and vector $\mathbf{x}$ cannot be multiplied
  
**Main**
1. for i $\in$ 1 to rowsA:
    1. for j $\in$ 1 to colsA:
        1. $y[i]~\leftarrow y[i] + A[i,j]\times{x[j]}$

**Return** vector $\mathbf{y}$
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

```{prf:algorithm} Naive left multiplication of a matrix by a vector
:class: dropdown
:label: algo-left-multiplication-matrix-vector

**Inputs:** Matrix $\mathbf{A}$, and vector $\mathbf{x}$

**Outputs:** Vector $\mathbf{y} = \mathbf{A}\times\mathbf{x}$.

**Initialize**
1. initialize (rowsA, colsA) $\leftarrow$ size($\mathbf{A}$)
1. initialize (rowsx) $\leftarrow$ length($\mathbf{x}$)
1. initialize vector $\mathbf{y}~\leftarrow$ zeros(rowsA)

**Check**
1. if colsA $\neq$ rowsx
    1. throw error $\leftarrow$ Matrix $\mathbf{A}$ and vector $\mathbf{x}$ cannot be multiplied
  
**Main**
1. for i $\in$ 1 to rowsA:
    1. for j $\in$ 1 to colsA:
        1. $y[i]~\leftarrow y[i] + A[i,j]\times{x[j]}$

**Return** vector $\mathbf{y}$
```


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

#### Properties of matrix norms
A matrix norm is any function $||\star||:\mathbb{R}^{m\times{n}}\rightarrow\mathbb{R}$ such that following properties are true:

* Non-negativity: $||\mathbf{A}||\geq{0}$ for any vector $\mathbf{A}\in\mathbb{R}^{m\times{n}}$ and $||\mathbf{A}||=0$ if and only if $\mathbf{A}=0$.
* Multiplication by a scalar: $||\alpha\mathbf{A}|| = \alpha{||\mathbf{A}||}$ for any $m\times{n}$ matrix $\mathbf{A}\in\mathbb{R}^{m\times{n}}$ and $\alpha\in\mathbb{R}$.
* Triangle inequality: $||\mathbf{A}+\mathbf{B}||\leq||\mathbf{A}||+||\mathbf{B}||$ for any matrices $\mathbf{A},\mathbf{B}\in\mathbb{R}^{m\times{n}}$

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




### Clustering and classification
Cluster analysis groups objects such that items in the same group (called a cluster) are more similar (in some sense) to each other than to those in different clusters. Cluster analysis is an essential task of exploratory data analysis and a common technique for statistical data analysis, used in many fields, including pattern recognition, image analysis, information retrieval, bioinformatics, data compression, computer graphics, and machine learning.

k-means clustering is a method of vector quantization, originally taken from signal processing, that aims to partition n observations into k clusters in which each observation belongs to the cluster with the nearest mean (cluster centers or cluster centroid), serving as a prototype of the cluster. 

````{prf:definition} k-means clustering
:label: defn-kmeans-problem

Given a set of observations $\left(\mathbf{x}_{1},\mathbf{x}_{2},\dots,\mathbf{x}_{d}\right)$, where each observation is a $n$-dimensional vector, k-means clustering partitions the $d$-observations into $k\leq{d}$ sets $\mathcal{S} = \left\{\mathcal{S}_{1}, \mathcal{S}_{2},\dots,\mathcal{S}_{k}\right\}$ so as to minimize the within-cluster sum of squares (WCSS):

```{math}
:label: eqn-k-means-clustering
\text{arg}\min_{\mathcal{S}} \sum_{i=1}^{k}\sum_{\mathbf{x}\in\mathcal{S}_{i}}||\mathbf{x}-\mu_{i}||^{2}
```

where $\mu_{i}$ denotes the mean of the points in set $\mathcal{S}_{i}$. 

````

---

## Summary
Fill me in. 

## Additonal resources
* The matrix vector and matrix $\times$ matrix product figures were inspired [Visualizing Matrix Multiplication as a Linear Combination, Eli Bendersky, Apr. 12, 15 · Big Data Zone](https://dzone.com/articles/visualizing-matrix)