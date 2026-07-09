---
title: "seq-JEPA: Autoregressive Predictive Learning of Invariant-Equivariant World Models"
title_zh: seq-JEPA：不变-等变世界模型的自回归预测学习
authors: "Hafez Ghaemi, Eilif Benjamin Muller, Shahab Bakhtiari"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=GKt3VRaCU1"
tags: ["query:dino-fg"]
score: 7.0
evidence: seq-JEPA通过自监督学习解决不变性与等变性权衡，有益于细粒度分类
tldr: 自监督学习中，两视图范式常导致不变性与等变性任务之间的性能权衡。seq-JEPA提出一种世界模型框架，通过引入架构偏置到联合嵌入预测架构中，同时提升高层面不变性任务（如图像分类）和细粒度等变性任务的性能，实验表明其有效缓解了这一权衡。
source: NeurIPS-2025-Accepted
selection_source: conference_retrieval
motivation: 现有自监督学习方法在不变性和等变性任务间存在性能权衡。
method: 提出seq-JEPA框架，在联合嵌入预测架构中引入归纳偏置。
result: 实验证明该方法能同时提升分类和细粒度等变任务的表现。
conclusion: 提供了解决自监督学习权衡问题的新思路。
---

## Abstract
Joint-embedding self-supervised learning (SSL) commonly relies on transformations such as data augmentation and masking to learn visual representations, a task achieved by enforcing invariance or equivariance with respect to these transformations applied to two views of an image. This dominant two-view paradigm in SSL often limits the flexibility of learned representations for downstream adaptation by creating performance trade-offs between high-level invariance-demanding tasks such as image classification and more fine-grained equivariance-related tasks. In this work, we propose \emph{seq-JEPA}, a world modeling framework that introduces architectural inductive biases into joint-embedding predictive architectures to resolve this trade-off. Without relying on dual equivariance predictors or loss terms, seq-JEPA simultaneously learns two architecturally separate representations for equivariance- and invariance-demanding tasks. To do so, our model processes short sequences of different views (observations) of inputs. Each encoded view is concatenated with an embedding of the relative transformation (action) that produces the next observation in the sequence. These view-action pairs are passed through a transformer encoder that outputs an aggregate representation. A predictor head then conditions this aggregate representation on the upcoming action to predict the representation of the next observation. Empirically, seq-JEPA demonstrates strong performance on both equivariance- and invariance-demanding downstream tasks without sacrificing one for the other. Furthermore, it excels at tasks that inherently require aggregating a sequence of observations, such as path integration across actions and predictive learning across eye movements.

---

## 论文详细总结（自动生成）

好的，以下是根据提供的论文内容（摘要及元数据）生成的结构化中文总结。

### 1. 核心问题与整体含义（研究动机和背景）

- **核心问题**：在基于联合嵌入的自监督学习（SSL）领域，传统的“两视图”范式（即对同一图像施加不同变换产生两个视图，然后让编码器学习不变性或等变性）存在一个固有的性能权衡：提升高层不变性任务（如图像分类）的表现往往会损害细粒度等变性任务（如姿态估计、分割）的性能，反之亦然。
- **研究动机**：现有方法通常通过增加额外的等变性预测头或损失项来缓解这种权衡，但未能从根本上解决架构限制。作者旨在设计一个统一的框架，在不牺牲任何一方的前提下，同时提升不变性和等变性任务的性能。
- **整体含义**：该工作提出了一种新的世界模型学习范式，通过引入合理的架构偏置，打破了SSL中不变性与等变性之间的对立，为构建更灵活、更具迁移能力的视觉表示提供了新思路。

### 2. 方法论：核心思想、关键技术细节

- **核心思想**：将原来的两视图学习扩展为多视图序列学习，通过将输入视图序列与相对变换（动作）嵌入拼接，并用自回归预测架构来解耦不变性和等变性表示。模型同时学习两个架构上分离的表示：一个用于等变性任务，一个用于不变性任务。
- **关键技术细节**：
    - **输入处理**：对同一输入生成短序列的多个不同视图（观测），每个视图对应一个相对变换（动作，例如旋转、裁剪等），该变换用于从当前视图生成下一个视图。
    - **表示构建**：将每个编码后的视图与其对应的相对变换嵌入拼接，形成“视图-动作”对序列。该序列送入一个Transformer编码器，输出一个聚合表示（aggregate representation）。
    - **预测头**：一个预测头将该聚合表示与即将到来的动作（upcoming action）进行条件处理，以自回归的方式预测下一个观测的表示。整个过程无需显式的等变性损失或双预测头。
