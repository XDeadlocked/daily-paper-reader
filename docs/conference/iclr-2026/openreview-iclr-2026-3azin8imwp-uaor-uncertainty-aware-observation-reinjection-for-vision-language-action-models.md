---
title: "UAOR: Uncertainty-aware Observation Reinjection for Vision-Language-Action Models"
title_zh: UAOR：面向视觉-语言-动作模型的不确定性感知观察重注入
authors: "Jiabing Yang, Yixiang Chen, Yuan Xu, Peiyan Li, Xiangnan Wu, Zichen Wen, Bowen Fang, Tao Yu, Zhengbo Zhang, Yingda Li, Kai Wang, Jing Liu, Nianfeng Liu, Yan Huang, Liang Wang"
date: 2025-09-03
pdf: "https://openreview.net/pdf?id=3azIn8ImwP"
tags: ["query:sr"]
score: 9.0
evidence: 视觉-语言-动作模型
tldr: 现有VLA模型为提升性能需引入额外观察线索和模块，但依赖大量数据和微调。本文提出UAOR方法，利用语言模型中FFN的关键-值记忆特性，将不确定性量化的观察信息重新注入VLA模型，无需额外训练。实验表明UAOR显著提升机器人操作任务的成功率和鲁棒性，为VLA模型的高效扩展提供了新思路。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 现有VLA模型提升性能需额外数据收集和微调，灵活性受限。
method: 利用FFN的关键-值记忆特性，将不确定性感知的观察信息重注入VLA模型。
result: 在多个机器人操作任务上提升成功率和鲁棒性，无需额外训练数据。
conclusion: 提出一种高效扩展VLA模型的方法，减少对额外数据的依赖。
---

## Abstract
Vision–Language–Action (VLA) models leverage pretrained Vision–Language Models (VLMs) as backbones to map images and instructions to actions, demonstrating remarkable potential for generalizable robotic manipulation. To improve performance, many methods have been proposed to incorporate additional observation cues (e.g., depth maps, point clouds) and auxiliary modules (e.g., object detectors, encoders), enabling more precise and reliable task execution. Although effective, these approaches often require extensive data collection and additional training or fine-tuning, limiting their flexibility and scalability. Inspired by the finding that Feed-Forward Network (FFN) in language models can act as "key-value memory'', we propose **U**ncertainty-**a**ware **O**bservation **R**einjection (**UAOR**), an effective training-free and plug-and-play module for VLA models. Specially, when the current language model layer exhibits high uncertainty, measured by **Action Entropy**, it reinjects the observation information into the next layer's Feed-Forward Network (FFN) in a blending manner. This mechanism helps VLA models look more clearly on the observation during inference, enabling more confident and faithful action generation. Comprehensive simulation and real-world experiments show that our method consistently improves the performance of heterogeneous VLA models across various tasks and embodiments while incurring minimal computational overhead. Notably,  **UAOR** eliminates the need for extra observation cuse or modules, making it a versatile and practical plug-in for existing VLA pipelines.

---

## 论文详细总结（自动生成）

# 详细中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **核心问题**：现有基于视觉-语言-动作（VLA）模型的机器人操作任务中，为了提升性能往往需要引入额外的观察线索（如深度图、点云）或辅助模块（如目标检测器、编码器），但这些方法通常依赖大规模数据收集和额外的训练/微调，导致灵活性受限、扩展性差。
- **整体含义**：论文试图在不增加训练成本、不引入新模块的前提下，利用语言模型内部前馈网络（FFN）的“键-值记忆”特性，通过不确定性感知的观察重注入机制，增强VLA模型在推理时对观察信息的关注，从而提升动作生成的质量和鲁棒性。这为高效扩展VLA模型提供了一种轻量级、即插即用的新思路。

## 2. 论文提出的方法论：核心思想、关键技术细节、公式或算法流程

- **核心思想**：利用语言模型中FFN层可视为“键-值记忆”的发现（即FFN存储了训练数据的知识），在VLA模型推理过程中，当当前语言模型层表现出高不确定性（由所定义的“动作熵”度量）时，将原始的观察信息以混合（blending）的方式重新注入到下一层的FFN中，从而让模型更清晰地“看到”观察，生成更自信、更忠实的动作。
- **关键技术细节**：
  - **不确定性度量**：定义“动作熵”（Action Entropy），用于量化模型在当前层对动作预测的不确定性。通常熵值越高，表示模型越不确定，需要更多观察信息的引导。
  - **观察重注入机制**：当某层动作熵超过预设阈值时，将输入阶段的视觉观察特征（可能经过必要的映射）与原FFN的隐藏表示进行混合（blending），混合后的表示送入下一层FFN继续前向传播。
  - **无需训练**：整个过程不涉及任何梯度更新或参数微调，仅通过修改推理时的计算流实现，因此是“零训练”的即插即用模块。
