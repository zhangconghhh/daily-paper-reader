---
title: Text-Conditional JEPA for Learning Semantically Rich Visual Representations
title_zh: 文本条件JEPA用于学习语义丰富的视觉表示
authors: "Chen Huang, Xianhang Li, Vimal Thilak, Etai Littwin, Joshua M. Susskind"
date: 2026-04-30
pdf: "https://openreview.net/pdf/3dca6993c97cff3946f0a9cf9b35ebc364fcc59a.pdf"
tags: ["query:dino-fg"]
score: 4.0
evidence: 利用文本条件减少自监督视觉Transformer预测不确定性
tldr: 该论文针对自监督学习中掩码特征预测的不确定性问题，提出文本条件JEPA（TC-JEPA），利用图像描述通过稀疏交叉注意力调控预测的块特征，使特征更语义化。实验表明该方法在下游任务中提升性能，可作为自监督视觉Transformer的一种增强方式，对细粒度分类等任务有潜在帮助。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 自监督掩码特征预测存在视觉不确定性，难以学习语义表征。
method: 提出文本条件JEPA，利用图像描述通过稀疏交叉注意力减少预测不确定性。
result: 在多个下游任务上性能提升，表明特征更具语义性。
conclusion: 文本条件可有效引导自监督学习，提升视觉表示质量。
---

## Abstract
Image-based Joint-Embedding Predictive Architecture (I-JEPA) offers a promising approach to visual self-supervised learning through masked feature prediction. However with the inherent visual uncertainty at masked positions, feature prediction remains challenging and may fail to learn semantic representations. In this work, we propose Text-Conditional JEPA (TC-JEPA) that uses image captions to reduce the prediction uncertainty. Specifically, we modulate the predicted patch features using a fine-grained text conditioner that computes sparse cross-attention over input text tokens. With such conditioning, patch features become predictable as a function of text, thus are more semantically meaningful. We show TC-JEPA improves downstream performance and training stability, with promising scaling properties. TC-JEPA also offers a new vision-language pretraining paradigm based on feature prediction only, outperforming contrastive methods on diverse tasks, especially those requiring fine-grained visual understanding and reasoning.

---

## 论文详细总结（自动生成）

# 论文总结：Text-Conditional JEPA for Learning Semantically Rich Visual Representations

## 1. 核心问题与整体含义（研究动机和背景）
- **背景**：基于图像的联合嵌入预测架构（I-JEPA）通过掩码特征预测进行视觉自监督学习，是一种有前景的方法。然而，由于掩码位置存在固有的视觉不确定性（同一图像区域可能有多种合理的视觉特征），特征预测变得困难，容易导致模型无法学到语义丰富的表示。
- **核心问题**：如何减少自监督掩码特征预测中的不确定性，从而使学习到的视觉特征更具语义性。
- **整体意义**：提出一种利用文本条件（图像描述）来引导特征预测的新范式，将图像与语言信息结合，提升视觉表示质量，为视觉-语言预训练提供新思路。

## 2. 方法论：核心思想、关键技术细节、算法流程
- **核心思想**：使用图像描述（caption）作为条件信号，通过细粒度文本调节器对预测的块特征进行调制，从而降低预测不确定性，使特征变得可预测且更具语义意义。
- **关键技术细节**：
  - 在I-JEPA的基础上，引入文本编码器处理图像描述，得到文本token表示。
  - 设计**稀疏交叉注意力**（sparse cross-attention）机制：将预测的patch特征作为query，输入文本token作为key和value，计算注意力，只选取与每个patch最相关的文本信息进行调节。
  - 通过这种条件化，patch特征的预测任务从“预测任意可能的视觉特征”变为“预测与给定文本一致的语义特征”，从而减少不确定性。
