# 12-Month Research Plan (Detailed, Paper-Validated)

**Title:** Vision-Guided Multi-Objective Reinforcement Learning (MORL) for UR5 Manipulation in Dynamic Environments
**Target Venue:** Q1 journal (IEEE TRO / IEEE RA-L); arXiv preprint first
**Method summary:** DP3 point-cloud encoder → multi-head MO critic (GCR-PPO-style asymmetric PCGrad) → preference-conditioned diffusion policy → UR5, with safety enforced as a CMDP constraint.
**Evaluation:** expected hypervolume, Pareto coverage/sparsity, success rate, safety-violation rate, sim-to-real transfer.
**Strategy:** **parallel sim + real tracks** — real UR5 + RGB-D camera brought up in Week 1 and run alongside simulation, so sim-to-real gaps surface early.

---

## Part A — Paper-to-Plan Validation

Every architectural choice below is grounded in a specific paper, its claimed numbers, and its code. This is the evidence base the plan is built on.

| # | Design decision | Source paper | Validation (evidence from the paper) |
|---|-----------------|--------------|--------------------------------------|
| 01 | Standard MO-Gymnasium vector-reward API + eval wrappers | Felten et al., NeurIPS 2023 | `env.step()` returns vector reward as numpy array; `LinearReward` wrapper for scalarization. Benchmark target for all MORL work. |
| 02 | MO-MPO noted as scale-invariance argument | Abdolmaleki et al., NeurIPS 2022 | Per-objective KL budgets ε_k make preference setting **scale-invariant** vs. weighted scalarization — the reason we cite it when justifying CMDP/normalized handling of heterogeneous UR5 objectives (success vs. energy vs. jerk). |
| 03 | IRIS as demo-collection option | Jiang et al., CoRL 2025 | XR teleop with real-time **point-cloud integration**, multi-simulator (LIBERO, Meta-World, RoboCasa), works across embodiments incl. ALOHA arms and real robots. Justifies point-cloud-native data collection. |
| 04 | **DP3 encoder** as vision backbone | Ze et al., RSS 2024 | Sparse point clouds, FPS to **512 or 1024 points**, DP3 encoder (no T-Net / no BatchNorm). **10-20 demos per task** (72 sim tasks; 85% real success with 40 demos). "Rarely violates safety" vs. image baselines. |
| 05 | ALOHA/ACT as imitation baseline | Zhao et al., RSS 2023 | Action-chunking CVAE (chunk=90 @50 Hz) reduces compounding error. Comparison baseline, not our backbone. |
| 06 | Mobile ALOHA / ACT++ as comparison | Fu et al., CoRL 2024 | Co-training static+mobile boosts success up to 90% with 50 demos. Shows demo scaling behavior we can reference in the data-budget discussion. |
| 07 | **Diffusion Policy** as action generator | Chi et al., RSS 2023 / IJRR 2024 | Receding-horizon control, **T_o=2, T_p=16, T_a=8**; CNN backbone first, switch to time-series diffusion transformer for fast-changing actions. **UR5 hardware**: EE-space positional commands @125 Hz interpolated from 10 Hz policy; velocity <0.43 m/s; ≥1 cm above table; 5× Realsense D415 @720p/30 fps (2 used, 320×240 @10 fps); SpaceMouse teleop @10 Hz. 46.9% avg improvement over prior SOTA on 12 tasks. |
| 08 | **Multi-head critic + asymmetric PCGrad** (MO backbone) | Munn et al., GCR-PPO, 2025 | IsaacLab 2.1.0 + RSL-RL; flags `--use_critic_multi --use_pcgrad`; `reward_component_names` / `reward_component_task_rew`. Multi-head critic `V_ϕ: S→R^K`, component-wise GAE, advantage normalization preserving ratios (global scale `C`), PCGrad with **task > regulariser priority**. ~4.55% avg return gain (up to 9.5% custom); 13 IsaacLab tasks + 2 custom MO benchmarks; >99% of added cost is gradient resolution. |
| 09 | MO-Playground as JAX alternative | Janwani et al., 2026 | Massively parallel JAX envs; BRUCE humanoid 6-D objective space (tracking vs. energy). Reference for parallel-scaling arguments; MORLAX if we pivot off IsaacLab. |
| 10 | **CMDP safety** (constraint, not objective) | He et al., PRC-CMORL, Neurocomputing 2023 | Constrained MOMDP with nonlinear constraints; single model spans whole preference space; **hypervolume + entropy index** for convergence/diversity/evenness. This is the metric set and constraint design we adopt. |
| 11 | **Preference conditioning** (interpolator + angle loss) | de Heuvel et al., IROS 2025 | PD-MORL: preference interpolator `I(λ)=λ^p` (power transform), **angle loss** `g(λ_p,Q)`, HER over preferences, `C_p` parallel envs; D-REX reward model from noise-injected BC (ε∈0-0.2, N_D=1000, Bradley-Terry). Dynamic preference adaptation **without retraining**; sim-to-real on two robots. The closest-work template for our thesis. |
| 12 | PGMORL baseline | Xu et al., ICML 2020 | Evolutionary MORL + prediction model (GP) + Pareto analysis + interpolation → continuous front; 7 continuous-control MO envs; hypervolume + sparsity metrics. Available in MORL-Baselines. |
| 13 | Envelope MOQ baseline | Yang et al., NeurIPS 2019 | Single `Q(s,a,ω)` over the whole preference space; envelope Bellman operator; homotopy loss `L=(1-λ)L_A+λL_B` annealed 0→1; HER over preferences. Available in MORL-Baselines. |
| 14 | **Safety constraint values** (velocity/workspace) | Chi et al. (DP, App. D) + Pushp et al. (POVNav) | DP UR5: EE velocity <0.43 m/s, position ≥1 cm above table. POVNav: explicit `d(s_t,O_t) ≥ D_safe` + velocity/accel bounds. These give concrete numeric CMDP thresholds for the UR5. |
| 15 | MO benchmark API + baselines library | Felten et al. + Alegre et al. | MO-Gymnasium envs + MORL-Baselines v1.1.0 (PGMORL, Envelope, CAPQL, MORL/D, PCN) — use directly, never reimplement. |
| 16 | Demo budget justification | Ze et al. (DP3) | 10-20 demos/task is enough for DP3-style training — sets the demo-collection budget in the plan. |
| 17 | Auto-generated demos fallback | Duan et al. (Manipulate-Anything), Bahl et al. (VRB) | VLM/affordance-driven data generation without teleop (Manipulate-Anything: zero-shot, no privileged state; VRB: affordance action parameterization for RL). Fallback if teleop is the bottleneck. |

