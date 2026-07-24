---
title: "STANCE: Motion Coherent Video Generation Via Sparse-To-dense Anchored Encoding"
title_zh: STANCE：通过稀疏到密集锚定编码实现运动连贯视频生成
authors: "ZhiFei Chen, Tianshuo Xu, Leyi Wu, Luozhou Wang, Dongyu Yan, Zihan You, Wenting Luo, Guo Zhang, Ying-Cong Chen"
date: 2025-09-08
pdf: "https://openreview.net/pdf?id=FwtKMYHov7"
tags: ["query:sr"]
score: 9.0
evidence: 通过稀疏到密集编码实现运动连贯视频生成
tldr: 本文提出STANCE视频生成框架，解决运动连贯性问题。引入实例线索将稀疏用户编辑转化为密集2.5D运动场，并解耦外观与运动优化，避免纹理优先于时间一致性。在图像到视频任务中，STANCE生成了运动更连贯、交互更自然的视频，且无需额外训练。
source: ICLR-2026-Public
selection_source: conference_retrieval
motivation: 视频生成中对象运动不连贯，用户提供的稀疏提示编码后有效token太少，且外观与运动共享优化头导致纹理优先。
method: 提出实例线索将稀疏提示转化为密集2.5D运动场，并解耦外观与运动优化。
result: 在多种视频生成场景中，STANCE生成的运动更连贯，时序一致性更强。
conclusion: STANCE以简洁组件有效提升了视频运动生成质量。
---

## Abstract
Video generation has recently made striking visual progress, but maintaining coherent object motion and interactions remains difficult. We trace two practical bottlenecks: (i) human-provided motion hints (e.g., small 2D maps) often collapse to too few effective tokens after encoding, weakening guidance; and (ii) optimizing for appearance and motion in a single head can favor texture over temporal consistency. We present STANCE, an image-to-video framework that addresses both issues with two simple components.
First, we introduce Instance Cues—a pixel-aligned control signal that turns sparse, user-editable hints into a dense 2.5D (camera-relative) motion field by averaging per-instance flow and augmenting with monocular depth over the instance mask. This reduces depth ambiguity compared to 2D drag/arrow inputs while remaining easy to user. Second, we preserve the salience of these cues in token space with Dense RoPE, which tags a small set of motion tokens (anchored on the first frame) with time-addressable rotary embeddings. Paired with joint RGB + auxiliary-map prediction (segmentation or depth), our model anchors structure while RGB handles appearance, stabilizing optimization and improving temporal coherence without requiring per-frame trajectory scripts.

---

## 论文详细总结（自动生成）

# 论文详细总结

## 1. 核心问题与整体含义（研究动机和背景）
- **问题**：视频生成在视觉质量上取得了显著进步，但保持对象运动的连贯性和交互的自然性仍然困难。
- **瓶颈**：
  - 用户提供的运动线索（如小尺寸2D图）经过编码后有效token太少，导致引导信号弱。
  - 外观和运动共享一个优化头时，模型倾向于优先优化纹理，忽略时间一致性。
- **目标**：提出一种图像到视频（I2V）框架，提升运动连贯性，同时保持用户编辑的易用性。

## 2. 方法论：核心思想、关键技术细节
- **核心思想**：通过两个简单组件解决上述两个瓶颈。
- **组件一：Instance Cues（实例线索）**
  - 将稀疏的用户可编辑提示（如拖拽箭头、掩码）转化为密集的2.5D（相机相对）运动场。
  - 具体方法：对每个实例取平均光流，并结合实例掩码上的单目深度信息，形成像素对齐的控制信号。
  - 优点：比纯2D拖拽/箭头输入减少深度模糊性，同时保持用户易用性。
- **组件二：Dense RoPE（密集旋转位置编码）**
  - 在token空间中保留运动线索的显著性。
  - 对锚定在首帧上的一小组运动token，使用可时间寻址的旋转嵌入（RoPE）进行标记。
- **联合训练策略**：
  - 模型联合预测RGB帧和辅助地图（分割或深度）。
  - RGB分支处理外观，辅助分支固定结构，从而稳定优化并提升时间一致性。
  - 无需每帧轨迹脚本（per-frame trajectory scripts）。

## 3. 实验设计
- **任务场景**：图像到视频生成（I2V），包括对象运动连贯性、交互自然性等。
- **数据集/基准**：论文中未明确列出具体数据集名称（如UCF101、DAVIS等），但推断涉及常见视频生成基准（如OpenReview摘要未提供完整实验部分）。
- **对比方法**：未明确提及对比基线，但声称STANCE在多种场景下生成的运动更连贯、时序一致性更强。

## 4. 资源与算力
- **未明确说明**：论文摘要及元数据中未提及GPU型号、数量、训练时长等算力细节。需要查阅完整论文才能获得此类信息。

## 5. 实验数量与充分性
- **实验覆盖**：摘要提到“在多种视频生成场景中”验证，但未给出具体消融实验数量或数据集数量。
- **充分性判断**：仅凭摘要无法评估实验充分性。缺失消融实验对比（如有无Instance Cues、Dense RoPE的变体实验）、量化指标（FVD、FID、用户研究等）和跨模型对比。因此实验细节不充分，需要完整论文确认。

## 6. 主要结论与发现
- STANCE以简洁的组件有效提升了视频运动生成质量，解决了稀疏运动线索弱和外观-运动优化冲突两个核心问题。
- 在运动连贯性和时序一致性方面优于现有方法（未具体说明对比方法）。
- 该方法无需额外的训练定制（如逐帧脚本），保持用户易用性。

## 7. 优点
- **方法简洁而有效**：仅用两个组件（Instance Cues + Dense RoPE）即解决实质性问题。
- **用户友好**：输入仅为稀疏编辑（如拖拽、箭头），无需专业轨迹或深度标注。
- **创新性**：将实例级平均光流与深度信息结合生成2.5D密集运动场，以及利用Dense RoPE锚定运动token，均为新颖思路。
- **结构-外观解耦**：联合RGB+辅助预测头，避免纹理优先于时间一致性。

## 8. 不足与局限
- **实验覆盖不透明**：缺少具体数据集、量化指标、消融实验、基线对比等细节，无法验证泛化性和公平性。
- **算力资源未报告**：无法评估可复现性和计算成本。
- **应用限制**：可能对复杂多物体交互、长视频、大运动幅度等场景的效果未知；是否支持文本条件等扩展未说明。
- **偏差风险**：仅依赖摘要信息，存在选择性报道（只报告优点）。实际局限可能包括对深度估计依赖、实例分割要求等。

（完）
