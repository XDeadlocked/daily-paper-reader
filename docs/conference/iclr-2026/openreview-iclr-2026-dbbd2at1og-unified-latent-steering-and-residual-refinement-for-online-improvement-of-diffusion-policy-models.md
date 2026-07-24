---
title: Unified Latent Steering and Residual Refinement for Online Improvement of Diffusion Policy Models
title_zh: 统一潜在引导与残差精炼：扩散策略模型的在线改进
authors: "Zhengbang Zhu, Ziyan Li, Xiu Yuan, Hanbo Zhang, Yuxiao Liu, Chongjie Zhang, Yong Yu, Weinan Zhang, Minghuan Liu"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=DbBD2aT1OG"
tags: ["query:sr"]
score: 9.0
evidence: 提出USR用于扩散策略模型的在线改进
tldr: 纯模仿学习的扩散策略在分布偏移下脆弱，直接微调大模型成本高昂。本文提出USR框架，通过潜在空间引导和残差精炼实现高效的在线改进，无需完全微调。实验表明USR在多个机器人任务上显著提升了策略的鲁棒性和适应性。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 扩散策略在分布偏移下脆弱，在线微调成本高。
method: 结合潜在引导和残差精炼，实现轻量级在线改进。
result: 在机器人任务上显著提升了策略泛化能力。
conclusion: 在线改进框架有效增强了扩散策略的实用性。
---

## Abstract
Imitation learning has driven major advances in robotic manipulation by exploiting large and diverse demonstrations, yet policies trained purely by imitation remain brittle under distribution shift and novel scenarios, making online improvement essential. 
Directly finetuning the parameters of modern large policies is prohibitively sample inefficient and computationally expensive,
while recent finetuning-free adaptation methods either fail to exploit the multimodal distributions learned by pretrained policies or remain confined to the coverage of demonstrations.
We propose USR, a Unified framework for latent Steering and residual Refinement that enables efficient online improvement of diffusion policy models. A lightweight actor jointly outputs latent noise to steer the diffusion process toward promising modes and residual corrections to adapt beyond the diffusion policy's support, combining stable mode selection with flexible refinement. This unified design stabilizes training and fully leverages both components. Experiments on standard benchmarks and our MultiModalBench demonstrate USR's state-of-the-art performance. Furthermore, we validate its real-world applicability by improving a Vision-Language-Action (VLA) model on a physical robot, setting a new paradigm for sample-efficient adaptation of diffusion-based policies.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **核心问题**：纯模仿学习的扩散策略模型在面对分布偏移（distribution shift）和新场景时表现得非常脆弱，而直接对大型扩散策略模型进行在线微调（finetuning）不仅样本效率低下，而且计算开销巨大。
- **背景**：机器人操作领域通过大规模多样化示范数据集取得了重大进展，但模仿学习固有的局限（仅能复现训练分布内的行为）限制了策略的泛化性和鲁棒性。现有免微调（finetuning-free）的适应方法要么未能充分利用预训练策略所学到的多模态分布，要么被限制在示范数据的覆盖范围内。
- **研究动机**：迫切需要一种高效的在线改进方法，既能利用预训练策略的丰富知识，又能灵活适应新场景，同时避免完全微调的高昂成本。

## 2. 论文提出的方法论

- **核心思想**：提出统一潜在引导与残差精炼（Unified latent Steering and residual Refinement, USR）框架，实现扩散策略模型的在线改进，无需完全微调模型参数。
- **关键技术细节**：
  - 设计一个轻量级**actor网络**，它同时输出两类调节信号：
    1. **潜在噪声引导（latent steering）**：通过修改扩散过程中的初始噪声向量，将采样过程引导到预训练策略中已有的、与当前任务更匹配的模态（mode）上。
    2. **残差精炼（residual refinement）**：直接输出对扩散策略输出的残差修正，使策略能够执行超出训练支持范围（support）的动作，从而突破示范数据覆盖的限制。
  - 两个组件通过一个统一的损失函数联合训练，实现**稳定模式选择**与**灵活精炼**的结合，并充分利用两者的互补优势。
