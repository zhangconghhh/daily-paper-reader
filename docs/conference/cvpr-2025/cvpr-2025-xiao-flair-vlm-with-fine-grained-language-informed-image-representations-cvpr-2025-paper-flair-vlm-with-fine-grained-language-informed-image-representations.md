---
title: "FLAIR: VLM with Fine-grained Language-informed Image Representations"
title_zh: "FLAIR: 具有细粒度语言引导图像表示的视觉语言模型"
authors: "Xiao, Rui, Kim, Sanghwan, Georgescu, Mariana-Iuliana, Akata, Zeynep, Alaniz, Stephan"
date: 2025-06-01
pdf: "https://openaccess.thecvf.com/content/CVPR2025/papers/Xiao_FLAIR_VLM_with_Fine-grained_Language-informed_Image_Representations_CVPR_2025_paper.pdf"
tags: ["query:dino-fg"]
score: 6.0
evidence: 使用视觉语言模型和注意力池化的细粒度图像表示
tldr: 针对CLIP全局匹配丢失细粒度细节的问题，提出FLAIR方法，利用长篇详细描述学习局部化图像嵌入，通过文本条件注意力池化生成细粒度表示。在细粒度检索任务上表现优异，方法基于Transformer架构，但并非直接用于分类。
source: CVPR-2025-Accepted
selection_source: conference_retrieval
motivation: CLIP全局匹配难以捕获细粒度视觉特征。
method: 基于多样子描述和文本条件注意力池化学习细粒度图像表示。
result: 在细粒度检索基准上显著优于CLIP。
conclusion: 提供了增强视觉语言模型细粒度能力的新途径。
---

## Abstract
CLIP has shown impressive results in aligning images and text at scale. However, its ability to capture detailed visual features remains limited because CLIP matches images and texts at a global level. To address this issue, we propose FLAIR, Fine-grained Language-informed Image Representations, an approach that utilizes long and detailed image descriptions to learn localized image embeddings. By sampling diverse sub-captions that describe fine-grained details about an image, we train our vision-language model to produce not only global embeddings but also text-specific image representations. Our model introduces text-conditioned attention pooling on top of local image tokens to produce fine-grained image representations that excel at retrieving detailed image content. We achieve state-of-the-art performance on both, existing multimodal retrieval benchmarks, as well as, our newly introduced fine-grained retrieval task which evaluates vision-language models' ability to retrieve partial image content. Furthermore, our experiments demonstrate the effectiveness of FLAIR trained on 30M image-text pairs in capturing fine-grained visual information, including zero-shot semantic segmentation, outperforming models trained on billions of pairs. Code is available at: https://github.com/ExplainableML/flair.

---

## 论文详细总结（自动生成）

### 1. 检索相关性
使用视觉语言模型和注意力池化的细粒度图像表示。

### 2. 核心内容
针对CLIP全局匹配丢失细粒度细节的问题，提出FLAIR方法，利用长篇详细描述学习局部化图像嵌入，通过文本条件注意力池化生成细粒度表示。在细粒度检索任务上表现优异，方法基于Transformer架构，但并非直接用于分类。

### 3. 对应检索需求
fine-grained classification using vision transformers。

### 4. 来源与原文
- Source：CVPR-2025-Accepted
- OpenReview：[https://openaccess.thecvf.com/content/CVPR2025/html/Xiao_FLAIR_VLM_with_Fine-grained_Language-informed_Image_Representations_CVPR_2025_paper.html](https://openaccess.thecvf.com/content/CVPR2025/html/Xiao_FLAIR_VLM_with_Fine-grained_Language-informed_Image_Representations_CVPR_2025_paper.html)
