---
jupytext:
  formats: md:myst
  text_representation:
    extension: .md
    format_name: myst
kernelspec:
  display_name: Julia
  language: julia
  name: julia-1.8
---

# Ordinary Least Squares Problems

## Introduction
[Ordinary least squares](https://en.wikipedia.org/wiki/Ordinary_least_squares) is a statistical method to estimate the parameters of a regression model, i.e., to find the best fit model that predicts the values of the response variable (dependent variable) based on the values of the observed variables (independent variables). 

We start the lecture by introducing the linear regression models, and the learning problem, and then explore two types of common ordinary least squares problems:

* {ref}`content:references:ols-general-introduction` is a statistical model used to analyze the relationship between a dependent variable (also known as the response variable) and one or more independent variables (also known as explanatory variables or predictors) that are believed to influence the dependent variable. 
* {ref}`content:references:ols-unconstrained-problem`. In unconstrained problems, the regression model parameters can take on any values and are not limited in any way.
* {ref}`content:references:ols-constrained-problem`. In constrained problems, the regression model parameters can be bound in some way, e.g., by physical reasoning or by some relationships between the parameters.

---

<!-- is a popular method for estimating the parameters of regression models because it is relatively simple to implement and has nice mathematical properties, such as being unbiased and having the smallest variance among all unbiased estimators. -->

(content:references:ols-general-introduction)=
## Linear regression models
Linear regression models are a statistical model used to analyze the relationship between a dependent variable (also known as the response variable) and one or more independent variables (also known as explanatory variables or predictors) that are believed to influence the dependent variable ({prf:ref}`defn-ols-introduction`):

````{prf:definition} Linear regression model
:label: defn-ols-introduction

There exists a dataset $\mathcal{D} = \left\{\mathbf{x}_{i},y_{i}\right\}_{i=1}^{n}$ where $\mathbf{x}_{i}$ is a p-vector of inputs (independent variables) and $y_{i}$ denotes a scalar response variable (dependent variable). Then, a linear regression model for $\mathcal{D}$ takes the form:

```{math}
:label: eqn-linear-regression-model
y_{i} = \mathbf{x}_{i}^{T}\mathbf{\beta} + \epsilon_{i}\qquad{i=1,2,\dots,n}
```

where $\mathbf{x}_{i}$ is a p-dimensional vector of inputs, $\mathbf{\beta}$ is a $p\times{1}$ vector of unknown model parameters and $\epsilon_{i}$ represents unobserved random errors. Eqn. {eq}`eqn-linear-regression-model` can be re-written in matrix form as:

```{math}
:label: eqn-linear-regression-model-matrix
\mathbf{y} = \mathbf{X}\mathbf{\beta} + \mathbf{\epsilon}
```
````

Linear reagression models are widely used to model physical, chemical or economic phenomena, e.g., a model of the rate of a chemical reaction, the return of a stock, the relationship between the home price and square footage, etc.

```{figure} ./figs/Fig-LinearRegression-Schematic.pdf
---
height: 320px
name: fig-linear-regression-schematic
---
Schematic of linear regression. Unknown model parameters $\mathbf{\beta}$ are chosen so that the squared difference (residual) between model predicted $\hat{y}_{i}=\mathbf{x}_{i}^{T}\mathbf{\beta}$ and observed $y_{i}$ output variables is minimized.
```

### Sum of squared errors loss function
Starting from observations, [ordinary least squares](https://en.wikipedia.org/wiki/Ordinary_least_squares) estimates the value of the unknown parameters $\mathbf{\beta}$ that appear in a linear regression model by _minimizing_ the sum of squared errors between model estimates and observed values as illustrated in {numref}`fig-linear-regression-schematic` and defined in ({prf:ref}`defn-sum-squared-error`):


````{prf:definition} Sum of squared error loss function
:label: defn-sum-squared-error

There exists a dataset $\mathcal{D} = \left\{\mathbf{x}_{i},y_{i}\right\}_{i=1}^{n}$ where $\mathbf{x}_{i}$ is a p-vector of observed inputs and $y_{i}$ denotes an observed response variable. Further, suppose we model the dataset $\mathcal{D}$ using the linear regression model:

```{math}
\mathbf{y} = \mathbf{X}\mathbf{\beta} + \mathbf{\epsilon}
```

Best-fit values for the unknown parameters $\mathbf{\beta}$ can be estimated by solving the optimization problem:

```{math}
:label: eqn-obj-f-ols
\hat{\mathbf{\beta}} = \arg\min_{\mathbf{\beta}} ||~\mathbf{y} - \mathbf{X}\mathbf{\beta}~||^{2}_{2}
```

where $||\star||^{2}_{2}$ is the square of the p = 2 vector norm, see {ref}`content:measurements-matrix-vector-norms`. 

````

[Ordinary least squares](https://en.wikipedia.org/wiki/Ordinary_least_squares) is an example of a [supervised learning approach](https://en.wikipedia.org/wiki/Supervised_learning), i.e., we use measured inputs and outputs to estimate parameters appearing in the model.


<!-- the sum of the squared differences between the observed values of the response variable and the predicted values based on the regression line. This is done by choosing the values of the model parameters that minimize the sum of squared errors (SSE). However, OLS has some limitations, such as assuming that the errors in the model are Normally distributed and having poor performance when the data is highly correlated or has outliers. -->

(content:references:ols-unconstrained-problem)=
## Unconstrained regression problems
In an unconstrained least-squares problem, we find parameter values that minimize the difference between the model predictions and the observed data. Least-squares problems are _unconstrained_ when there are no constraints on the permissible values of the parameters. 

However, the data matrix $\mathbf{X}\in\mathbb{R}^{n\times{p}}$ can have different shapes: 
* __Square__: the number of observations (rows) is the same as the number of parameters (columns), in which case the system is square ($n = p$).
* __Overdetermined__: the data matrix $\mathbf{X}$ has more observations (rows) than parameters (columns), in which case the system is overdetermined ($n\gg{p}$).
* __Underdetermined__: the data matrix $\mathbf{X}$ has fewer observations (rows) than parameters (columns), in which case the system is underdetermined ($n\ll{p}$).

### Square regression problems
Suppose we are estimating the values of the unknown parameters $\mathbf{\beta}$ in a linear regression model:

```{math}
:label: eqn-square-ols-model
\mathbf{y} = \mathbf{X}\mathbf{\beta} + \mathbf{\epsilon}
```

where the data matrix $\mathbf{X}\in\mathbb{R}^{p\times{p}}$ has the same number of rows and columns as the number of unknown parameters. In this case, Eqn {eq}`eqn-square-ols-model` is a square system of linear algebraic equations which we can solve for the unknown parameter vector $\mathbf{\beta}$ by solving the system of equations:

```{math}
\hat{\mathbf{\beta}} = \mathbf{X}^{-1}\mathbf{y} - \mathbf{X}^{-1}\mathbf{\epsilon}
```

either directly or iteratively, see our previous discussion of {ref}`content:references:soln-laes-start`.
However, for this approach to be applicable, the inverse of the data matrix must exist, see {prf:ref}`defn-matrix-inverse`. 

### Overdetermined regression problems
In most applications, it is more likely that the data matrix $\mathbf{X}\in\mathbb{R}^{n\times{p}}$ will be overdetermined ($n\gg{p}$). The inverse of an overdetermined data matrix can not be computed directly. Instead, we solve a particular system of equations called the [normal equations](https://en.wikipedia.org/wiki/Ordinary_least_squares) which transforms the original overdetermined problem into a square system:

````{prf:definition} Normal solution overdetermined linear regression model
:label: defn-normal-eqn-ols

There exists dataset $\mathcal{D} = \left\{\mathbf{x}_{i},y_{i}\right\}_{i=1}^{n}$ where $\mathbf{x}_{i}$ is a p-dimensional vector of inputs (independent variables) and $y_{i}$ denotes a scalar response variable (dependent variable), and $n\gg{p}$.  Further, suppose we model the dataset $\mathcal{D}$ using the linear regression model:

```{math}
\mathbf{y} = \mathbf{X}\mathbf{\beta} + \mathbf{\epsilon}
```

Then, the value of the unknown parameter vector $\mathbf{\beta}$ that minimizes the sum of the squares loss function for an overdetermined system is given by:

```{math}
:label: eqn-loss-function-soln

\hat{\mathbf{\beta}} = \left(\mathbf{X}^{T}\mathbf{X}\right)^{-1}\mathbf{X}^{T}\mathbf{y} - \left(\mathbf{X}^{T}\mathbf{X}\right)^{-1}\mathbf{X}^{T}\mathbf{\epsilon}
```

The matrix $\mathbf{X}^{T}\mathbf{X}$ is called the normal matrix, while $\mathbf{X}^{T}\mathbf{y}$ is called the moment matrix. The existence of the normal solution $\hat{\mathbf{\beta}}$ requires that the normal matrix inverse $\left(\mathbf{X}^{T}\mathbf{X}\right)^{-1}$ exists.
````

#### Computation of the overdetermined matrix inverse
Assuming the normal matrix inverse $\left(\mathbf{X}^{T}\mathbf{X}\right)^{-1}$ exists, we can compute it directly by computing the matrix product and inverting using the `inv` function. Alternatively, we can use the [pinv](https://docs.julialang.org/en/v1/stdlib/LinearAlgebra/#LinearAlgebra.pinv) function included in the `LinearAlgebra` package in [Julia](https://julialang.org):

```{code-cell} julia
# load the LinearAlgebra package
using LinearAlgebra

# Define a random overdetermined array
A = rand(10,8) 

# compute the pinverse 
LI = pinv(A); # this will be an 8 x 10 matrix

# Test -
IM = LI*A; 
println("Is IM the identity matrix -> check the trace: $(tr(IM))")
```

This matrix inverse is called a [left Moore-Penrose inverse](https://en.wikipedia.org/wiki/Moore–Penrose_inverse).


### Underdetermined regression problems
If the number of observations (rows) in the data matrix $\mathbf{X}$ is less than the number of columns, i.e., the number of unknown parameters, the system is underdetermined. In general, there will be infinitely many solutions for an underdetermined system. How do we choose which solution to use?

#### Least-norm solutions
The classic strategy to solve an underdetermined system is to estimate the _smallest_ values (measured by some norm $||\star||$) for the unknown parameters $\mathbf{\beta}$ such that they satisfy the original equations:

````{math}
\begin{eqnarray}
\text{minimize}~& & ||~\mathbf{\beta}~|| \\
\text{subject to} & & \mathbf{X}\mathbf{\beta} = \mathbf{y} \\
\end{eqnarray}
````

Thus, the solution of an underdetermined least squares problem is _constrained_, i.e., the possible values of $\mathbf{\beta}$ are restricted somehow. An analytical solution for the parmeter vector $\mathbf{\beta}$ can be computed for the underdetermined case ({prf:ref}`defn-normal-eqn-ols-ln`):

````{prf:definition} Solution underdetermined linear regression model
:label: defn-normal-eqn-ols-ln

There exists dataset $\mathcal{D} = \left\{\mathbf{x}_{i},y_{i}\right\}_{i=1}^{n}$ where $\mathbf{x}_{i}$ is a p-dimensional vector of inputs (independent variables) and $y_{i}$ denotes a scalar response variable (dependent variable), and $n\gg{p}$.  Further, we model the dataset $\mathcal{D}$ using the linear regression model:

```{math}
\mathbf{y} = \mathbf{X}\mathbf{\beta} + \mathbf{\epsilon}
```

Then, the least-norm solution of the unknown parameter vector $\mathbf{\beta}$ is given by:

```{math}
:label: eqn-loss-function-soln-least-norm

\hat{\mathbf{\beta}} =\mathbf{X}^{T}\left(\mathbf{X}\mathbf{X}^{T}\right)^{-1}\mathbf{y} - \mathbf{X}^{T}\left(\mathbf{X}\mathbf{X}^{T}\right)^{-1}\mathbf{\epsilon}
```

The existence of the least-norm solution $\hat{\mathbf{\beta}}$ requires that the matrix inverse $\left(\mathbf{X}\mathbf{X}^{T}\right)^{-1}$ exists.
````

Equivalently, we can solve for the unknown model parameters $\mathbf{\beta}$ using singular value decomposition (SVD) of the data matrix ({prf:ref}`defn-svd-soln-ud-ols`):

````{prf:definition} Singular value decomposition underdetermined system
:label: defn-svd-soln-ud-ols

There exists a dataset $\mathcal{D} = \left\{\mathbf{x}_{i},y_{i}\right\}_{i=1}^{n}$ where $\mathbf{x}_{i}$ is a p-vector of inputs (independent variables) and $y_{i}$ denotes a scalar response variable (dependent variable), and $n\ll{p}$.  Further, suppose we model the dataset $\mathcal{D}$ using the linear regression model:

```{math}
\mathbf{y} = \mathbf{X}\mathbf{\beta} + \mathbf{\epsilon}
```

Then, the smallest values of the unknown parameter vector $\mathbf{\beta}$ that satisfies the linear regression model are given by:

```{math}
\hat{\mathbf{\beta}} = \mathbf{V}\mathbf{\Sigma}^{-1}\mathbf{U}^{T}\mathbf{y} - \mathbf{V}\mathbf{\Sigma}^{-1}\mathbf{U}^{T}\mathbf{\epsilon}
```
````


#### Computation of the underdetermined matrix inverse
We can compute the matrix inverse $\left(\mathbf{X}\mathbf{X}^{T}\right)^{-1}$, assuming it exists, by computing the matrix product and inverting using the `inv` function. Alternatively, we can use the [pinv](https://docs.julialang.org/en/v1/stdlib/LinearAlgebra/#LinearAlgebra.pinv) function included in the `LinearAlgebra` package in [Julia](https://julialang.org):

```{code-cell} julia
# load the LinearAlgebra package
using LinearAlgebra

# Define a random undetermined array
A = rand(8,10) 

# compute the pinverse 
RI = pinv(A); # this will be an 10 x 8 matrix

# Test -
IM = A*RI; 
println("Is IM the identity matrix -> check the trace: $(tr(IM))")
```

This matrix inverse is called a [right Moore-Penrose inverse](https://en.wikipedia.org/wiki/Moore–Penrose_inverse).

<!-- This is done by defining a cost function that measures the difference between the predictions and the experimental data and then finding the parameter values that minimize this cost function. Least-squares problems are commonly used in linear regression, where the goal is to find the line of best fit for a set of data points. They are also used in many other applications, such as curve fitting, signal processing, and control systems.

Many optimization algorithms can solve least-squares problems, including gradient descent, Levenberg-Marquardt, and Gauss-Newton algorithms. The choice of algorithm will depend on the problem being solved and the characteristics of the data. -->


(content:references:ols-constrained-problem)=
## Constrained regression problems
Constrained least squares estimates the parameters of a linear regression model subject to one or more constraints on the values the parameters can take, e.g., 
there exists prior knowledge or physical relationships that must be satisfied by the parameter estimates. 

<!-- To solve constrained least squares problems, we first define the linear regression model and the constraints on the parameters. We then define a loss function that measures the model’s fit to the data subject to the constraints. We minimize the loss function to find the estimates of the parameters that best fit the data while satisfying the constraints. -->

### Lagrange multipliers
Lagrange multipliers are a mathematical tool used in optimization to find a function’s maximum or minimum value subject to constraints. The [method of Lagrange multipliers](https://en.wikipedia.org/wiki/Lagrange_multiplier) involves introducing additional variables, called Lagrange multipliers, which integrate the constraints into the loss function ({prf:ref}`defn-method-l-multipliers`):

````{prf:definition} Method of Lagrange multipliers
:label: defn-method-l-multipliers

To find the maximum or minimum of a function $f(x)$ subject to the equality constraint $g(x)$, we can form the Lagrangian function:

```{math}
:label: eqn-lagrangian-1d
\mathcal{L}(x,\lambda) = f(x) + \lambda\cdot{g}(x)
```

where $\lambda$ is the Lagrange multiplier for constraint $g(x)$, and $\mathcal{L}(x,\lambda)$ is called the Lagrangian function. At a critical point (maximum or minimum), the partial derivatives of the 
Lagrangian function with respect to $x$ and $\lambda$ vanish:

```{math}
:label: eqn-first-order-condition-lagrange

\begin{eqnarray}
\frac{\partial\mathcal{L}}{\partial{x}} & = & 0\\
\frac{\partial\mathcal{L}}{\partial{\lambda}} & = & 0\\
\end{eqnarray}
```

The system of equations defined by Eqn. {eq}`eqn-first-order-condition-lagrange` callled the Lagrange equations, can be solved to find the critical points and, thus, the maximum or minimum value of the objective function.
````

Let's show how we can use the [method of Lagrange multipliers](https://en.wikipedia.org/wiki/Lagrange_multiplier) to derive the least-norm solution for the underdetermined learning problem ({prf:ref}`example-lagrange-multipliers-lns`):

````{prf:example} Derivation least norm solution
:label: example-lagrange-multipliers-lns
:class: dropdown

Derive the least norm solution to the optimization problem:

```{math}
\begin{eqnarray}
\text{minimize} & & \mathbf{\beta}^{T}\mathbf{\beta} \\
\text{subject to} & & \mathbf{X}\mathbf{\beta} = \mathbf{y}
\end{eqnarray}
```

using the [method of Lagrange multipliers](https://en.wikipedia.org/wiki/Lagrange_multiplier) to compute an estimate of the regression parameters $\mathbf{\beta}$.

__Solution__: The first step is to form the Lagrangian function:

```{math}
\mathcal{L}(\mathbf{\beta},\lambda) = \mathbf{\beta}^{T}\mathbf{\beta} + \lambda\left(\mathbf{X}\mathbf{\beta} - \mathbf{y}\right)
```

which can differentiate with respect to $\mathbf{\beta}$ and the Lagrange multipliers $\lambda$:

```{math}
\begin{eqnarray}
\frac{\partial\mathcal{L}}{\partial{\mathbf{\beta}}} & = & 2\mathbf{\beta} + \mathbf{X}^{T}\lambda = 0 \\
\frac{\partial\mathcal{L}}{\partial{\lambda}} & = & \left(\mathbf{X}\mathbf{\beta} - \mathbf{y}\right) = 0\\
\end{eqnarray}
```

We can solve the first equation for $\mathbf{\beta}$ in terms of $\lambda$:

```{math}
\mathbf{\beta} = -\mathbf{X}^{T}\left(\frac{\lambda}{2}\right)
```

which we can then substitute into the second equation to get an expression for $\lambda = -2\left(\mathbf{X}\mathbf{X}^{T}\right)^{-1}\mathbf{y}$. Substituting the $\lambda$ expression into the expression for $\beta$, i.e., eliminating the [Lagrange multipliers](https://en.wikipedia.org/wiki/Lagrange_multiplier) gives the least norm solution for $\mathbf{\beta}$:

```{math}
\mathbf{\beta} = \mathbf{X}^{T}\left(\mathbf{X}\mathbf{X}^{T}\right)^{-1}\mathbf{y}
```
````


<!-- 
The Lagrangian, which is the sum of the objective function and the product of the Lagrange multipliers and the constraints, is used to find the critical points of the objective function subject to the constraints by taking the partial derivatives with respect to both the variables and the Lagrange multipliers and setting them to zero. -->

### Penalty methods
A penalty method transforms a constrained least squares problem into an unconstrained problem that can be solved. In a penalty method, a penalty is added to the loss function to encourage specific desirable properties of the solution. 

#### Quadratic penalty functions
Before we look at the applications of penalty methods, let's consider a simple example to work out the basic strategy. This example was reproduced from the [Mathematical Optimization course at Stanford](https://web.stanford.edu/group/sisl/k12/optimization/#!index.md). Suppose we wanted to solve the problem:

```{math}
:label: eqn-example-pmethod
\begin{eqnarray}
\arg \min_{x} \left(f(x) = \frac{100}{x}\right) & &  \\
\text{subject to} & & \\
x\leq{5} & &   
\end{eqnarray}
```
Before starting, convert any constraints into the form (expression) $\leq{0}$, so the $x\leq{5}$ becomes:

```{math}
x - 5 \leq{0}
```

Once the constraints have been converted, the next step is to start charging a penalty for violating them. Since we’re trying to minimize $f(x)$, we need to _add value_ when the constraint is violated. If you are trying to maximize, the penalty will _subtract value_. With the constraint $x-5\leq{0}$ we need a penalty that is:
* __Constraint satisfied__: 0 when $x-5\leq{0}$
* __Constraint violated__: postive when $x-5>0$.

This can be done by using a penalty $P\left(x\right)$ of the form:

```{math}
:label: eqn-quad-penalty
P(x) = \max\left(0,x-5\right)^{2}
```

Eqn. {eq}`eqn-quad-penalty` is a quadratic penalty (loss) function. This approach works for equality constraints as well. For example, suppose we had the constraint $h(x) = c$, where $c$ is a constant. We can convert this type of constraint into a penalty of the form:

```{math}
:label: eqn-equality-constraint-pm
P(x) = \left(h(x) - c\right)^{2}
```

The lowest penalty value in {eq}`eqn-equality-constraint-pm` will occur when $h(x) = c$; thus, we satisfy the constraint. Once we have converted the constraints into penalty functions, we add all the penalty functions to the original objective function $f(x) + \lambda\cdot{P(x)}$ and minimize the total function (objective plus penalties). For example, the original problem in {eq}`eqn-example-pmethod` becomes:

```{math}
:label: eqn-final-penality-form
\arg \min_{x} \left(\frac{100}{x} +\lambda\cdot\max\left(0,x-5\right)^{2}\right)
```

where $\lambda$ is a _hyper-parameter_ (a parameter associated with the method, not the problem) that is adjusted during the process to estimate the unknown value of $x$ according to the policy ({prf:ref}`obs-lambda-policy`):

```{prf:observation} $\lambda$-policy penalty method
:label: obs-lambda-policy
For a penalty method, we start with a small $\lambda$ and repeat the $x$ estimation problem with larger and larger values of $\lambda$. This makes constraint violation more expensive for each subsequent refinement of the estimate of $x$.
```

With a penalty method, we can choose any value for the starting value of $x$. Let's do an example penalty method calculation that uses a [simple evolutionary algorithm](https://en.wikipedia.org/wiki/Evolutionary_algorithm) to generate new guesses for $x$ ({prf:ref}`example-quad-penalty-method`):

````{prf:example} Penalty method example
:label: example-quad-penalty-method
:class: dropdown

Solve the minimization problem for $x$ given by Eqn. {eq}`eqn-example-pmethod` using a quadratic penalty method for an initial guess of $x = 20$ for $\lambda = 1e3,1e6,1e9$.

```julia
"""
    _loss(x::Float64; λ::Float64 = 1.0) -> Float64

Loss function for penalty method example. 
"""
function _loss(x::Float64; λ::Float64 = 1.0)::Float64

    # compute the f(x) and the penalty -
    f = 100.0/x;
    P = (max(0.0, x - 5.0))^2;

    # return -
    return f + λ*P;
end

"""
    main() -> Float64
Runs an evolutionary algorithm to estimate x̂ (min of loss function). 
"""
function main()::Float64

    # initialize -
    Λ = [1e3,1e6,1e9];
    xₒ = 20.0;
    best_loss = Inf;
    x̂ = xₒ;
    max_number_of_iterations = 1000;

    # use a simple evolutionary algorithm -
    for λ ∈ Λ

        x′ = x̂ # initialize the current x, with the best value we have so far

        # refine our best guess
        for _ ∈ 1:max_number_of_iterations
            
            # compute the loss -
            l = _loss(x′, λ = λ);
            
            # is this loss *better* than our best solution so far?
            if (l < best_loss)
                x̂ = x′;
                best_loss = l;
            end

            # generate a new guess for x -
            x′ = x̂ + 0.1*randn()
        end
    end

    # return -
    return x̂;
end

# call the main -
x = main();
```

__source__: [Source for penalty method example](https://github.com/varnerlab/CHEME-1800-4800-Course-Repository-S23/tree/main/examples/unit-3-examples/penalty_method)

````

#### Barrier functions
Let's rethink the problem shown in Eqn. {eq}`eqn-example-pmethod`. Suppose, instead of developing the penality function shown in Eqn. {eq}`eqn-quad-penalty` to minimize $f(x)$, for a ccontraint of the form $g(x)\leq{0}$ we developed a barrier function:

```{math}
:label: eqn-barrier-function
B\left(x\right) = - \frac{1}{g(x)}
```

As the $g(x)\rightarrow{0}$, i.e., we approach constraint violation, the value of $B\left(x\right)\rightarrow\infty$. In a similar way to penality approach shown in Eqn. {eq}`eqn-final-penality-form`, we could augment the objective (loss) function:

```{math}
:label: eqn-final-barrier-method
\arg \min_{x}\left(f(x) -\frac{1}{\lambda}\cdot{B(x)}\right)
```


The challenge of the barrier method shown in Eqn. {eq}`eqn-final-barrier-method` is selecting a starting point. The initial guess of the $x$ must be inside the barrier. If this is true, we adjust the hyperparameter $\lambda$ using the policy ({prf:ref}`obs-barrier-method-lambda-policy`):

```{prf:observation} $\lambda$-policy barrier method
:label: obs-barrier-method-lambda-policy
For a barrier method, we start with a large $\lambda$ and repeat the $x$ estimation problem with smaller and smaller values of $\lambda$. This allows us to solve the problem near the solution with limited input from the barrier. When
we gradually reduce $\lambda$, using our previous solution as a starting point, we move closer and closer to the barrier.
```

Barrier methods have on critical pathology ({prf:ref}`remark-barrier-method`):

```{prf:remark} Barrier method pathology
:label: remark-barrier-method

If, for some reason, we jump over the barrier, e.g., we start outside the feasible region (the set of $x$ values allowed by the constraints), or during the calculation, we generate a candidate solution outside the barrier, this approach can fail.
```

Let's do an example barrier method calculation that uses a [simple evolutionary algorithm](https://en.wikipedia.org/wiki/Evolutionary_algorithm) to generate new guesses for $x$ ({prf:ref}`example-barrier-method`):

````{prf:example} Barrier method example
:label: example-barrier-method
:class: dropdown

Solve the minimization problem for $x$ given by Eqn. {eq}`eqn-example-pmethod` using a barrier method for an initial guess of $x = 2.0$ for $\lambda = 10,1,0.1$.


```julia
"""
    _loss(x::Float64; λ::Float64 = 1.0) -> Float64

Loss function for barrier method example. 
"""
function _loss(x::Float64; λ::Float64 = 1.0)::Float64

    # compute the f(x) and the penalty -
    f = 100.0/x;
    B = -1/(x-5)

    # return -
    return f + (1/λ)*B;
end

"""
    main() -> Float64
Runs an evolutionary algorithm to estimate x̂ (min of loss function). 
"""
function main()::Float64

    # initialize -
    Λ = [10,1.0,0.01];
    xₒ = 2.0;
    best_loss = Inf;
    x̂ = xₒ;
    max_number_of_iterations = 1000;

    # use a simple evolutionary algorithm -
    for λ ∈ Λ

        x′ = x̂ # initialize the current x, with the best value we have so far

        # refine our best guess
        for _ ∈ 1:max_number_of_iterations
            
            # compute the loss -
            l = _loss(x′, λ = λ);
            
            # is this loss *better* than our best solution so far?
            if (l < best_loss)
                x̂ = x′;
                best_loss = l;
            end

            # generate a new guess for x -
            x′ = x̂ + 0.1*randn()
        end
    end

    # return -
    return x̂;
end

# call the main -
x = main();
```

__source__: [Source for barrier method example](https://github.com/varnerlab/CHEME-1800-4800-Course-Repository-S23/tree/main/examples/unit-3-examples/barrier_method)

````

#### Application of penalty and barrier methods
In the context of statistical modeling, penalty, and barrier methods are often used to regularize the model, which means imposing constraints on the model parameters to prevent overfitting and improve the model’s generalization ability

Several different types of penalty methods are commonly used in statistical modeling, including:
* [Ridge regression](https://en.wikipedia.org/wiki/Ridge_regression) adds a penalty term to the loss function, which is the sum of the squares of the model parameters. This has the effect of shrinking the parameters towards zero and can help reduce the model’s variance.
* [Lasso regression](https://en.wikipedia.org/wiki/Lasso_(statistics)) adds a penalty term to the loss function, which is the sum of the absolute values of the model parameters. This has the effect of setting some of the parameters to zero, which can help to select a subset of essential features and reduce the complexity of the model.
* [Group Lasso](https://en.wikipedia.org/wiki/Lasso_(statistics)#Group_lasso) is similar to [lasso regression](https://en.wikipedia.org/wiki/Lasso_(statistics)) but allows the user to group variables together and applies the penalty to the group rather than to each variable.
* [Elastic net](https://en.wikipedia.org/wiki/Lasso_(statistics)#Elastic_net) combines the ridge and lasso regression penalties and allows users to tune the balance between the two penalties.

Penalty methods can be combined with [optimization algorithms](https://optimization.cbe.cornell.edu/index.php?title=Main_Page) to find the best (optimal) values of the model parameters that minimize the loss function subject to the penalty constraints. These methods are often used in situations with many predictors, and the goal is to select a parsimonious model with a small number of essential features.

---

<!-- ## Clustering and classification
Cluster analysis groups objects such that items in the same group (called a cluster) are more similar (in some sense) to each other than to those in different clusters. Cluster analysis is an essential task of exploratory data analysis and a common technique for statistical data analysis, used in many fields, including pattern recognition, image analysis, information retrieval, bioinformatics, data compression, computer graphics, and machine learning.

k-means clustering is a method of vector quantization, originally taken from signal processing, that aims to partition n observations into k clusters in which each observation belongs to the cluster with the nearest mean (cluster centers or cluster centroid), serving as a prototype of the cluster. 

````{prf:definition} k-means clustering
:label: defn-kmeans-problem

Given a set of observations $\left(\mathbf{x}_{1},\mathbf{x}_{2},\dots,\mathbf{x}_{d}\right)$, where each observation is a $n$-dimensional vector, k-means clustering partitions the $d$-observations into $k\leq{d}$ sets $\mathcal{S} = \left\{\mathcal{S}_{1}, \mathcal{S}_{2},\dots,\mathcal{S}_{k}\right\}$ so as to minimize the within-cluster sum of squares (WCSS):

```{math}
:label: eqn-k-means-clustering
\text{arg}\min_{\mathcal{S}} \sum_{i=1}^{k}\sum_{\mathbf{x}\in\mathcal{S}_{i}}||\mathbf{x}-\mu_{i}||^{2}
```

where $\mu_{i}$ denotes the mean of the points in set $\mathcal{S}_{i}$. 

````

--- -->

## Summary
In this lecture, we introduced the learning problem, and [Ordinary least squares](https://en.wikipedia.org/wiki/Ordinary_least_squares). We started the lecture by introducing linear regression models, and the learning problem, we then explored two types of common ordinary least squares problems:

* {ref}`content:references:ols-general-introduction` is a statistical model used to analyze the relationship between a dependent variable (also known as the response variable) and one or more independent variables (also known as explanatory variables or predictors) that are believed to influence the dependent variable. 
* {ref}`content:references:ols-unconstrained-problem`. In unconstrained problems, the regression model parameters can take on any values and are not limited in any way.
* {ref}`content:references:ols-constrained-problem`. In constrained problems, the regression model parameters can be bound in some way, e.g., by physical reasoning or by some relationships between the parameters.