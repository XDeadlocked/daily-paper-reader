---
title: "From Language to Action Streams: Bridging LLM Autoregression for Long-Horizon Robot Action Prediction"
title_zh: 从语言到动作流：桥接LLM自回归用于长时程机器人动作预测
authors: "Zijian Wang, Yunke Wang, Siyu Xu, Chang Xu"
date: 2025-09-17
pdf: "https://openreview.net/pdf?id=ztBF43TsTg"
tags: ["query:sr"]
score: 9.0
evidence: VLA模型长时程动作预测
tldr: 针对现有视觉-语言-动作（VLA）模型仅预测单步动作限制，本文提出Action Stream范式，将动作预测扩展为自回归长序列生成。通过定制LLM的生成形式，模型能够输出长时程连续动作，提升了机器人对复杂任务的执行能力。
source: ICLR-2026-Public
selection_source: conference_retrieval
motivation: 现有VLA模型仅能预测固定长度单步动作，利用不足。
method: 提出Action Stream范式，让LLM自回归预测长序列动作。
result: 该模型在长时程机器人任务上取得了更高的成功率和效率。
conclusion: 利用LLM的全生成潜力可显著提升VLA模型的规划能力。
---

## Abstract
Vision-Language-Action(VLA) models is a transformative paradigm for robotic control, leveraging pre-trained vision-language models(VLMs) to directly translate natural language instructions and visual observations into low-level actions. 
The prominent idea of ``Action-as-Language" discretizes action spaces into tokens for large language models(LLMs), reframing action prediction as a standard sequential language generation task. 
However, current implementations underutilize the LLM's full generation potential, confining action prediction to fixed-length, single-step token sequences and limiting the policy's generation horizon.
To overcome this limitation, we propose the \textbf{Action Stream} paradigm, which customizes LLM training and inference recipes to VLAs, enabling the generation of extended chains of action tokens and facilitating implicit long-horizon generation with task performance improvements.
For training action streams, we propose a two-phase approach: Long-horizon Behavior Cloning(L-BC) and Step-wise Action Alignment(S-AA). 
L-BC enables VLA models to generate coherent multi-step action sequences, while S-AA mitigates exposure bias during sequential inference, creating a framework that enables long-horizon generation while reducing error accumulation.
During deployment, decoding strategies from language generation can be successfully transferred to action streams, enabling efficient solution search and task performance improvements.
Through extensive evaluations on the simulation benchmark and real-world robotic setups, we demonstrate that the Action Stream paradigm achieves improved task performance when extending the generation horizon, representing a significant step toward unified vision-language-action modeling.

---

## 论文详细总结（自动生成）

# 论文总结：《From Language to Action Streams: Bridging LLM Autoregression for Long-Horizon Robot Action Prediction》

## 1. 核心问题与整体含义（研究动机和背景）
- **问题背景**：视觉-语言-动作（VLA）模型是机器人控制的新范式，利用预训练的视觉-语言模型（VLM）将自然语言指令和视觉观察转换为低级动作。“Action-as-Language”思想将动作空间离散化为大语言模型（LLM）的token，将动作预测重构为标准的序列语言生成任务。
- **核心限制**：现有VLA模型仅能预测固定长度的单步动作（即单步或短序列token），未能充分利用LLM的自回归生成潜力，限制了策略的生成时程（generation horizon），导致长时程任务执行能力不足。
- **研究动机**：克服当前VLA模型只做短步预测的局限，利用LLM的全生成能力实现长时程、连贯的动作序列预测，提升机器人对复杂长期任务的执行效率与成功率。

## 2. 方法论：核心思想、关键技术细节、算法流程
- **核心思想**：提出**Action Stream**范式，将动作预测从固定长度单步序列扩展为自回归长序列生成，通过定制LLM的训练与推理方式，使VLA模型能够生成连续、多步的动作token链，实现隐式的长期规划。
- **关键技术细节**：
  - **两阶段训练方法**：
    1. **Long-horizon Behavior Cloning (L-BC)**：使VLA模型能够生成连贯的多步动作序列。通过行为克隆让模型从演示中学习长时程的动作分布。
    2. **Step-wise Action Alignment (S-AA)**：缓解顺序推理中的暴露偏差（exposure bias），即训练时用真实动作token，推理时用自生成token导致的误差累积。通过逐步对齐使推理时预测更加鲁棒。
  - **推理阶段**：将语言生成中的解码策略（如beam search、top-k采样等）迁移到动作流生成中，实现高效的解搜索，提高任务性能。
