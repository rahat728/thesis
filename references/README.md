# Literature Index — Vision-guided MORL using UR5 in Dynamic Environments

Compiled: 2026-08-02
Purpose: Reference collection for Q1 paper on vision-guided Multi-Objective Reinforcement Learning (MORL) with a UR5 in a dynamic environment.

## Papers / references (23 total)

### MORL theory, algorithms, and benchmarks

| File | Paper | Venue | Why it matters for your thesis |
|------|-------|-------|-------------------------------|
| `references/01-mo-gymnasium.md` | A Toolkit for Reliable Benchmarking and Research in MORL | NeurIPS 2023 | Standard MORL env suite + vector-reward API; benchmark target. |
| `references/02-mo-mpo.md` | A Distributional View on Multi-Objective Policy Optimization | — | Scale-invariant MORL on high-DoF robots; strong baseline. |
| `references/08-gcr-ppo.md` | Scalable MO Robot RL through Gradient Conflict Resolution | arXiv 2509.14816 | Multi-head critic + PCGrad surgery; IsaacLab robot RL at GPU scale. |
| `references/09-mo-playground.md` | MO-Playground: Massively Parallelized MORL for Robotics | arXiv 2603.09237 | JAX-based parallel MORL envs; BRUCE humanoid 6-D objective space. |
| `references/10-prc-cmorl.md` | Personalized Robotic Control via Constrained MORL | Neurocomputing 2023 | Constrained MORL with preference conditioning; hypervolume+entropy metrics. |
| `references/12-morl-baselines.md` | MORL-Baselines | — | Library of MORL baselines following MO-Gymnasium API. |
| `references/13-pgmorl.md` | Prediction-Guided MORL for Continuous Robot Control | ICML 2020 | Canonical evolutionary MORL; continuous Pareto front interpolation. |
| `references/15-dol.md` | Multi-Objective Deep RL (DOL) | — | First deep MORL; image-based testbed (ImageDeepSea). |
| `references/16-envelope-morl.md` | A Generalized Algorithm for MORL and Policy Adaptation | NeurIPS 2019 | Envelope MOQ-learning; single model over all preferences. |

### Vision + MORL / vision-guided decision-making

| File | Paper | Venue | Why it matters |
|------|-------|-------|----------------|
| `references/14-pareto-visual-navigation.md` | Navigating the Wild: Pareto-Optimal Visual Decision-Making in Image Space | arXiv 2511.07750 | Recent example of vision + Pareto-optimal decision-making; real-time. |
| `references/11-demo-enhanced-morl-nav.md` | Demonstration-Enhanced Adaptable Multi-Objective Robot Navigation | IROS 2025 | MORL + preference conditioning + dynamic adaptation; sim-to-real on two robots. |
| `references/17-mo-vla.md` | MO-VLA (Multi-Objective VLA?) | — | Empty repo; watchlist for vision-language-action + MO. |

### Vision-guided policy learning (manipulation / UR5)

| File | Paper | Venue | Why it matters |
|------|-------|-------|----------------|
| `references/04-3d-diffusion-policy.md` | 3D Diffusion Policy (DP3) | RSS 2024 | Point-cloud visuomotor policy; data-efficient, safe, generalizable. |
| `references/05-aloha.md` | ALOHA / ACT | RSS 2023 | Action-chunking transformer baseline; multi-view RGB observation. |
| `references/06-mobile-aloha.md` | Mobile ALOHA / ACT++ | CoRL 2024 | Mobile manipulation; co-training; whole-body teleoperation. |
| `references/07-diffusion-policy.md` | Diffusion Policy | RSS 2023 / IJRR 2024 | Core visuomotor diffusion baseline; tested on UR5 hardware. |
| `references/20-aloha-unleashed.md` | ALOHA Unleashed | — (DeepMind) | Large-scale teleop + Diffusion Policy recipe; bimanual 6-DoF arms. |
| `references/22-videomanip.md` | VideoManip | — | Device-free dexterous manipulation from RGB videos; uses DP3 policy. |

### Vision-language models & affordance-driven manipulation

