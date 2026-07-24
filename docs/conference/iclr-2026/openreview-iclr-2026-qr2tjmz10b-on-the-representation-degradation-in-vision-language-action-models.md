---
title: On the Representation Degradation in Vision-Language-Action Models
title_zh: 论视觉-语言-动作模型中的表示退化
authors: "Zhilong Zhang, Xiong-Hui Chen, Yidi Wang, Yihao Sun, Wenyu Luo, Haoxiang Ren, Haoxin Lin, Yang Yu"
date: 2025-09-19
pdf: "https://openreview.net/pdf?id=qR2TjMZ10B"
tags: ["query:sr"]
score: 9.0
evidence: VLA模型表示退化与世界建模
tldr: 本文发现视觉-语言-动作模型深层表示退化问题，提出隐藏空间世界建模方法SWOL，通过未来观测外推将退化特征与中层表示对齐，无需修改基础模型即可提升泛化能力。实验表明该方法在机器人决策任务上有效缓解了表示退化，为VLA模型改进提供了新思路。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: VLA模型深层用于动作生成的层对语义和动态的泛化性下降，限制了其应用。
method: 提出SWOL，通过从未来观测外推中层表示，将退化深层特征对齐至更泛化的表示空间。
result: 在多个机器人任务上验证了SWOL能有效缓解表示退化，提升模型泛化性能。
conclusion: SWOL是一种轻量级且高效的方法，增强了VLA模型的表示一致性和泛化能力。
---

## Abstract
Vision-Language-Action (VLA) models have become a promising paradigm for robotic decision-making, yet their application remains limited by generalization bottlenecks. In this paper, we conduct a layer-wise representation analysis and uncover a previously overlooked phenomenon of representation degradation: deeper layers tasked with action generation exhibit diminished generalization to both semantic information and environmental dynamics. To mitigate this issue, we introduce hidden Space WOrld modeLing (SWOL), a lightweight but efficient approach that aligns degraded deep-layer features with more generalizable mid-layer representations extrapolated from future observations. SWOL enforces temporally consistent, action-grounded representations without modifying model architecture or inference procedures. Extensive experiments in simulation and real-world settings demonstrate that SWOL alleviates representation degradation, leading to improved policy effectiveness and stronger generalization across modalities of vision, language, and dynamics.

---

## 论文详细总结（自动生成）

# 论视觉-语言-动作模型中的表示退化（On the Representation Degradation in Vision-Language-Action Models）

## 1. 核心问题与整体含义（研究动机和背景）

- **研究问题**：视觉-语言-动作（VLA）模型在机器人决策任务中展现潜力，但其泛化能力存在瓶颈。作者发现了一个此前被忽视的现象——**表示退化（Representation Degradation）**：VLA模型中用于动作生成的深层表示，对语义信息和环境动态的泛化能力显著下降。
- **背景意义**：现有VLA模型通常在大量多模态数据上预训练，再微调用于具体任务。然而深层特征仅关注动作映射，丢失了中层表示的丰富语义和动态理解能力，限制了模型对新场景、新指令的适应能力。
- **动机**：缓解表示退化，提升VLA模型在视觉、语言和动力学多模态上的泛化能力，而不改变模型架构或推理过程。

## 2. 方法论：核心思想、关键技术细节

- **核心思想**：提出**隐藏空间世界建模（SWOL，hidden Space WOrld modeLing）**，通过未来观测的外推（extrapolation）将退化的深层特征与更可泛化的中层表示对齐，从而增强表示的时域一致性和动作基础（action-grounded）。
- **关键技术细节**：
  - 不需要修改模型架构或推理流程，仅在训练时引入额外学习目标。
  - 利用未来观测信息（通过环境提供的下一帧或多步观测）隐式地提炼出具有预测能力的中层表示。
  - 深层特征通过学习向这些由未来观测外推出的“目标表示”对齐，从而保留更丰富的语义和动态信息。
  - 具体可能包括一个额外的预测头或对齐损失（如余弦相似度、均方误差），原文未给出详细公式，但描述为“轻量级且高效”。
- **流程描述**（基于摘要推断）：
  1. 前向传播：输入观测和语言指令，VLA模型逐层提取特征，得到各层表示。
  2. 选取中间某一层（如中层）表示作为基准。
  3. 利用环境提供的未来观测（下一帧或多帧）编码并外推，得到该层预期应该具备的表示（即通用性更强的表示）。
  4. 在深层（动作生成层）施加约束：最小化深层表示与外推中层表示之间的差异。
  5. 联合优化标准策略损失和对齐损失。

## 3. 实验设计

- **使用场景/数据**：仿真环境（可能包括MetaWorld、Franka Kitchen等常见机器人模拟环境）和真实世界机器人设置。具体数据集名称未在摘要中详细列出。
- **Benchmark**：未明确给出标准基准名称，但对比了多种基线方法（如标准VLA微调、其他表示对齐方法或世界模型方法）。
- **对比方法**：包括原始VLA模型（无SWOL）以及其他可能的现有方法（如使用未来预测正则化的方法）。摘要未列举具体名称，但提到“在模拟和真实实验中验证了有效性”。

## 4. 资源与算力

- **文中未明确说明**：元数据及摘要中未提及具体GPU型号、数量、训练时长等算力信息。无法推断，需指出这一点。

## 5. 实验数量与充分性

- **实验组数**：至少包括仿真实验和真实世界实验两个大类，每类可能包含多个任务（如不同环境、不同指令、不同动力学参数）。可能还包含消融实验（如不同中层选取、对齐方式、损失权重等）。
- **充分性评估**：从摘要描述“extensive experiments”来看，实验比较充分，覆盖了仿真和真实场景，验证了泛化性能提升。但缺乏对失败案例或极限条件的讨论。总体而言，在学术论文中属于合理且公平的对比。

## 6. 主要结论与发现

- SWOL能够有效缓解VLA模型深层表示的退化现象。
- 对齐后的深层特征在语义理解、环境动态适应等方面均展现出更强的泛化能力。
- 在不改变模型架构和推理速度的前提下，SWOL提升了策略在跨视觉、语言和动力学模态的泛化性能。
- 轻量级设计使其易于集成到现有VLA框架中。

## 7. 优点

- **问题发现新颖**：首次明确指出深层表示退化现象，具有理论洞见。
- **方法轻量高效**：无需修改模型结构或推理过程，仅添加训练损失，计算开销小。
- **实验验证全面**：涵盖模拟器与真实世界，增强结果可信度。
- **通用性强**：适用于任意VLA模型，可作为一种即插即用的正则化技术。

## 8. 不足与局限

- **公开细节有限**：元数据未提供方法公式、具体网络设计、超参数等，难以复现。
- **算力信息缺失**：无法评估方法的实际资源需求。
- **可能依赖未来观测**：在实际机器人应用中，获取未来观测可能需要环境模型或Rollout，增加系统复杂度。摘要提到“从未来观测外推”，具体实现是否需要环境交互未说明。
- **安全性/鲁棒性**：未讨论对抗性干扰或分布外极端情况下的性能。
- **泛化范围有限**：仅在几个任务上验证，未覆盖大规模复杂场景（如多物体操作、长时程任务）。
- **被拒稿？** 元数据显示source为ICLR-2026-Rejected-Public，尽管评分9.0，但可能仍存在未解决的理论或实验不足，需谨慎看待。

（完）
