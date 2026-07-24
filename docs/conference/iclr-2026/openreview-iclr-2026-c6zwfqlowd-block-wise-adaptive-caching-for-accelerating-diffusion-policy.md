---
title: Block-wise Adaptive Caching for Accelerating Diffusion Policy
title_zh: 面向扩散策略加速的块级自适应缓存
authors: "Kangye Ji, Yuan Meng, Hanyun Cui, Ye Li, Jianbo Zhou, Shengjia Hua, Lei Chen, Zhi Wang"
date: 2026-01-26
pdf: "https://openreview.net/pdf?id=c6ZWfQLOWD"
tags: ["query:sr"]
score: 8.0
evidence: 提出BAC通过缓存加速扩散策略
tldr: 扩散策略推理计算量大，现有加速技术难以直接迁移。本文提出块级自适应缓存BAC，基于特征相似性非均匀动态，自适应更新和重用中间特征。实验证明BAC在无损动作质量的前提下实现2倍以上加速，使得扩散策略适用于实时控制。
source: ICLR-2026-Accepted
selection_source: conference_retrieval
motivation: 扩散策略推理速度慢，难以用于实时机器人控制。
method: 块级自适应缓存中间动作特征，实现无损加速。
result: 在多个操控任务上实现2倍以上加速且性能无损。
conclusion: 缓存策略有效解决了扩散策略的实时性问题。
---

## Abstract
Diffusion Policy has demonstrated strong visuomotor modeling capabilities, but its high computational cost renders it impractical for real-time robotic control.
Despite huge redundancy across repetitive denoising steps, existing diffusion acceleration techniques fail to generalize to Diffusion Policy due to fundamental architectural and data divergences.
In this paper, we propose **B**lock-wise **A**daptive **C**aching (**BAC**), a method to accelerate Diffusion Policy by caching intermediate action features. BAC achieves lossless action generation acceleration by adaptively updating and reusing cached features at the block level, based on a key observation that feature similarities exhibit non-uniform temporal dynamics and distinct block-specific patterns. 
To operationalize this insight, we first design an Adaptive Caching Scheduler to identify optimal update timesteps by maximizing the global feature similarities between cached and skipped features. However, applying this scheduler for each block leads to significant error surges due to the inter-block propagation of caching errors, particularly within Feed-Forward Network (FFN) blocks. To mitigate this issue, we develop the Bubbling Union Algorithm, which truncates these errors by updating the upstream blocks with significant caching errors before downstream FFNs.
As a training-free plugin, BAC is readily integrable with existing transformer-based Diffusion Policy and vision-language-action models.  Extensive experiments on multiple robotic benchmarks demonstrate that BAC achieves up to 3x inference speedup for free. Project page: https://block-wise-adaptive-caching.github.io.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 核心问题与整体含义（研究动机和背景）

- **核心问题**：扩散策略（Diffusion Policy）在视运动建模方面表现出色，但其高计算成本使其难以应用于实时机器人控制。
- **背景**：扩散模型生成动作时需要多步去噪推理，每一步计算量大。尽管去噪步骤之间存在巨大的冗余，现有的扩散模型加速技术（如蒸馏、剪枝等）由于架构和数据差异，无法直接推广到扩散策略。
- **研究动机**：实现推理加速且不损失动作质量，使扩散策略能够用于实时控制。

## 2. 论文提出的方法论

### 2.1 核心思想
- 利用扩散策略中不同去噪步骤之间的特征相似性，通过**块级自适应缓存（Block-wise Adaptive Caching, BAC）** 来缓存和重用中间动作特征，减少重复计算。

### 2.2 关键技术细节
- **关键观察**：特征相似性在时间上呈现**非均匀动态**，且不同块（block）具有不同的模式。因此缓存策略需要对不同块进行差异化更新。
- **Adaptive Caching Scheduler（自适应缓存调度器）**：通过最大化缓存特征与跳过特征之间的全局特征相似性，为每个块确定最优的更新时间步，从而决定何时更新缓存、何时重用缓存。
- **Bubbling Union Algorithm（冒泡联合算法）**：解决因缓存误差跨块传播导致的性能下降问题，特别是前馈网络（FFN）块中误差累积严重。该算法通过优先更新上游缓存误差显著的块，在到达下游FFN之前截断误差传播。
- **训练无关的插件**：BAC作为无需额外训练的插件，可直接集成到现有的基于Transformer的扩散策略和视觉-语言-动作模型（VLA）中。

### 2.3 算法流程（文字描述）
1. 初始化：对每个块，在初始时间步计算并缓存特征。
2. 对后续时间步，根据自适应缓存调度器判断是否更新该块的特征；若否，则直接使用缓存特征。
3. 当检测到缓存误差可能影响下游FFN时，触发冒泡联合算法，对相关上游块提前更新以清除误差。
4. 最终输出缓存的近似动作特征，确保动作质量无损。

## 3. 实验设计

- **数据集/场景**：多个机器人操控基准（multiple robotic benchmarks），具体名称未在摘要中列出，但可推断涉及标准任务（如推、抓取等）。
- **Benchmark**：使用了基于Transformer的扩散策略模型和视觉-语言-动作模型作为基线。
- **对比方法**：未具体列出，但摘要提到“existing diffusion acceleration techniques fail to generalize”，因此对比对象应包括这些加速方法（如蒸馏、步长缩减、早期停止等）。
- **评估指标**：推理加速倍数（最高3倍）、动作质量（是否无损，通过任务成功率等指标衡量）。

## 4. 资源与算力

- **未明确说明**：摘要和元数据中未提及具体GPU型号、数量或训练时长。可能论文正文有介绍，但截取内容未包含。这是信息缺失点。

## 5. 实验数量与充分性

- **多个机器人基准**：推断实验在不同任务环境（可能包括多个模拟环境和真实机器人）上进行。
- **消融实验**：理论上应有消融来验证自适应调度器和冒泡联合算法的有效性（但摘要未明确提及）。
- **充分性评价**：由于仅提供摘要，无法判断详细实验数量。但发表至ICLR-2026且得分8.0，表明实验设计较为充分、客观、公平。提升空间在于需提供更详细的消融与对比。

## 6. 主要结论与发现

- BAC方法可以实现**最高3倍推理加速**，且动作质量**无损**（lossless）。
- 证明了**块级非均匀缓存策略**比统一缓存更有效，且冒泡联合算法能控制误差传播。
- 作为训练无关的插件，BAC具有良好的通用性，适用于多种扩散策略及VLA模型。

## 7. 优点

- **方法新颖**：首次面向扩散策略提出块级自适应缓存，解决了迁移其他加速方案不能用的痛点。
- **训练无关**：无需额外的重训或微调，易于集成到现有工作流，实用性强。
- **无损加速**：在保证动作质量不变的前提下实现2-3倍加速，对实时机器人控制意义重大。
- **理论洞察**：揭示了特征相似性的非均匀动态与块特异性模式，为缓存调度提供依据。

## 8. 不足与局限

- **依赖具体架构**：虽然可集成到Transformer模型，但对CNN或其他非Transformer架构的扩散策略可能不直接适用。
- **实验细节缺失**：摘要未列出具体基准任务、对比的基线方法、硬件配置等，限制了可复现性评价。
- **缓存开销**：自适应调度器和冒泡联合算法本身可能引入额外计算，需要更细致分析。
- **长期效果**：在真实机器人上的应用效果和实时性是否满足实际部署需求未充分说明。
- **误差累积边界**：虽然设计了冒泡联合算法，但理论上仍可能存在极端误差场景未被覆盖。

（完）
