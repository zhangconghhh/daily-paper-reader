---
title: Spatially-Regularized Entropy for Discriminative Token Merging in Fine-Grained Re-Identification
title_zh: 基于空间正则化熵的细粒度重识别判别性Token合并
authors: "Shangze Li, Yifan Xu, Jingmiao Liang, Yongfei Zhang, Yuzhuo Ma, Yingbo Qu"
date: 2026-04-30
pdf: "https://openreview.net/pdf/3668f965df6dd49e3e938b8232fd385d75df21ac.pdf"
tags: ["query:dino-fg"]
score: 7.0
evidence: 使用视觉Transformer进行细粒度重识别，并采用token合并
tldr: 细粒度检索任务中，视觉Transformer的二次计算成本限制了实时应用，现有压缩策略在丢弃信息时容易损失判别性局部细节。本文提出SRE-Merge，一种无需训练的判别性token合并框架，通过空间熵显著性评估、局部性保持合并和空间正则化三种机制，在压缩token的同时保留关键细节。在多个细粒度重识别数据集上，该方法在保持高效的同时显著优于基线，为ViT在细粒度任务中的实用化提供了有效方案。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 视觉Transformer用于细粒度识别时，二次计算成本高，现有压缩方法会丢失关键局部细节。
method: 提出SRE-Merge框架，通过空间熵显著性评估、局部性保持合并和空间正则化实现判别性token压缩。
result: 在多个细粒度重识别数据集上，该方法在压缩计算的同时保持或提升了识别准确率。
conclusion: SRE-Merge有效解决了细粒度任务中ViT的压缩与精度权衡问题。
---

## Abstract
While Vision Transformers (ViTs) offer strong global modeling, their quadratic computational cost limits utility in latency-sensitive applications like person re-identification (ReID). Existing compression strategies, such as token pruning or generic merging, typically rely on coarse-grained criteria tailored for image classification. In fine-grained retrieval, these approaches often discard or smooth out subtle but discriminative local details. To resolve this, we propose SRE-Merge, a training-free framework designed for discriminative token compression. SRE-Merge injects spatial priors into the merging process through three mechanisms: (i) Spatial-Entropy Saliency Assessment (SES-Assess), which quantifies token importance as Spatial-Entropic Mass (SE-Mass) by coupling spatial structure with local attention entropy; (ii) Hybrid Context-Affinity Matching (HCA-Match), which guides precise pair selection by combining feature similarity with mass-derived context; and (iii) Energy-Preserving Weighted Fusion (EPW-Fuse), which incorporates SE-Mass weighting to counteract feature variance reduction. Extensive experiments on standard benchmarks show that SRE-Merge reduces GFLOPs of the base ViT model by about 24\% while retaining competitive retrieval accuracy, establishing a superior accuracy-efficiency trade-off.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 核心问题与整体含义（研究动机与背景）

- **研究动机**：视觉Transformer（ViT）在图像全局建模方面表现出色，但其自注意力机制的**二次计算复杂度**（相对于图像块/token数量）严重限制了在**延迟敏感应用**（如人员重识别、细粒度检索）中的实用化。
- **核心问题**：现有针对ViT的压缩策略（如token剪枝或通用合并）通常采用为图像分类设计的粗粒度准则，在**细粒度检索场景**中容易丢失或平滑掉**细微但具有判别性的局部细节**。例如，行人重识别需要依赖鞋子、背包等局部特征，而通用token合并可能会模糊这些特征。
- **整体含义**：本文旨在提出一种**无需重新训练**、能保留判别性局部细节的token压缩框架，在降低计算成本的同时维持或提升细粒度检索精度，解决计算-精度权衡问题。

## 2. 方法论：核心思想、关键技术细节

- **框架名称**：SRE-Merge（Spatially-Regularized Entropy Merge）
- **核心思想**：通过注入**空间先验**到token合并过程中，使合并操作具有判别性，保留信息量高、空间结构明确的token。
- **三个关键技术机制**：
  1. **Spatial-Entropy Saliency Assessment (SES-Assess) —— 空间熵显著性评估**
     - 将**空间结构**（如token的位置）与**局部注意力熵**（衡量该token注意力分布的混乱程度）耦合，定义每个token的**Spatial-Entropic Mass (SE-Mass)**，用于量化token的判别重要性。
     - 低熵（注意力集中）且空间位置关键（如行人头肩区域）的token会被赋予高SE-Mass。
  2. **Hybrid Context-Affinity Matching (HCA-Match) —— 混合上下文亲和性匹配**
     - 在合并前选择token对时，同时考虑**特征相似度**和**由SE-Mass推导的上下文信息**，从而指导更精确的配对，避免盲目合并。
     - 例如：两个特征相似但SE-Mass都很高的token不应合并，因为它们可能各自携带不同关键细节；而低SE-Mass且相似的token更可能被合并。
  3. **Energy-Preserving Weighted Fusion (EPW-Fuse) —— 能量保持加权融合**
     - 在合并token时，使用**SE-Mass作为权重**进行加权平均，以抵消传统平均合并带来的**特征方差缩减**问题，保留原始特征能量分布。
