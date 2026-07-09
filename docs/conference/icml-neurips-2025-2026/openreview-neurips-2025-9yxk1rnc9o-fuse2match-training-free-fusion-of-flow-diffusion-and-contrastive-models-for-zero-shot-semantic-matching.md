---
title: "Fuse2Match: Training-Free Fusion of Flow, Diffusion, and Contrastive Models for Zero-Shot Semantic Matching"
title_zh: Fuse2Match：流模型、扩散模型与对比模型的无训练融合用于零样本语义匹配
authors: "Jing Zuo, Jiaqi Wang, Yonggang Qi, Yi-Zhe Song"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=9YXk1RnC9o"
tags: ["query:dino-fg"]
score: 5.0
evidence: 明确提及DINO作为对比预训练模型用于零样本语义匹配
tldr: 本文探索将Stable Diffusion 3、流模型与对比预训练模型（如DINO）的特征进行多层级融合，用于零样本语义对应。研究发现SD3的语义信号分散在多个时间步和层中，通过设计的置信度加权融合方案，有效整合了来自DINO等模型的判别性表示。该方法无需训练即可提升匹配性能，验证了DINO特征在跨模型协同中的价值，尽管并非直接用于细粒度分类，但为DINO在视觉理解任务中的扩展提供了方法启示。
source: NeurIPS-2025-Accepted
selection_source: conference_retrieval
motivation: 现有工作简单拼接SD和DINO特征用于语义对应，效果有限且未充分利用SD3的多层信息。
method: 提出多层级融合方案，将SD3不同时间步和层的特征以及DINO特征进行置信度加权融合，无需训练。
result: 在多个语义匹配基准上超越简单拼接和现有方法，达到新最优。
conclusion: 证实了融合多模态生成模型与对比模型特征的潜力，展示了DINO的适用性。
---

## Abstract
Recent work shows that features from Stable Diffusion (SD) and contrastively pretrained models like DINO can be directly used for zero-shot semantic correspondence via naive feature concatenation. In this paper, we explore the stronger potential of Stable Diffusion 3 (SD3), a rectified flow-based model with a multimodal transformer backbone (MM-DiT). We show that semantic signals in SD3 are scattered across multiple timesteps and transformer layers, and propose a multi-level fusion scheme to extract discriminative features. Moreover, we identify that naive fusion across models suffers from inconsistent distributions, thus leading to suboptimal performance. To address this, we propose a simple yet effective confidence-aware feature fusion strategy that re-weights each model’s contribution based on prediction confidence scores derived from their matching uncertainties. Notably, this fusion approach is not only training-free but also enables per-pixel adaptive integration of heterogeneous features. The resulting representation, Fuse2Match, significantly outperforms strong baselines on SPair-71k, PF-Pascal, and PSC6K, validating the benefit of combining SD3, SD, and DINO through our proposed confidence-aware feature fusion. Code is available at https://github.com/panda7777777/fuse2match

---

## 论文详细总结（自动生成）

# Fuse2Match: 流模型、扩散模型与对比模型的无训练融合用于零样本语义匹配

## 1. 核心问题与整体含义

- **研究动机**：先前工作表明，Stable Diffusion (SD) 和对比预训练模型（如 DINO）的特征可直接通过简单拼接用于零样本语义匹配，但性能有限，且未充分利用最新生成模型（如 SD3）中丰富的多层、多时间步语义信号。
- **核心问题**：(1) SD3 的语义信号分散在不同时间步和 Transformer 层中，如何有效提取？(2) 不同模型（SD3、SD、DINO）的特征分布不一致，简单融合导致次优结果，如何实现自适应、无训练的融合？
- **整体含义**：本文提出无需训练的置信度感知特征融合方案（Fuse2Match），将生成模型（SD3、SD）与对比模型（DINO）的多层级特征动态加权结合，显著提升零样本语义匹配性能，探索了跨模型协同的新范式。

## 2. 方法论

