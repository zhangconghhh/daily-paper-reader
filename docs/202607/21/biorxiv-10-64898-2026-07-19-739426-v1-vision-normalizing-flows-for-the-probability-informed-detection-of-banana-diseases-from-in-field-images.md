---
title: Vision Normalizing Flows for the probability-informed detection of banana diseases from in-field images
title_zh: 视觉归一化流：基于概率的田间图像香蕉病害检测
authors: "Prusokiene, A., Prusokas, A., Retkute, R."
date: 2026-07-21
pdf: "https://www.biorxiv.org/content/10.64898/2026.07.19.739426v1.full.pdf"
tags: ["query:dino-fg"]
score: 9.0
evidence: 使用DINOv3视觉基础模型进行香蕉病害分类
tldr: "香蕉病害在小农农业中造成严重损失，但田间视觉诊断因症状重叠而困难。本文提出概率识别框架，利用DINOv3提取冻结嵌入，结合条件normalizing flow进行病害检测。在5种香蕉病害的独立测试集上F1>0.98，AUROC接近1。通过似然比定义二维诊断空间，可区分患病、健康及分布外图像，支持不确定性感知，适用于智能手机部署。"
source: biorxiv
selection_source: fresh_fetch
figures_json: "[{\"url\": \"assets/figures/biorxiv/biorxiv-10-64898-2026-07-19-739426-v1/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 853, \"height\": 1360, \"label\": \"Figure\"}, {\"url\": \"assets/figures/biorxiv/biorxiv-10-64898-2026-07-19-739426-v1/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1034, \"height\": 1539, \"label\": \"Figure\"}, {\"url\": \"assets/figures/biorxiv/biorxiv-10-64898-2026-07-19-739426-v1/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1420, \"height\": 1008, \"label\": \"Figure\"}, {\"url\": \"assets/figures/biorxiv/biorxiv-10-64898-2026-07-19-739426-v1/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 1239, \"height\": 1144, \"label\": \"Figure\"}, {\"url\": \"assets/figures/biorxiv/biorxiv-10-64898-2026-07-19-739426-v1/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 1793, \"height\": 959, \"label\": \"Figure\"}, {\"url\": \"assets/figures/biorxiv/biorxiv-10-64898-2026-07-19-739426-v1/fig-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 1446, \"height\": 961, \"label\": \"Figure\"}, {\"url\": \"assets/figures/biorxiv/biorxiv-10-64898-2026-07-19-739426-v1/fig-007.webp\", \"caption\": \"\", \"page\": 0, \"index\": 7, \"width\": 1761, \"height\": 1949, \"label\": \"Figure\"}, {\"url\": \"assets/figures/biorxiv/biorxiv-10-64898-2026-07-19-739426-v1/fig-008.webp\", \"caption\": \"\", \"page\": 0, \"index\": 8, \"width\": 1387, \"height\": 1733, \"label\": \"Figure\"}, {\"url\": \"assets/figures/biorxiv/biorxiv-10-64898-2026-07-19-739426-v1/fig-009.webp\", \"caption\": \"\", \"page\": 0, \"index\": 9, \"width\": 1301, \"height\": 1008, \"label\": \"Figure\"}, {\"url\": \"assets/figures/biorxiv/biorxiv-10-64898-2026-07-19-739426-v1/fig-010.webp\", \"caption\": \"\", \"page\": 0, \"index\": 10, \"width\": 1348, \"height\": 1888, \"label\": \"Figure\"}, {\"url\": \"assets/figures/biorxiv/biorxiv-10-64898-2026-07-19-739426-v1/fig-011.webp\", \"caption\": \"\", \"page\": 0, \"index\": 11, \"width\": 1782, \"height\": 1235, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/biorxiv/biorxiv-10-64898-2026-07-19-739426-v1/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1069, \"height\": 588, \"label\": \"Table\"}, {\"url\": \"assets/tables/biorxiv/biorxiv-10-64898-2026-07-19-739426-v1/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1401, \"height\": 526, \"label\": \"Table\"}, {\"url\": \"assets/tables/biorxiv/biorxiv-10-64898-2026-07-19-739426-v1/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 900, \"height\": 330, \"label\": \"Table\"}]"
motivation: 香蕉病害诊断困难，现有方法需病害特异性微调且缺乏概率输出。
method: 使用DINOv3提取1152维冻结嵌入，训练条件normalizing flow估计条件概率密度。
result: "F1>0.98，AUROC 0.997-1.0，多类准确率接近完美，仅黄叶斑与黑叶斑有轻微混淆。"
conclusion: 提供可解释的二维诊断空间，轻量推理可部署于手机，实现不确定性感知的病害诊断。
---

