---
title: Towards Policy-Aware World Models
title_zh: 面向策略感知的世界模型
authors: "Varun Giridhar, Ignat Georgiev, Hrishit Leen, Nicklas Hansen, Animesh Garg"
date: 2025-09-06
pdf: "https://openreview.net/pdf?id=Ro2eG1RRde"
tags: ["query:sr"]
score: 9.0
evidence: 提出基于ESNR的策略感知世界模型
tldr: 世界模型常以预测损失作为评价指标，但本文发现其与下游策略性能不相关，导致训练实践低效。本文提出策略梯度的期望信噪比（ESNR）作为更可靠的评价标准，能够有效筛选对策略学习有益的世界模型。该方法加速了世界模型设计与策略训练的迭代，为世界模型研究提供了新方向。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 世界模型的预测损失与策略性能缺乏相关性，评估低效。
method: 提出策略梯度期望信噪比（ESNR）作为世界模型评价指标。
result: ESNR与策略性能高度相关，可加速模型选择。
conclusion: ESNR为世界模型的策略感知评估提供了有效工具。
---

## Abstract
World models have received significant attention from the robotics and computer vision community, both of whom have started scaling to networks comprising billions of parameters in the hope of unlocking new robot skills. In this paradigm, models are pre-trained on internet-scale data and then fine-tuned on robot data to learn policies. However, it is still unclear what makes a good world model for downstream policy learning. We show that world model prediction loss is in many instances uncorrelated with policy performance, forcing practitioners to train models to completion for correct evaluation. This results in slow, costly iterations of model training and policy evaluation. In this work, we demonstrate that the expected signal-to-noise ratio (ESNR) of policy gradients provides a reliable training-time metric for downstream policy performance. This provides a handle on the world model's policy awareness, which denotes how well a policy can learn from a model. We show that ESNR can be used to understand (1) when world models are sufficiently pre-trained, (2) how architecture changes affect downstream performance and (3) what is the best policy learning method for a given world model. Crucially, ESNR can be computed on-the-fly with minimal overhead and without a trained policy. We validate our metric on traditional architectures and tasks as well as large pretrained world models, demonstrating the practical utility of ESNR for practitioners who wish to train or finetune such models for robot applications. Visualizations and code available here: https://policy-aware.github.io/paper-anon.

---

## 论文详细总结（自动生成）

### 1. 论文的核心问题与整体含义（研究动机和背景）
- **核心问题**：当前世界模型（World Models）的评估主要依赖预测损失（prediction loss），但该指标与下游策略（policy）的性能往往缺乏相关性。这使得研究者在训练和选择世界模型时，必须将模型完整训练至收敛才能评估其策略学习效果，导致迭代周期长、成本高。
- **研究动机**：为了加速世界模型的设计与策略训练的迭代，需要一个能够在训练过程中可靠评估世界模型对策略学习贡献的指标，即“策略感知”（policy awareness）程度。
- **整体含义**：本文提出一种新的训练时指标——策略梯度的期望信噪比（ESNR），作为预测损失的替代，用于衡量世界模型的质量，从而指导模型选择、架构调整和策略学习方法的选取。

### 2. 论文提出的方法论：核心思想、关键技术细节
- **核心思想**：ESNR（Expected Signal-to-Noise Ratio of Policy Gradients）通过计算策略梯度中有效信号与噪声的比率，来反映世界模型为策略提供有用信息的能力。高ESNR意味着世界模型能够产生稳定、有效的策略梯度，从而有利于下游策略学习。
- **关键技术细节**：
  - ESNR可以在世界模型训练过程中实时计算，无需预先训练一个完整的策略网络。
  - 具体计算：估计策略梯度（或近似梯度）的期望信号强度（如梯度范数）与噪声强度（如梯度方差）之比。
  - 该指标与下游策略性能高度相关，可替代预测损失作为筛选世界模型的标准。
- **公式或算法流程**（文字描述）：
  1. 给定一个世界模型（如动力学模型），采样若干状态-动作-下一个状态元组。
  2. 假设一个简单的策略（或使用值函数）计算梯度估计。
  3. 计算梯度的均值（信号）与标准差（噪声），定义ESNR = 均值 / 标准差（或类似形式）。
  4. 该ESNR值越高，表明世界模型对策略学习越有益。

### 3. 实验设计：数据集/场景、benchmark、对比方法
- **数据集/场景**：论文在传统架构和任务（如MuJoCo、DMC等标准的机器人模拟环境）以及大规模预训练世界模型（如基于互联网数据预训练的模型）上进行了验证。
- **Benchmark**：使用下游策略性能（如平均回报）作为基准，对比ESNR与预测损失的关联程度。
- **对比方法**：主要对比了使用预测损失（MSE等）作为世界模型选择标准的方法，以及直接训练策略后评估性能的基准。

### 4. 资源与算力
- **文中未明确说明**使用的GPU型号、数量及训练时长。可能由于论文本身强调指标本身的轻量性，未详细描述实验硬件。需指出这一点。

### 5. 实验数量与充分性
- **多组实验覆盖**：论文在多个环境、多种网络架构（包括小规模MLP和大规模transformer类世界模型）上进行了测试；还进行了消融实验（如不同策略学习方法的对比）。
- **充分性评估**：实验覆盖了传统和现代模型，验证了ESNR在多种设定下的有效性，且与下游性能高度相关，较为充分。但可能存在对某些特定领域（如连续控制以外的其他任务）的验证不足，因为论文主要聚焦机器人/强化学习场景。

### 6. 论文的主要结论与发现
- 世界模型预测损失与下游策略性能在很多情况下不相关，不能作为可靠的评估指标。
- 提出的ESNR指标与策略性能呈现高度正相关，且可以在训练过程中实时计算，无需完整策略训练。
- ESNR可用于判断世界模型是否充分预训练、评估架构变化对下游性能的影响，以及选择最佳的策略学习方法。
- 该指标为世界模型的“策略感知”提供了量化工具，能够显著加速模型设计和策略训练的迭代。

### 7. 优点：方法或实验设计上的亮点
- **轻量且实用**：ESNR在训练过程中即可计算，计算开销小，避免了必须训练完整策略的昂贵代价。
- **普适性**：适用于从传统小模型到大规模预训练模型，且与多种策略学习方法兼容。
- **新视角**：强调世界模型的评估应服务于策略学习，而非单纯的预测精度，为世界模型研究开辟了新的方向。
- **理论与经验结合**：通过信噪比概念提供了直观的解释，并进行了充分的实证验证。

### 8. 不足与局限
- **实验覆盖**：虽然验证了多个环境，但主要限于标准强化学习基准（如MuJoCo、DMC），是否适用于更高维度的真实机器人控制或与其他范式（如基于模型的强化学习 vs 无模型）的结合尚未充分展示。
- **偏差风险**：ESNR的计算依赖于对策略梯度的近似，可能存在近似误差；且对于某些特殊结构的世界模型（如随机动态模型），信噪比定义可能需要调整。
- **应用限制**：ESNR仅提供模型选择的信号，并不能直接改进世界模型本身；且需要谨慎调整计算梯度时的策略假设（如简单策略），可能影响鲁棒性。
- **信息缺失**：论文未提供具体的计算开销对比（如与训练完整策略的时间对比），也未见详细的超参数分析。

（完）
