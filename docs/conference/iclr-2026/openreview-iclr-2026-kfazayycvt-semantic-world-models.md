---
title: Semantic World Models
title_zh: 语义世界模型
authors: "Jacob Berg, Chuning Zhu, Yanda Bao, Ishan Durugkar, Abhishek Gupta"
date: 2025-09-20
pdf: "https://openreview.net/pdf?id=KfaZaYYCvt"
tags: ["query:sr"]
score: 9.0
evidence: 语义世界模型用于机器人规划
tldr: 针对传统世界模型预测未来像素与规划目标不一致的问题，本文提出语义世界模型，将世界建模转化为视觉问答问题，仅预测未来帧中任务相关的语义信息。该方法利用视觉语言模型工具，在机器人规划任务中取得了优于像素级世界模型的表现。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 像素级世界模型的目标与规划决策不一致。
method: 将世界建模视为视觉问答，预测未来帧的语义信息。
result: 语义世界模型在机器人规划任务中取得更好决策效果。
conclusion: 语义预测比像素重建更适合作为世界模型的学习目标。
---

## Abstract
Planning with world models offers a powerful paradigm for robotic control. Conventional approaches train a model to predict future frames conditioned on current frames and actions, which can then be used for planning. However, the objective of predicting future pixels is often at odds with the actual planning objective; strong pixel reconstruction does not always correlate with good planning decisions. We posit that instead of reconstructing future frames as pixels, world models only need to predict task-relevant _semantic_ information about the future. To do this, we pose world modeling as a visual question answering problem, about semantic information in _future frames_. This perspective allows world modeling to be approached with the same tools underlying vision language models. We show how vision language models can be trained as "semantic world models" through a supervised finetuning process on image-action-text data, enabling planning for decision-making while inheriting many of the generalization and robustness properties from the pretrained vision-language models. We demonstrate how such a semantic world model can be used for policy improvement on open-ended robotics tasks, leading to significant generalization improvements over typical paradigms of reconstruction-based action-conditional world modeling.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **背景**：基于世界模型的规划是机器人控制中一种强大的范式。传统方法训练模型根据当前帧和动作预测未来帧（像素级预测），然后用于规划。
- **核心问题**：预测未来像素的目标与实际的规划目标往往不一致——强的像素重建能力并不总是与好的规划决策相关。换言之，像素级世界模型的学习目标与决策目标之间存在错配。
- **研究动机**：论文提出，世界模型不需要重建未来帧的所有像素，而只需预测与任务相关的**语义信息**。这一观点将世界建模从像素预测转化为对**未来帧语义信息**的视觉问答（VQA）问题，从而可以利用视觉语言模型（VLM）的工具进行建模，提升规划性能与泛化能力。

## 2. 论文提出的方法论：核心思想、关键技术细节、公式或算法流程

- **核心思想**：将世界建模视为视觉问答（VQA）问题——预测未来帧中与任务相关的语义信息，而非像素。通过训练视觉语言模型作为“语义世界模型”，实现对未来语义的预测，并用于规划决策。
- **关键技术细节**：
  - 使用预训练的视觉语言模型（VLM）作为基础。
  - 通过**监督微调（SFT）** 在图像-动作-文本数据上训练，使模型学会根据当前帧和动作预测未来帧的语义答案（如物体位置、状态等文本描述）。
  - 训练数据为`(图像, 动作, 文本)`三元组，其中文本是对未来帧语义信息的问答答案。
  - 规划时，利用训练好的语义世界模型滚动预测未来语义状态，然后根据语义状态选择最优动作（策略改进）。
- **算法流程**（文字描述）：
  1. 收集或生成包含当前帧、动作、未来帧语义问答的数据集。
  2. 将预训练VLM的输入格式调整为：图像+动作（可能以文本形式嵌入）+问题（关于未来语义），输出答案。
  3. 在数据集上进行监督微调，使模型学会条件未来语义预测。
  4. 在机器人任务中，使用该模型进行多步语义滚动预测，基于预测的语义状态评估不同动作序列，选择最优动作执行（模型预测控制或策略搜索）。
- **公式**：未提供具体数学公式，但本质上是最大化条件概率 P(语义答案 | 当前图像, 动作)。

## 3. 实验设计：使用了哪些数据集 / 场景，它的 benchmark 是什么，对比了哪些方法

- **数据集/场景**：根据摘要和元数据，实验在**开放式的机器人任务**中进行，具体场景未详述，但提到了“image-action-text data”。从“robotics tasks”和“generalization improvements”推断可能涉及仿真或真实机器人操控任务（如物体抓取、摆放等）。
- **Benchmark**：未明确给出特定基准名称，可能与常见的机器人规划基准（如CALVIN、MetaWorld、RLBench等）相关，但论文未提及。
- **对比方法**：
  - 典型的**基于重建的动作条件世界模型**（reconstruction-based action-conditional world modeling），即传统的像素级预测世界模型（如Dreamer、TD-MPC等）。
  - 未提到与无模型RL或其他方法的对比。

## 4. 资源与算力

- **未明确说明**：文中未提及使用的GPU型号、数量、训练时长等算力信息。仅提到使用预训练的视觉语言模型进行微调，但未说明具体资源消耗。

## 5. 实验数量与充分性

- **实验数量**：从摘要和元数据看，仅概述了主要结果，未列出具体实验组数。典型论文可能包含多个任务、消融实验（如对比不同语义信息粒度、不同VLM骨干等），但本摘要未提供细节。
- **充分性与公平性**：
  - 仅提及“significant generalization improvements”，但缺乏对实验设置、重复次数、统计显著性等详细描述。
  - 对比方法只涉及重建型世界模型，未与其他语义预测方法或规划算法比较，可能不够全面。
  - 因信息不全，无法判断实验是否充分客观。

## 6. 论文的主要结论与发现

- **主要结论**：语义预测（预测任务相关语义信息）比像素重建更适合作为世界模型的学习目标。语义世界模型在机器人规划任务中取得了更好的决策效果和泛化能力。
- **具体发现**：
  - 语义世界模型能够继承预训练VLM的泛化性和鲁棒性。
  - 在开放式机器人任务中，基于语义世界模型的策略改进明显优于基于重建的传统世界模型。

## 7. 优点：方法或实验设计上的亮点

- **方法创新**：将世界建模重新定义为VQA问题，利用成熟的视觉语言模型工具，避免了像素重建的计算冗余和目标不一致问题。
- **简洁高效**：只需预测任务相关的语义信息，无需重建整个场景，模型更轻量且聚焦于决策相关特征。
- **泛化优势**：借助预训练VLM的多模态知识，语义世界模型对新场景、新物体具有更好的泛化能力。
- **实践可行性**：直接使用现有的VLM微调流程，易于实现。

## 8. 不足与局限：包括实验覆盖、偏差风险、应用限制

- **实验覆盖不足**：论文仅给出了概括性结果，缺乏具体任务性能数值、消融实验（如不同语义粒度、不同VLM骨干、不同规划 horizon）的详细分析。
- **偏差风险**：可能仅挑选了展示语义模型优势的任务，未报告性能持平或更差的情况。
- **应用限制**：
  - 依赖于预训练VLM的质量，低质量VLM可能限制性能。
  - 语义信息的设计（问题与答案定义）需要人工先验，不同任务需要定制；可能无法处理需要细粒度连续状态（如精确力控）的任务。
  - 未讨论实时性、计算开销等工程问题，可能不适用于高速实时控制。
- **公平性**：对比方法仅包含重建型世界模型，未与最新的基于潜在表示的世界模型（如DreamerV3、TD-MPC2）或基于语言的世界模型进行对比。

（完）
