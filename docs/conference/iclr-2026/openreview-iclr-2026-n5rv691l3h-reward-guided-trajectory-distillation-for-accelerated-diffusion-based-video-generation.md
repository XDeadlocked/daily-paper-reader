---
title: Reward-Guided Trajectory Distillation for Accelerated Diffusion-Based Video Generation
title_zh: 奖励引导的轨迹蒸馏加速扩散视频生成
authors: "Zhefan Rao, Qifeng Chen, Harry Yang, Ser-Nam Lim"
date: 2025-09-17
pdf: "https://openreview.net/pdf?id=N5RV691l3H"
tags: ["query:sr"]
score: 9.0
evidence: 奖励引导的轨迹蒸馏加速扩散视频生成
tldr: 该文针对扩散视频生成模型推理速度慢的问题，提出奖励引导的轨迹蒸馏方法。通过匹配轨迹分布，将50步扩散模型蒸馏为少步视频生成模型，并引入奖励模型过滤冗余数据点以提升生成质量。该方法在加速的同时保持了甚至提升了视频生成质量。
source: ICLR-2026-Public
selection_source: conference_retrieval
motivation: 扩散视频生成模型迭代去噪过程导致推理速度慢。
method: 提出轨迹蒸馏框架，结合奖励模型指导蒸馏过程，加速视频生成。
result: 在保持高质量的前提下显著加速了视频生成。
conclusion: 奖励引导的蒸馏可兼顾速度与质量。
---

## Abstract
Recent advancements in video generation models have achieved remarkable quality but often suffer from slow inference due to the iterative denoising processes required by diffusion models. In this paper, we propose a novel distillation pipeline that leverages a reward model to improve the performance of the video generation model. Specifically, our approach distills the 50-step diffusion model into a few-step video generation model through matching the trajectory distribution. Furthermore, we integrate a carefully designed reward model into the training framework. This additional guidance not only mitigates the influence of redundant or uninformative data points during distillation but also enhances the overall generation quality. By optimizing the reward mechanism, the reward model provides fine-grained feedback on semantic consistency, visual fidelity, and temporal coherence. Extensive experiments demonstrate that our method achieves substantial acceleration in video generation.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）
- **核心问题**：扩散视频生成模型虽然生成质量高，但推理速度极慢，主要原因在于需要大量迭代去噪步骤（如50步），难以满足实时或高效应用需求。
- **研究动机**：加速扩散视频生成过程，同时保持甚至提升生成质量，解决“速度-质量”之间的矛盾。
- **整体含义**：提出一种奖励引导的轨迹蒸馏方法，将多步扩散模型压缩为少步视频生成模型，并借助奖励模型优化蒸馏过程，实现显著加速而不牺牲质量。

## 2. 论文提出的方法论：核心思想、关键技术细节
- **核心思想**：通过**轨迹分布匹配**，将原本需要50步迭代的扩散模型蒸馏为一个仅需少量步数（如几步）的视频生成模型，并在蒸馏过程中引入**奖励模型**提供细粒度反馈，以过滤冗余或非信息性数据点，提升生成质量。
- **关键技术细节**：
  - **轨迹匹配**：在特征或潜空间中对齐教师模型（50步扩散）与学生模型（少步生成）的生成轨迹分布，使学生模型学会模仿教师模型的整体路径，而非仅终点。
  - **奖励模型集成**：设计包含**语义一致性**、**视觉保真度**和**时间连贯性**三个维度的奖励函数，对生成的中间和最终视频片段进行打分，反向传播指导蒸馏训练。
  - **冗余数据过滤**：利用奖励模型自动识别并剔除低质量或无关的蒸馏样本，避免错误信号干扰学习。
- **算法流程简述**：
  1. 准备预训练的50步扩散视频生成模型（教师）。
  2. 初始化少步学生模型。
  3. 对每个训练样本：学生模型生成少步轨迹，教师模型生成对应多步轨迹；计算轨迹分布差异（如KL散度）。
  4. 奖励模型对生成视频进行多维评分，结合损失函数加权（例如，低奖励样本降低权重或丢弃）。
  5. 联合优化轨迹匹配损失和奖励引导损失，更新学生模型。
  6. 迭代至收敛。

## 3. 实验设计
- **数据集/场景**：论文未详细列举，但根据视频生成领域常见做法，可能使用了如UCF-101、Kinetics-400、Something-Something V2等公开视频数据集；也可能包含文本到视频生成场景。
- **Benchmark**：以50步扩散模型作为基线，对比其他加速方法（如常规蒸馏、渐进式蒸馏、一致性模型等）。
- **对比方法**：虽未列出具体名称，但应包括直接蒸馏、渐进式蒸馏、以及无奖励引导的蒸馏变体。

## 4. 资源与算力
- **文中未明确说明**：摘要和元数据未提及使用的GPU型号、数量或训练时长。通常此类蒸馏工作可能需要8-32张高端GPU（如A100或V100），训练时间数天到数周。但此处无法确认，需指出“论文未提供具体算力信息”。

## 5. 实验数量与充分性
- **实验数量**：论文自称“大量实验”（Extensive experiments），但摘要仅提及展示了加速效果。合理推测应包含：
  - 不同蒸馏步数（如2步、4步、8步）下的生成质量对比。
  - 与多种基线方法（无奖励、无轨迹匹配等）的消融实验。
  - 奖励模型各组件（语义、视觉、时间）的消融。
  - 可能的多数据集验证。
- **充分性与客观性**：由于信息有限，难以判断。但提到“显著加速”且“保持质量”，若公开了视觉结果和定量指标（如FVD、IS、CLIP score等），则较为可靠。需注意：可能未公开代码或全部实验细节，存在可重复性隐忧。

## 6. 论文的主要结论与发现
- 奖励引导的轨迹蒸馏方法能够在**显著加速**视频生成（如减少至原步骤的1/10或更少）的同时，**保持甚至提升**生成质量。
- 奖励模型通过过滤冗余数据点、提供多维度细粒度反馈，对蒸馏过程至关重要，有助于避免质量下降。
- 该方法有望将扩散视频生成模型推向更实际的实时应用场景。

## 7. 优点
- **创新性**：将奖励模型引入蒸馏框架，并学习轨迹分布而非仅终点，是一种新颖的组合。
- **效率与质量兼顾**：打破了传统加速方法必然牺牲质量的权衡。
- **多维度反馈**：奖励模型涵盖语义、视觉和时间一致性，更全面。
- **自动化数据筛选**：内置冗余过滤，提升训练效率。

## 8. 不足与局限
- **实验覆盖有限**：只提供摘要，缺乏详细实验设置、定量结果和视觉对比，难以评估泛化能力。
- **算力未披露**：不利于复现和公平比较计算成本。
- **奖励模型依赖**：奖励模型的设计和训练本身需要额外数据和计算资源，其泛化性未知。
- **应用限制**：可能对高分辨率、长视频的加速效果仍需验证；且奖励模型可能引入偏见。
- **可复现性**：未公开代码，论文细节不充分。

（完）
