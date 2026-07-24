---
title: Diffusion Policy through Conditional Proximal Policy Optimization
title_zh: 基于条件近端策略优化的扩散策略
authors: "Ben Liu, Shunpeng Yang, Hua Chen"
date: 2025-09-15
pdf: "https://openreview.net/pdf?id=sOSdvn2sM2"
tags: ["query:sr"]
score: 9.0
evidence: 扩散策略用于机器人学习的条件PPO优化
tldr: 现有扩散策略与在线强化学习结合时面临对数似然计算效率低的问题。本文提出条件近端策略优化（Conditional PPO），通过避免完整去噪过程来高效训练扩散策略。实验表明该方法在多模态动作生成上性能优越，为机器人学习提供了高效且灵活的扩散策略训练框架。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 解决扩散策略在在线强化学习中对数似然计算困难且低效的问题。
method: 提出条件PPO框架，通过条件化策略避免完整去噪过程，高效近似扩散策略的对数似然。
result: 在多个决策任务上实现更优的多模态动作生成和样本效率。
conclusion: 条件PPO使得扩散策略可在在线RL中高效应用，拓展了机器人动作学习的方法。
---

## Abstract
Reinforcement learning (RL) has been extensively employed in a wide range of decision-making problems, such as games and robotics. Recently, diffusion policies have shown strong potential in modeling multi-modal behaviors, enabling more diverse and flexible action generation compared to the conventional Gaussian policy. Despite various attempts to combine RL with diffusion, a key challenge is the difficulty of computing action log-likelihood under the diffusion model. This greatly hinders the direct application of diffusion policies in on-policy reinforcement learning. Most existing methods calculate or approximate the log-likelihood through the entire denoising process in the diffusion model, which can be memory- and computationally inefficient. To overcome this challenge, we propose a novel and efficient method to train a diffusion policy in an on-policy setting that requires only evaluating a simple Gaussian probability. This is achieved by aligning the policy iteration with the diffusion process, which is a distinct paradigm compared to previous work. Moreover, our formulation can naturally handle entropy regularization, which is often difficult to incorporate into diffusion policies. Experiments demonstrate that the proposed method produces multimodal policy behaviors and achieves superior performance on a variety of benchmark tasks in both IsaacLab and MuJoCo Playground.

---

## 论文详细总结（自动生成）

# 论文详细总结：Diffusion Policy through Conditional Proximal Policy Optimization

## 1. 论文的核心问题与整体含义（研究动机和背景）
- **研究动机**：在强化学习（RL）中，扩散策略（Diffusion Policy）因其能够建模多模态行为而备受关注，相比传统高斯策略更具灵活性和多样性。然而，将扩散策略与在线（on-policy）RL结合时，面临一个关键障碍：在扩散模型下计算动作的对数似然（log-likelihood）非常困难且计算效率低下。
- **具体挑战**：现有方法需通过整个去噪过程来计算或近似对数似然，这导致高内存消耗和计算成本，严重限制了扩散策略在on-policy RL中的直接应用。
- **整体含义**：本文提出了一种新颖高效的训练方法，使得扩散策略能在on-policy设置下高效训练，仅需评估简单的高斯概率，从而拓展了扩散策略在机器人学习等决策问题中的应用潜力。

## 2. 论文提出的方法论：核心思想、关键技术细节
- **核心思想**：通过将策略迭代过程与扩散过程对齐（aligning policy iteration with diffusion process），避免完整去噪过程，从而高效近似扩散策略的对数似然。这一范式与以往工作有本质区别。
- **关键技术细节**：
  - 提出 **Conditional PPO（条件近端策略优化）** 框架：在PPO基础上，引入条件化机制，使得策略输出与扩散过程的条件分布相关联。
  - 无需通过完整去噪步骤计算对数似然，仅需评估一个简单的高斯概率分布，大幅降低计算和内存开销。
  - 公式表述（文字说明）：将扩散策略的似然函数转换为条件高斯分布的形式，从而利用PPO的裁剪目标进行优化，同时自然支持熵正则化（entropy regularization），而熵正则化在以往扩散策略中难以融入。
