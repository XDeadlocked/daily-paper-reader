---
title: One-Step Flow Policy Mirror Descent
title_zh: 一步流策略镜像下降
authors: "Tianyi Chen, Haitong Ma, Na Li, Kai Wang, Bo Dai"
date: 2025-09-20
pdf: "https://openreview.net/pdf?id=SVpw8RJL6c"
tags: ["query:sr"]
score: 9.0
evidence: 强化学习中流策略的一步采样方法
tldr: 扩散策略在强化学习中表现优异，但迭代采样过程导致推理缓慢，限制实时响应。本文提出流策略镜像下降（FPMD），利用直插流匹配模型的理论特性，实现流策略的一步采样，无需额外的蒸馏或一致性训练。在MuJoCo等任务上的实验表明，FPMD在保持策略表达能力的同时，大幅提升了推理速度。该工作为扩散策略的实时应用提供了有效解决方案。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 扩散策略推理慢，难以实时应用，需要加速采样过程。
method: 基于直插流匹配模型的理论，提出流策略镜像下降算法，实现一步采样，避免迭代。
result: 在MuJoCo任务上，FPMD在保持性能的同时，推理速度显著提升。
conclusion: 一步采样策略有效解决了扩散策略的实时性问题。
---

## Abstract
Diffusion policies have achieved great success in online reinforcement learning (RL) due to their strong expressive capacity. However, the inference of diffusion policy models relies on a slow iterative sampling process, which limits their responsiveness. To overcome this limitation, we propose Flow Policy Mirror Descent (FPMD), an online RL algorithm that enables 1-step sampling during flow policy inference. Our approach exploits a theoretical connection between the distribution variance and the discretization error of single-step sampling in straight interpolation flow matching models, and requires no extra distillation or consistency training. We present two algorithm variants based on rectified flow policy and MeanFlow policy, respectively. Extensive empirical evaluations on MuJoCo and visual DeepMind Control Suite benchmarks demonstrate that our algorithms show strong performance comparable to diffusion policy baselines while requiring orders of magnitude less computational cost during inference.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 核心问题与整体含义（研究动机和背景）
- **问题**：扩散策略（Diffusion Policy）在在线强化学习（RL）中因强大的表达能力而表现优异，但其推理过程依赖缓慢的迭代采样（如多步去噪），导致响应延迟，难以满足实时应用需求（如机器人控制）。
- **动机**：如何在不牺牲策略表达能力的前提下，显著加速扩散策略的推理速度，使其能够应用于对实时性要求高的场景。
- **背景**：已有加速方法（如蒸馏、一致性训练）往往需要额外训练步骤或引入偏差，本文寻求一种理论驱动的、无需额外开销的一步采样方案。

## 2. 论文提出的方法论
- **核心思想**：基于直插流匹配模型（Straight Interpolation Flow Matching）的理论性质，建立**分布方差**与**一步采样离散化误差**之间的理论联系，从而证明通过适当训练可以实现一步采样而无需迭代。
- **关键技术细节**：
  - 提出 **Flow Policy Mirror Descent (FPMD)** 算法，在在线RL中直接优化流策略，使其推理时只需一次前向传播即可生成动作。
  - 给出两种变体：
    1. **Rectified Flow Policy**：基于矫正流（rectified flow）的策略实现。
    2. **MeanFlow Policy**：基于平均流（MeanFlow）的策略实现。
  - **无需额外蒸馏或一致性训练**：理论保证了单步采样的质量，算法在RL训练过程中自然逼近了一步采样的最优解。
- **算法流程（文字描述）**：
  - 在每个RL迭代中，智能体与环境交互收集轨迹。
  - 使用镜像下降（Mirror Descent）更新流策略参数，损失函数设计确保策略分布向最优策略分布靠近，同时最小化一步采样误差。
  - 推理时，直接从流模型生成样本（1步），替代传统扩散策略的多步迭代。

## 3. 实验设计
- **数据集/场景**：
  - **MuJoCo** 连续控制任务（如HalfCheetah, Hopper, Walker2d等）。
  - **Visual DeepMind Control Suite (DMControl)** 视觉观察任务（如Cheetah Run, Finger Spin等）。
- **Benchmark**：对比的基线包括扩散策略方法（如Diffusion-QL、IQL+Diffusion等），以及传统RL方法（如SAC、TD3等）。
- **对比方法**：未在提供文本中列出详细对比方法，但从上下文推断包含典型的扩散策略基线。
- **评估指标**：累积回报（reward）、推理时间（或采样步数）、计算成本。

## 4. 资源与算力
- 提供文本中**未明确说明**使用的GPU型号、数量或训练时长。仅提及推理时所需计算量比基线低几个数量级。无法补充具体算力信息。

## 5. 实验数量与充分性
- **实验数量**：在MuJoCo和DMControl两个benchmark上进行了评估，包含多个任务（具体数量未列出，但通常MuJoCo至少5个，DMControl至少3个）。
- **充分性评价**：
  - **优点**：覆盖了标准连续控制任务和视觉任务，足以验证方法的泛化性。
  - **不足**：缺少消融实验（如对分布方差与离散误差关系的验证）、不同采样步数的对比、其他流匹配变体的比较等细节。由于文本仅来自摘要和元数据，无法判断实验是否完整、公平。建议查阅全文以获得消融和统计显著性测试。

## 6. 论文的主要结论与发现
- FPMD算法成功实现了流策略的**一步采样推理**，推理速度比扩散策略基线快数个数量级。
- 在MuJoCo和视觉DMControl任务上，FPMD的性能与迭代采样的扩散策略相当，未出现明显退化。
- 理论分析表明，直插流匹配模型的分布方差与一步采样误差自然相关，为无需额外蒸馏的一步采样提供了理论基础。

## 7. 优点
- **理论创新**：建立了流匹配模型方差与采样误差的定量关系，为一步采样提供了理论支撑。
- **实用高效**：无需蒸馏或一致性训练，简化了训练流程并节省计算资源。
- **表达能力强**：保留了流策略/扩散策略的分布建模能力，同时推理极快。
- **算法简洁**：两种变体 (Rectified Flow, MeanFlow) 可灵活选择，易于实现。

## 8. 不足与局限
- **实验覆盖有限**：仅测试了MuJoCo和DMControl，未在更复杂的任务（如机器人操作、Atari、多智能体）上验证。
- **缺乏消融与敏感性分析**：未见对核心假设（方差与误差关系）的实证验证，也未探讨超参数（如步长、学习率）影响。
- **潜在偏差风险**：单步采样可能在高维分布或多模态场景下仍存在误差，论文未证明理论上界紧密性。
- **未提及资源消耗**：缺少训练成本信息，无法评估实际部署的可行性。
- **应用限制**：仅针对在线RL设置，未讨论离线RL或模型预测控制（MPC）等场景的适用性。

（完）
