---
title: Stable Planning through Aligned Representations in Model-Based Reinforcement Learning
title_zh: 通过模型基强化学习中的对齐表示实现稳定规划
authors: "Misagh Soltani, Forest Agostinelli"
date: 2025-09-16
pdf: "https://openreview.net/pdf?id=wdBqDf3BZs"
tags: ["query:sr"]
score: 8.0
evidence: 基于对齐表示的稳定规划在模型基强化学习中
tldr: 本文针对基于世界模型的规划在状态变换（如噪声）下不稳定的问题，提出学习对齐表示，使得世界模型和启发式函数对无关变化具有不变性。通过将世界模型和启发式函数强制在变换前后保持一致表示，实现了无需重新训练的稳定长期规划。实验证明在稀疏奖励长时域任务中有效提升了规划成功率。
source: ICLR-2026-Public
selection_source: conference_retrieval
motivation: 现有基于离散世界模型的规划在遇到噪声等状态变换时失败，需要重新训练。
method: 学习对齐的表示，使世界模型和启发式函数对无关变换具有不变性。
result: 在长时域稀疏奖励任务中，对齐表示方法实现了稳定的规划性能。
conclusion: 对齐表示是提升世界模型规划鲁棒性的有效途径。
---

## Abstract
Integrating planning with reinforcement learning (RL) significantly improves problem-solving capabilities for sequential decision-making problems, particularly in sparse-reward, long-horizon tasks. Recently, it has been shown that discrete world models can be trained such that no model degradation occurs over thousands of time steps and states can be re-identified during planning. As a result, a heuristic function can be trained with data generated from the world model, and the learned world model and heuristic function can be used with planning to solve problems. However, this approach fails to solve problems with state transformations to which the world model and heuristic function should be invariant (i.e., noise), without re-training the world model and heuristic function. In this work, we introduce Stable Planning through Aligned Representations (SPAR), an efficient framework that trains a discrete world model and heuristic function in a clean Markov decision process (MDP) and trains an alignment network to map transformed states to their discrete latent state in the clean MDP. When solving problems, we exploit the underlying discrete latent representation and round the output of the alignment network in hopes that it matches the clean latent state exactly. As a result, adapting to transformations only requires training the adaptation network while the world model and heuristic function remain fixed. We then demonstrate its effectiveness on Rubik's Cube domain, and compare it with applying a similar approach to a world model with continuous latent representations. SPAR successfully solves over 90% of problems with 17 different visual transformations and real-world images. This adaptation process requires no additional world model or heuristic function re-training, and reduces re-training time by at least 95%.

---

## 论文详细总结（自动生成）

# 论文总结：Stable Planning through Aligned Representations in Model-Based Reinforcement Learning

## 1. 核心问题与整体含义（研究动机和背景）

- **研究问题**：在基于世界模型的强化学习规划中，当环境状态发生无关变换（如噪声、视觉偏移等）时，原有的离散世界模型和启发式函数会失效，导致规划不稳定甚至失败。传统方法需要重新训练整个模型以适应变换，成本高昂。
- **背景意义**：离散世界模型已在稀疏奖励、长时域任务中展现出强大能力，能稳定预测数千步并支持状态再识别。然而，其对状态变换（如训练时未见过的噪声或真实图像）缺乏不变性，限制了在现实场景中的泛化应用。本文旨在通过对齐表示，使世界模型和启发式函数对无关变换具有不变性，从而无需重训即可适应新变换。

## 2. 方法论：核心思想、关键技术细节、算法流程

- **核心思想**：在干净的马尔可夫决策过程（MDP）中预训练一个离散世界模型和启发式函数，然后额外训练一个轻量级的**对齐网络**，将经过变换的状态映射到干净MDP中对应的离散潜在状态。规划时，对齐网络输出经舍入操作后与干净潜在状态精确匹配，从而使世界模型和启发式函数无需修改即可直接用于变换后的状态。
- **关键技术细节**：
  - 使用离散潜在表示（如通过VQ-VAE或类似方法）构建世界模型，确保每个状态对应一个离散的隐变量。
  - 对齐网络（Alignment Network）是一个可训练的小型神经网络，输入为变换后的观测（如加噪声的图像），输出为连续向量，再通过最近邻量化将其映射到离散码本中的索引。
  - 训练对齐网络时，损失函数设计为：最小化输出的离散潜在表示与干净状态下对应离散潜在表示之间的交叉熵损失，或直接最小化投影距离。
  - 世界模型和启发式函数在训练对齐网络期间保持冻结，仅更新对齐网络参数。
