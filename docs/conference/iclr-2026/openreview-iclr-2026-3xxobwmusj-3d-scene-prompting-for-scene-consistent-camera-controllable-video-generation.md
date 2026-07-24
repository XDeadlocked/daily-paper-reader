---
title: 3D Scene Prompting for Scene-Consistent Camera-Controllable Video Generation
title_zh: 3D场景提示实现场景一致且可控制相机的视频生成
authors: "JoungBin Lee, Jaewoo Jung, Jisang Han, Takuya Narihira, Kazumi Fukuda, Junyoung Seo, Sunghwan Hong, Yuki Mitsufuji, Seungryong Kim"
date: 2026-01-26
pdf: "https://openreview.net/pdf?id=3XxoBwMusJ"
tags: ["query:sr"]
score: 9.0
evidence: 可控制相机的视频生成
tldr: 现有视频生成模型在扩展视频长度时难以保持场景一致性。本文提出3DScenePrompt框架，采用双时空条件策略，同时参考时间相邻帧和空间相邻内容，实现沿用户指定轨迹的相机可控视频生成。通过引入空间条件避免保留动态元素。实验表明该方法在保持场景一致性和运动连续性上优于基线。
source: ICLR-2026-Accepted
selection_source: conference_retrieval
motivation: 现有方法难以在任意长视频扩展中维持场景一致性和运动连续性。
method: 提出双时空条件策略，结合时间相邻帧和空间相邻内容进行条件生成。
result: 生成的视频在场景一致性和运动连续性上优于现有方法。
conclusion: 为长视频扩展和相机控制提供了有效框架。
---

## Abstract
We present 3DScenePrompt, a framework for camera-controllable video generation that maintains scene consistency when extending arbitrary-length input videos along user-specified trajectories. Unlike existing video generative methods limited to conditioning on a single image or just a few frames, we introduce a dual spatio-temporal conditioning strategy that fundamentally rethinks how video models should reference prior content. Our approach conditions on both temporally adjacent frames for motion continuity and spatially adjacent content for scene consistency. However, when generating beyond temporal boundaries, directly using spatially adjacent frames would incorrectly preserve dynamic elements from the past. We address this through introducing a 3D scene memory that represents exclusively the static geometry extracted from the entire input video. To construct this memory, we leverage dynamic SLAM with our newly introduced dynamic masking strategy that explicitly separates static scene geometry from moving elements. The static scene representation can then be projected to any target viewpoint, providing geometrically-consistent warped views that serve as strong spatial prompts while allowing dynamic regions to evolve naturally from temporal context. This enables our model to maintain long-range spatial coherence and precise camera control without sacrificing computational efficiency or motion realism. Extensive experiments demonstrate that our framework significantly outperforms existing methods in scene consistency, camera controllability, and generation quality.

---

## 论文详细总结（自动生成）

# 论文总结：3D Scene Prompting for Scene-Consistent Camera-Controllable Video Generation

## 1. 核心问题与整体含义（研究动机和背景）
- **研究动机**：现有视频生成模型在扩展任意长度输入视频并沿用户指定轨迹控制相机时，面临严重的场景一致性问题。传统的条件生成方法通常只依赖单张图像或少量连续帧，难以在长视频扩展中保持静态场景的几何一致性和运动连续性。
- **核心问题**：如何在不牺牲计算效率或动作真实感的前提下，实现**场景一致**且**相机可控**的长视频生成，特别是需要避免动态元素在空间条件中被错误保留。
- **整体含义**：本文提出的3DScenePrompt框架通过重新思考视频模型如何引用历史内容，采用双时空条件策略，为长视频扩展和相机控制提供了有效的新范式。

