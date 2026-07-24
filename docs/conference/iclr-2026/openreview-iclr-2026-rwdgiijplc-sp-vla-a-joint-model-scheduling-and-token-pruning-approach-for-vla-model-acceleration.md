---
title: "SP-VLA: A Joint Model Scheduling and Token Pruning Approach for VLA Model Acceleration"
title_zh: "SP-VLA: 联合模型调度与标记剪枝的VLA模型加速方法"
authors: "Ye Li, Yuan Meng, Zewen Sun, Kangye Ji, Chen Tang, Jiajun Fan, Xinzhu Ma, Shu-Tao Xia, Zhi Wang, Wenwu Zhu"
date: 2026-01-26
pdf: "https://openreview.net/pdf?id=RwdGIIjPlC"
tags: ["query:sr"]
score: 8.0
evidence: 结合调度与剪枝的VLA加速
tldr: 针对VLA模型计算成本高的问题，提出SP-VLA统一框架，通过动作感知的模型调度和标记剪枝，同时利用序列决策中的时间冗余和视觉输入的空间冗余。在机器人操作和导航任务上，该方法显著提升推理速度而不牺牲控制性能。
source: ICLR-2026-Accepted
selection_source: conference_retrieval
motivation: VLA模型存在时序和空间冗余未充分利用。
method: 联合调度不同复杂度模型并剪枝冗余视觉标记。
result: 推理速度大幅提升，控制性能保持。
conclusion: 同时利用时序与空间冗余可有效加速VLA模型。
---

## Abstract
Vision-Language-Action (VLA) models have attracted increasing attention for their strong control capabilities. However, their high computational cost and low execution frequency hinder their suitability for real-time tasks such as robotic manipulation and autonomous navigation. Existing VLA acceleration methods primarily focus on structural optimization, overlooking the fact that these models operate in sequential decision-making environments. As a result, temporal redundancy in sequential action generation and spatial redundancy in visual input remain unaddressed. To this end, we propose SP-VLA, a unified framework that accelerates VLA models by jointly scheduling models and pruning tokens. Specifically, we design an action-aware model scheduling mechanism that reduces temporal redundancy by dynamically switching between VLA model and a lightweight generator.  Inspired by the human motion pattern of focusing on key decision points while relying on intuition for other actions, we categorize VLA actions into deliberative and intuitive, assigning the former to the VLA model and the latter to the lightweight generator, enabling frequency-adaptive execution through collaborative model scheduling. To address spatial redundancy, we further develop a spatio-semantic dual-aware token pruning method.  Tokens are classified into spatial and semantic types and pruned based on their dual-aware importance to accelerate VLA inference. These two mechanisms work jointly to guide the VLA in focusing on critical actions and salient visual information, achieving effective acceleration while maintaining high accuracy. Extensive experiments show that our method achieves 1.5$\times$ lossless acceleration in LIBERO and 2.4$\times$ in SimplerEnv, with up to 6\% average performance gain. Inference frequency and latency improve by 2.2$\times$ in SimplerEnv and 1.4$\times$ in LIBERO.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **核心问题**：视觉-语言-动作（Vision-Language-Action, VLA）模型虽然具备强大的控制能力，但其高计算成本和低执行频率限制了在机器人操作和自主导航等实时任务中的应用。
- **现有方法局限**：已有的VLA加速方法主要聚焦于结构优化（如模型压缩、量化），忽视了VLA模型运行在序列决策环境中这一事实，未能利用序列动作生成中的**时间冗余**和视觉输入中的**空间冗余**。
- **研究目标**：提出一种统一框架SP-VLA，通过联合模型调度与标记剪枝，在保持高控制精度的同时大幅提升推理速度，从而解决VLA模型在实时场景下的效率瓶颈。

## 2. 论文提出的方法论：核心思想、关键技术细节

### 核心思想
- 模仿人类的运动模式：在关键决策点进行“审慎思考”（deliberative），而其他动作依赖“直觉”（intuitive）。据此将VLA动作分为**审慎型**和**直觉型**，分别由不同复杂度的模型处理。
- 同时利用时序冗余（动作之间的相似性）和空间冗余（视觉标记中的无关区域）进行双重加速。

### 关键技术细节
1. **动作感知的模型调度机制**：
   - 设计一个轻量级的“判断器”，动态决定当前动作是“审慎型”还是“直觉型”。
   - **审慎型动作**：交由完整的VLA模型处理（高精度、高计算成本）。
   - **直觉型动作**：交由一个轻量级的生成器（如小型MLP或扩散模型）快速输出动作。
   - 通过这种**协同调度**实现频率自适应的执行，减少不必要的VLA模型调用，降低时序冗余。

