```{important}
参与组队学习的同学须知：

本章学习时间：3天

本章配套视频教程：https://www.bilibili.com/video/BV1Mh411e7VU?p=14
```

# 第9章 聚类

到目前为止，前面章节介绍的方法都是针对监督学习(supervised
learning)的，本章介绍的聚类(clustering)和下一章介绍的降维属于无监督学习(unsupervised
learning)。

## 9.1 聚类任务

单词"cluster"既是动词也是名词，作为名词时翻译为"簇"，即聚类得到的子集；一般谈到"聚类"这个概念时对应其动名词形式"clustering"。

## 9.2 性能度量

本节给出了聚类性能度量的三种外部指标和两种内部指标，其中式(9.5)
\~式(9.7)是基于式(9.1)
\~式(9.4)导出的三种外部指标，而式(9.12)和式(9.13)是基于式(9.8)
\~式(9.11)导出的两种内部指标。读本节内容需要心里清楚的一点：本节给出的指标仅是该领域的前辈们定义的指标，在个人研究过程中可以根据需要自己定义，说不定就会被业内同行广泛使用。

### 9.2.1 式(9.5)的解释

给定两个集合$A$和$B$，则Jaccard系数定义为如下公式


$$
JC=\frac{|A\bigcap B|}{|A\bigcup B|}=\frac{|A\bigcap B|}{|A|+|B|-|A\bigcap B|}
$$


Jaccard系数可以用来描述两个集合的相似程度。
推论：假设全集$U$共有$n$个元素，且$A\subseteq U$，$B\subseteq U$，则每一个元素的位置共有四种情况：

1.  元素同时在集合$A$和$B$中，这样的元素个数记为$M_{11}$；

2.  元素出现在集合$A$中，但没有出现在集合$B$中，这样的元素个数记为$M_{10}$；

3.  元素没有出现在集合$A$中，但出现在集合$B$中，这样的元素个数记为$M_{01}$；

4.  元素既没有出现在集合$A$中，也没有出现在集合$B$中，这样的元素个数记为$M_{00}$。

根据Jaccard系数的定义，此时的Jaccard系数为如下公式


$$
\mathrm{JC}=\frac{M_{11}}{M_{11}+M_{10}+M_{01}}
$$


由于聚类属于无监督学习，事先并不知道聚类后样本所属类别的类别标记所代表的意义，即便参考模型的类别标记意义是已知的，我们也无法知道聚类后的类别标记与参考模型的类别标记是如何对应的，况且聚类后的类别总数与参考模型的类别总数还可能不一样，因此只用单个样本无法衡量聚类性能的好坏。

由于外部指标的基本思想就是以参考模型的类别划分为参照，因此如果某一个样本对中的两个样本在聚类结果中同属于一个类，在参考模型中也同属于一个类，或者这两个样本在聚类结果中不同属于一个类，在参考模型中也不同属于一个类，那么对于这两个样本来说这是一个好的聚类结果。

总的来说所有样本对中的两个样本共存在四种情况：

1.  样本对中的两个样本在聚类结果中属于同一个类，在参考模型中也属于同一个类；

2.  样本对中的两个样本在聚类结果中属于同一个类，在参考模型中不属于同一个类；

3.  样本对中的两个样本在聚类结果中不属于同一个类，在参考模型中属于同一个类；

4.  样本对中的两个样本在聚类结果中不属于同一个类，在参考模型中也不属于同一个类。

综上所述，即所有样本对存在着书中式(9.1)
\~式(9.4)的四种情况，现在假设集合$A$中存放着两个样本都同属于聚类结果的同一个类的样本对，即$A=SS\bigcup SD$，集合$B$中存放着两个样本都同属于参考模型的同一个类的样本对，即$B=SS\bigcup DS$，那么根据Jaccard系数的定义有：


$$
\mathrm{JC}=\frac{|A\bigcap B|}{|A\bigcup B|}=\frac{|SS|}{|SS\bigcup SD\bigcup DS|}=\frac{a}{a+b+c}
$$


也可直接将书中式(9.1)
\~式(9.4)的四种情况类比推论，即$M_{11}=a$，$M_{10}=b$，$M_{01}=c$，所以


$$
\mathrm{JC}=\frac{M_{11}}{M_{11}+M_{10}+M_{01}}=\frac{a}{a+b+c}
$$



### 9.2.2 式(9.6)的解释

其中$\frac{a}{a+b}$和$\frac{a}{a+c}$为Wallace提出的两个非对称指标，$a$代表两个样本在聚类结果和参考模型中均属于同一类的样本对的个数，$a+b$代表两个样本在聚类结果中属于同一类的样本对的个数，$a+c$代表两个样本在参考模型中属于同一类的样本对的个数，这两个非对称指标均可理解为样本对中的两个样本在聚类结果和参考模型中均属于同一类的概率。由于指标的非对称性，这两个概率值往往不一样，因此Fowlkes和Mallows提出利用几何平均数将这两个非对称指标转化为一个对称指标，即Fowlkes
and Mallows Index, FMI。

### 9.2.3 式(9.7)的解释

Rand Index定义如下：


$$
\mathrm{RI}=\frac{a+d}{a+b+c+d}=\frac{a+d}{m(m-1)/2}=\frac{2(a+d)}{m(m-1)}
$$


由第一个等号可知RI肯定不大于1。之所以 $a+b+c+d=m(m-1) / 2$,
这是因为式(9.1) \~式(9.4)遍历 了所有
$\left(\boldsymbol{x}_i, \boldsymbol{x}_j\right)$ 组合对 $(i \neq j)$ :
其中 $i=1$ 时 $j$ 可以取 2 到 $m$ 共 $m-1$ 个值, $i=2$ 时 $j$ 可以取 3
到 $m$ 共 $m-2$ 个值, $\cdots \cdots, i=m-1$ 时 $j$ 仅可以取 $m$ 共 1
个值, 因此 $\left(\boldsymbol{x}_i, \boldsymbol{x}_j\right)$ 组合对的个
数为从 1 到 $m - 1$求和, 根据等差数列求和公式即得 $m(m-1) / 2$ 。

这个指标可以理解为两个样本都属于聚类结果和参考模型中的同一类的样本对的个数与两个样本都分别不属于聚类结果和参考模型中的同一类的样本对的个数的总和在所有样本对中出现的频率，可以简单理解为聚类结果与参考模型的一致性。

### 9.2.4 式(9.8)的解释

簇内距离的定义式：求和号左边是$(x_i, x_j)$组合个数的倒数，求和号右边是这些组合的距离和，所以两者相乘定义为平均距离。

### 9.2.5 式(9.12)的解释

式中, $k$ 表示聚类结果中簇的个数。该式的DBI值越小越好,
因为我们希望"物以类聚", 即同一簇的样本尽可能彼此相似,
$\operatorname{avg}\left(C_i\right)$ 和
$\operatorname{avg}\left(C_j\right)$ 越小越好; 我们希望不同簇的样本尽
可能不同, 即 $d_{\text {cen }}\left(C_i, C_j\right)$ 越大越好。 [勘误:
第 25 次印刷将分母
$d_{\text {cen }}\left(\boldsymbol{\mu}_i, \boldsymbol{\mu}_j\right)$
改为
$d_{\text {cen }}\left(C_i, C_j\right)$]{style="background-color: lightgray"}

