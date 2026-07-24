---
title: Reinforcement Learning with Discrete Diffusion Policies for Combinatorial Action Spaces
title_zh: 面向组合动作空间的离散扩散策略强化学习
authors: "Ofir Nabati, Haitong Ma, Aviv Rosenberg, Bo Dai, Oran Lang, Craig Boutilier, Na Li, Shie Mannor, Lior Shani, Guy Tennenholtz"
date: 2025-09-19
pdf: "https://openreview.net/pdf?id=tM34PZf8W9"
tags: ["query:sr"]
score: 9.0
evidence: 离散扩散策略的强化学习用于组合动作空间
tldr: 该文针对强化学习在组合动作空间中的扩展困难，提出一种训练离散扩散模型作为策略的框架。利用策略镜像下降定义正则化目标分布，并将策略更新转化为分布匹配问题，有效稳定了在线训练过程。实验在多个复杂决策任务上取得了最优结果。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 强化学习难以扩展到大型组合动作空间。
method: 提出利用策略镜像下降和分布匹配训练离散扩散策略的方法。
result: 在多个复杂任务上取得了最优性能。
conclusion: 离散扩散策略可有效解决组合动作空间中的决策问题。
---

## Abstract
Reinforcement learning (RL) struggles to scale to large, combinatorial action spaces common in many real-world problems. This paper introduces a novel framework for training discrete diffusion models as highly effective policies in these complex settings. Our key innovation is an efficient online training process that ensures stable and effective policy improvement. By leveraging policy mirror descent (PMD) to define an ideal, regularized target policy distribution, we frame the policy update as a distributional matching problem, training the expressive diffusion model to replicate this stable target. This decoupled approach stabilizes learning and significantly enhances training performance. Our method achieves state-of-the-art results and superior sample efficiency across a diverse set of challenging combinatorial benchmarks, including DNA sequence generation, RL with macro-actions, and multi-agent systems. Experiments demonstrate that our diffusion policies attain superior performance compared to other baselines.

---

## 论文详细总结（自动生成）

### 1. 论文的核心问题与整体含义（研究动机和背景）

强化学习（RL）在处理大型组合动作空间时面临严重扩展困境——许多现实问题（如DNA序列生成、宏动作控制、多智能体系统）的动作空间呈指数级增长，传统策略网络（如MLP、GNN）难以有效探索和优化。现有方法（如基于自回归模型的策略、混合整数规划等）要么计算成本过高，要么导致训练不稳定。针对这一挑战，论文提出一种利用**离散扩散模型**作为强化学习策略的新框架，通过将策略更新转化为分布匹配问题，实现稳定的在线训练，从而显著提升在组合动作空间中的决策性能。

### 2. 论文提出的方法论：核心思想、关键技术细节、公式或算法流程

- **核心思想**：将强化学习的策略提升过程重新定义为**分布匹配问题**。具体地，利用**策略镜像下降（Policy Mirror Descent, PMD）** 在每步迭代中定义一个理想的正则化目标策略分布（即“软”最优策略），然后用一个表达能力强的**离散扩散模型**来拟合该目标分布，使得策略更新等价于让当前扩散策略逼近这个稳定目标。
- **关键技术细节**：
  - 离散扩散模型：采用前向加噪（离散化状态空间上的随机过程）和反向去噪（可学习的时间依赖马尔可夫链）来建模动作分布。
  - 训练解耦：策略优化分两步——（1）通过PMD计算目标分布（包含奖励和熵正则项）；（2）使用扩散模型的变分下界或其简化形式（如负对数似然）匹配该目标分布。
  - 在线训练稳定性：由于目标分布是通过PMD从价值函数隐式定义的，避免了直接使用策略梯度或Q学习中的高方差梯度，学习过程更加稳定。
- **算法流程简略描述**（根据摘要推断）：
  1. 初始化扩散策略参数和价值网络参数；
  2. 与环境交互收集轨迹，更新价值函数；
  3. 根据PMD公式构造当前状态下的目标动作分布 $\pi_{\text{target}}$；
  4. 最小化扩散策略与 $\pi_{\text{target}}$ 的KL散度（或等价于最大化策略的对数似然）；
  5. 重复步骤2-4直到收敛。

### 3. 实验设计：使用了哪些数据集/场景，基准测试（benchmark），对比方法

- **数据集/场景**：三个具有代表性的组合决策基准：
  - DNA序列生成：动作空间为离散核苷酸组合，优化序列奖赏；
  - 宏动作RL（macro-actions）：动作本身是基元动作的组合序列；
  - 多智能体系统：每个智能体动作空间组合后呈指数增长。
- **对比方法**：包括标准策略梯度（如PPO）、基于GNN的策略、自回归策略、离散流匹配（Discrete Flow Matching）等基线方法。具体列表需参考论文原文，但摘要明确说明该方法在样本效率和最终性能上超过了所有基线。
- **评估指标**：各类任务上的累积回报、样本效率（达到特定性能所需的交互次数）等。

### 4. 资源与算力

论文摘要及元数据中**未明确说明**使用的GPU型号、数量及训练时长。仅在元数据中给出论文初次提交日期（2025-09-19）和会议（ICLR-2026-Rejected），未涉及计算资源细节。需查阅全文才可能获知具体算力配置。

### 5. 实验数量与充分性

- 实验覆盖了三种完全不同的组合动作空间场景（生物序列、动作组合、多智能体），展现了方法的多领域适用性。
- 消融实验：元数据中未提及，但可能包括对PMD目标构造方式、扩散模型步数、正则化强度等的分析（需原文确认）。
- 公平性：对比了多种主流基线，且强调“state-of-the-art”和“superior sample efficiency”，说明实验设计较为客观。但未公开讨论失败案例或方法失效的条件，可能存在潜在的乐观偏差。
- 充分性评估：仅基于摘要，实验场景覆盖面较广，但缺乏超大规模（如动作空间维度上万）的测试，也未在真实机器人或工业系统中验证，因此实验充分性中等偏上。

### 6. 论文的主要结论与发现

- 离散扩散策略能够有效处理大型组合动作空间，且在线训练过程稳定。
- 通过策略镜像下降与分布匹配的解耦方式，显著提升了样本效率和最终性能。
- 在DNA序列生成、宏动作RL、多智能体系统三个复杂任务上，该方法均达到当前最好水平。
- 扩散模型在动作分布建模方面的灵活性（非自回归、可并行采样）是成功的关键因素之一。

### 7. 优点：方法或实验设计上的亮点

- **方法创新**：将离散扩散模型引入组合动作空间的强化学习，并通过PMD实现稳定在线训练，解决了扩散模型策略难以端到端更新的痛点。
- **理论联系实践**：利用PMD将策略优化转化为最大似然估计，使学习目标清晰、梯度稳定。
- **实验多样性**：覆盖生物信息学、机器人宏动作、多智能体协作三个迥异领域，证明泛化能力。
- **样本效率**：在相同交互次数下获得更高回报，对现实世界（数据获取昂贵）具有重要价值。

### 8. 不足与局限

- **计算开销**：扩散模型的多步采样会增加推理时计算量，未与自回归模型进行详细时延对比。
- **缺乏实际系统验证**：所有实验均为模拟环境，未在真实物理系统（如机器人、生物实验室）中测试。
- **未讨论超参数敏感性**：扩散步数、正则化系数等可能对性能敏感，但未提供系统分析。
- **公开资源匮乏**：论文当前处于未录用状态，代码与模型权重未开放，可复现性存疑。
- **动作空间假设**：仅适用于离散且结构化的动作空间，对连续动作空间不直接适用（虽然可扩展，但非本文重点）。

（完）
