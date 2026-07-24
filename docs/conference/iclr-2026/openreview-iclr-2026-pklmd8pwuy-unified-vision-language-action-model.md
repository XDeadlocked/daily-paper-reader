---
title: Unified Vision-Language-Action Model
title_zh: 统一视觉-语言-动作模型
authors: "Yuqi Wang, Xinghang Li, Wenxuan Wang, Junbo Zhang, Yingyan Li, Yuntao Chen, Xinlong Wang, Zhaoxiang Zhang"
date: 2026-01-26
pdf: "https://openreview.net/pdf?id=PklMD8PwUy"
tags: ["query:sr"]
score: 9.0
evidence: 统一VLA模型，将视觉、语言、动作和世界模型令牌化
tldr: 该论文提出UniVLA统一多模态VLA模型，将视觉、语言和动作都视为离散令牌进行自回归建模。通过生成式视觉监督（世界模型）增强视觉理解，并支持大规模视频数据训练。在多个机器人操作任务上展示了统一架构的优越性和泛化能力。
source: ICLR-2026-Accepted
selection_source: conference_retrieval
motivation: 现有VLA模型忽视观测中的时序和因果结构。
method: 将视觉、语言、动作令牌化，统一自回归建模，并引入世界模型监督。
result: 统一模型在多项任务上优于分离结构，泛化性强。
conclusion: 令牌化统一建模可提升VLA性能和数据效率。
---

## Abstract
Vision-language-action models (VLAs) have garnered significant attention for their potential in advancing robotic manipulation.
However, previous approaches predominantly rely on the general comprehension capabilities of vision-language models (VLMs) to generate action signals, often overlooking the rich temporal and causal structure embedded in visual observations. In this paper, we present UniVLA, a unified and native multimodal VLA model that autoregressively models vision, language, and action signals as discrete token sequences. This tokenized formulation naturally supports flexible multimodal task learning, particularly from large-scale video data, and further demonstrates that generative vision supervision can significantly enhance visual understanding. By incorporating world modeling during post-training, UniVLA captures causal dynamics from videos, facilitating effective transfer to downstream policy learning—especially for long-horizon tasks. Our approach sets new state-of-the-art results across several widely used simulation benchmarks, including CALVIN, LIBERO, and Simplenv-Bridge, substantially outperforming prior methods. For example, UniVLA achieves 95.5% average success rate on LIBERO benchmark, surpassing π₀-FAST's 85.5%. We further demonstrate its broad applicability through experiments on real-world ALOHA manipulation tasks and autonomous driving scenarios.

---

## 论文详细总结（自动生成）

以下是根据论文内容生成的结构化总结：

### 1. 核心问题与整体含义（研究动机和背景）
- **研究动机**：现有的视觉-语言-动作模型（VLA）主要依赖视觉-语言模型（VLM）的通用理解能力来生成动作信号，但往往忽略了视觉观测中蕴含的丰富时间与因果结构。这种分离式架构限制了模型对动态环境的深度理解，尤其是在长时域任务中表现不佳。
- **核心问题**：如何构建一个统一的、原生多模态的VLA模型，使其能够同时处理视觉、语言和动作信息，并充分利用时序与因果信息来提升机器人操作性能。
- **整体含义**：提出UniVLA，将视觉、语言、动作均视为离散令牌，通过自回归方式统一建模，并引入生成式视觉监督（世界模型）增强视觉理解，从而实现更高效、更泛化的机器人学习。

