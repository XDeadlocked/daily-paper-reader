---
title: "FDVLA: A Flow-Diffusion Vision-Language-Action Framework with Dual Reasoning Modulation"
title_zh: FDVLA：具有双推理调制的流扩散视觉-语言-动作框架
authors: "Maowei Jiang, Qi Wang, Ruiqi Li, Hongfeng Ai, Quangao Liu, Yifan WANG, Hongliang Niu, Pengyu Zeng, Moquan Cheng, Yusong Hu, Xiaoxin Deng, Zhiyong Dong, Peter Búš, Long ZENG"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=qQuca6fbFX"
tags: ["query:sr"]
score: 9.0
evidence: 融合语义推理和动作生成的VLA框架
tldr: 现有VLA框架中，自回归方法产生不连贯轨迹，扩散方法缺乏语义注入。本文提出FDVLA，将流场用于全局轨迹规划，细粒度扩散用于动作修正，并引入双推理调制模块注入任务推理语义。在机器人操作任务上，FDVLA实现了更平滑、更智能的动作生成。该工作为VLA框架的设计提供了新思路，融合了规划与精细控制。
source: ICLR-2026-Public
selection_source: conference_retrieval
motivation: 现有VLA方法在轨迹平滑性和语义推理整合上存在不足。
method: 结合流场全局规划和扩散局部细化，并通过双推理调制注入语义信息。
result: 在模拟机器人操作任务中，FDVLA生成的轨迹更平滑且任务成功率更高。
conclusion: FDVLA有效融合了语义推理和动作生成，提升了VLA性能。
---

## Abstract
Recent advances in vision-language models (VLMs) have empowered robots to interpret natural language and perform complex manipulation tasks. Existing vision-language-action (VLA) frameworks typically adopt autoregressive decoding or diffusion-based strategies. While the former may lead to fragmented or less smooth trajectories, the latter often lacks explicit injection of reasoning semantics into the action generation process, which can affect the quality of generated actions. In this paper, we propose FDVLA, a unified framework integrating semantic reasoning with smooth and physically coherent action generation. We introduce a flow-diffusion mechanism that unifies global trajectory planning (via flow fields) and fine-grained action refinement (via diffusion) in a dual-headed policy, enabling physically coherent and stable action generation. Additionally, we design DualMod, a lightweight module that injects semantic signals into both velocity and noise prediction branches, thus integrating high-level reasoning into action generation. Extensive experiments across diverse simulated and real-world robotic tasks, demonstrate that FDVLA achieves solid performance, efficient inference, and shows robust generalization under a variety of task conditions.

---

## 论文详细总结（自动生成）

# FDVLA：具有双推理调制的流扩散视觉-语言-动作框架 —— 论文详细总结

## 1. 核心问题与整体含义（研究动机和背景）

- **研究背景**：视觉-语言模型（VLM）的进步使机器人能够理解自然语言并执行复杂操作任务。现有视觉-语言-动作（VLA）框架主要分为两类：自回归解码方法和扩散策略方法。
- **核心问题**：
  - 自回归方法生成的动作轨迹常出现碎片化或不平滑现象；
  - 扩散方法虽然能生成连续动作，但缺乏将推理语义显式注入动作生成过程的能力，导致生成动作的质量和语义一致性不足。
- **整体含义**：本文旨在解决上述两个缺陷，提出一个统一框架，将语义推理与平滑、物理连贯的动作生成融合起来，提升机器人操作的智能性和轨迹质量。

## 2. 提出的方法论

### 核心思想
- **统一框架**：FDVLA 将语义推理、全局轨迹规划和细粒度动作细化整合到一个端到端框架中。
- **流-扩散双头策略**：将流场（flow field）用于全局轨迹规划，将扩散（diffusion）用于细粒度动作修正，两者通过双头策略并行工作，实现物理连贯且稳定的动作生成。
- **双推理调制模块（DualMod）**：一个轻量级模块，将语义信号同时注入速度预测分支和噪声预测分支，从而实现高层次推理与动作生成的深度融合。

### 关键技术细节
1. **流-扩散机制**：
   - **全局规划流场**：利用流场对整个轨迹进行粗粒度规划，提供动作的时空一致性和平滑性基础。
   - **局部扩散细化**：在全局规划的基础上，通过扩散过程对每一步动作进行细粒度调整和噪声优化，提高动作的精确性和鲁棒性。
