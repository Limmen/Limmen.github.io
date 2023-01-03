---
title: The Need for Reinforcement Learning Research in Cyber Security
updated: 2023-01-03 16:52
---

### Introduction and Statement of Need
Despite the many societal benefits of digitalization, it also makes our society more vulnerable. Recent studies estimate that over 40% of organizations globally have suffered from cyber attacks in the last two years, and it is expected that an even larger proportion will be affected in the coming year [2]. Private organizations are not the only ones affected by this trend. Most of the public welfare infrastructure, e.g. healthcare and transport infrastructure, also relies on IT systems and is therefore vulnerable to cyber attacks. This vulnerability was demonstrated in 2015 when Ukraine’s national power grid was hacked by Russia, leaving roughly 230, 000 consumers without power for several hours, including hospitals and public transport systems [1].

As the ubiquity and evolving nature of cyber attacks is of growing concern to society, automation of security processes and functions has been recognized as an important part of the response to this threat. In fact, since the early 1990s, there has been a broad research interest in automating security functions in IT systems, especially in the areas of attack detection, attack prevention, and attack response. While the task of automatically detecting attacks has been largely automated using expert systems and statistical anomaly detection methods, prevention and response to attacks usually remains a manual process performed by system administrators. This manual approach is, however, unsustainable. Given the rapid increase in both the number of cyber attacks and their complexity, management of IT security without a high level of automation becomes impossible.

### Our Research Objective

To cope with the increasing number of cyber attacks, we aim to develop methods for automating attack responses. Consider the following use case. The operator of an IT-infrastructure (see the figure below), which we call the defender, takes measures to protect it against a possible attacker while, at the same time, providing services to a client population. The infrastructure includes a public gateway through which the clients access the service and which is also open to a possible attacker. The attacker seeks to intrude on the infrastructure and compromise a set of its components. Conversely, the defender aims at preventing intrusions from the attacker and maintaining service to its clients. The goal of this project is to develop methods to automatically compute effective attack response strategies for the use case described above. Specifically, we aim to: (1) construct a formal model of the use case; (2) analyze the model theoretically; (3), compute optimal response strategies based on the model; and (4) implement the computed strategies in an automated attack-response system.

![The IT infrastructure and the actors in the use case.](/assets/method_use_case.png "Figure 1: The IT infrastructure and the actors in the use case")

### Our Approach

Our approach to achieving the objective stated above, i.e automating the response to cyber attacks, is based on three well established formal frameworks: game theory, control theory, and reinforcement learning. We plan to use game theory to model cyber attacks and response actions on an IT infrastructure as a game between an attacker and a defender, and we intend to use control theory and reinforcement learning to compute optimal game strategies for the defender. Following this approach, we plan to build two systems (see the figure below). First, we plan to develop an emulation system where key functional components of the target IT infrastructure are replicated. This system closely approximates the functionality of the target infrastructure and is used to run attack scenarios and defender responses. These runs produce system measurements and logs, from which we estimate distributions of infrastructure metrics. We then plan to use the estimated distributions to instantiate a simulation of our game model. Second, we plan to build a simulation system where the game is simulated and where automated response strategies are incrementally learned through reinforcement learning and control theory. Lastly, to evaluate the learned strategies, we will extract them from the simulation system and evaluate them in the emulation system. The approach described above has three key properties. Firstly, the emulation system allows secure and realistic evaluation of response strategies without degrading functionality of the target infrastructure. Second, the combination of control theory, reinforcement learning, and simulation enables efficient learning of response strategies. Finally, by developing the approach around formal frameworks, such as game theory and control theory, we ensure a high level of mathematical rigor and theoretical insight into the automated response strategies.

![Our approach for finding automated intrusion response strategies](/assets/method_approach.png "Figure 2: Our approach for finding automated intrusion response strategies")

### Potential Impact

As vital societal functions are increasingly reliant on digital technology, e.g. power systems, public transport systems, and healthcare systems, IT security becomes a cornerstone of the digital society. Unfortunately, the ubiquity of cyber attacks has never been more apparent and the far-reaching impacts they have on society demonstrate the critical need for automated security solutions. Based on this need, we aim to automate the response to cyber attacks through reinforcement learning and automatic control. If the project is successful, it will allow responding to cyber attacks in computer time-scales, i.e milliseconds or seconds, rather than human time-scale, i.e. minutes or hours.

### References

1] Defense Use Case. Analysis of the cyber attack on the ukrainian power grid. Electricity Information Sharing and Analysis Center (E-ISAC), 388:1–29, 2016.

[2] World Economic Forum. Global cybersecurity outlook, 2022.

[3] Kim Hammar and Rolf Stadler. Intrusion prevention through optimal stopping. IEEE Transactions on Network and Service Management, 19(3):2333–2348, 2022.
