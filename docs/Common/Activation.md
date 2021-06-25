# Activation

## ReLU
$$ f(x)= \begin{cases}
            x& x > 0 \\
            0& x \le 0
         \end{cases}$$
<div style="text-align: center;">
    <img src=Common/_images/ReLU.png width=80%/>
</div>


## LeakyReLU
$$ f(x)= \begin{cases}
            x& x > 0 \\
            \alpha x& x \le 0 \ 其中\alpha是固定值
         \end{cases}$$
<div style="text-align: center;">
    <img src=Common/_images/LeakyReLU.png width=80%/>
</div>

## PReLU
$$ f(x)= \begin{cases}
            x& x > 0 \\
            \alpha x& x \le 0 \ 其中\alpha是待学习的参数
         \end{cases}$$
<div style="text-align: center;">
    <img src=Common/_images/PReLU.png width=80%/>
</div>

## ReLU6
$$ f(x)= \begin{cases}
            0& x<0 \\
            x& 0\le x<6 \\
            6& 6\le x
         \end{cases}$$
<div style="text-align: center;">
    <img src=Common/_images/ReLU6.png width=80%/>
</div>

## SELU
$$ f(x)= \lambda \begin{cases}
            x& x > 0 \\
            \alpha e^x-\alpha& x \le 0
         \end{cases}$$
其中$\alpha=1.6732632423543772848170429916717$， $\lambda=1.0507009873554804934193349852946$
<div style="text-align: center;">
    <img src=Common/_images/SELU.png width=80%/>
</div>

## Swish
<font size=5>$$f(x)= x\ sigmoid(\beta x)$$</font>
<div style="text-align: center;">
    <img src=Common/_images/Swish.png width=80%/>
</div>

## Mish
$$f(x)= x\ tanh(ln(1+e^x))$$
<div style="text-align: center;">
    <img src=Common/_images/Mish.png width=80%/>
</div>