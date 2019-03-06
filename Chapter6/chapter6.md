### 6.9-6.10
$$\begin{aligned}
w &= \sum\_{i=1}^m\alpha\_iy\_i\boldsymbol{x}\_i \\\\
0 &=\sum\_{i=1}^m\alpha\_iy\_i
\end{aligned}​$$
[推导]：式（6.8）可作如下展开：
$$\begin{aligned}
L(\boldsymbol{w},b,\boldsymbol{\alpha}) &= \frac{1}{2}||\boldsymbol{w}||^2+\sum\_{i=1}^m\alpha\_i(1-y\_i(\boldsymbol{w}^T\boldsymbol{x}\_i+b)) \\\\
& =  \frac{1}{2}||\boldsymbol{w}||^2+\sum\_{i=1}^m(\alpha\_i-\alpha\_iy\_i \boldsymbol{w}^T\boldsymbol{x}\_i-\alpha\_iy\_ib)\\\\
& =\frac{1}{2}\boldsymbol{w}^T\boldsymbol{w}+\sum\_{i=1}^m\alpha\_i -\sum\_{i=1}^m\alpha\_iy\_i\boldsymbol{w}^T\boldsymbol{x}\_i-\sum\_{i=1}^m\alpha\_iy\_ib
\end{aligned}​$$
对$\boldsymbol{w}$和$b$分别求偏导数​并令其等于0：

$$\frac {\partial L}{\partial \boldsymbol{w}}=\frac{1}{2}\times2\times\boldsymbol{w} + 0 - \sum\_{i=1}^{m}\alpha\_iy\_i \boldsymbol{x}\_i-0= 0 \Longrightarrow \boldsymbol{w}=\sum\_{i=1}^{m}\alpha\_iy\_i \boldsymbol{x}\_i$$

$$\frac {\partial L}{\partial b}=0+0-0-\sum\_{i=1}^{m}\alpha\_iy\_i=0  \Longrightarrow  \sum\_{i=1}^{m}\alpha\_iy\_i=0$$		

### 6.11
$$\begin{aligned}
\max\_{\boldsymbol{\alpha}} & \sum\_{i=1}^m\alpha\_i - \frac{1}{2}\sum\_{i = 1}^m\sum\_{j=1}^m\alpha\_i \alpha\_j y\_iy\_j\boldsymbol{x}\_i^T\boldsymbol{x}\_j \\\\
s.t. & \sum\_{i=1}^m \alpha\_i y\_i =0 \\\\ 
& \alpha\_i \geq 0 \quad i=1,2,\dots ,m
\end{aligned}$$  
[推导]：将式 (6.9)代人 (6.8) ，即可将$L(\boldsymbol{w},b,\boldsymbol{\alpha})$ 中的 $\boldsymbol{w}$ 和 $b$ 消去,再考虑式 (6.10) 的约束,就得到式 (6.6) 的对偶问题：
$$\begin{aligned}
\min\_{\boldsymbol{w},b} L(\boldsymbol{w},b,\boldsymbol{\alpha})  &=\frac{1}{2}\boldsymbol{w}^T\boldsymbol{w}+\sum\_{i=1}^m\alpha\_i -\sum\_{i=1}^m\alpha\_iy\_i\boldsymbol{w}^T\boldsymbol{x}\_i-\sum\_{i=1}^m\alpha\_iy\_ib \\\\
&=\frac {1}{2}\boldsymbol{w}^T\sum \_{i=1}^m\alpha\_iy\_i\boldsymbol{x}\_i-\boldsymbol{w}^T\sum \_{i=1}^m\alpha\_iy\_i\boldsymbol{x}\_i+\sum \_{i=1}^m\alpha\_
i -b\sum \_{i=1}^m\alpha\_iy\_i \\\\
& = -\frac {1}{2}\boldsymbol{w}^T\sum \_{i=1}^m\alpha\_iy\_i\boldsymbol{x}\_i+\sum \_{i=1}^m\alpha\_i -b\sum \_{i=1}^m\alpha\_iy\_i
\end{aligned}$$
又$\sum\limits\_{i=1}^{m}\alpha\_iy\_i=0$，所以上式最后一项可化为0，于是得：
$$\begin{aligned}
\min\_{\boldsymbol{w},b} L(\boldsymbol{w},b,\boldsymbol{\alpha}) &= -\frac {1}{2}\boldsymbol{w}^T\sum \_{i=1}^m\alpha\_iy\_i\boldsymbol{x}\_i+\sum \_{i=1}^m\alpha\_i \\\\
&=-\frac {1}{2}(\sum\_{i=1}^{m}\alpha\_iy\_i\boldsymbol{x}\_i)^T(\sum \_{i=1}^m\alpha\_iy\_i\boldsymbol{x}\_i)+\sum \_{i=1}^m\alpha\_i \\\\
&=-\frac {1}{2}\sum\_{i=1}^{m}\alpha\_iy\_i\boldsymbol{x}\_i^T\sum \_{i=1}^m\alpha\_iy\_i\boldsymbol{x}\_i+\sum \_{i=1}^m\alpha\_i \\\\
&=\sum \_{i=1}^m\alpha\_i-\frac {1}{2}\sum\_{i=1 }^{m}\sum\_{j=1}^{m}\alpha\_i\alpha\_jy\_iy\_j\boldsymbol{x}\_i^T\boldsymbol{x}\_j
\end{aligned}$$
所以
$$\max\_{\boldsymbol{\alpha}}\min\_{\boldsymbol{w},b} L(\boldsymbol{w},b,\boldsymbol{\alpha}) =\max\_{\boldsymbol{\alpha}} \sum\_{i=1}^m\alpha\_i - \frac{1}{2}\sum\_{i = 1}^m\sum\_{j=1}^m\alpha\_i \alpha\_j y\_iy\_j\boldsymbol{x}\_i^T\boldsymbol{x}\_j $$
### 6.39
$$ C=\alpha\_i +\mu\_i $$
[推导]：对式（6.36）关于$\xi\_i$求偏导并令其等于0可得：
​                                                     
$$\frac{\partial L}{\partial \xi\_i}=0+C \times 1 - \alpha\_i \times 1-\mu\_i
\times 1 =0\Longrightarrow C=\alpha\_i +\mu\_i$$

