# domain-adaptation-object-detection
awesome domain adaptation works with interpretations for object detection.
关于领域自适应，重点关注目标检测中的领域自适应任务，包括：相关资源、综述、代表工作及其代码，常见域迁移场景及数据集等等。

# Content
* [1. resources (相关资源)](#1)
* [2. surveys (综述)](#2)
* [3. papers (代表工作)](#3)
* [4. datasets (数据集)](#4)
<!--* [5. leaderboard (排行榜)](#5)-->

<h2 id="1">1. resources (相关资源)</h2>

### github仓库
[1. awesome-domain-adaptation](https://github.com/zhaoxin94/awesome-domain-adaptation#other-resources)
[2. transerlearning](https://github.com/jindongwang/transferlearning)
### Tutorials 学习资源
* Video tutorials 视频教程
  * [Domain adaptation-迁移学习中的领域自适应方法](https://www.bilibili.com/video/BV1T7411R75a/)
  * [Transfer learning-Hungyi Lee 台湾大学李宏毅视频讲解](https://www.youtube.com/watch?v=qD6iD4TFsdQ)

<h2 id="2">2. surveys (综述)</h2>

* Deep Domain Adaptive Object Detection: a Survey [[ICIP2020]](https://arxiv.org/abs/2002.06797v1)


<h2 id="3">3. papers (代表工作)</h2>

各论文中会提到某个级别的特征对齐，这里简单说一下我对各特征对齐的理解：
global、instance、local、pixel-level的含义
* global-level\image-level 表示backbone网络输出的feature map
* instance-level 表示RoIpooling 输出的ROIs的表征
* local-level 表示backbone的中间层卷积的表征
* pixel-level 表示利用风格转化或者CycleGAN这些方法关注的部分

### Conference
* Adapting Object Detectors with Conditional Domain Normalization [[ECCV2020]](http://www.ecva.net/papers/eccv_2020/papers_ECCV/papers/123560392.pdf)[[wenote]](http://wenote.huawei.com/wapp/pages/view/share/s/0NhUxM0oTx7H2BiOcT12g4ts36iE6w2xKN7H2COHI63HQgBG)
```domain embedding```
作者给源域学习了一个**域向量(embedding)** ，并用类似于attention的思想，用这个域向量来查询目标域的表征，从目标域中提取域属性，最后再进行特征分布对齐，即进行对抗学习。这样可能的效果是降低域对齐的难度，因为生成的源域和目标域的表征更加接近。该方法在cityscape->foggy场景下相比于同年的CVPR提升并没有很大。

* Adaptive Object Detection with Dual Multi-Label Prediction [[ECCV2020]](https://www.ecva.net/papers/eccv_2020/papers_ECCV/papers/123730052.pdf)[[wenote]](http://wenote.huawei.com/wapp/pages/view/share/s/0NhUxM0oTx7H2BiOcT12g4ts36iE6w2xKN7H2COHI63HQgBG)
```弱监督```
本论文利用多标签分类，对源域进行弱监督，同时将分类概率融入对抗损失中，用MLP进行联合学习，并对弱监督标签进行一致性约束。

* Collaborative Training between Region Proposal Localization and Classification for Domain Adaptive Object Detection [[ECCV2020]](http://www.ecva.net/papers/eccv_2020/papers_ECCV/papers/123630086.pdf)[[code]](https://github.com/GanlongZhao/CST_DA_detection)[[wenote]](http://wenote.huawei.com/wapp/pages/view/share/s/0NhUxM0oTx7H2BiOcT12g4ts3movKw2yrx7H2UE0K32LSSa9)
```RPN&RPC``` ```MCD```
作者认为Faster RCNN中RPN和RPC并没有梯度传递，在进行域迁移任务时，会产生不同结果，在无监督情况下，会影响性能。作者提出对于目标域让RPN和RPC互相监督，利用maximized discrepancy classifier对RPN和RPC的输出进行一致性约束。

* Domain Adaptive Object Detection via Asymmetric Tri-way Faster-RCNN [[ECCV2020]](http://www.ecva.net/papers/eccv_2020/papers_ECCV/papers/123690307.pdf)[[wenote]](http://wenote.huawei.com/wapp/pages/view/share/s/0NhUxM0oTx7H2BiOcT12g4ts0OGVGg2xLh7H2UE0K32LSSa9)
```三路Faster RCCN``` ```缓解源域崩坏```
基于模型共享的对抗域对齐容易造成模型坏掉，在源域上失控的风险，带来特征迁移的负影响。作者提出用三路Faster RCNN缓解这一问题的同时，保证了提取目标的特征不仅是领域无关的，还是类别分离的。

* Every Pixel Matters: Center-aware Feature Alignment for Domain Adaptive Object Detector [[ECCV2020]](http://www.ecva.net/papers/eccv_2020/papers_ECCV/papers/123540698.pdf)[[code]](https://github.com/chengchunhsu/EveryPixelMatters)
```attention map加权```
首先，在image-level或者叫global-level进行**对抗的**特征对齐，注意这里是对每个像素点都进行域分类。其次，由于作者期望提高模型对前景的关注，减少背景信息带来的噪声，降低迁移效率，作者利用门控极值增加了一个模块来学习前景的注意力矩阵，并用这个注意力矩阵对feature map进行加权。最后，作者展示了注意力矩阵的可视化，模型确实关注到更多的前景信息。

* Prior-based Domain Adaptive Object Detection for Hazy and Rainy Conditions [[ECCV2020]](http://www.ecva.net/papers/eccv_2020/papers_ECCV/papers/123560392.pdf)
```soft domain label```
作者的出发点在于以往训练域分类器时标签均为0,1，但是对于天气变化这种相似度很高的场景，有雾的图片里距离相机近的部分和没有雾的图片之间相似度很高，如果这部分的特征再进行对齐，可能会导致负对齐。作者利用transmision map得到更细粒度的域标签。

* One-Shot Unsupervised Cross-Domain Detection [[ECCV2020]](http://www.ecva.net/papers/eccv_2020/papers_ECCV/papers/123610715.pdf)[[wenote]](http://wenote.huawei.com/wapp/pages/view/share/s/0NhUxM0oTx7H2BiOcT12g4ts3KcV9w2ykh7H2bZXTs1WER4j)
```自监督``` ```self-training```
本文关注工业端云协同场景下的目标检测算法面临的域偏移问题，相比于传统域迁移场景，此时目标域可能只有一张。作者结合自监督学习及self-training的思想来优化backbone，为了减少伪标签带来的影响，并没有更新检测头的参数。

* YOLO in the Dark - Domain Adaptation Method for Merging Multiple Models [[ECCV2020]](http://www.ecva.net/papers/eccv_2020/papers_ECCV/papers/123660341.pdf)
```知识蒸馏```
在低光照条件下视觉任务非常困难，本文利用知识蒸馏的方法，将多个不同域的模型进行合并，来提升目标域下检测模型的性能，同时为了解决缺少蒸馏需要的额外数据问题，作者设计了生成式模型来生成数据。

* Cross-domain Object Detection through Coarse-to-Fine Feature Adaptation [[CVPR2020]](https://openaccess.thecvf.com/content_CVPR_2020/papers/Zheng_Cross-domain_Object_Detection_through_Coarse-to-Fine_Feature_Adaptation_CVPR_2020_paper.pdf)[[wenote]](http://wenote.huawei.com/wapp/pages/view/share/s/0NhUxM0oTx7H2BiOcT12g4ts398mMw2yuN7H2bZXTs1WER4j)
```attention map``` ```prototype-based```
前景对于域迁移来说十分重要，一个好的域无关目标检测器，在做到提取域无关的前景特征的同时，还需要类别可区分的（可判别性）。可判别性指的是检测器定位和区分不同实例的能力。作者先利用特征图学习前景的权重，再利用原型的思想，计算对应类别的损失。

* Harmonizing Transferability and Discriminability for Adapting Object Detectors [[CVPR2020]](https://openaccess.thecvf.com/content_CVPR_2020/papers/Chen_Harmonizing_Transferability_and_Discriminability_for_Adapting_Object_Detectors_CVPR_2020_paper.pdf) [[code]](https://github.com/chaoqichen/HTCN)[[wenote]]()

<h2 id="4">4. datasets (数据集)</h2>

* pascal voc 2007+2012
* clipart+watercolor+comic
* cityscapes
* foggy cityscapes (需要申请才可以下载)
* KITTI
* SIM10K

<!--
<h2 id="5">5. leaderboard (排行榜)</h2>

### pascal voc->clipart
源域为pascal voc 2007+2012，目标域为clipart

|model|backbone|||

### Weather Adaptation
Cityscapes->Foggy Cityscapes

|model|backbone|person|rider|car|truck|bus|train|mbike|bicycle|<img src="https://render.githubusercontent.com/render/math?math=mAP_{50}">|
|:--|-|-|-|-|-|-|-|-|-|-|
|Faster RCNN|VGG-16|17.8|23.6|27.1|11.9|23.8|9.1|14.4|22.8|18.8|
|Every Pixel Matter|VGG-16|41.9|38.7|56.7|22.6|41.5|26.8|24.6|35.5|36.0|
|Every Pixel Matter|ResNet-101|41.5|43.6|57.1|29.4|44.9|39.7|29.0|36.1|40.2|

### Synthetic-to-real
Sim10k->Cityscapes

|model|backbone|<img src="https://render.githubusercontent.com/render/math?math=mAP_{50}">|
|:--|-|-|
|Faster RCNN|VGG-16|30.1|
|Every Pixel Matter|VGG-16|43.2|
|Every Pixel Matter|ResNet-101|45.0|
-->
