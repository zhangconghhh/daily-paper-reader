---
title: "RADIOv2.5: Improved Baselines for Agglomerative Vision Foundation Models"
title_zh: RADIOv2.5：聚合视觉基础模型的改进基线
authors: "Heinrich, Greg, Ranzinger, Mike, Yin, Hongxu, Lu, Yao, Kautz, Jan, Tao, Andrew, Catanzaro, Bryan, Molchanov, Pavlo"
date: 2025-06-01
pdf: "https://openaccess.thecvf.com/content/CVPR2025/papers/Heinrich_RADIOv2.5_Improved_Baselines_for_Agglomerative_Vision_Foundation_Models_CVPR_2025_paper.pdf"
tags: ["query:dino-fg"]
score: 8.0
evidence: 改进基于DINO的聚合视觉基础模型
tldr: 针对聚合型视觉基础模型（如DINO）存在的分辨率模式偏移、教师不平衡等问题，本文提出多分辨率训练、马赛克增强和教师损失平衡等改进方法。在新基线RADIOv2.5上，这些方法显著提升了模型性能，为DINO系列模型在细粒度分类等下游任务上的应用提供了更强的基础。
source: CVPR-2025-Accepted
selection_source: conference_retrieval
motivation: 现有聚合模型存在分辨率偏移、教师不平衡等关键挑战。
method: 提出多分辨率训练、马赛克增强和均衡教师损失函数等解决方案。
result: RADIOv2.5在多个视觉任务上达到更优性能。
conclusion: 这些改进有效提升了聚合模型的鲁棒性和效率。
---

## Abstract
Agglomerative models have recently emerged as a powerful approach to training vision foundation models, leveraging multi-teacher distillation from existing models such as CLIP, DINO, and SAM. This strategy enables the efficient creation of robust models, combining the strengths of individual teachers while significantly reducing computational and resource demands. In this paper, we thoroughly analyze state-of-the-art agglomerative models, identifying critical challenges including resolution mode shifts, teacher imbalance, idiosyncratic teacher artifacts, and an excessive number of output tokens. To address these issues, we propose several novel solutions: multi-resolution training, mosaic augmentation, and improved balancing of teacher loss functions. Specifically, in the context of Vision Language Models, we introduce a token compression technique to maintain high-resolution information within a fixed token count. We release our top-performing variants at multiple scales (-B, -L, -H, and -g), along with inference code and pretrained weights.

---

## 论文详细总结（自动生成）

# RADIOv2.5：聚合视觉基础模型的改进基线 —— 详细中文总结

## 1. 核心问题与整体含义（研究动机和背景）

- **背景**：近年来，多种专用视觉基础模型（如 CLIP、DINO、SAM）蓬勃发展，但如何将它们的知识融合到一个统一模型中成为一个关键需求。已有工作（如 AM-RADIO、SAM-CLIP、Theia、UNIC 等）通过**多教师蒸馏**（multi-teacher distillation）实现知识聚合，但仍存在几个重要挑战未解决：
  - **分辨率模式切换**：学生模型在不同输入分辨率下产生截然不同的特征分布（低分辨率表现 DINO 类特征，高分辨率表现 SAM 类特征），导致准确性不一致。
  - **教师不平衡**：不同教师模型的激活量级、分布差异大，易导致蒸馏过程中偏重某些教师。
  - **教师伪影**：如 SAM 的 VitDet 窗口效应等特有 artifact 被学生继承。
  - **令牌数量过多**：高分辨率处理产生大量视觉标记，给视觉语言模型（VLM）带来二次复杂度压力。

- **研究意图**：本文旨在系统分析上述问题，提出一系列创新解决方案，构建一个鲁棒、灵活、高效的统一视觉基础模型 **RADIOv2.5**，并公开发布多个尺度的预训练权重。

## 2. 方法论：核心思想、关键技术细节

### 2.1 多分辨率训练（Multi-Resolution Training）
- **核心思想**：让学生模型在所有分辨率下都能从所有教师学习，避免仅在特定分辨率接触特定教师导致的模式切换。
- **具体实现**：
  - 分三阶段训练：① 低分辨率 256² 训练 300k 迭代；② 中分辨率 432² 训练 300k 迭代；③ 同时以 432² 和 1024² 分辨率训练 300k 迭代。
  - 对于高分辨率学生 vs 低分辨率教师：通过插值下采样学生特征匹配教师。
  - 对于低分辨率学生 vs 高分辨率教师（SAM 需 1024²）：使用**马赛克增强**（详见 2.2）。
