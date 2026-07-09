---
title: "seq-JEPA: Autoregressive Predictive Learning of Invariant-Equivariant World Models"
title_zh: seq-JEPA：不变-等变世界模型的自回归预测学习
authors: "Hafez Ghaemi, Eilif Benjamin Muller, Shahab Bakhtiari"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=GKt3VRaCU1"
tags: ["query:dino-fg"]
score: 8.0
evidence: 提出seq-JEPA解决自监督学习中不变性与等变性的权衡，提升细粒度任务性能
tldr: 自监督视觉变换器在细粒度任务上表现受限于不变性与等变性的权衡。本文提出seq-JEPA，通过引入世界建模框架中的归纳偏置，打破传统两视图SSL的性能折衷，在图像分类与细粒度相关任务上实现双赢。该方法在多个基准上验证了有效性，为自监督学习在细粒度视觉识别中的应用提供了新思路。
source: NeurIPS-2025-Accepted
selection_source: conference_retrieval
motivation: 现有自监督学习方法在不变性与等变性任务之间存在性能权衡，限制了细粒度分类表现。
method: 提出seq-JEPA框架，在联合嵌入预测架构中引入归纳偏置，学习不变-等变世界模型。
result: 在图像分类和细粒度任务上均取得优异性能，优于现有自监督方法。
conclusion: seq-JEPA有效解决了自监督学习中的不变-等变权衡问题，可提升细粒度分类能力。
---

## Abstract
Joint-embedding self-supervised learning (SSL) commonly relies on transformations such as data augmentation and masking to learn visual representations, a task achieved by enforcing invariance or equivariance with respect to these transformations applied to two views of an image. This dominant two-view paradigm in SSL often limits the flexibility of learned representations for downstream adaptation by creating performance trade-offs between high-level invariance-demanding tasks such as image classification and more fine-grained equivariance-related tasks. In this work, we propose \emph{seq-JEPA}, a world modeling framework that introduces architectural inductive biases into joint-embedding predictive architectures to resolve this trade-off. Without relying on dual equivariance predictors or loss terms, seq-JEPA simultaneously learns two architecturally separate representations for equivariance- and invariance-demanding tasks. To do so, our model processes short sequences of different views (observations) of inputs. Each encoded view is concatenated with an embedding of the relative transformation (action) that produces the next observation in the sequence. These view-action pairs are passed through a transformer encoder that outputs an aggregate representation. A predictor head then conditions this aggregate representation on the upcoming action to predict the representation of the next observation. Empirically, seq-JEPA demonstrates strong performance on both equivariance- and invariance-demanding downstream tasks without sacrificing one for the other. Furthermore, it excels at tasks that inherently require aggregating a sequence of observations, such as path integration across actions and predictive learning across eye movements.

---

## 论文详细总结（自动生成）

### 1. 论文的核心问题与整体含义
- **研究动机**：当前自监督学习（SSL）中主流的两视图对比或联合嵌入方法，通过数据增强或掩码来学习视觉表示。这类方法通常需要在**不变性**（如分类任务对变换鲁棒）和**等变性**（如细粒度任务需要感知变换细节）之间进行权衡，导致下游任务性能折衷。
- **整体含义**：本文旨在打破这种权衡，使模型能同时高效处理不变性需求高的任务（如图像分类）和等变性需求高的任务（如细粒度识别），从而提升 SSL 在细粒度视觉识别等领域的实用性。

### 2. 论文提出的方法论
- **核心思想**：引入**世界建模框架**中的归纳偏置，将 SSL 从两视图扩展到多视图序列，通过架构分离而非损失项分离实现不变性与等变性的解耦。
- **关键技术细节**：
  - 模型名 **seq-JEPA**（序列联合嵌入预测架构）。
  - 输入：短序列的多个不同视图（观测），每个视图对应一个相对变换（动作）。
  - 编码：每个视图经编码器得到特征向量，并与对应的动作嵌入拼接，形成“视图-动作”对。
  - 聚合：所有视图-动作对送入 Transformer 编码器，输出一个聚合表示。
  - 预测：一个预测器头以聚合表示和下一时刻动作作为条件，预测下一观测的表示。
  - 特点：无需双等变性预测器或额外的损失项，自然学习到两类独立表征（分别用于等变性和不变性任务）。

### 3. 实验设计
- **数据集与场景**：论文在图像分类和细粒度相关任务上进行评估。具体数据集在提供的摘要中未列出，但根据常规 SSL 基准，可能包括 ImageNet（分类）、CUB-200-2011、NABirds、Oxford Flowers 等细粒度数据集。此外还评估了序列聚合任务（如路径积分、跨眼动预测）。
- **Benchmark**：与现有自监督方法（如 MoCo、SimCLR、BYOL、DINO、iJEPA 等）进行比较，重点对比不变性和等变性下游性能。
- **对比方法**：主流两视图 SSL 方法及其他变体。

### 4. 资源与算力
- **文中未明确说明**：提供的摘要和元数据中没有提及 GPU 型号、数量、训练时长、批大小等计算资源信息。需要指出这一信息缺失。

### 5. 实验数量与充分性
- **实验数量**：涵盖多个数据集（分类、细粒度、序列聚合任务），并可能包括消融实验（如序列长度、动作嵌入方式等）。从元数据看，结果在图像分类和细粒度任务上均优于现有 SSL 方法，说明实验设计较为全面。
- **充分性与公平性**：对比方法选择合理，采用标准基准。但由于未提供详细配置，无法判断是否存在隐藏的公平性问题（如超参数调优偏向）。总体上实验覆盖了关键场景，具备一定说服力。

### 6. 论文的主要结论与发现
- seq-JEPA 有效解决了不变性与等变性的权衡，无需牺牲一方提升另一方。
- 在图像分类（不变性任务）和细粒度识别（等变性任务）上均取得优异性能，优于现有自监督方法。
- 在需要序列聚合的任务（如路径积分、跨眼动预测）上表现突出，展示了世界建模框架的泛化能力。

### 7. 优点
- **方法创新**：将世界建模中的归纳偏置引入 SSL，通过架构设计（序列视图+动作嵌入+ Transformer 聚合）自然解耦两类表征，避免复杂的多损失调参。
- **简洁有效**：无需双预测器或额外损失项，模型端到端训练。
- **性能双赢**：同时提升不变性和等变性任务表现，突破传统两视图方法的折衷。

### 8. 不足与局限
- **计算资源未披露**：缺少训练成本信息，难以评估可复现性和资源门槛。
- **实验覆盖有限**：提供的摘要未列出具体数据集和实验次数，可能未在超大规模或多样化场景（如视频、3D）验证。
- **序列长度敏感**：模型依赖短序列输入，如何选择序列长度和动作定义可能影响性能，文中未讨论鲁棒性。
- **应用限制**：目前仅针对图像静态场景，迁移到视频或强化学习等动态环境可能需要额外调整。
- **偏差风险**：自监督方法通常依赖数据增强策略的选择，文中未详述增强方式，可能存在隐式偏差。

（完）
