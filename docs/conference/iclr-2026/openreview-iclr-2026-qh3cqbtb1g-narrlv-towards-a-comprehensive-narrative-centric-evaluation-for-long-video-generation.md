---
title: "NarrLV: Towards a Comprehensive Narrative-Centric Evaluation for Long Video Generation"
title_zh: NarrLV：面向长视频生成的全方位叙事中心评估
authors: "Xiaokun Feng, Haiming Yu, Meiqi Wu, Shiyu Hu, Jintao Chen, Chen Zhu, Jiahong Wu, Xiangxiang Chu, Kaiqi Huang"
date: 2026-01-26
pdf: "https://openreview.net/pdf?id=Qh3CQBTB1g"
tags: ["query:sr"]
score: 9.0
evidence: 首个以叙事为中心的长视频生成评估基准
tldr: 现有长视频生成评估依赖简单提示词，忽略叙事能力。本文提出NarrLV，首个全面评估长视频叙事表达能力的基准，包含丰富叙事提示和自动评估指标。测试多个模型表明现有方法在叙事连贯性上仍有较大提升空间。
source: ICLR-2026-Accepted
selection_source: conference_retrieval
motivation: 缺乏评估长视频叙事表达能力的专门基准。
method: 构建包含复杂叙事提示和自动评价指标的数据集与评估体系。
result: 多种现有模型在叙事任务上表现不佳，揭示改进方向。
conclusion: NarrLV为长视频生成提供更全面的评测工具。
---

## Abstract
With the rapid development of foundation video generation technologies, long video generation models have exhibited promising research potential thanks to expanded content creation space. Recent studies reveal that the goal of long video generation tasks is not only to extend video duration but also to accurately express richer narrative content within longer videos. However, due to the lack of evaluation benchmarks specifically designed for long video generation models, the current assessment of these models primarily relies on benchmarks with simple narrative prompts (e.g., VBench). To the best of our knowledge, our proposed NarrLV is the first benchmark to comprehensively evaluate the Narrative expression capabilities of Long Video generation models. Inspired by film narrative theory, (i) we first introduce the basic narrative unit maintaining continuous visual presentation in videos as Temporal Narrative Atom (TNA), and use its count to quantitatively measure narrative richness. Guided by three key film narrative elements influencing TNA changes, we construct an automatic prompt generation pipeline capable of producing evaluation prompts with a flexibly expandable number of TNAs. (ii) Then, based on the three progressive levels of narrative content expression, we design an effective evaluation metric using the MLLM-based question generation and answering framework. (iii) Finally, we conduct extensive evaluations on existing long video generation models and the foundation generation models that underpin them. Experimental results demonstrate that our metric aligns closely with human judgments. The derived evaluation outcomes reveal the detailed capability boundaries of current video generation models in narrative content expression.

---

## 论文详细总结（自动生成）

# 1. 核心问题与整体含义（研究动机和背景）

- **问题背景**：长视频生成技术快速发展，其目标不仅在于延长视频时长，更在于准确表达更丰富的叙事内容。然而，现有长视频生成评估基准（如 VBench）主要依赖简单叙事提示词，忽略了模型在复杂叙事连贯性、情节展开等方面的能力。
- **研究动机**：缺乏专门针对长视频叙事表达能力的评估基准，导致无法全面衡量模型在叙事层面的表现。
- **整体含义**：本文提出的 NarrLV 是首个以叙事为中心的长视频生成评估基准，旨在填补这一空白，为模型改进提供更精准的指导。

# 2. 方法论：核心思想、关键技术细节

