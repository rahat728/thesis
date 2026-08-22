# Deep Paper Notes — Vision-guided MORL with a UR5 in Dynamic Environments

Compiled: 2026-08-02 · Reordered by relevance: 2026-08-21
Purpose: Deep technical analysis of the papers I have read in full (extracted to `/tmp/opencode/*.txt`). Formulations, key hyperparameters, benchmark numbers, and UR5 hardware details, for thesis writing. Ordered most-relevant-first.

Reading status legend:
- **DEEP** = read in full, detailed notes below
- **REF** = skimmed / reference note only (see `references/`)
- **PDF** = PDF saved in `papers/`

---

## 1. Demonstration-Enhanced Adaptable MO Robot Navigation (11) — DEEP

**Venue**: IROS 2025 (Humanoid Robots Lab, U. Bonn) · arXiv:2404.04857 · Code: github.com/HumanoidsBonn/demo_enhanced_morl_nav

### Core idea
Combines **demonstration-based learning (LfD)** with **MORL** for human-aware navigation. Demonstrations become a *tuneable objective* modulated by preference λ **after training** — policy adapts to changing user preferences on-the-fly without retraining.

### Method components
1. **PD-MORL backbone** (Basaklar et al., TD3-based, preference-driven): learns a single network covering the whole preference space via 4 modifications:
   - **Preference interpolator** `I(λ) = λ^p` (power-transform) projecting preferences into a normalized solution space → better alignment with multi-objective Q values.
   - **Angle loss** `g(λ_p, Q)` minimizing the directional angle between interpolated preference and the vector Q — improves preference-reflection.
   - Actor updated by maximizing `λ^T Q` while minimizing the angle term.
   - **Hindsight experience replay** over preferences + **C_p parallel envs** each exploring a distinct preference-region segment.
2. **D-REX reward model for demonstrations**: demonstrations have no natural ranking → build *artificial rankings* by executing a noise-injected BC policy `π_BC(·|ε)` at increasing noise levels ε ∈ (0,…,0.2); lower-noise trajectories are "better". Train reward model `R̂(s,a) ∈ [0,1]` via **Bradley–Terry** with binary CE loss. Data: N_D=1000 augmented demonstrations from a single demo pattern (obstacle randomization).
3. **Reward vector** `r_t = (r_core, r_demo, r_distance, r_efficiency)`:
   - Core (static weight 1): `r_goal = 125·Δd_tg` (non-discounted cumulative to avoid path-length bias) + `r_collision = −1000` sparse.
   - Dynamic/tuneable: demo-reflection (R̂), proxemics `r_distance = −10(d_h − d_thresh)²` within 2 m, and efficiency.

### State/action
- State: polar goal `p_g`, human position `p_h`, lidar scan min-pooled 720→30 rays (range 4 m). `s_t = (p_g, p_h, L_t)`.
- Action: `a_t = (v, ω)`, v∈[0,0.5] m/s, ω∈[−π,π] rad/s, control loop at 5 Hz.
- Networks: MLP 4×256 (actor, critic, BC policy, reward model).
- Sim-to-real transfer on **two wheeled robot platforms**.

### Thesis relevance
- **The single most thesis-relevant work found**: it already does MORL + preference conditioning + demonstrations + dynamic-λ adaptation + sim-to-real. Its *gaps* define our novelty:
  - LIDAR, not vision (no image/point-cloud observation).
  - Navigation (2D velocity commands), not manipulation (no arm, no contact-rich tasks).
  - No explicit safety constraints (collision is an objective, not a constraint).
- Directly cite as "closest work"; our thesis = the UR5 vision + manipulation + CMDP-safety extension.

---

## 2. GCR-PPO — Scalable MO Robot RL via Gradient Conflict Resolution (08) — DEEP

**Venue**: arXiv:2509.14816 · Code: github.com/humphreymunn/GCR-PPO

