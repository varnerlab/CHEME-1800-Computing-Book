# Probability and Simple Choices under Uncertainty

Uncertain decisions are those that involve a certain degree of risk or ambiguity. Uncertain decisions arise in many situations, such as investing in the stock market, choosing a career path, making technical choices, or deciding whether to pursue a romantic relationship. Making uncertain decisions involves weighing each option’s potential benefits and drawbacks and considering the likelihood of different outcomes.

In this lecture, we introduce tools to model and analyze simple uncertain decisions:

* {ref}`content:references:utility-and-utlity-max` is the process of selecting the option that provides the highest level of satisfaction, or utility, based on the individual's preferences and constraints. This concept is often used in economics to model consumer behavior and in decision theory to analyze choices under uncertainty.

* {ref}`content:references:utility-and-uncetain-decisions` are decision-making situations where the outcomes of different options are unknown or uncertain. These types of decisions often involve risk and involve balancing the potential rewards and risks of different options. To make optimal choices under uncertainty, decision-makers may use probabilistic tools such as expected utility theory to analyze the expected outcomes of different options.

---

(content:references:utility-and-utlity-max)=
## Utility maximization
A utility function is a mathematical expression representing an individual's preferences over different choices or outcomes. It assigns a numerical value, called _utility_, to each possible choice based on how much the individual values it. A utility function is subjective and can vary from person to person, as different individuals may have other preferences.

Let's begin our discussion of utility by introducing the classical application of this concept, namely, the choice governings the choice of which combination of $n$ goods (or services) to consume ({prf:ref}`defn-individual-utility`):

````{prf:definition} Utility
:label: defn-individual-utility

The preferences of individual decision-making agents for $n$ goods are assumed to be represented by a utility function:

```{math}
:label: eqn-utility-function-generic
U(x_{1},x_{2},\dots,x_{n})
```

where $x_{1},x_{2},\dots,x_{n}$ are the quantities of $n$ goods consumed in time period $t\rightarrow{t+dt}$. The utility
function $U(\dots)$ is unique only up to an order-preserving transformation. 

````

{prf:ref}`defn-individual-utility` does not give specifics about the properties of a utility function. However, it does establish a critical concept; the utility can be used to rank order the preference for different choices. For example, suppose we have two options, $A$ and $B$:

* If $A\succ{B}$, the decision maker _strictly prefers_ $A$ to $B$. Then, the utility of choice $A$ is greater than $B$, or $U(A)>U(B)$.
* If $A\sim{B}$, the decision maker is _indifferent_ between $A$ to $B$. Then, the utility of choice $A$ is the same as $B$, or $U(A)=U(B)$.
* If $A\succsim{B}$, the decision maker _weakly prefers_ $A$ over $B$, or they are indifferent. Then, the utility of choice $A$ is greater than or equal to $B$, or $U(A)\geq{U(A)}$.


### Properties of utility functions
The mathematical properties of a _proper_ utility function $U(\dots)$ include:

* __Continuity__: A small change in the input variable should lead to a small change in the output value of the function.
Monotonicity: The utility function should be non-decreasing, meaning that as the input variable increases, the output value of the function also increases.
* __Convexity__: The utility function should exhibit a diminishing marginal utility, meaning that the additional satisfaction gained from an additional unit of the input variable decreases as the input variable increases.
* __Independence:__ The utility function should be independent of irrelevant alternatives, meaning that adding or removing irrelevant options should not affect the ordering of preferences.
* __Substitutability__: The utility function should exhibit substitution, meaning that if one option becomes unavailable, the utility can be derived from a substitute option.

These properties ensure that the utility function is well-behaved and represents an individual's preferences over different options. Let's consider a specific example utility function ({prf:ref}`example-cobb-douglas-uf`):

````{prf:example} Cobb–Douglas utility function
:label: example-cobb-douglas-uf
:class: dropdown

