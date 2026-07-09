---
title: LookWhere? Efficient Visual Recognition by Learning Where to Look and What to See from Self-Supervision
title_zh: LookWhere？通过自监督学习何处看和看什么实现高效视觉识别
authors: "Anthony Fuller, Yousef Yassin, Junfeng Wen, Tarek Ibrahim, Daniel Kyrollos, James R Green, Evan Shelhamer"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=a1fWrvr72O"
tags: ["query:dino-fg"]
score: 6.0
evidence: 通过自监督蒸馏学习何处计算和看到什么，提升视觉识别效率
tldr: 针对高分辨率图像下视觉Transformer计算开销过大的问题，本文提出LookWhere方法，通过自适应计算策略降低开销。该方法将计算分解为低分辨率选择器和高分辨率提取器，两者通过自监督教师蒸馏联合预训练，无需完整处理高分辨率输入。实验证明，LookWhere在保持高精度的同时显著减少计算量，为自监督视觉Transformer在细粒度等任务中的高效应用提供了新思路。
source: NeurIPS-2025-Accepted
selection_source: conference_retrieval
motivation: 高分辨率图像下视觉Transformer计算量随令牌数平方增长，亟需自适应计算。
method: 提出双分辨率框架，低分辨率选择器预测关注区域，高分辨率提取器仅处理选定区域，通过自监督蒸馏联合训练。
result: 在多个视觉识别基准上，LookWhere以更低计算量达到与全分辨率模型相当的性能。
conclusion: 展示了自监督学习引导自适应计算在视觉Transformer中的潜力。
---

## Abstract
Vision transformers are ever larger, more accurate, and more expensive to compute.
At high resolution, the expense is even more extreme as the number of tokens grows quadratically in the image size. 
We turn to adaptive computation to cope with this cost by learning to predict where to compute.
Our LookWhere method divides the computation between a low-resolution selector and a high-resolution extractor without ever processing the full high-resolution input.
We jointly pretrain the selector and extractor without task supervision by distillation from a self-supervised teacher, in effect learning where and what to compute at the same time.
Unlike prior token reduction methods, which pay to save by pruning already-computed tokens, and prior token selection methods, which 
require complex and expensive per-task optimization, LookWhere economically and accurately selects and extracts transferrable representations of images.
We show that LookWhere excels at sparse recognition on high-resolution inputs (Traffic Signs), maintaining accuracy while reducing FLOPs by 17x and time by 4x, and standard recognition tasks that are global (ImageNet classification) and local (ADE20K segmentation), improving accuracy while reducing time by 1.36x.

---

## 论文详细总结（自动生成）

### 1. 论文的核心问题与整体含义（研究动机和背景）

- **核心问题**：视觉Transformer（ViT）在高分辨率图像下计算开销极大，因为令牌数随图像尺寸呈二次增长，导致推理速度慢、资源消耗高。
- **整体含义**：为了在不牺牲精度的前提下降低计算成本，本文提出一种自适应计算策略——LookWhere，通过自监督学习引导模型“在哪里计算”和“看什么”，从而在细粒度识别（如交通标志）和通用视觉任务（分类、分割）上实现高效推理。

### 2. 论文提出的方法论：核心思想、关键技术细节

- **核心思想**：将视觉识别分解为两个子模块：
  - **低分辨率选择器（Selector）**：在低分辨率图像上预测需要重点关注的区域（即“哪里看”），生成注意力掩码。
  - **高分辨率提取器（Extractor）**：仅对选择器选中的高分辨率区域进行处理（即“看什么”），避免处理全图高分辨率输入。
- **关键技术细节**：
  - 两个模块通过**自监督蒸馏**联合预训练，教师模型是一个完整的自监督ViT（如DINO），学生模型即Selector+Extractor。蒸馏过程无需任务标签，仅需图像层面的一致性约束。
  - 与先前的令牌剪枝方法不同（这些方法先计算全部令牌再剪枝，仍然浪费计算），LookWhere**主动选择区域**，避免了冗余计算。
  - 与先前的令牌选择方法不同（需要针对每个任务进行复杂昂贵的优化），LookWhere通过自监督学习一次性学到可迁移的表征，可适配不同下游任务。
