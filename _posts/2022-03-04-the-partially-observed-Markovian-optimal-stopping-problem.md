---
title: The Partially Observed Markovian Optimal Stopping Problem
updated: 2022-03-04 11:10
---
Optimal stopping is a classical problem domain with a well-developed theory (Wald 1947; Shirayev 2007; Peskir and Shiryaev 2006; Chow, H. Robbins, and Siegmund 1971; Dimitri P. Bertsekas 2005; Ross 1983; Bather 2000; Puterman 1994).. Example use cases for this theory include: asset selling (Dimitri P. Bertsekas 2005), change detection (Tartakovsky et al. 2006), machine replacement (Krishnamurthy 2016), hypothesis testing (Wald 1947), gambling (Chow, H. Robbins, and Siegmund 1971), selling decisions (Toit and Peskir 2009), queue management (Roy et al. 2019), advertisement scheduling (Krishnamurthy, Aprem, and Bhatt 2018), industrial control (Rabi and Johansson 2008), and the secretary problem (Puterman 1994; Kleinberg 2005).

Many variants of the optimal stopping problem have been studied. For example, discrete-time and continuous-time problems, finite horizon and infinite horizon problems, problems with fully observed state spaces and partially observed state spaces, problems with finite and infinite state spaces, Markovian and non-Markovian problems, and single-stop and multi-stop problems. Consequently, different solution methods for these variants have been developed. The most commonly used methods are the *martingale approach* (Peskir and Shiryaev 2006; Chow, H. Robbins, and Siegmund 1971) and the *Markovian approach* (Shirayev 2007; Dimitri P. Bertsekas 2005; Puterman 1994; Ross 1983; Bather 2000).

In our recent work (Hammar and Stadler 2021), we investigate the multiple stopping problem with a *maximum* of $$L$$ stops, a finite time horizon $$T$$, discrete-time progression, bounded rewards, a finite state space, and the Markov property. We use the Markovian solution approach and model the problem as a POMDP, where the system state evolves as a discrete-time Markov process $$(s_{t,l})_{t=1}^{T}$$ that is partially observed and depends on the number of stops remaining $$l \in \{1,\hdots,L\}$$.

At each time-step $$t$$ of the decision process, two actions are available: ``stop'' ($$S$$) and ``continue'' ($$C$$). The *stop* action with $$l$$ stops remaining yields a reward $$\mathcal{R}^{S}_{s_t,s_{t+1},l_t}$$ and if only one of the $$L$$ stops remain, the process terminates. In the case of a *continue* action or a non-final stop action $$a_t$$, the decision process transitions to the next state according to the transition probabilities $$\mathcal{P}^{a_t}_{s_t,s_{t+1},l_t}$$ and yields a reward $$\mathcal{R}^{a_t}_{s_t,s_{t+1},l_t}$$.

The *stopping time* with $$l$$ stops remaining is a random variable $$\tau_l$$ that is dependent on $$s_1,\hdots,s_{\tau_l}$$ and independent of $$s_{\tau_{l}+1},\hdots s_{T}$$ (Peskir and Shiryaev 2006):
$$\tau_l = \inf\{t: t > \tau_{l+1}, a_t=S\}, \quad \quad \quad  l\in 1,..,L,\text{ }\tau_{L+1}=0$$

The objective is to find a stopping policy $$\pi_l^{*}(s_t) \rightarrow \{S,C\}$$ that depends on $$l$$ and maximizes the expected discounted cumulative reward of the stopping times $$\tau_{L},\tau_{L-1},\hdots, \tau_1$$:
$$\pi_l^{*} \in arg\max_{\pi_l} \mathbb{E}_{\pi_l}\Bigg[\sum_{t=1}^{\tau_{L}-1}\gamma^{t-1}\mathcal{R}^{C}_{s_t,s_{t+1},L} + \gamma^{\tau_{L}-1}\mathcal{R}^{S}_{s_{\tau_L},s_{\tau_L+1},L} + ... + \sum_{t=\tau_2+1}^{\tau_{1}-1}\gamma^{t-1}\mathcal{R}^{C}_{s_t,s_{t+1},1} + \gamma^{\tau_{1}-1}\mathcal{R}^{S}_{s_{\tau_1},s_{\tau_1+1},1} \Bigg]$$

