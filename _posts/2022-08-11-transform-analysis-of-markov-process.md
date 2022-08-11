---
title: Transform Analysis of Markov Processes.
updated: 2022-08-11 17:30
---

In this post I will explain how to use transforms to analyze finite discrete-time Markov processes and their limiting behavior, an approach to Markov process analysis that is not very well known. Before I explain the transform analysis I briefly review the basics of discrete-time Markov processes; this material can be skipped if you are already familiar with the basics.

## A Review of discrete-time Markov Processes and their Stationary Distributions

A finite discrete-time Markov process $$(X_t)_{t\geq 1}$$ is a stochastic process defined by $$\langle \mathcal{S}, \mathcal{P} \rangle$$ where $$\mathcal{S} = {1,2,\hdots,|\mathcal{S}|} $$

### Classification of States

**Absorbing state**. A state $$i \in \mathcal{S}$$ of a discrete-time and finite Markov process $$\langle \mathcal{S}, \mathcal{P} \rangle$$ is *absorbing if it is impossible to leave it. That is, $$\mathcal{P}_{ii} = 1$$. A Markov process is absorbing if it has at least one absorbing state, and if from every state it is possible to go to an absorbing state (not necessarily in one step).

**Periodic state**. A state $$i \in \mathcal{S}$$ of a discrete-time and finite Markov process $$\langle \mathcal{S}, \mathcal{P} \rangle$$ is *periodic* with (with period $$k > 1$$) if $$k$$ is the smallest number such that all the paths leading from state $$i$$ back to state i have a period that is a multiple of $$k$$. Thus it is possible to predict the number of steps until the Markov process will revisit a periodic state. In other words, for a periodic state, all cycles to that state have a greatest common divisor (GCD) that is greater than $$1$$.

**Accessible state.** A state $$j \in \mathcal{S}$$ of a discrete-time and finite Markov process $$\langle \mathcal{S}, \mathcal{P} \rangle$$ is *accessible* from another state $$i \in \mathbb{S}$$, and we write $$i \rightarrow j$$, if there exists a *finite* integer $$n \geq 0$$ such that:
$$\mathcal{P}^{n}_{i,j} = \mathbb{P}\left[X_n = j | X_0 = i\right] > 0$$
  In other words, it is possible to travel from $$i$$ to $$j$$ with non-zero probability in a certain (random) number of steps.

**Recurrent state.** A state $$i \in \mathcal{S}$$ of a discrete-time and finite Markov process $$\langle \mathcal{S}, \mathcal{P} \rangle$$ is *recurrent* if, starting from state $$i$$, the process will return to state $$i$$ within a finite (random) time $$T^r_i$$, with probability $$1$$, i.e.
$$\mathcal{P}_{i,i} = \mathbb{P}\left[T_i^r < \infty | X_0 = i\right] = \mathbb{P}[X_n = i \text{ for some } n \geq 1 | X_0 = i] = 1$$

**Transient state.** A state $$i \in \mathcal{S}$$ of a discrete-time and finite Markov process $$\langle \mathcal{S}, \mathcal{P} \rangle$$ is *transient* when it is not recurrent, i.e.
$$\mathcal{P}_{i,i} = \mathbb{P}\left[T_i^r < \infty | X_0 = i\right] = \mathbb{P}[X_n = i \text{ for some }n \geq 1 | X_0 = i] < 1$$

When analyzing discrete-time Markov processes in the limit, you can be assured that as $$t\rightarrow \infty$$, the Markov process will eventually leave a transient state and never revisit again. Similarly, as $$t\rightarrow \infty$$ you can be assured that the Markov process will eventually revisit a recurrent state (assuming that it has visited that state before). Sometimes, a Markov process contains multiple ``classes'' (sub-graphs in the graphical model) of recurrent states as well as some transient states that connects the two recurrent classes. In such cases, depending on in which recurrent class the Markov process starts in, or to which recurrent class it transitions in case it starts in a transient state, the limiting behavior of the Markov process will be different.

### Classification of Markov Processes

**Periodic Markov process.** A discrete-time and finite Markov process $\langle \mathcal{S}, \mathcal{P} \rangle$ is *periodic* if it has at least one periodic state.

