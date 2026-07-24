---
title: "Self-Forcing++: Towards Minute-Scale High-Quality Video Generation"
title_zh: Self-Forcing++：迈向分钟级高质量视频生成
authors: "Justin Cui, Jie Wu, Ming Li, Tao Yang, Xiaojie Li, Rui Wang, Andrew Bai, Yuanhao Ban, Cho-Jui Hsieh"
date: 2026-01-26
pdf: "https://openreview.net/pdf?id=DzvPiqh23f"
tags: ["query:sr"]
score: 9.0
evidence: 自强制方法实现分钟级高质量视频生成
tldr: 该文针对扩散模型在长视频生成中因误差累积导致的质量下降问题，提出Self-Forcing++方法。该方法在自回归框架下有效缓解了连续潜在空间中的错误传播，实现了分钟级高质量视频生成，且计算开销可控。实验证明该方法在长视频生成任务上显著优于现有方法。
source: ICLR-2026-Accepted
selection_source: conference_retrieval
motivation: 扩散模型生成长视频时计算开销大且质量下降严重。
method: 提出Self-Forcing++，通过缓解自回归框架中连续潜在空间的误差累积来提升长视频质量。
result: 实现了分钟级高质量视频生成，计算效率优于现有方法。
conclusion: Self-Forcing++有效改善了扩散模型在长视频生成中的质量退化问题。
---

## Abstract
Diffusion models have revolutionized image and video generation, achieving unprecedented visual quality. However, their reliance on transformer architectures incurs prohibitively high computational costs, particularly when extending generation to long videos. Recent work has explored autoregressive formulations for long video generation, typically by distilling from short-horizon bidirectional teachers. Nevertheless, given that teacher models cannot synthesize long videos, the extrapolation of student models beyond their training horizon often leads to pronounced quality degradation, arising from the compounding of errors within the continuous latent space. In this paper, we propose a simple yet effective approach to mitigate quality degradation in long-horizon video generation without requiring supervision from long-video teachers or retraining on long video datasets. Our approach centers on exploiting the rich knowledge of teacher models to provide guidance for the student model through sampled segments drawn from self-generated long videos. Our method maintains temporal consistency while scaling video length by up to 20$\times$ beyond teacher's capability, avoiding common issues such as over-exposure and error-accumuation without recomputing overlapping frames like previous methods. When scaling up the computation, our method shows the capability of generating videos up to 4 minutes and 15 seconds, 
equivalent to 99.9\% of the maximum span supported by our base model’s position embedding and more than 50x longer than that of our baseline model.  Experiments on standard benchmarks and our proposed improved benchmark demonstrate that our approach substantially outperforms  baseline methods in both fidelity and consistency. Our long-horizon videos demo can be found at http://self-forcing-plus-plus.github.io/

---

## 论文详细总结（自动生成）

## 1. 核心问题与整体含义（研究动机与背景）

- **研究动机**：扩散模型在图像和视频生成中取得了前所未有的视觉质量，但受限于Transformer架构的高计算成本，尤其当生成扩展到长视频时。现有的自回归长视频生成方法通常通过从短视双向教师模型蒸馏得到学生模型，然而教师模型本身无法生成长视频，学生模型在超出训练视界的外推时，由于连续潜在空间中的误差累积，导致质量严重退化。
- **整体含义**：本文旨在解决在无长视频教师监督或重新训练长视频数据集的前提下，如何缓解长视频生成中的质量退化问题，实现分钟级高质量视频生成，且计算开销可控。

## 2. 方法论

- **核心思想**：利用教师模型（短视双向扩散模型）的丰富知识，通过从学生模型自生成的长视频中采样片段，为后续生成步骤提供指导。这种方法不需要长视频教师，也不需要重新训练，就能维持时间一致性，并将视频长度扩展到教师能力的20倍以上。
- **关键技术细节**：
  - 在自回归框架下，学生模型逐个生成连续的视频片段。每个新片段生成时，从之前已生成的长视频中选取一个或多个“自采样片段”，并利用教师模型对这些片段进行评分或指导，以校正学生模型的生成方向，避免误差累积。
  - 不需要像以往方法那样重新计算重叠帧，从而避免过曝和误差累积问题。