## 9.3 距离计算

距离计算在各种算法中都很常见，本节介绍的距离计算方式和"西瓜书"10.6节介绍的马氏距离基本囊括了一般的距离计算方法。另外可能还会碰到"西瓜书"10.5节的测地线距离。

本节有很多概念和名词很常见，比如本节开篇介绍的距离度量的四个基本性质、闵可夫斯基距离、欧氏距离、曼哈顿距离、切比雪夫距离、数值属性、离散属性、有序属性、无序属性、非度量距离等，注意对应的中文和英文。

### 9.3.1 式(9.21)的解释

该式符号较为抽象, 下面计算"西瓜书"第 76 页表4.1西瓜数据集2.0属性根蒂上
"蜷缩" 和 "稍 蜷"两个离散值之间的距离。

此时, $u$ 为 "根蒂", $a$ 为属性根蒂上取值为 "蜷缩", $b$
为属性根蒂上取值为 "稍蜷", 根据边注, 此时样本类别已知 (好瓜/坏瓜), 因此
$k=2$ 。

从"西瓜书"表4.1中可知, 根蒂为蜷缩的样本共有 8 个 (编号 $1\sim 5$、编号
12、编号 $16\sim 17$), 即 $m_{u, a}=8$, 根蒂为稍蜷的样本共有 7 个 (编号
$6 \sim 9$ 和编号 $13 \sim 15$), 即 $m_{u, b}=7$; 设 $i=1$ 对 应好瓜,
$i=2$ 对应坏瓜, 好瓜中根蒂为蜷缩的样本共有 5 个（编号 $1 \sim 5$ ), 即
$m_{u, a, 1}=5$, 好瓜中根蒂为稍蜷的样本共有 3 个 (编号 6 8), 即
$m_{u, b, 1}=3$, 坏瓜中根蒂为蜷缩的样本 共有 3 个 (编号 12 和编号
$16 \sim 17$ ), 即 $m_{u, a, 2}=3$, 坏瓜中根蒂为稍蜷的样本共有 4 个（编
号 9 和编号 $13 \sim 15)$, 即 $m_{u, b, 2}=4$, 因此 VDM 距离为


$$
\begin{aligned}
\operatorname{VDM}_p(a, b) & =\left|\frac{m_{u, a, 1}}{m_{u, a}}-\frac{m_{u, b, 1}}{m_{u, b}}\right|^p+\left|\frac{m_{u, a, 2}}{m_{u, a}}-\frac{m_{u, b, 2}}{m_{u, b}}\right|^p \\
& =\left|\frac{5}{8}-\frac{3}{7}\right|^p+\left|\frac{3}{8}-\frac{4}{7}\right|^p
\end{aligned}
$$



## 9.4 原型聚类

本节介绍了三个原型聚类算法, 其中 $k$ 均值算法最为经典,
几乎成为聚类的代名词, 在 Matlab、scikit-learn等主流的科学计算包中均有
kmeans 函数供调用。学习向量量化也是无监督聚类的一种方式,
在向量检索的引擎，比如facebook faiss中发挥重要的应用。

前两个聚类算法比较易懂, 下面主要推导第三个聚类算法：高斯混合聚类。

### 9.4.1 式(9.28)的解释

该式就是多元高斯分布概率密度函数的定义式:


$$
p(\boldsymbol{x})=\frac{1}{(2 \pi)^{\frac{n}{2}}|\boldsymbol{\Sigma}|^{\frac{1}{2}}} e^{-\frac{1}{2}(\boldsymbol{x}-\boldsymbol{\mu})^{\top} \boldsymbol{\Sigma}^{-1}(\boldsymbol{x}-\boldsymbol{\mu})}
$$


对应到我们常见的一元高斯分布概率密度函数的定义式:


$$
p(x)=\frac{1}{\sqrt{2 \pi} \sigma} e^{-\frac{(x-\mu)^2}{2 \sigma^2}}
$$


其中 $\sqrt{2 \pi}=(2 \pi)^{\frac{1}{2}}$ 对应
$(2 \pi)^{\frac{n}{2}}, \sigma$ 对应
$|\boldsymbol{\Sigma}|^{\frac{1}{2}}$, 指数项中分母中的方差 $\sigma^2$
对应协方差矩阵 $\boldsymbol{\Sigma}$,
$\frac{(x-\mu)^2}{\sigma^2} \text { 对应 }(\boldsymbol{x}-\boldsymbol{\mu})^{\top} \boldsymbol{\Sigma}^{-1}(\boldsymbol{x}-\boldsymbol{\mu})$。

概率密度函数 $p(\boldsymbol{x})$ 是 $\boldsymbol{x}$
的函数。其中对于某个特定的 $\boldsymbol{x}$ 来说, 函数值
$p(\boldsymbol{x})$ 就是一个数, 若 $\boldsymbol{x}$ 的维度为 2 ,
则可以将函数 $p(\boldsymbol{x})$ 的图像可视化,
是三维空间的一个曲面。类似于一元高 斯分布 $p(x)$ 与横轴 $p(x)=0$
之间的面积等于 1 (即 $\int p(x) \mathrm{d} x=1$ )， $p(\boldsymbol{x})$
曲面与平面 $p(\boldsymbol{x})=0$ 之间的体积等于 1 （即
$\int p(\boldsymbol{x}) \mathrm{d} \boldsymbol{x}=1$ 。

注意, "西瓜书"中后面将 $p(\boldsymbol{x})$ 记为
$p(\boldsymbol{x} \mid \boldsymbol{\mu}, \boldsymbol{\Sigma})$ 。

### 9.4.2 式(9.29)的解释

对于该式表达的高斯混合分布概率密度函数
$p_{\mathcal{M}}(\boldsymbol{x})$, 与式 $(9.28)$ 中的
$p(\boldsymbol{x})$ 不同的是, 它 由 $k$
个不同的多元高斯分布加权而来。具体来说, $p(\boldsymbol{x})$ 仅由参数
$\boldsymbol{\mu}, \boldsymbol{\Sigma}$ 确定, 而
$p_{\mathcal{M}}(\boldsymbol{x})$ 由 $k$ 个 "混合系数" $\alpha_i$ 以及
$k$ 组参数 $\boldsymbol{\mu}_i, \boldsymbol{\Sigma}_i$ 确定。

