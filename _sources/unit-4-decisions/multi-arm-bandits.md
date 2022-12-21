# Multiple Arm Bandit Problems

## Introduction
[Reinforcement Learning (RL)](https://en.wikipedia.org/wiki/Reinforcement_learning) agents learn by performing actions in the world and then analyzing the rewards they receive ({numref}`fig-rl-schematic`). Thus, reinforcement learning differs from other machine learning approaches, e.g., [supervised learning](https://en.wikipedia.org/wiki/Supervised_learning) because labeled input/output pairs, e.g., what actions lead to positive rewards are not presented to the agent _a priori_.  Instead, reinforcement learning agents learn what is good or bad by trying different actions. 

Reinforcement learning agents learn optimal choices in different situations by balancing the exploration of their environment, e.g., by taking random actions and watching what happens, with the exploitation of the knowledge they have built up so far, i.e., choosing what the agent thinks is an optimal action based upon previous experience. The balance between exploration and exploitation is one of the critical concepts in reinforcement learning.

---

 ```{figure} ./figs/Fig-Schematic-RL.pdf
---
height: 260px
name: fig-rl-schematic
---
Schematic of the reinforcement learning (RL) process. An agent takes action $a$ and then observes reward $r$ and changes in the state of the environment ($s\rightarrow{s^{\prime}}$) following the action $a$. 
```

## Bandit Problems
Learning agents must balance exploration of the environment, e.g., taking random actions with exploiting knowledge already obtained through interacting with the world. Pure exploration allows agents to construct a comprehensive model of their environment, but likely at the expense of positive reward. On the other hand, pure exploitation has the agent continually choosing the action it thinks best to accumulate reward, but different, better actions could be taken. 

A classic approach to understanding the exploration and exploitation tradeoff, which itself has many real-world applications, is the [multi arm bandit problem](https://en.wikipedia.org/wiki/Multi-armed_bandit). A general bandit problem is a sequential game played between an agent and the environment. The game is played over $n$ rounds called the _horizon_. In each round the agent chooses an action $a\in\mathcal{A}$ and the environment then reveals a reward $r$. 

Let's look at a particular sub-type of multi-arm bandit problem, namely, the Bernoulli bandit problem and Thompson sampling, a particular solution approach to this class of problem {cite}`RUSSO-2017-TS`:

````{prf:definition} Binary Bernoulli bandit problem
:label: defn-multi-arm-bernoulli-bandit-problem

Suppose an agent has a choice between $K$ possible actions $\mathcal{A}=\left\{a_{1},a_{2},\dots,a_{K}\right\}$. Further, the success or failure of any action $a_{i}\in\mathcal{A}$ is governed by a Bernoulli random variable (see {prf:ref}`defn-pmf-bernouli`).

The agent receives a reward of $R_{a_{i}} = 1$ (success) with probability $p_{a_{i}}$, and $R_{a_{i}} = 0$ (failure) otherwise. 

The success probabilities of any action $p_{a_{i}}$, while static over the horizon of the game, are unknown to the agent, but they can be learned by experimentation. The agent’s objective is to maximize the cumulative number of successes over the game horizon, e.g., T-periods, where $T\gg{K}$. 

````

There are multiple heuristic strategies to solve the Binary Bernoulli bandit problem described in {prf:ref}`defn-multi-arm-bernoulli-bandit-problem`:

* [Thompson sampling](https://en.wikipedia.org/wiki/Thompson_sampling) which is shown in {prf:ref}`algo-thompson-sampling` consists of choosing the action that maximizes the expected reward with respect to a randomly drawn belief, where the belief distribution follows a [Beta distribution](https://en.wikipedia.org/wiki/Beta_distribution). 

```{prf:algorithm} Thompson-sampling
:label: algo-thompson-sampling

**Inputs**  number of time periods in the horizon $T$, the dimension of the action space $K = |\mathcal{A}|$, and
$\alpha_{1},\dots,\alpha_{K}$ and $\beta_{1},\dots,\beta_{K}$ the initial values (prior) for the parameters that appear in the 
Beta-distribution for each action 

**Outputs** posterior values for the parameters $\alpha_{1},\dots,\alpha_{K}$ and $\beta_{1},\dots,\beta_{K}$ that appear in the Beta-distributions for each action

**Initialize**
a Beta-distribution for each action using initial values $(\alpha,\beta)$.

**Main**
1. for t $\in$ 1 to T
    1. for k $\in$ 1 to K
        1. sample $\hat{\theta}_{k} \sim \beta(\alpha_{k}, \beta_{k})$
    

    1. Select action: $a_{t} = \text{arg}\max_{k}\hat{\theta}_{k}$
    1. Apply $a_{t}$ and observe $r_{t}$.
    1. Update distribution parameters:
        1. $(\alpha_{t},\beta_{t})\leftarrow\left(\alpha_{t}+r_{t},\beta_{t}+(1-r_{t})\right)$

**Return**
$(\alpha_{1},\dots,\alpha_{K})$ and $(\beta_{1},\dots,\beta_{K})$
```

* An obvious (but naive) alternative approach is to allocate a fixed fraction of the time steps in the time horizon, say $\epsilon$, to explore purely random actions. In contrast, in the remaining periods, only successful actions are chosen via [Thompson sampling](https://en.wikipedia.org/wiki/Thompson_sampling) or another exploitation approach. This strategy, called $\epsilon$-greedy exploration, is shown in {prf:ref}`algo-e-greedy` with a [Thompson sampling](https://en.wikipedia.org/wiki/Thompson_sampling) exploitation step:

```{prf:algorithm} $\epsilon$-greedy exploration
:label: algo-e-greedy

**Inputs**  the pure exploration parameter $\epsilon$, the number of time periods in the horizon $T$, the dimension of the action space $K = |\mathcal{A}|$, and
$\alpha_{1},\dots,\alpha_{K}$ and $\beta_{1},\dots,\beta_{K}$ the initial values (prior) for the parameters that appear in the 
Beta-distribution for each action 

**Outputs** posterior values for the parameters $\alpha_{1},\dots,\alpha_{K}$ and $\beta_{1},\dots,\beta_{K}$ that appear in the Beta-distributions for each action

**Initialize**
A Beta-distribution for each action using initial values $(\alpha,\beta)$.

**Main**
1. for t $\in$ 1 to T
    1. if rand() $<\epsilon$
        1. Select random action: $a_{t}\sim\text{Uniform}(\mathcal{A})$
    1. else
        1. for k $\in$ 1 to K
            1. sample $\hat{\theta}_{k} \sim \beta(\alpha_{k}, \beta_{k})$
    
        1. Select action: $a_{t} = \text{arg}\max_{k}\hat{\theta}_{k}$
    
    1. Apply $a_{t}$ and observe $r_{t}$.
    1. Update distribution parameters:
        1. $(\alpha_{t},\beta_{t})\leftarrow\left(\alpha_{t}+r_{t},\beta_{t}+(1-r_{t})\right)$

**Return**
$(\alpha_{1},\dots,\alpha_{K})$ and $(\beta_{1},\dots,\beta_{K})$
```


---

## Summary 
Fill me in.