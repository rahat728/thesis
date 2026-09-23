# Related Work, Contributions, and Expected Results

**Paper:** Vision-Guided Multi-Objective Reinforcement Learning for UR5 Manipulation in Dynamic Environments
**Style:** Q1 journal sections (IEEE TRO / RA-L), gap-first
**Grounding:** full paper collection in `references/` and `paper-notes.md`

This document is the claims-driven core of the manuscript: it opens with a five-capability matrix, groups prior work by the capability each line of research is missing, states the contributions that fill the empty cell, and maps expected results to research questions with numbers taken from the cited papers.

---

## 2. Related Work

Real arm manipulation in changing scenes requires trading off conflicting objectives (task success, energy, smoothness, time) while remaining safe. A useful method would therefore provide all five of the following capabilities at once:

1. **Vision** — policies that act from RGB-D / point-cloud observations, not proprioception or lidar alone.
2. **MORL** — vector rewards and a Pareto set, not a single scalar reward.
3. **Manipulation** — contact-rich 6-DoF arm control, not 2D navigation.
4. **Preference conditioning** — a single policy whose behavior is retuned by a runtime preference vector $\lambda$, without retraining.
5. **Safety as a constraint** — collision, velocity, and workspace limits enforced as a constrained MDP (CMDP), not as another hand-tuned reward term.

Table 1 is the gap argument. Every existing line of work occupies some of these cells; none occupies all five. The rest of this section is organized by that missing cell, not by paper chronology.

**Table 1. Capability matrix.** `ok` = the method provides the capability; `x` = it does not; `partial` = related but incomplete for our setting.

| Method | Vision | MORL | Dynamic | Manip | Preference | Safety-as-constraint |
|--------|--------|------|---------|-------|------------|----------------------|
| Envelope MOQ (Yang et al., NeurIPS 2019) | x | ok | x | x | ok | x |
| PGMORL (Xu et al., ICML 2020) | x | ok | x | x | ok | x |
| MO-MPO (Abdolmaleki et al., NeurIPS 2022) | x | ok | x | partial | ok | x |
| DOL / ImageDeepSea (Mossalam et al.) | partial | ok | x | x | ok | x |
| Diffusion Policy (Chi et al., RSS 2023 / IJRR 2024) | ok | x | ok | ok | x | partial |
| DP3 (Ze et al., RSS 2024) | ok | x | ok | ok | x | empirical |
| ACT / ALOHA (Zhao et al., RSS 2023) | ok | x | ok | ok | x | x |
| GCR-PPO (Munn et al., 2025) | x | ok | partial | ok | x | x |
| PRC-CMORL (He et al., Neurocomputing 2023) | x | ok | x | x | ok | ok |
| Demo-enhanced MORL nav (de Heuvel et al., IROS 2025) | lidar | ok | ok | x | ok | x |
| POVNav (Pushp et al., 2025) | ok | classical | ok | x | partial | ok |
| **This work** | **ok** | **ok** | **ok** | **ok** | **ok** | **ok** |

### 2.1 Preference-conditioned MORL without vision or manipulation

The MORL literature established how to learn a set of Pareto-optimal policies, or a single preference-conditioned model, when the observation is low-dimensional and the plant is a locomotion or classic-control body.

**Envelope MOQ-learning** (Yang et al., NeurIPS 2019) trains one network $Q(s,a,\omega)$ over the entire linear preference space. An envelope Bellman operator maximizes utility over both actions and other preferences; homotopy loss $L=(1-\lambda)L_A+\lambda L_B$ and hindsight experience replay over preferences make the single model sample-efficient. The convex coverage set (CCS) language of this paper is the standard vocabulary for single-policy MORL. The domains, however, are Deep Sea Treasure, Fruit Tree Navigation, dialog, and Super Mario — discrete or grid, no robot arm, no visual observation.

