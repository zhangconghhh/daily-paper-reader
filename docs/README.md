<div class="dpr-home-notice-card">
  <h3 class="dpr-home-notice-title">🚀 Start Here</h3>
  <ul class="dpr-home-notice-list">
    <li><a href="#/tutorial/README">使用教程</a></li>
  </ul>
</div>

## 每次日报
- 最新运行日期：2026-08-04
- 运行时间：2026-08-04 22:02:42 UTC
- 运行状态：成功
- 本次总论文数：7
- 精读区：2
- 速读区：5

### 今日简报（AI）
今日精读聚焦医学影像基础模型，两项工作均获8.0高分，覆盖超广角视网膜成像与位置感知细粒度表示学习；速读则涉及异常检测、ViT注意力剪枝及病理切片分类。  
最值得关注方向：医学影像基础模型的特征迁移与位置感知学习，是提升下游任务性能的关键。  
下一步可结合精读方法，尝试将位置感知表示应用于自有视网膜或病理影像数据集，验证泛化效果。
- 详情：[/202608/04/README](/202608/04/README)

### 精读区论文标签
1. [Representation Transfer of Foundation Models for Ultra-Widefield Retinal Imaging](/202608/04/2608.00586v1-representation-transfer-of-foundation-models-for-ultra-widefield-retinal-imaging)  
   标签：评分：8.0/10、query:dino-fg
   evidence：比较有监督、MAE与自蒸馏（类DINO）ViT基础编码器作为疾病分类特征提取器
2. [Location-Aware Fine-Grained Representation Learning for Medical Vision Foundation Models](/202608/04/2608.00976v1-location-aware-fine-grained-representation-learning-for-medical-vision-foundation-models)  
   标签：评分：8.0/10、query:dino-fg
   evidence：面向基于Transformer的医学视觉基础模型做位置感知细粒度表示学习，契合细粒度视觉分类的基础模型需求。

### 速读区论文标签
1. [DuoAD: Leveraging [CLS] Dual Characteristics for Training-Free Few-Shot Anomaly Detection](/202608/04/2607.23924v1-duoad-leveraging-cls-dual-characteristics-for-training-free-few-shot-anomaly-detection)  
   标签：评分：6.0/10、query:dino-fg
   evidence：利用视觉基础模型的ViT [CLS]令牌和注意力图进行图像级异常检测
2. [Interpretability-Guided Soft Pruning of Attention Heads in Vision Transformers](/202608/04/2608.00264v1-interpretability-guided-soft-pruning-of-attention-heads-in-vision-transformers)  
   标签：评分：6.0/10、query:dino-fg
   evidence：以DINOv2视觉基础模型和ViT注意力头软剪枝为核心，有助于高效的Transformer分类模型。
3. [From Patches to Evidence Balls: Class-Conditioned Evidence Retrieval for Few-Shot Whole Slide Image Classification](/202608/04/2608.01104v1-from-patches-to-evidence-balls-class-conditioned-evidence-retrieval-for-few-shot-whole-slide-image-classification)  
   标签：评分：6.0/10、query:dino-fg
   evidence：面向少样本全切片分类的类别条件证据检索，可迁移至依赖局部线索的细粒度分类
4. [EulerLoRA: Rank-Driven Jump Dynamics for Calibrated Parameter-Efficient Fine-Tuning](/202608/04/2608.01142v1-eulerlora-rank-driven-jump-dynamics-for-calibrated-parameter-efficient-fine-tuning)  
   标签：评分：6.0/10、query:dino-fg
   evidence：使用视觉Transformer在图像分类基准上进行参数高效微调
5. [Transformer Geometry Observatory TGO-III: Semantic Geometry Observatory](/202608/04/2608.01876v1-transformer-geometry-observatory-tgo-iii-semantic-geometry-observatory)  
   标签：评分：6.0/10、query:dino-fg
   evidence：分析ViT-Small/16逐层的类别可分性与语义几何，与用于分类的视觉Transformer架构相关。


<div class="dpr-home-promo-card">
  <h3 class="dpr-home-promo-title">💬 社区与支持</h3>
  <ul class="dpr-home-promo-list">
    <li>欢迎 Star / Fork / Issue / PR</li>
    <li>QQ群：583867967（欢迎交流，已有：1151人）</li>
  </ul>
</div>