**Ergodic Markov process.** A discrete-time and finite Markov process $\langle \mathcal{S}, \mathcal{P} \rangle$ is *ergodic* if it is possible to go to from every state to every state (not necessarily in one move). A synonym for ergodic that is sometimes used in the literature is *irreducible*. A square matrix $\bm{A}$ is irreducible if and only if its directed graph is strongly connected. In other words, $\bm{A}$ is irreducible if and only if for each pair of indices $(i,j)$ there is a sequence of entries in $\bm{A}$ such that $a_{i,k}, a_{k_1,k_2}, \hdots, a_{k_t,j} \neq 0$.

### Limiting and Stationary Distributions

**Limiting distribution of a Markov process.** A discrete-time and finite Markov process $\langle \mathcal{S}, \mathcal{P} \rangle$ is said to admit a *limiting probability distribution* if the following conditions are satisfied:
- the limits $\lim_{n \rightarrow \infty} \mathbb{P}[X_n = j | X_0 = i]$ exists for all $i,j \in \mathcal{S}$ and
- they form a *probability distribution* on $\mathcal{S}$, i.e.
  $$\sum_{j \in \mathbb{S}} \lim_{n\rightarrow \infty}\mathbb{P}[X_n = j | X_0 = i] = 1$$
  for all $i \in \mathcal{S}$.

**Stationary Distribution of a Markov process.** Suppose a distribution $\pi^{*}$ over states $\mathcal{S}$ is such that, if our discrete-time and finite Markov process $\langle \mathcal{S}, \mathcal{P} \rangle$ starts out with initial distribution $\pi_{0} = \pi^{*}$, then we also have $\pi_1 = \pi^{*}$. If that is the case, then $\pi^{*}$ is called a stationary distribution for the Markov process. A Markov process that starts out in a stationary distribution $\pi^{*}$ stays in the distribution $\pi^{*}$ forever. Formally, a stationary distribution $\pi^{*}$ is one such that:
$$\pi^{*} = \mathcal{P}^T\pi^{*} \iff (\mathcal{P}^T - \mathcal{I}_n)\pi^{*} = 0$$
In other words, we say that a distribution $\pi$ is stationary when the distribution does not change when taking a step in the Markov process, $\pi^{*} = \mathcal{P}^T\pi^{*}$.

From the above definition, it can be seen that the stationary distribution $\pi^*$ is not any distribution, it is the eigenvector of the transition matrix $\mathcal{P}^T$ that is associated to the unitary eigenvalue\textemdash the principal eigenvector. We also note that the stationarity and limiting properties of distributions are quite different concepts. If the Markov chain is started in the stationary distribution then it will remain in that distribution at any subsequent time step (which is stronger than saying that the chain will reach that distribution after an *infinite* number of time steps). On the other hand, in order to reach the limiting distribution the chain can be started from any given initial distribution or even from any fixed given state, and it will converge to the limiting distribution if it exists. Nevertheless, the limiting and stationary distribution may coincide in some situations as we will see shortly.

**Review of eigenvectors and eigenvalues.** For a matrix $\bm{A} \in C^{n \times n}$, the scalars $\lambda$ and the vectors $x_{n\times 1} \neq 0$ satisfying $\bm{A}x = \lambda x$ are the respective eigenvalues and eigenvectors for $\bm{A}$. A row vector $y^{T}$ is a left-hand eigenvector if $y^T\bm{A} = \lambda y^T$. The set $\sigma(\bm{A})$ of a distinct eigenvalues is called the spectrum of $\bm{A}$:
$$|\lambda_1| > |\lambda_2| \geq |\lambda_3| \geq \hdots \geq |\lambda_n|$$
where the dominant eigenvalue is $\lambda_1 = 1$ (unitary eigenvalue) if $\bm{A}$ is row-stochastic, and the spectral radius of $A$ is the non-negative number:
$$\rho(\bm{A}) = \max_{\lambda \in \sigma(\bm{A})}|\lambda |$$

### Fundamental Theorem of Discrete-Time Finite Markov Processes

For an ergodic, aperiodic, discrete-time and finite Markov process, there is a unique probability vector $\pi$ satisfying $\pi^T\mathcal{P} = \pi^T$. Moreover, $\pi$ is the principal eigenvector of $\mathcal{P}$.

## An Example Discrete-Time and Finite Markov Process

