## 10.4
$$\sum^m_{i=1}dist^2_{ij}=tr(\boldsymbol B)+mb_{jj}$$
[推导]：
$$\begin{aligned}
\sum^m_{i=1}dist^2_{ij}&= \sum^m_{i=1}b_{ii}+\sum^m_{i=1}b_{jj}-2\sum^m_{i=1}b_{ij}\\
&=tr(B)+mb_{jj}
\end{aligned}​$$

## 10.10
$$b_{ij}=-\frac{1}{2}(dist^2_{ij}-dist^2_{i\cdot}-dist^2_{\cdot j}+dist^2_{\cdot\cdot})$$
[推导]：由公式（10.3）可得，
$$b_{ij}=-\frac{1}{2}(dist^2_{ij}-b_{ii}-b_{jj})$$
由公式（10.6）和（10.9）可得，
$$\begin{aligned}
tr(\boldsymbol B)&=\frac{1}{2m}\sum^m_{i=1}\sum^m_{j=1}dist^2_{ij}\\
&=\frac{m}{2}dist^2_{\cdot\cdot}
\end{aligned}$$
由公式（10.4）和（10.8）可得，
$$\begin{aligned}
b_{jj}&=\frac{1}{m}\sum^m_{i=1}dist^2_{ij}-\frac{1}{m}tr(\boldsymbol B)\\
&=dist^2_{\cdot j}-\frac{1}{2}dist^2_{\cdot\cdot}
\end{aligned}$$
由公式（10.5）和（10.7）可得，
$$\begin{aligned}
b_{ii}&=\frac{1}{m}\sum^m_{j=1}dist^2_{ij}-\frac{1}{m}tr(\boldsymbol B)\\
&=dist^2_{i\cdot}-\frac{1}{2}dist^2_{\cdot\cdot}
\end{aligned}$$
综合可得，
$$\begin{aligned}
b_{ij}&=-\frac{1}{2}(dist^2_{ij}-b_{ii}-b_{jj})\\
&=-\frac{1}{2}(dist^2_{ij}-dist^2_{i\cdot}+\frac{1}{2}dist^2_{\cdot\cdot}-dist^2_{\cdot j}+\frac{1}{2}dist^2_{\cdot\cdot})\\
&=-\frac{1}{2}(dist^2_{ij}-dist^2_{i\cdot}-dist^2_{\cdot j}+dist^2_{\cdot\cdot})
\end{aligned}$$

