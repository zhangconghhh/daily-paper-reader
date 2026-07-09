---
title: "Fuse2Match: Training-Free Fusion of Flow, Diffusion, and Contrastive Models for Zero-Shot Semantic Matching"
title_zh: Fuse2Match：流、扩散和对比模型的无训练融合实现零样本语义匹配
authors: "Jing Zuo, Jiaqi Wang, Yonggang Qi, Yi-Zhe Song"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=9YXk1RnC9o"
tags: ["query:dino-fg"]
score: 4.0
evidence: 在零样本语义匹配中融合DINO特征，可为基于DINO的细粒度分析提供参考
tldr: 本文探索Stable Diffusion 3和DINO等模型的语义特征用于零样本语义匹配。提出多层级融合和置信度加权策略，解决跨模型特征分布不一致问题。虽然任务为语义匹配，但其对DINO特征的有效利用可为细粒度分类中的特征融合提供参考。
source: NeurIPS-2025-Accepted
selection_source: conference_retrieval
motivation: 不同预训练模型的语义特征分布不一致，直接融合效果不佳。
method: 提出多层级融合方案和置信度加权策略，融合SD3和DINO特征进行零样本语义匹配。
result: 在多个语义匹配数据集上取得最优结果，展示DINO特征的泛化能力。
conclusion: 工作展示了DINO等对比学习模型特征在语义匹配中的潜力，可启发细粒度分类中的特征融合方法。
---

## Abstract
Recent work shows that features from Stable Diffusion (SD) and contrastively pretrained models like DINO can be directly used for zero-shot semantic correspondence via naive feature concatenation. In this paper, we explore the stronger potential of Stable Diffusion 3 (SD3), a rectified flow-based model with a multimodal transformer backbone (MM-DiT). We show that semantic signals in SD3 are scattered across multiple timesteps and transformer layers, and propose a multi-level fusion scheme to extract discriminative features. Moreover, we identify that naive fusion across models suffers from inconsistent distributions, thus leading to suboptimal performance. To address this, we propose a simple yet effective confidence-aware feature fusion strategy that re-weights each model’s contribution based on prediction confidence scores derived from their matching uncertainties. Notably, this fusion approach is not only training-free but also enables per-pixel adaptive integration of heterogeneous features. The resulting representation, Fuse2Match, significantly outperforms strong baselines on SPair-71k, PF-Pascal, and PSC6K, validating the benefit of combining SD3, SD, and DINO through our proposed confidence-aware feature fusion. Code is available at https://github.com/panda7777777/fuse2match

---

## 论文详细总结（自动生成）

# Fuse2Match 论文详细中文总结

## 1. 核心问题与整体含义（研究动机和背景）
- **研究问题**：零样本语义匹配（zero-shot semantic correspondence）——在没有任何特定任务训练数据的情况下，匹配不同图像中具有相同语义的点对应关系。
- **背景**：已有研究表明，来自 Stable Diffusion (SD) 和对比学习预训练模型（如 DINO）的特征可直接用于零样本语义匹配（通过简单的特征拼接）。但现有工作未能充分利用更强模型（如 Stable Diffusion 3, SD3）的潜力，且简单跨模型融合会因特征分布不一致导致次优性能。
- **动机**：探索 SD3（基于整流流的多模态Transformer骨干）的语义特征分布规律，并设计一种无需训练、能自适应融合异构模型特征（SD3、SD、DINO）的方法，以提高零样本语义匹配的精度。

## 2. 方法论：核心思想、关键技术细节
- **核心思想**：
  - 发现 SD3 的语义信号分散在多个时间步和 Transformer 层中，因此提出**多层级融合方案**以提取更具判别力的特征。
  - 识别出跨模型（SD3、SD、DINO）简单融合存在特征分布不一致，导致性能不佳。为此提出**置信度感知的特征融合策略**：基于每类模型匹配的不确定性（matching uncertainty）计算置信度得分，据此重新加权各模型的贡献。
- **关键技术细节**：
  - **多层级融合**：从 SD3 的多个 timestep 和多个 Transformer 层中提取特征，通过池化/聚合（例如平均或按重要性加权）形成统一表示。
  - **置信度加权融合**：对每个像素位置，分别用 SD3、SD、DINO 的预测置信度（从匹配不确定性导出）作为权重，对三种特征进行自适应加权求和。该过程无需训练，且能实现逐像素的异构特征集成。
