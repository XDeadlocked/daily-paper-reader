---
title: "VITA-VLA: Efficiently Teaching Vision-Language Models to Act via Action Expert Distillation"
title_zh: VITA-VLA：通过动作专家蒸馏高效教授视觉语言模型执行动作
authors: "Shaoqi Dong, Chaoyou Fu, Haihan Gao, YiFan Zhang, Chi Yan, Chu Wu, Xiaoyu Liu, Yunhang Shen, Jing Huo, Deqiang Jiang, Haoyu Cao, Yang Gao, Xing Sun, Ran He, Caifeng Shan"
date: 2025-09-05
pdf: "https://openreview.net/pdf?id=dIqJaNbHmP"
tags: ["query:sr"]
score: 9.0
evidence: 蒸馏框架使VLM具备动作执行能力，用于VLA
tldr: 该论文提出基于蒸馏的VLA框架，通过从预训练的小型动作模型转移知识来赋予VLM动作执行能力。仅添加动作令牌和状态编码器，保留原始VLM结构，以低成本实现高效训练。实验表明该方法在多种操作任务上提升泛化性和鲁棒性。
source: ICLR-2026-Public
selection_source: conference_retrieval
motivation: 端到端训练VLA模型成本高昂且需要大量数据。
method: 提出动作专家蒸馏框架，从预训练小模型转移动作知识到VLM。
result: 在多个机器人操作任务上取得高效且鲁棒的性能。
conclusion: 蒸馏策略可低成本构建VLA模型，并保持VLM的感知能力。
---

## Abstract
Vision-Language Action (VLA) models significantly advance robotic manipulation by leveraging the strong perception capabilities of pretrained vision-language models (VLMs). By integrating action modules into these pretrained models, VLA methods exhibit improved generalization and robustness. However, training them end-to-end is costly, as modeling action distributions typically requires massive datasets and heavy computation. In this work, we propose a simple yet effective distillation-based framework that equips VLMs with action-execution capability by transferring knowledge from pretrained small action models. Our architecture retains the original VLM structure, adding only an action token and a state encoder to incorporate physical inputs, as illustrated in Figure 1. To distill action knowledge, we adopt a two-stage training strategy. First, we perform lightweight alignment by mapping VLM hidden states into the action space of the small action model, enabling effective reuse of its pretrained action decoder and avoiding expensive end-to-end pretraining. This also facilitates better transfer of action modeling capabilities to the VLM. Second, we selectively fine-tune the language model, state encoder, and action modules, enabling the system to integrate multimodal inputs with precise action generation. Specifically, the action token provides the VLM with a direct handle for predicting future actions, while the state encoder allows the model to incorporate robot dynamics not captured by vision alone (see Figure 2). This design yields substantial efficiency gains over training large VLA models from scratch. Compared with previous state-of-the-art methods, our method achieves 97.3\% average success rate on LIBERO (11.8\% improvement), 93.5\% on LIBERO-LONG (24.5\% improvement), 92.5\% first task success rate on CALVIN ABC-D (4.1\% improvement). In real-world experiments across five manipulation tasks, our method consistently outperforms the teacher model Seer, achieving 82.0\% average success rate (17\% improvement). These results demonstrate that action distillation effectively enables VLMs to generate precise, executable actions while substantially reducing training costs.

---

## 论文详细总结（自动生成）

### 1. 论文的核心问题与整体含义（研究动机和背景）

- **问题**：端到端训练视觉-语言-动作（VLA）模型成本高昂，需要海量数据和大量计算资源，因为建模动作分布通常很复杂。
- **背景**：预训练的视觉语言模型（VLM）具备强大的感知能力，但直接将其用于机器人动作生成需要集成动作模块。现有 VLA 方法虽提升了泛化性和鲁棒性，但训练开销巨大。因此，需要一种低成本、高效的方法让 VLM 获得动作执行能力。
- **整体含义**：提出一种基于蒸馏的框架，通过从预训练的小型动作模型迁移知识，避免昂贵的端到端预训练，从而以较低代价构建 VLA 模型，同时保持 VLM 原有的感知能力。

### 2. 论文提出的方法论：核心思想、关键技术细节

- **核心思想**：动作专家蒸馏（Action Expert Distillation）：将预训练小动作模型的动作知识转移到 VLM 中，而无需从头训练大规模 VLA 模型。
- **关键技术细节**：
  - **架构保留**：保留原始 VLM 结构，仅添加一个**动作令牌（action token）**和一个**状态编码器（state encoder）**。动作令牌为 VLM 提供直接预测未来动作的句柄；状态编码器将物理输入（如机器人动力学信息）整合进模型，弥补纯视觉输入无法捕获的物理状态。
  - **两阶段训练策略**：
    1. **第一阶段：轻量对齐**。将 VLM 的隐藏状态映射到小动作模型的动作空间，复用其预训练的动作解码器，避免端到端预训练的高开销，同时促进动作建模能力的迁移。
    2. **第二阶段：选择性微调**。对语言模型、状态编码器和动作模块进行微调，使系统能够融合多模态输入并生成精确动作。
