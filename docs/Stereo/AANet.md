# AANet
> **AANet: Adaptive Aggregation Network for Efficient Stereo Matching**

## 简介 

这篇文章也是想要将繁重的3D卷积给拿掉，和DispNetC一样的做法通过correlation的方式。
构建出来的costvolume是四维度的（n,disp,h,w），然后经过后续2D卷积的aggregation。
为了保持高精度，作者用了两种手段，一是使用可变形卷积（DCN），一是在aggregation阶段进行多尺度的交互，分别见图一图二。还有一种做法值得借鉴，特征提取为下采样3、6、12，而不是通常的2、4、8...。这样做在后续对特征的操作上可以节省1.5×1.5的计算量，对精度的影响不清楚（既然作者这么做了，应该是值得的）。

## 可变形卷积
> 可变形卷积善于抓住物体的形状特征，有利于物体边缘视差的预测

<div style="text-align: center;">
    <img src=Stereo/_images/AANet_fig1.png width=60%/>
</div>

## 结构
> 使用ISA和CSA在aggregation阶段使信息在多尺度上交互。

<div style="text-align: center;">
    <img src=Stereo/_images/AANet_fig2.png width=100%/>
</div>


## 性能

#### 在sceneflow上的表现
<div style="text-align: center;">
    <img src=Stereo/_images/AANet_fig3.png width=100%/>
</div>

#### 在kitti上的表现
<div style="text-align: center;">
    <img src=Stereo/_images/AANet_fig4.png width=80%/>
</div>

#### 消融实验
<div style="text-align: center;">
    <img src=Stereo/_images/AANet_fig5.png width=80%/>
</div>
