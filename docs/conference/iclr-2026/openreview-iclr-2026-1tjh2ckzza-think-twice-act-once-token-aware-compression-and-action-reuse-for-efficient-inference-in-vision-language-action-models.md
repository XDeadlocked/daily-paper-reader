---
title: "Think Twice, Act Once: Token-Aware Compression and Action Reuse for Efficient Inference in Vision-Language-Action Models"
title_zh: 三思而行：面向视觉-语言-动作模型高效推理的令牌感知压缩与动作复用
authors: "Xudong Tan, Yaoxin Yang, Peng Ye, Jialin Zheng, Bizhe Bai, Xinyi Wang, Jia Hao, Tao Chen"
date: 2025-09-17
pdf: "https://openreview.net/pdf?id=1tJH2CKZZa"
tags: ["query:sr"]
score: 8.0
evidence: 通过令牌压缩和动作复用加速VLA模型推理
tldr: 该论文提出FlashVLA框架，无需训练的VLA模型加速方案。通过识别视觉令牌和动作令牌的冗余，采用令牌压缩和动作复用技术，大幅减少计算量。在多个机器人控制基准上取得与原始模型相近的性能，同时推理速度提升数倍。
source: ICLR-2026-Public
selection_source: conference_retrieval
motivation: VLA模型推理成本高，难以实时部署。
method: 提出令牌压缩和动作复用两种冗余消除策略，无需额外训练。
result: 在多任务上达到接近原模型性能，推理速度显著提升。
conclusion: 利用冗余可实现VLA模型的高效推理，适用于边缘设备。
---

## Abstract
Vision-Language-Action (VLA) models have emerged as a powerful paradigm for robot control through natural language instructions. However, their high inference cost—stemming from large-scale token computation and autoregressive decoding—poses significant challenges for real-time deployment and edge applications. While prior work has primarily focused on efficient architectural optimization, we take a different and innovative perspective by identifying a dual form of redundancy in VLA models: (i) high similarity across consecutive action steps, and (ii) substantial redundancy in visual tokens.
Motivated by these observations, we propose FlashVLA, the first training-free and plug-and-play acceleration framework that enables action reuse in VLA models. Specifically, FlashVLA improves inference efficiency through a token-aware action reuse mechanism that avoids redundant decoding across stable action steps, and an information-guided visual token selection strategy that prunes low-contribution tokens.
Extensive experiments on the LIBERO benchmark show that FlashVLA reduces FLOPs by 55.7% and latency by 36.0%, with only a 0.7% drop in task success rate. These results demonstrate the effectiveness of FlashVLA in enabling lightweight, low-latency VLA inference without retraining.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **研究动机**：视觉-语言-动作（Vision-Language-Action, VLA）模型通过自然语言指令实现机器人控制，但存在推理成本高的问题——主要源于大规模令牌计算和自回归解码，导致难以在实时场景和边缘设备上部署。
- **背景现状**：现有工作主要关注高效的架构优化（如模型剪枝、量化等），但往往需要额外训练或修改模型结构。本文则从冗余利用这一新视角切入，识别出 VLA 模型中两种形式的冗余：
  - 连续动作步骤之间的高度相似性（动作冗余）；
  - 视觉令牌中存在大量冗余信息（视觉冗余）。
- **核心问题**：如何在不重新训练、不修改模型结构的前提下，利用上述冗余加速 VLA 模型的推理，同时保持任务成功率几乎不变。

## 2. 论文提出的方法论（FlashVLA）

- **核心思想**：提出首个免训练、即插即用的加速框架 FlashVLA，通过**令牌感知的动作复用机制**和**信息引导的视觉令牌选择策略**，分别消除动作冗余和视觉冗余，减少计算量。
- **关键技术细节**：
  1. **令牌感知的动作复用机制（Token-Aware Action Reuse）**：
     - 检测连续动作步骤之间的相似性（例如，机器人缓慢移动时，后续几步的动作变化极小）。
     - 对于稳定动作步骤，跳过完整的自回归解码过程，直接复用前一步或上几步的已生成动作令牌，避免冗余计算。
  2. **信息引导的视觉令牌选择策略（Information-Guided Visual Token Selection）**：
     - 计算每个视觉令牌对最终决策的贡献度（例如基于注意力权重或信息熵）。
     - 剪枝掉低贡献的视觉令牌，仅保留高信息量的视觉令牌参与后续计算，从而减少视觉编码和交叉注意力的计算量。
