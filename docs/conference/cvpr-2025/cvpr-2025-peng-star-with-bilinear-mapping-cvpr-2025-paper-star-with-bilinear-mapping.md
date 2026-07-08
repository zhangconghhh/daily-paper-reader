---
title: Star with Bilinear Mapping
title_zh: 星型双线性映射
authors: "Peng, Zelin, Huang, Yu, Xu, Zhengqin, Tang, Feilong, Hu, Ming, Yang, Xiaokang, Shen, Wei"
date: 2025-06-01
pdf: "https://openaccess.thecvf.com/content/CVPR2025/papers/Peng_Star_with_Bilinear_Mapping_CVPR_2025_paper.pdf"
tags: ["query:dino-fg"]
score: 7.0
evidence: 用于分类的视觉Transformer架构
tldr: 针对Transformer全局建模中二次复杂度的问题，提出星型双线性映射（SBM）架构，通过低秩分解和逐元素乘法实现线性复杂度的全局上下文建模。在图像分类和语义分割任务上，SBM以显著降低的计算成本取得了与标准ViT相当的分类性能，表明高效架构同样能胜任分类任务。
source: CVPR-2025-Accepted
selection_source: conference_retrieval
motivation: 传统Transformer的全局注意力复杂度为二次，亟需线性复杂度的替代架构用于视觉分类。
method: 提出星型双线性映射模块（SBM），融合低秩分解和星型操作（逐元素乘法），以线性复杂度捕获全局上下文。
result: 在图像分类和语义分割数据集上，SBM在降低计算量的同时保持了与标准Transformer可比的性能。
conclusion: SBM为高效视觉骨干网络提供了新选择，尤其适用于资源受限的分类场景。
---

## Abstract
Contextual modeling is crucial for robust visual representation learning, especially in computer vision. Although Transformers have become a leading architecture for vision tasks due to their attention mechanism, the quadratic complexity of full attention operations presents substantial computational challenges. To address this, we introduce Star with Bilinear Mapping (SBM), a Transformer-like architecture that achieves global contextual modeling with linear complexity. SBM employs a bilinear mapping module (BM) with low-rank decomposition strategy and star operations (element-wise multiplication) to efficiently capture global contextual information. Our model demonstrates competitive performance on image classification and semantic segmentation tasks, delivering significant computational efficiency gains compared to traditional attention-based models.

---

## 论文详细总结（自动生成）

# 论文总结：Star with Bilinear Mapping (SBM)

## 1. 核心问题与整体含义（研究动机和背景）

- **研究动机**：Transformer 凭借全局注意力机制成为视觉任务的主流架构，但全注意力的二次复杂度（O(n²d)）限制了其在高分辨率图像或长序列场景下的可扩展性。现有解决方案（如线性注意力、Mamba 状态空间模型）虽将复杂度降至线性，但常面临性能退化或训练效率瓶颈。
- **核心目标**：设计一种 Transformer 类架构，在保持全局上下文建模能力的同时，实现**线性复杂度**，并且支持快速训练与推理，避免像 Mamba 那样依赖定制 CUDA 加速。
- **整体含义**：通过将双线性映射（Bilinear Mapping）与星操作（element-wise multiplication）相结合，SBM 在理论上实现了指数级特征维度增长（源于星操作的隐藏维度扩展），同时以线性复杂度捕获全局依赖，为高效视觉骨干网络提供了新范式。

## 2. 方法论：核心思想、关键技术细节

### 核心思想
- **双线性映射 + 低秩分解**：传统双线性映射 B(x,y)=xᵀAy 复杂度为 O(nm)，但 SBM 将其适配到特征空间（x 为空间维度，y 为通道维度），并通过低秩分解将空间投影矩阵 Ws 拆分为两个小矩阵（n×m 和 m×n，m≪n），实现线性复杂度。
- **星操作（逐元素乘法）**：两个线性变换后的特征进行逐元素相乘，可产生 O(d²) 量级的隐式高维特征，且堆叠多层后特征空间维度呈指数增长，增强上下文建模能力。
- **三分支设计**：
  - **双线性映射分支（U）**：由 x_proj 经空间低秩投影 Ws₁、Ws₂ 和通道投影 Wc 得到，激活后用于捕获全局交互。
  - **线性映射分支（V）**：由 x_proj 经通道投影 Wv 得到，保留完整空间信息。
  - **映射控制分支（G）**：由原始输入 x 经线性投影 WG 和激活函数得到，通过星操作与主分支融合，增强稳定性并保留空间结构。

### 关键技术细节
- **低秩分解**：空间投影矩阵 Ws 分解为两个低秩矩阵（n×m 和 m×n），m 为超参数（论文固定为 64），大幅减少参数量和计算量。
- **SBM 模块公式**：
  - f(x) = σ(U) ∗ V，其中 U = Ws·x_proj·Wcᵀ，V = x_proj·Wvᵀ，x_proj = Conv(x·Wiᵀ)
  - G(x) = σ(x·Wgᵀ)
  - h(x) = G(x) ∗ f(x)
- **复杂度**：Ω(SBM) = 5nd² + 2ndm + 2nd，为线性复杂度（相对于传统注意力的 4nd² + 2n²d）。
- **架构**：采用四阶段层级结构（类似 Swin Transformer），每个阶段包含 Stem 或下采样层 + 若干 SBM 块（含 CPE、LN、SBM 模块、FFN）。

## 3. 实验设计

