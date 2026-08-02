# ALOHA Unleashed: A Simple Recipe for Robot Dexterity

Source: https://aloha-unleashed.github.io/
Date saved: 2026-08-02

Authors: Tony Z. Zhao*, Jonathan Tompson, Danny Driess, Pete Florence, Kamyar Ghasemipour, Chelsea Finn, Ayzaan Wahid* (*equal contribution) — Google DeepMind

Paper: https://aloha-unleashed.github.io/assets/aloha_unleashed.pdf

## Abstract

Recent work has shown promising results for learning end-to-end robot policies using imitation learning. In this work we address the question of how far can we push imitation learning for challenging dexterous manipulation tasks. We show that a simple recipe of large scale data collection on the ALOHA 2 platform, combined with expressive models such as Diffusion Policies, can be effective in learning challenging bimanual manipulation tasks involving deformable objects and complex contact rich dynamics. We demonstrate our recipe on 5 challenging real-world and 3 simulated tasks and demonstrate improved performance over state-of-the-art baselines.

## System

- ALOHA Unleashed: general imitation learning system for training dexterous policies on robots.
- ALOHA 2: bimanual parallel-jaw gripper workcell with two 6-DoF arms.
- Framework: scalable teleoperation for data collection + Transformer-based neural network trained with Diffusion Policy.

## Tasks

Real world (5):
- Shoe Lace Tying (center shoe, straighten laces, tie bow).
- Robot Finger Replacement (remove finger from slotted mechanism, pick up replacement, orient, precisely insert with millimeter tolerance).
- Shirt Hanging (flatten/orient shirt, pick hanger, handover, insert both sides of hanger into collar, hang back) — handles unseen shirt types.
- Gear Insertion (insert 3 plastic gears onto socket with millimeter precision, friction fit, teeth meshing).
- Random Kitchen (clean up randomly initialized table by stacking bowls/cups/utensils).

Simulated (3):
- Single peg insertion, double peg insertion, placing a mug on a plate.

## Relevance to thesis (Vision-guided MORL with UR5)

- Confirms Diffusion Policy + large-scale teleoperation as a strong simple recipe for bimanual dexterous manipulation.
- ALOHA 2 uses two 6-DoF arms (similar kinematic scale to dual UR5s).
- Contact-rich and deformable object manipulation relevance to dynamic environments.
- Directly extends the ALOHA/ACT line and pairs with Diffusion Policy.
