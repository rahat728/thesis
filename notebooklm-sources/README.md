# NotebookLM Source Bundle — Vision-guided MORL (UR5)

Plain-text extracts of the papers for uploading to NotebookLM. Generated 2026-08-02 from PDFs in `papers/` via `pdftotext`. Tier assignments reordered by relevance 2026-08-21 (most-relevant-first).

## Tier 1 (core — method backbones & closest work)
| File | Paper |
|------|-------|
| `tier1/11-demo-enhanced-morl-nav.txt` | Demonstration-Enhanced Adaptable Multi-Objective Robot Navigation |
| `tier1/08-gcr-ppo.txt` | Scalable Multi-Objective Robot RL through Gradient Conflict Resolution |
| `tier1/07-diffusion-policy.txt` | Diffusion Policy: Visuomotor Policy Learning via Action Diffusion |
| `tier1/04-3d-diffusion-policy.txt` | 3D Diffusion Policy (DP3) |

**Note**: Tier 1 also lists PRC-CMORL (10) in the original plan, but no PDF is saved in `papers/` yet. The reference note is at `references/10-prc-cmorl.md`; extract text from the arXiv PDF when available and place as `tier1/10-prc-cmorl.txt`.

## Tier 2 (supporting)
| File | Paper |
|------|-------|
| `tier2/14-pareto-visual-navigation.txt` | Navigating the Wild: Pareto-Optimal Visual Decision-Making in Image Space |
| `tier2/16-envelope-morl.txt` | A Generalized Algorithm for MORL and Policy Adaptation |
| `tier2/02-mo-mpo.txt` | A Distributional View on Multi-Objective Policy Optimization |
| `tier2/13-pgmorl.txt` | Prediction-Guided MORL for Continuous Robot Control |
| `tier2/01-mo-gymnasium.txt` | A Toolkit for Reliable Benchmarking and Research in MORL |
| `tier2/24-umi.txt` | Universal Manipulation Interface (UMI) |
| `tier2/27-im2flow2act.txt` | Flow as the Cross-domain Manipulation Interface (Im2Flow2Act) |
| `tier2/25-doughnet.txt` | DoughNet: A Visual Predictive Model for Topological Manipulation of Deformable Objects |
| `tier2/26-open-x-embodiment.txt` | Open X-Embodiment: Robotic Learning Datasets and RT-X Models |

## Upload tips
- Upload each `.txt` as a separate source in NotebookLM.
- Ask NotebookLM for: (1) a comparison table of MORL algorithms, (2) methodology details of GCR-PPO vs. PD-MORL, (3) UR5 hardware/setup facts, (4) open research gaps at the vision × MORL intersection.
- Beware: pdftotext output contains layout artifacts (line-break math, duplicated text). For 02-mo-mpo.txt there are "Badly formatted number" warnings but text is usable.