### 数据集与任务
| 任务 | 数据集 | 评估指标 |
|------|--------|----------|
| 图像分类 | ImageNet-1K（1.28M 训练，50K 验证，1000 类） | Top-1 准确率 |
| 迁移学习 | CIFAR-100（32×32） | Fine-tuning 和 Linear Probing 的 Top-1 |
| 语义分割 | ADE20K（20K 训练，2K 验证，150 类） | mIoU |
| 目标检测与实例分割 | COCO 2017（118K 训练，5K 验证，80 类） | APb、APm |

### 对比方法
- **CNN**：ConvNeXt、MambaOut、RepLKNet 等
- **Transformer**：Swin、PVTv2、CSwin、Focal、DeiT、NAT、CoAtNet 等
- **Mamba**：VMamba、LocalVMamba、Vim、PlainMamba、EfficientVMamba 等
- **SBM**：SBM-T、SBM-S、SBM-B（分别与 Swin-T/S/B 参数量相当）

### 基准设置
- ImageNet：训练 300 epoch，AdamW，cosine decay，30 epoch warm-up，8×A800 GPU。
- 分割：UperNet 框架，基于 mmsegmentation，单尺度测试。
- 检测：Mask R-CNN，1× 训练计划。

## 4. 资源与算力

- **明确说明**：ImageNet 分类实验使用 **8 张 NVIDIA A800 GPU**，训练 300 epoch，使用 AdamW 优化器、余弦学习率衰减和 30 epoch 预热。
- **未明确说明**：总训练时间未给出；其他任务（CIFAR-100、ADE20K、COCO）所使用的 GPU 数量、时长未提及，仅说明采用 mmsegmentation 标准框架。但考虑到实验规模，通常也在类似或更少的 GPU 上进行。

## 5. 实验数量与充分性

- **主要实验**：4 个任务（分类×2、分割、检测），共约 10 个结果表（表 1-4）。
- **消融实验**：
  - **表 5**：对 SBM 模块的三个分支（双线性映射、线性映射、映射控制）分别进行消融，每个分支移除后观察 Top-1 下降（↓0.9、↓1.6、↓0.6）。
  - **表 6**：对 SBM 块中的组件（绝对位置编码、条件位置编码、层归一化、shortcut）进行消融，其中移除所有 LN 导致性能暴跌至 73.4%，凸显其关键性。
- **充分性评价**：实验设计较为全面，覆盖主流视觉任务和多种模型尺度，与大量 SOTA 方法公平对比（使用相同框架和调度）。消融实验直接验证各组件贡献，逻辑清晰。但**缺少对超参数 m（低秩维度）的敏感度分析**，也**未在更高分辨率（如 ImageNet-21K）或大模型预训练场景下评估**，存在一定局限性。

## 6. 主要结论与发现

1. **线性复杂度有效**：SBM 在保持线性复杂度的前提下，在 ImageNet-1K 上达到 83.2%（SBM-T）、84.4%（SBM-S）、85.1%（SBM-B）的 Top-1 准确率，全面超越同量级的 Swin、ConvNeXt、VMamba 等方法。
2. **跨任务泛化能力强**：在 ADE20K 语义分割上，SBM-B 达到 50.2% mIoU，优于 ConvNeXt-B（49.1）、MambaOut-B（49.6）；在 COCO 目标检测上，SBM-B 的 APb 为 50.2，高于 VMamba-B 的 48.5。
3. **通过星操作与低秩双线性映射结合**，能够高效捕获全局依赖，且星操作堆叠带来的隐式高维特征空间是性能提升的关键。
4. **层归一化不可或缺**：移除 LN 后准确率陡降 9.8%，说明训练稳定性高度依赖归一化。
5. **映射控制分支提供额外性能增益**（↑0.6%），但会轻微降低吞吐量。

## 7. 优点

- **方法创新**：首次将双线性映射与星操作结合用于视觉骨干网络，利用低秩分解实现线性复杂度，思路新颖且理论支撑清晰（隐式特征维度指数增长）。
- **效率突出**：在分类任务中，SBM 不仅 FLOPs 与主流模型相当，实际吞吐量（图 4）也高于多数 Transformer 和 Mamba 变体，无需定制 CUDA 内核。
- **实验全面**：覆盖分类、分割、检测三大主流任务，对比方法涵盖 CNN、Transformer、Mamba 三类，消融实验直接验证每个设计决策。
- **开源可用**：代码已公开，便于复现与后续研究。

## 8. 不足与局限

- **低秩维度 m 的敏感性未探索**：论文固定 m=64，未讨论其对不同任务或分辨率的影响，这可能限制架构的通用性。
- **训练规模有限**：仅在 ImageNet-1K 上训练，未在更大规模数据集（如 ImageNet-21K、JFT）上预训练，也未验证在视频或多模态任务上的表现。
- **与线性注意力的直接对比缺失**：虽然与 Mamba 和标准 Transformer 对比，但未与 Linear Attention、Linformer 等纯线性注意力方法比较，难以完全说明 SBM 在同类方法中的竞争优势。
- **超参数依赖**：控制分支引入额外超参数（通道数等），虽然消融显示增益不大，但可能增加调参成本。
- **推理吞吐量**：虽然整体效率高，但 SBM-B 的 FLOPs（18.2G）略高于同参数量模型（如 Swin-B 15.4G），在计算预算严格受限时可能不如轻量方法。

（完）