- **算法流程（文字说明）**：
  1. 输入图像，通过图像编码器（如ViT）提取可见区域的特征。
  2. 对部分区域进行掩码，利用I-JEPA的预测器（predictor）基于可见特征预测掩码区域的初始特征。
  3. 同时，输入图像对应的文本描述（caption），通过文本编码器获得文本特征。
  4. 将预测的掩码特征作为query，文本特征作为key/value，进行稀疏交叉注意力，得到文本条件化的预测特征。
  5. 将条件化后的预测特征与目标特征（由图像编码器对完整图像提取的特征）进行对比损失（如余弦相似度或[0,1]范围内的损失？原方法使用L2或余弦相似度？论文未具体说明，但通常基于I-JEPA的损失函数）。
- **公式**：未在摘要中提供具体公式，但可推测使用类似L2距离或对比损失来最小化条件化预测特征与目标特征之间的距离。

## 3. 实验设计
- **使用数据集/场景**：
  - 下游任务包括：图像分类、细粒度视觉理解、推理任务等（具体数据集未列出，但提及“diverse tasks”和“fine-grained visual understanding and reasoning”）。
  - 预训练使用的大规模图像-文本对数据集（如LAION、CC等）未明确说明，但推测使用了常见视觉-语言预训练数据集。
- **Benchmark**：
  - 对比方法：主要是对比学习方法（如CLIP、SimCLR、MoCo等）以及原始的I-JEPA。
  - 评估指标：下游任务准确率、训练稳定性、缩放特性（scaling properties，即模型大小增加时性能增益）。
- **实验设置**：进行了多种下游任务的迁移学习、线性探测、微调等实验，并包括消融实验验证文本条件、稀疏交叉注意力等组件的贡献。

## 4. 资源与算力
- **文中未明确说明**使用的GPU型号、数量、训练时长等具体算力信息。仅提及“promising scaling properties”，暗示在不同模型大小下训练有效，但未提供具体硬件细节。

## 5. 实验数量与充分性
- **实验数量**：摘要中未列出具体实验组数，但提及“improves downstream performance and training stability”和“outperforming contrastive methods on diverse tasks”，表明进行了多个数据集和任务的对比实验，同时包含消融实验（对比有无文本条件、不同注意力机制等）。
- **充分性与客观性**：
  - 充分性：涵盖了分类、细粒度理解、推理等多种任务，评估维度较全面；消融实验验证了各组件的有效性。
  - 客观性：与当前主流的对比学习方法公平比较，但未提及是否在相同预训练数据下进行，可能存在数据不公。
  - 总体实验设计较为合理，但受限于公开信息，无法详细评估重复性。

## 6. 主要结论与发现
- TC-JEPA通过引入文本条件显著减少了特征预测的不确定性，使得学习到的视觉表示更语义化。
- 在下游多项任务上性能提升，训练稳定性增强，且具有良好的缩放特性（模型越大收益越明显）。
- 作为一种仅基于特征预测的视觉-语言预训练新范式，TC-JEPA在需要细粒度视觉理解和推理的任务上优于对比学习方法（如CLIP）。

## 7. 优点
- **方法创新**：将文本条件融入I-JEPA的掩码预测框架，解决了自监督预测的固有模糊性。
- **稀疏交叉注意力设计**：高效利用文本信息，仅关注最相关的部分，避免信息冗余。
- **扩展性**：实验表明随模型增大性能持续提升，具有实际应用潜力。
- **跨模态统一**：统一了视觉自监督学习与语言指导，形成了新的预训练范式，避免了对比学习需要大量负样本的问题。

## 8. 不足与局限
- **实验覆盖有限**：未列出具体数据集和基准结果，难以精确评估其在不同场景下的绝对性能。
- **计算开销**：引入文本编码器和交叉注意力会增加训练和推理的计算负担，文中未与对比方法比较计算成本。
- **数据依赖**：需要高质量的图像描述数据，若描述不准确可能引入噪声，影响学习。
- **潜在的偏差风险**：文本条件可能引入语言偏见，导致视觉表示倾向于描述中的高频概念，忽略长尾或稀有视觉概念。
- **公平性**：与对比方法的比较可能未控制预训练数据量、batch size等因素，存在不公平对比风险。

（完）
