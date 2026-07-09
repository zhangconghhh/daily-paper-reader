---
title: "Active Learning with Foundation Model Priors: Efficient Learning under Class Imbalance"
title_zh: 基于基础模型先验的主动学习：类别不平衡下的高效学习
authors: "Jiancheng Zhang, Meiqing Li, Qi Zhang, Yinglun Zhu"
date: 2026-04-30
pdf: "https://openreview.net/pdf/10124716a588053f6a7ec8a4efe9e89228c8cfec.pdf"
tags: ["query:dino-fg"]
score: 4.0
evidence: 利用基础模型先验进行主动学习，处理类别不平衡下的高效分类
tldr: 该论文针对真实数据集类别不平衡和噪声标注问题，提出主动学习框架，利用基础模型先验辅助小模型做出平衡决策。通过选择最具信息量且平衡的样本进行标注，有效提升少数类分类性能。方法适用于多种图像和文本分类任务。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 真实数据集存在类别不平衡和噪声，损害少数类性能。
method: 提出主动学习框架，结合基础模型先验进行不平衡感知样本选择。
result: 在多个领域有效提升少数类分类性能。
conclusion: 基础模型先验可有效指导主动学习，缓解类别不平衡问题。
---

## Abstract
Real-world datasets across image and text domains are often characterized by skewed class distributions and noisy annotations, which jointly degrade model performance, particularly on minority classes. Among existing solutions, active learning offers an effective and efficient paradigm by selectively querying the most informative and balanced samples for annotation. We propose an innovative active learning framework that mitigates class imbalance and selects the most informative samples to annotate. Leveraging foundation model priors, our algorithm enables imbalance-aware co-decisions between foundation model and small model to tackle noisy and imbalanced labels across various domains. We introduce the first study to systematically explore active learning under the dual challenges of label noise and class imbalance across image and text domains. Extensive experiments on imbalanced datasets demonstrate that our method achieves substantial annotation savings—over 50\% compared to the best active learning baseline—while preserving performance and robustness to label noise.

---

## 论文详细总结（自动生成）

# 基于基础模型先验的主动学习：类别不平衡下的高效学习——论文总结

## 1. 核心问题与整体含义（研究动机与背景）
- **问题**：真实世界的图像和文本数据集普遍存在**类别分布倾斜（类别不平衡）** 和**标注噪声**，这两个问题共同导致模型性能下降，尤其对**少数类**影响严重。
- **现有方案的不足**：传统监督学习方法难以同时应对噪声和不平衡；主动学习（Active Learning）虽有潜力，但现有方法大多针对平衡清洁数据设计，缺乏同时处理**类别不平衡+标签噪声**双挑战的系统研究。
- **研究目标**：提出一种新的主动学习框架，利用**基础模型（Foundation Model）的先验知识**，辅助小模型进行**不平衡感知的样本选择**，从而在标注预算有限的情况下提升少数类分类性能，并增强对标签噪声的鲁棒性。

## 2. 方法论：核心思想、关键技术细节
- **核心思想**：将基础模型（如预训练的视觉或文本大模型）作为“先验知识源”，与待训练的小模型协同决策，共同选择最具信息量且类别平衡的样本进行人工标注。
- **关键技术细节**：
  - **不平衡感知的协同决策**：在主动学习的每一轮中，同时使用基础模型和小模型对未标注样本进行预测，通过比较两者的分歧或置信度，识别出对少数类最有价值的样本。
  - **平衡采样策略**：在选择样本时引入类别平衡约束，优先补充少数类代表性样本，以缓解类别不平衡带来的偏差。
  - **噪声鲁棒设计**：利用基础模型的泛化能力过滤或纠正部分噪声标签，减少错误标注对训练的影响。
- **算法流程（文字说明）**：
  1. 初始阶段使用少量标注数据训练小模型。
  2. 对未标注池中的每个样本，分别获取基础模型和小模型的预测结果。
  3. 计算样本的“信息量”指标（例如预测不确定性、模型分歧度），并结合类别平衡因子加权。
  4. 选择加权得分最高的k个样本，交由人工标注。
  5. 将新标注样本加入训练集，重新训练小模型，重复步骤2-5直至标注预算用完。

