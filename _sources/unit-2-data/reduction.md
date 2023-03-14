# Dimensionality Reduction

## Introduction

In this lecture, we'll discuss measurements and distances and dimensionality reduction. Measurement and distance tools measure the size of matrix or vector objects and the distances between these objects. We'll explore two types of measurement and distance approaches:

* {ref}`content:measurements-matrix-vector-norms` are mathematical tools to measure the magnitude of matrices and vectors, respectively. They are essential in many areas of mathematics, including linear algebra, optimization, and analysis. 

* {ref}`content:measurements-similarity-funtions` are mathematical tools that quantify the similarity between objects, such as vectors or points. They are widely used in machine learning, pattern recognition, and data analysis. One example of a similarity function is the radial basis function, which measures the distance between two points using a Gaussian distribution and is commonly used in clustering and classification algorithms.

On the other hand, dimensionality reduction techniques reduce the number of variables in a dataset while retaining as much information as possible:

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
Dimensionality reduction tools systematically reduce the number of variables in a dataset while preserving as much of the information in the data as possible. Dimensionality reduction simplifies data, removes noise, and makes patterns in the data more visible. It can also help visualize data, improve machine learning algorithms’ performance, and reduce the storage and computational requirements of working with large datasets. 

There are many techniques for dimensionality reduction, including [principal component analysis](https://en.wikipedia.org/wiki/Principal_component_analysis) and [singular value decomposition](https://en.wikipedia.org/wiki/Singular_value_decomposition), which are both related to {ref}`content:eigenvalue-eigenvector-problems`. Other approaches, such as clustering and [t-distributed stochastic neighbor embedding (t-SNE)](https://en.wikipedia.org/wiki/T-distributed_stochastic_neighbor_embedding), are based upon minimizing a particular distance measure.  

(content:eigenvalue-eigenvector-problems)=
### Eigenvalue-eigenvector problems
Eigenvalue-eigenvector problems involve finding a set of scalar values $\left\{\lambda_{1},\dots,\lambda_{m}\right\}$ called [eigenvalues](https://mathworld.wolfram.com/Eigenvalue.html) and a set of linearly independent vectors $\left\{\mathbf{v}_{1},\dots,\mathbf{v}_{m}\right\}$ called [eigenvectors](https://mathworld.wolfram.com/Eigenvector.html) such that:

```{math}
:label: eqn-eigenvalue-eigenvector-problem
\mathbf{A}\mathbf{v}_{j} = \lambda_{j}\mathbf{v}_{j}\qquad{j=1,2,\dots,m}
```

where $\mathbf{A}\in\mathbb{R}^{m\times{m}}$, $\mathbf{v}\in\mathbb{R}^{m\times{1}}$, and $\lambda\in\mathbb{R}$ is a scalar. Eigenvalues and eigenvectors are used in many areas of mathematics, engineering, and physics, including dynamics, image compression and data reduction:

* __Solution of Differential Equations__: Eigenvalues and eigenvectors are used to solve systems of differential equations. Eigenvectors form a set of linearly independent solutions, while eigenvalues determine the stability of these solutions.

* __Structural Analysis__: Eigenvalues and eigenvectors can be used to study the structural properties of a matrix or a graph. For example, in structural engineering, eigenvalues and eigenvectors can be used to analyze a structure’s natural frequencies and modes of vibration, e.g., buildings and bridges.
* __Principal Component Analysis (PCA)__: PCA is a statistical technique that reduces the dimensionality of a dataset. It is commonly used in data analysis, computer vision, and image processing. In PCA, the eigenvalues and eigenvectors of the [covariance matrix](https://en.wikipedia.org/wiki/Covariance_matrix) are used to find the most important features of the dataset.

In addition, eigenvalues have another interesting feature ({prf:ref}`obs-eigenvalues-determinants`):

````{prf:observation} Determinants and eigenvalues
:label: obs-eigenvalues-determinants

Eigenvalues can be used directly to calculate the determinant of a matrix $\mathbf{A}\in\mathbb{R}^{m\times{m}}$. Denote the set of 
eignenvalues for the matrix $\mathbf{A}\in\mathbb{R}^{m\times{m}}$ as $\left\{\lambda_{1},\dots,\lambda_{m}\right\}$. Then, the $\det\left(\mathbf{A}\right)$ is given by:

```{math}
:label: eqn-det-A-eigenvalues
\det\left(\mathbf{A}\right) = \prod_{i=1}^{m}\lambda_{i}
```

A matrix $\mathbf{A}\in\mathbb{R}^{m\times{m}}$ is non-singular if $\text{abs}(\lambda_{i})>0~\forall{i}$, otherwise it is singular.

````

#### Characteristic polynomial
The roots of the characteristic polynomial are the eigenvalues of a square matrix $\mathbf{A}\in\mathbb{R}^{n\times{n}}$. The characteristic polynomial plays a central theoretical role in many areas of mathematics, including linear algebra, differential equations, and dynamical systems theory ({prf:ref}`defn-characteristic-polynomial`):

````{prf:definition} Characteristic polynomial
:label: defn-characteristic-polynomial

The characteristic polynomial of a square matrix $\mathbf{A}\in\mathbb{R}^{n\times{n}}$ is a polynomial that is obtained by subtracting the scalar $\lambda$ from the diagonal of the matrix $\mathbf{A}$ and computing its determinant:

```{math}
:label: eqn-characteristic-polynomial
\det\left(\mathbf{A}-\lambda\mathbf{I}\right) = 0
```

where $\mathbf{I}$ is the $n\times{n}$ identity matrix, and $\lambda$ is an eigenvalue of the matrix $\mathbf{A}$. The characteristic polynomial is a polynomial of degree $n$ with $\lambda$ as the variable, and its roots are precisely the eigenvalues of the matrix $\mathbf{A}$. 

````

However, despite its theoretical importance, in applications, we will rarely explicitly solve in the characteristic polynomial for the eigenvalues of the matrix $\mathbf{A}$. Instead, we'll use one of several more accessible techniques, see {ref}`content:compute-eigenvalues-eigenvectors`.

#### Eigenvectors
To compute the eigenvectors of the matrix $\mathbf{A}\in\mathbb{R}^{n\times{n}}$, we first need the eigenvalues $\left\{\lambda_{1},\dots,\lambda_{n}\right\}$. Once we have the eigenvalues, we rearrange Eqn {eq}`eqn-eigenvalue-eigenvector-problem` for each eigenvalue $\lambda_{j}$ to give a system of homogenous linear algebraic equations that can be solved for the associated eigenvector 
({prf:ref}`defn-homogenous-eigenvector-system`):

````{prf:definition} Eigenvector system
:label: defn-homogenous-eigenvector-system

Let the eigenvalues of the matrix $\mathbf{A}\in\mathbb{R}^{n\times{n}}$ be given by the set $\left\{\lambda_{1},\dots,\lambda_{n}\right\}$. Then for each eigenvalue $\lambda_{j}$, there exists an eigenvector $\mathbf{v}_{j}$ that is a solution of the homogenous system of equations:

```{math}
:label: eqn-homogenous-eigenvector-system
\left(\mathbf{A}-\lambda_{j}\mathbf{I}\right)\mathbf{v}_{j} = \mathbf{0}
```

where $\mathbf{I}$ denotes the $n\times{n}$ identity matrix. While eigenvectors are always linearly independent, they are not unique (in any case) and orthogonal only for a symmetric $\mathbf{A}$.
````


(content:compute-eigenvalues-eigenvectors)=
### Methods to compute eigenvalues and eigenvectors
* [Power iteration](https://en.wikipedia.org/wiki/Power_iteration) is an iterative method that starts with a random vector and repeatedly multiplies the matrix by this vector, normalizing the result each time. As the iteration proceeds, the vector converges to the eigenvector corresponding to the largest eigenvalue. This method efficiently computes the dominant eigenvalue and eigenvector of a large, sparse matrix. [Lanczos iteration](https://en.wikipedia.org/wiki/Lanczos_algorithm) is similar to power iteration but uses a truncated orthogonalization process to compute a small number of eigenvalues and eigenvectors of a large, sparse matrix.
* [Divide-and-conquer algorithms](https://en.wikipedia.org/wiki/Divide-and-conquer_algorithm) decompose the matrix into smaller matrices, recursively compute their eigenvalues and eigenvectors, and then combine them to obtain the eigenvalues and eigenvectors of the original matrix. This method is efficient for symmetric matrices.
* [QR iteration](https://en.wikipedia.org/wiki/QR_algorithm) applies a sequence of orthogonal similarity transformations to the matrix, which gradually transforms it into a diagonal matrix with the eigenvalues on the diagonal. The eigenvectors can be computed by back-substitution. This method is more expensive than power iteration but can compute all eigenvalues and eigenvectors of a matrix.
* [Singular value decomposition](https://en.wikipedia.org/wiki/Singular_value_decomposition) computes a matrix’s singular values and singular vectors, which are related to its eigenvalues and eigenvectors. It is handy for calculating the low-rank approximations of a matrix or for dimensionality reduction.

In [Julia](https://julialang.org), eigenvalues and eigenvectors of a dense matrix $\mathbf{A}\in\mathbb{R}^{n\times{n}}$ can be calculated using the [eigen](https://docs.julialang.org/en/v1/stdlib/LinearAlgebra/#LinearAlgebra.eigen) function. In [Python](https://www.python.org), eigenvalues and eigenvectors can be computed using the [eig](https://numpy.org/doc/stable/reference/generated/numpy.linalg.eig.html) function of the [NumPy](https://numpy.org) library.

#### QR iteration
The [QR algorithm](https://en.wikipedia.org/wiki/QR_algorithm) iteratively calculates the eigenvalues and eigenvectors of a square matrix. The [QR algorithm](https://en.wikipedia.org/wiki/QR_algorithm), independently developed in the late 1950s by [John G. F. Francis](https://en.wikipedia.org/wiki/John_G._F._Francis) and by [Vera N. Kublanovskaya](https://en.wikipedia.org/wiki/Vera_Kublanovskaya), is one of the ten algorithms of the twentieth century (REFHERE).

````{prf:definition} Eigenvalues and Eigenvectors using QR iteration
:label: defn-qr-iteration

The basic idea is to perform [QR decomposition](https://en.wikipedia.org/wiki/QR_decomposition) repeatedly, writing the matrix as a product of an orthogonal matrix $\mathbf{Q}$ and an upper triangular matrix $\mathbf{R}$, multiply the factors in the reverse order, and iterate.

__Phase 1: Eigenvalues__. Let $\mathbf{A}\in\mathbb{R}^{n\times{n}}$. Then, at the kth step of the procedure (starting with $k = 0$), we compute the [QR decomposition](https://en.wikipedia.org/wiki/QR_decomposition) of the matrix $\mathbf{A}$:

```{math}
:label: eqn-qr-iteration
\mathbf{A}_{k+1}=\mathbf{Q}_{k}\mathbf{R}_{k} = \mathbf{Q}_{k}^{T}\mathbf{A}_{k}\mathbf{Q}_{k}\qquad{k=0,1,\dots} 
```

where $\mathbf{Q}_{k}$ is an orthogonal matrix, i.e., $\mathbf{Q}^{T}_{k} = \mathbf{Q}^{−1}_{k}$ and $\mathbf{R}_{k}$ is an upper triangular matrix. As $k\rightarrow\infty$, the matrix $\mathbf{A}_{k}$ converge to a triangular matrix where eigenvalues are listed along the diagonal.

__Phase 2: Eigenvectors__. Once we've calculated the eigenvalues $\left\{\lambda_{1},\dots,\lambda_{n}\right\}$, we can compute the eigenvectors associated with each of the eigenvalues $\left\{\mathbf{v}_{1},\dots,\mathbf{v}_{n}\right\}$  by solving the homogenous system of equations:

```{math}
\left(\mathbf{A}-\lambda_{j}\mathbf{I}\right)\mathbf{v}_{j} = \mathbf{0}
```
````

Let's do an example ({prf:ref}`example-QR-iteration`) where we compute the eigenvalues and eigenvectors of a square matrix $\mathbf{A}$ using the [QR iteration algorithm](https://en.wikipedia.org/wiki/QR_algorithm) outlined in {prf:ref}`defn-qr-iteration`.

````{prf:example} QR iteration to compute Eigenvalues and Eigenvectors
:label: example-QR-iteration
:class: dropdown

Compute the eigenvalues and eigenvectors of the matrix:

$$
\mathbf{A} = \begin{bmatrix}
3.0 & -0.3 & -0.2 \\
0.1 & 7.0 & -0.3 \\
0.3 & -0.2 & 10.0 \\
\end{bmatrix}
$$

using the [QR iteration algorithm](https://en.wikipedia.org/wiki/QR_algorithm) outlined in {prf:ref}`defn-qr-iteration`. How well does your answer compare to the values generated by [eigen](https://docs.julialang.org/en/v1/stdlib/LinearAlgebra/#LinearAlgebra.eigen) function?

__Solution__: First, compute the eigenvalues and eigenvectors using the [Julia eigen function](https://docs.julialang.org/en/v1/stdlib/LinearAlgebra/#LinearAlgebra.eigen):

```julia
using LinearAlgebra

# Setup matrix the n x n matrix A (n = 3)
A = [3.0 -0.3 -0.2 ; 0.1 7.0 -0.3 ; 0.3 -0.2 10.0];

# Decompose using the built-in function
F = eigen(A);   # eigenvalues and vectors in F of type Eigen
λ = F.values;   # vector of eigenvalues
V = F.vectors;  # 3 x 3 matrix of eigenvectors, each col is an eigenvector

# Call our qriteration function (assumed to be included in the workspace)
(L,V) = qriteration(A; maxiter=10000, tolerance=1e-6);

# compare the difference between eigenvalues
δ = norm(L - λ); # see lecture notes on vector norms
```

The difference between our `qriteration` implementation and `eigen` was $\delta = 2.25e-6$. Thus, our function produces eigenvalues that are close to `eigen`,  with an error of the same magnitude as the `tolerance` parameter.

__Source__: The implementation of our `qriteration` function can be found on [GitHub](https://github.com/varnerlab/CHEME-1800-4800-Course-Repository-S23/tree/main/examples/unit-2-examples/qr).
````

#### Singular value decomposition
[Singular value decomposition (SVD)](https://en.wikipedia.org/wiki/Singular_value_decomposition) is a powerful tool used in many applications, such as image and data compression, signal processing, and machine learning. Singular value decomposition factors a matrix into the product of an orthogonal matrix, a diagonal matrix, and another orthogonal matrix ({prf:ref}`defn-svd-real-matrix`):

````{prf:definition} Singular value decomposition
:label: defn-svd-real-matrix

Let the matrix $\mathbf{A}\in\mathbb{R}^{m\times{n}}$. The singular value decomposition of the matrix $\mathbf{A}$ is given by:

```{math}
:label: eqn-math-SVD
\mathbf{A} = \mathbf{U}\mathbf{\Sigma}\mathbf{V}^{T}
```

where $\mathbf{U}\in\mathbb{R}^{n\times{n}}$ and $\mathbf{V}\in\mathbb{R}^{m\times{m}}$ are orthogonal matrices and $\mathbf{\Sigma}\in\mathbb{R}^{n\times{m}}$ is a diagonal matrix containing the singular values $\sigma_{i}=\Sigma_{ii}$ along the main diagonal. 

The columns of $\mathbf{U}$ are called left-singular vectors, while the columns of $\mathbf{V}$ are called right-singular vectors.
Singular vectors have a unique property: unlike eigenvectors, left- and right-singular vectors are linearly independent and orthogonal.
````

The singular value decomposition and eigendecomposition have important connections:
* Singular values and eigenvalues are related: $\sigma_{i} = \sqrt\lambda_{i}$
* The columns of $\mathbf{U}$ (left-singular vectors) are eigenvectors of the matrix product $\mathbf{A}\mathbf{A}^{T}$.
* The columns of $\mathbf{V}$ (right-singular vectors) are eigenvectors of the matrix product $\mathbf{A}^{T}\mathbf{A}$.

##### Structural decomposition using SVD
Let's explore another interesting use of singular value decomposition, namely structural decomposition of a matrix ({prf:ref}`obs-svd-matrix-decomposition`): 

````{prf:observation} SVD structural decomposition
:label: obs-svd-matrix-decomposition

Singular value decomposition (SVD) decomposes a rectangular matrix $\mathbf{A}$ into a weighted, ordered sum of separable matrices. 
Let $\mathbf{A}\in\mathbb{R}^{m\times{n}}$ have the singular value decomposition $\mathbf{A} = \mathbf{U}\mathbf{\Sigma}\mathbf{V}^{T}$. 

Then, the matrix $\mathbf{A}\in\mathbb{R}^{m\times{n}}$ can be re-written as:

```{math}
:label: eqn-matrix-decomp
\mathbf{A} = \sum_{i=1}^{R_{\mathbf{A}}}\sigma_{i}\left(\mathbf{u}_{i}\otimes\mathbf{v}_{i}\right)
```

where $R_{\mathbf{A}}$ is the rank of matrix $\mathbf{A}$, the vectors $\mathbf{u}_{i}$ and $\mathbf{v}_{i}$ are the ith left and right singular vectors, respectively, and $\sigma_{i}$ are the ordered singular values. 

The [outer-product](https://en.wikipedia.org/wiki/Outer_product) $\left(\mathbf{u}_{i}\otimes\mathbf{v}_{i}\right)$ is the separable component of the matrix $\mathbf{A}$. For more details on computing the [outer-product](https://en.wikipedia.org/wiki/Outer_product), see {ref}`content:vector-vector-operations`.
````



### Principle component analysis (PCA)
Fill me in.

---

## Summary

In this lecture, we discussed distance measurements and dimensionality reduction. Measurement and distance tools measure the size of matrix or vector objects and the distances between these objects. We explored two types of measurement and distance approaches:

* {ref}`content:measurements-matrix-vector-norms` are mathematical tools to measure the magnitude of matrices and vectors, respectively. They are essential in many areas of mathematics, including linear algebra, optimization, and analysis. 

* {ref}`content:measurements-similarity-funtions` are mathematical tools that quantify the similarity between objects, such as vectors or points. They are widely used in machine learning, pattern recognition, and data analysis. One example of a similarity function is the radial basis function, which measures the distance between two points using a Gaussian distribution and is commonly used in clustering and classification algorithms.

Dimensionality reduction techniques reduce the number of variables in a dataset while retaining as much information as possible:

* {ref}`content:dimensionality-reduction` systematically reduces the number of variables in a dataset while preserving as much information as possible. Dimensionality reduction simplifies data, removes noise, and makes patterns in the data more visible. It can also help visualize data, improve machine learning algorithms’ performance, and reduce the storage and computational requirements of working with large datasets. 
