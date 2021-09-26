# PP-OCRv2 五大关键技术点解读
> **资料来源 [知乎](https://zhuanlan.zhihu.com/p/408148217)**

## 简介 
全新升级的 PP-OCRv2 版本，整体的框架图保持了与 PP-OCR 相同的 Pipeline，如下图所示。
在优化策略方面，主要从五个角度进行了深入优化（如上图红框所示），主要包括：
- 检测模型优化：采用 CML 知识蒸馏策略
- 检测模型优化：CopyPaste 数据增广策略
- 识别模型优化：LCNet 轻量级骨干网络
- 识别模型优化：UDML 知识蒸馏策略
- 识别模型优化：Enhanced CTC loss 改进


## 检测模型优化 采用 CML 协同互学习知识蒸馏策略。
如上图所示，CML 的核心思想结合了①传统的 Teacher 指导 Student 的标准蒸馏与 ② Students 网络直接的 DML 互学习，可以让 Students 网络互学习的同时，Teacher 网络予以指导。对应的，精心设计关键的三个 Loss 损失函数：GT Loss、DML Loss 和 Distill Loss，在 Teacher 网络 Backbone 为 ResNet18 的条件下，对 Student 的 MobileNetV3 起到了良好的提升效果。

## 检测模型优化：CopyPaste 数据增广策略
数据增广是提升模型泛化能力重要的手段之一，CopyPaste 是一种新颖的数据增强技巧，已经在目标检测和实例分割任务中验证了有效性。利用 CopyPaste，可以合成文本实例来平衡训练图像中的正负样本之间的比例。相比而言，传统图像旋转、随机翻转和随机裁剪是无法做到的。

CopyPaste 主要步骤包括：①随机选择两幅训练图像，②随机尺度抖动缩放，③随机水平翻转，④随机选择一幅图像中的目标子集，⑤粘贴在另一幅图像中随机的位置。这样，就比较好的提升了样本丰富度，同时也增加了模型对环境鲁棒性。

## 识别模型优化：LCNet 轻量级骨干网络

这里，PP-OCRv2 的研发团队提出了一种基于 MobileNetV1 改进的新的骨干网络 LCNet，主要的改动包括：

①除 SE 模块，网络中所有的 relu 替换为 h-swish，精度提升1%-2%②LCNet 第五阶段，DW 的 kernel size变为5x5，精度提升0.5%-1%③LCNet 第五阶段的最后两个 DepthSepConv block 添加 SE 模块， 精度提升0.5%-1%④GAP 后添加1280维的 FC 层，增加特征表达能力，精度提升2%-3%

## 识别模型优化：UDML 知识蒸馏策略

在标准的 DML 知识蒸馏的基础上，新增引入了对于 Feature Map 的监督机制，新增 Feature Loss，增加迭代次数，在 Head 部分增加额外的 FC 网络，最终加快蒸馏的速度同时提升效果。

## 识别模型优化：Enhanced CTC loss 改进

考虑到中文 OCR 任务经常遇到的识别难点是相似字符数太多，容易误识，借鉴 Metric Learning 的想法，引入 Center Loss，进一步增大类间距离，核心思路如上图公式所示。