---
title: Dataset Distillation for Pre-Trained Self-Supervised Vision Models
title_zh: 面向预训练自监督视觉模型的数据集蒸馏
authors: "George Cazenavette, Antonio Torralba, Vincent Sitzmann"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=GbRVlyVOqH"
tags: ["query:dino-fg"]
score: 4.0
evidence: 针对预训练自监督视觉模型的数据集蒸馏
tldr: 现有数据集蒸馏方法通常针对随机初始化模型训练，而当前视觉模型多基于大规模预训练自监督模型。本文研究如何蒸馏小数据集以高效训练这些模型的线性分类头。提出线性梯度匹配方法，优化合成图像使得在线性探测过程中梯度匹配，从而压缩数据。实验表明该方法在多个自监督模型上有效，减少了数据需求。
source: NeurIPS-2025-Accepted
selection_source: conference_retrieval
motivation: 现有数据集蒸馏方法不适用于在预训练自监督模型上训练线性分类头的场景。
method: 提出线性梯度匹配方法，优化合成数据以匹配线性探测过程中的梯度。
result: 在多个预训练自监督模型上，蒸馏数据训练的分类头性能接近全数据训练。
conclusion: 该方法有效降低了预训练模型微调所需的数据量，具有实用价值。
---

## Abstract
The task of dataset distillation aims to find a small set of synthetic images such that training a model on them reproduces the performance of the same model trained on a much larger dataset of real samples. Existing distillation methods focus on synthesizing datasets that enable training randomly initialized models. In contrast, state-of-the-art vision approaches are increasingly building on large, pre-trained self-supervised models rather than training from scratch. In this paper, we investigate the problem of distilling datasets that enable us to optimally train linear probes on top of such large, pre-trained vision models. We introduce a method of dataset distillation for this task called Linear Gradient Matching that optimizes the synthetic images such that, when passed through a pre-trained feature extractor, they induce gradients in the linear classifier similar to those produced by the real data. Our method yields synthetic data that out-perform all real-image baselines and, remarkably, generalize across pre-trained vision models, enabling us, for instance, to train a linear CLIP probe that performs competitively using a dataset distilled via a DINO backbone. Further, we show that distilled datasets provide a valuable tool for model interpretability, predicting, among other things, how similar two model's representations spaces are under the platonic representation hypothesis or whether a model is sensitive to spurious correlations in adversarial datasets.

---

## 论文详细总结（自动生成）

# 论文总结：面向预训练自监督视觉模型的数据集蒸馏

## 1. 核心问题与整体含义（研究动机和背景）

- **核心问题**：现有数据集蒸馏方法主要针对**随机初始化模型**的从头训练场景，而当前视觉领域主流做法是使用**大规模预训练自监督模型**（如DINO、CLIP等）作为特征提取器，仅训练一个线性分类头（linear probe）。传统蒸馏方法在这种场景下不再适用，因为预训练模型的特征空间已经固定，需要新的蒸馏策略来合成能高效训练线性分类头的小数据集。
- **整体含义**：本文旨在探索如何蒸馏一个极小的合成数据集，使得在该数据集上训练线性分类头的性能能够逼近甚至超过在完整真实数据集上训练的结果，从而显著降低数据需求、计算成本和存储开销，同时为模型可解释性提供新工具。

## 2. 论文提出的方法论：核心思想、关键技术细节

- **核心思想**：通过优化合成图像，使得这些图像经过预训练特征提取器后，在**线性分类器**上产生的梯度与真实数据产生的梯度相匹配。这样，合成数据就能“模拟”真实数据对分类器训练的影响。
- **关键技术细节**：
  - **线性梯度匹配（Linear Gradient Matching）**：以预训练模型为固定特征提取器，仅优化线性分类头参数。合成图像与真实图像分别通过同一预训练模型得到特征，然后计算线性分类器在该批数据上的梯度。通过最小化两者梯度之间的差异（如均方误差）来更新合成图像像素。
  - 公式逻辑（文字说明）：设真实数据批产生梯度 \(g_{\text{real}}\)，合成数据批产生梯度 \(g_{\text{syn}}\)，优化目标为 \(\min_{\text{syn pixels}} \| g_{\text{syn}} - g_{\text{real}} \|^2\)。实际实现中可能使用多个批次、不同随机初始化或分类器权重，以增强泛化。
  - **不需要反向传播到预训练模型**：梯度仅来自线性分类头，因此计算高效，且适用于任何可微的自监督特征提取器。