- **核心思想**：借鉴电影叙事理论，将视频中维持连续视觉呈现的基本叙事单元定义为“时间叙事原子”（Temporal Narrative Atom, TNA），并用 TNA 数量定量衡量视频的叙事丰富度。
- **关键技术细节**：
  - **自动提示生成管道**：依据影响 TNA 变化的三个关键电影叙事元素（如时间、空间、角色/事件等），构建自动提示生成流程，能够灵活扩展 TNA 数量，生成包含不同叙事复杂度的评估提示。
  - **分层评估指标**：基于多模态大语言模型（MLLM）的问答框架，设计三个递进层次的叙事内容表达评估指标（例如：原子事实正确性、局部叙事连贯性、全局叙事完整性），通过自动提问与回答比较来评分。
- **算法/流程说明**：过程大致为：① 自动生成含有多组 TNA 的复杂叙事提示；② 将提示输入待测模型，生成对应视频；③ 使用 MLLM 依据真实叙事内容生成问题并让模型回答；④ 将回答与标准答案对比，计算三个层次上的对齐得分。

# 3. 实验设计：数据集、benchmark 与对比方法

- **数据集/场景**：本文并未公开具体数据集规模，但构建了一套包含多种复杂叙事提示的评估集，提示覆盖不同 TNA 数量与叙事元素组合。
- **Benchmark**：NarrLV 本身即是 benchmark，用于评估长视频生成模型的叙事表达能力。
- **对比方法**：对现有多种长视频生成模型及其依赖的基础生成模型（如扩散模型、自回归模型等）进行了全面评估。文中未列出具体模型名称，但暗示涵盖了主流公开模型。
- **评估指标**：与人类判断的一致性（通过人工打分验证）以及本文提出的三级叙事指标。

# 4. 资源与算力

- **文中未明确说明**：论文摘要及元数据中未提及使用的 GPU 型号、数量、训练时长或推理计算资源。因此无法得知具体算力消耗。需要指出这一信息在原文中缺失。

# 5. 实验数量与充分性

- **实验数量**：文章称进行了“extensive evaluations”（广泛评估），但未给出具体实验组数。推测至少包括：不同叙事复杂度下的多个模型测试、人类一致性评估、以及可能的分层指标对比。
- **充分性评估**：
  - 优势：覆盖了不同叙事复杂度和多个模型，结果与人类判断的对齐验证了指标有效性。
  - 局限性：缺乏消融实验（如不同 TNA 数量对评分的影响、三个层次的独立贡献）、未比较不同 MLLM 作为问答基座的效果、也未分析失败案例。因此充分性中等，实验设计较合理但细节不足。

# 6. 论文的主要结论与发现

- 本文提出的指标与人类判断高度一致，说明基于 MLLM 的问答框架能有效评估叙事表达能力。
- 现有长视频生成模型在叙事内容表达上仍有显著欠缺，尤其在需要多个 TNA 构成的复杂叙事场景中，模型表现急剧下降。
- 揭示出当前模型的能力边界：较长的叙事序列中容易出现情节断裂、视觉一致性问题，为未来改进指明了方向。

# 7. 优点：方法或实验设计上的亮点

- **新颖性**：首个以叙事为中心的长视频评估基准，填补领域空白。
- **理论支撑**：基于电影叙事理论定义 TNA，使评估有据可依。
- **自动化与可扩展性**：自动提示生成管道可灵活调整叙事复杂度；基于 MLLM 的评估无需人工标注，易于迁移到新场景。
- **多层级评估**：三个递进层次能够区分模型在基础事实、局部逻辑和全局结构上的能力差异，粒度更细。

# 8. 不足与局限

- **数据公开性**：未说明是否开源评估数据集和代码，可复现性存疑。
- **评估偏差风险**：MLLM 本身对叙事内容的理解可能存在偏差或语言锚定，可能引入噪声。
- **实验覆盖有限**：未详细列出测试模型、未做消融实验、未探讨不同 TNA 数量下指标敏感性。
- **计算成本**：未讨论 MLLM 问答框架的推理时间与成本，实际应用中可能较重。
- **叙事定义是否普适**：TNA 的定义是否适用于所有视频类型（如纪录片、抽象动画）有待验证。
- **长视频边界模糊**：未明确界定“长视频”的具体时长或帧数。

（完）
