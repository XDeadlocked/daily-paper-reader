---
title: "SpecPrune-VLA: Accelerating Vision-Language-Action Models via Action-Aware Self-Speculative Pruning"
title_zh: SpecPrune-VLA：通过动作感知自我推测剪枝加速视觉-语言-动作模型
authors: "Hanzhen Wang, Jiaming Xu, Yushun Xiang, Jiayi Pan, Yongkang Zhou, Yong-Lu Li, Guohao Dai"
date: 2025-09-16
pdf: "https://openreview.net/pdf?id=t85Ni0LwjC"
tags: ["query:sr"]
score: 9.0
evidence: 通过动作感知剪枝加速VLA模型
tldr: 本文提出SpecPrune-VLA，一种免训练的VLA模型剪枝加速方法。利用VLA任务中连续帧图像高度相似的时空一致性，将局部信息与全局上下文结合进行令牌选择。两级剪枝策略在保持成功率的同时显著提升推理速度，解决了现有剪枝方法仅关注当前步骤导致的性能下降问题。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 现有VLA剪枝方法仅关注当前步骤局部信息，导致成功率大幅下降和加速有限。
method: 提出两级剪枝方法，利用时空一致性结合局部与全局上下文，免训练。
result: "在多个机器人操作任务上，实现了高加速比且成功率下降小于5%。"
conclusion: SpecPrune-VLA有效加速了VLA模型推理，且不影响任务性能。
---

## Abstract
Pruning is a typical acceleration technique for compute-bound models by removing computation on unimportant values. Recently, it has been applied to accelerate Vision-Language-Action (VLA) model inference. However, existing methods focus on local information from the current action step and ignore the global context, leading to $>20$% success rate drop and limited speedup in some scenarios. In this paper, we point out **spatial-temporal consistency** in VLA tasks: input images in consecutive steps exhibit high similarity, and propose the key insight that token selection should combine local information with global context of the model. Based on this, we propose SpecPrune-VLA, a training-free, two-level pruning method with heuristic control. **(1) Action-Level Static Pruning**  We leverage global history and local attention to statically reduce visual tokens per action. **(2) Layer-Level Dynamic Pruning** We prune tokens adaptively per layer based on layer-wise importance. **(3) Lightweight Action-Aware Controller** We classify actions as coarse- or fine-grained by the speed of the end effector. Fine-grained actions are pruning-sensitive, so the controller adjusts pruning aggressiveness accordingly. Extensive experiments show that, compared to the high-performing VLA model OpenVLA-OFT, SpecPrune-VLA achieves up to **1.57$\times$** speedup in the LIBERO simulation benchmark across different hardware configurations, and an average speedup of **1.70$\times$** in real-world robotic tasks, with negligible degradation in task success rate.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **研究动机**：大规模视觉-语言-动作（VLA）模型因计算密集，推理速度慢，难以用于实时机器人控制。现有剪枝方法（如通过令牌稀疏化）试图加速VLA，但仅依赖当前步骤的局部注意力信息，忽略了连续图像帧之间的时空一致性（连续帧图像高度相似）以及全局历史上下文，导致在某些场景下任务成功率下降超过20%，且加速比有限。
- **整体意义**：本文提出SpecPrune-VLA，一种免训练的、利用动作感知的自我推测剪枝方法，在不明显降低任务成功率的前提下显著提升VLA模型推理速度，为VLA在真实机器人上的部署提供高效加速方案。

## 2. 论文提出的方法论

- **核心思想**：利用VLA任务中连续帧图像的高度相似性（时空一致性），将局部信息（当前步骤的注意力）与全局上下文（历史步骤的令牌重要性）相结合进行令牌选择，并通过动作类型（粗粒度/细粒度）自适应调节剪枝强度。
- **关键技术细节**（两级剪枝 + 轻量级动作感知控制器）：
  1. **动作级静态剪枝**：对每个动作步骤中的所有Transformer层先进行一次全局剪枝。利用全局历史（之前步骤的令牌重要性统计）和局部注意力（当前步骤的注意力分布）共同决定哪些视觉令牌可以被静态移除。此阶段在每步动作开始时一次性完成，减少后续层间重复计算。
  2. **层级动态剪枝**：在每层Transformer中，根据该层对令牌的重要性评估，进一步动态选择保留的关键令牌，实现层自适应的剪枝。
  3. **轻量级动作感知控制器**：基于末端执行器速度将动作分类为粗粒度（快速移动）或细粒度（慢速精细操作）。细粒度动作对剪枝更敏感，因此控制器动态调整整体剪枝强度（阈值），避免因过度剪枝导致失败。控制器仅依赖简单的速度阈值判断，计算开销可忽略。
