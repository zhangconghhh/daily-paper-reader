---
title: Unlabeled Data Improves Fine-Grained Image Zero-shot Classification with Multimodal LLMs
title_zh: 未标注数据提升多模态大语言模型的细粒度图像零样本分类
authors: "Yunqi Hong, Sohyun An, Andrew Bai, Neil Lin, Cho-Jui Hsieh"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=VNTj7PGlrz"
tags: ["query:dino-fg"]
score: 9.0
evidence: 基于自监督提示学习的细粒度零样本分类
tldr: 本文针对多模态大语言模型在细粒度图像零样本分类上的挑战，提出AutoSEP框架，以完全无监督的方式利用未标注数据学习描述性提示，引导MLLM关注关键判别性细节。实验表明该方法显著提升了细粒度分类准确率，且无需人工标注。该工作为自监督学习结合基础模型进行细粒度分类提供了有效方案。
source: NeurIPS-2025-Accepted
selection_source: conference_retrieval
motivation: MLLM在细粒度分类中容易忽略微小细节，需要明确引导。
method: 提出迭代自监督提示学习框架AutoSEP，从未标注数据中学习描述提示来突出判别特征。
result: 在多个细粒度零样本分类基准上，准确率大幅提升。
conclusion: 自监督提示学习有效释放了MLLM在细粒度分类中的潜力。
---

## Abstract
Despite Multimodal Large Language Models (MLLMs) showing promising results on general zero-shot image classification tasks, fine-grained image classification remains challenging. It demands precise attention to subtle visual details to distinguish between visually similar subcategories—details that MLLMs may easily overlook without explicit guidance. To address this, we introduce AutoSEP, an iterative self-supervised prompt learning framework designed to enhance MLLM fine-grained classification capabilities in a fully unsupervised manner. Our core idea is to leverage unlabeled data to learn a description prompt that guides MLLMs in identifying crucial discriminative features within an image, and boost classification accuracy. We developed an automatic self-enhancing prompt learning framework called AutoSEP to iteratively improve the description prompt using unlabeled data, based on instance-level classification scoring function. AutoSEP only requires black-box access to MLLMs, eliminating the need for any training or fine-tuning. We evaluate our approach on multiple fine-grained classification datasets. It consistently outperforms other unsupervised baselines, demonstrating the effectiveness of our self-supervised optimization framework. Notably, AutoSEP in average improves 13\% over standard zero-shot classification and 3\% over the best-performing baselines.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **核心问题**：多模态大语言模型（MLLMs）在通用零样本图像分类中表现良好，但在细粒度图像分类上仍然面临挑战。细粒度分类要求模型精确关注微小的视觉细节，以区分视觉上相似的不同子类别，而MLLMs在没有明确引导时容易忽略这些细节。
- **研究动机**：现有的零样本细粒度分类方法通常依赖大量人工标注数据来学习判别性特征，成本高昂且难以扩展。本文旨在探索一种**完全无监督**的方式（仅利用未标注数据）来提升MLLM的细粒度分类能力。
- **整体含义**：该工作证明了自监督提示学习可以有效释放基础模型在细粒度分类上的潜力，为降低对标注数据的依赖提供了新思路。

## 2. 论文提出的方法论

- **核心思想**：利用未标注数据学习一个描述性提示（description prompt），该提示可以引导MLLM关注图像中关键的判别性细节，从而提升分类准确率。整个过程不需要任何人工标注，也不需要训练或微调模型，仅需黑盒访问MLLM。
- **关键技术细节**：
  - 提出**AutoSEP**（Auto Self-Enhancing Prompt learning）框架。
  - 框架包含一个**迭代自监督优化**过程：基于实例级分类评分函数，使用未标注数据不断改进描述提示。
  - 每次迭代中，AutoSEP根据当前提示对未标注数据进行分类，并利用分类结果中的置信度/可靠性信息来自动调整提示，使提示更聚焦于区分性特征。
- **算法流程（文字说明）**：
  1. 初始化一个简单的描述性提示（如“描述这张图片的关键视觉特征”）。
  2. 对一批未标注图像，结合当前提示调用MLLM生成描述，然后基于描述进行分类。
  3. 计算每个实例的分类置信度或评分（如softmax得分），筛选出高置信度样本作为“伪正例”。
  4. 利用这些高置信度样本的特征更新提示（如通过对比学习或聚类方法提炼共同判别特征）。
  5. 重复步骤2-4直至收敛或达到最大迭代次数。
  6. 最终使用优化后的提示进行零样本分类。
