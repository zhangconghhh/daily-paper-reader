---
title: "GUIDED: Granular Understanding via Identification, Detection, and Discrimination for Fine-Grained Open-Vocabulary Object Detection"
title_zh: "GUIDED: 通过检测与判别实现细粒度理解"
authors: "Jiaming Li, Zhijia Liang, Weikai Chen, Lin Ma, Guanbin Li"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=REAhulrjzR"
tags: ["query:dino-fg"]
score: 7.0
evidence: 使用分解框架的细粒度开放词汇目标检测
tldr: 针对细粒度开放词汇目标检测中主体与属性语义纠缠的问题，提出GUIDED分解框架，将目标定位与细粒度识别分离，缓解了预训练视觉语言模型中的属性过表示和语义漂移。在多个细粒度检测基准上显著提升了开放词汇检测性能，为细粒度视觉识别提供了新思路。
source: NeurIPS-2025-Accepted
selection_source: conference_retrieval
motivation: 现有开放词汇检测器在细粒度场景下因语义纠缠而性能不佳。
method: 设计分解框架，将主体定位与细粒度属性识别解耦。
result: 在多个细粒度检测数据集上取得了先进的开放词汇检测性能。
conclusion: GUIDED有效缓解了细粒度检测中的语义混淆，为细粒度识别提供通用框架。
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

## 1. 核心问题与整体含义（研究动机和背景）

- **研究问题**：细粒度开放词汇目标检测（FG-OVD）旨在检测由富含属性的文本描述的新颖对象类别。现有开放词汇检测器在基类级别表现良好，但在细粒度场景下性能不佳。
- **根本原因**：预训练视觉语言模型（VLM）的嵌入中，主体（subject）和属性（attribute）存在语义纠缠（semantic entanglement），导致属性过度表示（over-representation）、目标定位错误（mislocalization）以及嵌入空间中的语义漂移（semantic drift）。
- **研究动机**：提出一种专门设计的分解框架，以解决细粒度提示中主体与属性之间的语义纠缠问题，从而提升细粒度检测性能。

## 2. 论文提出的方法论

### 核心思想
将目标定位（localization）与细粒度识别（fine-grained recognition）分离到不同的通路中，使每个子任务与最适合的模块对齐，从而避免语义纠缠带来的干扰。

### 关键技术细节
1. **粗粒度主体提取**：给定一个细粒度类名，首先使用语言模型提取粗粒度主体（coarse-grained subject）及其描述性属性（descriptive attributes）。
2. **主体定位引导**：检测器仅由主体嵌入（subject embedding）引导，确保定位稳定，不受无关或过度表示属性的影响。
3. **属性嵌入融合模块**：采用基于注意力（attention）的方式将属性信息融入检测查询（detection queries）中，选择性地保留有用的属性，同时缓解过度表示问题，保持判别能力。
4. **区域级属性判别模块**：使用带投影头（projection head）的精细化视觉语言模型，对每个检测区域与完整的细粒度类名进行对齐，进一步提升判别精度。

### 算法流程说明（文字描述）
- 输入：细粒度类名（如“red apple”）。
- 步骤1：语言模型解析为“apple”（主体）和“red”（属性）。
- 步骤2：检测器使用“apple”的嵌入进行目标定位，生成候选区域。
- 步骤3：属性嵌入融合模块将这些候选区域与属性信息结合，更新检测查询。
- 步骤4：区域级判别模块对每个区域与完整类名进行精细对齐，输出最终检测结果。

## 3. 实验设计

- **使用的数据集/场景**：FG-OVD 基准（Fine-Grained Open-Vocabulary Detection）和 3F-OVD 基准（可能为更细粒度的变体）。
- **Benchmark**：在细粒度开放词汇检测领域常用的评估基准（具体数据集名称未在摘要中列出，但通常包括如 LVIS、ODinW 等中的细粒度子集）。
- **对比方法**：现有的开放词汇检测器（如基于 CLIP 的检测框架），但未列出具体方法名称。

## 4. 资源与算力

- **文中未明确说明使用了多少算力**（如 GPU 型号、数量、训练时长等）。仅提及“extensive experiments”，未提供硬件细节。因此无法总结具体算力信息。

## 5. 实验数量与充分性

- **实验数量**：摘要提到在 FG-OVD 和 3F-OVD 两个基准上进行了大量实验，但未给出具体实验组数。通常此类工作会包括多个数据集上的主实验、消融实验、可视化分析等。
- **充分性与客观性**：
  - 实验覆盖了至少两个标准基准，说明涵盖了一定的难度等级。
  - 但缺少与具体基线方法的对比细节（如表格数据）以及消融实验的量化说明，无法完全判断实验的充分性。
  - 由于是 NeurIPS 接收论文，一般要求充分的消融和对比，可以认为实验设计基本客观公平。

## 6. 论文的主要结论与发现

- GUIDED 在 FG-OVD 和 3F-OVD 基准上取得了新的最先进结果（new state-of-the-art）。
- 分解建模（disentangled modeling）和模块化优化（modular optimization）对细粒度识别有显著益处。
- 有效地缓解了属性过度表示和语义漂移问题。

## 7. 优点（方法或实验设计亮点）

- **创新性**：首次将主体与属性在检测流程中进行显式分解，设计双通路结构，对齐不同任务到最适合的模块。
- **模块化设计**：提出属性融合和区域判别两个模块，可插拔且易于优化。
- **缓解语义纠缠**：通过只使用主体嵌入定位，避免属性干扰，是一种简洁有效的思路。
- **通用性**：框架可应用于不同细粒度检测场景，具备扩展潜力。

## 8. 不足与局限

- **实验覆盖不足**：未提供在更广泛的开放词汇检测数据集（如 LVIS 全量、COCO 等）上的常规检测性能对比，可能只在特定细粒度子集上有效。
- **偏差风险**：语言模型提取主体和属性可能存在误差（如属性缺失或错误），导致检测性能依赖上游解析质量。
- **应用限制**：方法需要额外的语言模型推理步骤，可能增加推理延迟；对于属性非常复杂或模糊的类别，分解效果可能下降。
- **未报告计算成本**：缺少训练和推理阶段的资源需求，难以评估实际部署可行性。
- **消融实验细节缺失**：摘要未展示每个模块的独立贡献，使得模块必要性验证不够透明。

（完）
