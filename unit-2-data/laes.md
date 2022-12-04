# Solution of Linear Algebraic Equations

## Introduction 
Systems of Linear Algebraic Equations (LAEs) arise in many different Engineering fields. In Chemical Engineering, these types of equations naturally arise from steady-state materials balances as we have seen. In this lecture, we'll explore both _direct_ and _iterative_ methods to solve the generic system of LAEs:

$$\mathbf{A}\mathbf{x} = \mathbf{b}$$

where $\mathbf{A}$ denotes a $m\times{n}$ matrix, $\mathbf{x}$ denotes a $n\times{1}$ column vector of unknowns (what we want to solve for) and $\mathbf{b}$ denotes a $m\times{1}$ vector.

There are two broad categories of methods to solve for $\mathbf{x}$, direct methods such as those based upon [Gaussian elimination](https://en.wikipedia.org/wiki/Gaussian_elimination) and iterative methods such as the [Gauss–Seidel method](https://en.wikipedia.org/wiki/Gauss–Seidel_method). Here we consider iterative methods in depth and will only briefly desribe direct substitution approaches such as [Gaussian elimination](https://en.wikipedia.org/wiki/Gaussian_elimination).

In all the disucssion below we assume:

* The matrix $\mathbf{A}$ is __square__ meaning $m=n$; we'll see how we can treat non-square stystems later.
* The matrix $\mathbf{A}$ has full rank or equivalently $\det{\mathbf{A}}\neq{0}$. 

## Rank
What is interesting about $\mathbf{A}^{-1}$ are the conditions governing its existence, and in one particular a concept called [matrix rank](https://en.wikipedia.org/wiki/Rank_(linear_algebra)). The rank of a matrix _r_:

$$r\leq\min\left(m,n\right)$$

is at most equal to the smallest dimesion of the matrix (full rank). Rank is a measure of the unique information contained in a matrix; thus, if there is redudant information (rows or columns that are not linearly independent) a matrix will be less than full rank. If a matrix is less than full rank, then $\det{\mathbf{A}}=0$, and an inverse will __not__ exist. 

There are many different ways to compute rank, however, we'll use the [rank](https://docs.julialang.org/en/v1/stdlib/LinearAlgebra/#LinearAlgebra.rank) function in [Julia](https://julialang.org).

## Guassian elimination 
The naive way to solve a system of LAEs for the unknown vector $\mathbf{x}$ is to directly compute the matrix inverse $\mathbf{A}^{-1}$.
A matrix inverse has the property $\mathbf{A}^{-1}\mathbf{A}=\mathbf{I}$, where $\mathbf{I}$ denotes the _identity matrix_. 
Thus, if a matrix inverse exists, the unknown vector $\mathbf{x}$ can be computed as:

$$\mathbf{x} = \mathbf{A}^{-1}\mathbf{b}$$

Directly computing the matrix inverse $\mathbf{A}^{-1}$ is computationally expensive, so we never really do that. Instead, we 
use substitution or iterative methods to effectively compute the inverse without having to do it for real.

## The Gauss–Seidel iterative method
An iterative method takes an initial solution guess, and refines it by substituting
this guess back into into the LAEs. The guess is then updated over and over again until either we exhaust the number of iterations we can take, or we converge to a solution. Two key iterative methods are: [Jacobi Iteration](https://en.wikipedia.org/wiki/Jacobi_method) and
[Gauss-Seidel](https://en.wikipedia.org/wiki/Gauss–Seidel_method). The central difference between these two methods is how they update the best estimate of a solution at any given iteration. 

In either case, we start with the ith equation (in index form):

$$\sum_{j = 1}^{n}a_{ij}x_{j} = b_{i}\qquad{i=1,2,\cdots{m}}$$

and solve it for _an estimate_ of $x_{i}$:

$$\hat{x}_{i}=\frac{1}{a_{ii}}\bigl(b_{i}-\sum_{j=1,i}^{n}a_{ij}x_{j}\bigr)\qquad{i=1,2,\cdots{m}}$$

denoted by $\hat{x}_{i}$. As the number of iterations increases (and the system of LAEs is _convergent_) $\hat{x}_{i}\rightarrow{x_{i}}$ for $i=1,2,\cdots,m$.

### Jacobi Iteration
Jacobi iteration __batch updates__ the best estimate of $x_{i}$ at the _end_ of each iteration. Suppose we define the best estimate
for the value of $x_{i}$ at iteration k as $\hat{x}_{i,k}$. Then the value of $x_{i}$ at iteration $k+1$ is given by:

$$\hat{x}_{i,k+1}=\frac{1}{a_{ii}}\bigl(b_{i}-\sum_{j=1,i}^{n}a_{ij}\hat{x}_{j,k}\bigr)\qquad{i=1,2,\cdots,n}$$

In Jacobi iteration, the best estimate for all variables from the previous iteration is used and we do not update the guess until
we have processed all $i=1,2,\cdots,m$ equations.

### Gauss-Seidel Iteration
Gauss-Seidel __live updates__ the best estimate of $x_{i}$ _during_ the processing of equations $i=1,2,\cdots,m$, generally leading to
better convergence properties when compared with Jacobi iteration. Suppose we define the best estimate
for the value of $x_{i}$ at iteration k as $\hat{x}_{i,k}$. Then the value of $x_{i}$ at iteration $k+1$ is given by:

$$\hat{x}_{i,k+1}=\frac{1}{a_{ii}}\bigl(b_{i}-\sum_{j=1}^{i-1}a_{ij}\hat{x}_{j,k+1}-\sum_{j=i+1}^{n}a_{ij}\hat{x}_{j,k}\bigr)\qquad{i=1,2,\cdots,n}$$

In Gauss-Seidel, the best estimate of the proceeding $x_{i}$'s are used in the calculation of the current $x_{i}$. 

## Successive Overrelaxation
Successive Overrelaxation methods (SORMs) are modified versions of Gauss-Seidel, where the best estimate of $x_{i}$ is further modified
before proceeding to the evaluation of the next equations. Suppose we define the best estimate
for the value of $x_{i}$ at iteration k as $\hat{x}_{i,k}$. Then _before_ processing the next Gauss-Seidel type step we update the best 
guess for $x_{i}$ using the rule:

$$\hat{x}_{i,k}=\lambda\hat{x}_{i,k}+\left(1-\lambda\right)\hat{x}_{i,k-1}\qquad{i=1,2,\cdots,n}$$

where $\lambda$ is an adjustable parameter governed by $0<\lambda\leq{2}$. The value of $\lambda$ can dramatically speed-up *or*
slow-down the convergence to the correct solution, so care should be taken when choosing it. 
The value of $\lambda$ can be in one of three regimes:

* Underrelaxation $0<\lambda<1$: Can help solve systems that have problematic convergence
* Nominal $\lambda=1$: Regular Gauss-Seidel algorithm
* Overrelaxation $1<\lambda\leq{2}$: This regime will increase the rate of convergence provided the algorithm can converge

### Convergence
A sufficient condition for the convergence of Jacobi or Gauss-Seidel Iteration is the well known diagonal dominance condition:

$$\sum_{j=1,i}^{n}\lvert{a_{ij}}\rvert<\lvert{a_{ii}}\rvert\qquad\forall{i}$$

Diagonal dominance is a sufficient (but not necessary) condition for convergence of an iterative method. Of course, this condition says
nothing regarding the rate of convergence. Convergence, which is a measure of the distance between the current
best solution and the true solution, can be calculated using any one of several metrics. Two common techniques are the calculation of the
percentage change in the solution as well as the measurement in a least-squares sense of the distance between the solutions. 
While the true solution is not explicitly known, you can calculate the squared difference between the known right-hand side vector, $\mathbf{b}$, and your current best estimate, $\mathbf{\hat{b}}_{k}$:

$$e_{k}=\left(\mathbf{b}-\mathbf{\hat{b}}_{k}\right)\left(\mathbf{b}-\mathbf{\hat{b}}_{k}\right)^{T}$$

where $e_{k}$ denotes the squared error for solution at iteration k of the algorithm. 

## Summary
Fill me in