### Core idea
On-policy PPO variant for additive-reward robot RL. Decomposes `r = Σ_k r^(k)` into per-component signals; uses a **multi-head critic** + **component-wise GAE** + **asymmetric PCGrad** with task > regulariser priority. Runs at massive GPU scale in IsaacLab/RSL-RL.

### Formulation
- Additive reward decomposition `r_t = Σ_k r_t^(k)`, components partitioned into task-based (I_T) and regulariser-based (I_R) sets.
- **Multi-head critic** `V_ϕ: S → R^K` with head `V_ϕ^(k)` predicting discounted value of component k; trained jointly by least squares against bootstrapped per-component targets.
- **Component-wise GAE**: TD residuals `δ_t^(k) = r̃_t^(k) + γ(1−d_t)V_ϕ^(k)(s_{t+1}) − V_ϕ^(k)(s_t)`; advantage `A_t^(k)` via per-component λ-return.
- **Advantage normalization preserving ratios**: center each component's advantage, then apply a **global scale** so the summed advantage has unit variance: `Â = (A − μ)/√(C·(1+ε))` with `C = Σ` of the covariance — keeps inter-component ratios while normalizing magnitude (standard per-component standardization would destroy relative scale).

### Gradient resolution (asymmetric PCGrad)
- Per-component clipped PPO surrogate loss (clip 0.2).
- Gradient `g^(k) = ∇L^(k)(θ)`. If `g^(i)^T g^(j) < 0` (conflict), project the **lower-priority** gradient onto the orthogonal complement of the higher-priority one:
  `g^(i) ← g^(i) − (g^(i)^T g^(j)/‖g^(j)‖²) g^(j)`.
- **Priority rules**: task-based > regulariser; task–task and regulariser–regulariser conflicts handled symmetrically (vanilla PCGrad). Higher-priority objectives are never weakened.

### Results
- 13 IsaacLab tasks + 2 custom multi-objective benchmarks (Humanoid Running, Full-Body Throwing; ≤10+ reward terms).
- Avg ~4.55% return improvement over baseline PPO (up to 9.5% on custom benchmarks); win-rate analysis; overhead ~>99% of added cost is gradient resolution.
- Multi-head critic ablation: separate gains from multi-head vs. conflict resolution.

### Thesis relevance
- **Closest template for our method**: replace/scale it to visual observation (DP3 encoder) + preference conditioning + safety constraints. Multi-head critic + asymmetric PCGrad is our MO backbone.
- Explicitly designed for robot RL at GPU scale (IsaacLab) — the natural training framework for a UR5 sim benchmark.

---

## 3. Diffusion Policy (07) — DEEP

**Venue**: RSS 2023 / IJRR 2024 · arXiv:2303.04137 · Code: diffusion_policy (Columbia)

### Core idea
Represent the visuomotor policy as a **conditional denoising diffusion process (DDPM)** generating action sequences. `ε_θ(O_t, A_t^k, k)` predicts noise; actions are denoised from Gaussian noise conditioned on observations.

### Key design decisions
1. **Closed-loop action-sequence prediction**: at time t, takes latest `T_o` observation steps, predicts `T_p` actions, executes `T_a` without replanning (typically T_o=2, T_p=16, T_a=8). Temporal consistency + responsiveness.
2. **Receding-horizon control**: warm-start next inference with previous action-sequence prediction → smoother actions.
3. **Conditioning on observations only**: `p(A_t|O_t)` not `p(A_t,O_t)` (unlike Janner planning) → faster inference, enables end-to-end training of the vision encoder. Loss: `L = MSE(ε_k, ε_θ(O_t, A_t^0 + ε_k, k))`.
4. **Network architectures**:
   - **CNN-based**: 1D temporal CNN (Janner Diffuser style) + FiLM conditioning on obs and denoising step k. Works out-of-the-box on most tasks; performs poorly on fast/sharply-changing action sequences (velocity-command spaces) due to low-frequency bias of temporal convs.
   - **Time-series diffusion transformer**: minGPT-style decoder; noisy actions as input tokens, sinusoidal embedding of k prepended as first token, obs → shared-MLP embedding fed as features; each output token predicts ε. Better for high task complexity / high-rate action change; more hyperparameter-sensitive.
   - Recommendation: start CNN, switch to transformer if needed.

