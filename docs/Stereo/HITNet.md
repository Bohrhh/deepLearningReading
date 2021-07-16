# HITNet
> **HITNet: Hierarchical Iterative Tile Refinement Network for Real-time Stereo Matching**

## 简介
双目立体视觉可以用来测距，在自动驾驶、三维重建等领域有广泛的应用前景。这是出自google的文章，据我所知，google上一篇双目立体视觉要追溯到2018年的StereoNet，google旗下的waymo自动驾驶公司是走激光雷达路线的（激光雷达也是用来测距的），当然立体视觉和激光雷达此两种解决方案各自有优缺点，具体的不在此处讨论。对于自动驾驶来说，传感器的冗余，也是提高安全性的一种手段。

这篇文章的侧重点是要构建一个同时兼顾速度和精度的模型。为了达到此目的，该模型抛弃了大计算量的3D卷积操作，转而依靠快速的视差初始化步骤、可微的2D几何信息传播和对齐特征（warping）。它的灵感来自于传统立体匹配中的[实践](https://www.microsoft.com/en-us/research/wp-content/uploads/2016/02/Scharstein-IJCV02.pdf)，结合深度学习中可学习的特征，构建了一个端到端的模型。看了文章，在实现细节上面还是挺复杂的。

## 结构

## 性能