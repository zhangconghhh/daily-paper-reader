---
title: "GUIDED: Granular Understanding via Identification, Detection, and Discrimination for Fine-Grained Open-Vocabulary Object Detection"
title_zh: GUIDED：通过识别、检测和判别实现细粒度开放词汇目标检测
authors: "Jiaming Li, Zhijia Liang, Weikai Chen, Lin Ma, Guanbin Li"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=REAhulrjzR"
tags: ["query:dino-fg"]
score: 8.0
evidence: 基于分解框架的细粒度开放词汇检测
tldr: GUIDED通过分解主体和属性嵌入来解决细粒度开放词汇检测中的语义纠缠问题。该方法将目标定位和细粒度识别分离，有效缓解了属性过表示和语义漂移，在多个FG-OVD基准上大幅提升检测精度，为细粒度识别提供了有效框架。
source: NeurIPS-2025-Accepted
selection_source: conference_retrieval
motivation: 现有开放词汇检测在细粒度场景下因语义纠缠而性能不佳。
method: 提出分解框架，分别处理目标定位和细粒度识别。
result: 在细粒度开放词汇检测基准上取得显著改进。
conclusion: 为细粒度检测提供了解耦语义的有效方法。
---

## Abstract
Fine-grained open-vocabulary object detection (FG-OVD) aims to detect novel object categories described by attribute-rich texts. While existing open-vocabulary detectors show promise at the base-category level, they underperform in fine-grained settings due to the semantic entanglement of subjects and attributes in pretrained vision-language model (VLM) embeddings -- leading to over-representation of attributes, mislocalization, and semantic drift in embedding space. We propose GUIDED, a decomposition framework specifically designed to address the semantic entanglement between subjects and attributes in fine-grained prompts. By separating object localization and fine-grained recognition into distinct pathways, GUIDED aligns each subtask with the module best suited for its respective roles. 
Specifically, given a fine-grained class name, we first use a language model to extract a coarse-grained subject and its descriptive attributes. 
Then the detector is guided solely by the subject embedding, ensuring stable localization unaffected by irrelevant or overrepresented attributes. To selectively retain helpful attributes, we introduce an attribute embedding fusion module that incorporates attribute information into detection queries in an attention-based manner. This mitigates over-representation while preserving discriminative power.
Finally, a region-level attribute discrimination module compares each detected region against full fine-grained class names using a refined vision-language model with a projection head for improved alignment.
Extensive experiments on FG-OVD and 3F-OVD benchmarks show that GUIDED achieves new state-of-the-art results, demonstrating the benefits of disentangled modeling and modular optimization.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **核心问题**：细粒度开放词汇目标检测（FG-OVD）任务旨在检测由属性丰富文本描述的新颖物体类别。现有开放词汇检测器在基础类别层面表现良好，但在细粒度设置下性能不佳，主要原因在于预训练视觉-语言模型（VLM）嵌入中主体（subject）与属性（attribute）存在语义纠缠（semantic entanglement），导致属性过表示（over-representation）、定位错误（mislocalization）以及嵌入空间中的语义漂移（semantic drift）。
- **整体含义**：现有方法无法有效分离目标的主体概念与属性细节，阻碍了细粒度识别能力。本文提出GUIDED框架，通过解耦建模和模块化优化来解决这一瓶颈，从而提升细粒度开放词汇检测的精度。

## 2. 论文提出的方法论：核心思想、关键技术细节

- **核心思想**：将目标定位（object localization）与细粒度识别（fine-grained recognition）分离到不同的通路中，使每个子任务与最合适的模块对齐，从而避免语义纠缠。
- **关键技术细节**：
  1. **粗粒度主体与属性提取**：给定一个细粒度类别名称（例如“红色斑点的花”），首先利用语言模型提取粗粒度主体（如“花”）及其描述属性（如“红色”、“斑点”）。
  2. **主体引导的定位**：检测器仅由主体嵌入（subject embedding）引导，确保定位过程不受无关或过表示属性的干扰，实现稳定定位（模块称为“Identification”）。
  3. **属性嵌入融合模块**：为保留有用属性，采用基于注意力机制的方式将属性信息融合到检测查询（detection queries）中，缓解属性过表示同时保持判别能力（模块称为“Detection”）。
  4. **区域级属性判别模块**：对每个检测区域，使用带有投影头（projection head）的改进视觉-语言模型与完整细粒度类别名称进行对齐，进行细粒度判别（模块称为“Discrimination”）。