For our subsequent discussion we will consider the following simple example of a Markov process $\langle \mathcal{S}, \mathcal{P} \rangle$.

**State space $\mathcal{S}$.** The state space is $\mathcal{S} = \{1,2\}$.

**Transition probability matrix $\mathcal{P}$.** The transition probability matrix is:
$$
\mathcal{P} =
\begin{bmatrix}
0.8 & 0.2\\
0.3 & 0.7
\end{bmatrix}
$$

**State transition diagram**

![State transition diagram](/assets/transition_diagram_example_markov_process.png "State transition diagram")

**Existence of stationary distribution.** It is trivial to show that the Markov process defined above has a unique stationary distribution by applying the fundamental theorem.

To show that the process is ergodic (irreducible) it is sufficient to show that each state $X_i \in \mathcal{S}$ is accessible from every other state $X_j \in \mathcal{S}$. In other words $i \leftrightarrow j \forall i,j \in \mathcal{S}$. Hence we have to show that $\mathcal{P}_{i,j}^{n} = \mathbb{P}[X_n = j| X_0 = i] > 0$ for some $n$ for each $i,j\in \mathcal{S}$. This follows directly from the definition of $\mathcal{P}$ where $\mathcal{P}_{1,1} = 0.8$, $\mathcal{P}_{1,2} = 0.2$, $\mathcal{P}_{2,1} = 0.3$, $\mathcal{P}_{2,2} = 0.7$.

To show that the process is aperiodic we must show that all states $X_i \in \mathcal{S}$ are aperiodic. By definition, a state is periodic if the length of \underline{all} cycles to that state have a greatest common divisor (GCD) that is \underline{larger} than $1$. Thus, to show that a state $i$ is *aperiodic* it is sufficient to show that the GCD of the length of two cycles to state $i$ are co-prime, i.e. have a GCD of $1$. Since both state $1$ and $2$ have cycles of length $1$ they are both aperiodic and hence the process is aperiodic.

### Finding the Stationary Distribution through Spectral Analysis

The equation for the stationary distribution of the Markov chain is $\pi^T\mathcal{P} = \pi^T$ where $\pi^{T}$ is a row vector, this is equivalent to the eigenvalue problem:
$$(\pi^T\mathcal{P})^T = (\pi^T)^T$$
$$\mathcal{P}^T\pi = \pi$$
Hence, $\pi$ is an eigenvector of $\mathcal{P}^T$ with eigenvalue $\lambda_1 = 1$. The stationary distribution for the Markov process derived below by solving the mentioned eigenvalue problem.
$$\mathcal{P}^T\pi = \pi$$
$$\implies \mathcal{P}^T\pi -\pi = 0$$
$$\implies (\mathcal{P}^T - I)\pi = 0$$
For a usual eigenvalue problem we would first have to compute the eigenvalue $\lambda$ but in this case we know, from the fundamental theorem that the eigenvalue is $1$ and hence we can proceed to calculate the eigenvector by solving the equation:
$$(\mathcal{P}^T - I)\pi = 0$$
where
$$\mathcal{P}^T - I =
\begin{bmatrix}
0.8 & 0.3\\
0.2 & 0.7
\end{bmatrix}
-
\begin{bmatrix}
1 & 0 \\
0 & 1
\end{bmatrix}
            =
\begin{bmatrix}
-0.2 & 0.3 \\
0.2 & -0.3
\end{bmatrix}
$$
Hence, the principal eigenvector $\pi$ is the solution to the equation:
$$
\begin{bmatrix}
-0.2 & 0.3 \\
0.2 & -0.3
\end{bmatrix}
\pi =
\begin{bmatrix}
  0\\
  0
\end{bmatrix}
$$
Adding the constraint that $\sum_i \pi_i = 1$, we get the linear system.
$$
\begin{bmatrix}
-0.2 & 0.3 & 0\\
0.2 & -0.3 & 0\\
1 & 1 & 1
\end{bmatrix}
$$
Performing Gauss-jordian reduction using elentary row operations we get the eigenvector:
$$
\pi =
\begin{bmatrix}
0.6\\
0.4
\end{bmatrix}
$$
which also, by the fundamental theorem, is the stationary distribution of the Markov Process.

### Finding the Stationary Distribution through Power Iteration

