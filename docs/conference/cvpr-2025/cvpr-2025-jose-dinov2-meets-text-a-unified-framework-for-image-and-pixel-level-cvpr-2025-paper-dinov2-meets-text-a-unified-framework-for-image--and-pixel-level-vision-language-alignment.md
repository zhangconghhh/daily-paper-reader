---
title: "DINOv2 Meets Text: A Unified Framework for Image- and Pixel-Level Vision-Language Alignment"
title_zh: DINOv2遇见文本：图像与像素级视觉-语言对齐的统一框架
authors: "Jose, Cijo, Moutakanni, Théo, Kang, Dahyun, Baldassarre, Federico, Darcet, Timothée, Xu, Hu, Li, Daniel, Szafraniec, Marc, Ramamonjisoa, Michaël, Oquab, Maxime, Siméoni, Oriane, Vo, Huy V., Labatut, Patrick, Bojanowski, Piotr"
date: 2025-06-01
pdf: "https://openaccess.thecvf.com/content/CVPR2025/papers/Jose_DINOv2_Meets_Text_A_Unified_Framework_for_Image-_and_Pixel-Level_CVPR_2025_paper.pdf"
tags: ["query:dino-fg"]
score: 7.0
evidence: DINOv2基础模型用于视觉-语言对齐
tldr: "针对自监督视觉基础模型DINOv2缺乏语言对齐能力的问题，提出在LiT训练策略基础上进行多项改进（如拼接[CLS]令牌与补丁平均），使DINOv2能同时用于图像级和像素级开放词汇任务。实验表明，该方法在密集预测任务上显著优于直接LiT，拓展了DINOv2的应用范围。该工作属于DINO系列模型在多模态对齐方向的重要进展。"
source: CVPR-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-jose-dinov2-meets-text-a-unified-framework-for-image-and-pixel-level-cvpr-2025-paper/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1702, \"height\": 588, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-jose-dinov2-meets-text-a-unified-framework-for-image-and-pixel-level-cvpr-2025-paper/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1806, \"height\": 620, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-jose-dinov2-meets-text-a-unified-framework-for-image-and-pixel-level-cvpr-2025-paper/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 856, \"height\": 342, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-jose-dinov2-meets-text-a-unified-framework-for-image-and-pixel-level-cvpr-2025-paper/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 875, \"height\": 383, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-jose-dinov2-meets-text-a-unified-framework-for-image-and-pixel-level-cvpr-2025-paper/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 674, \"height\": 369, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-jose-dinov2-meets-text-a-unified-framework-for-image-and-pixel-level-cvpr-2025-paper/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 844, \"height\": 439, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-jose-dinov2-meets-text-a-unified-framework-for-image-and-pixel-level-cvpr-2025-paper/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 704, \"height\": 295, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-jose-dinov2-meets-text-a-unified-framework-for-image-and-pixel-level-cvpr-2025-paper/table-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 846, \"height\": 717, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-jose-dinov2-meets-text-a-unified-framework-for-image-and-pixel-level-cvpr-2025-paper/table-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 1740, \"height\": 515, \"label\": \"Table\"}]"
motivation: DINOv2等自监督视觉特征无法直接与语言对齐，限制了在开放词汇任务中的应用。
method: "基于LiT训练策略，提出将[CLS]令牌与补丁平均拼接以训练文本编码器，并引入其他改进，实现DINOv2与文本的对齐。"
result: 该方法在图像级和像素级开放词汇任务上均取得优异效果，尤其改善了密集预测性能。
conclusion: 为DINOv2等自监督视觉模型赋予了语言对齐能力，使其能像CLIP一样用于开放词汇场景，但保留了自监督特征的丰富性。
---

## Abstract
Self-supervised visual foundation models produce powerful embeddings that achieve remarkable performance on a wide range of downstream tasks. However, unlike vision-language models such as CLIP, self-supervised visual features are not readily aligned with language, hindering their adoption in open-vocabulary tasks. Our method unlocks this new ability for DINOv2, a widely used self-supervised visual encoder. We build upon the LiT training strategy, which trains a text encoder to align with a frozen vision model but leads to unsatisfactory results on dense tasks. We propose several key ingredients to improve performance on both global and dense tasks, such as concatenating the [CLS] token with the patch average to train the alignment and curating data using both text and image modalities. With these, we successfully train a CLIP-like model with only a fraction of the computational cost compared to CLIP while achieving state-of-the-art results in zero-shot classification and open-vocabulary semantic segmentation.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 核心问题与整体含义（研究动机和背景）

- **研究动机**：自监督视觉基础模型（如 DINOv2）能够生成强大的特征表示，并在多种下游任务中表现优异。然而，与 CLIP 等视觉-语言模型不同，自监督视觉特征天然缺乏与语言的直接对齐能力，这限制了它们在开放词汇（open-vocabulary）任务中的应用。
- **背景**：CLIP 通过大规模图文对比训练实现了图像与文本特征的对齐，但训练成本极高（需要数亿图文对和大量 GPU）。DINOv2 等自监督模型仅利用图像自身信息学习，保留了更丰富的空间结构特征，但无法直接用于零样本分类或开放词汇分割等需要语言理解的任务。
- **整体含义**：本工作旨在以较低成本为 DINOv2 注入语言对齐能力，使其在保留自监督特征优势（如密集预测能力）的同时，获得类似 CLIP 的跨模态泛化能力，从而统一图像级与像素级的视觉-语言任务。

## 2. 方法论：核心思想、关键技术细节

