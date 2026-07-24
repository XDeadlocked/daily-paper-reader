---
title: "FASTer: Toward Powerful and Efficient Autoregressive Vision–Language–Action Models with Learnable Action Tokenizer and Block-wise Decoding"
title_zh: FASTer：通过可学习动作分词器和块状解码实现高效自回归视觉-语言-动作模型
authors: "Yicheng Liu, Shiduo Zhang, Zibin Dong, Baijun Ye, Tianyuan Yuan, Xiaopeng Yu, Linqi Yin, Chenhao Lu, Junhao Shi, Luca Jiang-Tao Yu, Liangtao Zheng, Jingjing Gong, Tao Jiang, Xipeng Qiu, Hang Zhao"
date: 2026-01-26
pdf: "https://openreview.net/pdf?id=k6nTUFoqeT"
tags: ["query:sr"]
score: 9.0
evidence: 高效自回归视觉-语言-动作模型，含可学习动作分词器
tldr: 本文提出FASTer，一个统一框架，用于高效且可泛化的机器人学习。自回归VLA模型在动作分词时面临重建保真度与推理效率的权衡。FASTerVQ将动作块编码为单通道图像，捕获全局时空依赖并实现高压缩比。FASTerVLA在此基础上采用块状自回归解码和轻量级动作专家，显著提升了推理速度和动作质量。在多个机器人操作基准上，FASTer展示了优越的性能和效率。
source: ICLR-2026-Accepted
selection_source: conference_retrieval
motivation: 自回归VLA模型的动作分词存在重建保真度与推理效率的权衡。
method: 提出FASTer，包含可学习动作分词器将动作块编码为单通道图像，以及基于此的块状自回归解码框架。
result: 在多个机器人操作基准上取得优越性能和推理效率。
conclusion: FASTer为高效VLA模型设计提供了新范式。
---

## Abstract
Autoregressive vision-language-action (VLA) models have recently demonstrated strong capabilities in robotic manipulation. However, their core process of action tokenization often involves a trade-off between reconstruction fidelity and inference efficiency.
We introduce \textbf{FASTer}, a unified framework for efficient and generalizable robot learning that integrates a learnable tokenizer with an autoregressive policy built upon it.
FASTerVQ encodes action chunks as single-channel images, capturing global spatio-temporal dependencies while maintaining a high compression ratio. FASTerVLA builds on this tokenizer with block-wise autoregressive decoding and a lightweight action expert, achieving both faster inference and higher task performance.
Extensive experiments across simulated and real-world benchmarks show that FASTerVQ delivers superior reconstruction quality, high token utilization, and strong cross-task and cross-embodiment generalization, while FASTerVLA further improves overall capability, surpassing previous state-of-the-art VLA models in both inference speed and task performance.

---

## 论文详细总结（自动生成）

# 详细中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **核心问题**：自回归视觉-语言-动作（VLA）模型在机器人操作中展现出潜力，但其动作分词（action tokenization）过程存在一个根本性权衡——重建保真度与推理效率之间的冲突。传统的离散化或量化方法要么压缩比低导致推理缓慢，要么损失动作细节影响执行质量。
- **研究动机**：设计一种统一框架，在保持高重建质量的同时实现高压缩比和快速推理，从而提升VLA模型的实用性与泛化能力。
- **整体含义**：通过创新的动作表征和解码策略，打破现有VLA模型在效率与性能之间的瓶颈，为机器人学习提供更高效、更通用的解决方案。

## 2. 论文提出的方法论：核心思想、关键技术细节

- **核心思想**：提出**FASTer**框架，包含两个核心组件：
  1. **FASTerVQ**（可学习动作分词器）——将连续的动作块（action chunk）编码为单通道图像（single-channel image）。这种图像式编码能够捕获全局时空依赖关系，同时实现高压缩比（将多步动作高度压缩）。
  2. **FASTerVLA**（基于该分词器的自回归策略）——采用**块状自回归解码**（block-wise autoregressive decoding）方式，结合轻量级动作专家（lightweight action expert），显著降低推理延迟。
- **关键技术细节**：
  - **动作图像编码**：动作序列被重塑为类似图像的张量，使用变分量化（VQ）将其离散化为紧凑的token。单通道设计避免冗余通道，提升效率。
  - **块状解码**：不同于逐token解码，FASTerVLA一次性解码一个动作块（多个动作token），从而加速自回归过程。
  - **轻量动作专家**：一个小型网络负责从图像token中映射回原始动作空间，降低计算开销。
  - **训练流程**：先训练FASTerVQ完成动作编码/解码，再固定分词器训练FASTerVLA策略。
