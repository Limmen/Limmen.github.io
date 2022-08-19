---
title: Structured Causal Models (SCMS) and MDPs
updated: 2022-08-19 08:30
---

Structured causal models are the standard models in the causal modeling literature and Markov decision processes are the standard in the decision-theoretic literature. In this blog post I will demonstrate how a single use case can be modeled both with an MDP and with an SCM. We start with some background

## Structured causal models (SCMs)

A structured causal model (SCM) encodes causal relationships among variables and is defined by the tuple

$$M = \langle U,V, F\rangle$$.

$$U$$ is a set of *exogeneous* variables that are determined by factors outside the model and

$$V=\{V_1,...,V_{|V|}\}$$

is a set of *endogeneous* variables that are determined by $$U \cup V$$. $$M$$ is associated with a directed acyclic graph (DAG) $$\mathcal{G}$$ that factorizes $$\mathbb{P}[V]$$ as:

$$\mathbb{P}[V] = \prod_{i}^{|V|}\mathbb{P}[V_i|Pa(V_i)]$$

, called a *causal diagram* of $$M$$. A node in $$\mathcal{G}$$ corresponds to a variable in $$U \cup V$$ and the directed edges point from elements of

$$U_i \cup Pa(V_i)$$

to $$V_i$$ for $$i=1,...,|V|$$, where

$$Pa(V_i) \subseteq V\setminus V_i$$

is the set of predecessors of $$V_i$$ in $$\mathcal{G}$$.

The relationships among the variables in $$U \cup V$$ are determined by a set of functions $$F = \{f_1,...,f_n\}$$ such that each $$f_i$$ is a mapping from

$$dom(U_i) \cup dom(Pa(V_i))$$ to $$dom(V_i)$$:

$$v_i = f_i(pa_i, u_i), \quad \quad i=1,...,n $$

where $$U_i \subseteq U$$, and $$dom(\cdot)$$ denotes the domain of a variable.

![example causal diagram](/assets/ctf_example_4.png "example causal diagram")

## Markov decision processes (MDPs)

A Markov Decision Process (MDP) models the control of a discrete-time dynamical system and is defined by a seven-tuple $$\mathcal{M} = \langle \mathcal{S}, \mathcal{A}, \mathcal{P}^{a_t}_{s_t,s_{t+1}}, \mathcal{R}^{a_t}_{s_t,s_{t+1}}, \gamma, \rho_1, T \rangle$$. $$\mathcal{S}$$ denotes the set of states and $$\mathcal{A}$$ denotes the set of actions. $$\mathcal{P}^{a_t}_{s_t,s_{t+1}}$$ refers to the probability of transitioning from state $$s_t$$ to state $$s_{t+1}$$ when taking action $$a_t$$, which has the Markov property $$\mathbb{P}\left[s_{t+1}|s_t\right] = \mathbb{P}\left[s_{t+1}| s_1, ..., s_t\right]$$:

$$\mathcal{P}^{a_t}_{s_t,s_{t+1}} = \mathbb{P}\left[s_{t+1}| s_t, a_t\right]$$

Similarly, $$\mathcal{R}^{a_t}_{s_t,s_{t+1}} \in \mathbb{R}$$ is the expected reward when taking action $$a_t$$ and transitioning from state $$s_t$$ to state $$s_{t+1}$$:

$$\mathcal{R}^{a_t}_{s_t,s_{t+1}} = \mathbb{E}\left[r_{t+1}| a_t,  s_t, s_{t+1}\right]$$

which is bounded, i.e. $$|\mathcal{R}^{a_t}_{s_t,s_{t+1}}| \leq M < \infty$$ for some $$M \in \mathbb{R}$$. If $$\mathcal{P}^{a_t}_{s_t,s_{t+1}}$$ and $$\mathcal{R}^{a_t}_{s_t,s_{t+1}}$$ are independent of the time-step $$t$$, the MDP is *stationary* and if $$\mathcal{S}$$ and $$\mathcal{A}$$ are finite, the MDP is *finite*. Finally, $$\gamma \in \left[0,1\right]$$ is the discount factor, $$\rho_1 : \mathcal{S} \rightarrow [0,1]$$ is the initial state distribution, and $$T$$ is the time horizon.

The system evolves in discrete time-steps from $$t=1$$ to $$t=T$$, which constitute one *episode* of the system.

## Example Problem Modeled both as an MDP and as an SCM

Consider the classical computer security challenge of *Capture the Flag* (CTF). In this challenge, the goal of the decision maker is to collect a set of flags hidden in a computer infrastructure. For ease of exposition, we focus on the case where there is only five computers, a single flag, and four time-steps ($$T=4$$) see the figure below:

![example shortest path problem](/assets/ctf_example_1.png "example shortest path problem")

