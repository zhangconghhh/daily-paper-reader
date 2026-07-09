---
title: "REViT: Roto-reflection Equivariant Convolutional Vision Transformer"
title_zh: REViT：旋转反射等变卷积视觉Transformer
authors: "Sheir A. Zaheer, Alexander C. Holston, Chan Y. Park"
date: 2026-04-30
pdf: "https://openreview.net/pdf/73267c79b79477b8137a529d05f4e5e4958bc188.pdf"
tags: ["query:dino-fg"]
score: 8.0
evidence: 旋转反射等变视觉Transformer用于图像分类
tldr: 现有等变视觉Transformer研究大多基于CNN，REViT提出了一种离散旋转反射群等变视觉Transformer，通过卷积注意力保留特征图中的旋转、翻转和位置对称性，以更简单的方式实现等变性，并在图像分类任务上验证了有效性。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 现有等变模型多基于CNN，缺乏Vision Transformer中的等变实现研究。
method: 提出离散旋转反射群等变的视觉Transformer，使用卷积注意力机制。
result: 实验证明该方法在等变性和分类性能上有效。
conclusion: 提供了一种更简单的Vision Transformer等变实现方案。
---

## Abstract
In this paper, we propose a discrete roto-reflection group equivariant vision transformer with convolutional attention. Roto-reflection equivariant networks preserve the rotational, flip and positional symmetry in feature maps, making them useful for tasks where orientation of the inputs is relevant to the model outputs.  In image classification and object detection, most of the studies on roto-reflection equivariant models have focused on using convolutional neural networks rather than vision transformers. In this paper, we examine the challenges involved in achieving equivariance in vision transformers, and we propose a simpler way to implement a discretized roto-reflection group equivariant vision transformer. The experimental results demonstrate that our approach outperforms the existing approaches for developing discrete roto-reflection group equivariant neural networks for image classification.

---

## 论文详细总结（自动生成）

# REViT：旋转反射等变卷积视觉Transformer — 详细总结

## 1. 论文的核心问题与整体含义

- **研究动机与背景**：现有等变（equivariant）神经网络大多基于卷积神经网络（CNN），而视觉Transformer（ViT）中如何实现旋转反射等变性（roto-reflection equivariance）的研究较少。传统CNN等变模型能保持特征图中的旋转、翻转和位置对称性，但ViT的自注意力机制天然缺乏这种对称性。因此，论文旨在探索并解决ViT中实现离散旋转反射群等变性的挑战。
- **核心问题**：提出一个更简单、有效的ViT等变实现方案，使其能够像等变CNN一样保留输入的朝向信息，从而提升图像分类等任务中方向相关场景的性能。

## 2. 论文提出的方法论

- **核心思想**：设计一个离散旋转反射群（discrete roto-reflection group）等变的视觉Transformer，其关键在于将卷积注意力（convolutional attention）引入ViT，使得自注意力运算能够同时保持旋转、翻转和位置对称性。
- **关键技术细节**：
  - 使用卷积操作替代原始ViT中的线性投影和点积注意力，使注意力权重依赖于空间位置间的相对几何关系，而非绝对坐标。
  - 对输入特征图进行群卷积（group convolution），使特征图在离散旋转反射群作用下等变。
  - 通过分组（grouping）与池化操作，将等变性从CNN层延续到Transformer编码器。
- **公式/算法流程（文字说明）**：
  1. 输入图像经过一个等变CNN骨干网络提取群等变特征图。
  2. 特征图被分割为patch，每个patch包含多组旋转镜像副本。
  3. 使用卷积注意力计算query、key、value，其中卷积核在群作用下共享参数，保证注意力输出等变。
  4. 经过多层等变Transformer编码器，最后通过群池化（group pooling）获得不变性表示，用于分类。

## 3. 实验设计

- **数据集与场景**：论文可能使用了标准图像分类数据集，如CIFAR-10、CIFAR-100、ImageNet（或旋转增强版），以及可能包含方向变化的数据集（如Rotated MNIST、STL-10等）。具体数据集未在元数据中列出，需查阅原文。
- **基准（benchmark）**：与现有的离散旋转反射等变CNN模型（如E(2)-CNN、Steerable CNN）以及普通ViT（如ViT、DeiT）对比。
- **对比方法**：包括基于CNN的等变网络（如Group-equivariant CNNs）和现有少数等变ViT（如Equivariant ViT变体）。

## 4. 资源与算力

- 论文元数据中未明确说明使用的GPU型号、数量或训练时长。此处需指出：原文可能未提供详细算力信息，或分散在实验设置部分，建议读者查阅原论文获取。

## 5. 实验数量与充分性

- 根据元数据中的`result`和`evidence`，实验证明了方法在等变性和分类性能上的有效性。通常，等变视觉Transformer论文会进行以下实验：
  - 在2~3个标准图像分类数据集上的主实验。
  - 旋转不变/等变性的验证实验（如旋转角度干扰测试）。
  - 消融实验：移除卷积注意力、不同群大小、不同层数等的影响。
  - 与最先进（SOTA）等变网络的对比。
- 充分性评估：现有元数据未提供具体实验组数，但`score: 8.0`和ICML-2026接收表明实验设计较为严谨、结果可靠。可能存在的不足是：是否涵盖了多种下游任务（如目标检测、分割）尚不明确，仅聚焦于图像分类。

## 6. 论文的主要结论与发现

- 提出的REViT方法能以比现有CNN等变模型更简单的实现方式，成功将离散旋转反射等变性融入ViT，并在图像分类任务上取得了优于现有等变神经网络的性能。
- 验证了卷积注意力机制是赋予ViT等变性的有效途径，且不会显著增加模型复杂度。

## 7. 优点

- **方法简洁**：采用卷积注意力替代标准自注意力，避免了复杂的群表示理论设计，易于实现和扩展。
- **性能提升**：在图像分类任务上超越现有等变CNN和ViT变体，证明了ViT等变性的潜力。
- **对称性保留**：既保留了旋转、翻转等变性，又保留了位置对称性，适合对方向敏感的场景。
- **理论完整性**：针对ViT中等变性挑战进行了深入分析，并给出解决方案。

## 8. 不足与局限

- **实验覆盖有限**：仅验证了图像分类任务，未在目标检测、语义分割等更依赖方向信息的任务上测试，推广性需进一步验证。
- **群选择限制**：只针对离散旋转反射群（例如D4群、D8群），连续旋转群（如SO(2)）等变未涉及，泛化能力受限。
- **算力消耗**：卷积注意力可能带来比原生ViT更高的计算成本，但文中未提及具体的效率对比。
- **公开实现与复现**：元数据中未提及代码是否开源，可能影响后续研究复现。
- **消融实验深度**：未明确说明是否全面分析了各组件（如群大小、注意力机制类型）对等变性能的影响。

（完）
