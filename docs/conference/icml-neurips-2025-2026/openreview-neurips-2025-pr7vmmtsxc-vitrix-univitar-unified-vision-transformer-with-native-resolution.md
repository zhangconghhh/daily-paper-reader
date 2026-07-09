---
title: "VITRIX-UniViTAR: Unified Vision Transformer with Native Resolution"
title_zh: VITRIX-UniViTAR：原生分辨率统一视觉Transformer
authors: "Limeng Qiao, Yiyang Gan, Bairui Wang, Jie Qin, Shuang Xu, Siqi Yang, Lin Ma"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=PR7VmMTsxC"
tags: ["query:dino-fg"]
score: 7.0
evidence: 统一视觉Transformer基础模型，支持原生分辨率分类
tldr: 该论文针对传统ViT固定输入分辨率忽视图像自然可变性的问题，提出UniViTAR系列视觉基础模型，通过多种架构升级（如2D RoPE）支持原生分辨率输入。该模型在统一视觉模态和原生分辨率场景下表现优异，可作为分类任务的通用骨干网络。
source: NeurIPS-2025-Accepted
selection_source: conference_retrieval
motivation: 传统ViT固定输入分辨率，牺牲空间上下文保真度。
method: 提出UniViTAR，整合多项架构升级实现原生分辨率支持。
result: 在统一视觉模态和原生分辨率任务上性能领先。
conclusion: UniViTAR可作为通用视觉基础模型，尤其适合需要保留细节的分类场景。
---

## Abstract
Conventional Vision Transformer streamlines visual modeling by employing a uniform input resolution, which underestimates the inherent variability of natural visual data and incurs a cost in spatial-contextual fidelity. While preliminary explorations have superficially investigated native resolution modeling, existing works still lack systematic training recipe from the visual representation perspective. To bridge this gap, we introduce Unified Vision Transformer with Native Resolution, i.e. UniViTAR, a family of homogeneous vision foundation models tailored for unified visual modality and native resolution scenario in the era of multimodal. Our framework first conducts architectural upgrades to the vanilla paradigm by integrating multiple advanced components. Building upon these improvements, a progressive training paradigm is introduced, which strategically combines two core mechanisms: (1) resolution curriculum learning, transitioning from fixed-resolution pretraining to native resolution tuning, thereby leveraging ViT’s inherent adaptability to variable-length sequences, and (2) visual modality adaptation via inter-batch image-video switching, which balances computational efficiency with enhanced temporal reasoning. In parallel, a hybrid training framework further synergizes sigmoid-based contrastive loss with feature distillation from a frozen teacher model, thereby accelerating early-stage convergence. Finally, trained exclusively on public accessible image-caption data, our UniViTAR family across multiple model scales from 0.3B to 1B achieves state-of-the-art performance on a wide variety of visual-related tasks. The code and models are available here.

---

## 论文详细总结（自动生成）

# 论文总结：VITRIX-UniViTAR: 原生分辨率统一视觉Transformer

## 1. 核心问题与整体含义（研究动机和背景）
- **研究动机**：传统 Vision Transformer (ViT) 强制使用统一的固定输入分辨率，忽略了自然视觉数据固有的分辨率可变性，导致空间-上下文保真度下降。
- **背景**：现有初步探索原生分辨率建模的工作，在视觉表示层面缺乏系统化的训练策略。
- **整体含义**：提出 UniViTAR 系列模型，旨在构建支持原生分辨率（即保持图像原始宽高比和分辨率）的统一视觉基础模型，尤其适合需要保留细节的分类场景。

## 2. 方法论：核心思想、关键技术细节
- **核心思想**：对经典 ViT 进行架构升级，并通过渐进式训练策略实现原生分辨率支持。
- **关键技术细节**：
  - **架构升级**：集成多种高级组件（例如 2D RoPE 位置编码等），增强对变长序列的适应性。
  - **渐进式训练范式**：
    - (1) **分辨率课程学习**：从固定分辨率预训练过渡到原生分辨率微调，利用 ViT 对变长序列的固有能力。
    - (2) **视觉模态适应**：通过批次间图像-视频切换（inter-batch image-video switching），平衡计算效率与时间推理能力。
  - **混合训练框架**：结合基于 Sigmoid 的对比损失与来自冻结教师模型的特征蒸馏，加速早期收敛。
- **公式/算法流程**：文本未给出具体公式，整体流程为：固定分辨率预训练 → 分辨率课程学习 → 原生分辨率微调，同时交织图像/视频数据训练，使用对比损失+蒸馏损失。

## 3. 实验设计
- **数据集**：仅说明使用公开可用的图像-标题数据（image-caption data）训练，未列出具体名称（如 LAION、CC 等）。
- **Benchmark**：多种视觉相关任务，包括分类、检测、分割等，但元数据特别提及在统一视觉模态和原生分辨率任务上性能领先。
- **对比方法**：未明确列出对比基线，但声称在多个模型规模（0.3B 到 1B）上达到 SOTA。

## 4. 资源与算力
- 论文文本**未明确说明**使用的 GPU 型号、数量及训练时长。
- 仅在元数据中提及模型规模从 0.3B 到 1B 参数，未提供计算资源细节。

## 5. 实验数量与充分性
- **实验数量**：由于摘要仅概述，未列出具体实验组数。但宣称在“大量”视觉相关任务上评估，涵盖不同规模模型。
- **充分性**：元数据评分 7.0/10，被 NeurIPS 2025 接收，说明实验设计被认为较充分。但未提供消融实验细节和数据集名称，客观性需通过全文判断。

## 6. 主要结论与发现
- UniViTAR 系列（0.3B~1B）在多种视觉任务上达到 SOTA 性能。
- 原生分辨率输入优于固定分辨率，能更好保留空间上下文。
- 渐进式训练范式和混合训练框架可有效加速收敛并提升模态适应能力。

## 7. 优点
- **架构创新**：结合 2D RoPE 等升级组件，适应原生分辨率。
- **训练策略**：分辨率课程学习 + 模态切换，兼顾数据多样性和效率。
- **通用性**：作为统一视觉基础模型，适用于分类、检测、分割等多种任务。
- **数据来源**：仅使用公开可访问的图像-标题数据，可复现性较好。

## 8. 不足与局限
- **实验细节缺失**：未说明具体数据集、对比方法、消融实验数量，难以独立复现对比。
- **算力信息空白**：未提供训练资源，难以评估成本。
- **应用限制**：仅针对视觉模态，未涉及多模态融合场景；仅支持图像和视频时间推理，可能对极端高分辨率场景仍有挑战。
- **偏差风险**：未讨论训练数据偏差（如图像-标题数据可能偏向英文、互联网内容）。

（完）
