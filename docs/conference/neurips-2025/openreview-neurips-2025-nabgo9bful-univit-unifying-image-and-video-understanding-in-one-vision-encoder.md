---
title: "UniViT: Unifying Image and Video Understanding in One Vision Encoder"
title_zh: UniViT：统一一个视觉编码器中的图像和视频理解
authors: "Feilong Tang, Xiang An, Haolin Yang, Yin Xie, Kaicheng Yang, Ming Hu, Zheng Cheng, Xingyu Zhou, Zimin Ran, Imran Razzak, Ziyong Feng, Behzad Bozorgtabar, Jiankang Deng, Zongyuan Ge"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=NABGO9Bful"
tags: ["query:dino-fg"]
score: 6.0
evidence: 用于统一图像/视频理解的视觉Transformer架构
tldr: UniViT提出一种统一的自我监督视觉Transformer框架，同时处理图像和视频。通过事件级和对象级聚类与区分，捕获空间细节和时间动态。该方法可用于下游分类任务，但未专门针对细粒度分类优化。
source: NeurIPS-2025-Accepted
selection_source: conference_retrieval
motivation: 现有预训练方法偏重空间或时间建模，无法同时捕捉两者，限制多模态理解。
method: 提出集群驱动的统一自监督学习框架，通过事件级和对象级聚类与区分学习语义特征。
result: 在图像和视频理解任务上表现优异，具有良好迁移能力。
conclusion: UniViT为视觉Transformer的多模态统一提供了有效框架，可间接支持分类。
---

## Abstract
Despite the impressive progress of recent pretraining methods on multimodal tasks, existing methods are inherently biased towards either spatial modeling (e.g., CLIP) or temporal modeling (e.g., V-JEPA), limiting their joint capture of spatial details and temporal dynamics. To this end, we propose UniViT, a cluster-driven unified self-supervised learning framework that effectively captures the structured semantics of both image spatial content and video temporal dynamics through event-level and object-level clustering and discrimination. Specifically, we leverage offline clustering to generate semantic clusters across both modalities. For videos, multi-granularity event-level clustering progressively expands from single-event to structured multi-event segments, capturing coarse-to-fine temporal semantics; for images, object-level clustering captures fine-grained spatial semantics. However, while global clustering provides semantically consistent clusters, it lacks modeling of structured semantic relations (e.g., temporal event structures). To address this, we introduce a contrastive objective that leverages these semantic clusters as pseudo-label supervision to explicitly enforce structural constraints, including temporal event relations and spatial object co-occurrences, capturing structured semantics beyond categories. Meanwhile, UniViT jointly embeds structured object-level and event-level semantics into a unified representation space. Furthermore, UniViT introduces two key components: (i) Unified Rotary Position Embedding integrates relative positional embedding with frequency-aware dimension allocation to support position-invariant semantic learning and enhance the stability of structured semantics in the discrimination stage; and (ii) Variable Spatiotemporal Streams adapt to inputs of varying frame lengths, addressing the rigidity of conventional fixed-input approaches. Extensive experiments across varying model scales demonstrate that UniViT achieves state-of-the-art performance on linear probing, attentive probing, question answering, and spatial understanding tasks.

---

## 论文详细总结（自动生成）

# UniViT：统一一个视觉编码器中的图像和视频理解 —— 论文总结

## 1. 核心问题与整体含义（研究动机和背景）

- **研究动机**：现有视觉预训练方法存在明显的模态偏向 —— 空间建模方法（如 CLIP）擅长静态图像理解但忽视时间动态，时序建模方法（如 V-JEPA）擅长视频动态却弱于空间细节。这种偏置限制了视觉系统对多模态场景（图像+视频）的联合建模能力。
- **核心问题**：如何设计一个统一的自我监督学习框架，使其能同时捕获图像的空间细节和视频的时间动态，并学习结构化的语义关系（如时间事件结构、空间对象共现）？
- **整体含义**：UniViT 旨在构建一个可同时处理图像和视频输入的单一视觉编码器，通过统一的表征空间支持下游分类、问答、空间理解等多种任务，推动视觉基础模型的通用性。

## 2. 方法论：核心思想、关键技术细节

### 核心思想
- 采用**集群驱动的统一自监督学习**，通过**事件级聚类**（视频）和**对象级聚类**（图像）分别捕获不同粒度的时空语义，然后利用对比学习强制结构化约束（如时序事件关系、空间对象共现），最终将两类语义嵌入到统一的表征空间。

