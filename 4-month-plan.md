# 4-Month Fast-Track Plan (Detailed, Paper-Validated)

**Title:** Vision-Guided Multi-Objective RL (MORL) for UR5 Manipulation in Dynamic Environments
**Target Venue:** IEEE RA-L (rolling monthly, 3-4 month decisions) + arXiv preprint first
**Method:** DP3 point-cloud encoder → multi-head MO critic (GCR-PPO-style asymmetric PCGrad) → preference-conditioned diffusion policy → UR5, safety as CMDP constraint.
**Strategy:** Keep the **parallel sim + real** tracks, compressed by reusing existing code, cutting ablations to 2, cutting baselines to 3, and writing continuously.

---

## Part A — Paper-to-Plan Validation

The compressed plan keeps only decisions with a paper-grounded basis (see `12-month-plan.md` Part A for the full 17-row table). The rows that matter for this compressed version:

| Design decision | Source paper | Evidence used |
|-----------------|--------------|---------------|
| DP3 point-cloud encoder + FPS to 512 pts, **10-20 demos** budget | Ze et al., RSS 2024 | 72 sim tasks w/ 10 demos; 85% real success w/ 40; safety rarely violated |
| Multi-head critic + component-wise GAE + ratio-preserving advantage normalization + asymmetric PCGrad (task > regulariser) | Munn et al., GCR-PPO, 2025 | IsaacLab/RSL-RL; `--use_critic_multi --use_pcgrad`; ~4.55% avg gain (to 9.5% custom) |
| Preference conditioning: power-transform interpolator `I(λ)=λ^p` + angle loss + HER over preferences | de Heuvel et al., IROS 2025 | PD-MORL core; dynamic preference adaptation without retraining; sim-to-real on 2 robots |
| CMDP safety: nonlinear constraints; **hypervolume + entropy** metrics | He et al., PRC-CMORL, Neurocomputing 2023 | CMOMDP; single model over full preference space; the metric set for Pareto evaluation |
| Diffusion policy action generator; receding horizon | Chi et al., RSS 2023 / IJRR 2024 | T_o=2, T_p=16, T_a=8; CNN first, time-series transformer for fast-changing actions; UR5 EE-space @125 Hz, <0.43 m/s, ≥1 cm above table |
| Safety threshold values (collision `d≥D_safe`, velocity/workspace) | Chi et al. (App. D) + Pushp et al., 2025 | Concrete numeric CMDP bounds for the UR5 |
| Baselines: PGMORL + Envelope (never reimplement) | Xu et al. ICML 2020; Yang et al. NeurIPS 2019; MORL-Baselines | Both available in MORL-Baselines v1.1.0; single-objective DP3 as the vision baseline |
| Benchmark API: vector-reward numpy | Felten et al., NeurIPS 2023 | `env.step()` returns vector reward; `LinearReward` wrapper |
| Auto-generated demos fallback | Duan et al. (Manipulate-Anything); Bahl et al. (VRB) | Zero-shot VLM data generation; affordance action parameterization for RL |
| Novelty gap | All papers (see 12-month-plan Part A) | No paper combines vision + preference-conditioned MORL + manipulation + CMDP safety + dynamic envs |

### Validation of the compressed scope

- **Cut ablations 4 → 2** (preference-conditioned vs fixed λ; CMDP vs collision-as-objective): these are the only two that directly test the thesis novelty claims (RQ3/RQ4). The vision ablation (DP3 vs RGB) is deferred to future work because DP3's superiority over RGB is already established in 04 (24.2% relative gain, safety).
- **Cut baselines 7 → 3** (PGMORL, Envelope, single-objective DP3; plus scalarized-PPO internal): PGMORL + Envelope are the two canonical multi-policy MORL algorithms and are one-command runs via MORL-Baselines (12). DP3 is the natural single-objective vision baseline (same encoder, so differences isolate the MORL contribution). MO-MPO, GCR-PPO, demo-enhanced MORL nav are moved to Related Work / discussion citations rather than reproduced baselines — justifiable because they are proprioceptive/lidar and not directly comparable on a vision-manipulation benchmark.
- **Cut benchmark 2-3 tasks**: reaching with static + dynamic variants (moving obstacle, moving target) is sufficient to demonstrate the five claimed capabilities; PGMORL's own benchmark is 7 locomotion envs and DP3's are 4 tasks — task count is not the contribution.

---

## Part B — Execution Phases

## Month 1 — Foundation + First Sim-to-Real Baseline

| Week | Simulation track | Real track | Deliverable / validation |
|------|------------------|-----------|--------------------------|
| 1 | Clone GCR-PPO/IsaacLab fork (08); load UR5 URDF + RGB-D camera; verify joint control + rendering. | Bring up real UR5 + RGB-D; verify joint control, E-stop. | Sim + real running. **Check:** same control convention both sides |
| 2 | Camera-to-robot calibration (extrinsics) + coordinate transforms. | Hand-eye calibration on the real rig. | Calibration scripts (sim + real). **Check:** a point projected in sim lands at the same real-world pose |
| 3 | Depth → point cloud + FPS to 512 pts; DP3 encoder forward pass (04). | Validate real point clouds match sim format. | Point-cloud pipeline (sim + real). **Check:** identical tensor shape + frame convention |
| 4 | Train single-objective visual PPO (DP3 + MLP) on static reaching; >90% success. | **Deploy sim policy on real UR5** (static reaching, first sim-to-real test). | **M1: baseline visual PPO in sim AND first real deployment.** **Check:** real success >0% (any functioning closed loop) + record gap causes |