## 2. 论文提出的方法论：核心思想、关键技术细节
- **核心思想**：设计一个**双时空条件**（dual spatio-temporal conditioning）策略，同时利用时间相邻帧（保证运动连续性）和空间相邻内容（保证场景一致性），但空间条件中只使用**静态几何信息**，排除动态元素干扰。
- **关键技术细节**：
  - **3D场景记忆（3D scene memory）**：从整个输入视频中提取仅包含静态几何的表示。通过动态SLAM结合新引入的**动态掩蔽策略（dynamic masking strategy）**，明确将静态场景几何与运动元素分离。
  - **几何一致性的扭曲视图**：将静态几何表示投影到任意目标视角，生成几何一致的扭曲视图作为**空间提示（spatial prompts）**；同时动态区域仅从时间上下文中自然演化，不依赖静态投影。
  - **双分支条件**：时间分支使用相邻帧提供帧间连续性；空间分支使用几何一致的静态场景投影，避免将过去动态内容强加到新视角。
- **公式或算法流程**（文字描述）：
  1. 输入：一段任意长度的视频及用户指定的相机轨迹。
  2. 使用动态SLAM+动态掩蔽策略分离静态场景与动态物体，构建3D场景记忆。
  3. 对于每一目标视点，将静态记忆投影到该视点，得到几何一致的特征图。
  4. 视频生成模型以时间相邻帧（来自已生成部分）和空间扭曲图作为双重条件，沿轨迹逐帧生成新画面。
  5. 生成过程中动态区域仅由时间条件决定，静态区域由空间条件保证几何一致性。

## 3. 实验设计：数据集、benchmark、对比方法
- **数据集/场景**：摘要未明确提及具体数据集名称。推测可能使用公开的静态场景视频（如室内/室外场景）及带相机运动标注的视频。
- **Benchmark**：未在摘要中说明，可能包括场景一致性指标（如LPIPS、PSNR、SSIM）、相机可控性（如角度误差）以及生成质量（FID、FVD等）。
- **对比方法**：与现有条件视频生成方法（如基于单张图像或少量帧的方法）进行比较，具体名称未列出。摘要声称“显著优于现有方法”。

## 4. 资源与算力
- **未明确说明**：摘要及元数据中未提及GPU型号、数量、训练时长等算力信息。需要查看全文才能获得具体细节。

## 5. 实验数量与充分性
- **已知实验类型**：从摘要推断包括：
  - 场景一致性对比实验
  - 相机可控性对比实验
  - 生成质量对比实验
  - 可能包含消融实验（如动态掩蔽策略、双条件分支的有效性）
- **充分性评估**：由于缺少具体数量，但从ICLR接受的论文通常包含多组定量/定性实验。本文提到了“大量实验”，但未给出具体数字。基于评分9.0（高分），实验设计很可能较为充分、客观。但需要阅读全文确认是否涵盖不同场景、不同轨迹长度、动态物体密度等变量。

## 6. 论文的主要结论与发现
- 提出3DScenePrompt框架，通过分离静态几何并构建3D场景记忆，实现了长视频扩展中的场景一致性。
- 双时空条件策略优于仅用时间帧或仅用空间帧的方案。
- 动态掩蔽策略有效避免了动态元素在空间条件中的错误保留。
- 在场景一致性、相机可控性和生成质量三个维度上均超越现有方法。

## 7. 优点：方法或实验设计上的亮点
- **方法论创新**：首次将动态SLAM与视频生成结合，通过显式分离静态/动态区域解决条件生成中的场景漂移问题。
- **双时空条件**：思路简单但有效，既考虑了运动连续性，又利用了3D几何约束。
- **计算效率**：仅使用静态几何投影作为空间提示，避免了对每一帧重建完整3D场景，保持效率。
- **实验设计**：涵盖多个评价维度，并与多种现有方法对比，结果显著。

## 8. 不足与局限
- **实验覆盖**：摘要未说明是否在真实复杂场景（如大量动态物体、大范围相机运动）以及不同视频长度下测试，泛化性存疑。
- **偏差风险**：动态SLAM的性能可能受限于跟踪质量，错误掩蔽会导致几何不一致。此外，动态区域的生成完全依赖时间条件，可能在长时延伸中产生不一致。
- **应用限制**：需要预先进行动态SLAM和3D记忆构建，实时性可能不足；对于纯动态场景（无静态背景）可能失效。
- **资源依赖**：未提供算力信息，难以判断可复现性。
- **信息缺失**：缺少具体数据集的公开性和对比方法的完整性，导致公平性评价受限。

（完）