For large matrices it may not be convenient to calculate the principal eigenvector algebraically. In this case, the *power iteration* method can be used, which computes the eigenvector iteratively through the following update:
$$
\pi = \frac{\mathcal{P}^T\pi}{\pi}
$$

In our case, power iteration converges in less than around $20$ iterations:

![Power iteration convergence](/assets/pi_convergence_1.png "Convergence of power iteration")

### Finding the Stationary Distribution through Transform Analysis

An alternative approach to understanding the asymptotic behavior of a Markov process is through transform analysis. In particular, we will see that we can obtain a closed form expression for the $t$-step transition matrix as $t \rightarrow \infty$ by using the *z-transform*, from which the stationary distribution of the Markov process can be extracted.

Recall that the $t$-step transition probabilities are given by the Chapman-Kolmogorov equations:
$$\mathbb{P}\left[X_{t} = j | X_0 = i \right] = \mathbb{P}\left[X_{n+t} = j | X_n = i \right] = (\mathcal{P}^{t})_{ij} \quad\quad \text{for any n}$$

Let $\bm{\phi}(t) = \mathcal{P}^{t}$ and $\bm{\phi} = \lim_{t\rightarrow \infty} \mathcal{P}^{t}$. Then it follows that, if the Markov process has a stationary distribution, then any row of $\bm{\phi}$ will be the stationary distribution. Now let us define the z-transform for a discrete matrix function $M(t)$ to be:
$$M_Z(z) = \sum_{t=0}^{\infty}M(t)z^t$$
with the inverse z-transform being:
$$M(t) = \frac{1}{2\pi j}\oint\limits_{C}M_Z(z)z^{1-n}\,\mathrm{d}z$$
where we can utilize the following table of elementary functions and their z-transforms to perform the inverse transform without solving the contour integral:

| Discrete function                            | z-transform                  |
|----------------------------------------------|------------------------------|
| $f(n)$                                       | $\sum_{n=0}^{\infty}f(n)z^n$ |
| \delta(n)$ (unit impulse)                    | $1$                          |
| u(n)=1$ (unit step)                          | $af_Z(z)$                    |
| $\sum_{m=0}^{n}f_1(m)f_2(n-m)$ (convolution) | $f_{1,z}(z)f_{2,z}(z)$       |
| $a^n$                                        | $\frac{1}{1-az}$             |

Now we note that $\bm{\phi}(t) = \mathcal{P}^{t}$ is a matrix function. If we apply the z-transform to $\bm{\phi}(t)$ we obtain:
$$\bm{\phi}_Z(z) = \sum_{t=0}^{\infty}\bm{\phi}(t)z^t = \sum_{t=0}^{\infty}\mathcal{P}^tz^t = I + \mathcal{P}z + \mathcal{P}^2z^2 + \hdots = [I - \mathcal{P}z]^{-1}$$

Now let's apply the z-transform to $\bm{\phi}(t) = \mathcal{P}^t$ where
$$
\mathcal{P}=
\begin{bmatrix}
0.8 & 0.2\\
0.3 & 0.7
\end{bmatrix}
$$
, we obtain:
$$
\bm{\phi}_Z(z) = [I - \mathcal{P}z]^{-1} =
\left(
\begin{bmatrix}
-0.2z & 0.2z\\
0.3z & -0.3z
\end{bmatrix}
\right)^{-1}
=
\begin{bmatrix}
\frac{1-0.7z}{1-1.5z + 0.5z^2} & \frac{0.2z}{1-1.5z+0.5z^2}\\
\frac{0.3z}{1-1.5z + 0.5z^2} & \frac{1-0.8z}{1-1.5z+0.5z^2}
\end{bmatrix}
$$
$$=
\frac{1}{1-1.5z + 0.5z^2}
\begin{bmatrix}
1-0.7z & 0.2z\\
0.3z & 1-0.8z
\end{bmatrix}
=
\frac{1}{(1-z)(1-0.5z)}
\begin{bmatrix}
1-0.7z & 0.2z\\
0.3z & 1-0.8z
\end{bmatrix}
$$
Next we want to take the inverse z-transform. To avoid having to compute a contour integral we can apply partial fraction decomposition and then utilize the pre-computed transforms in Table \ref{tab:z_transforms}.

