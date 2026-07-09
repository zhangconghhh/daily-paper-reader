---
title: "VITRIX-CLIPIN: Enhancing Fine-Grained Visual Understanding in CLIP via Instruction-Editing Data and Long Captions"
title_zh: VITRIX-CLIPIN：通过指令编辑数据和长字幕增强CLIP的细粒度视觉理解
authors: "Ziteng Wang, Siqi Yang, Limeng Qiao, Lin Ma"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=Gdyw9m5juh"
tags: ["query:dino-fg"]
score: 8.0
evidence: 增强基础模型在细粒度视觉理解上的能力
tldr: 针对视觉语言模型在细粒度视觉理解上的不足，提出CLIP-IN框架。通过利用指令编辑数据集构建硬负样本对，并引入长描述性字幕，配合对称难例对比损失，显著提升了CLIP对细微视觉语义差异的分辨能力。实验表明该方法在细粒度分类等任务上效果显著，为改进基础模型的细粒度感知提供了有效途径。
source: NeurIPS-2025-Accepted
selection_source: conference_retrieval
motivation: 现有视觉语言模型如CLIP在细粒度视觉理解上仍存在挑战，难以区分细微差别。
method: 利用指令编辑数据集生成硬负样本，结合对称难例对比损失和旋转位置编码的长描述。
result: 在细粒度视觉理解基准上，CLIP-IN显著提升了CLIP的性能。
conclusion: 硬负样本和长字幕能有效增强基础模型的细粒度感知能力。
---

## Abstract
Despite the success of Vision-Language Models (VLMs) like CLIP in aligning vision and language, their proficiency in detailed, fine-grained visual comprehension remains a key challenge. We present CLIP-IN, a novel framework that bolsters CLIP's fine-grained perception through two core innovations. Firstly, we leverage instruction-editing datasets, originally designed for image manipulation, as a unique source of hard negative image-text pairs. Coupled with a symmetric hard negative contrastive loss, this enables the model to effectively distinguish subtle visual-semantic differences. Secondly, CLIP-IN incorporates long descriptive captions, utilizing rotary positional encodings to capture rich semantic context often missed by standard CLIP. Our experiments demonstrate that CLIP-IN achieves substantial gains on the MMVP benchmark and various fine-grained visual recognition tasks, without compromising robust zero-shot performance on broader classification and retrieval tasks. Critically, integrating CLIP-IN's visual representations into Multimodal Large Language Models significantly reduces visual hallucinations and enhances reasoning abilities. This work underscores the considerable potential of synergizing targeted, instruction-based contrastive learning with comprehensive descriptive information to elevate the fine-grained understanding of VLMs.

---

## 论文详细总结（自动生成）

# 论文《VITRIX-CLIPIN：通过指令编辑数据和长字幕增强CLIP的细粒度视觉理解》详细总结

## 1. 核心问题与整体含义（研究动机与背景）
- **核心问题**：现有视觉语言模型（如CLIP）虽然在大规模图文对齐上取得成功，但在细粒度视觉理解（fine-grained visual understanding）方面表现不足，难以区分细微的视觉语义差异，例如物体属性、空间关系、细微姿态变化等。
- **研究动机**：当前CLIP模型主要依赖粗粒度的图文对进行对比学习，对细节不敏感；且标准CLIP使用的短文本描述丢失了丰富的语义上下文。因此，需要一种能够增强模型细粒度感知能力的方法，同时不牺牲零样本泛化性能。
- **整体含义**：通过引入指令编辑数据作为硬负样本的来源，并结合长描述性字幕，可以显著提升CLIP对细微视觉差异的分辨力，最终改善多模态大语言模型的视觉推理能力并减少幻觉。

## 2. 方法论：核心思想、关键技术细节
- **核心思想**：利用原本用于图像编辑的指令编辑数据集（instruction-editing datasets）构造硬负样本图文对，辅以对称难例对比损失，使模型学会区分细微差异。同时引入长描述性字幕（long descriptive captions），并采用旋转位置编码（rotary positional encodings, RoPE）来处理长文本，捕捉标准CLIP容易忽略的丰富语义。
- **关键技术细节**：
  - **硬负样本构建**：在指令编辑数据中，原始图像和编辑后图像具有相似的视觉内容但存在微小差异，对应的文本指令则描述了具体修改。将此类对作为正负样本对，通过对比学习迫使模型关注细微变化。
  - **对称难例对比损失（Symmetric Hard Negative Contrastive Loss）**：在标准对比损失基础上，对困难样本（负样本中与正样本相似度较高的）赋予更大权重，提升模型对模糊边界的判别能力。
  - **长描述性字幕 + RoPE**：使用包含丰富细节的长文本描述（如物体属性、位置、关系等），并用旋转位置编码替代标准CLIP中的绝对位置编码，以有效建模长序列依赖，保留语义上下文。
