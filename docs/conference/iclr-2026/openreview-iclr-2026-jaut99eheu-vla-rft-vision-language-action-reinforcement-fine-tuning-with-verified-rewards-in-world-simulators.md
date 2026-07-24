---
title: "VLA-RFT: Vision-Language-Action Reinforcement Fine-Tuning with Verified Rewards in World Simulators"
title_zh: VLA-RFT：在世界模拟器中通过验证奖励进行视觉-语言-动作强化微调
authors: "Hengtao Li, Pengxiang Ding, Runze Suo, Zirui Ge, Dongyuan Zang, Kexian Yu, Mingyang Sun, Hongyin Zhang, Donglin Wang, Weihua Su"
date: 2025-09-19
pdf: "https://openreview.net/pdf?id=Jaut99EHeu"
tags: ["query:sr"]
score: 9.0
evidence: 利用世界模型作为模拟器的VLA强化微调
tldr: 针对VLA模型依赖模仿学习、缺乏鲁棒性的问题，提出VLA-RFT框架，利用从真实数据中训练的世界模型作为可控模拟器，提供密集轨迹奖励进行强化微调。该方法避免了真实交互成本，在多个机器人任务上提升了策略的鲁棒性和泛化能力，为VLA模型的RL训练提供了高效途径。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: VLA模型依赖模仿学习导致泛化差，而真实环境交互成本高。
method: 使用基于数据的世界模型作为模拟器，生成轨迹奖励进行强化微调。
result: 在多个任务上提升了VLA策略的鲁棒性和性能。
conclusion: 世界模型作为模拟器可有效支持VLA模型的RL训练。
---

## Abstract
Vision-Language-Action (VLA) models enable embodied decision-making but rely heavily on imitation learning, leading to compounding errors and poor robustness under distribution shift. Reinforcement learning (RL) can mitigate these issues yet typically demands costly real-world interactions or suffers from sim-to-real gaps. We introduce VLA-RFT, a Reinforcement Fine-Tuning framework that leverages a data-driven world model as a controllable simulator. Trained from real interaction data, the simulator predicts future visual observations conditioned on actions, allowing policy rollouts with dense, trajectory-level rewards derived from goal-achieving references. This design delivers an efficient and action-aligned learning signal, drastically lowering sample requirements. With fewer than 400 fine-tuning steps, VLA-RFT surpasses strong supervised baselines and achieves greater efficiency than simulator-based RL. Moreover, it exhibits strong robustness under perturbed conditions, sustaining stable task execution. Our results establish world-model-based RFT as a practical post-training paradigm to enhance the generalization and robustness of VLA models.

---

## 论文详细总结（自动生成）

# VLA-RFT 论文详细中文总结

## 1. 核心问题与整体含义（研究动机和背景）

- **研究背景**：视觉-语言-动作（Vision-Language-Action, VLA）模型在具身智能体中显示出潜力，能够根据视觉和语言指令生成动作。然而，当前VLA模型主要依赖模仿学习（behavioral cloning）进行训练，这会导致在分布偏移（distribution shift）时出现累积误差和鲁棒性差的问题。
- **核心问题**：如何在不依赖昂贵真实环境交互或避免sim-to-real差距的前提下，通过强化学习（RL）提升VLA模型的泛化能力和鲁棒性？真实环境RL交互成本极高，而基于模拟器的RL又存在仿真到现实的迁移鸿沟。
- **整体含义**：本文提出一种基于数据驱动世界模型作为可控模拟器的强化微调框架，旨在为VLA模型提供高效、低成本的后训练优化方案，增强其在扰动条件下的稳定执行能力。

## 2. 方法论：核心思想、关键技术细节、公式或算法流程

- **核心思想**：利用从真实交互数据中训练得到的**世界模型**作为模拟器，在该模拟器内进行策略rollout，并通过基于目标达成参考（goal-achieving references）的密集轨迹奖励（dense trajectory-level reward）进行强化微调（Reinforcement Fine-Tuning, RFT）。这种方法避免了真实环境交互成本，同时保留了与动作对齐的学习信号。
- **关键技术细节**：
  - 世界模型：基于真实机器人交互数据训练，能够根据当前视觉观察和动作序列预测未来视觉观测（future visual observations conditioned on actions）。该模型充当可控模拟器。
  - 奖励设计：使用与目标达成相关的密集轨迹奖励（例如与参考轨迹的距离或目标完成度），为每个时间步提供细粒度反馈。
  - 微调过程：使用强化学习（如PPO）在模拟器中对VLA策略进行微调，但实际训练时仅在模拟器内执行rollout并计算奖励，无需与真实环境交互。
  - 样本效率极高：需要**少于400步微调步骤**即可超越强监督基线，且比基于模拟器的RL方法更高效。
