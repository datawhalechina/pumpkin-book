## 10.1

$$
P(e r r)=1-\sum_{c \in \mathcal{Y}} P(c | \boldsymbol{x}) P(c | \boldsymbol{z})
$$

[解析]：$P(c | \boldsymbol{x}) P(c | \boldsymbol{z})$表示$x$和$z$同属类$c$的概率，对所有可能的类别$c\in\mathcal{Y}$求和，则得到$x$和$z$同属相同类别的概率，因此$1-\sum_{c \in \mathcal{Y}} P(c | \boldsymbol{x}) P(c | \boldsymbol{z})$表示$x$和$z$分属不同类别的概率。



## 10.2

$$
\begin{aligned}
P(e r r) &=1-\sum_{c \in \mathcal{Y}} P(c | \boldsymbol{x}) P(c | \boldsymbol{z}) \\
& \simeq 1-\sum_{c \in \mathcal{Y}} P^{2}(c | \boldsymbol{x}) \\
& \leqslant 1-P^{2}\left(c^{*} | \boldsymbol{x}\right) \\
&=\left(1+P\left(c^{*} | \boldsymbol{x}\right)\right)\left(1-P\left(c^{*} | \boldsymbol{x}\right)\right) \\
& \leqslant 2 \times\left(1-P\left(c^{*} | \boldsymbol{x}\right)\right)
\end{aligned}
$$

[解析]：第二个式子是来源于前提假设"假设样本独立同分布，且对任意$x$和任意小正数$\delta$，在$x$附近$\delta$距离范围内总能找到一个训练样本"，则$P(c | \boldsymbol{z}) = P(c | \boldsymbol{x\pm\delta})\simeq P(c|\boldsymbol{x})$。第三个式子是应为$c^{*}\in\mathcal{Y}$，因此$P^{2}\left(c^{*} | \boldsymbol{x}\right)$是$\sum_{c \in \mathcal{Y}} P^{2}(c | \boldsymbol{x})$的一个分量，所以$\sum_{c \in \mathcal{Y}} P^{2}(c | \boldsymbol{x}) \geqslant P^{2}\left(c^{*} | \boldsymbol{x}\right)$。第四个式子是平方差公式展开，最后一个式子因为$1 + P^{2}\left(c^{*} | \boldsymbol{x}\right)\leqslant 2$。



## 10.3

$$
\begin{aligned}
\operatorname{dist}_{i j}^{2} &=\left\|\boldsymbol{z}_{i}\right\|^{2}+\left\|\boldsymbol{z}_{j}\right\|^{2}-2 \boldsymbol{z}_{i}^{\mathrm{T}} \boldsymbol{z}_{j} \\
&=b_{i i}+b_{j j}-2 b_{i j}
\end{aligned}
$$

[推导]：
$$
\begin{aligned}
\operatorname{dist}_{i j}^{2} &=\left\|\boldsymbol{z}_{i}-\boldsymbol{z}_{j}\right\|^{2}=\left(\boldsymbol{z}_{i}-\boldsymbol{z}_{j}\right)^{\top}\left(\boldsymbol{z}_{i}-\boldsymbol{z}_{j}\right) \\
&=\boldsymbol{z}_{i}^{\top} \boldsymbol{z}_{i}-\boldsymbol{z}_{i}^{\top} \boldsymbol{z}_{j}-\boldsymbol{z}_{j}^{\top} \boldsymbol{z}_{i}+\boldsymbol{z}_{j}^{\top} \boldsymbol{z}_{j} \\
&=\boldsymbol{z}_{i}^{\top} \boldsymbol{z}_{i}+\boldsymbol{z}_{j}^{\top} \boldsymbol{z}_{j}-2 \boldsymbol{z}_{i}^{\top} \boldsymbol{z}_{j} \\
&=\left\|\boldsymbol{z}_{i}\right\|^{2}+\left\|\boldsymbol{z}_{j}\right\|^{2}-2 \boldsymbol{z}_{i}^{\top} \boldsymbol{z}_{j} \\
&=b_{i i}+b_{j j}-2 b_{i j}
\end{aligned}
$$


## 10.4

$$
\sum^m_{i=1}dist^2_{ij}=tr(\boldsymbol B)+mb_{jj}
$$