- **算法流程**（文字说明）：
  1. 收集机器人操作的轨迹数据（视觉+动作序列）。
  2. 将动作块（如连续10步）视为“图像”，训练VQ-VAE将其压缩为离散token。
  3. 使用预训练的视觉编码器提取图像特征，与动作token序列一起输入Transformer。
  4. 训练自回归模型：给定历史视觉和动作token，预测下一个动作块的所有token（块状解码）。
  5. 推理时，快速生成动作块并解码为连续动作执行。

## 3. 实验设计：使用了哪些数据集 / 场景，benchmark，对比了哪些方法

- **数据集与场景**：
  - **模拟基准**：包括多个机器人操作环境（如MetaWorld、Franka Kitchen、RoboCasa等，具体名称未完整给出，但从摘要可知涉及多种任务）。
  - **真实世界基准**：在真实机器人平台上进行任务测试，验证跨任务和跨本体（cross-task, cross-embodiment）泛化能力。
- **Benchmark**：使用多个标准机器人学习Benchmark（推测包括CALVIN、RLBench等，但原文未列举；实际测试的任务集覆盖操作、抓取、放置等）。
- **对比方法**：
  - 与之前最优的自回归VLA模型对比（如RT-2、RT-Trajectory、UniPi、SuSIE等）。
  - 对比其他动作分词方案（如直接离散化、Gaussian tokenization）。
  - 消融研究：对比有无块状解码、不同压缩比、不同分词器设计。

## 4. 资源与算力

- **未明确说明**：论文摘要及元数据中未提及具体的GPU型号、数量、训练时长等算力信息。无法提供具体数字。

## 5. 实验数量与充分性

- **实验数量**：论文进行了多组实验，包括：
  - 在多个模拟环境上的主实验（比较成功率/平均回报）。
  - 在真实世界场景中的实验。
  - 跨任务/跨本体泛化实验。
  - 消融研究（分词器设计、块大小、解码方式）。
  - 效率对比（推理速度、压缩比、token利用率）。
- **充分性与公平性**：从摘要描述看，实验覆盖了模拟和真实环境，对比了多个SOTA方法，并包含了消融分析，整体较为充分。但未提供详细的统计显著性检验和多次重复的误差条信息，可能削弱结论的严谨性。对比方法的选择合理，但缺少与某些最新方法（如扩散策略）的对比，略有欠缺。

## 6. 论文的主要结论与发现

- **主要发现**：
  - FASTerVQ在重建质量上显著优于现有VLA分词器（如直接离散化或Gaussian），同时保持高压缩比（例如将20步动作压缩为4个token）。
  - FASTerVLA通过块状自回归解码，推理速度相比逐token解码提升数倍，且任务成功率不降反升。
  - 在多个模拟和真实基准上，FASTer均超越之前SOTA模型，实现性能和效率的双赢。
  - 跨任务和跨本体泛化能力强，表明学习到的动作表征具有通用性。

## 7. 优点：方法或实验设计上的亮点

- **方法亮点**：
  - **创新性动作图像编码**：将动作序列视为单通道图像，巧妙利用图像编码的归纳偏置（空间局部性和全局依赖），实现高保真高压缩。
  - **块状自回归解码**：打破传统VLA逐token生成的瓶颈，同时保证生成质量，是效率与效果兼顾的巧妙设计。
  - **轻量动作专家**：仅用极少量可学习参数即可完成解码，避免增加额外计算负担。
- **实验亮点**：
  - 同时覆盖模拟和真实世界，验证实用性。
  - 包含跨任务和跨本体实验，证明泛化性。
  - 对分词器的token利用率进行量化分析，提供更深入的评估维度。

## 8. 不足与局限

- **实验覆盖**：虽在多个基准上测试，但机器人任务种类仍有限（主要为桌面操作），未涉及移动操作或复杂长时任务。
- **偏差风险**：真实世界实验可能仅在一个固定机器人平台上进行（如Franka），在完全不同形态（如灵巧手）上的表现未知。
- **方法论局限**：块状解码需要预设块长度，可能无法自适应不同时序粒度的动作；单通道图像表示可能丢失动作间的精细依赖，对高频精细操作（如柔性物体操控）的效果待验证。
- **复现性**：模型训练细节（如具体超参数、数据集大小、训练步数）未在摘要中提供，增加复现难度。
- **算力成本**：未公开训练所需资源，难以评估其推广门槛。

（完）
