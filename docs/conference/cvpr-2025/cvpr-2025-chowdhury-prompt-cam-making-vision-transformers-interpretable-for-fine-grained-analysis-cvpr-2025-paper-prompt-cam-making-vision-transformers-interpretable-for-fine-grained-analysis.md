---
title: "Prompt-CAM: Making Vision Transformers Interpretable for Fine-Grained Analysis"
title_zh: Prompt-CAM：使视觉Transformer可解释用于细粒度分析
authors: "Chowdhury, Arpita, Paul, Dipanjyoti, Mai, Zheda, Gu, Jianyang, Zhang, Ziheng, Mehrab, Kazi Sajeed, Campolongo, Elizabeth G., Rubenstein, Daniel, Stewart, Charles V., Karpatne, Anuj, Berger-Wolf, Tanya, Su, Yu, Chao, Wei-Lun"
date: 2025-06-01
pdf: "https://openaccess.thecvf.com/content/CVPR2025/papers/Chowdhury_Prompt-CAM_Making_Vision_Transformers_Interpretable_for_Fine-Grained_Analysis_CVPR_2025_paper.pdf"
tags: ["query:dino-fg"]
score: 9.0
evidence: 使用DINO模型进行细粒度分类的Prompt-CAM方法
tldr: 针对预训练ViT（如DINO）在细粒度分析中可解释性不足的问题，提出Prompt-CAM方法，通过学习类特定提示生成精细注意力图，准确识别鸟类等相似类别的区分性特征，实验证明其生成的注意力图优于传统Grad-CAM，为细粒度分类提供了可解释的决策依据。
source: CVPR-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-chowdhury-prompt-cam-making-vision-transformers-interpretable-for-fine-grained-analysis-cvpr-2025-paper/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1805, \"height\": 547, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-chowdhury-prompt-cam-making-vision-transformers-interpretable-for-fine-grained-analysis-cvpr-2025-paper/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 823, \"height\": 369, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-chowdhury-prompt-cam-making-vision-transformers-interpretable-for-fine-grained-analysis-cvpr-2025-paper/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1647, \"height\": 621, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-chowdhury-prompt-cam-making-vision-transformers-interpretable-for-fine-grained-analysis-cvpr-2025-paper/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 784, \"height\": 265, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-chowdhury-prompt-cam-making-vision-transformers-interpretable-for-fine-grained-analysis-cvpr-2025-paper/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 1629, \"height\": 830, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-chowdhury-prompt-cam-making-vision-transformers-interpretable-for-fine-grained-analysis-cvpr-2025-paper/fig-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 855, \"height\": 619, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-chowdhury-prompt-cam-making-vision-transformers-interpretable-for-fine-grained-analysis-cvpr-2025-paper/fig-007.webp\", \"caption\": \"\", \"page\": 0, \"index\": 7, \"width\": 537, \"height\": 358, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-chowdhury-prompt-cam-making-vision-transformers-interpretable-for-fine-grained-analysis-cvpr-2025-paper/fig-008.webp\", \"caption\": \"\", \"page\": 0, \"index\": 8, \"width\": 848, \"height\": 361, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-chowdhury-prompt-cam-making-vision-transformers-interpretable-for-fine-grained-analysis-cvpr-2025-paper/fig-009.webp\", \"caption\": \"\", \"page\": 0, \"index\": 9, \"width\": 791, \"height\": 593, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-chowdhury-prompt-cam-making-vision-transformers-interpretable-for-fine-grained-analysis-cvpr-2025-paper/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 641, \"height\": 269, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-chowdhury-prompt-cam-making-vision-transformers-interpretable-for-fine-grained-analysis-cvpr-2025-paper/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 635, \"height\": 162, \"label\": \"Table\"}]"
motivation: 预训练ViT（如DINO）在细粒度分类中提取的判别特征难以通过Grad-CAM等热力图可视化，导致模型不可解释。
method: 提出Prompt-CAM，为每个类别学习可学习的提示符，输入预训练ViT获取分类输出，并利用注意力图生成可解释的类激活图。
result: 在细粒度鸟类数据集上，Prompt-CAM生成的注意力图比Grad-CAM更精准地定位细粒度差异区域。
conclusion: Prompt-CAM有效提升了Vision Transformer在细粒度分类中的可解释性。
---

## Abstract
We present a simple approach to make pre-trained Vision Transformers (ViTs) interpretable for fine-grained analysis, aiming to identify and localize the traits that distinguish visually similar categories, such as bird species. Pre-trained ViTs, such as DINO, have demonstrated remarkable capabilities in extracting localized, discriminative features. However, saliency maps like Grad-CAM often fail to identify these traits, producing blurred, coarse heatmaps that highlight entire objects instead. We propose a novel approach, Prompt Class Attention Map (Prompt-CAM), to address this limitation. Prompt-CAM learns class-specific prompts for a pre-trained ViT and uses the corresponding outputs for classification. To correctly classify an image, the true-class prompt must attend to unique image patches not present in other classes' images (i.e., traits). As a result, the true class's multi-head attention maps reveal traits and their locations. Implementation-wise, Prompt-CAM is almost a "free lunch," requiring only a modification to the prediction head of Visual Prompt Tuning (VPT). This makes Prompt-CAM easy to train and apply, in stark contrast to other interpretable methods that require designing specific models and training processes. Extensive empirical studies on a dozen datasets from various domains (e.g., birds, fishes, insects, fungi, flowers, food, and cars) validate the superior interpretation capability of Prompt-CAM. The source code and demo are available at https://github.com/Imageomics/Prompt_CAM.

---

## 论文详细总结（自动生成）

### 1. 检索相关性
使用DINO模型进行细粒度分类的Prompt-CAM方法。

### 2. 核心内容
针对预训练ViT（如DINO）在细粒度分析中可解释性不足的问题，提出Prompt-CAM方法，通过学习类特定提示生成精细注意力图，准确识别鸟类等相似类别的区分性特征，实验证明其生成的注意力图优于传统Grad-CAM，为细粒度分类提供了可解释的决策依据。

### 3. 对应检索需求
DINO model for fine-grained classification。

### 4. 来源与原文
- Source：CVPR-2025-Accepted
- OpenReview：[https://openaccess.thecvf.com/content/CVPR2025/html/Chowdhury_Prompt-CAM_Making_Vision_Transformers_Interpretable_for_Fine-Grained_Analysis_CVPR_2025_paper.html](https://openaccess.thecvf.com/content/CVPR2025/html/Chowdhury_Prompt-CAM_Making_Vision_Transformers_Interpretable_for_Fine-Grained_Analysis_CVPR_2025_paper.html)
