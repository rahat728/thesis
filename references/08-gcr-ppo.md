# GCR-PPO: Scalable Multi-Objective Robot Reinforcement Learning through Gradient Conflict Resolution

Source: https://github.com/humphreymunn/GCR-PPO
Date saved: 2026-08-02

Paper: https://arxiv.org/abs/2509.14816
Code: https://github.com/humphreymunn/GCR-PPO

## Overview

GCR-PPO is a modification of PPO for multi-objective robot RL that:
- uses a multi-head critic to obtain per-reward advantages and gradients,
- applies priority-aware gradient surgery (PCGrad-style projection) to protect task objectives from regularisers,
- runs at massively parallel GPU scale within IsaacLab/RSL-RL.

This repo is a focused fork/adaptation of IsaacLab targeting IsaacLab 2.1.0 with RSL-RL.

## How to Run

GCR-PPO (multi-head critic + priority-aware PCGrad):
```
./isaaclab.sh -p scripts/reinforcement_learning/rsl_rl/train.py \
  --task=Isaac-Humanoid-v0 \
  --num_envs 4096 --seed 0 --headless \
  --use_critic_multi --use_pcgrad
```

Multi-head critic only (no conflict resolution):
```
./isaaclab.sh -p scripts/reinforcement_learning/rsl_rl/train.py \
  --task=Isaac-Humanoid-v0 \
  --num_envs 4096 --seed 0 --headless \
  --use_critic_multi
```

Supported example tasks: Isaac-Humanoid-Direct-v0 (multi-objective humanoid running), Throwing-G1-General (full-body throwing).

Adding a new task requires setting reward_component_names and reward_component_task_rew (task rewards get priority during gradient surgery so they are protected from being weakened by regularisers).

## Paper

- Comparisons against massively parallel GPU PPO across 13 IsaacLab tasks.
- Two custom multi-objective suites (Humanoid Running, Full-Body Throwing).
- Ablations (multi-head only vs. GCR) and conflict-performance analyses.

## Citation

```
@article{munn2025scalable,
  title={Scalable Multi-Objective Robot Reinforcement Learning through Gradient Conflict Resolution},
  author={Munn, Humphrey and Tidd, Brendan and Böhm, Peter and Gallagher, Marcus and Howard, David},
  journal={arXiv preprint arXiv:2509.14816},
  year={2025}
}
```

## Relevance to thesis (Vision-guided MORL with UR5)

- Directly relevant: multi-objective robot RL at massively parallel GPU scale.
- Multi-head critic + PCGrad-style gradient surgery is a strong candidate baseline/component for combining vision objectives (e.g., reaching success) with regularisers (e.g., smoothness, safety, energy) for a UR5.
- IsaacLab framework is well suited to UR5-based simulation experiments.