- **公式/算法**（文字说明）：无显式公式，但训练流程可概括为：VLM 提取视觉和语言特征 → 动作令牌与状态编码器融合物理状态 → 通过对齐模块映射到小动作模型的动作空间 → 两阶段优化（先对齐，后联合微调）。

### 3. 实验设计：使用的数据集、场景、基准与对比方法

- **数据集/场景**：
  - **LIBERO**（基准操作任务）
  - **LIBERO-LONG**（长时程任务）
  - **CALVIN ABC-D**（连续多任务场景，关注首次任务成功率）
  - **真实世界实验**：包含5种不同操作任务
- **基准（Benchmark）**：上述数据集的标准评估协议。
- **对比方法**：
  - 与先前最先进的 VLA 方法比较（摘要中未列出具体方法名，但提及“previous state-of-the-art methods”）。
  - 与教师模型 **Seer** 在真实世界实验中比较（Seer 是一个预训练的小动作模型）。
- **关键结果**：
  - LIBERO: 平均成功率 **97.3%**（提升 11.8%）
  - LIBERO-LONG: 平均成功率 **93.5%**（提升 24.5%）
  - CALVIN ABC-D: 首次任务成功率 **92.5%**（提升 4.1%）
  - 真实世界5任务：平均成功率 **82.0%**（比教师 Seer 提升 17%）

### 4. 资源与算力

- 文**未明确说明**使用的 GPU 型号、数量及训练时长。摘要和元数据中均未提及算力相关细节，因此无法总结。但可以推断，由于采用了蒸馏和轻量对齐，所需算力应远低于端到端训练大 VLA 模型。

### 5. 实验数量与充分性

- **实验组数**：涉及3个公开数据集（LIBERO、LIBERO-LONG、CALVIN ABC-D）以及真实世界的5个任务，涵盖了模拟和实物场景。
- **充分性与公平性**：实验覆盖了多个难度递进的任务（短任务、长任务、连续任务），且与最先进方法及教师模型对比，结果有显著提升。但摘要中未提及消融实验（如不同蒸馏策略、是否使用状态编码器等），也未给出方差或统计显著性。总体而言，实验设计较为充分，但缺少对方法本身的深入剖析（如各组件贡献）。结果报告直接为平均成功率提升，对比清晰。

### 6. 论文的主要结论与发现

- 蒸馏框架能够高效地让 VLM 获得精确的可执行动作生成能力，同时保持其感知能力。
- 采用两阶段训练策略（轻量对齐+选择性微调）可以大幅降低训练成本，并实现比教师模型和先前最先进方法更好的性能。
- 在多个公开基准和真实世界任务上，该方法均取得了显著改进，验证了动作蒸馏的有效性和泛化能力。

### 7. 优点

- **方法简洁高效**：仅添加一个动作令牌和一个状态编码器，保留 VLM 原有结构，易于实现和迁移。
- **训练成本低**：通过蒸馏复用预训练小模型的动作解码器，避免了昂贵的大规模端到端预训练。
- **性能提升显著**：在多个难度不同的任务上取得大幅提升，特别是在 LIBERO-LONG 上提升 24.5%，真实世界提升 17%。
- **泛化性与鲁棒性好**：在模拟和实物场景中均优于教师模型，表明蒸馏后的 VLM 能更好地处理多模态输入与机器人动力学。
- **实用性强**：适用于资源受限的场景，为低成本构建 VLA 模型提供了可行路径。

### 8. 不足与局限

- **缺乏消融研究**：摘要中未报告关键组件（如动作令牌、状态编码器、两阶段训练 vs 单阶段等）的消融实验，导致难以评估各部分的独立贡献。
- **未说明计算资源**：无具体 GPU 数量和训练时间，无法评估实际训练成本，也限制了可复现性。
- **真实世界任务规模有限**：仅5个任务，且未披露任务类型、难度和随机性，可能不足以完全证明泛化能力。
- **依赖预训练动作模型**：蒸馏效果受教师模型（Seer）质量影响，若教师模型本身性能不佳，提升可能受限。
- **未讨论局限性**：如蒸馏是否会导致 VLM 丧失部分语言理解能力？是否对未见过场景鲁棒？这些未提及。
- **统计细节缺失**：未报告置信区间或重复实验次数，结果可靠性有待验证。

（完）
