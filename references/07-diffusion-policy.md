# Diffusion Policy: Visuomotor Policy Learning via Action Diffusion

Source: https://diffusion-policy.cs.columbia.edu/
Date saved: 2026-08-02

Venue: RSS 2023 (IJRR 2024 extended version)

RSS 2023 authors: Cheng Chi, Siyuan Feng, Yilun Du, Zhenjia Xu, Eric Cousineau, Benjamin Burchfiel, Shuran Song
IJRR 2024 authors: Cheng Chi, Zhenjia Xu, Siyuan Feng, Eric Cousineau, Yilun Du, Benjamin Burchfiel, Russ Tedrake, Shuran Song

Affiliations: Columbia University, Toyota Research Institute, MIT

Links:
- arXiv RSS v4: https://arxiv.org/abs/2303.04137v4
- arXiv IJRR v5: https://arxiv.org/abs/2303.04137v5
- Sim & Real Repo: https://github.com/columbia-ai-robotics/diffusion_policy
- Experiment data and Colab notebooks on original page.

## Abstract (page summary)

This paper introduces Diffusion Policy, a new way of generating robot behavior by representing a robot's visuomotor policy as a conditional denoising diffusion process. We benchmark Diffusion Policy across 12 different tasks from 4 different robot manipulation benchmarks and find that it consistently outperforms existing state-of-the-art robot learning methods with an average improvement of 46.9%. Diffusion Policy learns the gradient of the action-distribution score function and iteratively optimizes with respect to this gradient field during inference via a series of stochastic Langevin dynamics steps. We find that the diffusion formulation yields powerful advantages when used for robot policies, including gracefully handling multimodal action distributions, being suitable for high-dimensional action spaces, and exhibiting impressive training stability. To fully unlock the potential of diffusion models for visuomotor policy learning on physical robots, this paper presents a set of key technical contributions including the incorporation of receding horizon control, visual conditioning, and the time-series diffusion transformer.

## Highlights

- Learns multi-modal behavior and commits to only one mode within each rollout (vs LSTM-GMM and IBC biased toward one mode, BET failed to commit).
- Predicts a sequence of actions for receding-horizon control.
- 6 DoF tasks near kinematic limits (Mug Flipping) and liquid manipulation with periodic actions (sauce pouring/spreading).
- Push-T: highly robust against perturbations and visual distractions.

## Benchmarks

- Outperforms prior SOTA on 12 tasks across 4 benchmarks with average success-rate improvement of 46.9%.
- Benchmarks: Robomimic (Lift, Can, Square, Tool Hang, Transport), Implicit Behavior Cloning (Push-T, Block Pushing), Behavior Transformer (Block Pushing, Franka Kitchen), Relay Policy Learning (Franka Kitchen).

## Code and Data

- Sim & Real Repo, Experiment Data, State-based Colab Notebook, Vision-based Colab Notebook.

## BibTeX

```
@inproceedings{chi2023diffusionpolicy,
	title={Diffusion Policy: Visuomotor Policy Learning via Action Diffusion},
	author={Chi, Cheng and Feng, Siyuan and Du, Yilun and Xu, Zhenjia and Cousineau, Eric and Burchfiel, Benjamin and Song, Shuran},
	booktitle={Proceedings of Robotics: Science and Systems (RSS)},
	year={2023}
}

@article{chi2024diffusionpolicy,
	author = {Cheng Chi and Zhenjia Xu and Siyuan Feng and Eric Cousineau and Yilun Du and Benjamin Burchfiel and Russ Tedrake and Shuran Song},
	title ={Diffusion Policy: Visuomotor Policy Learning via Action Diffusion},
	journal = {The International Journal of Robotics Research},
	year = {2024},
}
```

## Real World Tasks

- Push-T: push T-shaped block into target region and move end-effector to end-zone. Robust against occlusion (hand waving in front of camera), perturbations during pushing/finishing stages.
- Mug Flipping: pickup randomly placed mug, place lip down, rotate handle to left.
- Sauce Pouring and Spreading on pizza dough.

## Acknowledgements

Supported in part by NSF Awards 2037101, 2132519, and Toyota Research Institute. Google provided the UR5 robot hardware.

## Relevance to thesis (Vision-guided MORL with UR5)

- Diffusion Policy is the central baseline/backbone for visuomotor policies (relevant to your "vision-guided" component).
- Notably, the paper's real robot hardware was a UR5 — directly relevant to your UR5 setup.
- Receding-horizon action prediction, multimodal action handling, and robustness to visual perturbations are directly relevant to dynamic environments.
- Natural combination point with MORL: diffusion policies as the action-generation module under multi-objective reward signals.
