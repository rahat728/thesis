# MO-MPO: A Distributional View on Multi-Objective Policy Optimization

Source: https://sites.google.com/view/mo-mpo
Date saved: 2026-08-02

## Title

A Distributional View on Multi-Objective Policy Optimization (MO-MPO)

Navigation subpages on the original site (videos): Home, Humanoid, Shadow Hand, Humanoid Mocap, Sawyer.

## Abstract

Many real-world problems require trading off multiple competing objectives. However, these objectives are often in different units and/or scales, which can make it challenging for practitioners to express numerical preferences over objectives in their native units. In this paper we propose a novel algorithm for multi-objective reinforcement learning that enables setting desired preferences for objectives in a scale-invariant way. We propose to learn an action distribution for each objective, and we use supervised learning to fit a parametric policy to a combination of these distributions. We demonstrate the effectiveness of our approach on challenging high-dimensional real and simulated robotics tasks, and show that setting different preferences in our framework allows us to trace out the space of nondominated solutions.

## Key ideas (derived from abstract)

- Scale-invariant preference setting over objectives (important for real robot tasks where objectives are in different units).
- Learns one action distribution per objective.
- Uses supervised learning to fit a parametric policy to a combination of these distributions.
- Demonstrated on high-dimensional real and simulated robotics tasks (Humanoid, Shadow Hand, Humanoid Mocap, Sawyer domains).
- Sweeping preferences traces out the Pareto/nondominated solution space.

## Relevance to thesis (Vision-guided MORL with UR5)

- MO-MPO is one of the strongest baseline algorithms for continuous-control MORL; directly comparable to any novel MORL algorithm you propose.
- Its scale-invariant preference mechanism is directly relevant when combining UR5 objectives such as task success, safety, energy cost, and time, which are in different units.
- Robotic domains (Sawyer, Humanoid) show the approach works with high-DoF robot manipulators/arms, analogous to UR5.