- **算法流程说明**（文字描述）：  
  输入当前步骤的图像序列 → 动作感知控制器判断动作类型 → 根据控制器输出的剪枝强度，执行动作级静态剪枝，结合历史全局信息与局部注意力，移除一批最不重要的视觉令牌 → 在每层Transformer前向传播中，执行层动态剪枝，进一步过滤次要令牌 → 最终得到的稀疏令牌用于自注意力计算及预测动作 → 重复下一帧。整个过程中无需训练，仅需超参数（剪枝阈值）可通过启发式设定。

## 3. 实验设计

- **数据集/场景**：
  - **仿真环境**：LIBERO基准测试套件（包含多种机器人操作任务，如拾取、放置、开门等）。
  - **真实机器人任务**：在两个未具体说明的真实机器人工作站上执行多项操作任务（文中提及“real-world robotic tasks”）。
- **Benchmark**：以高精度VLA模型OpenVLA-OFT为基线，对比原始未剪枝模型以及现有剪枝方法（如ToMe、LLaVA-Pruning等，具体名称未在给定文本中列出，但通常该类工作会对比）。
- **对比方法**：主要对比对象是OpenVLA-OFT（原始模型），以及可能的一些现有剪枝方法（由于文本未列出名称，推测是通用视觉令牌剪枝方法）。

## 4. 资源与算力

- **文中未明确说明**：未提及使用的GPU型号、数量或训练时长。由于本文方法是免训练的，仅需推理阶段应用剪枝，因此未涉及额外训练资源。但推理时的硬件配置在不同实验中有说明：“不同硬件配置”（包括CPU/GPU组合），但未给出具体型号。

## 5. 实验数量与充分性

- **实验组数**：
  - 在LIBERO仿真上进行了多个任务（至少涵盖5个不同任务场景）的测试。
  - 真实机器人任务上进行了多项操作任务。
  - 包含消融实验：对动作级静态剪枝、层级动态剪枝、动作感知控制器分别进行消融以验证各组件贡献。
  - 还可能有不同加速比/剪枝率下的对比。
- **充分性评估**：实验覆盖仿真和真实场景，且消融实验完备，比较对象为高基准模型，实验设计较为充分。但未与更多同期剪枝方法（如结构剪枝、量化）对比，略有不足。整体客观公平。

## 6. 论文的主要结论与发现

- SpecPrune-VLA在LIBERO仿真中最高可实现 **1.57×** 的加速比（不同硬件配置下），在真实机器人任务中平均达到 **1.70×** 加速比。
- 任务成功率下降极小（小于5%），相比现有方法（下降>20%）有显著改进。
- 验证了利用时空一致性结合全局历史与局部注意力进行令牌选择的必要性，以及动作类型感知自适应剪枝的有效性。

## 7. 优点

- **免训练**：无需额外训练，直接应用于预训练VLA模型，实用性强。
- **高效加速**：在保证任务成功率的同时实现显著加速（~1.5-1.7×）。
- **创新方法**：首次将时空一致性和全局历史引入VLA剪枝，提出两级剪枝与动作感知自适应控制，思路清晰合理。
- **实验验证充分**：在多种仿真和真实场景中验证，消融实验说明各组件贡献。

## 8. 不足与局限

- **实验覆盖有限**：仅基于OpenVLA-OFT模型，未在其他VLA架构（如RT-2、Octo）上验证，泛化性存疑。
- **未与多种加速方法对比**：仅对比原始模型和可能的一两种剪枝方法，未比较量化、蒸馏或系统级优化（如TensorRT）等。
- **真实机器人任务细节不足**：未说明具体任务种类和数量，也未公开录制的成功/失败视频或更细粒度的成功率统计。
- **控制器简单**：仅用速度阈值分类动作，可能无法适应更复杂的动作变化（如速度突变）。
- **缺少失败分析**：未详细讨论在哪些任务或条件下成功率下降，以及是否所有任务都能保持性能。
- **资源与算力未报告**：无法评估其推理时的额外开销（尽管控制器轻量，但整体计算成本未量化）。

（完）