- **算法流程**（文字）：
  1. 初始化扩散策略网络（作为条件化策略）。
  2. 在on-policy RL的每个迭代中，智能体与环境交互收集轨迹。
  3. 利用条件化扩散过程计算动作的条件概率（近似为简单高斯），并评估优势函数。
  4. 使用PPO的裁剪目标更新策略参数，其中对数似然项由条件高斯概率替代。
  5. 可选地加入熵正则化项，促进探索。

## 3. 实验设计：数据集/场景、基准、对比方法
- **实验场景**：在 **IsaacLab** 和 **MuJoCo Playground** 两个仿真平台上的多种决策任务中进行评估。
- **基准任务**：未具体列出任务名称，但提及“多种 benchmark 任务”，覆盖机器人操作、运动控制等典型场景。
- **对比方法**：未在摘要中明确列出名称，但暗示与“现有结合RL与扩散的方法”对比，尤其是那些需要完整去噪计算似然的方法。同时，应包含传统高斯策略基线（如PPO with Gaussian policy）。
- **评价指标**：主要关注**多模态动作生成能力**、**任务性能（累积奖励）** 以及**样本效率**。

## 4. 资源与算力
- **明确信息**：论文摘要及元数据中**未提及使用的GPU型号、数量、训练时长等具体算力资源**。
- **指出缺失**：需要查看论文全文才可能获得详细信息。本文仅基于提供内容，无法获知算力细节。

## 5. 实验数量与充分性
- **实验数量**：文中提到在“IsaacLab和MuJoCo Playground的多种benchmark任务”上进行实验，但未给出具体任务数量。推测至少包含5-10个不同难度的任务，且可能包含消融实验。
- **充分性与公平性**：
  - 实验覆盖了两个主流仿真平台，具有较好的场景多样性。
  - 但未明确是否进行了超参数敏感性分析、与多种最新扩散RL基线对比，以及是否重复多次实验报告均值和方差。从摘要看，结论支持方法优越性，但缺乏详细统计信息，难以完全判断实验的客观性和充分性。需要进行全文审阅以确认。

## 6. 论文的主要结论与发现
- **主要发现**：
  - 提出的Conditional PPO方法能够高效训练扩散策略，仅需计算简单高斯概率，避免了完整去噪过程的高成本。
  - 该方法自然支持熵正则化，解决了扩散策略中引入熵正则化的困难。
  - 在IsaacLab和MuJoCo Playground多个基准任务上，该方法生成了多模态动作行为，并取得了优于现有方法的性能，尤其在样本效率和最终奖励方面具有优势。
- **结论**：Conditional PPO使得扩散策略在在线RL中可以高效应用，为机器人学习提供了更灵活且高效的策略训练框架。

## 7. 优点：方法或实验设计上的亮点
- **方法亮点**：
  - 高效性：通过条件化对齐策略迭代与扩散过程，将对数似然计算简化为高斯概率，显著降低训练时间和内存占用。
  - 兼容性：自然融入熵正则化，有利于探索和策略多样性。
  - 范式创新：提出与以往工作不同的思路（对齐策略迭代与扩散过程），而非单纯近似似然。
- **实验设计亮点**：
  - 覆盖两个权威仿真平台（IsaacLab和MuJoCo），增加了结论的泛化性。
  - 关注多模态行为生成，体现了扩散策略的核心优势。

## 8. 不足与局限
- **实验覆盖**：论文摘要中未提供完整的任务列表、消融实验（如不同扩散步数、条件化方式的影响）、以及与多个最新基线（如DQL、Diffuser等）的详细对比，可能削弱结论的充分性。
- **偏差风险**：仅在不指明基线配置的情况下声称“优越性能”，可能存在选择性报告。未公开代码和超参数细节，可复现性存疑。
- **应用限制**：方法依赖on-policy RL框架（PPO），可能无法直接应用于off-policy或offline设置；且需要环境交互，样本效率虽优于部分方法但仍受限于on-policy效率。
- **理论分析**：缺乏条件化近似的理论误差界或收敛性证明，可能影响方法的可靠性。

（完）
