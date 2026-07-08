---
title: Language-Assisted Debiasing and Smoothing for Foundation Model-Based Semi-Supervised Learning
title_zh: 语言辅助去偏与平滑：应用于基于基础模型的半监督学习
authors: "Zheng, Na, Song, Xuemeng, Dong, Xue, Ghosh, Aashish Nikhil, Nie, Liqiang, Zimmermann, Roger"
date: 2025-06-01
pdf: "https://openaccess.thecvf.com/content/CVPR2025/papers/Zheng_Language-Assisted_Debiasing_and_Smoothing_for_Foundation_Model-Based_Semi-Supervised_Learning_CVPR_2025_paper.pdf"
tags: ["query:dino-fg"]
score: 4.0
evidence: 基于基础模型的半监督学习与语言辅助去偏
tldr: 针对基础模型在半监督学习中产生偏置伪标签的问题，提出语言辅助去偏与平滑方法。利用语言知识估计偏置偏移量，并对伪标签进行平滑处理，有效提升了未标记样本的利用效率和模型性能。相关工作为将基础模型应用于图像分类提供了有益借鉴。
source: CVPR-2025-Accepted
selection_source: conference_retrieval
motivation: 基础模型在半监督学习中存在类别偏置，导致伪标签不平衡。
method: 利用语言知识辅助估计偏置偏移，并引入平滑机制优化伪标签。
result: 在多个半监督学习基准上提升了分类准确率。
conclusion: 语言先验可以有效纠正基础模型的偏置，提升半监督学习效果。
---

## Abstract
Recent studies have focused on introducing pre-trained foundation models into semi-supervised learning (SSL) tasks. Nevertheless, these foundation models can exhibit biases toward different classes and tend to generate imbalanced pseudo-labels for SSL. Thus, efforts have been made to introduce the logit adjustment offset to reduce the inherent bias in foundation models for SSL tasks.Despite their success, existing foundation model-based SSL methods face challenges: 1) unreliability in the estimated logit adjustment offset, 2) overlooking the potential of linguistic knowledge in capturing model biases and 3) fail to fully exploit the unlabeled samples. To address these issues, we propose Language-Assisted Debiasing and Smoothing framework, namely LADaS, for foundation model-based SSL. It consists of two components: 1) Language-assisted Pseudo-Label Debiasing (LPLD) to reduce biases in foundation models, and 2) Language-aware Pseudo-Label Smoothing (LPLS) to fully exploit low-confidence samples to facilitate SSL training. In particular, LPLD introduces a reliability score to dynamically assess the reliability of the logit adjustment. Additionally, it incorporates a language-oriented preference to reduce model biases using linguistic knowledge derived from pre-trained language models. Finally, LPLS introduces language-aware soft labels and devises language-aware pseudo-label smoothing loss to guide the learning of unlabeled samples with low-quality pseudo-labels. Extensive experiments demonstrate the superiority of our proposed LADaS.

---

## 论文详细总结（自动生成）

### 1. 检索相关性
基于基础模型的半监督学习与语言辅助去偏。

### 2. 核心内容
针对基础模型在半监督学习中产生偏置伪标签的问题，提出语言辅助去偏与平滑方法。利用语言知识估计偏置偏移量，并对伪标签进行平滑处理，有效提升了未标记样本的利用效率和模型性能。相关工作为将基础模型应用于图像分类提供了有益借鉴。

### 3. 对应检索需求
foundation model for image classification。

### 4. 来源与原文
- Source：CVPR-2025-Accepted
- OpenReview：[https://openaccess.thecvf.com/content/CVPR2025/html/Zheng_Language-Assisted_Debiasing_and_Smoothing_for_Foundation_Model-Based_Semi-Supervised_Learning_CVPR_2025_paper.html](https://openaccess.thecvf.com/content/CVPR2025/html/Zheng_Language-Assisted_Debiasing_and_Smoothing_for_Foundation_Model-Based_Semi-Supervised_Learning_CVPR_2025_paper.html)
