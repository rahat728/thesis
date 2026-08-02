# VideoManip: Dexterous Manipulation Policies from RGB Human Videos via 3D Hand-Object Trajectory Reconstruction

Source: https://videomanip.github.io/
Date saved: 2026-08-02

## Abstract

Multi-finger robotic hand manipulation and grasping are challenging due to the high-dimensional action space and the difficulty of acquiring large-scale training data. Existing approaches largely rely on human teleoperation with wearable devices or specialized sensing equipment to capture hand-object interactions, which limits scalability. In this work, we propose VideoManip, a device-free framework that learns dexterous manipulation directly from RGB human videos. Leveraging recent advances in computer vision, VideoManip reconstructs explicit 3D robot-object trajectories from monocular videos by estimating human hand poses, object meshes, and retargets the reconstructed human motions to robotic hands for manipulation learning. To make the reconstructed robot data suitable for dexterous manipulation training, we introduce hand-object contact optimization with interaction-centric grasp modeling, as well as a demonstration synthesis strategy that generates diverse training trajectories from a single video, enabling generalizable policy learning without additional robot demonstrations. In simulation, the learned grasping model achieves a 70.25% success rate across 20 diverse objects using the Inspire Hand. In the real world, manipulation policies trained from RGB videos achieve an average 62.86% success rate across seven tasks using the LEAP Hand, outperforming retargeting-based methods by 15.87%.

## Method

- Reconstructs explicit 3D hand-object trajectories from monocular videos (human hand poses, object meshes, object scales).
- Retargets reconstructed human motions to robot hands.
- In-scene videos: known camera-robot extrinsic calibration transforms trajectories to robot base frame.
- In-the-wild videos: gravity direction estimated from visual observations; camera-centric trajectories aligned to physically meaningful world frame.
- Components: (i) differential hand pose optimization via predicted hand-object contact maps; (ii) interaction-centric grasp modeling; (iii) DemoGen for one-to-many demonstration synthesis.

## Results

- Simulation: 70.25% success rate across 20 diverse objects (18-DoF Inspire Hand, IsaacGym).
- Real world: average 62.86% success rate across 7 tasks (LEAP Hand), +15.87% over retargeting-based methods.
- Real-world rollouts use a closed-loop trained DP3 policy.
- Failure analysis: object pose estimation errors (occlusion), hand retargeting failures (hand size mismatch), manipulation failures.

## Relevance to thesis (Vision-guided MORL with UR5)

- Device-free data generation for dexterous manipulation from RGB videos.
- Uses DP3 as the real-world policy — connects directly to the vision-guided policy line in your thesis.
- Demonstrates large-scale training data acquisition without robot demonstrations — relevant to reducing UR5 data collection cost.