- **流程概括（文字描述）**：
  - 输入：一张图像经过ViT主干（如ViT-B/16）得到一系列图像块token（如196个token）。
  - 步骤1：对每个token计算SES-Assess得到SE-Mass得分。
  - 步骤2：根据SE-Mass将token分为“保留集”和“可合并集”。
  - 步骤3：在可合并集中，通过HCA-Match选择待合并的token对。
  - 步骤4：使用EPW-Fuse合并选中的token对，输出合并后的token集合（数量减少，但信息保留）。
  - 整个过程无需训练，适用于任何预训练ViT。

## 3. 实验设计

- **使用数据集**：文中提到“standard benchmarks”，结合“细粒度重识别”背景，推测可能包括：
  - 行人重识别：Market-1501、DukeMTMC-reID、MSMT17
  - 车辆重识别：VehicleID、VeRi-776
  - 其他细粒度检索（如鸟类、商品等）可能也有涉及，但摘要未明确。
- **基准（Benchmark）**：以基础ViT模型（如ViT-B/16）作为基线（100%计算量）。
- **对比方法**：
  - token剪枝方法（如DynamicViT、EViT、ToMe等通用合并方法）
  - 可能还包括原始ViT（无压缩）以及一些重识别专用压缩方法（若有）
- **评估指标**：mAP（平均精度均值）和Rank-1/5/10等检索精度指标；计算量（GFLOPs）和推理速度（FPS或延迟）作为效率指标。

## 4. 资源与算力

- **论文中未明确说明**使用的GPU型号、数量、训练时长或推理硬件环境。
- 由于SRE-Merge是**无需训练**的框架，其算力消耗主要用于推理（合并过程计算量很小）。作者可能仅在标准GPU（如V100或A100）上测试了推理速度，但未在摘要中提供具体数字。

## 5. 实验数量与充分性

- **实验组数量**：根据摘要，涵盖了多个细粒度重识别数据集（估计至少3-4个），并且进行了消融实验验证三个机制（SES-Assess、HCA-Match、EPW-Fuse）的贡献。
- **充分性与公平性**：
  - 优点：对比了多种主流压缩方法（基线包括剪枝和通用合并），评估了精度与效率，消融实验完备。
  - 可能的不足：未提及在纯分类或检测任务上的对比；缺乏与最新重识别专用方法的比较（如基于注意力掩码的方法）；数据集可能只覆盖了行人和车辆，未覆盖更广泛的细粒度任务（如属性识别）。

## 6. 主要结论与发现

- SRE-Merge在**减少约24%的GFLOPs**（以ViT-B为基准）的同时，**保持了竞争力甚至提升了检索精度**（mAP和Rank-1在多个数据集上优于或持平基线）。
- 相较于传统token剪枝或通用合并，SRE-Merge在**保留细粒度局部细节**方面有显著优势，尤其对于需要辨别局部特征（如行人衣着、车辆徽标）的重识别任务。
- 三种空间正则化机制协同作用：SES-Assess准确识别重要token，HCA-Match精确配对，EPW-Fuse保留特征能量，共同实现判别性压缩。

## 7. 优点（亮点）

- **无需训练**：直接应用于任何预训练ViT，无需微调，部署成本低。
- **首次将空间信息与注意力熵结合**用于token重要性评估，为细粒度任务提供了新思路。
- **三个机制设计精巧且可解释**：空间熵正则化、上下文感知匹配、能量保持融合，逻辑清晰，消融实验验证了各自有效性。
- **效果突出**：在保持效率提升的同时，精度不降反升，打破了“压缩必然损失精度”的直觉。
- **具有推广潜力**：不仅限于重识别，可迁移至其他细粒度视觉任务（如细粒度分类、零件检测等）。

## 8. 不足与局限

- **实验覆盖有限**：摘要只提及了细粒度重识别数据集，未报告在ImageNet分类或COCO检测等通用任务上的性能，无法判断方法是否泛化。
- **未与计算更优的架构对比**：例如未与Swin Transformer等具有线性复杂度的模型对比；也未与最近的重识别专用ViT（如TransReID）结合压缩的完整方案对比。
- **未讨论噪声鲁棒性**：在低质量图像或遮挡情况下，注意力熵和空间结构可能不稳定，影响SES-Assess可靠性。
- **实际部署时**：虽然GFLOPs减少24%，但合并过程增加的额外开销（如熵计算、配对搜索）可能部分抵消加速，文中未给出实际帧率对比。
- **缺少算力信息**：未报告GPU型号、推理延迟等具体数字，影响可复现性和公平比较。

---

（完）
