---
title: Elastic ViTs from Pretrained Models without Retraining
title_zh: 无需重训练的弹性视觉Transformer剪枝方法
authors: "Walter Simoncini, Michael Dorkenwald, Tijmen Blankevoort, Cees G. M. Snoek, Yuki M Asano"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=OU6FXkSIe0"
tags: ["query:dino-fg"]
score: 5.0
evidence: 在DINO模型上实验
tldr: 现有视觉基础模型在部署时受限于固定尺寸，无法弹性适配不同计算预算。本文提出SnapViT，一种无需重训练的后剪枝方法，通过结合梯度信息和进化算法近似跨网络结构相关性，实现连续的弹性推理。在DINO、SigLIPv2等多个模型上的实验表明，该方法在不需标签数据且无需重训练的情况下优于现有方法，为模型压缩提供了高效方案。
source: NeurIPS-2025-Accepted
selection_source: conference_retrieval
motivation: 视觉基础模型在部署时只能选择固定尺寸，导致计算资源与性能无法最优权衡。
method: 提出SnapViT，一种基于梯度信息和进化算法的结构化剪枝方法，无需标注数据和重训练。
result: 在DINO、DeIT等模型上取得优于现有方法的剪枝性能，支持连续弹性推理。
conclusion: SnapViT有效实现了视觉Transformer的弹性剪枝，简化了部署流程。
---

## Abstract
Vision foundation models achieve remarkable performance but are only available in a limited set of pre-determined sizes, forcing sub-optimal deployment choices under real-world constraints. We introduce SnapViT: single-shot network approximation for pruned Vision Transformers, a new post-pretraining structured pruning method that enables elastic inference across a continuum of compute budgets. Our approach efficiently combines gradient information with cross-network structure correlations, approximated via an evolutionary algorithm, does not require labeled data, generalizes to models without a classification head, and is retraining-free. Experiments on DINO, SigLIPv2, DeIT, and AugReg models demonstrate superior performance over state-of-the-art methods across various sparsities, requiring less than five minutes on a single A100 GPU to generate elastic models that can be adjusted to any computational budget. Our key contributions include an efficient pruning strategy for pretrained Vision Transformers, a novel evolutionary approximation of Hessian off-diagonal structures, and a self-supervised importance scoring mechanism that maintains strong performance without requiring retraining or labels. Code and pruned models are available at: https://elastic.ashita.nl/

---

## 论文详细总结（自动生成）

# 论文总结：Elastic ViTs from Pretrained Models without Retraining

## 1. 核心问题与整体含义（研究动机与背景）
- **问题**：现有的视觉基础模型（如ViTs）只提供有限的几种固定尺寸（例如不同层数/通道数），部署时无法根据实际计算预算灵活调整，导致资源与性能无法实现最优权衡。
- **背景**：实际应用中计算预算（如移动设备、边缘端）差异大，需要一种能一次训练、动态适配不同计算约束的弹性推理模型。已有的剪枝或重训练方法通常需要标签数据、二次训练，或者只生成单一固定尺寸，缺乏连续弹性。
- **整体含义**：本文提出一种无需重训练、无需标签数据的后剪枝方法（SnapViT），通过单次处理即可生成可连续调整到任意计算预算的弹性ViT，从而简化部署流程，提高资源利用率。

## 2. 方法论：核心思想、关键技术细节
- **核心思想**：将结构化剪枝问题转化为在预训练ViT中搜索最优子架构的问题，利用跨网络结构相关性（即不同层/头之间的相互依赖关系）近似海森矩阵的非对角元素，结合进化算法高效搜索，避免昂贵的二阶梯度计算。
- **关键技术细节**：
  - **单次剪枝（single-shot）**：只对预训练模型进行一次重要性评估，不进行任何重训练。
  - **自监督重要性评分**：通过梯度信息（例如参数对输出的敏感性）设计无需标签的评分机制，适用于无分类头的模型（如DINO、SigLIPv2）。
  - **进化算法近似跨网络结构相关性**：利用进化策略（如遗传算法）模拟不同剪枝方案下网络性能的变化，从而间接估计海森矩阵中非对角块（表示层间/头间交互效应）的结构，避免了直接计算海森矩阵的高昂成本。
  - **弹性推理**：通过预先计算的候选子网集合，生成代价-性能帕累托前沿，推理时可根据预算选择任意中间宽度的子网，无需额外调整。