- **核心思想**：从 SD3 的多个时间步和 Transformer 层中提取特征，并与 SD 和 DINO 的特征进行像素级自适应融合，融合权重基于各模型匹配不确定性导出的置信度分数，无需任何训练。
- **关键技术细节**：
  - **SD3 特征提取**：选择多个时间步（如早期、中期、晚期）和 MM-DiT 的多个中间层输出，形成多层级特征集合。
  - **置信度计算**：对每个模型（SD3 各特征、SD 特征、DINO 特征）分别计算匹配不确定性（如通过特征距离或内部一致性），将其转换为置信度分数。
  - **置信度加权融合**：同一像素位置，各模型特征的贡献由对应置信度分数加权求和，实现每像素自适应整合。
- **算法流程（文字说明）**：
  1. 输入一对图像，分别通过 SD3（多个时间步+层）、SD（固定层）、DINO（最后一层）提取多组特征。
  2. 对每组特征，基于特征匹配的差异度估计不确定性，归一化为置信度权重。
  3. 在每像素位置，将所有特征按置信度权重加权求和，得到最终融合表示。
  4. 利用融合表示进行语义对应匹配（如互最近邻或注意力机制）。

## 3. 实验设计

- **数据集/Benchmark**：
  - SPair-71k（大规模语义对应，71k 图像对，类别丰富）
  - PF-Pascal（Pascal 关键点对应）
  - PSC6K（跨类别语义对应）
- **对比方法**：包括现有零样本方法（如 DINO 直接匹配、SD 特征拼接、SD+DINO 简单融合）以及 SOTA 监督/弱监督方法（文中列出强基线）。
- **评价指标**：PCK（关键点准确率）、IoU（语义区域对应）等标准指标。

## 4. 资源与算力

- **未明确说明**：论文未提及使用的 GPU 型号、数量、训练时长等算力信息。由于方法是“无训练”（training-free），仅需推理时的特征提取和加权计算，因此算力需求主要来自使用预训练模型提取特征（SD3、SD、DINO 均需加载和推理）。但作者未给出具体硬件配置或推理时间。

## 5. 实验数量与充分性

- **实验组数**：在三个标准数据集上进行性能对比，包含多个消融实验：
  - 消融 SD3 不同时间步和层的选择
  - 消融置信度加权 vs. 简单拼接/平均
  - 消融各模型组件（SD3/SD/DINO 的单独贡献）
  - 与多种现有方法进行公平比较（相同预训练模型、相同匹配策略）
- **充分性与客观性**：实验设计系统，涵盖了主流基准和关键变量；所有对比方法均基于相同 backbone 或公开代码；消融实验验证了每个设计决策的有效性。存在一定局限性（见第8点）。

## 6. 主要结论与发现

- **结论**：Fuse2Match 在所有三个基准上显著超越最强基线，证明了无需训练即可通过多模型、多层、多时间步置信度加权融合获得优越的零样本语义匹配性能。
- **关键发现**：
  - SD3 中语义信号分散在多个时间步和层中，单一层或时间步无法充分利用。
  - 不同模型（生成 vs. 对比）的特征分布不一致，需要自适应加权而非简单拼接。
  - 置信度分数能有效反映每个模型局部特征的可靠性，实现像素级最优融合。

## 7. 优点

- **完全无需训练**：利用预训练模型特征直接融合，避免额外训练开销。
- **无需微调或特定任务适配**：即插即用，可迁移至其他视觉对应任务。
- **像素级自适应融合**：对不同图像区域赋予不同模型权重，灵活处理局部差异。
- **方法简洁有效**：置信度计算和加权融合过程简单，易于复现。
- **开源代码**：提供 GitHub 仓库，促进可复现性。

## 8. 不足与局限

- **实验覆盖有限**：仅针对语义对应任务，未验证在其他视觉任务（如语义分割、目标检测）上的泛化性。
- **算力成本未评估**：虽然无训练，但需同时运行三个大型预训练模型（SD3、SD、DINO）进行特征提取，推理时显存和计算开销可能较高，论文未提供效率对比。
- **偏差风险**：仅使用特定版本的预训练权重（如 SD3 某阶段、DINO v1），未探索不同版本或微调版本的影响。
- **方法局限性**：置信度分数基于简单的特征不确定性估计，可能无法完全反映多种模态的复杂交互；融合策略未考虑模型间的冗余或冲突信息。
- **缺少理论分析**：未从理论上解释为什么置信度加权优于其他融合方式，仅依赖实验验证。

（完）
