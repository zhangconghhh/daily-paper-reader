---
title: "Argus: A Compact and Versatile Foundation Model for Vision"
title_zh: Argus：紧凑且多功能的视觉基础模型
authors: "Zhuang, Weiming, Chen, Chen, Li, Zhizhong, Sajadmanesh, Sina, Li, Jingtao, Huang, Jiabo, Sehwag, Vikash, Sharma, Vivek, Shinozaki, Hirotaka, Garcia, Felan Carlo, Zhan, Yihao, Adachi, Naohiro, Eki, Ryoji, Spranger, Michael, Stone, Peter, Lyu, Lingjuan"
date: 2025-06-01
pdf: "https://openaccess.thecvf.com/content/CVPR2025/papers/Zhuang_Argus_A_Compact_and_Versatile_Foundation_Model_for_Vision_CVPR_2025_paper.pdf"
tags: ["query:dino-fg"]
score: 6.0
evidence: 紧凑视觉基础模型，适用于图像分类
tldr: 现有的视觉基础模型存在数据消耗大、任务性能不一致等问题。本文提出Argus，一个紧凑多功能的视觉基础模型，采用统一多任务架构和两阶段训练策略，通过轻量适配器注入任务特定归纳偏置。实验表明Argus在多个视觉任务上取得有竞争力的性能，为分类等下游任务提供了高效的基础模型方案。
source: CVPR-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-zhuang-argus-a-compact-and-versatile-foundation-model-for-vision-cvpr-2025-paper/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 825, \"height\": 712, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-zhuang-argus-a-compact-and-versatile-foundation-model-for-vision-cvpr-2025-paper/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1794, \"height\": 532, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-zhuang-argus-a-compact-and-versatile-foundation-model-for-vision-cvpr-2025-paper/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 812, \"height\": 658, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-zhuang-argus-a-compact-and-versatile-foundation-model-for-vision-cvpr-2025-paper/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 1801, \"height\": 533, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-zhuang-argus-a-compact-and-versatile-foundation-model-for-vision-cvpr-2025-paper/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1799, \"height\": 708, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-zhuang-argus-a-compact-and-versatile-foundation-model-for-vision-cvpr-2025-paper/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1800, \"height\": 433, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-zhuang-argus-a-compact-and-versatile-foundation-model-for-vision-cvpr-2025-paper/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 862, \"height\": 347, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-zhuang-argus-a-compact-and-versatile-foundation-model-for-vision-cvpr-2025-paper/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 862, \"height\": 235, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-zhuang-argus-a-compact-and-versatile-foundation-model-for-vision-cvpr-2025-paper/table-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 858, \"height\": 350, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-zhuang-argus-a-compact-and-versatile-foundation-model-for-vision-cvpr-2025-paper/table-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 870, \"height\": 221, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-zhuang-argus-a-compact-and-versatile-foundation-model-for-vision-cvpr-2025-paper/table-007.webp\", \"caption\": \"\", \"page\": 0, \"index\": 7, \"width\": 869, \"height\": 185, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-zhuang-argus-a-compact-and-versatile-foundation-model-for-vision-cvpr-2025-paper/table-008.webp\", \"caption\": \"\", \"page\": 0, \"index\": 8, \"width\": 863, \"height\": 211, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-zhuang-argus-a-compact-and-versatile-foundation-model-for-vision-cvpr-2025-paper/table-009.webp\", \"caption\": \"\", \"page\": 0, \"index\": 9, \"width\": 1786, \"height\": 227, \"label\": \"Table\"}]"
motivation: 现有视觉基础模型训练资源消耗大且跨任务性能不稳定，亟需紧凑统一的多任务架构。
method: 采用共享骨干网络和轻量适配器实现多任务预训练，两阶段训练策略支持多种视觉任务。
result: 在多个视觉任务上达到有竞争力的性能，验证了紧凑架构的有效性。
conclusion: Argus为紧凑高效的基础模型设计提供了新方向，可推广至图像分类等应用。
---

