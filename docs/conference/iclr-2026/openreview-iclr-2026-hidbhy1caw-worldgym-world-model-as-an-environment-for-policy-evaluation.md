---
title: "WorldGym: World Model as An Environment for Policy Evaluation"
title_zh: WorldGym：世界模型作为策略评估环境
authors: "Julian Hector Quevedo, Ansh Kumar Sharma, Yixiang Sun, Varad Suryavanshi, Percy Liang, Sherry Yang"
date: 2026-01-26
pdf: "https://openreview.net/pdf?id=hidBHy1CAw"
tags: ["query:sr"]
score: 9.0
evidence: 将世界模型作为策略评估的环境
tldr: 本文提出WorldGym，一种基于世界模型的策略评估环境。世界模型是自回归的、以动作为条件的视频生成模型，作为真实环境的代理。策略通过在世界模型中进行蒙特卡罗 rollout 评估，并由视觉语言模型提供奖励。在多个真实机器人VLA策略上的实验表明，世界模型中的成功率与真实世界成功率高度相关，且能保持策略的相对排名。这为低成本、可扩展的策略评估提供了新方案。
source: ICLR-2026-Accepted
selection_source: conference_retrieval
motivation: 真实机器人策略评估成本高，传统仿真器缺乏真实感和通用性。
method: 提出WorldGym，利用以动作为条件的视频生成世界模型进行蒙特卡罗 rollout，并用VLM提供奖励。
result: 世界模型中的策略成功率与真实世界高度相关，且保持排名一致性。
conclusion: WorldGym为机器人策略评估提供了低成本且可靠的替代方案。
---

## Abstract
Evaluating robot control policies is difficult: real-world testing is costly, and handcrafted simulators require manual effort to improve in realism and generality. We propose a world-model-based policy evaluation environment (WorldGym), an autoregressive, action-conditioned video generation model which serves as a proxy to real world environments. Policies are evaluated via Monte Carlo rollouts in the world model, with a vision-language model providing rewards. We evaluate a set of VLA-based real-robot policies in the world model using only initial frames from real robots, and show that policy success rates within the world model highly correlate with real-world success rates. Moreoever, we show that WorldGym is able to preserve relative policy rankings across different policy versions, sizes, and training checkpoints. Due to requiring only a single start frame as input, the world model further enables efficient evaluation of robot policies' generalization ability on novel tasks and environments. We find that modern VLA-based robot policies still struggle to distinguish object shapes and can become distracted by adversarial facades of objects. While generating highly realistic object interaction remains challenging, WorldGym faithfully emulates robot motions and offers a practical starting point for safe and reproducible policy evaluation before deployment.

---

## 论文详细总结（自动生成）

### 论文详细中文总结

#### 1. 核心问题与整体含义（研究动机和背景）
- **核心问题**：机器人控制策略的评估非常困难——真实世界测试成本高昂，而手工搭建的仿真器在真实感和通用性上需要大量人工投入。
- **研究动机**：需要一种低成本、可扩展且能反映真实性能的策略评估方法，以替代昂贵且耗时的真实机器人部署和手工仿真器。
- **背景**：现有评估手段要么依赖真实硬件（代价高、不可重复），要么使用传统仿真器（缺乏真实感、难以泛化）。随着视觉-语言-动作（VLA）策略的发展，亟需一种既能模拟真实环境又能自动提供奖励的评估平台。

#### 2. 提出的方法论：核心思想、关键技术细节
- **核心思想**：利用世界模型（自回归、以动作为条件的视频生成模型）作为真实环境的代理，通过在该世界模型中进行蒙特卡罗 rollout 来评估策略，并由视觉语言模型（VLM）自动提供奖励信号。
- **关键技术细节**：
  - **世界模型**：自回归视频生成模型，以初始帧和动作序列为条件，预测后续帧。作为真实环境的低成本替代。
  - **策略评估流程**：给定一个初始帧（真实机器人单帧图像），策略在世界模型中生成动作序列；世界模型根据动作生成对应视频 rollout；VLM 对 rollout 结果进行打分（如任务是否成功），作为奖励。
  - **无需真实交互**：只依赖单张初始帧，即可评估策略在不同任务和环境上的泛化能力。
