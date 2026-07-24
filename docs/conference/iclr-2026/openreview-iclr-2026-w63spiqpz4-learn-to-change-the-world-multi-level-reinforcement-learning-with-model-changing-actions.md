---
title: "Learn to change the world: Multi-level reinforcement learning with model-changing actions"
title_zh: 学会改变世界：具有模型改变动作的多层强化学习
authors: "Ziqing Lu, Babak Hassibi, Lifeng Lai, Weiyu Xu"
date: 2025-09-19
pdf: "https://openreview.net/pdf?id=W63sPIQPz4"
tags: ["query:sr"]
score: 9.0
evidence: 通过模型改变动作修改世界动力学的强化学习
tldr: 传统强化学习假设环境固定，但智能体若能主动改变世界模型本身，可能获得更高收益。本文提出多层可配置时变MDP（MCTVMDP），允许智能体通过上层模型改变动作修改下层MDP的转移函数。智能体的目标包含两部分：在给定动力学下的优化和改变动力学的收益。该框架拓展了强化学习中智能体可行动的范围，为主动环境修改提供了理论基础。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 标准RL假设环境固定，但智能体若能修改世界模型，可能获得更高奖励。
method: 提出MCTVMDP框架，将MDP分为上下两层，上层动作可改变下层的转移函数。
result: 在设计的模拟环境中，模型改变动作带来了显著的收益提升。
conclusion: 该工作为主动环境修改的RL提供了新范式。
---

## Abstract
Reinforcement learning usually assumes a given or sometimes even fixed environment in which an agent seeks an optimal policy to maximize its long-term discounted reward. In contrast, we consider agents that are not limited to passive adaptations: they instead have model-changing actions that actively modify the RL model of world dynamics itself. Reconfiguring the underlying transition processes can potentially increase the agents' rewards. Motivated by this setting, we introduce the multi-layer configurable time-varying  Markov decision process (MCTVMDP). In an MCTVMDP, the lower-level MDP has a non-stationary transition function that is configurable through upper-level model-changing actions. The agent's objective consists of two parts: Optimize the configuration policies in the upper-level MDP and optimize the primitive action policies in the lower-level MDP to jointly improve its expected long-term reward.

---

## 论文详细总结（自动生成）

### 1. 论文的核心问题与整体含义（研究动机和背景）
- **研究动机**：标准强化学习（RL）假设环境是给定的、甚至固定不变的，智能体只能通过选择动作来适应环境，无法主动改变环境的动力学（即转移函数）。但在现实场景中，智能体若能修改世界模型本身（如调整物理参数、改变规则），可能获得更高长期收益。论文旨在突破传统RL的被动适应局限，探索智能体通过“模型改变动作”主动重新配置世界动力学的可能性。
- **整体含义**：提出一个全新的强化学习范式——允许智能体通过上层动作修改下层MDP的转移函数，从而拓展智能体的行动范围，为主动环境修改提供理论基础。

### 2. 论文提出的方法论：核心思想、关键技术细节、公式或算法流程
- **核心思想**：引入**多层可配置时变马尔可夫决策过程（MCTVMDP）**。将MDP分为两层：
  - **下层MDP**：具有非平稳（时变）的转移函数，智能体在该层执行原始动作（primitive actions）以获取奖励。
  - **上层MDP**：智能体通过**模型改变动作（model-changing actions）** 配置下层MDP的转移函数（即改变世界动力学）。
- **关键技术细节**：
  - 智能体的总目标由两部分组成：①优化上层MDP中的配置策略（决定何时以及如何改变转移函数）；②优化下层MDP中的原始动作策略。两者协同最大化长期折扣奖励。
  - 下层MDP的转移函数不再是固定的，而是受上层动作影响的时变函数。
- **算法流程**（文字说明）：
  - 在每个时间步，智能体首先根据上层策略选择是否执行模型改变动作（可能消耗成本或延迟），修改下层环境的转移概率。
  - 然后在下层MDP中，智能体根据当前（已改变的）转移函数执行原始动作，获得即时奖励并转移到新状态。
  - 上下层策略通过联合优化（如分层强化学习或元学习方式）进行更新。

### 3. 实验设计：使用了哪些数据集/场景，benchmark是什么，对比了哪些方法
- **实验场景**：论文在**模拟环境**中进行验证（具体场景未详述，根据元数据提及“在设计的模拟环境中”）。可能设计了需要主动改变转移函数才能获得高奖励的任务（例如改变环境中的物理参数、修改奖励函数结构等）。
- **Benchmark**：未明确说明标准benchmark。对比方法应为标准RL算法（如DQN、PPO）在不允许模型改变动作的情况下的表现，以及可能消去上层动作的变体。
- **对比方法**：未详细列出，但推测包括“无模型改变动作”的传统RL基线，以及“固定转移函数”下的RL策略。

### 4. 资源与算力
- 论文元数据及摘要**未明确说明**使用的GPU型号、数量或训练时长。由于是Rejected论文，可能实验规模较小，未强调算力开销。因此，无法总结具体资源信息。

### 5. 实验数量与充分性
- **实验数量**：元数据仅提到“在设计的模拟环境中”进行验证，未列出具体实验组数（如不同参数设置、随机种子次数、消融实验数量）。推断至少包括主实验（比较有无模型改变动作的性能差异）和可能的敏感性分析。
- **充分性与公平性**：由于实验细节缺失，难以判断充分性。通常此类理论性工作需要在多个随机种子下重复实验并报告均值和方差。若未提供，则实验充分性受限。客观性依赖于是否使用标准对比方法和统一评估指标。

### 6. 论文的主要结论与发现
- 提出MCTVMDP框架，证明在允许模型改变动作的环境下，智能体能够获得比传统固定环境RL**显著更高的长期收益**。
- 模型改变动作并不是无代价的，需要智能体权衡改变成本与未来收益的增益。
- 该工作为“主动环境修改”的强化学习提供了新范式，拓展了智能体可行动的范围。

### 7. 优点：方法或实验设计上的亮点
- **理论创新**：首次定义“模型改变动作”并将其形式化为多层MDP结构，打破了传统RL环境固定的固有假设。
- **问题本质**：将环境动力学视为可调参数，符合许多现实应用（如机器人调整自身物理参数、系统配置、政策修改等）。
- **目标明确**：将优化目标分解为上下两层策略的联合优化，逻辑清晰，便于后续算法设计。

### 8. 不足与局限
- **实验覆盖不足**：仅在模拟环境中验证，未在标准RL基准（如Atari、MuJoCo、GridWorld）或现实场景中测试，泛化能力存疑。
- **算法细节缺失**：论文未给出具体优化算法（如如何联合学习上下层策略，是否使用了分层RL、元学习或梯度估计方法）。
- **缺乏对比基线**：未与相关领域工作（如参数化环境、可微环境、元学习适应环境变化等）进行全面比较。
- **应用限制**：模型改变动作可能需要现实世界中的物理修改或重新配置，存在成本、安全性和可实施性挑战。
- **偏差风险**：模拟环境可能经过特别设计以有利于MCTVMDP，存在选择性偏差。

（完）
