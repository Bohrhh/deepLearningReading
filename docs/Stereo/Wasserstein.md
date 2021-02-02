# WStereo
> **Wasserstein Distances for Stereo Disparity Estimation**

## 简介 

这篇文章解决了我的很多困惑，值得细读。它的方法使得在物体边缘区域，那些过渡点的预测更加准确。具体的，该模型的输出包括 probs(n,d,h,w)，offset(n,d,h,w)，d表示视差（或者深度）的维度。预测的视差可以表示为 

```python
index = torch.argmax(probs, dim=1, keepdim=True)
offset = torch.gather(offset, 1, index)
disp = index.float()+offset
```
那么，为什么这样的输出可以一定程度上解决边缘预测的难题呢？可以参见fig3。在物体边缘处的点既有可能是物体的点，也可能是背景的点，所以该点的预测值处于一个多模态，如蓝色线和绿色线所示。通常的预测是加权平均如红色Mean所示，然后和ground ture比较得到cost用来回归，这种回归方式，最终会使得边缘处的预测值趋于物体和背景真实值的平均。如果采用绿色线的预测方式，加之Wasserstein Distances作为cost回归，就可以一定程度上缓解边缘预测的难题。


<div style="text-align: center;">
    <img src=Stereo/_images/Wasserstein_fig3.png width=100%/>
</div>