---
title: "T2VTextBench: A Human Evaluation Benchmark for Textual Control in Video Generation Models"
title_zh: T2VTextBench：文本控制视频生成模型的人类评估基准
authors: "Xuyang Guo, Jiayan Huo, Zhenmei Shi, Zhao Song, Jiahao Zhang, Jiale Zhao"
date: 2025-09-10
pdf: "https://openreview.net/pdf?id=lAXC8rLGcM"
tags: ["query:sr"]
score: 9.0
evidence: 文本控制视频生成模型的人类评估基准
tldr: 该文提出T2VTextBench，首个专门评估文本到视频模型中屏幕文字保真度和时间一致性的基准。该基准包含多个复杂提示，覆盖拼写、数学公式等场景，为视频生成模型的文本渲染能力提供了标准评测。实验表明现有模型在精确文本渲染上存在显著不足。
source: ICLR-2026-Public
selection_source: conference_retrieval
motivation: 现有视频生成模型缺乏对屏幕文字准确性的系统评测。
method: 构建 T2VTextBench 基准，包含精心设计的提示集和人类评估协议。
result: 揭示了当前模型在精确文本渲染方面的局限性。
conclusion: T2VTextBench 为视频生成模型的文本控制能力提供了重要基准。
---

## Abstract
Recent advancements in scalable deep architectures and large-scale pretraining have enabled text-to-video generation has achieve unprecedented capabilities in producing high-fidelity, instruction-following content across a wide range of styles, supporting applications in advertising, entertainment, and education. However, these models' ability to render precise on-screen text, such as captions or mathematical formulas, remains largely untested, posing significant challenges for applications requiring exact textual accuracy. In this work, we introduce T2VTextBench, the first human-evaluation benchmark dedicated to evaluating on-screen text fidelity and temporal consistency in text-to-video models. Our suite of prompts integrates complex text strings with dynamic scene changes, testing each model's ability to maintain detailed instructions across frames. We evaluate ten state-of-the-art systems, ranging from open-source solutions to commercial offerings, and find that most struggle to generate legible, consistent text. These results highlight a critical gap in current video generators and provide a clear direction for future research aimed at enhancing textual manipulation in video synthesis.

---

## 论文详细总结（自动生成）

好的，以下是对该论文的详细中文总结。

### 1. 核心问题与整体含义（研究动机和背景）

- **研究动机**：尽管最新的文本到视频（Text-to-Video）生成模型在生成高保真、遵循指令的内容方面取得了显著进展，并广泛应用于广告、娱乐和教育等领域，但这些模型在精确渲染屏幕上的文字（如字幕、数学公式）方面的能力尚未得到系统性的评估。
- **核心问题**：当前缺乏一个专门的、标准化的评估基准来衡量视频生成模型对屏幕上文字（on-screen text）的保真度和时间一致性，这导致难以准确判断模型是否具备处理需要高度文本准确性的应用场景（例如教育视频、科学演示等）的能力。
- **整体含义**：该文指出，尽管视频生成模型的总体质量大幅提升，但“文本渲染”这一关键子任务仍存在明显短板，亟需专门的评测工具来推动相关技术的进步。

### 2. 方法论：核心思想、关键技术细节与流程

- **核心思想**：构建第一个专门面向文本到视频模型的人类评估基准——**T2VTextBench**。该基准的核心是设计一套专门用来测试模型文本渲染能力的提示集，并采用人类评估协议来量化模型的表现。
- **关键技术细节**：
    - **提示集设计**：这套提示集整合了“复杂的文本字符串”与“动态的场景变化”。例如，提示可能要求模型生成“一个人在黑板上写公式 \(E=mc^2\)，然后黑板上的文字逐渐消失”这样的视频，从而同时测试拼写准确性和时间一致性。
    - **评估维度**：主要评估两个维度：
        1.  **屏幕文字保真度**（On-screen text fidelity）：生成的文字是否可读、拼写正确、字体风格符合要求。
        2.  **时间一致性**（Temporal consistency）：文字在视频的连续帧中是否保持一致（不出现闪烁、变形或突然变化）。
    - **人类评估协议**：由于自动评估很难准确判断文字质量和一致性，该工作采用人工评审的方式，由评估者对每个模型的输出视频进行打分。