### Novelty validation (gap check against the papers)

The intersection we claim — **vision + preference-conditioned MORL + manipulation + safety constraints + dynamic environments** — is checked against every paper read:

- MORL classics (PGMORL, Envelope, MO-MPO) are **proprioceptive, locomotion/classic-control**, no visual observations, no arm manipulation.
- Vision-guided manipulation (DP3, Diffusion Policy, ACT/ALOHA, ALOHA Unleashed) are **single-objective imitation/RL**, no preference conditioning.
- Robot MORL at scale (GCR-PPO) is **proprioceptive** (Humanoid/Throwing), no vision, no preference input.
- Constrained MORL (PRC-CMORL) is **proprioceptive MuJoCo locomotion** (MO-Swimmer/Hopper/…), no vision.
- Closest work (demo-enhanced MORL nav, IROS 2025) is **lidar navigation**, not vision-manipulation, and has **no explicit safety constraint** (collision is an objective). This is the explicit gap we fill.
- Vision+MORL intersections (POVNav) are **classical planning** (subgoal selection), not an end-to-end learned preference-conditioned policy.

**Conclusion:** no paper read combines all five capabilities; the plan's contribution claim is valid. Explicit citations for the gap argument: `11-demo-enhanced-morl-nav.md`, `14-pareto-visual-navigation.md`, `10-prc-cmorl.md`, `08-gcr-ppo.md`, `13-pgmorl.md`, `16-envelope-morl.md`, `04-3d-diffusion-policy.md`, `07-diffusion-policy.md`.

### Grounding evidence / expected numbers used in the plan

| Quantity | Paper value | Where the plan uses it |
|----------|-------------|------------------------|
| 10-20 demos per task | DP3 | Demo budget for BC warm-start (Months 1-2) |
| 85% real success w/ 40 demos | DP3 | Real-robot success target reference |
| 125 Hz / <0.43 m/s / ≥1 cm | Diffusion Policy (UR5 App. D) | Real control-loop + CMDP velocity/workspace bounds |
| T_o=2, T_p=16, T_a=8 | Diffusion Policy | Diffusion policy action-chunk defaults |
| ~4.55% avg gain (to 9.5%) | GCR-PPO | Expected magnitude of MO-critic gain over scalarized PPO |
| Hypervolume + entropy | PRC-CMORL | Primary Pareto-front metrics |
| ε∈0-0.2, N_D=1000, Bradley-Terry | Demo-enhanced MORL nav | D-REX reward model if demos are used as an objective |
| `d(s,O) ≥ D_safe` | POVNav | CMDP collision-avoidance threshold template |
| Vector-reward numpy API | MO-Gymnasium | Benchmark wrapper API |

