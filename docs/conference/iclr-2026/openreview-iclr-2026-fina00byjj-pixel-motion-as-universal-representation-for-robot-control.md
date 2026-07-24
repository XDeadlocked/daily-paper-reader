---
title: Pixel Motion as Universal Representation for Robot Control
title_zh: 像素运动作为机器人控制的通用表征
authors: "Kanchana Ranasinghe, Xiang Li, E-Ro Nguyen, Cristina Mata, Jongwoo Park, Michael S Ryoo"
date: 2025-09-11
pdf: "https://openreview.net/pdf?id=finA00bYJj"
tags: ["query:sr"]
score: 9.0
evidence: 使用像素运动作为中间表示的视觉-语言-动作机器人控制框架
tldr: 该文提出LangToMo，一种双系统视觉-语言-动作框架，利用像素运动预测作为通用表征。高层系统2使用图像扩散模型生成文本条件像素运动序列，低层系统1将其转换为机器人动作。像素运动可从视频中弱监督提取，支持任意视频-字幕数据训练扩散模型，实现跨实体控制。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 现有机器人控制方法依赖任务特定表征，缺乏通用性。
method: 提出LangToMo框架，使用像素运动作为中间表征，结合扩散模型和运动到动作映射。
result: 在多种机器人任务上验证了像素运动表征的有效性和泛化能力。
conclusion: 像素运动可作为通用、可解释的表征，简化机器人控制学习。
---

## Abstract
We present LangToMo, a vision-language-action framework structured as a dual-system architecture that uses pixel motion forecasts as intermediate representations. 
Our high-level $\textit{System 2}$, an image diffusion model, generates text-conditioned pixel motion sequences from a single frame and past motion to guide robot control.
Pixel motion—a universal, interpretable, and motion-centric representation—can be extracted from videos in a weakly-supervised manner, enabling diffusion model training on any video-caption data.
Treating the generated pixel motion as largely embodiment-agnostic $\textit{universal representations}$, our embodiment-aware $\textit{System 1}$ module translates these into robot actions via motion-to-action mapping functions, which can be either hand-crafted or learned with minimal supervision.
System 2 operates as a high-level policy applied at sparse temporal intervals, while System 1 acts as a low-level policy at dense temporal intervals.
This hierarchical decoupling enables flexible, scalable, and generalizable robot control under both unsupervised and supervised settings, bridging the gap between language, motion, and action.
Visualizations at https://anonymous.4open.science/w/LangToMo.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **研究动机**：现有机器人控制方法依赖任务特定的表征（如关节角度、末端执行器位姿等），缺乏通用性和可解释性，难以在不同机器人实体、不同任务间迁移。语言指令到动作的映射通常需要大量标注数据，且难以扩展到新场景。
- **核心问题**：如何设计一种**通用、可解释、可弱监督获取**的中间表征，能够桥接语言、视觉与机器人动作，实现灵活、可扩展、泛化的机器人控制。
- **整体含义**：论文提出**像素运动（Pixel Motion）**作为这种通用表征，构建双系统视觉-语言-动作框架**LangToMo**，高层（System 2）利用图像扩散模型生成文本条件的像素运动序列，低层（System 1）将像素运动映射为具体机器人动作。像素运动可从任意视频-字幕数据中弱监督提取，从而支持大规模预训练，降低对机器人特定数据的依赖。

## 2. 论文提出的方法论

### 核心思想
- 使用**像素运动**（即视频帧中每个像素的运动矢量场）作为中间表示。该表示具有**通用性**（不依赖特定机器人形态）、**可解释性**（直观显示物体运动）、**弱监督可提取性**（可从视频通过光流等方法自动获取）。
- 构建**双系统架构**：
  - **System 2（高层策略）**：基于图像扩散模型，输入单帧图像、过去运动以及文本指令，生成未来像素运动序列。此过程是**体现无关**的，可仅使用视频-字幕数据进行训练。
  - **System 1（低层策略）**：将System 2生成的像素运动序列转换为具体的机器人动作（如关节扭矩、速度）。映射函数可以是**手工设计的**（如基于逆运动学），也可以**通过少量监督学习**获得。
