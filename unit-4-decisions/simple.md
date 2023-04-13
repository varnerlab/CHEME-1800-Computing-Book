# Probability and Simple Choices under Uncertainty

Uncertain decisions are those that involve a certain degree of risk or ambiguity. Uncertain decisions arise in many situations, such as investing in the stock market, choosing a career path, making technical choices, or deciding whether to pursue a romantic relationship. Making uncertain decisions involves weighing each option’s potential benefits and drawbacks and considering the likelihood of different outcomes.

In this lecture, we introduce tools to model and analyze simple uncertain decisions:

* {ref}`content:references:utility-and-utlity-max` is the process of selecting the option that provides the highest level of satisfaction, or utility, based on the individual's preferences and constraints. This concept is often used in economics to model consumer behavior and in decision theory to analyze choices under uncertainty.

* {ref}`content:references:utility-and-uncetain-decisions` are decision-making situations where the outcomes of different options are unknown or uncertain. These types of decisions often involve risk and involve balancing the potential rewards and risks of different options. To make optimal choices under uncertainty, decision-makers may use probabilistic tools such as expected utility theory to analyze the expected outcomes of different options.

---

(content:references:utility-and-utlity-max)=
## Utility maximization
A utility function is a mathematical expression representing an individual's preferences over different choices or outcomes. It assigns a numerical value, called _utility_, to each possible choice based on how much the individual values it. A utility function is subjective and can vary from person to person, as different individuals may have other preferences.

Let's begin our discussion of utility by introducing the classical application of this concept, namely, the choice of how much of $n$ goods to purchase ({prf:ref}`defn-individual-utility`):

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

### Properties of utility functions
The mathematical properties of a _proper_ utility function $U(\dots)$ include:

* __Continuity__: A small change in the input variable should lead to a small change in the output value of the function.
Monotonicity: The utility function should be non-decreasing, meaning that as the input variable increases, the output value of the function also increases.
* __Convexity__: The utility function should exhibit a diminishing marginal utility, meaning that the additional satisfaction gained from an additional unit of the input variable decreases as the input variable increases.
* __Independence:__ The utility function should be independent of irrelevant alternatives, meaning that adding or removing irrelevant options should not affect the ordering of preferences.
* __Substitutability__: The utility function should exhibit substitution, meaning that if one option becomes unavailable, the utility can be derived from a substitute option.

These mathematical properties ensure that the utility function is a well-behaved mathematical function that accurately represents an individual's preferences over different options.

#### Example
For example, consider a utility function governing the satisfaction gained by purchasing two goods $(x_{1},x_{2})$ of the form:

```{math}
:label: eqn-simple-utility-function

U(x_{1},x_{2}) = \sqrt{x_{1}\cdot{x_{2}}}
```

Equation {eq}`eqn-simple-utility-function` is continuous, so the first condition is satisfied. To explore the convexity property, compute the [marginal utility](https://en.wikipedia.org/wiki/Marginal_utility), i.e., the partial derivative of $U(\dots)$ with respect to goods $x_{1}$ and $x_{2}$:

```{math}
:label: eqn-mu-u-function

\begin{eqnarray}
\text{MU}_{x_{1}} = \frac{\partial{U}}{\partial{x_{1}}} & = & \left(\frac{1}{2}\right)\frac{x_{2}}{\sqrt{x_{1}\cdot{x_{2}}}} \\
\text{MU}_{x_{2}} = \frac{\partial{U}}{\partial{x_{2}}} & = & \left(\frac{1}{2}\right)\frac{x_{1}}{\sqrt{x_{1}\cdot{x_{2}}}} \\
\end{eqnarray}

```

As $x_{i}\rightarrow\infty$, the marginal utility $\text{MU}_{x_{i}}\rightarrow{0}$, thus, the convexity property is satisfied.  

### Indifference curves and optimal choices
An indifference curve is a graphical representation of a combination of choices that provide an individual with the same level of satisfaction or utility. Each point on the indifference curve represents a combination of choices that provides the same satisfaction or utility. The curve slopes downwards because the individual is willing to give up some of one good to obtain more of the other while remaining indifferent. Indifference curves are valuable tools in economics for analyzing consumer preferences and making predictions about consumer behavior in response to changes in prices or income.

Utility maximization is the process of choosing the option that provides the highest level of utility, given a set of available options and the individual's preferences. It involves evaluating each option using a utility function, and selecting the one that maximizes the utility subject to constraints.

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