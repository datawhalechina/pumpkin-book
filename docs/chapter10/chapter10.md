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
\boldsymbol X\boldsymbol X^T\boldsymbol w_i=\lambda _i\boldsymbol w_i
$$
[推导]：已知
$$\begin{aligned}
&\min\limits_{\boldsymbol W}-tr(\boldsymbol W^T\boldsymbol X\boldsymbol X^T\boldsymbol W)\\
&s.t. \boldsymbol W^T\boldsymbol W=\boldsymbol I. 
\end{aligned}$$
运用拉格朗日乘子法可得，
$$\begin{aligned}
J(\boldsymbol W)&=-tr(\boldsymbol W^T\boldsymbol X\boldsymbol X^T\boldsymbol W+\boldsymbol\lambda'(\boldsymbol W^T\boldsymbol W-\boldsymbol I))\\
\cfrac{\partial J(\boldsymbol W)}{\partial \boldsymbol W} &=-(2\boldsymbol X\boldsymbol X^T\boldsymbol W+2\boldsymbol\lambda'\boldsymbol W)
\end{aligned}$$
令$\cfrac{\partial J(\boldsymbol W)}{\partial \boldsymbol W}=\boldsymbol 0$，故
$$\begin{aligned}
\boldsymbol X\boldsymbol X^T\boldsymbol W&=-\boldsymbol\lambda'\boldsymbol W\\
\boldsymbol X\boldsymbol X^T\boldsymbol W&=\boldsymbol\lambda\boldsymbol W\\
\end{aligned}$$
其中，$\boldsymbol W=\{\boldsymbol w_1,\boldsymbol w_2,\cdot\cdot\cdot,\boldsymbol w_d\}$和$\boldsymbol \lambda=\boldsymbol{diag}(\lambda_1,\lambda_2,\cdot\cdot\cdot,\lambda_d)$。

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
