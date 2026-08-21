# DoughNet: A Visual Predictive Model for Topological Manipulation of Deformable Objects

Source: https://dough-net.github.io/
Date saved: 2026-08-21

Venue: ECCV 2024

Authors: Dominik Bauer, Zhenjia Xu, Shuran Song — Columbia University, Stanford University

Links:
- arXiv: http://arxiv.org/abs/2404.12524
- Code: https://github.com/dornik/doughnet
- Dataset: Google Drive (synthetic topological manipulation dataset)

## Abstract

We present DoughNet, a visual predictive model that enables planning of robotic manipulation for geometrical deformation and topological change of elastoplastic objects. Taking only a single RGBD observation as input, DoughNet selects the best tool, its in-plane pose, and opening width to recreate the desired robot- or human-made goal shape. The predictive model consists of two components: (1) a denoising autoencoder that embeds and completes partial point-cloud observations in a topology-aware latent space; and (2) an autoregressive set-to-set model that, given the current object state representation and the desired tool motion, predicts the next latent state.

## Method

- Input: single partial RGBD observation of (held-out) objects + a set of (held-out) tools.
- Output: (1) best suited tool, (2) its in-plane pose, (3) its final opening width, satisfying both geometry and topology of the goal state.
- Topology-aware latent space: denoising autoencoder completes partial point clouds; the autoregressive set-to-set predictor advances the latent state given tool motion.
- Predicts the complete object even when the observation is severely occluded (e.g., by the closed tool).

## Synthetic topological manipulation dataset

- Uses MPM-based simulation to create a synthetic dataset (perturbation is destructive/geometry-deforming in the real world).
- Two checking operations for topological change:
  1. (Self-)merge check: pull previously disconnected components apart (opposite of tool direction); if they remain connected, a merge is recorded.
  2. Split check: pull previously connected components apart (orthogonal to tool direction); if not connected in the radius graph, a split is recorded.

## Demonstrated capabilities

- Robot-defined goal: splitting a doughnut shape into two smaller doughnuts (vs a figure-8).
- Human-defined goal (pinching rolls → X-shape): selects a square tool to reproduce the finger-pinching effect.
- Human-defined goal (squeezing doughnut between palms → roll): selects a wide tool; self-merging closes the hole.
- Predictive accuracy: correctly predicts component separation/merging and self-merging (genus reduction) with tool-conditioned dynamics.

## BibTeX

```
@article{bauer2024doughnet,
  title={DoughNet: A Visual Predictive Model for Topological Manipulation of Deformable Objects},
  author={Bauer, Dominik and Xu, Zhenjia and Song, Shuran},
  journal={European Conference on Computer Vision (ECCV)},
  year={2024}
}
```

## Relevance to thesis (Vision-guided MORL with UR5)

- A visual predictive model operating from a single RGBD point-cloud observation — directly relevant to vision-guided manipulation of deformable objects.
- Demonstrates planning over tool selection, pose, and opening width, i.e., discrete+continuous action decision-making over object states.
- The latent space prediction + tool-conditioned dynamics could be integrated with a multi-objective planner (e.g., trade-offs between geometric fidelity and topological change).
- Deformable-object manipulation is a natural scenario for dynamic environments and complements rigid-object diffusion/BC baselines in this collection.
