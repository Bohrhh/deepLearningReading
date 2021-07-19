# YOLOv5
> 目标检测最强实践


## 数据增强
- mosaic
- translation
- scale
- HSV
- flip left-right

mosaic数据增强是将4张图片的内容混合成一张图片，如下图所示。这样小物体的比重增加，有利于模型倾向于学习对小物体的检测。同样有利于模型学习对常规内容外的物体的检测。还有batch normalization能够统计到更多的图片，这大大减小了对batch size的要求。

<div style="text-align: center;">
    <div><font size=5>train batch 0</font></div>
    <img src=Detection/_images/YOLOv5_train_batch0.jpg width=70%/>
</div>


## 模型结构
参考文献
- YOLOv4: Optimal speed and accuracy of object detection. [arXiv 2020](https://arxiv.org/pdf/2004.10934)
- CSPNet: A new backbone that can enhance learning capability of cnn. [CVPR 2020](http://openaccess.thecvf.com/content_CVPRW_2020/papers/w28/Wang_CSPNet_A_New_Backbone_That_Can_Enhance_Learning_Capability_of_CVPRW_2020_paper.pdf)
- Path aggregation network for instance segmentation. [CVPR 2018](https://openaccess.thecvf.com/content_cvpr_2018/papers/Liu_Path_Aggregation_Network_CVPR_2018_paper.pdf)
- Spatial pyramid pooling in deep convolutional networks for visual recognition. [TPAMI 2015](https://link.springer.com/content/pdf/10.1007/978-3-319-10578-9_23.pdf)

yolov5的backbone借鉴CSPNet来提取特征。yolov5的neck借鉴PANet来做不同scale特征之间的融合，这一部分和UNet的结构也很类似了，甚至更一般的，可以多次重复这一模块，让信息在不同scale之间充分流动。

<div style="text-align: center;">
    <img src=Detection/_images/YOLOv5_structure.png width=100%/>
</div>


## 正样本选择
正样本就是那些负责预测回归target box的anchor box。下图中展示了不同分辨率的feature map下正样本的情况，黑色框为target box，彩色框为被选为正样本的anchor box，它的选择条件是  
- anchor box和target box对应长宽的比例在某一范围内
- anchor box所在的grid是与target box中心最近的3个grid

<div style="text-align: center;">
    <img src=Detection/_images/YOLOV5_positive_anchor_box.png width=100%/>
</div>

所以一个target box最多对应nl×na×3个anchor box，最少0个。其中nl（number of layers）是模型输出的feature map的个数，na（number of anchors）是每一层anchor box种类的数目（通常为3）。这种选择正样本的方式有三点需要注意  
- 1，不保证每一个target box都有anchor box负责预测
- 2，一个target box可以对应多个anchor box，anchor box可以来自同一层，也可以来自多个层
- 3，一个anchor box也许会对应多个target box，物体密集的时候更是如此。

这种方式，增加了正样本的数目，大大加快了模型的收敛。同时也引入了很多质量不高的anchor box和一对多的模糊定义（<font color='red'>这个是否可以仿照FCOS那样，给不同质量的anchor box赋予不同的权重</font>）。


## 损失函数

#### 1. box： [CIoU loss](https://ojs.aaai.org/index.php/AAAI/article/view/6999/6853)
发展过程
$IoU$ ---> $GIoU$ ---> $CIoU$

- **IoU(intersection over union) 为交并比**
<div style="text-align: center;">
    <img src=Detection/_images/YOLOv5_IoU.png width=45%/>
</div>

- **GIoU(generalized intersection over union)**  
当目标框和预测框没有交集时，IoU为0，这种情况无法通过梯度下降来优化。为了度量目标框和预测框没有交集时的损失，提出GIoU。计算公式如下所示，正是由于将第三项作为惩罚项，在目标框和预测框没有交集的时候，优化的结果会使得预测框向目标框移动。
$$\mathcal{L}_{GIoU}=1-IoU+\frac{|\mathcal{C}-\mathcal{B}\cup\mathcal{B}^{gt}|}{|\mathcal{C}|}$$
其中$\mathcal{B}$为预测框，$\mathcal{B}^{gt}$为目标框，$\mathcal{C}$为$\mathcal{B}$和$\mathcal{B}^{gt}$的最小外接框(如下图虚线框所示)，$|\mathcal{C}|$的意思是矩形框的面积。
<div style="text-align: center;">
    <img src=Detection/_images/YOLOv5_GIoU.png width=80%/>
</div>

- **CIoU(complete intersection over union)**  
GIoU存在的一个问题是收敛速度慢，当目标框和预测框靠近后，GIoU的第三项变成一个小量，所起到的作用也就不大了。CIoU考虑了更多的几何因素作为损失，而不仅仅是两个框之间的重叠区域。具体的
$$\mathcal{L}_{CIoU}=1-IoU+D(\mathcal{B}, \mathcal{B}^{gt})+V(\mathcal{B}, \mathcal{B}^{gt})$$
$$D(\mathcal{B}, \mathcal{B}^{gt})=\frac{d^2}{c^2}$$
$$V(\mathcal{B}, \mathcal{B}^{gt})=\frac{4}{\pi^2}(arctan\frac{w^{gt}}{h^{gt}}-arctan\frac{w}{h})^2$$
其中d和c如下图所示
<div style="text-align: center;">
    <img src=Detection/_images/YOLOv5_CIoU.png width=40%/>
</div>

#### 2. obj: binary cross entropy

#### 3. cls: binary cross entropy


## 评估指标
COCO的AP计算过程大致分为以下四步
- 以间隔0.05对在0.5~0.95内采样iou阈值。
- 在每个iou阈值下，计算PR曲线，得到AP值
- 将所有iou阈值下的AP值取平均
- 所有类别的AP值再取平均

更具体的可以看👉[here](https://jonathan-hui.medium.com/map-mean-average-precision-for-object-detection-45c121a31173)
<div style="text-align: center;">
    <img src=Detection/_images/YOLOv5_cocoAP.png width=85%/>
    <div>图片来源https://github.com/ultralytics/yolov5</div>
</div>


## 其他
- bias 和 BN weights没有decay， 其他的weights需要decay
- SGD nesterov=True
- ema
- accumulate gradient
- auto anchor


## 参考资料

- [yolov5](https://github.com/ultralytics/yolov5)