---

## Part B — Execution Phases

## Phase 1: Foundation & Formulation (Months 1-2)

### Month 1 — Setup: Simulation and Real Hardware in Parallel

| Week | Simulation track | Real track | Deliverables |
|------|------------------|-----------|--------------|
| 1 | Finish the vision × MORL lit review (11, 14, 15, 17) and method backbones (08, 07, 04, 10). Install IsaacLab 2.1.0 + RSL-RL (GCR-PPO fork); load UR5 URDF + RGB-D camera; verify joint control, camera feed, rendering. | Install real UR5 + RGB-D camera; verify joint control, E-stop, collaborative safety mode. | Gap statement (S6) draft; simulator + real rig running |
| 2 | Implement camera-to-robot calibration (extrinsics) + coordinate transforms in sim. | Hand-eye calibration on the real rig; same transform convention. | Calibration scripts (sim + real), unified frames |
| 3 | Depth → point cloud (extrinsics), FPS to 512 points; DP3 encoder forward pass (04). | Validate real point clouds match sim format; record first real demos. | Point-cloud preprocessing script (sim + real) |
| 4 | Start teleop/demo tooling in sim (or scripted demos; IRIS 03 or Manipulate-Anything 21 style). | Test teleop (SpaceMouse @10 Hz per 07) on real UR5; collect first demo set (target 10-20 demos, 04). | Demo pipeline working on real UR5 |

**Validation checkpoints (M1):** (a) sim and real use identical point-cloud tensors (same FPS count, same frame); (b) ≥10 reach demos recorded on real hardware; (c) DP3 encoder encodes target position (probe with a small regression test).

### Month 2 — MOMDP Formulation & First Baselines

| Week | Simulation track | Real track | Deliverables |
|------|------------------|-----------|--------------|
| 1 | Formalize MOMDP: objectives (success, safety, energy, time), vector rewards (01), preference space Ω. Define CMDP safety constraints: collision `d≥D_safe` (14), velocity <0.43 m/s, workspace bounds (07). | Apply the same numeric safety limits on the real UR5 (workspace, velocity, E-stop). | MOMDP formulation (S10); real safety config |
| 2 | Implement DP3-style encoder + MLP actor; validate it encodes target pose. | Record structured real demo dataset (20-50 reach demos) for BC warm-start (04 budget). | Encoder training script; real demo dataset |
| 3 | Train single-objective visual PPO (DP3 + MLP, scalar reward) for static reaching in sim. | Build sim↔real bridge: replay real camera images in sim; log both streams in one format. | Baseline RL pipeline; unified data format |
| 4 | Tune PPO (reward shaping, LR, batch, episode length); target >90% success on static reaching. | **Deploy sim-trained baseline on real UR5** (static reaching; first sim-to-real test). | **M2: baseline visual PPO in sim AND first real deployment** |

---

## Phase 2: Vision-Based MO Benchmark (Months 3-4)

### Month 3 — MO Benchmark (Static), Sim + Real Parity

| Week | Simulation track | Real track | Deliverables |
|------|------------------|-----------|--------------|
| 1 | Design the vision-based MO UR5 benchmark: reaching with 3-4 objectives (success, energy, smoothness/jerk; safety as constraint per 10). | Mirror the task spec on the real rig (same targets, same metrics). | Benchmark task spec (sim + real) |
| 2 | Implement the 3 objective reward functions; validate each independently (each must be learnable by single-objective PPO). | Record real trajectories for the same tasks; compute objective metrics on real data. | Verified reward functions; real metric baseline |
| 3 | Add task variants: random targets, 1-2 static obstacles, clutter. | Replicate a static-obstacle subset on the real rig. | Static benchmark tasks (sim + real) |
| 4 | Wrap tasks in MO-Gymnasium-style vector-reward API (01); add eval harness: hypervolume, Pareto coverage/sparsity, entropy (10, 12, 13). | Run the harness on real recorded data to confirm metrics transfer. | Benchmark API + eval harness |

### Month 4 — MO Benchmark (Dynamic), Sim + Real

