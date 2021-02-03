# WStereo
> **Wasserstein Distances for Stereo Disparity Estimation**

## 简介 
<div style="text-align: center;">
    <img src=Stereo/_images/Wasserstein_fig1.png width=60%/>
</div>

这篇文章解决了我的很多困惑，值得细读。它的方法使得在物体边缘区域，那些过渡点的预测更加准确。具体的，该模型的输出包括 probs(n,d,h,w)，offset(n,d,h,w)，d表示视差（或者深度）的维度。预测的视差可以表示为 

```python
index = torch.argmax(probs, dim=1, keepdim=True)
offset = torch.gather(offset, 1, index)
disp = index.float()+offset
```
那么，为什么这样的输出可以一定程度上解决边缘预测的难题呢？可以参见fig3。在物体边缘处的点既有可能是物体的点，也可能是背景的点，所以该点的预测值处于一个多模态，如蓝色线和绿色线所示。通常的预测是加权平均如红色Mean所示，然后和ground ture比较得到loss用来回归，这种回归方式，最终会使得边缘处的预测值趋于物体和背景真实值的平均。如果采用绿色线的预测方式，加之Wasserstein Distances作为loss回归训练，推理时就取绿色线最高处的值，这样就可以一定程度上缓解边缘预测的难题。这同样给下游任务，如3D目标检测带来了好处。如fig1所示，蓝色点在边缘处拉出长长的过渡区域，对3D框的判断产生了很大的困扰。红色点在边缘的过渡明显减少，更加接近黄色点即激光雷达数据。

<div style="text-align: center;">
    <img src=Stereo/_images/Wasserstein_fig3.png width=80%/>
</div>

## 结构
<div style="text-align: center;">
    <img src=Stereo/_images/Wasserstein_fig2.png width=100%/>
</div>

## 性能

<div style="text-align: center;">
    <img src=Stereo/_images/Wasserstein_table1.png width=100%/>
</div>
