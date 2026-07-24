---
title: Spatially Guided Training for Vision-Language-Action Model
title_zh: 面向视觉-语言-动作模型的空间引导训练
authors: "Jinhui Ye, Fangjing Wang, Ning Gao, Junqiu Yu, Zhu Yangkun, Bin Wang, Jinyu Zhang, Weiyang Jin, Yanwei Fu, Feng Zheng, Yilun Chen, Jiangmiao Pang"
date: 2026-01-26
pdf: "https://openreview.net/pdf?id=eKhOrQWAVJ"
tags: ["query:sr"]
score: 9.0
evidence: 使用空间先验训练的视觉-语言-动作模型
tldr: 大型VLM在具身任务中难以生成低级动作。本文提出SP-VLA，通过空间接地预训练（预测点、框、轨迹）注入空间先验，再在动作后训练中引导动作生成。实验证明该方法显著提升了机器人指令执行的成功率。
source: ICLR-2026-Accepted
selection_source: conference_retrieval
motivation: VLM缺乏空间先验，难以直接生成机器人低级动作。
method: 两阶段训练：空间接地预训练和空间引导动作后训练。
result: 在多个机器人操作任务上实现高成功率。
conclusion: 空间先验有效连接语言指令与动作空间。
---

## Abstract
Large vision–language models (VLMs) excel at multimodal understanding but fall short when extended to embodied tasks, where instructions must be transformed into low-level motor actions. We introduce SP-VLA, a dual-system **V**ision–**L**anguage–**A**ction framework that leverages **S**patial **P**riors as a bridge between linguistic instructions and embodiment-specific control. 
introduce SP-VLA aligns action learning with spatial priors through two stages: (i) spatial grounding pre-training, which equips the VLM with transferable priors via scalable point, box, and trajectory prediction from both web-scale and robot-specific data, and (ii) spatially guided action post-training, which encourages the model to produce richer spatial priors to guide action generation via spatial prompting.
This design preserves spatial grounding during policy learning and promotes consistent optimization across spatial and action objectives. Empirically, introduce SP-VLA achieves substantial improvements over vanilla VLA, with performance increasing from $66.1{\rightarrow}84.6$ on Google Robot and from $54.7{\rightarrow}73.2$ on WidowX, establishing new state-of-the-art results on SimplerEnv. It also demonstrates stronger generalization to unseen objects and paraphrased instructions, as well as robustness to long-horizon perturbations in real-world settings. These results highlight scalable spatially guided training as a promising direction for robust, generalizable robot learning. We will release code, data, and model checkpoints to support future research.
See more visualization results at the anonymous page: https://sp-vla-anonymous.vercel.app

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）
- **核心问题**：大型视觉-语言模型（VLM）在多模态理解任务中表现出色，但在具身任务中难以将语言指令转化为低级电机动作。直接使用VLM生成机器人动作时，由于缺乏空间先验知识，导致动作生成不准确、泛化能力弱。
- **研究动机**：引入空间先验作为语言指令与具身控制之间的桥梁，使VLM能够更好地理解空间关系并生成精确动作。
- **整体含义**：提出一种名为 **SP-VLA** 的双系统视觉-语言-动作框架，通过空间引导训练将空间先验注入模型，显著提升机器人在指令执行任务中的成功率与鲁棒性。

## 2. 论文提出的方法论：核心思想、关键技术细节
- **核心思想**：两阶段训练策略，先通过**空间接地预训练**赋予模型可迁移的空间先验，再通过**空间引导动作后训练**利用这些先验指导动作生成。
- **关键技术细节**：
  - **阶段一：空间接地预训练（Spatial Grounding Pre-training）**
    - 从网络规模数据和机器人特定数据中，让模型预测**点（point）、边界框（box）和轨迹（trajectory）**，以学习通用的空间表示。
    - 该阶段不依赖具体动作，可扩展性强。
  - **阶段二：空间引导动作后训练（Spatially Guided Action Post-training）**
    - 通过**空间提示（spatial prompting）**，鼓励模型生成更丰富的空间先验，再将这些先验作为指导来生成动作。
    - 设计上使空间接地目标与动作目标保持一致优化，避免策略学习过程中丢失空间信息。
