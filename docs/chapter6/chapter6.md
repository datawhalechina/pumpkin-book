## 6.9-6.10
$$\begin{aligned}
w &= \sum_{i=1}^m\alpha_iy_i\boldsymbol{x}_i \\
0 &=\sum_{i=1}^m\alpha_iy_i
\end{aligned}​$$
[推导]：式（6.8）可作如下展开：
$$\begin{aligned}
L(\boldsymbol{w},b,\boldsymbol{\alpha}) &= \frac{1}{2}||\boldsymbol{w}||^2+\sum_{i=1}^m\alpha_i(1-y_i(\boldsymbol{w}^T\boldsymbol{x}_i+b)) \\
& =  \frac{1}{2}||\boldsymbol{w}||^2+\sum_{i=1}^m(\alpha_i-\alpha_iy_i \boldsymbol{w}^T\boldsymbol{x}_i-\alpha_iy_ib)\\
& =\frac{1}{2}\boldsymbol{w}^T\boldsymbol{w}+\sum_{i=1}^m\alpha_i -\sum_{i=1}^m\alpha_iy_i\boldsymbol{w}^T\boldsymbol{x}_i-\sum_{i=1}^m\alpha_iy_ib
\end{aligned}​$$
对$\boldsymbol{w}$和$b$分别求偏导数​并令其等于0：

$$\frac {\partial L}{\partial \boldsymbol{w}}=\frac{1}{2}\times2\times\boldsymbol{w} + 0 - \sum_{i=1}^{m}\alpha_iy_i \boldsymbol{x}_i-0= 0 \Longrightarrow \boldsymbol{w}=\sum_{i=1}^{m}\alpha_iy_i \boldsymbol{x}_i$$

$$\frac {\partial L}{\partial b}=0+0-0-\sum_{i=1}^{m}\alpha_iy_i=0  \Longrightarrow  \sum_{i=1}^{m}\alpha_iy_i=0$$		

## 6.11
$$\begin{aligned}
\max_{\boldsymbol{\alpha}} & \sum_{i=1}^m\alpha_i - \frac{1}{2}\sum_{i = 1}^m\sum_{j=1}^m\alpha_i \alpha_j y_iy_j\boldsymbol{x}_i^T\boldsymbol{x}_j \\
s.t. & \sum_{i=1}^m \alpha_i y_i =0 \\ 
& \alpha_i \geq 0 \quad i=1,2,\dots ,m
\end{aligned}$$  
[推导]：将式 (6.9)代人 (6.8) ，即可将$L(\boldsymbol{w},b,\boldsymbol{\alpha})$ 中的 $\boldsymbol{w}$ 和 $b$ 消去,再考虑式 (6.10) 的约束,就得到式 (6.6) 的对偶问题：
$$\begin{aligned}
\min_{\boldsymbol{w},b} L(\boldsymbol{w},b,\boldsymbol{\alpha})  &=\frac{1}{2}\boldsymbol{w}^T\boldsymbol{w}+\sum_{i=1}^m\alpha_i -\sum_{i=1}^m\alpha_iy_i\boldsymbol{w}^T\boldsymbol{x}_i-\sum_{i=1}^m\alpha_iy_ib \\
&=\frac {1}{2}\boldsymbol{w}^T\sum _{i=1}^m\alpha_iy_i\boldsymbol{x}_i-\boldsymbol{w}^T\sum _{i=1}^m\alpha_iy_i\boldsymbol{x}_i+\sum _{i=1}^m\alpha_
i -b\sum _{i=1}^m\alpha_iy_i \\
& = -\frac {1}{2}\boldsymbol{w}^T\sum _{i=1}^m\alpha_iy_i\boldsymbol{x}_i+\sum _{i=1}^m\alpha_i -b\sum _{i=1}^m\alpha_iy_i
\end{aligned}$$
又$\sum\limits_{i=1}^{m}\alpha_iy_i=0$，所以上式最后一项可化为0，于是得：
$$\begin{aligned}
\min_{\boldsymbol{w},b} L(\boldsymbol{w},b,\boldsymbol{\alpha}) &= -\frac {1}{2}\boldsymbol{w}^T\sum _{i=1}^m\alpha_iy_i\boldsymbol{x}_i+\sum _{i=1}^m\alpha_i \\
&=-\frac {1}{2}(\sum_{i=1}^{m}\alpha_iy_i\boldsymbol{x}_i)^T(\sum _{i=1}^m\alpha_iy_i\boldsymbol{x}_i)+\sum _{i=1}^m\alpha_i \\
&=-\frac {1}{2}\sum_{i=1}^{m}\alpha_iy_i\boldsymbol{x}_i^T\sum _{i=1}^m\alpha_iy_i\boldsymbol{x}_i+\sum _{i=1}^m\alpha_i \\
&=\sum _{i=1}^m\alpha_i-\frac {1}{2}\sum_{i=1 }^{m}\sum_{j=1}^{m}\alpha_i\alpha_jy_iy_j\boldsymbol{x}_i^T\boldsymbol{x}_j
\end{aligned}$$
所以
$$\max_{\boldsymbol{\alpha}}\min_{\boldsymbol{w},b} L(\boldsymbol{w},b,\boldsymbol{\alpha}) =\max_{\boldsymbol{\alpha}} \sum_{i=1}^m\alpha_i - \frac{1}{2}\sum_{i = 1}^m\sum_{j=1}^m\alpha_i \alpha_j y_iy_j\boldsymbol{x}_i^T\boldsymbol{x}_j $$
## 6.39
$$ C=\alpha_i +\mu_i $$
[推导]：对式（6.36）关于$\xi_i$求偏导并令其等于0可得：
​                                                     
$$\frac{\partial L}{\partial \xi_i}=0+C \times 1 - \alpha_i \times 1-\mu_i
\times 1 =0\Longrightarrow C=\alpha_i +\mu_i$$