We model the infrastructure as a graph $$\mathcal{G} = \langle \mathcal{N}, \mathcal{E} \rangle$$. The nodes $$\mathcal{N} = \{N_1,...,N_5\}$$ represent components of the infrastructure and the edges $$\mathcal{E} = \{e_{1,2}, e_{1,3}, e_{3,1}, e_{2,4}, e_{4,2}, e_{3,4}, e_{4,5}, e_{5,4}\}$$ represent connectivity between components. A flag is hidden at node $$N_5$$. The decision maker is located in a dedicated start position $$N_1$$, from which he can move to adjacent nodes in the infrastructure through execution of network commands. Associated with each edge $$e_{i,j}$$ is a cost $$C_{i,j}$$, that represents the cost of moving from node $$i$$ to node $$j$$ in the network:

$$
C =
\begin{bmatrix}
50 & 1 & 10 & \infty & \infty \\
\infty & 50 & \infty & 20 & \infty \\
10 & \infty & 50 & 1 & \infty \\
\infty & 20 & \infty & 50 & 5 \\
\infty & \infty & \infty & 5 & 0 \\
\end{bmatrix}
$$

here we adopt the convention that $$C_{i,j} = \infty$$ means that there is no control that moves the decision maker from $$i$$ to $$j$$.

Commands are assumed to be executed in a round-based fashion and time progresses in discrete time-steps $$t=1,...,T$$, where $$T=4$$. At $$t=T$$, the decision maker incurs a terminal reward $$D_i$$:

$$
D =
\begin{bmatrix}
0\\
0\\
0\\
0\\
10
\end{bmatrix}
$$

, which depends on whether his current position in $$\mathcal{G}$$ contains a flag or not.

We assume a model of perfect information where all parameters of the model are observable to the decision maker. The objective is to capture the flag with the minimal cost within a fixed time horizon $$T$$. What strategy achieves this end?

### The Example Problem as a Markov Decision Process

We model the above use case as an MDP $$\mathcal{M} = \langle \mathcal{S}, \mathcal{A}, \mathcal{R}, \mathcal{P}, \gamma, \rho_1, T \rangle$$ as follows:

$$
\mathcal{S} = \{1,2,3,4,5\} \quad\quad\quad \text{state space}\\
\mathcal{A} = \mathcal{S} = \{1,2,3,4,5\} \quad\quad\quad \text{action space}\\
\mathcal{R}^{a_t}_{s_t} = -C_{s_t,a_t} \quad\quad\quad \text{reward function}\\
\mathcal{R}_{s_t} = D_{s_t} \quad\quad\quad \text{terminal rewards}\\
\mathcal{P}_{s_t,s_{t+1}}^{a_t} =
\begin{dcases}
  1 & \quad \text{if }s_{t+1}= a_t \\
  0 & \quad \text{otherwise}
\end{dcases} \quad\quad\quad \text{transition probabilities}\\
\rho_1(s) =
\begin{dcases}
  1 & \quad \text{if }s=1 \\
  0 & \quad \text{otherwise}
\end{dcases} \quad\quad\quad \text{initial state distribution}\\
T &= 4 && \text{time horizon}  \\
\gamma &= 1 \quad\quad\quad \text{discount factor}\\
\pi_t^{*} \in \argmax_{\pi_t \in \Pi} \mathbb{E}_{\pi_t}\left[\sum_{t=1}^{T}\gamma^{t-1}r_{t}\right] \quad\quad\quad \text{objective}
$$

where $$\Pi$$ is the set of Markovian and deterministic policies (It follows from classical results in Markov decision theory that an optimal policy $$\pi^{*}$$ that is deterministic and Markovian exists.):

$$
  \Pi = \Bigg\{\Big((1,1)\rightarrow 1, (1,2)\rightarrow 2, (1,3)\rightarrow 3, (1,4)\rightarrow 4, (1,5)\rightarrow 5,\\
           ..\\
          (T,1)\rightarrow 1, (T,2)\rightarrow 2, (T,3)\rightarrow 3, (T,4)\rightarrow 4, (T,5)\rightarrow 5\Big),
            ..\\
  \Bigg\}
$$

### The Example Problem as a Structured Causal Model (SCM)

Now we model the same use case as an SCM $$M = \langle U,V, F\rangle$$:

$$
V = X \cup Z \cup Y \quad\quad\quad \text{endogeneous variables}\\
X = \{X_1,X_2,X_3\} \quad\quad\quad \text{control variables}    \\
Z = \{Z_1,Z_2,Z_3, Z_4\} \quad\quad\quad \text{covariates (states)}      \\
Y = \{Y\} \quad\quad\quad \text{outcome variable}\\
U = \{U,\pi\} \quad\quad\quad \text{exogeneous variables}\\
F = \{f_{X_1},f_{X_2},f_{X_3},f_{Z_1},f_{Z_2},f_{Z_3},f_{Z_4},f_{Y}\} \quad\quad\quad \text{system functions}\\
dom(X_i) = \{1,2,3,4,5\} \quad\quad\quad \text{control values}\\
dom(Z_i) = \{1,2,3,4,5\} \quad\quad\quad \text{state values}\\
dom(Y) = \mathbb{R} \quad\quad\quad \text{cumulative reward}\\
dom(U) = \{1\} \quad\quad\quad \text{initial state}\\
dom(\pi) = \{(1\rightarrow 1, 2\rightarrow 2, 3\rightarrow 3, 4\rightarrow 4, 5\rightarrow 5,...),...\} && \text{policy space}\\
f_{Z_1}: Z_1 = U_1, f_{X_1}: X_1 = \pi(Z_1),f_{Z_2} : Z_2 = X_1,f_{X_2} : X_2 = \pi(Z_2),\\
f_{Z_3} : Z_3 p= X_2,f_{X_3} : X_3 = \pi(Z_3),f_{Z_4} : Z_4 = X_3,\\
f_{Y} : Y = C_{Z_1,X_1} + C_{Z_2,X_2} + C_{Z_3,X_3} + D_{Z_4}
$$

which has an associated causal diagram $$\mathcal{G}$$:

![causal diagram for the example use case](/assets/ctf_example_2.png "causal diagram for the example use case")


Assume that we intervene and set the policy to:

$$
  \pi = \big( (1,1) \rightarrow 1, (2,1) \rightarrow 1, (3,1) \rightarrow 1, (4,1) \rightarrow 1,\\
  (1,2) \rightarrow 2, (2,2) \rightarrow 1, (3,2) \rightarrow 2, (4,2) \rightarrow 2,\\
  (1,3) \rightarrow 3, (2,3) \rightarrow 3, (3,3) \rightarrow 3, (4,3) \rightarrow 3,\\
  (1,4) \rightarrow 4, (2,4) \rightarrow 4, (3,4) \rightarrow 4, (4,4) \rightarrow 4,\\
  (1,5) \rightarrow 5, (2,5) \rightarrow 5, (3,5) \rightarrow 5, (4,5) \rightarrow 4\big)
$$
Then, the SCM simplifies to:

$$
V = X \cup Z \cup Y \quad\quad\quad \text{endogeneous variables}\\
X = \{X_1,X_2,X_3\} \quad\quad\quad \text{control variables}    \\
Z = \{Z_1,Z_2,Z_3, Z_4\} \quad\quad\quad \text{covariates (states)}      \\
Y = \{Y\} \quad\quad\quad \text{outcome variable}\\
U = \{U\} \quad\quad\quad \text{exogeneous variables}\\
F = \{f_{X_1},f_{X_2},f_{X_3},f_{Z_1},f_{Z_2},f_{Z_3},f_{Z_4},f_{Y}\} \quad\quad\quad \text{system functions}\\
dom(X_i) = \{1,2,3,4,5\} \quad\quad\quad \text{control values}\\
dom(Z_i) = \{1,2,3,4,5\} \quad\quad\quad \text{state values}\\
dom(Y) = \mathbb{R} \quad\quad\quad \text{cumulative reward}\\
dom(U) = \{1\} \quad\quad\quad \text{initial state}\\
f_{Z_1}: Z_1 = U_1, f_{X_1}: X_1 = Z_1,f_{Z_2} : Z_2 = X_1,f_{X_2} : X_2 = Z_2,\\
f_{Z_3} : Z_3 = X_2,f_{X_3} : X_3 = Z_3,f_{Z_4} : Z_4 = X_3,\\
f_{Y} : Y = C_{Z_1,X_1} + C_{Z_2,X_2} + C_{Z_3,X_3} + D_{Z_4}
$$

which has an associated causal diagram $$\mathcal{G}$$:

![causal diagram for the example use case](/assets/ctf_example_3.png "causal diagram for the example use case")


## Conclusions

From the above exercise in modeling a decision problem both as an MDP and as an SCM, we note that any MDP can be converted into an SCM but not vice-versa. Hence, an MDP is encoding implicit causal structure that is explicit in the SCM. In this sense, the SCM is a more general model than an MDP. Thus, the SCM should be preferred over the MDP when additional causal information is available that is not already encoded in the MDP. If no additional causal information than what is already implicit in the MDP is available, the gain of using an SCM is limited.

## References

Pearl, Judea (2009). Causality. Models, Reasoning, and Inference. 2nd ed. Cambridge, UK: Cam-
bridge University Press.

Puterman, Martin L. (1994). Markov Decision Processes: Discrete Stochastic Dynamic Program-
ming. 1st. USA: John Wiley and Sons
