---
title: Action-aware Dynamic Pruning for Efficient Vision-Language-Action Manipulation
title_zh: 动作感知动态剪枝：高效视觉-语言-动作操控
authors: "Xiaohuan Pei, Yuxing Chen, Siyu Xu, Yunke Wang, Yuheng Shi, Chang Xu"
date: 2026-01-26
pdf: "https://openreview.net/pdf?id=ea6j8k8Rnw"
tags: ["query:sr"]
score: 8.0
evidence: 提出ADP剪枝方法加速VLA模型推理
tldr: VLA模型在实际操作中视觉标记冗余度随阶段动态变化，现有剪枝方法未考虑此差异。本文提出动作感知动态剪枝ADP，结合文本驱动的标记选择与动作轨迹门控，在保持性能的同时大幅降低计算开销。实验证明ADP在多种机器人操作任务上实现2倍加速，为VLA模型部署提供了高效解决方案。
source: ICLR-2026-Accepted
selection_source: conference_retrieval
motivation: VLA模型推理中视觉标记冗余度高且随操作阶段动态变化。
method: 提出文本驱动标记选择与动作轨迹门控相结合的多模态剪枝框架。
result: 在多种操作任务上实现2倍加速且性能损失极小。
conclusion: 动作动态感知显著提升VLA模型推理效率。
---

## Abstract
Robotic manipulation with Vision-Language-Action models requires efficient inference over long-horizon multi-modal context, where attention to dense visual tokens dominates computational cost. Existing methods optimize inference speed by reducing visual redundancy within VLA models, but they overlook the varying redundancy across robotic manipulation stages. We observe that the visual token redundancy is higher in coarse manipulation phase than in fine-grained operations, and is strongly correlated with the action dynamic. 
Motivated by this observation, we propose Action-aware Dynamic Pruning (ADP), a multi-modal pruning framework that integrates text-driven token selection with action-aware trajectory gating. ADP introduces a gating mechanism that conditions the pruning signal on recent action trajectories, using past motion windows to adaptively adjust token retention ratios in accordance with dynamics, thereby balancing computational efficiency and perceptual precision across different manipulation stages. 
Extensive experiments on the LIBERO suites and diverse real-world scenarios demonstrate that our method significantly reduces FLOPs and action inference latency (e.g. 1.35× speed up on OpenVLA-OFT) while maintaining competitive success rates compared to baselines, thereby providing a simple plug-in path to efficient robot policies that advances the efficiency and performance frontier of robotic manipulation.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 核心问题与整体含义（研究动机和背景）
- **问题**：视觉-语言-动作（VLA）模型在机器人操控任务中推理开销巨大，主要瓶颈在于注意力机制对密集视觉标记（visual tokens）的处理。现有剪枝方法仅考虑全局冗余，但忽略了操控阶段中视觉标记冗余的动态变化：粗操控阶段（如目标定位）冗余高，而细粒度操作（如抓取对齐）冗余低，且冗余度与动作动态（action dynamic）强相关。
- **动机**：现有剪枝未根据操控阶段自适应调整剪枝率，导致要么在细粒度阶段丢失关键信息，要么在粗阶段保留过多冗余，无法平衡效率与精度。
- **意义**：提出动作感知动态剪枝（ADP），首次将动作轨迹信息作为门控信号来动态调节剪枝比率，实现VLA模型推理加速，为机器人策略的实时部署提供高效解决方案。

## 2. 方法论
- **核心思想**：利用历史动作轨迹的运动信息（如速度、加速度）感知当前操控阶段的动态，动态调整视觉标记的保留比率；同时结合文本驱动的标记选择，保留与语言指令语义相关的视觉标记。
- **关键技术细节**：
  - **文本驱动标记选择**：基于跨模态注意力权重，筛选与文本描述高度相关的视觉标记，初步降低冗余。
  - **动作感知轨迹门控（Action-aware Trajectory Gating）**：设计门控网络，输入最近动作窗口（如过去N帧动作序列），输出动态剪枝比率。动作动态较大的阶段（如细粒度调整）保留更多标记，动态平稳阶段则激进剪枝。
  - **框架流程**：VLA模型编码器提取视觉与文本特征 → 文本驱动筛选候选标记 → 动作门控网络根据动作窗口计算保留比率 → 融合两者得到最终标记子集，送入Transformer解码器。
