---
title: Iterative Refinement of Flow Policies in Probability Space for Online Reinforcement Learning
title_zh: 概率空间中流策略的迭代细化用于在线强化学习
authors: "Mingyang Sun, Pengxiang Ding, Weinan Zhang, Donglin Wang"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=qf9c1rQoXG"
tags: ["query:sr"]
score: 8.0
evidence: 流策略的迭代细化用于在线强化学习
tldr: 本文提出步进流策略SWFP框架，将流匹配推理过程离散化为固定步长欧拉步骤，并证明其与最优传输中的JKO变分原理一致。SWFP将全局流分解为小增量变换，使得在线强化学习可以对流/扩散策略进行微调，解决了行为克隆中的分布偏移问题。实验显示SWFP在复杂运动任务中显著提升了策略性能。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 行为克隆的流/扩散策略易受分布偏移影响，标准RL难以微调迭代推理模型。
method: SWFP框架将流匹配离散化为与JKO原理对齐的欧拉步，分解为小增量变换。
result: 在多个运动控制任务上，SWFP实现了有效的在线RL微调，提升了策略性能。
conclusion: SWFP为流/扩散策略的在线强化学习提供了理论基础和实用框架。
---

## Abstract
While behavior cloning with flow/diffusion policies excels at learning complex skills from demonstrations, it remains vulnerable to distributional shift, and standard RL methods struggle to fine-tune these models due to their iterative inference process and the limitations of existing workarounds. In this work, we introduce the Stepwise Flow Policy (SWFP) framework, founded on the key insight that discretizing the flow-matching inference process via a fixed-step Euler scheme inherently aligns it with the variational Jordan–Kinderlehrer–Otto (JKO) principle from optimal transport. SWFP decomposes the global flow into a sequence of small, incremental transformations between proximate distributions. Each step corresponds to a JKO update, regularizing policy changes to stay near the previous iterate and ensuring stable online adaptation with entropic regularization. This decomposition yields an efficient algorithm that fine-tunes pre-trained flows via a cascade of small flow blocks, offering significant advantages: simpler/faster training of sub-models, reduced computational/memory costs, and provable stability grounded in Wasserstein trust regions. Comprehensive experiments demonstrate SWFP's enhanced stability, efficiency, and superior adaptation performance across diverse robotic control benchmarks.

---

## 论文详细总结（自动生成）

# 论文总结：概率空间中流策略的迭代细化用于在线强化学习（SWFP）

## 1. 核心问题与整体含义（研究动机和背景）
- **背景**：基于行为克隆（Behavior Cloning, BC）的流/扩散策略虽然在从演示中学习复杂技能方面表现出色，但容易受到**分布偏移（distributional shift）** 的影响——当测试状态与训练演示分布不一致时，策略性能急剧下降。
- **问题**：标准强化学习（RL）方法难以直接微调这类迭代推理模型（如流匹配、扩散模型），因为它们的推理过程是多步迭代的，且已有的变通方法（如行为正则化、重参数化）存在效率低下或理论不清晰的问题。
- **核心目标**：提出一种在**在线强化学习**中有效微调预训练流策略的框架，在保持稳定性的同时提升策略的适应性能。

## 2. 论文提出的方法论：核心思想、关键技术细节
- **核心思想**：将流匹配推理过程离散化为**固定步长的欧拉步**，并证明该过程与最优传输中的**Jordan–Kinderlehrer–Otto（JKO）变分原理**自然对齐。通过将全局流分解为一系列小增量变换（每一步对应一次JKO更新），实现稳定在线适配。
- **关键技术细节**：
  - **Stepwise Flow Policy (SWFP)** 框架：将预训练的连续流策略分解为多个**小的流块（flow blocks）**，每个块对应一个从当前分布到相邻分布的JKO步骤。
  - **正则化机制**：每个JKO更新都通过**熵正则化（entropic regularization）** 限制策略变化幅度，确保新策略保持在上一轮策略的Wasserstein信任域内，从而避免分布偏移导致的灾难性失败。
  - **算法流程**（文字说明）：
    1. 预训练一个流匹配策略（如通过行为克隆）。
    2. 将流推理过程离散化为N个等步长的欧拉步，得到N个子流块。
    3. 在在线RL微调阶段，依次或联合微调这些子流块，每个子流块的更新都受JKO约束。
    4. 利用熵正则化项控制每一步的策略变化，保证整体微调的稳定性。
  - **优势**：子模型训练更简单/更快、计算和内存成本降低、在Wasserstein信任域内具有可证明的稳定性。

