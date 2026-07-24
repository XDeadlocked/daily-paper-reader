---
title: Flow Caching for Autoregressive Video Generation
title_zh: "FlowCache: 自回归视频生成的流缓存方法"
authors: "Yuexiao Ma, Xuzhe Zheng, Jing Xu, Xiwei Xu, Feng Ling, Xiawu Zheng, Huafeng Kuang, Huixia Li, XING WANG, Xuefeng Xiao, Fei Chao, Rongrong Ji"
date: 2026-01-26
pdf: "https://openreview.net/pdf?id=vko4DuhKbh"
tags: ["query:sr"]
score: 9.0
evidence: 自回归视频生成的流缓存方法
tldr: 自回归视频生成逐块合成速度慢，而现有缓存策略不适用于不同块相似性模式变化的情况。提出FlowCache，首个专为自回归视频生成设计的缓存框架，允许每个视频块维护独立缓存策略，实现细粒度加速。实验表明，在保证质量的前提下显著提升生成速度。
source: ICLR-2026-Accepted
selection_source: conference_retrieval
motivation: 自回归视频生成速度缓慢，现有缓存方法不适用。
method: 为每个视频块设计独立缓存策略。
result: 显著加速生成速度，质量保持。
conclusion: 独立分块缓存有效加速自回归视频生成。
---

## Abstract
Autoregressive models, often built on Transformer architectures, represent a powerful paradigm for generating ultra-long videos by synthesizing content in sequential chunks. However, this sequential generation process is notoriously slow. While caching strategies have proven effective for accelerating traditional video diffusion models, existing methods assume uniform denoising across all frames—an assumption that breaks down in autoregressive models where different video chunks exhibit varying similarity patterns at identical timesteps.
In this paper, we present \textbf{FlowCache}, the first caching framework specifically designed for autoregressive video generation. Our key insight is that each video chunk should maintain independent caching policies, allowing fine-grained control over which chunks require recomputation at each timestep. We introduce a chunkwise caching strategy that dynamically adapts to the unique denoising characteristics of each chunk, complemented by a joint importance–redundancy optimized KV cache compression mechanism that maintains fixed memory bounds while preserving generation quality.
Our method achieves remarkable speedups of $\textbf{2.38}\times$ on MAGI-1 and $\textbf{6.7}\times$ on SkyReels-V2, with negligible quality degradation (VBench: $0.87\uparrow$ and $0.79\downarrow$ respectively). These results demonstrate that FlowCache, successfully unlocks the potential of autoregressive models for real-time, ultra-long video generation—establishing a new benchmark for efficient video synthesis at scale. The code is available at https://github.com/mikeallen39/FlowCache.

---

## 论文详细总结（自动生成）

# 论文总结：FlowCache: 自回归视频生成的流缓存方法

## 1. 核心问题与整体含义（研究动机和背景）
- **研究动机**：自回归模型（基于Transformer架构）通过逐块顺序合成实现超长视频生成，但该过程极为缓慢。现有缓存加速策略（如KV Cache）主要用于视频扩散模型，它们假设所有帧在相同时间步的噪声去除过程是均匀的，这一假设在自回归生成中不成立——不同视频块在相同时间步表现出不同的相似性模式。
- **整体含义**：亟需一种专为自回归视频生成设计的缓存框架，以显著提升生成速度，同时保持输出质量，从而解锁实时、超长视频生成的潜力。

## 2. 方法论：核心思想、关键技术细节
- **核心思想**：每个视频块应维护独立的缓存策略，允许细粒度控制哪些块在哪个时间步需要重计算。  
- **关键技术细节**：
  - **分块级缓存策略（Chunkwise Caching）**：动态适应每个视频块独特的去噪特性，针对不同块分别决定缓存命中或重算。
  - **联合重要性-冗余度优化的KV缓存压缩机制**：在固定内存上限内，通过优化重要性（保留关键信息）和冗余度（去除重复内容）来压缩KV缓存，在保证生成质量的同时控制内存占用。
