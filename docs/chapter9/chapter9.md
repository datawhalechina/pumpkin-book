## 9.5 

$$
\mathrm{JC}=\frac{a}{a+b+c}
$$

[解析]：给定两个集合$A$和$B$，则Jaccard系数定义为如下公式

$$
\mathrm{JC}=\frac{|A\bigcap B|}{|A\bigcup B|}=\frac{|A\bigcap B|}{|A|+|B|-|A\bigcap B|}
$$
Jaccard系数可以用来描述两个集合的相似程度。

推论：假设全集$U$共有$n$个元素，且$A\subseteq U$，$B\subseteq U$，则每一个元素的位置共有四种情况：

1、元素同时在集合$A$和$B$中，这样的元素个数记为$M_{11}$；

2、元素出现在集合$A$中，但没有出现在集合$B$中，这样的元素个数记为$M_{10}$；

3、元素没有出现在集合$A$中，但出现在集合$B$中，这样的元素个数记为$M_{01}$；

4、元素既没有出现在集合$A$中，也没有出现在集合$B$中，这样的元素个数记为$M_{00}$。

根据Jaccard系数的定义，此时的Jaccard系数为如下公式
$$
\mathrm{JC}=\frac{M_{11}}{M_{11}+M_{10}+M_{01}}
$$
由于聚类属于无监督学习，事先并不知道聚类后样本所属类别的类别标记所代表的意义，即便参考模型的类别标记意义是已知的，我们也无法知道聚类后的类别标记与参考模型的类别标记是如何对应的，况且聚类后的类别总数与参考模型的类别总数还可能不一样，因此只用单个样本无法衡量聚类性能的好坏。

由于外部指标的基本思想就是以参考模型的类别划分为参照，因此如果某一个样本对中的两个样本在聚类结果中同属于一个类，在参考模型中也同属于一个类，或者这两个样本在聚类结果中不同属于一个类，在参考模型中也不同属于一个类，那么对于这两个样本来说这是一个好的聚类结果。

总的来说所有样本对中的两个样本共存在四种情况：	
1、样本对中的两个样本在聚类结果中属于同一个类，在参考模型中也属于同一个类；	
2、样本对中的两个样本在聚类结果中属于同一个类，在参考模型中不属于同一个类；	
3、样本对中的两个样本在聚类结果中不属于同一个类，在参考模型中属于同一个类；	
4、样本对中的两个样本在聚类结果中不属于同一个类，在参考模型中也不属于同一个类。

综上所述，即所有样本对存在着书中公式(9.1)-(9.4)的四种情况，现在假设集合$A$中存放着两个样本都同属于聚类结果的同一个类的样本对，即$A=SS\bigcup SD$，集合$B$中存放着两个样本都同属于参考模型的同一个类的样本对，即$B=SS\bigcup DS$，那么根据Jaccard系数的定义有：
$$
\mathrm{JC}=\frac{|A\bigcap B|}{|A\bigcup B|}=\frac{|SS|}{|SS\bigcup SD\bigcup DS|}=\frac{a}{a+b+c}
$$
也可直接将书中公式(9.1)-(9.4)的四种情况类比推论，即$M_{11}=a$，$M_{10}=b$，$M_{01}=c$，所以
$$
\mathrm{JC}=\frac{M_{11}}{M_{11}+M_{10}+M_{01}}=\frac{a}{a+b+c}
$$

## 9.6
$$
\mathrm{FMI}=\sqrt{\frac{a}{a+b}\cdot \frac{a}{a+c}}
$$

[解析]：其中$\frac{a}{a+b}$和$\frac{a}{a+c}$为Wallace提出的两个非对称指标，$a$代表两个样本在聚类结果和参考模型中均属于同一类的样本对的个数，$a+b$代表两个样本在聚类结果中属于同一类的样本对的个数，$a+c$代表两个样本在参考模型中属于同一类的样本对的个数，这两个非对称指标均可理解为样本对中的两个样本在聚类结果和参考模型中均属于同一类的概率。由于指标的非对称性，这两个概率值往往不一样，因此Fowlkes和Mallows提出利用几何平均数将这两个非对称指标转化为一个对称指标，即Fowlkes and Mallows Index, FMI。

