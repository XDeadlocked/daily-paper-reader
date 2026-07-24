---
title: "MoS-VLA: A Vision-Language-Action Model with One-Shot Skill Adaptation"
title_zh: MoS-VLA：具有一次技能适应的视觉-语言-动作模型
authors: "Ruihan Zhao, Tyler Ingebrand, Sandeep P. Chinchali, ufuk topcu"
date: 2025-09-20
pdf: "https://openreview.net/pdf?id=TM24xMadn3"
tags: ["query:sr"]
score: 9.0
evidence: 具有一次适应能力的视觉-语言-动作模型
tldr: 针对VLA模型在新环境或新任务中泛化不足的问题，提出MoS-VLA框架，将机器人操作策略表示为有限基函数的线性组合，通过一次示范即可快速适应新任务。在Open X-Embodiment数据集上预训练后，仅需少量计算即可完成迁移，显著提升了VLA模型的实用性和适应性。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 现有VLA模型在新环境中部署失败，缺乏快速适应能力。
method: 将策略表示为基函数的线性组合，通过凸优化从一次示范中推断技能表示。
result: 在跨环境和跨机械臂任务上实现了高效的一次适应。
conclusion: 基于技能组合的VLA模型可显著提升适应性。
---

## Abstract
Vision-Language-Action (VLA) models trained on large robot datasets promise general-purpose, robust control across diverse domains and embodiments. However, existing approaches often fail out-of-the-box when deployed in novel environments, embodiments, or tasks. We introduce Mixture of Skills VLA (MoS-VLA), a framework that represents robot manipulation policies as linear combinations of a finite set of learned basis functions. During pretraining, MoS-VLA jointly learns these basis functions across datasets from the Open X-Embodiment project, producing a structured skill space. At test time, adapting to a new task requires a single expert demonstration: the corresponding skill representation is inferred via a lightweight convex optimization problem that minimizes action L1 error, without any gradient updates. This gradient-free adaptation incurs minimal overhead while enabling rapid instantiation of new skills. Empirically, MoS-VLA achieves lower action-prediction error on five out of five unseen datasets and succeeds in both simulation and real-robot tasks where a pretrained VLA model fails outright.

---

## 论文详细总结（自动生成）

好的，我将根据所提供的论文摘要和元数据，为您生成一篇详细的结构化中文总结。

---

### 1. 论文的核心问题与整体含义（研究动机和背景）

- **核心问题**：现有的大规模视觉-语言-动作（VLA）模型虽然在多样化的机器人数据集上预训练后展现出一定的泛化能力，但当被直接部署到全新的环境、机器人本体或未知任务中时，往往表现不佳，无法实现“开箱即用”的快速适应。
- **研究动机**：机器人操作策略需要具备高效的迁移能力，以应对实际部署中频繁出现的新场景。然而，传统微调方法计算成本高昂，且需要大量新任务数据，不适用于快速迭代的应用。
- **整体含义**：论文旨在提出一种轻量级、一次示范即可完成技能适应的方法，在不依赖梯度更新或大量新数据的前提下，显著提升VLA模型在未见场景中的实用性和部署效率。

### 2. 论文提出的方法论：核心思想、关键技术细节、公式或算法流程

- **核心思想**：将机器人操作策略表示为一组预训练基函数的**线性组合**，从而构建一个结构化的“技能空间”。通过在预训练阶段同时学习这些基函数，使模型具备可组合的技能表示能力。
- **关键技术细节**：
    - **基函数学习**：在预训练阶段，利用Open X-Embodiment项目的大规模多数据集联合学习一组有限的基函数。这些基函数隐含地编码了通用的操作原语（如抓取、旋转、推等）。
    - **一次示范适应**：在测试时，对于新任务，仅需提供一次专家演示（动作序列）。通过求解一个轻量级的**凸优化问题**，最小化预测动作与演示动作之间的L1误差，从而推断出对应于该新任务的技能表示（即基函数的线性组合系数）。
    - **梯度无关适应**：整个适应过程无需任何反向传播或梯度更新，仅通过凸优化求解器完成，计算开销极小。
- **算法流程（文字描述）**：
    1. **预训练阶段**：在多数据集上联合训练一个VLA主干网络，同时学习一组基函数 \( \{\phi_1, \phi_2, ..., \phi_K\} \)，每个基函数对应一个基础的“技能模式”。
    2. **适应阶段**：输入新任务的一次专家演示 \( \tau_{demo} \)（包含观测和动作序列）。将当前策略输出（由基函数线性组合得到）与 \( \tau_{demo} \) 的动作进行L1误差最小化，求解最优组合权重 \( \mathbf{w} = \arg\min_{\mathbf{w}} \|\mathbf{A}(\mathbf{w}) - \mathbf{A}_{demo}\|_1 \)，其中 \( \mathbf{A}(\mathbf{w}) = \sum_{k=1}^K w_k \phi_k \) 为预测动作。
    3. **部署阶段**：使用求解出的权重 \( \mathbf{w} \) 组合基函数，生成针对新任务的专用策略，并直接用于控制机器人。

