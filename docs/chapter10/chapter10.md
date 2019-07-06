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
\sum^m_{i=1}\| \sum^{d'}_{j=1}z_{ij}\boldsymbol w_j-\boldsymbol x_i \|^2_2&=\sum^m_{i=1}\boldsymbol z^T_i\boldsymbol z_i-2\sum^m_{i=1}\boldsymbol z^T_i\boldsymbol W^T\boldsymbol x_i + const\\
&\propto -tr(\boldsymbol W^T(\sum^m_{i=1}\boldsymbol x_i\boldsymbol x^T_i)\boldsymbol W)
\end{aligned}$$
[推导]：已知$\boldsymbol W^T \boldsymbol W=\boldsymbol I$和$\boldsymbol z_i=\boldsymbol W^T \boldsymbol x_i$，
$$\begin{aligned}
\sum^m_{i=1}\| \sum^{d'}_{j=1}z_{ij}\boldsymbol w_j-\boldsymbol x_i \|^2_2&=\sum^m_{i=1}\| \boldsymbol W\boldsymbol z_i-\boldsymbol x_i \|^2_2\\
&=\sum^m_{i=1}(\boldsymbol W\boldsymbol z_i)^T(\boldsymbol W\boldsymbol z_i)-2\sum^m_{i=1}(\boldsymbol W\boldsymbol z_i)^T\boldsymbol x_i+\sum^m_{i=1}\boldsymbol x^T_i\boldsymbol x_i\\
&=\sum^m_{i=1}\boldsymbol z_i^T\boldsymbol z_i-2\sum^m_{i=1}\boldsymbol z_i^T\boldsymbol W^T\boldsymbol x_i+\sum^m_{i=1}\boldsymbol x^T_i\boldsymbol x_i\\
&=\sum^m_{i=1}\boldsymbol z_i^T\boldsymbol z_i-2\sum^m_{i=1}\boldsymbol z_i^T\boldsymbol z_i+\sum^m_{i=1}\boldsymbol x^T_i\boldsymbol x_i\\
&=-\sum^m_{i=1}\boldsymbol z_i^T\boldsymbol z_i+\sum^m_{i=1}\boldsymbol x^T_i\boldsymbol x_i\\
&=-tr(\boldsymbol W^T(\sum^m_{i=1}\boldsymbol x_i\boldsymbol x^T_i)\boldsymbol W)+\sum^m_{i=1}\boldsymbol x^T_i\boldsymbol x_i\\
&\propto -tr(\boldsymbol W^T(\sum^m_{i=1}\boldsymbol x_i\boldsymbol x^T_i)\boldsymbol W)
\end{aligned}$$
其中，$\sum^m_{i=1}\boldsymbol x^T_i\boldsymbol x_i$是常数。

## 10.17
$$
\mathbf X\mathbf X^T\boldsymbol w_i=\lambda _i\boldsymbol w_i
$$
[推导]：由式（10.15）可知，主成分分析的优化目标为
$$\begin{aligned}
&\min\limits_{\mathbf W} \quad-\text { tr }(\mathbf W^T\mathbf X\mathbf X^T\mathbf W)\\
&s.t. \quad\mathbf W^T\mathbf W=\mathbf I
\end{aligned}$$
其中，$\mathbf{X}=\left(\boldsymbol{x}_{1}, \boldsymbol{x}_{2}, \ldots, \boldsymbol{x}_{m}\right) \in \mathbb{R}^{d \times m},\mathbf{W}=\left(\boldsymbol{w}_{1}, \boldsymbol{w}_{2}, \ldots, \boldsymbol{w}_{d}\right) \in \mathbb{R}^{d \times d}$，且$\mathbf{W}$为正交矩阵，$\mathbf{I} \in \mathbb{R}^{d \times d}$为单位矩阵。对于带矩阵约束的优化问题，根据[How to set up Lagrangian optimization with matrix constrains](https://math.stackexchange.com/questions/1104376/how-to-set-up-lagrangian-optimization-with-matrix-constrains)中讲述的方法可得上述优化目标的拉格朗日函数为
$$L(\mathbf W)=-\text { tr }(\mathbf W^T\mathbf X\mathbf X^T\mathbf W)+\langle \Theta,\mathbf W^T\mathbf W-\mathbf I\rangle$$
其中，$\Theta  \in \mathbb{R}^{d \times d}$为拉格朗日乘子矩阵，其维度恒等于约束条件的维度，且其中的每个元素均为未知的拉格朗日乘子，$\langle \mathbf A, \mathbf B \rangle = \text { tr }(\mathbf A^T \mathbf B) = \sum\limits_{i,j} \mathbf A_{ij} \mathbf B_{ij}$为[矩阵的内积](https://en.wikipedia.org/wiki/Frobenius_inner_product)
，根据矩阵内积的运算性质我们可以将拉格朗日函数恒等变形为
$$ L(\mathbf W)=-\text { tr }(\mathbf W^T\mathbf X\mathbf X^T\mathbf W)+\text { tr }\left(\Theta^T(\mathbf W^T\mathbf W-\mathbf I)\right) $$
对拉格朗日函数关于$\mathbf{W}$求导可得
$$\begin{aligned}
\cfrac{\partial L(\mathbf W)}{\partial \mathbf W}&=\cfrac{\partial}{\partial \mathbf W}\left[-\text { tr }(\mathbf W^T\mathbf X\mathbf X^T\mathbf W)+\text { tr }\left(\Theta^T(\mathbf W^T\mathbf W-\mathbf I)\right)\right] \\
&=-\cfrac{\partial}{\partial \mathbf W}\text { tr }(\mathbf W^T\mathbf X\mathbf X^T\mathbf W)+\cfrac{\partial}{\partial \mathbf W}\text { tr }\left(\Theta^T(\mathbf W^T\mathbf W-\mathbf I)\right) \\
\end{aligned}$$
由矩阵微分公式$\cfrac{\partial}{\partial \mathbf{X}} \text { tr }(\mathbf{X}^{T} \mathbf{B} \mathbf{X})=\mathbf{B X}+\mathbf{B}^{T} \mathbf{X},\cfrac{\partial}{\partial \mathbf{X}} \text { tr }\left(\mathbf{B X}^{T} \mathbf{X}\right)=\mathbf{X B}^{T}+\mathbf{X B}$可得
$$\begin{aligned}
\cfrac{\partial L(\mathbf W)}{\partial \mathbf W}&=-2\mathbf X\mathbf X^T\mathbf W+\mathbf{W}\Theta+\mathbf{W}\Theta^T \\
&=-2\mathbf X\mathbf X^T\mathbf W+\mathbf{W}(\Theta+\Theta^T)
\end{aligned}$$
令$\cfrac{\partial L(\mathbf W)}{\partial \mathbf W}=\mathbf 0$可得
$$-2\mathbf X\mathbf X^T\mathbf W+\mathbf{W}(\Theta+\Theta^T)=\mathbf 0$$
$$\mathbf X\mathbf X^T\mathbf W=\cfrac{1}{2}\mathbf{W}(\Theta+\Theta^T)$$
令$\Lambda=\cfrac{1}{2}(\Theta+\Theta^T)$，则上式可化为
$$\mathbf X\mathbf X^T\mathbf W=\mathbf{W}\Lambda$$
又因为$\mathbf{W}$满足约束$\mathbf W^T\mathbf W=\mathbf I$，则考虑对上式两边同时左乘上一个$\mathbf{W}^T$可得
$$\mathbf{W}^T\mathbf X\mathbf X^T\mathbf W=\mathbf{W}^T\mathbf{W}\Lambda$$
$$\mathbf{W}^T\mathbf X\mathbf X^T\mathbf W=\Lambda$$
又因为$\mathbf{W}$是正交矩阵，所以$\mathbf{W}^T=\mathbf{W}^{-1}$，于是上式可化为
$$\mathbf{W}^{-1}\mathbf X\mathbf X^T\mathbf W=\Lambda$$
仔细观察目前得到的这个式子可以发现，此式为线性代数里经典的矩阵相似问题，也就是给定矩阵$\mathbf X\mathbf X^T$求其相似矩阵$\Lambda$以及相似变换矩阵$\mathbf{W}$，并且$\Lambda$和$\mathbf{W}$的解不唯一。尽管$\Lambda$和$\mathbf{W}$的解不唯一，但是根据相似矩阵的性质可知$\Lambda$的迹与$\mathbf X\mathbf X^T$的迹恒相等，则优化目标的函数值也恒保持不变，也即
$$-\text { tr }(\mathbf W^T\mathbf X\mathbf X^T\mathbf W)=-\text { tr }(\mathbf W^{-1}\mathbf X\mathbf X^T\mathbf W)=-\text { tr }(\Lambda)=-\text { tr }(\mathbf X\mathbf X^T)$$
所以我们只需要求出任意一个满足$\mathbf{W}^{-1}\mathbf X\mathbf X^T\mathbf W=\Lambda$的$\Lambda$和$\mathbf{W}$即可。由于$\mathbf X\mathbf X^T$是实对称矩阵，实对称矩阵一定正交相似于由其特征值构成的对角矩阵，且相似变换矩阵是由其特征向量构成，所以我们可以令$\mathbf W=\left(\boldsymbol{w}_{1}, \boldsymbol{w}_{2},...,\boldsymbol{w}_{d} \right)\in \mathbb{R}^{d \times d}$为由$\mathbf X\mathbf X^T$的$d$个相互正交的特征向量$\boldsymbol{w}_{i}$构成的正交矩阵，$\Lambda=\text{diag}(\lambda_1,\lambda_2,...,\lambda_d)\in \mathbb{R}^{d \times d}$为由$\mathbf X\mathbf X^T$的$d$个特征值$\lambda_i$构成的对角矩阵，因此求出了$\mathbf X\mathbf X^T$的特征值和特征向量也就求出了$\Lambda$和$\mathbf{W}$。按照特征值和特征向量的定义可知
$$\mathbf X\mathbf X^T\boldsymbol w_i=\lambda _i\boldsymbol w_i$$
此即为式（10.17）。

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
$$
J(\boldsymbol W)==\sum^m_{i=1}\boldsymbol W^T_i\boldsymbol C_i\boldsymbol W_i+\lambda(\boldsymbol W_i^T\boldsymbol 1_k-1)
$$
$$\begin{aligned}
\cfrac{\partial J(\boldsymbol W)}{\partial \boldsymbol W_i} &=2\boldsymbol C_i\boldsymbol W_i+\lambda'\boldsymbol 1_k
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
