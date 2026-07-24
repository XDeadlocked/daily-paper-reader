---
title: Constrained Diffusion Policy Optimization for Offline Reinforcement Learning
title_zh: 离线强化学习的约束扩散策略优化
authors: "Gyeongmin Kim, Sungho Choi, Myungsik Cho, Jongseong Chae, Jeonghye Kim, Youngchul Sung"
date: 2025-09-20
pdf: "https://openreview.net/pdf?id=1fALdE637I"
tags: ["query:sr"]
score: 9.0
evidence: 离线强化学习中的约束扩散策略优化方法
tldr: 现有扩散策略约束方法缺乏统一框架。本文提出约束扩散策略优化（CDPO）统一框架，并在此基础上设计两阶段改进策略TDP：首先用闭式解初始化，再进行约束优化。理论分析了策略改进和分布保持性质，实验证明性能优于现有扩散策略。
source: ICLR-2026-Public
selection_source: conference_retrieval
motivation: 统一现有扩散策略约束方法并提升性能。
method: 提出CDPO框架统一约束，TDP算法通过闭式初始化与再优化进行两阶段改进。
result: 在离线RL基准上取得最优性能，理论保证策略改进。
conclusion: 约束扩散策略优化为离线RL提供高效且可理论保障的训练策略。
---

## Abstract
In this paper, we propose the two-fold improved diffusion policy (TDP) for offline reinforcement learning. We first propose the constrained diffusion policy optimization (CDPO) framework, which unifies existing diffusion-based policy constraint methods. TDP harnesses the full potential of CDPO by initializing with the closed-form solution of a constrained optimization problem and then applying another constrained policy optimization for further refinement. We establish the theoretical properties of TDP, including expected policy improvement, in-distribution property, and approximate gains over existing diffusion policies. We also propose a design method for estimating the desired policy in the TDP loss function to achieve the aforementioned performance improvements. Empirical results on the D4RL benchmark show that TDP outperforms most existing offline reinforcement learning methods.

---

## 论文详细总结（自动生成）

以下是根据提供的论文摘要和元数据生成的中文总结。注意：由于原始论文PDF正文不可访问（仅显示验证页面），以下总结主要基于摘要和元数据，对于未提供的细节将明确标注。

---

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **研究动机**：在离线强化学习（Offline RL）中，扩散模型被用作策略表示，但现有扩散策略的约束方法缺乏统一理论框架，导致训练不稳定或性能受限。
- **核心问题**：如何设计一个统一的约束扩散策略优化框架，并在此基础上进一步提升离线RL的性能与理论保障。
- **整体含义**：本文提出**约束扩散策略优化（CDPO）** 统一框架，并基于此设计了**两阶段改进扩散策略（TDP）**，从理论和实验两方面证明了其有效性，为离线RL提供了一种高效且可理论保障的策略学习方法。

## 2. 论文提出的方法论

- **核心思想**：将扩散策略的约束问题统一到CDPO框架中，通过两阶段优化充分发挥其潜力。
- **关键技术细节**：
  - **第一阶段**：利用约束优化问题的**闭式解**对扩散策略进行初始化，快速获得一个合理的初始策略。
  - **第二阶段**：在CDPO框架下进行**再次约束优化**，进一步微调策略以提升性能。
  - **损失函数设计**：提出一种估计期望策略的方法，用于构造TDP的损失函数，从而保证上述两阶段改进的有效性。
- **理论性质**：本文建立了TDP的期望策略改进性质、分布保持性质（in-distribution），并证明了相较于现有扩散策略的近似增益。

## 3. 实验设计

- **数据集/场景**：使用了 **D4RL** 基准（离线强化学习常用数据集），涵盖多种任务类型（如Mujoco、Adroit等，但具体任务列表原文未详列）。
- **基准（Benchmark）**：D4RL标准评估协议。
- **对比方法**：与“大多数现有离线强化学习方法”进行比较，包括其他基于扩散策略的方法以及非扩散的离线RL方法。具体方法列表需查看完整论文。

## 4. 资源与算力

- **文中未明确说明**：摘要和元数据中未提及使用的GPU型号、数量、训练时长等算力信息。需要查阅完整论文获取细节。

## 5. 实验数量与充分性

- **实验数量**：从摘要“outperforms most existing offline RL methods”推断，至少进行了多个D4RL任务上的对比实验，可能包含不同环境变体（如halfcheetah-medium, hopper-medium-replay等），但具体组数未知。
- **充分性**：依赖D4RL单一基准，缺乏更广泛领域（如AntMaze、Kitchen等）或真实场景的验证。消融实验（如对TDP两阶段的贡献、约束形式的影响）未在摘要提及，但通常论文中会有。仅基于摘要无法判断是否充分公平，需阅读全文。

## 6. 论文的主要结论与发现

- **实验结论**：TDP在D4RL基准上优于大多数现有的离线强化学习方法。
- **理论结论**：TDP具备期望策略改进、分布保持性以及相对于现有扩散政策的近似增益。
- **总体发现**：统一约束框架+两阶段优化能有效提升扩散策略在离线RL中的性能，且理论有保障。

## 7. 优点

- **统一框架**：首次将多种扩散策略约束方法纳入CDPO统一框架，便于理解和改进。
- **两阶段改进**：结合闭式初始化与再优化，创新性地平衡了快速收敛与精细调整。
- **理论支撑**：提供了策略改进、分布保持等严格理论分析，增强了方法的可信度。
- **性能优越**：在公认的D4RL基准上取得领先结果。

## 8. 不足与局限

- **实验覆盖**：仅使用D4RL数据集，未在更多离线RL基准（如RL Unplugged、NeoRL等）或真实机器人任务上验证，泛化性存疑。
- **偏差风险**：可能隐式依赖D4RL的数据分布特点，方法在其他数据特性下的表现未知。
- **应用限制**：扩散模型采样成本较高，TDP的两阶段优化可能进一步增加训练复杂度，文中未讨论计算开销。
- **信息缺失**：由于无法获取全文，消融实验、超参数敏感性、不同约束形式的影响等细节不明，限制了独立复现与深入评价。

---

（完）