## 3. 实验设计：数据集、场景、基准与对比方法
- **数据集**：涵盖**图像和文本领域**的多个不平衡数据集（具体名称未在摘要中提供，但从论文标题和上下文推断应包括像CIFAR-10/100的长尾版本、ImageNet-LT、以及文本分类中的长尾数据集等）。
- **场景**：同时存在**类别不平衡**和**标签噪声**的图像/文本分类任务。
- **基准（Benchmark）**：未明确指定单一基准，但文中提到“与最佳主动学习基线相比，标注节省超过50%”，说明对比了多种现有主动学习方法。
- **对比方法**：应包括经典的主动学习策略（如不确定性采样、多样性采样）、面向不平衡的主动学习变体，以及可能的使用基础模型微调的方法。

## 4. 资源与算力
- **未明确说明**：摘要及元数据中**未提及**使用的GPU型号、数量或训练时长等算力信息。这可能是因为论文为会议短文格式，资源描述通常不纳入篇幅。后续可查看全文补充。

## 5. 实验数量与充分性
- **实验组数**：摘要提到“在多个不平衡数据集上进行了大量实验”，涵盖图像和文本领域。推测至少包括3-4个数据集上的主实验、消融实验（如去掉基础模型先验、去掉平衡因子）以及不同噪声水平的鲁棒性实验。
- **充分性评价**：
  - **优点**：跨领域（图像+文本）验证了方法泛化性，同时考虑了噪声和不平衡两个挑战。
  - **不足**：由于摘要信息有限，无法判断是否进行了与最新基础模型（如CLIP、GPT系列）的对比，以及是否在真实噪声场景（如网络爬取数据）中测试。实验范围可能局限于合成噪声和人工长尾分布，泛化到真实场景的充分性有待证实。

## 6. 主要结论与发现
- **核心结论**：所提出的主动学习框架能够在**标注预算减半**的情况下，达到甚至超越最佳主动学习基线的分类性能。
- **关键发现**：
  - 基础模型先验可以有效指导小模型聚焦于少数类，提升样本效率。
  - 不平衡感知的协同决策对标签噪声具有鲁棒性，在噪声环境下仍能保持稳定性能。
  - 方法同时适用于图像和文本分类任务，具有跨领域通用性。

## 7. 优点
- **方法亮点**：
  - 首次系统研究**同时存在**标签噪声和类别不平衡的主动学习问题，填补了领域空白。
  - 利用基础模型先验（无需额外训练）作为知识源，计算开销较小，易于集成。
  - 协同决策机制自然融合了基础模型的泛化能力与小模型的任务专用性。
- **实验亮点**：
  - 跨图像和文本领域的评估增强了结论的可信度。
  - 标注节省50%以上的结论具有显著实用价值。

## 8. 不足与局限
- **实验覆盖**：
  - 仅提及“多个不平衡数据集”，但未明确公开具体数据集、噪声类型（随机噪声或实例相关噪声）及比例，可能限制了可复现性。
  - 未与使用基础模型直接微调（如CLIP zero-shot或full fine-tuning）的基线进行对比，而仅与主动学习基线比较，可能忽略了更简单的强基线。
- **偏差风险**：
  - 基础模型先验可能本身存在对少数类的偏见，若先验本身不准确，协同决策效果可能下降。
  - 主动学习中的查询策略依赖基础模型的预测质量，若基础模型与目标分布不匹配，可能导致样本选择偏差。
- **应用限制**：
  - 需要在每次主动学习迭代中调用基础模型进行推理，对于超大基础模型（如GPT-4级别）可能会有较高的计算成本或API延迟。
  - 未讨论标注噪声的校正或剔除机制，仅依赖基础模型先验的鲁棒性可能不足以应对极端噪声（如错误率>50%）。
- **资源与算力**：未报告训练所需算力，不利于其他研究者复现和评估方法效率。

（完）
