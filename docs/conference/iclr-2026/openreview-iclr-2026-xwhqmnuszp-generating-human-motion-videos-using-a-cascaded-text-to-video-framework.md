---
title: Generating Human Motion Videos using a Cascaded Text-to-Video Framework
title_zh: 使用级联文本到视频框架生成人体运动视频
authors: "Hyelin Nam, Hyojun Go, Byeongjun Park, Byung-Hoon Kim, Hyungjin Chung"
date: 2025-09-13
pdf: "https://openreview.net/pdf?id=XwHQMNUSZP"
tags: ["query:sr"]
score: 9.0
evidence: 人类视频生成级联框架
tldr: 针对现有视频扩散模型在通用人体视频生成上的局限，本文提出CAMEO级联框架，将文本到运动模型与条件视频扩散模型巧妙结合。通过优化训练和推理中的文本提示与视觉条件，实现了高质量的人体运动视频生成，拓展了视频生成的应用范围。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 现有视频扩散模型在通用人体视频生成上应用不足。
method: 提出级联框架CAMEO，桥接文本到运动模型和条件视频扩散模型。
result: CAMEO成功生成了多样化的高清人体运动视频。
conclusion: 级联策略有效提升了视频生成模型在人体运动领域的泛化能力。
---

## Abstract
Human video generation is becoming an increasingly important task with broad applications in graphics, entertainment, and embodied AI. 
Despite the rapid progress of video diffusion models (VDMs), their use for general-purpose human video generation remains underexplored, with most works constrained to image-to-video setups or narrow domains like dance videos. 
In this work, we propose CAMEO, a Cascaded framework for general human Motion vidEO generation. It seamlessly bridges Text-to-Motion (T2M) models and conditional VDMs, mitigating suboptimal factors that may arise in this process across both training and inference through carefully designed components. 
Specifically, we analyze and prepare both textual prompts and visual conditions to effectively train the VDM, ensuring robust alignment between motion descriptions, conditioning signals, and the generated videos. 
Furthermore, we introduce a camera-aware conditioning module that connects the two stages, automatically selecting viewpoints aligned with the input text to enhance coherence and reduce manual intervention. 
We demonstrate the effectiveness of our approach on both the MovieGen benchmark and a newly introduced benchmark tailored to the T2M–VDM combination, while highlighting its versatility across diverse use cases.

---

## 论文详细总结（自动生成）

# 使用级联文本到视频框架生成人体运动视频 —— 详细中文总结

## 1. 核心问题与整体含义（研究动机和背景）
- **研究动机**：人体视频生成在图形学、娱乐和具身AI中越来越重要。现有视频扩散模型（VDM）进展迅速，但在**通用人体运动视频生成**任务上探索不足——大多数工作局限于图像到视频的设置或狭窄领域（如舞蹈视频）。
- **整体含义**：现有VDM缺乏对多样化人体运动（文字描述到视频）的泛化能力，需要一种能够桥接文本描述（Text-to-Motion, T2M）与视频生成的方法，以实现自然、高质量、可控的人体运动视频生成。

## 2. 提出的方法论
- **核心思想**：提出**CAMEO**（Cascaded framework for general human Motion vidEO generation）级联框架，巧妙连接**文本到运动模型（T2M）** 和**条件视频扩散模型（条件VDM）**，并通过精心设计的组件缓解两阶段衔接中可能出现的次优因素。
- **关键技术细节**：
  - **训练阶段**：分析与准备文本提示和视觉条件，确保VDM能有效训练，使运动描述、条件信号与生成视频之间实现鲁棒对齐。
  - **推理阶段**：引入**相机感知条件模块**，自动选择与输入文本一致的视角，增强生成视频的连贯性并减少手动干预。
- **流程说明**：输入文本首先由T2M生成人体运动序列（如骨骼/关键点），然后该运动序列作为条件送入VDM，结合相机感知模块生成最终视频。

## 3. 实验设计
- **数据集/场景**：未明确列出具体数据集名称，但提到在**MovieGen基准**和**新引入的专门针对T2M-VDM组合的基准**上进行评估。
- **Benchmark**：MovieGen（通用视频生成基准自建） + 自建T2M-VDM组合基准。
- **对比方法**：未明确列出对比的具体方法名称；仅表示展示了方法的有效性（未做消融等对比分析说明？）。

## 4. 资源与算力
- **文中未明确说明**：未提及使用的GPU型号、数量、训练时长等算力信息。需指出论文未提供相关细节。

## 5. 实验数量与充分性
- **实验数量**：大体两组基准（MovieGen + 自建），未详细说明实验组数（如不同提示、不同视角的对比）。未提及消融实验数量。
- **充分性与公平性**：信息不足。由于未列出详细对比方法、消融设置，难以评估实验是否充分、客观。仅通过摘要描述可推测实验覆盖了通用场景，但缺乏量化结果和公平对比。

## 6. 主要结论与发现
- 级联框架CAMEO成功生成了多样化的高清人体运动视频。
- 级联策略（文本→运动→视频）有效提升了视频生成模型在人体运动领域的泛化能力。
- 相机感知条件模块增强了文本与视频视角的一致性，减少了人工干预。

## 7. 优点（方法/实验设计亮点）
- **方法创新性**：将T2M与VDM级联，克服了传统VDM在人体运动视频生成中应用不足的局限。
- **模块设计**：文本/视觉条件预处理和相机感知模块，提升了跨模态对齐和生成连贯性。
- **适用性**：展示了在多种用例（diverse use cases）下的潜力，具有通用性。

## 8. 不足与局限
- **实验覆盖有限**：未提供与SOTA方法的详细对比、量化指标（FVD、CLIP score等），仅泛泛提到有效。
- **偏差风险**：仅针对人体运动视频，可能不适用于其他类型视频；缺乏对复杂背景、多人运动等场景的测试。
- **应用限制**：依赖T2M模型和VDM的预训练质量，级联误差累积可能影响生成质量；相机视角自动选择可能无法处理用户指定精确视角的需求。
- **算力与可复现性**：未公开训练资源，可复现性存疑。

（完）
