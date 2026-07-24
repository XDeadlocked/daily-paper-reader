---
title: "Demystifying Robot Diffusion Policies: Action Memorization and a Simple Lookup Table Alternative"
title_zh: 揭秘机器人扩散策略：动作记忆与简单的查找表替代方案
authors: "Chengyang He, Xu Liu, Gadiel Mark Sznaier Camps, Joseph Bruno, Guillaume Adrien Sartoretti, Mac Schwager"
date: 2026-01-26
pdf: "https://openreview.net/pdf?id=PL0tJOfm7I"
tags: ["query:sr"]
score: 9.0
evidence: 机器人扩散策略的动作记忆本质
tldr: "发现扩散策略在少样本操作任务中表现出色的原因可能是记忆了动作查找表，而非泛化。提出假设并在实验中验证，最终给出一个简单的查找表替代方案，达到与扩散策略相当的性能，揭示了扩散策略的'记忆'本质。"
source: ICLR-2026-Accepted
selection_source: conference_retrieval
motivation: 扩散策略为何在少样本场景有效尚不清楚。
method: 通过实验验证扩散策略本质是记忆动作查找表。
result: 简单查找表可达到与扩散策略相当的效果。
conclusion: 扩散策略在稀疏数据下主要依赖记忆而非泛化。
---