在"西瓜书"中该式下方(P207 最后一段)中介绍了样本的生成过程, 实际也反应了
"混合系数" $\alpha_i$ 的含义, 即 $\alpha_i$ 为选择第 $i$
个混合成分的概率, 或者反过来说, $\alpha_i$ 为样本属于第 $i$ 个混合
成分的概率。重新描述一下样本生成过程, 根据先验分布
$\alpha_1, \alpha_2, \ldots, \alpha_k$ 选择其中一个高斯 混合成分 (即第
$i$ 个高斯混合成分被选到的概率为 $\alpha_i$ ), 假设选到了第 $i$
个高斯混合成分, 其 参数为 $\boldsymbol{\mu}_i, \boldsymbol{\Sigma}_i$;
然后根据概率密度函数
$p\left(\boldsymbol{x} \mid \boldsymbol{\mu}_i, \boldsymbol{\Sigma}_i\right)$
(即将式(9.28) 中的 $\boldsymbol{\mu}, \boldsymbol{\Sigma}$ 替换为
$\left.\boldsymbol{\mu}_i, \boldsymbol{\Sigma}_i\right)$
进行采样生成样本 $\boldsymbol{x}$ 。两个步骤的区别在于第 1
步选择高斯混合成分时是从 $k$ 个之中选其一 (相当于概率密度函数是离散的),
而第 2 步生成样本时是从 $\boldsymbol{x}$ 定义域中根据
$p\left(\boldsymbol{x} \mid \boldsymbol{\mu}_i, \boldsymbol{\Sigma}_i\right.$
)选 择其中一个样本, 样本 $\boldsymbol{x}$ 被选中的概率即为
$p\left(\boldsymbol{x} \mid \boldsymbol{\mu}_i, \boldsymbol{\Sigma}_i\right)$
。即第 1 步对应于离散型随机变量, 第 2 步对应于连续型随机变量。

### 9.4.3 式(9.30)的解释

若由上述样本生成方式得到训练集
$D=\left\{\boldsymbol{x}_1, \boldsymbol{x}_2, \ldots, \boldsymbol{x}_m\right\}$,
现在的问题是对于给定样 本 $\boldsymbol{x}_j$,
它是由哪个高斯混合成分生成的呢? 该问题即求后验概率
$p_{\mathcal{M}}\left(z_j \mid \boldsymbol{x}_j\right)$, 其中
$z_j \in\{1,2, \ldots, k\}$ 。下面对式(9.30)进行推导。

对于任意样本, 在不考虑样本本身之前 (即先验), 若瞎猜一下它由第 $i$
个高斯混合成分 生成的概率 $P\left(z_j=i\right)$, 那么肯定按先验概率
$\alpha_1, \alpha_2, \ldots, \alpha_k$ 进行猜测, 即
$P\left(z_j=i\right)=\alpha_i$ 。 若考虑样本本身带来的信息 (即后验),
此时再猜一下它由第 $i$ 个高斯混合成分生成的概率
$p_{\mathcal{M}}\left(z_j=i \mid \boldsymbol{x}_j\right)$,
根据贝叶斯公式, 后验概率
$p_{\mathcal{M}}\left(z_j=i \mid \boldsymbol{x}_j\right)$ 可写为


$$
p_{\mathcal{M}}\left(z_j=i \mid \boldsymbol{x}_j\right)=\frac{P\left(z_j=i\right) \cdot p_{\mathcal{M}}\left(\boldsymbol{x}_j \mid z_j=i\right)}{p_{\mathcal{M}}\left(\boldsymbol{x}_j\right)}
$$


分子第 1 项 $P\left(z_j=i\right)=\alpha_i$; 第 2 项即第 $i$
个高斯混合成分生成样本 $\boldsymbol{x}_j$ 的概率
$p\left(\boldsymbol{x}_j \mid \boldsymbol{\mu}_i, \boldsymbol{\Sigma}_i\right)$,
根据式(9.28)将 $\boldsymbol{x}, \boldsymbol{\mu}, \boldsymbol{\Sigma}$
替换为 $\boldsymbol{x}_j, \boldsymbol{\mu}_i, \boldsymbol{\Sigma}_i$
即得; 分母 $p_{\mathcal{M}}\left(\boldsymbol{x}_j\right)$ 即为将
$\boldsymbol{x}_j$ 代入式(9.29)即得。

注意, "西瓜书"中后面将
$p_{\mathcal{M}}\left(z_j=i \mid \boldsymbol{x}_j\right)$ 记为
$\gamma_{j i}$, 其中 $1 \leq j \leq m, 1 \leq i \leq k$。

### 9.4.4 式(9.31)的解释

若将所有 $\gamma_{j i}$ 组成一个矩阵 $\Gamma$, 其中 $\gamma_{j i}$ 为第
$j$ 行第例的元素, 矩阵 $\Gamma$ 大小为 $m \times k$, 即


$$
\Gamma=\left[\begin{array}{cccc}
\gamma_{11} & \gamma_{12} & \cdots & \gamma_{1 k} \\
\gamma_{21} & \gamma_{22} & \cdots & \gamma_{2 k} \\
\vdots & \vdots & \ddots & \vdots \\
\gamma_{m 1} & \gamma_{m 2} & \cdots & \gamma_{m k}
\end{array}\right]_{m \times k}
$$

 其中 $m$ 为训练集样本个数, $k$
为高斯混合模型包含的混合模型个数。 可以看出, 式(9.31)就是找出矩阵
$\Gamma$ 第 $j$ 行的所有 $k$ 个元素中最大的那个元素的位置。

### 9.4.5 式(9.32)的解释

对于训练集
$D=\left\{\boldsymbol{x}_1, \boldsymbol{x}_2, \ldots, \boldsymbol{x}_m\right\}$,
现在要把 $m$ 个样本划分为 $k$ 个簇, 即认为训练集 $D$ 的样本是根据 $k$
个不同的多元高斯分布加权而得的高斯混合模型生成的。

现在的问题是, $k$ 个不同的多元高斯分布的参数
$\left\{\left(\boldsymbol{\mu}_i, \boldsymbol{\Sigma}_i\right) \mid 1 \leqslant i \leqslant k\right\}$
及它们各自的权 重 $\alpha_1, \alpha_2, \ldots, \alpha_k$ 不知道, $m$
个样本归到底属于哪个簇也不知道, 该怎么办呢?

其实这跟 $k$ 均值算法类似, 开始时既不知道 $k$ 个簇的均值向量, 也不知道
$m$ 个样本归到 底属于哪个簇, 最后我们采用了贪心策略,
通过迭代优化来近似求解式(9.24)。

本节的高斯混合聚类求解方法与 $k$ 均值算法, 只是具体问题具体解法不同,
从整体上来 说, 它们都应用了 $7.6$ 节的期望最大化算法(EM 算法)。

具体来说, 现假设已知式(9.30)的后验概率, 此时即可通过式(9.31)知道 $m$
个样本归到底 属于哪个簇, 再来求解参数
$\left\{\left(\alpha_i, \boldsymbol{\mu}_i, \boldsymbol{\Sigma}_i\right) \mid 1 \leqslant i \leqslant k\right\}$,
怎么求解呢? 对于每个样本 $\boldsymbol{x}_j$ 来说, 它出现的概率是
$p_{\mathcal{M}}\left(\boldsymbol{x}_j\right)$, 既然现在训练集 $D$
中确实出现了 $\boldsymbol{x}_j$, 我们当然希望待求解的参数
$\left\{\left(\alpha_i, \boldsymbol{\mu}_i, \boldsymbol{\Sigma}_i\right) \mid 1 \leqslant i \leqslant k\right\}$
能够使这种可能性 $p_{\mathcal{M}}\left(\boldsymbol{x}_j\right)$ 最大;
又因为我们假设 $m$ 个样本是独 立的, 因此它们恰好一起出现的概率就是
$\prod_{j=1}^m p_{\mathcal{M}}\left(\boldsymbol{x}_j\right)$,
即所谓的似然函数; 一般来说, 连乘容易造成下溢 ( $m$ 个大于 0 小于 1
的数相乘, 当 $m$ 较大时, 乘积会非常非常小, 以致
于计算机无法表达这么小的数, 产生下溢), 所以常用对数似然替代,
即式(9.32)。

