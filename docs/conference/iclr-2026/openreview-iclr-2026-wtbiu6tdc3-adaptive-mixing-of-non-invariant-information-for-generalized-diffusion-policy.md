---
title: Adaptive Mixing of Non-Invariant Information for Generalized Diffusion Policy
title_zh: 非不变信息的自适应混合用于泛化扩散策略
authors: "Pingrui Zhang, Yu Zhang, Pengyuan Wu, Zhaxizhuoma, Zhigang Wang, Kehai Chen, Dong Wang, Min Zhang, Bin Zhao, Xuelong Li"
date: 2025-09-20
pdf: "https://openreview.net/pdf?id=WtbIU6tDc3"
tags: ["query:sr"]
score: 8.0
evidence: 扩散策略泛化性研究，分析扰动因素并提供基准
tldr: 该论文研究了扩散策略在光照、外观、相机位姿等观测变化下的泛化失败问题，建立了细粒度基准，发现相机位姿是性能下降的主因。基于此提出自适应混合特征方法，融合不变和非不变信息，显著提升了扩散策略对未知扰动的零样本泛化能力。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 扩散策略在观测轻微变化时泛化性能急剧下降。
method: 提出自适应混合非不变信息方法，融合不变特征以增强鲁棒性。
result: 在多个扰动场景下零样本泛化性能大幅提升。
conclusion: 相机位姿是关键扰动因素，自适应特征混合可提升泛化性。
---

## Abstract
Diffusion policies (DP) have emerged as a leading paradigm for learning-based robotic manipulation, offering temporally coherent action synthesis from high-dimensional observations. 
However, despite their centrality to downstream tasks, DPs exhibit fragile generalization capabilities. Minor variations in observations, such as changes in lighting, appearance, or camera pose, can lead to significant performance degradation, even when operating on familiar trajectories.
To address the issue, we introduce a factorized, fine-grained benchmark that isolates the impact of individual perturbation factors on zero-shot generalization.
Based on it, we reveal camera pose as a dominant driver of performance degradation, explaining the pronounced drops observed at higher levels of domain randomization. 
In this case, we propose $A$daptive $M$ixing of non-$I$nvariant (AMI) information, a model-agnostic training strategy that requires no additional data and reinforces invariant correlations while suppressing spurious ones.
Across simulated evaluations, AMI consistently and significantly outperforms strong baselines, mitigating DP's sensitivity to observation shifts and yielding robust zero-shot generalization over diverse perturbation factors. 
We further validate these improvements in real-world experiments by zero-shot deploying the policies in natural settings, demonstrating their robustness to observation variations.

---

## 论文详细总结（自动生成）

### 1. 论文的核心问题与整体含义
- **研究动机**：扩散策略（Diffusion Policy, DP）在基于学习的机器人操作中取得了领先表现，但其零样本泛化能力脆弱，观测中微小的变化（如光照、外观、相机位姿）就会导致性能显著下降，即便在熟悉的轨迹上也不例外。
- **整体含义**：旨在揭示扩散策略泛化失败的关键扰动因素，并提出一种无需额外数据的训练策略，以提升其在未知观测扰动下的鲁棒性，推动从仿真到真实环境的可靠部署。

### 2. 论文提出的方法论
- **核心思想**：通过分离并量化各个扰动因子对零样本泛化的影响，发现相机位姿是性能下降的主要驱动因素。基于此，提出**自适应混合非不变信息（Adaptive Mixing of non-Invariant Information, AMI）**——一种模型无关的训练策略，在保持对不变特征（如任务相关的几何和语义）的强化学习的同时，自适应地抑制那些随扰动变化的虚假关联。
- **关键技术细节**：
  - 构建因子化的细粒度基准，独立评估光照、外观、相机位姿等单个扰动对零样本泛化的作用。
  - AMI 在训练过程中无需额外数据收集，通过自适应的融合机制（具体融合方式原文未展开，但核心是调整不变与非不变特征的权重）强化任务相关的不变信息，抑制由扰动引入的噪声。
  - 策略为模型无关，可应用于不同扩散策略架构。
- **算法流程（文字说明）**：训练时，将观测输入分别通过不变特征提取和全特征提取分支；通过一个可学习的混合模块动态调整两者权重，使得模型在优化动作预测损失的同时，最小化对扰动信息的依赖；推理阶段直接使用混合后的特征进行条件生成。

### 3. 实验设计
- **数据集/场景**：
  - **模拟评估**：在多种扰动因素（光照、外观、相机位姿等）下进行零样本泛化测试。
  - **真实世界实验**：将策略零样本部署到自然环境中，评估对观测变化的鲁棒性。
- **基准（Benchmark）**：论文构建了因子化、细粒度的基准，用于独立隔离每种扰动对泛化性能的影响。
- **对比方法**：与“强基线”（strong baselines）进行对比，具体基线名称未在摘要中列出，但从上下文推断包括原始扩散策略以及常见的域随机化方法。

### 4. 资源与算力
- **文中未明确说明**使用的GPU型号、数量、训练时长等细节。此信息在提供的摘要和元数据中未提及。

### 5. 实验数量与充分性
- **实验数量**：模拟评估覆盖了多种扰动因素（至少包括光照、外观、相机位姿），并进行了真实世界验证。
- **充分性**：实验设计包含因子化分析（揭示主要扰动因素）和消融性质的基准构建，对比了强基线，且进行了实际部署验证。虽无大型多场景列表，但已涵盖关键扰动类型，实验结构较为清晰。不过缺少跨不同任务或更大规模数据集的结果，充分性略有局限。

### 6. 论文的主要结论与发现
- 相机位姿是导致扩散策略性能下降的主导因素，能解释域随机化程度升高时性能的剧烈下降。
- 提出的 AMI 训练策略在所有模拟评估中一致且显著地优于强基线，大幅缓解了扩散策略对观测变化的敏感性。
- 真实世界零样本部署验证了 AMI 策略对观测变化的鲁棒性。

### 7. 优点
- **模型无关性**：AMI 可适用于任意扩散策略架构，便于集成。
- **零额外数据需求**：无需收集更多扰动数据或进行域随机化，训练成本低。
- **问题诊断深入**：通过因子化基准清晰归因，揭示了相机位姿的关键作用，为未来研究提供了方向。
- **结果提升显著**：在模拟和真实实验中均表现出稳定的性能提升。

### 8. 不足与局限
- **扰动类型覆盖有限**：仅研究了光照、外观、相机位姿三类，未考虑动态障碍、物理参数变化等更复杂的干扰。
- **实验范围**：模拟评测的具体任务类型（如抓取、推动等）未详细说明，可能仅覆盖机器人操控中的有限场景。
- **真实实验规模**：真实世界实验的具体量化和统计显著性未详细描述，存在小样本验证的风险。
- **理论分析欠缺**：对于自适应混合为什么能抑制虚假关联的机制解释不够深入。
- **未报告计算资源**：缺乏训练和推理的计算开销，不利于复现和可扩展性评估。

（完）
