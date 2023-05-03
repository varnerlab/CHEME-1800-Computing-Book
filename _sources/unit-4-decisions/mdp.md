# Markov Decision Processes

## Introduction
A Markov decision process (MDP) provides a mathematical framework for modeling decision-making in situations where outcomes are partly random and partly under the control of a decision-maker. MDPs take their name from the Russian mathematician [Andrey Markov](https://en.wikipedia.org/wiki/Andrey_Markov),  as they are an extension of [Markov chains](https://en.wikipedia.org/wiki/Markov_chain), a stochastic model which describes a sequence of possible events in which the probability of each event depends only on the system state in the previous event.

In this lecture:

* We will discuss {ref}`content:references:markov-chains` and discrete time {ref}`content:references:structure-of-an-hmm`, which are approaches for modeling the evolution of a stochastic system as a series of possible events.
* We will also discuss {ref}`content:references:structure-of-an-mdp`, which is an approach for making decisions in a probabilistic world. 

---

(content:references:markov-chains)=
## Markov chains
A Markov chain is a stochastic model describing a sequence of possible events where the probability of each of these events depends only on the system’s current state and not on past system states. A system's state space and time (much like a probability) can be either discrete or continuous; for most of the applications we'll be interested in, we'll focus on discrete time and discrete finite state spaces.


(content:references:discrete-time-markov-chains)=
### Discrete-time Markov chains


 ```{figure} ./figs/Fig-Discrete-MarkovChain-Schematic.pdf
---
height: 120px
name: fig-discrete-markov-model
---
Schematic of a discrete two-state time-invariant Markov model; $p_{ij}$ denotes the time-invariant transition probability between state $i$ and $j$.
```

A discrete-time Markov chain is a sequence of random variables $X_{1}$, $X_{2}$, $X_{3}$, ..., $X_{n}$ that have the [Markov property](https://en.wikipedia.org/wiki/Markov_property), i.e., the probability of moving to the _next state_ depends only on the _present state_ and not on the _previous states_:

```{math}
:label: eqn-markov-property
P(X_{n+1} = x | X_{1}=x_{1}, \dots, X_{n}=x_{n}) = P(X_{n+1} = x | X_{n}=y)
```

where _states_ refer to a finite set of discrete values in which the system can exist.  If the state space is finite, the transition probability distribution, i.e., the probability of moving from the state(s) $i\rightarrow{j}$, can be encoded in the transition matrix $\mathbf{P}$. Elements of $\mathbf{P}$, denoted as $p_{ij}$, encode the probability of moving from state $i\rightarrow{j}$ during the next time step:

```{math}
:label: eqn-transition-prob-matrix
p_{ij} = P(X_{n+1}~=~j~|~X_{n}~=~i)
```

The transition matrix $\mathbf{P}$ has interesting properties: 
* First, the rows of transition matrix $\mathbf{P}$ must sum to unity, i.e., each row encodes the probability of all possible outcomes. Thus, it must sum to one. 
* Second, if the transition matrix  $\mathbf{P}$ is time-invariant, then $\mathbf{P}$ is the same at each step, i.e., $p_{ij}$ doesn't change as $n\rightarrow{n+1}~\forall{n}$.

Putting these ideas together gives ({prf:ref}`defn-n-transition`):

````{prf:definition} Time-invariant state transition
:label: defn-n-transition

A Markov chain has finite state set $\mathcal{S}$ and the time-invariant state transition matrix $\mathbf{P}$. Let the state vector at time $j$ be given by $\mathbf{x}_{j}$, where $x_{s,j}\geq{0},\forall{s}\in\mathcal{S}$ and:

```{math}
\sum_{s\in\mathcal{S}}x_{s,j} = 1\qquad\forall{j}
```

Then, the state of the Markov chain at time step $n+1$ is given by:

$$\mathbf{x}_{n+1} = \mathbf{x}_{n}\mathbf{P}^n$$

where $\mathbf{x}_{n}$ denotes the system state vector at time step $n$. 
````

Finally, suppose that a Markov chain is both time-invariant and non-periodic. Then, there exists a unique stationary distribution $\pi$ such that $\mathbf{P}^{k}$ converges to a rank-one matrix in which each row is the stationary distribution $\pi$:

```{math}
\lim_{k\rightarrow\infty} \mathbf{P}^{k} = \mathbf{1}\pi
```

where $\mathbf{1}$ is a column vector of all 1's. Let's consider an example to make these ideas less abstract ({prf:ref}`example-dicrete-mchain`): 


````{prf:example} Discrete Markov chain stationary distribution
:class: dropdown
:label: example-dicrete-mchain

Consider the time-invariant two-state Discrete Markov chain with state transition matrix $\mathbf{P}$:

$$
\mathbf{P} = \begin{bmatrix}
0.9 & 0.1 \\
0.6 & 0.4 \\
\end{bmatrix}
$$

shown in ({numref}`fig-discrete-markov-model`). This transition matrix admits a stationary (non-periodic) solution. As the number of iterations $n$ becomes large the system state converges to a stationary distribution $\pi$. Thus, regardless of the starting state of this Markov chain, the long-term behavior is given by the stationary distribution $\pi$.

Develop a script to compute the stationary distribution $\pi$:

```julia
# load package -
using LinearAlgebra

"""
    iterate(P::Array{Float64,2}, counter::Int; 
        maxcount::Int = 100, ϵ::Float64 = 0.1) -> Array{Float64,2}

Recursively computes a stationary distribution. 
Computation stops if ||P_new - P|| < ϵ or the max number of iterations is hit. 
"""
function iterate(P::Array{Float64,2}, counter::Int; maxcount::Int = 100, ϵ::Float64 = 0.1)::Array{Float64,2}

    # base case -
    if (counter == maxcount)
        return P
    else
        # generate a new P -
        P_new = P^(counter+1)
        err = P_new - P;
        if (norm(err)<=ϵ)
            return P_new
        else
            # we have NOT hit the error target, or the max iterations
            iterate(P_new, (counter+1), maxcount=maxcount, ϵ = ϵ)
        end
    end
end

# Setup the transition probability matrix -
P = [
    0.9 0.1;
    0.6 0.4;
];

# compute -
counter = 1
P_stationary = iterate(P, counter;  maxcount = 10, ϵ = 0.001)
```
````

#### Sampling the stationary distribution
Once the stationary distribution $\pi$ of the Markov chain has been estimated, we can use it to generate samples from that chain. 
For example, the stationary distribution for the two-state Markov chain in {prf:ref}`example-dicrete-mchain` is given by:

```{math}
\pi = (0.857,0.143)
```

We model this system as a [Categorical distribution](https://en.wikipedia.org/wiki/Categorical_distribution). A categorical distribution is a discrete probability distribution that models the probability of a random variable taking on one of a finite set of possible outcomes:

```{math}
:label: eqn-categorical-dist

P(X = k) = \pi\left[k\right]
```

Sampling a categorical distribution constructed from the stationary distribution $\pi$ will return the stationary 
distribution ({prf:ref}`example-categorical-dist`):

````{prf:example} Categorical distribution
:class: dropdown
:label: example-categorical-dist

Sample a categorical distribution constructed using the stationary distribution:

```{math}
\pi = (0.857,0.143)
```

Generate $N = 1000$ samples and compute the fraction of state `1` and state `2`.


```julia
# include -
include("Include.jl")

# setup -
π = [ 0.857, 0.143];
number_of_samples = 1000;

# build a categorical distribution 
d = Categorical(π);

# sample -
samples = rand(d, number_of_samples)

# how many 1's -
number_of_1 = findall(x-> x == 1, samples) |> length;
number_of_2 = findall(x-> x == 2, samples) |> length;

# println -
println("Fraction of state 1: $(number_of_1/number_of_samples) and state 2: $(number_of_2/number_of_samples)")
``` 

````

(content:references:structure-of-an-hmm)=
### Hidden Markov Models (HMMs)
Hidden Markov models (HMMs) are statistical models in which the system being modeled is assumed to be a Markov process with unobservable states $s\in\mathcal{S}$ but observable outcomes $o\in\mathcal{O}$. HMMs have the same structural components as a standard Markov chain model. However, each hidden state can be considered sending an observable single, with the emission probability. 


 ```{figure} ./figs/Fig-HMM-Schematic-23.pdf
---
height: 280px
name: fig-discrete-hidden-markov-model
---
Schematic of a discrete two-state time-invariant hidden Markov model (HMM); $p_{ij}$ denotes the time-invariant transition probability between state $i$ and $j$ while $t_{ij}$ denotes the emission probability for state $i$ and observation $j$.
```


The emission probability refers to the likelihood of observing a particular output $Y = o_{t}$, given the current state of the Markov chain $X = s_{t}$:

```{math}
:label: eqn-hmm-output
P(Y = o_{t} | X = s_{t})
```

Similar to the transition probability, the emission probability must sum to unity:

```{math}
\sum_{o\in\mathcal{O}} P(Y = o | X = s) = 1\qquad\forall{s\in\mathcal{S}}
```

The emission probability plays a crucial role in HMMs, as it is used to calculate the likelihood of a sequence of observed symbols, given the current state of the hidden Markov chain. This likelihood is then used in various applications, including speech recognition, natural language processing, and bioinformatics. The emission probability can be computed using different methods, including maximum likelihood estimation or Bayesian inference.

Let's build upon {prf:ref}`example-dicrete-mchain` and construct an HMM that mimics a [trinomial lattice model of Boyle](https://en.wikipedia.org/wiki/Trinomial_tree) ({prf:ref}`example-dicrete-mchain-hmm`):

````{prf:example} Stationary hidden Markov model
:class: dropdown
:label: example-dicrete-mchain-hmm

Model the share price of a stock `XYZ` using a time-invariant two-state Discrete Markov chain with the state transition matrix $\mathbf{P}$:

$$
\mathbf{P} = \begin{bmatrix}
0.60 & 0.40 \\
0.35 & 0.65 \\
\end{bmatrix}
$$

and an emission probability matrix $\mathbf{E}$:

$$
\mathbf{E} = \begin{bmatrix}
0.70 & 0.20 & 0.1 \\
0.10 & 0.20 & 0.7 \\
\end{bmatrix}
$$

Let $Y_{i}$ denote the observable value for state $i$. Assume output state `1` is an up move (a 1% increase), state `2` the price stays the same, and state `3` indicates a price drop (a 1% decrease).

Simulation script:

```julia
# include the include -
include("Include.jl"); # load paths, packages and codes

# PHASE 1: Setup the calculation
# Setup/initialize
number_of_hidden_states = 2
number_of_output_states = 3
number_of_simulation_steps = 480
emission_probability_dict = Dict{Int,Categorical}()
simulation_dict = Dict{Int,Int}()

# Transition matrix for the MC
P = [
    0.60 0.40;
    0.35 0.65;
];

# Emission probability matrix -
EPM = [
    0.70 0.20 0.1 ;
    0.10 0.20 0.7 ;
]

# compute the stationary distribution for the MC -
π = iterate(P, 1;  maxcount = 10, ϵ = 0.001)[1,:] # assumption: iterate converges
mcd = Categorical(π); # Markov-chain distribution

# construct emission probability dictionary -
for i ∈ 1:number_of_hidden_states
    emission_probability_dict[i] = Categorical(EPM[i,:])
end

# PHASE 2: Simulation 
for i ∈ 1:number_of_simulation_steps
    
    # which state is the mc in?
    hidden_state = rand(mcd);
    
    # grab the emission probability model from the emission_probability_dict -
    epd = emission_probability_dict[hidden_state];
    
    # role for a random ouput -
    simulation_dict[i] = rand(epd);
end
```

Plotting script:

```julia
# runme -
include("runme.jl") # loads packages, computes simulation_dict -

# Setup -
Sₒ = 100.0;
Δ = [0.01, 0.0, -0.01];
S = Dict{Int,Float64}()
S[0] = Sₒ

# setup colors -
colors = Dict{Int,RGB}();
colors[1] = colorant"#0077BB"

# sim loop -
for i ∈ 1:number_of_simulation_steps

    # market state -
    market_state_index = simulation_dict[i]

    # get previous price -
    S_old = S[i-1];
    S[i] = S_old*exp(Δ[market_state_index]);
end

# make a plot -
plot!(S,lw=3,c=colors[1],label="")
xlabel!("Time index (AU)", fontsize=18)
ylabel!("Share price (USD/share)", fontsize=18)
```

````


(content:references:structure-of-an-mdp)=
## Markov decision processes (MDPs)
A Markov decision process (MDP) is a way to model decision-making where outcomes are partly random and partly controlled by a decision-maker who receives a reward or penalty for each decision ({prf:ref}`defn-formal-mdp`):

````{prf:definition} Markov decision tuple 
:label: defn-formal-mdp

A Markov decision process is the tuple $\left(\mathcal{S}, \mathcal{A}, R_{a}\left(s, s^{\prime}\right), T_{a}\left(s,s^{\prime}\right), \gamma\right)$ where:

* The state space $\mathcal{S}$ is the set of all possible states $s$ that a system can exist in
* The action space $\mathcal{A}$ is the set of all possible actions $a$ that are available to the agent, where $\mathcal{A}_{s} \subseteq \mathcal{A}$ is the subset of the action space $\mathcal{A}$ that is accessible from state $s$.
* An expected immediate reward $R_{a}\left(s, s^{\prime}\right)$ is received after transitioning from state $s\rightarrow{s}^{\prime}$ due to action $a$. 
* The transition $T_{a}\left(s,s^{\prime}\right) = P(s_{t+1} = s^{\prime}~|~s_{t}=s,a_{t} = a)$ denotes the probability that action $a$ in state $s$ at time $t$ will result in state $s^{\prime}$ at time $t+1$
* The quantity $\gamma$ is a _discount factor_; the discount factor is used to weight the _future expected utility_.

Finally, a policy function $\pi$ is the (potentially probabilistic) mapping from states $s\in\mathcal{S}$ to actions $a\in\mathcal{A}$ used by the agent to solve the decision task. 
````

At each time step, the system is in state $s\in\mathcal{S}$ and the decision maker may choose any action $a\in\mathcal{A}$ that are available in state $s$. The system responds at the next time step by _potentially_ moving into a new state $s^{\prime}$ and rewarding the decision maker $R_{a}\left(s, s^{\prime}\right)$. The probability that the system moves into a new state $s^{\prime}$ depends upon the chosen action $a$ and the current state $s$; this probability is governed by a state transition function $P_{a}\left(s,s^{\prime}\right)$.

### Policy evaluation
When determining the best policy for a decision problem, we first need to understand what a policy function $\pi$ is and how to evaluate it. This process is known as policy evaluation. We calculate the expected utility gained by executing a policy $\pi(s)$ from state $s$ and denote it as $U^{\pi}(s)$. An optimal policy function $\pi^{\star}$ maximizes the expected utility and can be defined as:

$$\pi^{\star}\left(s\right) = \text{arg max}~U^{\pi}(s)$$

for all $s\in\mathcal{S}$. To determine the utility of a policy $\pi$, we can iteratively calculate it. If the agent makes a single move, the utility is the reward received by implementing policy $\pi:\mathcal{S}\rightarrow\mathcal{A}$:

$$U_{1}^{\pi}(s) = R(s,\pi(s))$$

However, if the agent performs two, three, or $k$ possible iterations, we use a lookahead equation that relates the utility value at iteration $k$ to $k+1$:

$$U_{k+1}^{\pi}(s) = R(s,\pi(s)) + \gamma\sum_{s^{\prime}\in\mathcal{S}}T(s^{\prime} | s,\pi(s))U_{k}^{\pi}(s^{\prime})$$

As $k\rightarrow\infty$ the lookahead utility converges to a stationary value $U^{\pi}(s)$ ({prf:ref}`defn-policy-evalution`):

````{prf:definition} Value function
:label: defn-policy-evalution

We have a Markov decision process with the tuple $\left(\mathcal{S}, \mathcal{A}, R_{a}\left(s, s^{\prime}\right), T_{a}\left(s,s^{\prime}\right), \gamma\right)$. The utility of the policy function $\pi:\mathcal{S}\rightarrow\mathcal{A}$ equals:

```{math}
:label: eqn-converged-policy-eval
U^{\pi}(s) = R(s,\pi(s)) + \gamma\sum_{s^{\prime}\in\mathcal{S}}T(s^{\prime} | s, \pi(s))U^{\pi}(s^{\prime})
```

The utility associated with an optimal policy $\pi^{\star}$ is called the optimal utility $U^{\star}$. 

````

Let's do an example to illustrate policy evaluation ({prf:ref}`example-MDP-line`):

````{prf:example} Tiger problem
:label: example-MDP-line
:class: dropdown

 ```{figure} ./figs/Fig-Linear-MDP-Schematic.pdf
---
height: 110px
name: fig-linear-mdp-schematic
---
Schematic of the Tiger problem modeled as an N-state, two-action Markov decision process. A tiger hides behind the red door while freedom awaits behind the green door.
```

An agent trapped in a long hallway with two doors at either end ({numref}`fig-linear-mdp-schematic`). Behind the red door is a tiger (and certain death), while behind the green door is freedom. If the agent opens the red door, the agent is eaten (and receives a large negative reward). However, if the agent opens the green door, it escapes and gets a positive reward. 

For this problem, the MDP has the tuple components:
* $\mathcal{S} = \left\{1,2,\dots,N\right\}$ while the action set is $\mathcal{A} = \left\{a_{1},a_{2}\right\}$; action $a_{1}$ moves the agent one state to the right, action $a_{2}$ moves the agent one state to the left.
* The agent receives a reward of +10 for entering state 1 (escapes). However, the agent is penalized -100 for entering state N (eaten by the tiger).  Finally, the agent is not charged to move to adjacent locations.
* Let the probability of correctly executing the action $a_{j}$ be $\alpha$

Let's compute $U^{\pi}(s)$ for different choices for the policy function $\pi$.

__source__: [download the live Jupyter notebook from GitHub](https://github.com/varnerlab/CHEME-1800-4800-Course-Repository-S23)
````

### Value function policies
{prf:ref}`defn-policy-evalution` gives us a method to compute the utility for a particular policy $U^{\pi}(s)$. 
However, suppose we were given the utility and wanted to estimate the policy $\pi(s)$ from that utility. 
Given a utility $U$, we can estimate a policy $\pi(s)$ using the $Q$-function (action-value function):

```{math}
:label: eqn-action-value-function
Q(s,a) = R(s,a) + \gamma\sum_{s^{\prime}\in\mathcal{S}}T(s^{\prime} | s, a)U(s^{\prime})
```

Equation {eq}`eqn-action-value-function` gives a $|\mathcal{S}|\times|\mathcal{A}|$ array (states on the rows, actions on the columns), where the utility is given by:

```{math}
:label: eqn-utility-from-Q
U(s) = \max_{a} Q(s,a)
```

and the policy $\pi(s)$ is:

```{math}
:label: eqn-policy-from-Q
\pi(s) = \text{arg}\max_{a}Q(s,a)
```

### Value iteration
In the previous section, we saw how we could develop _a policy_ $\pi(s)$ by looking at the values in the $Q$-array. However, this required the utility vector; thus, we needed to hypothesize a policy that may not be the _optimal policy_. There are two techniques to compute optimal policies, and we'll explore the simpler of the two, namely _value iteration_. 

In _value iteration_, the value function (the vector of utility values) is updated iteratively using the _Bellman update_ procedure:

```{math}
U_{k+1}(s) = \max_{a}\left(R(s,a) + \gamma\sum_{s^{\prime}\in\mathcal{S}}T(s^{\prime} | s, a)U_{k}(s^{\prime})\right)
```

This procedure is guaranteed to converge to the optimal utility vector (value function).  

````{prf:example} Modified Tiger problem
:label: example-MDP-line-mod
:class: dropdown

 ```{figure} ./figs/Fig-Branched-MDP-Schematic.pdf
---
height: 300px
name: fig-branched-mdp-schematic-mod
---
Schematic of the Tiger problem modeled as an N-state, four-action (left, right, up, down) Markov decision process. The hallway has three types of paths: unobstructed (white, free), mildly obstructed (light gray, small cost), and obstructed (dark gray, large cost).
```

An agent is trapped in a long hallway with two doors at either end ({numref}`fig-branched-mdp-schematic-mod`). Behind the green door is a tiger (and certain death), while behind the red door is freedom. If the agent opens the green door, the agent is eaten (and receives a large negative reward). However, if the agent opens the red door, it escapes and gets a positive reward. 

For this problem, the MDP has the tuple components:
* $\mathcal{S} = \left\{1,2,\dots,N\right\}$ while the action set is $\mathcal{A} = \left\{a_{1},a_{2}, a_{3}, a_{4}\right\}$; action $a_{1}$ moves the agent one state to the left, action $a_{2}$ moves the agent one state to the right, action $a_{3}$ moves the agent one stop up, and action $a_{4}$ moves the agent one step down.
* The agent receives a positive reward for entering the red state $N$ (escapes). However, the agent is penalized for entering the green state $1$ (eaten by the tiger).  Finally, the agent is not charged to move to adjacent locations if those locations are unobstructed. However, there is a small charge to move through mildly obstructed locations (light gray circles) and a larger charge to move through obstructed areas (dark gray circles).
* Let the probability of correctly executing an action $a_{j}\in\mathcal{A}$ be $\alpha$.

Let's use value iteration to estimate the _optimal policy_ $\pi^{\star}(s)$

__source:__ [download the live Jupyter notebook from GitHub](https://github.com/varnerlab/CHEME-5660-Markets-Mayhem-Example-Notebooks)
````

---

# Summary 
A Markov decision process (MDP) provides a mathematical framework for modeling decision-making in situations where outcomes are partly random and partly under the control of a decision-maker. MDPs take their name from the Russian mathematician [Andrey Markov](https://en.wikipedia.org/wiki/Andrey_Markov),  as they are an extension of [Markov chains](https://en.wikipedia.org/wiki/Markov_chain), a stochastic model which describes a sequence of possible events in which the probability of each event depends only on the system state in the previous event.

In this lecture:

* We discussed {ref}`content:references:markov-chains` and discrete time {ref}`content:references:structure-of-an-hmm`, which are approaches for modeling the evolution of a stochastic system as a series of possible events. We developed a hidden Markov model for the nodes in a binomial lattice model. 
* We also discussed {ref}`content:references:structure-of-an-mdp`, which is an approach for making decisions in a probabilistic world. In particular, we 
introduced tools to compute the utility of a decision-making policy and the value iteration approach for estimating optimal decision-making policies. 