| Week | Simulation track | Real track | Deliverables |
|------|------------------|-----------|--------------|
| 1 | Add dynamic scenarios: moving obstacles (linear/waypoint/sinusoidal), moving target, sudden appearance. | Build real dynamic rigs: pendulum/moving-obstacle rig, handheld/conveyor moving target. | Dynamic benchmark tasks (sim + real rigs) |
| 2 | Add temporal reasoning: frame stacking (last 3-5 point-cloud frames) and/or a recurrent/GPT layer; compare variants (07 backbone notes). | Validate real dynamic scenes are safely scripted and repeatable. | Temporal input variant; safe real rigs |
| 3 | Learnability check: train single-objective PPO on each objective; verify each is learnable. | Collect real dynamic demos for later evaluation. | Learnability results; real dynamic demos |
| 4 | Freeze benchmark configs; document all task parameters + randomization. | **Deploy updated baseline to real UR5 on static + simple dynamic scenes.** | **M3: vision-based MO UR5 benchmark ready (sim + real), baseline redeployed on real** |

---

## Phase 3: MORL Algorithm Implementation (Months 5-6)

### Month 5 — MO Critic & Gradient Resolution

| Week | Simulation track | Real track | Deliverables |
|------|------------------|-----------|--------------|
| 1 | Implement multi-head critic `V_ϕ: S→R^K` + component-wise GAE (08). | Weekly regression: run latest sim policy on real static scenes. | Multi-head critic script; weekly real eval |
| 2 | Advantage normalization preserving inter-objective ratios: global scale `C`, not per-component standardization (08). | Instrument real logs with the same objective metrics to detect drift early. | Ratio-preserving normalized advantages; real metric dashboard |
| 3 | Asymmetric PCGrad with task > regulariser priority (08); reward components flagged via `reward_component_task_rew`. | Run sim policy on real rig; log failures (lighting, friction, latency). | Gradient-surgery pipeline; failure log |
| 4 | Compare multi-head critic + PCGrad vs. scalarized PPO (weight sampling) on static benchmark (expect ~4.55%-class gain, 08). | Tune domain randomization in sim to close real gaps found in the failure log. | Comparison results; improved DR config |

### Month 6 — Preference-Conditioned Diffusion Policy + CMDP

| Week | Simulation track | Real track | Deliverables |
|------|------------------|-----------|--------------|
| 1 | Preference conditioning: continuous λ input to policy; preference interpolator `I(λ)=λ^p` (power transform) + angle loss `g(λ_p,Q)` (11). | Validate a λ-sweep is feasible on the real robot (2-3 λ settings on static scenes). | Preference-conditioned policy; real λ sanity check |
| 2 | Replace MLP actor with preference-conditioned diffusion policy (07: CNN first, or time-series diffusion transformer for fast-changing actions; keep DP3 encoder). | Measure real-time inference latency of diffusion policy on the real control loop (target 125 Hz feasibility, 07). | Diffusion policy backbone; latency measured |
| 3 | Enforce safety as CMDP constraints: Lagrangian / constrained optimization (10) on collision `d≥D_safe` (14), velocity, workspace (07). | Validate CMDP safety limits on real robot (workspace + velocity bounds enforced). | CMDP safety layer; real safety validation |
| 4 | Integrate full pipeline: DP3 → multi-head critic → preference-conditioned diffusion policy → CMDP safety → UR5. | **Deploy full pipeline on real UR5 for static scenes; verify preference slider changes behavior on hardware.** | **M4: full MORL algorithm integrated AND deployed on real UR5 (static)** |

---

## Phase 4: Simulation Experiments & Ablations (Months 7-8)

### Month 7 — Main Experiments (Static + Dynamic)

| Week | Simulation track | Real track | Deliverables |
|------|------------------|-----------|--------------|
| 1 | Train preference-conditioned policy across the preference space; generate Pareto front (hypervolume, coverage, sparsity, entropy — 10). | Run a λ-sweep on real UR5 (3-5 preference settings × static tasks) to check front transfer. | Pareto front (sim); real front sanity check |
| 2 | Evaluate on dynamic benchmarks at various obstacle speeds; measure success + safety-violation rate. | Test real dynamic scenes with the deployed policy (pendulum, moving target). | Dynamic results (sim + real) |
| 3 | Test dynamic preference adaptation: λ drifting mid-episode → on-the-fly behavior change (11). | Validate on-the-fly λ change on real hardware (manual λ switch, safety-first). | Adaptation results (sim + real) |
| 4 | Compare vs. baselines: **PGMORL, Envelope MOQ (via MORL-Baselines 12), scalarized PPO, single-objective DP3/Diffusion Policy, GCR-PPO, MO-MPO (02), demo-enhanced MORL nav (11)**. | Run a subset of baselines on the real rig for head-to-head. | Baseline comparison tables (sim + real subset) |

### Month 8 — Ablations & Robustness