## 3. 实验设计
- **数据集/场景**：多个**机器人控制基准**（robotic control benchmarks），具体环境未在摘要中列出，但元数据标注为“多个运动控制任务”，推测包括D4RL、MuJoCo等标准连续控制任务。
- **Benchmark**：未明确列出，但对比方法应包括：
  - 标准行为克隆（BC）流/扩散策略（无微调）。
  - 已有的在线微调方法（如基于奖励的BC、基于扩散策略的RL变体等）。
  - 可能还对比了其他基于扩散/流的RL方法（如Diffuser、Decision Diffuser等）。
- **对比方法**：摘要未详细列出，但暗示与“标准RL方法”和“现有变通方法”对比。

## 4. 资源与算力
- **文中未明确说明**使用的GPU型号、数量、训练时长等具体算力信息。仅从元数据看，论文是ICLR 2026被拒稿（但似乎为公开版本），可能因篇幅限制未列出硬件细节。

## 5. 实验数量与充分性
- **数量**：摘要中仅提到“comprehensive experiments”，并在多个机器人控制基准上验证，未给出具体实验组数。但元数据提到“experiments show SWFP's enhanced stability, efficiency, and superior adaptation performance”。
- **充分性**：虽然没有详细消融实验清单，但从摘要判断至少包含了：
  - 不同复杂度的运动控制任务对比。
  - 与基准方法的性能比较。
  - 可能包含消融实验（如步长大小、正则化强度的影响）。
- **客观性与公平性**：论文声称实验“comprehensive”，且方法具有理论保证（JKO对齐），但因为没有提供详细结果表格，无法完全判断是否进行了公平的超参数调优和统计验证。

## 6. 论文的主要结论与发现
- SWFP框架为流/扩散策略的在线强化学习微调提供了**理论基础**（JKO变分原理）和**实用框架**。
- 通过分解流为小增量变换，SWFP显著提升了策略的稳定性，有效克服了行为克隆中的分布偏移问题。
- 在多个运动控制任务上，SWFP实现了**有效的在线RL微调**，策略性能优于原始BC策略和传统RL微调方法。
- 该框架还降低了计算和内存开销，使得迭代推理模型更容易部署。

## 7. 优点（方法或实验设计的亮点）
- **理论创新**：首次将流匹配离散化与最优传输中的JKO变分原理明确对齐，为策略微调的稳定性提供了经过证明的保证（Wasserstein信任域）。
- **实用性**：将全局流拆解为小流块，不仅降低了单步更新的难度，还允许利用熵正则化进行自然约束，避免复杂的额外KL约束计算。
- **效率提升**：子模型训练简单、速度快、内存需求低，适用于实际机器人学习场景。

## 8. 不足与局限
- **实验覆盖有限**：摘要未提及具体任务名称和结果数值，且未分析在图像、语言等更复杂连续控制任务上的表现。
- **偏差风险**：方法依赖预训练流策略的质量，若预训练策略本身很差，微调效果可能受限。
- **应用限制**：假设流推理过程可离散化为等步长欧拉步，对于非欧拉格式的扩散模型（如DDIM、解析逆过程）可能需要额外适配。
- **算力信息缺失**：未提供实验硬件和计算成本，难以评估方法的实际资源需求。
- **对比方法不充分**：未明确列出所有对比基线，且未提及与最新在线扩散策略方法（如在线Diffuser、MSR等）的对比。

（完）
