---
title: "Vlaser: Vision-Language-Action Model with Synergistic Embodied Reasoning"
title_zh: Vlaser：具备协同具身推理的视觉-语言-动作模型
authors: "Ganlin Yang, Tianyi Zhang, Haoran Hao, Weiyun Wang, Yibin Liu, Dehui Wang, Guanzhou Chen, Zijian Cai, Junting Chen, Weijie Su, Wengang Zhou, Yu Qiao, Jifeng Dai, Jiangmiao Pang, Gen Luo, Wenhai Wang, Yao Mu, Zhi Hou"
date: 2026-01-26
pdf: "https://openreview.net/pdf?id=8xTDnj39Ti"
tags: ["query:sr"]
score: 9.0
evidence: 提出Vlaser，一种具有协同具身推理能力的VLA模型
tldr: 当前VLA模型在高层推理与底层策略学习之间存在鸿沟。本文提出Vlaser模型，通过构建高质量Vlaser-6M数据集，实现协同具身推理，将视觉-语言推理与动作控制无缝融合。实验证明Vlaser在多个机器人任务上达到领先性能，为通用机器人智能提供了新思路。
source: ICLR-2026-Accepted
selection_source: conference_retrieval
motivation: 现有VLA模型缺乏高层推理与底层控制的协同。
method: 构建大规模Vlaser-6M数据集，训练端到端VLA模型实现协同推理。
result: 在多个机器人操控任务上取得当前最优结果。
conclusion: 协同具身推理有效提升了VLA模型的任务完成能力。
---

## Abstract
While significant research has focused on developing embodied reasoning capabilities using Vision-Language Models (VLMs) or integrating advanced VLMs into Vision-Language-Action (VLA) models for end-to-end robot control, few studies directly address the critical gap between upstream VLM-based reasoning and downstream VLA policy learning. In this work, we take an initial step toward bridging embodied reasoning with VLA policy learning by introducing **Vlaser** - a **V**ision-**L**anguage-**A**ction Model with **s**ynergistic **e**mbodied **r**easoning capability, which is a foundational vision-language model designed to integrate high-level reasoning with low-level control for embodied agents. Built upon the high-quality Vlaser-6M dataset, Vlaser achieves state-of-the-art performance across a range of embodied reasoning benchmarks—including spatial reasoning, embodied grounding, embodied QA, and task planning.
Furthermore, we systematically examine how different VLM initializations affect supervised VLA fine-tuning, offering novel insights into mitigating the domain shift between internet-scale pre-training data and embodied-specific policy learning data. Based on these insights, our approach achieves state-of-the-art results on the WidowX benchmark and competitive performance on the Google Robot benchmark. We will open-source the model weights, data generation pipelines, and the full dataset to support future research.

---

## 论文详细总结（自动生成）

# 论文总结：Vlaser：具备协同具身推理的视觉-语言-动作模型

## 1. 核心问题与整体含义（研究动机和背景）
- **核心问题**：当前视觉-语言-动作（VLA）模型在高层推理（如空间推理、任务规划）与底层策略学习（如机械臂精细操作）之间存在显著鸿沟。现有研究要么单独利用视觉-语言模型（VLM）进行推理，要么将VLM集成到VLA模型中进行端到端控制，但很少将两者协同结合。
- **研究动机**：为了弥补上游VLM推理与下游VLA策略学习之间的断裂，实现机器人智能体在高层推理与底层控制上的无缝融合。
- **整体意义**：提出了一个名为**Vlaser**的视觉-语言-动作基础模型，具备协同具身推理能力，为通用机器人智能提供新思路。

## 2. 方法论
- **核心思想**：构建一个端到端的VLA模型，将视觉-语言推理与动作控制整合在统一的框架中，通过协同推理提升任务完成能力。
- **关键技术细节**：
  - **数据集构建**：创建了高质量大规模数据集**Vlaser-6M**，包含6百万条样本，覆盖空间推理、具身定位、具身问答、任务规划等具身推理任务。
  - **模型架构**：基于VLM初始化，通过监督式VLA微调实现端到端学习。模型将视觉输入、语言指令和动作输出统一建模。
  - **领域迁移缓解**：系统研究了不同VLM初始化对VLA微调的影响，提出缓解网络预训练数据与具身策略学习数据之间领域漂移的策略。
- **算法流程**（文字说明）：
  1. 收集包含多种具身任务的标注数据，构建Vlaser-6M数据集。
  2. 选择预训练的VLM作为初始化参数。
  3. 对VLM进行监督微调，同时联合训练推理（如QA、规划）和动作预测（如关节角度、末端位姿）。
  4. 在多个基准上评估协同推理效果。

## 3. 实验设计
- **数据集与场景**：
  - **Vlaser-6M**：用于训练，包含空间推理、具身定位、具身QA、任务规划等数据。
  - **WidowX基准**：桌面操控任务（如抓取、放置）。
  - **Google Robot基准**：真实机器人操控任务。
- **基准任务**：具身推理基准（空间推理、具身QA、任务规划等）。
- **对比方法**：与其他VLA模型（如RT-2、PALM-E、RobotGPT等）及VLM推理方法对比。
- **结果**：在WidowX基准上达到当前最优（SOTA），在Google Robot基准上取得有竞争力的性能；在具身推理基准上也取得SOTA。

## 4. 资源与算力
- **文中未明确说明**：论文元数据及摘要未提及具体的GPU型号、数量或训练时长。无法给出具体算力细节。

## 5. 实验数量与充分性
- **实验组数**：至少包括：
  - 在WidowX和Google Robot两个操控基准上的性能对比。
  - 在多个具身推理基准（空间推理、具身定位、具身QA、任务规划）上的评估。
  - 对不同VLM初始化的消融研究（以分析领域漂移）。
- **充分性与公平性**：
  - 实验覆盖了推理与操控两方面，范围较广。
  - 对比了多个现有方法，结论可信。
  - 消融实验提供了对领域迁移问题的洞见，较为客观。
  - 但未披露实验设置的详细超参数或随机种子，公平性细节待开源后验证。

## 6. 主要结论与发现
- Vlaser通过协同具身推理，有效提升了VLA模型的任务完成能力，在多个基准上取得最优或竞争性结果。
- VLM的初始化选择对VLA微调性能有显著影响，缓解领域漂移是提升模型泛化能力的关键。
- 高质量数据集Vlaser-6M是协同推理的基础。

## 7. 优点
- **方法创新**：首次系统性地将高层推理与底层策略学习协同融合，而非简单拼接。
- **数据贡献**：构建了大规模、多任务的具身推理数据集Vlaser-6M，开源将推动领域发展。
- **实验全面**：既评估推理能力又评估操控能力，且分析了领域迁移问题。
- **性能领先**：在多个基准上达到SOTA，验证了方法的有效性。
- **开源承诺**：将开源模型、数据生成流程和完整数据集，促进可复现性。

## 8. 不足与局限
- **算力细节缺失**：未报告训练所需算力，不利于成本评估和复现。
- **实验场景有限**：仅在桌面（WidowX）和Google Robot两个操控场景测试，未涉及移动操作、多机器人协作等更复杂环境。
- **领域迁移分析深度**：虽然提出缓解策略，但未给出理论上的域适应分析，仅停留在实验观察。
- **潜在偏差**：Vlaser-6M数据集的数据分布可能偏向特定任务和场景，泛化到未见过的推理类型或机器人硬件可能存在风险。
- **未讨论失败案例**：未对模型失败情况进行分析，缺乏对模型鲁棒性的深入评估。

（完）
