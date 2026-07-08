---
title: "TADFormer: Task-Adaptive Dynamic TransFormer for Efficient Multi-Task Learning"
title_zh: TADFormer：面向高效多任务学习的任务自适应动态transformer
authors: "Baek, Seungmin, Lee, Soyul, Jo, Hayeon, Choi, Hyesong, Min, Dongbo"
date: 2025-06-01
pdf: "https://openaccess.thecvf.com/content/CVPR2025/papers/Baek_TADFormer_Task-Adaptive_Dynamic_TransFormer_for_Efficient_Multi-Task_Learning_CVPR_2025_paper.pdf"
tags: ["query:dino-fg"]
score: 7.0
evidence: 任务自适应动态transformer实现细粒度特征适应，与使用ViT进行细粒度分类相关
tldr: 多任务学习中，参数高效微调方法难以捕获细粒度任务特定特征。本文提出TADFormer，一种任务自适应动态transformer，通过动态特征适应在细粒度级别进行任务感知调整。虽然专注多任务学习，其细粒度特征适应机制可直接迁移至细粒度分类任务，为使用ViT进行细粒度分类提供了高效微调思路。
source: CVPR-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-baek-tadformer-task-adaptive-dynamic-transformer-for-efficient-multi-task-learning-cvpr-2025-paper/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 862, \"height\": 717, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-baek-tadformer-task-adaptive-dynamic-transformer-for-efficient-multi-task-learning-cvpr-2025-paper/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 853, \"height\": 478, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-baek-tadformer-task-adaptive-dynamic-transformer-for-efficient-multi-task-learning-cvpr-2025-paper/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 773, \"height\": 580, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-baek-tadformer-task-adaptive-dynamic-transformer-for-efficient-multi-task-learning-cvpr-2025-paper/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 1788, \"height\": 875, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-baek-tadformer-task-adaptive-dynamic-transformer-for-efficient-multi-task-learning-cvpr-2025-paper/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 527, \"height\": 350, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-baek-tadformer-task-adaptive-dynamic-transformer-for-efficient-multi-task-learning-cvpr-2025-paper/fig-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 599, \"height\": 374, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-baek-tadformer-task-adaptive-dynamic-transformer-for-efficient-multi-task-learning-cvpr-2025-paper/fig-007.webp\", \"caption\": \"\", \"page\": 0, \"index\": 7, \"width\": 771, \"height\": 492, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-baek-tadformer-task-adaptive-dynamic-transformer-for-efficient-multi-task-learning-cvpr-2025-paper/fig-008.webp\", \"caption\": \"\", \"page\": 0, \"index\": 8, \"width\": 779, \"height\": 484, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-baek-tadformer-task-adaptive-dynamic-transformer-for-efficient-multi-task-learning-cvpr-2025-paper/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1554, \"height\": 806, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-baek-tadformer-task-adaptive-dynamic-transformer-for-efficient-multi-task-learning-cvpr-2025-paper/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1816, \"height\": 462, \"label\": \"Table\"}]"
motivation: 现有参数高效微调在多任务学习中无法捕获细粒度任务特定特征。
method: 设计任务自适应动态transformer，在细粒度级别进行任务感知特征适应。
result: 在多任务学习基准上达到高效且精细的适应效果。
conclusion: 其细粒度适应能力对单任务细粒度分类具有借鉴意义。
---

## Abstract
Transfer learning paradigm has driven substantial advancements in various vision tasks. However, as state-of-the-art models continue to grow, classical full fine-tuning often becomes computationally impractical, particularly in multi-task learning (MTL) setup where training complexity increases proportional to the number of tasks. Consequently, recent studies have explored Parameter-Efficient Fine-Tuning (PEFT) for MTL architectures. Despite some progress, these approaches still exhibit limitations in capturing fine-grained, task-specific features that are crucial to MTL. In this paper, we introduce Task-Adaptive Dynamic transFormer, termed TADFormer, a novel PEFT framework that performs task-aware feature adaptation in the fine-grained manner by dynamically considering task-specific input contexts. TADFormer proposes the parameter-efficient prompting for task adaptation and the Dynamic Task Filter (DTF) to capture task information conditioned on input contexts. Experiments on the PASCAL-Context benchmark demonstrate that the proposed method achieves higher accuracy in dense scene understanding tasks, while reducing the number of trainable parameters by up to 8.4 times when compared to full fine-tuning of MTL models. TADFormer also demonstrates superior parameter efficiency and accuracy compared to recent PEFT methods. Our code is available at supplementary material.

---

## 论文详细总结（自动生成）

### 1. 检索相关性
任务自适应动态transformer实现细粒度特征适应，与使用ViT进行细粒度分类相关。

### 2. 核心内容
多任务学习中，参数高效微调方法难以捕获细粒度任务特定特征。本文提出TADFormer，一种任务自适应动态transformer，通过动态特征适应在细粒度级别进行任务感知调整。虽然专注多任务学习，其细粒度特征适应机制可直接迁移至细粒度分类任务，为使用ViT进行细粒度分类提供了高效微调思路。

### 3. 对应检索需求
fine-grained classification using vision transformers。

### 4. 来源与原文
- Source：CVPR-2025-Accepted
- OpenReview：[https://openaccess.thecvf.com/content/CVPR2025/html/Baek_TADFormer_Task-Adaptive_Dynamic_TransFormer_for_Efficient_Multi-Task_Learning_CVPR_2025_paper.html](https://openaccess.thecvf.com/content/CVPR2025/html/Baek_TADFormer_Task-Adaptive_Dynamic_TransFormer_for_Efficient_Multi-Task_Learning_CVPR_2025_paper.html)
