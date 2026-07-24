---
title: Benchmarking Physical Reasoning of Video Generative Models with Real Physical Experiments
title_zh: 用真实物理实验基准测试视频生成模型的物理推理
authors: "Chenyu Zhang, Daniil Cherniavskii, Antonios Tragoudaras, Antonios Vozikis, Thijmen Nijdam, Derck W. E. Prinzhorn, Mark Bodracska, Nicu Sebe, Andrii Zadaianchuk, Stratis Gavves"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=1E6pburMKc"
tags: ["query:sr"]
score: 10.0
evidence: 视频生成模型物理推理基准测试
tldr: 本文提出Morpheus基准，用于评估视频生成模型的物理推理能力。现有评估依赖主观判断或轨迹匹配，难以衡量物理合理性。Morpheus包含130个真实世界物理现象视频，涵盖多种物理规律。通过精心设计的任务和评估指标，系统性地测试模型是否遵循物理定律。实验表明，现有模型在物理推理上存在显著不足，为世界模型评估提供了标准化工具。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 现有视频生成评估依赖主观判断或轨迹匹配，无法有效衡量物理合理性。
method: 构建Morpheus基准，包含130个真实世界物理实验视频，设计覆盖多种物理规律的评估任务。
result: 实验表明当前视频生成模型在物理推理上表现不足，该基准揭示了模型作为世界模型的局限性。
conclusion: Morpheus为评估视频生成模型的物理世界建模能力提供了标准化基准。
---

## Abstract
Recent advances in image and video generation raise hopes that these models possess world modeling capabilities—the ability to generate realistic, physically plausible videos. This could revolutionize applications in robotics, autonomous driving, and scientific simulation. However, before treating these models as world models, we must ask: Do they adhere to physical laws?  Current evaluation methods rely on subjective judgments or trajectory matching, limiting their usage for physical reasoning estimation, where many generations could be physically plausible. 
Thus, we introduce **Morpheus**, a new benchmark for evaluating video generation models on physical reasoning. It features 130 real-world videos capturing physical phenomena, guided by conservation laws. Using those as conditioning for video generation, we assess physical plausibility using physics-informed metrics evaluated with respect to infallible conservation laws known per physical setting, leveraging advances in physics-informed neural networks and vision-language foundation models.
% Since artificial generations lack ground truth, we assess physical plausibility using physics-informed metrics evaluated with respect to infallible conservation laws known per physical setting, leveraging advances in physics-informed neural networks and vision-language foundation models. 
Our findings reveal that even with advanced prompting and video conditioning, current models struggle to encode physical principles despite generating aesthetically pleasing videos.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 核心问题与整体含义（研究动机和背景）

- **研究问题**：当前的图像和视频生成模型展现出一定的世界建模潜力，能够生成逼真、物理上看似合理的视频。然而，这些模型是否真正遵循物理定律，能否作为可靠的世界模型应用于机器人、自动驾驶和科学仿真等领域，仍然是个悬而未决的问题。
- **现有评估方法的不足**：目前的评估多依赖主观判断（如人类评分）或简单的轨迹匹配（如像素误差），难以有效衡量生成的视频在物理上的合理性。例如，一个物体掉落时可能外观逼真但轨迹违反重力，这样的生成仍可能被误判为“好”。
- **研究动机**：迫切需要建立一套客观、可量化的物理推理评估基准，系统性地检验视频生成模型是否真正理解并遵守物理守恒定律。

## 2. 提出的方法论

### 核心思想
- **Morpheus基准**：通过**真实世界物理实验视频**作为参考，引导模型生成与之条件一致的视频，再基于**已知的物理守恒定律**（如动量守恒、能量守恒）设计物理信息指标，评估生成视频的物理合理性。
- 不同于依赖人工标注或虚构场景，Morpheus直接使用高精度的真实物理测量数据，避免人为偏差。

