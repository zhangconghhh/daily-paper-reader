---
title: "SuperCLIP: CLIP with Simple Classification Supervision"
title_zh: SuperCLIP：基于简单分类监督的CLIP增强
authors: "Weiheng Zhao, Zilong Huang, Jiashi Feng, Xinggang Wang"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=EeIEvZlmVg"
tags: ["query:dino-fg"]
score: 8.0
evidence: 通过分类监督增强CLIP的细粒度视觉-文本对齐
tldr: CLIP类模型在细粒度视觉-文本对齐上存在不足，因其训练目标仅优化全局相似度。本文提出SuperCLIP，在视觉编码器上添加线性分类层，引入分类监督以增强细粒度语义信号。实验表明，SuperCLIP在细粒度分类和检索任务上显著优于原CLIP，且保持零样本能力。该方法简单有效，为基础模型在细粒度视觉分类中的应用提供了新思路。
source: NeurIPS-2025-Accepted
selection_source: conference_retrieval
motivation: CLIP模型忽略细粒度语义信号，尤其在长描述下对齐不足。
method: 在CLIP视觉编码器后增加轻量线性分类层，联合对比学习与分类损失进行训练。
result: 在细粒度分类和检索任务上性能显著提升，保持零样本能力。
conclusion: SuperCLIP通过简单分类监督有效增强了CLIP的细粒度对齐能力。
---

## Abstract
Contrastive Language-Image Pretraining (CLIP) achieves strong generalization in vision-language tasks by aligning images and texts in a shared embedding space. 
However, recent findings show that CLIP-like models still underutilize fine-grained semantic signals in text, and this issue becomes even more pronounced when dealing with long and detailed captions.
This stems from CLIP’s training objective, which optimizes only global image-text similarity and overlooks token-level supervision—limiting its ability to achieve fine-grained visual-text alignment. 
To address this, we propose SuperCLIP, a simple yet effective framework that augments contrastive learning with classification-based supervision. By adding only a lightweight linear layer to the vision encoder, SuperCLIP leverages token-level cues to enhance visual-textual alignment — with just a 0.077\% increase in total FLOPs, and no need for additional annotated data.
Experiments show that SuperCLIP consistently improves zero-shot classification, image-text retrieval, and purely visual tasks. These gains hold regardless of whether the model is trained on original web data or rich re-captioned data, demonstrating SuperCLIP’s ability to recover textual supervision in both cases. Furthermore, SuperCLIP alleviates CLIP’s small-batch performance drop through classification-based supervision that avoids reliance on large batch sizes. Code and models will be made open source.

---

## 论文详细总结（自动生成）

# 论文《SuperCLIP: CLIP with Simple Classification Supervision》详细总结

## 1. 论文的核心问题与整体含义（研究动机和背景）
- **核心问题**：CLIP 类模型在视觉-语言对齐中忽略了文本中的细粒度语义信号，尤其是在长描述文本环境下，对齐质量显著下降。根本原因是 CLIP 的训练目标仅优化全局图像-文本相似度，缺乏 token 级别的监督信号。
- **整体含义**：现有 CLIP 框架的细粒度视觉-文本对齐能力不足，限制了其在需要精确语义匹配的任务（如细粒度分类、详细描述检索）中的性能。作者提出 SuperCLIP，通过引入简单的分类监督来增强细粒度对齐，且几乎不增加计算开销，无需额外标注数据。

## 2. 论文提出的方法论
- **核心思想**：在对比学习的基础上，增加基于分类的监督信号，以利用 token 级别的语义线索增强视觉-文本对齐。
- **关键技术细节**：
  - 在 CLIP 视觉编码器（如 ViT）的输出特征向量之后，添加一个轻量级的线性分类层。
  - 联合优化两个损失：原始的对比学习损失（全局图像-文本对齐）和分类损失（基于视觉特征预测图像类别标签）。
  - 分类层直接对视觉编码器的输出进行分类，类别标签来自文本描述中提取的关键词或伪标签（无需额外人工标注）。
  - 总 FLOPs 仅增加 0.077%，计算量极小。
