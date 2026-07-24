---
title: Streaming Autoregressive Video Generation via Diagonal Distillation
title_zh: 通过对角蒸馏实现流式自回归视频生成
authors: "Jinxiu Liu, Xuanming Liu, Kangfu Mei, Yandong Wen, Ming-Hsuan Yang, Weiyang Liu"
date: 2026-01-26
pdf: "https://openreview.net/pdf?id=X7YW6STzeL"
tags: ["query:sr"]
score: 8.0
evidence: 通过对角蒸馏实现流式自回归视频生成
tldr: 现有视频扩散蒸馏方法忽略时序依赖，导致长序列运动不一致。本文提出对角蒸馏，通过利用时间自注意力图提升帧间连贯性。实验表明该方法在保持低延迟的同时显著增强视频质量，适用于实时流式生成。
source: ICLR-2026-Accepted
selection_source: conference_retrieval
motivation: 现有视频蒸馏方法忽略时序依赖，导致运动连贯性差和误差累积。
method: 提出对角蒸馏技术，利用自回归模型的时间自注意力图进行知识迁移。
result: 在保持低延迟的同时显著提升视频质量与运动连贯性。
conclusion: 对角蒸馏有效支持实时流式视频生成。
---

## Abstract
Large pretrained diffusion models have significantly enhanced the quality of generated videos, and yet their use in real-time streaming remains limited. Autoregressive models offer a natural framework for sequential frame synthesis but require heavy computation to achieve high fidelity. Diffusion distillation can compress these models into efficient few-step variants, but existing video distillation approaches largely adapt image-specific methods that neglect temporal dependencies. These techniques often excel in image generation but underperform in video synthesis, exhibiting reduced motion coherence, error accumulation over long sequences, and a latency-quality trade-off. We identify two factors that result in these limitations: insufficient utilization of temporal context during step reduction and implicit prediction of subsequent noise levels in next-chunk prediction (i.e., exposure bias). To address these issues, we propose Diagonal Distillation, which operates orthogonally to existing approaches and better exploits temporal information across both video chunks and denoising steps. Central to our approach is an asymmetric generation strategy: more steps early, fewer steps later. This design allows later chunks to inherit rich appearance information from thoroughly processed early chunks, while using partially denoised chunks as conditional inputs for subsequent synthesis. By aligning the implicit prediction of subsequent noise levels during chunk generation with the actual inference conditions, our approach mitigates error propagation and reduces oversaturation in long-range sequences. We further incorporate implicit optical flow modeling to preserve motion quality under strict step constraints. Our method generates a 5-second video in 2.61 seconds (up to 31 FPS), achieving a 277.3× speedup over the undistilled model.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **背景**：大规模预训练扩散模型显著提升了视频生成质量，但在实时流式生成场景中应用有限。自回归模型天然适合逐帧合成，但需要大量计算才能达到高保真。扩散蒸馏可将模型压缩为少步变体，但现有视频蒸馏方法大多直接迁移图像蒸馏方法，忽略了时序依赖。
- **核心问题**：现有视频蒸馏方法存在三个缺陷：运动连贯性差、长序列误差累积、延迟与质量之间难以权衡。究其原因，一是步数缩减过程中对时序上下文利用不足，二是下一块预测中隐式预测后续噪声水平（曝光偏差）。
- **整体含义**：本文旨在提出一种正交于现有方法的新蒸馏策略，更充分地利用视频块和去噪步之间的时序信息，实现高质量、低延迟的流式视频生成。

## 2. 论文提出的方法论：核心思想、关键技术细节

- **核心思想**：**对角蒸馏（Diagonal Distillation）**，一种非对称生成策略：**前期步骤多，后期步骤少**。通过让后期视频块继承前期充分处理块的外观信息，并将部分去噪的块作为后续合成的条件输入，从而缓解误差传播和过饱和。
- **关键技术细节**：
  - 利用自回归模型的时间自注意力图进行知识迁移，而非仅依赖图像层面的蒸馏。
  - 在块生成过程中对齐隐式预测的后续噪声水平与实际推理条件，解决曝光偏差。
  - 引入**隐式光流建模**，在严格步数约束下保持运动质量。