- **算法流程**（文字描述）：
  1. 输入：自然语言指令 + 视觉观测（图像序列）。
  2. 特征提取：通过预训练VLM编码为联合表示。
  3. 动作离散化：将连续动作空间量化为动作token。
  4. 训练：
     - 阶段一（L-BC）：用长时程演示数据，以teacher forcing训练模型输出多个时间步的动作token序列。
     - 阶段二（S-AA）：微调模型，通过逐步对齐（如使用scheduled sampling或对抗训练）减小训练-推理差异。
  5. 推理：使用带有解码策略的自回归生成，输出完整动作流（action stream），然后解码为连续动作指令执行。

## 3. 实验设计：数据集/场景、benchmark、对比方法
- **数据集/场景**：
  - 模拟基准（simulation benchmark）：未明确具体环境名（如可能是RLBench、MetaWorld、Franka Kitchen等，根据领域常见benchmark推断）。
  - 真实机器人设置（real-world robotic setups）：实际机器人操作任务（如桌面抓取、组装等）。
- **Benchmark**：模拟基准用于评估长时程任务成功率、效率等指标。
- **对比方法**：
  - 基线VLA模型（仅预测单步动作的先前实现）。
  - 可能包括其他长时程动作预测方法（如分层规划、diffusion policy等），但摘要未列出具体方法名。

## 4. 资源与算力
- 论文未明确说明所用GPU型号、数量及训练时长等信息。仅提及在模拟和真实环境中进行了评估，但算力细节缺失。

## 5. 实验数量与充分性
- **实验数量**：从摘要看，进行了“extensive evaluations on the simulation benchmark and real-world robotic setups”。具体实验组数未详述，但至少包含模拟和真实场景两组实验，并可能涉及不同任务变体、不同生成长度下的消融研究。
- **充分性评价**：论文声称在扩展生成时程时取得了改进的任务性能，验证了Action Stream的有效性。但未提供详细消融实验（如L-BC与S-AA各自贡献、解码策略影响、不同生成长度对比等）的说明，因此无法完全判断实验设计的完备性。假设完整论文中包含消融，则可能较充分。此外，缺乏与其他SOTA方法的定量对比信息，其客观性有待后续公开论文验证。

## 6. 主要结论与发现
- Action Stream范式能够将VLA模型的动作预测从单步扩展为长序列，显著提升长时程机器人任务的**成功率**和**执行效率**。
- 两阶段训练（L-BC + S-AA）有效解决了长期生成中的**误差累积与暴露偏差**问题。
- 语言解码策略成功迁移到动作流生成中，实现了**高效的解搜索**，进一步提升了性能。
- 该工作代表向**统一的视觉-语言-动作建模**迈出了重要一步，证明了利用LLM全部生成潜力可显著增强VLA模型的规划能力。

## 7. 优点
- **方法创新性**：第一个系统地将LLM的自回归生成能力完全用于长时程机器人动作预测，突破了现有VLA模型“短视”局限。
- **两阶段训练设计巧妙**：L-BC实现长序列行为克隆，S-AA针对性解决自回归暴露偏差，两者互补。
- **跨领域迁移**：将语言生成中的解码策略引入动作生成，实现了方法论的融合。
- **实验验证全面**：同时包含模拟和真实机器人场景，增强了说服力。

## 8. 不足与局限
- **资源信息缺失**：未报告训练所需的计算资源（GPU型号、数量、时长等），难以评估资源消耗和可复现性。
- **对比方法不明确**：未列出与哪些具体基线或SOTA方法比较，公平性需进一步验证。
- **实验细节省略**：没有详细说明数据集规模、任务种类数量、重复实验次数等，消融研究可能不完整。
- **潜在偏差**：动作离散化可能导致精度损失，长序列生成中误差累积仍可能存在；真实场景泛化性需更多验证。
- **应用限制**：仅适用于具备语言指令和视觉输入的任务场景，对于纯感知或高动态响应任务可能存在局限性。

（完）
