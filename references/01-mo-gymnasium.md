# MO-Gymnasium

Source: https://mo-gymnasium.farama.org/
Date saved: 2026-08-02

## Overview

MO-Gymnasium is a standardized API and a suite of environments for multi-objective reinforcement learning (MORL). It is an open source Python library for developing and comparing multi-objective reinforcement learning algorithms by providing a standard API to communicate between learning algorithms and environments, as well as a standard set of environments compliant with that API. Essentially, the environments follow the standard Gymnasium API, but return vectorized rewards as numpy arrays.

## API

As for Gymnasium, the MO-Gymnasium API models environments as simple Python `env` classes.

```python
import gymnasium as gym
import mo_gymnasium as mo_gym
import numpy as np

# It follows the original Gymnasium API ...
env = mo_gym.make('minecart-v0')

obs, info = env.reset()
# but vector_reward is a numpy array!
next_obs, vector_reward, terminated, truncated, info = env.step(your_agent.act(obs))

# Optionally, you can scalarize the reward function with the LinearReward wrapper
env = mo_gym.wrappers.LinearReward(env, weight=np.array([0.8, 0.2, 0.2]))
```

For details on multi-objective MDP's (MOMDP's) and other MORL definitions, see "A practical guide to multi-objective reinforcement learning and planning" (https://link.springer.com/article/10.1007/s10458-022-09552-y).

## Install

```
pip install mo-gymnasium
```

This does not include dependencies for all families of environments. You can install these dependencies for one family like `pip install "mo-gymnasium[mujoco]"` or use `pip install "mo-gymnasium[all]"` to install all dependencies.

## Environments

### Grid-World
- Deep-Sea-Treasure
- Deep-Sea-Treasure-Concave
- Deep-Sea-Treasure-Mirrored
- Resource-Gathering
- Four-Room
- Fruit-Tree
- Breakable-Bottles
- Fishwood

### Classic Control
- MO-Mountaincar
- MO-Mountaincarcontinuous
- MO-Lunar-Lander
- MO-Lunar-Lander-Continuous

### Miscellaneous
- Water-Reservoir
- Minecart
- Minecart-Deterministic
- Minecart-Rgb
- MO-Highway
- MO-Supermario

### MuJoCo
- MO-Reacher
- MO-Hopper
- MO-Halfcheetah
- MO-Walker2D
- MO-Ant
- MO-Swimmer
- MO-Humanoid

## Citation

```
@inproceedings{felten_toolkit_2023,
	author = {Felten, Florian and Alegre, Lucas N. and Now{e}, Ann and Bazzan, Ana L. C. and Talbi, El Ghazali and Danoy, Gr{e}goire and Silva, Bruno C. {relax da}},
	title = {A Toolkit for Reliable Benchmarking and Research in Multi-Objective Reinforcement Learning},
	booktitle = {Proceedings of the 37th Conference on Neural Information Processing Systems ({NeurIPS} 2023)},
	year = {2023}
}
```

## Relevance to thesis (Vision-guided MORL with UR5)

- Provides the standard benchmark suite + API for MORL experiments.
- Vectorized reward environments (numpy arrays) are the de-facto testbed for MORL algorithms.
- Useful for validating any novel MORL algorithm on standardized tasks before/alongside UR5 experiments.
- GitHub: https://github.com/Farama-Foundation/MO-Gymnasium