### 9.4.6 式(9.33)的推导

根据公式(9.28)可知：


$$
p\left(\boldsymbol{x}_{j} | \boldsymbol{\mu}_{i}, \boldsymbol{\Sigma}_{i}\right)=\frac{1}{(2 \pi)^{\frac{n}{2}}\left|\boldsymbol{\Sigma}_{i}\right|^{\frac{1}{2}}} \exp \left(-\frac{1}{2}\left(\boldsymbol{x}_{j}-\boldsymbol{\mu}_{i}\right)^{T} \boldsymbol{\Sigma}_{i}^{-1}\left(\boldsymbol{x}_{j}-\boldsymbol{\mu}_{i}\right)\right)
$$


又根据公式(9.32)，由


$$
\frac{\partial L L(D)}{\partial \boldsymbol{\mu}_{i}}=\frac{\partial L L(D)}{\partial p\left(\boldsymbol{x}_{j} | \boldsymbol{\mu}_{i}, \boldsymbol{\Sigma}_{i}\right)} \cdot \frac{\partial p\left(\boldsymbol{x}_{j} | \boldsymbol{\mu}_{i}, \boldsymbol{\Sigma}_{i}\right)}{\partial \boldsymbol{\mu}_{i}}=0
$$


其中： 

$$
\begin{aligned}
\frac{\partial L L(D)}{\partial p\left(\boldsymbol{x}_{j} | \boldsymbol{\mu}_{i}, \mathbf{\Sigma}_{i}\right)} &=\frac{\partial \sum_{j=1}^{m} \ln \left(\sum_{l=1}^{k} \alpha_{l} \cdot p\left(\boldsymbol{x}_{j} | \boldsymbol{\mu}_{l}, \boldsymbol{\Sigma}_{l}\right)\right)}{\partial p\left(\boldsymbol{x}_{j} | \boldsymbol{\mu}_{i}, \boldsymbol{\Sigma}_{i}\right)} \\
&=\sum_{j=1}^{m} \frac{\partial \ln \left(\sum_{l=1}^{k} \alpha_{l} \cdot p\left(\boldsymbol{x}_{j} | \boldsymbol{\mu}_{l}, \boldsymbol{\Sigma}_{l}\right)\right)}{\partial p\left(\boldsymbol{x}_{j} | \boldsymbol{\mu}_{i}, \boldsymbol{\Sigma}_{i}\right)} \\
&=\sum_{j=1}^{m} \frac{\alpha_{i}}{\sum_{l=1}^{k} \alpha_{l} \cdot p\left(\boldsymbol{x}_{j} | \boldsymbol{\mu}_{l}, \boldsymbol{\Sigma}_{l}\right)}
\end{aligned}
$$





$$
\begin{aligned}
\frac{\partial p\left(\boldsymbol{x}_{j} | \boldsymbol{\mu}_{i}, \boldsymbol{\Sigma}_{i}\right)}{\partial \boldsymbol{\mu}_{i}} &=\frac{\partial \frac{1}{(2 \pi)^{\frac{n}{2}}\left|\Sigma_{i}\right|^{\frac{1}{2}}} \exp\left({-\frac{1}{2}\left(\boldsymbol{x}_{j}-\boldsymbol{\mu}_{i}\right)^{\top}\boldsymbol{\Sigma}_{i}^{-1}\left(\boldsymbol{x}_{j}-\boldsymbol{\mu}_{i}\right)}\right)}{\partial \boldsymbol{\mu}_{i}} \\
&=\frac{1}{(2 \pi)^{\frac{n}{2}}\left|\boldsymbol{\Sigma}_{i}\right|^{\frac{1}{2}}} \cdot \frac{\partial \exp\left({-\frac{1}{2}\left(\boldsymbol{x}_{j}-\boldsymbol{\mu}_{i}\right)^{\top} \boldsymbol{\Sigma}_{i}^{-1}\left(\boldsymbol{x}_{j}-\boldsymbol{\mu}_{i}\right)}\right)}{\partial \boldsymbol{\mu}_{i}}\\
&=\frac{1}{(2 \pi)^{\frac{n}{2}}\left|\boldsymbol{\Sigma}_{i}\right|^{\frac{1}{2}}}\cdot \exp\left({-\frac{1}{2}\left(\boldsymbol{x}_{j}-\boldsymbol{\mu}_{i}\right)^{\top} \boldsymbol{\Sigma}_{i}^{-1}\left(\boldsymbol{x}_{j}-\boldsymbol{\mu}_{i}\right)}\right) \cdot-\frac{1}{2} \frac{\partial\left(\boldsymbol{x}_{j}-\boldsymbol{\mu}_{i}\right)^{\top} \boldsymbol{\Sigma}_{i}^{-1}\left(\boldsymbol{x}_{j}-\boldsymbol{\mu}_{i}\right)}{\partial \boldsymbol{\mu}_{i}}\\
&=\frac{1}{(2 \pi)^{\frac{n}{2}}\left|\boldsymbol{\Sigma}_{i}\right|^{\frac{1}{2}}}\cdot \exp\left({-\frac{1}{2}\left(\boldsymbol{x}_{j}-\boldsymbol{\mu}_{i}\right)^{\top} \boldsymbol{\Sigma}_{i}^{-1}\left(\boldsymbol{x}_{j}-\boldsymbol{\mu}_{i}\right)}\right) \cdot\boldsymbol{\Sigma}_{i}^{-1}\left(\boldsymbol{x}_{j}-\boldsymbol{\mu}_{i}\right)\\
&=p\left(\boldsymbol{x}_{j} | \boldsymbol{\mu}_{i}, \boldsymbol{\Sigma}_{i}\right) \cdot \boldsymbol{\Sigma}_{i}^{-1}\left(\boldsymbol{x}_{j}-\boldsymbol{\mu}_{i}\right)
\end{aligned}
$$


其中，由矩阵求导的法则$\frac{\partial \mathbf{a}^{T} \mathbf{X} \mathbf{a}}{\partial \mathbf{a}}=2\mathbf{X} \mathbf{a}$可得：


$$
\begin{aligned}
-\frac{1}{2} \frac{\partial\left(\boldsymbol{x}_{j}-\boldsymbol{\mu}_{i}\right)^{\top} \boldsymbol{\Sigma}_{i}^{-1}\left(\boldsymbol{x}_{j}-\boldsymbol{\mu}_{i}\right)}{\partial \boldsymbol{\mu}_{i}} &=-\frac{1}{2} \cdot 2 \boldsymbol{\Sigma}_{i}^{-1}\left(\boldsymbol{\mu}_{i}-\boldsymbol{x}_{j}\right) \\
&=\boldsymbol{\Sigma}_{i}^{-1}\left(\boldsymbol{x}_{j}-\boldsymbol{\mu}_{i}\right)
\end{aligned}
$$

 因此有：


