# HD3
> **Hierarchical Discrete Distribution Decomposition for Match Density Estimation**

## 简介 

这篇文章提出的模型结构适用于预测视差和光流两种任务，这里我们主要讲视差的预测。
视差的预测过程事实上就是两张图片像素点之间的匹配过程，其中像素点之间的匹配会在两张图片的同一行中进行。
通常的搜索范围是192个像素，这样的搜索其实是费时的。这篇文章采用了从粗到细的预测方式(coarse to fine)，从低分辨率到高分辨率逐级微调，这也类似残差的思想。
每一次微调时，搜索的视差范围都不大，2-4个像素就够了。这样某种程度上可以减少计算量，然而我觉得提速并不明显。不过这种残差或者差分的思想确实会使得网络更容易训练，
更容易有好的结果。而且该网络全是2D卷积，在深度预测3D卷积用的飞起的时候，还能够用全2D卷积的网络达到如此优秀的结果是很不错的。
2D卷积的计算量和在工业上的加速是3D卷积不能比的，该网络稍微精简一下可以在速度上有更好的表现。

## 结构
<div style="text-align: center;">
    <img src=Stereo/_images/HD3_fig1.png width=100%/>
</div>

## 性能
<div style="text-align: center;">
    <img src=Stereo/_images/HD3_fig2.png width=60%/>
</div>
  
<div style="text-align: center;">
    <img src=Stereo/_images/HD3_fig3.png width=100%/>
</div>