We obtain:
$$
\frac{1}{(1-z)(1-0.5z)} = \frac{A}{1-z} + \frac{B}{1-0.5z}
$$
$$
\implies A = \frac{1}{1-0.5z} - B\frac{1-z}{1-0.5z}
$$
$$
\implies B = \frac{1}{(1-z)} - A\frac{1-0.5z}{1-z}
$$
Now if we evaluate the first equation at $z=1$, we get:
$$
\implies A = 2
$$
and if we evalute the second equation at $z=2$, we get:
$$
\implies B = -1
$$
Hence:
$$
\frac{1}{(1-z)(1-0.5z)} = \frac{2}{1-z} + \frac{-1}{1-0.5z}
$$
From which it follows that:
$$
\frac{1-0.7z}{(1-z)(1-0.5z)} = \frac{0.6}{1-z} + \frac{0.4}{1-0.5z}
$$
$$
\frac{0.2z}{(1-z)(1-0.5z)} = \frac{0.4}{1-z} - \frac{0.4}{1-0.5z}
$$
$$
\frac{0.3z}{(1-z)(1-0.5z)} = \frac{0.6}{1-z} + \frac{0.6}{1-0.5z}
$$
$$
\frac{1-0.8z}{(1-z)(1-0.5z)} = \frac{0.4}{1-z} - \frac{0.6}{1-0.5z}
$$
Thus:
$$
\bm{\phi}_Z(z) =
\frac{1}{(1-z)(1-0.5z)}
\begin{bmatrix}
1-0.7z & 0.2z\\
0.3z & 1-0.8z
\end{bmatrix}
=
\frac{1}{1-z}
\begin{bmatrix}
0.6 & 0.4\\
0.6 & 0.4
\end{bmatrix}
+
\frac{1}{1-0.5z}
\begin{bmatrix}
0.4 & -0.4\\
0.6 & -0.6
\end{bmatrix}
$$
Now from table \ref{tab:z_transforms} we know that the inverse z-transform of $\frac{1}{1-z}$ is $1$ and that the inverse z-transform of $\frac{1}{1-0.5z}$ is $0.5^t$. As a consequence, we obtain that the inverse z-transform of $\bm{\phi}_Z(z)$ is:
$$
\mathcal{Z}^{-1}\left[\bm{\phi}_Z(z)\right] =
\begin{bmatrix}
0.6 & 0.4\\
0.6 & 0.4
\end{bmatrix}
+
0.5^t
\begin{bmatrix}
0.4 & -0.4\\
0.6 & -0.6
\end{bmatrix}
= \bm{\phi}(t)
$$
Now why is this useful? We already knew $\bm{\phi}(t) = \mathcal{P}^t$ to begin with. The useful property here is that we obtained a closed form expression for the $t$th power of $\mathcal{P}$. Recall that $\bm{\phi} = \lim_{t\rightarrow \infty}\bm{\phi}(t)$ and that the rows of $\bm{\phi}$ contain the stationary distribution of the Markov process with transition matrix $\mathcal{P}$ if it exists. Using the closed form expression of $\bm{\phi}(t)$ it becomes trivial to compute the limit $\bm{\phi} = \lim_{t\rightarrow \infty}\bm{\phi}(t)$:
$$
\bm{\phi} = \lim_{t\rightarrow \infty}\bm{\phi}(t) = \lim_{t\rightarrow \infty}
\begin{bmatrix}
0.6 & 0.4\\
0.6 & 0.4
\end{bmatrix}
+
0.5^t
\begin{bmatrix}
0.4 & -0.4\\
0.6 & -0.6
\end{bmatrix}
=
\begin{bmatrix}
0.6 & 0.4\\
0.6 & 0.4
\end{bmatrix}
$$
Since the rows of $\bm{\phi}$ are all equal, we conclude that the stationary distribution of the Markov process is $\pi^T = [0.6, 0.4]$.

In general the inverse z-transform of the $t$-step transition matrix will always be of the form $\bm{\phi} = \bm{\phi} + T(t)$ for $t=0,1,\hdots$, where the first term is the limiting $t$-step transition matrix and the second term, $T(t)$ are transient terms that disappears when $t\rightarrow \infty$.

In summary, we have seen that we can find a closed form expression of matrix powers using z-transform, which allows us to compute the limiting $t$-step transition matrix and thus obtain the stationary distribution of any Markov process if it exists.
