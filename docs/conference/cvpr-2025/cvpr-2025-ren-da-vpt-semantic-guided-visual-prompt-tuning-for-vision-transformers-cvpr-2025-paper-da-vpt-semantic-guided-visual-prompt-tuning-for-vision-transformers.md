---
title: "DA-VPT: Semantic-Guided Visual Prompt Tuning for Vision Transformers"
title_zh: DA-VPT：面向视觉transformer的语义引导视觉提示调优
authors: "Ren, Li, Chen, Chen, Wang, Liqiang, Hua, Kien"
date: 2025-06-01
pdf: "https://openaccess.thecvf.com/content/CVPR2025/papers/Ren_DA-VPT_Semantic-Guided_Visual_Prompt_Tuning_for_Vision_Transformers_CVPR_2025_paper.pdf"
tags: ["query:dino-fg"]
score: 8.0
evidence: 面向视觉transformer的语义引导提示调优，直接适用于分类微调
tldr: 视觉提示调优（VPT）是ViT参数高效微调的有效方法，但提示与图像块的分布关系未被充分利用。本文提出分布感知视觉提示调优（DA-VPT），利用度量学习从类别相关语义数据中学习距离度量，从而引导提示分布。实验证明DA-VPT在多个分类数据集上优于现有VPT方法，为ViT在分类任务中的高效微调提供了新工具。
source: CVPR-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-ren-da-vpt-semantic-guided-visual-prompt-tuning-for-vision-transformers-cvpr-2025-paper/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 816, \"height\": 611, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-ren-da-vpt-semantic-guided-visual-prompt-tuning-for-vision-transformers-cvpr-2025-paper/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 915, \"height\": 414, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-ren-da-vpt-semantic-guided-visual-prompt-tuning-for-vision-transformers-cvpr-2025-paper/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1799, \"height\": 645, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-ren-da-vpt-semantic-guided-visual-prompt-tuning-for-vision-transformers-cvpr-2025-paper/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 1747, \"height\": 348, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-ren-da-vpt-semantic-guided-visual-prompt-tuning-for-vision-transformers-cvpr-2025-paper/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 848, \"height\": 634, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-ren-da-vpt-semantic-guided-visual-prompt-tuning-for-vision-transformers-cvpr-2025-paper/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 823, \"height\": 313, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-ren-da-vpt-semantic-guided-visual-prompt-tuning-for-vision-transformers-cvpr-2025-paper/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 855, \"height\": 452, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-ren-da-vpt-semantic-guided-visual-prompt-tuning-for-vision-transformers-cvpr-2025-paper/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 1438, \"height\": 355, \"label\": \"Table\"}]"
motivation: 现有VPT方法忽略了提示与图像块之间的分布关系。
method: 利用度量学习学习类别语义距离，引导提示分布对齐类相关特征。
result: 在多个图像分类基准上取得优于现有VPT方法的性能。
conclusion: 为ViT在分类任务中的参数高效微调提供了新的分布视角。
---

## Abstract
Visual Prompt Tuning (VPT) has become a promising solution for Parameter-Efficient Fine-Tuning (PEFT) approach for Vision Transformer (ViT) models by partially fine-tuning learnable tokens while keeping most model parameters frozen. Recent research has explored modifying the connection structures of the prompts. However, the fundamental correlation and distribution between the prompts and image tokens remain unexplored. In this paper, we leverage metric learning techniques to investigate how the distribution of prompts affects fine-tuning performance. Specifically, we propose a novel framework, Distribution Aware Visual Prompt Tuning (DA-VPT), to guide the distributions of the prompts by learning the distance metric from their class-related semantic data. Our method demonstrates that the prompts can serve as an effective bridge to share semantic information between image patches and the class token. We extensively evaluated our approach on popular benchmarks in both recognition and segmentation tasks. The results demonstrate that our approach enables more effective and efficient fine-tuning of ViT models by leveraging semantic information to guide the learning of the prompts, leading to improved performance on various downstream vision tasks.

---

## 论文详细总结（自动生成）

## 1. 论文的核心问题与整体含义（研究动机和背景）

视觉提示调优（Visual Prompt Tuning, VPT）通过仅微调少量可学习的提示 token 而冻结大部分模型参数，已成为 Vision Transformer（ViT）参数高效微调（PEFT）的流行方法。近年工作主要改进提示的连接结构（如跨层连接、动态门控），但忽略了提示与图像 token 之间**本质的分布关系和语义关联**。现有 VPT 随机初始化提示并仅由下游任务目标优化，导致提示可能从任意类别吸引特征，阻碍类 token 聚合类特定信息。本文旨在回答：**能否通过引导提示的分布，使其成为图像 token 和类 token 之间语义信息流的有效桥梁？**

## 2. 论文提出的方法论

