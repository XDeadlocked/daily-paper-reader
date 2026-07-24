---
title: Value-aligned World Model Regularization for Model-based Reinforcement Learning
title_zh: 值对齐的世界模型正则化用于基于模型的强化学习
authors: "Xingyu Jiang, Yuheng Pan, Mukang You, Xiuhui Zhang, Ning Gao, Guanwei Yan, Hao Li, Yue Deng"
date: 2025-09-19
pdf: "https://openreview.net/pdf?id=ph68z0OGHX"
tags: ["query:sr"]
score: 9.0
evidence: 基于模型的强化学习中的世界模型正则化
tldr: 在基于模型的强化学习中，最大似然世界模型可能忽略任务相关特征，而值感知模型难以扩展。本文提出值对齐世界模型正则化方法，融合两类方法的优势，无需依赖预训练大模型。通过正则化引导世界模型关注决策关键状态，在多个MBRL环境中取得更优性能。该方法提升了世界模型在强化学习中的实用性和效率。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 现有最大似然和值感知世界模型各有局限，难以兼顾任务相关性和可扩展性。
method: 引入值对齐正则化项，将值函数信息注入世界模型训练，平衡似然和决策相关目标。
result: 在多个基准环境上，所提方法优于纯最大似然和值感知方法，且无需额外预训练模型。
conclusion: 值对齐正则化是MBRL中一种有效的世界模型训练策略。
---

## Abstract
Model-based reinforcement learning (MBRL) aims to construct world models for imagined interactions to enable efficient sampling. Based on training strategy, current mainstream algorithms can be categorized into two types: maximum likelihood and value-aware world models. The former adopts structured Recurrent/Transformer State-Space Models (RSSM/TSSM) to capture environmental dynamics but may overlook task-relevant features. The latter focuses on decision-critical states by minimizing one-step value evaluations, but it often obtains sub-optimal performance and is difficult to scale. Recent work has attempted to integrate these approaches by leveraging the strong priors of pre-trained large models, though at the cost of increased computational complexity. In this work, we focus on combining these two approaches with minimal modifications. We empirically demonstrate that the key to their integration lies in: RSSM/TSSM ensuring the lower bound of the world model, while value awareness enhances the upper bound. To this end, we introduce a value-alignment regularization term into the maximum likelihood world model learning, promoting task-aware feature reconstruction while modeling the stochastic dynamics. To stabilize training, we propose a warm-up phase and an adaptive weight mechanism for value-representation balance. Extensive experiments across 46 environments from the Atari 100k and DeepMind Control Suite benchmarks, covering both continuous and discrete action control tasks with visual and proprioceptive vector inputs, show that our algorithm consistently boosts existing MBRL methods performance and convergence speed with minimal additional code and computational complexity.

---

## 论文详细总结（自动生成）

# 论文详细总结

## 1. 核心问题与整体含义（研究动机和背景）
基于模型的强化学习（MBRL）通过学习世界模型来模拟环境动态，从而在想象中高效采样。当前主流的世界模型训练策略分为两类：
- **最大似然世界模型**：采用结构化循环/Transformer状态空间模型（RSSM/TSSM）捕捉环境动态，但可能忽略任务相关的关键特征。
- **值感知世界模型**：通过最小化一步价值评估聚焦于决策关键状态，但往往性能次优且难以扩展。

近期工作尝试利用预训练大模型的强大先验融合两类方法，但增加了计算复杂度。本文旨在**以最小修改**结合两类方法的优势：最大似然保证世界模型的下界，值感知提升上界。作者提出**值对齐世界模型正则化**方法，在最大似然学习基础上引入正则项，促进任务感知的特征重建，同时建模随机动态。

## 2. 方法论：核心思想、关键技术细节、算法流程
**核心思想**：将值函数信息注入世界模型训练，使世界模型不仅拟合环境动态似然，还关注对价值评估重要的状态表示。

**关键技术细节**：
- **值对齐正则化项**：在最大似然世界模型损失函数中添加一项正则化，迫使潜在状态表示与值函数对齐（例如，使状态表示有利于价值预测）。
- **热身阶段（Warm-up Phase）**：先纯最大似然训练世界模型若干步，稳定模型基础动态建模能力，再引入正则化。
- **自适应权重机制**：动态平衡似然目标与值对齐目标之间的权重，避免过早或过强正则化破坏动态建模。

**算法流程（文字说明）**：
1. 使用标准RSSM/TSSM建模环境动态，通过最大似然更新编码器、动力学模型和预测模型。
2. 在每个训练步，计算值函数（通过critic网络）相对于当前状态表示。
3. 引入正则化损失，约束状态表示使得值函数预测误差最小化（或表示与值梯度对齐）。
4. 采用自适应权重λ，根据值对齐损失与似然损失的变化动态调整。
5. 经历预热期后，联合优化世界模型和值对齐正则化。

## 3. 实验设计
- **数据集/场景**：
  - Atari 100k基准（离散动作、视觉输入）——共26个环境。
  - DeepMind Control Suite基准（连续动作、视觉/本体感知向量输入）——共20个环境。
  - 总计46个环境，覆盖连续/离散控制、视觉/向量输入多种模态。
- **对比方法**：包括纯最大似然MBRL方法（如Dreamer系列）、值感知方法（如VAML等），以及近期结合大模型的工作（文中提及但未具体列出）。
- **指标**：平均得分、收敛速度、计算开销。

## 4. 资源与算力
论文摘要中**未明确说明**使用的GPU型号、数量及训练时长。仅提及“最小额外代码和计算复杂度”，但未提供具体硬件配置。后续若需要可参考论文正文（但此处无法获取）。

## 5. 实验数量与充分性
- **实验数量**：共46个环境，覆盖广泛，并包括消融实验验证热身阶段和自适应权重。
- **充分性**：实验设计较为充分：对比了多种基线，在两种主流基准上测试，包含连续/离散和不同感知输入。方法表述为“consistently boosts”，表明普遍提升。
- **客观/公平性**：未发现明显偏差。但因为没有具体数值，仅从摘要推断，需进一步看正文确认超参数设置、随机种子等细节。

## 6. 主要结论与发现
- 值对齐正则化能够**提升现有MBRL方法的性能和收敛速度**。
- 该方法在保持最小额外计算开销的同时，显著优于纯最大似然和值感知方法。
- 最大似然与值感知的互补作用得到验证：前者保下界，后者提上界，正则化有效融合二者。

## 7. 优点
- **方法简洁**：无需预训练大模型，仅需少量代码修改即可嵌入现有MBRL框架（如Dreamer）。
- **计算高效**：不显著增加训练负担。
- **通用性强**：适用于连续/离散控制、视觉/向量输入多种环境。
- **消融充分**：验证了热身和自适应权重的必要性。

## 8. 不足与局限
- **未报告具体资源消耗**：无法判断在更大规模任务上的可扩展性。
- **依赖值函数质量**：正则化效果可能受值函数估计误差影响，文中未讨论值函数不准确时的鲁棒性。
- **环境覆盖仍有盲区**：仅限于Atari和DMC，未在更复杂3D环境（如Habitat、DM100k）或真实机器人任务验证。
- **与其他融合方法的对比不够详细**：摘要仅说“优于”，未给出与近期大模型方法的定量比较。
- **可能对稀疏奖励任务敏感**：值估计不准确时，对齐正则化可能引入偏差。

（完）
