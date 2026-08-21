# Universal Manipulation Interface (UMI): In-The-Wild Robot Teaching Without In-The-Wild Robots

Source: https://umi-gripper.github.io/
Date saved: 2026-08-21

Venue: RSS 2024 — Best Systems Paper Award Finalist

Authors: Cheng Chi*, Zhenjia Xu*, Chuer Pan, Eric Cousineau, Benjamin Burchfiel, Siyuan Feng, Russ Tedrake, Shuran Song (* equal contribution) — Stanford University, Columbia University, Toyota Research Institute

Links:
- arXiv: https://arxiv.org/abs/2402.10329
- Code: https://github.com/real-stanford/universal_manipulation_interface

## Abstract

We present Universal Manipulation Interface (UMI) — a data collection and policy learning framework that allows direct skill transfer from in-the-wild human demonstrations to deployable robot policies. UMI employs hand-held grippers coupled with careful interface design to enable portable, low-cost, and information-rich data collection for challenging bimanual and dynamic manipulation demonstrations. To facilitate deployable policy learning, UMI incorporates a carefully designed policy interface with inference-time latency matching and a relative-trajectory action representation. The resulting learned policies are hardware-agnostic and deployable across multiple robot platforms. Equipped with these features, UMI framework unlocks new robot manipulation capabilities, allowing zero-shot generalizable dynamic, bimanual, precise, and long-horizon behaviors, by only changing the training data for each task. We demonstrate UMI's versatility and efficacy with comprehensive real-world experiments, where policies learned via UMI zero-shot generalize to novel environments and objects when trained on diverse human demonstrations.

## System

- Data collection hardware: hand-held parallel jaw gripper with a wrist-mounted GoPro camera (used for SLAM-based 6-DoF action tracking and visual context/depth estimation).
- Policy interface: inference-time observation and action latency matching + relative-trajectory action representation (camera-centric), making policies hardware-agnostic.
- 100% calibration-free; robust against base movement, distractors, and drastic lighting changes.

## Key results

- Data collection: ~30 s/demonstration, over 3x faster than teleoperation with space mouse (111/h vs 35/h), reaching 48% of human hand speed.
- Deployed the same cup-arrangement policy on both UR5e and Franka robots (works on any robot with parallel jaw stroke > 85 mm).
- Ablations: no latency matching → jittery movement; no inter-gripper proprioception → worse bimanual coordination; ResNet encoder → non-reactive dish-washing behavior (vs CLIP-pretrained ViT).
- In-the-wild generalization: trained on diverse human cup-manipulation data, a diffusion policy generalizes to out-of-distribution objects and environments (e.g., serving an espresso cup on a water fountain).
- Capability tasks: dynamic tossing, cup arrangement, bimanual cloth folding, dish washing (7 sequential dependent actions).

## BibTeX

```
@inproceedings{chi2024universal,
	title={Universal Manipulation Interface: In-The-Wild Robot Teaching Without In-The-Wild Robots},
	author={Chi, Cheng and Xu, Zhenjia and Pan, Chuer and Cousineau, Eric and Burchfiel, Benjamin and Feng, Siyuan and Tedrake, Russ and Song, Shuran},
	booktitle={Proceedings of Robotics: Science and Systems (RSS)},
	year={2024}
}
```

## Relevance to thesis (Vision-guided MORL with UR5)

- Provides a low-cost, portable, in-the-wild data collection interface — a practical alternative to teleoperation for gathering diverse demonstration data for UR5 policies.
- Demonstrates policy deployment directly on UR5e hardware with zero calibration; camera-centric relative action representation is relevant to vision-guided control.
- Dynamic and bimanual manipulation demonstrations are directly relevant to dynamic environments.
- Pairs naturally with Diffusion Policy (the framework's learned policy backbone), a key baseline in this collection.