### 6.40
$$\begin{aligned}
\max\_{\boldsymbol{\alpha}}&\sum \_{i=1}^m\alpha\_i-\frac {1}{2}\sum\_{i=1 }^{m}\sum\_{j=1}^{m}\alpha\_i\alpha\_jy\_iy\_j\boldsymbol{x}\_i^T\boldsymbol{x}\_j \\\\
 s.t. &\sum\_{i=1}^m \alpha\_i y\_i=0 \\\\ 
 &  0 \leq\alpha\_i \leq C \quad i=1,2,\dots ,m
 \end{aligned}$$
将式6.37-6.39代入6.36可以得到6.35的对偶问题：
$$\begin{aligned}
 \min\_{\boldsymbol{w},b,\boldsymbol{\xi}}L(\boldsymbol{w},b,\boldsymbol{\alpha},\boldsymbol{\xi},\boldsymbol{\mu}) &= \frac{1}{2}||\boldsymbol{w}||^2+C\sum\_{i=1}^m \xi\_i+\sum\_{i=1}^m \alpha\_i(1-\xi\_i-y\_i(\boldsymbol{w}^T\boldsymbol{x}\_i+b))-\sum\_{i=1}^m\mu\_i \xi\_i  \\\\
&=\frac{1}{2}||\boldsymbol{w}||^2+\sum\_{i=1}^m\alpha\_i(1-y\_i(\boldsymbol{w}^T\boldsymbol{x}\_i+b))+C\sum\_{i=1}^m \xi\_i-\sum\_{i=1}^m \alpha\_i \xi\_i-\sum\_{i=1}^m\mu\_i \xi\_i \\\\
& = -\frac {1}{2}\sum\_{i=1}^{m}\alpha\_iy\_i\boldsymbol{x}\_i^T\sum \_{i=1}^m\alpha\_iy\_i\boldsymbol{x}\_i+\sum \_{i=1}^m\alpha\_i +\sum\_{i=1}^m C\xi\_i-\sum\_{i=1}^m \alpha\_i \xi\_i-\sum\_{i=1}^m\mu\_i \xi\_i \\\\
&  = -\frac {1}{2}\sum\_{i=1}^{m}\alpha\_iy\_i\boldsymbol{x}\_i^T\sum \_{i=1}^m\alpha\_iy\_i\boldsymbol{x}\_i+\sum \_{i=1}^m\alpha\_i +\sum\_{i=1}^m (C-\alpha\_i-\mu\_i)\xi\_i \\\\
&=\sum \_{i=1}^m\alpha\_i-\frac {1}{2}\sum\_{i=1 }^{m}\sum\_{j=1}^{m}\alpha\_i\alpha\_jy\_iy\_j\boldsymbol{x}\_i^T\boldsymbol{x}\_j
\end{aligned}$$  
所以
$$\begin{aligned}
\max\_{\boldsymbol{\alpha},\boldsymbol{\mu}} \min\_{\boldsymbol{w},b,\boldsymbol{\xi}}L(\boldsymbol{w},b,\boldsymbol{\alpha},\boldsymbol{\xi},\boldsymbol{\mu})&=\max\_{\boldsymbol{\alpha},\boldsymbol{\mu}}\sum \_{i=1}^m\alpha\_i-\frac {1}{2}\sum\_{i=1 }^{m}\sum\_{j=1}^{m}\alpha\_i\alpha\_jy\_iy\_j\boldsymbol{x}\_i^T\boldsymbol{x}\_j \\\\
&=\max\_{\boldsymbol{\alpha}}\sum \_{i=1}^m\alpha\_i-\frac {1}{2}\sum\_{i=1 }^{m}\sum\_{j=1}^{m}\alpha\_i\alpha\_jy\_iy\_j\boldsymbol{x}\_i^T\boldsymbol{x}\_j 
\end{aligned}$$
又
$$\begin{aligned}
\alpha\_i &\geq 0 \\\\
\mu\_i &\geq 0 \\\\
C &= \alpha\_i+\mu\_i
\end{aligned}$$
消去$\mu\_i$可得等价约束条件为：
$$0 \leq\alpha\_i \leq C \quad i=1,2,\dots ,m$$


