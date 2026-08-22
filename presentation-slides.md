# Presentation Slides — Vision-Guided MORL with UR5 in Dynamic Environments

Writing style: **project-page style** (Diffusion Policy / DP3 / ALOHA) — big titles, teaser visuals, short plain sentences, one idea per slide, bolded key claims, concrete numbers. Structure maps to the ThesisLaunch 10-section proposal template.

## Section-to-slide map (per ThesisLaunch)

| ThesisLaunch section | Slide(s) |
|----------------------|----------|
| 1. Title Page | S1 |
| 2. Abstract | S2 |
| 3. Introduction | S3 (context: industrial example), S4 (problem: workflow), S5 (purpose: deliverables) |
| 4. Literature Review | S6 (timeline), S7 (gap: capability matrix), S8 (why not existing methods: comparison table) |
| 5. Research Questions / Hypotheses | S9 |
| 6. Methodology | S10 (definitions), S11 (pipeline), S12 (evaluation + ablations) |
| 7. Significance | S13 (contributions) |
| 8. Limitations | S14 |
| 9. Timeline | S15 |
| 10. References | S16 |

---

## S1 — Title Page

**Hero text (project-page style):**
```
Vision-Guided Multi-Objective RL
for UR5 in Dynamic Environments

A single preference-conditioned policy for a 6-DoF arm
in scenes that change.

[Your Full Name] — Student ID · Supervisor: [Name]
Department of [X], [University] · [Degree Program] · [Date]
```

**Visual:** full-width teaser — UR5 with camera over a dynamic/cluttered table. Caption: "One policy, trained once, tuned by a slider — safe under dynamic obstacles."

---

## S2 — Abstract

**Text (one plain paragraph, flow = problem -> limitation -> method -> contribution -> results):**
```
Real arm manipulation faces conflicting goals — success, safety, energy,
time — and scenes that change. Existing methods either optimize a single
scalar reward (vision-guided policies) or assume static, proprioceptive
environments (MORL); neither adapts to new preferences without retraining.

We propose a preference-conditioned vision-guided MORL framework for a
UR5 in dynamic environments. It pairs a DP3 point-cloud encoder with a
preference-conditioned diffusion policy and a multi-head multi-objective
critic, and enforces safety as an explicit constraint (CMDP) rather than
a hand-tuned weight.

To the best of our knowledge this is the first vision-guided MORL
framework for arm manipulation. We release a new vision-based
multi-objective UR5 benchmark with dynamic environments.

In simulation and on a real UR5 we compare against scalarized and
single-objective baselines on expected hypervolume, Pareto coverage,
success rate, and safety-violation rate.
```
Takeaway to speak: **"A single preference-conditioned policy, trained once, tuned by a slider — safety enforced explicitly, not hand-tuned."**

**Visual:** none — clean abstract block. Bold the three key terms in the deck.

---

## S3 — Introduction: Context (industrial motivation)

**Text (concrete example — one robot, three jobs):**
```
A UR5 packing fragile products.

Morning   maximize throughput
Afternoon minimize energy
Night     maximize safety

Same robot. Different preferences.

Current RL     -> retrain for every job.
Our approach   -> move the preference slider.
```

**Visual:** three clock/icon panels over one UR5 image (throughput / energy / safety).

---

## S4 — Introduction: Statement of the Problem (workflow, not paper list)

**Text:**
```
Today — engineer hand-tunes weights, then hopes.

Engineer -> choose reward weights -> train policy
        -> environment changes -> weights fail -> retrain

We want — preference is an input, not a design decision.

Preference vector -> same policy -> different behavior
```

**Visual:** two flow diagrams side by side: current (red, loops back to retrain) vs desired (green, single forward path).

---

## S5 — Introduction: Purpose & Deliverables

**Text:**
```
We deliver three things:
  Framework   preference-conditioned vision-guided MORL for arms
  Benchmark   vision-based MO UR5 environment set, dynamic variants
  Evaluation  open baselines + ablations, sim-to-real

Roadmap: Lit Review -> Questions -> Method -> Significance
         -> Limitations -> Timeline.
```

**Visual:** three deliverable cards + funnel/breadcrumb diagram.

---

## S6 — Literature Review: Timeline

