---
title: "SANA-Video: Efficient Video Generation with Block Linear Diffusion Transformer"
title_zh: SANA-Video：基于块线性扩散Transformer的高效视频生成
authors: "Junsong Chen, Yuyang Zhao, Jincheng YU, Ruihang Chu, Junyu Chen, Shuai Yang, Xianbang Wang, Yicheng Pan, Daquan Zhou, Huan Ling, Haozhe Liu, Hongwei Yi, Hao Zhang, Muyang Li, Yukang Chen, Han Cai, Sanja Fidler, Ping Luo, Song Han, Enze Xie"
date: 2026-01-26
pdf: "https://openreview.net/pdf?id=mzAchylAtf"
tags: ["query:sr"]
score: 9.0
evidence: 使用扩散Transformer的高效视频生成
tldr: 针对视频生成中高计算成本和长序列处理难题，提出SANA-Video模型，采用线性注意力DiT和常记忆KV缓存技术，在RTX 5090上实现720×1280分辨率、分钟级时长视频的高效生成，同时保持强文本-视频对齐，为高效视频扩散模型提供了新方案。
source: ICLR-2026-Accepted
selection_source: conference_retrieval
motivation: 现有视频扩散模型计算成本高，难以生成高分辨率长视频。
method: 提出线性DiT和常记忆KV缓存，实现高效视频生成。
result: 在RTX 5090上快速生成高分辨率长视频，文本对齐良好。
conclusion: 线性注意力机制可大幅提升视频扩散模型的效率。
---

## Abstract
We introduce SANA-Video, a small diffusion model that can efficiently generate videos up to 720×1280 resolution and minute-length duration. SANA-Video synthesizes high-resolution, high-quality and long videos with strong text-video alignment at a remarkably fast speed, deployable on RTX 5090 GPU. Two core designs ensure our efficient, effective and long video generation:  (1) Linear DiT: We leverage linear attention as the core operation, which is more efficient than vanilla attention given the large number of tokens processed in video generation. (2) Constant-Memory KV cache for Block Linear Attention: we design block-wise autoregressive approach for long video generation by employing a constant-memory state, derived from the cumulative properties of linear attention. This KV cache provides the Linear DiT with global context at a fixed memory cost, eliminating the need for a traditional KV cache and enabling efficient, minute-long video generation. In addition, we explore effective data filters and model training strategies, narrowing the training cost to 12 days on 64 H100 GPUs, which is only 1\% of the cost of MovieGen. Given its low cost, SANA-Video achieves competitive performance compared to modern state-of-the-art small diffusion models (e.g., Wan 2.1-1.3B and SkyReel-V2-1.3B) while being 16x faster in measured latency. Moreover, SANA-Video can be deployed on RTX 5090 GPUs with NVFP4 precision, accelerating the inference speed of generating a 5-second 720p video from 71s to 29s (2.4x} speedup). In summary, SANA-Video enables low-cost, high-quality video generation. Code and model will be publicly released.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 核心问题与整体含义（研究动机和背景）

- **研究动机**：现有视频扩散模型（如MovieGen）计算成本极高，难以在消费级GPU上生成高分辨率（如720p）、长时长（分钟级）视频。同时，Transformer架构中的标准注意力机制在面对视频生成中大量token时效率低下。
- **核心挑战**：如何在保持强文本-视频对齐和高质量的前提下，大幅降低视频生成的计算和内存开销，实现高效、低成本的部署。
- **整体含义**：提出SANA-Video，一个轻量级扩散模型，能在RTX 5090 GPU上快速生成720×1280分辨率、分钟级时长的视频，将训练成本降至MovieGen的1%，为高效视频扩散模型提供了新范本。

## 2. 方法论：核心思想、关键技术细节

