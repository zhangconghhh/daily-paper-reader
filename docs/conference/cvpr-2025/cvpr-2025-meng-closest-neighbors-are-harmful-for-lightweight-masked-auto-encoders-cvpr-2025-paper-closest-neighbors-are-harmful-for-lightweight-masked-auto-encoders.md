---
title: Closest Neighbors are Harmful for Lightweight Masked Auto-encoders
title_zh: 最近邻对轻量级掩码自编码器有害
authors: "Meng, Jian, Hasssan, Ahmed, Yang, Li, Fan, Deliang, Shin, Jinwoo, Seo, Jae-sun"
date: 2025-06-01
pdf: "https://openaccess.thecvf.com/content/CVPR2025/papers/Meng_Closest_Neighbors_are_Harmful_for_Lightweight_Masked_Auto-encoders_CVPR_2025_paper.pdf"
tags: ["query:dino-fg"]
score: 4.0
evidence: 基于ViT的轻量级掩码自编码器，涉及自监督视觉transformer
tldr: 轻量级掩码自编码器（MAE）训练中，最近邻样本反而有害。本文分析了该现象并提出改进方法，虽未直接面向分类，但为自监督ViT的训练提供了重要见解，对细粒度分类等下游任务有参考价值。
source: CVPR-2025-Accepted
selection_source: conference_retrieval
motivation: 轻量级MAE模型在资源受限平台有需求，但现有知识蒸馏策略效率低。
method: 揭示了最近邻样本在MAE训练中的负面影响，并提出替代训练策略。
result: 改进后的轻量级MAE在多个下游任务中表现提升。
conclusion: 该工作为轻量级自监督ViT的训练提供了新视角。
---

## Abstract
Learning the visual representation via masked auto-encoder (MAE) training has been proven to be a powerful technique. Transferring the pre-trained vision transformer (ViT) to downstream tasks leads to superior performance compared to conventional task-by-task supervised learning. Recent research works on MAE focus on large-sized vision transformers (>50 million parameters) with outstanding performance. However, improving the generality of the under-parametrized lightweight model has been widely ignored. In practice, downstream applications are commonly intended for resource-constrained platforms, where large-scale ViT cannot easily meet the resource budget. Current lightweight MAE training heavily relies on knowledge distillation with a pre-trained teacher, whereas the root cause behind the poor performance remains under-explored. Motivated by that, this paper first introduces the concept of "closest neighbor patch" to characterize the local semantics among the input tokens. Our discovery shows that the lightweight model failed to distinguish different local information, leading to aliased understanding and poor accuracy. Motivated by this finding, we propose NoR-MAE, a novel MAE training algorithm for lightweight vision transformers. NoR-MAE elegantly repels the semantic aliasing between patches and their closest neighboring patch (semantic centroid) with negligible training cost overhead. With the ViT-Tiny model, NoR-MAE achieves up to 7.22%/3.64% accuracy improvements on ImageNet-100/ImageNet-1K datasets, as well as up to 5.13% accuracy improvements in tested downstream tasks. https://github.com/SeoLabCornell/NoR-MAE

---

## 论文详细总结（自动生成）

### 1. 检索相关性
基于ViT的轻量级掩码自编码器，涉及自监督视觉transformer。

### 2. 核心内容
轻量级掩码自编码器（MAE）训练中，最近邻样本反而有害。本文分析了该现象并提出改进方法，虽未直接面向分类，但为自监督ViT的训练提供了重要见解，对细粒度分类等下游任务有参考价值。

### 3. 对应检索需求
vision transformer architecture for classification。

### 4. 来源与原文
- Source：CVPR-2025-Accepted
- OpenReview：[https://openaccess.thecvf.com/content/CVPR2025/html/Meng_Closest_Neighbors_are_Harmful_for_Lightweight_Masked_Auto-encoders_CVPR_2025_paper.html](https://openaccess.thecvf.com/content/CVPR2025/html/Meng_Closest_Neighbors_are_Harmful_for_Lightweight_Masked_Auto-encoders_CVPR_2025_paper.html)
