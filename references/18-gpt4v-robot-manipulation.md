# GPT-4V(ision) for Robotics: Multimodal Task Planning from Human Demonstration

Source: https://microsoft.github.io/GPT4Vision-Robot-Manipulation-Prompts/
Date saved: 2026-08-02

Authors: Naoki Wake, Atsushi Kanehira, Kazuhiro Sasabuchi, Jun Takamatsu, Katsushi Ikeuchi (Applied Robotics Research, Microsoft, Redmond)

Links:
- arXiv: https://arxiv.org/abs/2311.12015
- Code: https://github.com/microsoft/GPT4Vision-Robot-Manipulation-Prompts
- Related (task planner): https://github.com/microsoft/ChatGPT-Robot-Manipulation-Prompts

## Abstract

We introduce a pipeline that enhances a general-purpose Vision Language Model, GPT-4V(ision), by integrating observations of human actions to facilitate robotic manipulation. This system analyzes videos of humans performing tasks and creates executable robot programs that incorporate affordance insights. The computation starts by analyzing the videos with GPT-4V to convert environmental and action details into text, followed by a GPT-4-empowered task planner. In the following analyses, vision systems reanalyze the video with the task plan. Object names are grounded using an open-vocabulary object detector, while focus on the hand-object relation helps to detect the moment of grasping and releasing. This spatiotemporal grounding allows the vision systems to further gather affordance data (e.g., grasp type, way points, and body postures). Experiments across various scenarios demonstrate this method's efficacy in achieving real robots' operations from human demonstrations in a zero-shot manner.

## Pipeline

- Video Analyzer: GPT-4V converts video frames into a one-sentence textual instruction (human-to-human form).
- Scene Analyzer: produces a Python-dictionary scene description (objects, object_properties, spatial_relations, explanation).
- Task Planner: GPT-4 generates a task plan from the instruction + scene description.
- Vision systems re-analyze video for spatiotemporal grounding; open-vocabulary object detection; hand-object contact to detect grasp/release moments; affordance data (grasp type, way points, body postures).

## Experiments

- Video grounding on drawer / shelf relocation tasks.
- Robot execution on a SEED-noid robot (first-person head camera); trajectories defined relative to object position; arm postures via inverse kinematics; some skills (e.g., grasp) trained with RL.
- End-to-end success rate: 85-95% across various operations from 20 human demonstrations each.
- Benefits: robustness to environments differing from teaching env (affordance-based); reusability across object instances.

## Relevance to thesis (Vision-guided MORL with UR5)

- Example of vision-language model (VLM) driving robot manipulation from human demonstrations (zero-shot).
- Relevant as a related-work/alternative for the "vision-guided" component; also demonstrates combining multiple vision models + task planning.
- GPT-4V-based perception is an alternative to learned visuomotor policies (Diffusion Policy etc.).
