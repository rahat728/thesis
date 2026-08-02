# VRB: Affordances from Human Videos as a Versatile Representation for Robotics

Source: https://vision-robotics-bridge.github.io/
Date saved: 2026-08-02

Title: Affordances from Human Videos as a Versatile Representation for Robotics (Vision-Robotics Bridge, VRB)
Venue: CVPR 2023

Authors: Shikhar Bahl*, Russell Mendonca*, Lili Chen, Unnat Jain, Deepak Pathak (Carnegie Mellon University, Meta AI) (*equal contribution)

Links:
- Paper: https://vision-robotics-bridge.github.io/resources/vrb_paper.pdf
- arXiv (paper page)
- Video: https://www.youtube.com/embed/WdMYGESu8Ak

## Abstract

Building a robot that can understand and learn to interact by watching humans has inspired several vision problems. However, despite some successful results on static datasets, it remains unclear how current models can be used on a robot directly. In this paper, we aim to bridge this gap by leveraging videos of human interactions in an environment centric manner. Utilizing internet videos of human behavior, we train a visual affordance model that estimates where and how in the scene a human is likely to interact. The structure of these behavioral affordances directly enables the robot to perform many complex tasks. We show how to seamlessly integrate our affordance model with four robot learning paradigms including offline imitation learning, exploration, goal-conditioned learning, and action parameterization for reinforcement learning. We show the efficacy of our approach, which we call Vision-Robotics Bridge (VRB) as we aim to seamlessly integrate computer vision techniques with robotic manipulation, across 4 real world environments, over 10 different tasks, and 2 robotic platforms operating in the wild.

## Method

- Learns actionable (visual) affordance representations from large-scale human video datasets (Ego4D, Epic Kitchens).
- Affordance defined agent-agnostically: contact point + post-contact wrist trajectory.
- Annotation pipeline: hand-object interaction detector finds contact frame; wrist tracking yields post-contact trajectory; affordances mapped back to human-agnostic first frame to avoid distribution shift.
- Model: contact head outputs contact heatmap; trajectory transformer predicts wrist waypoints. Used at inference with sparse 3D info (depth) + robot kinematics.

## Applications

Integrated with 4 robot learning paradigms:
1. Affordance-model-driven data collection for offline imitation.
2. Reward-free exploration.
3. Goal-conditioned policy learning.
4. Action parameterization for RL (using affordance model outputs to reparameterize actions).

Results:
- 10+ tasks, 2 robot morphologies, 4 learning paradigms, 4 real-world environments.
- Simulation benchmark: Franka Kitchen (D4RL) — superior performance on three tasks.
- Handles rare objects well (outperforms Hotspots baseline on held-out items).

## BibTeX

```
@inproceedings{bahl2023affordances,
              title={Affordances from Human Videos as a Versatile Representation for Robotics},
              author={Bahl, Shikhar and Mendonca, Russell and Chen, Lili and Jain, Unnat and Pathak, Deepak},
              journal={CVPR},
              year={2023}
            }
```

## Relevance to thesis (Vision-guided MORL with UR5)

- Affordance-based action parameterization for RL is directly relevant — affordances can structure the action space for UR5 RL.
- Human-video-driven learning reduces need for robot demonstrations.
- Bridges computer vision and robotic manipulation; relevant to the "vision-guided" pillar and to integrating vision into the MORL loop.