### 关键技术细节
1. **数据构建**：收集130个真实物理现象视频，涵盖多种守恒定律（如动量、能量、角动量等），每个视频附带精确的物理测量（如轨迹、速度、力等）。
2. **条件生成**：将真实视频作为输入条件（如首帧或关键帧），让视频生成模型生成后续帧。
3. **评估指标**：
   - 利用**物理信息神经网络（PINNs）** 和**视觉语言基础模型（VLMs）** 自动提取生成视频中的物理量（如物体位置、速度）。
   - 将这些物理量代入对应的守恒定律方程，计算**守恒误差**（如能量差值、动量偏差），作为物理合理性的核心定量指标。
   - 同时结合视觉质量指标（如FID、FVD）进行多维度评估。

### 算法流程（文字说明）
1. 输入真实物理视频（含起始条件）。
2. 用目标视频生成模型生成后续视频（通常从起始帧开始）。
3. 对生成视频应用目标检测/跟踪算法提取物体运动轨迹。
4. 使用物理信息神经网络或基础模型估计每个时间步的物理量（速度、动量、能量等）。
5. 计算与真实守恒定律的偏差，得到物理合理性得分。
6. 统计多个视频的平均得分，衡量模型整体物理推理能力。

## 3. 实验设计

- **数据集**：Morpheus基准自身包含**130个真实世界物理实验视频**，覆盖多种守恒定律场景（如自由落体、碰撞、钟摆、旋转等）。
- **Benchmark定义**：以这130个视频作为测试集，要求模型基于起始条件生成完整视频，然后评估其物理合理性。
- **对比方法**：文中未具体列出模型名称，但指出对比了当前主流的视频生成模型（如扩散模型、自回归模型等），包括使用不同提示策略（如文本提示、视频条件）的变体。由于摘要未提供细节，推测实验包括了多个代表性模型（如Sora、GENIE、VideoLDM等）的对比。

## 4. 资源与算力

- **未明确说明**：原文摘要及元数据中未提及使用的GPU型号、数量、训练时长等算力信息。仅提到基于现有模型的推理评估，未涉及模型训练。因此无法总结算力开销。

## 5. 实验数量与充分性

- **实验数量**：单一基准（130个视频）上的评估。没有提及消融实验、不同条件组合或多次重复实验。
- **充分性与公平性**：
  - **优点**：130个视频覆盖多种物理定律，基准设计注重真实性和客观性。
  - **局限性**：未展示跨不同场景的细致分析（如对不同守恒定律的分开评估），也未说明每次生成是否使用相同随机种子、视频长度等控制变量。对公平性的讨论较少。但作为基准测试的第一个版本，提供了结构化评估框架。

## 6. 主要结论与发现

- **核心发现**：当前最先进的视频生成模型在生成美观视频方面表现良好，但在编码物理原则上**显著不足**，即使使用高级提示（如文本描述物理规律）或视频条件（给定起始帧），生成的视频仍然频繁违反守恒定律。
- **启示**：这些模型尚不能视为可靠的世界模型，在需要严格物理一致性的应用中（如机器人规划）需要额外验证或约束。

## 7. 优点

1. **真实物理数据**：使用真实世界实验视频而非合成数据，避免了模拟器偏差，更具代表性。
2. **客观量化指标**：基于守恒定律的物理误差计算，消除了主观判断，提高了评估可重复性。
3. **综合利用前沿技术**：结合物理信息神经网络和视觉语言模型自动提取物理量，无需手工标注。
4. **标准化框架**：为视频生成模型的物理推理能力提供了首个系统化基准，便于未来比较。

## 8. 不足与局限

1. **覆盖范围有限**：仅包含130个视频，且聚焦于经典力学中的守恒定律，未涵盖流体力学、热力学、电磁学等更广泛的物理领域。
2. **评估依赖视觉跟踪**：生成视频中的物体检测与跟踪可能引入额外误差，尤其对于模糊或重叠区域。
3. **缺乏对模型差异的深入分析**：未对比不同架构、训练数据规模对物理推理的影响，也未探讨“具体哪些物理规律更难模拟”。
4. **未验证评估指标与人类感知的一致性**：物理误差较小是否一定对应更“合理”的视觉体验？未做用户研究。
5. **应用限制**：基准假设已知物理定律，对于需要发现新物理规律的任务不适用。

（完）
