# Linear Algebraic Equations

<!-- In the special case where the number of rows and columns of $\mathbf{A}$ are equal ($m=n$), and some additional conditions are true, we can calculate a unique solution for Eqn. {eq}`eqn-general-system-laes` and the inverse $\mathbf{A}^{\dagger} = \mathbf{A}^{-1}$  has the 
special property: -->

<!-- If an inverse exists, then it has 
the special property:

$$\mathbf{A}^{\dagger}\mathbf{A} = \mathbf{I}$$

where $\mathbf{I}$ denotes the [identity Matrix](https://en.wikipedia.org/wiki/Identity_matrix); the [identity Matrix](https://en.wikipedia.org/wiki/Identity_matrix) is a diagonal matrix with 1's on the diagonal and 0's everywhere else. -->


## Introduction 
Systems of Linear Algebraic Equations (LAEs) arise in many different Engineering fields. In Chemical Engineering, these equations naturally arise from steady-state materials balances, as we have seen. In this lecture, we'll explore both _direct_ and _iterative_ methods to solve the generic system of LAEs:

```{math}
:label: eqn-general-system-laes
\mathbf{A}\mathbf{x} = \mathbf{b}
```

where $\mathbf{A}$ denotes a $m\times{n}$ matrix, $\mathbf{x}$ denotes a $n\times{1}$ column vector of unknowns (what we want to solve for), and $\mathbf{b}$ represents a $m\times{1}$ vector. Let's consider two cases:

* __Homogenous system__: A homogeneous system of linear algebraic equations is a system in which the right-hand side vector $\mathbf{b}=\mathbf{0}$; every entry in the $\mathbf{b}$-vector is equal to zero. The solution set for a homogeneous system is a subspace in linear algebra, which contains all the solutions that can be added to get new solutions.

* __Non-homogeneous system__: A non-homogeneous system of linear algebraic equations is a system in which at least one entry of the right-hand side vector $\mathbf{b}$ is non-zero.

<!-- To compute a value for the unknown vector $\mathbf{x}$ we compute a [matrix inverse](https://mathworld.wolfram.com/MatrixInverse.html):

$$\mathbf{x} = \mathbf{A}^{\dagger}\mathbf{b}$$

where $\mathbf{A}^{\dagger}$ denotes the inverse of the matrix $\mathbf{A}$.  -->

In this lecture, we'll look at techniques to solve homogeneous and non-homogeneous system of linear algebraic equations. We will:

* Begin by introducing some {ref}`content:references:motivating-examples` that will be handy later to frame our discussion.
* Next, we will introduce {ref}`content:references:solution-approaches` to solve square systems of linear algebraic equations.
* Finally, we'll conclude with a discussion of {ref}`content:references:solution-approaches-non-square`. These approaches can compute approximate inverses for non-square systems of linear algebraic equations.
---


(content:references:motivating-examples)=
## Motivating examples and concepts

### Underdetermined systems
Suppose you had two steady-state blending tanks $T_{1}$ and $T_{2}$ in series, each with two input streams and a single output stream ({numref}`fig-blending-tanks-example`):

```{figure} ./figs/Fig-Blending-Tanks-Example.pdf
---
height: 140px
name: fig-blending-tanks-example
---
Caption goes here
```

Further, we have species set $\mathcal{M}$ in this system and no chemical reactions. Then, the steady-state species concentration balances for component $i\in\mathcal{M}$ in blending tanks 1 and 2 can be written as:

$$
\begin{eqnarray}
\dot{V}_{1}C_{i,1} + \dot{V}_{4}C_{i,4}-\dot{V}_{2}C_{i,2} & = & 0\\
\dot{V}_{2}C_{i,2} + \dot{V}_{5}C_{i,5}-\dot{V}_{3}C_{i,3} & = & 0
\end{eqnarray}
$$

where $\dot{V}_{j}$ denotes the volumetric flow-rate of stream $j$ (units: volume/time), and $C_{i,j}$ denotes the 
concentration of component $i$ in stream $j$ (units: mole/volume). This system of concentration balances can be re-written as a matrix-vector product:

$$
\begin{bmatrix}
\dot{V}_{1} & -\dot{V}_{2} & 0 & \dot{V}_{4} & 0 \\
0 & \dot{V}_{2} & -\dot{V}_{3} & 0 & \dot{V}_{5} 
\end{bmatrix}
\begin{pmatrix}
C_{i,1} \\
C_{i,2} \\
C_{i,3} \\
C_{i,4} \\
C_{i,5}
\end{pmatrix} = 
\begin{pmatrix}
0 \\
0 \\
0 \\
0 \\
0
\end{pmatrix}
$$

or in matrix-vector notation:

```{math}
:label: eqn-matrix-vector-notation-tanks
\dot{\mathbf{V}}\mathbf{C} = \mathbf{0}
```

Eqn {eq}`eqn-matrix-vector-notation-tanks` is an _underdetermined_ system, i.e., we have more unknowns (in this case, five) than equations (only two equations). Thus, without additional information, this system of equations does not have a unique solution. 

### Square systems
Instead of knowing nothing, suppose in our blending tank example we know the concentrations of component $i$ in the input streams (1,4 and 5). Then, we only need to calculate the concentrations of component $i$ in streams 2 and 3. 
We know have a _square_ system, i.e., the number of equations (rows) and the number of unknowns (columns) are equal. 

In this case, we can rearrange our concentration balance equations, moving the knowns to the right-hand side:

$$
\begin{bmatrix}
-\dot{V}_{2} & 0 \\
0 & -\dot{V}_{2}
\end{bmatrix}
\begin{pmatrix}
C_{i,2} \\
C_{i,3}
\end{pmatrix} = 
\begin{pmatrix}
-\dot{V}_{1}C_{i,1} - \dot{V}_{4}C_{i,4} \\
-\dot{V}_{5}C_{i,5}
\end{pmatrix}
$$

The additional information gives a 2$\times$2 system, i.e., a _sqaure_ system of equations. A _square_ system of equations, depending upon the properties of the system can have a unique solution. The system above can be written in matrix-vector form as:

```{math}
\dot{\mathbf{V}}\mathbf{C} = \mathbf{b}
```

where $\dot{\mathbf{V}}$ is now a 2$\times$2 square matrix, and $\mathbf{C}$ is a 2$\times$1 column-vector. However, unlike the previous underdetermined case, the right-hand side is now replaced by the 
2$\times$1 column-vector $\mathbf{b}$ which holds the new information about the measured inputs. 

### Overdetermined systems
As our motivating example for overdetermined systems, which have more equations than unknowns, let's consider the classic machine in a box problem ({numref}`fig-enzyme-catalyzed-rxn-example`). In this problem, we'll consider a particular type of molecular machine, an enzyme. [Enzymes](https://en.wikipedia.org/wiki/Enzyme) are molecular machines that convert a starting material (called a substrate) into a product.

```{figure} ./figs/Fig-Enzyme-Catalyzed-Reaction.pdf
---
height: 540px
name: fig-enzyme-catalyzed-rxn-example
---
Caption goes here
```

The process of converting substrate $S$ to product $P$ can be modeled as four elementary steps:
* Step 1: The enzyme $E$ binds substrate $S$ to form the enzyme-substrate complex $E:S$. 
* Step 2: The enzyme-substrate complex $E:S$ can fall apart giving back $E$ and $S$. 
* Step 3: Alternatively, enzyme-substrate complex $E:S$ can form the the enzyme-product complex $E:P$. 
* Step 4: Finally, the enzyme-product complex $E:P$ can disassociate, giving the product $P$ and the enzyme $E$; the enzyme $E$ is free to bind with another substrate molecule and run the cycle over again. 

Let's assume the box has a single input ($s=1$) and a single output stream ($s=2$). Then, the steady-state species mole balance equations for chemical component $i$ in the set of chemical species $\mathcal{M}$ and reactions $\mathcal{R}$ are given by:

$$\dot{n}_{i,1} - \dot{n}_{i,2} + \sum_{r\in\mathcal{R}}\sigma_{ir}\dot{\epsilon}_{r} = 0$$

where $\sigma_{ir}$ denotes the stoichiometric coefficient for species $i$ in reaction $r$, $\dot{\epsilon}_{r}$ denotes the open extent of reaction (units: mole/time) for reaction $r$ and $\mathcal{R}$ denotes the set of chemical reactions that occur in the box. In terms of elementary chemical reactions, this cycle can be written as the four elementary reactions: 

$$
\begin{eqnarray}
\text{S + E} &\stackrel{\rm 1}{\longrightarrow}& \text{E:S}\\
\text{E:S} &\stackrel{\rm 2}{\longrightarrow}& \text{S+E}\\
\text{E:S} &\stackrel{\rm 3}{\longrightarrow}& \text{E:P}\\
\text{E:P} &\stackrel{\rm 4}{\longrightarrow}& \text{E+P}
\end{eqnarray}
$$

Putting all these ideas together, where $\mathcal{M}=\left\{S, P, E, E:S, E:P\right\}$ and $\mathcal{R} =\left\{1, 2, 3, 4\right\}$, gives the 5$\times$4 system:

$$
\begin{bmatrix}
-1 & 1 & 0 & 0 \\
0 & 0 & 0 & 1 \\
-1 & 1 & 0 & 1 \\
1 & -1 & -1 & 0 \\
0 & 0 & 1 & -1
\end{bmatrix}
\begin{pmatrix}
\dot{\epsilon}_{1} \\
\dot{\epsilon}_{2} \\
\dot{\epsilon}_{3} \\
\dot{\epsilon}_{4} 
\end{pmatrix} = 
\begin{pmatrix}
\dot{n}_{1,2} - \dot{n}_{1,1}\\
\dot{n}_{2,2} - \dot{n}_{2,1}\\
\dot{n}_{3,2} - \dot{n}_{3,1}\\
\dot{n}_{4,2} - \dot{n}_{4,1}\\
\dot{n}_{5,2} - \dot{n}_{5,1}
\end{pmatrix}
$$

where can be re-written in matrix-vector form as:

```{math}
:label: eqn-system-mole-balances
\mathbf{S}\dot{\mathbf{\epsilon}} = \dot{\mathbf{n}}_{2} - \dot{\mathbf{n}}_{1}
```

## Solution existence

### Homogeneous system and rank
A homogeneous system of linear algebraic equations with $n\times{m}$ coefficient matrix $\mathbf{A}$ and unknown vector $\mathbf{x}$

```{math}
:label: eqn-homogenous-laes
\mathbf{A}\mathbf{x} = \mathbf{0}
```

has a solution if and only if the coefficient matrix $\mathbf{A}$ is singular; determinant $\det\mathbf{A} = 0$. The determinant condition is an easy theoretical test to check for the existence of a solution to a homogenous system of linear algebraic equations. However, in a practical sense computing the determinant directly is computationally expensive. Alternatively, the determinant condition can be also checked by computing the [rank](https://en.wikipedia.org/wiki/Rank_(linear_algebra)) of matrix $\mathbf{A}$. The rank _r_ of a matrix $\mathbf{A}$:

```{math}
:label: eqn-rank-inequality
r\leq\min\left(m,n\right)
```

is at most equal to the smallest dimesion of the matrix (full rank). Rank is a measure of the unique information contained in a matrix; thus, if there is redudant information (rows or columns that are not linearly independent) a matrix will be less than full rank. If a matrix is less than full rank, then $\det{\mathbf{A}}=0$. 

There are many different ways to compute rank, however, we'll use the [rank](https://docs.julialang.org/en/v1/stdlib/LinearAlgebra/#LinearAlgebra.rank) function in [Julia](https://julialang.org).

### Non-homogeneous system
A non-homogeneous system of linear algebraic equations with $n\times{m}$ coefficient matrix $\mathbf{A}$, unknown vector $\mathbf{x}$ and right-hand-side vector $\mathbf{b}$:

```{math}
:label: eqn-non-homogenous-laes
\mathbf{A}\mathbf{x} = \mathbf{b}
```

will have a solution if there exists a matrix $\mathbf{A}^{\dagger}$ such that:

```{math}
:label: eqn-non-homogenous-laes-inverse
\mathbf{x} = \mathbf{A}^{\dagger}\mathbf{b}
```

and $\mathbf{A}^{\dagger}\mathbf{A}=\mathbf{I}$, where $\mathbf{I}$ is the identity matrix and $\mathbf{A}^{\dagger}$ is the matrix inverse of $\mathbf{A}$.

(content:references:solution-approaches)=
## Solution approaches
The naive way to solve a system of LAEs for the unknown vector $\mathbf{x}$ is to directly compute the matrix inverse $\mathbf{A}^{-1}$. A matrix inverse has the property $\mathbf{A}^{-1}\mathbf{A}=\mathbf{I}$, where $\mathbf{I}$ denotes the _identity matrix_.  Thus, if a matrix inverse exists, the unknown vector $\mathbf{x}$ can be computed as:

$$\mathbf{x} = \mathbf{A}^{-1}\mathbf{b}$$

Directly computing the matrix inverse $\mathbf{A}^{-1}$ is computationally expensive, so we never really do that. Instead, we use substitution or iterative methods to effectively compute the inverse without having to do it for real.

There are two broad categories of methods to solve for $\mathbf{x}$, direct methods, such as those based upon [Gaussian elimination](https://en.wikipedia.org/wiki/Gaussian_elimination), and iterative methods, such as the [Gauss–Seidel method](https://en.wikipedia.org/wiki/Gauss–Seidel_method). Here we consider iterative methods in-depth and will only briefly describe direct substitution approaches such as [Gaussian elimination](https://en.wikipedia.org/wiki/Gaussian_elimination).

In all the discussion below, we assume:

* The matrix $\mathbf{A}$ is __square__ meaning $m=n$; we'll see how we can treat non-square systems later.
* The matrix $\mathbf{A}$ has full rank or equivalently $\det{\mathbf{A}}\neq{0}$. 

For the study of the different solution approaches, let's consider a motivating example. 

### Gaussian elimination 
[Gaussian elimination](https://en.wikipedia.org/wiki/Gaussian_elimination) is an efficient method for solving large square systems of linear algebraic equations. [Gaussian elimination](https://en.wikipedia.org/wiki/Gaussian_elimination) is based on "eliminating" variables by adding or subtracting equations so that the coefficients of one variable are eliminated in subsequent equations. This allows you to solve for the remaining variables one at a time until you have a solution for the entire system.

````{prf:algorithm} Naive Gaussian Elimination
:class: dropdown
:label: algo-ge-basic

**Input**: 
Matrix $\mathbf{A}$, the vector $\mathbf{b}$, guess $\mathbf{x}_{o}$, tolerance $\epsilon$, maximum iterations $\mathcal{M}_{\infty}$.

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
        1. for k = i to m:
            1. set $a_{jk}\leftarrow{a_{jk}} - \text{factor}\times{a_{ik}}$

**Backtrace**
1. set $\hat{\mathbf{x}}\leftarrow\text{zeros}(n)$
    1. for $i\in{n\dots{1}}$
        1. set $\hat{x}_{i}\leftarrow{\bar{a}_{im}}/\bar{a}_{ii}$
        1. for $j\in{i-1\dots{1}}$
            1. set $\bar{a}_{jm}\leftarrow\bar{a}_{jm} - \bar{a}_{ji}\times\hat{x}_{i}$

**Return**
$\hat{\mathbf{x}}$
````

Let's walkthrough a simple generic example using {numref}`algo-ge-basic` to illustrate the steps involved with [Gaussian elimination](https://en.wikipedia.org/wiki/Gaussian_elimination); consider the solution of the 3$\times$3 system:

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

__Step 1__: Form the augemented matrix $\bar{\mathbf{A}}$. The augemented matrix $\bar{\mathbf{A}}$ is defined as the matrix $\mathbf{A}$ with the righ-hand-side vector $\mathbf{b}$ appended as the last column:

```{math}
:label: eqn-augmented-array-A
\bar{\mathbf{A}} = \begin{bmatrix}
2 & -1 & 0 &\bigm| & 0 \\
-1 & 2 & -1 &\bigm| & 1 \\
0 & -1 & 2 &\bigm| & 0
\end{bmatrix}
```

__Step 2__: Perform row operations to reduce the augmented matrix $\bar{\mathbf{A}}$ to [row echelon form](https://en.wikipedia.org/wiki/Row_echelon_form):

1. Starting with the first column, move to the next column (to the right) until we encounter a non-zero element.
1. One we encounter a column that has nonzero entries, interchange rows, if necessary, to get a nonzero entry on top.
1. Change the top entry to 1: If the first nonzero entry of row $R_{i}$ is $\lambda$, we can convert to $1$ through the operation: $R_{i}\leftarrow{1/\lambda}R_{i}$
1. For any nonzero entry below the top one, use an elementary row operation to change it to zero; If two rows $R_{i}$ and $R_{j}$ have nonzero entries in column $k$, we can transform the (j,k) entry into a zero using $R_{j}\leftarrow{R}_{j} - (a_{jk}/a_{ik})R_{i}$. Repeat this operation until all entries are zero in this column expect the top one. When finished move to row 2, step 1.
1. Carry out this procedure until the augmented matrix $\bar{\mathbf{A}}$ is in [row echelon form](https://en.wikipedia.org/wiki/Row_echelon_form).

In Eqn {eq}`eqn-augmented-array-A`, the first column is non-zero, and a interchanging rows will not improve the our progress, thus, let's move to step 3:

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

After completing step 3, we can now move to step 4 and eliminate the coefficient below thw top row; in this case $i=1$, $j=1$ and $k=1$ which gives the operation: $R_{2}\leftarrow{R}_{2} + R_{1}$:

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

Because row 3 was already zero, we have now completed the operations for row 1. Moving to row 2, the first non-zero coefficient is in column 2. Scale row 2, and then substract from row 3, etc.

__Step 3__: Solve for unknown variables using back-substitution.

```{math}
:label: eqn-back-sub-matrix-A
\begin{bmatrix}
1 & -0.5 & 0 &\bigm| & 0 \\
0 & 1 & -0.66 &\bigm| & 0.66 \\
0 & 0 & 1 &\bigm| & 0.49
\end{bmatrix}
```

This system shown in {eq}`eqn-back-sub-matrix-A` can be solved by _back substitution_. In the back substitution algorithm, we assume that the matrix $\mathbf{U}$ is the row echelon matrix containing the coefficients of the system, and $\mathbf{y}$ is the vector containing the right-hand sides of the equations, then:

````{prf:algorithm} Naive back substitution
:label: algo-ge-basic-back-sub

**Main**
1. for $i\in{n},n-1,\dots,1$
    1. set $x_{i}\leftarrow{y_{i}}$
        1. for $j\in{i+1},i+2,\dots,n$
            1. set $x_{i}\leftarrow{x_{i}}-u_{ij}x_{j}$

````


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

(content:references:solution-approaches-non-square)=
## Generalized inverses and pseudoinverses
Fill me in. 

---

## Summary
Fill me in