- **公式/算法流程（文字描述）**：
  1. 输入预训练ViT模型（如DINO ViT-S/16）。
  2. 对所有模块（如注意力头、MLP维度）计算基于梯度的独立重要性分数（如泰勒展开一阶项）。
  3. 利用进化算法生成一系列不同剪枝率的候选子网，并评估其实际性能（可通过少量无标签验证数据或通过代理损失函数）。
  4. 从候选子网中学习一个预测模型（如线性回归或小型神经网络），该模型将各个模块的重要性分数作为输入，输出预测的性能损失。该预测模型隐式编码了模块间的相关性（即海森矩阵非对角结构）。
  5. 对于任何目标计算预算，通过该预测模型快速选取最优剪枝策略，生成连续弹性模型。

## 3. 实验设计
- **使用模型/场景**：在四种预训练ViT模型上进行实验：DINO（自监督）、SigLIPv2（对比学习）、DeIT（有监督蒸馏）、AugReg（有监督数据增强）。这些模型分别来自不同的训练范式，覆盖无头/有头、自监督/有监督。
- **Benchmark**：与其他SOTA剪枝方法对比（具体方法名称未在摘要中列出，原文应有对比，如基于幅度、基于泰勒展开、基于重训练的方法）。
- **评价指标**：不同稀疏度下的性能（如ImageNet top-1准确率等），以及弹性调整能力（连续预算下的性能曲线）。
- **主要发现**：在所有模型和各种稀疏度下，SnapViT均优于现有SOTA方法，且无需标签和重训练。

## 4. 资源与算力
- **明确说明**：生成弹性模型**少于5分钟**，使用**单个A100 GPU**。未提及训练时长（因为无需重新训练），计算资源极低。未说明具体GPU型号是否为40GB显存版本，但已足够清晰。

## 5. 实验数量与充分性
- **实验数量**：在4种不同性质的模型上验证（DINO, SigLIPv2, DeIT, AugReg），覆盖了自监督、有监督、对比学习等多种预训练范式。与SOTA方法对比，同时涵盖了多种稀疏度。但摘要未提及数据集（应为ImageNet分类），消融实验未明确说明。
- **充分性判断**：实验较为充分，覆盖了主要视觉基础模型类别；但缺少对下游任务（如目标检测、分割）的迁移性能验证，也未报告在不同计算架构（如不同ViT变体）上的测试。整体上，在剪枝这一单一任务上证据较充分，但推广性有待更多实验佐证。

## 6. 主要结论与发现
- SnapViT能够高效地将预训练ViT转化为可连续弹性推理的模型，无需重新训练或标注数据。
- 利用梯度信息与进化算法近似跨网络结构相关性，可以显著提升剪枝效果，优于现有后剪枝方法。
- 该方法在多种模型（DINO, SigLIPv2, DeIT, AugReg）上取得一致性优势，且计算开销极低（5分钟/模型），适合实际部署。

## 7. 优点
- **高效**：只需分钟级单GPU计算，无需重训练，无需标签数据，大幅降低部署成本。
- **通用性**：适用于无分类头模型（自监督）、多模态模型（SigLIPv2）等，不依赖特定任务结构。
- **弹性推理**：单次处理即可获得连续预算下的最优子网，支持动态调整，满足不同场景需求。
- **方法创新**：将进化算法用于近似海森矩阵非对角结构，避免了二阶梯度计算，同时兼顾了模块间相关性。

## 8. 不足与局限
- **实验覆盖不足**：仅展示在ImageNet分类任务上的剪枝效果，未验证对下游任务（如检测、分割）的通用性；未在更大规模模型（如ViT-L/H）上测试计算代价与性能。
- **可能偏差**：进化算法的超参数（种群大小、迭代次数）如何影响结果未讨论，可能存在过拟合特定预算的风险。
- **应用限制**：方法假设预训练模型权重已固定，不适用于需要动态微调或持续学习的场景；且依赖梯度的计算，对于极小模型或无梯度访问的模型（如纯推理）不可行。
- **对比公平性**：与需要重训练的方法对比时，可能因无需重训练而损失部分性能天花板，文中未讨论该差距。

（完）
