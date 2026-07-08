---
title: "MambaVision: A Hybrid Mamba-Transformer Vision Backbone"
title_zh: MambaVision：一种混合Mamba-Transformer视觉骨干
authors: "Hatamizadeh, Ali, Kautz, Jan"
date: 2025-06-01
pdf: "https://openaccess.thecvf.com/content/CVPR2025/papers/Hatamizadeh_MambaVision_A_Hybrid_Mamba-Transformer_Vision_Backbone_CVPR_2025_paper.pdf"
tags: ["query:dino-fg"]
score: 6.0
evidence: 混合Mamba-Transformer视觉骨干用于分类
tldr: 针对纯Mamba模型在视觉任务中长程依赖建模不足的问题，本文提出混合Mamba-Transformer架构MambaVision。通过在最后层引入自注意力，显著提升了对长程空间依赖的捕捉能力。在ImageNet-1K分类上，MambaVision系列模型取得了最先进的结果，为视觉Transformer架构设计提供了新思路。
source: CVPR-2025-Accepted
selection_source: conference_retrieval
motivation: 现有Mamba架构难以有效建模视觉特征中的长程依赖性。
method: 重新设计Mamba公式，并在最后几层结合自注意力机制形成混合架构。
result: MambaVision在ImageNet-1K分类上达到最先进水平。
conclusion: 证明Mamba与Transformer的有效集成可以提升视觉骨干性能。
---

## Abstract
We propose a novel hybrid Mamba-Transformer backbone, MambaVision, specifically tailored for vision applications. Our core contribution includes redesigning the Mamba formulation to enhance its capability for efficient modeling of visual features. Through a comprehensive ablation study, we demonstrate the feasibility of integrating Vision Transformers (ViT) with Mamba. Our results show that equipping the Mamba architecture with self-attention blocks in the final layers greatly improves its capacity to capture long-range spatial dependencies. Based on these findings, we introduce a family of MambaVision models with a hierarchical architecture to meet various design criteria. For classification on the ImageNet-1K dataset, MambaVision variants achieve state-of-the-art (SOTA) performance in terms of both Top-1 accuracy and throughput. In downstream tasks such as object detection, instance segmentation, and semantic segmentation on MS COCO and ADE20K datasets, MambaVision outperforms comparably sized backbones while demonstrating favorable performance. Code: https://github.com/NVlabs/MambaVision

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 核心问题与整体含义（研究动机和背景）

- **问题**：Mamba（状态空间模型）在语言任务中实现了线性复杂度，但直接应用于视觉任务存在局限：
  - 因果卷积和自回归式处理不适合图像的空间并行性；
  - 单向或双向SSM难以高效捕获全局上下文和长程空间依赖；
  - 已有的纯Mamba视觉模型（如Vim、VMamba）在精度和吞吐量上仍落后于CNN与ViT。
- **目标**：设计一种**混合Mamba-Transformer**视觉骨干，兼顾Mamba的线性效率和Transformer的全局感受野，实现精度-吞吐量的新Pareto前沿。

## 2. 方法论：核心思想与关键技术细节

### 2.1 宏观架构（Hierarchical Architecture）
- 4个阶段，采用**多分辨率**设计：
  - **Stem**：两个步长为2的3×3卷积，将输入降采样至H/4 × W/4。
  - **Stage 1 & 2**：仅使用**残差卷积块**（GELU + BN + 3×3卷积），实现高分辨率下快速特征提取。
  - **Stage 3 & 4**：混合使用**MambaVision Mixer**和**Transformer（自注意力）块**。
    - 对于N层，前N/2层用MambaVision Mixer + MLP，后N/2层用自注意力 + MLP。
- 阶段间用3×3步长为2的卷积下采样。

### 2.2 微观设计：MambaVision Mixer（核心创新）
- **改造Mamba原始块**：
  - 将**因果卷积**替换为**常规1D卷积**（组卷积，kernel=3），避免单向限制。
  - 增加**对称分支**（不含SSM）：对输入线性投影后同样经过SiLU + 1D卷积，补偿SSM的序列约束带来的信息损失。
  - 两个分支（SSM分支和对称分支）输出分别投影到C/2通道，然后**拼接**，再经过线性层输出。
- **数学表达**：  
  X1 = Scan( σ( Conv( Linear_{C→C/2}(Xin) ) ) )  
  X2 = σ( Conv( Linear_{C→C/2}(Xin) ) )  
  Xout = Linear_{C/2→C}( Concat(X1, X2) )
- 自注意力采用标准多头注意力，支持窗口模式（stage 3窗口14，stage 4窗口7）。

## 3. 实验设计

