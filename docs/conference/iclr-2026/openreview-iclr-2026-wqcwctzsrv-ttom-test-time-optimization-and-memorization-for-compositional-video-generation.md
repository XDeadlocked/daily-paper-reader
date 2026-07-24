---
title: "TTOM: Test-Time Optimization and Memorization for Compositional Video Generation"
title_zh: TTOM：面向组合式视频生成的测试时优化与记忆
authors: "Leigang Qu, Ziyang Wang, Na Zheng, Wenjie Wang, Liqiang Nie, Tat-Seng Chua"
date: 2026-01-26
pdf: "https://openreview.net/pdf?id=wqCwcTZsrv"
tags: ["query:sr"]
score: 8.0
evidence: 提出TTOM用于组合式视频生成
tldr: 视频基础模型在组合场景（如运动、数量、空间关系）中表现不佳。本文提出TTOM，一种无需训练的测试时优化与记忆框架，通过参数记忆机制对齐输出与时空布局。实验表明TTOM显著提升了组合视频生成的准确性和一致性，且无需修改预训练模型。
source: ICLR-2026-Accepted
selection_source: conference_retrieval
motivation: 视频基础模型在组合场景下生成质量差。
method: 测试时优化新参数，结合布局注意力目标与记忆机制。
result: 在组合视频生成任务上显著提升语义对齐。
conclusion: 测试时优化有效改善了视频基础模型的组合能力。
---

## Abstract
Video Foundation Models (VFMs) exhibit remarkable visual generation performance, but struggle in compositional scenarios (\eg, motion, numeracy, and spatial relation). 
In this work, we introduce **Test-Time Optimization and Memorization (TTOM)**, a training-free framework that aligns VFM outputs with spatiotemporal layouts during inference for better text-image alignment.
Rather than direct intervention to latents or attention per-sample in existing work, we integrate and optimize new parameters guided by a general layout-attention objective. 
Furthermore, we formulate
video generation within a streaming setting, and maintain historical optimization contexts with a parametric memory mechanism that supports flexible operations, such as insert, read, update, and delete. 
Notably, we found that TTOM disentangles compositional world knowledge, showing powerful transferability and generalization. 
Experimental results on the T2V-CompBench and Vbench benchmarks establish TTOM as an effective, practical, scalable, and efficient framework to achieve cross-modal alignment for compositional video generation on the fly.

---

## 论文详细总结（自动生成）

# TTOM：面向组合式视频生成的测试时优化与记忆——论文详细总结

## 1. 论文的核心问题与整体含义

- **研究动机**：当前视频基础模型（Video Foundation Models, VFMs）在生成视觉内容方面表现卓越，但在处理组合性场景（例如运动、数量、空间关系）时出现严重语义偏差。模型难以准确理解并生成包含多重属性、实体间特定空间关系或计数要求的视频。
- **整体含义**：论文提出一种无需重新训练或微调的测试时优化框架，旨在实时对齐视频基础模型的输出与用户指定的时空布局，从而在不改变预训练模型参数的前提下提升组合式视频生成的质量和一致性。

## 2. 论文提出的方法论

- **核心思想**：采用“测试时优化与记忆”（Test-Time Optimization and Memorization, TTOM）框架。与现有工作直接干预隐层表示或每个样本的注意力不同，TTOM在推理阶段集成并优化一组新参数，这些参数由通用的布局‑注意力目标引导。
- **关键技术细节**：
  - 将视频生成过程建模为流式（streaming）设置，允许动态处理连续帧。
  - 引入参数化记忆机制（parametric memory），支持对历史优化上下文进行灵活操作（插入、读取、更新、删除）。
  - 优化目标为布局‑注意力目标（layout-attention objective），引导模型关注指定区域和关系。
  - 整个框架无需训练（training-free），直接在推理时调整参数。
- **算法流程**（文字说明）：
  1. 输入用户描述（包含组合语义，如“两个球在左侧滚动”）。
  2. 初始化记忆结构，并建立新参数副本。
  3. 对当前帧进行前向推理，计算布局‑注意力损失。
  4. 通过梯度下降优化新参数（不更新预训练基底模型）。
  5. 将优化上下文存储到记忆中，以支持未来帧的一致性。
  6. 重复步骤3‑5直至视频生成完成。

## 3. 实验设计

- **数据集/场景**：使用两个标准组合视频生成基准——**T2V-CompBench** 和 **VBench**。这些基准包含多种组合性测试维度（如运动、数量、空间关系、物体属性等）。
- **对比方法**：论文未在摘要中列出具体对比方法，但结合社区背景，通常对比主流视频生成模型（如VideoLDM, ModelScope等）及已有的测试时干预方法（如直接注意力编辑、隐空间引导等）。
- **评估指标**：主要关注语义对齐准确率、生成视频与文本匹配程度、组合条件满足率等。

## 4. 资源与算力

- **元数据未明确说明**：论文摘要、元数据及标题部分均未提及具体的GPU型号、数量、训练时长或推理时间。因此无法量化算力消耗。但鉴于其为“测试时优化”且无需训练，预计推理代价相对可控，可能仅需单张高效GPU即可完成。

## 5. 实验数量与充分性

- **实验组数**：摘要中仅提及在T2V-CompBench和VBench两个基准上进行了评估。通常被ICLR录用的论文会包含多组主要实验、消融实验（如记忆机制有效性、不同优化步数的影响、组合维度分解测试）、以及跨模型泛化实验。但从摘要信息看，不能断言实验数量，但合理推测其充分性通过审稿。
- **充分性与客观性**：所选基准为领域内公认的组合视频生成测试集，对比方法应覆盖多种基线。实验设计上考虑了不同组合维度，有助于客观评估。但缺乏用户研究或更复杂现实场景验证。

## 6. 论文的主要结论与发现

- TTOM显著提升了视频基础模型在组合场景下的语义对齐准确性和生成一致性。
- 该方法能够解耦组合性世界知识（disentangles compositional world knowledge），展现出强大的可迁移性和泛化能力——即在一个组合维度上优化后的知识可迁移到其他类似组合任务。
- 实验证明TTOM是一个有效、实用、可扩展且高效的跨模态对齐框架，能够“即时”（on the fly）优化组合视频生成。

## 7. 优点

- **无需训练**：不修改预训练模型参数，降低了计算资源和数据依赖。
- **参数记忆机制**：支持流式视频生成，保持跨帧一致性，且支持灵活的上下文操作。
- **可迁移性**：优化所得参数蕴含组合知识，可重复应用于相似组合场景。
- **通用性**：适用于不同视频基础模型，可能作为插件式模块。
- **高效**：测试时优化通常仅需少量迭代即可提升质量。

## 8. 不足与局限

- **实验覆盖有限**：摘要未展示在更多样化、更长视频或实时应用场景中的表现；也未与更多最新的组合生成方法（如布局条件扩散模型）进行对比。
- **偏差风险**：仅在两个特定基准上验证，可能存在基准偏向；真实世界中的组合复杂性（如罕见关系、多物体遮挡）可能未充分覆盖。
- **应用限制**：测试时优化仍需要额外的推理开销，在高帧率或长视频场景下可能影响实时性；记忆机制的内存消耗未报告。
- **局限性声明缺失**：论文摘要未指出任何局限性，但实际全文应包含对失败案例、稳定性、超参数敏感性的讨论。

（完）
