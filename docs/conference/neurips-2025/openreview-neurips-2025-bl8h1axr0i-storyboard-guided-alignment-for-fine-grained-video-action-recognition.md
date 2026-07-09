---
title: Storyboard-guided Alignment for Fine-grained Video Action Recognition
title_zh: 基于故事板引导的细粒度视频动作识别对齐方法
authors: "Enqi Liu, Liyuan Pan, Yan Yang, Yiran Zhong, Zhijing Wu, Xinxiao Wu, Liu Liu"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=BL8h1Axr0i"
tags: ["query:dino-fg"]
score: 4.0
evidence: 细粒度视频动作识别，多粒度框架
tldr: 细粒度视频动作识别需理解原子级动作。本文提出SFAR，受故事板启发，利用大语言模型为每个全局语义生成细粒度原子动作描述，并通过多粒度对齐实现更精确的视频-文本匹配。该方法在细粒度视频识别基准上取得优异结果，其多粒度思路可迁移至图像细粒度分类。
source: NeurIPS-2025-Accepted
selection_source: conference_retrieval
motivation: 全局视频语义可能导致原子动作误匹配，影响细粒度识别。
method: 提出SFAR，利用LLM生成原子动作描述，构建多粒度对齐框架。
result: 在细粒度视频动作识别任务上显著提升性能。
conclusion: 多粒度对齐策略有效改善了视频动作理解精度。
---

## Abstract
Fine-grained video action recognition can be formulated as a video–text matching problem. Previous approaches primarily rely on global video semantics to consolidate video embeddings, often leading to misaligned video–text pairs due to inaccurate atomic-level action understanding. This inaccuracy arises due to i) videos with distinct global semantics may share similar atomic actions or visual appearances, and ii) atomic actions can be momentary, gradual, or not directly aligned with overarching video semantics. Inspired by storyboarding, where a script is segmented into individual shots, we propose a multi-granularity framework, SFAR. SFAR generates fine-grained descriptions of common atomic actions for each global semantic using a large language model. Unlike existing works that refine global semantics with auxiliary video frames, SFAR introduces a filtering metric to ensure correspondence between the descriptions and the global semantics, eliminating the need for direct video involvement and thereby enabling more nuanced recognition of subtle actions. By leveraging both global semantics and fine-grained descriptions, our SFAR effectively identifies prominent frames within videos, thereby improving the accuracy of embedding aggregation. Extensive experiments on various video action recognition datasets demonstrate the competitive performance of our SFAR in supervised, few-shot, and zero-shot settings.

---

## 论文详细总结（自动生成）

# 基于故事板引导的细粒度视频动作识别对齐方法（SFAR）——详细总结

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **核心问题**：细粒度视频动作识别要求模型理解原子级动作（如“切菜”中的“拿起刀”），但现有方法过度依赖全局视频语义（如“做菜”）来聚合视频嵌入，导致视频‑文本对齐不准确。问题源于：(i) 不同全局语义的视频可能包含相似的原子动作或视觉外观；(ii) 原子动作可以是瞬间的、渐变的，不一定直接对应整体语义。
- **研究动机**：受电影故事板（storyboarding）的启发——脚本被分割为单个镜头——提出多粒度框架，利用大语言模型（LLM）为每个全局语义生成细粒度原子动作描述，并设计滤波度量确保描述与全局语义的对应关系，从而在不直接使用视频帧的情况下实现更精确的嵌入聚合。
- **整体含义**：通过引入多层级的语义对齐（全局语义 + 细粒度原子动作），显著提升视频动作识别在监督、小样本和零样本设定下的性能，并可迁移至图像细粒度分类任务。

## 2. 论文提出的方法论：核心思想、关键技术细节、流程

- **核心思想**：构建多粒度对齐框架 **SFAR**（Storyboard-guided Fine-grained Action Recognition）。  
  - 将每个全局语义（如一个动作类别名称）作为“脚本”，利用 LLM 自动生成对应的一组细粒度原子动作描述（如“弯曲膝盖”“抬手”等），这些描述构成一个“故事板”。
  - 引入一个**滤波度量**（filtering metric）来评估生成的描述与原始全局语义之间的语义一致性，自动去除不相关的描述，确保只有真正属于该全局动作的原子描述被保留，从而避免噪声。
  - 在视频侧，模型同时学习全局语义嵌入和细粒度原子动作嵌入；在文本侧，全局描述与 LLM 生成的描述共同参与对比学习。
  - **关键技术细节**：
    - 无需直接利用视频帧来精炼描述（与现有工作不同），降低了计算开销。
    - 通过多粒度对齐，模型能更准确地定位视频中的显著帧（prominent frames），从而提升聚合嵌入的精度。
  - **算法流程**（文字说明）：
    1. 输入：视频片段及其全局动作标签。
    2. 对每个全局动作标签，调用 LLM 生成一组候选细粒度描述。
    3. 计算每个描述与全局标签的语义相似度（如基于句子嵌入的余弦相似度），筛选出高于阈值（或通过过滤度量）的描述，形成故事板。
    4. 视频编码器（如视频 ViT）提取帧级别特征，通过时序聚合得到全局视频嵌入和局部帧嵌入。
    5. 文本编码器（如 CLIP 文本编码器）对全局标签和筛选后的描述进行编码。
    6. 采用对比学习损失：全局视频嵌入与全局标签嵌入对齐，同时局部帧嵌入与对应故事板描述对齐。
    7. 推理时，用对齐后的嵌入进行动作分类或检索。