### 2. 方法论：核心思想、关键技术细节
- **核心思想**：将视觉、语言和动作信号统一表示为离散token序列，采用自回归方式建模，使模型能够端到端地学习多模态任务。同时在后训练阶段引入世界建模，从视频数据中捕获因果动态。
- **关键技术细节**：
  - **令牌化**：将视觉观测、语言指令和动作指令分别离散化为token，形成统一的序列。
  - **自回归建模**：使用Transformer架构按顺序预测下一个token，支持灵活的模态组合。
  - **生成式视觉监督（世界模型）**：后训练阶段，模型不仅预测动作，还生成下一帧视觉观察（或视觉特征），从而学习环境的因果动态，提升视觉理解能力。
  - **大规模视频数据训练**：利用海量无动作标签的视频数据进行预训练或辅助训练，增强模型的视觉表征和时序建模能力。
- **算法流程（文字说明）**：
  1. 输入：视觉帧、语言指令、历史动作 → 分别编码为离散token。
  2. 拼接成完整序列，送入Transformer进行自回归训练。
  3. 预训练阶段：使用视频数据（无动作标签）进行下一个视觉帧预测任务，学习世界模型。
  4. 后训练/微调阶段：在机器人操作数据上联合优化动作预测和视觉预测，实现策略学习。

### 3. 实验设计
- **数据集与场景**：
  - **仿真基准**：CALVIN、LIBERO、Simplenv-Bridge。
  - **真实世界**：ALOHA机械臂操作任务、自动驾驶场景。
- **Benchmark**：多个广泛使用的机器人操作模拟基准，包括长时域任务和多任务评估。
- **对比方法**：主要包括π₀-FAST（成功率85.5%）及其他先前VLA方法。UniVLA在LIBERO上达到95.5%平均成功率，显著超越。
- **评估任务**：涵盖多步操作、泛化能力、长时域任务等。

### 4. 资源与算力
- **文中未明确说明**：摘要及元数据未提及具体GPU型号、数量或训练时长。因此无法总结算力细节，需指出信息缺失。

### 5. 实验数量与充分性
- **实验数量**：覆盖3个模拟基准（CALVIN、LIBERO、Simplenv-Bridge）和2个真实场景（ALOHA、自动驾驶）。但未详细说明每个基准下的具体子任务数量及消融组数。
- **充分性与公平性**：
  - 充分性：对比了SOTA方法（π₀-FAST），且结果明显提升，表明方法有效。
  - 公平性：未详细说明数据分割、超参数设置等细节，但使用标准公开基准，对比方法均为已知。
  - 可能的不足：缺少对部件组件（如世界模型贡献）的定量消融分析；未报告统计显著性或多次重复实验的标准差。

### 6. 主要结论与发现
- 统一令牌化建模（视觉、语言、动作）可以显著提升VLA性能和数据效率。
- 生成式视觉监督（世界模型）能有效增强视觉理解，尤其对长时域任务有显著帮助。
- UniVLA在多个模拟基准和真实任务上达到新的SOTA，展现了统一架构的泛化能力。
- 后训练阶段引入视频因果动态学习是提升策略迁移的关键。

### 7. 优点
- **方法创新**：首次将视觉、语言和动作原生统一为离散token，并内置世界模型学习，突破了以往分离处理的限制。
- **数据效率**：能够利用大规模无标签视频数据预训练，降低对昂贵动作标签的依赖。
- **任务泛化**：统一的序列建模允许灵活处理各种多模态任务（操作、导航、自动驾驶）。
- **性能卓越**：在LIBERO上准确率提升10个百分点，证明了实用性。

### 8. 不足与局限
- **实验覆盖有限**：虽然包含模拟和真实场景，但真实任务仅涉及ALOHA和自动驾驶，未涵盖更复杂的灵巧操作或非结构化环境。
- **可重复性细节缺失**：未提供算力、超参数、模型规模等关键信息，难以复现。
- **缺乏消融实验**：未明确评估各组分（如世界模型、视频预训练）的单独贡献，消融的充分性存疑。
- **偏差风险**：训练数据可能偏向特定任务分布，泛化到全新环境（如家庭场景）的能力未知。
- **应用限制**：自回归推理可能带来延迟，实时控制受限；令牌化离散空间可能丢失连续动作的精细度。

（完）
