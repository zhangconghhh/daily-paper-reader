---
title: "Attribute-formed Class-specific Concept Space: Endowing Language Bottleneck Model with Better Interpretability and Scalability"
title_zh: 基于属性形成的类特定概念空间：赋予语言瓶颈模型更好的可解释性和可扩展性
authors: "Zhang, Jianyang, Luo, Qianli, Yang, Guowu, Yang, Wenjing, Liu, Weide, Lin, Guosheng, Lv, Fengmao"
date: 2025-06-01
pdf: "https://openaccess.thecvf.com/content/CVPR2025/papers/Zhang_Attribute-formed_Class-specific_Concept_Space_Endowing_Language_Bottleneck_Model_with_Better_CVPR_2025_paper.pdf"
tags: ["query:dino-fg"]
score: 4.0
evidence: 细粒度视觉概念关系
tldr: 针对现有概念瓶颈模型仅学习粗粒度图像-概念关系、产生伪相关和缺乏区域级别解释的问题，本文提出基于解耦最优传输的DOT-CBM框架，通过引入局部图像信息实现细粒度视觉-概念关系挖掘。在多个数据集上的实验表明，该方法有效提升了模型的可靠性和可解释性，为细粒度视觉分类提供了一种新的透明化途径。
source: CVPR-2025-Accepted
selection_source: conference_retrieval
motivation: 现有语言瓶颈模型存在虚假线索推断问题，且无法泛化到未见类别。
method: 提出属性形成类特定概念空间（ALBM），按类组织属性描述作为概念瓶颈。
result: ALBM避免了虚假线索，提升了模型可解释性和对新类别的泛化能力。
conclusion: 通过类特定的概念组织，实现了更可靠的基于概念的图像分类。
---

## Abstract
Language Bottleneck Models (LBMs) are proposed to achieve interpretable image recognition by classifying images based on textual concept bottlenecks. However, current LBMs simply list all concepts together as the bottleneck layer, leading to the spurious cue inference problem and cannot generalized to unseen classes. To address these limitations, we propose the Attribute-formed Language Bottleneck Model (ALBM). ALBM organizes concepts in the attribute-formed class-specific space, where concepts are descriptions of specific attributes for specific classes. In this way, ALBM can avoid the spurious cue inference problem by classifying solely based on the essential concepts of each class. In addition, the cross-class unified attribute set also ensures that the concept spaces of different classes have strong correlations, as a result, the learned concept classifier can be easily generalized to unseen classes. Moreover, to further improve interpretability, we propose Visual Attribute Prompt Learning (VAPL) to extract visual features on fine-grained attributes. Furthermore, to avoid labor-intensive concept annotation, we propose the Description, Summary, and Supplement (DSS) strategy to automatically generate high-quality concept sets with a complete and precise attribute. Extensive experiments on 9 widely used few-shot benchmarks demonstrate the interpretability, transferability, and performance of our approach. The code and collected concept sets are available at https://github.com/tiggers23/ALBM.

---

## 论文详细总结（自动生成）

### 1. 检索相关性
细粒度视觉概念关系。

### 2. 核心内容
针对现有概念瓶颈模型仅学习粗粒度图像-概念关系、产生伪相关和缺乏区域级别解释的问题，本文提出基于解耦最优传输的DOT-CBM框架，通过引入局部图像信息实现细粒度视觉-概念关系挖掘。在多个数据集上的实验表明，该方法有效提升了模型的可靠性和可解释性，为细粒度视觉分类提供了一种新的透明化途径。

### 3. 对应检索需求
Foundation models for fine grained visual classification。

### 4. 来源与原文
- Source：CVPR-2025-Accepted
- OpenReview：[https://openaccess.thecvf.com/content/CVPR2025/html/Zhang_Attribute-formed_Class-specific_Concept_Space_Endowing_Language_Bottleneck_Model_with_Better_CVPR_2025_paper.html](https://openaccess.thecvf.com/content/CVPR2025/html/Zhang_Attribute-formed_Class-specific_Concept_Space_Endowing_Language_Bottleneck_Model_with_Better_CVPR_2025_paper.html)
