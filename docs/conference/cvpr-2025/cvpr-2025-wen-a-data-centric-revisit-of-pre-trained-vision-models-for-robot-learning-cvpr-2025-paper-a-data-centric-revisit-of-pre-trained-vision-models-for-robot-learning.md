---
title: A Data-Centric Revisit of Pre-Trained Vision Models for Robot Learning
title_zh: 面向机器人学习的预训练视觉模型的数据中心重新审视
authors: "Wen, Xin, Zhao, Bingchen, Chen, Yilun, Pang, Jiangmiao, Qi, Xiaojuan"
date: 2025-06-01
pdf: "https://openaccess.thecvf.com/content/CVPR2025/papers/Wen_A_Data-Centric_Revisit_of_Pre-Trained_Vision_Models_for_Robot_Learning_CVPR_2025_paper.pdf"
tags: ["query:dino-fg"]
score: 8.0
evidence: 评估DINO并发现其局限，提出SlotMIM增强对象中心表示
tldr: 预训练视觉模型在机器人学习中至关重要，但DINO和iBOT在非目标中心数据上表现不佳，原因是对象中心表示能力弱。本文通过系统评估发现这一局限，并设计SlotMIM方法，利用语义瓶颈减少原型数量，强制模型学习对象中心表示。实验证明SlotMIM有效提升了DINO在非对象中心数据上的性能，对细粒度分类等需要对象中心表示的任务有重要启示。
source: CVPR-2025-Accepted
selection_source: conference_retrieval
motivation: DINO等预训练模型在机器人任务中缺乏对象中心表示能力，限制其在非目标中心数据上的泛化。
method: 提出SlotMIM，通过语义瓶颈减少原型数量，强制模型学习对象中心表示。
result: SlotMIM显著提升了DINO在非对象中心数据上的表示质量。
conclusion: 为DINO在细粒度分类等下游任务中的应用提供了改进方向。
---

## Abstract
Pre-trained vision models (PVMs) are fundamental to modern robotics, yet their optimal configuration remains unclear. Through systematic evaluation, we find that while DINO and iBOT outperform MAE across visuomotor control and perception tasks, they struggle when trained on non-(single-)object-centric (NOC) data--a limitation strongly correlated with their diminished ability to learn object-centric representations. This investigation indicates that the ability to form object-centric representations from the non-object-centric robotics dataset is the key to success for PVMs. Motivated by this discovery, we designed SlotMIM, a method that induces object-centric representations by introducing a semantic bottleneck to reduce the number of prototypes to encourage the emergence of objectness as well as cross-view consistency regularization for encouraging multiview invariance. Our experiments encompass pre-training on object-centric, scene-centric, web-crawled, and ego-centric data. Across all settings, our approach learns transferrable representations and achieves significant improvements over prior work in image recognition, scene understanding, and robot learning evaluations. When scaled up with million-scale datasets, our method also demonstrates superior data efficiency and scalability. Our code and models are publicly available at https://github.com/CVMI-Lab/SlotMIM.

---

## 论文详细总结（自动生成）

好的，以下是根据论文内容生成的详细中文总结，按要求的8个要点展开。

---

### 1. 论文的核心问题与整体含义（研究动机和背景）

- **核心问题**：预训练视觉模型（PVMs）已成为现代机器人学习的基础，但最优的预训练方法（如MAE、DINO、iBOT等）和最优的预训练数据（如目标中心数据、场景中心数据、网络爬取数据、自我中心数据）组合尚未明确。现有工作多默认MAE + 自我中心数据为最佳，但缺乏系统验证。
- **研究动机**：通过数据中心（data-centric）的视角，系统评估不同PVM方法和不同数据源在多种机器人任务上的表现，探索更优的预训练范式。
- **整体含义**：挑战现有共识，发现DINO/iBOT虽然在目标中心数据上远超MAE，但在非目标中心（NOC）数据上性能显著下降，其根源在于对象中心表示学习能力不足。基于此发现，设计了SlotMIM方法，能够从NOC数据中有效学习对象中心表示，从而提升在多样化下游任务上的表现。

### 2. 论文提出的方法论：核心思想、关键技术细节、公式或算法流程

- **核心思想**：将NOC数据上的自监督学习分解为两个子任务：①将图像补丁分组为对象（object grouping）；②对分组后的对象进行判别性对比学习。关键在于无监督对象发现，论文发现iBOT在使用较少原型（prototypes）时能自然涌现对象性，但缺乏语义意义。
- **关键技术细节**：
  - **语义瓶颈**：减少iBOT原型数量（例如从8192降至512），强制模型学习更语义化的对象概念。
  - **跨视图一致性正则化（Cross-view consistency）**：利用ROIAlign对齐不同数据增强视图间的重叠补丁，施加交叉视图的聚类分配一致性损失，鼓励视图不变表示。
  - **对象级对比学习**：基于语义补丁和原型初始化对象特征（slot），对匹配的slot对进行MoCo风格的对比学习，增强表示判别力。
- **算法流程（文字说明）**：
  1. 对输入图像进行两次数据增强得到两个视图，对其中一个视图进行随机掩码。
  2. 学生和教师编码器分别处理掩码和未掩码视图，输出补丁嵌入。
  3. 通过一组可学习原型（C个）计算每个补丁的聚类分配（soft assignment）。
  4. 计算三个损失：
     - 视图内补丁聚类分配一致性损失（即iBOT的`L_within_patch`）：预测掩码补丁的分配与教师未掩码补丁的分配一致。
     - 跨视图补丁聚类分配一致性损失（`L_cross_patch`）：匹配两个视图间重叠区域的补丁分配。
     - 对象级对比损失（`L_slot`）：将补丁按聚类分配加权平均得到slot特征，对正例slot对做对比学习。
  5. 总损失为三项损失的加权和，对称优化两个视图。

