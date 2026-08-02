# A Generalized Algorithm for Multi-Objective Reinforcement Learning and Policy Adaptation (Envelope MOQ-learning / MORL)

Source: https://github.com/RunzheYang/MORL
Date saved: 2026-08-02

Venue: NeurIPS 2019
Authors: Runzhe Yang, Xingyuan Sun, Karthik Narasimhan

Paper: https://arxiv.org/abs/1908.08342
Code: https://github.com/RunzheYang/MORL

## Abstract

We introduce a new algorithm for multi-objective reinforcement learning (MORL) with linear preferences, with the goal of enabling few-shot adaptation to new tasks. In MORL, the aim is to learn policies over multiple competing objectives whose relative importance (preferences) is unknown to the agent. While this alleviates dependence on scalar reward design, the expected return of a policy can change significantly with varying preferences, making it challenging to learn a single model to produce optimal policies under different preference conditions. We propose a generalized version of the Bellman equation to learn a single parametric representation for optimal policies over the space of all possible preferences. After this initial learning phase, our agent can execute the optimal policy under any given preference, or automatically infer an underlying preference with very few samples. Experiments across four different domains demonstrate the effectiveness of our approach.

## Key ideas

- Envelope MOQ-learning: generalized Bellman equation over the space of all possible (linear) preferences.
- Single parametric representation learns optimal policies across the full preference space.
- Few-shot preference inference (execute optimal policy under any given preference; infer preference with few samples).
- Domains: Deep Sea Treasure (DST), Fruit Tree Navigation (FTN), Task-Oriented Dialog (PyDial), SuperMario (multimario).

## Citation

```
@incollection{yang2019morl,
  title = {A Generalized Algorithm for Multi-Objective Reinforcement Learning and Policy Adaptation},
  author = {Yang, Runzhe and Sun, Xingyuan and Narasimhan, Karthik},
  booktitle = {Advances in Neural Information Processing Systems 32},
  pages = {14610--14621},
  year = {2019},
  publisher = {Curran Associates, Inc.},
}
```

## Relevance to thesis (Vision-guided MORL with UR5)

- Envelope MOQ-learning (MORL) is a canonical multi-policy MORL algorithm — must-cite baseline available in MORL-Baselines.
- Single-model-over-all-preferences design matches the "tunable preference" requirement for personalized UR5 control.
- Few-shot preference adaptation concept is relevant for dynamic environments where user preferences change.