- **公式**：学生输出特征 \( \mathbf{z} = f(x) \)，对每位教师 \(t\) 设置适配器头 \(g^{(t)}\)，损失函数 \( \mathcal{L}_t = \ell_s(\hat{z}_s^{(t)}, z_s^{(t)}) + \sum_i \ell_p(\hat{z}_p^{(t,i)}, z_p^{(t,i)}) \)。

### 2.2 马赛克增强（Mosaic Augmentation）
- **目的**：高效训练低分辨率学生与高分辨率教师（如 SAM）的配对，降低计算开销。
- **方法**：将 \(k \times k\) 张低分辨率图像拼成一张 1024² 的大图，对 SAM 进行一次推理，然后裁剪出 \(k^2\) 个特征图用于蒸馏。例如，在 432² 学生分辨率下使用 2×2 马赛克，每幅图周围填充 80 像素。
- **效果**：不仅节省计算，还因图像位置多样性减少了位置编码伪影，提升了学生质量。

### 2.3 教师损失平衡（PHI-S 标准化）
- **方法**：采用 PCA-Hadamard 各向同性标准化（PHI-S），将每个教师的激活特征旋转并缩放，使各通道方差均匀化且总体方差为 1，但保持通道间关联结构。
- **公式**：\( \mathbf{X}_i' = \phi_i^{-1} \mathbf{R}_i \mathbf{X}_i \)，其中 \(\mathbf{R}_i = \mathbf{H}_C \mathbf{U}_i^\intercal\)，\(\phi_i = \sqrt{\frac{1}{C}\sum_j \lambda_j}\)。
- **优点**：稳定训练、平衡各教师的损失贡献，且可事后逆向调整。

### 2.4 令牌压缩（Token Merging）
- **灵感**：来自 ToMeSD（Bolya & Hoffman, 2023）。
- **方法**：在视觉编码器输出上应用**二分图软匹配**（bipartite soft matching），将相似令牌合并，同时采用步长分区确保每个图像区域保留表征。使用特征向量本身（而非注意力键）计算相似度，并在编码器末端一次性合并而非逐层合并。
- **效果**：在保持 256 个令牌的前提下，相比 pixel unshuffling（如 InternVL1.5），在 VLM 基准上平均提升 +5.9 分（表 1，配置 E vs D）。对 RADIOv2.5 的提升大于 SigLIP，因其特征噪声更低，聚类更稳定。

## 3. 实验设计：数据集、基准、对比方法

### 3.1 评估基准
- **分类**：ImageNet-1k 零样本（zero-shot）和 k-NN 准确率。
- **语义分割**：ADE20k，使用线性探测（固定骨干）。
- **实例分割**：COCO，使用 EfficientViT 协议 + SAM 适配器。
- **3D 理解**：Probe3D（Navi 数据集），评估深度、表面法线、对应关系。
- **视觉语言（VLM）**：基于 VILA 框架 + MN-Minitron-8B 大语言模型，使用 ShareGPT4v 和 VFLAN 数据集训练。报告 TextVQA、ChartQA、DocVQA、InfoVQA、OCRBench 等高分基准。
- **稠密多任务**：NYUDv2、PASCAL Context，使用 MLoRE 的 conv 头进行冻骨探测。

### 3.2 对比方法
- **聚合模型**：SAM-CLIP、Theia、UNIC-B/L、UNIT（不同规模）。
- **视觉编码器**：OpenAI-CLIP、RADIOv2.1-H、SigLIP-SO400M。
- **消融对照**：基线 RADIOv2.1-L（复现 AM-RADIO，ViT-L），逐步加入多分辨率训练、更换教师（SigLIP 替换 OpenAI CLIP）、扩大骨干（ViT-H）、令牌压缩。

### 3.3 实验公平性
- 所有 VLM 实验统一使用相同 LLM（MN-Minitron-8B）、相同数据混合、相同训练流程。
- 冻骨探测时保持学习率、权重一致，不做参数调优。
- 令牌压缩后保持所有编码器给 LLM 的视觉令牌数相同（196 或 256）。

## 4. 资源与算力

- **文末明确说明的部分**：
  - 训练数据：DataComp1B 数据集。
  - 迭代次数：600k 迭代（分三阶段，每阶段 300k）。
  - 批次大小：低分辨率分支 1024，高分辨率（SAM）分支 128。
  - 骨干规模：ViT-L（320M 参数）、ViT-H（653M 参数）。
- **未明确说明的部分**：
  - GPU 型号、节点数量、总训练耗时、显存占用等均未提供。这在一定程度上降低了可复现性细节。

## 5. 实验数量与充分性

- **组数**：论文包含约 6 组主要表格（表 1-5）及多个附录表格（如表 11、12、18 等），涵盖分类、分割、3D、VLM、尺度等变性等多个方面。
- **消融实验**：表 1 逐项消融了多分辨率训练、教师更换、骨干扩大、令牌压缩，清晰展示了每个改进的贡献。
- **对比实验**：与 5 个聚合模型、3 个主流视觉编码器在多个任务上比较，涉及不同参数规模。
- **充分性评价**：实验设计较全面，覆盖了论文提出的主要创新点并进行定量验证。但对比方法数量有限（尤其聚合模型只选了少数几篇），且未与其他最新多编码器融合方法（如 Eagle、BRAVE、Cambrian-1）进行直接比较（文中虽在相关工作提及，但未将它们的 VLM 结果纳入表 5）。

## 6. 主要结论与发现

1. **多分辨率训练**有效消除模式切换，使模型在 256² 至 1024² 全范围保持一致的准确性（图 2）。
2. **马赛克增强**不仅降低了高分辨率教师蒸馏的计算开销，还因位置多样性提升了学生质量。
3. **PHI-S 标准化**改善了教师损失平衡和训练稳定性。
4. **令牌压缩（ToMe）** 在保持信息多样性的前提下大幅减少视觉令牌数，对 VLM 提升显著（+5.9 平均分），且 RADIOv2.5 比 SigLIP 受益更大。
5. **RADIOv2.5 系列**在分类、分割、3D、VLM 等几乎所有下游任务上超越先前基线（如 AM-RADIO、UNIC、UNIT），在稠密任务上尤其突出（例：ADE20k mIoU 53.97 vs UNIC-L 48.30）。
6. **尺度等变性**显著改善：RADIOv2.5-H 的尺度等变误差（0.119）接近 DINOv2（0.126），远优于 AM-RADIO（0.357）和 SigLIP 分块推理（0.623）。

## 7. 优点

- **系统性解决实际问题**：对模式切换、教师不平衡、令牌爆炸等痛点逐一分析并给出可行方案，而非仅堆叠技巧。
- **多分辨率训练独创性**：同时训练两个分辨率分支，简单有效。
- **马赛克增强的创新应用**：将多图像拼接用于高分辨率教师蒸馏，兼顾效率和效果。
- **令牌压缩方法适配 VLM**：在编码器输出端一次性合并，比逐层合并或 pixel unshuffling 更好保留信息。
- **开源贡献**：公开多个尺度的预训练模型和推理代码（GitHub + Hugging Face），促进重复研究和应用。
- **严谨评估**：采用统一框架比较不同视觉编码器，确保公平。

## 8. 不足与局限

1. **算力资源未透明报告**：缺少 GPU 型号/数量、训练总时长等关键信息，影响可复现性和能耗评估。
2. **对比范围有限**：仅与 SAM-CLIP、Theia、UNIC、UNIT 四个聚合模型比较，未纳入近期更先进的多编码器融合模型（如 Eagle、BRAVE、Cambrian-1）的 VLM 结果，也缺乏与更小规模的模型（如 ViT-B 等）的系统对比。
3. **VLM 实验单一框架**：仅使用 VILA 框架，未验证在其他 VLM 架构（如 LLaVA-NeXT、InternVL）上的泛化性。
4. **教师依赖**：当前教师集合固定（CLIP、DINOv2、SAM），若未来出现更优教师，模型可能需重新蒸馏，未讨论持续学习或动态教师选择。
5. **应用限制**：令牌压缩虽好，但下游分割等稠密任务需要按压缩位置还原，文中主要评估了 VLM 场景，对其他任务的影响未深入分析。
6. **消融范围**：对 PHI-S 的影响仅在附录简要提及，正文中未单独列出消融表量化其贡献（如与直接标准化对比）。

（完）
