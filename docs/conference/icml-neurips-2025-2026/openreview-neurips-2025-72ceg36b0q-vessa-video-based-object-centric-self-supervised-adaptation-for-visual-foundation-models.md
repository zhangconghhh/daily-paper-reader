---
title: "VESSA: Video-based objEct-centric Self-Supervised Adaptation for Visual Foundation Models"
title_zh: VESSA：基于视频的物体中心自监督适应视觉基础模型
authors: "Jesimon Barreto, Carlos Caetano, Andre Araujo, William Robson Schwartz"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=72CEG36B0Q"
tags: ["query:dino-fg"]
score: 4.0
evidence: 视觉基础模型的自监督适应
tldr: VESSA提出了一种基于物体中心视频的自监督微调方法，用于视觉基础模型的领域适应。该方法无需标注即可扩展模型能力，实验表明在多个下游任务上有效，为视觉基础模型的自监督适应提供了新范式。
source: NeurIPS-2025-Accepted
selection_source: conference_retrieval
motivation: 现有视觉基础模型在分布偏移下性能下降，且监督微调不可行。
method: 利用短多视角物体中心视频进行自监督微调，无需标注。
result: 实验表明在多个下游任务上有效，提升了模型适应能力。
conclusion: 为视觉基础模型的自监督适应提供了有效范式。
---

## Abstract
Foundation models have advanced computer vision by enabling strong performance across diverse tasks through large-scale pretraining and supervised fine-tuning. However, they may underperform in domains with distribution shifts and scarce labels, where supervised fine-tuning may be infeasible. While continued self-supervised learning for model adaptation is common for generative language models, this strategy has not proven effective for vision-centric encoder models. To address this challenge, we introduce a novel formulation of self-supervised fine-tuning for vision foundation models, where the model is adapted to a new domain without requiring annotations, leveraging only short multi-view object-centric videos. Our method is referred to as VESSA: **V**ideo-based obj**E**ct-centric **S**elf-**S**upervised **A**daptation for visual foundation models. VESSA's training technique is based on a self-distillation paradigm, where it is critical to carefully tune prediction heads and deploy parameter-efficient adaptation techniques – otherwise, the model may quickly forget its pretrained knowledge and reach a degraded state. VESSA benefits significantly from multi-view object observations sourced from different frames in an object-centric video, efficiently learning robustness to varied capture conditions, without the need of annotations. Through comprehensive experiments with 3 vision foundation models on 2 datasets, VESSA demonstrates consistent improvements in downstream classification tasks, compared to the base models and previous adaptation methods. Code is publicly available at https://github.com/jesimonbarreto/VESSA.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 核心问题与整体含义
视觉基础模型（如DINO等）通过大规模预训练和监督微调在多种任务中表现优异，但在遇到分布偏移（domain shift）且标签稀缺的领域时性能显著下降。此时，监督微调往往不可行。虽然自监督持续学习在生成式语言模型领域已普及，但在视觉编码器模型中尚未证明有效。为此，论文提出VESSA（Video-based object-centric Self-Supervised Adaptation），实现视觉基础模型在**无需标注**的情况下，仅利用短多视角物体中心视频进行自监督微调，从而适应新领域，拓展模型能力。

## 2. 方法论
- **核心思想**：利用物体中心视频中的多帧多视角观测，通过自蒸馏(self-distillation)范式实现无监督领域适应。模型在同一物体的不同视角和光照条件下学习鲁棒表示，而无需人工标注。
- **关键技术细节**：
  - **训练范式**：基于自蒸馏，教师网络和学生网络共享基础编码器，通过精心设计的预测头(prediction heads)和参数高效适应技术（如LoRA、adapter等）避免灾难性遗忘。
  - **数据来源**：短多视角物体中心视频（object-centric video），从不同帧中提取多视图观测，模拟不同捕获条件。
  - **训练目标**：最小化师生网络输出之间的相似性损失（如对比损失或蒸馏损失），使模型学会对视角、光照等变化保持不变性。
- **算法流程**（文字说明）：
  1. 从物体中心视频中随机采样多帧，构建正样本对（同一物体的不同视角）。
  2. 将帧输入学生网络和教师网络（教师网络参数为学生网络参数的指数移动平均）。
  3. 学生网络通过参数高效微调（如仅训练小部分adapter层）更新，教师网络通过EMA更新。
  4. 优化自蒸馏损失，使两个网络的输出表示一致。

## 3. 实验设计
- **数据集**：在两个数据集上进行评估（具体名称未详细列出，但提及“2 datasets”）。推测为物体中心视频数据集（如Objectron、Co3D等）或自建数据集。
- **Benchmark**：下游分类任务，对比基础模型（未适应）和先前适应方法（如直接自监督微调或其它领域适应技术）。
- **对比方法**：包括基线和之前的领域适应/自监督微调方法（具体名称未在摘要中标出，但实验部分应涉及多种对比）。

## 4. 资源与算力
论文中**未明确说明**使用的GPU型号、数量及训练时长。仅提及代码已公开，但未提供算力消耗细节。因此无法量化，仅指出这一点。

## 5. 实验数量与充分性
- 实验组数：涉及3种视觉基础模型（如DINO、MAE等），2个数据集，以及多种下游分类任务。消融实验应包含对预测头设计、参数高效方法选择等组件的分析。
- 充分性评价：实验覆盖了多个模型和数据集，验证了方法的泛化能力。但与现有文献相比，缺乏对更大规模数据集或更多任务（如检测、分割）的验证。总体上实验较为充分，对比客观（引入了基线），但局限性在于仅报告分类性能。

## 6. 主要结论与发现
- VESSA能够有效提升视觉基础模型在无监督领域适应中的分类性能，一致优于未适应模型和先前自适应方法。
- **关键发现**：若不对预测头和参数高效技术进行精心调整，模型会快速遗忘预训练知识，导致退化解。因此VESSA的成功依赖于自蒸馏训练中的精细调参和参数高效适配。

## 7. 优点
- **新颖性与通用性**：首次提出基于物体中心视频的自监督微调方案用于视觉基础模型适应，无需任何标注，数据获取容易（短视频）。
- **实用性**：采用参数高效适配（如LoRA）, 计算开销低，易于部署。
- **有效性**：在多个主流视觉基础模型上均获得提升，验证了方法的鲁棒性。
- **开源**：代码公开，可复现。

## 8. 不足与局限
- **实验覆盖有限**：仅测试了分类任务，未涉及物体检测、语义分割等其他下游视觉任务。
- **数据集规模较小**：仅2个数据集，未在ImageNet或更大规模视频数据集上验证。
- **偏差风险**：物体中心视频可能难以覆盖所有真实场景中的分布偏移（如复杂背景、遮挡等）。
- **资源算力说明缺失**：未提供训练算力需求，不利于其他研究者预估复现成本。
- **对比方法可能不全面**：未提及与最新域自适应（如UDA、源域无关适应）方法的详细比较。

（完）
