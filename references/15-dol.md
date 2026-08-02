# Multi-Objective Deep Reinforcement Learning (DOL)

Source: https://github.com/hossam-mossalam/multi-objective-deep-rl
Date saved: 2026-08-02

Authors: Hossam Mossalam, Yannis M. Assael, Diederik M. Roijers, Shimon Whiteson
Code: https://github.com/hossam-mossalam/multi-objective-deep-rl

## Overview

The repo proposes Deep Optimistic Linear Support Learning (DOL) to solve high-dimensional multi-objective decision problems where the relative importances of the objectives are not known a priori. Using features from the high-dimensional inputs, DOL computes the convex coverage set containing all potential optimal solutions of the convex combinations of the objectives. To our knowledge, this is the first time that deep reinforcement learning has succeeded in learning multi-objective policies. In addition, it provides a testbed with two experiments to be used as a benchmark for deep multi-objective reinforcement learning.

## Key ideas

- Deep Optimistic Linear Support Learning (DOL).
- Computes the convex coverage set (CCS) over convex combinations of objectives.
- Handles unknown relative objective importances.
- Claims first success of deep RL for multi-objective policies.
- Testbed with two experiments (DeepSea.lua, ImageDeepSea.lua — including an image-based variant, ImageDeepSea).
- Implementation in Lua (Torch).

## Relevance to thesis (Vision-guided MORL with UR5)

- Foundational work connecting deep RL + MORL; the ImageDeepSea experiment is an early example of image-based (vision) input for multi-objective RL.
- Convex coverage set / optimistic linear support concepts are foundational for MORL.
- Good historical citation for "deep MORL" and image-based MORL motivation.
