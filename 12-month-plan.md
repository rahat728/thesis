# 12-Month Research Plan

**Title:** Vision-Guided Multi-Objective Reinforcement Learning (MORL) for UR5 Manipulation in Dynamic Environments
**Target Venue:** Q1 journal (e.g., IEEE TRO / IEEE RA-L)
**Method summary:** DP3 point-cloud encoder → multi-head MO critic (GCR-PPO-style asymmetric PCGrad) → preference-conditioned diffusion policy → UR5, with safety enforced as a CMDP constraint.
**Evaluation:** expected hypervolume, Pareto coverage/sparsity, success rate, safety-violation rate, sim-to-real transfer.
**Strategy:** **parallel sim + real tracks** — real UR5 + camera are brought up in Week 1 and run alongside simulation throughout, so sim-to-real gaps surface early and policies deploy incrementally rather than all at once at the end.

---

## Phase 1: Foundation & Formulation (Months 1-2)

### Month 1 — Setup: Simulation and Real Hardware in Parallel

| Week | Simulation track | Real-hardware track | Deliverables |
|------|------------------|---------------------|--------------|
| 1 | Finish literature review on the vision × MORL intersection (demo-enhanced MORL nav, POVNav, DOL) and method backbones (GCR-PPO, Diffusion Policy, DP3, PRC-CMORL). | Install IsaacLab with UR5 URDF + RGB-D camera. Verify joint control, camera feed, rendering. | Gap statement (S6) draft; simulator running |
| 2 | Implement camera-to-robot calibration (extrinsic) and coordinate transforms in sim. | Set up **real** UR5 + RGB-D camera; verify joint control; run built-in calibration / hand-eye calibration. | Calibration scripts (sim + real) |
| 3 | Build point-cloud pipeline: depth → point cloud (extrinsics), Farthest Point Sampling to 512–1024 points. | Validate real point-cloud capture matches sim format; record initial real demos for data-collection tooling. | Point-cloud preprocessing script (sim + real) |
| 4 | Start teleoperation/demo tooling in sim (or scripted demos). | Test teleop (SpaceMouse / IRIS-style / UMI) on real UR5; collect first demo set. | Demo pipeline working on real UR5 |

### Month 2 — MOMDP Formulation & First Baselines (Sim + Real)

| Week | Simulation track | Real-hardware track | Deliverables |
|------|------------------|---------------------|--------------|
| 1 | Formalize MOMDP: objectives (success, safety, energy, time), vector rewards, preference space Ω. Define CMDP safety constraints (collision, velocity, workspace bounds). | Establish real-robot safety policies: velocity/workspace limits, E-stop, collaborative safety settings (same values as sim CMDP). | MOMDP formulation (S10); real safety config |
| 2 | Implement DP3-style point-cloud encoder + MLP/action output. Validate it encodes target position. | Record structured real demo dataset (e.g., 20-50 reaching demos) for later BC warm-start. | Encoder training script; real demo dataset |
| 3 | Implement single-objective visual PPO baseline (DP3 + MLP actor) for static reaching in sim. | Build sim↔real bridge: replay real camera images in sim; log both streams in the same format. | Baseline RL pipeline; unified data format |
| 4 | Tune PPO (reward shaping, LR, batch, episode length). Target >90% success on static reaching in sim. | **Deploy the sim-trained baseline policy on the real UR5** for static reaching (first sim-to-real test, even if imperfect). | **Milestone: baseline visual PPO working in sim AND first real-robot deployment** |

---

## Phase 2: Vision-Based MO Benchmark (Months 3-4)

### Month 3 — MO Benchmark (Static), Sim + Real Parity

| Week | Simulation track | Real-hardware track | Deliverables |
|------|------------------|---------------------|--------------|
| 1 | Design vision-based multi-objective UR5 benchmark suite: reaching with 3-4 objectives (success, energy, smoothness/jerk; safety as constraint). | Mirror the benchmark task spec on the real rig (same targets, same metrics). | Benchmark task spec (sim + real) |
| 2 | Implement 3 objective reward functions; validate each independently in sim. | Record real trajectories for the same tasks to compute objective metrics on real data. | Verified reward functions; real metric baseline |
| 3 | Add task variants: random targets, 1-2 static obstacles, clutter. | Replicate a subset of static obstacle scenes on the real rig. | Static benchmark tasks (sim + real) |
| 4 | Wrap tasks in MO-Gymnasium-style vector-reward API; add evaluation harness (hypervolume, coverage, sparsity). | Run the harness on real recorded data to confirm metrics transfer. | Benchmark API + eval harness |

