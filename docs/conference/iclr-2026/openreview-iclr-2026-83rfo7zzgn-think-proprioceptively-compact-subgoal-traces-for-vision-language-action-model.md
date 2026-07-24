---
title: "Think Proprioceptively: Compact Subgoal Traces for Vision-Language-Action Model"
title_zh: 本体感知思考：面向视觉-语言-动作模型的紧凑子目标轨迹
authors: "Fangyuan Wang, Peng ZHOU, Shipeng Lyu, Gu Gong, Weiwei Lin, Anqing Duan, David Navarro-Alarcon, Guodong Guo"
date: 2025-09-10
pdf: "https://openreview.net/pdf?id=83RFO7Zzgn"
tags: ["query:sr"]
score: 9.0
evidence: 提出利用本体感觉进行推理的VLA模型SubgoalVLA
tldr: 当前VLA模型将本体感觉视为被动输入，导致多模态特征与机器人物理配置脱节。本文提出SubgoalVLA框架，将本体感觉状态作为交叉注意力查询，通过紧凑子目标轨迹实现主动推理。实验表明该方法在多阶段操作任务中显著提升了动作成功率与泛化能力，为VLA模型的本体感知推理提供了新范式。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 现有VLA模型缺乏本体感觉引导，多模态处理脱离机器人物理配置。
method: 利用本体感觉状态作为交叉注意力查询，生成紧凑子目标轨迹。
result: 在多种机器人操作任务上实现更高的成功率和泛化性。
conclusion: 本体感知推理有效提升了VLA模型的物理接地能力。
---

## Abstract
Vision-language-action (VLA) models translate visual observations and language instructions to robot actions, yet current architectures regard proprioception as a passive input rather than an active reasoning component. Without proprioceptive guidance, VLA models process multimodal features in isolation from the robot’s physical configuration, and hierarchical approaches often encode subgoals in high-dimensional visual or textual spaces that are ungrounded in the robot’s embodiment. We present SubgoalVLA, a framework built on the \textit{think proprioceptively} paradigm that redefines how multimodal information is processed. SubgoalVLA leverages proprioception in two ways. First, proprioceptive states serve as cross-attention queries to select vision-language features, enabling configuration-aware feature extraction. Second, subgoals are encoded as compact sequences of joint configurations that eliminate the need for cross-modal translation. Through a two-stage training protocol that begins with supervised learning on ground-truth subgoals and then fine-tunes with self-predicted subgoals, we mitigate distribution shift between training and inference. On the CALVIN benchmark, SubgoalVLA achieves state-of-the-art performance with an average task completion length of 3.32, demonstrating that proprioceptive reasoning provides the critical bridge between high-level task understanding and embodied control.

---

## 论文详细总结（自动生成）

# 论文《Think Proprioceptively: Compact Subgoal Traces for Vision-Language-Action Model》详细总结

## 1. 核心问题与整体含义（研究动机和背景）

- **研究动机**：当前视觉-语言-动作（VLA）模型将本体感觉（proprioception）仅视为被动输入，未将其作为主动推理组件。这导致多模态特征的处理与机器人物理配置脱节，层次化方法常将子目标编码为高维视觉或文本空间，缺乏与机器人具身性的接地。
- **整体含义**：论文提出“本体感知思考”（think proprioceptively）范式，认为本体感觉应作为推理核心，连接高层任务理解与具身控制。通过紧凑子目标轨迹实现配置感知的特征提取，提升VLA模型在多阶段操作任务中的物理接地能力。

## 2. 方法论：核心思想、关键技术细节

- **核心思想**：基于“本体感知思考”范式，重新定义多模态信息的处理方式。本体感觉状态同时用于：
  - 作为交叉注意力查询（cross-attention queries）选择视觉-语言特征，实现配置感知的特征提取。
  - 将子目标编码为紧凑的关节配置序列（compact sequences of joint configurations），消除跨模态翻译需求。