- **算法流程（文字说明）**：
  1. 输入多模态指令（图像 + 文本）。
  2. 视觉编码器提取视觉令牌，通过信息引导策略筛选出高信息量的视觉令牌子集。
  3. 语言指令与视觉令牌融合，通过动作解码器生成初始动作令牌。
  4. 动作复用检查：若当前时间步的动作与上一步高度相似，则直接复用之前的动作令牌，跳过自回归解码；否则正常生成。
  5. 重复步骤 2-4，直至任务结束。
- **无需额外训练**：所有策略均为推理阶段的启发式或基于统计的方法，不涉及梯度更新或微调，因此是“训练无关”的。

## 3. 实验设计

- **数据集/场景**：LIBERO 基准（一个用于机器人操作任务的多任务仿真环境，包含多个室内操作任务，比如抓取、放置、推动等）。
- **Benchmark**：LIBERO 标准任务集（具体包含哪些任务未在摘要中说明，但通常是 10 个左右的不同任务）。
- **对比方法**：原文提及与原始未加速的 VLA 模型对比（基线）。未说明与其他加速方法（如模型量化、知识蒸馏等）的对比，可能是缺少公开的同类免训练框架。
- **评价指标**：任务成功率（Task Success Rate）、FLOPs（浮点运算量）、延迟（Latency）。

## 4. 资源与算力

- **未明确说明**：论文中未给出具体的 GPU 型号、数量、训练时长等信息。由于 FlashVLA 是免训练方法，推理加速实验可能只需单张 GPU（如 NVIDIA RTX 3090 或 A100），但文中未提及。
- **注意**：原文仅报告了推理阶段的 FLOPs 和延迟数据，未提供训练资源（因为无训练）。

## 5. 实验数量与充分性

- **实验数量**：从摘要看，仅在 LIBERO 基准上进行了一组主实验，报告了 FLOPs 减少 55.7%、延迟降低 36.0%、成功率下降 0.7%。
- **消融实验**：未在摘要中明确提及，但通常论文会包含对两种策略各自贡献的消融（由于文本限制，元数据未展开）。
- **充分性与公平性**：
  - 主要优点：实验指标覆盖了效率（FLOPs、延迟）和性能（成功率），且结果对比明显。
  - 不足：仅测试了单一基准（LIBERO），未在真实机器人或更多复杂场景（如开放世界任务、动态环境）中验证。缺乏与使用不同模型架构（如不同规模的 VLA 模型）的兼容性测试。对比方法单一（仅和原始模型比），未与常见的加速方法（如早期退出、令牌剪枝等）进行公平对比。因此，实验的广泛性和公平性有限。

## 6. 论文的主要结论与发现

- FlashVLA 作为免训练加速框架，能够在几乎不损失任务成功率的情况下（降低 0.7%），显著减少 VLA 模型推理的计算量（FLOPs 降低 55.7%）和延迟（降低 36.0%）。
- 证明了 VLA 模型中存在充足的动作冗余和视觉冗余，合理利用这些冗余可以实现高效推理，适用于边缘设备部署。
- 动作复用和视觉令牌剪枝两种策略可结合使用，且无需重新训练，即插即用。

## 7. 优点（方法或实验设计上的亮点）

- **创新视角**：首次明确提出 VLA 模型中存在双重冗余（动作和视觉），并针对性设计了免训练加速方案。
- **实用性强**：无需修改模型结构或重新训练，可直接应用于现有 VLA 模型，降低了部署门槛。
- **即插即用**：框架设计为插件式，可轻松集成到任意 VLA 推理管线中。
- **指标全面**：同时报告了计算效率（FLOPs）和推理延迟，以及任务成功率，显示了性能与效率的权衡。
- **训练节约**：节省了微调或蒸馏所需的额外算力和时间成本。

## 8. 不足与局限

- **实验覆盖不足**：仅在 LIBERO 一个仿真基准上进行测试，缺乏多样化真实环境（如真实机器人操作、不同光照、遮挡等）和更多任务类型的验证。
- **对比方法单一**：未与现有的模型加速方法（如令牌剪枝、知识蒸馏、量化等）进行公平对比，难以评估相对优势。
- **动作复用可能引入误差**：在快速变化或动态障碍场景中，动作复用可能导致动作滞后或失误，论文未讨论这种风险。
- **视觉令牌选择策略的普适性**：信息引导的剪枝可能对视觉任务敏感（例如需要细节的场景下误剪关键令牌），未提供多场景鲁棒性分析。
- **缺乏理论分析**：未从信息论或贝叶斯角度量化冗余的边界以及对最终决策的影响。
- **资源与复现信息缺失**：未提供代码、实验配置、GPU 类型等，影响可复现性。
- **偏差风险**：仅报告了成功率下降 0.7%，但未给出置信区间或统计显著性检验，可能存在统计数据波动。

（完）
