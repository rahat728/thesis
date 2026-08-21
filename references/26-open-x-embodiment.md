# Open X-Embodiment: Robotic Learning Datasets and RT-X Models

Source: https://robotics-transformer-x.github.io/
Date saved: 2026-08-21

Venue: arXiv 2310.08864 (Open X-Embodiment Collaboration; 21 institutions)

Links:
- arXiv: https://arxiv.org/abs/2310.08864
- Code: https://github.com/google-deepmind/open_x_embodiment
- Dataset listing: Google Sheets (dataset spreadsheet with per-dataset citations)

## Abstract

Large, high-capacity models trained on diverse datasets have shown remarkable successes on efficiently tackling downstream applications. In domains from NLP to Computer Vision, this has led to a consolidation of pretrained models, with general pretrained backbones serving as a starting point for many applications. Can such a consolidation happen in robotics? Conventionally, robotic learning methods train a separate model for every application, every robot, and even every environment. Can we instead train "generalist" X-robot policies that can be adapted efficiently to new robots, tasks, and environments? In this paper, we provide datasets in standardized data formats and models to make it possible to explore this possibility in the context of robotic manipulation, alongside experimental results that provide an example of effective X-robot policies. We assemble a dataset from 22 different robots collected through a collaboration between 21 institutions, demonstrating 527 skills (160266 tasks). We show that a high-capacity model trained on this data, which we call RT-X, exhibits positive transfer and improves the capabilities of multiple robots by leveraging experience from other platforms.

## Dataset overview

- Open X-Embodiment Dataset: the largest open-source real robot dataset to date; 1M+ real robot trajectories spanning 22 robot embodiments (single arms to bimanual and quadrupeds).
- Pooled 60 existing robot datasets from 34 robotic research labs worldwide.
- 527 skills (160,266 tasks); visually distinct scenes well-distributed across embodiments; wide range of common behaviors and household objects.

## Model overview

- Trains two models on the robotics data mixture:
  1. RT-1-X: RT-1, an efficient Transformer-based architecture designed for robotic control.
  2. RT-2-X: RT-2, a large vision-language model co-fine-tuned to output robot actions as natural-language tokens (55B params — one of the biggest models performing unseen tasks in academic labs).
- Action representation: 7-D vector (x, y, z, roll, pitch, yaw, gripper opening) or rates, expressed in the robot gripper frame; unused dimensions zeroed during training.

## Key results

- RT-1-X outperforms RT-1 / Original Methods trained on individual datasets by 50% in the small-data domain; evaluated in 6 academic labs (Berkeley RAIL/AUTOLab, Freiburg AiS, NYU CILVR, Stanford IRIS, USC CLVR).
- RT-2-X outperforms RT-2 by 3x in emergent-skill evaluations; modulates low-level behavior based on small preposition changes (e.g., "on" vs "near"), demonstrating spatial understanding in absolute and relative senses.

## Relevance to thesis (Vision-guided MORL with UR5)

- Open X-Embodiment is the canonical large-scale, cross-embodiment dataset and RT-X model family for generalist manipulation; the dataset could provide pre-training/diverse demonstration data relevant to UR5 skills.
- Demonstrates positive transfer across embodiments and environments — relevant framing for multi-task, multi-objective generalization.
- RT-2-X's language-conditioned action generation is a strong VLM-based comparison to vision-guided RL/MORL approaches.
- Context for the data-collection/dataset section of the literature review (alongside UMI, Manipulate-Anything, IRIS).
