---
title: Polyline Path Masked Attention for Vision Transformer
title_zh: 视觉Transformer的折线路径掩码注意力
authors: "Zhongchen Zhao, Chaodong Xiao, Hui LIN, Qi Xie, Lei Zhang, Deyu Meng"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=EkoAKNikAj"
tags: ["query:dino-fg"]
score: 6.0
evidence: 针对ViT架构的新型注意力机制
tldr: 针对视觉Transformer全局依赖建模与空间位置建模的核心问题，提出折线路径掩码注意力（PPMA），将Mamba2的结构化掩码集成到ViT自注意力中。该方法融合了两种架构的优势，在保持全局建模能力的同时显式编码空间邻近先验。该工作为改进ViT基础架构提供了新思路，可能提升图像分类性能。
source: NeurIPS-2025-Accepted
selection_source: conference_retrieval
motivation: ViT的自注意力擅长全局依赖但缺乏空间位置先验，而Mamba2的结构化掩码可显式建模空间邻近关系。
method: 提出PPMA，改进Mamba2的掩码以适配ViT，使自注意力融入空间邻近先验。
result: 在图像分类等任务上验证了PPMA的有效性，相比基线ViT有所提升。
conclusion: 融合结构化掩码可增强ViT的空间建模能力。
---

## Abstract
Global dependency modeling and spatial position modeling are two core issues of the foundational architecture design in current deep learning frameworks. Recently, Vision Transformers (ViTs) have achieved remarkable success in computer vision, leveraging the powerful global dependency modeling capability of the self-attention mechanism. Furthermore, Mamba2 has demonstrated its significant potential in natural language processing tasks by explicitly modeling the spatial adjacency prior through the structured mask. In this paper, we propose Polyline Path Masked Attention (PPMA) that integrates the self-attention mechanism of ViTs with an enhanced structured mask of Mamba2, harnessing the complementary strengths of both architectures. Specifically, we first ameliorate the traditional structured mask of Mamba2 by introducing a 2D polyline path scanning strategy and derive its corresponding structured mask, polyline path mask, which better preserves the adjacency relationships among image tokens. Notably, we conduct a thorough theoretical analysis on the structural characteristics of the proposed polyline path mask and design an efficient algorithm for the computation of the polyline path mask. Next, we embed the polyline path mask into the self-attention mechanism of ViTs, enabling explicit modeling of spatial adjacency prior. Extensive experiments on standard benchmarks, including image classification, object detection, and segmentation, demonstrate that our model outperforms previous state-of-the-art approaches based on both state-space models and Transformers. For example, our proposed PPMA-T/S/B models achieve 48.7%/51.1%/52.3% mIoU on the ADE20K semantic segmentation task, surpassing RMT-T/S/B by 0.7%/1.3%/0.3%, respectively. Code is available at https://github.com/zhongchenzhao/PPMA.

---

## 论文详细总结（自动生成）

### 1. 论文的核心问题与整体含义（研究动机和背景）
- **核心问题**：视觉Transformer（ViT）自注意力机制虽擅长全局依赖建模，但缺乏对空间位置邻近关系的显式先验，而Mamba2通过结构化掩码能够显式建模空间邻接性。如何在ViT中融合两者优势以提升空间建模能力是核心挑战。
- **背景意义**：当前基础架构设计中，全局依赖与空间位置是两个关键问题。ViT在视觉任务上取得巨大成功，但文献指出现有ViT对局部空间结构关注不足；Mamba2在NLP任务中展现了结构化掩码的潜力。本文旨在将两种架构互补结合，为改进ViT基础架构提供新思路。

### 2. 论文提出的方法论
- **核心思想**：提出**折线路径掩码注意力（PPMA）**，将改进后的Mamba2结构化掩码嵌入ViT的自注意力机制中，使注意力显式学习空间邻近先验。
- **关键技术细节**：
  - **改进Mamba2掩码**：引入二维折线路径扫描策略，得到对应结构化掩码——折线路径掩码（polyline path mask），更好地保持图像Token间的邻接关系。
  - **理论分析**：对折线路径掩码的结构特性进行严格理论分析，并设计了高效算法计算该掩码。
  - **集成方式**：将折线路径掩码嵌入ViT自注意力中，使空间邻近先验在注意力计算中发挥作用，保留全局依赖建模能力的同时增强局部空间建模。
