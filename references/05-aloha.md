# ALOHA: Learning Fine-Grained Bimanual Manipulation with Low-Cost Hardware

Source: https://tonyzhaozh.github.io/aloha/
Date saved: 2026-08-02

Venue: RSS 2023

Authors: Tony Zhao, Vikash Kumar, Sergey Levine, Chelsea Finn (Stanford University, UC Berkeley, Meta)

Links:
- Paper: https://arxiv.org/abs/2304.13705
- ALOHA Codebase: https://github.com/tonyzhaozh/aloha
- ACT+Sim Codebase: https://github.com/tonyzhaozh/act
- Hardware Tutorial and ALOHA Kit (Trossen Robotics) links on original page

## Abstract

Fine manipulation tasks, such as threading cable ties or slotting a battery, are notoriously difficult for robots because they require precision, careful coordination of contact forces, and closed-loop visual feedback. Performing these tasks typically requires high-end robots, accurate sensors, or careful calibration, which can be expensive and difficult to set up. Can learning enable low-cost and imprecise hardware to perform these fine manipulation tasks? We present a low-cost system that performs end-to-end imitation learning directly from real demonstrations, collected with a custom teleoperation interface. Imitation learning, however, presents its own challenges, particularly in high-precision domains: the error of the policy can compound over time, drifting out of the training distribution. To address this challenge, we develop a novel algorithm Action Chunking with Transformers (ACT) which reduces the effective horizon by simply predicting actions in chunks. This allows us to learn difficult tasks such as opening a translucent condiment cup and slotting a battery with 80-90% success, with only 10 minutes worth of demonstration data.

## Teleoperation System

- ALOHA: A Low-cost Open-source Hardware System for Bimanual Teleoperation.
- ~$20k budget; capable of teleoperating precise tasks (threading a zip tie), dynamic tasks (juggling a ping pong ball), and contact-rich tasks (assembling chain in NIST board #2).

## Learning Algorithm: ACT

Action Chunking with Transformers (ACT):
- Key design: predict a sequence of actions ("an action chunk") instead of a single action like standard Behavior Cloning.
- ACT policy is trained as the decoder of a Conditional VAE (CVAE), a generative model.
- Synthesizes images from multiple viewpoints, joint positions, and a style variable z with a transformer encoder, and predicts a sequence of actions with a transformer decoder.
- At test time, the CVAE encoder is discarded and z is set to the mean of the prior (zero).

Real-time ACT rollouts from 50 demonstrations per task; ACT directly predicts joint positions at 50Hz with a fixed chunk size of 90. Episode length is between 600 and 1000. Object position randomized along 15cm line. Success: 96%, 84%, 64%, 92% on four tasks.

## Reactiveness & Robustness

- ACT reacts to novel environment disturbances (not only memorizing training data).
- Robust against a certain level of distractors.
- Observations at evaluation time: 4 RGB cameras streaming at 480x640 (2 stationary, 2 wrist-mounted).

## BibTeX (from the mobile-aloha page reference style / paper)

Not explicitly given on page; canonical citation:

```
@inproceedings{zhao2023learning,
  title={Learning fine-grained bimanual manipulation with low-cost hardware},
  author={Zhao, Tony Z and Kumar, Vikash and Levine, Sergey and Finn, Chelsea},
  booktitle={Robotics: Science and Systems},
  year={2023}
}
```

## Relevance to thesis (Vision-guided MORL with UR5)

- ACT is a widely used imitation-learning baseline for visuomotor policies (action chunking reduces compounding error — directly relevant to vision-guided control in dynamic environments).
- Multi-view RGB observation (stationary + wrist cameras) pattern is common for UR5 setups.
- Demonstrates low-cost hardware + data-efficient learning; relevant if your UR5 approach uses demonstration data.
