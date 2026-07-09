---
title: Unlabeled Data Improves Fine-Grained Image Zero-shot Classification with Multimodal LLMs
title_zh: 无标签数据提升多模态大模型细粒度图像零样本分类
authors: "Yunqi Hong, Sohyun An, Andrew Bai, Neil Lin, Cho-Jui Hsieh"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=VNTj7PGlrz"
tags: ["query:dino-fg"]
score: 7.0
evidence: 利用无标签数据和自监督提示学习提升多模态大模型细粒度分类性能
tldr: 针对多模态大模型在细粒度图像分类上易忽略细微视觉差异的问题，提出AutoSEP框架，无需监督信号，通过迭代自监督提示学习，让MLLM聚焦于关键判别特征。实验显示该方法显著提升细粒度分类准确率，为零样本细粒度分类提供新范式。
source: NeurIPS-2025-Accepted
selection_source: conference_retrieval
motivation: 多模态大模型在细粒度分类中易忽略细微视觉差异。
method: 提出AutoSEP，利用无标签数据自监督学习图像描述提示，引导MLLM关注判别特征。
result: 在细粒度分类任务上准确率显著提升。
conclusion: 自监督提示学习可有效增强多模态大模型细粒度分类能力。
---

## Abstract
Despite Multimodal Large Language Models (MLLMs) showing promising results on general zero-shot image classification tasks, fine-grained image classification remains challenging. It demands precise attention to subtle visual details to distinguish between visually similar subcategories—details that MLLMs may easily overlook without explicit guidance. To address this, we introduce AutoSEP, an iterative self-supervised prompt learning framework designed to enhance MLLM fine-grained classification capabilities in a fully unsupervised manner. Our core idea is to leverage unlabeled data to learn a description prompt that guides MLLMs in identifying crucial discriminative features within an image, and boost classification accuracy. We developed an automatic self-enhancing prompt learning framework called AutoSEP to iteratively improve the description prompt using unlabeled data, based on instance-level classification scoring function. AutoSEP only requires black-box access to MLLMs, eliminating the need for any training or fine-tuning. We evaluate our approach on multiple fine-grained classification datasets. It consistently outperforms other unsupervised baselines, demonstrating the effectiveness of our self-supervised optimization framework. Notably, AutoSEP in average improves 13\% over standard zero-shot classification and 3\% over the best-performing baselines.

---

## 论文详细总结（自动生成）

# 详细中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **研究动机**：多模态大语言模型（MLLMs）在通用零样本图像分类中表现良好，但在**细粒度图像分类**任务中依然面临挑战。细粒度分类需要模型精准关注图像中微小的视觉差异（如不同鸟类、车型之间的细微特征），而MLLMs在没有显式指导的情况下容易忽略这些细节，导致分类准确率低下。
- **核心问题**：如何在**完全无监督**（不依赖人工标注）的条件下，引导MLLMs关注图像中关键的判别性特征，提升细粒度零样本分类性能。
- **背景意义**：现有方法往往依赖额外标注数据或模型微调，成本高且泛化性受限。利用大量易获取的**无标签数据**来增强模型对细微差异的感知能力，具有重要的实用价值和理论意义。

## 2. 论文提出的方法论

- **核心思想**：提出 **AutoSEP**（Automatic Self-Enhancing Prompt learning）框架，通过迭代自监督学习从无标签数据中自动生成描述提示（description prompt），引导MLLMs聚焦于图像中关键的判别性特征，从而提升零样本分类准确率。
- **关键技术细节**：
  - 采用**实例级分类评分函数**（instance-level classification scoring function）评估当前提示对无标签样本的分类效果。
  - 利用该评分函数，**自动迭代优化**描述提示：每次迭代基于当前提示对无标签数据的分类结果，更新提示文本，使其更关注判别性细节。
  - 框架仅需对MLLMs进行**黑盒访问**（black-box access），完全无需微调或训练MLLMs内部参数。
- **算法流程（文字说明）**：
  1. 初始化一个通用描述提示（如“请描述该图像的区分性特征”）；
  2. 输入一批无标签图像，使用当前提示让MLLM生成自然语言描述；
  3. 将生成的描述与候选类别标签进行匹配，计算实例级分类得分；
  4. 基于得分反馈，通过某种优化策略（如梯度近似或搜索）更新提示文本；
  5. 重复步骤2-4，直至提示收敛或达到迭代上限；
  6. 用最终优化的提示对测试图像进行零样本分类。

## 3. 实验设计

- **数据集/场景**：在**多个细粒度图像分类数据集**上进行评估（具体数据集名称未在摘要中列出，根据元数据推测可能包含CUB-200-2011、Stanford Cars、Oxford Flowers等常见细粒度基准）。
- **Benchmark**：对比标准零样本分类（Standard zero-shot classification）以及其他**无监督基线方法**（如固定提示、随机提示等）。
- **对比方法**：未在摘要中详列，但提及“consistently outperforms other unsupervised baselines”。

## 4. 资源与算力

- **文中未明确说明**使用了多少GPU型号、数量或训练时长。仅提及方法“无需训练或微调核心模型”，因此**计算开销理论上较低**，主要集中在提示迭代优化过程中对MLLMs的API调用或前向推理。

## 5. 实验数量与充分性

- **实验数量**：在“多个”细粒度数据集上评估，并对比了多项基线。摘要未提供具体实验组数，但暗示包含**主要性能对比实验**以及可能的**消融实验**（通过“iterative self-supervised”等描述推测）。
- **充分性判断**：由于缺乏实验数据集列表及消融细节，**尚无法完全评估实验覆盖的全面性**。但对比了标准零样本和最佳无监督基线，效果提升显著（平均13%，相对最优基线3%），结果有一定说服力。未提及跨领域泛化性测试（如从鸟类到车辆），存在一定局限。

## 6. 论文的主要结论与发现

- AutoSEP框架**显著提升**了MLLMs在细粒度零样本分类中的准确率：平均比标准零样本分类**提升13%**，比最佳无监督基线**提升3%**。
- 证明**利用无标签数据通过自监督提示学习**可以有效引导MLLMs关注细微视觉差异，无需任何人工标注。
- 验证了**迭代自优化**在无监督细粒度分类场景中的有效性。

## 7. 优点

- **完全无监督**：不依赖任何标注数据，利用广泛存在的无标签图像，实用性强。
- **黑盒可迁移**：仅需访问MLLMs的输入输出接口，无需模型权重或训练硬件，适配多种商业或开源MLLM。
- **无需微调**：避免昂贵的模型微调，降低了计算资源需求。
- **自动迭代优化**：通过评分函数自动改进提示，避免了手工设计提示的麻烦和不确定性。

## 8. 不足与局限

- **潜在的数据偏差**：无标签数据的质量与分布可能会影响提示优化的效果（例如数据集中细粒度特征不明显时）。
- **缺乏细粒度类间相似度控制**：如果类别间视觉差异极小，仅靠无标签自监督可能仍难以区分；文中未讨论极端难例场景。
- **实验覆盖有限**：未报告在更多样化的MLLM架构（如LLaVA、Gemini等）上的结果，仅在摘要中提到“多个数据集”，具体数据集及消融实验细节不透明。
- **未提及白盒场景下的对比**：若可访问模型内部，可能有更优方案，但本文未讨论。
- **可复现性隐患**：提示优化算法细节（如评分函数具体形式、优化策略）未在摘要中完全公开，需要阅读全文确认。

（完）