$$
\frac{\partial L L(D)}{\partial \boldsymbol{\mu}_{i}}=\sum_{j=1}^{m} \frac{\alpha_{i}}{\sum_{l=1}^{k} \alpha_{l} \cdot p\left(\boldsymbol{x}_{j} | \boldsymbol{\mu}_{l}, \mathbf{\Sigma}_{l}\right)} \cdot p\left(\boldsymbol{x}_{j} | \boldsymbol{\mu}_{i}, \boldsymbol{\Sigma}_{i}\right) \cdot \boldsymbol{\Sigma}_{i}^{-1}\left(\boldsymbol{x}_{j}-\boldsymbol{\mu}_{i}\right)=0
$$



### 9.4.7 式(9.34)的推导

由式(9.30)


$$
\gamma_{j i}=p_{\mathcal{M}}\left(z_{j}=i | \mathbf{X}_{j}\right)=\frac{\alpha_{i} \cdot p\left(\mathbf{X}_{j} | \boldsymbol{\mu}_{i}, \boldsymbol{\Sigma}_{i}\right)}{\sum_{l=1}^{k} \alpha_{l} \cdot p\left(\mathbf{X}_{j} | \boldsymbol{\mu}_{l}, \boldsymbol{\Sigma}_{l}\right)}
$$


带入式(9.33)


$$
\sum_{j=1}^{m} \gamma_{j i}\left(\mathbf{X}_{j}-\boldsymbol{\mu}_{i}\right)=0
$$


移项, 得


$$
\sum_{j=1}^m \gamma_{j i} \boldsymbol{x}_j=\sum_{j=1}^m \gamma_{j i} \boldsymbol{\mu}_i=\boldsymbol{\mu}_i \cdot \sum_{j=1}^m \gamma_{j i}
$$


第二个等号是因为 $\mu_i$ 对于求和变量 $j$ 来说是常量,
因此可以提到求和号外面; 因此


$$
\boldsymbol{\mu}_i=\frac{\sum_{j=1}^m \gamma_{j i} \boldsymbol{x}_j}{\sum_{j=1}^m \gamma_{j i}}
$$



### 9.4.8 式(9.35)的推导

根据公式(9.28)可知：


$$
p(\boldsymbol x_{j}|\boldsymbol\mu_{i},\boldsymbol\Sigma_{i})=\cfrac{1}{(2\pi)^\frac{n}{2}\left| \boldsymbol\Sigma_{i}\right |^\frac{1}{2}}\exp\left({-\frac{1}{2}(\boldsymbol x_{j}-\boldsymbol\mu_{i})^T\boldsymbol\Sigma_{i}^{-1}(\boldsymbol x_{j}-\boldsymbol\mu_{i})}\right)
$$


又根据公式(9.32)，由


$$
\cfrac{\partial LL(D)}{\partial \boldsymbol\Sigma_{i}}=0
$$

 可得


$$
\begin{aligned}
\cfrac {\partial LL(D)}{\partial\boldsymbol\Sigma_{i}}&=\cfrac {\partial}{\partial \boldsymbol\Sigma_{i}}\left[\sum_{j=1}^m\ln\Bigg(\sum_{i=1}^k \alpha_{i}\cdot p(\boldsymbol x_{j}|\boldsymbol\mu_{i},\boldsymbol\Sigma_{i})\Bigg)\right] \\
&=\sum_{j=1}^m\frac{\partial}{\partial\boldsymbol\Sigma_{i}}\left[\ln\Bigg(\sum_{i=1}^k \alpha_{i}\cdot p(\boldsymbol x_{j}|\boldsymbol\mu_{i},\boldsymbol\Sigma_{i})\Bigg)\right] \\
&=\sum_{j=1}^m\cfrac{\alpha_{i}\cdot \cfrac{\partial}{\partial\boldsymbol\Sigma_{i}}\left(p(\boldsymbol x_{j}|\boldsymbol\mu_{i},\boldsymbol\Sigma_{i})\right)}{\sum_{l=1}^k\alpha_{l}\cdot p(\boldsymbol x_{j}|\boldsymbol\mu_{l},\boldsymbol\Sigma_{l})} \\
\end{aligned}
$$

 其中 

$$
\begin{aligned}
\cfrac{\partial}{\partial\boldsymbol\Sigma_{i}}\left(p(\boldsymbol x_{j}|\boldsymbol\mu_{i},\boldsymbol\Sigma_{i})\right)&=\cfrac{\partial}{\partial\boldsymbol\Sigma_{i}}\left[\cfrac{1}{(2\pi)^\frac{n}{2}\left| \boldsymbol\Sigma_{i}\right |^\frac{1}{2}}\exp\left({-\frac{1}{2}(\boldsymbol x_{j}-\boldsymbol\mu_{i})^T\boldsymbol\Sigma_{i}^{-1}(\boldsymbol x_{j}-\boldsymbol\mu_{i})}\right)\right] \\
&=\cfrac{\partial}{\partial\boldsymbol\Sigma_{i}}\left\{\exp\left[\ln\left(\cfrac{1}{(2\pi)^\frac{n}{2}\left| \boldsymbol\Sigma_{i}\right |^\frac{1}{2}}\exp\left({-\frac{1}{2}(\boldsymbol x_{j}-\boldsymbol\mu_{i})^T\boldsymbol\Sigma_{i}^{-1}(\boldsymbol x_{j}-\boldsymbol\mu_{i})}\right)\right)\right]\right\} \\
&=p(\boldsymbol x_{j}|\boldsymbol\mu_{i},\boldsymbol\Sigma_{i})\cdot\cfrac{\partial}{\partial\boldsymbol\Sigma_{i}}\left[\ln\left(\cfrac{1}{(2\pi)^\frac{n}{2}\left| \boldsymbol\Sigma_{i}\right |^\frac{1}{2}}\exp\left({-\frac{1}{2}(\boldsymbol x_{j}-\boldsymbol\mu_{i})^T\boldsymbol\Sigma_{i}^{-1}(\boldsymbol x_{j}-\boldsymbol\mu_{i})}\right)\right)\right]\\
&=p(\boldsymbol x_{j}|\boldsymbol\mu_{i},\boldsymbol\Sigma_{i})\cdot\cfrac{\partial}{\partial\boldsymbol\Sigma_{i}}\left[\ln\cfrac{1}{(2\pi)^{\frac{n}{2}}}-\cfrac{1}{2}\ln{|\boldsymbol{\Sigma}_i|}-\frac{1}{2}(\boldsymbol x_j-\boldsymbol\mu_i)^T\boldsymbol{\Sigma}_i^{-1}(\boldsymbol x_j-\boldsymbol\mu_i)\right]\\
&=p(\boldsymbol x_{j}|\boldsymbol\mu_{i},\boldsymbol\Sigma_{i})\cdot\left[-\cfrac{1}{2}\cfrac{\partial\left(\ln{|\boldsymbol{\Sigma}_i|}\right) }{\partial \boldsymbol{\Sigma}_i}-\cfrac{1}{2}\cfrac{\partial \left[(\boldsymbol x_j-\boldsymbol\mu_i)^T\boldsymbol{\Sigma}_i^{-1}(\boldsymbol x_j-\boldsymbol\mu_i)\right]}{\partial \boldsymbol{\Sigma}_i}\right]\\
\end{aligned}
$$


