# Mobile ALOHA: Learning Bimanual Mobile Manipulation with Low-Cost Whole-Body Teleoperation

Source: https://mobile-aloha.github.io/
Date saved: 2026-08-02

Venue: CoRL 2024

Authors: Zipeng Fu, Tony Z. Zhao, Chelsea Finn (Stanford University)

Links:
- Paper: ./resources/mobile-aloha.pdf (on original page)
- arXiv: http://arxiv.org/abs/2401.02117
- Tutorial: https://docs.google.com/document/d/1_3yhWjodSNNYlpxkRCPIlvIAaQ76Nqk2wsqhnEVM6Dc
- Datasets: https://drive.google.com/drive/folders/1FP5eakcxQrsHyiWBRDsMRvUfSxeykiDc
- Hardware Code: https://github.com/MarkFzp/mobile-aloha
- ML Code: https://github.com/MarkFzp/act-plus-plus
- Chinese version: https://mobile-aloha.github.io/cn.html

## Abstract

Imitation learning from human demonstrations has shown impressive performance in robotics. However, most results focus on table-top manipulation, lacking the mobility and dexterity necessary for generally useful tasks. In this work, we develop a system for imitating mobile manipulation tasks that are bimanual and require whole-body control. We first present Mobile ALOHA, a low-cost and whole-body teleoperation system for data collection. It augments the ALOHA system with a mobile base, and a whole-body teleoperation interface. Using data collected with Mobile ALOHA, we then perform supervised behavior cloning and find that co-training with existing static ALOHA datasets boosts performance on mobile manipulation tasks. With 50 demonstrations for each task, co-training can increase success rates by up to 90%, allowing Mobile ALOHA to autonomously complete complex mobile manipulation tasks such as sauteing and serving a piece of shrimp, opening a two-door wall cabinet to store heavy cooking pots, calling and entering an elevator, and lightly rinsing a used pan using a kitchen faucet.

## Key contributions

- Mobile ALOHA: low-cost whole-body teleoperation system (ALOHA + mobile base).
- Supervised behavior cloning + co-training with static ALOHA datasets boosts performance on mobile manipulation.
- 50 demonstrations per task; co-training can increase success rates by up to 90%.
- Autonomous tasks: saute/serve shrimp, open two-door cabinet, enter elevator, rinse pan with faucet.

## BibTeX

```
@inproceedings{fu2024mobile,
  author    = {Fu, Zipeng and Zhao, Tony Z. and Finn, Chelsea},
  title     = {Mobile ALOHA: Learning Bimanual Mobile Manipulation with Low-Cost Whole-Body Teleoperation},
  booktitle = {{Conference on Robot Learning (CoRL)}},
  year      = {2024},
}
```

## Relevance to thesis (Vision-guided MORL with UR5)

- ACT++ (act-plus-plus) is a strong baseline / backbone for visuomotor policies; widely reused for manipulation learning.
- Whole-body teleoperation and co-training (static + mobile data) matter if your UR5 system is mobile or augmented.
- Relevant to dynamic-environment manipulation where mobility + closed-loop vision are required.
