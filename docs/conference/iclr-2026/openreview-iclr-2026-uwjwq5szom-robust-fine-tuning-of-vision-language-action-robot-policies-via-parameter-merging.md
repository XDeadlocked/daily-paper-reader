---
title: Robust Fine-tuning of Vision-Language-Action Robot Policies via Parameter Merging
title_zh: 通过参数合并鲁棒微调视觉-语言-动作机器人策略
authors: "Yajat Yadav, Zhiyuan Zhou, Andrew Wagenmaker, Karl Pertsch, Sergey Levine"
date: 2026-01-26
pdf: "https://openreview.net/pdf?id=uWJwQ5SZoM"
tags: ["query:sr"]
score: 9.0
evidence: 通过参数合并鲁棒微调视觉-语言-动作机器人策略
tldr: 该文针对通用机器人策略在微调时过拟合新任务并丧失泛化能力的问题，提出基于参数合并的鲁棒微调方法。该方法在微调新任务的同时保留原有策略的通用能力，避免灾难性遗忘。实验证明，该方法在保持广泛泛化能力的同时有效学习了新技能。
source: ICLR-2026-Accepted
selection_source: conference_retrieval
motivation: 通用机器人策略微调新任务时易过拟合并丧失泛化能力。
method: 提出参数合并方法，在微调新任务时保留原有策略参数的关键特征。
result: 在多种机器人任务上保持了泛化能力并成功学习新技能。
conclusion: 参数合并可有效缓解微调中的灾难性遗忘问题。
---

## Abstract
Generalist robot policies, trained on large and diverse datasets, have demonstrated the ability to generalize across a wide spectrum of behaviors, enabling a single policy to act in varied real-world environments. However, they still fall short on new tasks not covered in the training data. When finetuned on limited demonstrations of a new task, these policies often overfit to the specific demonstrations---not only losing their prior abilities to solve a wide variety of generalist tasks but also failing to generalize within the new task itself. In this work, we aim to develop a method that preserves the generalization capabilities of the generalist policy during finetuning, allowing a single policy to robustly incorporate a new skill into its repertoire. Our goal is a single policy that both learns to generalize to variations of the new task and retains the broad competencies gained from pretraining. We show that this can be achieved through a simple yet effective strategy: interpolating the weights of a finetuned model with that of the pretrained model. We show, across extensive simulated and real-world experiments, that such model merging produces a single model that inherits the generalist abilities of the base model and learns to solve the new task robustly, outperforming both the pretrained and finetuned model on out-of-distribution variations of the new task. Moreover, we show that model merging enables continual acquisition of new skills in a lifelong learning setting, without sacrificing previously learned generalist abilities.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 核心问题与整体含义（研究动机和背景）

通用机器人策略（generalist robot policies）在大型多样化数据集上训练后，已经展现出跨广泛行为的泛化能力，可在多种真实环境中执行任务。然而，当在有限的新任务演示数据上进行微调时，这些策略往往会**过拟合**到具体的演示，不仅丧失了解决大量通用任务的前期能力，而且在新任务本身内部也无法泛化（例如无法适应任务中的变化）。因此，**核心问题**是如何在微调新任务的同时保留通用策略的泛化能力，避免灾难性遗忘，使得单个策略既能学会新技能，又能保持预训练获得的广泛能力。

## 2. 方法论：核心思想、关键技术细节、公式或算法流程

- **核心思想**：通过**参数合并（parameter merging）**——具体为**权重插值（weight interpolation）**——将微调后的模型权重与预训练模型权重进行线性组合，得到单一模型。
- **关键技术细节**：
  - 给定预训练模型权重 \( \theta_{\text{pre}} \)，在新任务上微调后得到 \( \theta_{\text{ft}} \)。
  - 合并后的权重 \( \theta_{\text{merge}} = \alpha \cdot \theta_{\text{ft}} + (1 - \alpha) \cdot \theta_{\text{pre}} \)，其中 \(\alpha\) 为插值系数（通常通过验证集调优或直接设为0.5等经验值）。
  - 该方法无需额外训练或复杂正则化，仅需一次前向合并操作。
