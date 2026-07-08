---
title: "MOS: Modeling Object-Scene Associations in Generalized Category Discovery"
title_zh: "MOS: 建模广义类别发现中的物体-场景关联"
authors: "Peng, Zhengyuan, Ma, Jinpeng, Sun, Zhimin, Yi, Ran, Song, Haichuan, Tan, Xin, Ma, Lizhuang"
date: 2025-06-01
pdf: "https://openaccess.thecvf.com/content/CVPR2025/papers/Peng_MOS_Modeling_Object-Scene_Associations_in_Generalized_Category_Discovery_CVPR_2025_paper.pdf"
tags: ["query:dino-fg"]
score: 4.0
evidence: 广义类别发现分类任务，非细粒度特定
tldr: 针对广义类别发现任务中忽视场景信息的问题，提出MOS方法建模物体-场景关联，利用场景先验改善新类分类性能。方法在多个数据集上提升了已知和新类别的分类准确率，但未专门针对细粒度分类设计。
source: CVPR-2025-Accepted
selection_source: conference_retrieval
motivation: 现有GCD方法忽视场景信息，导致新类分类混淆。
method: 通过建模物体与场景的关联来缓解歧义性挑战。
result: 在标准GCD基准上取得分类性能提升。
conclusion: 场景信息可作为分类的有效先验。
---

## Abstract
Generalized Category Discovery (GCD) is a classification task that aims to classify both base and novel classes in unlabeled images, using knowledge from a labeled dataset. In GCD, previous research overlooks scene information or treats it as noise, reducing its impact during model training. However, in this paper, we argue that scene information should be viewed as a strong prior for inferring novel classes. We attribute the misinterpretation of scene information to a key factor: the Ambiguity Challenge inherent in GCD. Specifically, novel objects in base scenes might be wrongly classified into base categories, while base objects in novel scenes might be mistakenly recognized as novel categories. Once the ambiguity challenge is addressed, scene information can reach its full potential, significantly enhancing the performance of GCD models. To more effectively leverage scene information, we propose the Modeling Object-Scene Associations (MOS) framework, which utilizes a simple MLP-based scene-awareness module to enhance GCD performance. It achieves an exceptional average accuracy improvement of 4% on the challenging fine-grained datasets compared to state-of-the-art methods, emphasizing its superior performance in fine-grained GCD. The code is publicly available at https://github.com/JethroPeng/MOS.

---

## 论文详细总结（自动生成）

### 1. 检索相关性
广义类别发现分类任务，非细粒度特定。

### 2. 核心内容
针对广义类别发现任务中忽视场景信息的问题，提出MOS方法建模物体-场景关联，利用场景先验改善新类分类性能。方法在多个数据集上提升了已知和新类别的分类准确率，但未专门针对细粒度分类设计。

### 3. 对应检索需求
fine-grained classification using vision transformers。

### 4. 来源与原文
- Source：CVPR-2025-Accepted
- OpenReview：[https://openaccess.thecvf.com/content/CVPR2025/html/Peng_MOS_Modeling_Object-Scene_Associations_in_Generalized_Category_Discovery_CVPR_2025_paper.html](https://openaccess.thecvf.com/content/CVPR2025/html/Peng_MOS_Modeling_Object-Scene_Associations_in_Generalized_Category_Discovery_CVPR_2025_paper.html)