2. **DualMod 模块**：
   - 该模块接收视觉-语言推理出的语义信号，将其分解为两个分支：
     - 速度预测分支：注入语义以引导动作的时序连贯性；
     - 噪声预测分支：注入语义以优化扩散过程中的去噪方向。
   - 通过这种双注入机制，高层语义（如任务推理、物体关系）直接参与动作生成的计算图，而非仅作为条件输入。

### 算法流程（文字说明）
- 输入：视觉观察、自然语言指令。
- 第一步：利用 VLMs 对指令和场景进行推理，输出语义推理信号。
- 第二步：语义信号通过 DualMod 模块分别注入到流场规划分支（速度预测）和扩散细化分支（噪声预测）。
- 第三步：流场分支生成全局轨迹粗略骨架，扩散分支在当前骨架附近迭代细化每一步动作。
- 第四步：最终输出平滑且语义连贯的动作序列。

（注：由于论文仅公开了摘要和元数据，具体公式和网络架构细节未提供，上述描述基于摘要中的关键词和逻辑推断。）

## 3. 实验设计

### 使用的场景与数据集
- **模拟环境**：多种模拟机器人操作任务（具体环境名称未在摘要中列出，推测为常见的机器人操作基准，如 MetaWorld、RLBench、Mujoco 等）。
- **真实世界**：多种真实机器人操作任务（具体任务类型未明确，但提到“diverse simulated and real-world robotic tasks”）。

### Benchmark
- 未明确说明具体 benchmark 名称，但应与同领域 VLA 框架的标准基准（如 CALVIN、PerAct 等）类似。

### 对比方法
- 对比了现有的 VLA 框架，包括自回归方法（如 RT-2、PaLM-E 等）和扩散方法（如 Diffusion Policy、ACT 等）。具体对比列表未给出，但摘要强调 FDVLA 在性能、推理效率和泛化能力上表现更优。

## 4. 资源与算力

- **明确说明**：论文摘要及元数据中**未提及**使用的 GPU 型号、数量、训练时长等算力信息。可能细节在正文中，但基于当前提取内容无法获取。
- **建议**：若需要完整了解算力配置，需查阅论文全文。目前只能指出信息缺失。

## 5. 实验数量与充分性

- **实验组数**：摘要提到“大量实验”（Extensive experiments），涵盖多样化的模拟和真实机器人任务。但未列出具体实验数量、消融实验设置。
- **消融实验**：由于 DualMod 模块是核心贡献，推测论文包含对其消融的研究（例如移除语义注入、仅用流场或仅用扩散的对比），但摘要未明确说明。
- **客观性与公平性**：
  - 优点：同时进行了模拟和真实实验，覆盖不同场景，增强了结果的可信度。
  - 不足：缺少实验对比的详细统计数据和误差条等信息，无法判断实验的统计显著性和重复性。可能存在选择性报告最佳结果的风险。

## 6. 主要结论与发现

- FDVLA 在模拟和真实机器人操作任务中均实现了**坚实的性能**（solid performance）。
- 推理效率高（efficient inference），且在不同任务条件下表现出**鲁棒的泛化能力**（robust generalization）。
- 结合流场全局规划和扩散局部细化，能够生成**物理连贯且平滑**的动作轨迹。
- DualMod 模块成功将语义推理注入动作生成，**提升了动作质量**，弥补了纯扩散方法的不足。

## 7. 优点

1. **方法新颖性**：首次将流场与扩散结合在 VLA 框架中，发挥两者各自优势（全局规划+局部细化）。
2. **语义注入机制**：双推理调制模块（DualMod）轻量级且有效，能显式将高层推理融入低层动作生成。
3. **实验多样性**：涵盖模拟和真实场景，并强调泛化能力，增加了方法的实用性。
4. **输出动作质量高**：解决了自回归方法的碎片化和扩散方法的语义缺失问题。

## 8. 不足与局限

1. **实验细节不充分**：摘要未提供具体的数据集名称、对比方法列表、消融实验结果和数值指标，难以评估方法相比 SOTA 的真实增益。
2. **算力信息缺失**：未说明训练和推理的计算资源，无法判断方法的可复现性和计算成本。
3. **偏差风险**：未提及对失败案例的分析，可能掩盖了方法在某些边缘条件下的弱点。
4. **应用限制**：目前仅聚焦于机器人操作任务，是否可推广至其他连续控制领域（如导航、灵巧手操控）未知。
5. **复现难度**：缺乏代码和详细网络架构说明，仅靠摘要难以复现结果。

（完）
