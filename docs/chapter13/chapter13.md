## 13.1

$$
p(\boldsymbol{x})=\sum_{i=1}^{N} \alpha_{i} \cdot p\left(\boldsymbol{x} | \boldsymbol{\mu}_{i}, \mathbf{\Sigma}_{i}\right)
$$

[解析]： 高斯混合分布的定义式

## 13.2

$$
\begin{aligned} f(\boldsymbol{x}) &=\underset{j \in \mathcal{Y}}{\arg \max } p(y=j | \boldsymbol{x}) \\ &=\underset{j \in \mathcal{Y}}{\arg \max } \sum_{i=1}^{N} p(y=j, \Theta=i | \boldsymbol{x}) \\ &=\underset{j \in \mathcal{Y}}{\arg \max } \sum_{i=1}^{N} p(y=j | \Theta=i, \boldsymbol{x}) \cdot p(\Theta=i | \boldsymbol{x}) \end{aligned}
$$

[解析]：从公式第 1 行到第 2 行是对概率进行边缘化(marginalization)；通过引入$\Theta$并对其求和 $$\sum_{i=1}^N$$以抵消引入的影响。从公式第 2 行到第 3 行推导如下
$$
\begin{aligned}p(y=j, \Theta=i | \boldsymbol{x}) &=\frac{p(y=j, \Theta=i, \boldsymbol{x})}{p(\boldsymbol{x})} \\&=\frac{p(y=j, \Theta=i, \boldsymbol{x})}{p(\Theta=i, \boldsymbol{x})} \cdot \frac{p(\Theta=i, \boldsymbol{x})}{p(\boldsymbol{x})} \\&=p(y=j | \Theta=i, \boldsymbol{x}) \cdot p(\Theta=i | \boldsymbol{x})\end{aligned}
$$

## 13.3

$$
p(\Theta=i | \boldsymbol{x})=\frac{\alpha_{i} \cdot p\left(\boldsymbol{x} | \boldsymbol{\mu}_{i}, \mathbf{\Sigma}_{i}\right)}{\sum_{i=1}^{N} \alpha_{i} \cdot p\left(\boldsymbol{x} | \boldsymbol{\mu}_{i}, \mathbf{\Sigma}_{i}\right)}
$$

[解析]：根据 13.1 
$$
p(\boldsymbol{x})=\sum_{i=1}^{N} \alpha_{i} \cdot p\left(\boldsymbol{x} | \boldsymbol{\mu}_{i}, \mathbf{\Sigma}_{i}\right)
$$
因此
$$
\begin{aligned}p(\Theta=i | \boldsymbol{x})&=\frac{p(\Theta=i , \boldsymbol{x})}{P(x)}\\&=\frac{\alpha_{i} \cdot p\left(\boldsymbol{x} | \boldsymbol{\mu}_{i}, \mathbf{\Sigma}_{i}\right)}{\sum_{i=1}^{N} \alpha_{i} \cdot p\left(\boldsymbol{x} | \boldsymbol{\mu}_{i}, \mathbf{\Sigma}_{i}\right)}\end{aligned}
$$

## 13.4

$$
\begin{aligned} L L\left(D_{l} \cup D_{u}\right)=& \sum_{\left(x_{j}, y_{j}\right) \in D_{l}} \ln \left(\sum_{i=1}^{N} \alpha_{i} \cdot p\left(\boldsymbol{x}_{j} | \boldsymbol{\mu}_{i}, \mathbf{\Sigma}_{i}\right) \cdot p\left(y_{j} | \Theta=i, \boldsymbol{x}_{j}\right)\right) \\ &+\sum_{x_{j} \in D_{u}} \ln \left(\sum_{i=1}^{N} \alpha_{i} \cdot p\left(\boldsymbol{x}_{j} | \boldsymbol{\mu}_{i}, \mathbf{\Sigma}_{i}\right)\right) \end{aligned}
$$

