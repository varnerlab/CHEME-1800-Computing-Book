# Dimensionality reduction

## Introduction

Measurement and distance tools measure the size of matrix or vector objects and the distances between these objects. We'll explore two types of measurement and distance approaches:

* {ref}`content:measurements-matrix-vector-norms` are mathematical tools to measure the magnitude of matrices and vectors, respectively. They are essential in many areas of mathematics, including linear algebra, optimization, and analysis. 

* {ref}`content:measurements-similarity-funtions` are mathematical tools that quantify the similarity between objects, such as vectors or points. They are widely used in machine learning, pattern recognition, and data analysis. One example of a similarity function is the radial basis function, which measures the distance between two points using a Gaussian distribution and is commonly used in clustering and classification algorithms.

* {ref}`content:dimensionality-reduction` systematically reduces the number of variables in a dataset while preserving as much information as possible. Dimensionality reduction simplifies data, removes noise, and makes patterns in the data more visible. It can also help visualize data, improve machine learning algorithms’ performance, and reduce the storage and computational requirements of working with large datasets. 

---

(content:measurements-distances)=
## Measurements and distances

(content:measurements-matrix-vector-norms)=
### Vector and matrix norms
A [norm](https://en.wikipedia.org/wiki/Norm_(mathematics)) is a function that measures the length of vectors or matrices. The notion of length is handy because it enables us to define distance, i.e., similarity between vectors (or matrices) in applications such as machine learning. 

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

(content:measurements-similarity-funtions)=
### Similarity functions
Other functions can be used to measure the similarity (or distance between) vectors and matrices:

````{prf:definition} Radial basis function
:label: defn-rbf-measure

A radial basis function (RBF) measures the similarity between two input vectors $\mathbf{x}_{p}\in\mathbb{R}^{m\times{1}}$ and $\mathbf{x_{q}}\in\mathbb{R}^{m\times{1}}$. The most common is the Gaussian radial basis function, defined by a bell-shaped curve:

```{math}
:label: eqn-rbf-similarity-function

k\left(\mathbf{x}_{p},\mathbf{x}_{q}\right) = \sigma_{f}^{2}\exp\left(-\frac{1}{2}\left(\mathbf{x}_{p} - \mathbf{x}_{q}\right)^{T}\mathbf{M}
\left(\mathbf{x}_{p} - \mathbf{x}_{q}\right)\right) +\sigma_{n}^{2}\delta_{pq}

```
where the matrix $\mathbf{M}\in\mathbb{R}^{m\times{m}}$ can be any symmetric matrix, $\sigma_{f}^{2}$ and $\sigma_{n}^{2}$ are constants and $\delta_{pq}$ denotes the [Kronecker delta](https://en.wikipedia.org/wiki/Kronecker_delta). 
````

We shall see that radial basis functions are used in various machine learning applications, such as classification. 

(content:dimensionality-reduction)=
## Dimensionality reduction
Dimensionality reduction systematically reduces the number of variables in a dataset while preserving as much of the information in the data as possible. Dimensionality reduction simplifies data, removes noise, and makes patterns in the data more visible. It can also help visualize data, improve machine learning algorithms’ performance, and reduce the storage and computational requirements of working with large datasets. 

There are many techniques for dimensionality reduction, including [principal component analysis](https://en.wikipedia.org/wiki/Principal_component_analysis) and [singular value decomposition](https://en.wikipedia.org/wiki/Singular_value_decomposition), which are both based on solving {ref}`content:eigenvalue-eigenvector-problems`. Other approaches, such as clustering and [t-distributed stochastic neighbor embedding (t-SNE)](https://en.wikipedia.org/wiki/T-distributed_stochastic_neighbor_embedding), are based upon minimizing some other distance measure.  

(content:eigenvalue-eigenvector-problems)=
### Eigenvalue-eigenvector problems
Eigenvalue-eigenvector problems are a type of mathematical problem that involves finding a set of scalar values $\left\{\lambda_{1},\dots,\lambda_{m}\right\}$ called [eigenvalues](https://mathworld.wolfram.com/Eigenvalue.html) and a set of vectors $\left\{\mathbf{v}_{1},\dots,\mathbf{v}_{m}\right\}$ called [eigenvectors](https://mathworld.wolfram.com/Eigenvector.html) such that:

```{math}
:label: eqn-eigenvalue-eigenvector-problem
\mathbf{A}\mathbf{v}_{j} = \lambda_{j}\mathbf{v}_{j}\qquad{j=1,2,\dots,m}
```

where $\mathbf{A}\in\mathbb{R}^{m\times{m}}$, $\mathbf{v}\in\mathbb{R}^{m\times{1}}$ is a column vector, and $\lambda\in\mathbb{R}$ is a scalar. Eigenvalues and eigenvectors are used in many areas of mathematics, engineering, and physics, including image compression and data reduction approaches such as [singular value decomposition](https://en.wikipedia.org/wiki/Singular_value_decomposition).

In addition to thier other uses, eigenvalues have a another interesting feature ({prf:ref}`obs-eigenvalues-determinants`):

````{prf:observation} Determinants and eigenvalues
:label: obs-eigenvalues-determinants

Eigenvalues can be used directly to calculate the determinant of a matrix $\mathbf{A}\in\mathbb{R}^{m\times{m}}$. Denote the set of 
eignenvalues for the matrix $\mathbf{A}\in\mathbb{R}^{m\times{m}}$ as $\left\{\lambda_{1},\dots,\lambda_{m}\right\}$. Then, the $\det\left(\mathbf{A}\right)$ is given by:

```{math}
:label: eqn-det-A-eigenvalues
\det\left(\mathbf{A}\right) = \prod_{i=1}^{m}\lambda_{i}
```

A matrix $\mathbf{A}\in\mathbb{R}^{m\times{m}}$ is non-singular if $\lambda_{i}>0~\forall{i}$, otherwise it is singular.

````

### Singular value decomposition
[Singular value decomposition (SVD)](https://en.wikipedia.org/wiki/Singular_value_decomposition) is a powerful tool used in many applications, such as image and data compression, signal processing, and machine learning. SVD factors a matrix into a canonical form composed of an orthogonal matrix, a diagonal matrix, and another orthogonal matrix:

````{prf:definition} Singular value decomposition
:label: defn-svd-real-matrix

Let $\mathbf{A}\in\mathbb{R}^{m\times{n}}$. The singular value decomposition of the matrix $\mathbf{A}$ is given by:

```{math}
:label: eqn-math-SVD
\mathbf{A} = \mathbf{U}\mathbf{\Sigma}\mathbf{V}^{T}
```

where $\mathbf{U}$ and $\mathbf{V}$ are orthogonal matrices and $\mathbf{\Sigma}$ is a diagonal matrix containing the singular values $\sigma_{i}=\Sigma_{ii}$ along the main diagonal. 

````

SVD can be used to diagonalize a matrix, find the eigenvalues and eigenvectors of a matrix, and solve linear equations. It is also essential in [principal component analysis (PCA)](https://en.wikipedia.org/wiki/Principal_component_analysis) as a dimensionality reduction technique.

````{prf:observation} SVD matrix decomposition
:label: obs-svd-matrix-decomposition

The singular value decomposition (SVD) can be thought of as decomposing a matrix into a weighted, ordered sum of separable matrices. 
Let $\mathbf{A}\in\mathbb{R}^{m\times{n}}$ have the singular value decomposition $\mathbf{A} = \mathbf{U}\mathbf{\Sigma}\mathbf{V}^{T}$.

Then, the matrix $\mathbf{A}\in\mathbb{R}^{m\times{n}}$ can be written as:

```{math}
:label: eqn-matrix-decomp
\mathbf{A} = \sum_{i=1}^{R_{\mathbf{A}}}\sigma_{i}\left(\mathbf{u}_{i}\otimes\mathbf{v}_{i}\right)
```

where $R_{\mathbf{A}}$ denotes the rank of matrix $\mathbf{A}$, the vectors $\mathbf{u}_{i}$ and $\mathbf{v}_{i}$ are the ith columns of the corresponding SVD matrices, and $\sigma_{i}$ are the ordered singular values. 

The outer-product $\left(\mathbf{u}_{i}\otimes\mathbf{v}_{i}\right)$ is the separable component of the matrix $\mathbf{A}$. 

````

#### Connection of SVD and eigendecomposition
Fill me in.


---

## Summary
* {ref}`content:dimensionality-reduction` systematically reduces the number of variables in a dataset while preserving as much information as possible. Dimensionality reduction simplifies data, removes noise, and makes patterns in the data more visible. It can also help visualize data, improve machine learning algorithms’ performance, and reduce the storage and computational requirements of working with large datasets. 