## Month 2 — MO Benchmark + MO Critic

| Week | Simulation track | Real track | Deliverable / validation |
|------|------------------|-----------|--------------------------|
| 1 | Define MOMDP: 3-4 objectives (success, energy, smoothness/jerk), vector rewards (01), preference space Ω, CMDP safety constraints with paper values (`d≥D_safe` 14; <0.43 m/s, workspace 07). | Mirror task spec + safety limits on real rig. | MOMDP formulation. **Check:** objective definitions match how each paper computes its metric |
| 2 | Implement reward functions; validate each learnable by single-objective PPO; add dynamic variants (moving obstacle, moving target). | Record real trajectories for the same tasks. | Verified rewards + dynamic benchmark tasks |
| 3 | Multi-head critic (per-objective value heads) + component-wise GAE + ratio-preserving advantage normalization (08). | Weekly regression: run latest sim policy on real static scenes. | Multi-head critic script; real eval loop. **Check:** multi-head critic loss converges per objective |
| 4 | Asymmetric PCGrad (task > regulariser priority, 08); compare vs scalarized-PPO on static benchmark. | Real metric dashboard (drift detection). | **M2: MO benchmark ready + MO critic integrated.** **Check:** MO-critic ≥ scalarized PPO on hypervolume (target ~4.55% class gain, 08) |

## Month 3 — Preference-Conditioned Diffusion Policy + CMDP + Full Pipeline

| Week | Simulation track | Real track | Deliverable / validation |
|------|------------------|-----------|--------------------------|
| 1 | Preference conditioning: continuous λ input + power-transform interpolator `I(λ)=λ^p` + angle loss `g(λ_p,Q)` (11). | Test 2-3 λ settings on real static scenes. | Preference-conditioned policy; real λ sanity check. **Check:** different λ produce measurably different behavior |
| 2 | Replace MLP actor with preference-conditioned diffusion policy (07: CNN first; keep DP3 encoder; receding horizon T_o=2/T_p=16/T_a=8). | Measure real-time inference latency on the control loop. | Diffusion backbone; latency measured. **Check:** inference fits the control budget or plan to 10 Hz + interpolation (07) |
| 3 | CMDP safety layer: Lagrangian / constrained optimization (10) on collision/velocity/workspace (values from 14 + 07). | Validate workspace + velocity safety bounds on real UR5. | CMDP safety layer; real safety validation. **Check:** zero constraint violations on the validation set |
| 4 | Integrate full pipeline: DP3 → multi-head critic → conditioned diffusion → CMDP → UR5. | **Deploy full pipeline on real UR5 (static); verify λ slider changes behavior.** | **M3: full MORL algorithm integrated AND deployed on real (static)** |

## Month 4 — Experiments, 2 Ablations, Sim-to-Real, Write, Submit

| Week | Tasks | Deliverable / validation |
|------|-------|--------------------------|
| 1 | Train across preference space → Pareto front (hypervolume, coverage, sparsity, entropy — 10); evaluate dynamic benchmarks; baselines: PGMORL + Envelope (MORL-Baselines 12), single-objective DP3 (04). | Main results tables (sim). **Check:** our front dominates or matches PGMORL/Envelope on hypervolume; beats DP3 on multi-objective metrics |
| 2 | Two ablations: (1) preference-conditioned vs fixed λ (11); (2) CMDP constraint vs collision-as-objective (10 vs 11). Real λ-sweep (3-5 settings × static tasks). | Ablation results + real front sanity check. **Check:** conditioned λ ↑ hypervolume; CMDP ↓ violation rate without killing success |
| 3 | Real dynamic trials; matched sim-to-real gap tables (success drop, violation-rate delta). Draft the full paper in parallel (Intro/Related Work/Method/Experiments). | Sim-to-real gap tables + full draft |
| 4 | Figures, supplementary video (preference comparison on real UR5), internal review, formatting, post **arXiv**, submit **IEEE RA-L**. | **M4: paper submitted** |

---

## Summary Milestones

| Month | Milestone |
|-------|-----------|
| 1 | Baseline visual PPO working in sim AND first real-robot deployment |
| 2 | Vision-based MO benchmark (dynamic) + multi-head MO critic integrated |
| 3 | Full preference-conditioned MORL + CMDP deployed on real UR5 (static) |
| 4 | Experiments + 2 ablations + sim-to-real analysis + paper submitted (RA-L / arXiv) |

## Risk Guardrails

- **Hardware stalls >2 weeks:** fall back to sim-only for the paper; real UR5 becomes a supplementary demo. Do not block submission.
- **Diffusion integration slips:** ship the MLP actor first (Month 2 critic already works with it), then swap in diffusion as a variant. MLP version can be the main result.
- **Demo/teleop bottleneck:** use scripted / auto-generated trajectories (Manipulate-Anything 21 / VRB 23 style). Do not build a teleop rig.
- **Never reimplement baselines:** PGMORL + Envelope run via MORL-Baselines (12) with default configs; tune only if numbers look broken.
- **Metric priority:** hypervolume (10) is the primary metric; if GPU time is short, drop sparsity/entropy and keep hypervolume + safety-violation rate.

## Venue Deadline

| Venue | Typical Deadline | Decision | Notes |
|-------|------------------|----------|-------|
| **arXiv** | Anytime | — | Post first; establishes priority |
| **IEEE RA-L** | Rolling (monthly) | 3-4 months | Primary fast-track target |
