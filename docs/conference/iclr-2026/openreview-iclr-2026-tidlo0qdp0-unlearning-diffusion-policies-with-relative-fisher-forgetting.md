---
title: Unlearning Diffusion Policies with Relative Fisher Forgetting
title_zh: 基于相对费舍尔遗忘的扩散策略遗忘
authors: "Manuel Kelly, Xin Zhang, Yingxue Zhang, Fangzhou Lin, Yanhua Li"
date: 2025-09-15
pdf: "https://openreview.net/pdf?id=TidLO0qdp0"
tags: ["query:sr"]
score: 9.0
evidence: 扩散策略遗忘
tldr: 扩散策略在离线强化学习中广泛应用，但现有遗忘方法无法处理扩散模型因去噪过程而分散的训练影响。本文提出相对费舍尔遗忘（RFF）框架，通过噪声感知影响梯度和评论价值调节，首次实现扩散策略的有效遗忘。实验表明RFF能移除指定数据影响而不损害模型整体性能，为隐私保护和安全性提供了新工具。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 扩散策略的训练影响分散在去噪过程中，现有方法无法有效遗忘特定数据。
method: 提出相对费舍尔遗忘框架，结合噪声感知影响梯度和评论价值调节进行遗忘。
result: 成功移除指定数据影响，保持模型整体性能，优于基线方法。
conclusion: 首个适用于扩散策略的遗忘方法，具有重要安全与隐私意义。
---

## Abstract
Diffusion policies have recently advanced offline reinforcement learning (RL) by enabling expressive and multi-modal action generation. 
As these models move closer to real applications, it becomes important to remove the influence of specific data, either for privacy reasons, to eliminate unsafe behaviors, or to meet regulatory requirements. Existing unlearning methods, however, cannot handle diffusion-based policies because training influence is spread across the denoising process and reinforced by critic values. In this paper, we present Relative Fisher Forgetting (RFF), the first framework for unlearning in diffusion-based offline RL. RFF removes unwanted data influence through two complementary components: actor unlearning with noise aware influence gradients that are scaled by relative Fisher importance, and critic unlearning that suppresses value estimates for forgotten trajectories. To ensure stability, RFF alternates actor and critic updates and introduces gradient norm control, retain set regularization, and convergence monitoring. Experiments on MuJoCo control benchmarks for both single-task and multi-task settings show that RFF reliably removes designated trajectories and behaviors while preserving performance on retained data, outperforming retraining and prior unlearning baselines in efficacy and efficiency.

---

## 论文详细总结（自动生成）

## 论文详细中文总结

### 1. 论文的核心问题与整体含义（研究动机和背景）
- **核心问题**：扩散策略（diffusion policies）在离线强化学习（offline RL）中因其表达能力和多模态动作生成能力而广泛应用。但随着这些模型走向实际应用，需要移除特定数据的影响（例如出于隐私保护、消除不安全行为或满足监管要求）。然而，现有遗忘（unlearning）方法无法处理基于扩散的策略，因为训练影响分散在去噪过程中，并且被评论家（critic）价值估计所强化。
- **整体含义**：本文旨在填补扩散策略遗忘领域的空白，首次提出专门用于扩散策略的遗忘框架，为隐私安全和伦理合规提供新工具。

### 2. 论文提出的方法论：核心思想、关键技术细节
- **核心思想**：提出**相对费舍尔遗忘**（Relative Fisher Forgetting, RFF）框架，通过两个互补组件移除不需要的数据影响：
  1. **演员（Actor）遗忘**：使用噪声感知影响梯度（noise-aware influence gradients），并通过相对费舍尔重要性（Relative Fisher importance）进行缩放，从而在去噪过程中的各个步骤定位并消除特定数据的影响。
  2. **评论家（Critic）遗忘**：抑制对被遗忘轨迹的价值估计，确保评论家不再对不应学习的数据赋予高价值。
- **关键技术细节**：
  - 交替更新演员和评论家以保证稳定性。
  - 引入**梯度范数控制**（gradient norm control）防止遗忘更新过大。
  - **保留集正则化**（retain set regularization）维护保留数据的表现。
  - **收敛监控**（convergence monitoring）确保遗忘过程稳定终止。
