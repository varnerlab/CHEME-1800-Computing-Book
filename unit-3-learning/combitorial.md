# Dynamic Progamming and Heuristic Optimization

## Introduction
Dynamic programming, heuristic optimization, and combinatorial optimization are all techniques used to solve optimization problems. In this lecture we'll introduce:

<!-- Dynamic programming is a method that solves problems by breaking them down into smaller subproblems and solving them independently, then combining the solutions to the subproblems to solve the original problem. On the other hand, heuristic optimization algorithms are a family of algorithms inspired by natural phenomena and human behavior. They explore the search space using rules of thumb, intuition, and trial-and-error methods to find the best solution to a problem. Finally, combinatorial optimization is a field that deals with discrete optimization problems, where the search space consists of discrete objects or structures. There are many different approaches to solving these optimization problems, including exact algorithms, approximation algorithms, and heuristics. -->


* {ref}`content:references:dynamic-programming` solves optimization problems by breaking them down into smaller subproblems, solving each subproblem once, and storing the solutions in a table or array. It is typically used for problems that can be divided into similar subproblems and for which the optimal solution can be constructed from optimal solutions to the subproblems.

* {ref}`content:references:heuristic-optimization` is a class of algorithms used to solve complex optimization problems, inspired by natural phenomena and human behavior. These algorithms are particularly useful when mathematical models are not available or are too expensive to compute. 

* {ref}`content:references:branch-and-bound` is an algorithmic technique for solving combinatorial optimization problems. It involves dividing the search space into smaller subproblems and then using bounds on the optimal solutions to prune the search space and avoid considering suboptimal solutions.

---

