# domain-adaptation-object-detection
awesome domain adaptation works with interpretations for object detection.
关于领域自适应，重点关注目标检测中的领域自适应任务，包括：相关资源、综述、代表工作及其代码，常见域迁移场景及数据集等等。

# Content
* [1. resources (相关资源)](#1)
* [2. surveys (综述)](#2)
* [3. papers (代表工作)](#3)
* [4. datasets (数据集)](#4)
* [5. leaderboard (排行榜)](#5)

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
* Every Pixel Matters: Center-aware Feature Alignment for Domain Adaptive Object Detector [[ECCV2020]](http://www.ecva.net/papers/eccv_2020/papers_ECCV/papers/123540698.pdf)[[code]](https://github.com/chengchunhsu/EveryPixelMatters)

首先，在image-level或者叫global-level进行**对抗的**特征对齐，注意这里是对每个像素点都进行域分类。其次，由于作者期望提高模型对前景的关注，减少背景信息带来的噪声，降低迁移效率，作者利用门控极值增加了一个模块来学习前景的注意力矩阵，并用这个注意力矩阵对feature map进行加权。最后，作者展示了注意力矩阵的可视化，模型确实关注到更多的前景信息。

* Adapting Object Detectors with Conditional Domain Normalization [[ECCV2020]](http://www.ecva.net/papers/eccv_2020/papers_ECCV/papers/123560392.pdf)

作者给源域学习了一个**域向量(embedding)** ，并用类似于attention的思想，用这个域向量来查询目标域的表征，从目标域中提取域属性，最后再进行特征分布对齐，即进行对抗学习。这样可能的效果是降低域对齐的难度，因为生成的源域和目标域的表征更加接近。该方法在cityscape->foggy场景下相比于同年的CVPR提升并没有很大。

* Adaptive Object Detection with Dual Multi-Label Prediction [[ECCV2020]](https://www.ecva.net/papers/eccv_2020/papers_ECCV/papers/123730052.pdf)

本论文利用多标签分类，对源域进行弱监督，同时将分类概率融入对抗损失中，用MLP进行联合学习，并对弱监督标签进行一致性约束。

<h2 id="4">4. datasets (数据集)</h2>

* pascal voc 2007+2012
* clipart+watercolor+comic
* cityscapes
* foggy cityscapes (需要申请才可以下载)
* KITTI
* SIM10K

<h2 id="5">5. leaderboard (排行榜)</h2>

### pascal voc->clipart
源域为pascal voc 2007+2012，目标域为clipart
|model|backbone|||