## Abstract
While existing vision and multi-modal foundation models can handle multiple computer vision tasks, they often suffer from significant limitations, including huge demand for data and computational resources during training and inconsistent performance across vision tasks at deployment time. To address these challenges, we introduce Argus (The name comes from Argus Panoptes -- a hundred-eyed giant with "all-seeing" capability in Greek mythology), a compact and versatile vision foundation model designed to support a wide range of vision tasks through a unified multitask architecture. Argus employs a two-stage training strategy: (i) multitask pretraining over core vision tasks with a shared backbone that includes a lightweight adapter to inject task-specific inductive biases, and (ii) scalable and efficient adaptation to new tasks by fine-tuning only the task-specific decoders. Extensive evaluations demonstrate that Argus, despite its relatively compact and training-efficient design of merely 100M backbone parameters (only 13.6% of which are trained using 1.6M images), competes with and even surpasses much larger models. Compared to state-of-the-art foundation models, Argus not only covers a broader set of vision tasks but also matches or outperforms the models with similar sizes on 12 tasks. We expect that Argus will accelerate the real-world adoption of vision foundation models in resource-constrained scenarios.

---

## 论文详细总结（自动生成）

# 论文《Argus: A Compact and Versatile Foundation Model for Vision》详细中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）
- **核心问题**：现有视觉基础模型（VFMs）虽然能处理多种计算机视觉任务，但存在两大缺陷：一是训练时对数据量和计算资源需求巨大（如Florence-2需126M图像和50亿标注）；二是不同任务间的性能不一致，且难以高效扩展到新任务。
- **研究动机**：希望构建一个紧凑、训练高效、性能优良且易于扩展的视觉基础模型，既能支持多样化视觉任务，又能在资源受限场景下实际部署。
- **整体含义**：提出一种统一多任务架构，通过两阶段训练策略（多任务预训练 + 任务特定微调），以轻量骨干（仅100M参数）在12个任务上与或超越大模型，提升了VFMs的实用性和可部署性。

## 2. 论文提出的方法论：核心思想、关键技术细节
- **核心思想**：以多任务学习为核心，结合强大的自监督预训练特征（DINOv2），通过轻量适配器注入任务特定归纳偏置，实现紧凑且可扩展的视觉基础模型。
- **两阶段训练策略**：
  - **第一阶段：多任务预训练**（Multi-task Pretraining）
    - 使用共享骨干（冻结的DINOv2 ViT-Base + 轻量适配器，仅13.6%骨干参数可训练）同时训练5个核心任务：目标检测、实例分割、语义分割、姿态估计、图像分类。
    - 适配器设计：基于ViT-Adapter改进，包含空间先验模块（SPM）生成多尺度特征，以及多个交互块（Injection/Extraction）实现双向交互。
    - 关键技术优化（3个Recipe）：
      1. **冻结ViT**：防止灾难性遗忘，保留DINOv2表示能力。
      2. **用组归一化（GN）替代批归一化（BN）**：避免不同任务共享BN统计量的冲突。
      3. **交互前缩放ViT特征**：稳定特征范数，提升适配效果。
  - **第二阶段：可扩展的任务特定适应**（Scalable Task-specific Adaptation）
    - 冻结整个骨干（包括适配器），仅微调各任务新添加的解码器。
    - 支持灵活更换先进解码器（如CO-DETR、Mask2Former等），无需重新预训练。
- **多任务损失**：加权求和，权重通过经验设定（未使用复杂梯度平衡算法）。

## 3. 实验设计：数据集、基准、对比方法
- **数据集**：
  - **多任务预训练**：COCO（检测/实例分割/姿态估计）、ADE20K（语义分割）、ImageNet（分类）。
  - **任务适应**：ADE20K（实例/全景分割）、NYUv2（深度/边界/表面法线）、PASCAL-Context（人体解析/显著性）、MVTecAD-4（异常检测）。
  - **总计约160万张图像**，所有任务使用标准训练/验证集，无数据泄露。
