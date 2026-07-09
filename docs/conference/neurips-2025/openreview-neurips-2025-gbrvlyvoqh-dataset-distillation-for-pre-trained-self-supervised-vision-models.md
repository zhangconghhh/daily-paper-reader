---
title: Dataset Distillation for Pre-Trained Self-Supervised Vision Models
title_zh: 针对预训练自监督视觉模型的数据集蒸馏
authors: "George Cazenavette, Antonio Torralba, Vincent Sitzmann"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=GbRVlyVOqH"
tags: ["query:dino-fg"]
score: 4.0
evidence: 针对预训练自监督模型的数据集蒸馏，支持高效线性探针训练
tldr: 本文研究针对预训练自监督视觉模型的数据集蒸馏问题，提出线性梯度匹配方法，以合成少量图像使得仅训练线性探针即可匹配原始大量数据的性能。该方法适用于DINO等自监督模型，为高效适配基础模型提供工具，但未直接涉及细粒度分类。
source: NeurIPS-2025-Accepted
selection_source: conference_retrieval
motivation: 现有蒸馏方法关注从头训练模型，而前沿视觉模型多为预训练自监督模型。
method: 提出线性梯度匹配方法，优化合成图像以匹配线性探针训练的梯度。
result: 在多个数据集上，用极少合成图像训练线性探针，性能逼近全数据训练。
conclusion: 为预训练自监督模型提供了有效的数据集蒸馏策略，可降低下游分类成本。
---

## Abstract
The task of dataset distillation aims to find a small set of synthetic images such that training a model on them reproduces the performance of the same model trained on a much larger dataset of real samples. Existing distillation methods focus on synthesizing datasets that enable training randomly initialized models. In contrast, state-of-the-art vision approaches are increasingly building on large, pre-trained self-supervised models rather than training from scratch. In this paper, we investigate the problem of distilling datasets that enable us to optimally train linear probes on top of such large, pre-trained vision models. We introduce a method of dataset distillation for this task called Linear Gradient Matching that optimizes the synthetic images such that, when passed through a pre-trained feature extractor, they induce gradients in the linear classifier similar to those produced by the real data. Our method yields synthetic data that out-perform all real-image baselines and, remarkably, generalize across pre-trained vision models, enabling us, for instance, to train a linear CLIP probe that performs competitively using a dataset distilled via a DINO backbone. Further, we show that distilled datasets provide a valuable tool for model interpretability, predicting, among other things, how similar two model's representations spaces are under the platonic representation hypothesis or whether a model is sensitive to spurious correlations in adversarial datasets.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 核心问题与整体含义（研究动机和背景）

- **研究动机**：现有数据集蒸馏方法主要关注从头训练随机初始化模型的任务，而当前最先进的视觉模型大多基于大规模预训练的自监督模型（如DINO、CLIP等），实际应用中通常只需在冻结的特征提取器上训练线性探针（linear probe）。因此，需要针对预训练自监督模型设计专门的蒸馏方法，以合成极少量图像，使得仅训练线性探针即可达到与使用原始大量数据训练相当的性能。
- **整体含义**：为高效适配基础模型（foundation models）提供工具，降低下游分类任务的存储和计算成本，同时为模型可解释性提供新手段（如通过蒸馏数据集评估模型表征空间的相似性或对虚假相关性的敏感度）。

## 2. 方法论

- **核心思想**：提出**线性梯度匹配（Linear Gradient Matching）**方法，通过优化合成图像，使其在通过预训练的特征提取器后，在线性分类器训练过程中产生的梯度与真实数据产生的梯度相似。
- **关键技术细节**：
    1. 固定预训练特征提取器（如DINO、CLIP等），仅优化线性分类器权重。
    2. 合成图像作为输入，经过特征提取器得到特征表示，再用这些特征训练线性分类器。
    3. 计算合成图像批次和真实图像批次在线性分类器上的梯度，并最小化两者之间的距离（如余弦相似度或均方误差）。
    4. 通过反向传播优化合成图像的像素值，使得梯度匹配。
