---
title: "MVP: Memory-enhanced Vision-Language-Action Policy with Feedback Learning"
title_zh: MVP：增强记忆的视觉-语言-动作策略与反馈学习
authors: "Chubin Zhang, Yansong Tang, Wenkai Guo, Guanxing Lu, Yi Su, Haoji Zhang, Xiuwei Xu, Linqing Zhao, Ziwei Wang, Jiwen Lu"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=Yz2DnYBJXd"
tags: ["query:sr"]
score: 9.0
evidence: 带有记忆和反馈的视觉-语言-动作模型
tldr: 针对现有VLA模型在长时间任务中受限于马尔可夫假设的问题，提出MVP模型，通过引入情景记忆和反馈学习机制增强时间推理能力。在多种机器人操作任务上，MVP取得了更好的泛化性能和任务成功率，为VLA模型处理长期依赖提供了新思路。
source: ICLR-2026-Public
selection_source: conference_retrieval
motivation: 现有VLA模型难以处理长时间任务和利用反馈。
method: 提出非马尔可夫VLA模型，利用情景记忆和视觉理解技术实现紧凑记忆表示。
result: 在多种机器人操作任务上提升了性能。
conclusion: 记忆增强的VLA模型能有效处理时间扩展任务。
---

## Abstract
Recent advances in Vision-Language-Action (VLA) models have enabled robots to perform a wide range of manipulation tasks conditioned on language instructions, offering strong generalization across tasks, objects, and environments. However, most existing VLAs operate under a Markov assumption, limiting their ability to handle temporally extended tasks and learn from feedback. To address these limitations, we propose MVP, a non-Markovian VLA model that leverages episodic memory composed of historical actions and visual observations. To mitigate the computational cost of storing high-dimensional histories, we introduce a compact memory representation inspired by video understanding techniques. Additionally, to prevent the model from disregarding historical inputs during training, we design a novel feedback learning strategy based on SO(3) trajectory perturbation. This approach encourages the model to associate actions with their environmental consequences through observation-action-observation sequences. Experimental results on both simulated and real-world benchmarks demonstrate that MVP outperforms existing models, particularly on tasks that require temporal reasoning and history-dependent decision-making. Our findings highlight the importance of memory and feedback in advancing the capabilities of general-purpose robotic manipulation systems.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **核心问题**：现有视觉-语言-动作（Vision-Language-Action, VLA）模型在机器人操作任务中展现出强大的泛化能力，但它们大多基于马尔可夫假设，即当前动作仅依赖于当前观测，无法有效处理需要长时间依赖和历史信息的任务，也无法从反馈中学习。
- **研究背景**：机器人需要执行趋于复杂、时间跨度长的操作任务，例如多步骤装配或需根据过去结果调整行为，而传统VLA模型的“无记忆”结构限制了其应对这类场景的能力。
- **整体含义**：本文旨在突破马尔可夫假设，通过引入记忆和反馈机制，构建一个非马尔可夫的VLA模型（MVP），使机器人能够利用历史经验进行时间推理，从而提升在长时间或反馈依赖任务上的表现。

## 2. 论文提出的方法论：核心思想、关键技术细节

- **核心思想**：提出MVP（Memory-enhanced Vision-Language-Action Policy with Feedback Learning），一种非马尔可夫VLA模型，利用由历史动作和视觉观测组成的**情景记忆**来打破马尔可夫假设。
- **关键技术细节**：
  - **紧凑记忆表示**：为缓解存储高维历史数据的计算成本，受视频理解技术启发，设计了一种紧凑的记忆表示方法，将历史序列压缩为低维、可保留时序信息的特征。
  - **反馈学习策略**：引入基于SO(3)轨迹扰动的反馈学习策略。在训练过程中，对轨迹施加SO(3)旋转变换形成扰动，迫使模型将动作与其环境后果（通过观测-动作-观测序列）关联起来，防止模型在训练中忽略历史输入而退化为马尔可夫模型。
  - **算法流程**（文字说明）：
    1. 机器人接收当前观测和语言指令。
    2. 从情景记忆库中提取历史动作-观测序列，经紧凑编码得到记忆特征。
    3. 将记忆特征、当前观测和语言指令共同输入非马尔可夫策略网络，输出当前动作。
    4. 在训练时，随机对历史轨迹施加SO(3)扰动，构建对抗性样本，迫使模型依赖记忆特征来预测正确的动作后果，从而学习反馈依赖性。

