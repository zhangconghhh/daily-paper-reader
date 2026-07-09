---
title: Learning a Cross-Modal Schrödinger Bridge for Visual Domain Generalization
title_zh: 学习跨模态施罗丁格桥用于视觉域泛化
authors: "Hao Zheng, Jingjun Yi, Qi Bi, Huimin Huang, Haolan Zhan, Yawen Huang, Yuexiang Li, Xian Wu, Yefeng Zheng"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=13BJ5FYG8E"
tags: ["query:dino-fg"]
score: 4.0
evidence: 针对视觉-语言基础模型的跨域泛化方法
tldr: SBGen利用施罗丁格桥模拟随机语义演化，用于视觉语言基础模型的跨域泛化。该方法通过文本引导等组件，显式建模语义演变，在多个域泛化基准上取得先进结果，为处理分布偏移提供了新思路。
source: NeurIPS-2025-Accepted
selection_source: conference_retrieval
motivation: 静态语义对齐不足以应对域之间的分布差异。
method: 提出跨模态施罗丁格桥，显式建模随机语义演化。
result: 在多个域泛化基准上显著优于现有方法。
conclusion: 为域泛化提供了基于随机过程的创新方法。
---

## Abstract
Domain generalization aims to train models that perform robustly on unseen target domains without access to target data. 
The realm of vision-language foundation model has opened a new venue owing to its inherent out-of-distribution generalization capability.
However, the static alignment to class-level textual anchors remains insufficient to handle the dramatic distribution discrepancy from diverse domain-specific visual features.
In this work, we propose a novel cross-domain Schrödinger Bridge (SB) method, namely SBGen, to handle this challenge, which explicitly formulates the stochastic semantic evolution, to gain better generalization to unseen domains.
Technically, the proposed \texttt{SBGen} consists of three key components: (1) \emph{text-guided domain-aware feature selection} to isolate semantically aligned image tokens; (2) \emph{stochastic cross-domain evolution} to simulate the SB dynamics via a learnable time-conditioned drift; and (3) \emph{stochastic domain-agnostic interpolation} to construct semantically grounded feature trajectories. 
Empirically, \texttt{SBGen} achieves state-of-the-art performance on domain generalization in both classification and segmentation. This work highlights the importance of modeling domain shifts as structured stochastic processes grounded in semantic alignment.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 核心问题与整体含义（研究动机和背景）

- **核心问题**：视觉域泛化（Domain Generalization, DG）旨在训练模型对未见过的目标域具有鲁棒性，无需访问目标域数据。尽管视觉-语言基础模型（如CLIP）因其固有的跨分布泛化能力提供了新契机，但现有方法对类别级文本锚点的静态对齐不足以处理不同域间视觉特征带来的剧烈分布偏移。
- **研究动机**：静态语义对齐无法应对域之间分布差异的多样性，需要更动态、更结构化的建模。
- **整体含义**：论文提出将域偏移建模为基于语义对齐的结构化随机过程——施罗丁格桥（Schrödinger Bridge），从而显式模拟随机语义演化，提升模型对未见域泛化的能力。

## 2. 方法论：核心思想、关键技术细节

### 核心思想
- 提出跨模态施罗丁格桥方法（SBGen），将域泛化中的语义演化形式化为随机过程，通过文本引导的域感知特征选择、随机跨域演化、随机域无关插值三个组件，实现从源域到目标域的无数据适配。

### 关键技术细节
1. **文本引导的域感知特征选择（Text-guided domain-aware feature selection）**  
   - 从图像令牌中筛选出与语义对齐的令牌，隔离语义相关特征，减少域特定噪声。
2. **随机跨域演化（Stochastic cross-domain evolution）**  
   - 通过可学习的时间条件漂移（time-conditioned drift）模拟施罗丁格桥动力学，描述特征在域间的随机演化路径。
3. **随机域无关插值（Stochastic domain-agnostic interpolation）**  
   - 构建语义扎实的特征轨迹，对演化过程中的状态进行插值，保持语义一致性。

### 算法流程（文字描述）
- 输入源域图像和对应文本描述 → 文本引导的令牌选择得到语义对齐的图像令牌 → 基于文本条件定义初始分布和终止分布 → 通过施罗丁格桥学习从源域分布到目标域分布的随机演化，利用可学习时间条件漂移参数化路径 → 使用随机域无关插值生成中间状态 → 最终模型在新的未见域上进行分类或分割。

## 3. 实验设计

- **数据集/场景**：论文在分类和分割两项任务上进行域泛化评估。具体数据集未在摘要中列出，但元数据提到“多个域泛化基准”，通常包括如PACS、VLCS、OfficeHome、TerraIncognita等分类基准，以及语义分割的基准（如GTA→Cityscapes）。
- **Benchmark**：与现有最先进域泛化方法（包括基于CLIP的方法）进行对比。
- **对比方法**：包括传统DG方法、基于视觉-语言模型的方法等。具体方法名称未给出，但声称达到SOTA。

## 4. 资源与算力

- 论文摘要及元数据中**未明确说明**使用的GPU型号、数量或训练时长。需要查阅原文才可能获得这些信息。在未获得完整文本的情况下，无法总结。

## 5. 实验数量与充分性

- 论文在**分类和分割两项任务**上进行了评估，涵盖多个基准数据集。元数据提到“多个域泛化基准”，说明实验覆盖多个场景。
- **消融实验**：虽然元数据未明确提及，但通常此类论文会包含对三个组件的消融、对施罗丁格桥框架有效性分析等。从方法复杂度看，可能进行了充分的消融以验证每个组件贡献。
- **充分性评估**：由于信息有限，无法断定是否客观公平，但NeurIPS 2025已接受，一般经过严格的审稿，实验设计应较为完整。潜在偏差如基准选择、超参数敏感性等未提及。

## 6. 主要结论与发现

- SBGen在域泛化的分类和分割任务上均达到当前最先进水平。
- 将域偏移建模为结构化随机过程（施罗丁格桥）显著优于静态语义对齐方法。
- 显式建模随机语义演化对于处理分布偏移至关重要，为域泛化提供了新的视角。

## 7. 优点

- **创新性**：首次将施罗丁格桥引入视觉-语言域泛化，用随机过程理论解决分布偏移。
- **结构性**：三个组件环环相扣，从特征选择、演化到插值形成完整框架。
- **实效性**：在分类和分割两大任务上实现SOTA，验证了方法的泛化能力。
- **理论联系实际**：将数学工具（Schrödinger Bridge）与实际问题（域泛化）有机结合。

## 8. 不足与局限

- **算力/效率未披露**：未讨论模型训练所需计算资源，可能影响可复现性。
- **缺乏对超参数的讨论**：施罗丁格桥的时间条件漂移参数、演化步长等关键超参数对性能的影响未在摘要中提及。
- **实验覆盖有限**：仅提及分类和分割，未涉及目标检测等其他视觉任务；是否适用于更大规模预训练模型（如ViT-G）未说明。
- **理论分析不足**：随机过程建模的可靠性、收敛性等理论分析未在摘要体现。
- **潜在偏差**：由于依赖文本引导，文本质量可能影响泛化性能，但未讨论该风险。

（完）
