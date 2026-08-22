# Literature Index — Vision-guided MORL using UR5 in Dynamic Environments

Compiled: 2026-08-02 · Reordered by relevance: 2026-08-21
Purpose: Reference collection for Q1 paper on vision-guided Multi-Objective Reinforcement Learning (MORL) with a UR5 in a dynamic environment.
Ordering: papers are listed most-relevant-first (closest to the vision × MORL × manipulation core), then less relevant.

## Papers / references (27 total)

### 1. Vision + MORL / vision-guided decision-making (core intersection)

| File | Paper | Venue | Why it matters |
|------|-------|-------|----------------|
| `references/11-demo-enhanced-morl-nav.md` | Demonstration-Enhanced Adaptable Multi-Objective Robot Navigation | IROS 2025 | Closest existing work: MORL + preference conditioning + dynamic λ adaptation + sim-to-real; lidar-based, no vision/manipulation — defines our gap. |
| `references/14-pareto-visual-navigation.md` | Navigating the Wild: Pareto-Optimal Visual Decision-Making in Image Space | arXiv 2511.07750 | Recent example of vision + Pareto-optimal decision-making; real-time; safety constraint template. |
| `references/15-dol.md` | Multi-Objective Deep RL (DOL) | — | First deep MORL; image-based testbed (ImageDeepSea). |
| `references/17-mo-vla.md` | MO-VLA (Multi-Objective VLA?) | — | Empty repo; watchlist for vision-language-action + MO. |

### 2. Vision-guided policy learning (manipulation / UR5)

| File | Paper | Venue | Why it matters |
|------|-------|-------|----------------|
| `references/07-diffusion-policy.md` | Diffusion Policy | RSS 2023 / IJRR 2024 | Core visuomotor diffusion baseline; tested on UR5 hardware; policy generator for our method. |
| `references/04-3d-diffusion-policy.md` | 3D Diffusion Policy (DP3) | RSS 2024 | Point-cloud visuomotor policy; data-efficient, safe, generalizable; our vision backbone. |
| `references/27-im2flow2act.md` | Flow as the Cross-domain Manipulation Interface (Im2Flow2Act) | CoRL 2024 | Object-flow interface bridging human/robot and real/sim; 81% avg success across rigid/articulated/deformable tasks. |
| `references/25-doughnet.md` | DoughNet | ECCV 2024 | Visual predictive model for topological manipulation of deformable objects from a single RGBD point cloud. |
| `references/05-aloha.md` | ALOHA / ACT | RSS 2023 | Action-chunking transformer baseline; multi-view RGB observation. |
| `references/06-mobile-aloha.md` | Mobile ALOHA / ACT++ | CoRL 2024 | Mobile manipulation; co-training; whole-body teleoperation. |
| `references/20-aloha-unleashed.md` | ALOHA Unleashed | — (DeepMind) | Large-scale teleop + Diffusion Policy recipe; bimanual 6-DoF arms. |
| `references/22-videomanip.md` | VideoManip | — | Device-free dexterous manipulation from RGB videos; uses DP3 policy. |

### 3. MORL theory, algorithms, and benchmarks

| File | Paper | Venue | Why it matters |
|------|-------|-------|----------------|
| `references/08-gcr-ppo.md` | Scalable MO Robot RL through Gradient Conflict Resolution | arXiv 2509.14816 | Multi-head critic + PCGrad surgery; IsaacLab robot RL at GPU scale; method backbone template. |
| `references/13-pgmorl.md` | Prediction-Guided MORL for Continuous Robot Control | ICML 2020 | Canonical evolutionary MORL; continuous Pareto front interpolation; baseline. |
| `references/16-envelope-morl.md` | A Generalized Algorithm for MORL and Policy Adaptation | NeurIPS 2019 | Envelope MOQ-learning; single model over all preferences. |
| `references/02-mo-mpo.md` | A Distributional View on Multi-Objective Policy Optimization | — | Scale-invariant MORL on high-DoF robots; strong baseline. |
| `references/10-prc-cmorl.md` | Personalized Robotic Control via Constrained MORL | Neurocomputing 2023 | Constrained MORL with preference conditioning; CMDP safety; hypervolume+entropy metrics. |
| `references/01-mo-gymnasium.md` | A Toolkit for Reliable Benchmarking and Research in MORL | NeurIPS 2023 | Standard MORL env suite + vector-reward API; benchmark target. |
| `references/12-morl-baselines.md` | MORL-Baselines | — | Library of MORL baselines following MO-Gymnasium API. |
| `references/09-mo-playground.md` | MO-Playground: Massively Parallelized MORL for Robotics | arXiv 2603.09237 | JAX-based parallel MORL envs; BRUCE humanoid 6-D objective space. |

### 4. Data collection / demonstration tools

| File | Paper | Venue | Why it matters |
|------|-------|-------|----------------|
| `references/24-umi.md` | Universal Manipulation Interface (UMI) | RSS 2024 | Hand-held gripper in-the-wild data collection; hardware-agnostic policies deployed on UR5e and Franka. |
| `references/03-iris-project.md` | IRIS: An Immersive Robot Interaction System | CoRL 2025 | XR teleop + point clouds across simulators and robots. |

### 5. Cross-embodiment datasets & generalist models

