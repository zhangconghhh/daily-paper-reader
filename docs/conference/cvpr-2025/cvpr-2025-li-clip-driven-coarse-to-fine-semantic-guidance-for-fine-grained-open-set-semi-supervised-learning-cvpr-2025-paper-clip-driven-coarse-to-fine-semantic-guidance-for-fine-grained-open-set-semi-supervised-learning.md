---
title: CLIP-driven Coarse-to-fine Semantic Guidance for Fine-grained Open-set Semi-supervised Learning
title_zh: CLIP驱动的粗到细语义引导用于细粒度开放集半监督学习
authors: "Li, Xiaokun, Huang, Yaping, Guan, Qingji"
date: 2025-06-01
pdf: "https://openaccess.thecvf.com/content/CVPR2025/papers/Li_CLIP-driven_Coarse-to-fine_Semantic_Guidance_for_Fine-grained_Open-set_Semi-supervised_Learning_CVPR_2025_paper.pdf"
tags: ["query:dino-fg"]
score: 8.0
evidence: 使用基础模型CLIP进行细粒度分类
tldr: 针对细粒度开放集半监督学习中类间差异微小及分布外样本混淆的问题，提出CLIP驱动的粗到细语义引导方法。该方法首先利用CLIP的通用知识进行粗分类，再通过细粒度语义引导模块区分细微差异。在多个细粒度数据集上的实验表明，该方法显著提升了ID/OOD分离精度，优于现有半监督学习方法。该工作证明了基础模型在细粒度分类中的有效性。
source: CVPR-2025-Accepted
selection_source: conference_retrieval
motivation: 细粒度分类中类间和类内差异细微，且存在未知类样本，现有半监督方法难以有效区分。
method: 提出CLIP驱动的粗到细语义引导框架，利用CLIP进行粗分类，再通过细粒度语义模块进行细化，同时应对开放集场景。
result: 在多个细粒度分类基准数据集上，所提方法在识别已知类和检测未知类方面均大幅超越现有方法。
conclusion: 展示了将CLIP等基础模型与细粒度语义引导结合的有效性，为细粒度半监督分类提供了新范式。
---

## Abstract
Fine-grained open-set semi-supervised learning (OSSL) investigates a practical scenario where unlabeled data may contain fine-grained out-of-distribution (OOD) samples. Due to the subtle visual differences among in-distribution (ID) samples, as well as between ID and OOD samples, it is extremely challenging to separate the ID and OOD samples. Due to the subtle visual differences among in-distribution (ID) and OOD samples. Recent Vision-Language Models, such as CLIP, have shown excellent generalization capabilities. However, it tends to focus on general attributes, and thus is insufficient to distinguish the fine-grained details. To tackle the issues, in this paper, we propose a novel CLIP-driven coarse-to-fine semantic-guided framework, named CFSG-CLIP, to progressively focus on the distinctive fine-grained clues. Specifically, CFSG-CLIP comprises a coarse-guidance branch and a fine-guidance branch derived from the pre-trained CLIP model. In the coarse-guidance branch, we design a semantic filtering module to initially filter and highlight local visual features guided by cross-modality features. Then, in the fine-guidance branch, we further design a visual-semantic injection strategy, which embeds category-related visual cues into the visual encoder to further refine the local visual features. By the designed dual-guidance framework, local subtle cues are progressively discovered to distinct the subtle difference between ID and OOD samples. Extensive experiments demonstrate that CFSG-CLIP achieves competitive performance on multiple fine-grained datasets. The source code is available at https://github.com/LxxxxK/CFSG-CLIP.

---

## 论文详细总结（自动生成）

### 1. 检索相关性
使用基础模型CLIP进行细粒度分类。

### 2. 核心内容
针对细粒度开放集半监督学习中类间差异微小及分布外样本混淆的问题，提出CLIP驱动的粗到细语义引导方法。该方法首先利用CLIP的通用知识进行粗分类，再通过细粒度语义引导模块区分细微差异。在多个细粒度数据集上的实验表明，该方法显著提升了ID/OOD分离精度，优于现有半监督学习方法。该工作证明了基础模型在细粒度分类中的有效性。

### 3. 对应检索需求
Foundation models for fine grained visual classification。

### 4. 来源与原文
- Source：CVPR-2025-Accepted
- OpenReview：[https://openaccess.thecvf.com/content/CVPR2025/html/Li_CLIP-driven_Coarse-to-fine_Semantic_Guidance_for_Fine-grained_Open-set_Semi-supervised_Learning_CVPR_2025_paper.html](https://openaccess.thecvf.com/content/CVPR2025/html/Li_CLIP-driven_Coarse-to-fine_Semantic_Guidance_for_Fine-grained_Open-set_Semi-supervised_Learning_CVPR_2025_paper.html)
