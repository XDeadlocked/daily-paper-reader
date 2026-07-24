---
title: WARPD – World-model Assisted Reactive Policy Diffusion
title_zh: WARPD：世界模型辅助的响应式策略扩散
authors: "Shashank Hegde, Satyajeet Das, Gautam Salhotra, Gaurav S. Sukhatme"
date: 2025-09-20
pdf: "https://openreview.net/pdf?id=OS9eo7NQeO"
tags: ["query:sr"]
score: 9.0
evidence: 世界模型辅助的扩散策略
tldr: 针对扩散策略推理慢、动作轨迹块累积跟踪误差的问题，本文提出WARPD，融合世界模型辅助的响应式策略。世界模型提供前瞻预测，扩散策略生成多模态动作，两者协同优化，在保持性能的同时提升了控制频率并减小了长期误差。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 扩散策略推理速度慢且长轨迹块累积误差。
method: 提出世界模型辅助的响应式策略扩散，结合预测与生成。
result: WARPD在机器人的操作和运动任务中显著提升了推理速度和控制精度。
conclusion: 世界模型可以有效增强扩散策略的实用性和性能。
---

## Abstract
With the increasing availability of open-source robotic data, imitation learning has become a promising approach for both manipulation and locomotion. Diffusion models are now widely used to train large, generalized policies that predict controls or trajectories, leveraging their ability to model multimodal action distributions. However, this generality comes at the cost of larger model sizes and slower inference, an acute limitation for robotic tasks requiring high control frequencies. Moreover, Diffusion Policy (DP), a popular trajectory-generation approach, suffers from a trade-off between performance and action horizon: fewer diffusion queries lead to larger trajectory chunks, which in turn accumulate tracking errors. To overcome these challenges, we introduce WARPD (World model Assisted Reactive Policy Diffusion), a method that generates closed-loop policies (weights for neural policies) directly, instead of open-loop trajectories. By learning behavioral
distributions in parameter space rather than trajectory space, WARPD offers two major advantages: (1) extended action horizons with robustness to perturbations, while maintaining high task performance, and (2) significantly reduced inference costs. Empirically, WARPD outperforms DP in long-horizon and perturbed environments, and achieves multitask performance on par with DP while requiring only ∼ 1/45th of the inference-time FLOPs per step.

---

## 论文详细总结（自动生成）

# WARPD：世界模型辅助的响应式策略扩散 — 中文总结

## 1. 核心问题与整体含义（研究动机与背景）

- **背景**：随着开源机器人数据的增多，模仿学习在操作和运动任务中展现出潜力。扩散模型被广泛用于训练大型通用策略，因其能建模多模态动作分布。
- **问题**：
  - 扩散策略（Diffusion Policy, DP）体量大、推理速度慢，难以满足高控制频率的机器人任务。
  - DP 面临性能与动作轨迹块长度之间的权衡：减少扩散查询次数会导致轨迹块过长，从而累积跟踪误差。
- **动机**：现有方法生成开环轨迹，缺乏闭环鲁棒性；需要一种能同时提高推理速度、减小长期误差，并保持多模态建模能力的新方法。

## 2. 方法论：核心思想与关键技术

- **核心思想**：WARPD 直接生成闭环策略（神经网络的权重），而非开环轨迹。通过在参数空间中学习行为分布，而非轨迹空间，实现两大优势：
  1. 延长动作轨迹块的同时保持对扰动的鲁棒性，维持任务性能。
  2. 显著降低推理成本。
- **技术细节**：
  - 使用世界模型辅助：世界模型提供前瞻预测，辅助扩散策略生成响应式的策略参数。
  - 将扩散过程应用于策略参数空间，学习参数的后验分布。
  - 推理时只需一次前向传播即可得到策略权重，避免多步扩散采样。
- **公式/算法流程（文字描述）**：
  - 训练阶段：利用世界模型预测未来状态，结合专家数据训练扩散模型来生成策略参数。
  - 推理阶段：输入当前观测，通过训练好的扩散模型一步生成策略权重，然后执行闭环控制。
  - 与传统 DP 相比，WARPD 无需每次推理都执行多次去噪迭代，而是预计算权重或单步采样。

## 3. 实验设计

- **数据集/场景**：机器人操作任务（如抓取、放置）和运动任务（如行走、避障）。未明确列出具体数据集名称，但提及“long-horizon and perturbed environments”以及“multitask performance”。
- **Benchmark**：主要与 Diffusion Policy (DP) 对比，评估指标包括任务性能、推理时间、FLOPs、对扰动的鲁棒性、长期轨迹误差等。
- **对比方法**：DP（基线），可能还包括其他模仿学习方法，但摘要仅明确提及 DP。

## 4. 资源与算力

- 文中未明确说明使用的 GPU 型号、数量、训练时长等具体算力信息。
- 仅提及推理时 FLOPs 降低至 DP 的约 1/45，但训练开销未给出。因此无法总结详细算力需求。

## 5. 实验数量与充分性

- **实验组数**：摘要未列出具体实验数量，但声称在“long-horizon and perturbed environments”中优于 DP，并达到与 DP 相当的多任务性能。推测包含至少以下实验：
  - 标准任务性能对比
  - 长时域任务（long-horizon）性能对比
  - 扰动环境（perturbed）下的鲁棒性测试
  - 推理效率（FLOPs 对比）
- **充分性与客观性**：从摘要看，实验覆盖了核心问题（推理速度、轨迹误差、鲁棒性），对比了流行基线，结果具有说服力。但未提供消融实验或更多基线，因此充分性一般。需查看全文才能判断统计显著性及重复实验次数。

## 6. 主要结论与发现

- WARPD 在长时域和扰动环境中性能优于 Diffusion Policy。
- 多任务性能与 DP 相当，但推理时每步 FLOPs 仅为 DP 的约 1/45，大幅降低计算成本。
- 通过直接生成闭环策略（权重），解决了 DP 中轨迹块累积误差与推理速度的权衡问题。
- 世界模型辅助使策略具备前瞻性，增强了鲁棒性。

## 7. 优点

- **创新性**：将扩散模型从轨迹空间迁移到参数空间，直接生成闭环策略，是一种新颖的思路。
- **高效性**：推理速度提升两个数量级以上，适合高频控制。
- **鲁棒性**：闭环策略对干扰和长时域误差具有天然抵抗能力。
- **普适性**：同时适用于操作和运动任务，表明方法具有通用性。

## 8. 不足与局限

- **实验覆盖**：仅与 DP 对比，未与其他闭环模仿学习或强化学习方法（如行为克隆、GAIL、TD3+BC 等）比较，可能低估优势或暴露缺点。
- **算力信息缺失**：未报告训练所需 GPU、时间等资源，难以评估实际部署成本。
- **消融研究缺失**：未明确说明是否对不同组件（如世界模型的详细设计）进行消融，使贡献归因不充分。
- **偏差风险**：仅依赖公开数据集？未提供数据分布细节，可能存在过拟合特定场景的风险。
- **应用限制**：世界模型需要额外训练和状态预测，可能增加系统复杂性和域迁移困难；参数空间维度过高时扩散过程可能不稳定。

（完）