- **算法流程（文字说明）**：
  - 输入：自回归视频生成模型，逐块生成视频序列。
  - 对每个时间步，为每个视频块独立评估是否需要重算KV缓存（基于块间的相似性模式）。
  - 利用联合重要性-冗余度度量对KV缓存进行压缩，确保内存不超过预设边界。
  - 缓存策略动态更新，使得后续块生成时能复用已缓存的计算结果。

## 3. 实验设计
- **数据集/场景**：未在摘要中明确列出具体数据集名称。但测试了两种模型：MAGI-1 和 SkyReels-V2，涵盖了不同规模和复杂度的自回归视频生成场景。
- **基准（Benchmark）**：使用 VBench 作为质量评估基准（分数上升/下降表示质量变化）。
- **对比方法**：未明确列出对比的缓存方法，但隐式对比了“无缓存”的原始自回归生成流程，以及传统均匀缓存策略（因文中指出其不适用于自回归生成）。实际对比应包含标准基线（如Full Recompute、现有视频扩散模型的缓存方法）——但摘要未提供完整信息。

## 4. 资源与算力
- **文中未明确说明**：未提及GPU型号、数量、训练时长等算力信息。仅提供了推理加速倍数（2.38×和6.7×）及质量指标。可能实验在通用GPU（如A100或V100）上进行，但无具体说明。

## 5. 实验数量与充分性
- **实验数量**：展示了两个模型（MAGI-1, SkyReels-V2）上的加速比和质量变化。未展示其他数据集或消融实验（如不同缓存压缩策略的对比、不同分块大小的效果等）。**实验覆盖有限**，缺乏对多个场景、不同视频长度、不同自回归架构的全面验证。
- **充分性评估**：虽然结果显著（加速倍数高且质量损失极小），但仅凭两个模型的结果不足以证明方法的普适性。缺少与更先进缓存方法（如StreamingLLM、H2O等）的对比，也未分析内存和计算开销的实际测量。因此，实验**不够充分**，可能引发偏差风险（例如仅在特定模型上表现良好）。

## 6. 主要结论与发现
- FlowCache作为首个专为自回归视频生成设计的缓存框架，实现了：
  - 在MAGI-1上加速2.38×，VBench质量上升0.87；
  - 在SkyReels-V2上加速6.7×，VBench质量仅下降0.79。
- 验证了“独立分块缓存有效加速自回归视频生成”的核心假设，并证明联合重要性-冗余度优化能在固定内存内保持生成质量。

## 7. 优点
- **方法创新性**：首次针对自回归视频生成设计逐块独立缓存策略，突破了扩散模型均匀缓存的假设限制。
- **实用高效**：显著提升生成速度（最高6.7×），且质量几乎无损，为实时超长视频生成铺平道路。
- **完整开源**：提供代码库，可复现结果。
- **理论清晰**：联合优化重要性和冗余度，在有限内存下实现有效压缩，兼具理论合理性和工程简洁性。

## 8. 不足与局限
- **实验覆盖不足**：仅测试两个模型，未在更多自回归视频生成模型（如CogVideo, VideoPoet等）上验证；未报告在不同视频长度、分辨率、块大小下的性能变化。
- **缺乏标准化对比**：未与现有流行的缓存方法（如KV cache offloading、sliding window attention、分块循环缓存等）进行定量比较。
- **资源信息缺失**：未公开推理所需的实际GPU类型、显存消耗、计算成本，难以评估部署可行性。
- **潜在偏差风险**：VBench指标仅反映部分质量维度，未涉及时序一致性、运动自然度等关键视频指标；加速比可能是由于特定架构（如MAGI-1的块设计）导致，在其他架构上未必保持。
- **消融研究缺位**：未单独分析分块缓存 vs 联合压缩各自的贡献，也未优化超参数（如重要性-冗余度权衡系数）的影响。

（完）