The [Cobb–Douglas utility function](https://en.wikipedia.org/wiki/Cobb–Douglas_production_function) governing the satisfaction gained by purchasing $n$ goods $(x_{1},x_{2},\dots,x_{n})$ is given by:

```{math}
:label: eqn-simple-utility-function
U(x)  = \prod_{i=1}^{n}x_{i}^{\alpha_{i}} 
```

where the coefficients $\alpha_{i}$ are governed by:

```{math}
\sum_{i=1}^{n}\alpha_{i}  = 1
```

Show the [Cobb–Douglas utility function](https://en.wikipedia.org/wiki/Cobb–Douglas_production_function) satisfies the first two utility function properties (continuity and convexity). 

__Solution__: Equation {eq}`eqn-simple-utility-function` is continuous, so the first condition is satisfied. To explore the convexity property, compute the [marginal utility](https://en.wikipedia.org/wiki/Marginal_utility), i.e., the partial derivative of $U(\dots)$ with respect to $x_{i}$:

```{math}
:label: eqn-mu-u-function

\text{MU}_{x_{i}} = \left(\alpha_{i}x^{\alpha_{i}-1}\right)\cdot\left(\prod_{j=1,i}^{n}x_{j}^{\alpha_{j}}\right)
```

where the $j=1,i$ notation denotes the _exclusion_ of index $i$. As $x_{i}\rightarrow\infty$, the marginal utility $\text{MU}_{x_{i}}\rightarrow{0}$ if $\alpha_{i}<1$. Thus, the convexity property is satisfied for $\alpha_{i}<1$.

````

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

The partial derivative of the utility with respect to a change in the consumption of good or service $i$ is defined as the [marginal utility](https://en.wikipedia.org/wiki/Marginal_utility):

```{math}
\text{MU}_{i} \equiv \left(\frac{\partial{U}}{\partial{x_{i}}}\right)_{\star}\qquad{i=1,2,\dots,n}
```

The utility $U(\dots)=c$ on an indifference curve; thus, along an indifference curve $dU = 0$. The marginal rate of substitution of good or service $i$ for $j$ (all other quantities held constant):

```{math}
:label: eqn-total-differential-ic-constant

\text{MU}_{i}dx_{i} + \text{MU}_{j}dx_{j} = 0\qquad{i\neq{j}}
```

can be computed for good $i$ and $j$:

```{math}
:label: eqn-total-differential-ic-mrs
\text{MRS}_{ij} = -\frac{dx_{j}}{dx_{i}} = \frac{\text{MU}_{i}}{\text{MU}_{j}}\qquad\forall\left(i,j\right)_{i\neq{j}}
```

````

#### Calculating the marginal utility and the marginal rate of substitution 
The marginal utility and the marginal rate of substitution can be computed by directly differentiating the utility function (as shown in {prf:ref}`example-cobb-douglas-uf`). However, sometimes it may not be convenient to analytically compute the derivative, e.g., the utility function is complicated. You can always approximate the marginal utility using a [finite difference approximation](https://en.wikipedia.org/wiki/Finite_difference), 
or [automatic differentiation](https://en.wikipedia.org/wiki/Automatic_differentiation). 

Sample code for [automatic differentiation](https://en.wikipedia.org/wiki/Automatic_differentiation) of the Cobb-Douglas utility function:
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

### Optimal choices and budgets
To estimate optimal choices, a decision-making agent _maximizes_ a utility function, i.e., the agent searches for a combination of goods and services that gives the highest satisfaction subject to various constraints, e.g., a budget constraint ({prf:ref}`eqn-budget-constraint`):

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



(content:references:utility-and-uncetain-decisions)=
## Simple choices under uncertainty
Fill me in.

### Moments of discrete random variables
It is often helpful to extract characteristics such as the mean, standard deviation, or other quantities of interest from random variables.  We compute these parameters of interest from random market data using a concept called the [moments of a random variable](https://en.wikipedia.org/wiki/Moment_(mathematics)). The mean, variance, skew, kurtosis, etc., are all examples of the [moments of a random variable](https://en.wikipedia.org/wiki/Moment_(mathematics)). We'll only focus on the first two moments, the expectation and the variance.

#### Expectation
The [expectation](https://en.wikipedia.org/wiki/Expected_value) measures the central tendency of the values of a random variable $X$. The expectation is defined as a weighted sum (for discrete $X$):

```{math}
:label: eqn-expectation
\mathbb{E}\left[X\right] = \sum_{x\in{X}(\Omega)}xp_{X}(x)
```

where $x$ denotes a value for the discrete random variable $X$, and $p_{X}(x)$ denotes the probability mass function evaluated at $X=x$; a A probability mass function (PMF) is a function that describes the probability of a discrete random variable taking on a particular value.

````{prf:observation} Properties of expectation
:label: obs-expectation-props
The expectation of a random variable $X$ (discrete or continuous) has several useful (and important) properties: 
1. $\mathbb{E}\left(c\right) = c$ for any constant $c$
1. $\mathbb{E}\left(cX\right) = c\times\mathbb{E}\left(X\right)$ for any constant $c$
1. $\mathbb{E}\left(g(X)\right) = \sum_{x\in{X(\Omega)}}g(x)p_{X}(x)$
1. $\mathbb{E}\left(g(X)+h(X)\right) = \mathbb{E}(g(X)) + \mathbb{E}(h(X))$
1. $\mathbb{E}\left(X+c\right) = \mathbb{E}(X) + c$ for any constant $c$
````

#### Variance

The [variance](https://en.wikipedia.org/wiki/Variance) measures the expected dispersion for
individual values of $X$, i.e., the average distance that values of $X$ are spread out from their expected value (mean). The [variance](https://en.wikipedia.org/wiki/Variance) is given by:

```{math}
:label: eqn-variance
\text{Var}(X) = \mathbb{E}\Bigl[(X-\mu)^{2}\Bigr]
```

where $\mu = \mathbb{E}(X)$ denotes the expected value of the random variable $X$.

````{prf:observation} Properties of variance
:label: obs-variances-var

Like the expected value, the variance $\text{Var}(X)$ has a few interesting (and important) properties:

* $\text{Var}(X) = \mathbb{E}\left(X^{2}\right) - \left(\mu\right)^2$
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
a geometric random variable gives the probability that the first occurrence of success requires $k$ independent trials, each with success probability $p$. In other words, 
a geometric random variable describes the number of failures obtained before final success.

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

---

# Summary
Uncertain decisions are those that involve risk or ambiguity. Uncertain decisions arise in many situations, such as investing in the stock market, choosing a career path, making technical choices, or deciding whether to pursue a romantic relationship. 

In this lecture, we introduced tools to model and analyze simple uncertain decisions:

* {ref}`content:references:utility-and-utlity-max` is the process of selecting the option that provides the highest level of satisfaction, or utility, based on the individual's preferences and constraints. This concept is often used in economics to model consumer behavior and in decision theory to analyze choices under uncertainty.

* {ref}`content:references:utility-and-uncetain-decisions` are decision-making situations where the outcomes of different options are unknown or uncertain. These types of decisions often involve risk and involve balancing the potential rewards and risks of different options. To make optimal choices under uncertainty, decision-makers may use probabilistic tools such as expected utility theory to analyze the expected outcomes of different options.