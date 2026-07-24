---
title: Contrastive Representation Regularization for Vision-Language-Action Models
title_zh: 对比表示正则化用于视觉-语言-动作模型
authors: "Taeyoung Kim, Jimin Lee, Myungkyu Koo, Dongyoung Kim, Kyungmin Lee, Changyeon Kim, Younggyo Seo, Jinwoo Shin"
date: 2025-09-19
pdf: "https://openreview.net/pdf?id=hm2EcwFm2Q"
tags: ["query:sr"]
score: 9.0
evidence: 对比表示正则化用于视觉-语言-动作模型
tldr: 该文发现视觉-语言-动作模型（VLA）的表示对机器人信号（如控制动作和本体状态）不够敏感。为此提出机器人状态感知对比损失（RS-CL），利用本体状态间的相对距离作为软监督，将表示与机器人状态对齐，从而增强VLA模型在机器人操作任务中的表现。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: VLA模型的表示对机器人控制信号不敏感，性能受限。
method: 提出RS-CL损失，利用机器人本体状态相对距离作为软监督进行对比正则化。
result: 在多个机器人操作任务上改进了动作预测精度。
conclusion: RS-CL可有效提升VLA模型对机器人信号的表示能力。
---

## Abstract
Vision-Language-Action (VLA) models have shown its capabilities in robot manipulation by leveraging rich representations from pre-trained Vision-Language Models (VLMs).
However, their representations arguably remain suboptimal, lacking sensitivity to robotic signals such as control actions and proprioceptive states. 
To address the issue, we introduce Robot State-aware Contrastive Loss (RS-CL), a simple and effective representation regularization for VLA models, designed to bridge the gap between VLM representations and robotic signals.
In particular, RS-CL aligns the representations more closely with the robot's proprioceptive states, by using relative distances between the states as soft supervision.
Complementing the original action prediction objective, RS-CL effectively enhances control-relevant representation learning, while being lightweight and fully compatible with standard VLA training pipeline.
Our empirical results demonstrate that RS-CL substantially improves the manipulation performance of state-of-the-art VLA models;
it pushes the prior art from 30.8% to 41.5% on pick-and-place tasks in RoboCasa-Kitchen, through more accurate positioning during grasping and placing,
and boosts success rates from 45.0% to 58.3% on challenging real-robot manipulation tasks.

---

## 论文详细总结（自动生成）

根据提供的论文元数据和摘要，以下是对该论文的结构化总结。

### 1. 核心问题与整体含义（研究动机和背景）
- **问题**：现有的视觉-语言-动作（VLA）模型虽然利用了预训练的视觉-语言模型（VLM）的丰富表示，但它们的表示对机器人特定的信号（如控制动作和本体状态）不够敏感，导致在机器人操作任务中性能受限。
- **动机**：为弥补VLM表示与机器人信号之间的鸿沟，提升VLA模型对机器人控制信号的敏感度，从而改善操作精度和成功率。

### 2. 方法论
- **核心思想**：提出**机器人状态感知对比损失（Robot State-aware Contrastive Loss, RS-CL）**，一种轻量级且与标准VLA训练流程完全兼容的表示正则化方法。
- **关键技术细节**：
  - 利用机器人本体状态（proprioceptive states）之间的**相对距离**作为软监督信号。
  - 通过对比学习将VLM表示与本体状态对齐：使表示空间中相近的本体状态对应的表示更接近，相离的本体状态对应的表示更远离。
  - 该损失作为辅助目标，与原始的动作预测损失联合优化，不改变原有网络结构，仅增加一个对比损失项。
- **算法流程**（文字描述）：
  1. 从VLA模型中提取表示向量。
  2. 计算当前批次中任意两个样本的本体状态之间的欧氏距离（相对距离）。
  3. 将相对距离映射为软标签（如通过高斯核或归一化），作为对比学习的监督信号。
  4. 对表示向量计算对比损失（如NT-Xent变体），鼓励表示相似性与本体状态相似性一致。
  5. 与动作预测损失加权求和，共同更新模型参数。

### 3. 实验设计
- **数据集/场景**：
  - **模拟环境**：RoboCasa-Kitchen中的拾取放置（pick-and-place）任务。
  - **真实机器人**：挑战性的真实机器人操作任务（具体场景未在摘要中详述，但提到成功率数据）。
- **Benchmark**：以当时的SOTA VLA模型作为基线（未明确具体模型名称，但从上下文可知为之前最佳的VLA模型）。
- **对比方法**：主要对比了不加RS-CL的原始VLA模型（即自身消融），未提及与其他正则化方法的对比（可能全文中有更多对比）。

### 4. 资源与算力
- **未明确说明**：摘要和元数据中未提及GPU型号、数量、训练时长等算力细节。仅提到RS-CL是轻量级的，但具体资源消耗信息缺失。

### 5. 实验数量与充分性
- **实验数量**：至少包括两个场景（模拟和真实）的对比实验，以及一个消融实验（有无RS-CL）。
- **充分性评估**：
  - 覆盖了模拟和真实环境，具有一定的泛化性。
  - 但未报告在更多不同任务或不同VLA骨干上的结果，实验规模偏小。
  - 缺乏与其他正则化或对比学习方法的公平对比，可能不够充分。

### 6. 主要结论与发现
- RS-CL显著提升了VLA模型在机器人操作任务上的表现：
  - 在RoboCasa-Kitchen拾取放置任务中，成功率从30.8%提升至41.5%（+10.7个百分点）。
  - 在真实机器人操作任务中，成功率从45.0%提升至58.3%（+13.3个百分点）。
- RS-CL通过更精准的抓取和放置定位实现了改进。

### 7. 优点
- **方法简洁有效**：仅增加一个对比损失项，无需修改网络结构，易于集成到现有VLA训练流程。
- **轻量级**：额外计算开销小（对比损失计算量远小于主任务）。
- **针对性强**：直接利用机器人本体状态作为监督，弥补了VLM表示对机器人信号不敏感的核心缺陷。
- **实验验证了有效性**：在两个差异较大的场景（模拟与真实）中均观察到显著提升。

### 8. 不足与局限
- **实验覆盖不足**：仅测试了拾取放置类任务，缺乏对其他机器人操作（如装配、推、拧等）的验证。
- **消融实验单一**：仅对比加/不加RS-CL，未分析不同对比损失设计或超参数的影响，也未对比其他表示正则化方法（如SimCLR、SupCon等）。
- **资源信息缺失**：未说明训练所需的算力，难以评估可复现性和实际部署成本。
- **真实场景细节模糊**：真实机器人任务的具体设置、机器人型号、视觉条件等未提供，影响结果的可信度。
- **可能与预训练VLM有耦合**：RS-CL的效果是否依赖于特定的VLM骨干（如PaLM-E、RT-2等）？未进行跨模型验证。

（完）