- **算法流程文字说明**：
  1. 收集真实机器人交互数据，训练一个世界模型（例如基于视频预测的扩散模型或Transformer）。
  2. 固定世界模型参数，将其作为模拟器。
  3. 初始化VLA策略（如基于预训练的VLAModel），在模拟器内进行多个轨迹的rollout，获取预测的未来视觉序列。
  4. 根据rollout结果与目标参考计算密集轨迹奖励（例如使用像素级差异、任务完成度等）。
  5. 使用RL算法（如PPO）更新VLA策略，重复步骤3-5直至收敛（≤400步）。
- **公式**：文中未提供具体公式，但核心可表示为：\(\max_{\pi} \mathbb{E}_{\tau \sim p_{\text{sim}}(\tau|\pi)}[R(\tau)]\)，其中 \(p_{\text{sim}}\) 由世界模型定义，\(R(\tau)\) 为密集轨迹奖励。

## 3. 实验设计

- **数据集与场景**：未在摘要中明确列举具体数据集名称，但提及在**多个机器人任务**上进行了评估。推测可能包括桌面操作、导航或抓取等标准任务（如CALVIN、MetaWorld、RLBench等常见benchmark，但原文未给出）。
- **基准（Benchmark）**：比较了强监督基线（如模仿学习训练后的VLA模型）以及基于模拟器的RL方法（可能指直接在模拟器中交互训练的RL基线）。
- **对比方法**：
  - 强监督基线（可能是Behavior Cloning或LM-based策略）。
  - 基于模拟器的RL方法（传统在模拟器中从头训练的RL策略）。
  - 可能还包括无微调的原始VLA模型。

## 4. 资源与算力

- **未明确说明**。文中未提及使用的GPU型号、数量、训练时长等具体算力信息。仅提到“少于400步微调步骤”，但未给出单步耗时或硬件配置。需要指出这一信息缺失。

## 5. 实验数量与充分性

- **实验数量**：据摘要描述，实验覆盖了**多个机器人任务**，并且进行了鲁棒性测试（扰动条件）。但具体任务数量、每组实验的重复次数、消融研究等未详细说明。
- **充分性评估**：
  - **优点**：展示了在多个任务上的性能提升，并单独验证了鲁棒性（扰动条件），具有一定说服力。
  - **不足**：缺乏消融实验的详细描述（例如是否对比了不同世界模型架构、不同奖励设计的影响？）。也未报告统计显著性或方差信息。由于论文被ICLR 2026拒稿（Rejected-Public），可能实验部分存在不充分之处。
- **客观性与公平性**：对比了强监督基线和模拟器RL基线，但未明确是否在相同计算资源下比较。奖励信号依赖于参考轨迹，可能存在对参考质量的依赖。

## 6. 主要结论与发现

- 提出的VLA-RFT框架能够以**极少的微调步数（<400步）**显著提升VLA策略的鲁棒性和泛化能力，在多个机器人任务上性能超越强监督基线。
- 基于数据驱动的世界模型作为模拟器，可以**有效支持VLA模型的强化学习训练**，规避真实交互成本和sim-to-real差距。
- 在扰动条件（如视觉噪声、目标位移）下，VLA-RFT策略仍能维持稳定执行，展示了良好的鲁棒性。
- 世界模型作为模拟器的RFT方式比传统基于模拟器的RL更高效（样本效率更高）。

## 7. 优点

- **创新性**：将数据驱动世界模型作为可控模拟器用于VLA模型的强化微调，避免了真实环境交互和sim-to-real问题。
- **高效性**：仅需不到400步微调即见显著提升，样本效率高，适合实际后训练部署。
- **动作对齐的密集奖励**：基于轨迹级别的奖励信号直接与动作序列相关，学习信号更精准。
- **鲁棒性提升**：通过RL优化分布外行为，增强了策略对扰动的抵抗能力。
- **实用性**：该方法可作为任何VLA模型的后训练微调范式，具有通用性。

## 8. 不足与局限

- **实验描述不充分**：缺少对任务具体定义、数据集、超参数、评估指标方差等细节的披露，使得可复现性存疑。
- **算力资源未说明**：未提及GPU型号、数量、训练时间，无法评估实际成本。
- **世界模型依赖性**：方法依赖于高质量的世界模型，若世界模型预测不准确，可能引入偏差。且世界模型的泛化能力尚未评估。
- **奖励工程依赖**：密集轨迹奖励需要设计合理的goal-achieving参考，可能在不同任务上需要人工调整。
- **应用限制**：仅在机器人任务上验证，未探索其他具身场景（如驾驶、人机交互）。此外，世界模型的训练仍需要真实数据，对数据量要求可能不低。
- **拒稿背景**：论文被ICLR 2026拒稿，可能存在方法论或实验上的重大缺陷未被本文本体现。

（完）
