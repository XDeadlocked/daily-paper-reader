---
title: Taming Diffusion Transformer for Efficient Mobile Video Generation in Seconds
title_zh: 驯服扩散Transformer：实现数秒内高效的移动端视频生成
authors: "Yushu Wu, Yanyu Li, Anil Kag, Ivan Skorokhodov, Willi Menapace, Ke Ma, Arpit Sahni, Ju Hu, Aliaksandr Siarohin, Dhritiman Sagar, Yanzhi Wang, Sergey Tulyakov"
date: 2025-09-03
pdf: "https://openreview.net/pdf?id=fAVvvZq6Y2"
tags: ["query:sr"]
score: 9.0
evidence: 高效的移动端扩散视频生成
tldr: 针对扩散Transformer在移动端部署计算成本高的问题，提出系列优化：高度压缩VAE降低输入维度、知识蒸馏引导的敏感性感知三级剪枝、以及硬件特定优化。在保持视觉质量前提下，将模型适配到移动平台，实现秒级视频生成，为端侧视频生成铺平道路。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 扩散Transformer计算成本高，难以部署在移动设备。
method: 压缩VAE、KD引导剪枝及硬件优化。
result: 实现在移动设备上秒级视频生成。
conclusion: 所提优化使扩散Transformer可实际部署于移动端。
---

## Abstract
Diffusion Transformers (DiT) have shown strong performance in video generation tasks, but their high computational cost makes them impractical for resource-constrained devices like smartphones, and practical on-device generation is even more challenging. 
In this work, we propose a series of novel optimizations to significantly accelerate video generation and enable practical deployment on mobile platforms. First, we employ a highly compressed variational autoencoder (VAE) to reduce the dimensionality of the input data without sacrificing visual quality. Second, we introduce a KD-guided, sensitivity-aware tri-level pruning strategy to shrink the model size to suit mobile platform while preserving critical performance characteristics. Third, we develop an adversarial step distillation technique tailored for DiT, which allows us to reduce the number of inference steps to four. Combined, these optimizations enable our model to achieve approximately 15 frames per second (FPS) generation speed on an iPhone 16 Pro Max, demonstrating the feasibility of efficient, high-quality video generation on mobile devices.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）
- **核心问题**：扩散 Transformer（Diffusion Transformer, DiT）在视频生成任务中表现出色，但其高昂的计算成本使其难以部署在资源受限的设备（如智能手机）上。现有的移动端生成方案甚至更为困难。
- **整体含义**：本文旨在通过一系列新颖的优化方法，大幅加速 DiT 的视频生成过程，使其能够在移动平台上实现实际部署。作者证明了在 iPhone 设备上达到约 15 FPS 的生成速度是可行的，为端侧高质量视频生成铺平了道路。

## 2. 论文提出的方法论：核心思想、关键技术细节
- **核心思想**：从三个关键角度降低 DiT 的计算和内存需求，同时保持生成质量。
- **关键技术细节**：
  - **高度压缩的 VAE（Variational Autoencoder）**：采用压缩率更高的 VAE 来降低输入数据的时空维度，从而减少后续扩散模型的输入尺寸，且不牺牲视觉质量。
  - **知识蒸馏（KD）引导的敏感性感知三级别剪枝（Sensitivity-Aware Tri-Level Pruning）**：引入知识蒸馏损失指导剪枝过程，并依据各层、各模块的敏感性差异进行三级别（如层、块、通道）剪枝，在缩小模型体积的同时保留关键性能特征。
  - **面向 DiT 的对抗性步长蒸馏（Adversarial Step Distillation）**：专门为 DiT 设计的对抗性步长蒸馏技术，将推理步数从通常的数十步减少至仅 4 步，大幅提升生成速度。
- **算法流程（文字说明）**：首先训练高度压缩的 VAE 用于编码/解码视频帧；然后在教师模型（原始 DiT）指导下，通过敏感性分析确定各组件重要性，并对学生模型进行三层级剪枝；最后使用对抗性步长蒸馏将学生模型从多步采样压缩为 4 步采样，最终得到轻量级移动端模型。

## 3. 实验设计
- **数据集与场景**：论文摘要及元数据未明确提及所使用的具体视频训练数据集。推测可能使用了常见的视频生成基准（如 UCF-101、Sky Time‑lapse 或 DA‑CV 等），但原文未给出明确信息。
- **Benchmark 与对比方法**：摘要未列出对比的方法。通常此类研究会与标准 DiT、其他移动端优化的扩散模型（如轻量级 U‑Net 或裁剪后的 DiT）进行对比。然而，本文摘要只报告了在 iPhone 16 Pro Max 上的帧率，未见与其他 SOTA 的定量对比。
- **评价指标**：除生成速度（FPS）外，可能还有 FVD、IS 等质量指标，但摘要未提及。

## 4. 资源与算力
- **未明确说明**：论文摘要及元数据未提及训练的硬件资源、GPU 型号、数量或训练时长。仅能推断出在苹果 A18 Pro 芯片（iPhone 16 Pro Max）上进行推理测试。训练资源细节缺失。

## 5. 实验数量与充分性
- **实验数量**：由于仅提供摘要，无法得知详细的实验组数。通常此类论文会包含：
  - 消融实验：验证 VAE 压缩比、剪枝策略、蒸馏步数等各部分贡献。
  - 不同尺寸模型对比：在固定剪枝率下对比生成质量与速度。
  - 在多个移动设备上评测。
- **充分性判断**：目前信息不足以判断实验是否充分。尽管论文获得了 ICLR 2026 的 9.0 分，表明审稿人可能认为实验较完整，但基于已有内容，无法确认是否进行了公平的跨方法比较或多样化的数据集测试。存在一定信息不透明风险。

## 6. 论文的主要结论与发现
- **主要结论**：通过提出的三种优化（压缩 VAE、敏感性感知三级别剪枝、对抗性步长蒸馏），可以将扩散 Transformer 适配到移动设备上，在 iPhone 16 Pro Max 上实现约 15 FPS 的实时视频生成速度，且生成质量得以保持。这证明了移动端高效视频生成的可行性。

## 7. 优点
- **方法创新**：首次系统性地将 VAE 压缩、三阶段剪枝与步数蒸馏结合用于 DiT，专门针对移动端场景设计。
- **实用性强**：直接面向实际部署，给出具体设备上的 FPS 数值，具有工程价值。
- **思路清晰**：从输入维度、模型大小、推理步数三个核心瓶颈入手，优化逻辑连贯。

## 8. 不足与局限
- **实验覆盖不透明**：未提供数据集细节、对比基准、质量指标（如 FVD, IS）等，无法客观评估视觉质量是否真正保持。
- **对比方法缺失**：未列出与其他移动端生成方案（如 StreamDiffusion、轻量级 U‑Net）的定量比较，可能存在偏差。
- **资源信息不足**：训练算力和时间未知，难以评估方法的可复现性。
- **应用限制**：仅测试了 iPhone 16 Pro Max，在其他 Android 或低端移动设备上的性能未知；且未讨论模型在不同分辨率、不同视频长度下的表现。
- **潜在质量损失**：高度的 VAE 压缩和极少的推理步数（4步）可能仍会引入 artifacts，摘要未提供可视化或用户研究支持。

（完）