### UR5 hardware details (appendix)
- **UR5 station** (Push-T task): end-effector-space positional commands at **125 Hz**, linearly interpolated from 10 Hz policy/demo commands.
- Interpolation controller limits end-effector velocity < 0.43 m/s, position ≥ 1 cm above table (safety).
- **5× Intel Realsense D415 depth cameras** recording 720p RGB @ 30 fps; only 2 used for policy obs, downsampled to **320×240 @ 10 fps**.
- Teleoperation via **3Dconnexion SpaceMouse** @ 10 Hz.
- Franka station: QP-based differential-kinematics mid-level controller imposing collision avoidance, safety regions, joint limits, null-space redundancy regulation; haptic mode = Operational Space Control QP @ 200 Hz.

### Benchmark
- 15 tasks / 4 robot platforms; multimodal action distributions, training stability, high-frequency control (500 Hz on some tasks); outperforms prior visuomotor policies incl. ACT/transformer baselines.

### Thesis relevance
- The **policy generator** for our method: a preference-conditioned diffusion policy outputting action sequences for the UR5. 
- Provides the exact UR5 setup (cameras, control loop, teleop) we can reference for the hardware section.

---

## 4. 3D Diffusion Policy / DP3 (04) — DEEP

**Venue**: RSS 2024 · arXiv:2403.03954 · Code: github.com/YanjieZe/3D-Diffusion-Policy

### Core idea
Diffusion Policy but with a **3D visual representation from sparse point clouds** instead of 2D RGB images. Encode point clouds → compact 3D representation → condition the diffusion action-generation process. Remarkably **data-efficient** (10–20 demos) and robust to camera changes.

### Pipeline
1. Convert depth → point clouds with camera extrinsics.
2. **Farthest Point Sampling (FPS)** downsample to 512 or 1024 points.
3. **DP3 Encoder** (compact, efficient point-cloud encoder; no color info) → representation (dim 128 after concatenation with robot pose).
4. Concatenate point-cloud representation + robot pose → condition diffusion policy.

### Key findings / ablations
- Point clouds beat RGB/RGBD images in efficiency and robustness.
- **Why DP3 Encoder beats PointNet**: T-Net and BatchNorm are the primary inhibitors to efficiency/sample-efficiency. Omitting them yields the DP3-style encoder.
- PointNet++ / PointNeXt / pre-trained PointNet++ alternatives — DP3 encoder best (or comparable).
- Cropping/segmenting point clouds in the workspace region helps (removes background).
- ~2x average performance improvement over 2D-based methods across 10 real-world tasks (data-efficient: as few as 10 demos per task).

### Thesis relevance
- Our **vision backbone**: point-cloud DP3 encoder on the UR5 — aligns with the "3D scene awareness" theses requirement for dynamic environments (point clouds make motion/target tracking + safety feasible).
- The data-efficiency result (10–20 demos) matters for a thesis with limited demo budget.
- IRIS (03) uses point-cloud teleop, so the whole data-collection chain can be point-cloud-native.

---

## 5. POVNav — Pareto-Optimal Visual Navigation (14) — DEEP

**Venue**: arXiv:2511.07750 (SAGE preprint, journal-style)

### Core idea
Vision-based navigation with **Pareto-optimal subgoal (Horizon Optic Goal, HOG) selection** on a navigability image from semantic segmentation. Real-time, camera-only.

### Method
- Observation: monocular RGB → semantic segmentation → navigable/non-navigable classes → **navigability image** with a *visual horizon*.
- Subgoal selection = **multi-objective optimization** over candidates `c = [c_nav, c_exp]` (navigability vs. exploration), Pareto-optimal set; scalarization `w1·c_nav + w2·c_exp` with weights set by environment complexity / user preference.
- Visual servoing to the HOG with two features: **proximity feature** (min distance from robot to visual horizon — controls linear velocity) and **alignment feature** (heading deviation toward HOG).
- Constraints: motion, safety `d(s_t, O_t) ≥ D_safe`, velocity/acceleration bounds.
- Acknowledges scalarization cannot find all Pareto-optimal solutions for non-convex fronts; weights need tuning/normalization.