## 摘要
香蕉病害在热带小农种植系统中造成严重的产量损失，然而准确的田间视觉诊断仍然困难：症状表达在不同品种和生长阶段有所不同，几种病害产生形态重叠的叶片症状。我们开发了一个概率图像识别框架，用于从田间照片中检测五种经济上重要的香蕉病害——香蕉细菌性枯萎病、香蕉束顶病、香蕉枯萎病（巴拿马病）、黄条叶斑病和黑条叶斑病，无需对视觉骨干进行任何特定病害的微调。该方法从DINOv3视觉基础模型中提取冻结的1,152维嵌入，并耦合条件归一化流，在四个公开可用数据集上训练，涵盖患病香蕉植株、健康组织、非香蕉植被和一般自然图像。在独立测试集上，模型在所有五种病害的二元检测问题中实现了超过0.98的F1分数，平均精度值0.968-0.999，AUROC值0.997-1.0。多类准确率近乎完美，黄条叶斑病和黑条叶斑病之间存在有限的混淆——这是由于早期感染叶片症状重叠造成的生物学上合理的模糊性。由于归一化流估计显式的条件概率密度而非决策边界，可以推导出两个互补的对数似然比：一个病害比（将每个病害类别与健康香蕉进行比较），一个植物比（将香蕉与非香蕉图像进行比较）。它们共同定义了一个可解释的二维诊断空间，同时量化病害存在和图像相关性的证据，清晰区分患病植株、健康植株和分布外图像，同时标记不确定的预测以进行确认测试。对冻结嵌入的推理轻量级且兼容智能手机部署，为小农种植系统和病害监测计划提供了可扩展、具有不确定性意识的诊断工具。

## Abstract
Banana diseases impose severe production losses in tropical smallholder farming systems, yet accurate in-field visual diagnosis remains difficult: symptom expression varies across cultivars and growth stages, and several diseases produce morphologically overlapping foliar signs. We developed a probabilistic image-recognition framework for detecting five economically important banana diseases - Xanthomonas Wilt, Banana Bunchy Top Disease, Fusarium Wilt (Panama disease), Yellow Sigatoka, and Black Sigatoka - from in-field photographs, without any disease-specific fine-tuning of the vision backbone. The approach extracts frozen 1,152-dimensional embeddings from the DINOv3 vision foundation model and couples them with a conditional normalizing flow, trained on four publicly available datasets spanning diseased banana plants, healthy tissue, non-banana vegetation, and general natural imagery. On an independent test set the model achieved F1 scores exceeding 0.98, average precision values of 0.968-0.999, and AUROC values of 0.997-1.0 across all five diseases evaluated as binary detection problems. Multi-class accuracy was near-perfect, with limited confusion between Yellow Sigatoka and Black Sigatoka - a biologically plausible ambiguity attributable to overlapping early-infection foliar symptoms. Because the normalizing flow estimates explicit conditional probability densities rather than decision boundaries, two complementary log-likelihood ratios can be derived: a disease ratio comparing each disease class against healthy banana, and a plant ratio comparing banana against non-banana imagery. Together these define an interpretable two-dimensional diagnostic space that simultaneously quantifies evidence for disease presence and image relevance, cleanly separating diseased plants, healthy plants, and out-of-distribution images while flagging uncertain predictions for confirmatory testing. Inference on frozen embeddings is lightweight and compatible with smartphone deployment, providing a scalable, uncertainty-aware diagnostic tool for smallholder farming systems and disease surveillance programmes.

---

## 论文详细总结（自动生成）

### 论文总结：Vision Normalizing Flows for the probability-informed detection of banana diseases from in-field images

#### 1. 核心问题与整体含义（研究动机和背景）
- **问题**：香蕉病害（如细菌性枯萎病、束顶病、枯萎病、黄条叶斑病、黑条叶斑病）在热带小农种植系统中造成严重产量损失，但田间视觉诊断困难——症状因品种、生长阶段而异，且多种病害的叶片症状存在形态重叠。
- **背景**：现有深度学习方法通常需要针对特定病害进行微调，且缺乏概率输出，难以量化预测不确定性，也无法有效区分分布外图像（如非香蕉植被或无关自然图像）。
- **整体含义**：开发一种**无需病害特异性微调**、**具有不确定性感知**、**可部署于智能手机**的概率图像识别框架，为小农农业和病害监测提供可扩展的辅助诊断工具。

#### 2. 方法论：核心思想、关键技术细节
- **核心思想**：利用**视觉基础模型（DINOv3）提取冻结嵌入**，结合**条件归一化流（conditional normalizing flow）** 估计条件概率密度，从而进行概率驱动的病害检测与分布外拒绝。
- **关键技术细节**：
  - **视觉骨干**：使用**DINOv3**（自监督视觉Transformer）提取**1152维冻结嵌入**，不进行任何病害相关的微调。
  - **概率密度估计**：训练一个**条件归一化流**，其对每个病害类别（包括健康香蕉、非香蕉图像）的条件概率密度 \( p(x|c) \) 进行显式建模。归一化流是可逆的生成模型，能计算精确对数似然。
  - **诊断空间**：从密度估计中推导出两个互补的**对数似然比**：
    - **病害比**：\( \log \frac{p(x|disease)}{p(x|healthy)} \) —— 量化疾病存在的证据。
    - **植物比**：\( \log \frac{p(x|banana)}{p(x|non-banana)} \) —— 量化图像是否属于香蕉相关（区分分布外图像）。
    - 两者共同定义**二维诊断空间**，可直观区分患病、健康、分布外图像，并标记不确定预测。
