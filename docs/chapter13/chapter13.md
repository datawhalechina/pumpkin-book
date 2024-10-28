# 第13章 半监督学习

## 13.1 未标记样本

"西瓜书"两张插图可谓本节亮点: 图 13.1 直观地说
明了使用末标记样本后带来的好处; 图 13.2
对比了主动学习、(纯)半监督学习和直推学习,
尤其是巧妙地将主动学习的概念融入进来。

直推学习是综合运用手头上已有的少量有标记样本和大量末标记样本,
对这些大量末标 记样本预测其标记;
而(纯)半监督学习是综合运用手头上已有的少量有标记样本和大量末标 记样本,
对新的末标记样本预测其标记。

对于直推学习, 当然可以仅利用有标记样本训练一个学习器,
再对末标记样本进行预测, 此即传统的监督学习; 对于(纯)半监督学习,
当然也可以舍弃大量末标记样本, 仅利用有标 记样本训练一个学习器,
再对新的末标记样本进行预测。但图 13.1 直观地说明了使用末标
记样本后带来的好处, 然而利用了末标记样本后是否真的会如图 13.1
所示带来预期的好处 呢? 此即 13.7 节阅读材料中提到的安全半监督学习。

接下来在 13.2 节、13.3 节、13.4 节、13.5 节介绍的四种半监督学习方法,
都可以应用 于直推学习, 但若要应用于(纯)半监督学习, 则要有额外的考虑,
尤其是 13.4 节介绍的图半 监督学习, 因为该节最后一段也明确提到
"构图过程仅能考虑训练样本集, 难以判知新样本 在图中的位置, 因此,
在接收到新样本时, 或是将其加入原数据集对图进行重构并重新进行 标记传播,
或是需引入额外的预测机制"。

## 13.2 生成式方法

本节与 9.4.3 节的高斯混合聚类密切相关, 有关 9.4.3 节的公式推导参见附录,
建议将高 斯混合聚类的内容理解之后再学习本节算法。

### 13.2.1 式(13.1)的解释

高斯混合分布的定义式。该式即为9.4.3节的式(9.29)，式(9.29)中的$k$个混合成分对应于此处的$N$个可能的类别。

### 13.2.2 式(13.2)的推导

首先, 该式的变量 $\Theta \in\{1,2, \ldots, N\}$ 即为式(9.30)中的
$z_j \in\{1,2, \ldots, k\}$ 。

从公式第 1 行到第 2
行是对概率进行边缘化(marginalization)；通过引入$\Theta$并对其求和
$\sum_{i=1}^N$以抵消引入的影响。从公式第 2 行到第 3 行推导如下


$$
\begin{aligned}p(y=j, \Theta=i | \boldsymbol{x}) &=\frac{p(y=j, \Theta=i, \boldsymbol{x})}{p(\boldsymbol{x})} \\&=\frac{p(y=j, \Theta=i, \boldsymbol{x})}{p(\Theta=i, \boldsymbol{x})} \cdot \frac{p(\Theta=i, \boldsymbol{x})}{p(\boldsymbol{x})} \\&=p(y=j | \Theta=i, \boldsymbol{x}) \cdot p(\Theta=i | \boldsymbol{x})\end{aligned}
$$



$p(y=j \mid \boldsymbol{x})$ 表示 $\boldsymbol{x}$ 的类别 $y$ 为第 $j$
个类别标记的后验概率（注意条件是已知 $\boldsymbol{x})$;

$p(y=j, \Theta=i \mid \boldsymbol{x})$ 表示 $\boldsymbol{x}$ 的类别 $y$
为第 $j$ 个类别标记且由第 $i$ 个高斯混合成分生成的后
验概率（注意条件是已知 $\boldsymbol{x}$ );

$p(y=j \mid \Theta=i, \boldsymbol{x})$ 表示第 $i$ 个高斯混合成分生成的
$\boldsymbol{x}$ 其类别 $y$ 为第 $j$ 个类别标记的概率 （注意条件是已知
$\Theta$ 和 $\boldsymbol{x}$, 这里修改了西瓜书式(13.3)下方对
$p(y=j \mid \Theta=i, \boldsymbol{x})$ 的表述);

$p(\Theta=i \mid \boldsymbol{x})$ 表示 $\boldsymbol{x}$ 由第 $i$
个高斯混合成分生成的后验概率（注意条件是已知 $\boldsymbol{x})$ 。

"西瓜书"第 296 页第 2 行提到 "假设样本由高斯混合模型生成,
且每个类别对应一个高斯 混合成分", 也就是说, 如果已知 $\boldsymbol{x}$
是由哪个高斯混合成分生成的, 也就知道了其类别。而
$p(y=j \mid \Theta=i, \boldsymbol{x})$ 表示已知 $\Theta$ 和
$\boldsymbol{x}$ 的条件概率（已知 $\Theta$ 就足够, 不需 $\boldsymbol{x}$
的信息), 因此


$$
p(y=j \mid \Theta=i, \boldsymbol{x})= \begin{cases}1, & i=j \\ 0, & i \neq j\end{cases}
$$



### 13.2.3 式(13.3)的推导

根据式(13.1)


$$
p(\boldsymbol{x})=\sum_{i=1}^{N} \alpha_{i} \cdot p\left(\boldsymbol{x} | \boldsymbol{\mu}_{i}, \boldsymbol{\Sigma}_{i}\right)
$$



因此


$$
\begin{aligned}p(\Theta=i | \boldsymbol{x})&=\frac{p(\Theta=i , \boldsymbol{x})}{P(x)}\\&=\frac{\alpha_{i} \cdot p\left(\boldsymbol{x} | \boldsymbol{\mu}_{i}, \boldsymbol{\Sigma}_{i}\right)}{\sum_{i=1}^{N} \alpha_{i} \cdot p\left(\boldsymbol{x} | \boldsymbol{\mu}_{i}, \boldsymbol{\Sigma}_{i}\right)}\end{aligned}
$$



### 13.2.4 式(13.4)的推导

第二项很好解释，当不知道类别信息的时候，样本$x_j$的概率可以用式 13.1
表示，所有无类别信息的样本$D_u$的似然是所有样本的乘积，因为$\ln$函数是单调的，所以也可以将$\ln$函数作用于这个乘积消除因为连乘产生的数值计算问题。第一项引入了样本的标签信息，由