## 9.7
$$
\mathrm{RI}=\frac{2(a+d)}{m(m-1)}
$$
[解析]：Rand Index定义如下：
$$
\mathrm{RI}=\frac{a+d}{a+b+c+d}=\frac{a+d}{m(m-1)/2}=\frac{2(a+d)}{m(m-1)}
$$
其可以理解为两个样本都属于聚类结果和参考模型中的同一类的样本对的个数与两个样本都分别不属于聚类结果和参考模型中的同一类的样本对的个数的总和在所有样本对中出现的频率，可以简单理解为聚类结果与参考模型的一致性。

## 9.8

$$
\operatorname{avg}(C)=\frac{2}{|C|(|C|-1)} \sum_{1 \leqslant i<j \leqslant|C|} \operatorname{dist}\left(\boldsymbol{x}_{i}, \boldsymbol{x}_{j}\right)
$$

[解析]：簇内距离的定义式：求和号左边是$(x_i, x_j)$组合个数的倒数，求和号右边是这些组合的距离和，所以两者相乘定义为平均距离。

## 9.33
$$
\sum_{j=1}^{m} \frac{\alpha_{i} \cdot p\left(\boldsymbol{x}_{j} | \boldsymbol{\mu}_{i}, \boldsymbol{\Sigma}_{i}\right)}{\sum_{l=1}^{k} \alpha_{l} \cdot p\left(\boldsymbol{x}_{j} | \boldsymbol{\mu}_{l}, \boldsymbol{\Sigma}_{l}\right)}\left(\boldsymbol{x}_{j}-\boldsymbol{\mu}_{i}\right)=0
$$
[推导]：根据公式(9.28)可知：
$$
p\left(\boldsymbol{x}_{j} | \boldsymbol{\mu}_{i}, \boldsymbol{\Sigma}_{i}\right)=\frac{1}{(2 \pi)^{\frac{n}{2}}\left|\boldsymbol{\Sigma}_{i}\right|^{\frac{1}{2}}} \exp \left(-\frac{1}{2}\left(\boldsymbol{x}_{j}-\boldsymbol{\mu}_{i}\right)^{T} \boldsymbol{\Sigma}_{i}^{-1}\left(\boldsymbol{x}_{j}-\boldsymbol{\mu}_{i}\right)\right)
$$
又根据公式(9.32)，由
$$
\cfrac{\partial LL(D)}{\partial \boldsymbol\mu_{i}}=0
$$
可得
$$\begin{aligned}
\frac{\partial L L(D)}{\partial \boldsymbol{\mu}_{i}} &=\frac{\partial}{\partial \boldsymbol{\mu}_{i}}\left[\sum_{j=1}^{m} \ln \left(\sum_{i=1}^{k} \alpha_{i} \cdot p\left(\boldsymbol{x}_{j} | \boldsymbol{\mu}_{i}, \boldsymbol{\Sigma}_{i}\right)\right)\right] \\
&=\sum_{j=1}^{m} \frac{\partial}{\partial \boldsymbol{\mu}_{i}}\left[\ln \left(\sum_{i=1}^{k} \alpha_{i} \cdot p\left(\boldsymbol{x}_{j} | \boldsymbol{\mu}_{i}, \boldsymbol{\Sigma}_{i}\right)\right)\right] \\
&=\sum_{j=1}^{m} \frac{\alpha_{i} \cdot \frac{\partial}{\partial \boldsymbol{\mu}_{i}}\left(p\left(\boldsymbol{x}_{j} | \boldsymbol{\mu}_{i}, \boldsymbol{\Sigma}_{i}\right)\right)}{\sum_{l=1}^{k} \alpha_{l} \cdot p\left(\boldsymbol{x}_{j} | \boldsymbol{\mu}_{l}, \boldsymbol{\Sigma}_{l}\right)} \\
&=\sum_{j=1}^{m} \frac{1}{(2 \pi)^{\frac{n}{2}}\left|\boldsymbol{\Sigma}_{i}\right|^{\frac{1}{2}} \exp \left(-\frac{1}{2}\left(\boldsymbol{x}_{j}-\boldsymbol{\mu}_{i}\right)^{T} \boldsymbol{\Sigma}_{i}^{-1}\left(\boldsymbol{x}_{j}-\boldsymbol{\mu}_{i}\right)\right)} \frac{\partial}{\partial \boldsymbol{\mu}_{l=1} \alpha_{l} \cdot p\left(\boldsymbol{x}_{j} | \boldsymbol{\mu}_{l}, \boldsymbol{\Sigma}_{l}\right)}\left(-\frac{1}{2}\left(\boldsymbol{x}_{i}\right)^{m}\right) \cdot \\ &\qquad\frac{\partial}{\partial \boldsymbol{\mu}_{i}}\left(\boldsymbol{x}_{j}^{T} \boldsymbol{\Sigma}_{i}^{-1} \boldsymbol{x}_{j}-\boldsymbol{x}_{j}^{T} \boldsymbol{\Sigma}_{i}^{-1} \boldsymbol{\mu}_{i}-\boldsymbol{\mu}_{i}^{T} \boldsymbol{\Sigma}_{i}^{-1} \boldsymbol{x}_{j}+\boldsymbol{\mu}_{i}^{T} \boldsymbol{\Sigma}_{i}^{-1} \boldsymbol{\mu}_{i}\right) \\
&=\sum_{j=1}^{m} \frac{\alpha_{i} \cdot p\left(\boldsymbol{x}_{j} | \boldsymbol{\mu}_{i}, \boldsymbol{\Sigma}_{i}\right)}{\sum_{l=1}^{k} \alpha_{l} \cdot p\left(\boldsymbol{x}_{j} | \boldsymbol{\mu}_{l}, \boldsymbol{\Sigma}_{l}\right)} \cdot\left(-\frac{1}{2}\right) \cdot \frac{\partial_{i}}{2}\left(\boldsymbol{x}_{i}, \boldsymbol{\Sigma}_{i}\right) \\
&=\sum_{j=1}^{m} \frac{\left.\alpha_{i} \cdot p\left(\boldsymbol{x}_{i}\right)^{T} \boldsymbol{\Sigma}_{l=1}^{-1}\left(\boldsymbol{x}_{j}-\boldsymbol{\mu}_{i}\right)\right)}{\left(x_{l} \cdot p\left(\boldsymbol{x}_{j} | \boldsymbol{\mu}_{l}, \boldsymbol{\Sigma}_{l}\right)\right.} \cdot\left(-\frac{1}{2}\right) \cdot \frac{\partial}{\partial \boldsymbol{\mu}_{i}}\left(-\boldsymbol{x}_{j}^{T} \boldsymbol{\Sigma}_{i}^{-1} \boldsymbol{\mu}_{i}-\boldsymbol{\mu}_{i}^{T} \boldsymbol{\Sigma}_{i}^{-1} \boldsymbol{x}_{j}+\boldsymbol{\mu}_{i}^{T} \boldsymbol{\Sigma}_{i}^{-1} \boldsymbol{\mu}_{i}\right)
\end{aligned}$$
由于$\boldsymbol x_j^T\mathbf{\Sigma}_i^{-1}\boldsymbol\mu_i$和$\boldsymbol\mu_i^T\mathbf{\Sigma}_i^{-1}\boldsymbol x_j$均为标量且$\mathbf{\Sigma}_i$为对称矩阵，所以
$$(\boldsymbol x_j^T\mathbf{\Sigma}_i^{-1}\boldsymbol\mu_i)^T=\boldsymbol\mu_i^T({\mathbf{\Sigma}_i^{-1}})^T\boldsymbol x_j=\boldsymbol\mu_i^T({\mathbf{\Sigma}_i^T})^{-1}\boldsymbol x_j=\boldsymbol\mu_i^T\mathbf{\Sigma}_i^{-1}\boldsymbol x_j=\boldsymbol x_j^T\mathbf{\Sigma}_i^{-1}\boldsymbol\mu_i$$
于是上式可进一步化简为
$$\cfrac {\partial LL(D)}{\partial\boldsymbol\mu_{i}}=\sum_{j=1}^m\cfrac{\alpha_{i}\cdot p(\boldsymbol x_{j}|\boldsymbol\mu_{i},\mathbf\Sigma_{i})}{\sum_{l=1}^k\alpha_{l}\cdot p(\boldsymbol x_{j}|\boldsymbol\mu_{l},\mathbf\Sigma_{l})}\cdot(-\cfrac{1}{2})\cdot\cfrac{\partial}{\partial\boldsymbol\mu_{i}}\left(-2\boldsymbol\mu_i^T\mathbf{\Sigma}_i^{-1}\boldsymbol x_j+\boldsymbol\mu_i^T\mathbf{\Sigma}_i^{-1}\boldsymbol\mu_i\right) $$
由矩阵微分公式$\cfrac{\partial \boldsymbol{x}^{T} \boldsymbol{a}}{\partial \boldsymbol{x}}=\boldsymbol{a},\cfrac{\partial \boldsymbol{x}^{T} \mathbf{B} \boldsymbol{x}}{\partial \boldsymbol{x}}=\left(\mathbf{B}+\mathbf{B}^{T}\right) \boldsymbol{x}$可得
$$\begin{aligned}
\cfrac {\partial LL(D)}{\partial\boldsymbol\mu_{i}}&=\sum_{j=1}^m\cfrac{\alpha_{i}\cdot p(\boldsymbol x_{j}|\boldsymbol\mu_{i},\mathbf\Sigma_{i})}{\sum_{l=1}^k\alpha_{l}\cdot p(\boldsymbol x_{j}|\boldsymbol\mu_{l},\mathbf\Sigma_{l})}\cdot(-\cfrac{1}{2})\cdot\left(-2\mathbf{\Sigma}_i^{-1}\boldsymbol x_j+2\mathbf{\Sigma}_i^{-1}\boldsymbol\mu_i\right) \\
&=\sum_{j=1}^m\cfrac{\alpha_{i}\cdot p(\boldsymbol x_{j}|\boldsymbol\mu_{i},\mathbf\Sigma_{i})}{\sum_{l=1}^k\alpha_{l}\cdot p(\boldsymbol x_{j}|\boldsymbol\mu_{l},\mathbf\Sigma_{l})}\mathbf{\Sigma}_i^{-1}\left(\boldsymbol x_j-\boldsymbol\mu_i\right) \\
\end{aligned}$$
令上式等于0可得：
$$\cfrac {\partial LL(D)}{\partial\boldsymbol\mu_{i}}=\sum_{j=1}^m\cfrac{\alpha_{i}\cdot p(\boldsymbol x_{j}|\boldsymbol\mu_{i},\mathbf\Sigma_{i})}{\sum_{l=1}^k\alpha_{l}\cdot p(\boldsymbol x_{j}|\boldsymbol\mu_{l},\mathbf\Sigma_{l})}\mathbf{\Sigma}_i^{-1}\left(\boldsymbol x_j-\boldsymbol\mu_i\right)=0 $$
左右两边同时乘上$\mathbf\Sigma_{i}$可得：
$$\sum_{j=1}^m\cfrac{\alpha_{i}\cdot p(\boldsymbol x_{j}|\boldsymbol\mu_{i},\mathbf\Sigma_{i})}{\sum_{l=1}^k\alpha_{l}\cdot p(\boldsymbol x_{j}|\boldsymbol\mu_{l},\mathbf\Sigma_{l})}\left(\boldsymbol x_j-\boldsymbol\mu_i\right)=0$$
此即为公式(9.33)

## 9.34

$$
\mu_{i}=\frac{\sum_{j=1}^{m} \gamma_{j i} \boldsymbol{x}_{j}}{\sum_{j=1}^{m} \gamma_{j i}}
$$



[推导]：由式9.30
$$
\gamma_{j i}=p_{\mathcal{M}}\left(z_{j}=i | \boldsymbol{x}_{j}\right)=\frac{\alpha_{i} \cdot p\left(\boldsymbol{x}_{j} | \boldsymbol{\mu}_{i}, \boldsymbol{\Sigma}_{i}\right)}{\sum_{l=1}^{k} \alpha_{l} \cdot p\left(\boldsymbol{x}_{j} | \boldsymbol{\mu}_{l}, \mathbf{\Sigma}_{l}\right)}
$$
带入9.33
$$
\sum_{j=1}^{m} \gamma_{j i}\left(\boldsymbol{x}_{j}-\boldsymbol{\mu}_{i}\right)=0
$$
因此有
$$
\mu_{i}=\frac{\sum_{j=1}^{m} \gamma_{j i} \boldsymbol{x}_{j}}{\sum_{j=1}^{m} \gamma_{j i}}
$$

## 9.35
$$
\mathbf\Sigma_{i}=\cfrac{\sum_{j=1}^m\gamma_{ji}(\boldsymbol x_{j}-\boldsymbol \mu_{i})(\boldsymbol x_{j}-\boldsymbol\mu_{i})^T}{\sum_{j=1}^m\gamma_{ji}}
$$
[推导]：根据公式(9.28)可知：
$$
p(\boldsymbol x_{j}|\boldsymbol\mu_{i},\mathbf\Sigma_{i})=\cfrac{1}{(2\pi)^\frac{n}{2}\left| \mathbf\Sigma_{i}\right |^\frac{1}{2}}\exp\left({-\frac{1}{2}(\boldsymbol x_{j}-\boldsymbol\mu_{i})^T\mathbf\Sigma_{i}^{-1}(\boldsymbol x_{j}-\boldsymbol\mu_{i})}\right)
$$
又根据公式(9.32)，由
$$
\cfrac{\partial LL(D)}{\partial \mathbf\Sigma_{i}}=0
$$
可得
$$\begin{aligned}
\cfrac {\partial LL(D)}{\partial\mathbf\Sigma_{i}}&=\cfrac {\partial}{\partial \mathbf\Sigma_{i}}\left[\sum_{j=1}^m\ln\Bigg(\sum_{i=1}^k \alpha_{i}\cdot p(\boldsymbol x_{j}|\boldsymbol\mu_{i},\mathbf\Sigma_{i})\Bigg)\right] \\
&=\sum_{j=1}^m\frac{\partial}{\partial\mathbf\Sigma_{i}}\left[\ln\Bigg(\sum_{i=1}^k \alpha_{i}\cdot p(\boldsymbol x_{j}|\boldsymbol\mu_{i},\mathbf\Sigma_{i})\Bigg)\right] \\
&=\sum_{j=1}^m\cfrac{\alpha_{i}\cdot \cfrac{\partial}{\partial\mathbf\Sigma_{i}}\left(p(\boldsymbol x_{j}|\boldsymbol\mu_{i},\mathbf\Sigma_{i})\right)}{\sum_{l=1}^k\alpha_{l}\cdot p(\boldsymbol x_{j}|\boldsymbol\mu_{l},\mathbf\Sigma_{l})} \\
\end{aligned}$$
其中
$$\begin{aligned}
\cfrac{\partial}{\partial\mathbf\Sigma_{i}}\left(p(\boldsymbol x_{j}|\boldsymbol\mu_{i},\mathbf\Sigma_{i})\right)&=\cfrac{\partial}{\partial\mathbf\Sigma_{i}}\left[\cfrac{1}{(2\pi)^\frac{n}{2}\left| \mathbf\Sigma_{i}\right |^\frac{1}{2}}\exp\left({-\frac{1}{2}(\boldsymbol x_{j}-\boldsymbol\mu_{i})^T\mathbf\Sigma_{i}^{-1}(\boldsymbol x_{j}-\boldsymbol\mu_{i})}\right)\right] \\
&=\cfrac{\partial}{\partial\mathbf\Sigma_{i}}\left\{\exp\left[\ln\left(\cfrac{1}{(2\pi)^\frac{n}{2}\left| \mathbf\Sigma_{i}\right |^\frac{1}{2}}\exp\left({-\frac{1}{2}(\boldsymbol x_{j}-\boldsymbol\mu_{i})^T\mathbf\Sigma_{i}^{-1}(\boldsymbol x_{j}-\boldsymbol\mu_{i})}\right)\right)\right]\right\} \\
&=p(\boldsymbol x_{j}|\boldsymbol\mu_{i},\mathbf\Sigma_{i})\cdot\cfrac{\partial}{\partial\mathbf\Sigma_{i}}\left[\ln\left(\cfrac{1}{(2\pi)^\frac{n}{2}\left| \mathbf\Sigma_{i}\right |^\frac{1}{2}}\exp\left({-\frac{1}{2}(\boldsymbol x_{j}-\boldsymbol\mu_{i})^T\mathbf\Sigma_{i}^{-1}(\boldsymbol x_{j}-\boldsymbol\mu_{i})}\right)\right)\right]\\
&=p(\boldsymbol x_{j}|\boldsymbol\mu_{i},\mathbf\Sigma_{i})\cdot\cfrac{\partial}{\partial\mathbf\Sigma_{i}}\left[\ln\cfrac{1}{(2\pi)^{\frac{n}{2}}}-\cfrac{1}{2}\ln{|\mathbf{\Sigma}_i|}-\frac{1}{2}(\boldsymbol x_j-\boldsymbol\mu_i)^T\mathbf{\Sigma}_i^{-1}(\boldsymbol x_j-\boldsymbol\mu_i)\right]\\
&=p(\boldsymbol x_{j}|\boldsymbol\mu_{i},\mathbf\Sigma_{i})\cdot\left[-\cfrac{1}{2}\cfrac{\partial\left(\ln{|\mathbf{\Sigma}_i|}\right) }{\partial \mathbf{\Sigma}_i}-\cfrac{1}{2}\cfrac{\partial \left[(\boldsymbol x_j-\boldsymbol\mu_i)^T\mathbf{\Sigma}_i^{-1}(\boldsymbol x_j-\boldsymbol\mu_i)\right]}{\partial \mathbf{\Sigma}_i}\right]\\
\end{aligned}$$
由矩阵微分公式$\cfrac{\partial |\mathbf{X}|}{\partial \mathbf{X}}=|\mathbf{X}|\cdot(\mathbf{X}^{-1})^{T},\cfrac{\partial \boldsymbol{a}^{T} \mathbf{X}^{-1} \boldsymbol{b}}{\partial \mathbf{X}}=-\mathbf{X}^{-T} \boldsymbol{a b}^{T} \mathbf{X}^{-T}$可得
$$
\begin{aligned}
\cfrac{\partial}{\partial\mathbf\Sigma_{i}}\left(p(\boldsymbol x_{j}|\boldsymbol\mu_{i},\mathbf\Sigma_{i})\right)&=p(\boldsymbol x_{j}|\boldsymbol\mu_{i},\mathbf\Sigma_{i})\cdot\left[-\cfrac{1}{2}\mathbf{\Sigma}_i^{-1}+\cfrac{1}{2}\mathbf{\Sigma}_i^{-1}(\boldsymbol x_j-\boldsymbol\mu_i)(\boldsymbol x_j-\boldsymbol\mu_i)^T\mathbf{\Sigma}_i^{-1}\right]\\
\end{aligned}
$$
将此式代回$\cfrac {\partial LL(D)}{\partial\mathbf\Sigma_{i}}$中可得
$$
\cfrac {\partial LL(D)}{\partial\mathbf\Sigma_{i}}=\sum_{j=1}^m\cfrac{\alpha_{i}\cdot p(\boldsymbol x_{j}|\boldsymbol\mu_{i},\mathbf\Sigma_{i})}{\sum_{l=1}^k\alpha_{l}\cdot p(\boldsymbol x_{j}|\boldsymbol\mu_{l},\mathbf\Sigma_{l})}\cdot\left[-\cfrac{1}{2}\mathbf{\Sigma}_i^{-1}+\cfrac{1}{2}\mathbf{\Sigma}_i^{-1}(\boldsymbol x_j-\boldsymbol\mu_i)(\boldsymbol x_j-\boldsymbol\mu_i)^T\mathbf{\Sigma}_i^{-1}\right]
$$


又由公式(9.30)可知$\cfrac{\alpha_{i}\cdot p(\boldsymbol x_{j}|\boldsymbol\mu_{i},\mathbf\Sigma_{i})}{\sum_{l=1}^k\alpha_{l}\cdot p(\boldsymbol x_{j}|\boldsymbol\mu_{l},\mathbf\Sigma_{l})}=\gamma_{ji}$，所以上式可进一步化简为
$$
\cfrac {\partial LL(D)}{\partial\mathbf\Sigma_{i}}=\sum_{j=1}^m\gamma_{ji}\cdot\left[-\cfrac{1}{2}\mathbf{\Sigma}_i^{-1}+\cfrac{1}{2}\mathbf{\Sigma}_i^{-1}(\boldsymbol x_j-\boldsymbol\mu_i)(\boldsymbol x_j-\boldsymbol\mu_i)^T\mathbf{\Sigma}_i^{-1}\right]
$$
令上式等于0可得
$$
\cfrac {\partial LL(D)}{\partial\mathbf\Sigma_{i}}=\sum_{j=1}^m\gamma_{ji}\cdot\left[-\cfrac{1}{2}\mathbf{\Sigma}_i^{-1}+\cfrac{1}{2}\mathbf{\Sigma}_i^{-1}(\boldsymbol x_j-\boldsymbol\mu_i)(\boldsymbol x_j-\boldsymbol\mu_i)^T\mathbf{\Sigma}_i^{-1}\right]=0
$$
移项推导有：
$$
\begin{aligned}
\sum_{j=1}^m\gamma_{ji}\cdot\left[-\boldsymbol{I}+(\boldsymbol x_j-\boldsymbol\mu_i)(\boldsymbol x_j-\boldsymbol\mu_i)^T\mathbf{\Sigma}_i^{-1}\right]&=0\\
\sum_{j=1}^m\gamma_{ji}(\boldsymbol x_j-\boldsymbol\mu_i)(\boldsymbol x_j-\boldsymbol\mu_i)^T\mathbf{\Sigma}_i^{-1}&=\sum_{j=1}^m\gamma_{ji}\boldsymbol{I}\\
\sum_{j=1}^m\gamma_{ji}(\boldsymbol x_j-\boldsymbol\mu_i)(\boldsymbol x_j-\boldsymbol\mu_i)^T&=\sum_{j=1}^m\gamma_{ji}\mathbf{\Sigma}_i\\
\mathbf{\Sigma}_i^{-1}\cdot\sum_{j=1}^m\gamma_{ji}(\boldsymbol x_j-\boldsymbol\mu_i)(\boldsymbol x_j-\boldsymbol\mu_i)^T&=\sum_{j=1}^m\gamma_{ji}\\
\mathbf{\Sigma}_i&=\cfrac{\sum_{j=1}^m\gamma_{ji}(\boldsymbol x_j-\boldsymbol\mu_i)(\boldsymbol x_j-\boldsymbol\mu_i)^T}{\sum_{j=1}^m\gamma_{ji}}
\end{aligned}
$$
此即为公式(9.35)

## 9.38
$$
\alpha_{i}=\frac{1}{m}\sum_{j=1}^m\gamma_{ji}
$$
[推导]：对公式(9.37)两边同时乘以$\alpha_{i}$可得
$$
\begin{aligned}
\sum_{j=1}^m\frac{\alpha_{i}\cdot p(\boldsymbol x_{j}|\boldsymbol\mu_{i},\mathbf\Sigma_{i})}{\sum_{l=1}^k\alpha_{l}\cdot p(\boldsymbol x_{j}|\boldsymbol\mu_{l},\mathbf\Sigma_{l})}+\lambda\alpha_{i}=0\\
\sum_{j=1}^m\frac{\alpha_{i}\cdot p(\boldsymbol x_{j}|\boldsymbol\mu_{i},\mathbf\Sigma_{i})}{\sum_{l=1}^k\alpha_{l}\cdot p(\boldsymbol x_{j}|\boldsymbol\mu_{l},\mathbf\Sigma_{l})}=-\lambda\alpha_{i}
\end{aligned}
$$
两边对所有混合成分求和可得
$$
\begin{aligned}\sum_{i=1}^k\sum_{j=1}^m\frac{\alpha_{i}\cdot p(\boldsymbol x_{j}|\boldsymbol\mu_{i},\mathbf\Sigma_{i})}{\sum_{l=1}^k\alpha_{l}\cdot p(\boldsymbol x_{j}|\boldsymbol\mu_{l},\mathbf\Sigma_{l})}&=-\lambda\sum_{i=1}^k\alpha_{i}\\
\sum_{j=1}^m\sum_{i=1}^k\frac{\alpha_{i}\cdot p(\boldsymbol x_{j}|\boldsymbol\mu_{i},\mathbf\Sigma_{i})}{\sum_{l=1}^k\alpha_{l}\cdot p(\boldsymbol x_{j}|\boldsymbol\mu_{l},\mathbf\Sigma_{l})}&=-\lambda\sum_{i=1}^k\alpha_{i}
\end{aligned}
$$
由$$m=-\lambda$$有
$$
\sum_{j=1}^m\frac{\alpha_{i}\cdot p(\boldsymbol x_{j}|\boldsymbol\mu_{i},\mathbf\Sigma_{i})}{\sum_{l=1}^k\alpha_{l}\cdot p(\boldsymbol x_{j}|\boldsymbol\mu_{l},\mathbf\Sigma_{l})}=-\lambda\alpha_{i}=m\alpha_{i}
$$
因此
$$
\alpha_{i}=\cfrac{1}{m}\sum_{j=1}^m\frac{\alpha_{i}\cdot p(\boldsymbol x_{j}|\boldsymbol\mu_{i},\mathbf\Sigma_{i})}{\sum_{l=1}^k\alpha_{l}\cdot p(\boldsymbol x_{j}|\boldsymbol\mu_{l},\mathbf\Sigma_{l})}
$$
又由公式(9.30)可知$\cfrac{\alpha_{i}\cdot p(\boldsymbol x_{j}|\boldsymbol\mu_{i},\mathbf\Sigma_{i})}{\sum_{l=1}^k\alpha_{l}\cdot p(\boldsymbol x_{j}|\boldsymbol\mu_{l},\mathbf\Sigma_{l})}=\gamma_{ji}$，所以上式可进一步化简为
$$
\alpha_{i}=\cfrac{1}{m}\sum_{j=1}^m\gamma_{ji}
$$

此即为公式(9.38)


