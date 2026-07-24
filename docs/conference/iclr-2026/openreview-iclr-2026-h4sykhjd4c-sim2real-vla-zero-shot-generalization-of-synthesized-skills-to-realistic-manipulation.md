---
title: "Sim2Real VLA: Zero-Shot Generalization of Synthesized Skills to Realistic Manipulation"
title_zh: Sim2Real VLA：从合成技能到真实操作的零样本泛化
authors: "Runyi Zhao, Sheng Xu, Ruixing Jin, Yueci Deng, Yunxin Tai, Kui Jia, Guiliang Liu"
date: 2026-01-26
pdf: "https://openreview.net/pdf?id=H4SyKHjd4c"
tags: ["query:sr"]
score: 9.0
evidence: Sim2Real VLA模型实现零样本泛化
tldr: 本文提出Sim2Real-VLA，一种完全在合成数据上训练但能零样本迁移到真实操作任务的通用机器人控制模型。其双系统架构包括高层规划器推断以物体为中心的affordance链，结合仿真随机化实现无缝迁移。实验证明在多个真实场景中达到与真实数据训练模型相当的性能，显著缩小了Sim2Real差距。
source: ICLR-2026-Accepted
selection_source: conference_retrieval
motivation: 仿真到现实的差距导致合成数据训练的策略难以泛化到真实环境。
method: 提出双系统架构：高层规划器推断物体中心的affordance链，结合仿真随机化实现零样本迁移。
result: 在多个真实操作任务上零样本成功，性能与真实数据训练模型相当。
conclusion: Sim2Real-VLA证明了纯合成训练在机器人操作中的可行性，为数据获取提供了新途径。
---

## Abstract
Vision-Language-Action (VLA) models represent a critical milestone toward embodied intelligence in robotic manipulation. To support their training, recent research has developed high-performance simulation engines for data synthesis. However, their effectiveness is still significantly limited by the simulation-to-reality (Sim2Real) gap, as policies trained on synthetic data often fail to generalize reliably to the real world. To address this challenge, we present Sim2Real-VLA, a generalist robot control model trained exclusively on synthetic data, yet capable of transferring seamlessly to real-world manipulation tasks. Sim2Real-VLA features a dual-system architecture: a high-level planner that infers object-centered chains-of-affordances, and a low-level actor that executes and validates these plans in real time via a tokenized action space. This design filters out manipulation-irrelevant features and prioritizes motion-critical dynamics, thereby enhancing Sim2Real domain transfer. Besides, a notable advantage of Sim2Real-VLA lies in its tight integration with automated data generation for manipulation skills, eliminating the need for manual fine-tuning and enabling scalable, hands-free training. Empirical evaluations across bimanual, dexterous, and long-horizon tasks show that Sim2Real-VLA consistently outperforms previous VLA baselines under diverse real-world environments and domain shifts.

---

## 论文详细总结（自动生成）

# 论文总结：Sim2Real VLA：从合成技能到真实操作的零样本泛化

## 1. 核心问题与整体含义（研究动机和背景）
- **研究动机**：视觉-语言-动作（VLA）模型对机器人操作具身智能至关重要，但训练依赖合成数据。然而，由于仿真到现实（Sim2Real）差距的存在，纯合成数据训练的模型往往无法可靠地泛化到真实世界任务。
- **核心问题**：如何实现一种在完全合成数据上训练、却能零样本迁移到真实操作任务，且性能与真实数据训练模型相当的机器人控制模型。
- **整体意义**：该工作旨在缩小Sim2Real差距，为机器人操作数据获取提供可扩展、免手动的思路，无需依赖昂贵、耗时的人工真实数据采集。

## 2. 方法论
- **核心思想**：提出双系统架构（Dual-System Architecture），通过高层规划器与低层执行器协同，过滤掉与操作无关的特征，优先保留运动关键动态，增强Sim2Real域迁移。
- **关键技术细节**：
  - **高层规划器（High-Level Planner）**：推断以物体为中心的“affordance链”（chains-of-affordances），即分解任务为一系列可操作的原子步骤。
  - **低层执行器（Low-Level Actor）**：通过令牌化动作空间（tokenized action space）实时执行并验证高层规划。
  - **紧耦合自动化数据生成**：与合成数据生成流程深度集成，无需人工微调，实现可扩展、免手的训练。
- **算法流程（文字说明）**：
  1. 在仿真环境中自动生成多样化操作技能的合成数据。
  2. 使用这些数据训练双系统VLA模型：高层规划器学习从视觉输入预测affordance链；低层执行器学习在令牌化动作空间中生成具体动作。
  3. 推理时模型直接应用于真实机器人，无需再训练或域适应，实现零样本迁移。

## 3. 实验设计
- **使用场景/任务**：双操作（bimanual）、灵巧操作（dexterous）、长时域任务（long-horizon）等多种真实世界操作任务。
- **Benchmark**：与之前的VLA基线方法对比，在多样化真实环境和域偏移下进行评估。
- **对比方法**：论文中称为“previous VLA baselines”，具体名称未详细列出，但声称Sim2Real-VLA“一致优于”这些基线。

## 4. 资源与算力
- 论文摘要及元数据中**未明确说明**使用的GPU型号、数量、训练时长等算力信息。仅提及模型完全在合成数据上训练，但未给出训练成本细节。

## 5. 实验数量与充分性
- 实验涉及**多个真实操作场景**（双操作、灵巧操作、长时域），并测试了域偏移（domain shifts）下的鲁棒性。
- 文中提到“大量的实证评估”（Empirical evaluations），但未列出具体实验组数或消融实验数量。从现有信息看，实验覆盖了关键任务类型，但缺少详尽的消融研究或多数据集对比，充分性有限。
- **客观与公平性**：声称优于之前VLA基线，但没有提供基线方法的完整实现细节或统计显著性检验。若公开代码和完整实验配置，可进一步确认公平性。

## 6. 主要结论与发现
- Sim2Real-VLA在纯合成训练下，对多种真实操作任务实现了零样本成功，性能与真实数据训练的模型相当。
- 双系统架构和物体中心affordance链的设计有效缩小了Sim2Real差距。
- 该工作证明了纯合成训练在机器人操作中的可行性，为数据获取提供了新的可扩展途径。

## 7. 优点
- **零样本泛化能力**：完全在合成数据上训练即可泛化到真实世界，显著降低对真实数据的依赖。
- **双系统架构设计**：高层规划与低层执行分离，过滤无关特征，专注运动关键动态，提升迁移效果。
- **自动数据生成集成**：无需手动微调，可扩展，有利于大规模训练。
- **任务覆盖广泛**：涵盖双操作、灵巧操作和长时域任务，验证了方法的通用性。

## 8. 不足与局限
- **实验细节不充分**：未提供具体实验组数、消融研究、基线方法列表，难以全面评估方法的鲁棒性。
- **资源消耗未报告**：缺少训练所需的算力信息，限制了可复现性和实用性评估。
- **潜在偏差风险**：仅与“之前的VLA基线”比较，未与最新Sim2Real方法或真实数据微调模型对比，可能存在选择性比较。
- **应用限制**：依赖合成数据生成引擎的质量和多样性，若合成数据与真实环境分布差异过大仍可能失败。此外，仅测试了操作任务，泛化到其他机器人任务（如移动操作）未验证。
- **无公开代码或模型**：论文接受于ICLR 2026，但未提及开源计划，影响复现和社区贡献。

（完）