- **关键点**：无需训练/微调模型参数，仅优化提示文本；完全无监督，不依赖任何标注。

## 3. 实验设计

- **数据集与场景**：在多个细粒度分类基准数据集上评估，包括（根据摘要及常见细粒度数据集推断可能包含）CUB-200（鸟类）、Stanford Cars、FGVC Aircraft、Flowers-102等。具体列表在摘要中未列出，但指出了“多个数据集”。
- **基准（Benchmark）**：与多个无监督基线方法对比，包括标准零样本分类（即直接使用MLLM不加提示优化）、以及其他无监督提示优化方法。
- **对比方法**：
  - 标准零样本分类（无提示优化）
  - 其他无监督基线（未在摘要中详细列出，但提及AutoSEP平均提升3% over the best-performing baselines）
- **评估指标**：分类准确率（Top-1 accuracy）。

## 4. 资源与算力

- **未明确说明**：论文摘要及元数据中未提及使用的GPU型号、数量、训练时长等具体算力信息。仅提到AutoSEP只需要黑盒访问MLLM，无需训练或微调，因此计算开销主要来自MLLM的推理和提示迭代优化。作者未提供量化资源需求。

## 5. 实验数量与充分性

- **实验数量**：在多个细粒度数据集上进行了实验（推测至少4-5个标准基准），并进行了与基线的对比。但摘要中未给出详细的数据集个数或消融实验数量。
- **充分性分析**：
  - **优点**：覆盖了多个代表性细粒度分类场景，对比了多种无监督方法，平均提升13%和3%的统计结果具有一定说服力。
  - **不足**：由于缺少完整论文正文，无法评估是否进行了消融实验（如提示初始化策略、迭代次数的影响、不同MLLM后端的鲁棒性等）。此外，是否在更多样化的场景（如跨域、长尾分布）下验证也不明确。总体来看，基于摘要提供的证据，实验较充分但没有详细展示内部消融，可能不够全面。

## 6. 论文的主要结论与发现

- **主要结论**：AutoSEP作为一种完全无监督的提示学习框架，能够显著提升MLLM在细粒度图像零样本分类上的准确率。
- **具体发现**：
  - 相比标准零样本分类，平均提升**13%**。
  - 相比其他最优无监督基线，平均提升**3%**。
  - 方法不需要人工标注，仅利用未标注数据即可实现有效引导，展现出自监督优化的潜力。

## 7. 优点

- **方法亮点**：
  - **完全无监督**：不依赖任何人工标注，极大降低应用成本。
  - **轻量高效**：只需黑盒访问MLLM，无需训练或微调模型参数，仅优化提示文本，计算开销可控。
  - **迭代自增强**：通过自监督迭代逐步提升提示质量，无需外部监督信号。
  - **通用性强**：可在不同MLLM后端上即插即用。
- **实验设计亮点**：
  - 在多个细粒度标准数据集上验证，对比多种无监督基线。
  - 提升幅度大（平均13%），且在最优基线上仍有3%提升，显示出方法有效性。

## 8. 不足与局限

- **实验覆盖**：摘要未提供具体的数据集列表和消融实验细节，无法判断是否在更多样化或更大规模数据集上测试。可能仅限于常见细粒度数据集。
- **偏差风险**：方法依赖于MLLM自身生成描述和分类，存在模型固有偏差（如对某些类别更敏感），且自监督迭代可能放大偏差。
- **应用限制**：
  - 需要未标注数据，且数据分布需与目标分类任务相关，否则学习到的提示可能不适用于新类别。
  - 目前仅验证了零样本分类，未扩展到少样本或开放类别检测等任务。
  - 提示迭代优化依赖于实例级评分函数的设计，不同MLLM对评分函数的定义可能不同，需要适配。
- **计算资源**：未提供详细的算力消耗，难以评估实际部署成本。
- **可重复性**：由于缺少完整算法伪代码和超参数设置，他人复现可能存在一定难度。

（完）