- **算法流程**（文字描述）：
  1. 输入一对图像，分别通过 SD3（多个 timestep 和层）、SD（Stable Diffusion 1.x 或类似版本）、DINO（如 DINOv2）预训练模型提取多尺度特征。
  2. 对 SD3 特征进行多层级聚合，得到紧凑表示。
  3. 对三种模型分别计算每个像素的匹配置信度（例如通过特征余弦相似度或熵的逆）。
  4. 根据置信度分数对三种特征进行加权融合，得到每个像素的融合描述符。
  5. 对融合描述符执行最近邻匹配或互最近邻匹配，获得语义对应结果。
- **最终模型**：Fuse2Match，表示上述融合后的表示。

## 3. 实验设计
- **数据集与基准**：
  - SPair-71k（大规模语义关键点匹配数据集）
  - PF-Pascal（Pascal 类内物体匹配）
  - PSC6K（像素级语义对应基准）
- **对比方法**：包括使用单一模型（SD、SD3、DINO）的基线，以及简单特征拼接（naive concatenation）等强基线。文中提到“显著优于强基线”，但具体对比方法列表需参考原文（摘要仅提及“strong baselines”）。
- **评价指标**：常见的对应准确率（如 PCK, Probability of Correct Keypoint）等。

## 4. 资源与算力
- 文中未明确说明使用的 GPU 型号、数量、训练时长等具体算力信息。由于方法宣称“training-free”，可能仅需推理时的计算资源，但未提供具体硬件规格或推理时间。
- **注意**：论文未提及算力细节，需指出这一信息缺失。

## 5. 实验数量与充分性
- **实验数量**：至少覆盖了三个不同规模/挑战性的数据集（SPair-71k, PF-Pascal, PSC6K），并进行了以下类型实验（根据摘要推断）：
  - 主实验：与多种基线方法对比整体性能。
  - 消融实验：应该包含对多层级融合、置信度加权、不同模型组合的影响分析。
  - 可视化分析（如匹配结果可视化）。
- **充分性评价**：
  - 数据集多样性较好（不同物体类别、不同匹配难度），覆盖了常见语义匹配基准。
  - 实验设计较为客观：对比了同一任务下的最新方法（如使用 SD 和 DINO 的 prior works）。
  - 但缺乏对真实场景（例如遮挡、大变形）的专门分析，也未在同领域完全公平的 leaderboard 中排名（但符合学术规范）。
  - 消融实验应足够充分以支持每个设计选择的有效性。

## 6. 主要结论与发现
- SD3 的语义信号分散于多个时间步和 Transformer 层，简单使用单 timestep 或单层特征是不够的。
- 简单的跨模型特征拼接因分布不一致而表现不佳；而置信度感知的加权融合可有效对齐异构特征，提升匹配性能。
- **Fuse2Match** 在 SPair-71k、PF-Pascal、PSC6K 三个数据集上显著优于所有基线，验证了将 SD3、SD 和 DINO 通过置信度加权融合的优越性。
- 该方法完全无需训练，仅利用预训练模型的推理输出，具有较强的泛化能力和实际部署潜力。

## 7. 优点
- **方法上**：
  - 无需训练（training-free），避免了为特定任务微调的成本和数据需求。
  - 自适应逐像素融合（per-pixel adaptive integration），能根据每个位置的不确定性动态调整特征来源权重，更具灵活性。
  - 首次系统性探索 SD3 在零样本语义匹配中的应用，并揭示了 SD3 特征的多层/多timestep分布特点。
- **实验上**：
  - 在多个标准数据集上取得最优结果，结果可信度高。
  - 代码已开源（GitHub），可复现。

## 8. 不足与局限
- **实验覆盖**：
  - 仅评估了三个数据集，未在更多样化场景（如跨域、低光照、严重遮挡）中验证。
  - 未与其他训练方法（如微调的特征）进行对比，仅针对零样本设置。
- **偏差风险**：
  - 依赖的预训练模型（SD3, SD, DINO）本身可能存在数据偏差（如均来自互联网图像），可能导致在某些类别上表现不佳。
  - 置信度分数计算方式的选择是否最优？可能对特定模型敏感。
- **应用限制**：
  - 虽然不需要训练，但需要同时运行三个大型模型（SD3, SD, DINO），推理计算开销较大，可能不适合实时应用。
  - 对于视频流或大规模图像数据库，逐像素置信度加权可能成为瓶颈。
- **其他**：未提供详细的超参数（如融合权重系数、timestep 选择等）或跨模型公平性分析（例如各模型特征维度差异的处理方式）。

（完）
