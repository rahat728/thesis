# Flow as the Cross-domain Manipulation Interface (Im2Flow2Act)

Source: https://im-flow-act.github.io/
Date saved: 2026-08-21

Venue: CoRL 2024 (8th Conference on Robot Learning)

Authors: Mengda Xu, Zhenjia Xu, Yinghao Xu, Cheng Chi, Gordon Wetzstein, Manuela Veloso, Shuran Song — Stanford University, Columbia University, JP Morgan AI Research, Carnegie Mellon University

Links:
- arXiv: https://arxiv.org/abs/2407.15208
- Code: https://github.com/real-stanford/im2Flow2Act

## Abstract

We present Im2Flow2Act, a scalable learning framework that enables robots to acquire manipulation skills from diverse data sources. The key idea behind Im2Flow2Act is to use object flow as the manipulation interface, bridging domain gaps between different embodiments (i.e., human and robot) and training environments (i.e., real-world and simulated). Im2Flow2Act comprises two components: a flow generation network and a flow-conditioned policy. The flow generation network, trained on human demonstration videos, generates object flow from the initial scene image, conditioned on the task description. The flow-conditioned policy, trained on simulated robot play data, maps the generated object flow to robot actions to realize the desired object movements. By using flow as input, this policy can be directly deployed in the real world with a minimal sim-to-real gap. By leveraging real-world human videos and simulated robot play data, we bypass the challenges of teleoperating physical robots in the real world, resulting in a scalable system for diverse tasks. We demonstrate Im2Flow2Act's capabilities in a variety of real-world tasks, including the manipulation of rigid, articulated, and deformable objects.

## Method

- Object flow = the exclusive motion of the manipulated object (excluding background/embodiment movement) — a unifying interface across embodiments (human/robot) and environments (real/sim).
- Two components:
  1. Flow generation network: language-conditioned, built on the video generation model AnimateDiff; learns high-level task planning from action-less human demonstration videos.
  2. Flow-conditioned policy: learns low-level execution entirely from simulated play data (4800 play episodes across rigid, articulated, and deformable objects; one single policy), mapping flow to robot actions.
- Inference: flow generation network produces task flow from initial RGB image + task description + initial keypoints (from Grounding DINO); a motion filter removes background keypoints; the flow-conditioned policy executes.

## Key results

- Average success rate of 81% across four real-world tasks (rigid, articulated, deformable objects).
- One-shot generalization to new real-world skills enabled by the cross-embodiment/cross-environment flow interface.
- Bypasses real-world teleoperation for scalable data acquisition.

## BibTeX

```
@inproceedings{xu2025flow,
  title={Flow as the Cross-domain Manipulation Interface},
  author={Xu, Mengda and Xu, Zhenjia and Xu, Yinghao and Chi, Cheng and Wetzstein, Gordon and Veloso, Manuela and Song, Shuran},
  booktitle={Conference on Robot Learning},
  pages={2475--2499},
  year={2025},
  organization={PMLR}
}
```

## Relevance to thesis (Vision-guided MORL with UR5)

- Object flow as a domain-agnostic interface is a strong conceptual tool for vision-guided control: it decouples high-level task planning (from human video) from low-level action generation (from simulated data).
- Relevant to multi-objective formulations where the robot must trade off, e.g., minimizing object disturbance vs task completion — flow provides a dense, task-relevant signal.
- Language-conditioned flow generation + flow-conditioned execution is a scalable data-collection/policy framework alternative to teleoperation, complementary to UMI and Diffusion Policy baselines.
- Handles rigid, articulated, and deformable objects, covering the dynamic-environment manipulation space.
