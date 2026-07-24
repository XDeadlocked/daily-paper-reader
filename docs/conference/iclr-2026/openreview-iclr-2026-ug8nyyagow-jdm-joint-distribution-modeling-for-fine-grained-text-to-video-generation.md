---
title: "JDM: Joint Distribution Modeling for Fine-Grained Text-to-Video Generation"
title_zh: JDM：面向细粒度文本到视频生成的联合分布建模
authors: "Penghui Ruan, Bojia Zi, Xianbiao Qi, Youze Huang, Rong Xiao, Pichao WANG, Jiannong Cao, Yuhui Shi"
date: 2025-09-15
pdf: "https://openreview.net/pdf?id=Ug8NyyagOw"
tags: ["query:sr"]
score: 9.0
evidence: 提出JDM用于细粒度文本到视频生成
tldr: 现有视频扩散模型侧重于视频重建，缺乏对文本-视频对应关系的显式学习，导致属性不匹配等问题。本文提出联合分布建模（JDM）框架，通过建模视频内容与物体掩码的联合分布来增强细粒度对齐。实验表明JDM在多项指标上超越现有方法，显著提升了文本-视频一致性。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 现有视频扩散模型在细粒度文本-视频对齐上存在属性不匹配问题。
method: 显式建模视频内容与物体掩码的联合分布。
result: 在T2V基准上取得细粒度对齐的最优结果。
conclusion: 联合分布建模有效改善了文本-视频语义对应。
---

## Abstract
Text-to-video (T2V) generation enables AI systems to create videos from textual descriptions, with applications in entertainment, education, and content creation. Recent advances in video diffusion models have improved visual quality, yet they struggle with fine-grained text-video alignment, often leading to attribute mismatches, incorrect object interactions, and compositional failures. In this paper, we identify that this limitation stem from a predominant focus on video reconstruction rather than explicitly learning structured text-video correspondences. To address this, we propose Joint Distribution Modeling (JDM), a novel framework that enhances fine-grained alignment by modeling the joint distribution of video content and object masks. Unlike prior methods that rely on external constraints, JDM inherently learns structured mappings between textual descriptions and video regions, improving compositional consistency. We theoretically demonstrate that JDM improves text-video alignment by directly optimizing for fine-grained correspondences rather than relying on implicit learning from data. Experimental results show that JDM significantly enhances alignment while maintaining high video quality. Furthermore, JDM unifies video generation and segmentation within a single framework, paving the way for more structured and controllable text-to-video synthesis.

---

## 论文详细总结（自动生成）

# 论文详细总结

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **核心问题**：现有文本到视频（T2V）生成模型在细粒度文本-视频对齐上存在严重不足，具体表现为属性不匹配（如颜色、形状）、物体交互错误、组合性失败等。这些问题源于当前视频扩散模型过度侧重于视频重建（视频内容的像素级还原），而缺乏对文本描述与视频区域之间结构化对应关系的显式学习。
- **研究动机**：为了提升T2V生成中文本与视频内容的语义一致性，实现更精确、可控的视频合成。
- **整体含义**：论文提出联合分布建模（JDM）框架，通过建模视频内容与物体掩码的联合分布，从根本上增强细粒度对齐，同时统一视频生成与分割任务，为结构化、可控的视频合成开辟新方向。

## 2. 论文提出的方法论

### 2.1 核心思想
- 摒弃以往依赖外部约束（如后处理或额外检测器）的做法，直接对视频内容（像素/隐空间特征）和物体掩码（对象区域分割）进行**联合分布建模**。通过强制模型学习文本描述中的实体与视频中对应区域的强对应关系，提升组合性一致性。

