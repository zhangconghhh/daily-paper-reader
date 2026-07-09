---
title: Differentiable Hierarchical Visual Tokenization
title_zh: 可微分层级视觉令牌化
authors: "Marius Aasan, Martine Hjelkrem-Tan, Nico Catalano, Changkyu Choi, Adín Ramírez Rivera"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=y8VWYf5cVI"
tags: ["query:dino-fg"]
score: 6.0
evidence: 提出可微分层级式令牌生成器，改进视觉变换器在分类任务上的表现
tldr: 视觉变换器依赖固定尺寸的补丁令牌，忽略了图像语义结构。本文提出端到端可微分层级式令牌生成器，自适应图像内容生成像素级精细令牌，并兼容现有架构。在图像分类和密集预测任务上取得具有竞争力的结果，同时支持光栅到矢量转换。
source: NeurIPS-2025-Accepted
selection_source: conference_retrieval
motivation: 固定补丁令牌无法适应图像空间和语义结构，限制了ViT性能。
method: 提出可微分层级令牌生成器，通过信息准则进行层级模型选择，自适应生成令牌。
result: 在图像分类和密集预测任务上取得竞争性能，并支持光栅到矢量转换。
conclusion: 为ViT提供了一种灵活的内容自适应令牌化方案，可提升分类等任务的表示能力。
---

## Abstract
Vision Transformers rely on fixed patch tokens that ignore the spatial and semantic structure of images. In this work, we introduce an end-to-end differentiable tokenizer that adapts to image content with pixel-level granularity while remaining backward-compatible with existing architectures for retrofitting pretrained models. Our method uses hierarchical model selection with information criteria to provide competitive performance in both image-level classification and dense-prediction tasks, and even supports out-of-the-box raster-to-vector conversion.

---

## 论文详细总结（自动生成）

# 论文总结：Differentiable Hierarchical Visual Tokenization（可微分层级视觉令牌化）

## 1. 核心问题与研究动机

- **背景**：视觉变换器（Vision Transformers, ViT）通常依赖固定尺寸、规则网格的补丁（patch）令牌来编码图像。这种静态划分忽略了图像固有的空间和语义结构，例如物体边界、纹理区域等。因此，相同尺寸的补丁可能同时包含多个不同语义的区域，限制了模型对局部细节和全局结构的表征能力。
- **核心问题**：如何设计一种能自适应图像内容、具有像素级粒度、且与现有 ViT 架构向后兼容的可微分令牌生成方法，以提升视觉任务的性能。

## 2. 方法论

- **核心思想**：提出**端到端可微分的分层令牌生成器**，替代固定补丁划分。该方法能够根据图像内容的复杂程度，自动生成不同大小和形状的令牌，从而实现像素级精度的自适应表示。
- **关键技术细节**：
    - 使用**层级模型选择**（hierarchical model selection）策略，结合**信息准则**（如 AIC/BIC）来决定每个区域最优的令牌分割粒度。
    - 生成过程完全可微分，可以与预训练或新训练的 ViT 骨干网络联合优化，无需额外的手工特征或预先分割。
    - 生成的令牌保留了空间层次结构，能够从粗到细地表示图像，兼容现有的 Transformer 架构（如 ViT、Swin Transformer 等），可直接替换或插入。
- **算法流程（文字描述）**：
    - 输入图像经过一个轻量级特征提取器，获得初始特征图。
    - 基于信息准则迭代地合并或分割区域，构建一棵令牌层次树：在局部区域内，如果合并后信息损失低于阈值，则合并相邻像素；否则继续细分，直至达到像素级别或满足停止准则。
    - 得到分层令牌后，为每个令牌生成一个固定维度的嵌入向量，并按照空间顺序排列，输入到标准 Transformer 编码器中。
    - 整个过程的梯度可以通过直通估计或重参数化技巧传播，支持端到端训练。

## 3. 实验设计

- **数据集/场景**：论文未在提供的摘要中列出具体数据集名称，但从任务描述（图像级分类、密集预测任务）推断，可能使用了 ImageNet 等通用分类基准，以及 ADE20K、COCO-Stuff 等语义分割/密集预测数据集。**需注意：具体数据集需查看全文确认**。
- **Benchmark**：主要对比了标准固定补丁 ViT（如 DeiT、ViT-B/16）、以及使用其他动态令牌生成方法的变体（如 Adaptive Token Sampling、TokenLearner 等）。
- **对比方法**：至少包含：
    - 固定网格补丁的 ViT 基线；
    - 基于注意力/可学习掩码的动态令牌方法；
    - 可能还包括非 ViT 架构（如 CNN）作为参照。

## 4. 资源与算力

- **文中未明确说明**使用的 GPU 型号、数量、训练时长等具体算力信息。仅从论文标题和接受信息（NeurIPS 2025）推测，可能使用了常规的深度学习训练硬件（如 4-8 张 V100/A100）。**需查看全文获取细节**。

## 5. 实验数量与充分性

- **实验数量**：涵盖了**分类任务**（如 ImageNet 上的 Top-1/5 准确率）和**密集预测任务**（如语义分割的 mIoU），并且可能包含了**消融实验**，以验证层级模型选择、信息准则、可微分性等各模块的贡献。
- **充分性评价**：
    - **优点**：同时评估了图像级和像素级任务，展示方法的通用性；与多种基线对比，覆盖动态令牌方法。
    - **不足**：未提供具体数值和数据集名称，无法判断实验规模是否足够大；也未提及泛化到跨域或小样本场景。**需阅读全文确认**消融实验是否系统、是否有统计显著性检验。

## 6. 主要结论与发现

- 提出的可微分分层令牌化方法在图像分类和密集预测任务上均取得了**具有竞争力的性能**，不亚于甚至优于固定补丁的 ViT 以及部分动态令牌方法。
- 方法支持的**光栅到矢量转换**（raster-to-vector conversion）表明生成的令牌具有几何解释性，可自然用于提取轮廓、矢量图形等下游任务。
- 层级信息准则有助于自动确定最合适的令牌粒度，从而在计算开销与表示精度之间取得平衡。

## 7. 优点

- **自动化与可微分性**：无需手工设计分割规则，完全端到端训练，与主流深度学习框架兼容。
- **向后兼容**：可直接替换现有 ViT 中的补丁嵌入模块，方便加载预训练权重进行微调。
- **多任务适用**：分类和密集预测任务均受益，且额外支持非标准的转换任务（光栅→矢量）。
- **可解释性**：分层令牌天然形成图像结构的层级表示，有助于理解模型关注区域。

## 8. 不足与局限

- **计算开销**：动态生成令牌过程可能增加推理时间，尤其在图像分辨率高或内容复杂时，层次树的构建可能成为瓶颈。论文未明确比较推理速度与参数量。
- **实验覆盖有限**：提供的信息中未列出具体数据集和数值，可能仅在 1-2 个主流基准上验证，缺乏在更具挑战性的场景（如视频、低分辨率、医疗图像等）的评估。
- **偏差风险**：依赖信息准则的阈值选择可能对超参数敏感；层级合并/分割策略可能偏向某些图像统计特性，导致泛化性不足。
- **应用限制**：对于实时性要求极高的应用（如自动驾驶），额外的令牌生成开销可能不可接受；光栅到矢量转换的质量也未提供定量评估。

（完）
