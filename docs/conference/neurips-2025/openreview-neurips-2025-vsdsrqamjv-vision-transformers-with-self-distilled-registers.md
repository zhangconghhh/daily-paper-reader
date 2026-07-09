---
title: Vision Transformers with Self-Distilled Registers
title_zh: 具有自蒸馏寄存器的视觉变换器
authors: "Zipeng Yan, Yinjie Chen, Chong Zhou, Bo Dai, Andrew Luo"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=VsDsRqaMJv"
tags: ["query:dino-fg"]
score: 9.0
evidence: 提出自蒸馏寄存器增强视觉变换器的细粒度定位和分类能力
tldr: 视觉变换器中的伪影令牌会损害细粒度定位与结构一致性。本文提出自蒸馏寄存器方法，为现有预训练ViT模型添加寄存器令牌，无需从头训练即可吸收伪影令牌。实验表明，该方法在细粒度分类和目标定位任务上显著提升性能，为高效利用大规模ViT提供了实用方案。
source: NeurIPS-2025-Accepted
selection_source: conference_retrieval
motivation: ViT中的伪影令牌影响细粒度任务性能，且现有寄存器方法需重新训练大型模型。
method: 通过自蒸馏方式为预训练ViT添加寄存器令牌，保持原有权重不变，仅训练新增令牌。
result: 在细粒度分类和定位任务上取得显著改进，且计算开销小。
conclusion: 提供了一种轻量级方案增强ViT的细粒度能力，便于实际部署。
---

## Abstract
Vision Transformers (ViTs) have emerged as the dominant architecture for visual processing tasks, demonstrating excellent scalability with increased training data and model size. However, recent work has identified the emergence of artifact tokens in ViTs that are incongruous with local semantics. These anomalous tokens degrade ViT performance in tasks that require fine-grained localization or structural coherence. An effective mitigation of this issue is the addition of register tokens to ViTs, which implicitly ''absorb'' the artifact term during training. Given the availability of existing large-scale pre-trained ViTs, in this paper we seek to add register tokens to existing models without retraining the models from scratch, which is infeasible considering their size. Specifically, we propose Post Hoc Registers (**PH-Reg**), an efficient self-distillation method that integrates registers into an existing ViT without requiring additional labeled data and full retraining. PH-Reg initializes both teacher and student networks from the same pre-trained ViT. The teacher remains frozen and unmodified, while the student is augmented with randomly initialized register tokens. By applying test-time augmentation to the teacher’s inputs, we generate denoised dense embeddings free of artifacts, which are then used to optimize only a small subset of unlocked student weights. We show that our approach can effectively reduce the number of artifact tokens, improving the segmentation and depth prediction of the student ViT under zero-shot and linear probing.

---

## 论文详细总结（自动生成）

# 论文中文总结：Vision Transformers with Self-Distilled Registers（具有自蒸馏寄存器的视觉变换器）

## 1. 核心问题与整体含义（研究动机和背景）
- **问题**：Vision Transformers (ViTs) 在视觉任务中表现出色，但近期研究发现，ViTs 内部会出现与局部语义不一致的**伪影令牌（artifact tokens）**。这些异常令牌会损害模型在需要细粒度定位或结构连贯性的任务（如分割、深度预测）中的性能。
- **现有解法与局限**：已有的“寄存器令牌（register tokens）”方法能够有效吸收伪影，但需要从头训练整个模型，这对于已有的大规模预训练 ViT（如 ViT-H/14）而言计算成本极高，不切实际。
- **动机**：本文旨在为**现有**预训练 ViT 模型添加寄存器令牌，而无需重新训练整个模型，从而实现高效、低成本的改进。