**PGMORL** (Xu et al., ICML 2020) is the canonical *multi-policy* alternative. A population of PPO policies is trained on randomly sampled preference vectors; a Gaussian-process prediction model steers the population toward the Pareto front; interpolation between adjacent archive policies yields a continuous front. Hypervolume and sparsity are the reported metrics. The associated benchmark — seven continuous-control MO environments (HalfCheetah, Ant, Hopper, Walker2d, Humanoid, …) — became the de-facto evaluation suite and is now packaged in MO-Gymnasium (Felten et al., NeurIPS 2023) and MORL-Baselines. All of these environments are proprioceptive locomotion; none has a camera or a manipulator.

**MO-MPO** (Abdolmaleki et al., NeurIPS 2022) encodes preferences as per-objective KL budgets $\varepsilon_k$ rather than scalar weights, which makes preference setting *scale-invariant*. That argument matters for a UR5, where success, energy, and jerk live in incommensurable units, and we cite it when justifying normalized / constrained handling of heterogeneous objectives. MO-MPO is demonstrated on high-DoF bodies including Sawyer, but still from proprioceptive state, without a visual encoder and without an explicit safety CMDP.

**DOL** (Mossalam et al.) is historically the first deep MORL method and includes an image-based testbed (ImageDeepSea). It computes a CCS via optimistic linear support. The image observation is a toy grid, not a camera on an arm, so it supports the *existence* of vision+MORL rather than a manipulation method.

**Takeaway.** This family gives us the single-policy preference-conditioning template (Envelope), the multi-policy baseline (PGMORL), the scale-invariance argument (MO-MPO), and the vector-reward API (MO-Gymnasium). It does not give us cameras, contact-rich manipulation, or dynamic obstacles.

### 2.2 Vision-guided manipulation without MORL

In parallel, visuomotor policy learning produced strong single-objective imitation methods for arms, several of them evaluated on a UR5.

**Diffusion Policy** (Chi et al., RSS 2023 / IJRR 2024) represents the visuomotor policy as a conditional denoising diffusion process over action chunks. Receding-horizon control ($T_o=2$, $T_p=16$, $T_a=8$) plus visual conditioning yields a 46.9% average success-rate gain over prior SOTA on 12 tasks. The UR5 station in Appendix D is the hardware recipe we adopt: end-effector positional commands at 125 Hz interpolated from a 10 Hz policy, velocity $<0.43$ m/s, position $\ge 1$ cm above the table, RealSense D415 cameras. Safety here is a *controller limit*, not a learned constraint, and the objective is a single task-success signal.

**3D Diffusion Policy (DP3)** (Ze et al., RSS 2024) replaces RGB with a sparse point-cloud encoder (FPS to 512 or 1024 points, no T-Net / no BatchNorm). With 10–20 demonstrations per task it reports a 24.2% relative gain over image baselines on 72 simulation tasks and 85% real success with 40 demos; it “rarely violates safety,” unlike RGB diffusion policies that required human intervention. DP3 is our vision backbone and our data-budget justification. It remains single-objective imitation: there is no preference input and no Pareto evaluation.

**ACT / ALOHA** (Zhao et al., RSS 2023), **Mobile ALOHA / ACT++** (Fu et al., CoRL 2024), and **ALOHA Unleashed** extend action-chunking transformers and large-scale teleoperation, including Diffusion Policy recipes for bimanual 6-DoF arms. They are comparison baselines for visuomotor imitation, not MORL methods. **UMI** (Chi et al., RSS 2024) and **IRIS** (Jiang et al., CoRL 2025) supply hardware-agnostic / XR demonstration collection, including UR5e deployment and point-cloud teleop; we use them as data-collection options, not as algorithmic competitors. **Im2Flow2Act**, **DoughNet**, **VideoManip**, **VRB**, and **Manipulate-Anything** address cross-domain flow, deformable objects, video-driven dexterity, affordances, and VLM-generated demos. They strengthen the vision-manipulation story and provide a fallback if teleop is the bottleneck; none is multi-objective.

**Takeaway.** This family gives us the action generator (Diffusion Policy), the point-cloud encoder (DP3), the UR5 control-loop numbers, and the demonstration toolchain. Every method optimizes one scalar objective. Changing the trade-off (throughput vs. energy vs. safety) requires collecting a new dataset and retraining.

### 2.3 Robot-scale MORL without vision or preferences

