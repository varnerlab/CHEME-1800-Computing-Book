# Linear Programming

## Introduction
In this lecture, we'll disucss linear programming.  Linear programming (LP) is a method to estimate the best outcome (such as maximum profit, lowest cost, or the best possible rate of production) using a mathematical model composed of linear relationships subject to linear constraints. Linear programming is widely used in business, economics, engineering, and other fields to model and solve real-world problems involving resource allocation, production planning, transportation, and more.

First, we'll introduce the mathematical structure of linear programs, and then move to an important application area within chemical Engineering, namely flux balance analysis:

* {ref}`content:references:primal-linear-problem` are a type of linear programming problem in which the goal is to maximize or minimize a linear objective function subject to a set of linear constraints in the form of inequalities or equalities. The term primal refers to the original form of the linear programming problem, as opposed to its dual form.

* {ref}`content:references:dual-linear-problem` are formulations derived from a primal linear program that provides an alternative way of looking at the same problem. The dual linear program is constructed by introducing a set of new variables and constraints that are related to the primal linear program. The objective of the dual program is to either maximize or minimize a function, subject to a different set of constraints, which are derived from the original primal program.

* {ref}`content:references:flux-balance-analysis` is a general tool that can be used to estimate _flows_ through many different types of networks and graphs, e.g., social graphs, communication networks, or other types of problems that can be represented as a network of graphs. Flux balance analysis can be implemented as a linear program.

---

(content:references:primal-linear-problem)=
## Primal linear programs
Linear programs maximize (or minimize) a _linear_ objective function $\mathcal{O}$ subject to _linear_ constraints and bounds on the variables we are searching over ({prf:ref}`defn-primael-linear program`): 

````{prf:definition} Primal Linear Program
:label: defn-primael-linear program

Let $\mathcal{O}$ denote a linear function of continuous decision variables $\mathbf{x}\in\mathbb{R}^{m}$ whose values are constrained by a system of linear equations with system matrix $\mathbf{A}\in\mathbb{R}^{n\times{m}}$, and bounded. Then, we can estimate the _optimal_ value for the variables $\mathbf{x}$ by solving the linear program (canonical form):

$$
\begin{eqnarray}
\text{maximize}~\mathcal{O} &=& \sum_{i=1}^{m} c_{i}x_{i}\\
\text{subject to}~\mathbf{Ax} &\leq&\mathbf{b}\\
\text{and}~x_{i}&\geq&{0}\qquad{i=1,2,\dots,m}
\end{eqnarray}
$$


The components of $\mathbf{x}$ are the variables to be determined, $c_{i}$ are constant coefficients in the _linear_ objective function $\mathcal{O}$, $\mathbf{A}\in\mathbb{R}^{n\times{m}}$ is the constraint matrix and $\mathbf{b}\in\mathbb{R}^{n}$ is a constant vector. 

Any linear program can be converted into this standard form.
````

Because of its general structure, linear programming provides a powerful tool for optimizing complex decision-making problems in various industries, allowing for better resource utilization, efficiency, and increased profitability. 