### 3. 实验设计：数据集/场景、benchmark、对比方法

- **预训练数据集**：
  - 目标中心（Object-centric）：ImageNet（241K子集，1.28M子集）
  - 场景中心（Scene-centric）：COCO+（COCO训练集+无标注子集，241K），Open Images（扩大至百万级）
  - 网络爬取（Web-crawled）：CC12M（241K）
  - 自我中心（Ego-centric）：Ego4D（241K、1.28M）
- **下游任务与benchmark**：
  - **操作任务**：Franka Kitchen（5个子任务）、Meta-World（8个子任务），采用行为克隆（behavior cloning），使用注意力池化读出头。
  - **导航任务**：ObjectNav（目标导航）、ImageNav（图像导航），遵循VC-1设置。
  - **感知任务**：Pascal VOC 2012分割（Jaccard Index）、ADE20K语义分割（mIoU）、COCO目标检测和实例分割（AP）。
- **对比方法**：
  - 预训练方法：BEiT、SplitMask、MAE、DINO、iBOT。
  - 已有机器人PVM方法：R3M、MVP、VC-1、V-Cond等。
- **公平性控制**：数据规模统一为241K（缩放实验中进一步扩大），模型架构均为ViT-B/16，训练epochs相同（241K时800 epochs，1.28M时400 epochs）。

### 4. 资源与算力

- 文中未明确说明使用的GPU型号、数量和总训练时长。仅提到训练配置（如ViT-B/16，800/400 epochs），但未提供具体计算资源细节。需要指出这一点。

### 5. 实验数量与充分性

- **实验数量**：大量实验覆盖了：
  - 4种数据源 × 6种预训练方法（共24个组合）在操作和分割任务上的完整结果（图2）。
  - 数据缩放到百万级后的对比（图6，涉及多个方法）。
  - 导航任务对比（表4，2种SlotMIM模型 vs MVP/VC-1）。
  - 消融实验（表3，对5个关键模块进行组合测试）。
  - 额外在COCO检测分割上的缩放实验（图7）。
- **充分性与公平性**：实验设计较为系统，控制了数据规模、模型架构、训练超参数等变量。在每个任务上重复3个种子并取平均，减少了随机性。同时也使用了公开已有检查点（如MVP、VC-1、iBOT在ImageNet-21K上的）进行公平对比。
- **客观性**：不仅报告了最好结果，也展示了多个方法的平行对比，并分析了性能下降的原因（过压缩）。消融实验清晰展现了各模块贡献。

### 6. 论文的主要结论与发现

- **发现1**：DINO和iBOT在目标中心数据上显著优于MAE，但在NOC数据（场景、网络、自我中心）上性能大幅下降，而MAE受数据影响较小。
- **发现2**：性能下降与模型的对象中心表示能力（objectness）高度相关（相关系数0.72），说明学习对象性是从NOC数据获得好表示的关键。
- **发现3**：传统方法（DINO/iBOT/MAE）在百万级NOC数据上出现性能饱和甚至下降（过压缩），而SlotMIM能持续提升，展现出更好的可扩展性和数据效率。
- **发现4**：SlotMIM在仅241K数据上就已超越使用超过1M数据的MVP、VC-1等方法；在场景中心数据上训练后，导航任务（ObjectNav）超越VC-1 6.6%。
- **发现5**：SlotMIM学习到的概念根据预训练数据分布自适应调整（如从自我中心数据中学到细粒度部件，从场景中心数据中学到常见物体），避免了过压缩。

### 7. 优点：方法或实验设计上的亮点

- **方法亮点**：
  - 提出语义瓶颈和跨视图一致性，将iBOT从细粒度模式学习转化为语义对象发现，简单有效。
  - 对象级对比学习直接作用于聚合后的slot特征，而非原始补丁，适合NOC数据中的多对象场景。
  - 无需外部监督，全自监督，且能适应不同类型数据。
- **实验设计亮点**：
  - 数据中心（data-centric）视角，系统评估“方法 × 数据”组合，而非仅关注某一维度。
  - 覆盖操作、导航、感知多种任务，全面评估泛化能力。
  - 控制数据规模进行公平比较，并逐步放大规模观察可扩展性。
  - 引入消融实验和可视化（图4），直观展示原型数量对对象性的影响。

### 8. 不足与局限

- **实验覆盖**：虽任务多样，但缺乏真实机器人部署实验，仅在模拟器（Franka Kitchen、Meta-World、Habitat）上验证。模拟环境与真实世界的差距可能影响结论的泛化性。
- **偏差风险**：行为克隆策略（BC）在相对简单的任务上可能掩盖高级视觉表示的真实差距；更复杂的策略（如RL或分层策略）下表现需进一步验证。
- **应用限制**：SlotMIM依赖原型数量和视图对齐等超参数，跨不同数据域可能需要调参；对掩码策略（ROIAlign）要求在增强中保留空间对应，可能不适用于大幅裁剪。
- **计算资源**：虽训练效率不错，但相对于直接使用MAE，SlotMIM增加了对比学习组件和双视图处理，在极大规模（如亿级）数据上的计算开销未评估。
- **公平性**：部分对比结果使用了第三方预训练检查点（如iBOT on ImageNet-21K），其训练设置与本文不完全一致，可能影响直接比较的公平性。作者已尽量控制，但仍需注意。

（完）