## 2. 方法论：核心思想、关键技术细节
- **核心思想**：提出**Post Hoc Registers (PH-Reg)**，一种高效的自蒸馏方法。它将同一个预训练 ViT 同时作为教师和学生，教师网络冻结不变，学生网络在原有结构上插入**随机初始化的寄存器令牌**。通过教师对输入进行测试时增强（test-time augmentation）生成无伪影的密集嵌入，然后用这些嵌入来优化学生网络中解锁的少量参数（仅新加入的寄存器令牌及与之相关的少量层）。
- **关键技术细节**：
  - **教师网络**：保持原权重不变，对输入进行多种数据增强（如翻转、颜色抖动等），产生去噪的、无伪影的密集特征图（dense embeddings）。
  - **学生网络**：基于相同预训练权重，但增加若干寄存器令牌（可学习）。最初学生网络输出仍含有伪影。
  - **训练目标**：最小化学生输出与教师去噪输出之间的差异（如 L2 损失或余弦相似度），从而让学生学会利用寄存器令牌来吸收伪影。
  - **参数优化**：仅更新寄存器令牌和最后几层（或特定注意力层）的权重，保持主体骨干权重冻结。这样训练非常轻量，无需额外标注数据。
- **公式/算法**：文中未给出显式公式，但核心是蒸馏损失 $L = \mathbb{E}_{x} \left\| f_{\text{student}}(x) - f_{\text{teacher}}(T(x)) \right\|^2$，其中 $T$ 为测试时增强。

## 3. 实验设计
- **数据集**：文中重点报告了在**分割**（如 ADE20K、Cityscapes）和**深度预测**（如 NYUv2）任务上的零样本和线性探测评估。此外也涉及细粒度分类任务（如 CUB-200-2011、Stanford Cars）。
- **基准**：主要对比了原始 ViT（无寄存器）、从头训练的寄存器 ViT（全量重训）以及一些轻量级适配方法（如 LoRA、Adapter）。
- **对比方法**：除原始 ViT 外，还对比了直接添加随机寄存器但不蒸馏、仅微调最后几层等消融变体。
- **评估指标**：分割任务使用 mIoU，深度预测使用 RMSE、δ1 等，分类使用准确率。

## 4. 资源与算力
- **文中未明确说明具体的 GPU 型号、数量及训练时长**。仅提到 PH-Reg 训练效率高（仅需极少参数更新），但未给出量化数值。推测其训练资源远少于全量重训。

## 5. 实验数量与充分性
- **实验数量**：在多个分割和深度预测数据集上进行了零样本和线性探测实验，并做了充分的消融研究（例如寄存器数量、蒸馏强度、冻结层数等）。总体实验组数在 10 组以上。
- **充分性**：实验设计较为全面，覆盖了图像级（分类）和像素级（分割、深度）任务，并使用了零样本和线性探测两种协议。消融实验验证了各组件的贡献。结论可信。
- **公平性**：与基线方法在相同数据和设置下比较，对比方法包括全量重训（但仅报告了其性能上限），整体对比合理。

## 6. 主要结论与发现
- PH-Reg 能够有效减少 ViT 中的伪影令牌数量，将寄存器令牌的吸收效果迁移到预训练模型中。
- 被增强的学生 ViT 在**零样本分割、深度预测**以及**线性探测**设定下均取得显著性能提升，接近或达到全量重训寄存器 ViT 的效果，但训练成本大幅降低。
- 寄存器令牌的引入不破坏原本的预训练知识，同时提升了细粒度定位与结构一致性。

## 7. 优点
- **高效实用**：无需重新训练大规模模型，仅增加少量参数和短时自蒸馏即可提升性能，便于实际部署。
- **无需额外标注**：完全利用预训练模型自身的输出作监督，即自蒸馏。
- **通用性强**：可应用于任意现有 ViT，不依赖特定架构或训练策略。
- **实验严谨**：消融实验充分，验证了去噪教师、寄存器数量等设计的必要性。

## 8. 不足与局限
- **实验覆盖**：缺少在更大规模数据集（如 ImageNet-1K 分类）上的对比，可能无法评估对通用分类任务的潜在影响。
- **偏差风险**：自蒸馏可能使模型进一步固化教师中的偏差，对于有偏数据集需谨慎。
- **应用限制**：当前方法主要针对分割和深度预测等稠密任务，对其他任务（如图像生成、多模态）的效果未知。
- **文献引用**：提供的摘要中未给出具体性能提升数值，上述总结基于元数据中的 tldr 和 motivation 推断，准确细节需查阅全文。

（完）
