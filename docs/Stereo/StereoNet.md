# StereoNet
> **StereoNet: Guided Hierarchical Refinement for Real-Time Edge-Aware Depth Prediction**

## 简介
提出一个端到端的实时立体匹配神经网络，在Nvidia Titan X上速度可达60FPS。本文主要的特点是视差的预测精确到子像素，不像传统方法用WTA（winner takes all）。本文还提出一个观点，用低分辨率的特征来构造代价空间，就已经拥有足够的信息来预测得到精确的视差，后续细节的恢复可以通过一个可学习的边缘感知的上采样操作来完成（learned edge-aware upsampling）。

在数据增强上值得借鉴的是，它会随机的将右图的某些区域用右图的其他区域替换。这可以增强模型的视差补全能力。

## 结构
如下图所示，模型在下采样8倍的特征上构建了代价空间，经过代价聚合，后续通过层级的refinement来恢复视差空间的细节。
<div style="text-align: center;">
    <img src=Stereo/_images/StereoNet_fig1.png width=100%/>
</div>


## 性能

#### 在sceneflow上的表现
<div style="text-align: center;">
    <img src=Stereo/_images/StereoNet_fig2.png width=80%/>
</div>

#### 在kitti上的表现
<div style="text-align: center;">
    <img src=Stereo/_images/StereoNet_fig3.png width=80%/>
</div>