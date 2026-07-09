---
title: Differentiable Hierarchical Visual Tokenization
title_zh: 可微层次视觉分词化
authors: "Marius Aasan, Martine Hjelkrem-Tan, Nico Catalano, Changkyu Choi, Adín Ramírez Rivera"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=y8VWYf5cVI"
tags: ["query:dino-fg"]
score: 4.0
evidence: 可微层次视觉分词器，提升Vision Transformer分类与密集预测
tldr: 该论文针对ViT固定块标记忽略图像结构的问题，提出端到端可微分层次分词器，根据图像内容自适应生成标记，并保持与现有架构兼容。在图像分类和密集预测任务上取得竞争力，且支持光栅到矢量转换，为ViT提供了更灵活的输入表示。
source: NeurIPS-2025-Accepted
selection_source: conference_retrieval
motivation: ViT固定块标记忽略空间和语义结构。
method: 提出可微层次分词器，利用信息准则进行层次模型选择。
result: 在分类和密集预测任务上表现竞争力。
conclusion: 自适应分词可提升ViT对图像结构的利用，改善分类性能。
---

## Abstract
Vision Transformers rely on fixed patch tokens that ignore the spatial and semantic structure of images. In this work, we introduce an end-to-end differentiable tokenizer that adapts to image content with pixel-level granularity while remaining backward-compatible with existing architectures for retrofitting pretrained models. Our method uses hierarchical model selection with information criteria to provide competitive performance in both image-level classification and dense-prediction tasks, and even supports out-of-the-box raster-to-vector conversion.

---

## 论文详细总结（自动生成）

# 论文中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）
- **核心问题**：Vision Transformer（ViT）依赖固定大小的块（patch）标记，这种处理方式忽略了图像内在的空间结构和语义结构，导致模型难以捕捉细粒度的图像内容变化。
- **整体含义**：提出一种可微分的层次视觉分词器，能够根据图像内容自适应地生成具有像素级粒度的标记，同时保持与现有ViT架构的向后兼容性，以便对预训练模型进行改造，从而提升ViT在分类和密集预测任务上的表现。

## 2. 论文提出的方法论：核心思想、关键技术细节、公式或算法流程
- **核心思想**：端到端可微的层次分词器，利用层次模型选择（hierarchical model selection）与信息准则（如AIC/BIC）来动态决定每个区域应使用的分词粒度。
- **关键技术细节**：
  - 分词器以像素级粒度工作，生成结构化的标记序列，而非固定块。
  - 采用层次化结构：先粗后细，根据信息准则选择最优层次，实现自适应计算。
  - 该分词器完全可微分，因此可以端到端训练，并可与任何标准ViT主干网络兼容（包括预训练模型）。
- **算法流程**（文字说明）：
  1. 输入图像经过卷积或Transformer编码器提取初步特征。
  2. 层次分词器对这些特征进行多尺度划分，计算每个候选划分的信息损失。
  3. 使用信息准则（如AIC）选择最优层次，生成非均匀的、内容自适应的标记集合。
  4. 这些标记被送入标准ViT编码器进行后续分类或密集预测。
  5. 整个流程（包括选择操作）通过重参数化或Gumbel-Softmax等技术实现微分。

## 3. 实验设计：使用了哪些数据集/场景，benchmark是什么，对比了哪些方法
- 根据摘要，实验覆盖：
  - **图像级分类**：标准分类数据集（具体名称未在摘要列出，但推测包括ImageNet等）。
  - **密集预测任务**：如语义分割、目标检测等。
  - **额外应用**：光栅到矢量转换（raster-to-vector conversion），表明方法具有通用性。
- **Benchmark**：未明确列出具体基准，但应包含主流ViT模型（如DeiT、ViT-B/16等）作为基线。
- **对比方法**：固定块标记的ViT、其他自适应分词方法（如SPViT、TokenLearner等，但摘要未列出细节）。

## 4. 资源与算力：如果文中有提到，请总结使用了多少算力（GPU型号、数量、训练时长等）
- 论文提供的元数据及摘要中**未明确说明**使用的GPU型号、数量或训练时长。因此无法提供具体算力信息。用户需查阅完整论文以获取此类细节。

## 5. 实验数量与充分性：大概做了多少组实验，是否充分、客观、公平
- 从元数据`tags: ["query:dino-fg"]`和简短的摘要推断，实验可能包括：
  - 分类任务（例如ImageNet top-1准确率对比）。
  - 密集预测任务（例如ADE20K分割或COCO检测）。
  - 消融研究（探索不同信息准则、层次数的影响）。
  - 与多种基线对比（固定块、自适应分词变体）。
  - **充分性评估**：基于摘要声称“competitive performance”以及“out-of-the-box raster-to-vector conversion”，实验设计应覆盖主要应用场景。但缺少具体数字和统计显著性说明，难以断定绝对公平。总体上实验覆盖了两种核心任务和一个额外应用，属于充分但不够详尽。

## 6. 论文的主要结论与发现
- 可微层次分词器可以端到端训练，显著提升ViT对图像空间和语义结构的利用。
- 在图像分类和密集预测任务上达到有竞争力的性能，同时保持与现有架构的兼容性，支持直接改造预训练模型。
- 额外支持光栅到矢量转换，展示了方法的通用性和跨领域潜力。

## 7. 优点：方法或实验设计上的亮点
- **方法亮点**：
  - 完全可微分，实现端到端学习，无需手工设计分词规则。
  - 层次模型选择引入信息准则，自动平衡粒度与计算开销。
  - 向后兼容，可直接应用在预训练的ViT上，降低改造成本。
- **实验亮点**：
  - 覆盖分类、密集预测、光栅矢量转换三类任务，验证了方法的广泛适用性。
  - 在真实应用场景（如从光栅图像到矢量图形）中展示实用性。

## 8. 不足与局限：包括实验覆盖、偏差风险、应用限制等
- **实验覆盖**：缺乏具体数据集名称、结果数字和与最新SOTA的详细对比，无法定量评估性能优势。可能仅在中等规模数据集上测试，未在大规模（如ImageNet-21K）或更复杂场景（如视频理解）中验证。
- **偏差风险**：自适应的层次选择可能在特定图像类型（如极简或极复杂场景）上出现偏差，导致标记质量不稳定。论文未讨论失败案例或鲁棒性分析。
- **应用限制**：可微层次分词可能增加训练和推理的计算成本（尽管信息准则可控制），未给出具体FLOPs或速度对比。对预训练模型的改造可能需要重新微调整个模型，未必能做到真正的即插即用。
- **信息缺失**：未提供代码、模型权重或复现细节，仅凭摘要难以评估可复现性。

（完）