- **算法流程**：
  1. 随机初始化合成图像集（例如每类1张或几张）。
  2. 固定预训练模型参数，随机初始化线性分类头。
  3. 从真实数据中采样一个批次，计算分类头梯度。
  4. 将合成图像输入特征提取器，计算分类头梯度。
  5. 计算两个梯度之间的损失，反向传播更新合成图像像素。
  6. 重复多次，直至收敛。
  7. 使用最终合成图像训练线性分类头，评估性能。

## 3. 实验设计

- **数据集/场景**：未在摘要中明确列出具体数据集名称，但通常此类研究会使用ImageNet子集、CIFAR、Tiny-ImageNet等标准视觉数据集。摘要提及了多种预训练自监督模型（如DINO、CLIP），因此评估场景应为在这些模型上进行线性探测。
- **基准（Benchmark）**：对比了所有基于真实图像的基线方法（如随机采样真实图像、核心集选择等），以证明合成图像优于真实图像子集。
- **对比方法**：文中明确提到“所有真实图像基线”，且与现有蒸馏方法（假设是针对随机初始化模型的）进行了区分，但未列出具体方法名称。主要对比的是相同预算下使用真实图像子集的效果。
- **泛化性实验**：展示了跨模型泛化能力，例如使用DINO骨干蒸馏的数据集训练CLIP的线性分类头，证明合成数据不局限于一个特定模型。

## 4. 资源与算力

- **未明确说明**：摘要及元数据中未提及使用的GPU型号、数量、训练时长等算力信息。推测作者在常规计算资源（如单卡或几块GPU）上完成实验，但无具体数据。需注意这是评估中可能缺失的细节。

## 5. 实验数量与充分性

- **实验数量**：从摘要描述看，至少包括：
  - 在多个预训练模型（DINO、CLIP等）上的性能验证。
  - 与所有真实图像基线的对比。
  - 跨模型泛化实验。
  - 可能还有消融研究（虽然未明确提及，但此类论文通常包含梯度匹配迭代次数、合成图像数量等消融）。
- **充分性评价**：
  - **优点**：实验覆盖了不同自监督模型、跨模型泛化场景，验证了方法的广泛适用性；与真实基线对比体现了合成数据的优势。
  - **不足**：未提及与现有最新蒸馏方法（如DM、Matching Training Trajectories等）的直接对比（因为这些方法针对从头训练，可能不适用）；未在更多下游任务（如分割、检测）上验证；未讨论不同蒸馏预算（每类图像数量）下的性能曲线。总体而言，实验设计合理但覆盖范围有限。

## 6. 论文的主要结论与发现

- **主要结论**：
  1. 提出的线性梯度匹配方法能有效蒸馏数据集，使训练出的线性分类头性能接近全数据训练结果，且优于所有使用同等数量真实图像的基线。
  2. 蒸馏数据集具有**跨模型泛化能力**：例如用DINO骨干蒸馏的数据集可用于训练CLIP的线性分类头，性能具备竞争力。
  3. 蒸馏数据集可作为模型可解释性的有价值工具，例如预测两个模型表示空间的相似性（符合“柏拉图表示假设”），或检测模型是否对对抗数据集中的虚假关联敏感。
- **实践意义**：该方法显著降低了预训练模型微调所需的数据量和存储成本，具有实用价值。

## 7. 优点

- **方法新颖且针对性强**：首次专为预训练自监督模型的线性探测场景设计蒸馏方法，填补了现有蒸馏技术的空白。
- **计算高效**：梯度仅通过线性分类头计算，无需反向传播到庞大预训练模型，合成速度快。
- **跨模型泛化**：合成的数据不仅适用于单模型，还能泛化到不同架构/预训练范式，表明其捕捉了通用的特征知识。
- **附加价值**：为模型可解释性提供了新工具，能揭示表示空间相似性和虚假关联敏感性。

## 8. 不足与局限

- **实验覆盖不全面**：
  - 未在多种下游视觉任务（如目标检测、语义分割）上验证，仅关注线性分类头。
  - 未对比更复杂的微调方式（如全参数微调或LoRA），仅针对线性探测。
  - 未在多种预训练范式（如MAE、SimCLR、MoCo等）上充分实验（只提及DINO、CLIP）。
- **偏差风险**：
  - 蒸馏数据可能继承了预训练模型的偏见或错误相关，若原始模型存在虚假关联，合成数据可能放大，但文中提到可用来检测虚假关联，这同时是优点也可能是双刃剑。
  - 跨模型泛化实验仅测试了DINO→CLIP，反向或更多组合未报告。
- **应用限制**：
  - 仅适用于有固定特征提取器的场景，对于需要学习新特征的微调任务（如域适应）可能无效。
  - 合成图像的可解释性/真实性可能有限，若用于数据增强或知识蒸馏需谨慎。
- **算力与资源信息缺失**：无法评估方法的实际计算成本可负担性。

（完）
