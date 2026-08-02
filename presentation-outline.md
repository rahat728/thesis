# Thesis Proposal Presentation Outline — Vision-guided MORL with UR5 in Dynamic Environments

Follows: https://thesislaunch.com/guide/thesis-proposal-structure-template-examples/ (10-section standard structure)
Target: Q1 journal paper
Audience: Thesis proposal defense (focus = research gap + contribution, NOT completed results)

---

## Mapping: ThesisLaunch 10-section structure -> presentation slides

| Proposal Section (ThesisLaunch) | Slide(s) |
|--------------------------------|----------|
| 1. Title Page | S1 Title |
| 2. Abstract | S2 Abstract |
| 3. Introduction | S3 Motivation, S4 Problem Statement & Purpose |
| 4. Literature Review | S5 Related Work, S6 Research Gap (CORE) |
| 5. Research Questions / Hypotheses | S7 Research Questions & Objectives |
| 6. Methodology | S8 Proposed Methodology |
| 7. Significance | S9 Expected Contributions (CORE) |
| 8. Limitations | S10 Limitations |
| 9. Timeline | S11 Timeline & Publication Plan |
| 10. References | S12 References |

---

## Slide-by-slide outline

### S1. Title Page
- Title: Vision-Guided Multi-Objective Reinforcement Learning for UR5 Manipulation in Dynamic Environments
- Your name, student ID, supervisor(s), department, university, date, degree program
- Title formula: [Specific focus]: [What you're examining] in [Context]

### S2. Abstract
- Problem (1-2 sentences): static-reward, single-objective RL fails for multi-objective manipulation in dynamic environments.
- Purpose: design a preference-conditioned vision-guided MORL policy for UR5.
- Method overview: diffusion-policy backbone + multi-head MO critic + preference conditioning + constraint handling.
- Expected significance: first vision-guided MORL framework for arm manipulation; Q1 target.

### S3. Motivation (Intro part 1)
- Real-world manipulation requires trading off **multiple conflicting objectives** (success vs. safety vs. energy vs. time), not a single scalar reward.
- **Dynamic environments** (moving objects, humans, clutter) break static-reward RL.
- UR5 is an industry-standard arm; current policies are mostly single-objective or hand-tuned scalarized.

### S4. Problem Statement & Purpose (Intro part 2)
- Problem: no established method for learning a preference-conditioned visuomotor policy that trades off objectives on a UR5 in dynamic, uncertain environments.
- Purpose statement: "The purpose of this study is to [develop] [a vision-guided MORL framework] in order to [enable multi-objective, preference-tunable UR5 manipulation under dynamic conditions]."
- Preview/roadmap of proposal structure.

### S5. Related Work (Literature Review — thematic synthesis, not a summary)
- Theme 1 — MORL: MOMDPs, vector rewards, Pareto front, preferences (MO-Gymnasium; Envelope MOQ, PGMORL, MO-MPO; MORL-Baselines).
- Theme 2 — Vision-guided manipulation: Diffusion Policy (tested on UR5), DP3 (point clouds), ACT/ALOHA, ALOHA Unleashed.
- Theme 3 — Constrained/safety-aware + dynamic environments: PRC-CMORL, GCR-PPO, demo-enhanced MO navigation (IROS 2025), Pareto visual navigation.
- Synthesis takeaway: MORL and vision-manipulation have developed largely in parallel; the intersection is barely explored.

### S6. Research Gap  (CORE)
- Gap 1: MORL applied mostly to locomotion/classic control (PGMORL, MO-MPO); very few works address **visual observations for arm manipulation**.
- Gap 2: MORL benchmarks (MO-Gymnasium, PGMORL) are **proprioceptive-only**; vision-driven MO benchmarks for arms are missing.
- Gap 3: MORL mostly assumes **static** environments; dynamic environments (moving objects, safety constraints) are underexplored.
- Gap 4: Constrained/safety-aware MORL for arms not validated with vision-guided UR5.
- Consequence: practitioners must hand-tune scalar weights, limiting adaptability to changing preferences/environments.

### S7. Research Questions & Objectives
- Primary RQ: Can a single preference-conditioned policy learn the Pareto-optimal visuomotor behaviors for UR5 manipulation under dynamic environments?
- Secondary RQs: (1) How to formulate objectives (success/safety/energy/time) with visual observations? (2) How to combine diffusion policy backbone with MO value/advantage heads robustly? (3) Does preference-conditioning improve sim-to-real transfer vs. scalarized baselines?
- Objectives O1-O4 (mapped to each RQ).

### S8. Proposed Methodology
- Research design: quantitative, simulation + real-robot experiments.
- Pipeline: RGB(-D)/point-cloud encoder (DP3-style) -> multi-head MO critic (per-reward advantages) -> preference-conditioned diffusion policy -> UR5.
- Gradient conflict resolution (PCGrad/GCR-PPO-style) to protect task objectives from regularisers.
- Constraint handling: CMDP-style safety (PRC-CMORL-style) for dynamic obstacles.
- Data collection: existing demonstration tools (teleop/IRIS-style), dynamic-environment benchmark.
- Evaluation plan: expected hypervolume, Pareto coverage/sparsity, task success, safety-violation rate, sim-to-real.
- Justification: methods chosen to answer RQ1-RQ3; replicable detail.

### S9. Expected Contributions (Significance)  (CORE)
- Theoretical: first preference-conditioned vision-guided MORL framework for arm manipulation (fills Gap 1-4).
- Practical: vision-based multi-objective UR5 benchmark for dynamic environments; open-source implementation + baselines.
- Broader impact: enables adaptable, multi-objective robots for industrial/human environments (target: Q1 journal).

### S10. Limitations
- Scope: single-arm UR5, 2-4 objectives, specific dynamic scenarios (moving targets, occlusions, disturbances).
- Methodological constraints: simulation-to-real gap, training cost at GPU scale, safety of real-robot trials.
- Generalizability: results may not directly transfer to other arms/morphologies.
- Acknowledging limitations shows maturity.

### S11. Timeline & Publication Plan
- Gantt/table: lit review -> formulation -> benchmark -> algorithm -> sim experiments -> real UR5 -> writing; buffer time added.
- Dependencies: benchmark before algorithm validation; real experiments after sim.
- Q1 journal submission target + milestone dates.

### S12. References
- All cited sources from `references/`, properly formatted.
- Suggestion: use a reference manager (Zotero) from day one.

---

## Sections from the old outline that were restructured
- "My Contribution" -> merged into S6 (Gap) + S9 (Significance).
- "Problem Statement" -> S4; "Background" -> S5; "Expected Results" -> S8 (evaluation plan).
- Removed: Technical Specifications table, Results/Performance (not needed in a proposal).

## NotebookLM sources (unchanged)
Tier 1: 07-diffusion-policy, 13-pgmorl, 01-mo-gymnasium, 10-prc-cmorl, 11-demo-enhanced-morl-nav
Tier 2: 04-3d-diffusion-policy, 16-envelope-morl, 02-mo-mpo, 14-pareto-visual-navigation, 08-gcr-ppo