- **关键技术细节**：
  - **SubgoalVLA框架**：包含两个关键设计：本体感受交叉注意力模块和紧凑子目标轨迹生成。
  - **两阶段训练协议**：
    1. 第一阶段：在真实子目标（ground-truth subgoals）上进行监督学习。
    2. 第二阶段：使用自预测子目标（self-predicted subgoals）进行微调，以缓解训练与推理之间的分布偏移。
- **公式/算法流程**（文字描述）：
  - 输入：视觉观测、语言指令、当前本体感觉状态。
  - 第一步：利用本体感觉状态作为查询，对视觉-语言特征进行交叉注意力，输出配置感知的多模态特征。
  - 第二步：将多模态特征解码为紧凑的关节配置子目标序列（子目标轨迹）。
  - 第三步：通过基于子目标轨迹的动作生成模块输出最终机器人控制信号。

## 3. 实验设计

- **数据集/场景**：CALVIN benchmark（机器人操作模拟环境，包含多个操作任务）。
- **Benchmark**：CALVIN，评测指标为平均任务完成长度（average task completion length）。
- **对比方法**：未在提供的文本中列出具体对比方法名称，但摘要指出SubgoalVLA在该benchmark上达到SOTA，平均任务完成长度为3.32。可能对比了基线VLA模型（如RT-2、Octo等）以及不包含本体感知推理的变体。

## 4. 资源与算力

- **未明确说明**：论文提供的文本（摘要和元数据）中未提及使用的GPU型号、数量、训练时长等具体算力信息。需查阅完整论文才能获知。通常此类模拟环境训练可能使用单卡或多卡（如A100），但此处无法确定。

## 5. 实验数量与充分性

- **实验数量**：仅从摘要可知在CALVIN benchmark上进行了评估，报告了平均任务完成长度这一综合指标。未提及消融实验、不同数据集测试、泛化性实验等具体数量。
- **充分性判断**：缺乏消融实验的细节（如去掉本体感知查询、去掉两阶段训练等）和跨领域泛化测试，实验充分性存疑。但CALVIN是VLA领域标准benchmark，单一指标可初步说明有效性。若完整论文包含更多实验，则可能更充分。此处仅基于提供文本，认为实验覆盖较窄，不够全面。

## 6. 主要结论与发现

- **主要结论**：本体感知推理（proprioceptive reasoning）能够有效连接高层任务理解与具身控制，SubgoalVLA在CALVIN benchmark上取得SOTA性能（平均任务完成长度3.32），证明本体感觉作为主动推理组件的有效性。
- **关键发现**：将本体感觉状态作为交叉注意力查询，以及使用紧凑关节配置子目标轨迹，可以避免多模态特征与机器人配置的脱节，减少跨模态翻译的复杂性，并提升泛化能力。

## 7. 优点

- **方法论亮点**：
  - 首次系统性地提出本体感知作为主动推理组件，而非被动输入，填补了VLA模型物理接地不足的空白。
  - 紧凑子目标轨迹（关节配置）设计简洁高效，无需高维视觉或文本子目标表示，降低了计算复杂度。
  - 两阶段训练机制有效缓解分布偏移，提升推理稳定性。
- **实验设计亮点**：在标准benchmark上达到SOTA，结果具有可比性。

## 8. 不足与局限

- **实验覆盖不足**：仅使用CALVIN单一benchmark，未在真实机器人平台或多领域任务（如抓取、移动操作）上验证，泛化性存疑。
- **偏差风险**：未报告消融实验，无法确定各组件（本体感知查询、紧凑子目标、两阶段训练）的单独贡献。
- **应用限制**：需假设机器人关节配置可精确获取，对于欠驱动机器人或部分可观测场景可能不适用。紧凑子目标轨迹可能难以处理非刚体或可变形的物体操作。
- **算力信息缺失**：未提供训练成本，难以评估方法的经济性。

（完）