- **层次化解耦**：System 2以稀疏时间间隔（如每秒一次）运行，提供运动规划；System 1以密集时间间隔（如每控制周期）运行，执行精细控制。

### 关键技术细节
- **像素运动提取**：从视频中通过光流算法（如RAFT）弱监督提取，无需人工标注。
- **扩散模型训练**：使用大量“视频-字幕”对（如网络视频），训练条件扩散模型生成像素运动。条件包括当前帧、历史运动、文本描述。
- **运动到动作映射**：对于已知运动学/动力学模型的机器人，可手工设计解析映射；对于未知模型，可收集少量机器人执行轨迹数据进行监督学习。

### 算法流程（文字说明）
1. **训练阶段**：
   - 从互联网视频中提取像素运动序列和字幕。
   - 训练扩散模型：输入(当前帧, 历史运动, 文本) → 输出未来像素运动。
   - （可选）收集少量机器人数据训练映射网络。
2. **推理阶段**：
   - 高层：给定当前相机图像、过去运动、语言指令，System 2扩散模型采样得到未来像素运动序列。
   - 低层：System 1将每个时间步的像素运动映射为机器人动作，执行控制。

（公式部分未在摘要中提及，无法详细说明。）

## 3. 实验设计

- **数据集与场景**：摘要未具体列出数据集，但提到“任意视频-字幕数据”可用于训练扩散模型。机器人实验可能涉及多种实体（如机械臂、移动机器人等），使用仿真或真实环境。
- **Benchmark**：未明确提及具体基准，推测对比了直接端到端方法（如RT-2）或基于其他中间表示的方法（如关键点、轨迹）。
- **对比方法**：未在摘要中列出，但可能对比了无中间表示的方法、使用其他表示（如深度图、光流作为输入）的方法等。

## 4. 资源与算力

- 摘要和元数据中**未明确说明**使用的GPU型号、数量或训练时长。仅能推断使用标准深度学习硬件（如A100或V100），训练时间取决于数据规模（数天至数周）。无法提供具体数字。

## 5. 实验数量与充分性

- **实验数量**：从元数据“result: 在多种机器人任务上验证”推测进行了多个任务和实体上的实验，可能包括**仿真环境**（如MetaWorld、RLBench）和**真实机器人**。但未给出具体消融实验的数目。
- **充分性与客观性**：由于论文被ICLR 2026拒绝，可能实验规模和对比不够充分，或者存在某些缺陷。但从方法新颖性看，像素运动作为中间表示具有一定的合理性。客观性方面，需要检查与基线方法的公平比较（如同样的数据集、计算资源），摘要未提供细节，难以判断。

## 6. 论文的主要结论与发现

- 像素运动可以作为**通用、可解释**的表示，简化机器人控制的学习。
- 通过双系统架构（扩散模型+运动到动作映射），可以实现**跨实体**的泛化，因为System 2是体现无关的。
- 弱监督提取像素运动使得利用大规模视频-字幕数据成为可能，减少了对机器人专用数据的依赖。
- 该框架在**无监督**和**有监督**设置下均有效，展示了灵活性。

## 7. 优点

- **表征的通用性**：像素运动不依赖于机器人具体形态，可跨实体迁移。
- **可解释性**：人类可以直观理解运动场，有助于调试和信任。
- **弱监督预训练**：利用互联网视频数据，扩展性强。
- **层次化解耦**：高层规划与低层控制分离，可独立优化。
- **简单有效的映射**：可以手工设计或少量样本学习，降低了动作标注成本。

## 8. 不足与局限

- **实验覆盖有限**：摘要未提及在不同场景（如复杂操作、动态环境）下的性能，以及与其他最先进方法（如RT-2、ACT）的定量对比。
- **像素运动不确定性问题**：光流在遮挡、纹理缺失区域可能不准确，影响后续映射。
- **扩散模型采样效率**：生成像素运动序列可能耗时，影响实时控制（摘要未提及时间开销）。
- **手工映射的局限性**：对于复杂机器人（如软体机器人）可能难以设计解析映射。
- **论文被拒**：可能方法本身存在缺陷，或实验不够充分，需注意结果的可信度。
- **缺乏开源代码和完整数据集**：只有匿名网站链接，未提供复现所需细节。

（完）
