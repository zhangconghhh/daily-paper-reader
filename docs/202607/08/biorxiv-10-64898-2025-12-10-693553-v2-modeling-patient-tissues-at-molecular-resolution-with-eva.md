---
title: Modeling patient tissues at molecular resolution with Eva
title_zh: 使用Eva在分子分辨率下对患者组织进行建模
authors: "Liu, Y., Sharma, R., Bieniosek, M., Kang, A., Wu, E., Chou, P., Li, I., Rahim, M., Bauer, E., Ji, R., Duan, W., Qian, L., Luo, R., Sharma, P., Dhanasekaran, R., Schürch, C. M., Charville, G., Mayer, A., Zou, J., Trevino, A. E., Wu, Z."
date: 2026-07-08
pdf: "https://www.biorxiv.org/content/10.64898/2025.12.10.693553v2.full.pdf"
tags: ["query:dino-fg"]
score: 7.0
evidence: 基于ViT架构的组织成像基础模型
tldr: 组织结构是器官功能与稳态的基础，但空间蛋白质组等数据难以直接分析。本文提出Eva，一种基于视觉Transformer的基础模型，通过掩码重建预训练于匹配的空间蛋白质组和组织病理图像，学习分子、细胞到样本级的多尺度表示。Eva在跨模态推断、质量控制、零样本检索、生存分析和患者分层等任务上表现优异。该模型有望通过桥接基础研究与临床实践，加速转化医学发展。
source: biorxiv
selection_source: fresh_fetch
motivation: 从多模态组织数据中提取结构-分子-临床关联困难，缺少统一的基础模型来整合这些信息。
method: 采用新颖视觉Transformer架构，在匹配的空间蛋白质组与组织病理图像上进行掩码重建预训练，学习多尺度空间表示。
result: Eva在跨模态推断、质量控制、数据标注、零样本检索、生存建模和患者分层等任务上展现出优异的泛化性能。
conclusion: Eva桥接了基础研究与临床实践，有望加速转化科学，推动精准诊断和治疗策略的发展。
---

## 摘要
组织结构对所有器官的功能和稳态至关重要，结构的破坏通常表明疾病。对组织的结构、分子和临床方面之间的关系进行建模，可以推动新的诊断和治疗策略。尽管空间蛋白质组学等分析技术能够捕捉这些关系，但从中提取洞察的数据仍然具有挑战性。在此，我们提出Eva，一个用于组织成像数据的基础模型，它能够在分子、细胞和样本水平上学习组织的多尺度空间表示。Eva采用新颖的视觉变换器架构，并通过掩码重建匹配的空间蛋白质组学和组织病理学图像进行预训练。我们证明Eva在多种任务中表现出色，包括跨模态推理、质量控制、数据标注、零样本检索、生存建模和患者分层。对保留验证数据的广泛评估展示了所学嵌入的多样性和泛化能力。我们预计Eva将通过桥接基础研究和临床实践来加速转化科学。

## Abstract
Tissue structure is essential to function and homeostasis in all organs, and disruptions to structure usually indicate disease. Modeling relationships between structural, molecular, and clinical aspects of tissues could advance new diagnostics and treatment strategies. Although profiling techniques like spatial proteomics can capture these relationships, the data remain challenging to extract insight from. Here, we present Eva, a foundation model for tissue imaging data that learns multi-scale spatial representations of tissues at the molecular, cellular, and sample level. Eva uses a novel vision transformer architecture and is pre-trained on masked reconstruction of matched spatial proteomics and histopathology images. We show that Eva excels at a variety of tasks, including cross-modal inference, quality control, data annotation, zero-shot retrieval, survival modeling, and patient stratification. Extensive evaluations on held-out validation data demonstrate the versatility and generalizability of the learned embeddings. We anticipate that Eva will accelerate translational science by bridging basic research and clinical practice.