- **分类**：ImageNet-1K（300 epoch，32×A100）。
- **检测/实例分割**：MS COCO，使用Cascade Mask R-CNN head，3×学习率调度，输入分辨率1280×800。
- **语义分割**：ADE20K，使用UperNet head，512×512分辨率。
- **对比方法**：涵盖CNN（ConvNeXt, RegNetY, EfficientNetV2）、Transformer（Swin, SwinV2, DeiT, Twins）、Conv-Transformer混合（NextViT, FasterViT, MaxViT）、Mamba-based（Vim, VMamba, EfficientVMamba, SiMBA）。此外有**ImageNet-21K大规模预训练**实验（MambaVision-B/L/L2/L3）。

## 4. 资源与算力

- **ImageNet-1K训练**：明确使用 **32块A100 GPU**，训练 **300个epoch**。
- **检测分割**：未明确说明，但提及使用8块A100 GPU进行语义分割实验。
- **ImageNet-21K预训练**：文中提到“第一个在ImageNet-21K上缩放训练的Mamba方法”，但未给出具体GPU数量和时间。

## 5. 实验数量与充分性

- **分类**：比较了超过20种backbone，覆盖各家族，并报告了FLOPs、参数量、吞吐量。
- **下游任务**：COCO上的检测/分割（表2）、ADE20K语义分割（表3），均与Swin、ConvNeXt、Focal等对比。
- **消融实验**：
  - Token mixer设计（表4）：因果卷积→普通卷积→加对称分支→拼接，共4组，在ImageNet、COCO、ADE20K上均报告。
  - 混合模式消融（表5）：随机、前N/2层attention、交替、最后N/4、最后N/2等5种模式，仅ImageNet Top-1。
- **大规模预训练**：展示4种模型在224/256/512分辨率下的精度曲线（图4）。
- **可视化**：注意力图展示模型关注语义区域。
- **充分性评价**：实验覆盖主要视觉任务，消融设计系统且公平（保持模型大小一致），对比方法全面。但缺少在其他数据集（如视频、医学图像）上的验证，且COCO检测仅使用了Cascade Mask R-CNN一种head，未测试其他检测器（如RetinaNet、DINO等）。

## 6. 主要结论与发现

- MambaVision在ImageNet-1K上达到**新SOTA Pareto前沿**，例如MambaVision-B（84.2% Top-1）优于ConvNeXt-B（83.8%）和Swin-B（83.5%），且吞吐量更高。
- 与纯Mamba模型相比，MambaVision-T（82.3%）远超VMamba-T（82.6%）但吞吐量高5倍；MambaVision-B（84.2%）也高于VMamba-B（83.9%）。
- 在下游任务上，MambaVision全面超越同等规模Swin和ConvNeXt（例如MambaVision-T在ADE20K上mIoU 46.0% vs Swin-T 44.5%）。
- 混合模式**最后N/2层使用自注意力**效果最佳，证明在深层放置自注意力可有效捕捉长程依赖。
- MambaVision可成功扩展到ImageNet-21K，MambaVision-L3在512分辨率上达到88.1% Top-1。

## 7. 优点

- **方法创新**：首次系统研究Mamba-Transformer混合视觉骨干，既保留Mamba的线性效率，又通过后段自注意力弥补全局感知缺陷。
- **工程优化**：MambaVision Mixer设计简洁（对称分支+拼接），比Vim/VMamba的复杂双向扫描方案更快、更准。
- **效率突出**：在相近精度下，吞吐量显著优于ViT和纯Mamba模型（如MambaVision-T吞吐量6298 img/s，是VMamba-T的4.9倍）。
- **实验充分**：报告了包括大尺度预训练在内的多任务、多角度对比，消融实验逻辑清晰，验证了每个设计选择的有效性。

## 8. 不足与局限

- **任务覆盖有限**：仅测试了分类、检测、分割，未涉及视频理解、点云、或多模态任务。
- **检测框架单一**：仅使用Cascade Mask R-CNN，未尝试更高效的检测头（如DINO、RT-DETR），可能隐藏泛化能力差异。
- **超参数敏感性**：窗口大小、混合比例等超参数仅在默认值下测试，缺少更宽范围搜索（如不同分辨率下的窗口配置）。
- **大规模预训练细节缺失**：ImageNet-21K训练配置、epoch数、学习率等未详细给出，复现难度较大。
- **理论分析不足**：为何最后N/2层放自注意力最优，缺乏理论解释或更深入的特征可视化分析（如消融中间层作用）。
- **公平性**：与部分方法（如MaxViT）相比，FLOPs虽低但参数量更大（MambaVision-B 97.7M vs ConvNeXt-B 88.6M），比较时未进行参数匹配。

（完）
