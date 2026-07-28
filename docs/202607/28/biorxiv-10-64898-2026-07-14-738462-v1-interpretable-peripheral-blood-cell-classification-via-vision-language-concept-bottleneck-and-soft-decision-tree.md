---
title: Interpretable Peripheral Blood Cell Classification via Vision-Language Concept Bottleneck and Soft Decision Tree
title_zh: 基于视觉-语言概念瓶颈和软决策树的可解释外周血细胞分类
authors: "Chen, K., Hu, T."
date: 2026-07-20
pdf: "https://www.biorxiv.org/content/10.64898/2026.07.14.738462v1.full.pdf"
tags: ["query:dino-fg"]
score: 7.0
evidence: 利用视觉-语言概念瓶颈进行细粒度血细胞分类
tldr: "现有深度学习外周血细胞分类模型缺乏可解释性，无法基于形态学概念推理。本文提出两阶段可解释流水线：先由ConceptCLIP零样本提取70维形态概念分数，再由软决策树(SDT)基于分数分类。在BloodMNIST上达94.86%准确率，决策逻辑与血液学标准一致，且无监督分离早幼粒细胞与中幼粒细胞亚型。该方法在保持高性能的同时提供透明决策路径，具有临床审计价值。"
source: biorxiv
selection_source: fresh_fetch
figures_json: "[{\"url\": \"assets/figures/biorxiv/biorxiv-10-64898-2026-07-14-738462-v1/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1763, \"height\": 572, \"label\": \"Figure\"}, {\"url\": \"assets/figures/biorxiv/biorxiv-10-64898-2026-07-14-738462-v1/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 877, \"height\": 527, \"label\": \"Figure\"}, {\"url\": \"assets/figures/biorxiv/biorxiv-10-64898-2026-07-14-738462-v1/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1833, \"height\": 606, \"label\": \"Figure\"}, {\"url\": \"assets/figures/biorxiv/biorxiv-10-64898-2026-07-14-738462-v1/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 1295, \"height\": 2488, \"label\": \"Figure\"}, {\"url\": \"assets/figures/biorxiv/biorxiv-10-64898-2026-07-14-738462-v1/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 809, \"height\": 442, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/biorxiv/biorxiv-10-64898-2026-07-14-738462-v1/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 905, \"height\": 275, \"label\": \"Table\"}, {\"url\": \"assets/tables/biorxiv/biorxiv-10-64898-2026-07-14-738462-v1/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 904, \"height\": 271, \"label\": \"Table\"}, {\"url\": \"assets/tables/biorxiv/biorxiv-10-64898-2026-07-14-738462-v1/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 920, \"height\": 273, \"label\": \"Table\"}, {\"url\": \"assets/tables/biorxiv/biorxiv-10-64898-2026-07-14-738462-v1/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 1852, \"height\": 356, \"label\": \"Table\"}]"
motivation: 解决外周血细胞分类中深度模型的黑箱问题，使决策基于可理解的形态学概念且路径可追踪。
method: 两阶段流水线：冷冻ConceptCLIP零样本提取70维概念分数，SDT基于分数分类并输出决策路径。
result: "BloodMNIST八类测试准确率94.86%，仅比黑箱低约3%，且决策规则符合临床标准，无监督发现未训练亚型。"
conclusion: 概念瓶颈结合软决策树实现可解释高精度血细胞分类，并能恢复细粒度亚型结构。
---

## 摘要
动机
用于医学图像分析的深度学习分类器通常作为黑箱模型运行，既不揭示其预测所依据的图像特征，也不披露每个决策的推理过程。外周血细胞分类体现了这一挑战：经验丰富的实验室专业人员通过结构化的形态学标准——细胞核形状、染色质纹理、核质比、颗粒性和染色特性——来识别细胞类型，然而现有的自动化系统无法用这些术语表达其推理，阻碍了临床审计和验证。

结果
我们提出了一种两阶段可解释流水线，以解决这两个层面的黑箱问题。在第一阶段，一个冻结的领域自适应视觉-语言模型（ConceptCLIP）通过零样本余弦相似度将每个细胞图像投影到一个70维的形态学概念得分向量上，消除了每个图像概念注释的需求。在第二阶段，一个软决策树（SDT）仅基于这些概念得分对细胞进行分类，为每个预测产生确定性的、基于概念的决策路径。在BloodMNIST（8种细胞类型，3421张测试图像）上，整个流水线达到了94.86%的测试准确率——比黑箱上限低约3个百分点——同时提供了完全可追溯的决策逻辑。训练后的组织学注释证实，学习到的路由逻辑与既定的血液形态学标准一致，并揭示了未成熟粒细胞亚型（早幼粒细胞与中幼粒细胞）在无亚型监督下的涌现性分离，表明基于概念的决策树可以恢复超出训练标签粒度的临床有意义的区分。

可用性与实现
源代码、训练后的SDT权重、预计算的概念得分数据和推理脚本公开可用，网址为 https://github.com/aquamarineaqua/CLIP-CBM-SoftDecisionTree。

## Abstract
MotivationDeep learning classifiers for medical image analysis typically function as black boxes, disclosing neither the image features underlying their predictions nor the reasoning by which individual decisions are reached. Peripheral blood cell classification exemplifies this challenge: experienced laboratory professionals identify cell types through structured morphological criteria--nucleus shape, chromatin texture, nucleus-to-cytoplasm ratio, granularity, and staining properties--yet existing automated systems cannot express their reasoning in these same terms, impeding clinical audit and verification.

ResultsWe present a two-stage interpretable pipeline that addresses both levels of opacity. In the first stage, a frozen domain-adapted vision-language model (ConceptCLIP) projects each cell image onto a 70-dimensional vector of morphological concept scores via zero-shot cosine similarity, eliminating the need for per-image concept annotations. In the second stage, a Soft Decision Tree (SDT) classifies cells solely on these concept scores, producing a deterministic, concept-based decision path for each prediction. On BloodMNIST (eight cell types, 3,421 test images), the full pipeline achieves 94.86% test accuracy--approximately 3 percentage points below the black-box ceiling--while providing fully traceable decision logic. Post-training histological annotation confirms that the learned routing logic aligns with established hematological morphology criteria and reveals an emergent separation of immature granulocyte subtypes (promyelocyte versus metamyelocyte) without subtype supervision, demonstrating that concept-based decision trees can recover clinically meaningful distinctions beyond the granularity of the training labels.

Availability and implementationThe source code, trained SDT weights, precomputed concept score data, and inference scripts are publicly available at https://github.com/aquamarineaqua/CLIP-CBM-SoftDecisionTree.