- **基准与指标**：12个任务分别对应标准指标（如ADE20K语义分割mIoU、COCO检测AP、ImageNet Top-1等）。
- **对比方法**：包括Florence-1&2、Unified-IO 1&2、Uni-Perceiver v2、4M、DINOv2、GLID、MAE等，涵盖现有主流视觉及多模态基础模型。

## 4. 资源与算力
- **训练配置**：8块NVIDIA H100 GPU。
- **多任务预训练（5个核心任务，300K迭代）**：约4天。
- **任务特定适应（7个新任务）**：在相同硬件下，每任务在12小时内完成。
- **骨干参数**：100M（适配器13.6M可训练），数据量约1.6M图像。

## 5. 实验数量与充分性
- **实验数量**：全面覆盖12项视觉任务的主性能对比，以及多项消融实验：
  - 训练配方（冻结ViT、GN、缩放、EMA、迭代步数） → 表3
  - 多任务学习算法对比（FAMO、GradNorm vs 经验加权） → 表4
  - 核心任务选择（移除某任务、替换任务组合、扩展至8/11任务） → 表5、表13
  - 解码器灵活性（Mask R-CNN、Mask DINO、CO-DETR） → 表6
  - 骨干大小（Base vs Large） → 表7
  - 未见过数据集泛化（Pascal VOC Person/Cars等） → 表8
  - 任务适应阶段是否微调适配器 → 表9
- **充分性与公平性**：
  - 实验设计较充分，涵盖了架构、算法、任务选择、泛化性等多维度。
  - 公平性讨论：对比模型使用相同或类似的解码器（如DINOv2加相同解码器），但部分模型（如Florence）在更大数据上训练，文章指出此差异。
  - 消融实验系统且全面，结论可信。

## 6. 论文的主要结论与发现
- Argus在12个视觉任务中的10个上达到最优（对比的模型如Florence-2、Unified-IO等），其余2个（目标检测、分类）排名第二。
- 尽管只有100M参数（仅为Florence-2的232M的43%），Argus在ADE20K语义分割上达56.5% mIoU，超过Florence-2的54.9%。
- 冻结ViT、用GN替换BN、缩放交互特征是实现有效多任务预训练的关键。
- 核心任务的选择（5个覆盖图像级、区域级、像素级）平衡了性能与可扩展性。
- 模型具备良好泛化能力，在未见过数据集上显著优于4M和Florence-2。

## 7. 优点：方法或实验设计上的亮点
- **架构层面**：
  - 巧妙地利用冻结的DINOv2 + 轻量适配器，实现紧凑高效的多任务学习。
  - 提出的3个训练Recipe（冻结ViT、GN、缩放）简单有效，显著缓解任务干扰。
  - 支持灵活替换最先进的解码器，兼容性好。
- **实验层面**：
  - 覆盖12个任务，是当时覆盖最广的VFM之一。
  - 消融实验全面，深入分析了每个设计选择的影响。
  - 在资源消耗远小于同类模型的情况下取得竞争力结果，实用性突出。
  - 进行了未见过数据集的泛化测试，验证鲁棒性。

## 8. 不足与局限
- **实验覆盖**：核心任务选择依赖经验，未系统研究所有可能的任务组合；扩展至8/11任务时出现过拟合（尽管数据量少）。
- **MTL算法**：未深入研究更高级的梯度/损失平衡算法，仅使用经验加权，可能存在优化空间。
- **对比公平性**：尽管尽力公平，但部分对比模型（如Florence）在大得多的私有数据上训练，Argus在检测/分类上落后；但总体评价客观。
- **应用限制**：模型主要聚焦在中等分辨率视觉任务，未涉足OCR、低层视觉等；在极端资源约束场景（如移动端）的部署未讨论。
- **偏差风险**：仅使用英文/公开数据集，未考虑领域偏移或长尾分布问题。

（完）
