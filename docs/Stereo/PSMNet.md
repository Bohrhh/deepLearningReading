# PSMNet
> **Pyramid Stereo Matching Network**

## 简介 
模型结构简单直接。为了应对纹理少等病态场景（illposed regions），模型需要足够的上下文信息。为此PSMNet引入了两个主要的模块：spatial pyramid pooling（SPP）和3D CNN。SPP有利于得到不同尺度的全局信息，之后用这些信息组成4D的cost volume，然后通过3D CNN的aggregation。这个模型的缺点跟很多其他使用了3D CNN的模型一样，非常的占用显存，运行速度很慢，在输入图片尺寸为(384, 1242, 3)时，Titan-Xp GPU 需要0.41s的推理时间。


## 结构

<div style="text-align: center;">
    <img src=Stereo/_images/PSMNet_fig1.png width=100%/>
</div>

<font color="red">其中cost volume的计算方式如下图所示，</font>将left feature和right feature沿着channel方向concat。并且沿着width方向错位移动，如$d_i=1$时，left feature第一格置为零，并将right feature右移一格。最后将这些错位的feature沿着d的维度concat，得到cost volume的维度为(n,c,d,h,w)
<div style="text-align: center;">
    <img src=Stereo/_images/PSMNet_fig2.png width=100%/>
</div>


实现的代码如下
```python
n,c,h,w = left_feature.shape
cost = Variable(torch.FloatTensor(n, c*2, maxdisp//4,  h,  w).zero_()).cuda()

for i in range(maxdisp//4):
    if i > 0 :
        cost[:, :c, i, :,i:] = left_feature[:,:,:,i:]
        cost[:, c:, i, :,i:] = right_feature[:,:,:,:-i]
    else:
        cost[:, :c, i, :,:] = left_feature
        cost[:, c:, i, :,:] = right_feature
```

## 性能

#### 在sceneflow上的表现
<div style="text-align: center;">
    <img src=Stereo/_images/PSMNet_fig3.png width=60%/>
</div>

#### 在kitti上的表现
<div style="text-align: center;">
    <img src=Stereo/_images/PSMNet_fig4.png width=100%/>
</div>

