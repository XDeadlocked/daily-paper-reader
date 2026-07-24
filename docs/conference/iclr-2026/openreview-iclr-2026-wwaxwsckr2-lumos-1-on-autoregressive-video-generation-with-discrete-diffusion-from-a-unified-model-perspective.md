---
title: "Lumos-1: On Autoregressive Video Generation with Discrete Diffusion from a Unified Model Perspective"
title_zh: Lumos-1：从统一模型视角看基于离散扩散的自回归视频生成
authors: "Hangjie Yuan, Weihua Chen, Jun CEN, Hu Yu, Jingyun Liang, Shuning Chang, Zhihui Lin, Tao Feng, Pengwei Liu, Jiazheng Xing, Hao Luo, Jiasheng Tang, Fan Wang, Yi Yang"
date: 2026-01-26
pdf: "https://openreview.net/pdf?id=wWAxwSCKR2"
tags: ["query:sr"]
score: 9.0
evidence: 提出Lumos-1，一种基于离散扩散的自回归视频生成模型
tldr: 现有自回归视频生成模型存在架构不统一、依赖外部编码器或推理延迟大等问题。本文提出Lumos-1，基于大语言模型统一框架，引入离散扩散和MM-RoPE位置编码以高效建模时空相关性。实验结果表明Lumos-1在视频质量与生成速度上达到先进水平，推动了自回归视频生成的发展。
source: ICLR-2026-Accepted
selection_source: conference_retrieval
motivation: 现有自回归视频生成模型架构不统一、推理效率低。
method: 利用离散扩散和MM-RoPE实现高效的自回归视频生成。
result: 在多个视频生成基准上取得高质量与高效率。
conclusion: 统一模型框架与离散扩散有效提升了视频生成性能。
---

## Abstract
Autoregressive large language models (LLMs) have unified a vast range of language tasks, inspiring preliminary efforts in autoregressive (AR) video generation. Existing AR video generators either diverge from standard LLM architectures, depend on bulky external text encoders, or incur prohibitive latency due to next-token decoding. In this paper, we introduce Lumos-1, an LLM-based unified model for AR video generation with efficient discrete diffusion. Firstly, to fit videos with LLMs, we identify that 1D RoPE is ill-suited for visual spatiotemporal correlation modeling, and while demonstrated to be useful, naive 3D RoPE exhibits imbalanced frequency spectra. Therefore, we propose MM‑RoPE, which preserves the original textual RoPE while seamlessly accommodating video data with comprehensive frequency spectra and scaled 3D positions. Secondly, to fit the video data's nature and overcome the inefficiency of next-token decoding, we adopt a parallel and mask-based discrete diffusion with the intra-frame bidirectional and inter-frame causal attention masks. Based on this attention mask, we uncover the frame‑wise loss imbalance issue caused by spatial information redundancy and propose Autoregressive Discrete Diffusion Forcing, which introduces temporal tube masking during training with a compatible inference‑time masking policy to avoid quality degradation. Despite using only 48 GPUs for pre-training, limited data and a discrete tokenizer, Lumos-1 achieves results surpassing those of Show-o2 on GenEval, COSMOS-Video2World on VBench-I2V, and OpenSoraPlan on VBench-T2V.

---

## 论文详细总结（自动生成）

# 论文详细总结：Lumos-1：从统一模型视角看基于离散扩散的自回归视频生成

## 1. 核心问题与整体含义（研究动机与背景）

- **研究动机**：现有自回归（AR）视频生成模型存在三大问题：
  - 架构不统一：偏离标准大语言模型（LLM）架构，缺乏通用性。
  - 依赖外部编码器：如需要笨重的文本编码器，增加模型复杂度和参数。
  - 推理延迟大：采用逐token自回归解码，生成长视频时计算成本极高。
- **背景**：LLM已成功统一各种语言任务，启发研究者探索将AR视频生成纳入LLM框架。然而，直接将LLM应用于视频生成会遇到位置编码不匹配（1D RoPE不适合视觉时空建模）、解码效率低、帧间损失不平衡等问题。
- **整体含义**：本文旨在构建一个基于LLM的统一视频生成模型，通过离散扩散和新型位置编码克服上述挑战，从而在保持高质量的同时大幅提升生成效率，推动自回归视频生成向实用化迈进。

## 2. 方法论：核心思想、关键技术细节

### 2.1 核心思想
- **统一框架**：基于LLM结构，将视频生成任务融入自回归/扩散混合范式，避免引入额外编码器。
- **离散扩散**：使用并行、基于掩码的离散扩散替代逐token自回归解码，显著降低推理延迟。
- **新型位置编码 MM‑RoPE**：针对视频的时空相关性，改进传统的RoPE编码，同时保留文本RoPE的优势。

### 2.2 关键技术细节
- **MM‑RoPE（多模态旋转位置编码）**：
  - 发现1D RoPE不适合视频的时空建模，而朴素3D RoPE存在频谱不平衡问题。
  - MM‑RoPE保留原始文本RoPE的结构，同时无缝适配视频数据：引入完整频谱和缩放后的3D位置信息，使模型能同时处理文本和视频的时空位置。
- **注意力掩码设计**：
  - 采用**帧内双向、帧间因果**的注意力掩码：视频帧内部token可互相双向关注，帧之间保持因果顺序（前帧影响后帧）。
  - 结合此掩码，使用**并行掩码离散扩散**：一次生成多帧，而非逐帧自回归解码。
