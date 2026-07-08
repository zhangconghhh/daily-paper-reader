---
title: Discovering Fine-Grained Visual-Concept Relations by Disentangled Optimal Transport Concept Bottleneck Models
title_zh: 通过解耦最优传输概念瓶颈模型发现细粒度视觉概念关系
authors: "Xie, Yan, Zeng, Zequn, Zhang, Hao, Ding, Yucheng, Wang, Yi, Wang, Zhengjue, Chen, Bo, Liu, Hongwei"
date: 2025-06-01
pdf: "https://openaccess.thecvf.com/content/CVPR2025/papers/Xie_Discovering_Fine-Grained_Visual-Concept_Relations_by_Disentangled_Optimal_Transport_Concept_Bottleneck_CVPR_2025_paper.pdf"
tags: ["query:dino-fg"]
score: 5.0
evidence: 细粒度视觉概念关系发现用于可解释分类
tldr: 针对现有概念瓶颈模型仅学习粗粒度图像-概念关系、产生伪相关和缺乏区域级别解释的问题，本文提出基于解耦最优传输的DOT-CBM框架，通过引入局部图像信息实现细粒度视觉-概念关系挖掘。在多个数据集上的实验表明，该方法有效提升了模型的可靠性和可解释性，为细粒度视觉分类提供了一种新的透明化途径。
source: CVPR-2025-Accepted
selection_source: conference_retrieval
motivation: 现有概念瓶颈模型忽视局部信息，导致伪相关和缺乏区域级解释。
method: 提出解耦最优传输概念瓶颈模型（DOT-CBM），利用局部图像信息发现细粒度视觉-概念关系。
result: 在多个基准上提升了模型可靠性和可解释性。
conclusion: 细粒度视觉概念关系挖掘能有效增强概念瓶颈模型的透明度和可靠性。
---

## Abstract
Concept Bottleneck Models (CBMs) try to make the decision-making process transparent by exploring an intermediate concept space between the input image and the output prediction. Existing CBMs just learn coarse-grained relations between the whole image and the concepts, less considering local image information, leading to two main drawbacks: i) they often produce spurious visual-concept relations, hence decreasing model reliability; and ii) though CBMs could explain the importance of every concept to the final prediction, it is still challenging to tell which visual region produces the prediction. To solve these problems, this paper proposes a Disentangled Optimal Transport CBM (DOT-CBM) framework to explore fine-grained visual-concept relations between local image patches and concepts. Specifically, we model the concept prediction process as a transportation problem between the patches and concepts, thereby achieving explicit fine-grained feature alignment. We also incorporate orthogonal projection losses within the modality to enhance local feature disentanglement. To further address the shortcut issues caused by statistical biases in the data, we utilize the visual saliency map and concept label statistics as transportation priors. Thus, DOT-CBM can visualize inversion heatmaps, provide more reliable concept predictions, and produce more accurate class predictions. Comprehensive experiments demonstrate that our proposed DOT-CBM achieves SOTA performance on several tasks, including image classification, local part detection and out-of-distribution generalization.

---

## 论文详细总结（自动生成）

### 1. 检索相关性
细粒度视觉概念关系发现用于可解释分类。

### 2. 核心内容
针对现有概念瓶颈模型仅学习粗粒度图像-概念关系、产生伪相关和缺乏区域级别解释的问题，本文提出基于解耦最优传输的DOT-CBM框架，通过引入局部图像信息实现细粒度视觉-概念关系挖掘。在多个数据集上的实验表明，该方法有效提升了模型的可靠性和可解释性，为细粒度视觉分类提供了一种新的透明化途径。

### 3. 对应检索需求
fine-grained classification using vision transformers。

### 4. 来源与原文
- Source：CVPR-2025-Accepted
- OpenReview：[https://openaccess.thecvf.com/content/CVPR2025/html/Xie_Discovering_Fine-Grained_Visual-Concept_Relations_by_Disentangled_Optimal_Transport_Concept_Bottleneck_CVPR_2025_paper.html](https://openaccess.thecvf.com/content/CVPR2025/html/Xie_Discovering_Fine-Grained_Visual-Concept_Relations_by_Disentangled_Optimal_Transport_Concept_Bottleneck_CVPR_2025_paper.html)
