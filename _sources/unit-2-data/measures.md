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

## Cluster analysis
Cluster analysis groups objects such that items in the same group (called a cluster) are more similar (in some sense) to each other than to those in different clusters. Cluster analysis is an essential task of exploratory data analysis and a common technique for statistical data analysis, used in many fields, including pattern recognition, image analysis, information retrieval, bioinformatics, data compression, computer graphics, and machine learning.

### k-means clustering
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

## Dimensionality reduction
Fill me in

---

## Summary
Fill me in