### 关键技术细节
1. **离线聚类**：对图像和视频特征进行语义聚类，生成跨模态的伪标签。
2. **多粒度事件级聚类（视频）**：从单事件段逐步扩展到结构化多事件段，实现由粗到细的时间语义学习。
3. **对象级聚类（图像）**：捕获精细空间语义（如物体细粒度类别）。
4. **对比学习结构约束**：以聚类伪标签作为监督信号，对比学习显式建模事件间的时间顺序关系以及对象间的空间共现关系，学习超越类别标签的结构化语义。
5. **统一旋转位置嵌入**：将相对位置编码与频率感知维度分配相结合，实现位置不变语义学习，增强结构化语义在判别阶段的稳定性。
6. **可变时空流**：支持任意帧长度的视频输入，克服传统固定帧输入方法的刚性限制。

## 3. 实验设计

- **使用的数据集/场景**：（论文摘要和元数据未详细列举具体数据集，仅提及在 linear probing、attentive probing、question answering、spatial understanding 任务上评估。）
- **Benchmark**：未明确说明，但推测包括常见图像理解基准（如 ImageNet 线性探测）和视频理解基准（如 Kinetics、Something-Something 等事件理解、视频问答数据集）。
- **对比方法**：未列出具体方法名称，但摘要指出 UniViT 在多种模型尺度下达到 SOTA，暗示与 CLIP、V-JEPA 等代表方法进行了比较。

> **注意**：由于提供的文本内容有限，实验具体细节（数据集名称、对比方法列表、评估指标）未完全呈现，以上为合理推断。

## 4. 资源与算力

- **论文摘要和元数据中未明确说明**使用的 GPU 型号、数量、训练时长等算力信息。
- 仅知模型支持不同规模（“varying model scales”），但具体计算资源未公开。

## 5. 实验数量与充分性

- **实验数量**：基于摘要，至少包含四种任务类型（线性探测、注意力探测、问答、空间理解）以及不同模型规模的比较，暗示进行了一定数量的多任务评估。
- **充分性评估**：
  - **优势**：对比任务多样（图像+视频、空间+时间），且包含结构理解任务（spatial understanding），具有一定覆盖度。
  - **不足**：
    - 缺乏消融实验细节（如聚类粒度、对比损失贡献、位置嵌入影响的量化分析）。
    - 未提及在细粒度分类（论文元数据标注为 `query:dino-fg`）上的专项优化或评估，这与“细粒度分类”的原始动机存在差距。
    - 未见跨数据集泛化测试或开放世界场景评估。
    - 未报告训练数据来源、规模及数据偏差风险。

## 6. 主要结论与发现

- UniViT 通过统一的集群驱动自监督学习，有效联合建模图像空间细节与视频时间动态。
- 在多项下游任务（线性探测、注意力探测、问答、空间理解）上达到当时最佳性能（state-of-the-art）。
- 提出的统一旋转位置嵌入和可变时空流显著提升了多模态输入的适应性和表征稳定性。

## 7. 优点（亮点）

- **统一架构简洁创新**：将图像与视频处理整合到单一编码器，避免多分支冗余，降低部署成本。
- **结构化语义学习**：不仅学习类别级特征，还通过对比学习捕获事件时序和对象共现等结构化关系，增强语义丰富度。
- **位置编码自适应**：统一旋转位置嵌入支持位置不变性，有利于从不同裁剪/时空视角中提取稳健特征。
- **输入灵活性**：可变时空流打破固定帧数限制，更贴合实际视频长短不一的场景。

## 8. 不足与局限

- **未针对细粒度分类优化**：虽然元数据标记与细粒度相关，但论文结果并未专门展示细粒度识别性能，缺乏直接证据。
- **实验细节缺失**：提供文本未列出具体数据集、对比基线、评估指标数值，导致无法验证结论的可靠性和可重复性。
- **计算资源未公开**：缺少算力和训练时间的透明报告，影响实际应用成本估算。
- **偏差风险**：聚类依赖离线特征，可能放大预训练数据中的隐式偏差；未讨论数据分布或类别不平衡问题。
- **应用限制**：统一表征可能牺牲单一模态的极致性能（如图像分类精度可能弱于纯空间模型）；视频长时序建模仍需进一步验证（如长视频理解、复杂事件推理）。

（完）