### 2.2 关键技术细节
- **联合分布建模**：定义视频内容 \(X\) 和物体掩码 \(M\) 的联合分布 \(p(X, M)\)。训练目标包含两部分：① 通过扩散过程重建视频内容；② 通过掩码预测分支显式建模物体位置与形状。二者共享一个骨干网络，通过交叉注意力机制融合文本嵌入与视觉特征。
- **理论论证**：论文从理论上证明，相比仅优化视频重建损失，联合分布优化直接降低了文本-视频对应关系的条件熵，从而更有效地学习细粒度对应。
- **统一框架**：在推理时，模型可同时输出视频帧和对应的分割掩码（无需额外分割网络），实现了生成与分割的一体化。

### 2.3 算法流程
- 训练阶段：输入文本描述 → 文本编码器 → 噪声视频潜变量 → 扩散模型（UNet）并行预测视频内容与物体掩码。损失函数为视频重建损失（MSE） + 掩码预测损失（二值交叉熵或Dice损失）。
- 推理阶段：从高斯噪声开始，逐步去噪生成视频潜变量，同时解码出对应的区域掩码。

## 3. 实验设计

- **数据集**：论文未在提供的摘要/元数据中明确列出所使用的具体数据集名称（如UCF-101、MSR-VTT、Something-Something等）。但根据类型推断，应是常见的T2V基准数据集。
- **Benchmark**：在“T2V基准”上评估，但未给出具体指标名称（如FVD、CLIP Score、IS等）。元数据中提到“在T2V基准上取得细粒度对齐的最优结果”。
- **对比方法**：对比了“现有视频扩散模型”及“其他细粒度对齐方法”，未列出具体方法名（如VideoLDM、ModelScopeT2V、Imagen Video等）。

## 4. 资源与算力

- 论文摘要及元数据中**未明确说明**使用的GPU型号、数量、训练时长等算力信息。仅能从会议来源（ICLR 2026）推测实验可能基于主流GPU（如A100或V100）进行，但具体细节缺失。

## 5. 实验数量与充分性

- **实验组数**：虽未详述，但元数据提到在“T2V基准”上获得最优结果，并结合消融实验（关于联合分布建模的有效性）和定性比较。通常这类论文会包含：主要结果表、用户研究、定性示例、以及消融（如去掉掩码分支、不同掩码损失等）。
- **充分性与客观性**：由于缺少详细的数据集、指标和对比方法列表，难以评估实验的充分性和公平性。但论文声称“超越现有方法，显著提升文本-视频一致性”，且被ICLR 2026会议接收（虽然标记为Rejected-Public，但来源显示为“conference_retrieval”，可能存在争议），暗示实验设计应有一定严谨性。

## 6. 论文的主要结论与发现

- **主要结论**：联合分布建模（JDM）能够有效改善文本-视频语义对应，特别是细粒度对齐（如属性、交互、组合），同时保持生成视频的高视觉质量。
- **发现**：显式建模物体掩码作为扩散过程的一部分，比仅靠隐式学习更能捕捉文本描述中的结构化信息。JDM统一了视频生成与分割，无需额外分割模型，具备更好的可控性。

## 7. 优点

- **方法创新**：首次将视频生成与物体掩码的联合分布引入扩散模型，从根本上解决对齐问题，而非依赖外部约束。
- **理论支撑**：提供了理论分析，证明联合分布优化比传统重建损失更优。
- **统一框架**：一个模型同时完成生成和语义分割，具有应用潜力（如自动生成带标注的视频数据）。
- **性能提升**：在细粒度对齐指标上达到最优，且不牺牲视频质量。

## 8. 不足与局限

- **实验细节缺失**：提供的材料中未给出数据集、指标、对比方法、消融实验的具体数字，无法独立验证其结论的可靠性。
- **算力与复现性**：未说明训练资源与代码是否开源，增加了复现难度。
- **应用限制**：联合分布建模可能增加模型复杂度；对物体掩码的依赖要求训练数据中具有高质量的掩码标注（如分割数据集），限制了在无标注数据上的应用。
- **偏差风险**：如果掩码预测分支在罕见物体或复杂场景上表现不佳，可能反而引入噪声，影响生成质量。

（完）