**GCR-PPO** (Munn et al., 2025) is the closest *algorithmic* template for our critic. It decomposes $r=\sum_k r^{(k)}$, trains a multi-head critic $V_\phi:S\to\mathbb{R}^K$ with component-wise GAE, normalizes advantages with a global scale that preserves inter-objective ratios, and applies asymmetric PCGrad so that task gradients are never projected away by regularisers. On 13 IsaacLab tasks plus two custom multi-objective benchmarks it reports ~4.55% average return gain over PPO (up to 9.5% on the custom suites), at GPU scale in IsaacLab / RSL-RL. Observations are proprioceptive (Humanoid, full-body throwing). There is no preference vector $\lambda$, no visual encoder, and safety is a regulariser among others rather than a CMDP constraint.

**MO-Playground** (Janwani et al., 2026) massively parallelizes MORL in JAX, including a 6-D objective space on the BRUCE humanoid. It is a scaling reference and a possible alternative stack if we leave IsaacLab; it is not a vision-manipulation result.

**Takeaway.** We adopt GCR-PPO’s multi-head critic, ratio-preserving advantage normalization, and asymmetric PCGrad as the MO backbone, then add a DP3 encoder, a preference input, a diffusion actor, and a CMDP safety layer — four pieces GCR-PPO does not have.

### 2.4 Constrained MORL without vision

**PRC-CMORL** (He et al., Neurocomputing 2023) formulates a constrained multi-objective MDP (CMOMDP): a single model approximates Pareto-optimal policies for any user-specified preference, subject to nonlinear constraints. Evaluation uses a composite index of hypervolume and entropy (convergence, diversity, evenness) on nine MuJoCo locomotion tasks (MO-Swimmer, MO-Hopper, MO-Walker2d, MO-HalfCheetah, MO-Ant). This is the constraint design and the metric set we adopt. The observation is proprioceptive; there is no camera and no arm.

Controller-level safety in Diffusion Policy (velocity / workspace clamps) and the empirical safety of DP3 (fewer real-world interventions) are complementary but not CMDP: they do not expose a constraint budget the agent learns to satisfy across a preference space.

**Takeaway.** Safety as an *objective* (a term in the vector reward) can be traded off and violated when $\lambda$ shifts. Safety as a *constraint* cannot. We take the latter from PRC-CMORL and instantiate it with numeric UR5 bounds from Diffusion Policy Appendix D and POVNav ($d(s_t,O_t)\ge D_{\mathrm{safe}}$, $\|\dot{x}\|<0.43$ m/s, workspace $\ge 1$ cm above the table).

### 2.5 The vision $\times$ MORL intersection — navigation, not manipulation

Two recent papers sit at the vision–MORL intersection. Both are navigation.

**Demonstration-enhanced adaptable MO robot navigation** (de Heuvel et al., IROS 2025) is the single closest prior work. It combines PD-MORL (preference interpolator $I(\lambda)=\lambda^p$, angle loss $g(\lambda_p,Q)$, hindsight replay over preferences, $C_p$ parallel envs) with a D-REX demonstration reward, and adapts $\lambda$ at runtime without retraining. Sim-to-real is shown on two wheeled platforms. Observations are polar goal, human position, and a 30-ray lidar; actions are $(v,\omega)$. Collision is a sparse objective ($-1000$), not a constraint. The gaps that define our novelty are therefore explicit: **lidar not vision, navigation not manipulation, collision-as-objective not CMDP**.

**POVNav** (Pushp et al., 2025) selects Pareto-optimal subgoals (Horizon Optic Goal) on a navigability image and servos to them under explicit motion and safety constraints. Pareto thinking is used for *classical planning*, not for a learned preference-conditioned policy. We reuse its constraint template $d(s_t,O_t)\ge D_{\mathrm{safe}}$ and contrast its planner with our end-to-end policy.

**Takeaway.** The intersection of vision and multi-objective decision-making is active, but only for navigation, and either lidar-based (IROS 2025) or planner-based (POVNav). No paper in this collection learns a preference-conditioned visuomotor policy for a 6-DoF arm.

### 2.6 Synthesis

MORL and vision-guided manipulation have developed largely in parallel:

- MORL classics are proprioceptive and locomotion-centric (Envelope, PGMORL, MO-MPO, PRC-CMORL).
- Visuomotor policies are single-objective imitation (Diffusion Policy, DP3, ACT/ALOHA).
- Robot MORL at GPU scale is proprioceptive and not preference-conditioned (GCR-PPO).
- The closest systems that combine MORL with sensing in changing scenes are lidar navigation (de Heuvel et al.) and classical Pareto visual planning (POVNav).

No paper combines **vision + preference-conditioned MORL + arm manipulation + CMDP safety + dynamic environments**. That empty cell in Table 1 is the research gap this work fills. Practitioners today still hand-tune scalar weights, retrain when the job or the scene changes, and treat safety as one more term in a sum. The purpose of this study is to replace that loop with a single preference-conditioned visuomotor policy whose safety constraints are explicit.

---

## 3. Contributions

We claim three contributions, each mapped to a cell that Table 1 leaves empty.

**C1. A preference-conditioned vision-guided MORL framework for arm manipulation, with safety as a CMDP constraint.**
The pipeline is: RGB-D $\to$ point cloud $\to$ DP3 encoder $\to$ multi-head MO critic (component-wise GAE, ratio-preserving advantage normalization, asymmetric PCGrad with task $>$ regulariser priority) $\to$ preference-conditioned diffusion policy ($I(\lambda)=\lambda^p$, angle loss, receding-horizon action chunks) $\to$ UR5, with collision, velocity, and workspace limits enforced as CMDP constraints rather than reward terms. To the best of our knowledge this is the first method that occupies every column of Table 1 for a 6-DoF arm. The design is not a new primitive: each block is taken from a paper whose numbers we reuse (DP3, GCR-PPO, PD-MORL, Diffusion Policy, PRC-CMORL). The contribution is the *composition* that none of those papers performs, plus the UR5 instantiation in dynamic scenes.

**C2. A vision-based multi-objective UR5 benchmark with static and dynamic variants.**
MO-Gymnasium and the PGMORL suite are proprioceptive. We release a vector-reward UR5 reaching suite (success, energy, smoothness/jerk; safety as constraint) with static clutter and dynamic variants (moving obstacle, moving target, sudden appearance), wrapped in the MO-Gymnasium API so that PGMORL, Envelope, and related baselines run without reimplementation. Metrics: expected hypervolume, Pareto coverage/sparsity, entropy, task success, safety-violation rate, and matched sim-to-real gaps.

**C3. An empirical protocol that isolates the two novelty claims, including sim-to-real on a real UR5.**
We compare against three external baselines — PGMORL, Envelope MOQ (both via MORL-Baselines), and single-objective DP3 — plus an internal scalarized-PPO control. Two ablations test the thesis directly: (i) preference-conditioned $\lambda$ vs. fixed $\lambda$; (ii) CMDP safety vs. collision-as-objective. Real-robot experiments run in parallel with simulation from week 1, so sim-to-real gaps (lighting, friction, latency) are measured rather than deferred. Broader impact: a packing cell can move a preference slider (throughput in the morning, energy in the afternoon, safety at night) on one trained policy, instead of retraining for each job.

**Scope limits (stated so the claims stay falsifiable).** Single arm (UR5), 2–4 objectives, scripted dynamic scenes, residual reward engineering to define the vector. Results are not assumed to transfer to other morphologies or fully unstructured human environments.

---

## 4. Expected Results

Results are targets, not measurements. Every numeric expectation below is inherited from a cited paper or from the evaluation protocol in `12-month-plan.md` / `4-month-plan.md`. The hypothesis is:

> A single preference-conditioned visuomotor policy, trained once, dominates scalarized and single-objective baselines on expected hypervolume and reduces safety-violation rate relative to treating collision as an objective, in simulation and on a real UR5.

### 4.1 Research questions and predicted outcomes

**Main RQ.** Can one preference-conditioned policy learn Pareto-optimal visuomotor behaviors for a UR5 in a dynamic scene?