## 6.40
$$\begin{aligned}
\max_{\boldsymbol{\alpha}}&\sum _{i=1}^m\alpha_i-\frac {1}{2}\sum_{i=1 }^{m}\sum_{j=1}^{m}\alpha_i\alpha_jy_iy_j\boldsymbol{x}_i^T\boldsymbol{x}_j \\
 s.t. &\sum_{i=1}^m \alpha_i y_i=0 \\ 
 &  0 \leq\alpha_i \leq C \quad i=1,2,\dots ,m
 \end{aligned}$$
将式6.37-6.39代入6.36可以得到6.35的对偶问题：
$$\begin{aligned}
 \min_{\boldsymbol{w},b,\boldsymbol{\xi}}L(\boldsymbol{w},b,\boldsymbol{\alpha},\boldsymbol{\xi},\boldsymbol{\mu}) &= \frac{1}{2}||\boldsymbol{w}||^2+C\sum_{i=1}^m \xi_i+\sum_{i=1}^m \alpha_i(1-\xi_i-y_i(\boldsymbol{w}^T\boldsymbol{x}_i+b))-\sum_{i=1}^m\mu_i \xi_i  \\
&=\frac{1}{2}||\boldsymbol{w}||^2+\sum_{i=1}^m\alpha_i(1-y_i(\boldsymbol{w}^T\boldsymbol{x}_i+b))+C\sum_{i=1}^m \xi_i-\sum_{i=1}^m \alpha_i \xi_i-\sum_{i=1}^m\mu_i \xi_i \\
& = -\frac {1}{2}\sum_{i=1}^{m}\alpha_iy_i\boldsymbol{x}_i^T\sum _{i=1}^m\alpha_iy_i\boldsymbol{x}_i+\sum _{i=1}^m\alpha_i +\sum_{i=1}^m C\xi_i-\sum_{i=1}^m \alpha_i \xi_i-\sum_{i=1}^m\mu_i \xi_i \\
&  = -\frac {1}{2}\sum_{i=1}^{m}\alpha_iy_i\boldsymbol{x}_i^T\sum _{i=1}^m\alpha_iy_i\boldsymbol{x}_i+\sum _{i=1}^m\alpha_i +\sum_{i=1}^m (C-\alpha_i-\mu_i)\xi_i \\
&=\sum _{i=1}^m\alpha_i-\frac {1}{2}\sum_{i=1 }^{m}\sum_{j=1}^{m}\alpha_i\alpha_jy_iy_j\boldsymbol{x}_i^T\boldsymbol{x}_j
\end{aligned}$$  
所以
$$\begin{aligned}
\max_{\boldsymbol{\alpha},\boldsymbol{\mu}} \min_{\boldsymbol{w},b,\boldsymbol{\xi}}L(\boldsymbol{w},b,\boldsymbol{\alpha},\boldsymbol{\xi},\boldsymbol{\mu})&=\max_{\boldsymbol{\alpha},\boldsymbol{\mu}}\sum _{i=1}^m\alpha_i-\frac {1}{2}\sum_{i=1 }^{m}\sum_{j=1}^{m}\alpha_i\alpha_jy_iy_j\boldsymbol{x}_i^T\boldsymbol{x}_j \\
&=\max_{\boldsymbol{\alpha}}\sum _{i=1}^m\alpha_i-\frac {1}{2}\sum_{i=1 }^{m}\sum_{j=1}^{m}\alpha_i\alpha_jy_iy_j\boldsymbol{x}_i^T\boldsymbol{x}_j 
\end{aligned}$$
又
$$\begin{aligned}
\alpha_i &\geq 0 \\
\mu_i &\geq 0 \\
C &= \alpha_i+\mu_i
\end{aligned}$$
消去$\mu_i$可得等价约束条件为：
$$0 \leq\alpha_i \leq C \quad i=1,2,\dots ,m$$