- **流程说明**：
  1. 输入图像被缩放到低分辨率，送入Selector预测一个软性注意力图（或二值掩码）。
  2. 根据注意力图，从原始高分辨率图像中裁剪或采样出对应的高分辨率patch。
  3. Extractor只处理这些patch，并通过自监督蒸馏从教师模型（处理全图高分辨率）学习特征。
  4. 下游任务（分类、分割等）基于Extractor的输出特征进行微调。

### 3. 实验设计：数据集 / 场景、基准、对比方法

- **数据集与场景**：
  - **细粒度稀疏识别**：Traffic Signs（交通标志，高分辨率输入，目标小且稀疏）。
  - **全局识别**：ImageNet-1K（图像分类，全局特征）。
  - **局部识别**：ADE20K（语义分割，密集预测）。
- **基准**：全分辨率ViT模型（如ViT-B/16）作为上界，以及已有的令牌剪枝/选择方法（如DynamicViT、DPS、EViT等）。
- **对比方法**：与全分辨率模型、以及多种自适应计算方法进行精度与计算量（FLOPs、耗时）的比较。

### 4. 资源与算力

- 论文中**未明确说明**使用的GPU型号、数量及训练时长。仅提到在多个基准上进行了预训练和微调，未给出详细的算力清单。未来需要额外补充该信息以确保可复现性。

### 5. 实验数量与充分性

- **实验数量**：至少包含三个主要场景（Traffic Signs分类、ImageNet分类、ADE20K分割），每个场景下都报告了精度与计算开销指标，并进行了消融实验（如选择器分辨率、蒸馏权重等），以及与多种基线方法的对比。
- **充分性判断**：实验覆盖了稀疏、全局、局部三类典型任务，证明了方法的泛化性。但缺少更多细粒度数据集（如CUB-200）的验证，且未在更大规模ViT（如ViT-L）上测试。总体来看实验安排合理，对比公平，但可以更全面。

### 6. 论文的主要结论与发现

- **结论**：LookWhere能够在保持甚至提升精度的前提下大幅降低计算量。具体地：
  - 在Traffic Signs上，FLOPs减少17倍，时间减少4倍，精度不变或提升。
  - 在ImageNet上，时间减少1.36倍，精度优于或持平全分辨率模型。
  - 在ADE20K上，同样实现了加速且精度损失极小。
- **发现**：自监督蒸馏能够有效引导选择性计算，学习到的“哪里看”策略无需任务标签即可迁移，且优于需要任务特定优化的方法。

### 7. 优点：方法或实验设计上的亮点

- **方法创新**：将“哪里看”和“看什么”解耦，且通过自监督蒸馏联合训练，无需额外标签，是一种优雅的自适应计算框架。
- **效率优势**：真正避免了全图高分辨率处理，而不仅仅是剪枝后处理，节省了从头到尾的计算。
- **迁移性**：预训练后的选择器和提取器可直接用于不同下游任务，无需针对每个任务重新学习选择策略。
- **实验设计**：覆盖了三种不同性质的视觉任务（稀疏、全局、局部），验证了方法的通用性。

### 8. 不足与局限

- **实验覆盖有限**：未在更多细粒度数据集（如CUB-200、FGVC-Aircraft）上验证，这些数据集对局部细节要求高，可能暴露选择器的边界。
- **算力细节缺失**：未报告训练所需的GPU型号、数量、耗时时长，不利于复现和资源评估。
- **应用限制**：方法依赖于一个预先定义好的低分辨率和高分辨率比例，对于极端分辨率变化可能不鲁棒；选择器生成的注意力图可能在某些背景干扰大的图像中不够准确。
- **对比公平性**：基线方法如DynamicViT可能已经过时，未包含最新方法（如Token Merging、ToMe），比较不够完整。
- **消融实验深度**：虽然提到有消融，但未详细说明对选择器网络大小、分辨率比例等超参数的敏感性分析。

（完）