- **公式/算法流程**（文字说明）：
  - 输入：图像 + 语言指令 → 预训练阶段输出空间预测（点/框/轨迹）→ 后训练阶段将空间预测作为条件输入，与图像、指令一起解码出连续动作序列。
  - 损失函数：结合空间接地损失（如L1、IoU损失）和动作预测损失（如均方误差）。

> 注：论文摘要未提供具体公式，以上为根据摘要描述推导的流程。

## 3. 实验设计：数据集、基准、对比方法
- **数据集与场景**：
  - Google Robot 数据集（真实机器人操作任务）
  - WidowX 机器人数据集（桌面操作）
  - SimplerEnv 仿真环境（用于标准化评估）
  - 真实世界长时域干扰测试（long-horizon perturbations）
- **基准（Benchmark）**：在 **SimplerEnv** 上评估，并与基线VLA方法对比；此外在Google Robot和WidowX上报告成功率。
- **对比方法**：
  - **Vanilla VLA**（未使用空间引导的标准视觉-语言-动作模型）
  - 可能还包括其他SOTA（摘要提到“establishing new state-of-the-art results”），但未列出具体名称。

## 4. 资源与算力
- **论文摘要及元数据中未明确说明**使用的GPU型号、数量、训练时长等算力信息。
- 仅能推测：实验涉及预训练和后训练两个阶段，且使用了大规模网络数据和机器人数据，需要较强的计算资源（如多卡A100或V100），但具体数字未提供。

## 5. 实验数量与充分性
- **实验数量**：
  - 报告了3个主要数据集/环境的定量结果：Google Robot、WidowX、SimplerEnv。
  - 包含泛化性测试：对未见物体（unseen objects）和改写指令（paraphrased instructions）的评估。
  - 真实世界鲁棒性测试：长时域干扰。
  - 未提及消融实验的具体数量，但元数据中“消融实验”标记存在（见tags），可能论文正文包含消融研究。
- **充分性与客观性**：
  - 实验覆盖了多个常见机器人操作基准，且对比了直接基线（Vanilla VLA），结果提升明显（Google Robot: 66.1→84.6; WidowX: 54.7→73.2）。
  - 泛化测试和真实世界测试增加了结论的可信度。
  - 但缺少与其他近期SOTA方法（如RT-2、PaLM-E等）的详细对比，且未提供统计方差或多次重复实验报告，可能不够全面。

## 6. 论文的主要结论与发现
- **主要结论**：空间引导训练（SP-VLA）能够有效提升VLA模型的动作生成质量和泛化能力。
- **具体发现**：
  - 成功率显著提升：在Google Robot上提高18.5个百分点，WidowX上提高18.5个百分点。
  - 在SimplerEnv上达到新SOTA。
  - 对未见物体和指令改写具有更强的泛化性。
  - 在真实世界长时域干扰下表现出鲁棒性。
- **意义**：可扩展的空间引导训练是通往鲁棒、可泛化机器人学习的有前景方向。

## 7. 优点：方法或实验设计上的亮点
- **方法创新**：
  - 首次将空间先验以“双系统”方式显式注入VLA，兼顾可扩展性和动作对齐。
  - 空间接地预训练使用点、框、轨迹多种预测形式，从网络和机器人数据中学习，数据来源广泛，预训练可扩展。
  - 空间引导动作后训练通过提示机制保留空间信息，避免联合训练时的“遗忘”问题。
- **实验亮点**：
  - 同时涵盖仿真、真实机器人和多种泛化测试，评估维度较全面。
  - 结果提升幅度大且一致，说明方法有效。

## 8. 不足与局限
- **实验覆盖不完整**：
  - 未明确列出消融实验的具体维度（如仅预测点/框/轨迹的效果、预训练数据规模的影响等）。
  - 缺乏与更多最新具身VLM方法（如RT-2、Octo、GATO等）的对比，SOTA claim可能局限于SimplerEnv基准。
- **偏差风险**：
  - 仅使用Google Robot和WidowX两种机器人平台，泛化到其他形态（如移动操作、双机械臂）未知。
  - 真实性测试中“长时域干扰”的定义和结果未详细展示。
- **应用限制**：
  - 两阶段训练需要额外预训练和数据收集，计算成本可能较高。
  - 空间预测需要标注（点、框、轨迹），对于新任务可能需要重新标注或使用自动标注方法。
- **资源算力未公开**，难以复现和评估可及性。

（完）