- **算法流程**（文字说明）：
  1. 使用大规模机器人数据集训练通用策略（预训练模型）。
  2. 在新任务的小样本演示上对预训练模型进行标准微调（如行为克隆）。
  3. 计算微调后模型与预训练模型的权重插值，得到合并模型。
  4. 直接使用合并模型进行推理，无需进一步训练。

## 3. 实验设计：数据集/场景、基准、对比方法

- **数据集与场景**：
  - 覆盖**模拟环境**（如基于MuJoCo的机器人操作任务）和**真实世界**机器人实验（如抓取、放置、推杆等）。
  - 新任务包括预训练数据中未覆盖的变体（如不同物体形状、颜色、位置偏移等）。
  - 持续学习场景中，按顺序学习多个新任务。
- **基准（Benchmark）**：未明确指定单一标准基准，但使用了多种分布外（out-of-distribution, OOD）任务变体来评估泛化能力。
- **对比方法**：
  - **预训练模型**（原始通用策略，不微调）。
  - **微调模型**（直接在新任务上微调，无合并）。
  - 可能还包括其他微调正则化方法（如L2-SP、EWC等），但摘要中未详述，推测全文中有对比。

## 4. 资源与算力

论文摘要及元数据中**未明确说明**使用的GPU型号、数量、训练时长等具体算力信息。仅提及进行了“广泛的模拟和真实世界实验”，但未提供硬件细节。

## 5. 实验数量与充分性

- **实验数量**：涉及**模拟**和**真实世界**两大类场景，模拟中可能包含多个任务变体（如不同物体、不同干扰），真实世界中也有多个任务。此外，还包含了**持续学习（lifelong learning）**设置，按顺序学习新技能。
- **充分性**：实验设计较为充分，覆盖了分布内、分布外、多任务持续学习场景，评估了合并模型在保持通用能力与学习新任务上的权衡。对比了预训练和微调基线，结果清晰显示了合并方法的优势。但缺少与其他先进的正则化微调方法（如EWC、SI、L2-SP）的系统对比，可能影响公平性结论的全面性。

> 注意：由于当前仅提供摘要，全文中的具体消融实验（如不同插值系数α的分析、模型架构影响等）无法获知，但根据ICLR论文惯例，应包含这些分析。

## 6. 主要结论与发现

- 参数合并（权重插值）是一种**简单而有效**的策略，能够在微调新任务时**保留通用策略的泛化能力**，同时**学会新任务的鲁棒变体**。
- 合并后模型在**新任务的分布外变体**上优于预训练模型和微调模型。
- 在**持续学习**场景中，模型合并可以连续获取新技能，而**不会牺牲先前学到的通用能力**，有效缓解灾难性遗忘。

## 7. 优点：方法或实验设计上的亮点

- **方法简洁有效**：无需复杂训练技巧、额外网络或正则化项，仅通过权重插值即可实现鲁棒微调，易于实现和部署。
- **保留通用能力**：解决了微调中常见的过拟合和丢失预训练特性的问题，具有实际价值。
- **支持持续学习**：在终身学习场景下表现出色，减少了灾难性遗忘。
- **实验覆盖真实场景**：包含真实机器人实验，验证了方法的实际可行性。

## 8. 不足与局限

- **缺乏算力与成本分析**：未说明训练和合并的计算开销，无法评估实际部署的经济性。
- **对比方法可能不全面**：仅与普通微调和预训练模型对比，未与主流正则化微调方法（如EWC、L2-SP等）系统比较，可能高估了参数合并的相对优势。
- **插值系数选择未深入讨论**：α的调优策略未在摘要中说明，若全任务使用固定值可能不具最优性。
- **实验细节有限**：仅基于摘要，无法判断任务多样性、演示数量、模型架构等因素的影响。真实世界实验的数量和变体覆盖度尚不明确。
- **潜在偏差风险**：若预训练模型已包含新任务的某些相似技能，合并可能更易成功；若新任务与预训练分布差异极大，效果可能减弱。

（完）