- **核心思想**：利用度量学习（Metric Learning）在 ViT 深层构建语义度量，引导提示向同类视觉 token 靠拢、远离异类 token，同时使类 token 与对应提示对齐，从而实现更有效的类特定信息聚合。
- **关键技术细节**：
  - **语义度量损失（L_ML）**：基于 Proxy-Anchor 损失，分别计算提示与视觉 token 的度量（L_ML(X,P)）以及提示与类 token 的度量（L_ML(P,x_cls)）。公式如下：
    - 对每个提示 p_k，拉近 p_k 与同类视觉 token（X^+_p），推远与异类 token（X^-_p），使用余弦相似度与 margin δ。
    - 类似地，引导类 token 与对应提示靠近，与异类提示远离。
  - **总损失**：L = L_CE + β·L_ML(X,P) + λ·L_ML(P,x_cls)，其中 β、λ 为超参数。
  - **动态类-提示映射**：由于提示数 M << 类别数 C，先通过一次预热 epoch 用 k-means 将 C 类聚类为 M 组，每组对应一个提示。每 epoch 后根据更新后的类表示重新聚类。
  - **高效偏置调优（Efficient Bias）**：额外解锁注意力层中 Key 和 Value 投影的偏置项（b_K, b_V），以提供分布灵活性，仅增加少量参数。
- **算法流程**：
  1. 预热阶段：用预训练 ViT 提取每个类的 [CLS] 均值，k-means 聚类建立提示–类别映射。
  2. 每层输入：将可学习提示与图像 token 拼接。
  3. 前向传播后，在深层（通常最后一层）计算 L_ML 损失，反向传播更新提示及可调偏置。
  4. 每 epoch 后更新类表示并重新聚类。

## 3. 实验设计

- **数据集与场景**：
  - **视觉识别**：FGVC 基准（5 个细粒度分类数据集：CUB-200-2011、NABirds、Oxford Flowers、Stanford Dogs、Stanford Cars）。
  - **少样本迁移**：VTAB-1k 基准（19 个数据集，分为自然、专业、结构化三类）。
  - **密集预测**：ADE20K 和 PASCAL Context 语义分割。
- **Benchmark**：与 Full Fine-tuning、Linear Probing、VPT-Shallow/Deep、E2VPT、GateVPT、Adapter、Bias、SSF、SNF、MoSA 等 SOTA PEFT 方法对比。
- **模型与预训练**：ViT-B（分类）和 ViT-L（分割），预训练包括监督 ImageNet-21K、自监督 MAE 和 MoCo v3。
- **评价指标**：分类任务 Top-1 准确率；分割任务 mIoU（单尺度和多尺度）。

## 4. 资源与算力

论文**未明确说明**训练所使用的 GPU 型号、数量及训练时长。仅在消融实验的硬件测试中提及在 **RTX 4090 GPU** 上测量了每张图像的推理延迟和内存占用（见表 4 注释）。因此，难以评估训练过程中的计算开销细节。

## 5. 实验数量与充分性

- **实验数量**：共涉及 **24 个识别任务**（5 FGVC + 19 VTAB-1k）和 **2 个分割任务**，外加多组消融实验（组件贡献、层位置、偏置类别、聚类延迟、提示初始化等）。实验规模较大。
- **充分性**：
  - 对比方法覆盖了绝大部分主流 PEFT 方法，且包括不同预训练范式（监督、自监督）。
  - 消融实验系统分析了各组件贡献、超参数影响、层选择。
  - 代码已开源，可复现。
- **客观性与公平性**：论文报告了本文复现的基线结果（如 VPT），并引用其他方法原文结果。超参数通过搜索确定，设置合理。实验设计较为公平，结论可信。

## 6. 论文的主要结论与发现

- DA-VPT 在 **24 个识别任务**上一致优于 VPT-Deep 和其他 VPT 变体，尤其在自监督骨干（MAE、MoCo v3）上提升显著（如 VTAB-1k 平均准确率提升 4~7 pp）。
- 使用 **更少的提示和参数**（如仅 0.24M 可调参数）即可超过全微调性能。
- **提示可作为语义桥接**：可视化表明，经度量子导向后，提示能聚焦于类相关区域，并通过注意力机制将信息传递给类 token。
- **高效偏置调优**（仅调整 K、V 偏置）进一步带来稳定提升，且参数增量极小。
- 动态语义映射避免了优化不平衡，而简单均值初始化反而有害。

## 7. 优点

- **创新性**：首次将度量学习引入 VPT，揭示并利用提示与视觉 token 的分布关系，而非仅修改结构。
- **方法简洁有效**：Proxy-Anchor 损失天然适配提示作为类代表角色；动态聚类映射解决了提示数远小于类别数的问题。
- **泛化性强**：在监督、自监督、不同骨干规模（ViT-B/L）、识别与分割任务上均取得显著提升。
- **参数效率高**：仅增加少量参数（0.2~0.3M）即实现大幅性能增益，优于许多更复杂的 PEFT 方法。

## 8. 不足与局限

- **计算开销**：每 epoch 后的 k-means 聚类有一定延迟（初始 epoch 较高），虽然后期稳定，但仍增加额外时间。
- **超参数敏感性**：度量损失中的 margin δ、温度 τ 及损失权重 β、λ 需要搜索，未探讨其广泛鲁棒性。
- **提示初始化影响**：均值初始化反而降低性能，表明提示初始策略仍需进一步研究。
- **未覆盖所有 PEFT 组合**：论文只与单独提示调优方法对比，未深入探索与其他 PEFT（如 LoRA、Adapter）的联合优化。
- **大模型验证有限**：分割任务使用了 ViT-L，但分类实验仅在 ViT-B 上进行，未在更大 ViT 模型（如 ViT-H、ViT-22B）上验证。
- **应用场景限制**：仅在视觉领域验证，未涉及多模态或 NLP 任务，通用性待检验。

（完）
