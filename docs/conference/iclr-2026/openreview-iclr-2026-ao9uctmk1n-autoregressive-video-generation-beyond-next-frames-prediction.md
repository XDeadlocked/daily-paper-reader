---
title: Autoregressive Video Generation beyond Next Frames Prediction
title_zh: 超越下一帧预测的自回归视频生成
authors: "Sucheng Ren, Jiasen Lu, Chen Chen, Zhenbang Wang, Liangchen Song, Xiangxin Zhu, Alan Yuille, Yinfei Yang"
date: 2025-09-11
pdf: "https://openreview.net/pdf?id=ao9uctmk1N"
tags: ["query:sr"]
score: 9.0
evidence: 超越帧预测的自回归视频生成
tldr: 质疑自回归视频生成中帧作为基本预测单元的假设，提出VideoAR框架，支持全帧、关键帧、多尺度精化及时空立方体等多种预测单元。发现使用时空立方体可同时在空间和时间维度进行自回归建模，免除了帧是自然原子单元的预设，提升了生成质量。
source: ICLR-2026-Public
selection_source: conference_retrieval
motivation: 现有自回归视频模型假设帧为原子预测单元，可能非最优。
method: 提出VideoAR框架，支持多种预测单元，优选时空立方体。
result: 使用时空立方体作为预测单元提升了生成效果。
conclusion: 重新定义自回归视频生成的预测单元可带来显著改进。
---

## Abstract
Autoregressive models for video generation typically operate frame-by-frame, extending next-token prediction from language to video's temporal dimension. We question that unlike word as token is universally agreed in language if frame is a appropriate autoregressive unit? To address this, we present VideoAR, a unified framework that supports a spectrum of prediction units including full frames, key-detail frames, multiscale refinements, and spatiotemporal cubes. Among these designs, we find model video generation using \textit{spatiotemporal} cubes as prediction units, which allows autoregressive models to operate across both spatial and temporal dimensions simultaneously. This approach eliminates the assumption that frames are the natural atomic units for video autoregression. We evaluate VideoAR across diverse prediction strategies, finding that cube-based prediction consistently delivers superior quality, speed, and temporal coherence. By removing the frame-by-frame constraint, our video generator surpasses state-of-the-art baselines on VBench while achieving faster inference and enabling seamless scaling to minute-long sequences. We hope this work will motivate rethinking sequence decomposition in video and other spatiotemporal domains.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 论文的核心问题与整体含义

- **研究动机与背景**：现有自回归视频生成模型普遍假设“帧”是基本的预测单元，即逐帧预测，这是从语言模型中单词作为token的类比。但论文质疑这一假设的合理性：帧是否真的是视频自回归建模的最优原子单元？过度依赖帧作为最小单元可能限制了模型的表达能力和生成质量。
- **整体含义**：突破帧级别的预测范式，重新定义视频自回归的预测单元，有望同时提升生成质量、推理速度和长视频连贯性。

## 2. 论文提出的方法论

- **核心思想**：提出 **VideoAR** 框架，不预设帧为唯一预测单元，而是支持多种预测单元，包括全帧、关键‑细节帧、多尺度精化以及时空立方体。
- **关键技术细节**：
  - 重点探索 **时空立方体** 作为预测单元，将视频分解为同时包含空间和时间维度的立方体块。
  - 自回归过程不再逐帧进行，而是在空间和时间两个维度上同时进行自回归建模，消除了“帧是自然原子单元”的假设。
  - 框架统一支持不同的分解策略，通过选择最优预测单元（即时空立方体）来提升性能。
- **算法流程说明**：未提供具体公式，整体思路为：输入视频→按所选预测单元（如时空立方体）分割→对每个单元按特定顺序进行自回归预测→拼接生成完整视频。关键创新在于预测单元的粒度与维度定义。

## 3. 实验设计

- **数据集&场景**：文中未明确列出训练数据集，但评估使用 **VBench** 作为主要 benchmark（一个综合视频生成评估基准）。
- **对比方法**：与“state-of-the-art baselines”进行了比较，具体方法名称未给出，但声称在 VBench 上超越了它们。
- **消融实验**：对比了多种预测单元（全帧、关键‑细节帧、多尺度、时空立方体），验证了时空立方体的优势。文中提到“evaluate across diverse prediction strategies”。

## 4. 资源与算力

- **未明确说明**：文中未提及 GPU 型号、数量、训练时长等算力信息。仅提到“seamless scaling to minute-long sequences”，暗示模型具有良好扩展性，但无具体硬件细节。

## 5. 实验数量与充分性

- **实验数量**：从摘要判断，至少包含不同预测单元的对比实验、与SOTA baseline的对比实验。但具体实验组数未说明。
- **充分性与公平性**：
  - 优势：考虑了多种预测单元，进行了内部消融；使用标准 benchmark VBench 进行外部对比，相对客观。
  - 不足：实验细节（如数据集规模、训练配置、评估指标的具体数值）缺失，无法判断统计显著性或是否进行了多 seed 重复。验证偏倚风险未被讨论。

## 6. 论文的主要结论与发现

- 使用**时空立方体**作为预测单元，可以同时空间和时间自回归，显著提升视频生成质量、推理速度和时间连贯性。
- 证明帧不是自回归视频生成的天然原子单元，重新定义序列分解有助于超越现有方法。
- 在 VBench 上达到最优性能，并且能无缝扩展到分钟级长视频。

## 7. 优点

- **创新性**：质疑并突破了领域内认为帧是原子单元的普遍共识，提出灵活的预测单元选择。
- **统一框架**：VideoAR 能兼容多种分解策略，便于对比研究，具有通用性。
- **性能优越**：同时提升了质量、速度和长视频能力，三项指标均有增益，体现了方法的有效性。

## 8. 不足与局限

- **实验覆盖有限**：仅使用 VBench 一个 benchmark，缺少在更多经典视频数据集（如 UCF-101、Kinetics、Sky Timelapse 等）上的结果，泛化性存疑。
- **细节缺失**：网络架构具体设计、训练超参、预测单元尺寸选择、自回归顺序等关键细节未给出（受限于可获取的元数据长度）。
- **算力与复现**：未提供计算资源信息，复现难度较高；实验充分性难以评估。
- **潜在偏差**：未讨论不同预测单元在不同类型视频（动态/静态、长/短）中的表现差异；对比 baseline 列表及其配置未知，可能存在不公平比较风险。
- **应用限制**：虽然声称可扩展到分钟级，但实际长视频测试的长度、质量衰减情况未量化。

（完）