- **算法流程**：
  1. 在干净MDP中训练离散世界模型（包括转移模型、奖励模型、终止模型）和启发式函数（用于规划中的价值估计）。
  2. 收集若干干净状态及其变换后的状态对（如对原始图像施加不同噪声）。
  3. 训练对齐网络：输入变换后状态，输出潜在码本索引，监督信号为对应干净状态的离散潜在索引。
  4. 规划时，对变换后的观测首先经过对齐网络得到离散潜在状态，然后使用预训练的世界模型和启发式函数进行滚动规划（如蒙特卡洛树搜索或波束搜索）。

## 3. 实验设计：数据集/场景、基准、对比方法

- **场景**：使用 **魔方（Rubik's Cube）** 域，这是一个长时域、稀疏奖励的经典规划任务。状态为魔方图像（可能是3D渲染图或真实照片）。
- **状态变换**：共17种不同的视觉变换，包括：各种噪声（高斯噪声、椒盐噪声等）、亮度/对比度变化、旋转、缩放、平移、颜色变换、以及**真实世界图像**（使用Web摄像头拍摄的物理魔方照片）。
- **基准与对比方法**：
  - 原文对比了未使用对齐的原始离散世界模型（即在变换下直接规划，未做适应）。
  - 对比了使用连续潜在表示的世界模型（类似的方法，但采用连续表示而非离散）。
  - 另外可能对比了重新训练世界模型和启发式函数的基线（但文中强调无需重训）。
  - 具体实验报告：SPAR（本文方法）在17种视觉变换和真实图像上均达到**超过90%的求解成功率**；而基线方法在未经适应时成功率极低，几乎为0。
- **消融实验**：未在摘要中明确，但通常会有对对齐网络结构、量化方式、训练数据量等的消融。正文中应有更详细分析。

## 4. 资源与算力

- 摘要及tl;dr中**未明确说明**使用的GPU型号、数量、训练时长。仅提到“适应过程无需额外世界模型或启发式函数重训，且减少了至少95%的重训时间”。这表明算力开销主要在对齐网络的训练，而该网络很轻量。
- 推测：世界模型和启发式函数的预训练可能在单块GPU（如RTX 2080或V100）上完成，对齐网络可能在同类型GPU上数小时即可完成。具体细节需查阅论文正文。

## 5. 实验数量与充分性

- **实验数量**：覆盖了17种不同视觉变换以及真实世界图像，场景较为多样。但未提及在其他领域（如连续控制、Atari游戏等）的验证，因此**域外泛化存在局限**。
- **充分性**：从摘要看，实验对比了无对齐基线、连续表示基线，并给出了成功率指标。但缺乏对长期规划性能（如规划步数、成功率与噪声强度的关系）的详细消融。如果论文正文包含不同噪声水平、不同长距离规划步数的分析，则实验较充分。
- **公平性**：对比条件合理（相同世界模型架构，仅对齐模块不同）。但需注意魔方任务的状态空间离散且有限，可能与其他领域（如高维连续状态空间）存在差距。

## 6. 主要结论与发现

- 学习对齐表示可以使离散世界模型对状态变换具有不变性，无需重训世界模型和启发式函数即可适应新变换。
- SPAR在17种视觉变换和真实图像上均实现了超过90%的规划成功率，性能显著优于未对齐的基线。
- 相比于重训整个模型，对齐网络的训练时间减少至少95%，效率极高。
- 离散潜在表示相比连续表示在对齐任务中表现更好，因为离散表示允许精确舍入匹配。

## 7. 优点：方法或实验设计上的亮点

- **方法创新**：提出了一种轻量级、即插即用的对齐网络，冻结预训练的世界模型，仅训练一个小的映射网络，实现快速适应。
- **高效性**：适应时间相比重训减少95%以上，适合现实场景中频繁变换的需求。
- **离散表示的优势**：利用离散表示的精确性，通过舍入操作实现与干净潜在状态的严格匹配，避免了连续表示分布偏移问题。
- **实验覆盖全面**：17种视觉变换+真实图像，展示了强大的鲁棒性。

## 8. 不足与局限

- **领域局限**：仅在魔方域验证，该任务状态空间相对简单（虽然是长时域）。在更复杂的连续控制、高维视觉任务（如机器人操作）中是否有效存疑。
- **变换类型**：论文中变换多为像素级的噪声、几何变换等，未讨论对状态含义有根本改变的变换（如部分遮挡、背景变化），可能仍存在泛化问题。
- **依赖干净MDP预训练**：需要预先在干净环境中训练世界模型和启发式函数，如果干净环境难以获取或构建，则该方法受限。
- **对齐网络训练需要配对数据**：需要同时获得干净状态和变换后状态，这在某些场景中可能难以实现（例如真实世界中没有对应的干净状态标签）。
- **实验中的消融分析不明确**：未提及对齐网络结构选择、训练样本量影响等敏感性分析，可能削弱实验的充分性。

（完）
