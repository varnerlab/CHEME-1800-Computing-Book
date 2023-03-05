# Ordinary Least Squares

## Introduction
We discuss Ordinary least squares (OLS) in this lecture. Ordinary least squares is a statistical method to estimate the parameters of a linear regression model. In a linear regression model, the goal is to find the line of best fit that predicts the values of the response variable (dependent variable) based on the values of the predictor variables (independent variables). The OLS method finds the best fit line by minimizing the sum of the squared differences between the observed values of the response variable and the predicted values based on the regression line. This is done by choosing the values of the model parameters that minimize the sum of squared errors (SSE).

OLS is a popular method for estimating the parameters of a regression models because it is relatively simple to implement and it has nice mathematical properties, such as being unbiased and having the smallest variance among all unbiased estimators. However, OLS has some limitations, such as assuming that the errors in the model are Normally distributed and having poor performance when the data is highly correlated or has outliers.

In particular, we discuss:
* Ordinary least squares (OLS) approaches for {ref}`content:references:ols-unconstrained-problem`. In this class or problem, the model parameters can take on any values
* We then switch to {ref}`content:references:ols-constrained-problem` in which the model parameters can be bound in some way, and the parameters can appear non-linearly in the model.

---

(content:references:ols-unconstrained-problem)=
## Unconstrained problems
In an unconstrained least-squares problem, we are trying to find the values of a set of parameters that minimize the difference between the predictions of a model and the observed data. This is done by defining a cost function that measures the difference between the predictions and the experimental data and then finding the parameter values that minimize this cost function. Least-squares problems are commonly used in linear regression, where the goal is to find the line of best fit for a set of data points. They are also used in many other applications, such as curve fitting, signal processing, and control systems.

Least-squares problems are called "unconstrained" because there are no constraints on the values of the parameters. This means that the optimization algorithm can choose any values for the parameters that minimize the cost function.

Many optimization algorithms can solve least-squares problems, including gradient descent, Levenberg-Marquardt, and Gauss-Newton algorithms. The choice of algorithm will depend on the problem being solved and the characteristics of the data.


(content:references:ols-constrained-problem)=
## Constraints
Constrained least squares is a method of estimating the parameters of a linear regression model subject to one or more constraints on the values the parameters can take. This is often used when prior knowledge or physical constraints must be satisfied by the estimates. To solve a constrained least squares problem, we first define the linear regression model and the constraints on the parameters. We then define an objective function that measures the model’s fit to the data subject to the constraints. We minimize this objective function to find the estimates of the parameters that best fit the data while satisfying the constraints.

### Penalty methods
A penalty method is used to modify an optimization problem to encourage specific desirable properties of the solution. In the context of statistical modeling, penalty methods are often used to regularize the model, which means imposing constraints on the model parameters to prevent overfitting and improve the model’s generalization ability.

There are several different types of penalty methods that are commonly used in statistical modeling, including:
* Ridge regression: This method adds a penalty term to the objective function, which is the sum of the squares of the model parameters. This has the effect of shrinking the parameters towards zero and can help reduce the model’s variance.
* Lasso regression: This method adds a penalty term to the objective function, which is the sum of the absolute values of the model parameters. This has the effect of setting some of the parameters to zero, which can help to select a subset of essential features and reduce the complexity of the model.
* Group Lasso: This method is similar to lasso regression but allows the user to group variables together and applies the penalty to the group rather than to each individual variable.
* Elastic net: This method combines the ridge and lasso regression penalties and allows users to tune the balance between the two penalties.

Penalty methods can be combined with optimization algorithms such as gradient descent or coordinate descent to find the optimal values of the model parameters that minimize the objective function subject to the penalty constraints. These methods are often used in situations with many predictors, and the goal is to select a parsimonious model with a small number of essential features.

---

## Clustering and classification
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

## Summary
Fill me in