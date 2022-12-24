# Combitorial Optimization

## Introduction
Fill me in

---

## Dynamic programming
Dynamic programming is an approach developed by [Richard Bellman in the 1950s](https://en.wikipedia.org/wiki/Richard_E._Bellman) that is used in numerous fields ranging from engineering to economics. 

Dynamic programming recursively decomposes complicated problems by breaking them down into simpler smaller sub-problems. While some problems cannot be decomposed in this way, problems that span several points in time or stages can often be decomposed recursively. If a problem can be solved optimally by breaking it into sub-problems and then recursively finding the optimal solutions to the sub-problems, then it is said to have _optimal substructure_.

If dynamic programming methods are applicable, then there is a relationship between the value of the larger problem and the values of the sub-problems; this relationship is called the [Bellman equation](https://en.wikipedia.org/wiki/Bellman_equation).

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