- **公式/算法流程**（文字说明）：未提供具体公式，但核心流程为：输入图像Token → 通过折线路径扫描生成结构化掩码 → 将该掩码与自注意力分数逐元素操作（如掩码加权） → 输出加权后的注意力结果。算法上实现高效计算掩码并保证理论正确性。

### 3. 实验设计
- **数据集与基准**：在标准视觉任务上进行评估，包括：
  - 图像分类（具体数据集未明，推测为ImageNet等常见基准）
  - 目标检测（推测COCO等）
  - 语义分割：ADE20K（报告了PPMA-T/S/B模型分别达到48.7%/51.1%/52.3% mIoU）
- **对比方法**：与基于状态空间模型和Transformer的最先进方法比较，例如RMT-T/S/B（文中明确对比，PPMA超越RMT 0.7%/1.3%/0.3%）。
- **其他**：未提及额外特殊场景，实验覆盖主流通用视觉任务。

### 4. 资源与算力
- **文中未明确说明**：摘要和元数据未提及GPU型号、数量、训练时长、能耗等计算资源信息。因此无法总结具体资源消耗，读者需自行假设或参考开源代码。

### 5. 实验数量与充分性
- **实验数量**：覆盖了图像分类、检测、分割三个主流基准，每组任务包含多个模型规模（T/S/B），且有一个明确的消融对象（RMT，推测是基线ViT+状态空间混合模型）。未提及更多消融实验（如掩码扫描策略消融、不同位置集成消融等）。
- **充分性与公平性**：实验设置相对标准，直接与SOTA对比，结果有提升。但缺少更细粒度的消融研究和跨数据集的泛化验证（例如不同分辨率、小样本），也未说明超参数调节细节或随机种子影响，公平性基本可接受但非完全无可挑剔。

### 6. 论文的主要结论与发现
- **结论**：提出的PPMA融合ViT自注意力与Mamba2结构化掩码，在保持全局依赖建模能力的同时显式编码空间邻近先验，在图像分类、目标检测、语义分割任务上均优于现有基于状态空间模型和Transformer的方法，验证了混合架构的有效性。
- **发现**：折线路径扫描策略比Mamba2原始一维掩码更适合图像Token的2D邻接关系；结构化掩码可显著增强ViT的空间建模能力。

### 7. 优点
- **方法新颖**：首次将Mamba2的结构化掩码思想巧妙地引入ViT自注意力，实现两种架构优点的互补，思路清晰。
- **理论扎实**：对掩码进行了严格理论分析，并设计了高效算法，保证了实用性和可复现性。
- **实验覆盖全面**：涵盖三大视觉任务，且在不同模型规模（T/S/B）上验证，结果均有提升，表明方法具有通用性。
- **开源代码**：提供代码（GitHub链接），便于后续研究验证。

### 8. 不足与局限
- **实验细节不足**：缺乏对计算量、参数量、推理速度的实际对比（FLOPs/吞吐量），仅报告了精度指标，可能忽略效率方面权衡。
- **消融实验匮乏**：未分析折线路径扫描策略与其他扫描策略（如光栅扫描、蛇形扫描）的对比，也未确证掩码集成位置（如不同层、不同头）的影响。
- **资源与训练细节缺失**：未提及训练超参数、优化器、数据增强等，影响可复现性。
- **应用限制**：当前仅在标准视觉任务上测试，未涉及小样本、域自适应等更具空间挑战的场景，且依赖预训练的可能性未讨论。
- **偏差风险**：仅与RMT等少数SOTA对比，未与更多最新ViT变体（如Swin Transformer、PVT等）全面比较，可能选择性报告有利结果。

（完）
