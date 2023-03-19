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

where $\mathbf{x}_{i}$ is a p-dimensional column vector of inputs, $\mathbf{\beta}$ is a $p\times{1}$ vector of unknown model parameters and $\epsilon_{i}$ represents unobserved random errors. Eqn. {eq}`eqn-linear-regression-model` can be re-written in matrix form as:

```{math}
:label: eqn-linear-regression-model-matrix
\mathbf{y} = \mathbf{X}\mathbf{\beta} + \mathbf{\epsilon}
```
````

Linear reagression models are widely used to model physical, chemical or economic phenomena, e.g., a model of the rate of a chemical reaction, the return of a stock, the relationship between the home price and square footage, etc.

### Sum of squared errors loss function
Starting from observations, [ordinary least squares](https://en.wikipedia.org/wiki/Ordinary_least_squares) estimates the value of the unknown parameters $\mathbf{\beta}$ that appear in a linear regression model by _minimizing_ the sum of squared errors  between model estimates and observed values ({prf:ref}`defn-sum-squared-error`):


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
In an unconstrained least-squares problem, we are trying to find the values of a set of parameters that minimize the difference between the predictions of a model and the observed data. Least-squares problems are _unconstrained_ when there are no constraints on the permissible values of the parameters. This means that the optimization algorithm can choose any values for the parameters that minimize the loss function.

### Square regression problems
Suppose we are estimating the values of the unknown parameters $\mathbf{\beta}$ in a linear regression model

```{math}
:label: eqn-square-ols-model
\mathbf{y} = \mathbf{X}\mathbf{\beta} + \mathbf{\epsilon}
```

where the data matrix $\mathbf{X}\in\mathbb{R}^{p\times{p}}$ has the same number of rows and columns as the number of unknown parameters, i.e., Eqn {eq}`eqn-square-ols-model` is a square system of linear algebraic equations. Then, we can solve for the unkown parameter vector $\mathbf{\beta}$ by solving the system of equations:

```{math}
\hat{\mathbf{\beta}} = \mathbf{X}^{-1}\mathbf{y}
```

either directly, or by using an interative approach, see our previous discussion of {ref}`content:references:soln-laes-start`.
For this approach to be applicable, the inverse of the data matrix must exist, see {prf:ref}`defn-matrix-inverse`. 


### Overdetermined regression problems
It is far more likely that the data matrix $\mathbf{X}\in\mathbb{R}^{n\times{p}}$ will be overdetermined $n\gg{p}$. In this case, the inverse of the data matrix can not be directly computed. Instead, we solve a special system of equations called the [normal equations](https://en.wikipedia.org/wiki/Ordinary_least_squares) which transforms the original overdetermined problem into a square system:

````{prf:definition} Normal solution linear regression model
:label: defn-normal-eqn-ols

There exists a dataset $\mathcal{D} = \left\{\mathbf{x}_{i},y_{i}\right\}_{i=1}^{n}$ where $\mathbf{x}_{i}$ is a p-vector of inputs (independent variables) and $y_{i}$ denotes a scalar response variable (dependent variable), and $n\gg{p}$.  Further, suppose we model the dataset $\mathcal{D}$ using the linear regression model:

```{math}
\mathbf{y} = \mathbf{X}\mathbf{\beta} + \mathbf{\epsilon}
```

Then, the value of the unknown parameter vector $\mathbf{\beta}$ that minimizes the sum of the squares loss function for an overdetermined system is given by:

```{math}
:label: eqn-loss-function-soln

\hat{\mathbf{\beta}} = \left(\mathbf{X}^{T}\mathbf{X}\right)^{-1}\mathbf{X}^{T}\mathbf{y} - \left(\mathbf{X}^{T}\mathbf{X}\right)^{-1}\mathbf{X}^{T}\mathbf{\epsilon}
```

The matrix $\mathbf{X}^{T}\mathbf{X}$ is the normal matrix, and $\mathbf{X}^{T}\mathbf{y}$ is the moment matrix. 
````

### Underdetermined regression problems
If the number of rows in the data matrix $\mathbf{X}$, i.e., the number of equations, is less than the number of columns, i.e., the number of unknowns, then the system is underdetermined. In general, there will be infinitely many solutions for an underdetermined system.

One strategy to solve this problem is to estimate values for the unknown parameters $\mathbf{\beta}$ such that they satisfy the original equations and are the smallest possible values. Thus, classically, the solution of an underdetermined least squares problem is _constrained_, i.e., the possible values are restricted somehow. 

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

<!-- This is done by defining a cost function that measures the difference between the predictions and the experimental data and then finding the parameter values that minimize this cost function. Least-squares problems are commonly used in linear regression, where the goal is to find the line of best fit for a set of data points. They are also used in many other applications, such as curve fitting, signal processing, and control systems.

Many optimization algorithms can solve least-squares problems, including gradient descent, Levenberg-Marquardt, and Gauss-Newton algorithms. The choice of algorithm will depend on the problem being solved and the characteristics of the data. -->


(content:references:ols-constrained-problem)=
## Constrained repression problems
Constrained least squares is a method of estimating the parameters of a linear regression model subject to one or more constraints on the values the parameters can take. This is often used when prior knowledge or physical constraints must be satisfied by the estimates. 

To solve a constrained least squares problem, we first define the linear regression model and the constraints on the parameters. We then define a loss function that measures the model’s fit to the data subject to the constraints. We minimize the loss function to find the estimates of the parameters that best fit the data while satisfying the constraints.

### Penalty methods
A penalty method is used to modify an optimization problem to encourage specific desirable properties of the solution. In the context of statistical modeling, penalty methods are often used to regularize the model, which means imposing constraints on the model parameters to prevent overfitting and improve the model’s generalization ability.

There are several different types of penalty methods that are commonly used in statistical modeling, including:
* [Ridge regression](https://en.wikipedia.org/wiki/Ridge_regression) adds a penalty term to the loss function, which is the sum of the squares of the model parameters. This has the effect of shrinking the parameters towards zero and can help reduce the model’s variance.
* [Lasso regression](https://en.wikipedia.org/wiki/Lasso_(statistics)) adds a penalty term to the loss function, which is the sum of the absolute values of the model parameters. This has the effect of setting some of the parameters to zero, which can help to select a subset of essential features and reduce the complexity of the model.
* [Group Lasso](https://en.wikipedia.org/wiki/Lasso_(statistics)#Group_lasso) is similar to [lasso regression](https://en.wikipedia.org/wiki/Lasso_(statistics)) but allows the user to group variables together and applies the penalty to the group rather than to each individual variable.
* [Elastic net](https://en.wikipedia.org/wiki/Lasso_(statistics)#Elastic_net) combines the ridge and lasso regression penalties and allows users to tune the balance between the two penalties.

Penalty methods can be combined with optimization algorithms such as gradient descent or coordinate descent to find the optimal values of the model parameters that minimize the objective function subject to the penalty constraints. These methods are often used in situations with many predictors, and the goal is to select a parsimonious model with a small number of essential features.

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