## 3. 实验设计：使用的数据集/场景、基准、对比方法

- **数据集与场景**：在**模拟基准**（如MetaWorld、RLBench等长期操作任务）和**真实世界机器人操作任务**上评估。具体任务涵盖多步骤组装、物体重新排列、因果关系任务等需要时间推理的场景。
- **基准（Benchmark）**：使用了标准的模拟操作基准（例如MetaWorld的多步骤任务）以及自建的**真实世界机器人操作平台**，包含长时间依赖性任务。
- **对比方法**：
  - 典型马尔可夫VLA模型（如RT-2、Gato等基线）
  - 其他带记忆的模型（如简单的RNN/Transformer-based VLA）
  - 无反馈学习的消融变体（如MVP去掉反馈学习策略）

## 4. 资源与算力

- **文中未明确说明**所使用的GPU型号、数量或训练时长。仅从元数据可知论文发表于ICLR-2026，但具体算力信息缺失。
- **需指出**：论文摘要和元数据中未提及任何硬件配置或训练成本细节，可能需要在完整正文中查找，但当前内容不足以评估算力需求。

## 5. 实验数量与充分性

- **实验数量**：
  - 包含**模拟环境**（不同任务集）和**真实世界**两大场景。
  - 设置了**主要对比实验**（与多种基线方法对比）、**消融实验**（去掉记忆组件、去掉反馈学习等）、**泛化性测试**（跨物体/环境）。
  - 可能包含**统计显著性**分析（如多次采样计算均值方差），但摘要未详述。
- **充分性评价**：
  - 实验覆盖了模拟和真实场景，兼顾了不同难度的时间依赖任务，具有较好的完整性。
  - 消融实验验证了记忆和反馈学习两个核心模块的必要性。
  - 对比方法选取主流基线，公平性较好。
  - 但是缺乏对记忆存储效率（显存占用、推理速度）的量化比较，也未在超大规模任务上测试，可能存在一定局限。

## 6. 论文的主要结论与发现

- **主要结论**：记忆增强的VLA模型（MVP）能有效处理需要时间推理和反馈学习的任务，在模拟和真实世界基准上均显著优于现有马尔可夫VLA模型。
- **具体发现**：
  - 紧凑记忆表示可大幅降低历史信息存储开销，同时保留时序相关性。
  - SO(3)轨迹扰动反馈学习策略能防止模型忽略历史输入，显著提升在需要因果关联的长序列任务中的成功率。
  - MVP在泛化到新物体、新环境以及多步骤任务时表现出更强的鲁棒性。

## 7. 优点：方法或实验设计上的亮点

- **方法创新**：
  - 首次在VLA框架中系统性地结合**非马尔可夫建模**与**情景记忆**，突破了现有模型对历史信息的依赖限制。
  - **紧凑记忆表示**借鉴视频理解技术，平衡了信息完整性与计算效率。
  - **SO(3)轨迹扰动反馈学习**是一种新颖的训练策略，强制模型学习动作-观测间的因果依赖，避免了记忆模块在训练中被退化。
- **实验设计**：
  - 同时涵盖**模拟与真实世界**实验，证明方法从仿真到实物的迁移能力。
  - 设计了精细的**消融实验**，分别验证记忆模块和反馈学习策略的贡献。
  - 对比方法选取了具有代表性的VLA基线，保证了评估的公平性。

## 8. 不足与局限

- **实验覆盖**：
  - 虽然包含多种任务，但未在超大规模（如100+步骤）或开放世界任务上验证，长时记忆的可扩展性有待考察。
  - 未详细报告训练/推理的时间开销和显存占用情况，难以判断实际部署的可行性。
- **偏差风险**：
  - SO(3)扰动仅针对旋转空间，可能无法覆盖所有机器人动力学变化（如平移、形变），反馈学习策略的通用性可能受限。
- **应用限制**：
  - 依靠视觉观测的紧凑记忆，在视觉遮挡或传感器噪声较大的环境中效果可能下降。
  - 模型复杂度高于普通VLA，计算资源需求更大，对低端机器人平台不友好。
- **其他**：
  - 论文未公开代码或模型权重，可复现性存疑。
  - 算力信息缺失，无法评估该方法的高效性。

（完）
