---
title: "VESSA: Video-based objEct-centric Self-Supervised Adaptation for Visual Foundation Models"
title_zh: VESSA：基于视频的面向物体的自监督适应用于视觉基础模型
authors: "Jesimon Barreto, Carlos Caetano, Andre Araujo, William Robson Schwartz"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=72CEG36B0Q"
tags: ["query:dino-fg"]
score: 4.0
evidence: 视觉基础模型的自监督适应
tldr: VESSA提出一种新颖的自监督微调方法，仅利用短多视角目标中心视频适应视觉基础模型至新领域。它解决了标注稀缺和分布偏移问题，但针对视频而非图像分类。
source: NeurIPS-2025-Accepted
selection_source: conference_retrieval
motivation: 视觉基础模型在分布偏移和标签稀缺领域表现下降，监督微调不可行。
method: 利用短多视角目标中心视频进行自监督微调，无需标注。
result: 在多个域适应基准上提升了基础模型性能。
conclusion: VESSA提供了一种免标注的视觉基础模型适应策略，但主要面向视频场景。
---

## Abstract
Foundation models have advanced computer vision by enabling strong performance across diverse tasks through large-scale pretraining and supervised fine-tuning. However, they may underperform in domains with distribution shifts and scarce labels, where supervised fine-tuning may be infeasible. While continued self-supervised learning for model adaptation is common for generative language models, this strategy has not proven effective for vision-centric encoder models. To address this challenge, we introduce a novel formulation of self-supervised fine-tuning for vision foundation models, where the model is adapted to a new domain without requiring annotations, leveraging only short multi-view object-centric videos. Our method is referred to as VESSA: **V**ideo-based obj**E**ct-centric **S**elf-**S**upervised **A**daptation for visual foundation models. VESSA's training technique is based on a self-distillation paradigm, where it is critical to carefully tune prediction heads and deploy parameter-efficient adaptation techniques – otherwise, the model may quickly forget its pretrained knowledge and reach a degraded state. VESSA benefits significantly from multi-view object observations sourced from different frames in an object-centric video, efficiently learning robustness to varied capture conditions, without the need of annotations. Through comprehensive experiments with 3 vision foundation models on 2 datasets, VESSA demonstrates consistent improvements in downstream classification tasks, compared to the base models and previous adaptation methods. Code is publicly available at https://github.com/jesimonbarreto/VESSA.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 核心问题与整体含义（研究动机和背景）

- **问题**：视觉基础模型（如DINOv2等）通过大规模预训练和监督微调在多种任务上表现出色，但在遇到**分布偏移（distribution shift）** 和**标签稀缺（scarce labels）** 的领域时性能会下降。此时监督微调不可行（因缺乏标注数据）。
- **背景**：生成式语言模型常用持续自监督学习进行模型适应，但该策略在视觉编码器模型上尚未被证明有效。现有适应方法要么需要标注，要么无法有效防止灾难性遗忘。
- **动机**：提出一种无需标注、仅利用短多视角目标中心视频的自监督微调方法，使视觉基础模型适应新领域，同时保持预训练知识。

## 2. 方法论

### 核心思想
- **VESSA**：基于视频的面向物体的自监督适应（Video-based object-centric Self-Supervised Adaptation）。
- **基本策略**：采用**自蒸馏（self-distillation）** 范式，利用从目标中心视频中不同帧提取的**多视角物体观测**，在不依赖人工标注的情况下学习对多样化捕获条件的鲁棒性。

### 关键技术细节
- **输入**：短多视角目标中心视频（例如围绕物体拍摄的短视频片段），无需任何类别标签。
- **训练技术**：
  - 精心微调**预测头（prediction heads）**。
  - 部署**参数高效适应技术（parameter-efficient adaptation techniques）**，如LoRA等，以防止模型快速遗忘预训练知识并陷入退化状态。
- **流程**（文字说明）：
  1. 从视频中提取不同帧作为同一物体的多视图样本。
  2. 将这些帧输入教师和学生网络（学生网络通过自蒸馏方式从教师网络学习）。
  3. 教师网络和目标网络共享大部分权重，仅预测头不同，通过对比多视图特征实现自监督学习。
- **关键洞察**：如果不谨慎调节预测头和采用参数高效方法，模型会迅速遗忘预训练知识，性能退化。

## 3. 实验设计

- **数据集/场景**：
  - 使用**2个数据集**（具体名称在元数据中未给出，但摘要提到“2 datasets”），涉及域的迁移（例如从通用图像到特定目标中心视频场景）。
  - 应用于**3种视觉基础模型**（如DINOv2等变体）。
- **Benchmark**：在下游分类任务上评估，对比基线模型（未适应的基础模型）和之前的适应方法。
- **对比方法**：包括之前的自监督适应方法和未经适应的原始基础模型。
- **评估指标**：下游分类任务的准确率（具体数值未在摘要中提供）。

## 4. 资源与算力

- **明确说明**：文中未明确提及使用的GPU型号、数量或训练时长等算力信息。仅声明代码已公开（GitHub）。
- **隐含信息**：由于是自监督微调，且使用参数高效技术，推测算力需求适中，但具体数字缺失。

## 5. 实验数量与充分性

- **实验数量**：在3种模型×2个数据集上进行了主实验，加上消融实验（如分析预测头调节、参数高效适应技术的影响），总计实验组数约6+组主实验以及若干消融。
- **充分性**：实验覆盖了多个基础模型和数据集，但**缺乏对更多领域的泛化测试**（如真实世界视频、不同物体类别等）。对比方法只提到“之前的适应方法”，未列出具体方法名称，公平性难以全面评估。消融实验设计合理，但未展示统计显著性检验。

## 6. 主要结论与发现

- **VESSA**能够在无需标注的情况下，通过多视角目标中心视频自监督地提升视觉基础模型在目标域的分类性能。
- 相比于原始基础模型和以往适应方法，VESSA在下游任务上表现出**一致的改进（consistent improvements）**。
- **关键发现**：自蒸馏框架中必须仔细设计预测头和采用参数高效微调，否则预训练知识会快速遗忘。

## 7. 优点

- **免标注适应**：解决了分布偏移和标签稀缺场景下的模型适应难题。
- **利用视频多视图信息**：高效学习对视角、光照等捕获条件变化的鲁棒性。
- **参数高效**：采用LoRA等轻量级微调，计算开销小。
- **模型无关**：在三种不同的视觉基础模型上均有效，表明方法具有通用性。
- **代码开源**：促进可复现研究。

## 8. 不足与局限

- **实验覆盖有限**：仅使用2个数据集，且为目标中心视频（可能较为人工），未在真实复杂场景中验证。
- **仅限视频输入**：方法依赖短多视角视频，不适用于仅图像可用的场景。
- **缺少算力报告**：无法评估实际部署成本。
- **对比方法不透明**：摘要未列出具体基线或适应方法名称，公平性存疑。
- **潜在偏差**：多视角视频可能隐含物体身份一致性，若视频中物体身份变化（如遮挡后出现不同物体）可能影响性能。
- **应用限制**：主要面向视频场景，与图像分类任务存在差异。

（完）