### Simplex algorithm
Linear programs, even those with thousands of decisions variables, can be efficently solved using simple off-the-shelf hardware, e.g., your laptop using the [simplex algorithm](https://optimization.cbe.cornell.edu/index.php?title=Simplex_algorithm). The simplex algorithm is a widely-used algorithm to solve the Linear Programming (LP) optimization problems first developed by [George Dantzig](https://en.wikipedia.org/wiki/George_Dantzig).

#### How does the simplex algorithm work
The [simplex algorithm](https://optimization.cbe.cornell.edu/index.php?title=Simplex_algorithm) is an iterative procedure for solving linear programming problems. The basic idea behind the simplex algorithm is to start with an initial feasible solution (i.e., a point that satisfies all constraints) and then iteratively improve it by moving along the edges of the feasible region (i.e., the region defined by the constraints). At each iteration, the algorithm selects a non-basic variable (i.e., a variable not currently part of the solution). It determines whether increasing or decreasing it will improve the objective function. If such an improvement can be made, the algorithm swaps the non-basic variable into the solution and updates the values of the basic variables (i.e., the variables already part of the solution). The process is repeated until no further improvement can be made, at which point the algorithm terminates, and the current solution is deemed optimal.

#### Aside: Showing up late is not always bad
As an aside, the [simplex algorithm](https://optimization.cbe.cornell.edu/index.php?title=Simplex_algorithm) resulted from [Dantzig](https://en.wikipedia.org/wiki/George_Dantzig) showing up late to class. In 1939, near the beginning of a class, [Professor Neyman](https://en.wikipedia.org/wiki/Jerzy_Neyman), an instructor for a class that [Dantzig](https://en.wikipedia.org/wiki/George_Dantzig) was taking, wrote two problems on the blackboard. [Dantzig](https://en.wikipedia.org/wiki/George_Dantzig) arrived late and assumed they were homework problems. According to [Dantzig](https://en.wikipedia.org/wiki/George_Dantzig), the problem set seemed more complicated than usual, but a few days later, he handed in solutions for both problems, believing that his assignment was late. 

However, six weeks later, an excited [Professor Neyman](https://en.wikipedia.org/wiki/Jerzy_Neyman) told [Dantzig](https://en.wikipedia.org/wiki/George_Dantzig) that the homework problems he had solved by mistake were two of the most famous unsolved problems in statistics. The rest is history; the simplex algorithm was born!


<!-- [The development of the simplex algorithm](https://optimization.cbe.cornell.edu/index.php?title=Simplex_algorithm), an efficient approach for solving linear programs, has led to LPs being used to solve problems in many engineering applications and other diverse industries such as banking, education, forestry, energy, and logistics.  -->

(content:references:dual-linear-problem)=
## Dual linear programs
The dual linear program, which is derived from the primal linear program, provides an alternative way of looking at the same problem. The dual linear program is constructed by introducing a set of new variables and constraints that are related to the primal linear program. Every primal linear programming problem can be converted into its dual problem, which provides an upper bound to the optimal value of the primal problem. 

The dual of a given linear program (LP) is another linear program that is derived from the original (the primal problem) in using the scheme:

* Each variable in the primal linear program becomes a constraint in the dual linear program
* Each constraint in the primal linear program becomes a variable in the dual linear program
* The objective direction is inversed – maximum in the primal becomes minimum in the dual and vice versa

Given the primal linear programming problem above, the dual problem is given by ({prf:ref}`defn-dual-linear program`):

````{prf:definition} Dual linear program
:label: defn-dual-linear program

Let $\mathcal{O}^{\prime}$ denote a linear function of continuous decision variables $\mathbf{y}\in\mathbb{R}^{n}$ whose values are constrained by a system of linear equations, and bounded. Then, we can estimate the _optimal_ value for the variables $\mathbf{y}$ of the dual problem by solving the linear program:

$$
\begin{eqnarray}
\text{minimize}~\mathcal{O}^{\prime} &=& \sum_{i=1}^{n} b_{i}y_{i}\\
\text{subject to}~\mathbf{A}^{T}\mathbf{y} &\geq&\mathbf{c}\\
\text{and}~y_{i}&\geq&{0}\qquad{i=1,2,\dots,n}
\end{eqnarray}
$$

__Differences between the primal and the dual problems__: The vectors $\mathbf{c}$ and $\mathbf{b}$ switch places, where the 
$c_{j}$ coefficients become the right-hand side vector in the dual, while the $b_{i}$ are now in the objective function. Finally, the less than or equal to constraints in the primal problem becomes greater than or equal to in the dual problem.
````

The duality theorem has an economic interpretation. If we interpret the primal linear program as a classical resource allocation problem, then its dual can be interpreted as a resource valuation problem.

(content:references:flux-balance-analysis)=
## Flux balance analyis
Flux balance analysis is a general tool to estimate _flows_ through [trees and graphs](../unit-2-data/trees.md), e.g., social graphs, communication networks, or other structures that can be represented as a network. In chemical engineering, flux balance analysis estimates chemical reaction rates (called _fluxes_) throughout _steady state_ reaction networks. The standard flux balance analysis problem is written in concentration units, e.g., the reaction flux has units of mmol/volume-time. However, sometimes it is more convenient to work in mole units instead.

### Mole-based problem formulation
We know from our [earlier discussion of material balances](../unit-2-data/vectors-matricies-nla.md) that the open species mole balance around component $i$ in a steady-state system with species $\mathcal{M}$, streams $\mathcal{S}$ and reactions $\mathcal{R}$ is given by:

$$\sum_{s\in\mathcal{S}}v_{s}\dot{n}_{is} + \sum_{j\in\mathcal{R}}\sigma_{ij}\dot{\epsilon}_{j} = 0\qquad\forall{i}\in\mathcal{M}$$

If we don't have [kinetic rate laws](https://en.wikipedia.org/wiki/Rate_equation), or we are not approaching this problem from the [equilibrium or energy perspective](https://en.wikipedia.org/wiki/Chemical_equilibrium), then we need some other way to estimate the extent of reaction $\dot{\epsilon}_{j}$, consider {prf:ref}`obs-simple-mole-based case`:

````{prf:observation} Mole based problem formulation
:label: obs-simple-mole-based case

Suppose we have an open steady-state system with species $\mathcal{M}$, streams $\mathcal{S}$ and reactions $\mathcal{R}$, but qith only a single stream entering (s = 1) and exiting (s = 2) the system. Further, suppose we want to maximize/minimize an objective of the form:

```{math}
\text{maximize/minimze}~\mathcal{O}=\sum_{i\in\mathcal{R}}c_{i}\dot{\epsilon}_{i}
```
In other words, we want to estimate the open extent such that some _technological_ objective is met, e.g., we maximize the production of a valuable product. For the case of a single input and output, the open mole balance becomes:

$$\dot{n}_{i,2} = \dot{n}_{i,1} + \sum_{j\in\mathcal{R}}\sigma_{ij}\dot{\epsilon}_{j}\qquad\forall{i}\in\mathcal{M}$$

Of course, when searching for the optimal set of reaction extents $\dot{\epsilon}_{j}$ we have to select values that give physically realistic answers, e.g., we can't have a negative mole flow rates: $\dot{n}_{i,j}\geq{0}$ for all $i$ and $j$. Thus, the species mole balances are linear constraints governing  the open extent: 

$$\dot{n}_{i,1} + \sum_{j\in\mathcal{R}}\sigma_{ij}\dot{\epsilon}_{j}\geq{0}\qquad\forall{i}\in\mathcal{M}$$

Next, the $\dot{\epsilon}_{j}$ terms must be bounded, i.e., we can't have infinite values for the open reaction extent, nor can irreversible reactions be chosen to run backward. Thus, the extents are _bounded_ from above and below:

$$\mathcal{L}_{j}\leq\dot{\epsilon}_{j}\leq\mathcal{U}_{j}\qquad{j=1,2\dots,\mathcal{R}}$$

The $\mathcal{L}_{j}$ and $\mathcal{U}_{j}$ denote the lower and upper bounds that $\dot{\epsilon}_{j}$ can take. Open extents $\dot{\epsilon}_{j}$ are just reaction rates times the volume; the lower and upper bounds describe the permissible range we expect the rate _could_ take.

````

Putting these ideas together for the general case of multiple input and output streams gives a general problem formulation to compute the flux through a reaction network using mole based units ({prf:ref}`defn-mol-based-units`):

````{prf:definition} Mole based flux problem
:label: defn-mol-based-units

We have an open system with species $\mathcal{M}$, streams $\mathcal{S}$, and reactions $\mathcal{R}$. Further, we partition the stream set $\mathcal{S}$ into streams entering the system $\mathcal{S}^{+}$, and streams leaving the system $\mathcal{S}^{-}$.  Then, the steady-state species mole balances are given by:

```{math}
\sum_{s\in\mathcal{S}^{+}}\dot{n}_{is} - \sum_{k\in\mathcal{S}^{-}}\dot{n}_{ik} + \sum_{j\in\mathcal{R}}\sigma_{ij}\dot{\epsilon}_{j} = 0\qquad\forall{i}\in\mathcal{M}
```

Finally, $\dot{n}_{i,j}\geq{0}$ for every $i$ and $j$; species mole flows must be non-negative. Then, the (unknown) open extents $\dot{\epsilon}_{j}$ are the solution of a linear programming problem in which the linear objective $\mathcal{O}$:

$$\text{maximize/minimize}~\mathcal{O} = \sum_{j\in\mathcal{R}}c_{j}\dot{\epsilon}_{j}$$

is minimized (or maximized) subject to the linear constraints:

$$\begin{eqnarray}
\sum_{j\in\mathcal{R}}\sigma_{ij}\dot{\epsilon}_{j}&\geq&{-\sum_{s\in\mathcal{S}^{+}}\dot{n}_{si}}\qquad\forall{i}\in\mathcal{M}\\
\sum_{k\in\mathcal{S}^{-}}\dot{n}_{ki}&\geq&{0}\qquad\forall{i}\in\mathcal{M}\\
\mathcal{L}_{j}&\leq\dot{\epsilon}_{j}\leq&\mathcal{U}_{j}\qquad\forall{j}\in\mathcal{R}
\end{eqnarray}$$
````

---

## Summary
In this lecture, we disucssed linear programming (LP).  Linear programming is a method to estimate the best outcome (such as maximum profit, lowest cost, or the best possible rate of production) using a mathematical model composed of linear relationships subject to linear constraints. Linear programming is widely used in business, economics, engineering, and other fields to model and solve real-world problems involving resource allocation, production planning, transportation, and more.

First, we introduced the mathematical structure of linear programs, the dual problem, and then moved to an important application area within chemical Engineering, namely flux balance analysis:

* {ref}`content:references:primal-linear-problem` are a type of linear programming problem in which the goal is to maximize or minimize a linear objective function subject to a set of linear constraints in the form of inequalities or equalities. The term primal refers to the original form of the linear programming problem, as opposed to its dual form.

* {ref}`content:references:dual-linear-problem` are formulations derived from a primal linear program that provides an alternative way of looking at the same problem. The dual linear program is constructed by introducing a set of new variables and constraints that are related to the primal linear program. The objective of the dual program is to either maximize or minimize a function, subject to a different set of constraints, which are derived from the original primal program.

* {ref}`content:references:flux-balance-analysis` is a general tool that can be used to estimate _flows_ through many different types of networks and graphs, e.g., social graphs, communication networks, or other types of problems that can be represented as a network of graphs. Flux balance analysis can be implemented as a linear program.

### Additional resources
Background resources for biochemical network information, and computational tools for working with flux balance analysis models:

* [Kanehisa M, Goto S. KEGG: kyoto encyclopedia of genes and genomes. Nucleic Acids Res. 2000 Jan 1;28(1):27-30. doi: 10.1093/nar/28.1.27. PMID: 10592173; PMCID: PMC102409.](https://www.genome.jp/kegg/)

* [Karp, Peter D et al. "The BioCyc collection of microbial genomes and metabolic pathways." Briefings in bioinformatics vol. 20,4 (2019): 1085-1093. doi:10.1093/bib/bbx085](https://pubmed.ncbi.nlm.nih.gov/29447345/)

* [Gama-Castro, Socorro et al. "RegulonDB version 9.0: high-level integration of gene regulation, coexpression, motif clustering and beyond." Nucleic acids research vol. 44,D1 (2016): D133-43. doi:10.1093/nar/gkv1156](https://pubmed.ncbi.nlm.nih.gov/26527724/)

* [Heirendt, Laurent et al. "Creation and analysis of biochemical constraint-based models using the COBRA Toolbox v.3.0." Nature protocols vol. 14,3 (2019): 639-702. doi:10.1038/s41596-018-0098-2](https://pubmed.ncbi.nlm.nih.gov/30787451/)