# 单目深度

#### 指标

- Absolute Relative Error
$$Abs\_Rel= \frac{1}{n}\sum{\left|\frac{y_{pred}-y_{gt}}{y_{gt}}\right|}$$

- Square Relative Error
$$Sq\_Rel = \frac{1}{n}\sum{\left(\frac{y_{pred}-y_{gt}}{y_{gt}}\right)^2}$$

- Root Mean Square Error
$$RMSE = \sqrt{\frac{1}{n}\sum{(y_{pred}-y_{gt})^2}}$$

- Root Mean Square Logarithmic Error
$$log\_RMSE = \sqrt{\frac{1}{n}\sum{(\log{y_{pred}}-\log{y_{gt}})^2}}$$

- Mean Absolute Error
$$MAE = \frac{1}{n}\sum{\left|y_{pred}-y_{gt}\right|}$$

- Accuracy  
精度指的是满足下述公式的像素点所占的比例。通常threshold T为 $[1.25, 1.25^2, 1.25^3]$。
$$max(\frac{y_{pred}}{y_{gt}}, \frac{y_{gt}}{y_{pred}}) = \delta < T $$