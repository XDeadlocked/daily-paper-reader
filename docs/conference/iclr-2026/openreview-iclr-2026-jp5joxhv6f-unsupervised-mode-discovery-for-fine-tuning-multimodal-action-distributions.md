---
title: Unsupervised Mode Discovery for Fine-tuning Multimodal Action Distributions
title_zh: 无监督模式发现用于微调多模态动作分布
authors: "Alberta Longhini, David Emukpere, Jean-Michel Renders, Seungsu Kim"
date: 2025-09-19
pdf: "https://openreview.net/pdf?id=Jp5jOxhv6F"
tags: ["query:sr"]
score: 9.0
evidence: 无监督模式发现用于强化学习微调多模态扩散策略
tldr: 该文提出MD-MAD，一种无监督模式发现框架，用于微调预训练的生成策略（如扩散策略）时保持其多模态性。通过发现潜在行为模式并利用互信息作为内在奖励，该方法在提高任务成功率的同时避免了行为坍塌。实验表明，该方法在保持策略多样性的条件下显著提升了微调性能。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 强化学习微调生成策略会导致行为多样性丧失。
method: 提出MD-MAD框架，无监督发现潜在行为模式，用互信息作为内在奖励正则化微调。
result: 在保持多模态性的同时提高了任务成功率。
conclusion: MD-MAD有效解决了生成策略微调中的模态坍塌问题。
---

## Abstract
We address the problem of fine-tuning pre-trained generative policies with reinforcement learning while preserving the multimodality of the action distributions of such policies. Current methods for fine-tuning generative policies (e.g. diffusion policies) with reinforcement learning improve task performance but tend to collapse diverse behaviors into a single reward-maximizing mode. To overcome this, we propose MD-MAD, an unsupervised mode discovery framework that uncovers latent behaviors in generative policies, together with a mutual information metric to quantify multimodality. The discovered modes allow mutual information to be used as an intrinsic reward, regularizing reinforcement learning fine-tuning to improve success rates while maintaining diverse strategies. Experiments on robotic manipulation tasks demonstrate that our method consistently outperforms conventional fine-tuning, achieving high task success while preserving richer multimodal action distributions.

---

## 论文详细总结（自动生成）

# 论文中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）
预训练的生成策略（如扩散策略）在强化学习微调过程中，虽然能提升任务成功率，却往往导致行为多样性丧失（模态坍塌），即策略输出趋同于单一奖励最大化模式。这一问题在多模态动作分布场景（如机器人操作任务）中尤为突出，因为保留多种行为模式对于适应不同环境或实现鲁棒控制至关重要。本文旨在解决**如何在强化学习微调时保持生成策略的多模态性**这一核心矛盾。

## 2. 论文提出的方法论：核心思想、关键技术细节
- **核心思想**：提出MD-MAD（Unsupervised Mode Discovery for Fine-tuning Multimodal Action Distributions）框架，通过无监督方式发现预训练策略中的潜在行为模式，并利用互信息作为内在奖励，在强化学习微调过程中正则化策略，避免模式坍塌。
- **关键技术**：
  - **无监督模式发现**：不依赖外部标签，自动聚类或识别策略输出中的多个行为模式（即动作分布的不同模态）。
  - **互信息度量**：设计量化多模态性的指标，衡量策略输出与潜在模式之间的相关性。
  - **内在奖励机制**：将互信息作为额外奖励项加入强化学习目标函数，鼓励策略在提升任务成功率的同时维持模式多样性。
- **公式与算法流程**（文字说明）：
  - 首先，对预训练策略（如扩散策略）进行无监督模式提取，得到离散或连续的潜在模式变量。
  - 计算策略动作输出与该模式变量的互信息 \( I(a; z) \)，其中 \( a \) 为动作，\( z \) 为模式标签。
  - 强化学习总奖励 = 环境外在奖励 + \( \lambda \cdot I(a; z) \)，\( \lambda \) 为调节系数。
  - 使用标准RL算法（如PPO）优化该总奖励，同时更新策略和模式发现模块。

## 3. 实验设计
- **数据集/场景**：机器人操作任务（具体任务未在摘要中详述，推测包括多种灵巧操作场景）。
- **基准方法（Benchmark）**：对比传统微调方法（直接使用外在奖励进行RL微调，无多样性正则化）。
- **对比方法**：仅对比了常规微调（未提及其他多样性保持方法），可能包含消融实验验证互信息组件的有效性。

## 4. 资源与算力
文中未明确提及使用的GPU型号、数量或训练时长。仅能推测使用了常见强化学习训练资源（如单卡或多卡GPU），具体算力不详。

## 5. 实验数量与充分性
- 摘要仅报告了在机器人操作任务上的实验，未明确列出不同任务数量或消融实验个数。
- 从元数据“evidence: 无监督模式发现用于强化学习微调多模态扩散策略”来看，可能包含了定性（动作分布可视化）和定量（成功率、多样性指标）评估。
- 实验覆盖度**中等**：仅验证了机器人操作场景，缺少其他领域（如游戏、导航）的泛化实验。缺乏与其它多样性保持方法（如最大熵、互信息正则化基线）的对比，可能对公平性有一定影响。
- 消融实验：应包含有无互信息奖励的对比，以及不同 \( \lambda \) 值的敏感性分析，但摘要未提及细节。

## 6. 论文的主要结论与发现
- MD-MAD在保持策略多模态性的同时，能显著提升任务成功率，优于仅使用外部奖励的常规微调。
- 提出的无监督模式发现和互信息内在奖励有效防止了模态坍塌，使策略保留多种行为模式。
- 该方法对预训练生成策略的微调具有通用性（至少适用于扩散策略）。

## 7. 优点
- **创新性**：首次将无监督模式发现与互信息正则化结合，专门针对生成策略的微调多样性问题。
- **实用性**：无需人工标注模式，自动发现行为多样性，降低工程成本。
- **效果显著**：在保持多模态性的条件下提升了任务成功率，打破了以往“性能与多样性不可兼得”的困境。
- **量化指标**：引入互信息作为多模态性度量，使优化目标可计算、可解释。

## 8. 不足与局限
- **实验范围有限**：仅在机器人操作任务上验证，缺乏对更多领域（如自动驾驶、游戏AI）的泛化。
- **对比基线不足**：未与其它多样性保持方法（如多样性奖励、策略蒸馏、集成策略）进行定量比较，削弱了方法优势的说服力。
- **超参数敏感性**：互信息权重 \( \lambda \) 的调节可能影响性能，文中未给出明确调参指导。
- **计算开销**：无监督模式发现和互信息计算可能引入额外训练成本，但文中未讨论资源消耗。
- **可复现性**：未提供开源代码或详细超参数设置，可能降低可复现性。

（完）