(content:references:dynamic-programming)=
## Dynamic programming
Dynamic programming is an approach developed by [Richard Bellman in the 1950s](https://en.wikipedia.org/wiki/Richard_E._Bellman) that solves problems by breaking them down into smaller subproblems and storing the solutions to these subproblems. This allows the program to avoid recomputing the solution to the same subproblem multiple times and instead reuse the stored solutions, which can significantly improve the algorithm’s efficiency. 

Dynamic programming is typically used for problems that can be divided into overlapping subproblems, which means that the solution to one subproblem can be used to solve other subproblems. This is often the case with optimization problems, where the goal is to find the optimal solution among a set of possible solutions. It is also true for _sequential decision problems_.

### Dynamic decision problems
Imagine we have a decision making agent, in some state $x_{t}$, at time $t$. At time $t$, the agent takes an action $a_{t}$ from a set of possible actions $a_{t}\in\mathcal{A}\left(x_{t}\right)$ that leads to a new state $x_{t+1} = T(x_{t},a_{t})$ and a payoff $F(x_{t},a_{t})$. In this situation, an infinite-horizon descision problem takes the form:

$$
\begin{eqnarray}
V(x_{\circ}) & = & \max_{a\in\mathcal{A}} \sum_{t=0}^{\infty}
\beta^{t}F(x_{t},a_{t}) \\
\text{subject to}&~& a_{t}\in\mathcal{A}(x_{t}) \\
\text{and} &~& x_{t+1} = T(x_{t},a_{t})
\end{eqnarray}
$$

where the function $V(x_{\circ})$ is the _value function_, $x_{\circ}$ is the initial state of the agent and $0\leq\beta\leq{1}$ denotes the discount factor. Dynamic programming breaks this decision problem into smaller subproblems using Bellman's principle of optimality ({prf:ref}`obs-bellman-principle-of-optimality`):

````{prf:observation} Bellman's principle of optimality
:label: obs-bellman-principle-of-optimality

__Principle of optimality__: An optimal policy has the property that whatever the initial state and initial decision are, the remaining decisions must constitute an optimal policy with regard to the state resulting from the first decision.
````

The optimality principle suggests that we can pull the first decision from the sum, which gives a value function of the form:

$$
\begin{eqnarray}
V(x_{\circ}) & = & \max_{a_{\circ}\in\mathcal{A}} \left\{F(x_{\circ},a_{\circ}) + \beta\left[\max_{a_{t}\mathcal{A}}\sum_{t=1}^{\infty}\beta^{t-1}F(x_{t},a_{t})\right]\right\}\\
\text{subject to}&~& a_{t}\in\mathcal{A}(x_{t}) \\
\text{and} &~& x_{t+1} = T(x_{t},a_{t})
\end{eqnarray}
$$

(content:references:heuristic-optimization)=
## Heuristic optimization
Heuristic optimization is a family of algorithms inspired by natural phenomena and human behavior. Unlike traditional optimization methods, heuristic approaches use rules of thumb, intuition, and trial-and-error to explore the search space and find the best solution. Heuristic methods are often used to solve complex, real-world problems where exact solutions are difficult to obtain or may not even exist.

### Simulated annealing 
Simulated annealing is a heuristic optimization algorithm inspired by the annealing process in metallurgy. It solves complex optimization problems by iteratively exploring the search space and gradually reducing the search space size. Simulated annealing works by randomly selecting a new solution and evaluating its fitness compared to the current best solution and then accepting or rejecting the new solution based on a probability function. Simulated annealing is especially good at finding near-optimal solutions to problems with many local optima.

Pseudo code for a simulated annealing algorithm is given in {prf:ref}`algo-simulated-annealing`:

````{prf:algorithm} Simulated Annealing
:label: algo-simulated-annealing
:class: dropdown

**Inputs** The objective function $f$, temperature, neighbor, and accept functions, initial guess $x_{\circ}$, initial temperature $T$, and max iterations $\mathcal{M}_{\infty}$

**Outputs** the $\min_{x}f(x)$ and $\arg\min_{x}f(x)$.

**Initialize** 
1. Set current solution $x^{\prime}\leftarrow{x}_{\circ}$
1. Set best solution $\hat{x}\leftarrow{x}_{\circ}$
1. Set initial temperature $T_{0}\leftarrow{T}$

**Main**
1. for $i\in\left\{1,\dots,\mathcal{M}_{\infty}\right\}$
    1. update temperature $T_{i}\leftarrow\text{temperature}(T_{i-1},i)$
    1. generate new solution $x^{\dagger}\leftarrow\text{neighbor}(x^{\prime})$
    1. compute objective function difference $\Delta_{i} = f(x^{\dagger}) - f(x^{\prime})$
    
    1. if $\Delta<0 ~\text{or}~\text{accept}(\Delta_{i},T_{i}) = \text{true}$
        1. update the current solution ${x^{\prime}}\leftarrow{x}^{\dagger}$
    
    1. if $f(x^{\prime}) < f(\hat{x})$
        1. update the best solution $\hat{x}\leftarrow{x}^{\prime}$

**Return**
the tuple $f(\hat{x})$ and $\hat{x}$.
````

The performance of simulated annealing depends upon the choice of the temperature, neighbor, and accept functions. Example pseudo code implementations for these functions is shown in {prf:ref}`algo-simulated-annealing-other-functions`:

````{prf:algorithm} Example temperature, neighbor, and accept functions
:label: algo-simulated-annealing-other-functions
:class: dropdown

**Temperature function**
1. function temperature($T$, $i$):
    1. return $T\leftarrow\alpha\times{T}~$ where $\alpha<1$

**Accept function**
1. function accept($\Delta$, $T$):
    1. if $\Delta < 0$:
        1. return true
    1. if random(0, 1) < $\exp\left(-\Delta/T\right)$:
        1. return true
1. return false

**Neighbor function**
1. function neighbor(solution):
    1. set new_solution = copy(solution)
    1. select random move
    1. perform move on new_solution
1. return new_solution


````



### Genetic algortihms
Genetic algorithms are heuristic optimization algorithms inspired by natural selection and genetic inheritance. Genetic algorithms solve problems by iteratively generating and evaluating a population of candidate solutions, then applying selection, crossover, and mutation operators to evolve the population toward better solutions. The algorithm’s performance depends on the population size, selection operators, and genetic operators used, and it can find reasonable solutions quickly in large and complex search spaces. Genetic algorithms can handle continuous and discrete search spaces and are often used in complex optimization problems such as scheduling, routing, and machine learning.


<!-- 
While some problems cannot be decomposed in this way, problems that span several time points or naturally structured in stages can often be decomposed recursively. If a problem can be solved optimally by breaking it into subproblems and then recursively finding the optimal solutions to the subproblems, then it is said to have _optimal substructure_.

If dynamic programming methods are applicable, then there is a relationship between the value of the larger problem and the values of the subproblems; this relationship is called the [Bellman equation](https://en.wikipedia.org/wiki/Bellman_equation). 

Dynamic programming can be a valuable tool for solving many problems, but it can be computationally intensive and may only sometimes be the most efficient solution. When deciding whether to use dynamic programming, it is essential to consider the trade-offs between the algorithm’s efficiency and the problem’s complexity. -->

(content:references:branch-and-bound)=
## Branch and Bound
Branch and bound is an approach to solve a variety of discrete and combinatorial optimization problems. A branch-and-bound algorithm enumerates candidate solutions by means of state space search: the set of candidate solutions forms a rooted tree with the full set at the root. 

Let's consider minimizing an arbitrary objective function $f$:

```{prf:algorithm} Branch and Bound Algorithm
:label: algo-branch-bound-generic
:class: dropdown

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
Dynamic programming, heuristic optimization, and combinatorial optimization are all techniques used to solve optimization problems:


<!-- 
Dynamic programming is a method that solves problems by breaking them down into smaller subproblems and solving them independently, then combining the solutions to the subproblems to solve the original problem. On the other hand, heuristic optimization algorithms are a family of algorithms inspired by natural phenomena and human behavior. They explore the search space using rules of thumb, intuition, and trial-and-error methods to find the best solution to a problem. Finally, combinatorial optimization is a field that deals with discrete optimization problems, where the search space consists of discrete objects or structures. There are many different approaches to solving these optimization problems, including exact algorithms, approximation algorithms, and heuristics.

In this lecture we introduced several optimization approaches: -->

* {ref}`content:references:dynamic-programming` solves optimization problems by breaking them down into smaller subproblems, solving each subproblem once, and storing the solutions in a table or array. It is typically used for problems that can be divided into similar subproblems and for which the optimal solution can be constructed from optimal solutions to the subproblems.

* {ref}`content:references:heuristic-optimization` is a class of algorithms used to solve complex optimization problems, inspired by natural phenomena and human behavior. These algorithms are particularly useful when mathematical models are not available or are too expensive to compute. 

* {ref}`content:references:branch-and-bound` is an algorithmic technique for solving combinatorial optimization problems. It involves dividing the search space into smaller subproblems and then using bounds on the optimal solutions to prune the search space and avoid considering suboptimal solutions.