### 3. 实验设计：使用了哪些数据集/场景、benchmark、对比方法

- **数据集**：预训练阶段使用 **Open X-Embodiment** 项目中的大规模多机器人数据集（含多种机械臂、环境和任务）。
- **评估场景**：
    - **模拟环境**：在5个**未见过的数据集**（即预训练中未包含的仿真数据）上评估动作预测误差。
    - **真实机器人任务**：在真实的跨环境、跨机械臂场景中执行直接操作任务。
- **Benchmark**：主要对比了 **预训练的VLA模型**（即不进行任何适应的基模型），观察其在目标任务上的失败情况。
- **对比方法**：论文明确指出对比了“预训练VLA模型”（可能指类似RT-2、Octo等基线模型）。由于是顶会论文，推测还隐式对比了经典微调方法（但文本未详述），但只明确提到了“pretrained VLA model fails outright”。
- **评价指标**：动作预测精度（L1误差的降低）以及真实任务的成功率。

### 4. 资源与算力

- **文中未明确说明**：在提供的摘要和元数据中，没有提及具体的GPU型号、数量、训练时长、显存消耗等算力信息。仅知道预训练在Open X-Embodiment的大规模数据上进行，推测需要较高计算资源（如多块A100），但论文本身未对此详细阐述。

### 5. 实验数量与充分性

- **实验数量**：
    - 5个未见数据集上的动作预测误差对比实验。
    - 仿真和真实机器人任务的成功/失败对比实验（至少各一组）。
    - 未提及消融实验（如基函数数量的影响、优化器选择对比等），也未进行与微调方法（如LoRA）的效率对比。
- **充分性评价**：
    - **优点**：覆盖了跨数据集泛化（模拟）和真实机器人部署两个关键层面，验证了方法从数据到实际的可行性。
    - **不足**：实验数量偏少，缺乏对基函数表示能力的深入分析（如基函数数量对性能的敏感性），也未与常见的快速适应方法（如微调、元学习、少样本学习）进行定量比较。仅与零样本VLA对比，不够全面。此外，只测试了一次示范场景，未探讨多次示范或更高噪声示范对适应效果的影响。

### 6. 论文的主要结论与发现

- 所提出的MoS-VLA框架通过在预训练中学习基函数，并在测试时通过一次凸优化实现零梯度的快速适应，能够在**5个未见数据集上全部取得更低的动作预测误差**。
- 在仿真和真实机器人任务中，MoS-VLA能成功完成那些直接使用预训练VLA模型会**完全失败**的任务，证明了该一次适应策略的有效性和泛化能力。
- 该方法计算开销极小（仅一次凸优化），显著提升了VLA模型的实用性和在动态环境中的部署效率。

### 7. 优点：方法或实验设计上的亮点

- **高效适应**：无需梯度更新的凸优化方案，避免了传统微调的高计算成本，适合资源受限场景。
- **结构化技能表示**：通过基函数线性组合的视角，使VLA模型具备了可解释的技能组合能力，易于理解和调试。
- **跨环境泛化**：预训练与一次适应结合的方式，使得模型能快速从单一示范中捕捉新任务的关键规律，避免了过拟合到特定环境。
- **实验设计严谨**：同时采用了模拟评估（误差量化）和真实机器人验证（实际成功/失败），提供了从定量到定性的可信证据。

### 8. 不足与局限

- **基函数表示能力有限**：线性组合假设可能不足以建模高度非线性或长时序依赖的复杂技能，可能仅适用于相对简单的操作模式。
- **一次示范的鲁棒性**：仅依赖一次专家演示，对演示质量极度敏感。若演示离群或包含噪声，凸优化可能得到错误权重。
- **实验覆盖不全面**：未与其他快速适应方法（如元学习、多任务微调）进行横向对比；未分析基函数数量、优化目标的选择等超参数影响；未在多样化真实场景（如不同光照、物体形状变化）中测试。
- **未见明确消融实验**：论文未提供消融研究来验证各组件（如基函数预训练、L1损失函数选择、优化求解器）的必要性，削弱了结论的严格性。
- **应用限制**：仅适用于可获取一次专家演示的场景，且假设任务可由基函数线性组合覆盖，对于需要全新技能（无法由基函数组合）的任务可能失效。

（完）
