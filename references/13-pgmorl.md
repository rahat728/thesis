# PGMORL: Prediction-Guided Multi-Objective Reinforcement Learning for Continuous Robot Control

Source: https://pgmorl.csail.mit.edu/
Date saved: 2026-08-02

Venue: International Conference on Machine Learning (ICML) 2020
Authors: Jie Xu, Yunsheng Tian, Pingchuan Ma, Daniela Rus (MIT), Shinjiro Sueda, Wojciech Matusik (MIT)

Links:
- Paper: https://people.csail.mit.edu/jiex/papers/PGMORL/paper.pdf
- Supp: https://people.csail.mit.edu/jiex/papers/PGMORL/supp.pdf
- Video: https://people.csail.mit.edu/jiex/papers/PGMORL/video.mp4
- Code: https://github.com/mit-gfx/PGMORL

## Abstract

Many real-world control problems involve conflicting objectives where we desire a dense and high-quality set of control policies that are optimal for different objective preferences (called Pareto-optimal). While extensive research in multi-objective reinforcement learning (MORL) has been conducted to tackle such problems, multi-objective optimization for complex continuous robot control is still under-explored. In this work, we propose an efficient evolutionary learning algorithm to find the Pareto set approximation for continuous robot control problems, by extending a state-of-the-art RL algorithm and presenting a novel prediction model to guide the learning process. In addition to efficiently discovering the individual policies on the Pareto front, we construct a continuous set of Pareto-optimal solutions by Pareto analysis and interpolation. Furthermore, we design seven multi-objective RL environments with continuous action space, which is the first benchmark platform to evaluate MORL algorithms on various robot control problems. We test the previous methods on the proposed benchmark problems, and the experiments show that our approach is able to find a much denser and higher-quality set of Pareto policies than the existing algorithms.

## Key ideas

- Evolutionary learning algorithm to find Pareto set approximation for continuous robot control.
- Extends a state-of-the-art RL algorithm with a prediction model to guide learning.
- Constructs a continuous set of Pareto-optimal solutions via Pareto analysis and interpolation.
- First benchmark platform of 7 multi-objective RL environments with continuous action space for robot control.

## Citation

```
@inproceedings{xu2020prediction,
  title={Prediction-Guided Multi-Objective Reinforcement Learning for Continuous Robot Control},
  author={Xu, Jie and Tian, Yunsheng and Ma, Pingchuan and Rus, Daniela and Sueda, Shinjiro and Matusik, Wojciech},
  booktitle={Proceedings of the 37th International Conference on Machine Learning},
  year={2020}
}
```

## Related paper

- Multi-Objective Graph Heuristic Search for Terrestrial Robot Design (Jie Xu, Andrew Spielberg, Allan Zhao, Daniela Rus, Wojciech Matusik), ICRA 2021. Project: http://moghs.csail.mit.edu

## Relevance to thesis (Vision-guided MORL with UR5)

- PGMORL is a canonical evolutionary MORL algorithm for continuous robot control — must-cite baseline.
- Continuous Pareto-front interpolation is directly relevant to producing a preference-tunable UR5 policy.
- Its 7 benchmark environments are a reference for designing your own UR5 multi-objective benchmark.
- Available in MORL-Baselines, so easy to compare against.