2. **时空语义双感知的标记剪枝方法**：
   - 将视觉标记（visual tokens）分为**空间类型**（关注位置信息）和**语义类型**（关注物体类别等）。
   - 计算每个标记的**双感知重要性分数**（dual-aware importance），融合空间和语义两方面的贡献。
   - 基于重要性分数对冗余标记进行剪枝，保留关键视觉信息，从而减少VLA模型处理的计算量，解决空间冗余。

3. **联合优化**：
   - 调度机制和剪枝机制协同工作：调度机制指导VLA模型只在关键动作上执行，剪枝机制压缩每次VLA推理的输入规模，两者相辅相成，实现高效加速。

（论文摘要中未提供具体的公式或算法伪代码，仅描述了上述方法流程。）

## 3. 实验设计

### 数据集/场景与Benchmark
- **LIBERO**：机器人操作模拟环境（常见于VLA模型评估）。
- **SimplerEnv**：另一种机器人模拟环境（具体任务未明确，但涉及导航或操作）。
- 实验设置：对比了原始VLA模型（无加速）、以及可能的结构优化方法（但摘要中未列出具体对比方法名称）。

### 对比方法
- 文中未明确列出对比方法名称，但可以推测对比了直接使用VLA模型、仅剪枝、仅调度的变体等。

### 评估指标
- 推理速度/加速比（如1.5×、2.4×等）
- 平均性能增益（up to 6%）
- 推理频率与延迟提升（如SimplerEnv中2.2×，LIBERO中1.4×）

## 4. 资源与算力

- **文中未明确说明**：使用的GPU型号、数量、训练时长等具体算力信息未在摘要或元数据中提及。仅通过“加速倍数”间接反映了计算效率提升，但未透露训练或推理所使用的硬件配置。

## 5. 实验数量与充分性

- **实验数量**：至少涉及两个不同的模拟环境（LIBERO和SimplerEnv），并报告了加速比和性能增益。但文中未详细说明消融实验的组数（如单独调度、单独剪枝、联合调度剪枝等），也未展示更多数据集或真实机器人实验。
- **充分性评价**：实验覆盖了两种典型机器人任务环境，结果显示了显著的加速效果且性能无损甚至提升。但缺少以下方面的分析：对调度阈值的敏感性、剪枝比例的鲁棒性、不同动作类型分布的统计、以及与其他SOTA加速方法（如知识蒸馏、模型量化）的详细对比。总体而言，实验设计较为扎实但不算极其全面。

## 6. 论文的主要结论与发现

- **主要结论**：联合利用时间冗余（通过模型调度）和空间冗余（通过标记剪枝）可以有效加速VLA模型，而不牺牲控制性能。
- **具体结果**：
  - 在LIBERO上实现1.5×无损加速，在SimplerEnv上实现2.4×无损加速，且平均性能提升最高达6%。
  - 推理频率和延迟在SimplerEnv中提升2.2×，在LIBERO中提升1.4×。
- **意义**：证明了“审慎-直觉”分工与视觉标记剪枝相结合的策略在VLA模型加速中的有效性，为实时机器人控制提供了新思路。

## 7. 优点

- **方法创新性**：首次将**模型调度**（动作类型区分）与**标记剪枝**（视觉冗余去除）统一在VLA加速框架中，利用了人类运动控制的启发思想。
- **兼顾效率与精度**：不仅实现加速，而且性能略有提升（最高6%），表明剪枝和调度可能帮助模型聚焦更关键的信息，减少噪声干扰。
- **场景适应性**：在两个不同的模拟环境中均取得显著加速效果，显示一定通用性。
- **结果明确**：提供了具体的加速倍数和性能增益，易于理解。

## 8. 不足与局限

- **实验覆盖不够广泛**：仅在两个模拟环境下测试，缺乏在真实机器人平台上的验证，也未涉及更多样化的任务（如复杂操作、多步推理）。
- **对比方法缺失**：未列出具体的基线方法名称，不利于判断其相对于最先进技术的优势程度。
- **消融分析不充分**：未展示单独调度、单独剪枝、不同剪枝策略的效果对比，难以量化各组件贡献。
- **论文文本不完整**：提供的仅为摘要和元数据，缺乏完整方法细节（如调度判断器的实现、剪枝阈值的设置、轻量生成器的架构等），限制了可复现性。
- **算力成本未说明**：无法评估该方法在训练和部署时的资源开销。
- **可能偏差风险**：动作分类的准确性依赖于调度判断器，若分类错误可能影响性能；剪枝可能导致关键视觉信息丢失，在极端场景下可能不安全。

（完）
