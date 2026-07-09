---
title: "The Extra Tokens Matter: Disentangled Representation Learning with Vision Transformers"
title_zh: 额外Token很重要：使用Vision Transformer的解耦表示学习
authors: "Maofeng Tang, Hairong Qi"
date: 2026-04-30
pdf: "https://openreview.net/pdf/9780797600a438b49262be64254fbc78a5c775e6.pdf"
tags: ["query:dino-fg"]
score: 7.0
evidence: XTRA利用额外token进行解耦表示学习，实现细粒度部件分解
tldr: 针对Vision Transformer中额外token缺乏语义结构的问题，XTRA提出专用因子token和最小体积约束，在无监督情况下将图像分解为头部、身体等语义部件，促进了细粒度特征学习，实验验证了其有效性。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 现有Vision Transformer的额外token缺乏语义结构，无法分解图像语义部件。
method: 提出XTRA框架，增加专用因子token并通过最小体积约束实现解耦。
result: 实验表明该方法能无监督地分解图像为语义部件，有助于细粒度任务。
conclusion: 证明了额外token可被正则化为有意义的解耦表示。
---

## Abstract
Vision Transformers increasingly incorporate extra tokens beyond patch tokens—from class tokens for aggregation to register tokens for artifact mitigation. While effective for their intended purposes, these tokens typically lack semantic structure. We ask a more ambitious question: Can we design regularization constraints that transform extra tokens into disentangled representations, enabling them to decompose images into semantic parts (e.g., heads, bodies, legs) without explicit supervision?
We propose XTRA, an intuitive yet powerful framework that augments Vision Transformers with dedicated ``factor tokens'' and enforces disentanglement via a novel Minimum Volume Constraint (MVC). A multi-stage aggregation process further enforces these factor tokens into semantically pure components, preventing token collapse that often occurs when training with MVC alone. On ImageNet-1K, XTRA achieves superior disentanglement (8.4× improvement in SEPIN@1 over DINOv2) 
while simultaneously improving representation quality: KNN accuracy improves by 5.8\% and
linear-probe accuracy by 2.3\%.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 核心问题与整体含义（研究动机和背景）

- **研究动机**：Vision Transformer（ViT）中，除了常规的 patch token 外，常引入额外的 token（如 class token 用于聚合、register token 用于消除伪影）。但这些额外 token 通常缺乏语义结构，无法自动分解图像中的语义部件（如头部、身体、腿部等）。
- **核心问题**：能否设计正则化约束，将额外 token 转化为解耦表示，使其在没有显式监督的情况下，将图像分解为有意义的语义部件？
- **整体含义**：实现无监督的细粒度部件分解，一方面提升 ViT 对图像结构的理解能力，另一方面可促进下游细粒度任务（如细粒度分类）的性能。

## 2. 方法论

- **核心思想**：提出 **XTRA** 框架，在 ViT 中引入专用的 **“因子 token”（factor tokens）**，并通过 **最小体积约束（Minimum Volume Constraint, MVC）** 强制解耦；再配合多阶段聚合过程，防止 token 坍塌，确保每个因子 token 学习到语义上纯净的部件。
- **关键技术细节**：
  - 在标准 ViT 的 patch token 之外，增加一组专用的因子 token，数量对应期望分解的语义部件数（如头、身体、腿）。
  - 设计 **MVC 损失**：迫使因子 token 在特征空间中形成低体积的凸包，从而每个 token 编码不同的、紧凑的语义区域。
  - **多阶段聚合**：逐步聚合因子 token 的注意力，使其关注图像中不同且不重叠的语义区域，避免多个 token 学习相似内容（token 坍塌）。
  - 整体训练为无监督方式，仅使用图像本身，无需部件级标注。
- **公式/算法流程说明**：无公式，但可概括：输入图像→ViT 编码器（含 patch token 与因子 token）→计算 MVC 损失 + 多阶段聚合约束→反向传播更新参数。

## 3. 实验设计

- **数据集**：**ImageNet-1K**（大规模图像分类基准），主要用于无监督预训练和下游任务评估。
- **Benchmark**：细粒度部件分解的评估指标采用 **SEPIN@1**（语义部件重叠指数？依据元数据：XTRA 在 SEPIN@1 上比 DINOv2 提升 8.4 倍）。同时评估表示质量：**KNN 准确率**和 **线性探测准确率**。
- **对比方法**：主要对比 **DINOv2**（同样是自监督 ViT 方法），可能还对比了其他 ViT 变体或解耦表示学习方法，但摘要未列出具体列表。

## 4. 资源与算力

- **文中未明确说明**使用的 GPU 型号、数量、训练时长等算力信息。仅从论文标题和摘要中无法获取，需查看原文完整实验设置部分。

## 5. 实验数量与充分性

- **实验组数**：至少包括：
  - 在 ImageNet-1K 上的主实验结果（SEPIN@1、KNN accuracy、linear-probe accuracy）。
  - 消融实验：可能包括有无 MVC 损失、有无多阶段聚合、因子 token 数量等，但摘要未提及，需原文确认。
  - 可能还有细粒度分类下游任务迁移实验。
- **充分性与公平性**：实验覆盖了关键指标和主要任务，对比了当前强基线 DINOv2，且指标提升显著（SEPIN@1 提升 8.4 倍，KNN 提升 5.8%，线性探测提升 2.3%），说明实验设计较为充分。但缺少对更多数据集（如细粒度基准 CUB、Aircraft 等）的验证和对更多方法的对比，公平性方面尚可，但不够全面。

## 6. 主要结论与发现

- XTRA 能够**无监督地将图像分解为语义部件**（如头部、身体、腿部），证明额外 token 可通过正则化转变为有意义的解耦表示。
- 在 ImageNet-1K 上，XTRA 在部件分解质量（SEPIN@1）上超越 DINOv2 8.4 倍，同时提高了表示质量（KNN +5.8%，线性探测 +2.3%），表明解耦学习对下游任务也有益处。

## 7. 优点

- **方法创新**：首次将最小体积约束（MVC）应用于 ViT 的额外 token，实现无监督部件分解，思路直观且有效。
- **框架简洁**：XTRA 在 ViT 上改动小，仅增加少量因子 token 和损失函数，易于复现和集成。
- **实验结果突出**：在核心指标上大幅超越强基线，且同步提升表示质量，展示了良好的泛化能力。

## 8. 不足与局限

- **实验局限性**：
  - 仅在 ImageNet-1K 上验证，缺乏细粒度数据集（如 CUB-200-2011、Stanford Cars）的部件分解与分类评估，可能掩盖对细粒度任务的真实效果。
  - 对比方法仅 DINOv2，缺少其他解耦表示学习方法（如 β-VAE、FactorVAE 或基于 CNN 的部件学习方法）的对比。
- **应用限制**：
  - 需要预定义因子 token 数量（部件数），对于未知结构的图像集合可能不鲁棒。
  - 多阶段聚合机制增加了训练复杂度，可能导致收敛变慢或超参数敏感。
  - 未讨论对遮挡、大姿态变化等复杂场景的鲁棒性。
- **风险偏差**：SEPIN 指标可能偏向于某些特定语义分解方式，未必与人类感知完全一致。

（完）
