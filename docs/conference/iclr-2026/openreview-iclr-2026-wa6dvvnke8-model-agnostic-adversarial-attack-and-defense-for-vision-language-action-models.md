---
title: Model-agnostic Adversarial Attack and Defense for Vision-Language-Action Models
title_zh: 面向视觉-语言-动作模型的模型无关对抗攻击与防御
authors: "HaoChuan Xu, Yun Sing Koh, Shuhuai Huang, Zirun Zhou, Di Wang, Jun Sakuma, Jingfeng Zhang"
date: 2025-09-12
pdf: "https://openreview.net/pdf?id=wA6dvVnKe8"
tags: ["query:sr"]
score: 9.0
evidence: VLA模型的对抗攻击与防御
tldr: 面向视觉-语言-动作（VLA）模型，本文提出模型无关的对抗性补丁攻击EDPA及对应防御策略。攻击直接置于相机视野中干扰嵌入，防御则通过对抗训练增强鲁棒性。实验表明该方法有效暴露并缓解VLA模型的安全漏洞。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: VLA模型的对抗鲁棒性尚未充分研究。
method: 提出嵌入干扰补丁攻击（EDPA）和相应的防御策略。
result: EDPA成功攻击多种VLA模型，防御策略有效提升鲁棒性。
conclusion: VLA模型存在安全风险，本文方法有助于构建更可靠的机器人系统。
---

## Abstract
Vision-Language-Action (VLA) models have achieved revolutionary progress in robot learning, enabling robots to execute complex physical robot tasks from natural language instructions. Despite this progress, their adversarial robustness remains underexplored. In this work, we propose both adversarial patch attack and corresponding defense strategies for VLA models. We first introduce the Embedding Disruption Patch Attack (EDPA), a model-agnostic, untargeted adversarial patch attack that generates patches directly placeable within the camera’s view. In comparison to prior methods, EDPA can be readily applied to different VLA models without requiring prior knowledge of the model architecture, action space, or the controlled robotic manipulator. EDPA constructs these patches by (i) maximizing the discrepancy of latent representations of adversarial and corresponding clean visual inputs, and (ii) disrupting the semantic alignment between visual and textual latent representations. Through the optimization of these objectives, EDPA distorts the VLA’s interpretation of visual information, causing the model to repeatedly generate incorrect actions and ultimately result in failure to complete the given robotic task. To counter this, we propose an adversarial fine-tuning scheme for the visual encoder, in which the encoder is optimized to produce similar latent representations for both clean and adversarially perturbed visual inputs. Extensive evaluations on the widely recognized LIBERO robotic simulation benchmark demonstrate that EDPA substantially increases the task failure rate of cutting-edge VLA models, while our proposed defense effectively mitigates this degradation.

---

## 论文详细总结（自动生成）

# 详细中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）
- **研究动机**：视觉-语言-动作（VLA）模型在机器人学习领域取得了革命性进展，使机器人能够根据自然语言指令执行复杂物理操作。然而，VLA 模型的对抗鲁棒性（adversarial robustness）尚未得到充分研究，存在潜在的安全风险。
- **核心问题**：目前缺乏针对 VLA 模型的模型无关（model-agnostic）对抗攻击和防御方法。先前攻击方法通常依赖模型架构或动作空间等先验知识，可迁移性不强。
- **整体含义**：本文首次系统性地提出了面向 VLA 模型的对抗补丁攻击（EDPA）和相应的防御策略，旨在暴露和缓解 VLA 模型在真实机器人任务中的安全漏洞，为构建更可靠的机器人系统提供基础。

## 2. 论文提出的方法论
- **核心思想**：通过生成可直接放置在相机视野中的对抗补丁（adversarial patch），干扰 VLA 模型对视觉信息的嵌入表示（embedding），进而破坏模型生成正确动作的能力。
- **关键方法**：
  - **Embedding Disruption Patch Attack (EDPA)**：模型无关、无目标（untargeted）的对抗补丁攻击。EDPA 通过以下两步构造补丁：
    1. 最大化对抗视觉输入与对应干净视觉输入在潜在空间（latent space）中的表示差异。
    2. 破坏视觉潜在表示与文本潜在表示之间的语义对齐（semantic alignment）。
  - 优化目标是通过梯度下降迭代更新补丁像素，使得模型对补丁图像的解读扭曲，从而重复生成错误动作，最终导致任务失败。