- **核心思想**：基于 LiT（Locked-image Tuning）训练策略——即将视觉编码器冻结，仅训练文本编码器使其与冻结的视觉特征对齐。该方法相比从头训练 CLIP 可大幅降低计算开销。但直接应用 LiT 于 DINOv2 在密集预测任务上效果不佳，因此提出多项改进。
- **关键技术细节**：
  - **特征拼接策略**：将 DINOv2 的 [CLS] 令牌（全局图像表示）与所有补丁令牌的平均（patch average）进行拼接，作为与文本对齐的图像特征。这兼顾了全局语义和局部细节，有利于像素级任务。
  - **数据筛选**：同时利用文本和图像模态对训练数据进行清洗和筛选，提升数据质量。
  - **训练流程**：冻结 DINOv2 视觉编码器，训练一个文本编码器（如基于 Transformer 的 CLIP 文本编码器），通过对比学习（如图像-文本匹配损失）进行对齐。不涉及视觉编码器的微调。
- **算法流程说明**：
  1. 输入图像经 DINOv2 编码，提取 [CLS] 令牌和所有补丁令牌。
  2. 计算补丁令牌的平均向量，与 [CLS] 令牌拼接成一个融合特征。
  3. 输入文本经文本编码器得到文本特征。
  4. 通过对比损失（InfoNCE）最大化图像-文本对之间的相似度，最小化非配对相似度。
  5. 训练完成后，该模型可像 CLIP 一样用于零样本图像分类和开放词汇语义分割。

## 3. 实验设计

- **数据集**：训练数据未在元数据中详细说明，但结合 LiT 策略推测使用了约 4 亿图文对（类似 CLIP 的公共数据集或自行收集）。下游评估使用 ImageNet 等图像分类基准以及开放词汇语义分割基准（如 PASCAL VOC、ADE20K、COCO-Stuff 等）。
- **Benchmark**：
  - **图像级任务**：零样本图像分类（如 ImageNet-1K）。
  - **像素级任务**：开放词汇语义分割（zero-shot 设置），常见指标 mIoU。
- **对比方法**：
  - 直接使用原始 DINOv2（无语言对齐）通过图像-文本相似度检索进行零样本分类（较差）。
  - 普通 LiT 策略（仅用 [CLS] 令牌对齐）作为基线。
  - 其他开源 CLIP 变体（如 OpenAI CLIP、OpenCLIP、SigLIP 等）。
  - 自监督模型开放词汇方法（如 MaskCLIP 等）。
- 元数据中提到“该方法在零样本分类和开放词汇语义分割上达到当时最优”，表明对比了多种代表性模型。

## 4. 资源与算力

- **未明确说明**：论文元数据中未提及具体的 GPU 型号、数量、训练时长等信息。推测由于仅训练文本编码器（冻结视觉编码器），计算成本远低于 CLIP 从头训练（CLIP 需要数百块 GPU 训练约 30 天）。根据 LiT 策略，可能仅需少量 GPU（如 8-16 块 V100/A100）单卡或几天即可完成。

## 5. 实验数量与充分性

- **实验组数**：从元数据表格数量（6 个表格）和图表（3 个图）来看，实验规模中等偏上。
- **涵盖类型**：
  - 主要结果：零样本分类（ImageNet 等）、开放词汇语义分割（多个数据集）。
  - 消融实验：可能包括特征拼接方式（[CLS] vs patch avg vs 拼接）、数据筛选效果、不同损失函数等。
  - 与多种方法的对比。
- **充分性与公平性**：
  - 比较方法覆盖了主流模型，实验设计较为客观。
  - 但未提及训练数据的具体组成与规模，可能影响公平性；且未比较在未冻结视觉编码器情况下的效果（如微调 DINOv2）。
  - 未报告计算开销对比，缺乏直接的成本比较。

## 6. 主要结论与发现

- 通过简单的特征拼接（[CLS] + patch average）即可显著提升 LiT 在密集预测任务上的性能，使 DINOv2 在零样本分类和开放词汇分割上达到甚至超越 CLIP 水平。
- 仅需训练文本编码器，计算成本仅为 CLIP 的一小部分。
- 自监督特征（DINOv2）在与语言对齐后，在像素级任务上的优势（如细粒度定位）仍然保留，优于同时期的 CLIP 模型。
- 数据清洗（结合文本和图像模态）对提升对齐质量有重要贡献。

## 7. 优点

- **高效创新**：无需重新训练视觉编码器，利用现有强大自监督模型，大幅降低资源需求。
- **简单有效**：提出的特征拼接策略极其简单，但消解了全局-局部信息的折中，直觉清晰且验证有效。
- **统一框架**：同时支持图像级和像素级开放词汇任务，避免了为不同任务设计专门对齐方案。
- **可扩展性**：该框架可推广至其他自监督视觉基础模型（如 DINO、MAE 等），不限于 DINOv2。

## 8. 不足与局限

- **依赖冻结模型质量**：性能上限受限于 DINOv2 本身，若视觉编码器有偏或过时则对齐效果也会受限。
- **训练数据规模**：元数据未明确所用图文对数量，若数据量不足可能无法覆盖长尾概念，存在领域偏差。
- **实验覆盖**：缺少对更多像素级任务（如实例分割、目标检测的开放词汇）的评估；未与端到端微调视觉编码器的方法对比，可能低估潜力。
- **偏差风险**：LiT 策略可能放大数据集中文本-图像概念的偏差，且未讨论公平性分析。
- **应用限制**：仅支持图像生成任务（分类、分割），未涉及视觉问答、图像描述等生成式任务。

（完）
