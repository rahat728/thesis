# Enabling Robot Manipulation of Soft and Rigid Objects with Vision-based Tactile Sensors

Source: https://vision-tactile-manip.github.io/exp/
Date saved: 2026-08-02

Authors: Michael C. Welle*, Martina Lippi*, Haofei Lu, Jens Lundell, Andrea Gasparri, Danica Kragic (*equal contribution)
Affiliations: KTH Royal Institute of Technology, Roma Tre University

Preprint: https://vision-tactile-manip.github.io/exp/files/tactile.pdf

## Abstract

Endowing robots with tactile capabilities opens up new possibilities for their interaction with the environment, including the ability to handle fragile and/or soft objects. In this work, we equip the robot gripper with low-cost vision-based tactile sensors and propose a manipulation algorithm that adapts both to rigid and soft objects, without requiring any knowledge on their properties. The algorithm relies on a simple touch and slip detection method, which takes into account the variation in the tactile images with respect to reference ones. We validate the approach on seven different objects, with different properties in terms of rigidity and fragility, to perform unplugging and lifting tasks. Furthermore, we integrate the manipulation algorithm into a comprehensive grasping pipeline, that recommends the optimal grasping pose based on the object cloud, and validate it to detach a grape from a bunch without damaging it.

## Method

- Low-cost vision-based tactile sensors (DIGIT) mounted on a robot gripper.
- Touch and slip detection based on variation of tactile images relative to reference images.
- Grasping pipeline recommending optimal grasping pose based on object point cloud.
- Validated on 7 objects (rough/smooth connectors, raw egg, plastic glass, tomato, table grapes) for unplugging and lifting tasks.

## Materials

- PyTouch library used for touch results: https://github.com/facebookresearch/PyTouch
- STL files for mounting DIGIT sensors on Franka Emika Panda gripper.

## Relevance to thesis (Vision-guided MORL with UR5)

- Shows tactile/vision sensor fusion for manipulation, useful for robustness in dynamic environments.
- Relevant if your UR5 setup includes tactile sensing or fragile/soft object manipulation.
- Vision-based tactile sensing can be one of the "objectives"/modalities in a multi-objective setup (e.g., force control, safety).