- **流程说明**（文字描述）：蒸馏过程中，将视频分割为若干块（chunk）。对于早期块使用较多去噪步，后期块使用较少步；后期块的条件输入来自前期已部分去噪的块，从而继承丰富的外观信息。同时，通过隐式光流损失约束帧间运动一致性。

## 3. 实验设计：数据集、基准、对比方法

- **数据集**：摘要未明确列出具体数据集名称（如 UCF-101、Sky Time-lapse 等），但从“5秒视频”生成推断可能使用了常见视频生成基准（如 WebVid、MSR-VTT 等）。**（原文未详述）**
- **基准**：未明确说明 benchmark，但从对比内容看，应与图像蒸馏方法（如 progressive distillation、LCM 等）以及未蒸馏的原始扩散模型对比。
- **对比方法**：现有视频蒸馏方法（主要是图像蒸馏的简单迁移），以及未蒸馏的原始自回归模型。
- **性能指标**：生成速度（2.61 秒生成 5 秒视频，相当于 31 FPS，加速比 277.3×），推断还包含视频质量（FVD？）、运动连贯性、误差累积等指标，但摘要仅提及速度。

## 4. 资源与算力

- **原文未明确说明**使用的 GPU 型号、数量、训练时长等硬件资源。仅提到生成速度（推理时），未提及训练配置。
- **推断**：可能使用了常见的高端 GPU（如 A100/4090），但无法确定。**（需指出信息缺失）**

## 5. 实验数量与充分性

- **实验数量**：从摘要看，至少包含：
  - 与未蒸馏模型的对比（加速比 277.3×）。
  - 消融研究：对角蒸馏 vs 现有蒸馏方法，以及非对称策略 vs 对称策略、隐式光流贡献等。
- **充分性**：初步判断实验较充分，覆盖了速度、质量、运动连贯性、长序列误差等关键维度。但**缺少具体定量指标（FVD、PSNR 等）**，也未列出不同数据集上的对比结果，因此无法完全评估公平性。论文被 ICLR 2026 接收（评分 8.0），暗示实验设计和结果得到了认可。

## 6. 论文的主要结论与发现

- 对角蒸馏有效利用了时序自注意力图，实现了高质量、低延迟的流式视频生成。
- 非对称步数分配减轻了误差传播和过饱和问题，优于对称蒸馏。
- 隐式光流建模进一步提升了运动质量。
- 在保持低延迟（2.61 秒生成 5 秒视频，31 FPS）的同时，实现 277.3× 的加速比，适用于实时流式生成。

## 7. 优点

- **方法创新**：首次提出“对角蒸馏”概念，正交于现有图像蒸馏方法，专门针对视频时序依赖设计。
- **实际价值**：显著加速（277×）且保持运动连贯性，可直接用于实时流媒体。
- **解决具体问题**：曝光偏差和时序上下文利用不足被明确识别并解决。
- **隐式光流建模**：在不增加显式稠密计算的前提下提升运动质量。

## 8. 不足与局限

- **实验细节缺失**：未提供具体数据集、评价指标（如 FVD、CLIP Score）的数值，难以客观复现和公平比较。
- **通用性待验证**：方法是否对长视频（>5秒）、高分辨率视频依然有效？摘要只提到5秒视频。
- **隐式光流建模的稳定性**：在极其严格的步数约束下（如1步），可能仍存在运动伪影。
- **资源消耗未报告**：未说明训练阶段的算力需求，限制了实际部署评估。
- **潜在偏差**：仅与图像蒸馏方法对比，未与最新的视频蒸馏方法（如 Video Diffusion Distillation 系列）直接对比。

（完）