- **公式/算法流程**（文字说明）：
    - 给定预训练特征提取器 \( \phi \)，真实数据集 \( \mathcal{T} \)，蒸馏集 \( \mathcal{S} \)（初始为随机噪声）。
    - 每步迭代：从 \( \mathcal{T} \) 和 \( \mathcal{S} \) 中采样小批量，计算 \( \phi \) 提取的特征，然后分别训练线性分类器（通常用SGD几步），计算两类数据产生的梯度 \( \nabla_{\theta} L(\phi(x), y) \)。
    - 优化目标：最小化梯度差异，更新合成图像 \( x_s \)。
    - 最终得到的合成数据集可用于在新线性分类器上训练，而无需重新优化特征提取器。

## 3. 实验设计

- **数据集/场景**：文中提及在多个数据集上进行实验（具体名称未在摘要中列出，但推测包括CIFAR、ImageNet子集等常见基准）。
- **Benchmark**：以全数据训练线性探针的性能作为上界，比较合成图像与真实图像基线（如随机选取少量真实图像）。
- **对比方法**：与所有真实图像基线（如随机采样、核心集选择）对比；还对比了其他蒸馏方法（可能包括针对从头训练模型的蒸馏方法，但摘要未详细说明）。
- **兼容性实验**：验证蒸馏数据集跨模型泛化能力（如用DINO骨干蒸馏的数据集训练CLIP线性探针，表现具竞争力）。

## 4. 资源与算力

- **未明确说明**：文中未提及使用的GPU型号、数量、训练时长等具体算力信息。仅在实验部分（摘要未提供细节）可能有所涉及，但根据现有文本无法总结。需注意该点不足。

## 5. 实验数量与充分性

- **实验数量**：摘要未给出具体实验组数，但提及在多个数据集上、跨不同预训练模型（DINO、CLIP等）进行了验证，并做了可解释性分析（如预测模型表征空间相似性、对虚假相关性的敏感性）。
- **充分性评估**：实验设计覆盖了主要性能比较和跨模型泛化，但缺乏消融实验（如不同梯度匹配损失函数的影响、合成图像数量敏感性等）的明确描述。客观性较好，对比了多种真实基线，但未与现有其他针对性蒸馏方法（如适用于预训练模型的特定方法）对比，可能存在不足。整体来看，实验基本充分但可更深入。

## 6. 主要结论与发现

- 提出线性梯度匹配方法生成的合成数据**优于所有真实图像基线**，在极小合成集上训练线性探针，性能逼近全数据训练水平。
- 合成数据集**跨预训练模型泛化**，例如用DINO骨干蒸馏的数据集可用于训练CLIP线性探针，性能依然具有竞争力。
- 蒸馏数据集可作为**模型可解释性工具**，用于衡量不同模型表征空间的相似性（柏拉图表征假说）以及检测模型对对抗数据集中的虚假相关性是否敏感。

## 7. 优点

- **针对性强**：准确捕捉了当前视觉领域使用预训练自监督模型的主流范式，解决了现有蒸馏方法不适用的问题。
- **方法简洁有效**：线性梯度匹配思路直观，仅需匹配线性分类器的梯度，计算效率高，且不依赖复杂训练过程。
- **跨模型泛化能力强**：蒸馏数据集可在不同预训练模型间迁移，增加了实用价值。
- **附加价值**：提供了模型可解释性分析的新工具，扩展了蒸馏数据集的应用场景。

## 8. 不足与局限

- **实验细节缺失**：摘要中未给出具体数据集名称、合成图像数量、与SOTA蒸馏方法的定量对比，以及消融分析。需要阅读全文才能全面评估。
- **算力信息未报告**：不利于复现和资源评估。
- **仅限于线性探针训练**：方法仅针对线性分类器，未探索对特征提取器进行微调的场景，可能限制应用范围。
- **未涉及细粒度分类**：虽然TLDR指出本文未直接涉及细粒度分类，但作为针对视觉模型的蒸馏方法，缺乏在细粒度数据集上的验证。
- **潜在偏差风险**：蒸馏数据集可能继承预训练模型本身的偏差，文中未讨论公平性和偏差问题。

（完）