- 算法流程（文字说明）：
  - 首先，定义要遗忘的数据集（如特定轨迹或行为）和保留的数据集。
  - 对于演员，计算每个去噪时间步的影响梯度，并用费舍尔信息矩阵的相对重要性权重调整梯度更新，使遗忘集中在重要参数上。
  - 对于评论家，通过降低对遗忘轨迹的价值预测误差进行更新。
  - 采用交替更新策略，并施加梯度范数约束和保留集正则化项，同时监控目标函数收敛性。
  - 最终输出遗忘后的策略模型。

### 3. 实验设计
- **数据集/场景**：使用MuJoCo控制基准，涵盖单任务（single-task）和多任务（multi-task）场景。单任务可能包括如Hopper、HalfCheetah等标准环境；多任务则涉及多个不同任务或混合数据集。
- **Benchmark**：以重训练（retraining，即从原始数据中移除遗忘数据后重新训练）作为黄金标准，同时对比先前的遗忘方法（prior unlearning baselines），但摘要未列出具体的方法名称。
- **对比方法**：重训练（Oracle）、现有遗忘基线（未详列）。评估指标包括：遗忘效果（能否移除指定轨迹/行为）、保留性能（在保留数据上的表现）以及训练效率。

### 4. 资源与算力
- 文中未明确说明所用GPU型号、数量或训练时长。这个信息在摘要及元数据中均未提及，因此无法报告具体算力消耗。

### 5. 实验数量与充分性
- **实验数量**：至少进行了单任务和多任务两种设定下的对比实验，并包含了与重训练和基线遗忘方法的比较。但摘要没有给出具体的实验数目（如若干随机种子、不同任务数量、消融研究组数）。
- **充分性与公平性**：
  - 实验覆盖了不同任务难度（单任务 vs 多任务），具有一定的广度。
  - 对比了重训练（理论上最优）和先前的遗忘方法，便于评估相对性能。
  - 但缺少对消融研究（如移除梯度范数控制、保留集正则化等组件）的详细描述，也未说明是否进行了超参数敏感性分析。此外，仅在MuJoCo基准上测试，对真实机器人或复杂高维任务（如图像输入）缺乏验证。
  - 整体来看，实验设计合理但覆盖范围有限，客观性较好但充分性一般。

### 6. 论文的主要结论与发现
- RFF框架能够可靠地移除指定的轨迹和行为，同时保持保留数据上的性能，在遗忘效果和保留性能上均优于重训练和先前遗忘基线。
- 证明了扩散策略遗忘的可行性，并展示了两组件（演员遗忘与评论家遗忘）互相增强的必要性。
- 交替更新及稳定性机制（梯度范数控制、保留集正则化、收敛监控）对于成功遗忘至关重要。

### 7. 优点
- **首创性**：第一个针对扩散策略的遗忘框架，解决了去噪过程导致训练影响分散的核心挑战。
- **双组件设计**：同时处理演员和评论家，符合离线强化学习 actor-critic 结构。
- **稳定性机制**：通过交替更新、梯度控制、正则化和收敛监控，有效防止遗忘过程崩溃，保持了模型的整体性能。
- **实验设计**：在单任务和多任务场景下均进行了评估，且与重训练（Oracle）和现有方法比较，验证了有效性。

### 8. 不足与局限
- **实验覆盖不足**：仅在MuJoCo控制基准上测试，未涉及更复杂的任务（如视觉输入、多模态场景）或真实机器人系统，结论的泛化性有待验证。
- **消融研究缺失**：摘要未说明各组件（噪声感知梯度、费舍尔重要性、评论家抑制等）的消融实验，难以确认每个组件贡献的具体大小。
- **计算与资源开销**：未报告遗忘过程的计算成本（如额外训练时间、内存使用），不利于实际部署评估。
- **偏差风险**：依赖于梯度信息，对模型架构和优化器选择可能敏感；费舍尔重要性计算在大规模模型上可能昂贵。
- **应用限制**：仅适用于 actor-critic 框架下的扩散策略，对其他生成式策略（如基于分数匹配的方法）是否通用未知。

（完）