## Abstract
Diffusion policies for visuomotor robot manipulation tasks achieve remarkable dexterity and robustness while only training on a small number of task demonstrations.  However, the reason for this performance remains a mystery. In this paper, we offer a surprising hypothesis: diffusion policies essentially memorize an action lookup table---\emph{and this is beneficial}. We posit that, at runtime, diffusion policies find the closest training image to the test image in a latent space, and recall the associated training action  (i.e. action chunk), offering reactivity without the need for action generalization. This is effective in the sparse data regime, where there is not enough data density for the model to learn action generalization. We support this claim with systematic empirical evidence, showing that even when conditioned on highly out of distribution (OOD) images, Diffusion Policy still outputs an action chunk from the training data. We evaluate and compare three representative policy families on the same data set: Diffusion Policy, Action Chunking with Transformers (ACT), and GR00T, a pre-trained generalist Vision-Language-Action (VLA) model.  We show that Diffusion Policy gives strong action memorization giving surprising robustness in OOD regimes, ACT shows action interpolation with poor robustness in OOD regimes, and GR00T (benefiting from substantial pre-training) shows both action interpolation and OOD robustness. As a simple alternative to Diffusion Policy, we introduce the Action Lookup Table (ALT) policy, showing that an explicit lookup table policy can perform comparably in this low data regime. Despite its simplicity, ALT attains Diffusion Policy–level performance while also providing faster inference and explicit OOD detection via latent-distance thresholds. These results reframe diffusion policies for robot manipulation as reactive memory retrieval under data sparsity, and provide practical tools for interpreting, evaluating, and monitoring such policies. More information can be found at: \url{https://stanfordmsl.github.io/alt/}.

---

## 论文详细总结（自动生成）

以下是基于提供的论文内容生成的详细中文总结：

### 1. 论文的核心问题与整体含义（研究动机和背景）

- **研究动机**：机器人视觉运动操控任务中，扩散策略（Diffusion Policy）仅需少量任务演示（少样本）即可实现惊人的灵活性和鲁棒性，但其成功的原因尚不明确。现有研究多关注其性能，鲜有深入分析其内在机制。
- **核心问题**：扩散策略在少样本操作任务中表现出色，究竟是因为学到了泛化的动作能力，还是仅仅记忆了训练数据中的动作？
- **整体含义**：论文提出一个反直觉的假设——扩散策略本质上是记忆了一个动作查找表（action lookup table），并且这种记忆行为是有益的。在数据稀疏时，模型无需泛化，只需在潜在空间中找到与测试图像最接近的训练图像，直接召回对应的动作块即可。这一发现重新定义了机器人扩散策略的角色：它更接近一种反应式的记忆检索，而非真正的泛化。

### 2. 论文提出的方法论：核心思想、关键技术细节

- **核心思想**：扩散策略在运行时，实际上是在潜在空间中寻找与测试图像最匹配的训练图像，然后直接输出该训练图像对应的动作块（action chunk）。这本质上是一种“记忆检索”行为，而不是学习到的动作泛化。
- **关键技术细节**：
  - 论文通过系统实验验证这一假设：将高度分布外（OOD）的图像输入扩散策略，观察其输出是否仍来自训练数据中的动作块。实验表明，即使测试图像与训练图像差异很大，扩散策略依然会输出训练集中的某个动作块。
  - 作为简单替代方案，论文提出 **Action Lookup Table (ALT)** 策略：直接构建一个显式的查找表，在推理时计算测试图像与所有训练图像在潜在空间的距离，选择最近邻训练图像对应的动作块作为输出。ALT策略无需训练，推理速度快，并且可以通过设置潜在距离阈值实现显式的OOD检测。
- **公式/算法流程**（文字说明）：ALT策略的流程包括：(1) 在训练阶段，将所有演示的图像和对应的动作块（action chunks）存储为键值对（图像特征→动作块）；(2) 在推理阶段，对当前观测图像提取特征（可使用预训练编码器），计算其与所有训练图像特征之间的欧氏距离；(3) 选择距离最小的训练样本，输出其动作块；若最小距离超过预设阈值，则判断为OOD并告警。

### 3. 实验设计：数据集、基准测试与对比方法

- **数据集/场景**：文中未明确列出具体数据集名称，但提到使用了“少样本任务演示”进行实验，并涉及**高度分布外（OOD）图像**的测试。推测是基于常见的机器人操控数据集（如RoboMimic、MetaWorld或自采集的特定任务数据）。
- **基准测试**：论文主要比较了三种具有代表性的策略家族在**同一数据集**上的表现：
  - **Diffusion Policy**（原始扩散策略）
  - **Action Chunking with Transformers (ACT)**
  - **GR00T**（一种预训练的通用视觉-语言-动作（VLA）模型）
- **对比方法**：除了上述三种方法，论文还引入了自己提出的**ALT策略**作为简单替代方案，进行性能比较。

### 4. 资源与算力

- 论文中**未明确提及**所使用的GPU型号、数量以及训练时长。只提到ALT策略可以“提供更快的推理速度”，但未给出具体硬件配置。这可能是因为作者主要关注概念验证和机制分析，而非优化算力。在“资源与算力”方面，需要指出文中缺乏相关细节。

### 5. 实验数量与充分性

- **实验数量**：文中描述的实验主要包括三部分：
  1. 验证扩散策略记忆行为的实验：将模型暴露于OOD图像，观察输出动作是否来自训练数据。
  2. 对比三种策略（Diffusion Policy, ACT, GR00T）在同一数据集上的表现，分别评估其动作记忆能力、OOD鲁棒性和动作插值能力。
  3. 引入ALT策略，并与扩散策略进行直接性能对比，包括任务成功率、推理速度以及OOD检测能力。
- **充分性评估**：
  - **充分性**：实验设计覆盖了核心假设验证，对比了不同类别的策略（扩散、Transformer、预训练VLA），并提出了简单的替代方法，逻辑链条完整。
  - **客观性与公平性**：文中提到“在同一数据集上”评估三种策略，但未说明是否使用了相同的数据预处理、训练超参数等，可能存在隐含偏差。此外，GR00T作为预训练模型，其优势可能部分来源于大规模预训练而非仅依赖于记忆机制，对比可能不完全公平。
  - **局限性**：仅基于少量任务演示（少样本场景）进行实验，未验证在数据充足情况下扩散策略是否也会表现出泛化能力。此外，未提供统计显著性检验或多次重复实验结果。

### 6. 论文的主要结论与发现

- **主要结论**：
  - 扩散策略在少样本场景下的优异性能，主要源于**强烈的动作记忆**，而非真正的动作泛化。
  - ACT策略表现出**动作插值**能力（在训练动作之间进行平滑过渡），但对OOD图像鲁棒性差。
  - GR00T受益于大量预训练，同时具备**动作插值**和**OOD鲁棒性**。
  - 提出的简单**ALT策略**（显式查找表）在低数据场景下可以达到与扩散策略相当的性能，且推理更快，能显式检测OOD输入。
- **发现**：扩散策略本质是一种反应式的记忆检索机制，这一发现有助于解释其鲁棒性来源，并为设计和评估类似策略提供了新视角。

### 7. 优点

- **理论创新**：首次系统性地揭示扩散策略在少样本场景下的“记忆”本质，颠覆了以往认为其具备泛化能力的直觉。
- **方法简洁**：提出的ALT策略极其简单，却能达到与复杂扩散策略相近的效果，体现了奥卡姆剃刀原则。
- **实用工具**：ALT策略提供显式的OOD检测（通过距离阈值），有助于提高机器人部署的安全性，而原扩散策略难以直接实现OOD监控。
- **实验对比全面**：涵盖了扩散策略、Transformer-based策略和预训练VLA模型三种不同范式的代表，使结论更具说服力。

### 8. 不足与局限

- **实验覆盖不足**：
  - 仅局限于少样本场景，未测试数据量充足时扩散策略是否仍仅依赖记忆。
  - 未涉及多种任务类型（如需要精细操作、长时域规划的任务），结论的泛化性存疑。
  - 未提供具体的性能数字（如成功率百分比），仅用文字描述结果，缺乏定量比较。
- **偏差风险**：
  - 对扩散策略“记忆”本质的验证主要依赖OOD测试，但OOD的定义和生成方法可能影响结论（例如，如果OOD图像与训练图像差异过大，任何模型都可能失败或退化到最近邻）。
  - GR00T作为预训练模型，其训练数据可能涵盖更广泛的分布，因此对比可能不公平——其OOD鲁棒性可能来自预训练而非记忆机制。
- **应用限制**：
  - ALT策略需要保存所有训练图像的特征和动作，随着演示数量增加，存储和检索成本会线性增长，在大规模数据下不适用。
  - 显式查找表缺乏插值能力，在需要连续平滑动作的任务中可能表现不佳（例如需要精细轨迹跟踪）。
  - 论文未讨论ALT策略在真实机器人硬件上的部署延迟和实时性，仅提及“推理更快”，但缺乏定量数据。

（完）
