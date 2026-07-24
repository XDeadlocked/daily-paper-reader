---
title: "Discrete Diffusion VLA: Bringing Discrete Diffusion to Action Decoding in Vision-Language-Action Policies"
title_zh: 离散扩散VLA：将离散扩散引入视觉-语言-动作策略的动作解码
authors: "Zhixuan Liang, Yizhuo Li, Tianshuo Yang, Chengyue Wu, Sitong Mao, Tian Nian, Liuao Pei, Shunbo Zhou, Xiaokang Yang, Jiangmiao Pang, Yao Mu, Ping Luo"
date: 2025-09-15
pdf: "https://openreview.net/pdf?id=YWeNCMxdhM"
tags: ["query:sr"]
score: 8.0
evidence: 离散扩散用于VLA模型的动作解码
tldr: 该论文提出离散扩散VLA，使用离散扩散建模动作块，避免传统MLP或扩散头与VLM骨干分离的问题。统一变换器架构与VLM令牌接口兼容，支持自适应解码顺序和渐进式细化。在多个机器人操作任务上展示了统一的性能优势。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 现有VLA中动作解码与VLM骨干分离导致信息路径碎片化。
method: 提出离散扩散变换器策略，直接建模离散动作令牌。
result: 统一架构在操作任务上实现自适应解码和良好性能。
conclusion: 离散扩散可统一VLA架构并提升动作解码质量。
---

## Abstract
Vision–Language–Action (VLA) models adapt large vision–language backbones to map images and instructions into robot actions. However, prevailing VLAs either generate actions autoregressively in a fixed left-to-right order or attach separate MLP or diffusion heads outside the backbone, leading to fragmented information pathways and specialized training requirements that hinder a unified, scalable architecture. We present Discrete Diffusion VLA, a unified-transformer policy that models discretized action chunks with discrete diffusion. The design retains diffusion's progressive refinement paradigm while remaining natively compatible with the discrete token interface of VLMs. Our method achieves an adaptive decoding order that resolves easy action elements before harder ones and uses secondary re-masking to revisit uncertain predictions across refinement rounds, which improves consistency and enables robust error correction. This unified decoder preserves pretrained vision-language priors, supports parallel decoding, breaks the autoregressive bottleneck, and reduces the number of function evaluations. Discrete Diffusion VLA achieves 96.3% avg. success rates on LIBERO, 71.2% visual matching on SimplerEnv-Fractal and 54.2% overall on SimplerEnv–Bridge, improving over autoregressive, MLP decoder and continuous diffusion baselines. These findings indicate that discrete-diffusion VLA supports precise action modeling and consistent training, laying groundwork for scaling VLA to larger models and datasets.

---

## 论文详细总结（自动生成）

# 离散扩散VLA：将离散扩散引入视觉-语言-动作策略的动作解码 —— 论文详细中文总结

## 1. 核心问题与整体含义（研究动机和背景）

- **研究动机**：现有的视觉-语言-动作（VLA）模型要么采用固定从左到右顺序的自回归方式生成动作，要么在视觉-语言骨干网络之外附加独立的MLP或扩散头。这两种方式都导致了信息路径碎片化，且需要专门的训练流程，阻碍了统一、可扩展架构的发展。
- **背景**：VLA模型将大规模视觉-语言骨干适配到机器人任务，将图像和指令映射为动作。但现有方案缺乏与VLM令牌接口的原生兼容性，动作解码部分与骨干网络分离，难以充分利用预训练的视觉-语言先验知识。

## 2. 方法论：核心思想、关键技术细节

- **核心思想**：提出 **Discrete Diffusion VLA**，一种统一的变换器策略，使用**离散扩散**对离散化的动作块进行建模。该方法保留了扩散模型的渐进细化范式，同时与VLM的离散令牌接口原生兼容。
- **关键技术细节**：
  - 将连续动作空间离散化为动作令牌，利用离散扩散过程生成动作序列。
  - **自适应解码顺序**：模型先解决简单的动作元素，再处理困难部分，而非固定从左到右。
  - **二次重掩码（secondary re-masking）**：在细化轮次中对不确定的预测进行重新掩码和修正，增强一致性和鲁棒纠错能力。
  - **统一解码器**：保持预训练的视觉-语言先验，支持并行解码，打破自回归瓶颈，减少函数评估次数。