**Text (evolution, one line per year):**
```
2019  Envelope MOQ       one model over all preferences (value-based)
2020  PGMORL             evolutionary MORL, continuous Pareto front
2022  MO-MPO             scale-invariant MORL via KL constraints
2023  Diffusion Policy   visuomotor diffusion, tested on a UR5
2024  DP3                3D point-cloud encoder, data-efficient
2025  Demo-enhanced MORL dynamic preference adaptation (lidar nav)
NOW   OURS               vision + preferences + safety for arms
```

**Visual:** horizontal timeline with paper logos/thumbnails; your node at the end in accent color.

---

## S7 — Research Gap (capability matrix)

**Text (capabilities that exist, and the one combination that doesn't):**
```
Preference-conditioned MORL   Envelope, PGMORL, MO-MPO
Vision-guided manipulation    DP3, Diffusion Policy
Robot MORL at scale           GCR-PPO
Dynamic preference adaptation Demo-enhanced MORL

No existing work combines:
  Vision-guided + Preference-conditioned + Manipulation
  + Safety-aware + Dynamic environments        <- the empty cell
```

**Visual:** capability matrix; the "combination" row highlighted in red.

---

## S8 — Why Not Existing Methods? (novelty evidence)

**Text (method-by-method feature table):**
```
Method               Vision  MORL  Dynamic  Manip  Preference
Envelope               x      ok     x       x       ok
PGMORL                 x      ok     x       x       ok
Diffusion Policy      ok      x     ok      ok       x
DP3                   ok      x     ok      ok       x
GCR-PPO                x      ok    partial ok       x
Demo-enhanced MORL    partial ok     ok      x       ok
Proposed (ours)       ok      ok    ok      ok      ok
```
Takeaway: **"Every existing method has at least one missing capability. Ours is the only one with all five."**

**Visual:** the table as a matrix; ours highlighted.

---

## S9 — Research Questions

**Text (plain-language questions):**
```
Main question:
  Can ONE preference-conditioned policy learn Pareto-optimal visuomotor
  behaviors for a UR5 in a dynamic scene?

RQ1   How do we define objectives (success, safety, energy, time)
      from what the camera sees?
RQ2   How do we combine a diffusion-policy backbone with a multi-head
      multi-objective critic?
RQ3   Does preference-conditioning help sim-to-real transfer
      vs. scalarized baselines?
RQ4   Do explicit safety constraints (CMDP) outperform treating safety
      as just another reward?

Hypothesis: preference-conditioned policies beat scalarized baselines
on expected hypervolume and safety-violation rate.
```

**Visual:** RQ <-> objective mapping table, or a small Pareto-front plot.

---

## S10 — Methodology: Design Definitions

**Text (concrete choices, answering RQ1):**
```
Observation   RGB-D -> point cloud (DP3 encoder)
Action        end-effector Cartesian pose / velocity commands (125 Hz loop)
Objectives    success | safety | energy | time
Preference    continuous weight vector λ (runtime input)
Constraints   collision | velocity | workspace bounds   (CMDP)
```
Justification: each choice is grounded in the studied baselines (DP3 for vision, Diffusion Policy for action, GCR-PPO for the critic).

**Visual:** six definition tiles around a small UR5 icon.

---

## S11 — Methodology: Proposed System (money slide)

**Text (one diagram + short labels):**
```
RGB-D / point cloud
     |
DP3 encoder  ---- safety constraints (CMDP) ----
     |                                          |
Multi-head critic (per-reward advantage)         |
     |                                          |
Preference slider w ---> Preference-conditioned ---> UR5
                         diffusion policy
```
Labels (one idea each):
```
- Multi-head critic + asymmetric PCGrad protect task goals from regularizers.
- The preference slider reshapes behavior at runtime. No retraining.
- Receding-horizon action prediction keeps motion smooth in dynamic scenes.
- CMDP keeps the arm safe near moving obstacles.
```

**Visual:** pipeline diagram (reuse DP3/GCR-PPO figure pieces).

---

## S12 — Methodology: Evaluation & Ablations

**Text:**
```
Baselines:
  MORL      PGMORL, Envelope MOQ, MO-MPO (scalarized)
  MO-robot  GCR-PPO (IsaacLab, ~4.55% avg gain), demo-enhanced MORL nav
  Vision    Diffusion Policy, DP3 (single-objective)

Metrics: expected hypervolume, Pareto coverage/sparsity,
success rate, safety-violation rate, sim-to-real transfer.

Ablations (the open design questions from the notes):
  - DP3 encoder vs RGB encoder        - preference-conditioned vs fixed λ
  - with vs without CMDP constraint   - static vs dynamic preference
```

**Visual:** metric tiles row + baseline logo row + ablation checklist.

---

## S13 — Significance: Contributions

**Text (three cards, one sentence each):**
```
Framework   To the best of our knowledge, the first preference-conditioned
            vision-guided MORL for arms, safety-aware (CMDP).
Benchmark   Open vision-based multi-objective UR5 benchmark,
            with dynamic environments.
Code        Reproducible baselines vs PGMORL, Envelope, GCR-PPO,
            Diffusion Policy, DP3.
```
Broader impact: adaptable, safety-aware robots for industrial and human-shared spaces. **Target: Q1 journal.**

**Visual:** three icon cards + UR5 industrial scene image.

---

## S14 — Limitations

**Text (plain, honest):**
```
We accept four limits:
  - one arm (UR5), 2-4 objectives — not other arms or big objective spaces
  - sim-to-real gap and GPU cost cap real-robot trial scale
  - reward engineering is still needed to define the objective vector
  - dynamic scenes are scripted, not fully unstructured
Even so: validated framework + reusable benchmark.
```

**Visual:** minimal; optional scope-bracket graphic.

---

## S15 — Timeline

**Text (Gantt chart, not bullets):**
```
M1-M2    Literature review & gap statement
M2-M3    Objective formulation (MOMDP)
M3-M5    Vision-based MO benchmark (sim)
M4-M6    Algorithm implementation
M6-M8    Simulation experiments + ablations
M8-M10   Real-UR5 validation (sim-to-real)
M10-M12  Writing + Q1 journal submission
(+1 month buffer per phase)
```

**Visual:** Gantt chart (Excel/Lucidchart).

---

## S16 — References

**Text (top 7 cited, most-relevant-first):**
```
[1] de Heuvel et al., "Demo-Enhanced Adaptable Multi-Objective Robot
    Navigation," IROS 2025.
[2] Munn et al., "Scalable Multi-Objective Robot RL through Gradient
    Conflict Resolution," arXiv 2509.14816, 2025.
[3] Chi et al., "Diffusion Policy: Visuomotor Policy Learning via Action
    Diffusion," RSS 2023 / IJRR 2024.
[4] Ze et al., "3D Diffusion Policy," RSS 2024.
[5] Xu et al., "Prediction-Guided MORL for Continuous Robot Control," ICML 2020.
[6] Yang et al., "A Generalized Algorithm for MORL and Policy Adaptation," NeurIPS 2019.
[7] He et al., "Personalized Robotic Control via Constrained MORL," Neurocomputing 2023.
```

**Visual:** none — text slide; full list in `references/`.

---

## Visual source map

| Slide | Visual | Source |
|-------|--------|--------|
| S1 teaser | UR5 rig banner | Diffusion Policy page / your render |
| S3 clock panels | UR5 + 3 preference icons | Your render |
| S4 workflows | Current vs desired flow | Draw yourself |
| S6 timeline | Paper timeline | Draw yourself |
| S7/S8 matrices | Capability / feature matrix | Draw yourself |
| S11 pipeline | Method diagram | Draw; reuse DP3/GCR-PPO figure pieces |
| S12 tiles | Metric + baseline icons | Draw yourself |
| S15 | Gantt chart | Excel/Lucidchart |

## NotebookLM sources (reordered by relevance 2026-08-21)
Tier 1 (core): 11-demo-enhanced-morl-nav, 08-gcr-ppo, 07-diffusion-policy, 04-3d-diffusion-policy, 10-prc-cmorl
Tier 2 (supporting): 14-pareto-visual-navigation, 16-envelope-morl, 02-mo-mpo, 13-pgmorl, 01-mo-gymnasium, 24-umi, 27-im2flow2act, 25-doughnet, 26-open-x-embodiment
