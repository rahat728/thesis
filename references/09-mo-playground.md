# MO-Playground: Massively Parallelized Multi-Objective Reinforcement Learning for Robotics

Source: https://github.com/dynamicmobility/moplayground
Date saved: 2026-08-02

Authors: Neil Janwani, Ellen Novoseller, Vernon Lawhern, Maegan Tucker
Paper: https://arxiv.org/abs/2603.09237v1

## Overview

MO-Playground is a collection of multi-objective environments built in JAX for GPU-Accelerated multi-objective RL.

Note: due to double-blind requirements, moplayground's documentation page and pip-installable package are not yet available.

## Installation

- Ubuntu 22.04, Python 3.12.12, CUDA 13.0 (GPU training; evaluation can happen without GPU).
- conda env create -f environment.yml / mac_environment.yml (evaluation only)
- pip3 install -e .

## Evaluation

- Uses Weights and Biases for experiment tracking.
- Environments: cheetah, hopper, walker, ant, humanoid, bruce.
- Download policies: python3 -m scripts.download_model --env cheetah
- Rollout: python3 -m scripts.rollout_policy config_path

## Training

- Config files in config/ specify model architecture, MORLAX parameters, reward and environment constants.
- python3 -m scripts.train config_path

## Creating your own environment

- Subclass MultiObjectiveBase (see src/moplayground/envs/dmcontrol/cheetah.py). Support for custom (non-mujoco) dynamics coming soon.

## Classic Environments

- ant, cheetah, hopper, humanoid, walker with reward pairs (e.g., Max Energy, Max Run, Max Vx, Max Vy, Max Height).

## BRUCE Robotics Example

Demonstrated on the BRUCE humanoid robot (Westwood Robotics). Seven possible reward functions; combining base_xyz_tracking and base_quat_tracking yields a 6-dimensional objective space. Rewards: gait_tracking, base_xyz_tracking, base_quat_tracking, arm_swinging, arm_static, minimize_energy, etc.

## Citation

```
@article{janwani2026mo,
  title={MO-Playground: Massively Parallelized Multi-Objective Reinforcement Learning for Robotics},
  author={Janwani, Neil and Novoseller, Ellen and Lawhern, Vernon J and Tucker, Maegan},
  journal={arXiv preprint arXiv:2603.09237},
  year={2026}
}
```

## Relevance to thesis (Vision-guided MORL with UR5)

- Massively parallelized (JAX) MORL environments for robotics — useful for scalable baselines.
- BRUCE humanoid example with 6-D objective space and energy/tracking objectives is a good model for UR5-style multi-objective setups.
- MORLAX library (JAX-based MORL) is a candidate implementation framework.