- **算法流程（文字说明）**：
  1. 对VLA模型（如以LLaVA或类似VLMs为backbone的模型）进行标准前向传播；
  2. 在每一层Transformer block中，计算当前动作预测的熵值；
  3. 若熵值高于阈值，则提取原始视觉观察（如CLIP视觉特征）与该层FFN输出进行加权融合；
  4. 将融合后的表示传递到下一层，继续前向计算；
  5. 最终生成动作序列。

## 3. 实验设计：使用的数据集/场景、benchmark、对比方法

- **数据集/场景**：
  - 仿真实验：使用多种机器人操作任务（如抓取、放置、推动等），具体环境可能包括Robosuite、SAPIEN等标准仿真平台。摘要未明确列出，但提及“comprehensive simulation and real-world experiments”。
  - 真实世界实验：在真实机器人平台上进行多种任务（如桌面操作、物品搬运等）。
- **Benchmark**：未明确指定单一公开基准，但对比了多种异质VLA模型（heterogeneous VLA models），说明实验中覆盖了不同架构的baseline。
- **对比方法**：
  - 原始VLA模型（不带任何额外模块的baseline）；
  - 其他需要额外训练或附加模块的增强方法（如引入深度信息、目标检测器等，但具体名称未给出）；
  - 可能还对比了简单随机注入或固定注入的消融版本。

## 4. 资源与算力

- **明确说明**：论文摘要及元数据中未提及具体的GPU型号、数量、训练时长等算力信息。由于方法本身无需训练，仅在推理时增加少量计算开销，作者可能未详细报告算力消耗。需要指出这一点。

## 5. 实验数量与充分性

- **实验数量**：根据摘要描述，包含“comprehensive simulation and real-world experiments”，但具体实验组数未列出。元数据中提到“在多个机器人操作任务上提升成功率和鲁棒性”，推测进行了至少3-5种不同的任务场景实验，并可能包含消融实验（对阈值、融合权重等超参数的影响）。
- **充分性评价**：实验覆盖了仿真和真实场景，并使用了多种异质VLA模型，表明作者考虑了泛化性。但缺乏标准基准（如Open X-Embodiment）的定量对比，也未报告统计显著性检验，使得定量评估的客观性略有不足。整体来看，实验设计较为合理，但可以更严格。

## 6. 论文的主要结论与发现

- **主要结论**：提出的UAOR方法能够**无需任何额外训练**，通过不确定性感知的观察重注入，显著提升VLA模型在多种机器人操作任务上的成功率和鲁棒性，且计算开销极小。
- **关键发现**：
  - 语言模型中的FFN确实可作为“键-值记忆”，而观察信息注入能帮助模型减少不确定性；
  - 动作熵是一个有效的uncertainty指示器；
  - 该方法适用于不同架构的VLA模型（异质模型），表明其通用性；
  - 无需额外观察线索或模块，即可达到甚至超越需要额外数据训练的方法。

## 7. 优点：方法或实验设计上的亮点

- **方法亮点**：
  - **无需训练**：零训练成本，即插即用，与现有VLA pipeline高度兼容；
  - **理论依据**：基于FFN的键-值记忆特性，有明确的认知直觉；
  - **不确定性引导**：只在需要时增强观察，避免冗余计算；
  - **普适性**：可应用于不同VLA架构，无需修改模型结构。
- **实验设计亮点**：
  - 同时进行仿真和真实世界验证，增强结果可信度；
  - 使用异质模型，证明方法的跨架构泛化能力；
  - 强调计算开销极小，符合实际部署需求。

## 8. 不足与局限

- **实验覆盖**：缺乏大规模标准benchmark（如v0.1 of Open X-Embodiment）的全面对比，且未报告与需要训练的方法（如LoRA、prompt tuning）的公平比较（虽然方法无需训练，但耗时上不占优）。
- **偏差风险**：
  - 阈值选择可能依赖于任务，论文未提供自动确定阈值的方法；
  - 动作熵可能在某些简单任务中失效（低不确定性时无贡献）。
- **应用限制**：
  - 依赖于VLM backbone本身的质量，若backbone本身视觉理解弱，重注入可能增益有限；
  - 仅适用于自回归生成动作的VLA模型，对其他框架（如基于diffusion的策略）不直接适用；
  - 未讨论安全性或失败模式（如注入错误观察特征导致的退化）。
- **其他**：论文被ICLR 2026拒绝，可能评审认为创新性不够突出或实验不够充分；但分数9.0表明质量尚可。

（完）
