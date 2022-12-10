# Linear Algebraic Equations

## Introduction 
Systems of Linear Algebraic Equations (LAEs) arise in many different Engineering fields. In Chemical Engineering, these equations naturally arise from steady-state materials balances, as we have seen. In this lecture, we'll explore both _direct_ and _iterative_ methods to solve the generic system of LAEs:

$$\mathbf{A}\mathbf{x} = \mathbf{b}$$

where $\mathbf{A}$ denotes a $m\times{n}$ matrix, $\mathbf{x}$ denotes a $n\times{1}$ column vector of unknowns (what we want to solve for), and $\mathbf{b}$ denotes a $m\times{1}$ vector. To compute a value for the unknown vector $\mathbf{x}$ we compute a [matrix inverse](https://mathworld.wolfram.com/MatrixInverse.html):

$$\mathbf{x} = \mathbf{A}^{\#}\mathbf{b}$$

where $\mathbf{A}^{\#}$ is an inverse of matrix $\mathbf{A}$. 

There are two broad categories of methods to solve for $\mathbf{x}$, direct methods, such as those based upon [Gaussian elimination](https://en.wikipedia.org/wiki/Gaussian_elimination), and iterative methods, such as the [Gauss–Seidel method](https://en.wikipedia.org/wiki/Gauss–Seidel_method). Here we consider iterative methods in-depth and will only briefly describe direct substitution approaches such as [Gaussian elimination](https://en.wikipedia.org/wiki/Gaussian_elimination).

In all the discussion below, we assume:

* The matrix $\mathbf{A}$ is __square__ meaning $m=n$; we'll see how we can treat non-square systems later.
* The matrix $\mathbf{A}$ has full rank or equivalently $\det{\mathbf{A}}\neq{0}$. 
---

## Motivating examples

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
Overdetermined systems have more equations than unknonws. As our motivating example for this class of system, consider the four elementary steps in a idealized enzyme catalyzed reaction {numref}`fig-enzyme-catalyzed-rxn-example`:

```{figure} ./figs/Fig-Enzyme-Catalyzed-Reaction.pdf
---
height: 440px
name: fig-enzyme-catalyzed-rxn-example
---
Caption goes here
```


In the first elementary reaction, enzyme $E$ binds substrate $S$ to form the enzyme-substrate complex $E:S$. The enzyme-substrate complex can fall apart (elementary reaction 2) or produce the enzyme-product complex $E:P$. Finally, the enzyme-product complex $E:P$ can disassociate, giving the product $P$ and the enzyme $E$; the enzyme is free to bind with another substrate molecule and run the cycle over again. 

In terms of elementary chemical reactions, this cycle can be written as the four elementary reactions: 

$$
\begin{eqnarray}
\text{S + E} &\stackrel{\rm 1}{\longrightarrow}& \text{E:S}\\
\text{E:S} &\stackrel{\rm 2}{\longrightarrow}& \text{S+E}\\
\text{E:S} &\stackrel{\rm 3}{\longrightarrow}& \text{E:P}\\
\text{E:P} &\stackrel{\rm 4}{\longrightarrow}& \text{E+P}
\end{eqnarray}
$$


### Rank
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

---

## Summary
Fill me in