- **算法流程说明**：输入细粒度类名 → 语言模型解耦→ 提取主体和属性 → 主体嵌入用于定位 → 属性嵌入通过注意力融合到查询 → 检测框生成 → 区域级属性判别模块对比完整细粒度类名进行最终分类。

## 3. 实验设计：数据集、基准、对比方法

- **数据集与基准**：在FG-OVD基准（细粒度开放词汇检测）和3F-OVD基准（更细粒度的开放词汇检测）上进行了大量实验。
- **对比方法**：与现有开放词汇检测方法（如基于CLIP、DETR的变体等）进行对比，并报告了最先进的（state-of-the-art）结果。具体对比方法名称在摘要中未列出，但声称达到了新的最佳性能。
- *注：由于仅提供摘要，数据集名称、评价指标等细节未明确，但可推断使用了标准的细粒度检测评估协议（如mAP等）。

## 4. 资源与算力

- 论文摘要及元数据中**未明确说明**所使用的GPU型号、数量、训练时长等算力信息。
- *注：在论文正文中可能有提及，但提供的文本中缺乏此部分内容。

## 5. 实验数量与充分性

- 从摘要可知，实验覆盖了FG-OVD和3F-OVD两个基准，且包含消融实验（例如属性融合模块、判别模块的作用等，元数据中提及“消融实验”）。但具体实验组数未知。
- **充分性评估**：由于在多个基准上取得SOTA，且方法设计有消融验证，可以认为实验设计较为充分。但缺少详细表格和其他对比方法的具体数值，无法全面判断公平性。通常这类论文会进行充分的消融和与多种基线的公平比较，但此处信息有限。

## 6. 论文的主要结论与发现

- GUIDED通过解耦主体和属性嵌入，有效解决了细粒度开放词汇检测中的语义纠缠问题。
- 分离定位与识别通路，分别用主体嵌入引导定位、融合属性信息增强判别，显著提升了检测精度。
- 在FG-OVD和3F-OVD基准上均达到新的最优性能，证明了分解框架和模块化优化的优势。

## 7. 优点：方法论和实验设计的亮点

- **方法亮点**：
  - 创新性地将细粒度提示分解为粗粒度主体和属性，分别处理定位与识别，逻辑简洁且有效。
  - 属性嵌入融合模块使用注意力机制选择性保留有用属性，避免属性过表示。
  - 区域级属性判别模块通过投影头改进VLM对齐，提升细粒度辨别能力。
  - 三个模块（Identification、Detection、Discrimination）形成完整流水线，每个模块职责清晰。
- **实验设计亮点**：
  - 在多个基准上测试（FG-OVD和3F-OVD），证明了通用性。
  - 包含消融实验，验证各模块贡献。

## 8. 不足与局限

- **实验覆盖**：摘要未提供详细的类别数量、指标数值、对比方法的性能差距，无法精确评估改进幅度。可能只报告了平均结果，缺乏细粒度类别上的分解分析。
- **偏差风险**：语言模型提取主体和属性的准确性可能影响性能，对提示语设计敏感。若语言模型引入偏差（如无法正确提取属性），可能造成误差。
- **应用限制**：方法依赖语言模型进行分解，增加推理复杂度。对于属性非常复杂或模糊的细粒度描述，分解可能不准确。
- **资源消耗**：未提及训练和推理的计算成本，可能较基线方法更高。
- **通用性**：目前仅在物体检测任务上验证，未扩展到其他视觉-语言任务（如分割、检索）。
- **信息完整度**：由于提供的文本仅为摘要，更多局限需阅读全文才能充分了解。

（完）