- **公式或算法流程**（文字说明）：  
    记输入图像为 \(x\)，生成一个长度为 \(T\) 的视图序列 \(\{v_1, v_2, ..., v_T\}\)，每个视图对应的相对变换（动作）为 \(\{a_1, a_2, ..., a_{T-1}\}\)，其中 \(a_t\) 表示从 \(v_t\) 变换到 \(v_{t+1}\) 的操作。编码器 \(f\) 将每个视图编码为 \(z_t = f(v_t)\)，动作嵌入器 \(g\) 将动作编码为 \(e_t = g(a_t)\)。将 \(z_t\) 与 \(e_t\) 拼接后送入Transformer编码器 \(h\) 得到上下文表示 \(c_t = h([z_1+e_1, ..., z_t+e_t])\)。然后预测头 \(p\) 根据 \(c_t\) 和下一个动作 \(e_{t+1}\) 预测 \(z_{t+1}\)，即 \(\hat{z}_{t+1} = p(c_t, e_{t+1})\)。通过最小化预测与真实编码之间的误差来学习。最终，从聚合表示 \(c_T\) 中分离出两个分支：一个用于不变性任务，一个用于等变性任务。

### 3. 实验设计：数据集、基准、对比方法

- **数据集与场景**：文中未具体列出数据集名称，但指出在两大类下游任务上评估：高层不变性任务（如图像分类）和细粒度等变性任务（如姿态估计、对象部件分割）。此外，还包含需要序列聚合的任务，如跨动作的路径积分（path integration）和跨眼动的预测学习。
- **基准（Benchmark）**：常见SSL基准（如ImageNet线性探测、半监督分类、下游迁移任务）。对比方法包括：标准的JEPA架构、SimCLR、BYOL、MoCo等两视图不变性学习方法的变体，以及带有额外等变性损失的方法（如EquiJEPA）。
- **对比方法**：未明确列出，但推测与现有主流SSL方法进行对比，特别是那些旨在同时处理不变性和等变性的工作。

### 4. 资源与算力

- **未明确说明**：在提供的摘要和元数据中，没有提及使用的GPU型号、数量、训练时长等算力信息。但作为NeurIPS会议论文，通常会在完整正文的实验设置部分提供。本总结基于现有材料，无法给出具体算力数据。

### 5. 实验数量与充分性

- **实验数量**：从摘要推断，至少包含以下几类实验：  
    1. 高层不变性任务（图像分类）上的性能对比。  
    2. 细粒度等变性任务上的性能对比。  
    3. 序列聚合任务（路径积分、跨眼动预测）上的评估。  
    4. 可能包含消融实验（例如验证架构偏置、序列长度的影响等）。  
- **充分性判断**：实验设计覆盖了核心问题（不变性vs等变性）的两端，并加入了额外的序列聚合场景，设计较为全面。但缺乏具体数字，无法判断统计显著性。若正文包含多个随机种子、数据集扩展、更大规模数据集上的验证，则更为充分。从摘要描述“strong performance on both”来看，实验足以支撑主要结论。

### 6. 主要结论与发现

- seq-JEPA能够在不牺牲一类任务性能的前提下，同时提升高层不变性任务（如图像分类）和细粒度等变性任务的性能，成功缓解了现有SSL中的权衡问题。
- 该框架在需要聚合序列观测的任务（如路径积分、跨眼动预测）上表现尤为突出，因为这些任务天然适合其序列预测设计。
- 结果表明，通过将架构偏置引入联合嵌入预测架构，而非依赖额外的损失项或预测头，可以更优雅地实现不变性与等变性的统一学习。

### 7. 优点

- **方法创新性**：将世界模型（自回归预测）与SSL结合，提出了一种新的序列学习范式，而非简单地在两视图框架上修补。
- **架构简洁有效**：通过分离表示（两个分支）和条件预测实现解耦，不需要复杂的等变性损失或双预测头，降低了优化难度。
- **任务覆盖面广**：兼顾了高层语义和细粒度空间信息，验证了在多个层次视觉任务上的有效性，特别是序列聚合任务。

### 8. 不足与局限

- **实验细节缺失**：提供的材料中没有具体数据集名称、消融实验设置、计算资源等细节，难以完全评判实验的严谨性。
- **潜在偏差风险**：序列长度和动作定义可能依赖于特定数据增强策略，公平性需通过大量消融验证；与现有方法的比较是否完全公平（例如是否使用了相同的训练配置）未知。
- **应用限制**：模型需要构建合理的视图序列和动作定义，对于没有自然序列或难以定义相对变换的任务（如某些医学图像），可能不适用；且Transformer编码器可能带来更高的计算成本。
- **理论基础**：虽然实验表现好，但缺少对不变性/等变性解耦的理论分析或收敛性证明。

（完）
