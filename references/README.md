# Literature Index — Vision-guided MORL using UR5 in Dynamic Environments

Compiled: 2026-08-02
Purpose: Reference collection for Q1 paper on vision-guided Multi-Objective Reinforcement Learning (MORL) with a UR5 in a dynamic environment.

## How the papers map to your topic

| File | Paper | Category | Why it matters for your thesis |
|------|-------|----------|-------------------------------|
| `references/01-mo-gymnasium.md` | A Toolkit for Reliable Benchmarking and Research in MORL (NeurIPS 2023) | MORL benchmark/API | Standard MORL env suite + vector-reward API; use for benchmarking your algorithm. |
| `references/02-mo-mpo.md` | A Distributional View on Multi-Objective Policy Optimization | MORL algorithm | Scale-invariant multi-objective policy optimization on high-DoF robots; strong baseline. |
| `references/03-iris-project.md` | IRIS: An Immersive Robot Interaction System (CoRL 2025) | Data collection / teleop | XR-based demo collection + point clouds across simulators and robots; pipeline support. |
| `references/04-3d-diffusion-policy.md` | 3D Diffusion Policy (RSS 2024) | Vision-guided policy | Point-cloud visuomotor policy; data-efficient, safety-aware, generalizable; candidate policy module. |
| `references/05-aloha.md` | ALOHA: Learning Fine-Grained Bimanual Manipulation (RSS 2023) | Imitation learning | ACT action-chunking baseline; multi-view RGB observation pattern. |
| `references/06-mobile-aloha.md` | Mobile ALOHA (CoRL 2024) | Imitation learning / mobile manipulation | ACT++ backbone; co-training; whole-body teleoperation for dynamic tasks. |
| `references/07-diffusion-policy.md` | Diffusion Policy (RSS 2023 / IJRR 2024) | Vision-guided policy | Core visuomotor diffusion baseline, tested on UR5 hardware; receding-horizon control. |

## Suggested taxonomy for your literature review

1. Multi-Objective RL
   - Foundations/MOMDPs: A practical guide to MORL and planning (referenced in MO-Gymnasium docs).
   - Benchmarks/API: MO-Gymnasium (Felten et al., NeurIPS 2023).
   - Algorithms: MO-MPO; other value-based (ENVIL, PGMORL, multi-policy) — expand via MO-Gymnasium publications list.

2. Vision-Guided Policy Learning (manipulation)
   - Diffusion Policy (Chi et al., RSS 2023 / IJRR 2024) — UR5 hardware, 4 cameras.
   - 3D Diffusion Policy / DP3 (Ze et al., RSS 2024) — point clouds, data-efficient, safety.
   - ALOHA / ACT (Zhao et al., RSS 2023); Mobile ALOHA / ACT++ (Fu et al., CoRL 2024).

3. Data Collection & Sim-to-Real Support
   - IRIS (Jiang et al., CoRL 2025) — XR teleop, point-cloud integration, multi-simulator.

## Reading order suggestion

1. `01-mo-gymnasium.md` + `02-mo-mpo.md` — establish the MORL side.
2. `07-diffusion-policy.md` + `04-3d-diffusion-policy.md` — establish the vision-guided policy side.
3. `05-aloha.md` + `06-mobile-aloha.md` — baselines/backbones for visuomotor imitation.
4. `03-iris-project.md` — data-collection tooling for your UR5 experiments.

## Open gaps to fill next

- MORL applied specifically to robot manipulation with visual observations (few prior works) — this is likely your novelty gap.
- Safe/constrained MORL for dynamic environments (collision avoidance as an objective).
- Sim-to-real transfer for multi-objective visual policies on UR5.
- Survey of dynamic-environment manipulation (moving objects, human co-presence).