Due to the Markov property, any policy that satisfies the above equation also satisfies the Bellman equation given below, which in the partially observed case is:
$$\pi_l^{*}(b) \in arg\max_{\{S,C\}}  \Bigg[\underbrace{\mathbb{E}_l\left[\mathcal{R}^{S}_{b,b^{o}_{S},l} + \gamma V_{l-1}^{*}(b^{o}_{S})\right]}_{\text{stop } (S)}, \underbrace{\mathbb{E}_l\left[\mathcal{R}^{C}_{b,b_C^o,l} + \gamma V_{l}^{*}(b_C^o)\right]}_{\text{continue } (C)}\Bigg] \quad \quad \forall b \in \mathcal{B}$$
where $$\pi_l$$ is the stopping policy with $$l$$ stops remaining, $$\mathbb{E}_l$$ denotes the expectation with $l$ stops remaining, $$b$$ is the belief state, $$V_{l}^{*}$$ is the value function with $$l$$ stops remaining, $$b^o_{S}$$ and $$b^{o}_{C}$$ can be computed using a Bayesian filter, and $$\mathcal{R}_{b,b^o_{a},l}^{a}$$ is the expected reward of action $$a \in \{S,C\}$$ in belief state $$b_t$$ when observing $$o$$ with $$l$$ stops remaining.

### References

- Peskir, Goran and Albert Shiryaev (2006). Optimal stopping and free-boundary problems. Lectures in mathematics (ETH Zürich). Springer. doi: 10.1007/978-3-7643-7390-0. url: https://cds.cern.ch/record/1250862.
- Wald, Abraham (1947). Sequential Analysis. Wiley and Sons, New York.
- Shirayev, Albert N. (2007). Optimal Stopping Rules. Reprint of russian edition from 1969. SpringerVerlag Berlin.
- Chow, Y., H. Robbins, and D. Siegmund (1971). “Great expectations: The theory of optimal stopping”
- Bertsekas, Dimitri P. (2005). Dynamic Programming and Optimal Control. 3rd. Vol. I. Belmont, MA, USA: Athena Scientific.
- Ross, Sheldon M. (1983). Introduction to Stochastic Dynamic Programming: Probability and Mathematical. USA: Academic Press, Inc. isbn: 0125984200.
- Bather, John (2000). Decision Theory: An Introduction to Dynamic Programming and Sequential Decisions. USA: John Wiley and Sons, Inc. isbn: 0471976490.
- Puterman, Martin L. (1994). Markov Decision Processes: Discrete Stochastic Dynamic Programming. 1st. USA: John Wiley and Sons, Inc. isbn: 0471619779.
- Tartakovsky, Alexander G. et al. (2006). “Detection of intrusions in information systems by sequential change-point methods”. In: Statistical Methodology 3.3. issn: 1572-3127. doi: https://doi.org/10.1016/j.stamet.2005.05.003.url: https://www.sciencedirect.com/science/article/pii/S1572312705000493.
- Toit, Jacques du and Goran Peskir (June 2009). “Selling a stock at the ultimate maximum”. In: The Annals of Applied Probability 19.3. issn: 1050-5164. doi: 10.1214/08-aap566. url: http://dx.doi.org/10.1214/08-AAP566.
- Krishnamurthy, Vikram, Anup Aprem, and Sujay Bhatt (2018). “Multiple stopping time POMDPs: Structural results & application in interactive advertising on social media”. In: Automatica 95, pp. 385–398. issn: 0005-1098. doi: https://doi.org/10.1016/j.automatica.2018.06.013. url: https://www.sciencedirect.com/science/article/pii/S0005109818303054
- Rabi, Maben and Karl H. Johansson (2008). “Event-Triggered Strategies for Industrial Control over Wireless Networks”. In: Proceedings of the 4th Annual International Conference on Wireless Internet. WICON ’08. Maui, Hawaii, USA: ICST (Institute for Computer Sciences, SocialInformatics and Telecommunications Engineering). isbn: 9789639799363.
- Kleinberg, Robert (2005). “A Multiple-Choice Secretary Algorithm with Applications to Online Auctions”. In: Proceedings of the Sixteenth Annual ACM-SIAM Symposium on Discrete Algorithms. SODA ’05. Vancouver, British Columbia. isbn: 0898715857
- Kim Hammar and Rolf Stadler (2021). Finding effective security strategies through reinforcement learning and Self-Play. In International Conference on Network and Service Management (CNSM 2020), Izmir, Turkey, 2020.
- Kim Hammar and Rolf Stadler (2021). Intrusion prevention through optimal stopping, 2021. https://arxiv.org/abs/2111.00289
- Kim Hammar and Rolf Stadler (2020). Learning intrusion prevention policies through optimal stopping. In International Conference on Network and Service Management (CNSM 2021), Izmir, Turkey, 2021. https://arxiv.org/pdf/2106.07160.pdf.
- Roy, Arghyadip et al. (2019). “Online Reinforcement Learning of Optimal Threshold Policies for Markov Decision Processes”. In: CoRR. http://arxiv.org/abs/1912.
