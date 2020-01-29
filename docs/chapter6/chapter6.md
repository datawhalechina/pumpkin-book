## 6.3
$$
\left\{\begin{array}{ll}{\boldsymbol{w}^{\mathrm{T}} \boldsymbol{x}_{i}+b \geqslant+1,} & {y_{i}=+1} \\ {\boldsymbol{w}^{\mathrm{T}} \boldsymbol{x}_{i}+b \leqslant-1,} & {y_{i}=-1}\end{array}\right.
$$
[推导]：假设这个超平面是$\left(\boldsymbol{w}^{\prime}\right)^{\top} \boldsymbol{x}+b^{\prime}=0$，对于$\left(\boldsymbol{x}_{i}, y_{i}\right) \in D$，有：
$$
\left\{\begin{array}{ll}{\left(\boldsymbol{w}^{\prime}\right)^{\top} \boldsymbol{x}_{i}+b^{\prime}>0,} & {y_{i}=+1} \\ {\left(\boldsymbol{w}^{\prime}\right)^{\top} \boldsymbol{x}_{i}+b^{\prime}<0,} & {y_{i}=-1}\end{array}\right.
$$
根据几何间隔，将以上关系修正为：
$$
\left\{\begin{array}{ll}{\left(\boldsymbol{w}^{\prime}\right)^{\top} \boldsymbol{x}_{i}+b^{\prime} \geq+\zeta,} & {y_{i}=+1} \\ {\left(\boldsymbol{w}^{\prime}\right)^{\top} \boldsymbol{x}_{i}+b^{\prime} \leq-\zeta,} & {y_{i}=-1}\end{array}\right.
$$
其中$\zeta$为某个大于零的常数，两边同除以$\zeta$，再次修正以上关系为：
$$
\left\{\begin{array}{ll}{\left(\frac{1}{\zeta} \boldsymbol{w}^{\prime}\right)^{\top} \boldsymbol{x}_{i}+\frac{b^{\prime}}{\zeta} \geq+1,} & {y_{i}=+1} \\ {\left(\frac{1}{\zeta} \boldsymbol{w}^{\prime}\right)^{\top} \boldsymbol{x}_{i}+\frac{b^{\prime}}{\zeta} \leq-1,} & {y_{i}=-1}\end{array}\right.
$$
令：$\boldsymbol{w}=\frac{1}{\zeta} \boldsymbol{w}^{\prime}, b=\frac{b^{\prime}}{\zeta}$，则以上关系可写为：
$$
\left\{\begin{array}{ll}{\boldsymbol{w}^{\top} \boldsymbol{x}_{i}+b \geq+1,} & {y_{i}=+1} \\ {\boldsymbol{w}^{\top} \boldsymbol{x}_{i}+b \leq-1,} & {y_{i}=-1}\end{array}\right.
$$

## 6.8
$$
L(\boldsymbol{w}, b, \boldsymbol{\alpha})=\frac{1}{2}\|\boldsymbol{w}\|^{2}+\sum_{i=1}^{m} \alpha_{i}\left(1-y_{i}\left(\boldsymbol{w}^{\top} \boldsymbol{x}_{i}+b\right)\right)
$$
[推导]：
待求目标:
$$\begin{aligned}
\min_{\boldsymbol{x}}\quad f(\boldsymbol{x})\\
s.t.\quad h(\boldsymbol{x})&=0\\
g(\boldsymbol{x}) &\leq 0
\end{aligned}$$

等式约束和不等式约束：$h(\boldsymbol{x})=0, g(\boldsymbol{x}) \leq 0$分别是由一个等式方程和一个不等式方程组成的方程组。

拉格朗日乘子：$\boldsymbol{\lambda}=\left(\lambda_{1}, \lambda_{2}, \ldots, \lambda_{m}\right)$  $\qquad\boldsymbol{\mu}=\left(\mu_{1}, \mu_{2}, \ldots, \mu_{n}\right)$

拉格朗日函数：$L(\boldsymbol{x}, \boldsymbol{\lambda}, \boldsymbol{\mu})=f(\boldsymbol{x})+\boldsymbol{\lambda} h(\boldsymbol{x})+\boldsymbol{\mu} g(\boldsymbol{x})$

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
[推导]：将式 (6.9)代入 (6.8) ，即可将$L(\boldsymbol{w},b,\boldsymbol{\alpha})$ 中的 $\boldsymbol{w}$ 和 $b$ 消去,再考虑式 (6.10) 的约束,就得到式 (6.6) 的对偶问题：
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

## 6.52
$$
\left\{\begin{array}{l}
{\alpha_{i}\left(f\left(\boldsymbol{x}_{i}\right)-y_{i}-\epsilon-\xi_{i}\right)=0} \\ {\hat{\alpha}_{i}\left(y_{i}-f\left(\boldsymbol{x}_{i}\right)-\epsilon-\hat{\xi}_{i}\right)=0} \\ {\alpha_{i} \hat{\alpha}_{i}=0, \xi_{i} \hat{\xi}_{i}=0} \\ 
{\left(C-\alpha_{i}\right) \xi_{i}=0,\left(C-\hat{\alpha}_{i}\right) \hat{\xi}_{i}=0}
\end{array}\right.
$$
[推导]：
将式（6.45）的约束条件全部恒等变形为小于等于0的形式可得：
$$
\left\{\begin{array}{l}
{f\left(\boldsymbol{x}_{i}\right)-y_{i}-\epsilon-\xi_{i} \leq 0 }  \\ 
{y_{i}-f\left(\boldsymbol{x}_{i}\right)-\epsilon-\hat{\xi}_{i} \leq 0 } \\ 
{-\xi_{i} \leq 0} \\
{-\hat{\xi}_{i} \leq 0}
\end{array}\right.
$$
由于以上四个约束条件的拉格朗日乘子分别为$\alpha_i,\hat{\alpha}_i,\mu_i,\hat{\mu}_i$，所以由西瓜书附录式（B.3）可知，以上四个约束条件可相应转化为以下KKT条件：
$$
\left\{\begin{array}{l}
{\alpha_i\left(f\left(\boldsymbol{x}_{i}\right)-y_{i}-\epsilon-\xi_{i} \right) = 0 }  \\ 
{\hat{\alpha}_i\left(y_{i}-f\left(\boldsymbol{x}_{i}\right)-\epsilon-\hat{\xi}_{i} \right) = 0 } \\ 
{-\mu_i\xi_{i} = 0 \Rightarrow \mu_i\xi_{i} = 0 }  \\
{-\hat{\mu}_i \hat{\xi}_{i} = 0  \Rightarrow \hat{\mu}_i \hat{\xi}_{i} = 0 }
\end{array}\right.
$$
由式（6.49）和式（6.50）可知：
$$
\begin{aligned}
\mu_i=C-\alpha_i \\
\hat{\mu}_i=C-\hat{\alpha}_i
\end{aligned}
$$
所以上述KKT条件可以进一步变形为：
$$
\left\{\begin{array}{l}
{\alpha_i\left(f\left(\boldsymbol{x}_{i}\right)-y_{i}-\epsilon-\xi_{i} \right) = 0 }  \\ 
{\hat{\alpha}_i\left(y_{i}-f\left(\boldsymbol{x}_{i}\right)-\epsilon-\hat{\xi}_{i} \right) = 0 } \\ 
{(C-\alpha_i)\xi_{i} = 0 }  \\
{(C-\hat{\alpha}_i) \hat{\xi}_{i} = 0 }
\end{array}\right.
$$
又因为样本$(\boldsymbol{x}_i,y_i)$只可能处在间隔带的某一侧，那么约束条件$f\left(\boldsymbol{x}_{i}\right)-y_{i}-\epsilon-\xi_{i}=0$和$y_{i}-f\left(\boldsymbol{x}_{i}\right)-\epsilon-\hat{\xi}_{i}=0$不可能同时成立，所以$\alpha_i$和$\hat{\alpha}_i$中至少有一个为0，也即$\alpha_i\hat{\alpha}_i=0$。在此基础上再进一步分析可知，如果$\alpha_i=0$的话，那么根据约束$(C-\alpha_i)\xi_{i} = 0$可知此时$\xi_i=0$，同理，如果$\hat{\alpha}_i=0$的话，那么根据约束$(C-\hat{\alpha}_i)\hat{\xi}_{i} = 0$可知此时$\hat{\xi}_i=0$，所以$\xi_i$和$\hat{\xi}_i$中也是至少有一个为0，也即$\xi_{i} \hat{\xi}_{i}=0$。将$\alpha_i\hat{\alpha}_i=0,\xi_{i} \hat{\xi}_{i}=0$整合进上述KKT条件中即可得到式（6.52）。

## 6.57
$$\min _{h \in \mathbb{H}} F(h)=\Omega\left(\|h\|_{\mathbb{H}}\right)+\ell\left(h\left(\boldsymbol{x}_{1}\right), h\left(\boldsymbol{x}_{2}\right), \ldots, h\left(\boldsymbol{x}_{m}\right)\right)$$
[解析]：略

## 6.58
$$h^{*}(\boldsymbol{x})=\sum_{i=1}^{m} \alpha_{i} \kappa\left(\boldsymbol{x}, \boldsymbol{x}_{i}\right)$$
[解析]：略

## 6.59
$$h(\boldsymbol{x})=\boldsymbol{w}^{\mathrm{T}}\phi(\boldsymbol{x})$$
[解析]：由于书上已经交代公式(6.60)是公式(3.35)引入核函数后的形式，而公式(3.35)是二分类LDA的损失函数，并且此式为直线方程，所以此时讨论的KLDA应当也是二分类KLDA。那么此公式就类似于第3章图3.3里的$y=\boldsymbol{w}^{\mathrm{T}}\boldsymbol{x}$，表示的是二分类KLDA中所要求解的那条投影直线。

## 6.60
$$\max _{\boldsymbol{w}} J(\boldsymbol{w})=\frac{\boldsymbol{w}^{\mathrm{T}} \mathbf{S}_{b}^{\phi} \boldsymbol{w}}{\boldsymbol{w}^{\mathrm{T}} \mathbf{S}_{w}^{\phi} \boldsymbol{w}}$$
[解析]：类似于第3章的公式(3.35)。

## 6.61
$$\boldsymbol{\mu}_{i}^{\phi}=\frac{1}{m_{i}} \sum_{\boldsymbol{x} \in X_{i}} \phi(\boldsymbol{x})$$
[解析]：略

## 6.62
$$\mathbf{S}_{b}^{\phi}=\left(\boldsymbol{\mu}_{1}^{\phi}-\boldsymbol{\mu}_{0}^{\phi}\right)\left(\boldsymbol{\mu}_{1}^{\phi}-\boldsymbol{\mu}_{0}^{\phi}\right)^{\mathrm{T}}$$
[解析]：类似于第3章的公式(3.34)。

## 6.63
$$\mathbf{S}_{w}^{\phi}=\sum_{i=0}^{1} \sum_{\boldsymbol{x} \in X_{i}}\left(\phi(\boldsymbol{x})-\boldsymbol{\mu}_{i}^{\phi}\right)\left(\phi(\boldsymbol{x})-\boldsymbol{\mu}_{i}^{\phi}\right)^{\mathrm{T}}$$
[解析]：类似于第3章的公式(3.33)。

## 6.64
$$h(\boldsymbol{x})=\sum_{i=1}^{m} \alpha_{i} \kappa\left(\boldsymbol{x}, \boldsymbol{x}_{i}\right)$$
[解析]：略

## 6.65
$$\boldsymbol{w}=\sum_{i=1}^{m} \alpha_{i} \phi\left(\boldsymbol{x}_{i}\right)$$
[推导]：由表示定理可知，此时二分类KLDA最终求得的投影直线方程总可以写成如下形式
$$h(\boldsymbol{x})=\sum_{i=1}^{m} \alpha_{i} \kappa\left(\boldsymbol{x}, \boldsymbol{x}_{i}\right)$$
又因为直线方程的固定形式为
$$h(\boldsymbol{x})=\boldsymbol{w}^{\mathrm{T}}\phi(\boldsymbol{x})$$
所以
$$\boldsymbol{w}^{\mathrm{T}}\phi(\boldsymbol{x})=\sum_{i=1}^{m} \alpha_{i} \kappa\left(\boldsymbol{x}, \boldsymbol{x}_{i}\right)$$
将$\kappa\left(\boldsymbol{x}, \boldsymbol{x}_{i}\right)=\phi(\boldsymbol{x})^{\mathrm{T}}\phi(\boldsymbol{x}_i)$代入可得
$$\boldsymbol{w}^{\mathrm{T}}\phi(\boldsymbol{x})=\sum_{i=1}^{m} \alpha_{i} \phi(\boldsymbol{x})^{\mathrm{T}}\phi(\boldsymbol{x}_i)$$
$$\boldsymbol{w}^{\mathrm{T}}\phi(\boldsymbol{x})=\phi(\boldsymbol{x})^{\mathrm{T}}\cdot\sum_{i=1}^{m} \alpha_{i} \phi(\boldsymbol{x}_i)$$
由于$\boldsymbol{w}^{\mathrm{T}}\phi(\boldsymbol{x})$的计算结果为标量，而标量的转置等于其本身，所以
$$\boldsymbol{w}^{\mathrm{T}}\phi(\boldsymbol{x})=\left(\boldsymbol{w}^{\mathrm{T}}\phi(\boldsymbol{x})\right)^{\mathrm{T}}=\phi(\boldsymbol{x})^{\mathrm{T}}\cdot\sum_{i=1}^{m} \alpha_{i} \phi(\boldsymbol{x}_i)$$
$$\boldsymbol{w}^{\mathrm{T}}\phi(\boldsymbol{x})=\phi(\boldsymbol{x})^{\mathrm{T}}\boldsymbol{w}=\phi(\boldsymbol{x})^{\mathrm{T}}\cdot\sum_{i=1}^{m} \alpha_{i} \phi(\boldsymbol{x}_i)$$
$$\boldsymbol{w}=\sum_{i=1}^{m} \alpha_{i} \phi(\boldsymbol{x}_i)$$

## 6.66
$$\hat{\boldsymbol{\mu}}_{0}=\frac{1}{m_{0}} \mathbf{K} \mathbf{1}_{0}$$
[解析]：为了详细地说明此公式的计算原理，下面首先先举例说明，然后再在例子的基础上延展出其一般形式。假设此时仅有4个样本，其中第1和第3个样本的标记为0，第2和第4个样本的标记为1，那么此时：
$$m=4$$
$$m_0=2,m_1=2$$
$$X_0=\{\boldsymbol{x}_1,\boldsymbol{x}_3\},X_1=\{\boldsymbol{x}_2,\boldsymbol{x}_4\}$$
$$\mathbf{K}=\left[ \begin{array}{cccc}
\kappa\left(\boldsymbol{x}_1, \boldsymbol{x}_1\right) & \kappa\left(\boldsymbol{x}_1, \boldsymbol{x}_2\right) & \kappa\left(\boldsymbol{x}_1, \boldsymbol{x}_3\right) & \kappa\left(\boldsymbol{x}_1, \boldsymbol{x}_4\right)\\ 
\kappa\left(\boldsymbol{x}_2, \boldsymbol{x}_1\right) & \kappa\left(\boldsymbol{x}_2, \boldsymbol{x}_2\right) & \kappa\left(\boldsymbol{x}_2, \boldsymbol{x}_3\right) & \kappa\left(\boldsymbol{x}_2, \boldsymbol{x}_4\right)\\ 
\kappa\left(\boldsymbol{x}_3, \boldsymbol{x}_1\right) & \kappa\left(\boldsymbol{x}_3, \boldsymbol{x}_2\right) & \kappa\left(\boldsymbol{x}_3, \boldsymbol{x}_3\right) & \kappa\left(\boldsymbol{x}_3, \boldsymbol{x}_4\right)\\ 
\kappa\left(\boldsymbol{x}_4, \boldsymbol{x}_1\right) & \kappa\left(\boldsymbol{x}_4, \boldsymbol{x}_2\right) & \kappa\left(\boldsymbol{x}_4, \boldsymbol{x}_3\right) & \kappa\left(\boldsymbol{x}_4, \boldsymbol{x}_4\right)\\ 
\end{array} \right]\in \mathbb{R}^{4\times 4}$$
$$\mathbf{1}_{0}=\left[ \begin{array}{c}
1\\ 
0\\ 
1\\ 
0\\ 
\end{array} \right]\in \mathbb{R}^{4\times 1}$$
$$\mathbf{1}_{1}=\left[ \begin{array}{c}
0\\ 
1\\ 
0\\ 
1\\ 
\end{array} \right]\in \mathbb{R}^{4\times 1}$$
所以
$$\hat{\boldsymbol{\mu}}_{0}=\frac{1}{m_{0}} \mathbf{K} \mathbf{1}_{0}=\frac{1}{2}\left[ \begin{array}{c}
\kappa\left(\boldsymbol{x}_1, \boldsymbol{x}_1\right)+\kappa\left(\boldsymbol{x}_1, \boldsymbol{x}_3\right)\\ 
\kappa\left(\boldsymbol{x}_2, \boldsymbol{x}_1\right)+\kappa\left(\boldsymbol{x}_2, \boldsymbol{x}_3\right)\\ 
\kappa\left(\boldsymbol{x}_3, \boldsymbol{x}_1\right)+\kappa\left(\boldsymbol{x}_3, \boldsymbol{x}_3\right)\\ 
\kappa\left(\boldsymbol{x}_4, \boldsymbol{x}_1\right)+\kappa\left(\boldsymbol{x}_4, \boldsymbol{x}_3\right)\\ 
\end{array} \right]\in \mathbb{R}^{4\times 1}$$
$$\hat{\boldsymbol{\mu}}_{1}=\frac{1}{m_{1}} \mathbf{K} \mathbf{1}_{1}=\frac{1}{2}\left[ \begin{array}{c}
\kappa\left(\boldsymbol{x}_1, \boldsymbol{x}_2\right)+\kappa\left(\boldsymbol{x}_1, \boldsymbol{x}_4\right)\\ 
\kappa\left(\boldsymbol{x}_2, \boldsymbol{x}_2\right)+\kappa\left(\boldsymbol{x}_2, \boldsymbol{x}_4\right)\\ 
\kappa\left(\boldsymbol{x}_3, \boldsymbol{x}_2\right)+\kappa\left(\boldsymbol{x}_3, \boldsymbol{x}_4\right)\\ 
\kappa\left(\boldsymbol{x}_4, \boldsymbol{x}_2\right)+\kappa\left(\boldsymbol{x}_4, \boldsymbol{x}_4\right)\\ 
\end{array} \right]\in \mathbb{R}^{4\times 1}$$
根据此结果易得$\hat{\boldsymbol{\mu}}_{0},\hat{\boldsymbol{\mu}}_{1}$的一般形式为
$$\hat{\boldsymbol{\mu}}_{0}=\frac{1}{m_{0}} \mathbf{K} \mathbf{1}_{0}=\frac{1}{m_{0}}\left[ \begin{array}{c}
\sum_{\boldsymbol{x} \in X_{0}}\kappa\left(\boldsymbol{x}_1, \boldsymbol{x}\right)\\ 
\sum_{\boldsymbol{x} \in X_{0}}\kappa\left(\boldsymbol{x}_2, \boldsymbol{x}\right)\\ 
\vdots\\ 
\sum_{\boldsymbol{x} \in X_{0}}\kappa\left(\boldsymbol{x}_m, \boldsymbol{x}\right)\\ 
\end{array} \right]\in \mathbb{R}^{m\times 1}$$
$$\hat{\boldsymbol{\mu}}_{1}=\frac{1}{m_{1}} \mathbf{K} \mathbf{1}_{1}=\frac{1}{m_{1}}\left[ \begin{array}{c}
\sum_{\boldsymbol{x} \in X_{1}}\kappa\left(\boldsymbol{x}_1, \boldsymbol{x}\right)\\ 
\sum_{\boldsymbol{x} \in X_{1}}\kappa\left(\boldsymbol{x}_2, \boldsymbol{x}\right)\\ 
\vdots\\ 
\sum_{\boldsymbol{x} \in X_{1}}\kappa\left(\boldsymbol{x}_m, \boldsymbol{x}\right)\\ 
\end{array} \right]\in \mathbb{R}^{m\times 1}$$

## 6.67
$$\hat{\boldsymbol{\mu}}_{1}=\frac{1}{m_{1}} \mathbf{K} \mathbf{1}_{1}$$
[解析]：参见公式(6.66)的解析。

## 6.68
$$\mathbf{M}=\left(\hat{\boldsymbol{\mu}}_{0}-\hat{\boldsymbol{\mu}}_{1}\right)\left(\hat{\boldsymbol{\mu}}_{0}-\hat{\boldsymbol{\mu}}_{1}\right)^{\mathrm{T}}$$
[解析]：略

## 6.69
$$\mathbf{N}=\mathbf{K} \mathbf{K}^{\mathrm{T}}-\sum_{i=0}^{1} m_{i} \hat{\boldsymbol{\mu}}_{i} \hat{\boldsymbol{\mu}}_{i}^{\mathrm{T}}$$
[解析]：略

## 6.70
$$\max _{\boldsymbol{\alpha}} J(\boldsymbol{\alpha})=\frac{\boldsymbol{\alpha}^{\mathrm{T}} \mathbf{M} \boldsymbol{\alpha}}{\boldsymbol{\alpha}^{\mathrm{T}} \mathbf{N} \boldsymbol{\alpha}}$$
[推导]：此公式是将公式(6.65)代入公式(6.60)后推得而来的，下面给出详细地推导过程。首先将公式(6.65)代入公式(6.60)的分子可得：
$$\begin{aligned}
\boldsymbol{w}^{\mathrm{T}} \mathbf{S}_{b}^{\phi} \boldsymbol{w}&=\left(\sum_{i=1}^{m} \alpha_{i} \phi\left(\boldsymbol{x}_{i}\right)\right)^{\mathrm{T}}\cdot\mathbf{S}_{b}^{\phi}\cdot \sum_{i=1}^{m} \alpha_{i} \phi\left(\boldsymbol{x}_{i}\right) \\
&=\sum_{i=1}^{m} \alpha_{i} \phi\left(\boldsymbol{x}_{i}\right)^{\mathrm{T}}\cdot\mathbf{S}_{b}^{\phi}\cdot \sum_{i=1}^{m} \alpha_{i} \phi\left(\boldsymbol{x}_{i}\right) \\
\end{aligned}$$
其中
$$\begin{aligned}
\mathbf{S}_{b}^{\phi} &=\left(\boldsymbol{\mu}_{1}^{\phi}-\boldsymbol{\mu}_{0}^{\phi}\right)\left(\boldsymbol{\mu}_{1}^{\phi}-\boldsymbol{\mu}_{0}^{\phi}\right)^{\mathrm{T}} \\
&=\left(\frac{1}{m_{1}} \sum_{\boldsymbol{x} \in X_{1}} \phi(\boldsymbol{x})-\frac{1}{m_{0}} \sum_{\boldsymbol{x} \in X_{0}} \phi(\boldsymbol{x})\right)\left(\frac{1}{m_{1}} \sum_{\boldsymbol{x} \in X_{1}} \phi(\boldsymbol{x})-\frac{1}{m_{0}} \sum_{\boldsymbol{x} \in X_{0}} \phi(\boldsymbol{x})\right)^{\mathrm{T}} \\
&=\left(\frac{1}{m_{1}} \sum_{\boldsymbol{x} \in X_{1}} \phi(\boldsymbol{x})-\frac{1}{m_{0}} \sum_{\boldsymbol{x} \in X_{0}} \phi(\boldsymbol{x})\right)\left(\frac{1}{m_{1}} \sum_{\boldsymbol{x} \in X_{1}} \phi(\boldsymbol{x})^{\mathrm{T}}-\frac{1}{m_{0}} \sum_{\boldsymbol{x} \in X_{0}} \phi(\boldsymbol{x})^{\mathrm{T}}\right) \\
\end{aligned}$$
将其代入上式可得
$$\begin{aligned}
\boldsymbol{w}^{\mathrm{T}} \mathbf{S}_{b}^{\phi} \boldsymbol{w}&=\sum_{i=1}^{m} \alpha_{i} \phi\left(\boldsymbol{x}_{i}\right)^{\mathrm{T}}\cdot\left(\frac{1}{m_{1}} \sum_{\boldsymbol{x} \in X_{1}} \phi(\boldsymbol{x})-\frac{1}{m_{0}} \sum_{\boldsymbol{x} \in X_{0}} \phi(\boldsymbol{x})\right)\cdot\left(\frac{1}{m_{1}} \sum_{\boldsymbol{x} \in X_{1}} \phi(\boldsymbol{x})^{\mathrm{T}}-\frac{1}{m_{0}} \sum_{\boldsymbol{x} \in X_{0}} \phi(\boldsymbol{x})^{\mathrm{T}}\right)\cdot \sum_{i=1}^{m} \alpha_{i} \phi\left(\boldsymbol{x}_{i}\right) \\
&=\left(\frac{1}{m_{1}} \sum_{\boldsymbol{x} \in X_{1}}\sum_{i=1}^{m} \alpha_{i} \phi\left(\boldsymbol{x}_{i}\right)^{\mathrm{T}} \phi(\boldsymbol{x})-\frac{1}{m_{0}} \sum_{\boldsymbol{x} \in X_{0}} \sum_{i=1}^{m} \alpha_{i} \phi\left(\boldsymbol{x}_{i}\right)^{\mathrm{T}}\phi(\boldsymbol{x})\right)\cdot\left(\frac{1}{m_{1}} \sum_{\boldsymbol{x} \in X_{1}} \sum_{i=1}^{m} \alpha_{i} \phi(\boldsymbol{x})^{\mathrm{T}}\phi\left(\boldsymbol{x}_{i}\right)-\frac{1}{m_{0}} \sum_{\boldsymbol{x} \in X_{0}} \sum_{i=1}^{m} \alpha_{i} \phi(\boldsymbol{x})^{\mathrm{T}}\phi\left(\boldsymbol{x}_{i}\right)\right) \\
\end{aligned}$$
由于$\kappa\left(\boldsymbol{x}_i, \boldsymbol{x}\right)=\phi(\boldsymbol{x}_i)^{\mathrm{T}}\phi(\boldsymbol{x})$为标量，所以其转置等于本身，也即$\kappa\left(\boldsymbol{x}_i, \boldsymbol{x}\right)=\phi(\boldsymbol{x}_i)^{\mathrm{T}}\phi(\boldsymbol{x})=\left(\phi(\boldsymbol{x}_i)^{\mathrm{T}}\phi(\boldsymbol{x})\right)^{\mathrm{T}}=\phi(\boldsymbol{x})^{\mathrm{T}}\phi(\boldsymbol{x}_i)=\kappa\left(\boldsymbol{x}_i, \boldsymbol{x}\right)^{\mathrm{T}}$，将其代入上式可得
$$\boldsymbol{w}^{\mathrm{T}} \mathbf{S}_{b}^{\phi} \boldsymbol{w}=\left(\frac{1}{m_{1}} \sum_{i=1}^{m}\sum_{\boldsymbol{x} \in X_{1}}\alpha_{i} \kappa\left(\boldsymbol{x}_i, \boldsymbol{x}\right)-\frac{1}{m_{0}} \sum_{i=1}^{m} \sum_{\boldsymbol{x} \in X_{0}}  \alpha_{i} \kappa\left(\boldsymbol{x}_i, \boldsymbol{x}\right)\right)\cdot\left(\frac{1}{m_{1}} \sum_{i=1}^{m}\sum_{\boldsymbol{x} \in X_{1}} \alpha_{i} \kappa\left(\boldsymbol{x}_i, \boldsymbol{x}\right)-\frac{1}{m_{0}}\sum_{i=1}^{m}  \sum_{\boldsymbol{x} \in X_{0}} \alpha_{i} \kappa\left(\boldsymbol{x}_i, \boldsymbol{x}\right)\right)$$
令$\boldsymbol{\alpha}=(\alpha_1;\alpha_2;...;\alpha_m)^{\mathrm{T}}\in \mathbb{R}^{m\times 1}$，同时结合公式(6.66)的解析中得到的$\hat{\boldsymbol{\mu}}_{0},\hat{\boldsymbol{\mu}}_{1}$的一般形式，上式可以化简为
$$\begin{aligned}
\boldsymbol{w}^{\mathrm{T}} \mathbf{S}_{b}^{\phi} \boldsymbol{w}&=\left(\boldsymbol{\alpha}^{\mathrm{T}}\hat{\boldsymbol{\mu}}_{1}-\boldsymbol{\alpha}^{\mathrm{T}}\hat{\boldsymbol{\mu}}_{0}\right)\cdot\left(\hat{\boldsymbol{\mu}}_{1}^{\mathrm{T}}\boldsymbol{\alpha}-\hat{\boldsymbol{\mu}}_{0}^{\mathrm{T}}\boldsymbol{\alpha}\right)\\
&=\boldsymbol{\alpha}^{\mathrm{T}}\cdot\left(\hat{\boldsymbol{\mu}}_{1}-\hat{\boldsymbol{\mu}}_{0}\right)\cdot\left(\hat{\boldsymbol{\mu}}_{1}^{\mathrm{T}}-\hat{\boldsymbol{\mu}}_{0}^{\mathrm{T}}\right)\cdot\boldsymbol{\alpha}\\
&=\boldsymbol{\alpha}^{\mathrm{T}}\cdot\left(\hat{\boldsymbol{\mu}}_{1}-\hat{\boldsymbol{\mu}}_{0}\right)\cdot\left(\hat{\boldsymbol{\mu}}_{1}-\hat{\boldsymbol{\mu}}_{0}\right)^{\mathrm{T}}\cdot\boldsymbol{\alpha}\\
&=\boldsymbol{\alpha}^{\mathrm{T}} \mathbf{M} \boldsymbol{\alpha}\\
\end{aligned}$$
以上便是公式(6.70)分子部分的推导，下面继续推导公式(6.70)的分母部分。将公式(6.65)代入公式(6.60)的分母可得：
$$\begin{aligned}
\boldsymbol{w}^{\mathrm{T}} \mathbf{S}_{w}^{\phi} \boldsymbol{w}&=\left(\sum_{i=1}^{m} \alpha_{i} \phi\left(\boldsymbol{x}_{i}\right)\right)^{\mathrm{T}}\cdot\mathbf{S}_{w}^{\phi}\cdot \sum_{i=1}^{m} \alpha_{i} \phi\left(\boldsymbol{x}_{i}\right) \\
&=\sum_{i=1}^{m} \alpha_{i} \phi\left(\boldsymbol{x}_{i}\right)^{\mathrm{T}}\cdot\mathbf{S}_{w}^{\phi}\cdot \sum_{i=1}^{m} \alpha_{i} \phi\left(\boldsymbol{x}_{i}\right) \\
\end{aligned}$$
其中
$$\begin{aligned}
\mathbf{S}_{w}^{\phi}&=\sum_{i=0}^{1} \sum_{\boldsymbol{x} \in X_{i}}\left(\phi(\boldsymbol{x})-\boldsymbol{\mu}_{i}^{\phi}\right)\left(\phi(\boldsymbol{x})-\boldsymbol{\mu}_{i}^{\phi}\right)^{\mathrm{T}} \\
&=\sum_{i=0}^{1} \sum_{\boldsymbol{x} \in X_{i}}\left(\phi(\boldsymbol{x})-\boldsymbol{\mu}_{i}^{\phi}\right)\left(\phi(\boldsymbol{x})^{\mathrm{T}}-\left(\boldsymbol{\mu}_{i}^{\phi}\right)^{\mathrm{T}}\right) \\
&=\sum_{i=0}^{1} \sum_{\boldsymbol{x} \in X_{i}}\left(\phi(\boldsymbol{x})\phi(\boldsymbol{x})^{\mathrm{T}}-\phi(\boldsymbol{x})\left(\boldsymbol{\mu}_{i}^{\phi}\right)^{\mathrm{T}}-\boldsymbol{\mu}_{i}^{\phi}\phi(\boldsymbol{x})^{\mathrm{T}}+\boldsymbol{\mu}_{i}^{\phi}\left(\boldsymbol{\mu}_{i}^{\phi}\right)^{\mathrm{T}}\right) \\
\end{aligned}$$
由于$\phi(\boldsymbol{x})\left(\boldsymbol{\mu}_{i}^{\phi}\right)^{\mathrm{T}}$的计算结果为标量，所以$\phi(\boldsymbol{x})\left(\boldsymbol{\mu}_{i}^{\phi}\right)^{\mathrm{T}}=\left[\phi(\boldsymbol{x})\left(\boldsymbol{\mu}_{i}^{\phi}\right)^{\mathrm{T}}\right]^{\mathrm{T}}=\boldsymbol{\mu}_{i}^{\phi}\phi(\boldsymbol{x})^{\mathrm{T}}$，将其代回上式可得
$$\begin{aligned}
\mathbf{S}_{w}^{\phi}&=\sum_{i=0}^{1} \sum_{\boldsymbol{x} \in X_{i}}\left(\phi(\boldsymbol{x})\phi(\boldsymbol{x})^{\mathrm{T}}-2\boldsymbol{\mu}_{i}^{\phi}\phi(\boldsymbol{x})^{\mathrm{T}}+\boldsymbol{\mu}_{i}^{\phi}\left(\boldsymbol{\mu}_{i}^{\phi}\right)^{\mathrm{T}}\right) \\
&=\sum_{i=0}^{1} \sum_{\boldsymbol{x} \in X_{i}}\phi(\boldsymbol{x})\phi(\boldsymbol{x})^{\mathrm{T}}-\sum_{i=0}^{1} \sum_{\boldsymbol{x} \in X_{i}}2\boldsymbol{\mu}_{i}^{\phi}\phi(\boldsymbol{x})^{\mathrm{T}}+\sum_{i=0}^{1} \sum_{\boldsymbol{x} \in X_{i}}\boldsymbol{\mu}_{i}^{\phi}\left(\boldsymbol{\mu}_{i}^{\phi}\right)^{\mathrm{T}} \\
&=\sum_{\boldsymbol{x} \in  D}\phi(\boldsymbol{x})\phi(\boldsymbol{x})^{\mathrm{T}}-2\boldsymbol{\mu}_{0}^{\phi}\sum_{\boldsymbol{x} \in X_{0}}\phi(\boldsymbol{x})^{\mathrm{T}}-2\boldsymbol{\mu}_{1}^{\phi}\sum_{\boldsymbol{x} \in X_{1}}\phi(\boldsymbol{x})^{\mathrm{T}}+\sum_{\boldsymbol{x} \in X_{0}}\boldsymbol{\mu}_{0}^{\phi}\left(\boldsymbol{\mu}_{0}^{\phi}\right)^{\mathrm{T}}+\sum_{\boldsymbol{x} \in X_{1}}\boldsymbol{\mu}_{1}^{\phi}\left(\boldsymbol{\mu}_{1}^{\phi}\right)^{\mathrm{T}} \\
&=\sum_{\boldsymbol{x} \in  D}\phi(\boldsymbol{x})\phi(\boldsymbol{x})^{\mathrm{T}}-2m_0\boldsymbol{\mu}_{0}^{\phi}\left(\boldsymbol{\mu}_{0}^{\phi}\right)^{\mathrm{T}}-2m_1\boldsymbol{\mu}_{1}^{\phi}\left(\boldsymbol{\mu}_{1}^{\phi}\right)^{\mathrm{T}}+m_0 \boldsymbol{\mu}_{0}^{\phi}\left(\boldsymbol{\mu}_{0}^{\phi}\right)^{\mathrm{T}}+m_1 \boldsymbol{\mu}_{1}^{\phi}\left(\boldsymbol{\mu}_{1}^{\phi}\right)^{\mathrm{T}} \\
&=\sum_{\boldsymbol{x} \in  D}\phi(\boldsymbol{x})\phi(\boldsymbol{x})^{\mathrm{T}}-m_0\boldsymbol{\mu}_{0}^{\phi}\left(\boldsymbol{\mu}_{0}^{\phi}\right)^{\mathrm{T}}-m_1\boldsymbol{\mu}_{1}^{\phi}\left(\boldsymbol{\mu}_{1}^{\phi}\right)^{\mathrm{T}}\\
\end{aligned}$$
再将此式代回$\boldsymbol{w}^{\mathrm{T}} \mathbf{S}_{b}^{\phi} \boldsymbol{w}$可得
$$\begin{aligned}
\boldsymbol{w}^{\mathrm{T}} \mathbf{S}_{w}^{\phi} \boldsymbol{w}&=\sum_{i=1}^{m} \alpha_{i} \phi\left(\boldsymbol{x}_{i}\right)^{\mathrm{T}}\cdot\mathbf{S}_{w}^{\phi}\cdot \sum_{i=1}^{m} \alpha_{i} \phi\left(\boldsymbol{x}_{i}\right) \\
&=\sum_{i=1}^{m} \alpha_{i} \phi\left(\boldsymbol{x}_{i}\right)^{\mathrm{T}}\cdot\left(\sum_{\boldsymbol{x} \in  D}\phi(\boldsymbol{x})\phi(\boldsymbol{x})^{\mathrm{T}}-m_0\boldsymbol{\mu}_{0}^{\phi}\left(\boldsymbol{\mu}_{0}^{\phi}\right)^{\mathrm{T}}-m_1\boldsymbol{\mu}_{1}^{\phi}\left(\boldsymbol{\mu}_{1}^{\phi}\right)^{\mathrm{T}}\right)\cdot \sum_{i=1}^{m} \alpha_{i} \phi\left(\boldsymbol{x}_{i}\right) \\
&=\sum_{i=1}^{m}\sum_{j=1}^{m}\sum_{\boldsymbol{x} \in  D}\alpha_{i} \phi\left(\boldsymbol{x}_{i}\right)^{\mathrm{T}}\phi(\boldsymbol{x})\phi(\boldsymbol{x})^{\mathrm{T}}\alpha_{j} \phi\left(\boldsymbol{x}_{j}\right)-\sum_{i=1}^{m}\sum_{j=1}^{m}\alpha_{i} \phi\left(\boldsymbol{x}_{i}\right)^{\mathrm{T}}m_0\boldsymbol{\mu}_{0}^{\phi}\left(\boldsymbol{\mu}_{0}^{\phi}\right)^{\mathrm{T}}\alpha_{j} \phi\left(\boldsymbol{x}_{j}\right)-\sum_{i=1}^{m}\sum_{j=1}^{m}\alpha_{i} \phi\left(\boldsymbol{x}_{i}\right)^{\mathrm{T}}m_1\boldsymbol{\mu}_{1}^{\phi}\left(\boldsymbol{\mu}_{1}^{\phi}\right)^{\mathrm{T}}\alpha_{j} \phi\left(\boldsymbol{x}_{j}\right) \\
\end{aligned}$$
其中，第1项可化简为
$$\begin{aligned}
\sum_{i=1}^{m}\sum_{j=1}^{m}\sum_{\boldsymbol{x} \in  D}\alpha_{i} \phi\left(\boldsymbol{x}_{i}\right)^{\mathrm{T}}\phi(\boldsymbol{x})\phi(\boldsymbol{x})^{\mathrm{T}}\alpha_{j} \phi\left(\boldsymbol{x}_{j}\right)&=\sum_{i=1}^{m}\sum_{j=1}^{m}\sum_{\boldsymbol{x} \in  D}\alpha_{i} \alpha_{j}\kappa\left(\boldsymbol{x}_i, \boldsymbol{x}\right)\kappa\left(\boldsymbol{x}_j, \boldsymbol{x}\right)\\
&=\boldsymbol{\alpha}^{\mathrm{T}} \mathbf{K} \mathbf{K}^{\mathrm{T}} \boldsymbol{\alpha}
\end{aligned}$$
第2项可化简为
$$\begin{aligned}
\sum_{i=1}^{m}\sum_{j=1}^{m}\alpha_{i} \phi\left(\boldsymbol{x}_{i}\right)^{\mathrm{T}}m_0\boldsymbol{\mu}_{0}^{\phi}\left(\boldsymbol{\mu}_{0}^{\phi}\right)^{\mathrm{T}}\alpha_{j} \phi\left(\boldsymbol{x}_{j}\right)&=m_0\sum_{i=1}^{m}\sum_{j=1}^{m}\alpha_{i}\alpha_{j}\phi\left(\boldsymbol{x}_{i}\right)^{\mathrm{T}}\boldsymbol{\mu}_{0}^{\phi}\left(\boldsymbol{\mu}_{0}^{\phi}\right)^{\mathrm{T}} \phi\left(\boldsymbol{x}_{j}\right)\\
&=m_0\sum_{i=1}^{m}\sum_{j=1}^{m}\alpha_{i}\alpha_{j}\phi\left(\boldsymbol{x}_{i}\right)^{\mathrm{T}}\left[\frac{1}{m_{0}} \sum_{\boldsymbol{x} \in X_{0}} \phi(\boldsymbol{x})\right]\left[\frac{1}{m_{0}} \sum_{\boldsymbol{x} \in X_{0}} \phi(\boldsymbol{x})\right]^{\mathrm{T}} \phi\left(\boldsymbol{x}_{j}\right)\\
&=m_0\sum_{i=1}^{m}\sum_{j=1}^{m}\alpha_{i}\alpha_{j}\left[\frac{1}{m_{0}} \sum_{\boldsymbol{x} \in X_{0}} \phi\left(\boldsymbol{x}_{i}\right)^{\mathrm{T}}\phi(\boldsymbol{x})\right]\left[\frac{1}{m_{0}} \sum_{\boldsymbol{x} \in X_{0}} \phi(\boldsymbol{x})^{\mathrm{T}}\phi\left(\boldsymbol{x}_{j}\right)\right] \\
&=m_0\sum_{i=1}^{m}\sum_{j=1}^{m}\alpha_{i}\alpha_{j}\left[\frac{1}{m_{0}} \sum_{\boldsymbol{x} \in X_{0}} \kappa\left(\boldsymbol{x}_i, \boldsymbol{x}\right)\right]\left[\frac{1}{m_{0}} \sum_{\boldsymbol{x} \in X_{0}} \kappa\left(\boldsymbol{x}_j, \boldsymbol{x}\right)\right] \\
&=m_0\boldsymbol{\alpha}^{\mathrm{T}} \hat{\boldsymbol{\mu}}_{0} \hat{\boldsymbol{\mu}}_{0}^{\mathrm{T}} \boldsymbol{\alpha}
\end{aligned}$$
同理可得，第3项可化简为
$$\sum_{i=1}^{m}\sum_{j=1}^{m}\alpha_{i} \phi\left(\boldsymbol{x}_{i}\right)^{\mathrm{T}}m_1\boldsymbol{\mu}_{1}^{\phi}\left(\boldsymbol{\mu}_{1}^{\phi}\right)^{\mathrm{T}}\alpha_{j} \phi\left(\boldsymbol{x}_{j}\right)=m_1\boldsymbol{\alpha}^{\mathrm{T}} \hat{\boldsymbol{\mu}}_{1} \hat{\boldsymbol{\mu}}_{1}^{\mathrm{T}} \boldsymbol{\alpha}$$
将上述三项的化简结果代回再将此式代回$\boldsymbol{w}^{\mathrm{T}} \mathbf{S}_{b}^{\phi} \boldsymbol{w}$可得
$$\begin{aligned}
\boldsymbol{w}^{\mathrm{T}} \mathbf{S}_{b}^{\phi} \boldsymbol{w}&=\boldsymbol{\alpha}^{\mathrm{T}} \mathbf{K} \mathbf{K}^{\mathrm{T}} \boldsymbol{\alpha}-m_0\boldsymbol{\alpha}^{\mathrm{T}} \hat{\boldsymbol{\mu}}_{0} \hat{\boldsymbol{\mu}}_{0}^{\mathrm{T}} \boldsymbol{\alpha}-m_1\boldsymbol{\alpha}^{\mathrm{T}} \hat{\boldsymbol{\mu}}_{1} \hat{\boldsymbol{\mu}}_{1}^{\mathrm{T}} \boldsymbol{\alpha}\\
&=\boldsymbol{\alpha}^{\mathrm{T}} \cdot\left(\mathbf{K} \mathbf{K}^{\mathrm{T}} -m_0\hat{\boldsymbol{\mu}}_{0} \hat{\boldsymbol{\mu}}_{0}^{\mathrm{T}} -m_1\hat{\boldsymbol{\mu}}_{1} \hat{\boldsymbol{\mu}}_{1}^{\mathrm{T}} \right)\cdot\boldsymbol{\alpha}\\
&=\boldsymbol{\alpha}^{\mathrm{T}} \cdot\left(\mathbf{K} \mathbf{K}^{\mathrm{T}}-\sum_{i=0}^{1} m_{i} \hat{\boldsymbol{\mu}}_{i} \hat{\boldsymbol{\mu}}_{i}^{\mathrm{T}} \right)\cdot\boldsymbol{\alpha}\\
&=\boldsymbol{\alpha}^{\mathrm{T}} \mathbf{N}\boldsymbol{\alpha}\\
\end{aligned}$$