- **核心思想**：用线性注意力替代标准注意力，并设计常记忆KV缓存机制，解决长序列生成中的内存爆炸问题。
- **关键技术细节**：
  - **Linear DiT**：将扩散Transformer中的标准自注意力替换为线性注意力（如通过核方法近似），使得注意力计算复杂度从O(N²)降至O(N)，适合处理视频生成中的大量token（时空位置）。
  - **Constant-Memory KV cache for Block Linear Attention**：针对长视频生成，提出分块自回归方法。利用线性注意力的累积性质，维护一个固定大小的常记忆状态（类似隐状态），该状态聚合了全局上下文信息。该缓存替代了传统KV缓存，内存开销不随视频长度增加而增长，从而支持分钟级视频生成。
  - **训练策略与数据过滤**：探索有效的数据过滤和模型训练策略，缩短训练周期（64台H100上仅12天）。
  - **推理加速**：支持NVFP4精度（4位浮点），在RTX 5090上将生成720p 5秒视频的推理时间从71秒降至29秒（2.4倍加速）。

## 3. 实验设计

- **数据集/场景**：未在摘要中明确提及具体训练数据集（可能为大规模视频-文本对）。
- **Benchmark**：未指定标准视频生成benchmark，但对比了现有小型扩散模型。
- **对比方法**：与Wan 2.1-1.3B、SkyReel-V2-1.3B等当前最先进的小型扩散模型对比。SANA-Video在保证竞争力的同时，实测延迟快16倍。
- **评估指标**：未详细列出，但提及“强文本-视频对齐”和“高质量”。

## 4. 资源与算力

- **训练硬件**：64块NVIDIA H100 GPU（H100为高性能数据中心GPU）。
- **训练时长**：12天。
- **对比**：训练成本仅为MovieGen的1%（MovieGen估计需要数千GPU天）。
- **部署硬件**：RTX 5090（消费级旗舰GPU），支持NVFP4精度加速推理。

## 5. 实验数量与充分性

- **实验数量**：摘要中仅提及与两个基线模型的比较以及加速比测试，未详细报告消融实验、多分辨率/时长测试等。信息有限，难以判断全面性。
- **充分性**：从摘要看，实验覆盖了效率（训练成本、推理速度）和质量（与先进小模型对比）两个关键维度，但缺乏对文本对齐、多样性、不同场景的定量评估（如FVD、CLIP Score等常用指标）。实验设计可能不够全面，但考虑到论文标题强调效率，重点突出即可。
- **客观性与公平性**：对比的基线模型为同量级小模型（1.3B参数），且SANA-Video声称在延迟上快16倍，但未说明是否在相同硬件和精度下对比。需要原文确认。

## 6. 主要结论与发现

- SANA-Video可在RTX 5090上以极低成本（训练12天）生成720p分钟级视频，且质量与Wan 2.1、SkyReel-V2等小模型相当。
- 线性注意力+常记忆KV缓存是高效长视频生成的关键：固定内存成本，支持分钟级时长。
- NVFP4精度可在保持质量的同时大幅加速推理（2.4倍）。
- 证明了小模型通过精心设计的架构和训练策略，可以达到接近大模型的效果，而计算成本降低两个数量级。

## 7. 优点

- **架构创新**：首次将块级线性注意力与常记忆KV缓存结合用于视频扩散模型，解决了长序列内存瓶颈。
- **极致效率**：训练成本仅为MovieGen的1%，推理可在消费级GPU上实时（或接近实时）生成高清长视频。
- **部署友好**：支持NVFP4精度，进一步降低硬件门槛。
- **透明度**：承诺开源代码和模型，促进复现和应用。

## 8. 不足与局限

- **实验覆盖不全**：摘要中未提及在标准视频生成benchmark（如UCF-101、Something-Something）上的定量结果（如FVD、IS、CLIP Score），也未提供与更大模型（如OpenAI Sora、MovieGen）的对比，仅与小模型比较。缺乏消融实验验证每个组件（线性注意力、常记忆缓存、数据过滤）的贡献。
- **文本对齐评估**：仅定性描述“强对齐”，无定量指标（如Video-Text Retrieval精度）。
- **应用限制**：论文未讨论生成视频的多样性、时间一致性和长程依赖性（分钟级视频中的场景突变、对象持久性等）是否良好。
- **可复现性**：虽然声明开源，但关键细节（如数据过滤规则、线性注意力的具体实现公式）未在摘要中给出，可能影响独立复现。
- **硬件依赖**：RTX 5090为最新高端卡，普通消费者尚难获取；NVFP4精度可能需要特定硬件支持。

（完）