由矩阵微分公式$\cfrac{\partial |\mathbf{X}|}{\partial \mathbf{X}}=|\mathbf{X}|\cdot(\mathbf{X}^{-1})^{T},\cfrac{\partial \boldsymbol{a}^{T} \mathbf{X}^{-1} \mathbf{B}}{\partial \mathbf{X}}=-\mathbf{X}^{-T} \boldsymbol{a b}^{T} \mathbf{X}^{-T}$可得


$$
\begin{aligned}
\cfrac{\partial}{\partial\boldsymbol\Sigma_{i}}\left(p(\boldsymbol x_{j}|\boldsymbol\mu_{i},\boldsymbol\Sigma_{i})\right)&=p(\boldsymbol x_{j}|\boldsymbol\mu_{i},\boldsymbol\Sigma_{i})\cdot\left[-\cfrac{1}{2}\boldsymbol{\Sigma}_i^{-1}+\cfrac{1}{2}\boldsymbol{\Sigma}_i^{-1}(\boldsymbol x_j-\boldsymbol\mu_i)(\boldsymbol x_j-\boldsymbol\mu_i)^T\boldsymbol{\Sigma}_i^{-1}\right]\\
\end{aligned}
$$


将此式代回$\cfrac {\partial LL(D)}{\partial\boldsymbol\Sigma_{i}}$中可得


$$
\cfrac {\partial LL(D)}{\partial\boldsymbol\Sigma_{i}}=\sum_{j=1}^m\cfrac{\alpha_{i}\cdot p(\boldsymbol x_{j}|\boldsymbol\mu_{i},\boldsymbol\Sigma_{i})}{\sum_{l=1}^k\alpha_{l}\cdot p(\boldsymbol x_{j}|\boldsymbol\mu_{l},\boldsymbol\Sigma_{l})}\cdot\left[-\cfrac{1}{2}\boldsymbol{\Sigma}_i^{-1}+\cfrac{1}{2}\boldsymbol{\Sigma}_i^{-1}(\boldsymbol x_j-\boldsymbol\mu_i)(\boldsymbol x_j-\boldsymbol\mu_i)^T\boldsymbol{\Sigma}_i^{-1}\right]
$$


又由公式(9.30)可知$\cfrac{\alpha_{i}\cdot p(\boldsymbol x_{j}|\boldsymbol\mu_{i},\boldsymbol\Sigma_{i})}{\sum_{l=1}^k\alpha_{l}\cdot p(\boldsymbol x_{j}|\boldsymbol\mu_{l},\boldsymbol\Sigma_{l})}=\gamma_{ji}$，所以上式可进一步化简为


$$
\cfrac {\partial LL(D)}{\partial\boldsymbol\Sigma_{i}}=\sum_{j=1}^m\gamma_{ji}\cdot\left[-\cfrac{1}{2}\boldsymbol{\Sigma}_i^{-1}+\cfrac{1}{2}\boldsymbol{\Sigma}_i^{-1}(\boldsymbol x_j-\boldsymbol\mu_i)(\boldsymbol x_j-\boldsymbol\mu_i)^T\boldsymbol{\Sigma}_i^{-1}\right]
$$


令上式等于0可得


$$
\cfrac {\partial LL(D)}{\partial\boldsymbol\Sigma_{i}}=\sum_{j=1}^m\gamma_{ji}\cdot\left[-\cfrac{1}{2}\boldsymbol{\Sigma}_i^{-1}+\cfrac{1}{2}\boldsymbol{\Sigma}_i^{-1}(\boldsymbol x_j-\boldsymbol\mu_i)(\boldsymbol x_j-\boldsymbol\mu_i)^T\boldsymbol{\Sigma}_i^{-1}\right]=0
$$


移项推导有： 

$$
\begin{aligned}
\sum_{j=1}^m\gamma_{ji}\cdot\left[-\boldsymbol{I}+(\boldsymbol x_j-\boldsymbol\mu_i)(\boldsymbol x_j-\boldsymbol\mu_i)^T\boldsymbol{\Sigma}_i^{-1}\right]&=0\\
\sum_{j=1}^m\gamma_{ji}(\boldsymbol x_j-\boldsymbol\mu_i)(\boldsymbol x_j-\boldsymbol\mu_i)^T\boldsymbol{\Sigma}_i^{-1}&=\sum_{j=1}^m\gamma_{ji}\boldsymbol{I}\\
\sum_{j=1}^m\gamma_{ji}(\boldsymbol x_j-\boldsymbol\mu_i)(\boldsymbol x_j-\boldsymbol\mu_i)^T&=\sum_{j=1}^m\gamma_{ji}\boldsymbol{\Sigma}_i\\
\boldsymbol{\Sigma}_i^{-1}\cdot\sum_{j=1}^m\gamma_{ji}(\boldsymbol x_j-\boldsymbol\mu_i)(\boldsymbol x_j-\boldsymbol\mu_i)^T&=\sum_{j=1}^m\gamma_{ji}\\
\boldsymbol{\Sigma}_i&=\cfrac{\sum_{j=1}^m\gamma_{ji}(\boldsymbol x_j-\boldsymbol\mu_i)(\boldsymbol x_j-\boldsymbol\mu_i)^T}{\sum_{j=1}^m\gamma_{ji}}
\end{aligned}
$$

 此即为公式(9.35)。

### 9.4.9 式(9.36)的解释

该式即$LL(D)$添加了等式约束$\sum_{i=1}^k{\alpha_i=1}$的拉格朗日形式。

### 9.4.10 式(9.37)的推导

重写式(9.32)如下:


$$
L L(D)=\sum_{j=1}^m \ln \left(\sum_{l=1}^k \alpha_l \cdot p\left(\boldsymbol{x}_j \mid \boldsymbol{\mu}_l, \boldsymbol{\Sigma}_l\right)\right)
$$


这里将第 2 个求和号的求和变量由式(9.32)的 $i$ 改为了 $l$, 这是为了避免对
$\alpha_i$ 求导时与变量 $i$ 相 混淆。将式(9.36)中的两项分别对 $\alpha_i$
求导, 得 

$$
\begin{aligned}
\frac{\partial L L(D)}{\partial \alpha_i} & =\frac{\partial \sum_{j=1}^m \ln \left(\sum_{l=1}^k \alpha_l \cdot p\left(\boldsymbol{x}_j \mid \boldsymbol{\mu}_l, \boldsymbol{\Sigma}_l\right)\right)}{\partial \alpha_i} \\
& =\sum_{j=1}^m \frac{1}{\sum_{l=1}^k \alpha_l \cdot p\left(\boldsymbol{x}_j \mid \boldsymbol{\mu}_l, \boldsymbol{\Sigma}_l\right)} \cdot \frac{\partial \sum_{l=1}^k \alpha_l \cdot p\left(\boldsymbol{x}_j \mid \boldsymbol{\mu}_l, \boldsymbol{\Sigma}_l\right)}{\partial \alpha_i} \\
& =\sum_{j=1}^m \frac{1}{\sum_{l=1}^k \alpha_l \cdot p\left(\boldsymbol{x}_j \mid \boldsymbol{\mu}_l, \boldsymbol{\Sigma}_l\right)} \cdot p\left(\boldsymbol{x}_j \mid \boldsymbol{\mu}_i, \boldsymbol{\Sigma}_i\right)
\end{aligned}
$$