- **Autoregressive Discrete Diffusion Forcing（自回归离散扩散强制）**：
  - 发现帧间因果掩码会导致**帧级损失不平衡**：空间信息冗余使某些帧的损失贡献过大或过小。
  - 解决：在训练中引入**时间管掩码（temporal tube masking）**，配合兼容的推理时掩码策略，避免质量下降。

### 2.3 算法流程（文字说明）
1. 输入文本/图像条件，经LLM编码后得到隐表示。
2. 视频帧序列使用离散分词器（如VQ-VAE）量化为离散token。
3. 在训练阶段：对离散token施加部分掩码（包括时间管掩码），模型学习预测被掩码部分；损失函数计算预测与真实token的交叉熵。
4. 推理阶段：从完全掩码的离散token开始，通过多步并行扩散逐步去掩码，同时严格按照帧间因果顺序（后帧利用前帧已生成的信息）。
5. 最终将生成的离散token解码为视频帧。

## 3. 实验设计

- **使用数据集**：文中未明确列出具体数据集名称，但提到“有限数据”和“离散分词器”，推测使用公开视频数据集（如WebVid、HD-VG等常见数据集）。
- **评估基准（Benchmark）**：
  - **GenEval**：用于评估文本到图像/视频生成质量。
  - **VBench-I2V**：图像到视频生成的综合基准。
  - **VBench-T2V**：文本到视频生成的综合基准。
- **对比方法**：
  - **Show-o2**（对比GenEval）
  - **COSMOS-Video2World**（对比VBench-I2V）
  - **OpenSoraPlan**（对比VBench-T2V）
- 实验设置：均为文本条件或图像条件视频生成，采用标准评估指标（如FID、FVD、CLIP score等，文中未详列）。

## 4. 资源与算力

- **GPU数量**：48张GPU用于预训练。
- **GPU型号**：文中未明确说明型号，通常此类研究使用NVIDIA A100或H100，但无法确定。
- **训练时长与数据量**：同样未提及，但指出“有限数据”，说明预训练规模不大。
- **总体评价**：算力相对轻量（48GPU），在资源有限条件下取得有竞争力的结果。

## 5. 实验数量与充分性

- **实验组数**：元数据中仅报告了在三个基准上对比三种方法的结果。未提及详细的消融实验数量，但从方法论部分可推断有消融研究：
  - 对比不同位置编码（1D RoPE vs 3D RoPE vs MM‑RoPE）
  - 对比不同掩码策略（有无时间管掩码）
  - 对比自回归 vs 离散扩散的推理效率
- **充分性评估**：
  - 优势：覆盖了主要视频生成任务（T2V、I2V）和常见基准，对比方法为最新SOTA，结果显著优于对比方法。
  - 不足：缺乏更多多样化数据集（如长视频、高分辨率）的测试；未报告标准偏差或多次运行统计；消融实验细节未在摘要中展开。总体实验设计基本合理，但信息不完整，无法完全判断公平性（如是否相同数据来源、相同评估代码等）。

## 6. 主要结论与发现

- **核心结论**：Lumos-1在仅用48GPU预训练、有限数据和简单离散分词器的情况下，在GenEval、VBench-I2V、VBench-T2V上均超越当前先进模型（Show-o2、COSMOS-Video2World、OpenSoraPlan）。
- **关键发现**：
  - 1D RoPE不适合视频时空建模，而MM‑RoPE能有效保留文本能力并提升视频生成质量。
  - 离散扩散结合帧内双向/帧间因果掩码可实现高效并行生成，且通过Autoregressive Discrete Diffusion Forcing解决帧级损失不平衡。
  - 统一LLM框架下融合离散扩散是提升视频生成速度与质量的有效途径。

## 7. 优点

- **方法创新**：
  - 提出MM‑RoPE，巧妙兼容文本和视频，解决频谱不平衡问题。
  - 设计帧内双向/帧间因果掩码，实现高效并行扩散，同时保留自回归的因果时序。
  - 引入时间管掩码训练策略，针对性地解决了因果掩码下的损失不平衡。
- **效率优势**：离散扩散消除了逐token解码的延迟，生成速度显著优于自回归模型。
- **资源高效**：仅用48GPU，数据量有限即达到SOTA，证明方法本身的高效性。
- **统一性**：完全基于LLM架构，无需外部编码器，便于未来扩展到其他多模态任务。

## 8. 不足与局限

- **实验覆盖有限**：
  - 仅报告三个基准的结果，未在更多视频生成基准（如UCF-101、Kinetics-600、MSR-VTT等）上验证。
  - 未评估长视频生成（>10秒）或高分辨率（如1080p）场景。
  - 缺乏对生成多样性和可控性的深入分析。
- **偏差风险**：
  - 数据集细节未公开，可能存在与对比方法数据分布不一致的风险。
  - 离散分词器本身可能引入信息损失，影响生成细节。
  - 仅48GPU预训练，若增加算力或许可进一步提升，但对比方法可能使用了更多资源，公平性需更多信息佐证。
- **应用限制**：
  - 当前仅支持文本到视频和图像到视频，未探索视频到视频、可控视频生成等更复杂任务。
  - 推理时掩码策略需要精心设计，可能增加部署复杂度。
  - 离散扩散的步数对质量的影响未详细分析。
- **可复现性**：代码和模型未明确公开，影响验证。

（完）
