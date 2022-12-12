# Measurements and Distances

## Introduction
Fill me in.

---

## Vector and matrix norms
A [norm](https://en.wikipedia.org/wiki/Norm_(mathematics)) is a function that measures the length of vectors or matrices. The notion of length is handy because it enables us to define distance, i.e., similarity between vectors (or matrices) in applications such as machine learning. 

### Properties of vector norms
A vector norm is any function $||\star||:\mathbb{R}^{n}\rightarrow\mathbb{R}$ such that following properties are true:

* Non-negativity: $||\mathbf{x}||\geq{0}$ for any vector $\mathbf{x}\in\mathbb{R}^{n}$
* Multiplication by a scalar: $||\alpha\mathbf{x}|| = \alpha{||\mathbf{x}||}$ for any vector $\mathbf{x}\in\mathbb{R}^{n}$ and $\alpha\in\mathbb{R}$.
* Triangle inequality: $||\mathbf{x}+\mathbf{x}||\leq||\mathbf{x}||+||\mathbf{x}||$ for any vectors $\mathbf{x},\mathbf{y}\in\mathbb{R}^{n}$

````{prf:definition} p-norm
:label: defn-vector-p-norm
Arguably, the most commonly used vector norms belong to the family of $p$-norms (also called $l_{p}$-norms) which is defined as:

```{math}
:label: eqn-p-norm-defn
||\mathbf{x}||_{p} = \left(\sum_{i=1}^{n}|x_{i}|^{p}\right)^{1/p}
```

for any $p>0$.

````

### Properties of matrix norms
A matrix norm is any function $||\star||:\mathbb{R}^{m\times{n}}\rightarrow\mathbb{R}$ such that following properties are true:

* Non-negativity: $||\mathbf{A}||\geq{0}$ for any vector $\mathbf{A}\in\mathbb{R}^{m\times{n}}$ and $||\mathbf{A}||=0$ if and only if $\mathbf{A}=0$.
* Multiplication by a scalar: $||\alpha\mathbf{A}|| = \alpha{||\mathbf{A}||}$ for any $m\times{n}$ matrix $\mathbf{A}\in\mathbb{R}^{m\times{n}}$ and $\alpha\in\mathbb{R}$.
* Triangle inequality: $||\mathbf{A}+\mathbf{B}||\leq||\mathbf{A}||+||\mathbf{B}||$ for any matrices $\mathbf{A},\mathbf{B}\in\mathbb{R}^{m\times{n}}$

## Clustering
Fill me 

## Dimensionality reduction
Fill me in

---

## Summary
Fill me in