$$
p(y=j | \Theta=i, \boldsymbol{x})=\left\{\begin{array}{ll}1, & i=j \\0, & i \neq j\end{array}\right.
$$



可知，这项限定了样本$x_j$只可能来自于$y_j$所对应的高斯分布。

### 13.2.5 式(13.5)的解释

参见式(13.3)，这项可以理解成样本$x_j$属于类别标签$i$(或者说由第$i$个高斯分布生成)的后验概率。其中$\alpha_i,\boldsymbol{\mu}_{i}\boldsymbol{\Sigma}_i$可以通过有标记样本预先计算出来。即：


$$
\begin{array}{l}\alpha_{i}=\frac{l_{i}}{\left|D_{l}\right|}, \text { where }\left|D_{l}\right|=\sum_{i=1}^{N} l_{i} \\\boldsymbol{\mu}_{i}=\frac{1}{l_{i}} \sum_{\left(\boldsymbol{x}_{j}, y_{j}\right) \in D_{l} \wedge y_{j}=i} \boldsymbol{x}_{j} \\\boldsymbol{\Sigma}_{i}=\frac{1}{l_{i}} \sum_{\left(\boldsymbol{x}_{j}, y_{j}\right) \in D_{l} \wedge y_{j}=i}\left(\boldsymbol{x}_{j}-\boldsymbol{\mu}_{i}\right)\left(\boldsymbol{x}_{j}-\boldsymbol{\mu}_{i}\right)^{\top}\end{array}
$$



其中 $l_i$ 表示第 $i$ 类样本的有标记样本数目, $\left|D_l\right|$
为有标记样本集样本总数, $\wedge$ 为 "逻辑与"。

### 13.2.6 式(13.6)的解释

这项可以由

$$
\cfrac{\partial LL(D_l \cup D_u) }{\partial \mu_i}=0
$$

而得，将式
13.4 的两项分别记为：


$$
\begin{aligned}LL(D_l)&=\sum_{(\boldsymbol{x_j},y_j \in D_l)}\ln\left(\sum_{s=1}^{N}\alpha_s \cdot p(\boldsymbol{x_j}\vert \boldsymbol{\mu}_s,\boldsymbol{\Sigma}_s) \cdot p(y_i|\Theta = s,\boldsymbol{x_j})\right)\\&=\sum_{(\boldsymbol{x_j},y_j \in D_l)}\ln\left(\alpha_{y_j} \cdot p(\boldsymbol{x_j} \vert \boldsymbol{\mu}_{y_j},\boldsymbol{\Sigma}_{y_j})\right)\\LL(D_u)&=\sum_{\boldsymbol{x_j} \in D_u} \ln\left(\sum_{s=1}^N \alpha_s \cdot p(\boldsymbol{x_j} | \boldsymbol{\mu}_s,\boldsymbol{\Sigma}_s)\right)\end{aligned}
$$



首先，$LL(D_l)$对$\boldsymbol{\mu_i}$求偏导，$LL(D_l)$求和号中只有$y_j=i$
的项能留下来，即


$$
\begin{aligned}\frac{\partial L L\left(D_{l}\right)}{\partial \boldsymbol{\mu}_{i}} &=\sum_{\left(\boldsymbol{x}_{j}, y_{j}\right) \in D_{l} \wedge y_{j}=i} \frac{\partial \ln \left(\alpha_{i} \cdot p\left(\boldsymbol{x}_{j} | \boldsymbol{\mu}_{i}, \boldsymbol{\Sigma}_{i}\right)\right)}{\partial \boldsymbol{\mu}_{i}} \\&=\sum_{\left(\boldsymbol{x}_{j}, y_{j}\right) \in D_{l} \wedge y_{j}=i} \frac{1}{p\left(\boldsymbol{x}_{j} | \boldsymbol{\mu}_{i}, \boldsymbol{\Sigma}_{i}\right)} \cdot \frac{\partial p\left(\boldsymbol{x}_{j} | \boldsymbol{\mu}_{i}, \boldsymbol{\Sigma}_{i}\right)}{\partial \boldsymbol{\mu}_{i}} \\&=\sum_{\left(\boldsymbol{x}_{j}, y_{j}\right) \in D_{l} \wedge y_{j}=i} \frac{1}{p\left(\boldsymbol{x}_{j} | \boldsymbol{\mu}_{i}, \boldsymbol{\Sigma}_{i}\right)} \cdot p\left(\boldsymbol{x}_{j} | \boldsymbol{\mu}_{i}, \boldsymbol{\Sigma}_{i}\right) \cdot \boldsymbol{\Sigma}_{i}^{-1}\left(\boldsymbol{x}_{j}-\boldsymbol{\mu}_{i}\right) \\&=\sum_{\left(\boldsymbol{x}_{j}, y_{j}\right) \in D_{l} \wedge y_{j}=i} \boldsymbol{\Sigma}_{i}^{-1}\left(\boldsymbol{x}_{j}-\boldsymbol{\mu}_{i}\right)\end{aligned}
$$



$LL(D_u)$对$\boldsymbol{\mu_i}$求导，参考 9.33 的推导：


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



上式中，$\boldsymbol{\mu_i}$
可以作为常量提到求和号外面，而$\sum_{\left(x_{j}, y_{j}\right) \in D_{l} \wedge y_{j}=i} 1=l_{i}$，即第$i$类样本的有标记
样本数目，因此


$$
\left(\sum_{x_{j} \in D_{u}} \gamma_{j i}+\sum_{\left(x_{j}, y_{j}\right) \in D_{l} \wedge y_{j}=i} 1\right) \boldsymbol{\mu}_{i}=\sum_{x_{j} \in D_{u}} \gamma_{j i} \cdot \boldsymbol{x}_{j}+\sum_{\left(x_{j}, y_{j}\right) \in D_{l} \wedge y_{j}=i} \boldsymbol{x}_{j}
$$



即得式(13.6)。

### 13.2.7 式(13.7)的解释

首先$LL(D_l)$对$\boldsymbol{\Sigma_i}$求偏导 ，类似于式(13.6)


$$
\begin{aligned} \frac{\partial L L\left(D_{l}\right)}{\partial \boldsymbol{\Sigma}_{i}} &=\sum_{\left(\boldsymbol{x}_{j}, y_{j}\right) \in D_{l} \wedge y_{j}=i} \frac{\partial \ln \left(\alpha_{i} \cdot p\left(\boldsymbol{x}_{j} | \boldsymbol{\mu}_{i}, \boldsymbol{\Sigma}_{i}\right)\right)}{\partial \boldsymbol{\Sigma}_{i}} \\ &=\sum_{\left(\boldsymbol{x}_{j}, y_{j}\right) \in D_{l} \wedge y_{j}=i} \frac{1}{p\left(\boldsymbol{x}_{j} | \boldsymbol{\mu}_{i}, \boldsymbol{\Sigma}_{i}\right)} \cdot \frac{\partial p\left(\boldsymbol{x}_{j} | \boldsymbol{\mu}_{i}, \boldsymbol{\Sigma}_{i}\right)}{\partial \boldsymbol{\Sigma}_{i}} \\
&=\sum_{\left(\boldsymbol{x}_{j}, y_{j}\right) \in D_{l} \wedge y_{j}=i} \frac{1}{p\left(\boldsymbol{x}_{j} | \boldsymbol{\mu}_{i}, \boldsymbol{\Sigma}_{i}\right)} \cdot p\left(\boldsymbol{x}_{j} | \boldsymbol{\mu}_{i}, \boldsymbol{\Sigma}_{i}\right) \cdot\left(\boldsymbol{\Sigma}_{i}^{-1}\left(\boldsymbol{x}_{j}-\boldsymbol{\mu}_{i}\right)\left(\boldsymbol{x}_{j}-\boldsymbol{\mu}_{i}\right)^{\top}-\boldsymbol{I}\right) \cdot \frac{1}{2} \boldsymbol{\Sigma}_{i}^{-1}\\
&=\sum_{\left(\boldsymbol{x}_{j}, y_{j}\right) \in D_{l} \wedge y_{j}=i}\left(\boldsymbol{\Sigma}_{i}^{-1}\left(\boldsymbol{x}_{j}-\boldsymbol{\mu}_{i}\right)\left(\boldsymbol{x}_{j}-\boldsymbol{\mu}_{i}\right)^{\top}-\boldsymbol{I}\right) \cdot \frac{1}{2} \boldsymbol{\Sigma}_{i}^{-1}
\end{aligned}
$$



然后$LL(D_u)$ 对$\boldsymbol{\Sigma_i}$求偏导，类似于式(9.35)


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



即得式(13.7)。

### 13.2.8 式(13.8)的解释

类似于式(9.36)，写出$LL(D_l \cup D_u)$的拉格朗日形式


$$
\begin{aligned}\mathcal{L}\left(D_{l} \cup D_{u}, \lambda\right) &=L L\left(D_{l} \cup D_{u}\right)+\lambda\left(\sum_{s=1}^{N} \alpha_{s}-1\right) \\&=L L\left(D_{l}\right)+L L\left(D_{u}\right)+\lambda\left(\sum_{s=1}^{N} \alpha_{s}-1\right)\end{aligned}
$$



类似于式(9.37)，对$\alpha_i$求偏导。对于$LL(D_u)$，求导结果与式(9.37)的推导过程一样


$$
\frac{\partial L L\left(D_{u}\right)}{\partial \alpha_{i}}=\sum_{\boldsymbol{x}_{j} \in D_{u}} \frac{1}{\sum_{s=1}^{N} \alpha_{s} \cdot p\left(\boldsymbol{x}_{j} | \boldsymbol{\mu}_{s}, \boldsymbol{\Sigma}_{s}\right)} \cdot p\left(\boldsymbol{x}_{j} | \boldsymbol{\mu}_{i}, \boldsymbol{\Sigma}_{i}\right)
$$



对于$LL(D_l)$，类似于式(13.6)和式(13.7)的推导过程


$$
\begin{aligned}\frac{\partial L L\left(D_{l}\right)}{\partial \alpha_{i}} &=\sum_{\left(x_{j}, y_{j}\right) \in D_{l} \wedge y_{j}=i} \frac{\partial \ln \left(\alpha_{i} \cdot p\left(\boldsymbol{x}_{j} | \boldsymbol{\mu}_{i}, \boldsymbol{\Sigma}_{i}\right)\right)}{\partial \alpha_{i}} \\&=\sum_{\left(\boldsymbol{x}_{j}, y_{j}\right) \in D_{l} \wedge y_{j}=i} \frac{1}{\alpha_{i} \cdot p\left(\boldsymbol{x}_{j} | \boldsymbol{\mu}_{i}, \boldsymbol{\Sigma}_{i}\right)} \cdot \frac{\partial\left(\alpha_{i} \cdot p\left(\boldsymbol{x}_{j} | \boldsymbol{\mu}_{i}, \boldsymbol{\Sigma}_{i}\right)\right)}{\partial \alpha_{i}} \\&=\sum_{\left(\boldsymbol{x}_{j}, y_{j}\right) \in D_{l} \wedge y_{j}=i} \frac{1}{\alpha_{i} \cdot p\left(\boldsymbol{x}_{j} | \boldsymbol{\mu}_{i}, \boldsymbol{\Sigma}_{i}\right)} \cdot p\left(\boldsymbol{x}_{j} | \boldsymbol{\mu}_{i}, \boldsymbol{\Sigma}_{i}\right) \\&=\sum_{\left(\boldsymbol{x}_{j}, y_{j}\right) \in D_{l} \wedge y_{j}=i} \frac{1}{\alpha_{i}}=\frac{1}{\alpha_{i}} \cdot \sum_{\left(\boldsymbol{x}_{j}, y_{j}\right) \in D_{l} \wedge y_{j}=i} 1=\frac{l_{i}}{\alpha_{i}}\end{aligned}
$$



上式推导过程中，重点注意变量是$\alpha_i$
，$p(x_j|\mu_i,\Sigma_i)$是常量；最后一行$\alpha_i$相对于求和变量为常量，因此作为公因子提到求和号外面；
$l_i$ 为第$i$类样本的有标记样本数目。

综合两项结果：


$$
\frac{\partial \mathcal{L}\left(D_{l} \cup D_{u}, \lambda\right)}{\partial \alpha_{i}}=\frac{l_{i}}{\alpha_{i}}+\sum_{\boldsymbol{x}_{j} \in D_{u}} \frac{p\left(\boldsymbol{x}_{j} | \boldsymbol{\mu}_{i}, \boldsymbol{\Sigma}_{i}\right)}{\sum_{s=1}^{N} \alpha_{s} \cdot p\left(\boldsymbol{x}_{j} | \boldsymbol{\mu}_{s}, \boldsymbol{\Sigma}_{s}\right)}+\lambda
$$



令$\cfrac{\partial LL(D_l \cup D_u) }{\partial \alpha_i}=0$
并且两边同乘以$\alpha_i$，得


$$
\alpha_{i} \cdot \frac{l_{i}}{\alpha_{i}}+\sum_{\boldsymbol{x}_{j} \in D_{u}} \frac{\alpha_{i} \cdot p\left(\boldsymbol{x}_{j} | \boldsymbol{\mu}_{i}, \boldsymbol{\Sigma}_{i}\right)}{\sum_{s=1}^{N} \alpha_{s} \cdot p\left(\boldsymbol{x}_{j} | \boldsymbol{\mu}_{s}, \boldsymbol{\Sigma}_{s}\right)}+\lambda \cdot \alpha_{i}=0
$$



结合式(9.30)发现，求和号内即为后验概率$\gamma_{ji}$,即


$$
l_i+\sum_{x_i \in D_u} \gamma_{ji}+\lambda \alpha_i = 0
$$



对所有混合成分求和，得


$$
\sum_{i=1}^N l_i+\sum_{i=1}^N  \sum_{x_i \in D_u} \gamma_{ji}+\sum_{i=1}^N \lambda \alpha_i = 0
$$



这里$\sum_{i=1}^N \alpha_i =1$
，因此$\sum_{i=1}^N \lambda \alpha_i=\lambda\sum_{i=1}^N \alpha_i=\lambda$，根据
9.30 中$\gamma_{ji}$表达式可知


$$
\sum_{i=1}^N \gamma_{ji} =  \sum_{i =1}^{N} \cfrac{\alpha_i \cdot  p(x_j|\mu_i,\Sigma_i)}{\Sigma_{s=1}^N \alpha_s \cdot p(x_j| \mu_s, \Sigma_s)}=  \cfrac{\sum_{i =1}^{N}\alpha_i \cdot  p(x_j|\mu_i,\Sigma_i)}{\sum_{s=1}^N \alpha_s \cdot p(x_j| \mu_s, \Sigma_s)}=1
$$



再结合加法满足交换律，所以


$$
\sum_{i=1}^N  \sum_{x_i \in D_u} \gamma_{ji}=\sum_{x_i \in D_u} \sum_{i=1}^N  \gamma_{ji} =\sum_{x_i \in D_u} 1=u
$$



以上分析过程中，$\sum_{x_j\in D_u}$
形式与$\sum_{j=1}^u$等价，其中u为未标记样本集的样本个数；
$\sum_{i=1}^Nl_i=l$其中$l$为有标记样本集的样本个数；将这些结果代入


$$
\sum_{i=1}^N l_i+\sum_{i=1}^N  \sum_{x_i \in D_u} \gamma_{ji}+\sum_{i=1}^N \lambda \alpha_i = 0
$$



解出$l+u+\lambda = 0$ 且$l+u =m$
其中$m$为样本总个数，移项即得$\lambda = -m$，最后带入整理解得


$$
l_i + \sum_{x_j \in{D_u}} \gamma_{ji}-\lambda \alpha_i = 0
$$



即 $l_i+\sum_{\boldsymbol{x}_j \in D_u} \gamma_{j i}-m \alpha_i=0$,
整理即得式 13.8。

## 13.3 半监督SVM

从本节名称"半监督SVM"即可知道与第6章的SVM内容联系紧密。建议理解了SVM之后再学习本节算法，会发现实际很简单；否则会感觉无从下手，难以理解。

由本节开篇的两段介绍可知，S3VM是SVM在半监督学习上的推广，是此类算法的总称而非某个具体的算法，其最著名的代表是TSVM。

### 13.3.1 图13.3的解释

注意对比S3VM划分超平面穿过的区域与SVM划分超平面穿过的区域的差别，明显S3VM划分超平面周围样本较少，也就是"数据低密度区域"，即"低密度分隔"。

### 13.3.2 式(13.9)的解释

这个公式和式(6.35)基本一致，除了引入了无标记样本的松弛变量$\xi_i, i=l+1,\cdots m$和对应的权重系数$C_u$和无标记样本的标记指派$\hat{y}_i$。因此，欲理解本节内容应该先理解SVM，否则会感觉无从下手，难以理解。

### 13.3.3 图13.4的解释

解释一下第 6 行:

（1）$\hat{y}_i \hat{y}_j<0$ 意味着末标记样本
$\boldsymbol{x}_i, \boldsymbol{x}_j$ 在此次迭代中被指派的标记
$\hat{y}_i, \hat{y}_j$ 相反(正例 $+1$ 和 反例 $-1$ 各 1 个);

（2）$\xi_i>0$ 意味着末标记样本 $\boldsymbol{x}_i$
在此次迭代中为支持向量: (a)在间隔带内但仍与自己标 记同侧
$\left(0<\xi_i<1\right)$, (b) 在间隔带内但与自己标记异侧
$\left(1<\xi_i<2\right)$, (c) 不在间隔带且与自 己标记异侧
$\left(\xi_i>2\right)$，三种情况分别如图13-1所示。

![图13-1 $\xi_i$的三种情况](https://datawhale-business.oss-cn-hangzhou.aliyuncs.com/3659/dashboard/1729938230376/ch13p4-summary.svg)

（3）$\xi_i+\xi_j>2$ 分两种情况。(I)
$\left(\xi_i>1\right) \wedge\left(\xi_j>1\right)$,
表示都位于自己指派标记异侧, 交换它们的标记后,
二者就都位于自己新指派标记同侧了,
如图13-2所示。

![图13-2 $\left(1<\xi_i, \xi_j<2\right)$](https://datawhale-business.oss-cn-hangzhou.aliyuncs.com/3659/dashboard/1729938944211/ch13p4-I1.svg)

可以发现, 当 $1<\xi_i, \xi_j<2$ 时, 交换之后虽然松弛变量仍然大于 0 ,
但至少 $\xi_i+\xi_j$ 比交换之前变小了; 若进一步的, 当 $\xi_i, \xi_j>2$
时, 则交换之后 $\xi_i+\xi_j$ 将变为 0 ,
如图13-3所示。

![图13-3 $\left(\xi_i>2\right)\wedge\left(\xi_j>2\right)$](https://datawhale-business.oss-cn-hangzhou.aliyuncs.com/3659/dashboard/1729938954756/ch13p4-I2.svg)

可以发现, 交换之后两个样本均被分类正确, 因此松弛变量均等于 0。至于
$\xi_i, \xi_j$ 其中之一位 于 $1 \sim 2$ 之间, 另一个大于 2 , 情况类似,
不单列出分析。

\(II\) $\left(0<\xi_i<1\right) \wedge\left(\xi_j>2-\xi_i\right)$,
表示有一个与自己标记同侧, 有一个与自己标记异侧, 此时可分两种情况。

(II.1) $1<\xi_j<2$, 表示样本与自己标记异侧,
但仍在间隔带内，如图13-4所示。

![图13-4 $\left(\xi_i+\xi_j>2\right)\wedge\left(0<\xi_i<1\right)\wedge\left(1<\xi_j<2\right)$](https://datawhale-business.oss-cn-hangzhou.aliyuncs.com/3659/dashboard/1729938966310/ch13p4-II1.svg)

可以发现, 此时两个样本位置超平面同一侧, 交换标记之后似乎没发生什么变化,
但是仔细观察会发现交换之后 $\xi_i+\xi_j$ 比交换之前变小了。

(II.2) $\xi_j>2$,
表示样本在间隔带外，如图13-5所示。

![图13-5 $\left(\xi_i+\xi_j>2\right)\wedge\left(0<\xi_i<1\right)\wedge\left(\xi_j>2\right)$](https://datawhale-business.oss-cn-hangzhou.aliyuncs.com/3659/dashboard/1729938971180/ch13p4-II2.svg)

可以发现, 交换之后其中之一被正确分类, $\xi_i+\xi_j$ 比交换之前也变小了。
综上所述, 当 $\xi_i+\xi_j>2$ 时, 交换指派标记 $\hat{y}_i, \hat{y}_j$
可以使 $\xi_i+\xi_j$ 下降, 也就是说分类结 果会得到改善。 再解释一下第 11
行: 逐步增长 $C_u$, 但不超过 $C_l$, 末标记样本的权重小于有标记样本。

### 13.3.4 式(13.10)的解释

将该式变形为 $\frac{C_u^{+}}{C_u^{-}}=\frac{u_{-}}{u_{+}}$,
即样本个数多的权重小, 样本个数少的权重大, 总体上保持 二者的作用相同。

## 13.4 图半监督学习

本节共讲了两种方法，其中式(13.11)
\~式(13.17)讲述了一个针对二分类问题的标记传播方法，式(13.18)
\~式(13.21)讲述了一个针对多分类问题的标记传播方法，两种方法的原理均为两种方法的原理均为"相似的样本应具有相似的标记"，只是面向的问题不同，而且具体实现的方法也不同。

### 13.4.1 式(13.12)的推导

注意, 该方法针对二分类问题的标记传播方法。我们希望能量函数 $E(f)$
越小越好, 注 意到式(13.11)的 $0<(\mathbf{W})_{i j} \leqslant 1$, 且样本
$\boldsymbol{x}_i$ 和样本 $\boldsymbol{x}_j$ 越相似(即
$\left\|\boldsymbol{x}_i-\boldsymbol{x}_j\right\|^2$ 越小)则
$(\mathbf{W})_{i j}$ 越 大, 因此要求式(13.12)中的
$\left(f\left(\boldsymbol{x}_i\right)-f\left(\boldsymbol{x}_j\right)\right)^2$
相应地越小越好 (即 "相似的样本应具有相似 的标记"), 如此才能达到能量函数
$E(f)$ 越小的目的。 首先对式(13.12)的第 1 行式子进行展开整理:


$$
\begin{aligned}
E(f) & =\frac{1}{2} \sum_{i=1}^m \sum_{j=1}^m(\mathbf{W})_{i j}\left(f\left(\boldsymbol{x}_i\right)-f\left(\boldsymbol{x}_j\right)\right)^2 \\
& =\frac{1}{2} \sum_{i=1}^m \sum_{j=1}^m(\mathbf{W})_{i j}\left(f^2\left(\boldsymbol{x}_i\right)-2 f\left(\boldsymbol{x}_i\right) f\left(\boldsymbol{x}_j\right)+f^2\left(\boldsymbol{x}_j\right)\right) \\
& =\frac{1}{2} \sum_{i=1}^m \sum_{j=1}^m(\mathbf{W})_{i j} f^2\left(\boldsymbol{x}_i\right)+\frac{1}{2} \sum_{i=1}^m \sum_{j=1}^m(\mathbf{W})_{i j} f^2\left(\boldsymbol{x}_j\right)-\sum_{i=1}^m \sum_{j=1}^m(\mathbf{W})_{i j} f\left(\boldsymbol{x}_i\right) f\left(\boldsymbol{x}_j\right)
\end{aligned}
$$



然后证明
$\sum_{i=1}^m \sum_{j=1}^m(\mathbf{W})_{i j} f^2\left(\boldsymbol{x}_i\right)=\sum_{i=1}^m \sum_{j=1}^m(\mathbf{W})_{i j} f^2\left(\boldsymbol{x}_j\right)$,
并变形: 

$$
\begin{aligned}
\sum_{i=1}^m \sum_{j=1}^m(\mathbf{W})_{i j} f^2\left(\boldsymbol{x}_j\right) & =\sum_{j=1}^m \sum_{i=1}^m(\mathbf{W})_{j i} f^2\left(\boldsymbol{x}_i\right)=\sum_{i=1}^m \sum_{j=1}^m(\mathbf{W})_{i j} f^2\left(\boldsymbol{x}_i\right) \\
& =\sum_{i=1}^m f^2\left(\boldsymbol{x}_i\right) \sum_{j=1}^m(\mathbf{W})_{i j}
\end{aligned}
$$



其中, 第 1 个等号是把变量 $i, j$ 分别用 $j, i$ 替代
(统一替换公式中的符号并不影响公式本身); 第 2 个等号是由于 $\mathbf{W}$
是对称矩阵 (即 $\left.(\mathbf{W})_{i j}=\mathbf{W}\right)_{j i}$ ),
并交换了求和号次序 (类似于多重 积分中交换积分号次序),
到此完成了该步骤的证明; 第 3 个等号是由于
$f^2\left(\boldsymbol{x}_i\right)$ 与求和变量 $j$ 无关,
因此拿到了该求和号外面 (与求和变量无关的项相对于该求和变量相当于常数),
该 步骤的变形主要是为了得到 $d_i$ 。 令
$d_i=\sum_{j=1}^m(\mathbf{W})_{i j}$ ( 既是 $\mathbf{W}$ 第 $i$
行元素之和, 实际亦是第 $j$ 列元素之和, 因为由于 $\mathbf{W}$ 是对称矩阵,
即 $\left.(\mathbf{W})_{i j}=\mathbf{W}\right)_{j i}$, 因此
$d_i=\sum_{j=1}^m(\mathbf{W})_{j i}$, 即第$i$列元素之和), 则


$$
E(f)=\sum_{i=1}^m d_i f^2\left(\boldsymbol{x}_i\right)-\sum_{i=1}^m \sum_{j=1}^m(\mathbf{W})_{i j} f\left(\boldsymbol{x}_i\right) f\left(\boldsymbol{x}_j\right)
$$



即式(13.12)的第 3 行, 其中第一项
$\sum_{i=1}^m d_i f^2\left(\boldsymbol{x}_i\right)$
可以写为如下矩阵形式: 

$$
\begin{aligned}
& =\boldsymbol{f}^{\mathrm{T}} \boldsymbol{D} \boldsymbol{f} \\
&
\end{aligned}
$$



第二项
$\sum_{i=1}^m \sum_{j=1}^m(\mathbf{W})_{i j} f\left(\boldsymbol{x}_i\right) f\left(\boldsymbol{x}_j\right)$
也可以写为如下矩阵形式: 

$$
\begin{aligned}
& \sum_{i=1}^m \sum_{j=1}^m(\mathbf{W})_{i j} f\left(\boldsymbol{x}_i\right) f\left(\boldsymbol{x}_j\right) \\
& =\left[\begin{array}{llll}
f\left(\boldsymbol{x}_1\right) & f\left(\boldsymbol{x}_2\right) & \cdots & f\left(\boldsymbol{x}_m\right)
\end{array}\right]\left[\begin{array}{cccc}
(\mathbf{W})_{11} & (\mathbf{W})_{12} & \cdots & (\mathbf{W})_{1 m} \\
(\mathbf{W})_{21} & (\mathbf{W})_{22} & \cdots & (\mathbf{W})_{2 m} \\
\vdots & \vdots & \ddots & \vdots \\
(\mathbf{W})_{m 1} & (\mathbf{W})_{m 2} & \cdots & (\mathbf{W})_{m m}
\end{array}\right]\left[\begin{array}{c}
f\left(\boldsymbol{x}_1\right) \\
f\left(\boldsymbol{x}_2\right) \\
\vdots \\
f\left(\boldsymbol{x}_m\right)
\end{array}\right] \\
& =\boldsymbol{f}^{\mathrm{T}} \boldsymbol{W} \boldsymbol{f}
\end{aligned}
$$



所以
$E(f)=\boldsymbol{f}^{\mathrm{T}} \boldsymbol{D}-\boldsymbol{f}^{\mathrm{T}} \boldsymbol{W} \boldsymbol{f}=\boldsymbol{f}^{\mathrm{T}}(\boldsymbol{D}-\boldsymbol{W}) \boldsymbol{f}$,
即式(13.12)。

### 13.4.2 式(13.13)的推导

本式就是将式(13.12)用分块矩阵形式表达而已,
拆分为标记样本和末标记样本两部分。

另外解释一下该式之前一段话中第一句的含义: "具有最小能量的函数 $f$
在有标记样本 上满足
$f\left(\boldsymbol{x}_i\right)=y_i(i=1,2, \ldots, l)$,
在末标记样本上满足 $\boldsymbol{\Delta} \boldsymbol{f}=\mathbf{0}$ ",
前半句是很容易理 解的, 有标记样本上满足
$f\left(\boldsymbol{x}_i\right)=y_i(i=1,2, \ldots, l)$, 这时末标记样本的
$f\left(\boldsymbol{x}_i\right)$ 是待求变 量且应该使 $E(f)$ 最小,
因此应将式(13.12)对末标记样本的 $f\left(\boldsymbol{x}_i\right)$
求导并令导数等于 0 即可, 此即表达式 $\Delta f=0$,
此处可以查看该算法的原始文献。

### 13.4.3 式(13.14)的推导

将式(13.13)根据矩阵运算规则进行变形,
这里第一项西瓜书中的符号有歧义，应该表示成$\left[\begin{array}{ll}
\boldsymbol{f}_{l}^{\mathrm{T}} & \boldsymbol{f}_{u}^{\mathrm{T}}
\end{array}\right]$即一个$\mathbb{R}^{1\times(l+u)}$的行向量。根据矩阵乘法的定义，有：


$$
\begin{aligned}
E(f) &=\left[\begin{array}{ll}
\boldsymbol{f}_{l}^{\mathrm{T}} & \boldsymbol{f}_{u}^{\mathrm{T}}
\end{array}\right]\left[\begin{array}{cc}
\boldsymbol{D}_{l l}-\boldsymbol{W}_{l l} & -\boldsymbol{W}_{l u} \\
-\boldsymbol{W}_{u l} & \boldsymbol{D}_{u u}-\boldsymbol{W}_{u u}
\end{array}\right]\left[\begin{array}{l}
\boldsymbol{f}_{l} \\
\boldsymbol{f}_{u}
\end{array}\right] \\
&=\left[\begin{array}{ll}\boldsymbol{f}_{l}^{\mathrm{T}}\left(\boldsymbol{D}_{l l}-\boldsymbol{W}_{l l}\right)-\boldsymbol{f}_{u}^{\mathrm{T}} \boldsymbol{W}_{u l} & -\boldsymbol{f}_{l}^{\mathrm{T}} \boldsymbol{W}_{l u}+\boldsymbol{f}_{u}^{\mathrm{T}}\left(\boldsymbol{D}_{u u}-\boldsymbol{W}_{u u}\right)\end{array}\right]\left[\begin{array}{l}
\boldsymbol{f}_{l} \\
\boldsymbol{f}_{u}
\end{array}\right] \\
&=\left(\boldsymbol{f}_{l}^{\mathrm{T}}\left(\boldsymbol{D}_{l l}-\boldsymbol{W}_{l l}\right)-\boldsymbol{f}_{u}^{\mathrm{T}} \boldsymbol{W}_{u l}\right) \boldsymbol{f}_{l}+\left(-\boldsymbol{f}_{l}^{\mathrm{T}} \boldsymbol{W}_{l u}+\boldsymbol{f}_{u}^{\mathrm{T}}\left(\boldsymbol{D}_{u u}-\boldsymbol{W}_{u u}\right)\right) \boldsymbol{f}_{u} \\
&=\boldsymbol{f}_{l}^{\mathrm{T}}\left(\boldsymbol{D}_{l l}-\boldsymbol{W}_{l l}\right) \boldsymbol{f}_{l}-\boldsymbol{f}_{u}^{\mathrm{T}} \boldsymbol{W}_{u l} \boldsymbol{f}_{l}-\boldsymbol{f}_{l}^{\mathrm{T}} \boldsymbol{W}_{l u} \boldsymbol{f}_{u}+\boldsymbol{f}_{u}^{\mathrm{T}}\left(\boldsymbol{D}_{u u}-\boldsymbol{W}_{u u}\right) \boldsymbol{f}_{u} \\
&=\boldsymbol{f}_{l}^{\mathrm{T}}\left(\boldsymbol{D}_{l l}-\boldsymbol{W}_{l l}\right) \boldsymbol{f}_{l}-2 \boldsymbol{f}_{u}^{\mathrm{T}} \boldsymbol{W}_{u l} \boldsymbol{f}_{l}+\boldsymbol{f}_{u}^{\mathrm{T}}\left(\boldsymbol{D}_{u u}-\boldsymbol{W}_{u u}\right) \boldsymbol{f}_{u}
\end{aligned}
$$



其中最后一步，$\boldsymbol{f}_{l}^{\mathrm{T}} \boldsymbol{W}_{l u} \boldsymbol{f}_{u}=\left(\boldsymbol{f}_{l}^{\mathrm{T}} \boldsymbol{W}_{l u} \boldsymbol{f}_{u}\right)^{\mathrm{T}}=f_{u}^{\mathrm{T}} \boldsymbol{W}_{u l} \boldsymbol{f}_{l}$，因为这个式子的结果是一个标量。

### 13.4.4 式(13.15)的推导

首先，基于式(13.14)对$\boldsymbol{f}_u$求导： 

$$
\begin{aligned}
\frac{\partial E(f)}{\partial \boldsymbol{f}_{u}} &=\frac{\partial \boldsymbol{f}_{l}^{\mathrm{T}}\left(\boldsymbol{D}_{l l}-\boldsymbol{W}_{l l}\right) \boldsymbol{f}_{l}-2 \boldsymbol{f}_{u}^{\mathrm{T}} \boldsymbol{W}_{u l} \boldsymbol{f}_{l}+\boldsymbol{f}_{u}^{\mathrm{T}}\left(\boldsymbol{D}_{u u}-\boldsymbol{W}_{u u}\right) \boldsymbol{f}_{u}}{\partial \boldsymbol{f}_{u}} \\
&=-2 \boldsymbol{W}_{u l} \boldsymbol{f}_{l}+2\left(\boldsymbol{D}_{u u}-\boldsymbol{W}_{u u}\right) \boldsymbol{f}_{u}
\end{aligned}
$$

 令结果等于 0 即得 13.15。

注意式中各项的含义:

$\boldsymbol{f}_u$ 即函数 $f$ 在末标记样本上的预测结果;

$\boldsymbol{D}_{u u}, \boldsymbol{W}_{u u}, \boldsymbol{W}_{u l}$
均可以由式(13.11)得到;

$\boldsymbol{f}_l$ 即函数 $f$ 在有标记样本上的预测结果 (即已知标记,
详见"西瓜书" P301 倒数第 3 行);

也就是说可以根据式(13.15)根据 $D_l$ 上的标记信息 (即 $\boldsymbol{f}_l$
) 求得末标记样本的标记 (即 $\boldsymbol{f}_u$ ),
式(13.17)仅是式(13.15)的进一步变形化简, 不再细述。

仔细回顾该方法, 实际就是根据 "相似的样本应具有相似的标记" 的原则,
构建了目标 函数式(13.12),
求解式(13.12)得到了使用标记样本信息表示的末标记样本的预测标记。

### 13.4.5 式(13.16)的解释

根据矩阵乘法的定义计算可得该式，其中需要注意的是，对角矩阵$\mathbf{D}$的拟等于其各个对角元素的倒数。

### 13.4.6 式(13.17)的推导

第一项到第二项是根据矩阵乘法逆的定义：$(\mathbf{A}\mathbf{B})^{-1}=\mathbf{B}^{-1}\mathbf{A}^{-1}$，在这个式子中

$$
\begin{aligned}
\mathbf{P}_{u u}&=\mathbf{D}_{u u}^{-1} \mathbf{W}_{u u}\\
\mathbf{P}_{ul}&=\mathbf{D}_{u u}^{-1} \mathbf{W}_{u l}
\end{aligned}
$$

均可以根据$\mathbf{W}_{ij}$计算得到，因此可以通过标记$\mathbf{f}_l$计算未标记数据的标签$\mathbf{f}_u$。

### 13.4.7 式(13.18)的解释

其中Y的第 $i$ 行表示第 $i$ 个样本的类别; 具体来说, 对于前 $l$
个有标记样本来说, 若第 $i$ 个样本 的类别为
$j(1 \leq j \leq|\mathcal{Y}|)$, 则 $\mathbf{Y}$ 的第行第 $j$ 列即为 1 ,
第行其余元素为 0 ; 对于后 $u$ 个末标 记样本来说, $Y$ 统一为零。注意
$|\mathcal{Y}|$ 表示集合 $\mathcal{Y}$ 的势, 即包含元素 (类别) 的个数。

### 13.4.8 式(13.20)的解释



$$
\mathbf{F}^{*}=\lim _{t \rightarrow \infty} \mathbf{F}(t)=(1-\alpha)(\mathbf{I}-\alpha \mathbf{S})^{-1} \mathbf{Y}
$$


\[解析\]：由式(13.19)


$$
\mathbf{F}(t+1)=\alpha \mathbf{S} \mathbf{F}(t)+(1-\alpha) \mathbf{Y}
$$


当 t取不同的值时，有： 

$$
\begin{aligned}
t=0: \mathbf{F}(1) &=\alpha \mathbf{S F}(0)+(1-\alpha) \mathbf{Y}\\
&=\alpha \mathbf{S} \mathbf{Y}+(1-\alpha) \mathbf{Y} \\
t=1: \mathbf{F}(2) &=\alpha \mathbf{S F}(1)+(1-\alpha) \mathbf{Y}=\alpha \mathbf{S}(\alpha \mathbf{S} \mathbf{Y}+(1-\alpha) \mathbf{Y})+(1-\alpha) \mathbf{Y} \\
&=(\alpha \mathbf{S})^{2} \mathbf{Y}+(1-\alpha)\left(\sum_{i=0}^{1}(\alpha \mathbf{S})^{i}\right) \mathbf{Y} \\
t=2:\mathbf{F}(3)&=\alpha\mathbf{S}\mathbf{F}(2)+(1-\alpha)\mathbf{Y}\\&=\alpha \mathbf{S}\left((\alpha \mathbf{S})^{2} \mathbf{Y}+(1-\alpha)\left(\sum_{i=0}^{1}(\alpha \mathbf{S})^{i}\right) \mathbf{Y}\right)+(1-\alpha) \mathbf{Y} \\
&=(\alpha \mathbf{S})^{3} \mathbf{Y}+(1-\alpha)\left(\sum_{i=0}^{2}(\alpha \mathbf{S})^{i}\right) \mathbf{Y}\\
\end{aligned}
$$

 可以观察到规律


$$
\mathbf{F}(t)=(\alpha \mathbf{S})^{t} \mathbf{Y}+(1-\alpha)\left(\sum_{i=0}^{t-1}(\alpha \mathbf{S})^{i}\right) \mathbf{Y}
$$


则


$$
\mathbf{F}^{*}=\lim _{t \rightarrow \infty}\mathbf{F}(t)=\lim _{t \rightarrow \infty}(\alpha \mathbf{S})^{t} \mathbf{Y}+\lim _{t \rightarrow \infty}(1-\alpha)\left(\sum_{i=0}^{t-1}(\alpha \mathbf{S})^{i}\right) \mathbf{Y}
$$


其中第一项由于$\mathbf{S}=\mathbf{D}^{-\frac{1}{2}} \mathbf{W} \mathbf{D}^{-\frac{1}{2}}$的特征值介于\[-1,
1\]之间[@lap_mat]，而$\alpha\in(0,1)$，所以$\lim _{t \rightarrow \infty}(\alpha \mathbf{S})^{t}=0$，第二项由等比数列公式


$$
\lim _{t \rightarrow \infty} \sum_{i=0}^{t-1}(\alpha \mathbf{S})^{i}=\frac{\mathbf{I}-\lim _{t \rightarrow \infty}(\alpha \mathbf{S})^{t}}{\mathbf{I}-\alpha \mathbf{S}}=\frac{\mathbf{I}}{\mathbf{I}-\alpha \mathbf{S}}=(\mathbf{I}-\alpha \mathbf{S})^{-1}
$$


综合可得式(13.20)。

### 13.4.9 式(13.21)的推导

这里主要是推导式(13.21)的最优解即为式(13.20)。将式(13.21)的目标函数进行变形。

第 1 部分:

先将范数平方拆开为四项 

$$
\begin{aligned}
\left\|\frac{1}{\sqrt{d_i}} \mathbf{F}_i-\frac{1}{\sqrt{d_j}} \mathbf{F}_j\right\|^2 & =\left(\frac{1}{\sqrt{d_i}} \mathbf{F}_i-\frac{1}{\sqrt{d_j}} \mathbf{F}_j\right)\left(\frac{1}{\sqrt{d_i}} \mathbf{F}_i-\frac{1}{\sqrt{d_j}} \mathbf{F}_j\right)^{\top} \\
& =\frac{1}{d_i} \mathbf{F}_i \mathbf{F}_i^{\top}+\frac{1}{d_j} \mathbf{F}_j \mathbf{F}_j^{\top}-\frac{1}{\sqrt{d_i d_j}} \mathbf{F}_i \mathbf{F}_j^{\top}-\frac{1}{\sqrt{d_j d_i}} \mathbf{F}_j \mathbf{F}_i^{\top}
\end{aligned}
$$



其中 $\mathbf{F}_i \in \mathbb{R}^{1 \times|\mathcal{Y}|}$ 表示矩阵
$\mathbf{F}$ 的第 $i$ 行, 即第 $i$ 个示例 $\boldsymbol{x}_i$
的标记向量。将第 1 项中的 $\sum_{i, j=1}^m$ 写为 两个和求号
$\sum_{i=1}^m \sum_{i=1}^m$ 的形式, 并将上面拆分的四项中的前两项代入, 得


$$
\begin{aligned}
& \sum_{i, j=1}^m(\mathbf{W})_{i j} \frac{1}{d_i} \mathbf{F}_i \mathbf{F}_i^{\top}=\sum_{i=1}^m \frac{1}{d_i} \mathbf{F}_i \mathbf{F}_i^{\top} \sum_{j=1}^m(\mathbf{W})_{i j}=\sum_{i=1}^m \frac{1}{d_i} \mathbf{F}_i \mathbf{F}_i^{\top} \cdot d_i=\sum_{i=1}^m \mathbf{F}_i \mathbf{F}_i^{\top} \\
& \sum_{i, j=1}^m(\mathbf{W})_{i j} \frac{1}{d_j} \mathbf{F}_j \mathbf{F}_j^{\top}=\sum_{j=1}^m \frac{1}{d_j} \mathbf{F}_j \mathbf{F}_j^{\top} \sum_{i=1}^m(\mathbf{W})_{i j}=\sum_{j=1}^m \frac{1}{d_j} \mathbf{F}_j \mathbf{F}_j^{\top} \cdot d_j=\sum_{j=1}^m \mathbf{F}_j \mathbf{F}_j^{\top} \\
&
\end{aligned}
$$



以上化简过程中, 两个求和号可以交换求和次序; 又因为 $\mathrm{W}$
为对称阵, 因此对行求和与对 列求和效果一样, 即
$d_i=\sum_{j=1}^m(\mathbf{W})_{i j}=\sum_{j=1}^m(\mathbf{W})_{j i}$
（已在式(13.12)推导时说明）。显然,


$$
\sum_{i=1}^m \mathbf{F}_i \mathbf{F}_i^{\top}=\sum_{j=1}^m \mathbf{F}_j \mathbf{F}_j^{\top}=\sum_{i=1}^m\left\|\mathbf{F}_i\right\|^2=\|\mathbf{F}\|_{\mathrm{F}}^2=\operatorname{tr}\left(\mathbf{F} \mathbf{F}^{\top}\right)
$$



以上推导过程中, 第 1 个等号显然成立, 因为二者仅是求和变量名称不同; 第 2
个等号即将 $\mathbf{F}_i \mathbf{F}_i^{\top}$ 写为
$\left\|\mathbf{F}_i\right\|^2$ 形式; 从第 2
个等号的结果可以看出这明显是在求矩阵 $\mathbf{F}$ 各元素平方之和,
也就是矩阵 $F$ 的 Frobenius 范数（简称 $\mathrm{F}$ 范数）的平方, 即第 3
个等号; 根据矩阵 $\mathrm{F}$ 范数与 矩阵的迹的关系有第 4 个等号
(详见本章预备知识: 矩阵的 $F$ 范数与迹)。 接下来,
将上面拆分的四项中的第三项代入，得


$$
\sum_{i, j=1}^m(\mathbf{W})_{i j} \frac{1}{\sqrt{d_i d_j}} \mathbf{F}_i \mathbf{F}_j^{\top}=\sum_{i, j=1}^m(\mathbf{S})_{i j} \mathbf{F}_i \mathbf{F}_j^{\top}=\operatorname{tr}\left(\mathbf{S}^{\top} \mathbf{F} \mathbf{F}^{\top}\right)=\operatorname{tr}\left(\mathbf{S F} \mathbf{F}^{\top}\right)
$$



具体来说, 以上化简过程为: 

$$
\begin{aligned}
& \mathbf{S}=\left[\begin{array}{cccc}
(\mathbf{S})_{11} & (\mathbf{S})_{12} & \cdots & (\mathbf{S})_{1 m} \\
(\mathbf{S})_{21} & (\mathbf{S})_{22} & \cdots & (\mathbf{S})_{2 m} \\
\vdots & \vdots & \ddots & \vdots \\
(\mathbf{S})_{m 1} & (\mathbf{S})_{m 2} & \cdots & (\mathbf{S})_{m m}
\end{array}\right] \\
& =\mathrm{D}^{-\frac{1}{2}} \mathbf{W D}^{-\frac{1}{2}} \\
& =\left[\begin{array}{ccccc}
\frac{1}{\sqrt{d_1}} & & & \\
& \frac{1}{\sqrt{d_2}} & & \\
& & \ddots & \\
& & & \frac{1}{\sqrt{d_m}}
\end{array}\right]\left[\begin{array}{cccc}
(\mathbf{W})_{11} & (\mathbf{W})_{12} & \cdots & (\mathbf{W})_{1 m} \\
(\mathbf{W})_{21} & (\mathbf{W})_{22} & \cdots & (\mathbf{W})_{2 m} \\
\vdots & \vdots & \ddots & \vdots \\
(\mathbf{W})_{m 1} & (\mathbf{W})_{m 2} & \cdots & (\mathbf{W})_{m m}
\end{array}\right]\left[\begin{array}{cccc}
\frac{1}{\sqrt{d_1}} & & & \\
& \frac{1}{\sqrt{d_2}} & & \\
& & \ddots & \\
& & & \frac{1}{\sqrt{d_m}}
\end{array}\right] \\
&
\end{aligned}
$$



由以上推导可以看出
$(\mathbf{S})_{i j}=\frac{1}{\sqrt{d_i d_j}}(\mathbf{W})_{i j}$, 即第 1
个等号; 而 

$$
\mathbf{F F}^{\top}=\left[\begin{array}{c}
\mathbf{F}_1 \\
\mathbf{F}_2 \\
\vdots \\
\mathbf{F}_m
\end{array}\right]\left[\begin{array}{llll}
\mathbf{F}_1^{\top} & \mathbf{F}_2^{\top} & \cdots & \mathbf{F}_m^{\top}
\end{array}\right]=\left[\begin{array}{cccc}
\mathbf{F}_1 \mathbf{F}_1^{\top} & \mathbf{F}_1 \mathbf{F}_2^{\top} & \ldots & \mathbf{F}_1 \mathbf{F}_m^{\top} \\
\mathbf{F}_2 \mathbf{F}_1^{\top} & \mathbf{F}_2 \mathbf{F}_2^{\top} & \cdots & \mathbf{F}_2 \mathbf{F}_m^{\top} \\
\vdots & \vdots & \ddots & \vdots \\
\mathbf{F}_m \mathbf{F}_1^{\top} & \mathbf{F}_m \mathbf{F}_2^{\top} & \cdots & \mathbf{F}_m \mathbf{F}_m^{\top}
\end{array}\right]
$$



若令 $\mathbf{A}=\mathbf{S} \circ \mathbf{F F}^{\top}$, 其中○表示
Hadmard 积, 即矩阵 $\mathbf{S}$ 与矩阵 $\mathbf{F} \mathbf{F}^{\top}$
元素对应相乘（参见百 度百科哈达玛积), 因此


$$
\sum_{i, j=1}^m(\mathbf{S})_{i j} \mathbf{F}_i \mathbf{F}_j^{\top}=\sum_{i, j=1}^m(\mathbf{A})_{i j}
$$



可以验证, 上式的矩阵
$\mathbf{A}=\mathbf{S} \circ \mathbf{F} \mathbf{F}^{\top}$ 元素之和
$\sum_{i, j=1}^m(\mathbf{A})_{i j}$ 等于
$\operatorname{tr}\left(\mathbf{S}^{\top} \mathbf{F F}^{\top}\right)$,
这是因为 

$$
\begin{aligned}
& \operatorname{tr}\left(\left[\begin{array}{cccc}
(\mathbf{S})_{11} & (\mathbf{S})_{12} & \cdots & (\mathbf{S})_{1 m} \\
(\mathbf{S})_{21} & (\mathbf{S})_{22} & \cdots & (\mathbf{S})_{2 m} \\
\vdots & \vdots & \ddots & \vdots \\
(\mathbf{S})_{m 1} & (\mathbf{S})_{m 2} & \cdots & (\mathbf{S})_{m m}
\end{array}\right]^{\top} \cdot\left[\begin{array}{cccc}
\mathbf{F}_1 \mathbf{F}_1^{\top} & \mathbf{F}_1 \mathbf{F}_2^{\top} & \cdots & \mathbf{F}_1 \mathbf{F}_m^{\top} \\
\mathbf{F}_2 \mathbf{F}_1^{\top} & \mathbf{F}_2 \mathbf{F}_2^{\top} & \cdots & \mathbf{F}_2 \mathbf{F}_m^{\top} \\
\vdots & \vdots & \ddots & \vdots \\
\mathbf{F}_m \mathbf{F}_1^{\top} & \mathbf{F}_m \mathbf{F}_2^{\top} & \cdots & \mathbf{F}_m \mathbf{F}_m^{\top}
\end{array}\right]\right) \\
&=\left[\begin{array}{c}
(\mathbf{S})_{11} \\
(\mathbf{S})_{21} \\
\vdots \\
(\mathbf{S})_{m 1}
\end{array}\right]^{\top} \cdot\left[\begin{array}{c}
\mathbf{F}_1 \mathbf{F}_1^{\top} \\
\mathbf{F}_2 \mathbf{F}_1^{\top} \\
\vdots \\
\mathbf{F}_m \mathbf{F}_1^{\top}
\end{array}\right]+\left[\begin{array}{c}
(\mathbf{S})_{12} \\
(\mathbf{S})_{22} \\
\vdots \\
(\mathbf{S})_{m 2}
\end{array}\right]^{\top} \cdot\left[\begin{array}{c}
\mathbf{F}_1 \mathbf{F}_2^{\top} \\
\mathbf{F}_2 \mathbf{F}_2^{\top} \\
\vdots \\
\mathbf{F}_m \mathbf{F}_2^{\top}
\end{array}\right]+\ldots+\left[\begin{array}{c}
(\mathbf{S})_{1 m} \\
(\mathbf{S})_{2 m} \\
\vdots \\
(\mathbf{S})_{m m}
\end{array}\right]^{\top}\left[\begin{array}{c}
\mathbf{F}_1 \mathbf{F}_m^{\top} \\
\mathbf{F}_2 \mathbf{F}_m^{\top} \\
\vdots \\
\mathbf{F}_m \mathbf{F}_m^{\top}
\end{array}\right] \\
&=\sum_{i=1}^m(\mathbf{S})_{i 1} \mathbf{F}_i \mathbf{F}_1^{\top}+\sum_{i=1}^m(\mathbf{S})_{i 2} \mathbf{F}_i \mathbf{F}_2^{\top}+\ldots+\sum_{i=1}(\mathbf{S})_{i m} \mathbf{F}_i \mathbf{F}_m^{\top} \\
&= \sum_{i, j=1}^m(\mathbf{S})_{i j} \mathbf{F}_i \mathbf{F}_j^{\top}
\end{aligned}
$$



即第 2 个等号; 易知矩阵 $\mathbf{S}$ 是对称阵
$\left(\mathbf{S}^{\top}=\mathbf{S}\right)$, 即得第 3 个等号。又由于内积
$\mathbf{F}_i \mathbf{F}_j^{\top}$ 是一个 数 (即大小为 $1 \times 1$
的矩阵), 因此其转置等于本身,


$$
\mathbf{F}_i \mathbf{F}_j^{\top}=\left(\mathbf{F}_i \mathbf{F}_j^{\top}\right)^{\top}=\left(\mathbf{F}_j^{\top}\right)^{\top}\left(\mathbf{F}_i\right)^{\top}=\mathbf{F}_j \mathbf{F}_i^{\top}
$$



因此


$$
\frac{1}{\sqrt{d_i d_j}} \mathbf{F}_i \mathbf{F}_j^{\top}=\frac{1}{\sqrt{d_j d_i}} \mathbf{F}_j \mathbf{F}_i^{\top}
$$



进而上面拆分的四项中的第三项和第四项相等:


$$
\sum_{i, j=1}^m(\mathbf{W})_{i j} \frac{1}{\sqrt{d_i d_j}} \mathbf{F}_i \mathbf{F}_j^{\top}=\sum_{i, j=1}^m(\mathbf{W})_{i j} \frac{1}{\sqrt{d_j d_i}} \mathbf{F}_j \mathbf{F}_i^{\top}
$$



综上所述 (以上拆分的四项中前两项相等、后两项相等, 正好抵消系数
$\frac{1}{2}$ ):


$$
\frac{1}{2}\left(\sum_{i, j=1}^m(\mathbf{W})_{i j}\left\|\frac{1}{\sqrt{d_i}} \mathbf{F}_i-\frac{1}{\sqrt{d_j}} \mathbf{F}_j\right\|^2\right)=\operatorname{tr}\left(\mathbf{F F}^{\top}\right)-\operatorname{tr}\left(\mathbf{S F F}^{\top}\right)
$$



第 2 部分:

西瓜书中式(13.21)的第 2 部分与原文献[@zhou2003learning]中式(4)的第 2
部分不同:


$$
\mathcal{Q}(F)=\frac{1}{2} \sum_{i, j=1}^n W_{i j}\left\|\frac{F_i}{\sqrt{D_{i i}}}-\frac{F_j}{\sqrt{D_{j j}}}\right\|^2+\mu \sum_{i=1}^n\left\|F_i-Y_i\right\|^2,
$$



原文献中第 2 部分包含了所有样本 (求和变量上限为 $n$ ),
而西瓜书只包含有标记样本, 并 且第 304
页第二段提到"式(13.21)右边第二项是迫使学得结果在有标记样本上的预测与真实标记尽可能相同";
若按原文献式(4)在第二项中将末标记样本也包含进来, 由于对于末标记 样本
$\mathbf{Y}_i=\mathbf{0}$,
因此直观上理解是迫使末标记样本学习结果尽可能接近 0 , 这显然是不对的;
有关这一点作者在第 24 次印刷勘误中进行了补充:
"考虑到有标记样本通常很少而末标记样 本很多, 为缓解过拟合,
可在式(13.21)中引入针对末标记样本的 $\mathrm{L}_2$ 范数项
$\mu \sum_{i=l+1}^{l+u}\left\|\mathbf{F}_i\right\|^2$, 式(13.21)
加上此项之后就与原文献的式(4)完全相同了。将第二项写为 $\mathrm{F}$
范数形式:


$$
\sum_{i=1}^m\left\|\mathbf{F}_i-\mathbf{Y}_i\right\|^2=\|\mathbf{F}-\mathbf{Y}\|_{\mathrm{F}}^2
$$



综上, 式(13.21)目标函数
$\mathcal{Q}(\mathbf{F})=\operatorname{tr}\left(\mathbf{F} \mathbf{F}^{\top}\right)-\operatorname{tr}\left(\mathbf{S F F ^ { \top }}\right)+\mu\|\mathbf{F}-\mathbf{Y}\|_{\mathrm{F}}^2$,
求导: 

$$
\begin{aligned}
\frac{\partial \mathcal{Q}(\mathbf{F})}{\partial \mathbf{F}} & =\frac{\partial \operatorname{tr}\left(\mathbf{F} \mathbf{F}^{\top}\right)}{\partial \mathbf{F}}-\frac{\partial \operatorname{tr}\left(\mathbf{S} \mathbf{F} \mathbf{F}^{\top}\right)}{\partial \mathbf{F}}+\mu \frac{\partial\|\mathbf{F}-\mathbf{Y}\|_{\mathrm{F}}^2}{\partial \mathbf{F}} \\
& =2 \mathbf{F}-2 \mathbf{S} \mathbf{F}+2 \mu(\mathbf{F}-\mathbf{Y})
\end{aligned}
$$

 令 $\mu=\frac{1-\alpha}{\alpha}$, 并令
$\frac{\partial \mathcal{Q}(\mathbf{F})}{\partial \mathbf{F}}=2 \mathbf{F}-2 \mathbf{S F}+2 \frac{1-\alpha}{\alpha}(\mathbf{F}-\mathbf{Y})=0$,
移项化简即可得式(13.20), 即式(13.20)是正则化框架式(13.21)的解。

## 13.5 基于分歧的方法

"西瓜书"的伟大之处在于巧妙地融入了很多机器学习的研究分支,
而非仅简单介绍经典的 机器学习算法。比如本节处于半监督学习章节范围内,
巧妙地将机器学习的研究热点之一多视图学习[@xu2013survey](multi-view
learning) 融入进来, 类似地还有本章 第一节将主动学习融入进来, 在第 10
章第一节将 $k$ 近邻算法融入进来, 在最后一节巧妙地 将度量学习(metric
learning)融入进来等等。

协同训练是多视图学习代表性算法之一, 本章叙述简单易懂。

### 13.5.1 图13.6的解释

第 2 行表示从样本集 $D_u$ 中去除缓冲池样本 $D_s$;

第 4 行, 当 $j=1$ 时
$\left\langle\boldsymbol{x}_i^j, \boldsymbol{x}_i^{3-j}\right\rangle$
即为 $\left\langle\boldsymbol{x}_i^1, \boldsymbol{x}_i^2\right\rangle$,
当 $j=2$ 时
$\left\langle\boldsymbol{x}_i^j, \boldsymbol{x}_i^{3-j}\right\rangle$
即为 $\left\langle\boldsymbol{x}_i^2, \boldsymbol{x}_i^1\right\rangle$,
往后 的 $3-j$ 与此相同; 注意本页左上角的注释:
$\left\langle\boldsymbol{x}_i^1, \boldsymbol{x}_i^2\right\rangle$ 与
$\left\langle\boldsymbol{x}_i^2, \boldsymbol{x}_i^1\right\rangle$
表示的是同一个样本, 因此 第 1 个视图的有标记标训练集为
$D_l^1=\left\{\left(\boldsymbol{x}_1^1, y_1\right), \ldots,\left(\boldsymbol{x}_l^1, y_l\right)\right\}$,
第 2 个视图的有标记标训 练集为
$D_l^2=\left\{\left(\boldsymbol{x}_1^2, y_1\right), \ldots,\left(\boldsymbol{x}_l^2, y_l\right)\right\}$;

第 9 行 第 11 行是根据第 $j$
个视图对缓冲池中无标记样本的分类置信度赋予伪标记, 准备交给第 $3-j$
个视图使用。

## 13.6 半监督聚类

### 13.6.1 图13.7的解释

注意算法第 4 行到第 21 行是依次对每个样本进行处理, 其中第 8 行到第 21
行是尝试将 样本 $\boldsymbol{x}_i$ 到底应该划入哪个族, 具体来说是按样本
$\boldsymbol{x}_i$ 到各均值向量的距离从小到大依次尝试, 若最小的不违背
$\mathcal{M}$ 和 $\mathcal{C}$ 中的约束, 则将样本 $\boldsymbol{x}_i$
划入该簇并置 is_merge=true, 此时第 8 行 的while循环条件为假不再继续循环,
若从小到大依次尝试各簇后均违背 $\mathcal{M}$ 和 $\mathcal{C}$ 中的约束则
第 16 行的if条件为真, 算法报错结束; 依次对每个样本进行处理后第 22 行到第
24 行更新均值向量, 重新开始新一轮迭代, 直到均值向量均末更新。

### 13.6.2 图13.9的解释

算法第6行到第10行即在聚类簇迭代更新过程中不改变种子样本的簇隶属关系；第11行到第15行即对非种子样本进行普通的k-means聚类过程；第16行到第18行更新均值向量，反复迭代，直到均值向量均未更新。

## 参考文献

[1] Wikipedia contributors. Laplacian matrix, 2020.

[2] Dengyong Zhou, Olivier Bousquet, Thomas Lal, Jason Weston, and Bernhard Schölkopf. Learning
with local and global consistency. Advances in neural information processing systems, 16, 2003.

[3] Chang Xu, Dacheng Tao, and Chao Xu. A survey on multi-view learning. arXiv preprint
arXiv:1304.5634, 2013.
