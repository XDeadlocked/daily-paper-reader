---
title: Planning with Reasoning using Vision Language World Model
title_zh: 基于视觉语言世界模型的推理规划
authors: "Delong Chen, Théo Moutakanni, Willy Chung, Yejin Bang, Ziwei Ji, Allen Bolourchi, Pascale Fung"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=uvRApnkdbk"
tags: ["query:sr"]
score: 9.0
evidence: 用于规划的视觉语言世界模型
tldr: 现有世界模型缺乏高层语义和时序抽象推理能力。提出视觉语言世界模型（VLWM），基于自然视频训练，首先推断整体目标，然后预测由动作和状态变化交织的轨迹。通过迭代LLM自精炼和描述树压缩未来观察，VLWM同时学习动作策略和动力学模型，支持反应式系统-1规划和反思式系统-2规划。
source: ICLR-2026-Public
selection_source: conference_retrieval
motivation: 现有世界模型缺乏高层语义与时间抽象推理。
method: 训练VLWM预测目标及轨迹，结合LLM自精炼与描述树。
result: 同时实现快速反应式规划和慢速反思式规划。
conclusion: 视觉语言世界模型可有效支持物理世界的规划。
---

## Abstract
Effective planning in the physical world requires strong world models, but models that can reason about high-level actions with semantic and temporal abstraction remain underdeveloped. We introduce the Vision Language World Model (VLWM), a foundation model trained for language-based world modeling on natural videos. Given visual observations, VLWM first infers the overall goal to be achieved and then predicts a trajectory composed of interleaved actions and world state changes. These targets are extracted by iterative LLM self-refinement conditioned on compressed future observations represented by a Tree of Captions. VLWM learns both an action policy and a dynamics model, enabling reactive system-1 plan decoding and reflective system-2 planning via cost minimization. The cost evaluates the semantic distance between hypothetical future states predicted by VLWM and the expected goal state, and is measured by a critic model trained in a self-supervised manner. VLWM achieves state-of-the-art performance on the Visual Planning for Assistance benchmark and our proposed PlannerArena human evaluations, where system-2 improves Elo score by 27% over system-1. It also outperforms strong VLM baselines on RoboVQA and WorldPrediction benchmarks.

---

## 论文详细总结（自动生成）

# 基于视觉语言世界模型的推理规划：中文总结

## 1. 核心问题与整体含义
- **研究动机**：物理世界中的有效规划需要强大的世界模型，但现有世界模型普遍缺乏高层语义抽象与时间抽象推理能力，无法对高层动作进行合理推理。
- **核心问题**：如何构建一个能够同时感知视觉观察、推断高层目标、并预测由动作和状态变化交织的完整轨迹的世界模型，以支持机器人或智能体在物理世界中的规划。
- **整体含义**：本文提出的视觉语言世界模型（VLWM）填补了高层语义推理与底层动力学建模之间的鸿沟，为基于语言的物理世界规划提供了新的基础范式。

## 2. 方法论
- **核心思想**：将世界建模视为一个语言任务，利用自然视频训练一个统一的视觉语言模型。该模型首先从观察中推断整体目标，然后预测一个由动作和状态变化交替组成的轨迹。
- **关键技术细节**：
  - **迭代LLM自精炼**：利用大型语言模型对未来的观测进行迭代自精炼，生成更准确的目标和轨迹描述。
  - **描述树（Tree of Captions）**：将压缩的未来观测表示为一棵描述树，用于条件化后续预测。
  - **双系统规划**：
    - **系统-1（反应式）**：直接解码动作策略，实现快速反应式规划。
    - **系统-2（反思式）**：通过成本最小化进行反思式规划。成本由自监督训练的批评模型（critic model）衡量，该模型评估VLWM所预测的未来状态与期望目标状态之间的语义距离。
  - **同时学习**：VLWM在同一框架内同时学习动作策略（policy）和动力学模型（dynamics model）。

## 3. 实验设计
- **数据集/场景**：
  - **Visual Planning for Assistance (VPA) 基准**：用于评估辅助场景下的视觉规划能力。
  - **RoboVQA 基准**：机器人问答与规划基准。
  - **WorldPrediction 基准**：世界状态预测基准。
  - **PlannerArena 人工评估**：作者自行提出的基于人工评判的评估平台。
- **对比方法**：与多种强视觉语言模型（VLM）基线进行了对比，具体方法未在摘要中列出，但提到在RoboVQA和WorldPrediction上超越强VLM基线。
- **实验指标**：Elo评分（系统-2相比系统-1提升27%）、VPA基准上的SOTA性能。

## 4. 资源与算力
- 论文摘要及元数据中**未明确说明**所使用的GPU型号、数量或训练时长。仅根据现有信息无法判断算力需求。

## 5. 实验数量与充分性
- **实验数量**：涉及3个主要基准（VPA、RoboVQA、WorldPrediction）以及一个自建人工评估（PlannerArena）。消融实验未在摘要中明确提及，但可能存在于全文。
- **充分性**：
  - 对比了多个强基线，且同时提供了自动评估和人工评估，增强了说服力。
  - 对系统-1与系统-2进行了内部对比，并量化了Elo提升。
  - 但缺少对方法论中各组件（如描述树、迭代精炼、批评模型）的消融分析细节，因此实验的完备性需参考全文进一步确认。

## 6. 主要结论与发现
- VLWM实现了**当时最先进的性能**，在VPA基准上成为SOTA，在RoboVQA和WorldPrediction上优于强VLM基线。
- **双系统规划有效**：系统-2（反思式）的Elo评分比系统-1（反应式）提升27%，表明结合成本最小化的反思式规划能显著改善规划质量。
- 视觉语言世界模型能够同时支持快速反应式规划和慢速反思式规划，为物理世界规划提供了统一框架。

## 7. 优点
- **高层语义与时间抽象整合**：首次将目标推断、动作策略和动力学建模统一在一个视觉语言框架内。
- **自监督批评模型**：无需人工标注，即可评估假设未来状态与目标的语义距离，增强了方法的可扩展性。
- **双系统架构**：融合了快速直觉与慢速推理，兼顾实时性与准确性。
- **迭代LLM自精炼与描述树**：创新地利用LLM对压缩观测进行迭代优化，提高了轨迹预测的准确性。

## 8. 不足与局限
- **算力与资源未公开**：无法评估方法的实际训练成本及对硬件的要求。
- **实验细节缺失**：消融实验数量、超参数设置、基线具体配置等未在摘要中提供，需参考完整论文。
- **应用限制**：方法依赖自然视频数据，可能受限于视频质量、场景覆盖范围以及LLM的固有偏见（如语言模型对特定场景的偏好）。
- **偏差风险**：自监督批评模型可能对某些语义距离的评估不够鲁棒，且人工评估（PlannerArena）可能存在标注偏差。
- **未见提及物理真实性验证**：模型预测的轨迹在真实机器人上的执行效果未在摘要中说明，尚不清楚从模拟到实际的迁移能力。

（完）