$$
\frac{\partial\left(\sum_{l=1}^k \alpha_l-1\right)}{\partial \alpha_i}=\frac{\partial\left(\alpha_1+\alpha_2+\ldots+\alpha_i+\ldots+\alpha_k-1\right)}{\partial \alpha_i}=1
$$


综合两项求导结果, 并令导数等于零即得式(9.37)。

### 9.4.11 式(9.38)的推导

注意, 在"西瓜书"第 14 次印刷中式(9.38)上方的一行话进行了勘误:
"两边同乘以 $\alpha_i$, 对 所有混合成分求和可知 $\lambda=-m$ ", 将原来的
"样本" 修改为 "混合成分"。

对公式(9.37)两边同时乘以$\alpha_{i}$可得 

$$
\begin{aligned}
\sum_{j=1}^m\frac{\alpha_{i}\cdot p(\boldsymbol x_{j}|\boldsymbol\mu_{i},\boldsymbol\Sigma_{i})}{\sum_{l=1}^k\alpha_{l}\cdot p(\boldsymbol x_{j}|\boldsymbol\mu_{l},\boldsymbol\Sigma_{l})}+\lambda\alpha_{i}=0\\
\sum_{j=1}^m\frac{\alpha_{i}\cdot p(\boldsymbol x_{j}|\boldsymbol\mu_{i},\boldsymbol\Sigma_{i})}{\sum_{l=1}^k\alpha_{l}\cdot p(\boldsymbol x_{j}|\boldsymbol\mu_{l},\boldsymbol\Sigma_{l})}=-\lambda\alpha_{i}
\end{aligned}
$$

 两边对所有混合成分求和可得


$$
\begin{aligned}\sum_{i=1}^k\sum_{j=1}^m\frac{\alpha_{i}\cdot p(\boldsymbol x_{j}|\boldsymbol\mu_{i},\boldsymbol\Sigma_{i})}{\sum_{l=1}^k\alpha_{l}\cdot p(\boldsymbol x_{j}|\boldsymbol\mu_{l},\boldsymbol\Sigma_{l})}&=-\lambda\sum_{i=1}^k\alpha_{i}\\
\sum_{j=1}^m\sum_{i=1}^k\frac{\alpha_{i}\cdot p(\boldsymbol x_{j}|\boldsymbol\mu_{i},\boldsymbol\Sigma_{i})}{\sum_{l=1}^k\alpha_{l}\cdot p(\boldsymbol x_{j}|\boldsymbol\mu_{l},\boldsymbol\Sigma_{l})}&=-\lambda\sum_{i=1}^k\alpha_{i}
\end{aligned}
$$

 因为


$$
\sum_{i=1}^k\frac{\alpha_{i}\cdot p(\boldsymbol x_{j}|\boldsymbol\mu_{i},\mathbf\Sigma_{i})}{\sum_{l=1}^k\alpha_{l}\cdot p(\boldsymbol x_{j}|\boldsymbol\mu_{l},\mathbf\Sigma_{l})}=\frac{\sum_{i=1}^k\alpha_{i}\cdot p(\boldsymbol x_{j}|\boldsymbol\mu_{i},\mathbf\Sigma_{i})}{\sum_{l=1}^k\alpha_{l}\cdot p(\boldsymbol x_{j}|\boldsymbol\mu_{l},\mathbf\Sigma_{l})}=1
$$


且 $\sum_{i=1}^k\alpha_{i}=1$，所以有$m=-\lambda$，因此


$$
\sum_{j=1}^m\frac{\alpha_{i}\cdot p(\boldsymbol x_{j}|\boldsymbol\mu_{i},\boldsymbol\Sigma_{i})}{\sum_{l=1}^k\alpha_{l}\cdot p(\boldsymbol x_{j}|\boldsymbol\mu_{l},\boldsymbol\Sigma_{l})}=-\lambda\alpha_{i}=m\alpha_{i}
$$


因此


$$
\alpha_{i}=\cfrac{1}{m}\sum_{j=1}^m\frac{\alpha_{i}\cdot p(\boldsymbol x_{j}|\boldsymbol\mu_{i},\boldsymbol\Sigma_{i})}{\sum_{l=1}^k\alpha_{l}\cdot p(\boldsymbol x_{j}|\boldsymbol\mu_{l},\boldsymbol\Sigma_{l})}
$$


又由公式(9.30)可知$\cfrac{\alpha_{i}\cdot p(\boldsymbol x_{j}|\boldsymbol\mu_{i},\boldsymbol\Sigma_{i})}{\sum_{l=1}^k\alpha_{l}\cdot p(\boldsymbol x_{j}|\boldsymbol\mu_{l},\boldsymbol\Sigma_{l})}=\gamma_{ji}$，所以上式可进一步化简为


$$
\alpha_{i}=\cfrac{1}{m}\sum_{j=1}^m\gamma_{ji}
$$

 此即为公式(9.38)。

### 9.4.12 图9.6的解释

第 1 行初始化参数, 本页接下来的例子是按如下策略初始化的: 混合系数
$\alpha_i=\frac{1}{k}$; 任 选训练集中的 $k$ 个样本分别初始化 $k$
个均值向量 $\boldsymbol{\mu}_i(1 \leqslant i \leqslant k)$;
使用对角元素为 $0.1$ 的对角阵 初始化 $k$ 个协方差矩阵
$\boldsymbol{\Sigma}_i(1 \leqslant i \leqslant k)$ 。

第 3\~5 行根据式(9.30)计算共 $m \times k$ 个 $\gamma_{j i}$ 。

第 6\~10 行分别根据式(9.34)、式(9.35)、式(9.38)使用刚刚计算得到的
$\gamma_{j i}$ 更新均值向量、 协方差矩阵、混合系数; 注意第 8
行计算协方差矩阵时使用的是第 7 行计算得到的均值向量, 这并没错,
因为协方差矩阵 $\boldsymbol{\Sigma}_i^{\prime}$ 与均值向量
$\mu_i^{\prime}$ 是对应的, 而非 $\boldsymbol{\mu}_i$; 第 7 行的
$\mu_i^{\prime}$ 在第 8 行使用 之后会在下一轮迭代中第 4 行计算
$\gamma_{j i}$ 再次使用。

整体来说, 第 2 \~12 行就是一个 EM 算法的具体使用例子, 学习完 $7.6$ 节 EM
算法可能 根本无法理解其思想。此例中有两组变量, 分别是 $\gamma_{j i}$ 和
$\left(\alpha_i, \boldsymbol{\mu}_i, \boldsymbol{\Sigma}_i\right)$,
它们之间相互影响, 但都是末知的, 因此 $\mathrm{EM}$ 算法就有了用武之地:
初始化其中一组变量
$\left(\alpha_i, \boldsymbol{\mu}_i, \boldsymbol{\Sigma}_i\right)$,
然后计 算 $\gamma_{j i}$; 再根据 $\gamma_{j i}$
根据最大似然推导出的公式更新
$\left(\alpha_i, \boldsymbol{\mu}_i, \boldsymbol{\Sigma}_i\right)$,
反复迭代, 直到满足停止条件。