### Thesis relevance
- Recent evidence that Pareto thinking + vision + robotics is active; but **subgoal selection is not learned policy** — it's classical planning with a scalarized MO scoring. Contrast: we learn a single preference-conditioned policy end-to-end.
- Its explicit safety/dynamics constraint set (`D_safe`, velocity/accel bounds) is a nice template for our CMDP safety layer.

---

## 6. PGMORL — Prediction-Guided Multi-Objective RL (13) — DEEP

**Venue**: ICML 2020 · arXiv:2003.08134 · Code: github.com/mit-gfx/PGMORL

### Core idea
Evolutionary MORL. Maintains a *population* of policies, each trained with PPO against a scalarized reward using a *random sampled preference vector*. A *prediction model* (offline Bayesian regression, GP) maps preference → expected return so the population can be steered toward the Pareto front. Final output is a **continuous Pareto front via interpolation** between adjacent policies.

### Algorithm structure (per generation)
1. Warm-up: train initial population on random preference samples.
2. **Prediction**: fit an analytical model per policy from past RL data (predicts each objective's return for a candidate preference).
3. **Evolutionary stage**: score candidate offspring preferences with the prediction model; train new policies (PPO) on the best candidates; update population + Pareto archive (external archive `EP`).
4. **Pareto analysis**: identify nondominated solutions; build Pareto front approximation.
5. **Interpolation**: linearly interpolate between adjacent Pareto-optimal policies to yield a continuous front. Two interpolation approaches (hyperparameter `I`): uniform vs. fixed-uniform sampling.

### Key details
- Inner RL: PPO adapted for MORL; each policy πi sees a scalarized reward `r = w·r_vec` with preference `w`.
- Selection in the evolutionary stage uses a bandit-like or score-based rule over predicted returns.
- **Metrics**: hypervolume (convergence + diversity) and a sparsity metric (uniformity of front).

### Benchmark
- Introduced 7 MO environments with continuous action spaces (HalfCheetah, Ant, Hopper, Walker2d, Humanoid, ... with 2–3 objectives). These are the core MO-Gymnasium/PGMORL-style continuous-control tasks.

### Thesis relevance
- Canonical multi-policy MORL baseline. Useful as **baseline** against a single preference-conditioned policy (efficiency/compute argument).
- PGMORL's own benchmark = the de-facto evaluation suite for continuous-control MORL; our UR5 task must contrast against this (locomotion) line of work.

---

## 7. Envelope MOQ-Learning (16) — DEEP

**Venue**: NeurIPS 2019 · arXiv:1908.08342 · Code: github.com/RunzheYang/MORL

### Core idea
Single deep network `Q(s, a, ω)` over the **entire preference space** (preference `ω` is an input to the Q-network, output = `|A| × m` Q-values). Envelope Bellman operator `T` maximizes utility over BOTH actions and other preferences, giving theoretically faster alignment of one preference with solutions found under others.

### Formulation
- MOMDP tuple `<S, A, P, r, Ω, f_ω>`; linear preference `f_ω(r) = ω^T r`.
- Convex coverage set (CCS): subset of the Pareto frontier that maximizes some linear utility.
- Value metric `d(Q,Q') = sup_{s,a,ω} |ω^T(Q(s,a,ω) − Q'(s,a,ω))|` forms a complete **pseudo-metric** space.
- Envelope optimality filter `(HQ)(s, ω) := argQ sup_{a,ω'} ω^T Q(s,a,ω')` — optimizes over actions AND preferences (vs. scalarized Q-learning which only optimizes over actions for one fixed ω).
- Optimality operator `(TQ)(s,a,ω) := r(s,a) + γ E_{s'}[ (HQ)(s',ω) ]`.

### Theory
- **Thm 1**: `Q* = TQ*` (preferred optimal value is fixed point).
- **Thm 2**: `T` is a contraction w.r.t. `d` with Lipschitz coefficient `γ`.
- **Thm 3**: generalized Banach fixed-point → iterative application converges to `Q*`.
- Envelope updates are more sample-efficient than scalarized ones: scalarized updates can't reuse solution F (found under ω₁) to improve the ω₂-aligned solution (only searches along one direction → may converge to non-optimal L).

### Loss / training trick
- `L_A(θ) = E‖y − Q(s,a,ω;θ)‖²` (TD-style, target `y = r + γ argQ max_{a,ω'} ω^T Q(s',a,ω';θₖ)`, double-Q with target nets).
- `L_B(θ) = E|ω^T y − ω^T Q(s,a,ω;θ)|` (auxiliary utility-alignment loss).
- **Homotopy optimization**: `L(θ) = (1−λ)L_A + λ·L_B`, with λ annealed 0→1 across training. L_A keeps Q close to real expected returns; L_B pulls toward higher utility.
- **Hindsight Experience Replay**: replay each transition under multiple sampled preferences (like HER but over preference space).

### Benchmarks
- Deep Sea Treasure (2D), Fruit Tree Navigation, Task-oriented Dialog, Super Mario Bros.
- ~10% avg user-utility gain over scalarized MORL on dialog; ~2x avg on Super Mario with random preferences. Hidden-preference inference at test time with few trajectories.

### Thesis relevance
- Key reference for **single-policy preference-conditioned** MORL (family we want for a UR5, so no per-preference retraining).
- The homotopy loss and HER-over-preferences are directly reusable ideas.
- Pseudo-metric/CCS language is standard vocabulary for the lit review.

---

## 8. MO-MPO — Distributional view of MO Policy Optimization (02) — DEEP

**Venue**: NeurIPS 2022 · arXiv:2005.07513 (NOTE: correct ID; 2105.14161 is a different paper)

### Core idea
Scale-invariant MORL via an RL-as-inference perspective. Encodes preferences over objectives as **KL-divergence constraints ε_k per objective**, not scalar weights — this makes the method **invariant to objective scale** (weighted scalarization is not).

### Algorithm (two-step MO policy improvement)
Per-objective action-value functions `Q_k`. Preference encoded by per-objective KL budgets `ε_k`.

1. **Per-objective improved action distributions (λE-step)**: for each objective k, compute a non-parametric improved distribution `q_k(a|s)` that maximizes `E[Q_k]` subject to `E_μ KL(q_k ‖ π_old) < ε_k`. The Lagrange multiplier (temperature) `η_k` is solved via a convex dual function (a few gradient steps).
2. **Projection (λM-step)**: fit the parametric policy `π_θ` to all `q_k` jointly: `min_θ Σ_k w_k E_μ KL(q_k ‖ π_θ)` subject to `E_μ KL(π_old ‖ π_θ) < β` (trust region). Weights `w_k` now only control the *relative* balance, while scale-invariance comes from the ε_k constraints.

Also proposes **MO-V-MPO** (variant using a value distribution / distributional MPO). Claims to find all Pareto-optimal policies; scale-invariance analysis in a simple env; strong results on high-DoF continuous control.

### Thesis relevance
- Strong baseline and a scale-invariance argument: on a UR5 with heterogeneous objectives (reach success vs. energy vs. jerk), naive weighted scalarization is brittle to reward scale — worth citing.
- MO-MPO is off-policy; GCR-PPO is on-policy GPU-parallel — we can position our method vs. both.

---

## 9. Quick reference for remaining papers (REF / PDF)

| # | Paper | Status | One-line relevance |
|---|-------|--------|--------------------|
| 24 | UMI | PDF | In-the-wild data collection + hardware-agnostic policy interface; deployed on UR5e/Franka |
| 10 | PRC-CMORL | — | **Constrained MORL** + preference conditioning; hypervolume+entropy metrics — read next for CMDP safety |
| 26 | Open X-Embodiment / RT-X | PDF | Cross-embodiment dataset + RT-X models; positive transfer across 22 embodiments |
| 27 | Im2Flow2Act | PDF | Object-flow interface bridging human/robot and real/sim; rigid/articulated/deformable |
| 25 | DoughNet | PDF | Visual predictive model for topological manipulation of deformable objects |
| 01 | MO-Gymnasium | PDF | Standard vector-reward API + env suite; benchmark target |
| 03 | IRIS | PDF | XR teleop + point-cloud data collection across sims/robots |
| 05 | ALOHA / ACT | PDF | Action-chunking transformer baseline; multi-view RGB |
| 06 | Mobile ALOHA / ACT++ | PDF | Mobile manipulation, co-training, whole-body teleop |
| 09 | MO-Playground | — | JAX massively-parallel MORL envs; BRUCE humanoid 6-D objective space |
| 12 | MORL-Baselines | — | Library of MORL baselines following MO-Gymnasium API |
| 15 | DOL | — | First deep MORL; image-based testbed (ImageDeepSea) |
| 17 | MO-VLA | — | Empty repo; watchlist only |
| 18 | GPT-4V for Robotics | PDF | VLM multimodal task planning from demos; zero-shot |
| 19 | Vision-Tactile Manipulation | PDF | Tactile+vision fusion for soft/fragile objects |
| 20 | ALOHA Unleashed | PDF | Large-scale teleop + Diffusion Policy recipe; bimanual 6-DoF arms |
| 21 | Manipulate-Anything | PDF | VLM automated data generation; zero-shot |
| 22 | VideoManip | — | Device-free dexterous manipulation from RGB videos (DP3 policy) |
| 23 | VRB | PDF | Affordances from human videos for RL |

### Next to read deeply (priority)
1. **10-prc-cmorl** — constrained MORL, needed for the CMDP/safety part of the methodology.
2. **27-im2flow2act** — cross-domain flow interface; latest manipulation approach (PDF saved).
3. **25-doughnet** — deformable-object visual prediction; dynamic-scene relevance (PDF saved).
4. **05-aloha / 06-mobile-aloha** — ACT baseline details for comparison.

---

## UR5 hardware facts collected (for the setup section)

- End-effector-space positional commands @ 125 Hz (Diffusion Policy station), interpolated from 10 Hz policy output; velocity limited < 0.43 m/s; position kept ≥ 1 cm above table (Diffusion Policy, Appendix D.0.1).
- 5× Realsense D415 depth cameras @ 720p/30 fps; 2 used for policy obs downsampled to 320×240 @ 10 fps (Diffusion Policy).
- Teleop: 3Dconnexion SpaceMouse @ 10 Hz (Diffusion Policy).
- IRIS supports XR teleop with point clouds for UR-style arms (CoRL 2025).
- DP3 uses single-view point clouds, FPS-downsampled to 512–1024 points, no color; DP3 encoder (no T-Net/BatchNorm) — 10–20 demos per task.
- UMI: hand-held gripper + GoPro (SLAM-based action tracking); camera-centric relative-trajectory action representation; deployable on UR5e (≥85 mm jaw stroke).

## Open questions to resolve (discuss with advisor)

1. Should our method use **2D RGB (Diffusion Policy style)** or **3D point clouds (DP3 style)** as the primary observation? → my recommendation: DP3 point clouds for dynamic-env robustness, but evaluate both.
2. Safety: explicit **CMDP constraint** (PRC-CMORL style) vs. **collision-as-objective** (GCR-PPO / demo-MORL style)? → the 2×2 novelty cell is cleaner if safety is a constraint.
3. Demonstrations: incorporate as a **tuneable objective** (demo-MORL D-REX) or only for warm-start/BC initialization?
4. Which preferences do we evaluate? Static (fixed λ) + dynamic (λ drifting during episode) to demonstrate on-the-fly adaptation.
5. Sim-to-real plan: IsaacLab UR5 sim → real UR5; which cameras/topology?