| File | Paper | Venue | Why it matters |
|------|-------|-------|----------------|
| `references/26-open-x-embodiment.md` | Open X-Embodiment / RT-X | arXiv 2310.08864 | 1M+ trajectories, 22 embodiments, 21 institutions; RT-X positive transfer; RT-2-X emergent skills. |

### 6. Vision-language models & affordance-driven manipulation

| File | Paper | Venue | Why it matters |
|------|-------|-------|----------------|
| `references/18-gpt4v-robot-manipulation.md` | GPT-4V(ision) for Robotics | — (Microsoft) | VLM multimodal task planning from human demonstration; zero-shot. |
| `references/21-manipulate-anything.md` | Manipulate-Anything | — | VLM-based automated data generation; zero-shot manipulation. |
| `references/23-vrb.md` | VRB: Affordances from Human Videos | CVPR 2023 | Affordance action parameterization for RL; human-video-driven learning. |
| `references/19-vision-tactile-manipulation.md` | Vision-based Tactile Manipulation | — | Tactile+vision fusion for soft/fragile object handling. |

## Suggested taxonomy for your literature review

1. Vision + Multi-Objective Decision-Making (your core intersection)
   - Closest work: Demonstration-Enhanced MO Navigation (IROS 2025).
   - DOL (image-based MORL, first deep MORL).
   - Pareto-Optimal Visual Navigation (arXiv 2511.07750).
   - MO-VLA (watchlist).
2. Vision-Guided Policy Learning (manipulation)
   - Diffusion Policy (RSS 2023 / IJRR 2024) — UR5 hardware.
   - 3D Diffusion Policy / DP3 (RSS 2024) — point clouds, data-efficient, safety.
   - Im2Flow2Act (CoRL 2024) — object flow as cross-domain interface (human/robot, real/sim).
   - DoughNet (ECCV 2024) — visual predictive model for topological manipulation of deformable objects.
   - ALOHA / ACT (RSS 2023); Mobile ALOHA / ACT++ (CoRL 2024).
   - ALOHA Unleashed (Diffusion Policy + large-scale teleop recipe).
   - VideoManip (RGB-video-driven dexterous manipulation, DP3 policy).
3. Multi-Objective RL
   - Foundations/MOMDPs: "A practical guide to MORL and planning" (referenced in MO-Gymnasium docs); "MORL Based on Decomposition: A Taxonomy and Framework" (JAIR).
   - Benchmarks/API: MO-Gymnasium (NeurIPS 2023); PGMORL benchmark (ICML 2020); MO-Playground (2026).
   - Multi-policy algorithms: Envelope MOQ-learning (NeurIPS 2019), PGMORL (ICML 2020), MORL/D, CAPQL, GPI-LS (all in MORL-Baselines), MO-MPO.
   - Constrained / safety-aware: PRC-CMORL (Neurocomputing 2023); GCR-PPO (2025).
   - Parallel/GPU-scale: GCR-PPO (IsaacLab), MO-Playground (JAX).
4. Vision-Language Models & Affordances for Manipulation
   - GPT-4V for Robotics (Microsoft) — VLM task planning from demos.
   - Manipulate-Anything — VLM automated data generation, zero-shot.
   - VRB (CVPR 2023) — affordances from human videos for RL.
   - Vision-based Tactile Manipulation — tactile+vision fusion.
   - Open X-Embodiment / RT-X (arXiv 2310.08864) — RT-2-X vision-language-action model over a cross-embodiment dataset.
5. Data Collection & Sim-to-Real Support
   - UMI (RSS 2024) — hand-held gripper in-the-wild data collection; hardware-agnostic policies.
   - IRIS (CoRL 2025) — XR teleop, point-cloud integration, multi-simulator.
   - Open X-Embodiment (arXiv 2310.08864) — 1M+ real robot trajectories across 22 embodiments.
   - Demo-enhanced MORL nav (IROS 2025) — sim-to-real on two robots.

## Reading order suggestion (most relevant first)

1. `11-demo-enhanced-morl-nav.md`, `08-gcr-ppo.md`, `07-diffusion-policy.md`, `04-3d-diffusion-policy.md` — the closest works and the vision/MO backbones of our method.
2. `14-pareto-visual-navigation.md`, `15-dol.md` — the vision+MORL intersection (your core novelty area).
3. `13-pgmorl.md`, `16-envelope-morl.md`, `02-mo-mpo.md`, `01-mo-gymnasium.md`, `12-morl-baselines.md` — core MORL baselines and benchmark API.
4. `10-prc-cmorl.md`, `09-mo-playground.md` — constrained/safety-aware and parallel MORL.
5. `27-im2flow2act.md`, `25-doughnet.md`, `24-umi.md`, `05-aloha.md`, `06-mobile-aloha.md`, `20-aloha-unleashed.md`, `22-videomanip.md` — vision-guided policy and data-collection tooling.
6. `26-open-x-embodiment.md`, `03-iris-project.md`, `18-gpt4v-robot-manipulation.md`, `21-manipulate-anything.md`, `23-vrb.md`, `19-vision-tactile-manipulation.md` — datasets, VLM/affordance-driven, and vision-tactile manipulation.

## Open gaps to fill next

- MORL applied specifically to robot manipulation with visual observations (few prior works) — this is likely your novelty gap; the closest works are the vision+MORL navigation papers.
- Safe/constrained MORL for dynamic environments (collision avoidance as an objective).
- Sim-to-real transfer for multi-objective visual policies on UR5.
- Survey of dynamic-environment manipulation (moving objects, human co-presence).
