# Linear Programming

## Introduction
Linear programming (LP) is a method to estimate the best outcome (such as maximum profit, lowest cost, or the best possible rate of production) using a mathematical model composed of linear relationships subject to linear constraints. 

[The development of the simplex algorithm](https://en.wikipedia.org/wiki/Simplex_algorithm), an efficient approach for solving linear programs, has led to LPs being used to solve problems in many engineering applications and other diverse industries such as banking, education, forestry, energy, and logistics. 



---

## Primal linear programs
Linear programs maximize (or minimize) a _linear_ objective function $\mathcal{O}$ subject to _linear_ constraints and bounds on the variables we are searching over. 

````{prf:definition} Primal Linear Program
:label: defn-primael-linear program

Suppose we wish to estimate the value of an M-dimensional (continuous) decision variable vector $\mathbf{x}$, e.g., the best possible allocation of some scarce resource such as cars to people in a ride-sharing application, or money to stocks. Further, our problem can be represented as a system of linear equations. 

Then, we can estimate the _optimal_ value for the variables $\mathbf{x}$ by solving the linear program (canonical form):

$$
\begin{eqnarray}
\text{maximize/minimize}~\mathcal{O} &=& \sum_{i=1}^{M} c_{i}x_{i}\\
\text{subject to}~\mathbf{Ax} &\leq&\mathbf{b}\\
\text{and}~x_{i}&\geq&{0}\qquad{i=1,2,\dots,M}
\end{eqnarray}
$$


The components of $\mathbf{x}$ are the variables to be determined, $c_{i}$ are constant coefficients in the _linear_ objective function $\mathcal{O}$, $\mathbf{A}$ is a $N\times{M}$ matrix and $\mathbf{b}$ is a $N\times{1}$ (constant) vector. 

````



## Duality
Every linear programming problem, referred to as a primal problem, can be converted into a dual problem, which provides an upper bound to the optimal value of the primal problem. 

## Simplex algorithm
Fill me in.

---

## Summary
Fill me in