# MORL-Baselines: A collection of multi-objective reinforcement learning algorithms

Source: https://lucasalegre.github.io/morl-baselines/
Date saved: 2026-08-02

Version: 1.1.0
Code: https://github.com/LucasAlegre/MORL-baselines

## Overview

MORL-Baselines is a library of Multi-Objective Reinforcement Learning (MORL) algorithms, providing reliable MORL algorithm implementations in PyTorch. It strictly follows the MO-Gymnasium API (differs from standard Gymnasium API only in that the environment returns a numpy array as the reward).

For details on multi-objective MDPs (MOMDPs) and MORL definitions: "A practical guide to multi-objective reinforcement learning and planning" (Springer). Overview of MORL techniques: "Multi-Objective Reinforcement Learning Based on Decomposition: A Taxonomy and Framework" (JAIR).

## Features

- Single and multi-policy algorithms under both SER and ESR criteria.
- All algorithms follow the MO-Gymnasium API.
- Performances automatically reported in Weights and Biases dashboards.
- Linting/formatting enforced by pre-commit hooks.
- Well documented and automatically tested.
- Utility functions: pareto pruning, experience buffers, etc.
- Reproducible performance reporting.
- Hyperparameter optimization available.

## Multi-policy algorithms

- GPI-Prioritized Dyna
- GPI-Linear Support (Jax)
- Envelope Q-Learning
- Concave-Augmented Pareto Q-Learning (CAPQL)
- PGMORL
- MORL/D
- Pareto Conditioned Networks
- Lorenz Conditioned Networks
- Pareto Q-Learning
- MPMOQ Learning

## Single-policy algorithms

- MOQ-Learning
- EUPG

## Benchmarks

- Participates in Open RL Benchmark (https://github.com/openrlbenchmark/openrlbenchmark).
- Results: https://wandb.ai/openrlbenchmark/MORL-Baselines
- Runs experiments on MO-Gymnasium environments.

## Citation

```
@inproceedings{felten_toolkit_2023,
	author = {Felten, Florian and Alegre, Lucas N. and Now{e}, Ann and Bazzan, Ana L. C. and Talbi, El Ghazali and Danoy, Gr{e}goire and Silva, Bruno Castro da},
	title = {A Toolkit for Reliable Benchmarking and Research in Multi-Objective Reinforcement Learning},
	booktitle = {Proceedings of the 37th Conference on Neural Information Processing Systems ({NeurIPS} 2023)},
	year = {2023}
}
```

## Relevance to thesis (Vision-guided MORL with UR5)

- Primary library of MORL baselines; use to benchmark your algorithm against established methods.
- Algorithms of particular interest for continuous control: PGMORL, MORL/D, Envelope Q-Learning, CAPQL.
- Preferable to implementing baselines from scratch.