## 9.5 密度聚类

本节介绍的DBSCAN算法并不难懂，只是本节在最后举例时并没有说清楚密度聚类算法与前面原型聚类算法的区别，当然这也可能是作者有意为之，因为在"西瓜书"本章习题9.7题就提到了"凸聚类"的概念。具体来说，前面介绍的聚类算法只能产生"凸聚类"，而本节介绍的DBSCAN则能产生"非凸聚类"，其本质原因，个人感觉在于聚类时使用的距离度量，均值算法使用欧氏距离，而DBSCAN使用类似于测地线距离（只是类似，并不相同，测地线距离参见"西瓜书"10.5节），因此可以产生如图9-1所示的聚类结果（中间为典型的非凸聚类）。

![图9-1 DBSCAN聚类结果](https://datawhale-business.oss-cn-hangzhou.aliyuncs.com/1cc9980e-da9c-4efd-ac66-3a97aa81ae62-cluster.svg)

注意，虽然左图为 "凸聚类"（四个簇都有一个凸包），但
均值算法却无法产生此结果，因为最大的簇太大了，其外围样本与另三个小簇的中心之间的距离更近，因此中间最大的簇肯定会被
均值算法划分到不同的簇之中，这显然不是我们希望的结果。

密度聚类算法可以产生任意形状的簇，不需要事先指定聚类个数k，并且对噪声鲁棒。

### 9.5.1 密度直达、密度可达与密度相连

$\boldsymbol{x}_j$ 由 $\boldsymbol{x}_i$ 密度直达, 该概念最易理解,
但要特别注意: 密度直达除了要求 $\boldsymbol{x}_j$ 位于
$\boldsymbol{x}_i$ 的 $\epsilon-$ 领域的条件之外, 还额外要求
$\boldsymbol{x}_i$ 是核心对象; $\epsilon$-领域满足对称性, 但
$\boldsymbol{x}_j$ 不一定为核心对象, 因此密度直达关系通常不满足对称性。

$\boldsymbol{x}_j$ 由 $\boldsymbol{x}_i$
密度可达，该概念基于密度直达，因此样本序列
$\boldsymbol{p}_1, \boldsymbol{p}_2, \ldots, \boldsymbol{p}_n$ 中除了
$\boldsymbol{p}_n=\boldsymbol{x}_j$ 之外, 其余样本均为核心对象 (当然包括
$\boldsymbol{p}_1=\boldsymbol{x}_i$ ), 所以同理, 一般不满足对称性。

以上两个概念中, 若 $\boldsymbol{x}_j$ 为核心对象, 已知
$\boldsymbol{x}_j$ 由 $\boldsymbol{x}_i$ 密度直达/可达, 则
$\boldsymbol{x}_i$ 由 $\boldsymbol{x}_j$ 密度直达/ 可达, 即满足对称性
(也就是说, 核心对象之间的密度直达/可达满足对称性)。

$\boldsymbol{x}_i$ 与 $\boldsymbol{x}_j$ 密度相连, 不要求
$\boldsymbol{x}_i$ 与 $\boldsymbol{x}_j$ 为核心对象, 所以满足对称性。

### 9.5.2 图9.9的解释

在第 $1\sim 7$ 行中, 算法先根据给定的邻域参数 $(\epsilon, M i n P t s)$
找出所有核心对象, 并存于集 合 $\Omega$ 之中; 第 4 行的 if
判断语句即在判别 $\boldsymbol{x}_j$ 是否为核心对象。

在第 $10\sim 24$ 行中, 以任一核心对象为出发点 (由第 12 行实现),
找出其密度可达的样本 生成聚类簇 (由第 $14\sim 21$ 行实现),
直到所有核心对象被访问过为止（由第 10 行和第 23 行 配合实现)。具体来说:

其中第 $14\sim 21$ 行 while 循环中的 if 判断语句（第 16
行）在第一次循环时一定为真（因 为 $Q$ 在第 12 行初始化为某核心对象),
此时会往队列 $Q$ 中加入 $\boldsymbol{q}$ 密度直达的样本 (已知
$\boldsymbol{q}$ 为核 心对象, $\boldsymbol{q}$ 的
$\epsilon$-领域中的样本即为 $\boldsymbol{q}$ 密度直达的),
队列遵循先进先出规则, 接下来的循环将 依次判别 $\boldsymbol{q}$ 的
$\epsilon$-领域中的样本是否为核心对象 (第 16 行), 若为核心对象,
则将密度直达的 样本 ( $\epsilon$-领域中的样本) 加入
Q。根据密度可达的概念, while 循环中的 if 判断语句（第 16
行）找出的核心对象之间一定是相互密度可达的, 非核心对象一定是密度相连的。

第 $14\sim 21$ 行 while 循环每跳出一次,
即生成一个聚类簇。每次生成聚类笶之前, 会记录 当前末访问过样本集合 (第 11
行 $\Gamma_{\mathrm{old}}=\Gamma$ ),
然后当前要生成的聚类簇每决定录取一个样 本后会将该样本从厂去除 (第 13
行和第 19 行), 因此第 14\~21 行 while 循环每跳出一次后,
$\Gamma_{\text {old }}$ 与 $\Gamma$ 差别即为聚类簇的样本成员 (第 22 行),
并将该聚类簇中的核心对象从第 $1 \sim 7$ 行生 成的核心对象集合 $\Omega$
中去除。

符号 " $\backslash$" 为集合求差, 例如集合
$A=\{a, b, c, d, e, f\}, B=\{a, d, f, g, h\}$, 则 $A \backslash B$ 为
$A \backslash B=\{b, c, e\}$, 即将 $A, B$ 所有相同元素从 $A$ 中去除。

## 9.6 层次聚类

本节主要介绍了层次聚类的代表算法 AGNES。

式(9.41) (9.43)介绍了三种距离计算方式,
这与"西瓜书"9.3节中介绍的距离不同之处在于, 此 三种距离计算面向集合之间,
而9.3节的距离则面向两点之间。正如"西瓜书"第 215 页左上边注所示,
集合间的距离计算常采用豪斯多夫距离(Hausdorff distance)。

算法 AGNES 很简单,
就是不断重复执行合并距离最近的两个聚类簇。"西瓜书"图9.11为具体实 现方法,
核心就是在合并两个聚类簇后更新距离矩阵（第 $11\sim 23$ 行),
之所以看起来复杂, 是因为该实现只更新原先距离矩阵中发生变化的行和列,
因此需要为此做一些调整。

在第 $1\sim 9$ 行,
算法先对仅含一个样本的初始聚类簇和相应的距离矩阵进行初始化。注意,
距离矩阵中, 第 $i$ 行为聚类簇 $C_i$ 到各聚类簇的距离, 第 $i$
列为各聚类簇到聚类簇 $C_i$ 的距离, 由 第 7 行可知, 距离矩阵为对称矩阵,
即使用的集合间的距离计算方法满足对称性。

第 $18\sim 21$ 行更新距离矩阵 $M$ 的第 $i^*$ 行与第 $i^*$ 列,
因为此时的聚类簇 $C_{i^*}$ 已经合并了 $C_{j^*}$,
因此与其余聚类簇之间的距离都发生了变化, 需要更新。
