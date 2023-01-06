# Combinatorial Optimization

## Introduction
Combinatorial optimization is a branch of computer science and mathematics that deals with the optimization of combinatorial objects, such as graphs, functions, and strings. It involves finding the best solution to a problem among a finite or countably infinite set of possible solutions. Combinatorial optimization has many applications in computer science, engineering, and other fields.

There are many different approaches to solving combinatorial optimization problems, including exact algorithms, approximation algorithms, and heuristics. The choice of method depends on the specific problem and the desired level of accuracy and efficiency. 

In this lecture we'll introduce two approaches for the solution of Combinatorial optimization problems:

* {ref}`content:references:dynamic-programming` solves optimization problems by breaking them down into smaller subproblems, solving each subproblem once, and storing the solutions in a table or array. It is typically used for problems that can be divided into similar subproblems and for which the optimal solution can be constructed from optimal solutions to the subproblems.

* {ref}`content:references:branch-and-bound` is an algorithmic technique for solving combinatorial optimization problems. It involves dividing the search space into smaller subproblems and then using bounds on the optimal solutions to prune the search space and avoid considering suboptimal solutions.

---

(content:references:dynamic-programming)=
## Dynamic programming
Dynamic programming is an approach developed by [Richard Bellman in the 1950s](https://en.wikipedia.org/wiki/Richard_E._Bellman) that solves problems by breaking them down into smaller subproblems and storing the solutions to these subproblems. This allows the program to avoid recomputing the solution to the same subproblem multiple times and instead reuse the stored solutions, which can significantly improve the algorithm’s efficiency. Dynamic programming is used in numerous fields ranging from engineering to economics.

Dynamic programming is typically used for problems that can be divided into overlapping subproblems, which means that the solution to one subproblem can be used to solve other subproblems. This is often the case with optimization problems, where the goal is to find the optimal solution among a set of possible solutions. While some problems cannot be decomposed in this way, problems that span several time points or naturally structured in stages can often be decomposed recursively. If a problem can be solved optimally by breaking it into subproblems and then recursively finding the optimal solutions to the subproblems, then it is said to have _optimal substructure_.

If dynamic programming methods are applicable, then there is a relationship between the value of the larger problem and the values of the subproblems; this relationship is called the [Bellman equation](https://en.wikipedia.org/wiki/Bellman_equation). 

Dynamic programming can be a valuable tool for solving many problems, but it can be computationally intensive and may only sometimes be the most efficient solution. When deciding whether to use dynamic programming, it is essential to consider the trade-offs between the algorithm’s efficiency and the problem’s complexity.

(content:references:branch-and-bound)=
## Branch and Bound
Branch and bound is an approach to solve a variety of discrete and combinatorial optimization problems. A branch-and-bound algorithm enumerates candidate solutions by means of state space search: the set of candidate solutions forms a rooted tree with the full set at the root. 

Let's consider minimizing an arbitrary objective function $f$:

```{prf:algorithm} Branch and Bound Algorithm
:label: algo-branch-bound-generic

**Inputs** the objective function $f$, a branch function, and a bound function

**Outputs** the minimum function value $\mathcal{B}$

**Initialize** 
1. Set $\mathcal{B}\leftarrow\infty$ where $\mathcal{B}$ is the best solution found so far
2. Initialize $Q$, a queue holding problem variables

**Main**
1. while $Q$ is not empty
    1. Take a node $N$ from the queue: $N\leftarrow\text{dequeue}(Q)$
    1. Evaluate the objective function value of node $N$: $B_{N}\leftarrow{f(x_{N})}$  
    1. if $B_{N}<\mathcal{B}$
        1. Update best solution: $\mathcal{B}\leftarrow{B_{N}}$
    1. else
        1. Branch on node $N$ to produce new candidate nodes: $(N_{1},\dots) \leftarrow\text{branch}(N)$
        1. for N $\in\left(N_{1},\dots\right)$
            1. if bound(N) $\leq\mathcal{B}$
                1. Q $\leftarrow$ enqueue(Q, N)

**Return**
the minimum function value $\mathcal{B}$
```

---

## Summary
Fill me in