- **算法流程**（文字说明）：
  1. 收集真实机器人单帧初始图像。
  2. 目标策略根据当前世界模型状态输出动作。
  3. 世界模型根据动作生成下一帧，重复直到 rollout 结束。
  4. VLM 对最终视频序列进行成功/失败判定。
  5. 统计多轮 rollout 的成功率作为策略性能估计。

#### 3. 实验设计：数据集、场景、基准与对比方法
- **数据集/场景**：使用真实机器人初始帧（未明确说明具体数据集，但涉及多种机器人任务）。
- **基准**：以真实世界部署的成功率为 ground truth。
- **对比方法**：评估了多种基于 VLA 的真实机器人策略，包括不同版本、不同规模及不同训练检查点。主要对比世界模型中的成功率与真实世界成功率之间的相关性。
- **实验设置**：仅使用初始帧输入，在世界模型中评估策略，并与真实环境部署结果比较。

#### 4. 资源与算力
- 文中未明确提及使用的 GPU 型号、数量和训练时长。仅指出世界模型是预训练的视频生成模型，未说明训练该世界模型所需的算力。因此**资源与算力信息缺失**。

#### 5. 实验数量与充分性
- **实验数量**：评估了多个 VLA 策略（不同版本、大小、检查点），但具体数量未详述。主要对比指标为成功率相关性及排名保持能力。
- **充分性**：实验覆盖了策略变体、规模、训练阶段，并验证了世界模型与真实世界的成功率高度相关且能保持相对排序，初步证明了方法的有效性。但**实验场景有限**（仅基于初始帧的若干任务），未在多种机器人形态、复杂动态交互或长时间任务上验证，因此充分性仍有限。
- **客观性与公平性**：使用真实世界部署作为对比基准，方法相对客观；但世界模型自身可能存在偏差，未进行与其他仿真器（如 MuJoCo、Isaac Sim）的对比，公平性有待加强。

#### 6. 主要结论与发现
- 世界模型中的策略成功率与真实世界成功率高度相关，能够作为真实评估的有效代理。
- WorldGym 能够保持不同策略版本、规模、检查点之间的相对排名，说明其排序能力可靠。
- 现代 VLA 策略仍存在局限：难以区分物体形状，易被物体对抗性外观（adversarial facades）干扰。
- 生成高度真实的物体交互仍具有挑战性，但 WorldGym 能忠实模拟机器人运动，为部署前的安全、可重复评估提供实用起点。

#### 7. 优点：方法与实验设计亮点
- **低成本**：无需真实机器人部署和手工设计仿真器，仅需一张初始帧。
- **可扩展**：能自然评估策略在新任务/环境上的泛化能力。
- **自动化奖励**：VLM 自动判断任务成功，消除了手动标注成本。
- **相关性高**：与真实世界成功率高度相关，且保持排名一致性，具有实际应用价值。
- **实验设计合理**：对比不同版本、大小、检查点，覆盖了策略演化的关键维度。

#### 8. 不足与局限
- **真实感不足**：世界模型在生成高度逼真的物体交互时仍困难（如对物体形状的区分、对抗性外观混淆），可能影响评估准确性。
- **场景覆盖有限**：仅在基于单初始帧的任务中验证，未涵盖更复杂的长期任务、多步骤任务或需要物理接触的精细操作。
- **世界模型依赖**：评估质量受限于世界模型的保真度；若世界模型有系统性偏差，可能导致错误评估。
- **缺乏标准基准对比**：未与传统强化学习仿真器（如 MuJoCo、Habitat）或基于物理的模拟器进行性能比较，难以评估其相对优势。
- **算力成本未说明**：训练世界模型和运行 VLM 奖励的计算开销未知，可能仍需要一定资源。

（完）
