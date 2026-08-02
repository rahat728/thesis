# IRIS: An Immersive Robot Interaction System

Source: https://intuitive-robots.github.io/iris-project-page/
Date saved: 2026-08-02

Authors: Xinkai Jiang, Qihao Yuan, Enes Ulas Dincer, Hongyi Zhou, Ge Li, Xueyin Li, Xiaogang Jia, Timo Schnizer, Nicolas Schreiber, Weiran Liao, Julius Haag, Kailai Li, Gerhard Neumann, Rudolf Lioutikov (Karlsruhe Institute of Technology, University of Groningen)

Venue: CoRL 2025

Links:
- Paper: https://openreview.net/pdf?id=b2mXmmGX8E
- arXiv: https://arxiv.org/abs/2502.03297
- Python Code: https://github.com/intuitive-robots/SimPublisher
- Documentation: https://intuitive-robots.github.io/iris-project-page/introduction/overview.html

## Abstract

This paper introduces IRIS, an Immersive Robot Interaction System leveraging Extended Reality (XR). Existing XR-based systems enable efficient data collection but are often challenging to reproduce and reuse due to their specificity to particular robots, objects, simulators, and environments. IRIS addresses these issues by supporting immersive interaction and data collection across diverse simulators and real-world scenarios. It visualizes arbitrary rigid and deformable objects, robots from simulation, and integrates real-time sensor-generated point clouds for real-world applications. Additionally, IRIS enhances collaborative capabilities by enabling multiple users to simultaneously interact within the same virtual scene. Extensive experiments demonstrate that IRIS offers efficient and intuitive data collection in both simulated and real-world settings.

## Demo content (videos on original page)

Data collection across frameworks/simulators: Libero, Meta-World, Robocasa, Deformable.
Data collection for different embodiments: ALOHA, Humanoid, More Options.
More applications: Collaborative, RL Agent, Real Robot.

## BibTeX

```
@inproceedings{jiang2025iris,
  title={IRIS: An Immersive Robot Interaction System},
  author={Jiang, Xinkai and Yuan, Qihao and Dincer, Enes Ulas and Zhou, Hongyi and Li, Ge and Li, Xueyin and Haag, Julius and Schreiber, Nicolas and Li, Kailai and Neumann, Gerhard and others},
  booktitle={9th Annual Conference on Robot Learning},
  year={2025}
}
```

## Relevance to thesis (Vision-guided MORL with UR5)

- Relevant to teleoperation / demonstration collection pipeline for imitation learning with vision-guided manipulators.
- Supports real-time point-cloud integration (vision modality) and multiple simulators, which may matter if you collect UR5 demonstrations or run sim-to-real.
- IRIS works across embodiments including ALOHA-style arms; relevant if your data-collection interface is a related bottleneck for vision-guided manipulation.