[解析]：首先根据式10.3有
$$
\sum^m_{i=1}dist^2_{ij}= \sum^m_{i=1}b_{ii}+\sum^m_{i=1}b_{jj}-2\sum^m_{i=1}b_{ij}
$$
对于第一项，根据矩阵迹的定义，$\sum^m_{i=1}b_{ii} = tr(\boldsymbol B)$，对于第二项，由于求和号内元素和$i$无关，因此$\sum^m_{i=1}b_{jj}=mb_{jj}$，对于第三项有，
$$
\sum_{i=1}^{m} b_{i j}=\sum_{i=1}^{m} \boldsymbol{z}_{i}^{\top} \boldsymbol{z}_{j}=
\sum_{i=1}^{m} \boldsymbol{z}_{j}^{\top} \boldsymbol{z}_{i}=
\boldsymbol{z}_{j}^{\top} \sum_{i=1}^{m} \boldsymbol{z}_{i}=
\boldsymbol{z}_{j}^{\top} \cdot \mathbf{0}=0
$$
其中$\sum_{i=1}^{m} \boldsymbol{z}_{i}=\mathbf{0}$是利用了书上的前提条件，即将降维后的样本被中心化。

## 10.5

$$
\sum_{j=1}^{m} \operatorname{dist}_{i j}^{2}=\operatorname{tr}(\mathbf{B})+m b_{i i}
$$

[解析]：参考10.4



## 10.6

$$
\sum_{i=1}^{m} \sum_{j=1}^{m} \operatorname{dist}_{i j}^{2}=2 m \operatorname{tr}(\mathbf{B})
$$

[推导]：
$$
\begin{aligned}
\sum_{i=1}^{m} \sum_{j=1}^{m} \operatorname{dist}_{i j}^{2} &=\sum_{i=1}^{m} \sum_{j=1}^{m}\left(\left\|z_{i}\right\|^{2}+\left\|\boldsymbol{z}_{j}\right\|^{2}-2 \boldsymbol{z}_{i}^{\top} \boldsymbol{z}_{j}\right) \\
&=\sum_{i=1}^{m} \sum_{j=1}^{m}\left\|\boldsymbol{z}_{i}\right\|^{2}+\sum_{i=1}^{m} \sum_{j=1}^{m}\left\|\boldsymbol{z}_{j}\right\|^{2}-2 \sum_{i=1}^{m} \sum_{j=1}^{m} \boldsymbol{z}_{i}^{\top} \boldsymbol{z}_{j} \\
\end{aligned}
$$
其中
$$
\sum_{i=1}^{m} \sum_{j=1}^{m}\left\|\boldsymbol{z}_{i}\right\|^{2}=m \sum_{i=1}^{m}\left\|\boldsymbol{z}_{i}\right\|^{2}=m \operatorname{tr}(\mathbf{B})
$$

$$
\sum_{i=1}^{m} \sum_{j=1}^{m}\left\|\boldsymbol{z}_{j}\right\|^{2}=m \sum_{j=1}^{m}\left\|\boldsymbol{z}_{j}\right\|^{2}=m \operatorname{tr}(\mathbf{B})
$$

$$
\sum_{i=1}^{m} \sum_{j=1}^{m} \boldsymbol{z}_{i}^{\top} \boldsymbol{z}_{j}=0
$$

最后一个式子是来自于书中的假设，假设降维后的样本$\mathbf{Z}$被中心化。

## 10.10

$$
b_{ij}=-\frac{1}{2}(dist^2_{ij}-dist^2_{i\cdot}-dist^2_{\cdot j}+dist^2_{\cdot\cdot})
$$



[推导]：由公式（10.3）可得
$$
b_{ij}=-\frac{1}{2}(dist^2_{ij}-b_{ii}-b_{jj})
$$


由公式（10.6）和（10.9）可得
$$
\begin{aligned}
tr(\boldsymbol B)&=\frac{1}{2m}\sum^m_{i=1}\sum^m_{j=1}dist^2_{ij}\\
&=\frac{m}{2}dist^2_{\cdot}
\end{aligned}
$$
由公式（10.4）和（10.8）可得
$$
\begin{aligned}
b_{jj}&=\frac{1}{m}\sum^m_{i=1}dist^2_{ij}-\frac{1}{m}tr(\boldsymbol B)\\
&=dist^2_{\cdot j}-\frac{1}{2}dist^2_{\cdot}
\end{aligned}
$$
由公式（10.5）和（10.7）可得
$$
\begin{aligned}
b_{ii}&=\frac{1}{m}\sum^m_{j=1}dist^2_{ij}-\frac{1}{m}tr(\boldsymbol B)\\
&=dist^2_{i\cdot}-\frac{1}{2}dist^2_{\cdot}
\end{aligned}
$$
综合可得
$$
\begin{aligned}
b_{ij}&=-\frac{1}{2}(dist^2_{ij}-b_{ii}-b_{jj})\\
&=-\frac{1}{2}(dist^2_{ij}-dist^2_{i\cdot}+\frac{1}{2}dist^2_{\cdot\cdot}-dist^2_{\cdot j}+\frac{1}{2}dist^2_{\cdot\cdot})\\
&=-\frac{1}{2}(dist^2_{ij}-dist^2_{i\cdot}-dist^2_{\cdot j}+dist^2_{\cdot\cdot})
\end{aligned}
$$


## 10.14

