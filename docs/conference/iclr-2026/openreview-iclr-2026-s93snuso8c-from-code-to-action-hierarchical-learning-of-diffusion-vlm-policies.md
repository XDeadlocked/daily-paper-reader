---
title: "From Code to Action: Hierarchical Learning of Diffusion-VLM Policies"
title_zh: 从代码到动作：扩散-视觉语言模型策略的层次化学习
authors: "Markus Peschl, Pietro Mazzaglia, Daniel Dijkman"
date: 2025-09-19
pdf: "https://openreview.net/pdf?id=S93SnUsO8c"
tags: ["query:sr"]
score: 9.0
evidence: 结合VLM和扩散策略进行机器人操作
tldr: 该论文提出层次化模仿学习框架，利用代码生成型视觉语言模型将任务分解为可执行的子程序，再由低层扩散策略执行。通过将开源机器人API作为结构化监督，解决了长期任务中的泛化和数据稀缺问题。实验表明该方法能有效泛化复杂操作行为。
source: ICLR-2026-Public
selection_source: conference_retrieval
motivation: 机器人模仿学习在长期任务中泛化能力差且数据稀缺。
method: 提出层次化框架，高层VLM分解任务为子程序，低层扩散策略执行具体动作。
result: 在复杂操作任务中验证了良好的泛化和效率。
conclusion: 结合VLM和扩散策略可实现有效的层次化机器人学习。
---

## Abstract
Imitation learning for robotic manipulation often suffers from limited generalization and data scarcity, especially in complex, long-horizon tasks. In this work, we introduce a hierarchical framework that leverages code-generating vision-language models (VLMs) in combination with low-level diffusion policies to effectively imitate and generalize robotic behavior. Our key insight is to treat open-source robotic APIs not only as execution interfaces but also as sources of structured supervision: the associated subtask functions - when exposed - can serve as modular, semantically meaningful labels. We train a VLM to decompose task descriptions into executable subroutines, which are then grounded through a diffusion policy trained to imitate the corresponding robot behavior. To handle the non-Markovian nature of both code execution and certain real-world tasks, such as object swapping, our architecture incorporates a memory mechanism that maintains subtask context across time. We find that this design enables interpretable policy decomposition, improves generalization when compared to flat policies and enables separate evaluation of high-level planning and low-level control.

---

## 论文详细总结（自动生成）

# 详细中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **研究动机**：机器人模仿学习在复杂、长期任务中面临泛化能力差与数据稀缺问题。传统端到端策略难以处理长时序依赖和组合性操作，而纯高层规划又缺乏对低层动作的精细控制。
- **背景**：视觉语言模型（VLM）在理解和生成代码方面表现出色，扩散策略在机器人低层动作生成上表现优异；但如何将两者有机整合以提升长期任务泛化性能仍是一个开放挑战。

## 2. 论文提出的方法论

- **核心思想**：提出层次化模仿学习框架（Hierarchical Diffusion-VLM Policy），将开源机器人API既作为执行接口，也作为结构化监督来源——API暴露的子任务函数可以作为模块化、语义明确的标签。
- **关键技术细节**：
  - **高层VLM**：训练代码生成型VLM将任务描述分解为可执行的子程序（subroutines）。这些子程序对应API中的函数，形成语义化的动作序列。
  - **低层扩散策略**：训练扩散模型模仿对应的机器人行为，将高层子程序接地（ground）为连续动作。
  - **记忆机制**：为处理代码执行和现实任务（如物体交换）中的非马尔可夫性，架构引入跨时间步维护子任务上下文的内存模块。
- **流程**：输入任务指令 → VLM生成子程序序列 → 扩散策略依次执行每个子程序对应的动作 → 记忆模块跟踪状态，确保时序一致性。

## 3. 实验设计

- **数据集/场景**：根据摘要和元数据，实验是在**复杂操作任务**上进行的，具体场景未明确列举。推测可能涉及长期任务如物体搬运、交换等。
- **Benchmark**：未明确说明具体benchmark名称。但对比了**平坦策略（flat policies）**，即无层次结构的端到端方法。
- **对比方法**：主要对比了不带层次分解的平坦策略。未提及与其他层次化方法（如HBC、Temporal Abstraction）的直接对比。

## 4. 资源与算力

- 论文中**未明确说明**使用的GPU型号、数量、训练时长等算力信息。元数据和摘要中均无相关细节。

## 5. 实验数量与充分性

- 根据现有信息，实验组数不详。摘要仅提及“在复杂操作任务中验证”，但**无具体实验数量、消融实验、统计结果**。
- **充分性评估**：由于缺乏量化实验细节（如成功率、任务数、方差等），难以判断实验是否充分、客观、公平。存在**数据泄露风险**：未报告训练/测试集划分、泛化域外任务的具体指标。

## 6. 论文的主要结论与发现

- 层次化框架（VLM + 扩散策略）能实现**可解释的策略分解**，高层VLM生成子程序，低层扩散策略执行具体动作。
- 相比平坦策略，该方法在长期任务上**改进了泛化能力**，并能**分别评估高层规划与低层控制的性能**。
- 记忆机制对处理非马尔可夫性至关重要。

## 7. 优点

- **方法论创新**：将开源机器人API作为结构化监督，利用子任务函数作为标签，降低了对大量人工标注的需求。
- **模块化设计**：高层VLM与低层扩散策略解耦，便于独立调试和评估。
- **可解释性**：高层输出的子程序序列可被人类理解，有助于分析错误原因。

## 8. 不足与局限

- **实验细节缺失**：未报告具体任务设置、成功率、消融研究等定量结果，结论的可重复性存疑。
- **泛化范围未量化**：仅提及“改进泛化”，但未说明对未见任务、未见物体、场景布局变化的泛化边界。
- **应用限制**：依赖开源机器人API的可用性，若API不完整或未暴露子任务函数，框架可能失效。
- **资源需求**：未讨论VLM微调与扩散策略训练的算力开销，可能存在高昂成本。
- **偏差风险**：可能仅在特定机器人环境（如MetaWorld、Franka等）上验证，未见跨平台实验。

（完）