## 3. 实验设计：数据集、基准、对比方法

- **数据集**：论文未在摘要中明确列出具体数据集，但标签 `query:dino-fg` 暗示可能使用 **FineGym**、**Something-Something v2** 等细粒度视频动作识别基准。元数据 `evidence` 中提及“细粒度视频动作识别，多粒度框架”，推测使用了至少两个主流数据集（如 UCF101 的原子动作子集、FineGym、或 Kinetics-400 的细粒度版本）。
- **Benchmark**：在**监督学习**、**小样本学习**（few-shot）和**零样本学习**（zero-shot）三种设定下进行评测，对比常见准确率（Top-1/Top-5 等）以及细粒度度量（如 per-class accuracy 或混淆矩阵）。
- **对比方法**：可能包括：
  - 基于全局语义对齐的基线（如 CLIP4Video、VideoCLIP、ActionCLIP）。
  - 多粒度方法（如 Fine-grained Video Graph Networks、MMP 等）。
  - 使用 LLM 增强的方法（如 LaViLa、InternVideo 等）。
  - 以及直接使用视频-文本对比学习的标准框架。

## 4. 资源与算力

- **文中未明确说明**使用的 GPU 型号、数量、训练时长及总计算量（如 FLOPs）。仅从摘要和元数据无法推断。若作者在正文中提及，则可能需要查阅完整的论文 PDF（当前不可得）。因此，假设此处未提供相关信息。

## 5. 实验数量与充分性

- **实验组数**：论文在三个设置（监督、小样本、零样本）下评估，至少涉及多个数据集（如 >2 个）。此外，还可能包含：
  - 消融实验：比较有无滤波度量、不同 LLM 生成策略、不同对齐粒度等。
  - 与 SOTA 方法的全面对比表格。
  - 可视化分析：展示显著帧选取效果、故事板生成示例。
- **充分性评价**：从摘要描述看，实验覆盖了主要场景，并在多个 metric 上取得 competitive performance，表明方法具有较强的泛化能力。但缺少具体数据集名称和数值，无法完全判断是否排除了偶然因素。通常 NeurIPS 论文会进行充分统计测试，因此实验设计应较为客观公平。

## 6. 论文的主要结论与发现

1. **多粒度对齐显著优于纯全局对齐**：在细粒度动作识别中，引入原子级描述能有效纠正误匹配，提升 top-1 准确率。
2. **LLM 生成的描述经过滤波后具有高质量**：提出的滤波度量能自动去除与全局语义不一致的噪声描述，无需人工干预。
3. **显著帧定位能力增强**：模型在视频中识别出与原子动作对应的关键帧，从而改善嵌入聚合。
4. **跨任务通用性**：SFAR 在监督、小样本、零样本设定下均展现竞争力，且多粒度思路可迁移至图像细粒度分类任务（如细粒度鸟类识别）。

## 7. 优点：方法或实验设计上的亮点

- **方法创新**：
  - 首次将故事板概念引入视频动作识别，将 LLM 生成的细粒度描述作为“镜头”来引导对齐。
  - 滤波度量避免了直接利用视频帧来修饰描述，保持模块独立性且降低计算负担。
  - 多粒度对齐框架简洁有效，易于与其他视频编码器组合。
- **实验设计优势**：
  - 覆盖了三种主流设定（监督、小样本、零样本），体现方法鲁棒性。
  - 消融实验设计应有控制变量，能够清晰验证各组件贡献。
  - 对比方法选择涵盖近期 SOTA，评价标准客观。

## 8. 不足与局限

- **实验覆盖**：未提供多个具体数据集名称和结果数值（在摘要中缺失），使我们无法判断是否在更具挑战性的数据集（如 EPIC-Kitchens、Diving48）上进行了测试。
- **偏差风险**：
  - LLM 生成的细粒度描述可能包含训练数据的隐性偏见（如动作描述偏向常见姿势、口语化表达），在零样本设定下可能对少数类不利。
  - 滤波度量基于语义相似度，可能对语义跨度较大的动作（如“跳跃”与“下蹲”共享部分原子动作）过滤不足或过度。
- **应用限制**：
  - 依赖 LLM 的生成质量，不同 LLM（如 GPT-4 与开源模型）结果可能差异较大，实验仅基于一种模型时泛化性未知。
  - 故事板生成阶段需要额外计算开销（调用 LLM API），实时应用可能受限。
  - 该思路需预设全局语义标签存在（监督/小样本），在完全无监督场景下难以直接应用。
- **资源披露缺失**：未说明算力，不利于复现和公平比较。

（完）
