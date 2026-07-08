---
title: Generalized cell phenotyping for spatial proteomics with language-informed vision models
title_zh: 语言引导的视觉模型用于空间蛋白质组学的通用细胞表型分析
authors: "Wang, X., Dilip, R., Iqbal, A. R., Bussi, Y., Brown, C., Pradhan, E., Jain, Y., Yu, K., Li, S., Abt, M., Borner, K., Keren, L., Yue, Y., Barnowski, R., Van Valen, D. A."
date: 2026-07-06
pdf: "https://www.biorxiv.org/content/10.1101/2024.11.02.621624v4.full.pdf"
tags: ["query:dino-fg"]
score: 6.0
evidence: 使用通道注意力Transformer进行细胞分类
tldr: 空间蛋白质组学中，不同标记面板和平台的细胞表型标注难以泛化。本文提出DeepCell Types，采用通道注意力transformer构建语言引导视觉模型，利用标记面板语义从异质数据学习。在Expanded TissueNet数据集上，模型在细胞类型预测和标记阳性判断上超越现有方法，微调即可适应新标记面板。该工作开源通用模型，持续可改进。
source: biorxiv
selection_source: fresh_fetch
motivation: 现有细胞表型方法难以跨标记面板和平台泛化，需要一种通用且可微调的模型。
method: 采用通道注意力transformer，利用标记面板语义信息构建语言引导视觉模型。
result: 在Expanded TissueNet上细胞类型预测超越基线，标记阳性预测匹配专家手动门控，微调即可适应新数据。
conclusion: 提供开源通用细胞表型模型DeepCell Types，有效泛化并可高效微调。
---

## 摘要
我们提出了DeepCell Types，一种用于空间蛋白质组学细胞表型分析的新方法，解决了跨不同平台采集的、标记物组合各异的多样化数据集的泛化挑战。我们的方法利用带有通道注意力的变换器构建语言引导的视觉模型；该模型对底层标记物组合的语义理解使其能够从异质数据集中学习并适应。借助一个名为Expanded TissueNet的精选多样化数据集（其细胞类型标签涵盖文献和NIH人类生物分子图谱计划（HuBMAP）联盟），我们的模型在各种细胞类型、组织和成像模态下展现出稳健性能。全面的基准测试表明，我们的方法在细胞类型预测上优于现有方法，并且同一模型在预测标记物阳性方面与专用专家模型竞争；它进一步匹配了人工专家门控，并通过适度微调适应新数据，远超基线从头训练所能达到的水平。这项工作为空间蛋白质组学界提供了一个单一的、可持续改进的表型分析模型，该模型可泛化至新的标记物组合，并在必要时能够高效微调。我们以开源资源的形式发布了DeepCell Types和Expanded TissueNet。

## Abstract
We present DeepCell Types, a novel approach to cell phenotyping for spatial proteomics that addresses the challenge of generalization across diverse datasets with varying marker panels collected across different platforms. Our approach utilizes a transformer with channel-wise attention to create a language-informed vision model; this model's semantic understanding of the underlying marker panel enables it to learn from and adapt to heterogeneous datasets. Leveraging a curated, diverse dataset named Expanded TissueNet with cell type labels spanning the literature and the NIH Human BioMolecular Atlas Program (HuBMAP) consortium, our model demonstrates robust performance across various cell types, tissues, and imaging modalities. Comprehensive benchmarking shows that our method outperforms existing approaches on cell-type prediction and, from the same model, predicts marker positivity competitively with a dedicated specialist; it further matches manual expert gating and adapts to new data with modest fine-tuning, well past what baselines reach when trained from scratch. This work equips the spatial proteomics community with a single, continuously improvable phenotyping model that generalizes to new marker panels and can be fine-tuned efficiently when needed. We release both DeepCell Types and Expanded TissueNet as open-source resources.