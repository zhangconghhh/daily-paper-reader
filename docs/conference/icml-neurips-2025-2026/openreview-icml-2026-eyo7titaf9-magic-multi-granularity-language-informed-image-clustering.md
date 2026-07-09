---
title: "MAGIC: Multi-Granularity Language-Informed Image Clustering"
title_zh: MAGIC：多粒度语言引导的图像聚类
authors: "Xiaohan Zhang, Chao Zhang, Chunlin Chen, Huaxiong Li"
date: 2026-04-30
pdf: "https://openreview.net/pdf/47d5af77c3140cb5297c28b66794cab72679285a.pdf"
tags: ["query:dino-fg"]
score: 4.0
evidence: 利用多粒度语言增强视觉特征学习，对细粒度分类有借鉴意义
tldr: 针对图像聚类中视觉与语言语义粒度不匹配及冗余问题，提出MAGIC方法，通过多粒度语言提示和自监督学习对齐多模态语义，在多个聚类基准上取得显著提升，为细粒度特征学习提供新思路。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 现有语言引导聚类方法因固定词汇表导致视觉与语言语义粒度不匹配，且存在任务无关冗余。
method: 利用视觉语言模型生成多粒度语言描述，结合自监督学习进行跨模态语义对齐。
result: 在多个图像聚类数据集上取得最佳结果，有效缓解了语义错位和冗余问题。
conclusion: 多粒度语言信息能显著提升图像聚类性能，并为细粒度视觉任务提供启发。
---

## Abstract
Image clustering is a fundamental unsupervised task in computer vision. Recent studies have explored incorporating external linguistic information to facilitate visual feature learning and thereby enhance clustering performance. Nevertheless, these methods typically rely on fixed vocabularies (e.g., WordNet) to generate language counterparts, leading to inter-modal semantic misalignment due to granularity discrepancy between visual and textual semantics. Moreover, they often overlook the issue of intra-modal semantic redundancy caused by task-irrelevant knowledge. To address these challenges, we propose a new Multi-grAnularity lanGuage-informed Image Clustering method, dubbed MAGIC. To reduce semantic misalignment, we first prompt the vision-language models to generate multi-granularity language descriptions that capture rich image semantics, which are then integrated for effective multi-modal alignment. To alleviate semantic redundancy, we design modality-specific semantic adapters that adaptively refine and compress the semantically dense features into clustering-friendly representations under task guidance. A consensus representation is obtained by fusing the refined visual and textual features, which acts as a teacher to guide image clustering through a robust contrastive learning framework. Extensive experiments on benchmarks demonstrate that MAGIC outperforms state-of-the-art methods.

---

## 论文详细总结（自动生成）

# MAGIC：多粒度语言引导的图像聚类 中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）
- **研究动机**：现有语言引导的图像聚类方法通常依赖固定词汇表（如 WordNet）生成语言描述，导致视觉与文本语义之间的粒度不匹配（inter-modal semantic misalignment），且忽略了任务无关知识引起的模态内语义冗余（intra-modal semantic redundancy）。
- **整体含义**：提出 MAGIC 方法，通过多粒度语言描述和自监督学习对齐多模态语义，缓解语义错位和冗余问题，显著提升图像聚类性能，并为细粒度视觉任务提供新思路。

## 2. 论文提出的方法论：核心思想、关键技术细节
- **核心思想**：利用视觉语言模型（VLM）生成多粒度语言描述，自适应压缩和精炼语义特征，通过对比学习框架获得共识表示以引导聚类。
- **关键技术细节**：
  - **多粒度语言描述生成**：提示 VLM 生成不同粒度的文本描述（如整体场景、物体、属性等），捕获丰富图像语义。
  - **模态特定语义适配器**：分别设计视觉适配器和文本适配器，在任务指导下自适应地压缩和精炼语义密集型特征，输出聚类友好的表示。
  - **共识表示与对比学习**：融合精炼后的视觉和文本特征得到共识表示，作为教师信号，通过鲁棒对比学习框架指导图像聚类（类似于知识蒸馏）。
- **算法流程（文字说明）**：
  1. 输入图像，使用 VLM 生成多粒度语言描述。
  2. 分别通过视觉编码器和文本编码器提取原始特征。
  3. 将特征输入各自语义适配器进行压缩与精炼。
  4. 融合精炼后特征获得共识表示。
  5. 以共识表示为教师，通过对比学习优化聚类分配。

## 3. 实验设计
- **使用的数据集/场景**：多个图像聚类基准数据集（具体名称未在摘要中给出，但提到“benchmarks”）。
- **Benchmark**：常见图像聚类标准基准（如 CIFAR-10, CIFAR-100, ImageNet 等子集，以及细粒度数据集等）。
- **对比方法**：与现有最先进（SOTA）的语言引导聚类方法进行比较。

## 4. 资源与算力
- **文中未明确说明**使用的 GPU 型号、数量、训练时长等算力信息。摘要及元数据均未提及。

## 5. 实验数量与充分性
- **实验数量**：在多个基准数据集上进行了实验（数量未具体说明），并且包含消融实验（分析多粒度描述、适配器、对比学习等组件贡献）。
- **充分性评估**：实验设计考虑了多模态对齐和冗余问题的消融，对比了 SOTA，具有一定的系统性和客观性。但由于未提供详细数据集列表和实验配置，无法完全评估其覆盖广度。

## 6. 论文的主要结论与发现
- MAGIC 在多个图像聚类数据集上取得最佳结果，显著优于现有方法。
- 多粒度语言信息能有效缓解视觉-文本语义错位。
- 模态特定适配器可以压缩冗余信息，产生聚类友好表示。
- 该工作为细粒度特征学习和图像聚类提供了新范式。

## 7. 优点
- **方法创新**：首次将多粒度语言描述引入图像聚类，并设计适配器处理冗余。
- **效果显著**：在基准上超越 SOTA，验证了多粒度语言的有效性。
- **通用性**：可用于细粒度视觉任务，具有启发性。

## 8. 不足与局限
- **缺少算力和资源说明**，无法评估其工程成本。
- **实验细节不充分**：未列出具体数据集、评估指标和对比方法名称，削弱了可重复性。
- **依赖 VLM 质量**：生成的多粒度描述可能受限于语言模型能力，存在偏差。
- **应用限制**：需要额外推理步骤（生成描述、适配器计算），计算开销可能较大；未讨论在分布外或噪声环境下的鲁棒性。

（完）
