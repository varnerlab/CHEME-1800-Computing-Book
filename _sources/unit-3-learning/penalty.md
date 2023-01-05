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
Fill me in

(content:references:ols-constrained-problem)=
## Constraints and non-linear models
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

## Summary
Fill me in