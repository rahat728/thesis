# Demonstration-Enhanced Adaptable Multi-Objective Robot Navigation

Source: https://www.hrl.uni-bonn.de/publications/2025/deheuvel2025iros_morl
Date saved: 2026-08-02

Authors: J. de Heuvel, T. Sethuraman, M. Bennewitz (Humanoid Robots Lab, University of Bonn)
Venue: IEEE/RSJ International Conference on Intelligent Robots and Systems (IROS) 2025

Links:
- Preprint: http://arxiv.org/abs/2404.04857
- Code: https://github.com/HumanoidsBonn/demo_enhanced_morl_nav
- Video: https://youtu.be/vS22B3HRdL4

## Abstract

Preference-aligned robot navigation in human environments is typically achieved through learning-based approaches, utilizing user feedback or demonstrations for personalization. However, personal preferences are subject to change and might even be context-dependent. Yet traditional reinforcement learning (RL) approaches with static reward functions often fall short in adapting to evolving user preferences, inevitably reflecting demonstrations once training is completed. This paper introduces a structured framework that combines demonstration-based learning with multi-objective reinforcement learning (MORL). To ensure real-world applicability, our approach allows for dynamic adaptation of the robot navigation policy to changing user preferences without retraining. It fluently modulates the amount of demonstration data reflection and other preference-related objectives. Through rigorous evaluations, including a baseline comparison and sim-to-real transfer on two robots, we demonstrate our framework's capability to adapt to user preferences accurately while achieving high navigational performance in terms of collision avoidance and goal pursuance.

## Key ideas

- Combines demonstration-based learning with MORL.
- Dynamic adaptation of policy to changing user preferences without retraining (preference-conditioned).
- Modulates amount of demonstration-data reflection and other preference-related objectives.
- Evaluated with baseline comparison and sim-to-real transfer on two robots.
- Performance metrics: collision avoidance and goal pursuance.

## Relevance to thesis (Vision-guided MORL with UR5)

- Directly relevant: MORL + preference conditioning + dynamic adaptation for robots in human/dynamic environments.
- Collision avoidance + goal pursuance objectives map to UR5 safety/performance trade-offs.
- Sim-to-real transfer methodology is directly applicable to UR5 experiments.
- Demonstration-enhanced MORL is a bridge between the imitation-learning papers (Diffusion Policy/ACT) and MORL.