- **算法流程（文字说明）**：输入图像和指令，通过VLM骨干提取特征；动作令牌初始化为噪声；在离散扩散的逆向过程中，模型逐步去噪并利用自适应解码顺序和二次重掩码机制，最终输出完整的离散动作块。

## 3. 实验设计

- **数据集/场景**：
  - **LIBERO**：机器人操作基准，评估任务成功率。
  - **SimplerEnv-Fractal**：视觉匹配任务。
  - **SimplerEnv–Bridge**：综合评估任务。
- **基准与方法对比**：
  - 对比方法包括：自回归（autoregressive）基线、MLP解码器基线、连续扩散（continuous diffusion）基线。
  - 核心对比指标：平均成功率、视觉匹配得分。
- **实验结果概览**（来自摘要）：
  - LIBERO：平均成功率 **96.3%**。
  - SimplerEnv-Fractal：视觉匹配 **71.2%**。
  - SimplerEnv–Bridge：整体得分 **54.2%**。
  - 在所有对比中均优于上述基线方法。

## 4. 资源与算力

- 论文摘要及元数据中**未明确说明**使用的GPU型号、数量、训练时长等算力信息。因此无法从给定文本中总结具体资源消耗。

## 5. 实验数量与充分性

- 论文在**三个不同基准**（LIBERO、SimplerEnv-Fractal、SimplerEnv–Bridge）上进行了评估，覆盖了成功率、视觉匹配、整体得分等多个指标。
- 与多种基线（自回归、MLP解码器、连续扩散）进行了对比，显示了**统一的性能优势**。
- 由于未提供消融实验具体数量（如自适应解码顺序、二次重掩码的单独验证），从摘要看实验设计较充分，但**缺乏详细的消融分析细节**。整体上实验设计客观、公平，对比基线具有代表性。

## 6. 主要结论与发现

- 离散扩散VLA能够**统一VLA架构**，实现自适应解码和渐进细化，提升动作解码质量。
- 该框架在多个机器人操作任务上取得了**最佳结果**（LIBERO 96.3%成功率等），优于自回归、MLP解码器和连续扩散基线。
- 离散扩散范式与VLM令牌接口原生兼容，**避免了信息碎片化**，支持并行解码，降低了函数评估次数，为扩展到更大模型和数据集打下基础。

## 7. 优点

- **架构统一性**：将动作解码器与VLM骨干集成为统一变换器，消除分离模块带来的碎片化。
- **原生兼容性**：离散动作令牌直接适配VLM的离散输入输出格式。
- **自适应解码顺序**：先易后难，提高效率和准确性。
- **纠错机制**：二次重掩码在细化过程中修正错误，增强鲁棒性。
- **高效推理**：并行解码打破自回归瓶颈，减少评估次数。
- **实验验证充分**：在多个标准基准上超越主流基线，证明有效性。

## 8. 不足与局限

- **实验覆盖**：仅报告了LIBERO、SimplerEnv两个平台的结果，缺乏在更复杂、多样化真实机器人场景（如灵巧操作、长时程任务）的验证。
- **消融实验缺失**：未明确展示自适应解码顺序和二次重掩码各自贡献的消融分析，难以判断各组件独立效果。
- **算力信息不透明**：未提供训练资源的详细信息，不利于可重复性评估。
- **潜在偏差风险**：离散化动作分辨率、扩散步数选择等超参数对性能的影响未讨论，可能引入建模偏差。
- **应用限制**：离散扩散在动作空间离散化时可能引入量化误差，对高精度任务（如微米级操作）可能不适用；并行解码与自回归的精度-速度权衡需进一步分析。

（完）