$$
\begin{aligned}
\sum^m_{i=1}\left\| \sum^{d'}_{j=1}z_{ij}\boldsymbol{w}_j-\boldsymbol x_i \right\|^2_2&=\sum^m_{i=1}\boldsymbol z^{\mathrm{T}}_i\boldsymbol z_i-2\sum^m_{i=1}\boldsymbol z^{\mathrm{T}}_i\mathbf{W}^{\mathrm{T}}\boldsymbol x_i +\text { const }\\
&\propto -\operatorname{tr}(\mathbf{W}^{\mathrm{T}}(\sum^m_{i=1}\boldsymbol x_i\boldsymbol x^{\mathrm{T}}_i)\mathbf{W})
\end{aligned}
$$

[推导]：已知$\mathbf{W}^{\mathrm{T}} \mathbf{W}=\mathbf{I},\boldsymbol z_i=\mathbf{W}^{\mathrm{T}} \boldsymbol x_i$，则
$$
\begin{aligned}
\sum^m_{i=1}\left\| \sum^{d'}_{j=1}z_{ij}\boldsymbol{w}_j-\boldsymbol x_i \right\|^2_2&=\sum^m_{i=1}\left\|\mathbf{W}\boldsymbol z_i-\boldsymbol x_i \right\|^2_2\\
&= \sum^m_{i=1} \left(\mathbf{W}\boldsymbol z_i-\boldsymbol x_i\right)^{\mathrm{T}}\left(\mathbf{W}\boldsymbol z_i-\boldsymbol x_i\right)\\
&= \sum^m_{i=1} \left(\boldsymbol z_i^{\mathrm{T}}\mathbf{W}^{\mathrm{T}}\mathbf{W}\boldsymbol z_i- \boldsymbol z_i^{\mathrm{T}}\mathbf{W}^{\mathrm{T}}\boldsymbol x_i-\boldsymbol x_i^{\mathrm{T}}\mathbf{W}\boldsymbol z_i+\boldsymbol x_i^{\mathrm{T}}\boldsymbol x_i \right)\\
&= \sum^m_{i=1} \left(\boldsymbol z_i^{\mathrm{T}}\boldsymbol z_i- 2\boldsymbol z_i^{\mathrm{T}}\mathbf{W}^{\mathrm{T}}\boldsymbol x_i+\boldsymbol x_i^{\mathrm{T}}\boldsymbol x_i \right)\\
&=\sum^m_{i=1}\boldsymbol z_i^{\mathrm{T}}\boldsymbol z_i-2\sum^m_{i=1}\boldsymbol z_i^{\mathrm{T}}\mathbf{W}^{\mathrm{T}}\boldsymbol x_i+\sum^m_{i=1}\boldsymbol x^{\mathrm{T}}_i\boldsymbol x_i\\
&=\sum^m_{i=1}\boldsymbol z_i^{\mathrm{T}}\boldsymbol z_i-2\sum^m_{i=1}\boldsymbol z_i^{\mathrm{T}}\mathbf{W}^{\mathrm{T}}\boldsymbol x_i+\text { const }\\
&=\sum^m_{i=1}\boldsymbol z_i^{\mathrm{T}}\boldsymbol z_i-2\sum^m_{i=1}\boldsymbol z_i^{\mathrm{T}}\boldsymbol z_i+\text { const }\\
&=-\sum^m_{i=1}\boldsymbol z_i^{\mathrm{T}}\boldsymbol z_i+\text { const }\\
&=-\sum^m_{i=1}\operatorname{tr}\left(\boldsymbol z_i\boldsymbol z_i^{\mathrm{T}}\right)+\text { const }\\
&=-\operatorname{tr}\left(\sum^m_{i=1}\boldsymbol z_i\boldsymbol z_i^{\mathrm{T}}\right)+\text { const }\\
&=-\operatorname{tr}\left(\sum^m_{i=1}\mathbf{W}^{\mathrm{T}} \boldsymbol x_i\boldsymbol x_i^{\mathrm{T}}\mathbf{W}\right)+\text { const }\\
&= -\operatorname{tr}\left(\mathbf{W}^{\mathrm{T}}\left(\sum^m_{i=1}\boldsymbol x_i\boldsymbol x^{\mathrm{T}}_i\right)\mathbf{W}\right)+\text { const }\\
&\propto-\operatorname{tr}\left(\mathbf{W}^{\mathrm{T}}\left(\sum^m_{i=1}\boldsymbol x_i\boldsymbol x^{\mathrm{T}}_i\right)\mathbf{W}\right)\\
\end{aligned}
$$


## 10.17
$$
\mathbf X\mathbf X^{\mathrm{T}} \boldsymbol w_i=\lambda _i\boldsymbol w_i
$$
[推导]：由式（10.15）可知，主成分分析的优化目标为
$$
\begin{aligned}
&\min\limits_{\mathbf W} \quad-\text { tr }(\mathbf W^{\mathrm{T}} \mathbf X\mathbf X^{\mathrm{T}} \mathbf W)\\
&s.t. \quad\mathbf W^{\mathrm{T}} \mathbf W=\mathbf I
\end{aligned}
$$
其中，$\mathbf{X}=\left(\boldsymbol{x}_{1}, \boldsymbol{x}_{2}, \ldots, \boldsymbol{x}_{m}\right) \in \mathbb{R}^{d \times m},\mathbf{W}=\left(\boldsymbol{w}_{1}, \boldsymbol{w}_{2}, \ldots, \boldsymbol{w}_{d^{\prime}}\right) \in \mathbb{R}^{d \times d^{\prime}}$，$\mathbf{I} \in \mathbb{R}^{d^{\prime} \times d^{\prime}}$为单位矩阵。对于带矩阵约束的优化问题，根据<a href="#ref1">[1]</a>中讲述的方法可得此优化目标的拉格朗日函数为
$$
\begin{aligned}
L(\mathbf W,\Theta)&=-\text { tr }(\mathbf W^{\mathrm{T}} \mathbf X\mathbf X^{\mathrm{T}} \mathbf W)+\langle \Theta,\mathbf W^{\mathrm{T}} \mathbf W-\mathbf I\rangle \\
&=-\text { tr }(\mathbf W^{\mathrm{T}} \mathbf X\mathbf X^{\mathrm{T}} \mathbf W)+\text { tr }\left(\Theta^{\mathrm{T}} (\mathbf W^{\mathrm{T}} \mathbf W-\mathbf I)\right) 
\end{aligned}
$$


其中，$\Theta  \in \mathbb{R}^{d^{\prime} \times d^{\prime}}$为拉格朗日乘子矩阵，其维度恒等于约束条件的维度，且其中的每个元素均为未知的拉格朗日乘子，$\langle \Theta,\mathbf W^{\mathrm{T}} \mathbf W-\mathbf I\rangle = \text { tr }\left(\Theta^{\mathrm{T}} (\mathbf W^{\mathrm{T}} \mathbf W-\mathbf I)\right)$为矩阵的内积<sup><a href="#ref2">[2]</a></sup>。若此时仅考虑约束$\boldsymbol{w}_i^{\mathrm{T}}\boldsymbol{w}_i=1(i=1,2,...,d^{\prime})$，则拉格朗日乘子矩阵$\Theta$此时为对角矩阵，令新的拉格朗日乘子矩阵为$\Lambda=diag(\lambda_1,\lambda_2,...,\lambda_{d^{\prime}})\in \mathbb{R}^{d^{\prime} \times d^{\prime}}$，则新的拉格朗日函数为
$$
L(\mathbf W,\Lambda)=-\text { tr }(\mathbf W^{\mathrm{T}} \mathbf X\mathbf X^{\mathrm{T}} \mathbf W)+\text { tr }\left(\Lambda^{\mathrm{T}} (\mathbf W^{\mathrm{T}} \mathbf W-\mathbf I)\right)
$$
对拉格朗日函数关于$\mathbf{W}$求导可得
$$
\begin{aligned}
\cfrac{\partial L(\mathbf W,\Lambda)}{\partial \mathbf W}&=\cfrac{\partial}{\partial \mathbf W}\left[-\text { tr }(\mathbf W^{\mathrm{T}} \mathbf X\mathbf X^{\mathrm{T}} \mathbf W)+\text { tr }\left(\Lambda^{\mathrm{T}} (\mathbf W^{\mathrm{T}} \mathbf W-\mathbf I)\right)\right] \\
&=-\cfrac{\partial}{\partial \mathbf W}\text { tr }(\mathbf W^{\mathrm{T}} \mathbf X\mathbf X^{\mathrm{T}} \mathbf W)+\cfrac{\partial}{\partial \mathbf W}\text { tr }\left(\Lambda^{\mathrm{T}} (\mathbf W^{\mathrm{T}} \mathbf W-\mathbf I)\right) \\
\end{aligned}
$$
由矩阵微分公式$\cfrac{\partial}{\partial \mathbf{X}} \text { tr }(\mathbf{X}^{\mathrm{T}}  \mathbf{B} \mathbf{X})=\mathbf{B X}+\mathbf{B}^{\mathrm{T}}  \mathbf{X},\cfrac{\partial}{\partial \mathbf{X}} \text { tr }\left(\mathbf{B X}^{\mathrm{T}}  \mathbf{X}\right)=\mathbf{X B}^{\mathrm{T}} +\mathbf{X B}$可得
$$
\begin{aligned}
\cfrac{\partial L(\mathbf W,\Lambda)}{\partial \mathbf W}&=-2\mathbf X\mathbf X^{\mathrm{T}} \mathbf W+\mathbf{W}\Lambda+\mathbf{W}\Lambda^{\mathrm{T}}  \\
&=-2\mathbf X\mathbf X^{\mathrm{T}} \mathbf W+\mathbf{W}(\Lambda+\Lambda^{\mathrm{T}} ) \\
&=-2\mathbf X\mathbf X^{\mathrm{T}} \mathbf W+2\mathbf{W}\Lambda
\end{aligned}
$$
令$\cfrac{\partial L(\mathbf W,\Lambda)}{\partial \mathbf W}=\mathbf 0$可得
$$
\begin{aligned}
-2\mathbf X\mathbf X^{\mathrm{T}} \mathbf W+2\mathbf{W}\Lambda&=\mathbf 0\\
\mathbf X\mathbf X^{\mathrm{T}} \mathbf W&=\mathbf{W}\Lambda\\
\end{aligned}
$$
将$\mathbf W$和$\Lambda$展开可得
$$
\mathbf X\mathbf X^{\mathrm{T}} \boldsymbol w_i=\lambda _i\boldsymbol w_i,\quad i=1,2,...,d^{\prime}
$$
显然，此式为矩阵特征值和特征向量的定义式，其中$\lambda_i,\boldsymbol w_i$分别表示矩阵$\mathbf X\mathbf X^{\mathrm{T}}$的特征值和单位特征向量。由于以上是仅考虑约束$\boldsymbol{w}_i^{\mathrm{T}}\boldsymbol{w}_i=1$所求得的结果，而$\boldsymbol{w}_i$还需满足约束$\boldsymbol{w}_{i}^{\mathrm{T}}\boldsymbol{w}_{j}=0(i\neq j)$。观察$\mathbf X\mathbf X^{\mathrm{T}}$的定义可知，$\mathbf X\mathbf X^{\mathrm{T}}$是一个实对称矩阵，实对称矩阵的不同特征值所对应的特征向量之间相互正交，同一特征值的不同特征向量可以通过施密特正交化使其变得正交，所以通过上式求得的$\boldsymbol w_i$可以同时满足约束$\boldsymbol{w}_i^{\mathrm{T}}\boldsymbol{w}_i=1,\boldsymbol{w}_{i}^{\mathrm{T}}\boldsymbol{w}_{j}=0(i\neq j)$。根据拉格朗日乘子法的原理可知，此时求得的结果仅是最优解的必要条件，而且$\mathbf X\mathbf X^{\mathrm{T}}$有$d$个相互正交的单位特征向量，所以还需要从这$d$个特征向量里找出$d^{\prime}$个能使得目标函数达到最优值的特征向量作为最优解。将$\mathbf X\mathbf X^{\mathrm{T}} \boldsymbol w_i=\lambda _i\boldsymbol w_i$代入目标函数可得
$$
\begin{aligned}
\min\limits_{\mathbf W}-\text { tr }(\mathbf W^{\mathrm{T}} \mathbf X\mathbf X^{\mathrm{T}} \mathbf W)&=\max\limits_{\mathbf W}\text { tr }(\mathbf W^{\mathrm{T}} \mathbf X\mathbf X^{\mathrm{T}} \mathbf W) \\
&=\max\limits_{\mathbf W}\sum_{i=1}^{d^{\prime}}\boldsymbol w_i^{\mathrm{T}}\mathbf X\mathbf X^{\mathrm{T}} \boldsymbol w_i \\
&=\max\limits_{\mathbf W}\sum_{i=1}^{d^{\prime}}\boldsymbol w_i^{\mathrm{T}}\cdot\lambda _i\boldsymbol w_i \\
&=\max\limits_{\mathbf W}\sum_{i=1}^{d^{\prime}}\lambda _i\boldsymbol w_i^{\mathrm{T}}\boldsymbol w_i \\
&=\max\limits_{\mathbf W}\sum_{i=1}^{d^{\prime}}\lambda _i \\
\end{aligned}
$$


显然，此时只需要令$\lambda_1,\lambda_2,...,\lambda_{d^{\prime}}$和$\boldsymbol{w}_{1}, \boldsymbol{w}_{2}, \ldots, \boldsymbol{w}_{d^{\prime}}$分别为矩阵$\mathbf X\mathbf X^{\mathrm{T}}$的前$d^{\prime}$个最大的特征值和单位特征向量就能使得目标函数达到最优值。

## 10.24
$$
\mathbf{K}\boldsymbol{\alpha}^j=\lambda_j\boldsymbol{\alpha}^j
$$

[推导]：已知$\boldsymbol z_i=\phi(\boldsymbol x_i)$，类比$\mathbf{X}=\{\boldsymbol x_1,\boldsymbol x_2,...,\boldsymbol x_m\}$可以构造$\mathbf{Z}=\{\boldsymbol z_1,\boldsymbol z_2,...,\boldsymbol z_m\}$，所以公式(10.21)可变换为
$$
\left(\sum_{i=1}^{m} \phi(\boldsymbol{x}_{i}) \phi(\boldsymbol{x}_{i})^{\mathrm{T}}\right)\boldsymbol w_j=\left(\sum_{i=1}^{m} \boldsymbol z_i \boldsymbol z_i^{\mathrm{T}}\right)\boldsymbol w_j=\mathbf{Z}\mathbf{Z}^{\mathrm{T}}\boldsymbol w_j=\lambda_j\boldsymbol w_j
$$
又由公式(10.22)可知
$$
\boldsymbol w_j=\sum_{i=1}^{m} \phi\left(\boldsymbol{x}_{i}\right) \alpha_{i}^j=\sum_{i=1}^{m} \boldsymbol z_i \alpha_{i}^j=\mathbf{Z}\boldsymbol{\alpha}^j
$$
其中，$\boldsymbol{\alpha}^j=(\alpha_{1}^j;\alpha_{2}^j;...;\alpha_{m}^j)\in \mathbb{R}^{m \times 1} $。所以公式(10.21)可以进一步变换为
$$
\begin{aligned}
\mathbf{Z}\mathbf{Z}^{\mathrm{T}}\mathbf{Z}\boldsymbol{\alpha}^j&=\lambda_j\mathbf{Z}\boldsymbol{\alpha}^j \\
\mathbf{Z}\mathbf{Z}^{\mathrm{T}}\mathbf{Z}\boldsymbol{\alpha}^j&=\mathbf{Z}\lambda_j\boldsymbol{\alpha}^j
\end{aligned}
$$
由于此时的目标是要求出$\boldsymbol w_j$，也就等价于要求出满足上式的$\boldsymbol{\alpha}^j$，显然，此时满足$\mathbf{Z}^{\mathrm{T}}\mathbf{Z}\boldsymbol{\alpha}^j=\lambda_j\boldsymbol{\alpha}^j $的$\boldsymbol{\alpha}^j$一定满足上式，所以问题转化为了求解满足下式的$\boldsymbol{\alpha}^j$：
$$
\mathbf{Z}^{\mathrm{T}}\mathbf{Z}\boldsymbol{\alpha}^j=\lambda_j\boldsymbol{\alpha}^j
$$


令$\mathbf{Z}^{\mathrm{T}}\mathbf{Z}=\mathbf{K}$，那么上式可化为
$$
\mathbf{K}\boldsymbol{\alpha}^j=\lambda_j\boldsymbol{\alpha}^j
$$


此式即为公式(10.24)，其中矩阵$\mathbf{K}$的第i行第j列的元素$(\mathbf{K})_{ij}=\boldsymbol z_i^{\mathrm{T}}\boldsymbol z_j=\phi(\boldsymbol x_i)^{\mathrm{T}}\phi(\boldsymbol x_j)=\kappa\left(\boldsymbol{x}_{i}, \boldsymbol{x}_{j}\right)$

## 10.28
$$
w_{i j}=\frac{\sum\limits_{k \in Q_{i}} C_{j k}^{-1}}{\sum\limits_{l, s \in Q_{i}} C_{l s}^{-1}}
$$



[推导]：由书中上下文可知，式(10.28)是如下优化问题的解。
$$
\begin{aligned} 
\min _{\boldsymbol{w}_{1}, \boldsymbol{w}_{2}, \ldots, \boldsymbol{w}_{m}} & \sum_{i=1}^{m}\left\|\boldsymbol{x}_{i}-\sum_{j \in Q_{i}} w_{i j} \boldsymbol{x}_{j}\right\|_{2}^{2} \\ 
\text { s.t. } & \sum_{j \in Q_{i}} w_{i j}=1 
\end{aligned}
$$


若令$\boldsymbol{x}_{i}\in \mathbb{R}^{d\times 1},Q_i=\{q_i^1,q_i^2,...,q_i^n\}$，则上述优化问题的目标函数可以进行如下恒等变形
$$
\begin{aligned} 
\sum_{i=1}^{m}\left\|\boldsymbol{x}_{i}-\sum_{j \in Q_{i}} w_{i j} \boldsymbol{x}_{j}\right\|_{2}^{2}&=\sum_{i=1}^{m}\left\|\sum_{j \in Q_{i}} w_{i j} \boldsymbol{x}_{i}-\sum_{j \in Q_{i}} w_{i j} \boldsymbol{x}_{j}\right\|_{2}^{2} \\ 
&=\sum_{i=1}^{m}\left\|\sum_{j \in Q_{i}} w_{i j}(\boldsymbol{x}_{i}-\boldsymbol{x}_{j}) \right\|_{2}^{2} \\ 
&=\sum_{i=1}^{m}\left\|\mathbf{X}_i\boldsymbol{w_i} \right\|_{2}^{2} \\
&=\sum_{i=1}^{m}\boldsymbol{w_i}^{\mathrm{T}}\mathbf{X}_i^{\mathrm{T}}\mathbf{X}_i\boldsymbol{w_i} \\ 
\end{aligned}
$$


其中$\boldsymbol{w_i}=(w_{iq_i^1},w_{iq_i^2},...,w_{iq_i^n})\in \mathbb{R}^{n\times 1}$，$\mathbf{X}_i=\left( \boldsymbol{x}_{i}-\boldsymbol{x}_{q_i^1}, \boldsymbol{x}_{i}-\boldsymbol{x}_{q_i^2},...,\boldsymbol{x}_{i}-\boldsymbol{x}_{q_i^n}\right)\in \mathbb{R}^{d\times n}$。同理，约束条件也可以进行如下恒等变形
$$
\sum_{j \in Q_{i}} w_{i j}=\boldsymbol{w_i}^{\mathrm{T}}\boldsymbol{I}=1 
$$


其中$\boldsymbol{I}=(1,1,...,1)\in \mathbb{R}^{n\times 1}$为$n$行1列的单位向量。因此，上述优化问题可以重写为
$$
\begin{aligned} 
\min _{\boldsymbol{w}_{1}, \boldsymbol{w}_{2}, \ldots, \boldsymbol{w}_{m}} & \sum_{i=1}^{m}\boldsymbol{w_i}^{\mathrm{T}}\mathbf{X}_i^{\mathrm{T}}\mathbf{X}_i\boldsymbol{w_i} \\ 
\text { s.t. } & \boldsymbol{w_i}^{\mathrm{T}}\boldsymbol{I}=1
\end{aligned}
$$


显然，此问题为带约束的优化问题，因此可以考虑使用拉格朗日乘子法来进行求解。由拉格朗日乘子法可得此优化问题的拉格朗日函数为
$$
L(\boldsymbol{w}_{1}, \boldsymbol{w}_{2}, \ldots, \boldsymbol{w}_{m},\lambda)=\sum_{i=1}^{m}\boldsymbol{w_i}^{\mathrm{T}}\mathbf{X}_i^{\mathrm{T}}\mathbf{X}_i\boldsymbol{w_i}+\lambda\left(\boldsymbol{w_i}^{\mathrm{T}}\boldsymbol{I}-1\right)
$$


对拉格朗日函数关于$\boldsymbol{w_i}$求偏导并令其等于0可得
$$
\begin{aligned} 
\cfrac{\partial L(\boldsymbol{w}_{1}, \boldsymbol{w}_{2}, \ldots, \boldsymbol{w}_{m},\lambda)}{\partial \boldsymbol{w_i}}&=\cfrac{\partial \left[\sum_{i=1}^{m}\boldsymbol{w_i}^{\mathrm{T}}\mathbf{X}_i^{\mathrm{T}}\mathbf{X}_i\boldsymbol{w_i}+\lambda\left(\boldsymbol{w_i}^{\mathrm{T}}\boldsymbol{I}-1\right)\right]}{\partial \boldsymbol{w_i}}=0\\
&=\cfrac{\partial \left[\boldsymbol{w_i}^{\mathrm{T}}\mathbf{X}_i^{\mathrm{T}}\mathbf{X}_i\boldsymbol{w_i}+\lambda\left(\boldsymbol{w_i}^{\mathrm{T}}\boldsymbol{I}-1\right)\right]}{\partial \boldsymbol{w_i}}=0\\
\end{aligned}
$$


又由矩阵微分公式$\cfrac{\partial \boldsymbol{x}^{T} \mathbf{B} \boldsymbol{x}}{\partial \boldsymbol{x}}=\left(\mathbf{B}+\mathbf{B}^{\mathrm{T}}\right) \boldsymbol{x},\cfrac{\partial \boldsymbol{x}^{T} \boldsymbol{a}}{\partial \boldsymbol{x}}=\boldsymbol{a}$可得
$$
\begin{aligned}
\cfrac{\partial \left[\boldsymbol{w_i}^{\mathrm{T}}\mathbf{X}_i^{\mathrm{T}}\mathbf{X}_i\boldsymbol{w_i}+\lambda\left(\boldsymbol{w_i}^{\mathrm{T}}\boldsymbol{I}-1\right)\right]}{\partial \boldsymbol{w_i}}&=2\mathbf{X}_i^{\mathrm{T}}\mathbf{X}_i\boldsymbol{w_i}+\lambda \boldsymbol{I}=0\\
\mathbf{X}_i^{\mathrm{T}}\mathbf{X}_i\boldsymbol{w_i}&=-\frac{1}{2}\lambda \boldsymbol{I}
\end{aligned}
$$
若$\mathbf{X}_i^{\mathrm{T}}\mathbf{X}_i$可逆，则
$$
\boldsymbol{w_i}=-\frac{1}{2}\lambda(\mathbf{X}_i^{\mathrm{T}}\mathbf{X}_i)^{-1}\boldsymbol{I}
$$


又因为$\boldsymbol{w_i}^{\mathrm{T}}\boldsymbol{I}=\boldsymbol{I}^{\mathrm{T}}\boldsymbol{w_i}=1$，则上式两边同时左乘$\boldsymbol{I}^{\mathrm{T}}$可得
$$
\begin{aligned}
\boldsymbol{I}^{\mathrm{T}}\boldsymbol{w_i}&=-\frac{1}{2}\lambda\boldsymbol{I}^{\mathrm{T}}(\mathbf{X}_i^{\mathrm{T}}\mathbf{X}_i)^{-1}\boldsymbol{I}=1\\
-\frac{1}{2}\lambda&=\cfrac{1}{\boldsymbol{I}^{\mathrm{T}}(\mathbf{X}_i^{\mathrm{T}}\mathbf{X}_i)^{-1}\boldsymbol{I}}
\end{aligned}
$$
将其代回$\boldsymbol{w_i}=-\frac{1}{2}\lambda(\mathbf{X}_i^{\mathrm{T}}\mathbf{X}_i)^{-1}\boldsymbol{I}$即可解得
$$
\boldsymbol{w_i}=\cfrac{(\mathbf{X}_i^{\mathrm{T}}\mathbf{X}_i)^{-1}\boldsymbol{I}}{\boldsymbol{I}^{\mathrm{T}}(\mathbf{X}_i^{\mathrm{T}}\mathbf{X}_i)^{-1}\boldsymbol{I}}
$$


若令矩阵$(\mathbf{X}_i^{\mathrm{T}}\mathbf{X}_i)^{-1}$第$j$行第$k$列的元素为$C_{jk}^{-1}$，则
$$
w_{ij}=w_{i q_i^j}=\frac{\sum\limits_{k \in Q_{i}} C_{j k}^{-1}}{\sum\limits_{l, s \in Q_{i}} C_{l s}^{-1}}
$$


此即为公式(10.28)。显然，若$\mathbf{X}_i^{\mathrm{T}}\mathbf{X}_i$可逆，此优化问题即为凸优化问题，且此时用拉格朗日乘子法求得的$\boldsymbol{w_i}$为全局最优解。

## 10.31
$$
\begin{aligned}
&\min\limits_{\boldsymbol Z}tr(\boldsymbol Z \boldsymbol M \boldsymbol Z^T)\\
&s.t. \boldsymbol Z^T\boldsymbol Z=\boldsymbol I. 
\end{aligned}
$$



[推导]：
$$
\begin{aligned}
\min\limits_{\boldsymbol Z}\sum^m_{i=1}\| \boldsymbol z_i-\sum_{j \in Q_i}w_{ij}\boldsymbol z_j \|^2_2&=\sum^m_{i=1}\|\boldsymbol Z\boldsymbol I_i-\boldsymbol Z\boldsymbol W_i\|^2_2\\
&=\sum^m_{i=1}\|\boldsymbol Z(\boldsymbol I_i-\boldsymbol W_i)\|^2_2\\
&=\sum^m_{i=1}(\boldsymbol Z(\boldsymbol I_i-\boldsymbol W_i))^T\boldsymbol Z(\boldsymbol I_i-\boldsymbol W_i)\\
&=\sum^m_{i=1}(\boldsymbol I_i-\boldsymbol W_i)^T\boldsymbol Z^T\boldsymbol Z(\boldsymbol I_i-\boldsymbol W_i)\\
&=tr((\boldsymbol I-\boldsymbol W)^T\boldsymbol Z^T\boldsymbol Z(\boldsymbol I-\boldsymbol W))\\
&=tr(\boldsymbol Z(\boldsymbol I-\boldsymbol W)(\boldsymbol I-\boldsymbol W)^T\boldsymbol Z^T)\\
&=tr(\boldsymbol Z\boldsymbol M\boldsymbol Z^T)
\end{aligned}
$$


其中，$\boldsymbol M=(\boldsymbol I-\boldsymbol W)(\boldsymbol I-\boldsymbol W)^T$。
[解析]：约束条件$\boldsymbol Z^T\boldsymbol Z=\boldsymbol I$是为了得到标准化（标准正交空间）的低维数据。

## 参考文献
<span id="ref1">[1][How to set up Lagrangian optimization with matrix constrains](https://math.stackexchange.com/questions/1104376/how-to-set-up-lagrangian-optimization-with-matrix-constrains)</span><br>
<span id="ref2">[2][Frobenius inner product](https://en.wikipedia.org/wiki/Frobenius_inner_product)</span>