---
title: Fine-tuning Diffusion Policies with Backpropagation Through Diffusion Timesteps
title_zh: 通过扩散时间步反向传播微调扩散策略
authors: "Ningyuan Yang, Jiaxuan Gao, Feng Gao, Yi Wu, Chao Yu"
date: 2025-09-16
pdf: "https://openreview.net/pdf?id=A2JF06XcPG"
tags: ["query:sr"]
score: 9.0
evidence: 通过扩散时间步反向传播微调扩散策略
tldr: 该文针对扩散策略在强化学习微调时动作似然估计困难的问题，提出通过扩散时间步反向传播来有效适配PPO算法。该方法使得扩散策略能够通过RL优化而不受限于似然计算，在机器人等决策任务上显著提升了性能和样本效率。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 现有RL方法难以有效适配扩散策略的动作似然估计。
method: 提出通过扩散时间步反向传播使PPO适用于扩散模型微调。
result: 在多个决策任务上提升了扩散策略的性能和样本效率。
conclusion: 该方法实现了扩散策略的有效RL微调。
---

## Abstract
Diffusion policies, widely adopted in decision-making scenarios such as robotics, gaming and autonomous driving, are capable of learning diverse skills from demonstration data due to their high representation power. However, the sub-optimal and limited coverage of demonstration data could lead to diffusion policies that generate sub-optimal trajectories and even catastrophic failures. While reinforcement learning (RL)-based fine-tuning has emerged as a promising solution to address these limitations, existing approaches struggle to effectively adapt Proximal Policy Optimization (PPO) to diffusion models. This challenge stems from the computational intractability of action likelihood estimation during the denoising process, which leads to complicated optimization objectives. In our experiments starting from randomly initialized policies, we find that online tuning of Diffusion Policies demonstrates much lower sample efficiency compared to directly applying PPO on MLP policies (MLP+PPO). To address these challenges, we introduce NCDPO, a novel framework that reformulates Diffusion Policy as a noise-conditioned deterministic policy. By treating each denoising step as a differentiable transformation conditioned on pre-sampled noise, NCDPO enables tractable likelihood evaluation and gradient backpropagation through all diffusion timesteps. This formulation enables direct optimization over the final denoised interactive actions without increasing MDP lengths. Our experiments demonstrate that NCDPO achieves sample efficiency comparable to MLP+PPO when training from scratch, outperforming existing methods in both sample efficiency and final performance across diverse benchmarks, including continuous robot control (with both state and vision inputs) and multi-agent coordination tasks. Furthermore, our experimental results show that our method is robust to the number denoising timesteps.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 核心问题与整体含义（研究动机和背景）

扩散策略（Diffusion Policies）因强大的表示能力，在机器人、游戏、自动驾驶等决策场景中被广泛采用，能够从演示数据中学习多样化技能。然而，演示数据通常存在次优性和有限覆盖，导致扩散策略生成次优轨迹甚至灾难性失败。强化学习（RL）微调是解决这一问题的有前景方案，但现有方法难以有效将近端策略优化（PPO）适配到扩散模型。根本困难在于去噪过程中动作似然估计的计算不可行，导致优化目标复杂。此外，从随机初始化策略在线调优扩散策略时，其样本效率比直接在MLP策略上应用PPO（MLP+PPO）低得多。

## 2. 方法论：核心思想、关键技术细节

**核心思想**：提出NCDPO（Noise-Conditioned Diffusion Policy Optimization）框架，将扩散策略重新定义为**噪声条件确定性策略**。通过将每个去噪步骤视为一个以预采样噪声为条件的可微变换，使得似然估计和梯度反向传播能够贯穿所有扩散时间步。

**关键技术细节**：
- 将扩散模型中的去噪过程看作一系列可微变换，每个时间步的输入是前一步输出和预采样噪声，输出是下一步去噪结果。
- 利用这一可微性，直接对最终去噪后的交互动作进行优化，而不增加MDP长度（即不将去噪步骤视为额外的时间步）。
- 实现了对动作似然的可处理评估，使PPO能够直接适配扩散策略的微调。

**算法流程（文字说明）**：
1. 预采样噪声序列（作为条件）。
2. 将扩散策略的解码器视为以噪声为条件的确定性函数，从初始噪声开始，通过多个可微去噪步骤得到最终动作。
3. 在RL训练中，使用PPO目标，并通过所有扩散时间步反向传播梯度来更新策略参数。
4. 保持MDP长度不变（仅考虑最终动作），避免传统方法中因展开去噪过程导致的复杂优化。

## 3. 实验设计

- **数据集/场景**：连续机器人控制任务（包括状态输入和视觉输入）以及多智能体协调任务。
- **Benchmark**：未明确说明具体环境，但提及了多种基准，涵盖不同输入模态（状态、视觉）和任务类型（单智能体、多智能体）。
- **对比方法**：与MLP+PPO（直接在MLP策略上应用PPO）以及现有扩散策略RL微调方法进行对比。

## 4. 资源与算力

文中未明确说明使用的GPU型号、数量或训练时长。因此在资源与算力方面信息缺失。

## 5. 实验数量与充分性

- 实验覆盖了**连续机器人控制（状态输入和视觉输入）** 和**多智能体协调**多种任务，至少包含2-3类场景。
- 进行了**与MLP+PPO的从头训练对比**，以及**与现有方法的性能对比**。
- 额外进行了**鲁棒性实验**：验证方法对去噪时间步数的鲁棒性。
- 消融实验：文中未明确列出消融实验细节，但从鲁棒性实验看有一定的分析。
- **充分性评估**：实验覆盖了不同模态和任务类型，对比了强基线MLP+PPO，较为充分。但缺乏与更多SOTA扩散策略微调方法的定量比较（文中仅提及“现有方法”），且未报告统计显著性或多次运行方差。

## 6. 主要结论与发现

- NCDPO在从头训练时实现了与MLP+PPO相当的样本效率。
- 在多个基准上（连续机器人控制、视觉输入、多智能体协调），NCDPO在样本效率和最终性能上均优于现有方法。
- 方法对去噪时间步数具有鲁棒性。

## 7. 优点

- **技术创新**：将扩散策略重新解释为噪声条件确定性策略，巧妙解决了似然不可计算的问题，使PPO可以直接适配。
- **效率提升**：从头训练时样本效率与MLP+PPO相当，远高于现有扩散策略RL微调方法。
- **通用性**：适用于多种输入模态（状态、视觉）和任务类型（单智能体、多智能体）。
- **稳定性**：对去噪步数鲁棒，降低了超参数调优难度。

## 8. 不足与局限

- **实验覆盖有限**：未在真实机器人或大规模自动驾驶场景上验证，仅限仿真环境。
- **资源信息缺失**：未提供计算资源消耗，难以评估实际部署成本。
- **对比方法不完整**：仅提及“现有方法”但未列出具体名称，缺乏与最新扩散策略微调方法（如DPO、Diffuser-based RL等）的定量比较。
- **理论分析不足**：未提供NCDPO与标准扩散策略在梯度方差、收敛性方面的理论分析。
- **偏差风险**：可能受益于精心设计的噪声采样，但未讨论噪声采样的影响。
- **应用限制**：假设扩散模型的全可微性，某些扩散模型变体（如离散扩散）可能不适用。

（完）