## 10.14
$$\begin{aligned}
\sum^m_{i=1}\left\| \sum^{d'}_{j=1}z_{ij}\boldsymbol{w}-\boldsymbol x_i \right\|^2_2&=\sum^m_{i=1}\boldsymbol z^{\mathrm{T}}_i\boldsymbol z_i-2\sum^m_{i=1}\boldsymbol z^{\mathrm{T}}_i\mathbf{W}^{\mathrm{T}}\boldsymbol x_i +\text { const }\\
&\propto -\operatorname{tr}(\mathbf{W}^{\mathrm{T}}(\sum^m_{i=1}\boldsymbol x_i\boldsymbol x^{\mathrm{T}}_i)\mathbf{W})
\end{aligned}$$
[推导]：已知$\mathbf{W}^{\mathrm{T}} \mathbf{W}=\mathbf{I},\boldsymbol z_i=\mathbf{W}^{\mathrm{T}} \boldsymbol x_i$，则
$$\begin{aligned}
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
\end{aligned}$$

## 10.17
$$
\mathbf X\mathbf X^{\mathrm{T}} \boldsymbol w_i=\lambda _i\boldsymbol w_i
$$
[推导]：由式（10.15）可知，主成分分析的优化目标为
$$\begin{aligned}
&\min\limits_{\mathbf W} \quad-\text { tr }(\mathbf W^{\mathrm{T}} \mathbf X\mathbf X^{\mathrm{T}} \mathbf W)\\
&s.t. \quad\mathbf W^{\mathrm{T}} \mathbf W=\mathbf I
\end{aligned}$$
其中，$\mathbf{X}=\left(\boldsymbol{x}_{1}, \boldsymbol{x}_{2}, \ldots, \boldsymbol{x}_{m}\right) \in \mathbb{R}^{d \times m},\mathbf{W}=\left(\boldsymbol{w}_{1}, \boldsymbol{w}_{2}, \ldots, \boldsymbol{w}_{d^{\prime}}\right) \in \mathbb{R}^{d \times d^{\prime}}$，$\mathbf{I} \in \mathbb{R}^{d^{\prime} \times d^{\prime}}$为单位矩阵。对于带矩阵约束的优化问题，根据<a href="#ref1">[1]</a>中讲述的方法可得此优化目标的拉格朗日函数为
$$\begin{aligned}
L(\mathbf W,\Theta)&=-\text { tr }(\mathbf W^{\mathrm{T}} \mathbf X\mathbf X^{\mathrm{T}} \mathbf W)+\langle \Theta,\mathbf W^{\mathrm{T}} \mathbf W-\mathbf I\rangle \\
&=-\text { tr }(\mathbf W^{\mathrm{T}} \mathbf X\mathbf X^{\mathrm{T}} \mathbf W)+\text { tr }\left(\Theta^{\mathrm{T}} (\mathbf W^{\mathrm{T}} \mathbf W-\mathbf I)\right) 
\end{aligned}$$
其中，$\Theta  \in \mathbb{R}^{d^{\prime} \times d^{\prime}}$为拉格朗日乘子矩阵，其维度恒等于约束条件的维度，且其中的每个元素均为未知的拉格朗日乘子，$\langle \Theta,\mathbf W^{\mathrm{T}} \mathbf W-\mathbf I\rangle = \text { tr }\left(\Theta^{\mathrm{T}} (\mathbf W^{\mathrm{T}} \mathbf W-\mathbf I)\right)$为矩阵的内积<sup><a href="#ref2">[2]</a></sup>。若此时只考虑约束$\left\|\boldsymbol{w}_{i}\right\|_{2}=1(i=1,2,...,d^{\prime})$，则拉格朗日乘子矩阵$\Theta$此时为对角矩阵，令新的拉格朗日乘子矩阵为$\Lambda=diag(\lambda_1,\lambda_2,...,\lambda_{d^{\prime}})\in \mathbb{R}^{d^{\prime} \times d^{\prime}}$，则新的拉格朗日函数为
$$L(\mathbf W,\Lambda)=-\text { tr }(\mathbf W^{\mathrm{T}} \mathbf X\mathbf X^{\mathrm{T}} \mathbf W)+\text { tr }\left(\Lambda^{\mathrm{T}} (\mathbf W^{\mathrm{T}} \mathbf W-\mathbf I)\right) $$
对拉格朗日函数关于$\mathbf{W}$求导可得
$$\begin{aligned}
\cfrac{\partial L(\mathbf W,\Lambda)}{\partial \mathbf W}&=\cfrac{\partial}{\partial \mathbf W}\left[-\text { tr }(\mathbf W^{\mathrm{T}} \mathbf X\mathbf X^{\mathrm{T}} \mathbf W)+\text { tr }\left(\Lambda^{\mathrm{T}} (\mathbf W^{\mathrm{T}} \mathbf W-\mathbf I)\right)\right] \\
&=-\cfrac{\partial}{\partial \mathbf W}\text { tr }(\mathbf W^{\mathrm{T}} \mathbf X\mathbf X^{\mathrm{T}} \mathbf W)+\cfrac{\partial}{\partial \mathbf W}\text { tr }\left(\Lambda^{\mathrm{T}} (\mathbf W^{\mathrm{T}} \mathbf W-\mathbf I)\right) \\
\end{aligned}$$
由矩阵微分公式$\cfrac{\partial}{\partial \mathbf{X}} \text { tr }(\mathbf{X}^{T} \mathbf{B} \mathbf{X})=\mathbf{B X}+\mathbf{B}^{T} \mathbf{X},\cfrac{\partial}{\partial \mathbf{X}} \text { tr }\left(\mathbf{B X}^{T} \mathbf{X}\right)=\mathbf{X B}^{T}+\mathbf{X B}$可得
$$\begin{aligned}
\cfrac{\partial L(\mathbf W,\Lambda)}{\partial \mathbf W}&=-2\mathbf X\mathbf X^{\mathrm{T}} \mathbf W+\mathbf{W}\Lambda+\mathbf{W}\Lambda^{\mathrm{T}}  \\
&=-2\mathbf X\mathbf X^{\mathrm{T}} \mathbf W+\mathbf{W}(\Lambda+\Lambda^{\mathrm{T}} ) \\
&=-2\mathbf X\mathbf X^{\mathrm{T}} \mathbf W+2\mathbf{W}\Lambda
\end{aligned}$$
令$\cfrac{\partial L(\mathbf W,\Lambda)}{\partial \mathbf W}=\mathbf 0$可得
$$-2\mathbf X\mathbf X^{\mathrm{T}} \mathbf W+2\mathbf{W}\Lambda=\mathbf 0$$
$$\mathbf X\mathbf X^{\mathrm{T}} \mathbf W=\mathbf{W}\Lambda$$
将$\mathbf W$和$\Lambda$展开可得
$$\mathbf X\mathbf X^{\mathrm{T}} \boldsymbol w_i=\lambda _i\boldsymbol w_i,\quad i=1,2,...,d^{\prime}$$
显然，此式为矩阵特征值和特征向量的定义式，其中$\lambda_i,\boldsymbol w_i$分别表示矩阵$\mathbf X\mathbf X^{\mathrm{T}}$的特征值和特征向量。由于$\mathbf X\mathbf X^{\mathrm{T}} $是实对称矩阵，而实对称矩阵的不同特征值所对应的特征向量之间相互正交，同一特征值的不同特征向量可以通过施密特正交化使其变得正交，所以$\boldsymbol w_i$同时还满足约束$\boldsymbol{w}_{i}^{\mathrm{T}}\boldsymbol{w}_{j}=0(i\neq j)$。又因为优化目标的目标函数为
$$\begin{aligned}
\min\limits_{\mathbf W}-\text { tr }(\mathbf W^{\mathrm{T}} \mathbf X\mathbf X^{\mathrm{T}} \mathbf W)&=\max\limits_{\mathbf W}\text { tr }(\mathbf W^{\mathrm{T}} \mathbf X\mathbf X^{\mathrm{T}} \mathbf W) \\
&=\max\limits_{\mathbf W}\sum_{i=1}^{d^{\prime}}\boldsymbol w_i^{\mathrm{T}}\mathbf X\mathbf X^{\mathrm{T}} \boldsymbol w_i \\
&=\max\limits_{\mathbf W}\sum_{i=1}^{d^{\prime}}\boldsymbol w_i^{\mathrm{T}}\cdot\lambda _i\boldsymbol w_i \\
&=\max\limits_{\mathbf W}\sum_{i=1}^{d^{\prime}}\lambda _i\boldsymbol w_i^{\mathrm{T}}\boldsymbol w_i \\
&=\max\limits_{\mathbf W}\sum_{i=1}^{d^{\prime}}\lambda _i \\
\end{aligned}$$
所以只需要令$\lambda_1,\lambda_2,...,\lambda_{d^{\prime}}$和$\boldsymbol{w}_{1}, \boldsymbol{w}_{2}, \ldots, \boldsymbol{w}_{d^{\prime}}$分别为矩阵$\mathbf X\mathbf X^{\mathrm{T}}$的前$d^{\prime}$个最大的特征值和特征向量就能保证目标函数达到最优值。

## 10.24
$$\mathbf{K}\boldsymbol{\alpha}^j=\lambda_j\boldsymbol{\alpha}^j $$
[推导]：已知$\boldsymbol z_i=\phi(\boldsymbol x_i)$，类比$\mathbf{X}=\{\boldsymbol x_1,\boldsymbol x_2,...,\boldsymbol x_m\}$可以构造$\mathbf{Z}=\{\boldsymbol z_1,\boldsymbol z_2,...,\boldsymbol z_m\}$，所以公式(10.21)可变换为
$$\left(\sum_{i=1}^{m} \phi(\boldsymbol{x}_{i}) \phi(\boldsymbol{x}_{i})^{\mathrm{T}}\right)\boldsymbol w_j=\left(\sum_{i=1}^{m} \boldsymbol z_i \boldsymbol z_i^{\mathrm{T}}\right)\boldsymbol w_j=\mathbf{Z}\mathbf{Z}^{\mathrm{T}}\boldsymbol w_j=\lambda_j\boldsymbol w_j $$
又由公式(10.22)可知
$$\boldsymbol w_j=\sum_{i=1}^{m} \phi\left(\boldsymbol{x}_{i}\right) \alpha_{i}^j=\sum_{i=1}^{m} \boldsymbol z_i \alpha_{i}^j=\mathbf{Z}\boldsymbol{\alpha}^j$$
其中，$\boldsymbol{\alpha}^j=(\alpha_{1}^j;\alpha_{2}^j;...;\alpha_{m}^j)\in \mathbb{R}^{m \times 1} $。所以公式(10.21)可以进一步变换为
$$\mathbf{Z}\mathbf{Z}^{\mathrm{T}}\mathbf{Z}\boldsymbol{\alpha}^j=\lambda_j\mathbf{Z}\boldsymbol{\alpha}^j $$
$$\mathbf{Z}\mathbf{Z}^{\mathrm{T}}\mathbf{Z}\boldsymbol{\alpha}^j=\mathbf{Z}\lambda_j\boldsymbol{\alpha}^j $$
由于此时的目标是要求出$\boldsymbol w_j$，也就等价于要求出满足上式的$\boldsymbol{\alpha}^j$，显然，此时满足$\mathbf{Z}^{\mathrm{T}}\mathbf{Z}\boldsymbol{\alpha}^j=\lambda_j\boldsymbol{\alpha}^j $的$\boldsymbol{\alpha}^j$一定满足上式，所以问题转化为了求解满足下式的$\boldsymbol{\alpha}^j$：
$$\mathbf{Z}^{\mathrm{T}}\mathbf{Z}\boldsymbol{\alpha}^j=\lambda_j\boldsymbol{\alpha}^j $$
令$\mathbf{Z}^{\mathrm{T}}\mathbf{Z}=\mathbf{K}$，那么上式可化为
$$\mathbf{K}\boldsymbol{\alpha}^j=\lambda_j\boldsymbol{\alpha}^j $$
此式即为公式(10.24)，其中矩阵$\mathbf{K}$的第i行第j列的元素$(\mathbf{K})_{ij}=\boldsymbol z_i^{\mathrm{T}}\boldsymbol z_j=\phi(\boldsymbol x_i)^{\mathrm{T}}\phi(\boldsymbol x_j)=\kappa\left(\boldsymbol{x}_{i}, \boldsymbol{x}_{j}\right)$

## 10.28
$$w_{ij}=\cfrac{\sum\limits_{k\in Q_i}C_{jk}^{-1}}{\sum\limits_{l,s\in Q_i}C_{ls}^{-1}}$$
[推导]：已知
$$\begin{aligned}
\min\limits_{\boldsymbol W}&\sum^m_{i=1}\| \boldsymbol x_i-\sum_{j \in Q_i}w_{ij}\boldsymbol x_j \|^2_2\\
s.t.&\sum_{j \in Q_i}w_{ij}=1
\end{aligned}$$
转换为
$$\begin{aligned}
\sum^m_{i=1}\| \boldsymbol x_i-\sum_{j \in Q_i}w_{ij}\boldsymbol x_j \|^2_2 &=\sum^m_{i=1}\| \sum_{j \in Q_i}w_{ij}\boldsymbol x_i- \sum_{j \in Q_i}w_{ij}\boldsymbol x_j \|^2_2 \\
&=\sum^m_{i=1}\| \sum_{j \in Q_i}w_{ij}(\boldsymbol x_i- \boldsymbol x_j) \|^2_2\\
&=\sum^m_{i=1}\boldsymbol W^T_i(\boldsymbol x_i-\boldsymbol x_j)(\boldsymbol x_i-\boldsymbol x_j)^T\boldsymbol W_i\\
&=\sum^m_{i=1}\boldsymbol W^T_i\boldsymbol C_i\boldsymbol W_i
\end{aligned}$$
其中，$\boldsymbol W_i=(w_{i1},w_{i2},\cdot\cdot\cdot,w_{ik})^T$，$k$是$Q_i$集合的长度，$\boldsymbol C_i=(\boldsymbol x_i-\boldsymbol x_j)(\boldsymbol x_i-\boldsymbol x_j)^T$，$j \in Q_i$。
$$
\sum_{j\in Q_i}w_{ij}=\boldsymbol W_i^T\boldsymbol 1_k=1
$$
其中，$\boldsymbol 1_k$为k维全1向量。
运用拉格朗日乘子法可得，
$$\begin{aligned}
J(\boldsymbol W)&=\sum^m_{i=1}\boldsymbol W^T_i\boldsymbol C_i\boldsymbol W_i+\lambda(\boldsymbol W_i^T\boldsymbol 1_k-1)\\
\cfrac{\partial J(\boldsymbol W)}{\partial \boldsymbol W_i} &=2\boldsymbol C_i\boldsymbol W_i+\lambda\boldsymbol 1_k
\end{aligned}$$
令$\cfrac{\partial J(\boldsymbol W)}{\partial \boldsymbol W_i}=0$，故
$$\begin{aligned}
\boldsymbol W_i&=-\cfrac{1}{2}\lambda\boldsymbol C_i^{-1}\boldsymbol 1_k\\
\boldsymbol W_i&=\lambda\boldsymbol C_i^{-1}\boldsymbol 1_k\\
\end{aligned}$$
其中，$\lambda$为一个常数。利用$\boldsymbol W^T_i\boldsymbol 1_k=1$，对$\boldsymbol W_i$归一化，可得
$$
\boldsymbol W_i=\cfrac{\boldsymbol C^{-1}_i\boldsymbol 1_k}{\boldsymbol 1_k\boldsymbol C^{-1}_i\boldsymbol 1_k}
$$

## 10.31
$$\begin{aligned}
&\min\limits_{\boldsymbol Z}tr(\boldsymbol Z \boldsymbol M \boldsymbol Z^T)\\
&s.t. \boldsymbol Z^T\boldsymbol Z=\boldsymbol I. 
\end{aligned}$$
[推导]：
$$\begin{aligned}
\min\limits_{\boldsymbol Z}\sum^m_{i=1}\| \boldsymbol z_i-\sum_{j \in Q_i}w_{ij}\boldsymbol z_j \|^2_2&=\sum^m_{i=1}\|\boldsymbol Z\boldsymbol I_i-\boldsymbol Z\boldsymbol W_i\|^2_2\\
&=\sum^m_{i=1}\|\boldsymbol Z(\boldsymbol I_i-\boldsymbol W_i)\|^2_2\\
&=\sum^m_{i=1}(\boldsymbol Z(\boldsymbol I_i-\boldsymbol W_i))^T\boldsymbol Z(\boldsymbol I_i-\boldsymbol W_i)\\
&=\sum^m_{i=1}(\boldsymbol I_i-\boldsymbol W_i)^T\boldsymbol Z^T\boldsymbol Z(\boldsymbol I_i-\boldsymbol W_i)\\
&=tr((\boldsymbol I-\boldsymbol W)^T\boldsymbol Z^T\boldsymbol Z(\boldsymbol I-\boldsymbol W))\\
&=tr(\boldsymbol Z(\boldsymbol I-\boldsymbol W)(\boldsymbol I-\boldsymbol W)^T\boldsymbol Z^T)\\
&=tr(\boldsymbol Z\boldsymbol M\boldsymbol Z^T)
\end{aligned}$$
其中，$\boldsymbol M=(\boldsymbol I-\boldsymbol W)(\boldsymbol I-\boldsymbol W)^T$。
[解析]：约束条件$\boldsymbol Z^T\boldsymbol Z=\boldsymbol I$是为了得到标准化（标准正交空间）的低维数据。

## 参考文献
<span id="ref1">[1][How to set up Lagrangian optimization with matrix constrains](https://math.stackexchange.com/questions/1104376/how-to-set-up-lagrangian-optimization-with-matrix-constrains)</span>
<span id="ref2">[2][Frobenius inner product](https://en.wikipedia.org/wiki/Frobenius_inner_product)</span>