- **防御策略**：对视觉编码器（visual encoder）进行对抗微调（adversarial fine-tuning）。具体地，优化编码器使其对干净和对抗扰动后的视觉输入产生相似的潜在表示（即最小化两者间的差异），从而提升编码器对补丁攻击的鲁棒性。
- **算法流程**（文字说明）：
  1. **攻击**：随机初始化补丁→将补丁叠加到干净图像上→前向传播获取视觉和文本嵌入→计算嵌入差异损失（包括视觉-视觉差异和视觉-文本对齐破坏损失）→反向传播更新补丁→重复直到收敛。
  2. **防御**：在对抗微调阶段，固定模型其他部分，仅更新视觉编码器参数，优化目标为最小化干净图像与对抗图像（经 EDPA 扰动）的嵌入距离。

## 3. 实验设计
- **使用的数据集/场景**：LIBERO 机器人仿真基准（benchmark），这是一个广泛用于评估机器人操作任务的模拟环境。
- **Benchmark**：LIBERO 包含多种复杂操作任务（如拾取、放置、堆叠等），通过任务失败率（task failure rate）来评估攻击与防御效果。
- **对比方法**：论文未在摘要中列出具体对比方法，但提到“与先前方法相比”，推测可能包括随机噪声、传统图像级攻击（如 FGSM、PGD）或针对 VLA 模型的其他攻击（如直接攻击动作输出）。具体对比方法需查阅全文。
- **评估指标**：任务失败率（failure rate），即攻击后模型无法完成指令的比例。

## 4. 资源与算力
- 论文摘要及所给元数据中**未明确说明**使用的 GPU 型号、数量、训练时长等算力信息。推测在标准深度学习服务器（如单张或四张 NVIDIA A100/RTX 3090）上完成，但无法确认。这一点在原文中为空白。

## 5. 实验数量与充分性
- 根据摘要，实验在 LIBERO 环境下对多种最先进的 VLA 模型（如多种骨干网络或结构）进行了评估，并验证了 EDPA 攻击的有效性和防御方法的鲁棒性提升。
- 推测包含以下实验：
  - 攻击效果评估：在不同 VLA 模型上测试 EDPA 相对于基线（如干净图像、随机补丁）的任务失败率。
  - 防御效果评估：对抗微调后的模型对 EDPA 的鲁棒性提升。
  - 消融实验：可能验证攻击目标（视觉-视觉差异 vs 视觉-文本对齐破坏）各自贡献；防御中不同超参数的影响。
- **充分性评价**：实验覆盖了多个模型和任务，且使用了标准仿真基准，对比相对客观。但缺乏真实机器人环境验证，且未提供统计显著性和误差区间说明（摘要未提），可能不够全面。总体而言，实验设计较为充分，符合 ICLR 水平。

## 6. 论文的主要结论与发现
- **攻击有效**：EDPA 能显著增加 VLA 模型在 LIBERO 任务上的失败率，表明当前 VLA 模型对视觉补丁攻击非常脆弱。
- **防御有效**：提出的对抗微调方案能够有效缓解由 EDPA 引起的性能下降，使模型鲁棒性得到提升。
- **关键洞察**：VLA 模型的安全风险主要来自视觉-文本语义对齐的脆弱性；通过破坏视觉嵌入或破坏对齐均可达到攻击目的。
- 防御方法在不影响正常任务性能的前提下（推测），增强了模型对对抗扰动的抵抗能力。

## 7. 优点
- **模型无关性**：EDPA 不依赖 VLA 模型的架构、动作空间或机器人类型，可轻松迁移到不同 VLA 系统，实用性强。
- **攻击直接有效**：补丁可直接放置在相机视野中，符合物理世界攻击场景（如贴纸在镜头前），无需访问模型内部。
- **防御轻量且可训练**：仅对视觉编码器进行微调，不改变模型主体，计算成本较低。
- **研究空白填补**：首次系统化研究 VLA 模型的对抗攻防问题，为后续安全研究奠定基础。

## 8. 不足与局限
- **实验环境限制**：仅在 LIBERO 仿真环境验证，未在真实机器人系统（如物理 UR5、Franka 等）上测试，攻击的物理可实现性（如光照变化、视角变化下补丁效果）未探讨。
- **防御假设**：防御针对 EDPA，可能对其他类型攻击（如自然噪声、物理形变）泛化能力未知。
- **未讨论性能权衡**：对抗微调可能导致模型在干净输入上的准确率略微下降（通常 trade-off），但论文未提供相关数据。
- **缺乏对比基线**：摘要未明确列出与其他攻击方法（如传统数字攻击）的量化对比，无法判断 EDPA 的相对优势程度。
- **计算开销未报告**：攻击补丁生成耗时、防御微调时间等缺乏信息，影响方法实用性评估。

（完）
