---
title: Elastic ViTs from Pretrained Models without Retraining
title_zh: 无需重新训练的弹性ViTs剪枝方法
authors: "Walter Simoncini, Michael Dorkenwald, Tijmen Blankevoort, Cees G. M. Snoek, Yuki M Asano"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=OU6FXkSIe0"
tags: ["query:dino-fg"]
score: 6.0
evidence: 在DINO模型上进行实验，弹性剪枝方法适用于视觉基础模型
tldr: 当前视觉基础模型仅在有限的预设尺寸下可用，限制了实际部署。本文提出SnapViT，一种无需重新训练的剪枝方法，通过梯度信息与进化算法实现弹性推理。在DINO、SigLIPv2、DeIT和AugReg等模型上的实验表明，该方法在不同计算预算下均优于现有技术。SnapViT为视觉Transformer的灵活部署提供了高效解决方案，适用于分类等下游任务。
source: NeurIPS-2025-Accepted
selection_source: conference_retrieval
motivation: 旨在解决视觉基础模型尺寸固定导致的部署次优问题。
method: 提出SnapViT，结合梯度与进化算法的结构化剪枝方法，无需标注数据和重新训练。
result: 在DINO等模型上剪枝性能超越现有方法，支持连续计算预算的弹性推理。
conclusion: SnapViT实现了高效、无需重新训练的ViT模型弹性部署。
---

## Abstract
Vision foundation models achieve remarkable performance but are only available in a limited set of pre-determined sizes, forcing sub-optimal deployment choices under real-world constraints. We introduce SnapViT: single-shot network approximation for pruned Vision Transformers, a new post-pretraining structured pruning method that enables elastic inference across a continuum of compute budgets. Our approach efficiently combines gradient information with cross-network structure correlations, approximated via an evolutionary algorithm, does not require labeled data, generalizes to models without a classification head, and is retraining-free. Experiments on DINO, SigLIPv2, DeIT, and AugReg models demonstrate superior performance over state-of-the-art methods across various sparsities, requiring less than five minutes on a single A100 GPU to generate elastic models that can be adjusted to any computational budget. Our key contributions include an efficient pruning strategy for pretrained Vision Transformers, a novel evolutionary approximation of Hessian off-diagonal structures, and a self-supervised importance scoring mechanism that maintains strong performance without requiring retraining or labels. Code and pruned models are available at: https://elastic.ashita.nl/

---

## 论文详细总结（自动生成）

# 论文中文总结

## 1. 核心问题与整体含义（研究动机和背景）

- **问题**：当前视觉基础模型（ViT）仅以少数预定义的固定尺寸发布，导致在实际部署中针对不同计算预算只能做出次优选择。许多下游任务需要模型在推理时动态调整计算量（弹性推理），但现有方法往往需要重新训练或依赖标注数据。
- **动机**：提出一种无需重新训练、无需标注数据的结构化剪枝方法，使得预训练的ViT模型能够沿连续计算预算弹性调整。

## 2. 方法论：核心思想、关键技术细节

- **方法名称**：SnapViT（Single-shot network approximation for pruned Vision Transformers）。
- **核心思想**：将梯度信息与跨网络结构相关性结合，通过进化算法近似结构相关性，实现一次性剪枝，支持任意计算预算的弹性推理。
- **关键技术细节**：
  - 无需标注数据，适用于无分类头的模型（如自监督模型DINO）。
  - 利用Hessian矩阵的非对角结构近似（通过进化算法），高效评估剪枝对损失的影响。
  - 自监督重要性评分机制：基于梯度信息对每个结构单元（如注意力头、MLP维度）的重要性进行排序，同时考虑结构之间的依赖关系。
  - 一次性剪枝后生成一组弹性子网络，推理时可根据目标计算预算直接选择对应子网络，无需重新训练。
- **算法流程**（文字说明）：
  1. 对预训练ViT模型，计算每个剪枝单元的梯度（基于少量无标签样本）。
  2. 使用进化算法近似Hessian矩阵的非对角项，捕获不同结构单元间的相关性。
  3. 结合梯度和相关性信息，计算每个单元的自监督重要性分数。
  4. 根据重要性分数排序，生成从最稀疏到最不稀疏的一系列子网络（弹性模型）。
  5. 推理时根据计算预算直接选取对应子网络，无需额外微调。

## 3. 实验设计

- **使用的模型**：DINO、SigLIPv2、DeIT、AugReg 等视觉Transformer模型。
- **数据集/场景**：分类任务（下游任务，如ImageNet等标准基准）。具体数据集未在摘要中列出，但推测为ImageNet-1K或类似分类基准。
- **Benchmark与对比方法**：与现有最先进的结构化剪枝方法对比，包括需要重新训练的方法和无需重新训练的方法。
- **评估指标**：各稀疏度下的分类精度（Top-1准确率等）。

## 4. 资源与算力

- **硬件**：单张A100 GPU。
- **时间**：生成弹性模型（所有子网络）耗时**不超过5分钟**。
- **说明**：文中明确给出了算力配置和耗时，表明该方法高效。

## 5. 实验数量与充分性

- **实验数量**：覆盖了多种预训练模型（DINO、SigLIPv2、DeIT、AugReg），每个模型在不同稀疏度下进行对比，并可能有消融实验（如对进化算法、梯度重要性评分等的消融）。
- **充分性**：模型种类多，涵盖自监督（DINO）、CLIP风格（SigLIPv2）、有监督（DeIT、AugReg），具有较好的代表性。对比的方法也是SOTA。但摘要未提及独立数据集上的迁移实验（如检测、分割），可能实验主要聚焦分类。整体实验设计较为充分、客观。

## 6. 主要结论与发现

- SnapViT在各种稀疏度下均优于现有方法，且生成弹性模型速度快（<5分钟）。
- 无需重新训练、无需标注数据，即可获得可任意调整计算预算的弹性ViT。
- 该方法可推广到没有分类头的自监督模型，通用性强。

## 7. 优点

- **高效**：单次剪枝即可生成连续计算预算的弹性模型，推理时无需调整。
- **无标注依赖**：完全自监督，适用于任何预训练ViT。
- **无需重新训练**：避免了昂贵的微调成本。
- **通用性强**：支持多种架构和训练范式（自监督、CLIP、有监督）。
- **创新点**：首次将Hessian非对角结构进化近似与梯度信息结合，用于ViT弹性剪枝。

## 8. 不足与局限

- **可能的应用范围有限**：实验主要基于分类任务，缺少在检测、分割等密集预测任务上的验证。
- **理论分析不足**：对于进化近似Hessian非对角结构的收敛性、泛化能力缺乏严格理论保证。
- **偏差风险**：仅用少量无标签样本计算梯度，可能对某些数据分布不鲁棒。
- **对比方法的选择**：是否包含了同样无需重新训练的最新剪枝方法？摘要未详细说明，可能存在对比遗漏。
- **超参数敏感性**：进化算法的参数（种群大小、迭代轮数）可能影响结果，但未讨论鲁棒性。

（完）
