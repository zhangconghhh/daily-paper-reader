---
title: "Adaptive Part Learning for Fine-Grained Generalized Category Discovery: A Plug-and-Play Enhancement"
title_zh: 自适应部件学习用于细粒度广义类别发现：一种即插即用增强方法
authors: "Dai, Qiyuan, Huang, Hanzhuo, Wu, Yu, Yang, Sibei"
date: 2025-06-01
pdf: "https://openaccess.thecvf.com/content/CVPR2025/papers/Dai_Adaptive_Part_Learning_for_Fine-Grained_Generalized_Category_Discovery_A_Plug-and-Play_CVPR_2025_paper.pdf"
tags: ["query:dino-fg"]
score: 9.0
evidence: 明确使用DINO进行细粒度广义类别发现并提出部件学习增强
tldr: 针对现有基于DINO的广义类别发现方法仅使用全局CLS令牌导致可判别性和泛化性权衡的问题，提出自适应部件学习方法APL。通过一组可学习部件查询生成一致的对象部件及其对应关系，提升细粒度特征表示。在多个细粒度数据集上显著提升了新类发现性能，是DINO用于细粒度分类的直接改进方法。
source: CVPR-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-dai-adaptive-part-learning-for-fine-grained-generalized-category-discovery-a-plug-and-play-cvpr-2025-paper/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1613, \"height\": 480, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-dai-adaptive-part-learning-for-fine-grained-generalized-category-discovery-a-plug-and-play-cvpr-2025-paper/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1288, \"height\": 410, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-dai-adaptive-part-learning-for-fine-grained-generalized-category-discovery-a-plug-and-play-cvpr-2025-paper/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 723, \"height\": 504, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-dai-adaptive-part-learning-for-fine-grained-generalized-category-discovery-a-plug-and-play-cvpr-2025-paper/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 675, \"height\": 585, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-dai-adaptive-part-learning-for-fine-grained-generalized-category-discovery-a-plug-and-play-cvpr-2025-paper/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 406, \"height\": 277, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-dai-adaptive-part-learning-for-fine-grained-generalized-category-discovery-a-plug-and-play-cvpr-2025-paper/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 817, \"height\": 253, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-dai-adaptive-part-learning-for-fine-grained-generalized-category-discovery-a-plug-and-play-cvpr-2025-paper/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1670, \"height\": 774, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-dai-adaptive-part-learning-for-fine-grained-generalized-category-discovery-a-plug-and-play-cvpr-2025-paper/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 617, \"height\": 279, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-dai-adaptive-part-learning-for-fine-grained-generalized-category-discovery-a-plug-and-play-cvpr-2025-paper/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 701, \"height\": 275, \"label\": \"Table\"}]"
motivation: DINO的全局CLS令牌在细粒度分类中存在可判别性与泛化性的权衡。
method: 提出自适应部件学习方法，通过共享可学习部件查询生成一致的对象部件特征。
result: 在CUB、Stanford Cars等细粒度数据集上取得GCD新SOTA。
conclusion: 为DINO在细粒度分类中的应用提供了有效增强。
---

## Abstract
Generalized Category Discovery (GCD) aims to recognize unlabeled images from known and novel classes by distinguishing novel classes from known ones, while also transferring knowledge from another set of labeled images with known classes. Existing GCD methods rely on self-supervised vision transformers such as DINO for representation learning. However, focusing solely on the global representation of the DINO CLS token introduces an inherent trade-off between discriminability and generalization. In this paper, we introduce an adaptive part discovery and learning method, called APL, which generates consistent object parts and their correspondences across different similar images using a set of shared learnable part queries and DINO part priors, without requiring any additional annotations. More importantly, we propose a novel all-min contrastive loss to learn discriminative yet generalizable part representation, which adaptively highlights discriminative object parts to distinguish similar categories for enhanced discriminability while simultaneously sharing other parts to facilitate knowledge transfer for improved generalization. Our APL can easily be incorporated into different GCD frameworks by replacing their CLS token feature with our part representations, showing significant enhancements on fine-grained datasets.

---

## 论文详细总结（自动生成）

### 1. 检索相关性
明确使用DINO进行细粒度广义类别发现并提出部件学习增强。

### 2. 核心内容
针对现有基于DINO的广义类别发现方法仅使用全局CLS令牌导致可判别性和泛化性权衡的问题，提出自适应部件学习方法APL。通过一组可学习部件查询生成一致的对象部件及其对应关系，提升细粒度特征表示。在多个细粒度数据集上显著提升了新类发现性能，是DINO用于细粒度分类的直接改进方法。

### 3. 对应检索需求
DINO fine grained classification papers。

### 4. 来源与原文
- Source：CVPR-2025-Accepted
- OpenReview：[https://openaccess.thecvf.com/content/CVPR2025/html/Dai_Adaptive_Part_Learning_for_Fine-Grained_Generalized_Category_Discovery_A_Plug-and-Play_CVPR_2025_paper.html](https://openaccess.thecvf.com/content/CVPR2025/html/Dai_Adaptive_Part_Learning_for_Fine-Grained_Generalized_Category_Discovery_A_Plug-and-Play_CVPR_2025_paper.html)
