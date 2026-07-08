---
title: "Beyond Words: Augmenting Discriminative Richness via Diffusions in Unsupervised Prompt Learning"
title_zh: 超越文字：通过扩散在无监督提示学习中增强判别丰富性
authors: "Ren, Hairui, Tang, Fan, Zhao, He, Wang, Zixuan, Guo, Dandan, Chang, Yi"
date: 2025-06-01
pdf: "https://openaccess.thecvf.com/content/CVPR2025/papers/Ren_Beyond_Words_Augmenting_Discriminative_Richness_via_Diffusions_in_Unsupervised_Prompt_CVPR_2025_paper.pdf"
tags: ["query:dino-fg"]
score: 6.0
evidence: 通过扩散增强提示进行视觉Transformer分类
tldr: 针对无监督提示学习中伪标签质量差的问题，提出AiR方法，利用扩散模型生成高保真伪标签，增强分类判别性。在多个分类基准上的实验表明，该方法显著提升了无监督提示学习的准确率，无需任何标注数据即可有效微调视觉-语言模型。
source: CVPR-2025-Accepted
selection_source: conference_retrieval
motivation: 现有无监督提示学习方法中伪标签存在语义与视觉信息不匹配，导致分类性能次优。
method: 提出AiR方法，包含基于高保真合成图像的伪标签生成模块，以及通过扩散模型增强类别表示的判别性学习策略。
result: 在多个图像分类数据集上，AiR显著优于现有无监督提示学习方法，表明扩散机制能有效提升分类判别性。
conclusion: 该方法展示了扩散模型在无监督分类中的潜力，为无需标注数据的视觉-语言模型微调提供了新思路。
---

## Abstract
Fine-tuning vision-language models (VLMs) with large amounts of unlabeled data has recently garnered significant interest. However, a key challenge remains the lack of high-quality pseudo-labeled data. Current pseudo-labeling strategies often struggle with mismatches between semantic and visual information, leading to sub-optimal performance of unsupervised prompt learning (UPL) methods.In this paper, we introduce a simple yet effective approach called Augmenting Discriminative Richness via Diffusions (AiR), toward learning a richer discriminating way to represent the class comprehensively and thus facilitate classification.Specifically, our approach includes a pseudo-label generation module that leverages high-fidelity synthetic samples to create an auxiliary classifier, which captures richer visual variation, bridging text-image-pair classification to a more robust image-image-pair classification. Additionally, we exploit the diversity of diffusion-based synthetic samples to enhance prompt learning, providing greater information for semantic-visual alignment.Extensive experiments on five public benchmarks, including RESISC45 and Flowers102, and across three learning paradigms-UL, SSL, and TRZSL-demonstrate that AiR achieves substantial and consistent performance improvements over state-of-the-art unsupervised prompt learning methods.

---

## 论文详细总结（自动生成）

### 1. 检索相关性
通过扩散增强提示进行视觉Transformer分类。

### 2. 核心内容
针对无监督提示学习中伪标签质量差的问题，提出AiR方法，利用扩散模型生成高保真伪标签，增强分类判别性。在多个分类基准上的实验表明，该方法显著提升了无监督提示学习的准确率，无需任何标注数据即可有效微调视觉-语言模型。

### 3. 对应检索需求
vision transformer architecture for classification。

### 4. 来源与原文
- Source：CVPR-2025-Accepted
- OpenReview：[https://openaccess.thecvf.com/content/CVPR2025/html/Ren_Beyond_Words_Augmenting_Discriminative_Richness_via_Diffusions_in_Unsupervised_Prompt_CVPR_2025_paper.html](https://openaccess.thecvf.com/content/CVPR2025/html/Ren_Beyond_Words_Augmenting_Discriminative_Richness_via_Diffusions_in_Unsupervised_Prompt_CVPR_2025_paper.html)