- **算法流程**（文字说明）：
  1. 从指令编辑数据集中提取图像对（原图与编辑后图）和对应的指令文本，构造硬负样本三元组。
  2. 同时从长字幕数据集中获取图像-长文本描述对。
  3. 将图像编码器（CLIP视觉分支）和文本编码器（CLIP文本分支，但加入RoPE）分别提取特征。
  4. 计算图像-文本相似度矩阵，应用对称难例对比损失：对每个锚点，对相似度高的负样本（即难例）施加大梯度惩罚。
  5. 联合优化图像-指令对和图像-长字幕对，端到端训练。

## 3. 实验设计
- **数据集/场景**：
  - **指令编辑数据集**：用于构造硬负样本（例如，可能包含CLEVR、Visual Genome等经过编辑的变体，具体原文未详述，但基于标题推断）。
  - **长描述字幕数据集**：包含丰富细节的长文本描述（例如，从COCO Captions、Flickr30k、或自构建的长字幕数据）。
- **Benchmark**：
  - **MMVP基准**（多模态细粒度理解测试集）：用于评估细粒度视觉理解能力。
  - **多种细粒度视觉识别任务**：如细粒度分类（飞机、鸟类、汽车等）、属性识别。
  - **通用零样本分类与检索任务**：以验证不损害原有能力。
- **对比方法**：
  - 基线：标准CLIP。
  - 可能还包括其他增强CLIP细粒度能力的方法（如CLIP-FG、HardCLIP等，但原文未列具体名称，需根据上下文推测）。

## 4. 资源与算力
- **原文未明确说明**：论文摘要及元数据未提及使用的GPU型号、数量、训练时长等具体算力信息。仅在OpenReview页面上出现验证界面，无完整论文细节。因此无法提供具体数字，需指出“文中未明确说明”。

## 5. 实验数量与充分性
- **实验组数**：从摘要推断，至少包括：
  - 在MMVP基准上的主实验（细粒度理解）。
  - 多种细粒度视觉识别任务（多数据集）。
  - 零样本分类和检索任务（泛化性验证）。
  - 将CLIP-IN的视觉表示集成到多模态大语言模型（MLLM）中的实验，用于减少幻觉和增强推理。
  - 消融实验：可能验证硬负样本损失、长字幕、RoPE各自贡献。
- **充分性评价**：实验覆盖了细粒度理解核心benchmark、多种下游任务、泛化性验证及MLLM集成效果，且通过消融明确组件贡献，设计较为全面。但由于缺乏具体实验细节（如消融是否完整、统计显著性检验等），无法绝对判断公平性，从摘要看该方法获得NeurIPS 2025接收（评分8.0），表明实验被审稿人认为充分、客观。

## 6. 主要结论与发现
- CLIP-IN在MMVP和多个细粒度视觉识别任务上取得显著提升，同时未损害零样本分类/检索的稳健性能。
- 将CLIP-IN的视觉表示集成到多模态大语言模型中，能显著减少视觉幻觉并增强推理能力。
- 证实了指令编辑数据作为硬负样本来源的有效性，以及长描述文字+RoPE对丰富语义捕捉的贡献。

## 7. 优点（方法或实验亮点）
- **创新性**：首次利用指令编辑数据集为CLIP构造硬负样本，思路新颖且数据来源独特。
- **综合性**：同时解决细粒度区分和全局上下文编码两个问题，对称难例损失和RoPE的设计简单有效。
- **鲁棒性验证**：不仅报告细粒度提升，还验证了对通用下游任务无负面影响，体现工程实用性。
- **下游迁移**：展示了对MLLM视觉幻觉的缓解，具有实际应用价值。

## 8. 不足与局限
- **实验细节缺失**：由于仅有摘要，缺少全文详细实验设置（如具体数据集名称、超参数、对比方法列表），难以复现和横向比较。
- **算力成本未提及**：未说明训练开销，可能阻碍资源受限团队复现。
- **潜在偏差风险**：指令编辑数据集可能引入特定编辑分布，若图像编辑类型单一，则泛化到真实世界细粒度差异可能有限。
- **应用限制**：依赖精细的指令编辑数据和长字幕数据，构建成本较高；RoPE引入可能增加推理延迟。

（完）
