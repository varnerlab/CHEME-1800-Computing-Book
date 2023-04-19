# Probability and Uncertain Choices

Uncertain decisions are those that involve a certain degree of risk or ambiguity. Uncertain decisions arise in many situations, such as investing in the stock market, choosing a career path, making technical choices, or deciding whether to pursue a romantic relationship. Making uncertain decisions involves weighing each option’s potential benefits and drawbacks and considering the likelihood of different outcomes.

In this lecture, we introduce tools to model and analyze simple uncertain decisions:

* {ref}`content:references:utility-and-utlity-max` is the process of selecting the option that provides the highest level of satisfaction, or utility, based on the individual's preferences and constraints. This concept is often used in economics to model consumer behavior and in decision theory to analyze choices under uncertainty.

* {ref}`content:references:utility-and-uncetain-decisions` are decision-making situations where the outcomes of different options are unknown or uncertain. These types of decisions often involve risk and involve balancing the potential rewards and risks of different options. To make optimal choices under uncertainty, decision-makers may use probabilistic tools such as expected utility theory to analyze the expected outcomes of different options.

---

(content:references:utility-and-utlity-max)=
## Utility maximization
A utility function is a mathematical expression representing an individual's preferences over different choices or outcomes. It assigns a numerical value, called _utility_, to each possible option based on how much the individual values it. A utility function is subjective and can vary from person to person, as different individuals may have other preferences.