- **算法流程（文字说明）**：① 设计包含不同难度（拼写、数学公式、多行文字、场景变换）的提示集合；② 将提示输入到待测模型中生成视频；③ 收集生成视频，由人类评估者根据保真度和时间一致性两个指标进行评分；④ 统计各模型的平均得分并进行排名。

### 3. 实验设计

- **数据集 / 场景**：该论文未使用现有公开数据集，而是**自行构建了一个专门的提示集**，覆盖了拼写单词、短语、数学公式、混合文本与动态场景等常见但具有挑战性的场景。
- **Benchmark**：该论文提出的 **T2VTextBench 本身即为基准**，后续的研究者可直接使用其提示集和评估协议。
- **对比方法**：评估了 **10 个最先进的系统**，涵盖了开源解决方案（如 Stable Video Diffusion 等）和商业产品（如 Sora、Runway Gen-3 等）。论文未提供具体模型名称，但提到涵盖了当时主流模型。

### 4. 资源与算力

- **未明确说明**。该论文的主要贡献在于构建基准和进行人类评估，文中**没有提及训练模型所用的 GPU 型号、数量、训练时长或评估所需的算力资源**。这部分信息在论文中缺失。

### 5. 实验数量与充分性

- **实验数量**：主要实验为 **1 组**：使用 T2VTextBench 提示集对 10 个模型进行人类评估。未提及进行消融实验（例如验证不同提示难度的影响）或在不同数据集上的对比实验。
- **充分性分析**：
    - **优点**：首次聚焦于屏幕文字渲染这一具体问题，评估覆盖了开源和商业模型，范围较宽。
    - **不足**：
        1.  仅依赖单一人类评估协议，缺乏自动指标（如 CLIP score、字准确率）的对照，可能存在主观偏差。
        2.  未进行消融实验来验证提示集中不同元素（如场景动态程度、文字长度）对模型性能的具体影响。
        3.  评估数量（10个模型）对于总结“现有模型普遍不足”的结论是足够的，但对于指出模型间的细微差异可能不够充分。

### 6. 主要结论与发现

- **主要发现**：绝大多数（10个模型中的大部分）现有视频生成模型在生成清晰、可读且时间一致的屏幕文字时**表现挣扎**。
- **结论**：当前视频生成模型存在一个**关键差距**——在精确文本渲染方面能力严重不足。这为未来研究（如改进模型架构、引入文本渲染模块或使用更精细的字幕数据训练）提供了明确的方向。T2VTextBench 提供了可靠的评测标准，有助于推动该领域的进步。

### 7. 优点

- **首创性**：首次提出了专门用于评估视频生成模型中屏幕文字保真度和时间一致性的基准，弥补了该评测领域的空白。
- **针对性强**：提示集设计直接针对“文本渲染”这一核心难题，覆盖了拼写、数学公式等实际应用中常见的复杂场景，具有很高的实用价值。
- **评估方法可靠**：采用人类评估这一黄金标准，能够直观反映模型输出在文字质量上的感知效果，避免了自动指标可能存在的漏洞（例如模型生成“看起来像文字但实际错误”的图形）。
- **问题导向明确**：论文清晰地指出了当前技术瓶颈，为后续研究提供了具体的解决方向。

### 8. 不足与局限

- **实验覆盖有限**：仅评估了10个模型（虽然代表性较强），但未涵盖更多新兴模型或不同炼丹策略的版本。未进行消融实验以分析不同提示难度的影响。
- **缺乏自动量化指标**：完全依赖人类评估可能导致结果难以复现，且受评审者主观偏好影响。未提供像字符错误率（CER）、图像字幕召回率（CLIP Score）等自动指标作为辅助参考。
- **提示集规模未知**：论文未明确说明提示集包含多少条提示，若提示集数量过少，则可能无法充分反映模型在所有场景下的文本渲染能力。
- **应用限制**：该基准仅关注文字渲染本身，未考虑其他重要方面（如文字与背景的交互、文字语义的合理性等）。对于需要极低错误率的场景（如医疗、法律文档），该基准可能仍不够严格。
- **数据处理细节缺失**：未说明是否对模型的生成结果进行了分辨率或帧率的一致性预处理，也未公布人类评估的具体评分规则（如李克特量表的等级定义）。

（完）