**RQ1 — Objective definition from vision.**
Point-cloud observations (DP3 encoder, FPS to 512 points) are sufficient to define a 3–4 dimensional MOMDP (success, energy, smoothness; safety as constraint) that is learnable objective-by-objective with single-objective PPO. *Prediction:* each isolated objective exceeds 90% of its single-objective PPO ceiling on static reaching, matching the Month-2 baseline checkpoint. Failure of this check would mean the vector reward is misspecified, not that MORL failed.

**RQ2 — Combining a diffusion actor with a multi-head MO critic.**
Replacing a scalar critic with GCR-PPO-style multi-head value heads and asymmetric PCGrad improves the Pareto front without collapsing task success. *Prediction:* on the static visual-MO benchmark, the MO critic yields a GCR-PPO-class gain of order **~4.55% average return** (up to ~9.5% on the more conflicted objective sets) over scalarized PPO, as reported by Munn et al. on IsaacLab. Preference-conditioned diffusion (CNN backbone first; $T_o=2$, $T_p=16$, $T_a=8$) matches or exceeds an MLP actor on success while producing smoother action chunks under moving obstacles. Inference is required to fit the UR5 loop: native 10 Hz policy + 125 Hz interpolation, as in Diffusion Policy Appendix D; if that budget is missed, the MLP actor remains the reported method (risk guardrail in the 4-month plan).

**RQ3 — Preference conditioning vs. scalarization, including sim-to-real.**
A policy conditioned on continuous $\lambda$ (power-transform interpolator + angle loss, following de Heuvel et al.) traces a denser, higher-hypervolume front than (a) a fixed-$\lambda$ policy and (b) a family of independently scalarized PPO runs, and transfers that front to the real UR5. *Prediction:* hypervolume of the conditioned policy is at least as high as PGMORL / Envelope on the same vector-reward wrapper (those two are the canonical multi-policy baselines and are run from MORL-Baselines with default configs). On hardware, 3–5 $\lambda$ settings produce *measurably different* behavior (success–energy–smoothness trade-off) without retraining — the qualitative result of IROS 2025, now on an arm. Sim-to-real success drop is reported per metric; DP3’s 85% real success with 40 demos is an *upper-reference* for the single-objective vision baseline, not a claim we copy onto the MO setting.

**RQ4 — CMDP safety vs. collision-as-objective.**
Enforcing $d(s,O)\ge D_{\mathrm{safe}}$, $\|\dot{x}\|<0.43$ m/s, and workspace bounds as CMDP constraints reduces safety-violation rate relative to putting collision in the reward vector (the IROS 2025 / GCR-PPO style), without a collapse in task success. *Prediction:* violation rate on the constrained policy approaches **zero on the static validation set** (Month-3 real-safety check) and stays strictly below the collision-as-objective ablation on dynamic scenes (moving obstacle / moving target). Success may decrease slightly; the expected result is a better *constrained* Pareto set, not a free lunch on every objective.

### 4.2 Metric set and baselines

Primary metric: **expected hypervolume** (PRC-CMORL; PGMORL). Secondary: Pareto coverage / sparsity, entropy index (diversity/evenness), task success rate, safety-violation rate, and per-metric sim-to-real delta.

| Baseline | Role | Source |
|----------|------|--------|
| PGMORL | multi-policy MORL | Xu et al., ICML 2020; MORL-Baselines |
| Envelope MOQ | single-model preference MORL | Yang et al., NeurIPS 2019; MORL-Baselines |
| Single-objective DP3 | vision / imitation control | Ze et al., RSS 2024 |
| Scalarized PPO | internal MO control | GCR-PPO comparison protocol |

GCR-PPO, MO-MPO, and demo-enhanced MORL navigation are cited in Related Work and Discussion; they are not reproduced as vision-manipulation baselines because they are proprioceptive or lidar-navigation methods and would not isolate the contribution.

Ablations (only two, matching the compressed protocol): preference-conditioned vs. fixed $\lambda$; CMDP vs. collision-as-objective. A DP3-vs-RGB vision ablation is *not* required to support C1–C3: Ze et al. already report a 24.2% relative gain and a safety advantage for point clouds.

### 4.3 What would count as a negative result

The contribution claim fails if any of the following holds after the planned experiments:

- The preference-conditioned policy does not dominate, or match, PGMORL/Envelope on hypervolume on our own benchmark (then C1 is a systems composition without a MORL gain).
- Different $\lambda$ values do not change real-UR5 behavior (then preference conditioning did not survive sim-to-real).
- CMDP does not reduce violation rate relative to collision-as-objective (then C1’s safety claim is unsupported; we would report safety as an objective and shrink the novelty statement).
- The dynamic variants are unlearnable from point clouds (then C2’s dynamic benchmark is too hard, and the paper falls back to static scenes plus a negative result).

These failure modes are listed so that the expected results remain a protocol, not a promise.

### 4.4 Grounding numbers (from the paper collection)

| Quantity | Paper value | Use in this work |
|----------|-------------|------------------|
| 10–20 demos / task | DP3 | BC warm-start budget |
| 85% real success, 40 demos | DP3 | single-objective real reference |
| 24.2% relative gain, point cloud vs. RGB | DP3 | justification for skipping the RGB ablation |
| $T_o=2$, $T_p=16$, $T_a=8$; 125 Hz; $<0.43$ m/s; $\ge 1$ cm | Diffusion Policy | actor horizon and CMDP numeric bounds |
| ~4.55% avg gain (to 9.5%) | GCR-PPO | expected MO-critic lift over scalarized PPO |
| Hypervolume + entropy | PRC-CMORL | primary Pareto metrics |
| $I(\lambda)=\lambda^p$, angle loss, no-retrain $\lambda$ shift | de Heuvel et al., IROS 2025 | preference module; qualitative real-robot target |
| $d(s,O)\ge D_{\mathrm{safe}}$ | POVNav | collision constraint template |
| Vector-reward numpy API | MO-Gymnasium | benchmark wrapper |

---

## References (in-order, as cited)

1. de Heuvel, J., Sethuraman, T., and Bennewitz, M. Demonstration-Enhanced Adaptable Multi-Objective Robot Navigation. IROS 2025. arXiv:2404.04857.
2. Munn, H., Tidd, B., Böhm, P., Gallagher, M., and Howard, D. Scalable Multi-Objective Robot Reinforcement Learning through Gradient Conflict Resolution. arXiv:2509.14816, 2025.
3. Chi, C. et al. Diffusion Policy: Visuomotor Policy Learning via Action Diffusion. RSS 2023 / IJRR 2024.
4. Ze, Y. et al. 3D Diffusion Policy: Generalizable Visuomotor Policy Learning via Simple 3D Representations. RSS 2024.
5. Xu, J. et al. Prediction-Guided Multi-Objective Reinforcement Learning for Continuous Robot Control. ICML 2020.
6. Yang, R., Sun, X., and Narasimhan, K. A Generalized Algorithm for Multi-Objective Reinforcement Learning and Policy Adaptation. NeurIPS 2019.
7. He, X., Hu, Z., Yang, H., and Lv, C. Personalized Robotic Control via Constrained Multi-Objective Reinforcement Learning. Neurocomputing, 2023.
8. Abdolmaleki, A. et al. A Distributional View on Multi-Objective Policy Optimization. NeurIPS 2022.
9. Felten, F. et al. A Toolkit for Reliable Benchmarking and Research in Multi-Objective Reinforcement Learning (MO-Gymnasium). NeurIPS 2023.
10. Pushp, D. et al. Navigating the Wild: Pareto-Optimal Visual Decision-Making in Image Space. arXiv:2511.07750, 2025.
11. Mossalam, H., Assael, Y. M., Roijers, D. M., and Whiteson, S. Multi-Objective Deep Reinforcement Learning (DOL).
12. Zhao, T. et al. Learning Fine-Grained Bimanual Manipulation with Low-Cost Hardware (ALOHA / ACT). RSS 2023.
13. Fu, Z. et al. Mobile ALOHA: Learning Bimanual Mobile Manipulation with Low-Cost Whole-Body Teleoperation. CoRL 2024.
14. Chi, C. et al. Universal Manipulation Interface (UMI). RSS 2024.
15. Jiang, Y. et al. IRIS: An Immersive Robot Interaction System. CoRL 2025.
