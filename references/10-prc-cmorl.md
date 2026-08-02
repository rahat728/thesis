# PRC-CMORL: Personalized Robotic Control via Constrained Multi-Objective Reinforcement Learning

Source: https://github.com/TMIS-Turbo/PRC-CMORL
Date saved: 2026-08-02

Venue: Neurocomputing, 2023
Authors: Xiangkun He, Zhongxu Hu, Haohan Yang, Chen Lv

Links:
- Code: https://github.com/TMIS-Turbo/PRC-CMORL
- Paper (ResearchGate): https://www.researchgate.net/publication/375254025_Personalized_robotic_control_via_constrained_multi-objective_reinforcement_learning
- Paper (ScienceDirect): https://www.sciencedirect.com/science/article/pii/S0925231223011098

## Abstract

Reinforcement learning is capable of providing state-of-art performance in end-to-end robotic control tasks. Nevertheless, many real-world control tasks necessitate the balancing of multiple conflicting objectives while simultaneously ensuring that the learned policies adhere to constraints. Additionally, individual users may typically prefer to explore the personalized and diversified robotic control modes via specific preferences. Therefore, this paper presents a novel constrained multi-objective reinforcement learning algorithm for personalized end-to-end robotic control with continuous actions, allowing a trained single model to approximate the Pareto optimal policies for any user-specified preferences. The proposed approach is formulated as a constrained multi-objective Markov decision process, incorporating a nonlinear constraint design to facilitate the agent in learning optimal policies that align with specified user preferences across the entire preference space. Meanwhile, a comprehensive index based on hypervolume and entropy is presented to measure the convergence, diversity and evenness of the learned control policies. The proposed scheme is evaluated on nine multi-objective end-to-end robotic control tasks with continuous action space.

## Key ideas

- Constrained multi-objective Markov decision process (CMOMDP) formulation.
- Nonlinear constraint design for user-preference alignment across the entire preference space.
- Single trained model approximates Pareto-optimal policies for any user-specified preference.
- Evaluation index based on hypervolume + entropy measuring convergence, diversity, evenness.
- Evaluated on 9 multi-objective end-to-end robotic control tasks with continuous action space (MO-Swimmer, MO-Hopper, MO-Walker2d, MO-HalfCheetah, MO-Ant in v2/v3).

## Implementation

- Python 3.7, PyTorch 1.3.1+, MuJoCo 2.0.
- cmo_ddpg.py (constrained MO DDPG), train_2d.py / train_3d.py for 2/3-objective training.

## Citation

```
@article{HE2023126986,
title = {Personalized robotic control via constrained multi-objective reinforcement learning},
journal = {Neurocomputing},
pages = {126986},
year = {2023},
issn = {0925-2312},
doi = {https://doi.org/10.1016/j.neucom.2023.126986},
url = {https://www.sciencedirect.com/science/article/pii/S0925231223011098},
author = {Xiangkun He and Zhongxu Hu and Haohan Yang and Chen Lv},
keywords = {Reinforcement learning, Multi-objective optimization, Personalized control, Robotic control, End-to-end control},
}
```

## Relevance to thesis (Vision-guided MORL with UR5)

- Constrained MORL with preference conditioning — directly relevant to UR5 control with constraints (e.g., safety, workspace limits).
- Single-model, whole-preference-space Pareto approximation is a useful design target.
- Hypervolume+entropy metrics are directly applicable for evaluating your Pareto fronts.
- Constraint handling is highly relevant for "dynamic environment" safety requirements.
