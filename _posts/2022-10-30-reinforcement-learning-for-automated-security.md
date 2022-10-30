---
title: Reinforcement Learning for Automated Security
updated: 2022-10-13 11:23
---

As the ubiquity and evolving nature of cyber attacks is of growing concern to society, automation of security processes and functions has been recognized as an important part of the response to this threat. In fact, since the early 2000s researchers have studied automated security through modeling attacks and response actions on an IT infrastructure as a game between an attacker and a defender [1-2]. Assuming the attacker follows a static strategy, the game is typically modeled as a Markov decision process (MDP), and the optimal defender strategy is obtained through dynamic programming [3]. In the more general case where the attacker follows a dynamic strategy, the game can be modeled as a Markov game, and the optimal defender strategy can be obtained through game-theoretic methods like fictitious self-play [1]. While these methods provide insight into optimal defender strategies, they are computationally intractable for most practical problems, which require the use of approximation techniques. Reinforcement learning has emerged as a promising approach to approximate optimal control strategies in non-trivial scenarios, and fundamental breakthroughs demonstrated by systems like AlphaGo [4] and OpenAI Five [5] have inspired us and other researchers to study reinforcement learning with the goal to automate security functions (see surveys [6-7]).

Over the last two years, we have developed a framework for *network intrusion prevention*, in which we model the interaction between an attacker and a defender as a partially observed Markov game [8-9]. Within this framework, reinforcement learning enables the controlled evolution of attack and defense strategies towards a Nash equilibrium through the process of self-play. In particular, we have studied the timing aspects of attacker and defender actions and modeled the player strategies using formulations from optimal stopping theory [8-9].

Our research aims at developing methods that can be evaluated as close as possible to a real system, which excludes simulation as a sufficient evaluation method. Towards this goal, our framework includes three parts: (1) a digital twin that emulates the target system and is used to evaluate defender strategies obtained from self-play; (2) a system identification method that produces a model of the target system based on measurements from the digital twin; and (3) a simulation environment that efficiently executes the self-play process using reinforcement learning techniques to obtain near optimal defender strategies. We found that our approach significantly improves the accuracy of current intrusion prevention systems for the scenarios we investigated [8-9].

While reinforcement learning provides effective approximation techniques for automated security approaches, researchers encounter the challenges of partial observability, high-dimensional state and action spaces, adversarial agents, and non-stationary environments, all of which are under study by the reinforcement learning community [10]. For instance, online rollout techniques have been proposed to adapt strategies to sudden changes in the environment [11]. Such changes are common in IT systems due to failures, reconfigurations, resource scaling etc. We also anticipate that recent results from multi-agent systems [12] will have many applications in the security domain.

A key issue in automated security is the level of abstraction at which the environment is modeled. The more detailed we construct the model the closer it can capture reality. This however comes at the expense of high computational complexity and a lack of generalization, which is why advancements in deep reinforcement learning will be instrumental to develop effective security solutions.

## References

[1] Alpcan, T. and Basar, T. (2010) Network Security: A Decision and Game-Theoretic Approach *Cambridge University Press.*

[2] Levente, B. and Hubaux, J. (2007) Security and Cooperation in Wireless Networks: Thwarting Malicious and Selfish Behavior in the Age of Ubiquitous Computing *Cambridge University Press.*

[3] Miehling, E. et al. (2019) Control-Theoretic Approaches to Cyber-Security *Springer International Publishing.*

[4] Silver, D. et al. (2016) Mastering the Game of Go with Deep Neural Networks and Tree Search *Nature.* https://www.nature.com/articles/nature16961

[5] Berner, C. et al. (2019) Dota 2 with Large Scale Deep Reinforcement Learning *arXiv.* https://arxiv.org/abs/1912.06680

[6] Nguyen, T.T et al.  (2019) Deep Reinforcement Learning for Cyber Security *IEEE Transactions on Neural Networks and Learning Systems.* https://ieeexplore.ieee.org/document/9596578

[7] Huang, Y. et al. (2022) Reinforcement Learning for feedback-enabled cyber resilience *Annual Reviews in Control.* https://www.sciencedirect.com/science/article/pii/S1367578822000013

[8] Hammar, K. and Stadler, R. (2022) Intrusion Prevention through Optimal Stopping *IEEE Transactions on Network and Service Management.* https://ieeexplore.ieee.org/document/9779345.

[9] Hammar, K. and Stadler, R. (2022) Learning Security Strategies through Game Play and Optimal Stopping *Proceedings of the ML4Cyber workshop at ICML 2022.* https://arxiv.org/abs/2205.14694

[10] Dulac-Arnold, G. et al. (2019) Challenges of Real-World Reinforcement Learning *arXiv.* https://arxiv.org/pdf/1904.12901.pdf