### Month 4 — MO Benchmark (Dynamic), Sim + Real

| Week | Simulation track | Real-hardware track | Deliverables |
|------|------------------|---------------------|--------------|
| 1 | Add dynamic scenarios in sim: moving obstacles (linear, waypoint, sinusoidal), moving target, sudden appearance. | Build real dynamic rigs: pendulum/moving obstacle rig, conveyor or handheld moving target. | Dynamic benchmark tasks (sim + real rigs) |
| 2 | Add frame stacking (last 3-5 point-cloud frames) and/or recurrent layer for temporal reasoning. Compare variants. | Validate real dynamic scenes are safely scripted and repeatable. | Temporal input variant; safe real rigs |
| 3 | Sanity-check benchmark learnability: train single-objective PPO on each objective; verify each is learnable. | Collect real dynamic demos for later evaluation. | Learnability check results; real dynamic demos |
| 4 | Freeze benchmark configs; document all task parameters and randomization. | **Deploy updated baseline to real UR5 on static + simple dynamic scenes.** | **Milestone: vision-based MO UR5 benchmark ready (sim + real), baseline re-deployed on real** |

---

## Phase 3: MORL Algorithm Implementation (Months 5-6)

### Month 5 — MO Critic & Gradient Resolution

| Week | Simulation track | Real-hardware track | Deliverables |
|------|------------------|---------------------|--------------|
| 1 | Implement multi-head critic (per-objective value heads) + component-wise GAE (GCR-PPO style). | Continue periodic real evaluation of latest sim policy on static scenes (weekly regression check). | Multi-head critic script; weekly real eval |
| 2 | Implement advantage normalization preserving inter-objective ratios (global scale, not per-component standardization). | Instrument real logs with the same objective metrics to detect metric drift early. | Normalized advantages; real metric dashboard |
| 3 | Implement asymmetric PCGrad with task > regulariser priority. Ablate on benchmark. | Run sim policy on real rig; log where it fails (lighting, friction, latency). | Gradient-surgery pipeline; failure log |
| 4 | Compare multi-head critic + PCGrad vs. scalarized PPO (weight sampling) on static benchmark. | Tune domain randomization in sim to close gaps found in the failure log. | Comparison results; improved DR config |

### Month 6 — Preference-Conditioned Diffusion Policy + CMDP

| Week | Simulation track | Real-hardware track | Deliverables |
|------|------------------|---------------------|--------------|
| 1 | Implement preference conditioning: continuous λ input to the policy; preference interpolator (power-transform) + angle loss (PD-MORL-style). | Validate λ-sweep is feasible on real robot (test 2-3 λ settings on static scenes). | Preference-conditioned policy; real λ sanity check |
| 2 | Replace MLP actor with preference-conditioned diffusion policy (time-series diffusion transformer); keep DP3 encoder. | Test real-time inference latency of diffusion policy on the real control loop. | Diffusion policy backbone; latency measured |
| 3 | Enforce safety as CMDP constraints: Lagrangian / constrained-optimization (PRC-CMORL style) on collision, velocity, workspace. | Validate CMDP-safety limits on real robot (workspace + velocity bounds enforced). | CMDP safety layer; real safety validation |
| 4 | Integrate full pipeline: DP3 → multi-head critic → preference-conditioned diffusion policy → CMDP safety → UR5. | **Deploy full pipeline on real UR5 for static scenes; verify preference slider changes behavior on hardware.** | **Milestone: full MORL algorithm integrated and deployed on real UR5 (static)** |

---

## Phase 4: Simulation Experiments & Ablations (Months 7-8)

### Month 7 — Main Experiments (Static + Dynamic)

| Week | Simulation track | Real-hardware track | Deliverables |
|------|------------------|---------------------|--------------|
| 1 | Train preference-conditioned policy across preference space; generate Pareto front (hypervolume, coverage, sparsity). | Run a λ-sweep on real UR5 (3-5 preference settings × static tasks) to validate front transfers. | Pareto front (sim); real front sanity check |
| 2 | Evaluate on dynamic benchmarks at various obstacle speeds; measure success + safety-violation rate. | Test real dynamic scenes with the deployed policy (pendulum, moving target). | Dynamic results (sim + real) |
| 3 | Test dynamic preference adaptation: λ drifting during episode → on-the-fly behavior change. | Validate on-the-fly λ change on real hardware (safety-first: manual λ switch). | Adaptation results (sim + real) |
| 4 | Compare vs. baselines: PGMORL, Envelope MOQ, MO-MPO (scalarized), GCR-PPO, Diffusion Policy, DP3, demo-enhanced MORL nav. | Run a subset of baselines on the real rig for head-to-head. | Baseline comparison tables (sim + real subset) |