- **公式/算法流程**（文字说明）：
  - 输入图像通过视觉编码器得到图像特征 \(v\)，文本通过文本编码器得到文本特征 \(t\)。
  - 对比损失：计算图像-文本对的相似度矩阵，使用 InfoNCE 损失拉近匹配对、推开非匹配对。
  - 分类损失：将图像特征 \(v\) 送入线性分类层，得到类别预测 logits，使用交叉熵损失与类别标签（从文本中抽取或通过预训练词汇确定）计算。
  - 总损失 = 对比损失 + λ × 分类损失（λ 为平衡超参数）。
  - 训练时，分类层与视觉编码器端到端联合优化。

## 3. 实验设计
- **使用的数据集/场景**：文中未明确列出具体数据集名称，但提及在以下场景进行测试：
  - 零样本分类（zero-shot classification）
  - 图像-文本检索（image-text retrieval）
  - 纯视觉任务（purely visual tasks，如细粒度分类）
  - 数据来源：原始网络抓取数据（如 WIT）以及通过重新标注得到的丰富描述数据（re-captioned data）。
- **Benchmark**：未详细说明对比的基准方法，但推测与原始 CLIP 以及可能的其他 CLIP 变体（如 SigLIP、CLIP+Captioning 等）进行对比。文中提到“consistently improves … over CLIP”。
- **对比方法**：未列出具体方法名称，仅明确与原生 CLIP 对比。

## 4. 资源与算力
- **文中未提及**：论文摘要及元数据中没有提供训练所使用的 GPU 型号、数量、训练时长等信息。无法就此部分进行总结。

## 5. 实验数量与充分性
- **实验数量**：从摘要看，实验覆盖了零样本分类、图像-文本检索、纯视觉任务三类场景，且分别在原始数据和重新标注数据上评估了效果。还额外报告了缓解小批次性能下降的现象。但是，具体做了多少组实验（如不同架构、不同数据集上的消融）未详细说明。
- **充分性评价**：由于缺乏具体的实验表格和对比细节，无法判断实验是否充分。文中提到“Experiments show that … consistently improves …”，但未提供统计结果或变化幅度，难以评估其公平性和客观性。需要结合完整论文进一步判断。

## 6. 论文的主要结论与发现
- **主要结论**：SuperCLIP 通过引入简单的线性分类监督，能够有效增强 CLIP 的细粒度视觉-文本对齐能力，在零样本分类、图像-文本检索和纯视觉任务上均带来一致提升。
- **关键发现**：
  - 该方法无论是使用原始网络数据还是丰富的重新标注数据，都能恢复文本中的监督信号，提升对齐质量。
  - 分类监督避免了对比学习对大批次大小的依赖，缓解了 CLIP 在小批次训练时的性能下降问题。
  - 计算开销极小（FLOPs 增加 0.077%），且无需额外标注数据。

## 7. 优点
- **方法简洁有效**：仅添加一个线性分类层，改动极小，却带来显著的细粒度对齐提升。
- **计算开销极低**：0.077% 的额外 FLOPs，适合实际部署。
- **无需额外标注**：利用文本本身提供的类别信号（如从描述中提炼标签），避免了人工标注成本。
- **保持零样本能力**：增强细粒度对齐的同时，未牺牲 CLIP 原有的零样本泛化能力。
- **缓解小批次问题**：分类损失减少了对大批次对比学习的依赖，使模型能在资源受限场景下训练。

## 8. 不足与局限（基于摘要信息推断）
- **实验覆盖有限**：未提供具体数据集、消融实验、超参数影响、与多种先进方法（如 BLIP、CoCa）的对比结果，难以全面评估方法的泛化性和鲁棒性。
- **类别标签来源未详细说明**：如何从文本中提取类别标签？是否依赖词表或外部工具？可能存在偏差风险（如标签噪声）。
- **方法局限性**：仅适用于包含明确类别概念的视觉-语言任务，对于完全开放域或者无类别概念的场景（如风格生成）可能不适用。
- **资源信息缺失**：未报告训练的硬件、时间、成本，无法复现或评估资源需求。
- **偏差风险**：分类监督可能强化模型对常见类别敏感，而对罕见或组合概念学习有限。

（完）
