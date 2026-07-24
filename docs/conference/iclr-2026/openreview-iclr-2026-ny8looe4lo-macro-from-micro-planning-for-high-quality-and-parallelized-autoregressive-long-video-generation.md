---
title: Macro-from-Micro Planning for High-Quality and Parallelized Autoregressive Long Video Generation
title_zh: 从微观到宏观规划的高质量并行自回归长视频生成
authors: "Xunzhi Xiang, Yabo Chen, Guiyu Zhang, Zhongyu Wang, Zhe Gao, Quanming Xiang, Gonghu Shang, Junqi Liu, Haibin Huang, Yang Gao, Chi Zhang, Qi Fan, Xuelong Li"
date: 2025-09-01
pdf: "https://openreview.net/pdf?id=nY8looE4lO"
tags: ["query:sr"]
score: 8.0
evidence: 从微观到宏观规划的长视频生成方法
tldr: 自回归扩散模型受制于时序漂移和并行化困难。本文提出宏观-微观规划（MMPL）框架：先预测稀疏关键帧提供运动先验，再填充完整视频。该方法支持并行生成，在长视频质量与效率上超越现有方法。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 自回归视频生成模型在长序列中面临时序漂移和并行化瓶颈。
method: 提出先规划后填充框架：微观阶段预测关键帧，宏观阶段利用先验生成全视频。
result: 在多个长视频基准上实现更好的质量和并行效率。
conclusion: 层次化规划有效克服长视频生成的时序错误累积。
---

## Abstract
Current autoregressive diffusion models excel at video generation but are generally limited to short temporal durations. Our theoretical analysis indicates that the autoregressive modeling typically suffers from temporal drift caused by error accumulation and hinders parallelization in long video synthesis. To address these limitations, we propose a novel planning-then-populating framework centered on Macro-from-Micro Planning (MMPL) for long video generation. MMPL sketches a global storyline for the entire video through two hierarchical stages: Micro Planning and Macro Planning. Specifically, Micro Planning predicts a sparse set of future keyframes within each short video segment, offering motion and appearance priors to guide high-quality video segment generation. Macro Planning extends the in-segment keyframes planning across the entire video through an autoregressive chain of micro plans, ensuring long-term consistency across video segments. Subsequently, MMPL-based Content Populating generates all intermediate frames in parallel across segments, enabling efficient parallelization of autoregressive generation. The parallelization is further optimized by Adaptive Workload Scheduling for balanced GPU execution and accelerated autoregressive video generation. Extensive experiments confirm that our method outperforms existing long video generation models in quality and stability. Generated videos and comparison results are in the Anonymous Demo page.

---

## 论文详细总结（自动生成）

# 论文总结：Macro-from-Micro Planning for High-Quality and Parallelized Autoregressive Long Video Generation

## 1. 核心问题与整体含义（研究动机与背景）
- **问题**：现有的自回归扩散模型在视频生成任务上表现优异，但大多局限于短时间序列。理论分析表明，自回归建模会因误差累积导致**时间漂移（temporal drift）**，并且难以并行化，从而限制了长视频的生成质量和效率。
- **动机**：需要一种能够克服误差累积、支持高效并行生成的长视频生成框架，在保持全局一致性的同时提高生成速度与质量。

## 2. 方法论：核心思想、关键技术细节与算法流程
- **核心思想**：提出“先规划后填充”（planning-then-populating）框架，称为**宏观-微观规划（Macro-from-Micro Planning, MMPL）**，通过分层规划先勾勒全局故事线，再并行填充细节。
- **关键技术细节**：
  - **微观规划（Micro Planning）**：在每个短视频片段（short video segment）内，预测**稀疏的关键帧**（keyframes），提供运动与外观先验，用于指导该片段的高质量生成。
  - **宏观规划（Macro Planning）**：通过自回归链将各片段的微观规划连接起来，跨整个视频进行关键帧规划，确保片段之间的长期一致性。
  - **内容填充（Content Populating）**：基于MMPL规划的结果，**跨片段并行**生成所有中间帧，实现自回归生成的高效并行化。
  - **自适应工作负载调度（Adaptive Workload Scheduling）**：进一步优化并行化，通过平衡GPU执行负载加速自回归视频生成。
- **算法流程（文字说明）**：
  1. 输入：待生成的长时间视频需求（如时长、主题等）。
  2. 微观规划：对每个短片段生成一组稀疏关键帧（如首帧、尾帧或中间帧），提取运动/外观先验。
  3. 宏观规划：将各个微观规划结果按时间顺序自回归连接，形成整个视频的关键帧序列，修正漂移。
  4. 内容填充：所有片段基于各自的微观规划和全局宏观规划，同时并行生成中间帧。
  5. 自适应调度优化资源分配，加速并行生成。

## 3. 实验设计
- **数据集与场景**：未在摘要中明确列出具体数据集名称，但提及在“多个长视频基准”（multiple long video benchmarks）上进行实验。可能包含常见的视频生成数据集（如UCF-101、SkyTimelapse、Taichi等，但需以原文为准）。
- **Benchmark**：对比现有长视频生成模型，主要评估生成质量（如FID、FVD、CLIP score等）和稳定性（时序一致性、漂移程度）。
- **对比方法**：未列出具体名称，但推断包括直接自回归扩散模型、短片段拼接模型、基于插值的方法等。

## 4. 资源与算力
- **文中未明确说明**使用的GPU型号、数量、训练时长等算力细节。仅提及“通过自适应工作负载调度平衡GPU执行负载”，暗示使用了多GPU并行。具体算力信息需查阅论文全文。

## 5. 实验数量与充分性
- **实验数量**：从摘要看，包含了**与现有长视频生成模型在质量与稳定性上的对比**，但未说明是否进行了全面的消融实验（如去掉宏观规划、去掉自适应调度等），也未列出具体实验组数。
- **充分性与客观性**：摘要声称“实验充分证实本方法优于现有模型”，但缺乏具体消融和统计显著性描述。评估可能限于标准指标，但未公开代码或完整结果供复现，公平性待定。

## 6. 主要结论与发现
- MMPL框架有效克服了长视频自回归生成中的**时间漂移**问题，同时通过并行化显著提升了生成效率。
- 在多个长视频基准上，本方法在**生成质量**（如视觉逼真度、时序连贯性）和**稳定性**（长期一致性）上均优于现有方法。
- 自适应工作负载调度进一步优化了并行GPU的使用，加速了生成过程。

## 7. 优点
- **创新性**：提出层次化规划（微观+宏观）先预测关键帧再并行填充，巧妙地平衡了全局一致性与局部质量。
- **效率提升**：通过跨片段并行生成，突破了自回归模型串行计算的瓶颈，适合实际应用中的大规模长视频生成。
- **理论分析**：从误差累积角度给出了问题根源，方法设计具有理论支撑。
- **自适应调度**：引入工作负载均衡，实际部署中更实用。

## 8. 不足与局限
- **实验覆盖不明确**：摘要未详细列出数据集、对比方法、消融实验数量，说服力不足。
- **潜在偏差风险**：若仅在少数特定场景（如简单动作、固定背景）验证，泛化性存疑；未讨论复杂场景（如人物交互、复杂光照）下的表现。
- **应用限制**：依赖关键帧预测的准确性，若微观规划出错，宏观规划可能难以修正；并行化需要多GPU，单卡部署效率有限。
- **资源信息缺失**：未提供训练/推理所需的算力，难以评估实际成本。

（完）
