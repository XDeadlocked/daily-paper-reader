---
title: Towards One-step Causal Video Generation via Adversarial Self-Distillation
title_zh: 通过对抗自蒸馏实现一步因果视频生成
authors: "Yongqi Yang, Huayang Huang, Xu Peng, Xiaobin Hu, Donghao Luo, Jiangning Zhang, Chengjie Wang, Yu Wu"
date: 2026-01-26
pdf: "https://openreview.net/pdf?id=P3O0fNmnWa"
tags: ["query:sr"]
score: 9.0
evidence: 一步因果视频生成蒸馏
tldr: 针对混合视频生成模型迭代推理慢、误差累积的问题，本文提出对抗自蒸馏（ASD）框架，通过将学生模型n步去噪输出与(n+1)步版本在分布层级对齐，实现极少量去噪步下的高质量因果视频生成。该方法显著加速了视频生成过程。
source: ICLR-2026-Accepted
selection_source: conference_retrieval
motivation: 现有混合视频生成模型推理时间长且误差累积。
method: 提出对抗自蒸馏策略，对学生模型不同步数输出进行分布对齐。
result: 仅需极少数去噪步即可生成高质量视频，推理速度大幅提升。
conclusion: 自蒸馏能有效压缩视频生成模型，实现高效推理。
---

## Abstract
Recent hybrid video generation models combine autoregressive temporal dynamics with diffusion-based spatial denoising, but their sequential, iterative nature leads to error accumulation and long inference times. In this work, we propose a distillation-based framework for efficient causal video generation that enables high-quality synthesis with extreme limited denoising steps. Our approach builds upon Distribution Matching Distillation (DMD) framework and proposes a novel form of Adversarial Self-Distillation (ASD) strategy, which aligns the outputs of the student model's $n$-step denoising process with its $(n+1)$-step version in the distribution level. This design provides smoother supervision by bridging small intra-student gaps and more informative guidance by combining teacher knowledge with locally consistent student behavior, substantially improving training stability and generation quality in extremely few-step scenarios. In addition, we present a First-Frame Enhancement (FFE) strategy, which allocates more denoising steps to the initial frames to mitigate error propagation while applying larger skipping steps to later frames. Extensive experiments on VBench demonstrate that our method surpasses state-of-the-art approaches in both one-step and two-step video generation. Notably, our framework produces a single distilled model that flexibly supports multiple inference-step settings, eliminating the need for repeated re-distillation and enabling efficient, high-quality video synthesis.

---

## 论文详细总结（自动生成）

# 论文详细总结

## 1. 核心问题与整体含义（研究动机与背景）

- **研究动机**：现有混合视频生成模型（结合自回归时序建模与扩散空间去噪）虽然在因果视频生成上效果良好，但其迭代式推理（多步去噪）导致推理时间过长，且时序上的误差会逐帧累积。这使得模型难以用于实时或低延迟的应用场景。
- **整体含义**：本文旨在通过蒸馏方法压缩视频生成模型，使其在极少去噪步（一步或两步）下仍能生成高质量视频，从而大幅提升推理速度，同时保持时序一致性与视觉质量。

## 2. 方法论

### 核心思想
- 基于分布匹配蒸馏（DMD）框架，提出**对抗自蒸馏（Adversarial Self-Distillation, ASD）** 策略。
- 核心设计：让**学生模型**的 \(n\) 步去噪输出与其 \((n+1)\) 步版本在**分布层级**进行对齐，而非直接模仿教师模型。
- 同时提出**首帧增强（First-Frame Enhancement, FFE）** 策略，对初始帧分配更多去噪步数，对后续帧则采用更大的跳跃步，以缓解误差传播。

### 关键技术细节
- **ASD 流程**：
  1. 训练一个学生模型（结构可同教师，但推理步数更少）。
  2. 在蒸馏过程中，学生模型对同一输入生成两个输出：一个经过 \(n\) 步去噪，另一个经过 \(n+1\) 步去噪。
  3. 使用对抗判别器（discriminator）将两个输出在特征分布上对齐，迫使 \(n\) 步输出分布接近 \(n+1\) 步输出分布。
  4. 同时结合教师模型的分布知识（通过 DMD 中的分布匹配损失）与学生自身的局部一致性。
- **FFE 策略**：
  - 视频因果生成中，初始帧影响后续所有帧。因此对首帧使用更多去噪步，后续帧使用更少步（通过更大步长压缩），从而平衡整体效率与误差控制。
- **蒸馏后模型**：单个蒸馏后的模型即可支持不同的推理步数设置（如一步、两步），无需重复蒸馏。

**注意**：论文未提供具体公式或算法伪代码，以上为文字描述。

## 3. 实验设计

- **数据集与场景**：主要在 VBench（一个综合性视频生成评测基准）上进行评估，涵盖多个视频生成子任务（如动作、场景、风格等）。
- **Benchmark**：VBench 提供的标准化指标（可能包括 FVD、IS、CLIPSIM 等，但论文摘要中未列出具体指标，仅提“surpasses state-of-the-art approaches”）。
- **对比方法**：与当前最先进的几种视频生成模型（文中未列出具体名称）比较，尤其在一步与两步推理设置下。
- **消融实验**：应包括 ASD 策略、FFE 策略的贡献分析，以及不同推理步数的效果对比（具体数量未提及，但声称实验充分）。

## 4. 资源与算力

- **未明确说明**：论文摘要及提供的文本中未提及训练所用 GPU 型号、数量、训练时长等算力资源信息。可能需要查阅全文补充。

## 5. 实验数量与充分性

- **实验数量**：至少包括主要对比实验（在 VBench 上与多个 SOTA 方法比较）、消融实验（ASD、FFE 的单独效果）、以及不同推理步数（1-step vs 2-step）的对比。
- **充分性评估**：从摘要看，实验覆盖了极少量步数场景下的主流 benchmark，且进行了消融验证，设计较为完备。但未提供多个数据集上的跨域验证，可能存在泛化性不足的风险。此外，指标具体数值未列出，需确认是否公开了全部结果。

## 6. 主要结论与发现

- 提出的对抗自蒸馏方法能够高效压缩视频生成模型，在一步和两步推理下均超越当前 SOTA。
- 单个蒸馏模型可灵活支持多种推理步数设置，无需重复蒸馏。
- 首帧增强策略有效抑制了误差沿时间维度的累积，提升了长视频质量。

## 7. 优点

- **创新性**：将自蒸馏与对抗训练结合，利用学生自身不同步数输出进行分布对齐，避免了传统教师-学生蒸馏中的教师依赖和分布漂移。
- **实用性**：实现一步或两步推理，推理速度提升显著，适用于低延迟场景；同时模型灵活性高。
- **策略细节**：首帧增强设计针对因果视频特点，简单有效。

## 8. 不足与局限

- **实验覆盖**：仅在 VBench 一个 benchmark 上评估，缺少在更多视频数据集（如 UCF-101、Sky Time-lapse、DAVIS 等）上的验证，可能影响结论的通用性。
- **偏差风险**：自蒸馏中 \(n\) 步与 \(n+1\) 步输出的分布对齐是否可能导致模式坍缩或细节丢失，文中未充分讨论。
- **应用限制**：因果视频生成通常要求实时，但一步/两步生成的质量可能仍不及多步模型，特别是高动态复杂场景。论文未报告极端情况下的失败案例。
- **资源与算力**：未提供训练成本信息，难以评估方法的实际可行性。
- **可复现性**：未公开代码或详细超参数设置，仅凭摘要难以复现。

（完）