- **算法流程**（文字说明）：
  1. 输入田间图像 → DINOv3提取冻结嵌入向量。
  2. 将嵌入输入训练好的条件归一化流，获得各类别（5种病害+健康+非香蕉）的条件对数似然。
  3. 计算病害比和植物比，形成二维坐标。
  4. 根据阈值进行诊断：若疾病比高则判为患病，植物比低则判为分布外，中间区域标记为不确定需确认。

#### 3. 实验设计：数据集、Benchmark、对比方法
- **数据集**：使用了**四个公开可用数据集**，涵盖患病香蕉植株、健康组织、非香蕉植被和一般自然图像（具体名称未在摘要中列出，但提及“four publicly available datasets”）。
- **场景**：
  - **二元检测**：对五种病害分别做患病 vs 健康（或非香蕉）的二分类测试。
  - **多类分类**：同时区分五种病害+健康+非香蕉。
  - **分布外检测**：评估对非香蕉图像的拒绝能力。
- **Benchmark**：未提及与其他现有方法的直接对比（如ResNet微调、其他视觉基础模型等），主要报告自身方法的性能指标。
- **评价指标**：F1分数、平均精度（AP）、AUROC、多类准确率。

#### 4. 资源与算力
- **文中未明确说明**：使用的GPU型号、数量、训练时长、推理时间等均未提及。仅在最后提到“推理轻量级且兼容智能手机部署”，但未给出具体算力需求或量化比较。

#### 5. 实验数量与充分性
- **实验组数**：报告了独立测试集上5种病害的二元检测结果（F1>0.98，AP 0.968-0.999，AUROC 0.997-1.0），以及多类准确率接近完美。但未提供消融实验（如不同视觉骨干、不同流结构、不同阈值的影响）。
- **充分性评价**：
  - **优点**：独立测试集表现优异，且二维诊断空间提供了可解释性。
  - **不足**：缺乏与主流方法的系统对比（例如微调ResNet/EfficientNet、基于softmax的CNN、其他概率方法如贝叶斯CNN）；未在更多不同地理/品种的数据集上验证泛化性；未进行统计显著性检验。

#### 6. 论文的主要结论与发现
- 所提出的**DINOv3 + 条件归一化流**框架在**无需微调视觉骨干**的情况下，在5种香蕉病害的二元检测中达到**F1>0.98，AUROC接近1**，多类准确率近乎完美。
- 仅有的混淆发生在**黄条叶斑病和黑条叶斑病**之间，这是由于早期感染症状重叠导致的生物学合理模糊性。
- **二维诊断空间**（疾病比+植物比）有效分离患病、健康、分布外图像，并能标记不确定预测，提供可解释的决策支持。
- 框架**轻量推理**，**可部署于智能手机**，适合小农应用。

#### 7. 优点：方法或实验设计上的亮点
- **方法创新**：将**视觉基础模型（DINOv3）的冻结嵌入**与**归一化流**结合，避免了病害特异性微调，降低了数据需求（小样本下也能表现良好？）。
- **概率建模**：显式密度估计比传统softmax分类器更完善，提供**似然比**这一可解释的诊断证据，支持不确定性量化。
- **分布外检测能力**：通过“植物比”明确拒绝非香蕉图像，这在真实田间场景中至关重要（用户可能拍摄无关物体）。
- **可部署性**：冻结嵌入的推理计算量小，适合移动端；无需微调视觉骨干，更新模型仅需重训轻量归一化流。
- **实验指标全面**：报告了F1、AP、AUROC等多维度指标，且结果接近天花板。

#### 8. 不足与局限
- **实验覆盖有限**：
  - 仅评估了5种病害，未覆盖更多香蕉病害（如病毒病、线虫、营养不良等）。
  - 数据集来源可能单一（公开数据集，未提及地域多样性），泛化性有待检验。
- **缺少对比基准**：未与当前先进方法（如微调ResNet-50、EfficientNet、MobileNet、ViT、或者其他概率模型如贝叶斯CNN）进行公平比较，难以判断相对优势。
- **缺乏消融实验**：未分析DINOv3不同层、不同嵌入维度、归一化流结构选择、阈值敏感性等。
- **未报告算力与时间开销**：无法评估实际部署的成本。
- **生物学模糊性**：黄叶斑和黑叶斑的混淆是合理但未提供解决方案（如多时间采样或多光谱）。
- **仅依赖视觉特征**：未结合作物生长模型、环境数据等辅助信息，纯视觉诊断可能受背景干扰（如土壤、光照）。

（完）