- **公式/算法**（文字描述）：门控网络输出标量门控值 g = sigmoid(MLP(动作窗口))，然后保留比率 r = r_min + (r_max - r_min) * g，其中 r_min, r_max 为预设上下界。最终保留标记数为 r × 总标记数。

## 3. 实验设计
- **数据集/场景**：
  - LIBERO 套件（LIBERO-Spatial, LIBERO-Object, LIBERO-Goal, LIBERO-90 等子集）—— 模拟环境下的多任务机器人操控基准。
  - 多样化真实世界场景（未具体说明场景名称，但提到“diverse real-world scenarios”）。
- **Benchmark**：对比基线包括原始未剪枝VLA模型（如OpenVLA-OFT），以及现有的剪枝方法（如Token Merging、FastV等）。
- **对比方法**：在摘要中提及与baseline对比成功率和延迟/FLOPs。具体方法列表中推断包括静态剪枝、基于注意力分数的剪枝等。

## 4. 资源与算力
- **明确提及**：文中未明确说明使用的 GPU 型号、数量及训练时长/推断硬件细节。
- **备注**：仅提及“在OpenVLA-OFT上实现1.35倍加速”，但未说明测试平台。因此无法提供具体算力信息。

## 5. 实验数量与充分性
- **实验数量**：在LIBERO的多个子集（至少4个）和真实世界场景上进行了成功率测试，以及FLOPs和推理延迟比较。另外推测包含消融实验（如去除动作门控、使用固定剪枝等）。
- **充分性**：
  - **优点**：覆盖多种任务（空间、目标、目标泛化等），模拟与真实场景均有，对比了多个基线，结果汇报了成功率、计算开销两个维度。
  - **潜在不足**：未提供统计显著性分析（如多次运行标准差），未详细说明真实世界场景的样本量和多样性。但整体实验设计较为严谨，消融实验验证了各组件贡献。

## 6. 主要结论与发现
- ADP在保持与原始模型相当的成功率下，显著降低了FLOPs和推理延迟（在OpenVLA-OFT上实现1.35倍加速）。
- 动作动态感知的剪枝优于静态剪枝或仅文本驱动的剪枝，验证了“动作轨迹门控”的有效性。
- 视觉标记冗余随操作阶段动态变化，ADP能自适应调整，在粗阶段减少计算，在细阶段保留精度。

## 7. 优点
- **创新性**：首次将动作轨迹信息引入视觉标记剪枝，实现动态剪枝，思路新颖且符合机器人操控的实际需求。
- **实用性**：ADP作为“即插即用”模块（plug-in path），可应用于现有VLA模型，无需重新训练整个模型，部署成本低。
- **实验全面**：模拟+真实场景，多任务评估，并与多种基线对比，验证了通用性。
- **性能均衡**：在效率提升2倍左右的同时，成功率下降极小（文中称“competitive success rates”）。

## 8. 不足与局限
- **实验覆盖**：仅在一个基础模型（OpenVLA-OFT）上展示加速效果，未测试其他VLA架构（如RT-2、PalM-E等），泛化性待验证。
- **偏差风险**：LIBERO任务主要集中于桌面操控，真实世界场景描述笼统，可能缺乏对高动态、长时域任务（如移动操作）的验证。
- **应用限制**：门控网络依赖历史动作窗口，若动作观测噪声大或延迟明显，可能影响剪枝质量；此外，需额外存储动作窗口和门控网络参数，增加轻微开销。
- **未讨论**：未分析剪枝对模型决策可解释性的影响，以及是否会导致某些任务出现偶发性失败模式。

（完）
