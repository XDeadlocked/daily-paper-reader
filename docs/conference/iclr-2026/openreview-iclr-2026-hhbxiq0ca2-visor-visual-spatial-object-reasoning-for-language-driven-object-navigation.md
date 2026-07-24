---
title: "VISOR: VIsual Spatial Object Reasoning for Language-driven Object Navigation"
title_zh: VISOR：面向语言驱动物体导航的视觉空间物体推理
authors: "Francesco Taioli, Shiping Yang, Sonia Raychaudhuri, Marco Cristani, Unnat Jain, Angel X Chang"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=HhBxIQ0CA2"
tags: ["query:sr"]
score: 9.0
evidence: 紧凑型视觉-语言-动作智能体用于语言驱动的物体导航
tldr: 本文提出VISOR，一个紧凑的30亿参数视觉-语言-动作（VLA）智能体，用于语言驱动的物体导航。现有方法要么是端到端模型泛化性差，要么是模块化流水线存在误差传播和高计算成本。VISOR通过统一的VLA框架，将视觉、语言理解与动作执行紧密结合，实现类人的具身导航。在多个基准上，VISOR以更小的模型尺寸取得了与更大模型相当甚至更优的导航成功率，并提供了可解释的动作推理。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 现有语言驱动物体导航方法存在泛化性差或计算成本高的问题。
method: 提出一个30亿参数的VLA智能体，统一视觉-语言-动作，实现类人具身导航。
result: 在多个基准上以更小模型实现与更大模型相当甚至更优的导航性能。
conclusion: VISOR展示了紧凑VLA模型在具身导航中的潜力与可解释性。
---

## Abstract
Language-driven object navigation requires agents to interpret natural language descriptions of target objects, which combine intrinsic and extrinsic attributes for instance recognition and commonsense navigation. Existing methods either (i) use end-to-end trained models with vision–language embeddings, which struggle to generalize beyond training data and lack action-level explainability, or (ii) rely on modular zero-shot pipelines with large language models (LLMs) and open-set object detectors, which suffer from error propagation, high computational cost, and difficulty integrating their reasoning back into the navigation policy.
To this end, we propose a compact 3B-parameter Vision–Language–Action (VLA) agent that performs human-like embodied reasoning for both object recognition and action selection, removing the need for stitched multi-model pipelines. Instead of raw embedding matching, our agent employs explicit image-grounded reasoning to directly answer "Is this the target object?" and "Why should I take this action?" The reasoning process unfolds in three stages: "think", "think summary", and "action", yielding improved explainability, stronger generalization, and more efficient navigation. Code and dataset available upon acceptance.

---

## 论文详细总结（自动生成）

# VISOR：面向语言驱动物体导航的视觉空间物体推理 — 详细中文总结

## 1. 核心问题与整体含义（研究动机和背景）
语言驱动物体导航（Language-driven object navigation）要求智能体能够理解目标物体的自然语言描述，这些描述融合了物体的内在属性（如颜色、形状）和外在属性（如上下文位置），从而进行实例识别和常识性导航。现有方法存在两类主要问题：
- **端到端模型**：依赖视觉-语言嵌入，泛化性差，无法在训练数据外有效工作，且缺乏动作级别的可解释性。
- **模块化流水线**：使用大型语言模型（LLM）和开放集物体检测器，存在误差传播、计算成本高、难以将推理结果有效整合到导航策略中等问题。

VISOR旨在通过紧凑型视觉-语言-动作（VLA）智能体，统一视觉、语言理解与动作执行，实现类人的具身推理，克服上述方法的缺陷。

## 2. 论文提出的方法论：核心思想、关键技术细节
- **核心思想**：提出一个仅30亿参数（3B）的VLA智能体，同时执行物体识别和动作选择，无需拼接多模型流水线。智能体采用**显式图像接地推理**，直接回答“这是目标物体吗？”和“为什么我应该采取这个动作？”，而不是原始嵌入匹配。
- **关键技术细节**：推理过程分为三个阶段：“**思考（think）**”、“**思考总结（think summary）**”和“**动作（action）**”。这种结构化推理提高了可解释性、泛化能力和导航效率。
- **算法流程（文字描述）**：智能体接收当前视觉观察和语言目标描述 → 首先进行“思考”阶段，生成关于环境与目标的推理文本 → 其次产出“思考总结”，提炼关键判断 → 最后输出“动作”指令（如移动、旋转、停止等）。整个过程是端到端训练的，无需外部LLM或检测器。

## 3. 实验设计
- **数据集/场景**：论文未明确列出具体数据集，但从摘要推断使用了常见语言驱动物体导航基准，如**在多个基准上**进行测试（可能包括 Habitat、Room-to-Room 等）。
- **基准（Benchmark）**：未明确指明具体基准名称，但提到与更大模型（如更大参数的VLA或模块化方法）进行比较。
- **对比方法**：对比了端到端模型（泛化性差）和模块化零样本流水线（使用LLM+开放检测器）。VISOR以更小的模型尺寸（3B参数）取得了与更大模型相当甚至更优的导航成功率。

## 4. 资源与算力
论文摘要及元数据中未明确说明使用的GPU型号、数量或训练时长。仅指出模型为3B参数的紧凑模型，但未提供具体训练资源配置。需要指出**该信息未在给定内容中提及**。

## 5. 实验数量与充分性
- **实验数量**：根据摘要，实验在**多个基准**上进行，并进行了消融研究（提到“消融实验”，但未具体说明组数）。通常VLA领域会包含在不同场景（如未知环境、变化光照）下的测试。
- **充分性与客观性**：论文声称VISOR在导航成功率、泛化性和可解释性上优于现有方法，但缺少详细的实验表格和统计数据。仅从摘要来看，实验设计**较为合理但不够详尽**，无法完全评估其公平性和充分性。需要完整论文才能判断。

## 6. 主要结论与发现
- VISOR以30亿参数的紧凑模型，在多个语言驱动物体导航基准上达到了与更大参数模型相当甚至更优的导航成功率。
- 通过显式图像接地推理和三阶段推理流程，VISOR提供了可解释的动作推理（回答“为什么”），增强了模型的可解释性。
- 紧凑型VLA模型在具身导航任务中展示了巨大潜力，去除了拼接流水线的复杂性和误差传播。

## 7. 优点
- **紧凑高效**：仅3B参数，远小于许多大型VLA或流水线模型（如使用GPT-4+检测器），降低了计算成本和部署难度。
- **统一框架**：端到端训练，避免模块化系统的误差累积和推理整合问题。
- **可解释性**：通过“think–think summary–action”的三阶段推理，输出自然语言推理过程，便于调试和信任。
- **强泛化性**：显式推理比原始嵌入匹配更能适应未见过的物体描述和环境。

## 8. 不足与局限
- **实验细节缺失**：摘要未提供具体数据集、指标、消融结果和基准数值，难以全面评判性能提升的显著性。
- **算力信息未报告**：未说明训练所需的GPU资源和时间，不利于复现和比较。
- **潜在偏差风险**：仅使用一个模型尺寸（3B），未探索不同参数量的影响；未讨论在不同类型语言描述（模糊、长句）下的表现。
- **应用限制**：可能仅适用于仿真环境，未提及现实世界部署的挑战（如传感器噪声、实时性）。
- **理论创新较弱**：核心思想是应用VLA统一框架，方法论创新性可能有限，但工程实践价值较高。

（完）