[解析]：第二项很好解释，当不知道类别信息的时候，样本$x_j$的概率可以用式 13.1 表示，所有无类别信息的样本$D_u$的似然是所有样本的乘积，因为$\ln$函数是单调的，所以也可以将$\ln$函数作用于这个乘积消除因为连乘产生的数值计算问题。第一项引入了样本的标签信息，由
$$
p(y=j | \Theta=i, \boldsymbol{x})=\left\{\begin{array}{ll}1, & i=j \\0, & i \neq j\end{array}\right.
$$
可知，这项限定了样本$$x_j$$只可能来自于$$y_j$$所对应的高斯分布。

## 13.5

$$
\gamma_{j i}=\frac{\alpha_{i} \cdot p\left(\boldsymbol{x}_{j} | \boldsymbol{\mu}_{i}, \mathbf{\Sigma}_{i}\right)}{\sum_{i=1}^{N} \alpha_{i} \cdot p\left(\boldsymbol{x}_{j} | \boldsymbol{\mu}_{i}, \mathbf{\Sigma}_{i}\right)}
$$

[解析]：参见式 13.3，这项可以理解成样本$x_j$属于类别标签$i$(或者说由第$i$个高斯分布生成)的后验概率。其中$\alpha_i,\boldsymbol{\mu}_{i}\boldsymbol{\Sigma}_i$可以通过有标记样本预先计算出来。即：
$$
\begin{array}{l}\alpha_{i}=\frac{l_{i}}{\left|D_{l}\right|}, \text { where }\left|D_{l}\right|=\sum_{i=1}^{N} l_{i} \\\boldsymbol{\mu}_{i}=\frac{1}{l_{i}} \sum_{\left(\boldsymbol{x}_{j}, y_{j}\right) \in D_{l} \wedge y_{j}=i} \boldsymbol{x}_{j} \\\boldsymbol{\Sigma}_{i}=\frac{1}{l_{i}} \sum_{\left(\boldsymbol{x}_{j}, y_{j}\right) \in D_{l} \wedge y_{j}=i}\left(\boldsymbol{x}_{j}-\boldsymbol{\mu}_{i}\right)\left(\boldsymbol{x}_{j}-\boldsymbol{\mu}_{i}\right)^{\top}\end{array}
$$


## 13.6

$$
\boldsymbol{\mu}_{i}=\frac{1}{\sum_{\boldsymbol{x}_{j} \in D_{u}} \gamma_{j i}+l_{i}}\left(\sum_{\boldsymbol{x}_{j} \in D_{u}} \gamma_{j i} \boldsymbol{x}_{j}+\sum_{\left(\boldsymbol{x}_{j}, y_{j}\right) \in D_{l} \wedge y_{j}=i} \boldsymbol{x}_{j}\right)
$$

[推导]：这项可以由$$\cfrac{\partial LL(D_l \cup D_u) }{\partial \mu_i}=0$$而得，将式 13.4 的两项分别记为：
$$
\begin{aligned}LL(D_l)&=\sum_{(\boldsymbol{x_j},y_j \in D_l)}\ln\left(\sum_{s=1}^{N}\alpha_s \cdot p(\boldsymbol{x_j}\vert \boldsymbol{\mu}_s,\boldsymbol{\Sigma}_s) \cdot p(y_i|\Theta = s,\boldsymbol{x_j}\right)\\&=\sum_{(\boldsymbol{x_j},y_j \in D_l)}\ln\left(\sum_{s=1}^{N}\alpha_{y_j} \cdot p(\boldsymbol{x_j} \vert \boldsymbol{\mu}_{y_j},\boldsymbol{\Sigma}_{y_j})\right)\\LL(D_u)&=\sum_{\boldsymbol{x_j} \in D_u} \ln\left(\sum_{s=1}^N \alpha_s \cdot p(\boldsymbol{x_j} | \boldsymbol{\mu}_s,\boldsymbol{\Sigma}_s)\right)\end{aligned}
$$
首先，$LL(D_l)$对$$\boldsymbol{\mu_i}$$求偏导，$LL(D_l)$求和号中只有$y_j=i$ 的项能留下来，即
$$
\begin{aligned}\frac{\partial L L\left(D_{l}\right)}{\partial \boldsymbol{\mu}_{i}} &=\sum_{\left(\boldsymbol{x}_{j}, y_{j}\right) \in D_{l} \wedge y_{j}=i} \frac{\partial \ln \left(\alpha_{i} \cdot p\left(\boldsymbol{x}_{j} | \boldsymbol{\mu}_{i}, \boldsymbol{\Sigma}_{i}\right)\right)}{\partial \boldsymbol{\mu}_{i}} \\&=\sum_{\left(\boldsymbol{x}_{j}, y_{j}\right) \in D_{l} \wedge y_{j}=i} \frac{1}{p\left(\boldsymbol{x}_{j} | \boldsymbol{\mu}_{i}, \boldsymbol{\Sigma}_{i}\right)} \cdot \frac{\partial p\left(\boldsymbol{x}_{j} | \boldsymbol{\mu}_{i}, \boldsymbol{\Sigma}_{i}\right)}{\partial \boldsymbol{\mu}_{i}} \\&=\sum_{\left(\boldsymbol{x}_{j}, y_{j}\right) \in D_{l} \wedge y_{j}=i} \frac{1}{p\left(\boldsymbol{x}_{j} | \boldsymbol{\mu}_{i}, \boldsymbol{\Sigma}_{i}\right)} \cdot p\left(\boldsymbol{x}_{j} | \boldsymbol{\mu}_{i}, \boldsymbol{\Sigma}_{i}\right) \cdot \boldsymbol{\Sigma}_{i}^{-1}\left(\boldsymbol{x}_{j}-\boldsymbol{\mu}_{i}\right) \\&=\sum_{\left(\boldsymbol{x}_{j}, y_{j}\right) \in D_{l} \wedge y_{j}=i} \boldsymbol{\Sigma}_{i}^{-1}\left(\boldsymbol{x}_{j}-\boldsymbol{\mu}_{i}\right)\end{aligned}
$$
$LL(D_u)$对$$\boldsymbol{\mu_i}$$求导，参考 9.33 的推导：
$$
\begin{aligned}
\frac{\partial L L\left(D_{u}\right)}{\partial \boldsymbol{\mu}_{i}} &=\sum_{\boldsymbol{x}_{j} \in D_{u}} \frac{\alpha_{i}}{\sum_{s=1}^{N} \alpha_{s} \cdot p\left(\boldsymbol{x}_{j} | \boldsymbol{\mu}_{s}, \boldsymbol{\Sigma}_{s}\right)} \cdot p\left(\boldsymbol{x}_{j} | \boldsymbol{\mu}_{i}, \boldsymbol{\Sigma}_{i}\right) \cdot \boldsymbol{\Sigma}_{i}^{-1}\left(\boldsymbol{x}_{j}-\boldsymbol{\mu}_{i}\right) \\
&=\sum_{\boldsymbol{x}_{j} \in D_{u}} \gamma_{j i} \cdot \boldsymbol{\Sigma}_{i}^{-1}\left(\boldsymbol{x}_{j}-\boldsymbol{\mu}_{i}\right)
\end{aligned}
$$


综上，
$$
\begin{aligned}\frac{\partial L L\left(D_{l} \cup D_{u}\right)}{\partial \boldsymbol{\mu}_{i}} &=\sum_{\left(\boldsymbol{x}_{j}, y_{j}\right) \in D_{l} \wedge y_{j}=i} \boldsymbol{\Sigma}_{i}^{-1}\left(\boldsymbol{x}_{j}-\boldsymbol{\mu}_{i}\right)+\sum_{\boldsymbol{x}_{j} \in D_{u}} \gamma_{j i} \cdot \boldsymbol{\Sigma}_{i}^{-1}\left(\boldsymbol{x}_{j}-\boldsymbol{\mu}_{i}\right) \\&=\boldsymbol{\Sigma}_{i}^{-1}\left(\sum_{\left(\boldsymbol{x}_{j}, y_{j}\right) \in D_{l} \wedge y_{j}=i}\left(\boldsymbol{x}_{j}-\boldsymbol{\mu}_{i}\right)+\sum_{\boldsymbol{x}_{j} \in D_{u}} \gamma_{j i} \cdot\left(\boldsymbol{x}_{j}-\boldsymbol{\mu}_{i}\right)\right) \\&=\boldsymbol{\Sigma}_{i}^{-1}\left(\sum_{\left(\boldsymbol{x}_{j}, y_{j}\right) \in D_{l} \wedge y_{j}=i} \boldsymbol{x}_{j}+\sum_{\boldsymbol{x}_{j} \in D_{u}} \gamma_{j i} \cdot \boldsymbol{x}_{j}-\sum_{\left(\boldsymbol{x}_{j}, y_{j}\right) \in D_{l} \wedge y_{j}=i} \boldsymbol{\mu}_{i}-\sum_{\boldsymbol{x}_{j} \in D_{u}} \gamma_{j i} \cdot \boldsymbol{\mu}_{i}\right)\end{aligned}
$$
令$\frac{\partial L L\left(D_{l} \cup D_{u}\right)}{\partial \boldsymbol{\mu}_{i}}=0$，两边同时左乘$\Sigma_i$并移项：
$$
\sum_{\boldsymbol{x}_{j} \in D_{u}} \gamma_{j i} \cdot \boldsymbol{\mu}_{i}+\sum_{\left(\boldsymbol{x}_{j}, y_{j}\right) \in D_{l} \wedge y_{j}=i} \boldsymbol{\mu}_{i}=\sum_{\boldsymbol{x}_{j} \in D_{u}} \gamma_{j i} \cdot \boldsymbol{x}_{j}+\sum_{\left(\boldsymbol{x}_{j}, y_{j}\right) \in D_{l} \wedge y_{j}=i} \boldsymbol{x}_{j}
$$
上式中，$\boldsymbol{\mu_i}$ 可以作为常量提到求和号外面，而$\sum_{\left(x_{j}, y_{j}\right) \in D_{l} \wedge y_{j}=i} 1=l_{i}$，即第$i$类样本的有标记 样本数目，因此

$$
\left(\sum_{x_{j} \in D_{u}} \gamma_{j i}+\sum_{\left(x_{j}, y_{j}\right) \in D_{l} \wedge y_{j}=i} 1\right) \boldsymbol{\mu}_{i}=\sum_{x_{j} \in D_{u}} \gamma_{j i} \cdot \boldsymbol{x}_{j}+\sum_{\left(x_{j}, y_{j}\right) \in D_{l} \wedge y_{j}=i} \boldsymbol{x}_{j}
$$


即得式 13.6

## 13.7

$$
\begin{aligned}\boldsymbol{\Sigma}_{i}=& \frac{1}{\sum_{\boldsymbol{x}_{j} \in D_{u}} \gamma_{j i}+l_{i}}\left(\sum_{\boldsymbol{x}_{j} \in D_{u}} \gamma_{j i} \cdot\left(\boldsymbol{x}_{j}-\boldsymbol{\mu}_{i}\right)\left(\boldsymbol{x}_{j}-\boldsymbol{\mu}_{i}\right)^{\top}\right.\\&\left.+\sum_{\left(\boldsymbol{x}_{j}, y_{j}\right) \in D_{l} \wedge y_{j}=i}\left(\boldsymbol{x}_{j}-\boldsymbol{\mu}_{i}\right)\left(\boldsymbol{x}_{j}-\boldsymbol{\mu}_{i}\right)^{\top}\right)\end{aligned}
$$

[推导]：类似于13.6 由$\cfrac{\partial LL(D_l \cup D_u) }{\partial \Sigma_i}=0$得，化简过程同13.6过程类似
首先$LL(D_l)$对$\boldsymbol{\Sigma_i}$求偏导 ，类似于 13.6 
$$
\begin{aligned} \frac{\partial L L\left(D_{l}\right)}{\partial \boldsymbol{\Sigma}_{i}} &=\sum_{\left(\boldsymbol{x}_{j}, y_{j}\right) \in D_{l} \wedge y_{j}=i} \frac{\partial \ln \left(\alpha_{i} \cdot p\left(\boldsymbol{x}_{j} | \boldsymbol{\mu}_{i}, \boldsymbol{\Sigma}_{i}\right)\right)}{\partial \boldsymbol{\Sigma}_{i}} \\ &=\sum_{\left(\boldsymbol{x}_{j}, y_{j}\right) \in D_{l} \wedge y_{j}=i} \frac{1}{p\left(\boldsymbol{x}_{j} | \boldsymbol{\mu}_{i}, \mathbf{\Sigma}_{i}\right)} \cdot \frac{\partial p\left(\boldsymbol{x}_{j} | \boldsymbol{\mu}_{i}, \boldsymbol{\Sigma}_{i}\right)}{\partial \boldsymbol{\Sigma}_{i}} \\
&=\sum_{\left(\boldsymbol{x}_{j}, y_{j}\right) \in D_{l} \wedge y_{j}=i} \frac{1}{p\left(\boldsymbol{x}_{j} | \boldsymbol{\mu}_{i}, \mathbf{\Sigma}_{i}\right)} \cdot p\left(\boldsymbol{x}_{j} | \boldsymbol{\mu}_{i}, \boldsymbol{\Sigma}_{i}\right) \cdot\left(\boldsymbol{\Sigma}_{i}^{-1}\left(\boldsymbol{x}_{j}-\boldsymbol{\mu}_{i}\right)\left(\boldsymbol{x}_{j}-\boldsymbol{\mu}_{i}\right)^{\top}-\boldsymbol{I}\right) \cdot \frac{1}{2} \boldsymbol{\Sigma}_{i}^{-1}\\
&=\sum_{\left(\boldsymbol{x}_{j}, y_{j}\right) \in D_{l} \wedge y_{j}=i}\left(\mathbf{\Sigma}_{i}^{-1}\left(\boldsymbol{x}_{j}-\boldsymbol{\mu}_{i}\right)\left(\boldsymbol{x}_{j}-\boldsymbol{\mu}_{i}\right)^{\top}-\boldsymbol{I}\right) \cdot \frac{1}{2} \boldsymbol{\Sigma}_{i}^{-1}
\end{aligned}
$$
然后$LL(D_u)$ 对$\boldsymbol{\Sigma_i}$求偏导，类似于 9.35
$$
\frac{\partial L L\left(D_{u}\right)}{\partial \boldsymbol{\Sigma}_{i}}=\sum_{\boldsymbol{x}_{j} \in D_{u}} \gamma_{j i} \cdot\left(\boldsymbol{\Sigma}_{i}^{-1}\left(\boldsymbol{x}_{j}-\boldsymbol{\mu}_{i}\right)\left(\boldsymbol{x}_{j}-\boldsymbol{\mu}_{i}\right)^{\top}-\boldsymbol{I}\right) \cdot \frac{1}{2} \boldsymbol{\Sigma}_{i}^{-1}
$$
综合可得：
$$
\begin{aligned} \frac{\partial L L\left(D_{l} \cup D_{u}\right)}{\partial \boldsymbol{\Sigma}_{i}}=& \sum_{\boldsymbol{x}_{j} \in D_{u}} \gamma_{j i} \cdot\left(\boldsymbol{\Sigma}_{i}^{-1}\left(\boldsymbol{x}_{j}-\boldsymbol{\mu}_{i}\right)\left(\boldsymbol{x}_{j}-\boldsymbol{\mu}_{i}\right)^{\top}-\boldsymbol{I}\right) \cdot \frac{1}{2} \boldsymbol{\Sigma}_{i}^{-1} \\ &+\sum_{\left(\boldsymbol{x}_{j}, y_{j}\right) \in D_{l} \wedge y_{j}=i}\left(\boldsymbol{\Sigma}_{i}^{-1}\left(\boldsymbol{x}_{j}-\boldsymbol{\mu}_{i}\right)\left(\boldsymbol{x}_{j}-\boldsymbol{\mu}_{i}\right)^{\top}-\boldsymbol{I}\right) \cdot \frac{1}{2} \boldsymbol{\Sigma}_{i}^{-1} \\=&\left(\sum_{\boldsymbol{x}_{j} \in D_{u}} \gamma_{j i} \cdot\left(\boldsymbol{\Sigma}_{i}^{-1}\left(\boldsymbol{x}_{j}-\boldsymbol{\mu}_{i}\right)\left(\boldsymbol{x}_{j}-\boldsymbol{\mu}_{i}\right)^{\top}-\boldsymbol{I}\right)\right.\\ &\left.+\sum_{\left(\boldsymbol{x}_{j}, y_{j}\right) \in D_{l} \wedge y_{j}=i}\left(\boldsymbol{\Sigma}_{i}^{-1}\left(\boldsymbol{x}_{j}-\boldsymbol{\mu}_{i}\right)\left(\boldsymbol{x}_{j}-\boldsymbol{\mu}_{i}\right)^{\top}-\boldsymbol{I}\right)\right) \cdot \frac{1}{2} \boldsymbol{\Sigma}_{i}^{-1} \end{aligned}
$$
令$\frac{\partial L L\left(D_{l} \cup D_{u}\right)}{\partial \boldsymbol{\Sigma}_{i}}=0$，两边同时右乘$2\Sigma_i$并移项：
$$
\begin{aligned} \sum_{\boldsymbol{x}_{j} \in D_{u}} \gamma_{j i} \cdot \boldsymbol{\Sigma}_{i}^{-1}\left(\boldsymbol{x}_{j}-\boldsymbol{\mu}_{i}\right)\left(\boldsymbol{x}_{j}-\boldsymbol{\mu}_{i}\right)^{\top}+& \sum_{\left(\boldsymbol{x}_{j}, y_{j} \in D_{l} \wedge y_{j}=i\right.} \boldsymbol{\Sigma}_{i}^{-1}\left(\boldsymbol{x}_{j}-\boldsymbol{\mu}_{i}\right)\left(\boldsymbol{x}_{j}-\boldsymbol{\mu}_{i}\right)^{\top} \\=& \sum_{\boldsymbol{x}_{j} \in D_{u}} \gamma_{j i} \cdot \boldsymbol{I}+\sum_{\left(\boldsymbol{x}_{j}, y_{j}\right) \in D_{l} \wedge y_{j}=i} \boldsymbol{I} \\ &=\left(\sum_{\boldsymbol{x}_{j} \in D_{u}} \gamma_{j i}+l_{i}\right) \boldsymbol{I} \end{aligned}
$$
两边同时左乘以$\Sigma_i$：
$$
\sum_{\boldsymbol{x}_{j} \in D_{u}} \gamma_{j i} \cdot\left(\boldsymbol{x}_{j}-\boldsymbol{\mu}_{i}\right)\left(\boldsymbol{x}_{j}-\boldsymbol{\mu}_{i}\right)^{\top}+\sum_{\left(\boldsymbol{x}_{j}, y_{j}\right) \in D_{l} \wedge y_{j}=i}\left(\boldsymbol{x}_{j}-\boldsymbol{\mu}_{i}\right)\left(\boldsymbol{x}_{j}-\boldsymbol{\mu}_{i}\right)^{\top}=\left(\sum_{\boldsymbol{x}_{j} \in D_{u}} \gamma_{j i}+l_{i}\right) \boldsymbol{\Sigma}_{i}
$$
即得式 13.7

## 13.8

$$
\alpha_{i}=\frac{1}{m}\left(\sum_{\boldsymbol{x}_{j} \in D_{u}} \gamma_{j i}+l_{i}\right)
$$

[推导]：类似于式 9.36，写出$LL(D_l \cup D_u)$的拉格朗日形式
$$
\begin{aligned}\mathcal{L}\left(D_{l} \cup D_{u}, \lambda\right) &=L L\left(D_{l} \cup D_{u}\right)+\lambda\left(\sum_{s=1}^{N} \alpha_{s}-1\right) \\&=L L\left(D_{l}\right)+L L\left(D_{u}\right)+\lambda\left(\sum_{s=1}^{N} \alpha_{s}-1\right)\end{aligned}
$$


类似于式 9.37，对$\alpha_i$求偏导。对于$$LL(D_u)$$，求导结果与式 9.37 的推导过程一样
$$
\frac{\partial L L\left(D_{u}\right)}{\partial \alpha_{i}}=\sum_{\boldsymbol{x}_{j} \in D_{u}} \frac{1}{\sum_{s=1}^{N} \alpha_{s} \cdot p\left(\boldsymbol{x}_{j} | \boldsymbol{\mu}_{s}, \boldsymbol{\Sigma}_{s}\right)} \cdot p\left(\boldsymbol{x}_{j} | \boldsymbol{\mu}_{i}, \boldsymbol{\Sigma}_{i}\right)
$$


对于$LL(D_l)$，类似于 13.6 和 13.7 的推导过程
$$
\begin{aligned}\frac{\partial L L\left(D_{l}\right)}{\partial \alpha_{i}} &=\sum_{\left(x_{j}, y_{j}\right) \in D_{l} \wedge y_{j}=i} \frac{\partial \ln \left(\alpha_{i} \cdot p\left(\boldsymbol{x}_{j} | \boldsymbol{\mu}_{i}, \boldsymbol{\Sigma}_{i}\right)\right)}{\partial \alpha_{i}} \\&=\sum_{\left(\boldsymbol{x}_{j}, y_{j}\right) \in D_{l} \wedge y_{j}=i} \frac{1}{\alpha_{i} \cdot p\left(\boldsymbol{x}_{j} | \boldsymbol{\mu}_{i}, \boldsymbol{\Sigma}_{i}\right)} \cdot \frac{\partial\left(\alpha_{i} \cdot p\left(\boldsymbol{x}_{j} | \boldsymbol{\mu}_{i}, \boldsymbol{\Sigma}_{i}\right)\right)}{\partial \alpha_{i}} \\&=\sum_{\left(\boldsymbol{x}_{j}, y_{j}\right) \in D_{l} \wedge y_{j}=i} \frac{1}{\alpha_{i} \cdot p\left(\boldsymbol{x}_{j} | \boldsymbol{\mu}_{i}, \boldsymbol{\Sigma}_{i}\right)} \cdot p\left(\boldsymbol{x}_{j} | \boldsymbol{\mu}_{i}, \boldsymbol{\Sigma}_{i}\right) \\&=\sum_{\left(\boldsymbol{x}_{j}, y_{j}\right) \in D_{l} \wedge y_{j}=i} \frac{1}{\alpha_{i}}=\frac{1}{\alpha_{i}} \cdot \sum_{\left(\boldsymbol{x}_{j}, y_{j}\right) \in D_{l} \wedge y_{j}=i} 1=\frac{l_{i}}{\alpha_{i}}\end{aligned}
$$


上式推导过程中，重点注意变量是$\alpha_i$ ，$p(x_j|\mu_i,\Sigma_i)$是常量；最后一行$\alpha_i$相对于求和变量为常量，因此作为公因子提到求和号外面； $l_i$ 为第$i$类样本的有标记样本数目。

综合两项结果：
$$
\frac{\partial \mathcal{L}\left(D_{l} \cup D_{u}, \lambda\right)}{\partial \alpha_{i}}=\frac{l_{i}}{\alpha_{i}}+\sum_{\boldsymbol{x}_{j} \in D_{u}} \frac{p\left(\boldsymbol{x}_{j} | \boldsymbol{\mu}_{i}, \boldsymbol{\Sigma}_{i}\right)}{\sum_{s=1}^{N} \alpha_{s} \cdot p\left(\boldsymbol{x}_{j} | \boldsymbol{\mu}_{s}, \mathbf{\Sigma}_{s}\right)}+\lambda
$$


令$\cfrac{\partial LL(D_l \cup D_u) }{\partial \alpha_i}=0$ 并且两边同乘以$\alpha_i$，得
$$
\alpha_{i} \cdot \frac{l_{i}}{\alpha_{i}}+\sum_{\boldsymbol{x}_{j} \in D_{u}} \frac{\alpha_{i} \cdot p\left(\boldsymbol{x}_{j} | \boldsymbol{\mu}_{i}, \boldsymbol{\Sigma}_{i}\right)}{\sum_{s=1}^{N} \alpha_{s} \cdot p\left(\boldsymbol{x}_{j} | \boldsymbol{\mu}_{s}, \boldsymbol{\Sigma}_{s}\right)}+\lambda \cdot \alpha_{i}=0
$$


结合式 9.30 发现，求和号内即为后验概率$\gamma_{ji}$,即
$$
l_i+\sum_{x_i \in D_u} \gamma_{ji}+\lambda \alpha_i = 0
$$
对所有混合成分求和，得
$$
\sum_{i=1}^N l_i+\sum_{i=1}^N  \sum_{x_i \in D_u} \gamma_{ji}+\sum_{i=1}^N \lambda \alpha_i = 0
$$


这里$\sum_{i=1}^N \alpha_i =1$ ，因此$\sum_{i=1}^N \lambda \alpha_i=\lambda\sum_{i=1}^N \alpha_i=\lambda$，根据 9.30 中$\gamma_{ji}$表达式可知
$$
\sum_{i=1}^N \gamma_{ji} =  \sum_{i =1}^{N} \cfrac{\alpha_i \cdot  p(x_j|\mu_i,\Sigma_i)}{\Sigma_{s=1}^N \alpha_s \cdot p(x_j| \mu_s, \Sigma_s)}=  \cfrac{\sum_{i =1}^{N}\alpha_i \cdot  p(x_j|\mu_i,\Sigma_i)}{\sum_{s=1}^N \alpha_s \cdot p(x_j| \mu_s, \Sigma_s)}=1
$$


再结合加法满足交换律，所以
$$
\sum_{i=1}^N  \sum_{x_i \in D_u} \gamma_{ji}=\sum_{x_i \in D_u} \sum_{i=1}^N  \gamma_{ji} =\sum_{x_i \in D_u} 1=u
$$


以上分析过程中，$\sum_{x_j\in D_u}$ 形式与$\sum_{j=1}^u$等价，其中u为未标记样本集的样本个数； $\sum_{i=1}^Nl_i=l$其中$l$为有标记样本集的样本个数；将这些结果代入
$$
\sum_{i=1}^N l_i+\sum_{i=1}^N  \sum_{x_i \in D_u} \gamma_{ji}+\sum_{i=1}^N \lambda \alpha_i = 0
$$


解出$l+u+\lambda = 0$  且$l+u =m$ 其中$m$为样本总个数，移项即得$\lambda = -m$
最后带入整理解得
$$
l_i + \sum_{x_j \in{D_u}} \gamma_{ji}-\lambda \alpha_i = 0
$$
整理即得式 13.8