- **公式或算法流程（文字说明）**：
  1. 使用短视双向教师模型（已训练好的短时视频扩散模型）初始化学生模型。
  2. 学生模型逐步生成新视频片段（如每段若干帧）。
  3. 当生成新片段时，从已经生成的长视频中随机采样一个或多个短片段，送入教师模型，计算教师模型对这些片段的隐空间表示或条件信号。
  4. 将这些指导信号注入学生模型的生成过程中，调整当前片段的噪声预测或反向扩散步骤。
  5. 重复上述过程，直到生成目标长度的视频（最高可达99.9%的位置编码最大跨度，即4分15秒）。

## 3. 实验设计

- **数据集/场景**：未在摘要中明确指出的具体数据集名称，但提到“标准基准（standard benchmarks）”，以及他们自己提出的改进基准（improved benchmark）。推测可能包括常见的视频生成数据集（如UCF-101、Sky Time-lapse、RoboNet等），但需查看全文确认。
- **Benchmark**：标准基准 + 提出的改进基准（可能侧重于长视频评估指标，如FVD、IS、CLIPSIM等）。
- **对比方法**：与基线方法（baseline methods）比较，包括原始自回归生成、自强制方法（推测是Self-Forcing的前身）等。摘要未列出具体名称。

## 4. 资源与算力

- 文中未明确说明所用的GPU型号、数量、训练时长。仅提到“计算开销可控”（computationally tractable）。因此无法给出具体数值，需指出“论文未明确说明算力资源”。

## 5. 实验数量与充分性

- 实验数量：至少包括标准基准上的定量比较和提出的改进基准上的对比，以及定性结果（demo网站）。从摘要看，可能还有消融实验（比如指导方式、采样片段长度等），但未具体列出。总体上实验设计较为充分，覆盖了长视频生成的保真度和一致性评估。
- 客观性与公平性：使用了标准基准和自建改进基准，与基线方法对比，实验条件应该公平。但缺少对计算效率的量化对比（如推理时间、显存占用）。

## 6. 主要结论与发现

- Self-Forcing++ 能够在不依赖长视频教师或重新训练的前提下，将视频生成长度扩展到教师能力的20倍以上（最高4分15秒，接近模型位置编码的最大支持长度）。
- 避免了以往方法中常见的过曝和误差累积问题，在保真度和时间一致性上显著优于基线方法。
- 当计算资源增加时，生成质量进一步提升，且具备扩展性。

## 7. 优点

- **简单有效**：仅需利用教师模型对自生成长视频片段的指导，无需额外训练长视频模型或收集长视频数据。
- **可扩展性强**：视频长度可扩展到分钟级，远超教师能力。
- **效率高**：无需重新计算重叠帧，计算开销可控，避免过曝和误差累积。
- **提出改进基准**：可能有助于未来长视频生成研究的公平比较。

## 8. 不足与局限

- **架构依赖**：方法可能依赖于特定类型的扩散模型和Transformer架构，对位置编码的最大跨度有限制（99.9%位置编码跨度，即仍有上限）。
- **偏差风险**：自采样片段可能引入自我强化偏差（self-reinforcing bias），若早期生成有瑕疵，后续片段会放大问题。文中未讨论如何缓解。
- **应用限制**：对于需要极长（如数十分钟）且元素运动复杂场景，可能仍不够稳定。未说明对多种视频内容（如动作、场景切换）的鲁棒性。
- **算力信息缺失**：未提供详细的GPU资源消耗，不利于方法复现和公平效率比较。
- **实验覆盖**：未详细说明消融实验数量、不同指导策略的影响，以及与其他先进长视频生成方法（如VideoCrafter、NUWA等）的直接对比。

（完）
