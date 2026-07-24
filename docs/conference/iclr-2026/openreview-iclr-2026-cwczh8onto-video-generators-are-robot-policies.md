---
title: Video Generators are Robot Policies
title_zh: 视频生成器即机器人策略
authors: "Junbang Liang, Pavel Tokmakov, Ruoshi Liu, Sruthi Sudhakar, Paarth Shah, Rares Andrei Ambrus, Carl Vondrick"
date: 2025-09-17
pdf: "https://openreview.net/pdf?id=cWczH8ontO"
tags: ["query:sr"]
score: 9.0
evidence: 将视频生成作为机器人策略代理
tldr: 针对现有机觉运动策略泛化差且依赖大量演示数据的问题，提出Video Policy框架，将视频生成作为策略学习的代理，端到端联合训练视频和动作生成。实验表明，该方法在极少演示数据下即可提取策略，显著提升鲁棒性和样本效率，对未见物体和背景展现出强泛化能力。
source: ICLR-2026-Public
selection_source: conference_retrieval
motivation: 现有机觉运动策略泛化差且依赖大量数据。
method: 将视频生成作为策略学习的代理，端到端训练。
result: 少量演示数据即可提取策略，泛化性强。
conclusion: 视频生成可作为机器人策略学习的有效替代范式。
---

## Abstract
Despite tremendous progress in dexterous manipulation, current visuomotor policies remain fundamentally limited by two challenges: they struggle to generalize under perceptual or behavioral distribution shifts, and their performance is constrained by the size of human demonstration data. In this paper, we use video generation as a proxy for robot policy learning to address both limitations simultaneously.
We propose Video Policy, a modular framework that combines video and action generation that can be trained end-to-end. Our results demonstrate that learning to generate videos of robot behavior allows for the extraction of policies with minimal demonstration data, significantly improving robustness and sample efficiency. Our method shows strong generalization to unseen objects, backgrounds, and tasks, both in simulation and the real world. We further highlight that task success is closely tied to the generated video, with action-free video data providing critical benefits for generalizing to novel tasks. By leveraging large-scale video generative models, we achieve superior performance compared to recent VLAs and video-action models, paving the way for more scalable and data-efficient robot policy learning.

---

## 论文详细总结（自动生成）

# 视频生成器即机器人策略（Video Generators are Robot Policies）

## 1. 核心问题与整体含义（研究动机与背景）
- **现有挑战**：当前视触觉运动策略（visuomotor policies）虽然在灵巧操作上取得了巨大进步，但仍然存在两个根本性局限：① 在感知或行为分布偏移下泛化能力差；② 性能受限于人类演示数据的规模（数据稀缺）。
- **核心动机**：同时解决泛化不足和数据效率低的问题，探索将视频生成（video generation）作为机器人策略学习的代理（proxy）。
- **整体含义**：提出一种新范式，将机器人行为视频的生成和动作生成端到端联合训练，使得模型能够从极少演示数据中提取策略，并显著提升鲁棒性、样本效率以及对未见物体、背景和任务的泛化能力。

## 2. 方法论（核心思想、关键技术细节）
- **核心思想**：使用视频生成作为策略学习的代理。即通过学习生成机器人行为的未来视频，从中提取控制策略，无需依赖大量演示数据。
- **关键框架**：Video Policy，一个模块化框架，联合训练视频生成和动作生成，可端到端优化。
- **技术细节**：
  - 模型同时输出视频帧（预测机器人未来行为）和对应的控制动作。
  - 动作无关的视频数据（action-free video data）被证明对泛化到新任务至关重要。
  - 利用大规模视频生成模型（如预训练的视频扩散模型或Transformer）作为基础，提升视频生成质量，从而改善策略提取效果。
- **算法流程（文字描述）**：输入当前观测（图像/状态）→ 视频生成模块产生若干未来帧序列 → 动作预测模块根据生成的视频输出动作→ 端到端反向传播更新整个框架。

## 3. 实验设计
- **数据集/场景**：仿真环境（具体名称未在元数据中明确，可能为常见的机器人操作模拟器如MetaWorld、Adroit等）和真实世界（real world）场景。
- **Benchmark**：未明确列出具体任务名，但提到测试了对未见物体、背景和任务的泛化能力。
- **对比方法**：与近期最先进的 VLA（Vision-Language-Action）模型和视频-动作模型（video-action models）进行了比较，结果显示Video Policy在性能和样本效率上更优。

## 4. 资源与算力
- **文中未明确说明**：元数据及摘要未提及使用的GPU型号、数量、训练时长等具体算力信息。需要指出：论文正文可能包含这些细节，但在提供的文本中缺失。

## 5. 实验数量与充分性
- **实验数量**：摘要提到“在仿真和真实世界中均进行了实验”，包含对泛化能力（未见过物体、背景、任务）的测试，以及比较不同方法。但具体组数不详。
- **充分性与公平性**：从摘要看，实验覆盖了仿真与真实场景，并对比了主流基线（VLA和视频-动作模型），样本效率提升和泛化性结果均被强调，实验设计较为全面。但缺少消融实验细节（如视频生成质量对策略影响的量化分析）和统计显著性说明。总体上，实验方向合理，但细节需待全文验证。

## 6. 主要结论与发现
- 学习生成机器人行为的视频，可以从极少量演示中提取有效策略。
- 该方法显著提升了鲁棒性和样本效率。
- 对未见物体、背景和任务展现出强泛化能力，在仿真和真实世界均成立。
- 动作无关的视频数据是泛化到新任务的关键。
- 利用大规模视频生成模型，性能超越近年来先进的 VLA 和视频-动作模型。
- 任务成功与生成视频的质量紧密相关。

## 7. 优点（方法与实验亮点）
- **创新范式**：将视频生成作为策略代理，突破了传统需要大量人工演示的限制。
- **端到端模块化框架**：视频生成与动作生成联合训练，设计简洁高效。
- **数据效率**：能从极少演示中学习，降低了对昂贵人工标注的依赖。
- **强泛化能力**：通过视频生成捕捉任务物理规律，对分布外变化鲁棒。
- **结合大规模预训练**：利用成熟视频生成模型，提升基座能力，取得更优结果。

## 8. 不足与局限
- **实验细节缺失**：所提供的元数据中没有列出具体仿真环境、任务数量、演示数据量级、消融实验组数，且算力信息完全缺失，难以全面评估复现成本和结果可靠性。
- **偏差风险**：未提及是否在多类型机器人或不同形态上验证，可能存在对特定硬件或场景的偏好。
- **应用限制**：视频生成本身需要较高计算资源，且推理时需先生成视频再提取动作，可能引入延迟，不适合高实时性任务。此外，生成视频的质量直接影响策略性能，若生成失真要可能造成严重错误。
- **对真实世界复杂场景**：论文只提及“未见物体、背景、任务”，但未覆盖光照变化、传感器噪声、动态障碍等更复杂的真实世界分布偏移。
- **潜在安全风险**：基于视频生成的策略可能继承视频模型中的偏见或产生不合规行为，缺乏安全性讨论。

（完）