### Month 8 — Ablations & Robustness

| Week | Simulation track | Real-hardware track | Deliverables |
|------|------------------|---------------------|--------------|
| 1 | Ablate vision: DP3 point cloud vs. 2D RGB encoder vs. RGB-D. | Confirm best vision variant also works on real camera images. | Vision ablation (sim + real confirm) |
| 2 | Ablate critic: multi-head + PCGrad vs. multi-head only vs. single-head scalarized. | Weekly real regression on selected config. | Critic ablation |
| 3 | Ablate safety: CMDP constraint vs. collision-as-objective. Ablate preference: fixed λ vs. conditioned. | Verify safety-ablation differences appear on real hardware (violation counts). | Safety & preference ablations (sim + real) |
| 4 | Domain randomization, robustness to camera perturbation; statistical significance tests (paired t-tests / bootstrap). | Finalize real experimental protocol; freeze all real-robot evaluation configs. | **Milestone: simulation experiments complete; real protocol frozen** |

---

## Phase 5: Real-UR5 Experiments & Sim-to-Real Analysis (Months 9-10)

### Month 9 — Systematic Real-Robot Experiments

| Week | Simulation track | Real-hardware track | Deliverables |
|------|------------------|---------------------|--------------|
| 1 | Support real experiments: provide per-task sim references for gap computation. | Run full real static experiment: all λ settings × all static tasks × 3+ trials each. | Real static Pareto data (full) |
| 2 | Provide sim counterparts for each real dynamic scene (matched initializations). | Run full real dynamic experiment: moving obstacles, moving target, sudden appearance. | Real dynamic data (full) |
| 3 | Generate matched sim↔real metric pairs. | Run real dynamic preference-adaptation trials; measure safety-violation rate. | Real adaptation + safety data |
| 4 | Compile matched sim vs. real tables. | Consolidate all real-robot logs, videos, and photos. | **Milestone: complete real-robot dataset (static + dynamic)** |

### Month 10 — Sim-to-Real Gap Analysis & Finalization

| Week | Simulation track | Real-hardware track | Deliverables |
|------|------------------|---------------------|--------------|
| 1 | Compute sim-to-real gap per metric (success drop, hypervolume drop, violation-rate delta). | Re-run any flagged real scenes to confirm gap causes (lighting/friction/latency). | Sim-to-real gap tables |
| 2 | Identify which gaps are closable via DR; run targeted DR updates in sim. | Confirm updated policies still work on real hardware (quick re-test). | Improved sim-to-real results |
| 3 | Finalize all results, figures, and statistical tests. | Final real-robot photo/video shoot for the paper. | Final result set + media |
| 4 | Lock paper result tables (sim + real). | — | **Milestone: sim-to-real analysis complete, all results final** |

---

## Phase 6: Writing & Submission (Months 11-12)

### Month 11 — Drafting

| Week | Tasks | Deliverables |
|------|-------|-------------|
| 1 | Write Introduction + Related Work (using the reordered lit-review taxonomy from `references/README.md`). | Sections 1-2 draft |
| 2 | Write Methodology: MOMDP formulation, MORL algorithm, DP3 encoder, diffusion policy, CMDP safety, training, **parallel sim-real protocol**. | Section 3 draft |
| 3 | Write Experiments: setup, baselines, metrics, results tables (sim static, sim dynamic, real). | Section 4 draft |
| 4 | Write Discussion + Conclusion. Limitations, future work. Complete first full draft. | Full paper draft |

### Month 12 — Polish & Submission

| Week | Tasks | Deliverables |
|------|-------|-------------|
| 1 | Create all figures: Pareto front plots, training curves, trajectory visualizations, real-robot photos. | Figure set complete |
| 2 | Record supplementary video: side-by-side comparison of preference settings on real UR5. | Video ready |
| 3 | Internal review with co-authors / labmates; second round of revisions; formatting for target venue. | Revised camera-ready draft |
| 4 | Proofread, reference formatting (BibTeX), cover letter, submit to Q1 journal. | **Paper submitted** |

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
|-------|-----------------|----------|
| **IEEE TRO** | Rolling | 4-6 months |
| **IEEE RA-L** | Rolling (monthly) | 3-4 months |
| **IJRR** | Rolling | 6-8 months |
| **arXiv preprint** | Anytime | — |