| Week | Simulation track | Real track | Deliverables |
|------|------------------|-----------|--------------|
| 1 | Ablate vision: DP3 point cloud vs. 2D RGB encoder vs. RGB-D (04 vs. 07). | Confirm best vision variant works on real camera images. | Vision ablation (sim + real confirm) |
| 2 | Ablate critic: multi-head + PCGrad vs. multi-head only vs. single-head scalarized (08). | Weekly real regression on the selected config. | Critic ablation |
| 3 | Ablate safety: CMDP constraint (10) vs. collision-as-objective (11/GCR-style). Ablate preference: fixed λ vs. conditioned (11). | Verify safety-ablation differences appear on real hardware (violation counts). | Safety & preference ablations (sim + real) |
| 4 | Domain randomization, robustness to camera perturbation; statistical significance (paired t-tests / bootstrap). | Finalize real experimental protocol; freeze all real-robot eval configs. | **M5: simulation experiments complete; real protocol frozen** |

---

## Phase 5: Real-UR5 Experiments & Sim-to-Real Analysis (Months 9-10)

### Month 9 — Systematic Real-Robot Experiments

| Week | Simulation track | Real track | Deliverables |
|------|------------------|-----------|--------------|
| 1 | Provide per-task sim references for gap computation. | Full real static experiment: all λ × all static tasks × ≥3 trials. | Real static Pareto data (full) |
| 2 | Provide matched sim counterparts for each real dynamic scene (same initializations). | Full real dynamic experiment: moving obstacles, moving target, sudden appearance. | Real dynamic data (full) |
| 3 | Generate matched sim↔real metric pairs. | Real dynamic preference-adaptation trials; measure safety-violation rate. | Real adaptation + safety data |
| 4 | Compile matched sim vs. real tables. | Consolidate all real-robot logs, videos, photos. | **M6: complete real-robot dataset (static + dynamic)** |

### Month 10 — Sim-to-Real Gap Analysis & Finalization

| Week | Simulation track | Real track | Deliverables |
|------|------------------|-----------|--------------|
| 1 | Compute sim-to-real gap per metric (success drop, hypervolume drop, violation-rate delta). | Re-run flagged real scenes to confirm gap causes (lighting/friction/latency). | Sim-to-real gap tables |
| 2 | Identify closable gaps via DR; targeted DR updates in sim. | Confirm updated policies still work on real hardware (quick re-test). | Improved sim-to-real results |
| 3 | Finalize results, figures, statistical tests. | Final real-robot photo/video shoot for the paper. | Final result set + media |
| 4 | Lock paper result tables (sim + real). | — | **M7: sim-to-real analysis complete, all results final** |

---

## Phase 6: Writing & Submission (Months 11-12)

### Month 11 — Drafting

| Week | Tasks | Deliverables |
|------|-------|-------------|
| 1 | Write Introduction + Related Work using the reordered lit-review taxonomy (`references/README.md`); foreground the novelty validation (Part A). | Sections 1-2 draft |
| 2 | Write Methodology: MOMDP formulation, DP3 encoder, multi-head MO critic + asymmetric PCGrad, preference-conditioned diffusion policy, CMDP safety, parallel sim-real protocol. | Section 3 draft |
| 3 | Write Experiments: setup, baselines, metrics, results (sim static, sim dynamic, real). | Section 4 draft |
| 4 | Write Discussion + Conclusion (limitations, future work). First full draft. | Full paper draft |

### Month 12 — Polish & Submission

| Week | Tasks | Deliverables |
|------|-------|-------------|
| 1 | Create figures: Pareto front plots, training curves, trajectory visualizations, real-robot photos. | Figure set complete |
| 2 | Record supplementary video: side-by-side preference settings on real UR5. | Video ready |
| 3 | Internal review; revisions; format for target venue. | Revised camera-ready draft |
| 4 | Proofread, BibTeX, cover letter; post arXiv; submit to Q1 journal. | **Paper submitted** |

---

## Summary Milestones

| Month | Milestone |
|-------|-----------|
| 2 | Baseline visual PPO working in sim AND first real-robot deployment |
| 4 | Vision-based MO UR5 benchmark ready (sim + real) |
| 6 | Full MORL algorithm integrated AND deployed on real UR5 (static) |
| 8 | Simulation experiments + ablations complete; real protocol frozen |
| 10 | Complete real-robot dataset + sim-to-real gap analysis |
| 12 | Q1 journal paper submitted |

## Venue Deadlines (approximate, Q1 journals)

| Venue | Typical Deadline | Decision |
|-------|------------------|----------|
| **arXiv** | Anytime | — |
| **IEEE TRO** | Rolling | 4-6 months |
| **IEEE RA-L** | Rolling (monthly) | 3-4 months |
| **IJRR** | Rolling | 6-8 months |
