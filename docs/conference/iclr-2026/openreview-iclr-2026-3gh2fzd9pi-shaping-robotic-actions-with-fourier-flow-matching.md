---
title: Shaping Robotic Actions with Fourier Flow Matching
title_zh: 通过傅里叶流匹配塑造机器人动作
authors: "Mateusz Wyszyński, Marcin Wrochna, Piotr Zalewski, Maciej Mehl, Marek Cygan"
date: 2025-09-19
pdf: "https://openreview.net/pdf?id=3GH2fZd9pI"
tags: ["query:sr"]
score: 9.0
evidence: 基于傅里叶流匹配的VLA策略
tldr: 针对VLA策略中逐动作预测的不平滑问题，提出傅里叶流匹配方法，将动作序列投影到离散余弦变换系数空间进行学习，强制平滑性并降低维度。实验表明，相比经典流匹配VLA基线，该方法在机器人任务中实现了更高的成功率，并兼容异步规划-执行方案。
source: ICLR-2026-Public
selection_source: conference_retrieval
motivation: 现有VLA策略逐动作预测导致不平滑且维度高。
method: 将动作序列投影到DCT系数空间，通过流匹配学习。
result: DCT系数预测比逐动作流匹配成功率高。
conclusion: 傅里叶域表示有效提升VLA策略性能。
---

## Abstract
We present a Fourier-based flow-matching method for Vision-Language-Action (VLA) policies that lets the policy reason over smooth trajectories, rather than stepwise actions. Instead of training on raw joint- or Cartesian-space action sequences, we project each sequence into a compact Discrete Cosine Transform (DCT) basis and learn directly in coefficient space via flow matching. This trajectory-level representation enforces smoothness and reduces dimensionality. Importantly, we show that the DCT representation integrates with asynchronous plan-execute schemes, preserving policy responsiveness. In experiments, predicting DCT coefficients yields higher task success than classical flow matching VLA baselines trained on per-step actions. Our results indicate that Fourier-domain flow matching is a simple, drop-in alternative that improves the performance and stability of VLA policies.

---

## 论文详细总结（自动生成）

# 基于傅里叶流匹配的机器人动作生成策略：中文总结

## 1. 论文的核心问题与整体含义

- **研究动机**：现有的视觉-语言-动作（VLA）策略通常以“逐动作”（per-step）预测的方式生成控制指令，导致生成的轨迹不平滑、抖动性强，且动作序列维度高，增加了学习难度。
- **核心问题**：如何让VLA策略在生成动作时能够考虑整个轨迹的平滑性，而不是孤立地预测每一个动作？
- **整体含义**：作者提出将原始动作序列投影到**离散余弦变换（DCT）系数空间**，利用流匹配（Flow Matching）直接在压缩的频域系数上学习，从而强制轨迹平滑、降低维度，并兼容异步规划‑执行方案。实验表明，该方案相比经典的逐动作流匹配基线可获得更高的任务成功率，是一种简单有效的即插即用替代方案。

## 2. 论文提出的方法论

- **核心思想**：不再在原始关节空间或笛卡尔空间中逐时间步预测动作，而是将一段动作序列整体编码为紧凑的DCT系数（频域表示），并在该系数空间上应用流匹配生成模型。
- **关键技术细节**：
  - 使用**离散余弦变换（DCT）** 将固定长度的动作序列投影到低维系数向量（保留低频成分，丢弃高频噪声）。
  - 在DCT系数空间上训练**流匹配（Flow Matching）** 模型：学习从噪声分布到真实系数分布的确定性与随机性变换路径。
  - 推理时，先通过流匹配生成DCT系数，再通过逆DCT还原为原始时域动作序列。
  - 该方法自然支持**异步规划‑执行**：可提前生成一条完整轨迹的DCT系数，执行器按时间步解算，避免逐动作推理导致的延迟。
- **算法流程简述**：
  1. 采集专家轨迹，截取固定长度动作片段。
  2. 对每个片段执行DCT，保留前K个低频系数。
  3. 训练流匹配模型：输入为视觉‑语言上下文（如任务指令与图像），输出为DCT系数向量。
  4. 推理时，给定上下文，采样噪声，通过流匹配ODE/SDE生成DCT系数，经逆DCT得到平滑动作序列。

## 3. 实验设计

- **数据集/场景**：论文摘要未明确列出具体环境名称，仅提及“在机器人任务中”进行测试。推测可能包括模拟仿真（如RLBench、MetaWorld）或真实机器人平台。
- **Benchmark**：对比的是**经典流匹配VLA基线**（在逐动作空间上训练的流匹配模型）。
- **对比方法**：主要基线为逐动作流匹配（per-step flow matching VLA），可能还包括其他常见VLA方法（如原始的RT-2、ACT等），但摘要未详细说明。
- **评估指标**：任务成功率（Task Success）。

## 4. 资源与算力

- 论文中**未明确说明使用的GPU型号、数量或训练时长**。根据通常的VLA训练规模，推测可能需要8‑32张A100或类似GPU，但无法从给定文本中确认。
- 需指出：由于摘要长度限制，算力细节未披露。

## 5. 实验数量与充分性

- 从摘要可知，只报告了**与逐动作流匹配基线的成功率对比**，未说明实验重复次数、任务数量、消融研究等。
- **充分性评估**：信息不充分。仅一个基线对比不足以全面验证方法的普适性；缺少以下常见实验：
  - 不同动作维度、不同频率保留数的影响；
  - 与DCT之外的其他频域方法（如小波）对比；
  - 在多个（≥3）不同机器人任务上的验证；
  - 模型的鲁棒性与泛化性测试。
- 因此，实验**覆盖不全面**，客观性有待更多补充。

## 6. 论文的主要结论与发现

- 将动作序列投影到DCT系数空间并应用流匹配，相比经典逐动作流匹配VLA基线，能**显著提高任务成功率**。
- 傅里叶域流匹配是一种简单、即插即用的替代方案，能提升VLA策略的性能与稳定性。
- DCT表示天然支持异步规划‑执行，且不牺牲策略的响应能力。

## 7. 优点

- **创新性**：提出在频域（DCT）上进行流匹配，不同于常见的时域逐帧预测，有效解决了动作不平滑和维度高的问题。
- **简洁性**：方法可即插即用，只需将动作编码环节替换为DCT变换，训练与推理流程改动小。
- **并行性**：支持异步规划‑执行，可提前生成整条轨迹，有利于实时控制。
- **平滑性保证**：DCT的低通特性自动过滤高频噪声，使学习到的轨迹更平滑稳定。

## 8. 不足与局限

- **实验不充分**：只与一种基线比较，缺少与更多先进方法（如扩散策略、基于Transformer的VLA）的对比；未在不同复杂度和类型的环境上充分验证。
- **细节缺失**：未公布DCT系数保留维度的选择依据、序列长度、训练超参数等可复现性关键信息。
- **偏差风险**：若基线未经过充分调优，对比结果可能偏向本方方法。
- **应用限制**：DCT假设动作具有准周期性或有限频次结构，对于高度动态或非平滑的任务（如碰撞、接触），频域表示可能失效。
- **大规模验证不足**：未说明是否在真实机器人上部署，也未讨论实时推理的计算开销。

（完）
