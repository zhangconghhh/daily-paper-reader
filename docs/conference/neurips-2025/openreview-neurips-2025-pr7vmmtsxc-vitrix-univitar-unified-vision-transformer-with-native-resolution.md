---
title: "VITRIX-UniViTAR: Unified Vision Transformer with Native Resolution"
title_zh: UniViTAR：统一原生分辨率视觉Transformer
authors: "Limeng Qiao, Yiyang Gan, Bairui Wang, Jie Qin, Shuang Xu, Siqi Yang, Lin Ma"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=PR7VmMTsxC"
tags: ["query:dino-fg"]
score: 6.0
evidence: 提出统一的视觉Transformer架构以支持原生分辨率
tldr: 标准ViT假设统一输入分辨率，忽视了自然图像的尺度多样性。本文提出UniViTAR，通过整合多个先进组件对ViT架构进行升级，形成适用于原生分辨率的同质视觉基础模型家族。该架构在多种视觉任务上展现竞争力，为分类等下游应用提供了更灵活的表示基础。
source: NeurIPS-2025-Accepted
selection_source: conference_retrieval
motivation: 克服统一分辨率ViT对自然图像可变性的低估。
method: 对ViT架构进行多项升级，集成先进组件以适应原生分辨率输入。
result: 在多种视觉任务上达到有竞争力的性能，支持灵活分辨率。
conclusion: UniViTAR作为同质基础模型，提升了视觉表示的适应性。
---

## Abstract
Conventional Vision Transformer streamlines visual modeling by employing a uniform input resolution, which underestimates the inherent variability of natural visual data and incurs a cost in spatial-contextual fidelity. While preliminary explorations have superficially investigated native resolution modeling, existing works still lack systematic training recipe from the visual representation perspective. To bridge this gap, we introduce Unified Vision Transformer with Native Resolution, i.e. UniViTAR, a family of homogeneous vision foundation models tailored for unified visual modality and native resolution scenario in the era of multimodal. Our framework first conducts architectural upgrades to the vanilla paradigm by integrating multiple advanced components. Building upon these improvements, a progressive training paradigm is introduced, which strategically combines two core mechanisms: (1) resolution curriculum learning, transitioning from fixed-resolution pretraining to native resolution tuning, thereby leveraging ViT’s inherent adaptability to variable-length sequences, and (2) visual modality adaptation via inter-batch image-video switching, which balances computational efficiency with enhanced temporal reasoning. In parallel, a hybrid training framework further synergizes sigmoid-based contrastive loss with feature distillation from a frozen teacher model, thereby accelerating early-stage convergence. Finally, trained exclusively on public accessible image-caption data, our UniViTAR family across multiple model scales from 0.3B to 1B achieves state-of-the-art performance on a wide variety of visual-related tasks. The code and models are available here.

---

## 论文详细总结（自动生成）

# 论文中文总结：UniViTAR：统一原生分辨率视觉Transformer

## 1. 核心问题与研究动机
- **问题**：传统视觉Transformer（ViT）采用统一的输入分辨率，这忽略了自然视觉数据固有的尺度可变性，导致空间-上下文保真度损失。
- **背景**：已有工作对原生分辨率建模进行了初步探索，但缺乏从视觉表示角度出发的系统性训练方案。
- **动机**：设计一个面向多模态时代的、统一视觉模态且支持原生分辨率的同质视觉基础模型家族。

## 2. 方法论
- **核心思想**：在标准ViT基础上进行架构升级，并结合渐进式训练范式，使模型能够原生地处理任意分辨率的输入。
- **关键技术细节**：
  - **架构升级**：集成多种先进组件（如可变长度序列适应组件），实现对原生分辨率的原生支持。
  - **分辨率课程学习**：从固定分辨率预训练过渡到原生分辨率微调，利用ViT对可变长度序列的固有能力。
  - **视觉模态适应**：通过批次间图像-视频切换，平衡计算效率与时间推理能力。
  - **混合训练框架**：结合基于sigmoid的对比损失与来自冻结教师模型的特征蒸馏，加速早期收敛。
- **算法流程（文字说明）**：
  1. 使用公共图像-文本数据，以固定分辨率进行预训练。
  2. 引入分辨率课程学习，逐步将输入从固定大小过渡到原生（任意）分辨率。
  3. 在训练中交替使用图像批次和视频批次，使模型同时学习空间与时间特征。
  4. 使用sigmoid对比损失（而非softmax）进行图文对齐，同时利用一个冻结的教师模型进行特征蒸馏，稳定早期训练。
- **公式**：文中未给出具体数学公式。

## 3. 实验设计
- **训练数据**：仅使用公开可访问的图像-文本数据（具体数据集名称未明确说明）。
- **Benchmark**：多种视觉相关任务（分类、检测等）的基准测试。
- **对比方法**：未列出具体对比模型，但声称在多个模型尺度（0.3B到1B参数）上达到state-of-the-art性能。

## 4. 资源与算力
- **文中未明确提及**使用的GPU型号、数量或训练时长。仅说明模型规模从0.3B到1B参数，并使用了公开数据。

## 5. 实验数量与充分性
- **实验数量**：文中仅概括性地提到“在多种视觉相关任务上达到最优”，未列出具体实验数量或消融实验细节。
- **充分性评估**：缺乏详细的消融研究、跨数据集对比、以及不同分辨率设置下的分析。由于信息有限，无法充分判断实验的客观性和公平性。

## 6. 主要结论与发现
- 提出UniViTAR家族（0.3B~1B），通过架构升级与渐进式训练，在多种视觉任务上达到SOTA性能。
- 该方法有效利用了ViT对可变分辨率的天生适应性，为下游任务提供更灵活的表示基础。

## 7. 优点
- **架构统一**：在同质框架下同时支持图像和视频输入，适应多模态场景。
- **分辨率自适应**：课程学习策略使模型平滑过渡到原生分辨率，避免训练不稳定。
- **训练效率**：混合训练（对比损失+蒸馏）加速收敛，且仅使用公开数据。
- **可扩展性**：提供多个模型规模，兼顾算力与性能需求。

## 8. 不足与局限
- **实验细节缺失**：未提供具体数据集、对比方法、消融实验，难以验证方法的普适性。
- **算力未说明**：缺乏计算资源消耗信息，可复现性受限。
- **应用限制**：仅基于图像-文本数据训练，未涉及纯视频或多模态联合训练场景的详细评估。
- **潜在偏差**：文中提到“初步探索缺乏系统性”，但自身也未提供详尽的系统分析，可能存在选择偏倚。

（完）