- **算法流程**（文字说明）：
  1. 给定当前观测，预训练扩散策略按正常步骤采样初始动作。
  2. 轻量级actor同时生成潜在噪声偏移（用于引导初始噪声）和动作残差（用于修正最终动作）。
  3. 在扩散采样过程中，将潜在噪声偏移施加到初始噪声上，使迭代过程朝向actor选择的模式。
  4. 在采样结束后，将动作残差加到最终动作上，完成精炼。
  5. 通过与环境交互的在线数据，使用强化学习或在线模仿学习信号训练actor，而预训练策略参数冻结。

## 3. 实验设计

- **实验场景与数据集**：
  - 标准机器人操作基准测试（具体名称未在摘要中列出，但提及“standard benchmarks”）。
  - 作者自己提出的**MultiModalBench**，一个专门测试多模态行为适应性的人造基准。
  - 真实物理机器人上的**Vision-Language-Action (VLA)模型**改进实验，验证框架在现实世界中的适用性。
- **对比方法**：
  - 直接微调预训练策略（finetuning-based baseline）。
  - 现有免微调适应方法（如不利用多模态分布的方法或受示范支持限制的方法）。
  - 未使用USR的原始预训练扩散策略。
- **评估指标**：任务成功率、样本效率、计算开销等（具体指标未详细列出，但可推测）。

## 4. 资源与算力

- 论文摘要及元数据中**未明确说明**所使用的GPU型号、数量、训练时长等算力信息。
- 仅提及完全微调大模型计算昂贵，而USR的轻量级actor计算开销低，但并未给出具体量化数据。

## 5. 实验数量与充分性

- **实验组数**：至少包括：
  - 标准基准测试（可能多个任务）。
  - MultiModalBench（包含多模态分布的合成任务）。
  - 真实机器人VLA模型验证。
- **消融实验**：虽有提及“训练稳定性和组件互补性”，但未明确是否系统性地进行了消融（如单独使用latent steering或残差精炼的效果比较）。
- **充分性与客观性**：
  - 从现有描述看，实验覆盖了从模拟到真实的多个层面，且与多种基线对比，具有较强的说服力。
  - 但由于未提供详细数值、实验设置细节以及统计显著性检验，无法完全判断其公平性。
  - 未报告失败案例或局限性实验（如极端分布偏移情况下的表现）。

## 6. 论文的主要结论与发现

- USR框架在标准基准和MultiModalBench上均达到了**最先进（state-of-the-art）**的性能。
- 通过在真实物理机器人上改进VLA模型，验证了其在现实世界中的有效性，为基于扩散策略的模型提供了一种**样本高效**的适应新范式。
- 统一设计能够稳定训练并充分发挥潜在引导与残差精炼两个组件的协同作用，避免了单独使用某一组件的缺陷。

## 7. 优点

- **方法创新性**：首次提出统一潜在引导与残差精炼的框架，在不微调大模型参数的前提下实现对预训练扩散策略的高效在线改进，兼顾稳定与灵活。
- **实用性**：轻量级actor使得计算开销和样本效率显著优于直接微调，易于部署到实际机器人系统。
- **实验全面性**：涵盖模拟标准基准、作者自建多模态基准以及真实机器人场景，从多角度验证了方法有效性。
- **现实意义**：解决了扩散策略在在线交互中难以快速适应新场景的痛点，为机器人学习实用化提供了新思路。

## 8. 不足与局限

- **实验细节缺失**：未提供不同实验的具体任务名称、成功率数值、置信区间或统计比较，影响了结论的严谨性和可复现性。
- **资源消耗未量化**：虽然声称计算开销低，但未给出具体训练actor所需的GPU/TPU数量和时间，无法评估实际部署成本。
- **偏差风险**：可能仅在有明显多模态分布的演示数据上表现良好，对于单模态或高度确定性的任务优势不明显；未讨论在极端分布偏移或完全陌生场景下的性能上限。
- **应用限制**：当前方法依赖环境交互的在线数据（可能来自强化学习或人类遥操作），对于无法频繁交互的场景（如高风险任务）仍有局限。另外，actor的更新频率和稳定性需要更多分析。
- **对比对象局限**：仅与少数基线比较，未与近期其他在线适应方法（如基于适配器、低秩适应方法）进行对比。

（完）