| File | Paper | Venue | Why it matters |
|------|-------|-------|----------------|
| `references/18-gpt4v-robot-manipulation.md` | GPT-4V(ision) for Robotics | — (Microsoft) | VLM multimodal task planning from human demonstration; zero-shot. |
| `references/21-manipulate-anything.md` | Manipulate-Anything | — | VLM-based automated data generation; zero-shot manipulation. |
| `references/23-vrb.md` | VRB: Affordances from Human Videos | CVPR 2023 | Affordance action parameterization for RL; human-video-driven learning. |
| `references/19-vision-tactile-manipulation.md` | Vision-based Tactile Manipulation | — | Tactile+vision fusion for soft/fragile object handling. |

### Data collection / demonstration tools

| File | Paper | Venue | Why it matters |
|------|-------|-------|----------------|
| `references/03-iris-project.md` | IRIS: An Immersive Robot Interaction System | CoRL 2025 | XR teleop + point clouds across simulators and robots. |

## Suggested taxonomy for your literature review

1. Multi-Objective RL
   - Foundations/MOMDPs: "A practical guide to MORL and planning" (referenced in MO-Gymnasium docs); "MORL Based on Decomposition: A Taxonomy and Framework" (JAIR).
   - Benchmarks/API: MO-Gymnasium (NeurIPS 2023); PGMORL benchmark (ICML 2020); MO-Playground (2026).
   - Multi-policy algorithms: Envelope MOQ-learning (NeurIPS 2019), PGMORL (ICML 2020), MORL/D, CAPQL, GPI-LS (all in MORL-Baselines), MO-MPO.
   - Constrained / safety-aware: PRC-CMORL (Neurocomputing 2023); GCR-PPO (2025).
   - Parallel/GPU-scale: GCR-PPO (IsaacLab), MO-Playground (JAX).

2. Vision-Guided Policy Learning (manipulation)
   - Diffusion Policy (RSS 2023 / IJRR 2024) — UR5 hardware.
   - 3D Diffusion Policy / DP3 (RSS 2024) — point clouds, data-efficient, safety.
   - ALOHA / ACT (RSS 2023); Mobile ALOHA / ACT++ (CoRL 2024).
   - ALOHA Unleashed (Diffusion Policy + large-scale teleop recipe).
   - VideoManip (RGB-video-driven dexterous manipulation, DP3 policy).

3. Vision-Language Models & Affordances for Manipulation
   - GPT-4V for Robotics (Microsoft) — VLM task planning from demos.
   - Manipulate-Anything — VLM automated data generation, zero-shot.
   - VRB (CVPR 2023) — affordances from human videos for RL.
   - Vision-based Tactile Manipulation — tactile+vision fusion.

4. Vision + Multi-Objective Decision-Making (your core intersection)
   - DOL (image-based MORL, first deep MORL).
   - Pareto-Optimal Visual Navigation (arXiv 2511.07750).
   - Demonstration-Enhanced Adaptable MO Robot Navigation (IROS 2025).
   - MO-VLA (watchlist).

5. Data Collection & Sim-to-Real Support
   - IRIS (CoRL 2025) — XR teleop, point-cloud integration, multi-simulator.
   - Demo-enhanced MORL nav (IROS 2025) — sim-to-real on two robots.

## Reading order suggestion

1. `01-mo-gymnasium.md`, `12-morl-baselines.md`, `13-pgmorl.md`, `16-envelope-morl.md` — establish the MORL side and baselines.
2. `07-diffusion-policy.md`, `04-3d-diffusion-policy.md`, `20-aloha-unleashed.md` — establish the vision-guided policy side.
3. `10-prc-cmorl.md`, `08-gcr-ppo.md`, `09-mo-playground.md` — recent robot-focused MORL methods (parallel scale, constraints).
4. `14-pareto-visual-navigation.md`, `11-demo-enhanced-morl-nav.md`, `15-dol.md` — the vision+MORL intersection (your core novelty area).
5. `18-gpt4v-robot-manipulation.md`, `21-manipulate-anything.md`, `23-vrb.md`, `22-videomanip.md` — VLM/affordance-driven and video-driven manipulation.
6. `05-aloha.md`, `06-mobile-aloha.md`, `03-iris-project.md`, `19-vision-tactile-manipulation.md` — baselines and data-collection tooling.

## Open gaps to fill next

- MORL applied specifically to robot manipulation with visual observations (few prior works) — this is likely your novelty gap; the closest works are the vision+MORL navigation papers.
- Safe/constrained MORL for dynamic environments (collision avoidance as an objective).
- Sim-to-real transfer for multi-objective visual policies on UR5.
- Survey of dynamic-environment manipulation (moving objects, human co-presence).