[Rational choice theory](https://en.wikipedia.org/wiki/Rational_choice_theory) assumes individuals make rational decisions based on their preferences and available information. [Rational choice theory](https://en.wikipedia.org/wiki/Rational_choice_theory) is based on computing the utility of actions or outcomes. Thus, utility functions are a crucial component of this theory, as they describe how individuals assign values to different results or choices ({prf:ref}`defn-individual-utility`):

````{prf:definition} Ordinal utility function
:label: defn-individual-utility

An individual decision-making agent is presented with a bundle of $n$ objects $x_{1},\dots,x_{n}$. A utility function represents the preference of the agent for combinations of these $n$ objects:

```{math}
:label: eqn-utility-function-generic
U(x_{1},x_{2},\dots,x_{n})
```

The utility function $U(\dots)$ is unique only up to an order-preserving transformation in period $t\rightarrow{t+dt}$. Further, utility functions are ordinal, i.e., they rank-order bundles but do not indicate how much better a bundle is compared to another.

````

{prf:ref}`defn-individual-utility` does not give specifics about the properties of a utility function. However, it does establish a critical concept; the utility can be used to rank order the preference for different choices. For example, suppose we have two choices, $A$ and $B$:

* If $A\succ{B}$, the decision maker _strictly prefers_ $A$ to $B$. Then, the utility of choice $A$ is greater than $B$, or $U(A)>U(B)$.
* If $A\sim{B}$, the decision maker is _indifferent_ between $A$ to $B$. Then, the utility of choice $A$ is the same as $B$, or $U(A)=U(B)$.
* If $A\succsim{B}$, the decision maker _weakly prefers_ $A$ over $B$, or they are indifferent. Then, the utility of choice $A$ is greater than or equal to $B$, or $U(A)\geq{U(A)}$.

### Properties of utility functions
A utility function is a mathematical representation of a person's preferences over different outcomes or alternatives. Here are some properties that a utility function should possess:

* __Completeness__: A utility function should be able to rank all possible outcomes or alternatives. In other words, for any two outcomes, the utility function should tell us which one is preferred or whether they are equally preferred.
* __Transitivity__: If outcome A is preferred to outcome B, and outcome B is preferred to outcome C, then outcome A must be preferred to outcome C. This is known as the transitivity property.
* __Monotonicity__: If more of a good is always preferred to less, then the utility function should increase in that good. Conversely, if less of a bad is always preferred to more, then the utility function should decrease in that bad.
* __Continuity__: Small changes in the outcome or alternatives should result in small changes in the utility function. This is known as the continuity property.
* __Concavity__: The utility function should be concave if the person exhibits diminishing marginal utility. This means that as a person consumes more of a good, the additional satisfaction gained from each unit consumed decreases.

These properties help ensure that a utility function accurately represents a person's preferences and can be used to make rational choices between alternatives.

#### Marginal utility
The marginal utility is the additional satisfaction or usefulness an agent derives from consuming one additional unit of a good or service. Mathematically, the marginal utility is the partial derivative of the utility with respect to the amount of item $i$ ({prf:ref}`defn-marginal-utility-math`):

````{prf:definition} Marginal utility
:label: defn-marginal-utility-math

The preference or satisfaction derived from a bundle of objects $x_{1},\dots,x_{n}$ is described by the utility function $U(x_{1},\dots,x_{n})$. Then, the satisfaction experienced by the agent from one additional unit of the object $i$ in the bundle is defined as the marginal utility of object $i$:

```{math}
\text{MU}^{\star}_{i} \equiv \left(\frac{\partial{U}}{\partial{x_{i}}}\right)_{\star}\qquad{i=1,2,\dots,n}
```

where all other objects are held constant at level $\star$.
````

##### Analytical marginal utility 
The marginal utility can be computed by directly differentiating the utility function. For example, consider an example utility function that we'll use later, the [Cobb-Douglas utility function](https://en.wikipedia.org/wiki/Cobb–Douglas_production_function). The [Cobb–Douglas utility function](https://en.wikipedia.org/wiki/Cobb–Douglas_production_function) governing the satisfaction gained by consuming a bundle of $n$ goods $(x_{1},x_{2},\dots,x_{n})$ is given by:

```{math}
:label: eqn-cobb-douglas-u-function
U(x)  = \prod_{i=1}^{n}x_{i}^{\alpha_{i}} 
```

where the coefficients $\alpha_{i}$ are governed by:

```{math}
\sum_{i=1}^{n}\alpha_{i}  = 1
```

The [Cobb–Douglas utility function](https://en.wikipedia.org/wiki/Cobb–Douglas_production_function) satisfies the utility function properties, where the marginal utility is given by: 

```{math}
:label: eqn-mu-u-function

\text{MU}_{x_{i}} = \left(\alpha_{i}x^{\alpha_{i}-1}\right)\cdot\left(\prod_{j=1,i}^{n}x_{j}^{\alpha_{j}}\right)
```

where the $j=1,i$ notation denotes the _exclusion_ of index $i$. As $x_{i}\rightarrow\infty$, the marginal utility $\text{MU}_{x_{i}}\rightarrow{0}$ if $\alpha_{i}<1$. Thus, the convexity property is satisfied for $\alpha_{i}<1$.

To compute analytical expressions for the marginal utility, we could remember the [differentiation rules from calculus](https://en.wikipedia.org/wiki/Differentiation_rules), or we can have the computer do it for us, using one of many [Computer Algebra Systems (CAS)](https://en.wikipedia.org/wiki/Computer_algebra_system), e.g., the [Symbolics.jl](https://github.com/JuliaSymbolics/Symbolics.jl) package in [Julia](https://julialang.org) or the [SymPy](https://www.sympy.org/en/index.html) library in [Python](https://www.python.org).

```julia
```

##### Numerical marginal utility 
However, sometimes it may not be convenient to analytically compute the derivative, e.g., the utility function is complicated. You can always approximate the marginal utility using a [finite difference approximation](https://en.wikipedia.org/wiki/Finite_difference), or [automatic differentiation](https://en.wikipedia.org/wiki/Automatic_differentiation) approach.

Sample code for [automatic differentiation](https://en.wikipedia.org/wiki/Automatic_differentiation) of the Cobb-Douglas utility function using the [ForwardDiff.jl](https://juliadiff.org/ForwardDiff.jl/stable/) package:

```julia
# load the ForwardDiff package
using ForwardDiff

# Cobb-Douglas utility function
function U(x)

    # initialize
    α₁ = 0.5;
    α₂ = 1.0 - α₁
    
    # return
    return (x[1]^α₁)*(x[2]^α₂)
end

# Estimate the MU -
x = [20.0,7.20]; # point A 
MU = ForwardDiff.gradient(U,x);
```

##### Aside: Marginal utility of wealth
The marginal utility quantifies the satisfaction gained by an agent from an additional unit of consumption of a good or service ({numref}`fig-wealth-schematic-simple-model`).

 ```{figure} ./figs/Fig-Wealth-Utility-Schematic.pdf
---
height: 400px
name: fig-wealth-schematic-simple-model
---
Wealth utility function $U(W) = W/(W+b)$ as a function of choices for the parameter $b$. The abundance of wealth is inversely proportional to the value of the parameter $b$. For example, the agent at $A$ values each additional wealth unit less than the agent at $D$.
```

The relationship between overall resource level (wealth) and the marginal utility of resources is a fundamental concept in economics that explains how individuals value resources and other material possessions. Marginal utility is basic to how an agent values a good or service, including wealth. According to this concept, the more wealth a person has, the less value each additional unit of wealth provides in terms of satisfaction or well-being. This is because as a person's wealth increases, the marginal utility of each additional wealth unit decreases due to the [diminishing marginal utility of money](https://en.wikipedia.org/wiki/Marginal_utility).


### Indifference curves
Indifference curves are graphical representations of combinations of choices that provide a decision-making agent with the same level of utility ({numref}`fig-cobb-douglas-ic`). Thus, decision-makers are _indifferent_ to the consumption of different combinations of goods (or services) on an indifference curve. 

 ```{figure} ./figs/Fig-CobbDouglas-IndifferenceCurves-Sqrt.pdf
---
height: 400px
name: fig-cobb-douglas-ic
---
Two-dimensional indifference curves were generated using the Cobb–Douglas utility function with $\alpha_{1} = \alpha_{2} = 0.5$. The decision maker is indifferent to a choice between point $A$ and $B$, i.e., $A\sim{B}$ or $C\sim{D}$. However, the decision maker strictly prefers $C$ and $D$ compared to $A$ and $B$, i.e., $C\sim{D}\succ{A}\sim{B}$.
```

For example, the decision agent with the Cobb–Douglas utility function shown in ({numref}`fig-cobb-douglas-ic`) is _indifferent_ to a choice between $A$ and $B$, but strictly prefers $C$ and $D$ to either $A$ or $B$. 

Sample code to compute the two-dimensional Cobb–Douglas utility function for $\alpha_{1} = \alpha_{2} = 0.5$:
```julia
# initialize
α₁ = 0.5 
α₂ = 1.0 - α₁

# Storage: holds indifference curves 
results = Dict{Int64,Array{Float64,2}}()

# Set values for the good and service 1
X1 = range(0.001,stop=100.0,step = 0.001) |> collect;
d = length(X1);

# Set utility values
U = [12.0, 24.0, 36.0, 48.0];
n = length(U);

# simulation loop
for i ∈ 1:n

    # Allocate storage for the indifference curve 
    Y = Array{Float64,2}(undef,d,2);
    U_val = U[i];

    # compute X2 from the X1 values
    for j ∈ 1:d

        # compute the log-transformed utility
        tmp = (1/α₂)*(log(U_val) - α₁*log(X1[j]));

        # store
        Y[j,1] = X1[j];
        Y[j,2] = exp(tmp); # inverse transform
    end

    # store -
    results[i] = Y;
end
```

### Marginal rate of substitution
How much of one good or service a decision maker is willing to trade for another is called the [marginal rate of substution](https://en.wikipedia.org/wiki/Marginal_rate_of_substitution) ({prf:ref}`defn-marginal-rate-of-sub`):

````{prf:definition} Marginal Rate of Substitution
:label: defn-marginal-rate-of-sub

A decision maker is analyzing the consumption of different combinations of $n$ goods or services $x_{1},\dots,x_{n}$. The utility 
function governing the decision maker $U(\dots)$ can be expanded by computing the [total differential](https://en.wikipedia.org/wiki/Differential_of_a_function#Differentials_in_several_variables) around some point $\left(x_{1}^{\star},\dots,x_{n}^{\star}\right)$ on an indifference curve with utility $c$:

```{math}
:label: eqn-total-differential-ic

dU = \sum_{i=1}^{n}\left(\frac{\partial{U}}{\partial{x_{i}}}\right)_{\star}dx_{i}
```

The partial derivative of the utility with respect to a change in the consumption of good or service $i$ is the [marginal utility](https://en.wikipedia.org/wiki/Marginal_utility):

```{math}
dU = \sum_{i=1}^{n}\left(\text{MU}_{i}^{\star}\right){dx_{i}}
```

The utility $U(\dots)=c$ on an indifference curve; thus, along an indifference curve $dU = 0$. The marginal rate of substitution of good or service $i$ for $j$ (all other quantities held constant):

```{math}
:label: eqn-total-differential-ic-constant

\text{MU}^{\star}_{i}dx_{i} + \text{MU}^{\star}_{j}dx_{j} = 0\qquad{i\neq{j}}
```

can be computed for good $i$ and $j$:

```{math}
:label: eqn-total-differential-ic-mrs
\text{MRS}^{\star}_{ij} = -\frac{dx_{j}}{dx_{i}} = \frac{\text{MU}^{\star}_{i}}{\text{MU}^{\star}_{j}}\qquad\forall\left(i,j\right)_{i\neq{j}}
```

````

### Optimal choices and budgets
An optimal decision-making agent _maximizes_ its utility function, i.e., the agent searches for a combination of goods and services that gives the highest satisfaction subject to various constraints, e.g., a budget constraint ({prf:ref}`eqn-budget-constraint`):

````{prf:definition} Maximum utility and budget constraints
:label: eqn-budget-constraint

A decision making agent has a utility function $U\left(x_{1},\dots,x_{n}\right)$ and $I$ dollars to spend between $t\rightarrow{t+dt}$. An optimal agent maximizes its utility subject to its budget:

```{math}
:label: eqn-max-ulity-problem

\begin{eqnarray}
\text{maximize}~\mathcal{O} &=& U\left(x_{1},\dots,x_{n}\right) \\
\text{subject to}~\sum_{i=1}^{n}c_{i}x_{i} & \leq & I\\
\text{and}~x_{i}&\geq&{0}\qquad{i=1,2,\dots,n}
\end{eqnarray}

```

where $c_{i}\geq{0}~\forall{i}$ denotes the cost of good or service $i$, and $x_{i}$ represents the amount of good or service purchased or consumed by the agent during time period $t\rightarrow{t+dt}$.

````

The problem in {prf:ref}`eqn-budget-constraint` can be solved for the unknown consumption levels of $x_{i}$ using various techniques. For example, we could use the [method of Lagrange multipliers](https://en.wikipedia.org/wiki/Lagrange_multiplier) or a [penalty method](https://en.wikipedia.org/wiki/Penalty_method) in combination with any number of heuristic optimization approaches, e.g., [simulated annealing](https://en.wikipedia.org/wiki/Simulated_annealing). Of course, if the utility function in {prf:ref}`eqn-budget-constraint` is linear, we could also use [linear programming](https://en.wikipedia.org/wiki/Linear_programming).

Let's do an example of a decision between two goods using a Cobb–Douglas utility function subject to a budget constraint ({prf:ref}`example-utility-max-budget`):

````{prf:example} Maximize utility subject to budget
:label: example-utility-max-budget
:class: dropdown

__Problem__: A decision making agent must decide how much of two goods to consume, $x_{1}$ and $x_{2}$, subject to a budget constraint. Good one costs 18.0 USD per unit, while good two costs 36.0 USD per unit. The agent has 100.0 USD to spend and is governed by a Cobb–Douglas utility function with $\alpha_{1} = 0.45$ and  $\alpha_{2} = 0.55$. Compute the optimal mixture of $x_{1}$ and $x_{2}$.

````


(content:references:utility-and-uncetain-decisions)=
## Choices under uncertainty
In the previous section, we developed tools to make optimal choices when the outcomes were sure, and the level of satisfaction derived from those choices was known, e.g., the utility of purchasing a particular bundle of goods or services could be computed using a utility function. However, in many real-world situations, the assumption of certainty is invalid. For example, betting, buying an insurance policy or investing in a new business, or the stock market all or _uncertain_. 

To understand how optimal agents behave when faced with uncertain situations, we introduce two critical concepts, random variables, and probability:

* A [random variable](https://en.wikipedia.org/wiki/Random_variable) is a variable that takes on different numerical values according to the outcome of a random event or process. There are two types of random variables: discrete random variables and continuous random variables. A discrete random variable can take on a countable number of distinct values, while a continuous random variable can take on any value in a continuous range. 
* [Probability](https://en.wikipedia.org/wiki/Probability) is a measure of the likelihood that a particular event or outcome will occur and is commonly used to quantify uncertainty in various fields, such as science, engineering, economics, and finance.

### Expectation
The [expectation](https://en.wikipedia.org/wiki/Expected_value) of a discrete random variable $X$ measures the central tendency of the values of that random variable ({prf:ref}`defn-discrete-random-variable-expectation`):

````{prf:definition} Expectation discrete random variable
:label: defn-discrete-random-variable-expectation

Let $X$ denote a discere random variable with the probability space $\left(\Omega,\mathcal{F},P\right)$, where $\Omega$ denotes the sample space, $\mathcal{F}$ denotes the event space, and $P$ denotes the probability measure. Then, the expected value of the random variable $X$ is given by:

```{math}
:label: eqn-expectation
\mathbb{E}\left[X\right] = \sum_{x\in\Omega}xp_{X}(x)
```

where $x$ denotes a value for the discrete random variable $X$, and $p_{X}(x)$ denotes the probability of $X=x$. The value of $p_{X}(x)$ is governed by a [Probability Mass Function](https://en.wikipedia.org/wiki/Probability_mass_function).

````

The expectation of a discrete random variable has a few interesting properties ({prf:ref}`obs-expectation-props`):

````{prf:observation} Properties of expectation
:label: obs-expectation-props
The expectation of a random variable $X$ has several useful (and important) properties: 
1. $\mathbb{E}\left(c\right) = c$ for any constant $c$
1. $\mathbb{E}\left(cX\right) = c\times\mathbb{E}\left(X\right)$ for any constant $c$
1. $\mathbb{E}\left(g(X)\right) = \sum_{x\in{X(\Omega)}}g(x)p_{X}(x)$
1. $\mathbb{E}\left(g(X)+h(X)\right) = \mathbb{E}(g(X)) + \mathbb{E}(h(X))$
1. $\mathbb{E}\left(X+c\right) = \mathbb{E}(X) + c$ for any constant $c$
````

### Variance
The [variance](https://en.wikipedia.org/wiki/Variance) measures the expected dispersion for
individual values of a random variable $X$, i.e., the average distance that values of $X$ are spread out from their expected value ({prf:ref}`defn-discrete-random-variable-variance`):

````{prf:definition} Expectation discrete random variable
:label: defn-discrete-random-variable-variance

Let $X$ denote a discrete random variable with the probability space $\left(\Omega,\mathcal{F},P\right)$, where $\Omega$ denotes the sample space, $\mathcal{F}$ denotes the event space, and $P$ denotes the probability measure. Then, the variance of the random variable $X$ is given by:

```{math}
:label: eqn-variance
\text{Var}(X) = \mathbb{E}\Bigl[(X-\mu)^{2}\Bigr]
```

where $\mu = \mathbb{E}(X)$ denotes the expected value of the random variable $X$.

````

The variance of a discrete random variable has a few interesting properties ({prf:ref}`obs-variances-var`):

````{prf:observation} Properties of variance
:label: obs-variances-var

The variance of a random variable $X$ has a few interesting (and important) properties:

* $\text{Var}(X) = \mathbb{E}\left(X^{2}\right) - \mathbb{E}\left(X\right)^2$
* $\text{Var}(cX) = {c^2}\text{Var}(X)$ for any constant $c$
* $\text{Var}(X+c) = \text{Var}(X)$ for any constant $c$

````

The more common quantity that is used to measure dispersion, the standard deviation $\sigma$, is related to the variance: $\sigma_{X} = \sqrt{\text{Var}(X)}$.

### Probability mass functions
In the case of discrete random variables, for example, dice roles, coin flips etc, this is done using a concept called a [probability mass function (PMF)](https://en.wikipedia.org/wiki/Probability_mass_function). 


````{prf:definition} Probability Mass Function
:label: defn-pmf

The probability mass function (PMF) of a discrete random variable $X$ is a function that specifies the probability of 
obtaining $X = x$, where $x$ is a particular event:

$$p_{X}(x) = P\left(X=x\right)$$

The set of all possible outcomes for a discrete random variable $X$ is denoted as $X\left(\Omega\right)$. A PMF must satisfy the condition:

$$\sum_{x\in{X(\Omega)}}p_{X}(x)=1$$
````

The PMF is the weighing function for discrete random variables. To illustrate this idea, let’s discuss some probability mass functions and associated examples.

#### Bernoulli random variable
A Bernoulli random variable, the simplest random variable, models a coin-flip or some other type of binary
outcome. Bernoulli random variable have two states: either 1 or 0. The probability of getting 1 is $p$, while the probability of getting a value of 0 is $1 − p$. Bernoulli random variables model many binary events: coin flips (H or T), binary bits (1 or 0), true or false, yes or no, present or absent, etc.

````{prf:definition} Bernoulli Random Variable
:label: defn-pmf-bernouli

Let $X$ be a Bernoulli random variable. Then, the probability mass function of the Bernoulli random variable $X$ is:

```{math}
p_{X}(x) =
\begin{cases}
  p & \text{if } x = 1 \\
  1 - p & \text{if } x = 0
\end{cases}
```

where $0<p<1$ is called the Bernoulli parameter. For a Bernoulli random variable $X(\Omega) \in [0,1]$ the expectation is given by:

```{math}
\mathbb{E}\left[X\right] = p
```

while the variance $\text{Var}(X)$ is given by:

```{math}
\text{Var}\left[X\right] = p(1-p)
```

````

#### Binomial random variable
The binomial distribution is the probability of getting exactly $k$ successes in $n$ independent Bernoulli trials. For example, the chance of getting four heads in 6 coin tosses. 

````{prf:definition} Binomial Random Variable
:label: defn-pmf-binomial

Suppose we do repeated Bernoulli trials $X(\Omega) \in [0,1]^n$, i.e., $n$ trials of an independent binary experiment.
The probability of getting exactly $k$ successes in $n$ independent Bernoulli trials is governed by the binomial probability mass function:

$$p_{X}(k) = \binom{n}{k}p^{k}\left(1-p\right)^{n-k}\qquad{k=0,1,\dots,n}$$

where $k$ denotes the number of successes in $n$ independent experiments, the binomial parameter $0<p<1$ is the probability 
of a successful trial and:

$$\binom{n}{k} = \frac{n!}{k!\left(n-k\right)!}$$

The expectation of a binomial random variable is given by:

```{math}
\mathbb{E}\left[X\right] = np
```

while the variance $\text{Var}(X)$ is given by:

```{math}
\text{Var}\left[X\right] = np(1-p)
```

````

#### Geometric random variable
We may be interested in doing a binary experiment, e.g., a coin flip until a specified outcome is obtained.
A geometric random variable governs the outcome of this type of experiment; 
a geometric random variable gives the probability that the first occurrence of success requires $k$ independent trials, each with success probability $p$. In other words, a geometric random variable describes the number of failures obtained before final success.

````{prf:definition} Geometric Random Variable
:label: defn-pmf-geometric

Let $X$ be a geometric random variable. The probability mass function for a geometric random variable is given by:

$$p_{X}(k) = (1-p)^{(k-1)}p\qquad{k=1,2,\dots}$$

where $p$ denotes the geometric parameter $0<p<1$. The expectation of a geometric random variable $X$ is given by:

```{math}
\mathbb{E}\left[X\right] = \frac{1}{p}
```

while the variance $\text{Var}(X)$ is given by:

```{math}
\text{Var}\left[X\right] = \frac{1-p}{p^2}
```
````

#### Poisson random variable
The Poisson distribution is a discrete probability distribution that expresses the probability of a given number of events occurring during a fixed interval if these events occur with a known constant mean rate and independently of the time since the last event. In other words, a Poisson distribution can be used to estimate how likely it is that something will happen `X` number of times. For example, the number of car crashes in a city of a given size or the number of cheeseburgers sold at a fast-food chain on a Friday night.

````{prf:definition} Poisson Random Variable
:label: defn-pmf-poisson

Let $X$ be a Poisson random variable. The probability mass function for a Poisson random variable is given by:

```{math}
p_{X}(x) = \frac{\lambda^{x}}{x!}\exp\left(-\lambda\right)
```

where $\lambda>0$ denotes the Poisson parameter, and $!$ denotes the factorial function. The expectation of a Poisson random variable $X$ is given by:

```{math}
\mathbb{E}\left[X\right] = \lambda
```

while the variance $\text{Var}(X)$ is given by:

```{math}
\text{Var}\left[X\right] = \lambda
```
````

### The von Neumann - Morgenstern theorem
The [von Neumann-Morgenstern theorem](https://en.wikipedia.org/wiki/Von_Neumann–Morgenstern_utility_theorem), also known as the [expected utility hypothesis](https://en.wikipedia.org/wiki/Expected_utility_hypothesis), is a fundamental result in decision theory that provides a framework for making _rational choices_ under uncertainty {cite}`vonneumann1947`. 


If an individual has preferences over a set of possible outcomes, and these preferences satisfy certain axioms, then there exists a unique function that assigns a numerical value to each outcome, known as its [expected utility](https://en.wikipedia.org/wiki/Expected_utility_hypothesis), such that the individual will choose the option with the highest expected utility. 


````{prf:definition} Expected utility hypothesis
:label: defn-expected-utility-hypothesis

An agent chooses amongst $n$ possible uncertain objects, $x_{1},\dots,x_{n}$. Object $k$ has probability $p_{k}$ and a utility payoff of $U(x_{k})$. Then, a rational decision maker maximizes the expected utility subject to constraints ({prf:ref}`defn-expected-utility-hypothesis`):


```{math}
:label: eqn-max-expected-ulity-problem

\begin{eqnarray}
\text{maximize}~\mathcal{O} &=& \sum_{k=1}^{n}p_{k}U(x_{k}) \\
\text{subject to}~g(x)~& \leq & 0\\
\text{and}~x_{k}&\geq&{0}\qquad{k=1,2,\dots,n}
\end{eqnarray}

```

where $p_{k}$ denotes the probability of object $k$, and $g(x)$ denotes constraints governing the objects $x_{1},\dots,x_{n}$.
````


This [von Neumann-Morgenstern theorem](https://en.wikipedia.org/wiki/Von_Neumann–Morgenstern_utility_theorem) provides a basis for understanding how people make decisions in uncertain situations.

---

# Summary
Uncertain decisions are those that involve risk or ambiguity. Uncertain decisions arise in many situations, such as investing in the stock market, choosing a career path, making technical choices, or deciding whether to pursue a romantic relationship. 

In this lecture, we introduced tools to model and analyze simple uncertain decisions:

* {ref}`content:references:utility-and-utlity-max` is the process of selecting the option that provides the highest level of satisfaction, or utility, based on the individual's preferences and constraints. This concept is often used in economics to model consumer behavior and in decision theory to analyze choices under uncertainty.

* {ref}`content:references:utility-and-uncetain-decisions` are decision-making situations where the outcomes of different options are unknown or uncertain. These types of decisions often involve risk and involve balancing the potential rewards and risks of different options. To make optimal choices under uncertainty, decision-makers may use probabilistic tools such as expected utility theory to analyze the expected outcomes of different options.