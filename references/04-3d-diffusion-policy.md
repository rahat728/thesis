# 3D Diffusion Policy (DP3)

Source: https://3d-diffusion-policy.github.io/
Date saved: 2026-08-02

Title: Generalizable Visuomotor Policy Learning via Simple 3D Representations
Venue: Robotics: Science and Systems (RSS) 2024

Authors: Yanjie Ze, Gu Zhang, Kangning Zhang, Chenyuan Hu, Muhan Wang, Huazhe Xu (Shanghai Qi Zhi Institute, Shanghai Jiao Tong University, Tsinghua University IIIS, Shanghai AI Lab)

Links:
- arXiv: https://arxiv.org/abs/2403.03954
- Paper: https://arxiv.org/pdf/2403.03954.pdf
- Code: https://github.com/YanjieZe/3D-Diffusion-Policy
- Simulation Data / Real Robot Data links on original page

## Abstract

Imitation learning provides an efficient way to teach robots dexterous skills; however, learning complex skills robustly and generalizablely usually consumes large amounts of human demonstrations. To tackle this challenging problem, we present 3D Diffusion Policy (DP3), a novel visual imitation learning approach that incorporates the power of 3D visual representations into diffusion policies, a class of conditional action generative models. The core design of DP3 is the utilization of a compact 3D visual representation, extracted from sparse point clouds with an efficient point encoder. In our experiments involving 72 simulation tasks, DP3 successfully handles most tasks with just 10 demonstrations and surpasses baselines with a 24.2% relative improvement. In 4 real robot tasks, DP3 demonstrates precise control with a high success rate of 85%, given only 40 demonstrations of each task, and shows excellent generalization abilities in diverse aspects, including space, viewpoint, appearance, and instance. Interestingly, in real robot experiments, DP3 rarely violates safety requirements, in contrast to baseline methods which frequently do, necessitating human intervention. Our extensive evaluation highlights the critical importance of 3D representations in real-world robot learning.

## Effectiveness & Generalization

- 72 simulated tasks and 4 real-world tasks.
- 67/72 simulated tasks use only 10 demonstrations; 4 real-world tasks use only 40 demonstrations.
- 24.2% relative improvement over baselines.
- 85% success rate on real robot tasks (Allegro Hand 22 DoF for 3 tasks, gripper 7 DoF for 1 task).
- Generalization: space, viewpoint, appearance, instance.
- Baselines compared: Diffusion Policy (image and depth), IBC, BCRNN.

## Method

DP3 perceives the environment through single-view point clouds. Sparsely sampled point clouds are encoded into compact 3D representations by a lightweight DP3 encoder. Subsequently, DP3 generates actions conditioned on these 3D representations and the robot states, using a diffusion model as the backbone.

## Benchmark

- Real-world tasks: Roll-Up, Dumpling, Drill, Pour.
- Simulated tasks: 72 tasks from 7 benchmarks — Adroit, Bi-DexHands, DexArt, DexDeform, DexMV, HORA, MetaWorld.

## Safety

- Image-based and depth-based diffusion policies often exhibit unpredictable real-world behaviors requiring human termination (safety violation).
- DP3 rarely violates safety; described as practical and hardware-friendly for real robot learning.

## Simple DP3

Simplified policy backbone of DP3 offering 2x inference speed while maintaining high accuracy (removes redundant UNet components). Implemented in the released code.

## BibTeX

```
@inproceedings{Ze2024DP3,
	title={3D Diffusion Policy: Generalizable Visuomotor Policy Learning via Simple 3D Representations},
	author={Yanjie Ze and Gu Zhang and Kangning Zhang and Chenyuan Hu and Muhan Wang and Huazhe Xu},
	booktitle={Proceedings of Robotics: Science and Systems (RSS)},
	year={2024}
}
```

## Relevance to thesis (Vision-guided MORL with UR5)

- Strong candidate for the "vision-guided" component of your policy architecture: point-cloud-based visuomotor policy with diffusion action generation.
- Demonstrates data-efficient learning and safety robustness on real robot arms (relevant for UR5 in dynamic environments).
- The 3D representation approach is highly relevant to a UR5 setup with depth/RGB-D cameras, especially in dynamic settings where appearance/viewpoint generalization matter.
