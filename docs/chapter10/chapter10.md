```{important}
参与组队学习的同学须知：

本章学习时间：3天

本章配套视频教程：

https://www.bilibili.com/video/BV1Mh411e7VU?p=15

https://www.bilibili.com/video/BV1Mh411e7VU?p=16
```

# 第10章 降维与度量学习

## 10.1 预备知识

本章内容需要较多的线性代数和矩阵分析的基础，因此将相关的预备知识整体整理如下。

### 10.1.1 符号约定

向量元素之间分号 ";" 表示列元素分隔符, 如
$\boldsymbol{\alpha}=\left(a_1 ; a_2 ; \ldots ; a_i ; \ldots ; a_m\right)$
表示 $m \times 1$ 的列向量; 而逗号 "," 表示行元素分隔符, 如
$\boldsymbol{\alpha}=\left(a_1, a_2, \ldots, a_i, \ldots, a_m\right)$
表示 $1 \times m$ 的 行向量。

### 10.1.2 矩阵与单位阵、向量的乘法

(1)矩阵左乘对角阵相当于矩阵每行乘以对应对角阵的对角线元素, 如：


$$
\left[\begin{array}{lll}
\lambda_1 & & \\
& \lambda_2 & \\
& & \lambda_3
\end{array}\right]\left[\begin{array}{lll}
x_{11} & x_{12} & x_{13} \\
x_{21} & x_{22} & x_{23} \\
x_{31} & x_{32} & x_{33}
\end{array}\right]=\left[\begin{array}{lll}
\lambda_1 x_{11} & \lambda_1 x_{12} & \lambda_1 x_{13} \\
\lambda_2 x_{21} & \lambda_2 x_{22} & \lambda_2 x_{23} \\
\lambda_3 x_{31} & \lambda_3 x_{32} & \lambda_3 x_{33}
\end{array}\right]
$$



(2)矩阵右乘对角阵相当于矩阵每列乘以对应对角阵的对角线元素, 如：


$$
\left[\begin{array}{lll}
x_{11} & x_{12} & x_{13} \\
x_{21} & x_{22} & x_{23} \\
x_{31} & x_{32} & x_{33}
\end{array}\right]\left[\begin{array}{llll}
\lambda_1 & & \\
& \lambda_2 & \\
& & \lambda_3
\end{array}\right]=\left[\begin{array}{lll}
\lambda_1 x_{11} & \lambda_2 x_{12} & \lambda_3 x_{13} \\
\lambda_1 x_{21} & \lambda_2 x_{22} & \lambda_3 x_{23} \\
\lambda_1 x_{31} & \lambda_2 x_{32} & \lambda_3 x_{33}
\end{array}\right]
$$



(3)矩阵左乘行向量相当于矩阵每行乘以对应行向量的元素之和, 如：


$$
\begin{aligned}
& {\left[\begin{array}{lll}
\lambda_1 & \lambda_2 & \lambda_3
\end{array}\right]\left[\begin{array}{lll}
x_{11} & x_{12} & x_{13} \\
x_{21} & x_{22} & x_{23} \\
x_{31} & x_{32} & x_{33}
\end{array}\right]} \\
& =\lambda_1\left[\begin{array}{llll}
x_{11} & x_{12} & x_{13}
\end{array}\right]+\lambda_2\left[\begin{array}{lll}
x_{21} & x_{22} & x_{23}
\end{array}\right]+\lambda_3\left[\begin{array}{lll}
x_{31} & x_{32} & x_{33}
\end{array}\right] \\
& =\left(\begin{array}{ll}
\lambda_1 x_{11}+\lambda_2 x_{21}+\lambda_3 x_{31}, \lambda_1 x_{12}+\lambda_2 x_{22}+\lambda_3 x_{32}, \lambda_1 x_{13}+\lambda_2 x_{23}+\lambda_3 x_{33}
\end{array}\right)
\end{aligned}
$$



(4)矩阵右乘列向量相当于矩阵每列乘以对应列向量的元素之和, 如：


$$
\begin{aligned}
& {\left[\begin{array}{lll}
x_{11} & x_{12} & x_{13} \\
x_{21} & x_{22} & x_{23} \\
x_{31} & x_{32} & x_{33}
\end{array}\right]\left[\begin{array}{l}
\lambda_1 \\
\lambda_2 \\
\lambda_3
\end{array}\right]} \\
& =\lambda_1\left[\begin{array}{l}
x_{11} \\
x_{21} \\
x_{31}
\end{array}\right]+\lambda_2\left[\begin{array}{l}
x_{12} \\
x_{22} \\
x_{32}
\end{array}\right]+\lambda_3\left[\begin{array}{l}
x_{13} \\
x_{23} \\
x_{33}
\end{array}\right]=\sum_{i=1}^3\left(\lambda_i\left[\begin{array}{l}
x_{1 i} \\
x_{2 i} \\
x_{3 i}
\end{array}\right]\right) \\
& =\left(\lambda_1 x_{11}+\lambda_2 x_{12}+\lambda_3 x_{13} ; \lambda_1 x_{21}+\lambda_2 x_{22}+\lambda_3 x_{23} ; \lambda_1 x_{31}+\lambda_2 x_{32}+\lambda_3 x_{33}\right)
\end{aligned}
$$



综上, 左乘是对矩阵的行操作, 而右乘则是对矩阵的列操作,
第(2)个和第(4)个结论后 面推导过程中灵活应用较多。

## 10.2 矩阵的F范数与迹

(1)对于矩阵 $\mathbf{A} \in \mathbb{R}^{m \times n}$, 其 Frobenius 范数
(简称 $\mathrm{F}$ 范数) $\|\mathbf{A}\|_F$ 定义为


$$
\|\mathbf{A}\|_F=\left(\sum_{i=1}^m \sum_{j=1}^n\left|a_{i j}\right|^2\right)^{\frac{1}{2}}
$$



其中 $a_{i j}$ 为矩阵 $\mathrm{A}$ 第 $i$ 行第 $j$ 列的元素, 即


$$
\mathbf{A}=\left[\begin{array}{cccccc}
a_{11} & a_{12} & \cdots & a_{1 j} & \cdots & a_{1 n} \\
a_{21} & a_{22} & \cdots & a_{2 j} & \cdots & a_{2 n} \\
\vdots & \vdots & \ddots & \vdots & \ddots & \vdots \\
a_{i 1} & a_{i 2} & \cdots & a_{i j} & \cdots & a_{i n} \\
\vdots & \vdots & \ddots & \vdots & \ddots & \vdots \\
a_{m 1} & a_{m 2} & \cdots & a_{m j} & \cdots & a_{m n}
\end{array}\right]
$$



(2)若
$\mathbf{A}=\left(\boldsymbol{\alpha}_1, \boldsymbol{\alpha}_2, \ldots, \boldsymbol{\alpha}_j, \ldots, \boldsymbol{\alpha}_n\right)$,
其中
$\boldsymbol{\alpha}_j=\left(a_{1 j} ; a_{2 j} ; \ldots ; a_{i j} ; \ldots ; a_{m j}\right)$
为其列向量,
$\mathbf{A} \in \mathbb{R}^{m \times n}, \boldsymbol{\alpha}_j \in \mathbb{R}^{m \times 1}$,
则
$\|\mathbf{A}\|_F^2=\sum_{j=1}^n\left\|\boldsymbol{\alpha}_j\right\|_2^2$;

同理, 若
$\mathbf{A}=\left(\boldsymbol{\beta}_1 ; \boldsymbol{\beta}_2 ; \ldots ; \boldsymbol{\beta}_i ; \ldots ; \boldsymbol{\beta}_m\right)$,
其中
$\boldsymbol{\beta}_i=\left(a_{i 1}, a_{i 2}, \ldots, a_{i j}, \ldots, a_{i n}\right)$
为其行向量,
$\mathbf{A} \in \mathbb{R}^{m \times n}, \boldsymbol{\beta}_i \in \mathbb{R}^{1 \times n}$,
则
$\|\mathbf{A}\|_F^2=\sum_{i=1}^m\left\|\boldsymbol{\beta}_i\right\|_2^2$
。

证明： 该结论是显而易见的, 因为
$\left\|\boldsymbol{\alpha}_j\right\|_2^2=\sum_{i=1}^m\left|a_{i j}\right|^2$,
而
$\|\mathbf{A}\|_F^{2}=\sum_{i=1}^m \sum_{j=1}^n\left|a_{i j}\right|^2$
。

(3)若 $\lambda_j\left(\mathbf{A}^{\top} \mathbf{A}\right)$ 表示 $n$
阶方阵 $\mathbf{A}^{\top} \mathbf{A}$ 的第 $j$ 个特征值,
$\operatorname{tr}\left(\mathbf{A}^{\top} \mathbf{A}\right)$ 是
$\mathbf{A}^{\top} \mathbf{A}$ 的迹（对角线 元素之和);
$\lambda_i\left(\mathbf{A A}^{\top}\right)$ 表示 $m$ 阶方阵
$\mathbf{A} \mathbf{A}^{\top}$ 的第 $i$ 个特征值,
$\operatorname{tr}\left(\mathbf{A A}^{\top}\right)$ 是
$\mathbf{A} \mathbf{A}^{\top}$ 的迹, 则 

$$
\begin{aligned}
\|\mathbf{A}\|_F^2 & =\operatorname{tr}\left(\mathbf{A}^{\top} \mathbf{A}\right)=\sum_{j=1}^n \lambda_j\left(\mathbf{A}^{\top} \mathbf{A}\right) \\
& =\operatorname{tr}\left(\mathbf{A} \mathbf{A}^{\top}\right)=\sum_{i=1}^m \lambda_i\left(\mathbf{A} \mathbf{A}^{\top}\right)
\end{aligned}
$$



证明： 先证
$\|\mathbf{A}\|_F^2=\operatorname{tr}\left(\mathbf{A}^{\top} \mathbf{A}\right)$，令
$\mathbf{B}=\mathbf{A}^{\top} \mathbf{A} \in \mathbb{R}^{n \times n}, b_{i j}$
表示 $\mathbf{B}$ 第 $i$ 行第 $j$ 列元素,
$\operatorname{tr}(\mathbf{B})=\sum_{j=1}^n b_{j \jmath}$


$$
\mathbf{B}=\mathbf{A}^{\top} \mathbf{A}=\left[\begin{array}{cccccc}
a_{11} & a_{21} & \cdots & a_{i 1} & \cdots & a_{m 1} \\
a_{12} & a_{22} & \cdots & a_{i 2} & \cdots & a_{m 2} \\
\vdots & \vdots & \ddots & \vdots & \ddots & \vdots \\
a_{1 j} & a_{2 j} & \cdots & a_{i j} & \cdots & a_{m j} \\
\vdots & \vdots & \ddots & \vdots & \ddots & \vdots \\
a_{1 n} & a_{2 n} & \cdots & a_{i n} & \cdots & a_{m n}
\end{array}\right]\left[\begin{array}{cccccc}
a_{11} & a_{12} & \cdots & a_{1 j} & \cdots & a_{1 n} \\
a_{21} & a_{22} & \cdots & a_{2 j} & \cdots & a_{2 n} \\
\vdots & \vdots & \ddots & \vdots & \ddots & \vdots \\
a_{i 1} & a_{i 2} & \cdots & a_{i j} & \cdots & a_{i n} \\
\vdots & \vdots & \ddots & \vdots & \ddots & \vdots \\
a_{m 1} & a_{m 2} & \cdots & a_{m j} & \cdots & a_{m n}
\end{array}\right]
$$

 由矩阵运算规则, $b_{j j}$ 等于 $\mathbf{A}^{\top}$
的第 $j$ 行与 $\mathbf{A}$ 的第 $j$ 列的内积, 因此


$$
\operatorname{tr}(\mathbf{B})=\sum_{j=1}^n b_{j j}=\sum_{j=1}^n\left(\sum_{i=1}^m\left|a_{i j}\right|^2\right)=\sum_{i=1}^m \sum_{j=1}^n\left|a_{i j}\right|^2=\|\mathbf{A}\|_F^2
$$



以上第三个等号交换了求和号次序（类似于交换积分号次序),
显然这不影响求和结果。

同理, 可证
$\|\mathbf{A}\|_F^2=\operatorname{tr}\left(\mathbf{A} \mathbf{A}^{\top}\right)$
： 

$$
\begin{aligned}
& \mathbf{C}=\mathbf{A A}^{\top}=\left[\begin{array}{cccccc}
a_{11} & a_{12} & \cdots & a_{1 j} & \cdots & a_{1 n} \\
a_{21} & a_{22} & \cdots & a_{2 j} & \cdots & a_{2 n} \\
\vdots & \vdots & \ddots & \vdots & \ddots & \vdots \\
a_{i 1} & a_{i 2} & \cdots & a_{i j} & \cdots & a_{i n} \\
\vdots & \vdots & \ddots & \vdots & \ddots & \vdots \\
a_{m 1} & a_{m 2} & \cdots & a_{m j} & \cdots & a_{m n}
\end{array}\right]\left[\begin{array}{cccccc}
a_{11} & a_{21} & \cdots & a_{i 1} & \cdots & a_{m 1} \\
a_{12} & a_{22} & \cdots & a_{i 2} & \cdots & a_{m 2} \\
\vdots & \vdots & \ddots & \vdots & \ddots & \vdots \\
a_{1 j} & a_{2 j} & \cdots & a_{i j} & \cdots & a_{m j} \\
\vdots & \vdots & \ddots & \vdots & \ddots & \vdots \\
a_{1 n} & a_{2 n} & \cdots & a_{i n} & \cdots & a_{m n}
\end{array}\right] \\
&
\end{aligned}
$$



由矩阵运算规则, $c_{i i}$ 等于 $\mathbf{A}$ 的第 $i$ 行与
$\mathbf{A}^{\top}$ 的第 $i$ 列的内积（红色元素), 因此


$$
\operatorname{tr}(\mathbf{C})=\sum_{i=1}^m c_{i i}=\sum_{i=1}^m\left(\sum_{j=1}^n\left|a_{i j}\right|^2\right)=\sum_{i=1}^m \sum_{j=1}^n\left|a_{i j}\right|^2=\|\mathbf{A}\|_F^2
$$



有关方阵的特征值之和等于对角线元素之和, 可以参见线性代数教材。

## 10.3 k近邻学习

### 10.3.1 式(10.1)的解释



$$
P(e r r)=1-\sum_{c \in \mathcal{Y}} P(c | \boldsymbol{x}) P(c | \boldsymbol{z})
$$


首先, $P(c \mid \boldsymbol{x})$ 表示样本 $\boldsymbol{x}$ 为类别 $c$
的后验概率, $P(c \mid \boldsymbol{z})$ 表示样本 $\boldsymbol{z}$ 为类别
$c$ 的后验概率; 其次,
$P(c \mid \boldsymbol{x}) P(c \mid \boldsymbol{z})$ 表示样本
$\boldsymbol{x}$ 和样本 $\boldsymbol{z}$ 同时为类别 $c$ 的概率;

再次,
$\sum_{c \in \mathcal{Y}} P(c \mid \boldsymbol{x}) P(c \mid \boldsymbol{z})$
表示样本 $\boldsymbol{x}$ 和样本 $\boldsymbol{z}$ 类别相同的概率;
这一点可以进一步解 释，设
$\mathcal{Y}=\left\{c_1, c_2, \cdots, c_N\right\}$, 则该求和式子变为：


$$
P\left(c_1 \mid \boldsymbol{x}\right) P\left(c_1 \mid \boldsymbol{z}\right)+P\left(c_2 \mid \boldsymbol{x}\right) P\left(c_2 \mid \boldsymbol{z}\right)+\cdots+P\left(c_N \mid \boldsymbol{x}\right) P\left(c_N \mid \boldsymbol{z}\right)
$$



即样本 $\boldsymbol{x}$ 和样本 $\boldsymbol{z}$ 同时为 $c_1$ 的概率,
加上同时为 $c_2$ 的概率, $\cdots \cdots$, 加上同时为 $c_N$ 的概率, 即
样本 $\boldsymbol{x}$ 和样本 $\boldsymbol{z}$ 类别相同的概率;

最后, $P(e r r)$ 表示样本 $\boldsymbol{x}$ 和样本 $\boldsymbol{z}$
类别不相同的概率, 即 1 减去二者类别相同的概率。

### 10.3.2 式(10.2)的推导

式(10.2)推导关键在于理解第二行的 "约等 $(\simeq)$ " 关系和第三行的
"小于等于 $(\leqslant)$ " 关系。

第二行的 "约等 $(\simeq)$ " 关系的依据在于该式前面一段话：
"假设样本独立同分布, 且对 任意 $\boldsymbol{x}$ 和任意小正数 $\delta$,
在 $\boldsymbol{x}$ 附近 $\delta$ 距离范围内总能找到一个训练样本",
这意味着对于任意 测试样本在训练集中都可以找出一个与其非常像 (任意小正数
$\delta$ ) 的近邻, 这里还有一个假 设书中末提及：
$P(c \mid \boldsymbol{x})$ 必须是连续函数 (对于连续函数 $f(x)$
和任意小正数 $\delta$, $f(x) \simeq f(x+\delta))$,
即对于两个非常像的样本 $\boldsymbol{z}$ 与 $\boldsymbol{x}$ 有
$P(c \mid \boldsymbol{x}) \simeq P(c \mid \boldsymbol{z})$, 即


$$
\sum_{c \in \mathcal{Y}} P(c \mid \boldsymbol{x}) P(c \mid \boldsymbol{z}) \simeq \sum_{c \in \mathcal{Y}} P^2(c \mid \boldsymbol{x})
$$



第三行的"小于等于 $(\leqslant)$ " 关系更简单： 由于
$c^* \in \mathcal{Y}$, 所以
$P^2\left(c^* \mid \boldsymbol{x}\right) \leqslant \sum_{c \in \mathcal{Y}} P^2(c \mid \boldsymbol{x})$,
也就是 "小于等于 $(\leqslant)$ " 左边只是右边的一部分,
所以肯定是小于等于的关系;

第四行就是数学公式 $a^2-b^2=(a+b)(a-b)$；

第五行是由于 $1+P\left(c^* \mid \boldsymbol{x}\right) \leqslant 2$,
这是由于概率值 $P\left(c^* \mid \boldsymbol{x}\right) \leqslant 1$

经过以上推导, 本节最后给出一个惊人的结论： 最近邻分类器虽简单,
但它的泛化错误 率不超过贝叶斯最优分类器的错误率的两倍!

然而这是一个没啥实际用途的结论, 因为这个结论必须满足两个假设条件, 且不说
$P(c \mid \boldsymbol{x})$ 是连续函数（第一个假设）是否满足, 单就
"对任意 $\boldsymbol{x}$ 和任意小正数 $\delta$, 在 $\boldsymbol{x}$ 附近
$\delta$ 距 离范围内总能找到一个训练样本" (第二个假设) 是不可能满足的,
这也就有了 $10.2$ 节开头 一段的讨论, 抛开 "任意小正数 $\delta$ " 不谈,
具体到 $\delta=0.001$ 都是不现实的。

## 10.4 低维嵌入

### 10.4.1 图10.2的解释

只要注意一点就行：在图(a)三维空间中，红色线是弯曲的，但去掉高度这一维（竖着的坐标轴）后，红色线变成直线，而直线更容易学习。

### 10.4.2 式(10.3)的推导

已知
$\mathbf{Z}=\left\{\boldsymbol{z}_1, \boldsymbol{z}_2, \ldots, \boldsymbol{z}_i, \ldots, \boldsymbol{z}_m\right\} \in \mathbb{R}^{d^{\prime} \times m}$,
其中
$\boldsymbol{z}_i=\left(z_{i 1} ; z_{i 2} ; \ldots ; z_{i d^{\prime}}\right) \in \mathbb{R}^{d^{\prime} \times 1}$;
降维后的内积矩阵
$\mathbf{B}=\mathbf{Z}^{\top} \mathbf{Z} \in \mathbb{R}^{m \times m}$,
其中第 $i$ 行第 $j$ 列元素 $b_{i j}$, 特别的


$$
b_{i i}=\boldsymbol{z}_i^{\top} \boldsymbol{z}_i=\left\|\boldsymbol{z}_i\right\|^2, b_{j j}=\boldsymbol{z}_j^{\top} \boldsymbol{z}_j=\left\|\boldsymbol{z}_j\right\|^2, b_{i j}=\boldsymbol{z}_i^{\top} \boldsymbol{z}_j
$$


MDS 算法的目标是
$\left\|\boldsymbol{z}_i-\boldsymbol{z}_j\right\|=d i s t_{i j}=\left\|\boldsymbol{x}_i-\boldsymbol{x}_j\right\|$,
即保持样本的欧氏距离在 $d^{\prime}$ 维空 间和原始 $d$ 维空间相同
$\left(d^{\prime} \leqslant d\right)$ 。 

$$
\begin{aligned}
d i s t_{i j}^2 & =\left\|\boldsymbol{z}_i-\boldsymbol{z}_j\right\|^2=\left(z_{i 1}-z_{j 1}\right)^2+\left(z_{i 2}-z_{j 2}\right)^2+\ldots+\left(z_{i d^{\prime}}-z_{j d^{\prime}}\right)^2 \\
& =\left(z_{i 1}^2-2 z_{i 1} z_{j 1}+z_{j 1}^2\right)+\left(z_{i 2}^2-2 z_{i 2} z_{j 2}+z_{j 2}^2\right)+\ldots+\left(z_{i d^{\prime}}^2-2 z_{i d^{\prime}} z_{j d^{\prime}}+z_{j d^{\prime}}^2\right) \\
& =\left(z_{i 1}^2+z_{i 2}^2+\ldots+z_{i d^{\prime}}^2\right)+\left(z_{j 1}^2+z_{j 2}^2+\ldots+z_{j d^{\prime}}^2\right) \\
& -2\left(z_{i 1} z_{j 1}+z_{i 2} z_{j 2}+\ldots+z_{i d^{\prime}} z_{j d^{\prime}}\right) \\
& =\left\|\boldsymbol{z}_i\right\|^2+\left\|\boldsymbol{z}_j\right\|^2-2 \boldsymbol{z}_i^{\top} \boldsymbol{z}_j \\
& =b_{i i}+b_{j j}-2 b_{i j}
\end{aligned}
$$



本章矩阵运算非常多, 刚刚是从矩阵元素层面的推导;
实际可发现上式运算结果基本与 标量运算规则相同,
因此后面会尽可能不再从元素层面推导。具体来说： 

$$
\begin{aligned}
d i s t_{i j}^2 & =\left\|\boldsymbol{z}_i-\boldsymbol{z}_j\right\|^2=\left(\boldsymbol{z}_i-\boldsymbol{z}_j\right)^{\top}\left(\boldsymbol{z}_i-\boldsymbol{z}_j\right) \\
& =\boldsymbol{z}_i^{\top} \boldsymbol{z}_i-\boldsymbol{z}_i^{\top} \boldsymbol{z}_j-\boldsymbol{z}_j^{\top} \boldsymbol{z}_i+\boldsymbol{z}_j^{\top} \boldsymbol{z}_j \\
& =\boldsymbol{z}_i^{\top} \boldsymbol{z}_i+\boldsymbol{z}_j^{\top} \boldsymbol{z}_j-2 \boldsymbol{z}_i^{\top} \boldsymbol{z}_j \\
& =\left\|\boldsymbol{z}_i\right\|^2+\left\|\boldsymbol{z}_j\right\|^2-2 \boldsymbol{z}_i^{\top} \boldsymbol{z}_j \\
& =b_{i i}+b_{j j}-2 b_{i j}
\end{aligned}
$$

 上式第三个等号化简是由于内积
$\boldsymbol{z}_i^{\top} \boldsymbol{z}_j$ 和
$\boldsymbol{z}_j^{\top} \boldsymbol{z}_i$ 均为标量, 因此转置等于本身。

### 10.4.3 式(10.4)的推导

首先解释两个条件：

(1)令降维后的样本Z被中心化, 即
$\sum_{i=1}^m \boldsymbol{z}_i=\mathbf{0}$ 注意
$\mathbf{Z} \in \mathbb{R}^{d^{\prime} \times m}$, $d^{\prime}$
是样本维度 (属性个数), $m$ 是样本个数, 易知 $\mathbf{Z}$ 的每一行有 $m$
个元 素 (每行表示样本集的一维属性), Z的每一列有 $d^{\prime}$ 个元素
(每列表示一个样本)。

式 $\sum_{i=1}^m \boldsymbol{z}_i=\mathbf{0}$ 中的 $\boldsymbol{z}_i$
明显表示的是第 $i$ 列, $m$ 列相加得到一个零向量
$\mathbf{0}_{d^{\prime} \times 1}$, 意思是样
本集合中所有样本的每一维属性之和均等于 0 ,
因此被中心化的意思是将样本集合Z的每一 行（属性）减去该行的均值。

(2)显然, 矩阵 $\mathbf{B}$ 的行与列之各均为零, 即
$\sum_{i=1}^m b_{i j}=\sum_{j=1}^m b_{i j}=0$ 。

注意 $b_{i j}=\boldsymbol{z}_i^{\top} \boldsymbol{z}_j$ (也可以写为
$b_{i j}=\boldsymbol{z}_j^{\top} \boldsymbol{z}_i$,
其实就是对应元素相乘, 再求和) 

$$
\begin{gathered}
\sum_{i=1}^m b_{i j}=\sum_{i=1}^m \boldsymbol{z}_j^{\top} \boldsymbol{z}_i=\boldsymbol{z}_j^{\top} \sum_{i=1}^m \boldsymbol{z}_i=\boldsymbol{z}_j^{\top} \cdot \mathbf{0}_{d^{\prime} \times 1}=0 \\
\sum_{j=1}^m b_{i j}=\sum_{j=1}^m \boldsymbol{z}_i^{\top} \boldsymbol{z}_j=\boldsymbol{z}_i^{\top} \sum_{j=1}^m \boldsymbol{z}_j=\boldsymbol{z}_i^{\top} \cdot \mathbf{0}_{d^{\prime} \times 1}=0
\end{gathered}
$$



接下来我们推导式(10.4), 将式(10.3)的 $d i s t_{i j}^2$ 表达式代入：


$$
\begin{aligned}
\sum_{i=1}^m d i s t_{i j}^2 & =\sum_{i=1}^m\left(\left\|\boldsymbol{z}_i\right\|^2+\left\|\boldsymbol{z}_j\right\|^2-2 \boldsymbol{z}_i^{\top} \boldsymbol{z}_j\right) \\
& =\sum_{i=1}^m\left\|\boldsymbol{z}_i\right\|^2+\sum_{i=1}^m\left\|\boldsymbol{z}_j\right\|^2-2 \sum_{i=1}^m \boldsymbol{z}_i^{\top} \boldsymbol{z}_j
\end{aligned}
$$



根据定义： 

$$
\begin{aligned}
& \sum_{i=1}^m\left\|\boldsymbol{z}_i\right\|^2=\sum_{i=1}^m \boldsymbol{z}_i^{\top} \boldsymbol{z}_i=\sum_{i=1}^m b_{i i}=\operatorname{tr}(\mathbf{B}) \\
& \sum_{i=1}^m\left\|\boldsymbol{z}_j\right\|^2=\left\|\boldsymbol{z}_j\right\|^2 \sum_{i=1}^m 1=m\left\|\boldsymbol{z}_j\right\|^2=m \boldsymbol{z}_j^{\top} \boldsymbol{z}_j=m b_{j j}
\end{aligned}
$$



根据前面结果：


$$
\sum_{i=1}^m \boldsymbol{z}_i^{\top} \boldsymbol{z}_j=\left(\sum_{i=1}^m \boldsymbol{z}_i^{\top}\right) \boldsymbol{z}_j=\mathbf{0}_{1 \times d^{\prime}} \cdot \boldsymbol{z}_j=0
$$



代入上式即得： 

$$
\begin{aligned}
\sum_{i=1}^m d i s t_{i j}^2 & =\sum_{i=1}^m\left\|\boldsymbol{z}_i\right\|^2+\sum_{i=1}^m\left\|\boldsymbol{z}_j\right\|^2-2 \sum_{i=1}^m \boldsymbol{z}_i^{\top} \boldsymbol{z}_j \\
& =\operatorname{tr}(\mathbf{B})+m b_{j j}
\end{aligned}
$$



### 10.4.4 式(10.5)的推导

与式(10.4)类似： 

$$
\begin{aligned}
\sum_{j=1}^m d i s t_{i j}^2 & =\sum_{j=1}^m\left(\left\|\boldsymbol{z}_i\right\|^2+\left\|\boldsymbol{z}_j\right\|^2-2 \boldsymbol{z}_i^{\top} \boldsymbol{z}_j\right) \\
& =\sum_{j=1}^m\left\|\boldsymbol{z}_i\right\|^2+\sum_{j=1}^m\left\|\boldsymbol{z}_j\right\|^2-2 \sum_{j=1}^m \boldsymbol{z}_i^{\top} \boldsymbol{z}_j \\
& =m b_{i i}+\operatorname{tr}(\mathbf{B})
\end{aligned}
$$



### 10.4.5 式(10.6)的推导



$$
\begin{aligned}
\sum_{i=1}^{m} \sum_{j=1}^{m} \operatorname{dist}_{i j}^{2} &=\sum_{i=1}^{m} \sum_{j=1}^{m}\left(\left\|z_{i}\right\|^{2}+\left\|\boldsymbol{z}_{j}\right\|^{2}-2 \boldsymbol{z}_{i}^{\top} \boldsymbol{z}_{j}\right) \\
&=\sum_{i=1}^{m} \sum_{j=1}^{m}\left\|\boldsymbol{z}_{i}\right\|^{2}+\sum_{i=1}^{m} \sum_{j=1}^{m}\left\|\boldsymbol{z}_{j}\right\|^{2}-2 \sum_{i=1}^{m} \sum_{j=1}^{m} \boldsymbol{z}_{i}^{\top} \boldsymbol{z}_{j} \\
\end{aligned}
$$



其中各子项的推导如下：


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

### 10.4.6 式(10.10)的推导

由式(10.3)可得 

$$
b_{ij}=-\frac{1}{2}(dist^2_{ij}-b_{ii}-b_{jj})
$$


由式(10.6)和(10.9)可得 

$$
\begin{aligned}
tr(\boldsymbol B)&=\frac{1}{2m}\sum^m_{i=1}\sum^m_{j=1}dist^2_{ij}\\
&=\frac{m}{2}dist^2_{\cdot}
\end{aligned}
$$



由式(10.4)和(10.8)可得 

$$
\begin{aligned}
b_{jj}&=\frac{1}{m}\sum^m_{i=1}dist^2_{ij}-\frac{1}{m}tr(\boldsymbol B)\\
&=dist^2_{\cdot j}-\frac{1}{2}dist^2_{\cdot}
\end{aligned}
$$



由式(10.5)和式(10.7)可得 

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



在式(10.10)后紧跟着一句话： "由此即可通过降维前后保持不变的距离矩阵
$\mathbf{D}$ 求取内积 矩阵B", 我们来解释一下这句话。

首先解释式(10.10)等号右侧的变量含义：
$\quad d i s t_{i j}=\left\|\boldsymbol{z}_i-\boldsymbol{z}_j\right\|$
表示降维后 $\boldsymbol{z}_i$ 与 $\boldsymbol{z}_j$ 的欧氏 距离,
注意这同时也应该是原始空间 $\boldsymbol{x}_i$ 与 $\boldsymbol{x}_j$
的距离, 因为降维的目标（也是约束条件）是 "任意两个样本在 $d^{\prime}$
维空间中的欧氏距离等于原始空间中的距离"; 其次, 式(10.10)等号左侧
$b_{i j}$ 是降维后内积矩阵 $\mathbf{B}$ 的元 素, 即 $\mathbf{B}$ 的元素
$b_{i j}$ 可以由距离矩阵 $\mathbf{D}$ 来表达求取。

### 10.4.7 式(10.11)的解释

由题设知，$d^*$为$\mathbf{V}$的非零特征值，因此$\mathbf{B}=\mathbf{V} \boldsymbol{\Lambda} \mathbf{V}^{\top}$可以写成$\mathbf{B}=\mathbf{V}_{*} \boldsymbol{\Lambda}_{*} \mathbf{V}_{*}^{\top}$，其中$\boldsymbol{\Lambda}_{*} \in \mathbb{R}^{d \times d}$为$d$个非零特征值构成的特征值对角矩阵，而$\mathbf{V}_{*} \in \mathbb{R}^{m \times d}$
为
$\boldsymbol{\Lambda}_{*} \in \mathbb{R}^{d \times d}$对应的特征值向量矩阵，因此有


$$
\mathbf{B}=\left(\mathbf{V}_{*} \boldsymbol{\Lambda}_{*}^{1 / 2}\right)\left(\boldsymbol{\Lambda}_{*}^{1 / 2} \mathbf{V}_{*}^{\top}\right)
$$



故而$\mathbf{Z}=\boldsymbol{\Lambda}_{*}^{1 / 2} \mathbf{V}_{*}^{\top} \in \mathbb{R}^{d \times m}$

### 10.4.8 图10.3关于MDS算法的解释

首先要清楚此处降维算法要完成的任务： 获得 $d$ 维空间的样本集合
$\mathbf{X} \in \mathbb{R}^{d \times m}$ 在 $d^{\prime}$ 维空 间的表示
$\mathbf{Z} \in \mathbb{R}^{d^{\prime} \times m}$, 并且保证距离矩阵
$\mathbf{D} \in \mathbb{R}^{m \times m}$ 相同, 其中 $d^{\prime}<d, m$
为样本个数,距离矩阵即样本之间的欧氏距离。那么怎么由$\mathbf{X} \in \mathbb{R}^{d \times m}$得到$\mathbf{Z} \in \mathbb{R}^{d^{\prime} \times m}$呢?

经过推导发现 (式(10.3) 式(10.10)), 在保证距离矩阵
$\mathbf{D} \in \mathbb{R}^{m \times m}$ 相同的前提下, $d^{\prime}$ 维
空间的样本集合 $\mathbf{Z} \in \mathbb{R}^{d^{\prime} \times m}$
的内积矩阵
$\mathbf{B}=\mathbf{Z}^{\top} \mathbf{Z} \in \mathbb{R}^{m \times m}$
可以由距离矩阵 $\mathbf{D} \in \mathbb{R}^{m \times m}$ 得到
(参见式(10.10)), 此时只要对 $\mathrm{B}$ 进行矩阵分解即可得到
$\mathrm{Z}$; 具体来说, 对 $\mathrm{B}$ 进行特征值分 解可得
$\mathbf{B}=\mathbf{V} \boldsymbol{\Lambda} \mathbf{V}^{\top}$, 其中
$\mathbf{V} \in \mathbb{R}^{m \times m}$ 为特征值向量矩阵,
$\mathbf{\Lambda} \in \mathbb{R}^{m \times m}$ 为特征值构成的对 角矩阵,
接下来分类讨论：

\(1\) 当 $d>m$ 时, 即样本属性比样本个数还要多 此时, 样本集合
$\mathbf{X} \in \mathbb{R}^{d \times m}$ 的 $d$ 维属性一定是线性相关的
(即有品几余), 因为矩阵 $\mathbf{X}$ 的 秩不会大于 $m$ (此处假设矩阵
$\mathbf{X}$ 的秩恰好等于 $m$ ), 因此
$\Lambda \in \mathbb{R}^{m \times m}$ 主对角线有 $m$ 个非零值, 进而
$\mathbf{B}=\left(\mathbf{V} \boldsymbol{\Lambda}^{1 / 2}\right)\left(\boldsymbol{\Lambda}^{1 / 2} \mathbf{V}^{\top}\right)$,
得到的
$\mathbf{Z}=\boldsymbol{\Lambda}^{1 / 2} \mathbf{V}^{\top} \in \mathbb{R}^{d^{\prime} \times m}$
实际将 $d$ 维属性降成了 $d^{\prime}=m$ 维属性。

\(2\) 当 $d<m$ 时, 即样本个数比样本属性多
这是现实中最常见的一种情况。此时 $\Lambda \in \mathbb{R}^{m \times m}$
至多有 $d$ 个非零值（此处假设恰有 $d$ 个 非零值), 因此
$\mathbf{B}=\mathbf{V} \boldsymbol{\Lambda} \mathbf{V}^{\top}$ 可以写成
$\mathbf{B}=\mathbf{V}_* \boldsymbol{\Lambda}_* \mathbf{V}_*^{\top}$,
其中 $\boldsymbol{\Lambda}_* \in \mathbb{R}^{d \times d}$ 为 $d$
个非零值特征 值构成的特征值对角矩阵,
$\mathbf{V}_* \in \mathbb{R}^{m \times d}$ 为
$\Lambda_* \in \mathbb{R}^{d \times d}$ 相应的特征值向量矩阵, 进而
$\mathbf{B}=\left(\mathbf{V}_* \boldsymbol{\Lambda}_*^{1 / 2}\right)\left(\boldsymbol{\Lambda}_*^{1 / 2} \mathbf{V}_*^{\top}\right)$,
求得
$\mathbf{Z}=\boldsymbol{\Lambda}_*^{1 / 2} \mathbf{V}_*^{\top} \in \mathbb{R}^{d \times m}$,
此时属性没有冗杂, 因此按降维
的规则（降维后距离矩阵不变）并不能实现有效降维。

由以上分析可以看出, 降维后的维度 $d^{\prime}$ 实际为 $\mathrm{B}$
特征值分解后非零特征值的个数。

## 10.5 主成分分析

注意，作者在数次印刷中对本节符号进行修订，详见勘误修订，直接搜索页码即可，此处仅按个人推导需求定义符号，可能与不同印次书中符号不一致。

### 10.5.1 式(10.14)的推导

在一个坐标系中,
任意向量等于其在各个坐标轴的坐标值乘以相应坐标轴单位向量之和。 例如,
在二维直角坐标系中, $\boldsymbol{x}$ 轴和 $\boldsymbol{y}$
轴的单位向量分别为 $\boldsymbol{v}_1=(1 ; 0)$ 和
$\boldsymbol{v}_2=(0 ; 1)$, 向量 $\boldsymbol{r}=(2 ; 3)$ 可以表示为
$\boldsymbol{r}=2 \boldsymbol{v}_1+3 \boldsymbol{v}_2$; 其实
$\boldsymbol{v}_1=(1 ; 0)$ 和 $\boldsymbol{v}_2=(0 ; 1)$
只是二维平面的一组 标准正交基, 但二维平面实际有无数标准正交基, 如
$\boldsymbol{v}_1^{\prime}=\left(\frac{1}{\sqrt{2}} ; \frac{1}{\sqrt{2}}\right)$
和
$\boldsymbol{v}_2^{\prime}=\left(-\frac{1}{\sqrt{2}} ; \frac{1}{\sqrt{2}}\right)$,
此时向量
$\boldsymbol{r}=\frac{5}{\sqrt{2}} \boldsymbol{v}_1^{\prime}+\frac{1}{\sqrt{2}} \boldsymbol{v}_2^{\prime}$,
其中
$\frac{5}{\sqrt{2}}=\left(\boldsymbol{v}_1^{\prime}\right)^{\top} \boldsymbol{r}, \frac{1}{\sqrt{2}}=\left(\boldsymbol{v}_2^{\prime}\right)^{\top} \boldsymbol{r}$,
即新坐标系里的坐标。

下面开始推导，对于 $d$ 维空间 $\mathbb{R}^{d \times 1}$ 来说,
传统的坐标系为
$\left\{\boldsymbol{v}_1, \boldsymbol{v}_2, \ldots, \boldsymbol{v}_k, \ldots, \boldsymbol{v}_d\right\}$,
其中 $\boldsymbol{v}_k$ 为除第 $k$ 个 元素为 1 其余元素均 0 的 $d$
维列向量; 此时对于样本点
$\boldsymbol{x}_i=\left(x_{i 1} ; x_{i 2} ; \ldots ; x_{i d}\right) \in \mathbb{R}^{d \times 1}$
来说 亦可表示为
$\boldsymbol{x}_i=x_{i 1} \boldsymbol{v}_1+x_{i 2} \boldsymbol{v}_2+\ldots+x_{i d} \boldsymbol{v}_d$
。

现假定投影变换后得到的新坐标系为
$\left\{\boldsymbol{w}_1, \boldsymbol{w}_2, \ldots, \boldsymbol{w}_k, \ldots, \boldsymbol{w}_d\right\}$
（即一组新的标准正 交基), 则 $\boldsymbol{x}_i$ 在新坐标系中的坐标为
$\left(\boldsymbol{w}_1^{\top} \boldsymbol{x}_i ; \boldsymbol{w}_2^{\top} \boldsymbol{x}_i ; \ldots ; \boldsymbol{w}_d^{\top} \boldsymbol{x}_i\right)$
。若丢弃新坐标系中的部分 坐标, 即将维度降低到 $d^{\prime}<d$
(不失一般性, 假设丢掉的是后 $d-d^{\prime}$ 维坐标), 并令


$$
\mathbf{W}=\left(\boldsymbol{w}_1, \boldsymbol{w}_2, \ldots, \boldsymbol{w}_{d^{\prime}}\right) \in \mathbb{R}^{d \times d^{\prime}}
$$



则 $\boldsymbol{x}_i$ 在低维坐标系中的投影为 

$$
\begin{aligned}
\boldsymbol{z}_i & =\left(z_{i 1} ; z_{i 2} ; \ldots ; z_{i d^{\prime}}\right)=\left(\boldsymbol{w}_1^{\top} \boldsymbol{x}_i ; \boldsymbol{w}_2^{\top} \boldsymbol{x}_i ; \ldots ; \boldsymbol{w}_{d^{\prime}}^{\top} \boldsymbol{x}_i\right) \\
& =\mathbf{W}^{\top} \boldsymbol{x}_i
\end{aligned}
$$



若基于 $\boldsymbol{z}_i$ 来重构 $\boldsymbol{x}_i$, 则会得到
$\hat{\boldsymbol{x}}_i=\sum_{j=1}^{d^{\prime}} z_{i j} \boldsymbol{w}_j=\mathbf{W} \boldsymbol{z}_i$
("西瓜书" P230 第 11 行)。

有了以上符号基础, 接下来将式(10.14)化简成式(10.15)目标函数形式
(可逐一核对各项 维数以验证推导是否有误)：



$$
\begin{aligned}
\sum_{i=1}^m\left\|\sum_{j=1}^{d^{\prime}} z_{i j} \boldsymbol{w}_j-\boldsymbol{x}_i\right\|_2^2 &=\sum_{i=1}^m\left\|\mathbf{W} \boldsymbol{z}_i-\boldsymbol{x}_i\right\|_2^2 &\textcircled{1}\\
&=\sum_{i=1}^m\left\|\mathbf{W} \mathbf{W}^{\top} \boldsymbol{x}_i-\boldsymbol{x}_i\right\|_2^2 &\textcircled{2}\\
&=\sum_{i=1}^m\left(\mathbf{W} \mathbf{W}^{\top} \boldsymbol{x}_i-\boldsymbol{x}_i\right)^{\top}\left(\mathbf{W} \mathbf{W}^{\top} \boldsymbol{x}_i-\boldsymbol{x}_i\right) &\textcircled{3}\\
&=\sum_{i=1}^m\left(\boldsymbol{x}_i^{\top} \mathbf{W} \mathbf{W}^{\top} \mathbf{W} \mathbf{W}^{\top} \boldsymbol{x}_i-2 \boldsymbol{x}_i^{\top} \mathbf{W} \mathbf{W}^{\top} \boldsymbol{x}_i+\boldsymbol{x}_i^{\top} \boldsymbol{x}_i\right) &\textcircled{4}\\
&=\sum_{i=1}^m\left(\boldsymbol{x}_i^{\top} \mathbf{W} \mathbf{W}^{\top} \boldsymbol{x}_i-2 \boldsymbol{x}_i^{\top} \mathbf{W} \mathbf{W}^{\top} \boldsymbol{x}_i+\boldsymbol{x}_i^{\top} \boldsymbol{x}_i\right) &\textcircled{5}\\
&=\sum_{i=1}^m\left(-\boldsymbol{x}_i^{\top} \mathbf{W}\mathbf{W}^{\top} \mathbf{x}_i+\boldsymbol{x}_i^{\top} \boldsymbol{x}_i\right) &\textcircled{6}\\
&=\sum_{i=1}^m\left(-\left(\mathbf{W}^{\top} \boldsymbol{x}_i\right)^{\top}\left(\mathbf{W}^{\top} \boldsymbol{x}_i\right)+\boldsymbol{x}_i^{\top} \boldsymbol{x}_i\right) &\textcircled{7}\\
&=\sum_{i=1}^m\left(-\left\|\mathbf{W}^{\top} \boldsymbol{x}_i\right\|_2^2+\boldsymbol{x}_i^{\top} \boldsymbol{x}_i\right) &\textcircled{8}\\
&\propto-\sum_{i=1}^m\left\|\mathbf{W}^{\top} \boldsymbol{x}_i\right\|_2^2 &\textcircled{9}
\end{aligned}
$$



$\textcircled{3}\to\textcircled{4}$是由于
$\left(\mathbf{W} \mathbf{W}^{\top}\right)^{\top}=\left(\mathbf{W}^{\top}\right)^{\top}(\mathbf{W})^{\top}=\mathbf{W} \mathbf{W}^{\top}$,
因此


$$
\left(\mathbf{W} \mathbf{W}^{\top} \boldsymbol{x}_i\right)^{\top}=\boldsymbol{x}_i^{\top}\left(\mathbf{W} \mathbf{W}^{\top}\right)^{\top}=\boldsymbol{x}_i^{\top} \mathbf{W} \mathbf{W}^{\top}
$$


代入即得$\textcircled{4}$;

$\textcircled{4}\to\textcircled{5}$是由于
$\boldsymbol{w}_i^{\top} \boldsymbol{w}_j=0,(i \neq j),\left\|\boldsymbol{w}_i\right\|=1$,
因此
$\mathbf{W}^{\top} \mathbf{W}=\mathbf{I} \in \mathbb{R}^{d^{\prime} \times d^{\prime}}$,
代入即得$\textcircled{5}$。由于最终目标是寻找 $\mathbf{W}$ 使目标函数(10.14)
最小, 而 $\boldsymbol{x}_i^{\top} \boldsymbol{x}_i$ 与 $\mathbf{W}$
无关, 因此在优化时可以去掉。令
$\mathbf{X}=\left(\boldsymbol{x}_1, \boldsymbol{x}_2, \ldots, \boldsymbol{x}_m\right) \in \mathbb{R}^{d \times m}$,
即每列为一个样本, 则式(10.14)可继续化简为
(参见10.2节) 

$$
\begin{aligned}
-\sum_{i=1}^m\left\|\mathbf{W}^{\top} \boldsymbol{x}_i\right\|_2^2 & =-\left\|\mathbf{W}^{\top} \mathbf{X}\right\|_F^2 \\
& =-\operatorname{tr}\left(\left(\mathbf{W}^{\top} \mathbf{X}\right)\left(\mathbf{W}^{\top} \mathbf{X}\right)^{\top}\right) \\
& =-\operatorname{tr}\left(\mathbf{W}^{\top} \mathbf{X} \mathbf{X}^{\top} \mathbf{W}\right)
\end{aligned}
$$



这里 $\mathbf{W}^{\top} \boldsymbol{x}_i=\boldsymbol{z}_i$,
这里仅为得到式 (10.15) 的形式才最终保留 $\mathbf{W}$ 和
$\boldsymbol{x}_i$ 的; 若令
$\mathbf{Z}=\left(\boldsymbol{z}_1, \boldsymbol{z}_2, \ldots, \boldsymbol{z}_m\right) \in \mathbb{R}^{d^{\prime} \times m}$
为低维坐标系中的样本集合, 则 $\mathbf{Z}=\mathbf{W}^{\top} \mathbf{X}$,
即 $\boldsymbol{z}_i$ 为矩阵 $\mathbf{Z}$ 的第 $i$ 列; 而
$\sum_{i=1}^m\left\|\mathbf{W}^{\top} \boldsymbol{x}_i\right\|_2^2=\sum_{i=1}^m\left\|\boldsymbol{z}_i\right\|_2^2$
表示Z所有列向量 2 范数的平方, 也就是Z所 有元素的平方和, 即为
$\|\mathbf{Z}\|_F^2$, 此即第一个等号的由来;
而根据10.2节中第(3)个结论, 即对于矩阵 $\mathbf{Z}$ 有
$\|\mathbf{Z}\|_F^2=\operatorname{tr}\left(\mathbf{Z}^{\top} \mathbf{Z}\right)=\operatorname{tr}\left(\mathbf{Z} \mathbf{Z}^{\top}\right)$,
其中 $\operatorname{tr}(\cdot)$ 表示求矩阵的迹, 即对角线元素之和,
此即第二个等号的由来; 第三个等号将转置化简即得。

到此即得式(10.15)的目标函数, 约束条件
$\mathbf{W}^{\top} \mathbf{W}=\mathbf{I}$ 已在推导中说明。

式(10.15)的目标函数式(10.14)结果略有差异, 接下来推导
$\sum_{i=1}^m \boldsymbol{x}_i \boldsymbol{x}_i^{\top}=\mathbf{X X}^{\top}$
以弥补这个差异（这个结论可以记下来)。

先化简 $\sum_{i=1}^m \boldsymbol{x}_i \boldsymbol{x}_i^{\top}$, 首先


$$
\boldsymbol{x}_i \boldsymbol{x}_i^{\top}=\left[\begin{array}{c}
x_{i 1} \\
x_{i 2} \\
\vdots \\
x_{i d}
\end{array}\right]\left[\begin{array}{llll}
x_{i 1} & x_{i 2} & \cdots & x_{i d}
\end{array}\right]=\left[\begin{array}{cccc}
x_{i 1} x_{i 1} & x_{i 1} x_{i 2} & \cdots & x_{i 1} x_{i d} \\
x_{i 2} x_{i 1} & x_{i 2} x_{i 2} & \cdots & x_{i 2} x_{i d} \\
\vdots & \vdots & \ddots & \vdots \\
x_{i d} x_{i 1} & x_{i d} x_{i 2} & \cdots & x_{i d} x_{i d}
\end{array}\right]_{d \times d}
$$



整体代入求和号 $\sum_{i=1}^m \boldsymbol{x}_i \boldsymbol{x}_i^{\top}$,
得 

$$
\begin{aligned}
\sum_{i=1}^m \boldsymbol{x}_i \boldsymbol{x}_i^{\top}= & \sum_{i=1}^m\left[\begin{array}{cccc}
x_{i 1} x_{i 1} & x_{i 1} x_{i 2} & \cdots & x_{i 1} x_{i d} \\
x_{i 2} x_{i 1} & x_{i 2} x_{i 2} & \cdots & x_{i 2} x_{i d} \\
\vdots & \vdots & \ddots & \vdots \\
x_{i d} x_{i 1} & x_{i d} x_{i 2} & \cdots & x_{i d} x_{i d}
\end{array}\right]_{d \times d} \\
= & {\left[\begin{array}{cccc}
\sum_{i=1}^m x_{i 1} x_{i 1} & \sum_{i=1}^m x_{i 1} x_{i 2} & \cdots & \sum_{i=1}^m x_{i 1} x_{i d} \\
\sum_{i=1}^m x_{i 2} x_{i 1} & \sum_{i=1}^m x_{i 2} x_{i 2} & \cdots & \sum_{i=1}^m x_{i 2} x_{i d} \\
\vdots & \vdots & \ddots & \vdots \\
\sum_{i=1}^m x_{i d} x_{i 1} & \sum_{i=1}^m x_{i d} x_{i 2} & \cdots & \sum_{i=1}^m x_{i d} x_{i d}
\end{array}\right]_{d \times d} }
\end{aligned}
$$



再化简 $\mathbf{X X}^{\top} \in \mathbb{R}^{d \times d}$


$$
\mathbf{X X}^{\top}=\left[\begin{array}{llll}
\boldsymbol{x}_1 & \boldsymbol{x}_2 & \cdots & \boldsymbol{x}_d
\end{array}\right]\left[\begin{array}{c}
\boldsymbol{x}_1^{\top} \\
\boldsymbol{x}_2^{\top} \\
\vdots \\
\boldsymbol{x}_d^{\top}
\end{array}\right]
$$



将列向量
$\boldsymbol{x}_i=\left(x_{i 1} ; x_{i 2} ; \ldots ; x_{i d}\right) \in \mathbb{R}^{d \times 1}$
代入 

$$
\begin{aligned}
& \mathbf{X X}^{\top}=\left[\begin{array}{cccc}
x_{11} & x_{21} & \cdots & x_{m 1} \\
x_{12} & x_{22} & \cdots & x_{m 2} \\
\vdots & \vdots & \ddots & \vdots \\
x_{1 d} & x_{2 d} & \cdots & x_{m d}
\end{array}\right]_{d \times m} \bullet\left[\begin{array}{cccc}
x_{11} & x_{12} & \cdots & x_{1 d} \\
x_{21} & x_{22} & \cdots & x_{2 d} \\
\vdots & \vdots & \ddots & \vdots \\
x_{m 1} & x_{m 2} & \cdots & x_{m d}
\end{array}\right]_{m \times d} \\
& =\left[\begin{array}{cccc}
\sum_{i=1}^m x_{i 1} x_{i 1} & \sum_{i=1}^m x_{i 1} x_{i 2} & \cdots & \sum_{i=1}^m x_{i 1} x_{i d} \\
\sum_{i=1}^m x_{i 2} x_{i 1} & \sum_{i=1}^m x_{i 2} x_{i 2} & \cdots & \sum_{i=1}^m x_{i 2} x_{i d} \\
\vdots & \vdots & \ddots & \vdots \\
\sum_{i=1}^m x_{i d} x_{i 1} & \sum_{i=1}^m x_{i d} x_{i 2} & \cdots & \sum_{i=1}^m x_{i d} x_{i d}
\end{array}\right]_{d \times d} \\
&
\end{aligned}
$$



综合 $\sum_{i=1}^m \boldsymbol{x}_i \boldsymbol{x}_i^{\top}$ 和
$\mathbf{X X}^{\top}$ 的化简结果, 即
$\sum_{i=1}^m \boldsymbol{x}_i \boldsymbol{x}_i^{\top}=\mathbf{X X}^{\top}$
(协方差矩阵)。 根据刚刚推导得到的结论,
式(10.14)最后的结果即可化为式(10.15)的目标函数


$$
\operatorname{tr}\left(\mathbf{W}^{\top}\left(\sum_{i=1}^m \boldsymbol{x}_i \boldsymbol{x}_i^{\top}\right) \mathbf{W}\right)=\operatorname{tr}\left(\mathbf{W}^{\top} \mathbf{X} \mathbf{X}^{\top} \mathbf{W}\right)
$$



式(10.15)描述的优化问题的求解详见式(10.17)最后的解释。

### 10.5.2 式(10.16)的解释

先说什么是方差，对于包含 $n$ 个样本的一组数据
$X=\left\{x_1, x_2, \ldots, x_n\right\}$ 来说, 均值 $M$ 为


$$
M=\frac{x_1+x_2+\ldots+x_n}{n}=\sum_{i=1}^n x_i
$$



则方差 $\sigma_X^2$ 公式为 

$$
\begin{aligned}
\sigma^2 & =\frac{\left(x_1-M\right)^2+\left(x_2-M\right)^2+\ldots+\left(x_n-M\right)^2}{n} \\
& =\frac{1}{n} \sum_{i=1}^n\left(x_i-M\right)^2
\end{aligned}
$$



方差衡量了该组数据偏离均值的程度，样本越分散, 其方差越大。

再说什么是协方差，若还有包含 $n$ 个样本的另一组数据
$X^{\prime}=\left\{x_1^{\prime}, x_2^{\prime}, \ldots, x_n^{\prime}\right\}$,
均值为 $M^{\prime}$, 则下式 

$$
\begin{aligned}
\sigma_{X X^{\prime}}^2 & =\frac{\left(x_1-M\right)\left(x_1^{\prime}-M^{\prime}\right)+\left(x_2-M\right)\left(x_2^{\prime}-M^{\prime}\right)+\ldots+\left(x_n-M\right)\left(x_n^{\prime}-M^{\prime}\right)}{n} \\
& =\frac{1}{n} \sum_{i=1}^n\left(x_i-M\right)\left(x_i^{\prime}-M^{\prime}\right)
\end{aligned}
$$



称为两组数据的协方差。 $\sigma_{X X^{\prime}}^2$ 能说明第一组数据
$x_1, x_2, \ldots, x_n$ 和第二组数据
$x_1^{\prime}, x_2^{\prime}, \ldots, x_n^{\prime}$ 的变化情况。具体来说,
如果两组数据总是同时大于或小于自己的均值, 则
$\left(x_i-M\right)\left(x_i^{\prime}-M^{\prime}\right)>0$, 此时
$\sigma_{X X^{\prime}}^2>0$; 如果两组数据总是一个大于 (或小于) 自己的均
值而别一个小于 (或大于) 自己的均值, 则
$\left(x_i-M\right)\left(x_i^{\prime}-M^{\prime}\right)<0$, 此时
$\sigma_{X X^{\prime}}^2<0$; 如果 两组数据与自己的均值的大小关系无规律,
则 $\left(x_i-M\right)\left(x_i^{\prime}-M^{\prime}\right)$
的正负号随机变化, 其平 均数 $\sigma_{X X}^2$, 则会趋近于 0
。引用百度百科协方差词条原话： "从直观上来看, 协方差表示的是
两个变量总体误差的期望。如果两个变量的变化趋势一致,
也就是说如果其中一个大于自身 的期望值时另外一个也大于自身的期望值,
那么两个变量之间的协方差就是正值; 如果两个 变量的变化趋势相反,
即其中一个变量大于自身的期望值时另外一个却小于自身的期望值,
那么两个变量之间的协方差就是负值。如果两个变量是统计独立的,
那么二者之间的协方差 就是 0 , 但是, 反过来并不成立。协方差为 0
的两个随机变量称为是不相关的。"

最后说什么是协方差矩阵，结合本书中的符号：


$$
\mathbf{X}=\left(\boldsymbol{x}_1, \boldsymbol{x}_2, \ldots, \boldsymbol{x}_m\right)=\left[\begin{array}{cccc}
x_{11} & x_{21} & \cdots & x_{m 1} \\
x_{12} & x_{22} & \cdots & x_{m 2} \\
\vdots & \vdots & \ddots & \vdots \\
x_{1 d} & x_{2 d} & \cdots & x_{m d}
\end{array}\right]_{d \times m}
$$

 矩阵 $\mathbf{X}$ 每一行表示一维特征,
每一列表示该数据集的一个样本; 而本节开始已假定数据样本 进行了中心化, 即
$\sum_{i=1}^m x_i=0 \in \mathbb{R}^{d \times 1}$ (中心化过程可通过
$\mathbf{X}\left(\mathrm{I}-\frac{1}{m} 11^{\top}\right.$ )实现, 其中
$\mathrm{I} \in \mathbb{R}^{m \times m}$ 为单位阵,
$\mathbf{1} \in \mathbb{R}^{m \times 1}$ 为全 1 列向量, 参见习题 10.3),
即上式矩阵的每一行平均 值等于零 (其实就是分别对所有 $\boldsymbol{x}_i$
的每一维坐标进行中心化, 而不是分别对单个样本 $\boldsymbol{x}_i$ 中
心化）对于包含 $d$ 个特征的特征空间（或称 $d$ 维特征空间）来说,
每一维特征可以看成是一 个随机变量, 而 $\mathbf{X}$ 中包含 $m$ 个样本,
也就是说每个随机变量有 $m$ 个数据, 根据前面 $\mathbf{X X}^{\top}$ 的
矩阵表达形式：


$$
\frac{1}{m} \mathbf{X X}^{\top}=\frac{1}{m}\left[\begin{array}{cccc}
\sum_{i=1}^m x_{i 1} x_{i 1} & \sum_{i=1}^m x_{i 1} x_{i 2} & \cdots & \sum_{i=1}^m x_{i 1} x_{i d} \\
\sum_{i=1}^m x_{i 2} x_{i 1} & \sum_{i=1}^m x_{i 2} x_{i 2} & \cdots & \sum_{i=1}^m x_{i 2} x_{i d} \\
\vdots & \vdots & \ddots & \vdots \\
\sum_{i=1}^m x_{i d} x_{i 1} & \sum_{i=1}^m x_{i d} x_{i 2} & \cdots & \sum_{i=1}^m x_{i d} x_{i d}
\end{array}\right]_{d \times d}
$$



根据前面的结果知道 $\frac{1}{m} \mathbf{X} \mathbf{X}^{\top}$ 的第 $i$
行第 $j$ 列的元素表示 $\mathbf{X}$ 中第 $i$ 行和 $\mathbf{X}^{\top}$ 第
$j$ 列（即 $\mathbf{X}$ 中第 $j$ 行） 的方差 $(i=j)$ 或协方差
$(i \neq j)$ 。注意： 协方差矩阵对角线元素为各行的方差。

接下来正式解释式(10.16)： 对于
$\mathbf{X}=\left(\boldsymbol{x}_1, \boldsymbol{x}_2, \ldots, \boldsymbol{x}_m\right) \in \mathbb{R}^{d \times m}$,
将其投影为
$\mathbf{Z}=\left(\boldsymbol{z}_1, \boldsymbol{z}_2, \ldots, \boldsymbol{z}_m\right) \in \mathbb{R}^{d^{\prime} \times m}$,
最大可分性出发, 我们希望在新空间的每一维坐标轴上样本都尽可能分散
(即每维特征尽可 能分散, 也就是Z各行方差最大; 参见图 $10.4$ 所示,
原空间只有两维坐标, 现考虑降至一维, 希望在新坐标系下样本尽可能分散,
图中画出了一种映射后的坐标系, 显然橘红色坐标方向 样本更分散, 方差更大),
即寻找 $\mathbf{W} \in \mathbb{R}^{d \times d^{\prime}}$ 使协方差矩阵
$\frac{1}{m} \mathbf{Z Z}^{\top}$ 对角线元素之和 (矩阵的 迹）最大（即使
$\mathbf{Z}$ 各行方差之和最大), 由于
$\mathbf{Z}=\mathbf{W}^{\top} \mathbf{X}$, 而常系数 $\frac{1}{m}$
在最大化时并不发生 影响, 求矩阵对角线元素之和即为矩阵的迹,
综上即得式(10.16)。

另外, 中心化后 $\mathbf{X}$ 的各行均值为零, 变换后
$\mathbf{Z}=\mathbf{W}^{\top} \mathbf{X}$ 的各行均值仍为零, 这是因为
$\mathbf{Z}$ 的第 $i$ 行
$\left(1 \leqslant i \leqslant d^{\prime}\right)$ 为
$\left\{\boldsymbol{w}_i^{\top} \boldsymbol{x}_1, \boldsymbol{w}_i^{\top} \boldsymbol{x}_2, \ldots, \boldsymbol{w}_i^{\top} \boldsymbol{x}_m\right\}$,
该行之和
$\boldsymbol{w}_i^{\top} \sum_{j=1}^m \boldsymbol{x}_j=\boldsymbol{w}_i^{\top} \mathbf{0}=0$
。

最后, 有关方差的公式, 有人认为应该除以样本数量 $m$,
有人认为应该除以样本数量减 1 即 $m-1$ 。简单来说,
根据总体样本集求方差就除以总体样本数量, 而根据抽样样本集求
方差就除以抽样样本集数量减 1; 总体样本集是真正想调查的对象集合,
而抽样样本集是从 总体样本集中被选出来的部分样本组成的集合,
用来估计总体样本集的方差; 一般来说, 总体样本集是不可得的,
我们拿到的都是抽样样本集。严格上来说，样本方差应该除以 $\mathrm{n}-1$
才会得到总体样本的无偏估计, 若除以 $\mathrm{n}$ 则得到的是有偏估计。

式(10.16)描述的优化问题的求解详见式(10.17)最后的解释。

### 10.5.3 式(10.17)的推导

由式（10.15）可知，主成分分析的优化目标为 

$$
\begin{aligned}
&\min\limits_{\mathbf W} \quad-\text { tr }(\mathbf W^{\mathrm{T}} \mathbf X\mathbf X^{\mathrm{T}} \mathbf W)\\
&s.t. \quad\mathbf W^{\mathrm{T}} \mathbf W=\mathbf I
\end{aligned}
$$


其中，$\mathbf{X}=\left(\boldsymbol{x}_{1}, \boldsymbol{x}_{2}, \ldots, \boldsymbol{x}_{m}\right) \in \mathbb{R}^{d \times m},\mathbf{W}=\left(\boldsymbol{w}_{1}, \boldsymbol{w}_{2}, \ldots, \boldsymbol{w}_{d^{\prime}}\right) \in \mathbb{R}^{d \times d^{\prime}}$，$\mathbf{I} \in \mathbb{R}^{d^{\prime} \times d^{\prime}}$为单位矩阵。对于带矩阵约束的优化问题，根据[1]中讲述的方法可得此优化目标的拉格朗日函数为


$$
\begin{aligned}
L(\mathbf W,\Theta)&=-\text { tr }(\mathbf W^{\mathrm{T}} \mathbf X\mathbf X^{\mathrm{T}} \mathbf W)+\langle \Theta,\mathbf W^{\mathrm{T}} \mathbf W-\mathbf I\rangle \\
&=-\text { tr }(\mathbf W^{\mathrm{T}} \mathbf X\mathbf X^{\mathrm{T}} \mathbf W)+\text { tr }\left(\Theta^{\mathrm{T}} (\mathbf W^{\mathrm{T}} \mathbf W-\mathbf I)\right) 
\end{aligned}
$$



其中，$\Theta  \in \mathbb{R}^{d^{\prime} \times d^{\prime}}$为拉格朗日乘子矩阵，其维度恒等于约束条件的维度，且其中的每个元素均为未知的拉格朗日乘子，$\langle \Theta,\mathbf W^{\mathrm{T}} \mathbf W-\mathbf I\rangle = \text { tr }\left(\Theta^{\mathrm{T}} (\mathbf W^{\mathrm{T}} \mathbf W-\mathbf I)\right)$为矩阵的内积[2]。若此时仅考虑约束$\boldsymbol{w}_i^{\mathrm{T}}\boldsymbol{w}_i=1(i=1,2,...,d^{\prime})$，则拉格朗日乘子矩阵$\Theta$此时为对角矩阵，令新的拉格朗日乘子矩阵为$\boldsymbol{\Lambda}=\operatorname{diag}(\lambda_1,\lambda_2,...,\lambda_{d^{\prime}})\in \mathbb{R}^{d^{\prime} \times d^{\prime}}$，则新的拉格朗日函数为


$$
L(\mathbf W,\boldsymbol{\Lambda})=-\text { tr }(\mathbf W^{\mathrm{T}} \mathbf X\mathbf X^{\mathrm{T}} \mathbf W)+\text { tr }\left(\boldsymbol{\Lambda}^{\mathrm{T}} (\mathbf W^{\mathrm{T}} \mathbf W-\mathbf I)\right)
$$



对拉格朗日函数关于$\mathbf{W}$求导可得 

$$
\begin{aligned}
\cfrac{\partial L(\mathbf W,\boldsymbol{\Lambda})}{\partial \mathbf W}&=\cfrac{\partial}{\partial \mathbf W}\left[-\text { tr }(\mathbf W^{\mathrm{T}} \mathbf X\mathbf X^{\mathrm{T}} \mathbf W)+\text { tr }\left(\boldsymbol{\Lambda}^{\mathrm{T}} (\mathbf W^{\mathrm{T}} \mathbf W-\mathbf I)\right)\right] \\
&=-\cfrac{\partial}{\partial \mathbf W}\text { tr }(\mathbf W^{\mathrm{T}} \mathbf X\mathbf X^{\mathrm{T}} \mathbf W)+\cfrac{\partial}{\partial \mathbf W}\text { tr }\left(\boldsymbol{\Lambda}^{\mathrm{T}} (\mathbf W^{\mathrm{T}} \mathbf W-\mathbf I)\right) \\
\end{aligned}
$$



由矩阵微分公式$\cfrac{\partial}{\partial \mathbf{X}} \text { tr }(\mathbf{X}^{\mathrm{T}}  \mathbf{B} \mathbf{X})=\mathbf{B X}+\mathbf{B}^{\mathrm{T}}  \mathbf{X},\cfrac{\partial}{\partial \mathbf{X}} \text { tr }\left(\mathbf{B X}^{\mathrm{T}}  \mathbf{X}\right)=\mathbf{X B}^{\mathrm{T}} +\mathbf{X B}$可得


$$
\begin{aligned}
\cfrac{\partial L(\mathbf W,\boldsymbol{\Lambda})}{\partial \mathbf W}&=-2\mathbf X\mathbf X^{\mathrm{T}} \mathbf W+\mathbf{W}\boldsymbol{\Lambda}+\mathbf{W}\boldsymbol{\Lambda}^{\mathrm{T}}  \\
&=-2\mathbf X\mathbf X^{\mathrm{T}} \mathbf W+\mathbf{W}(\boldsymbol{\Lambda}+\boldsymbol{\Lambda}^{\mathrm{T}} ) \\
&=-2\mathbf X\mathbf X^{\mathrm{T}} \mathbf W+2\mathbf{W}\boldsymbol{\Lambda}
\end{aligned}
$$


令$\cfrac{\partial L(\mathbf W,\boldsymbol{\Lambda})}{\partial \mathbf W}=\mathbf 0$可得


$$
\begin{aligned}
-2\mathbf X\mathbf X^{\mathrm{T}} \mathbf W+2\mathbf{W}\boldsymbol{\Lambda}&=\mathbf 0\\
\mathbf X\mathbf X^{\mathrm{T}} \mathbf W&=\mathbf{W}\boldsymbol{\Lambda}\\
\end{aligned}
$$



将$\mathbf W$和$\boldsymbol{\Lambda}$展开可得


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

### 10.5.4 根据式(10.17)求解式(10.16)

注意式(10.16)中 $\mathbf{W} \in \mathbb{R}^{d \times d^{\prime}}$, 只有
$d^{\prime}$ 列, 而式(10.17)可以得到 $d$ 列, 如何根据式(10.17)
求解式(10.16)呢? 对
$\mathbf{X X}^{\top} \mathbf{W}=\mathbf{W} \boldsymbol{\Lambda}$
两边同乘 $\mathbf{W}^{\top}$, 得


$$
\mathbf{W}^{\top} \mathbf{X} \mathbf{X}^{\top} \mathbf{W}=\mathbf{W}^{\top} \mathbf{W} \boldsymbol{\Lambda} = \boldsymbol{\Lambda}
$$



注意使用了约束条件 $\mathbf{W}^{\top} \mathbf{W}=\mathbf{I}$;
上式左边与式(10.16)的优化目标对应矩阵相同, 而右边
$\Lambda \in \mathbb{R}^{d^{\prime} \times d^{\prime}}$ 是由
$\mathbf{X X} \mathbf{X}^{\top}$ 的 $d^{\prime}$ 个特征值组成的对角阵,
两边同时取矩阵的迹, 得


$$
\operatorname{tr}\left(\mathbf{W}^{\top} \mathbf{X} \mathbf{X}^{\top} \mathbf{W}\right)=\operatorname{tr}(\boldsymbol{\Lambda})=\sum_{i=1}^{d^{\prime}} \lambda_i
$$


$d$ 个特征值, 因此当然是取出最大的前 $d^{\prime}$ 个特征值, 而
$\mathbf{W}$ 即特征值对应的标准化特征向量组 成的矩阵。

特别注意, 图 $10.5$ 只是得到了投影矩阵 $\mathbf{W}$, 而降维后的样本为
$\mathbf{Z}=\mathbf{W}^{\top} \mathbf{X}$。

## 10.6 核化线性降维

注意, 本节符号在第14次印刷中进行了修订,
另外有一点需要注意的是，在上一节中用 $\boldsymbol{z}_i$ 表示
$\boldsymbol{x}_i$ 降维后的像, 而本节用 $\boldsymbol{z}_i$ 表示
$\boldsymbol{x}_i$ 在高 维特征空间中的像。

本节推导实际上有一个前提, 以式(10.19)为例（式(10.21)仅将
$\boldsymbol{z}_i$ 换为 $\phi\left(\boldsymbol{x}_i\right)$ 而已), 那就
是 $\boldsymbol{z}_i$ 已经中心化 (计算方差要用样本减去均值,
式(10.19)是均值为零时特殊形式, 详见式 (10.16)的解释), 但
$\boldsymbol{z}_i=\phi\left(\boldsymbol{x}_i\right)$ 是
$\boldsymbol{x}_i$ 高维特征空间中的像, 即使 $\boldsymbol{x}_i$
已进行中心化, 但 $\boldsymbol{z}_i$ 却不 一定是中心化的,
此时本节推导均不再成立。推广工作详见 KPCA[3]的附录
A。

### 10.6.1 式(10.19)的解释

首先, 类似于式(10.14)的推导后半部分内容可知
$\sum_{i=1}^m \boldsymbol{z}_i \boldsymbol{z}_i^{\top}=\mathbf{Z} \mathbf{Z}^{\top}$,
其中 $\mathbf{Z}$ 的每一列 为一个样本, 设高维空间的维度为 $h$, 则
$\mathbf{Z} \in \mathbb{R}^{h \times m}$, 其中 $m$ 为数据集样本数量。

其次, 式(10.19)中的 $\mathbf{W}$ 为从高维空间降至低维 (维度为 $d$ )
后的正交基, 在第 14 次印 刷中加入表述
$\mathbf{W}=\left(\boldsymbol{w}_1, \boldsymbol{w}_2, \ldots, \boldsymbol{w}_d\right)$,
其中 $\mathbf{W} \in \mathbb{R}^{h \times d}$, 降维过程为
$\mathbf{X}=\mathbf{W}^{\top} \mathbf{Z}$ 。

最后, 式(10.19)类似于式 (10.17), 是为了求解降维投影矩阵
$\mathbf{W}=\left(\boldsymbol{w}_1, \boldsymbol{w}_2, \ldots, \boldsymbol{w}_d\right)$
。 但问题在于 $\mathbf{Z Z}^{\top} \in \mathbb{R}^{h \times h}$, 当维度
$h$ 很大时 (注意本节为核化线性降维, 第六章核方法中高
斯核会把样本映射至无穷维), 此时根本无法求解 $\mathrm{Z}^{\top}$
的特征值和特征向量。因此才有了后 面的式(10.20)。

第 14 次印刷及之后印次, 式(10.19)为
$\left(\sum_{i=1}^m \boldsymbol{z}_i \boldsymbol{z}_i^{\top}\right) \boldsymbol{w}_j=\lambda_j \boldsymbol{w}_j$,
而在之前的印次中表 达有误, 实际应该为
$\left(\sum_{i=1}^m \boldsymbol{z}_i \boldsymbol{z}_i^{\top}\right) \mathbf{W}=\mathbf{W} \boldsymbol{\Lambda}$,
类似于式(10.17)。而这两种表达本质相同, $\lambda_j \boldsymbol{w}_j$ 为
$\mathbf{W} \Lambda$ 的第 $j$ 列, 仅此而已。

### 10.6.2 式(10.20)的解释

本节为核化线性降维, 而式(10.19)是在维度为 $h$ 的高维空间运算,
式(10.20)变形（咋一 看似乎有点无厘头)
的目的是为了避免直接在高维空间运算, 即想办法能够使用式(6.22)的核技巧,
也就是后面的式(10.24)。

第 14 次印刷及之后印次该式没问题, 之前的式(10.20)应该是：


$$
\begin{aligned}
\mathbf{W} & =\left(\sum_{i=1}^m \boldsymbol{z}_i \boldsymbol{z}_i^{\top}\right) \mathbf{W} \boldsymbol{\Lambda}^{-1}=\sum_{i=1}^m\left(\boldsymbol{z}_i\left(\boldsymbol{z}_i^{\top} \mathbf{W} \boldsymbol{\Lambda}^{-1}\right)\right) \\
& =\sum_{i=1}^m\left(\boldsymbol{z}_i \boldsymbol{\alpha}_i\right)
\end{aligned}
$$

 其中
$\boldsymbol{\alpha}_i=\boldsymbol{z}_i^{\top} \mathbf{W} \boldsymbol{\Lambda}^{-1} \in \mathbb{R}^{1 \times d}, \boldsymbol{z}_i^{\top} \in \mathbb{R}^{1 \times h}, \mathbf{W} \in \mathbb{R}^{h \times d}, \boldsymbol{\Lambda} \in \mathbb{R}^{d \times d}$
为对角阵。这个结果 看似等号右侧也包含 $W$,
但将此式代入式(10.19)后经化简可避免在高维空间的运算, 而将
目标转化为求低维空间的
$\boldsymbol{\alpha}_i \in \mathbb{R}^{1 \times d}$,
详见式(10.24)的推导。

### 10.6.3 式(10.21)的解释

该式即为将式(10.19)中的 $\boldsymbol{z}_i$ 换为
$\phi\left(\boldsymbol{x}_i\right)$ 的结果。

### 10.6.4 式(10.22)的解释

该式即为将式(10.20)中的 $\boldsymbol{z}_i$ 换为
$\phi\left(\boldsymbol{x}_i\right)$ 的结果。

### 10.6.5 式(10.24)的推导

已知$\boldsymbol z_i=\phi(\boldsymbol x_i)$，类比$\mathbf{X}=\{\boldsymbol x_1,\boldsymbol x_2,...,\boldsymbol x_m\}$可以构造$\mathbf{Z}=\{\boldsymbol z_1,\boldsymbol z_2,...,\boldsymbol z_m\}$，所以公式(10.21)可变换为


$$
\left(\sum_{i=1}^{m} \phi(\boldsymbol{x}_{i}) \phi(\boldsymbol{x}_{i})^{\mathrm{T}}\right)\boldsymbol w_j=\left(\sum_{i=1}^{m} \boldsymbol z_i \boldsymbol z_i^{\mathrm{T}}\right)\boldsymbol w_j=\mathbf{Z}\mathbf{Z}^{\mathrm{T}}\boldsymbol w_j=\lambda_j\boldsymbol w_j
$$


又由公式(10.22)可知


$$
\boldsymbol w_j=\sum_{i=1}^{m} \phi\left(\boldsymbol{x}_{i}\right) \alpha_{i}^j=\sum_{i=1}^{m} \boldsymbol z_i \alpha_{i}^j=\mathbf{Z}\boldsymbol{\alpha}^j
$$


其中，$\boldsymbol{\alpha}^j=(\alpha_{1}^j;\alpha_{2}^j;...;\alpha_{m}^j)\in \mathbb{R}^{m \times 1}$。所以公式(10.21)可以进一步变换为


$$
\begin{aligned}
\mathbf{Z}\mathbf{Z}^{\mathrm{T}}\mathbf{Z}\boldsymbol{\alpha}^j&=\lambda_j\mathbf{Z}\boldsymbol{\alpha}^j \\
\mathbf{Z}\mathbf{Z}^{\mathrm{T}}\mathbf{Z}\boldsymbol{\alpha}^j&=\mathbf{Z}\lambda_j\boldsymbol{\alpha}^j
\end{aligned}
$$


由于此时的目标是要求出$\boldsymbol w_j$，也就等价于要求出满足上式的$\boldsymbol{\alpha}^j$，显然，此时满足$\mathbf{Z}^{\mathrm{T}}\mathbf{Z}\boldsymbol{\alpha}^j=\lambda_j\boldsymbol{\alpha}^j$的$\boldsymbol{\alpha}^j$一定满足上式，所以问题转化为了求解满足下式的$\boldsymbol{\alpha}^j$：


$$
\mathbf{Z}^{\mathrm{T}}\mathbf{Z}\boldsymbol{\alpha}^j=\lambda_j\boldsymbol{\alpha}^j
$$


令$\mathbf{Z}^{\mathrm{T}}\mathbf{Z}=\mathbf{K}$，那么上式可化为


$$
\mathbf{K}\boldsymbol{\alpha}^j=\lambda_j\boldsymbol{\alpha}^j
$$


此式即为公式(10.24)，其中矩阵$\mathbf{K}$的第i行第j列的元素$(\mathbf{K})_{ij}=\boldsymbol z_i^{\mathrm{T}}\boldsymbol z_j=\phi(\boldsymbol x_i)^{\mathrm{T}}\phi(\boldsymbol x_j)=\kappa\left(\boldsymbol{x}_{i}, \boldsymbol{x}_{j}\right)$

### 10.6.6 式(10.25)的解释

式(10.25)仅需将第 14 次印刷中式(10.22)的 $\boldsymbol{w}_j$
表达式转置后代入即可。

该式的意义在于, 求解新样本 $\boldsymbol{x} \in \mathbb{R}^{d \times 1}$
映射至高维空间 $\phi(\boldsymbol{x}) \in \mathbb{R}^{h \times 1}$
后再降至低维空 高维空间 $\mathbb{R}^{h \times 1}$
的运算。但是由于此处没有类似第 6 章支持向量的概念, 可以发现式(10.25)
计算时需要对所有样本求和, 因此它的计算开销比较大。

注意, 此处书中符号使用略有混乱, 因为在式(10.19)中 $\boldsymbol{z}_i$
表示 $\boldsymbol{x}_i$ 在高维特征空间中的像, 而此处又用 $z_j$
表示新样本 $\boldsymbol{x}$ 映射为 $\phi(\boldsymbol{x})$ 后再降维至
$\mathbb{R}^{d^{\prime} \times 1}$ 空间时的第 $j$ 维坐标。

## 10.7 流形学习

不要被 "流形学习" 的名字所欺骗, 本节开篇就明确说了,
它是一类借鉴了拓扑流形概 念的降维方法而已, 因此称为 "流形学习"。 $10.2$
节 MDS 算法的降维准则是要求原始空间
中样本之间的距离在低维空间中得以保持, $10.3$ 节 PCA
算法的降维准则是要求低维子空间 对样本具有最大可分性,
因为它们都是基于线性变换来进行降维的方法（参见式(10.13)，
故称为线性降维方法。

### 10.7.1 等度量映射(Isomap)的解释

如图"西瓜书"10.8所示, Isomap 算法与MDS 算法的区别仅在于距离矩阵
$\mathbf{D} \in \mathbb{R}^{m \times m}$ 的计算方法不同。在 MDS 算法中,
距离矩阵 $\mathbf{D} \in \mathbb{R}^{m \times m}$
即为普通的样本之间欧氏距离; 而本节的 Isomap 算法中, 距离矩阵
$\mathbf{D} \in \mathbb{R}^{m \times m}$ 由"西瓜书"图10.8的 Step1
\~Step5 生成, 即 遵循流形假设。当然, 对新样本降维时也有不同,
这在"西瓜书"图 10.8下的一段话中已阐明。

另外解释一下测地线距离, 欧氏距离即两点之间的直线距离,
而测地线距离是实际中可 以到达的路径, 如"西瓜书"图 10.7(a)中黑线
(欧氏距离) 和红线 (测地线距离)。

### 10.7.2 式(10.28)的推导



$$
w_{i j}=\frac{\sum\limits_{k \in Q_{i}} C_{j k}^{-1}}{\sum\limits_{l, s \in Q_{i}} C_{l s}^{-1}}
$$


由书中上下文可知，式(10.28)是如下优化问题的解。 

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


其中$\boldsymbol{I}=(1,1,...,1)\in \mathbb{R}^{n\times 1}$为$n$行1列的元素值全为1的向量。因此，上述优化问题可以重写为


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

### 10.7.3 式(10.31)的推导

以下推导需要使用预备知识中的10.2节：矩阵的 $\mathrm{F}$ 范数与迹。

观察式(10.29), 求和号内实际是一个列向量的 2 范数平方, 令
$\boldsymbol{v}_i=\boldsymbol{z}_i-\sum_{j \in Q_i} w_{i j} \boldsymbol{z}_j$,
$\boldsymbol{v}_i$ 的维度与 $\boldsymbol{z}_i$ 相同,
$\boldsymbol{v}_i \in \mathbb{R}^{d^{\prime} \times 1}$,
则式(10.29)可重写为 

$$
\begin{aligned}
\min _{\boldsymbol{z}_1, \boldsymbol{z}_2, \ldots, \boldsymbol{z}_m} & \sum_{i=1}^m\left\|\boldsymbol{v}_i\right\|_2^2 \\
\text { s.t. } & \boldsymbol{v}_i=\boldsymbol{z}_i-\sum_{j \in Q_i} w_{i j} \boldsymbol{z}_j, i=1,2, \ldots, m
\end{aligned}
$$



令
$\mathbf{Z}=\left(\boldsymbol{z}_1, \boldsymbol{z}_2, \ldots, \boldsymbol{z}_i, \ldots, \boldsymbol{z}_m\right) \in \mathbb{R}^{d^{\prime} \times m}, \mathbf{I}_i=(0 ; 0 ; \ldots ; 1 ; \ldots ; 0) \in \mathbb{R}^{m \times 1}$,
即 $\mathbf{I}_i$ 为$m \times 1$ 的列向量, 除第 $i$ 个元素等于 1
之外其余元素均为零, 则 

$$
\boldsymbol{z}_i=\mathbf{Z I}_i
$$

 令
$(\mathbf{W})_{i j}=w_{i j}$ （P237 页第 1 行 ), 即
$\mathbf{W}=\left(\boldsymbol{w}_1, \boldsymbol{w}_2, \ldots, \boldsymbol{w}_i, \ldots, \boldsymbol{w}_m\right)^{\top} \in \mathbb{R}^{m \times m}$,
也就是说 $\mathbf{W}$ 的第 $i$ 行的转置（没错, 就是第 $i$ 行) 对应第 $i$
个样 数 $\boldsymbol{w}_i$ （这里符号之所以别扭是因为 $w_{i j}$
已用来表示列向量 $\boldsymbol{w}_i$ 的第 $j$ 个元素, 但为了与习惯保
持一致即 $w_{i j}$ 表示 $\mathbf{W}$ 的第 $i$ 行第 $j$ 列元素, 只能忍忍,
此处暂时别扭着）， 即


$$
\mathbf{W}=\left(\boldsymbol{w}_1, \boldsymbol{w}_2, \ldots, \boldsymbol{w}_i, \ldots, \boldsymbol{w}_m\right)^{\top}=\left[\begin{array}{cccccc}
w_{11} & w_{21} & \cdots & w_{i 1} & \cdots & w_{m 1} \\
w_{12} & w_{22} & \cdots & w_{i 2} & \cdots & w_{m 2} \\
\vdots & \vdots & \ddots & \vdots & \ddots & \vdots \\
w_{1 j} & w_{2 j} & \cdots & w_{i j} & \cdots & w_{m j} \\
\vdots & \vdots & \ddots & \vdots & \ddots & \vdots \\
w_{1 m} & w_{2 m} & \cdots & w_{i m} & \cdots & w_{m m}
\end{array}\right]^{\top}
$$



对于 $\boldsymbol{w}_i \in \mathbb{R}^{m \times 1}$ 来说, 只有
$\boldsymbol{x}_i$ 的 $K$ 个近邻样本对应的下标对应的
$w_{i j} \neq 0, j \in Q_i$, 且它们 的和等于 1 , 则


$$
\sum_{j \in Q_i} w_{i j} \boldsymbol{z}_j=\mathbf{Z} \boldsymbol{w}_i
$$



因此


$$
\boldsymbol{v}_i=\boldsymbol{z}_i-\sum_{j \in Q_i} w_{i j} \boldsymbol{z}_j=\mathbf{Z I}_i-\mathbf{Z} \boldsymbol{w}_i=\mathbf{Z}\left(\mathbf{I}_i-\boldsymbol{w}_i\right)
$$



令
$\mathbf{V}=\left(\boldsymbol{v}_1, \boldsymbol{v}_2, \ldots, \boldsymbol{v}_i, \ldots, \boldsymbol{v}_m\right) \in \mathbb{R}^{d^{\prime} \times m}, \mathbf{I}=\left(\mathbf{I}_1, \mathbf{I}_2, \ldots, \mathbf{I}_i, \ldots, \mathbf{I}_m\right) \in \mathbb{R}^{m \times m}$,
则


$$
\mathbf{V}=\mathbf{Z}\left(\mathbf{I}-\mathbf{W}^{\top}\right)=\mathbf{Z}\left(\mathbf{I}^{\top}-\mathbf{W}^{\top}\right)=\mathbf{Z}(\mathbf{I}-\mathbf{W})^{\top}
$$



根据前面的预备知识, 并将上式 $V$ 和式(10.30)代入, 得式(10.31)目标函数：


$$
\begin{aligned}
\sum_{i=1}^m\left\|\boldsymbol{v}_i\right\|_2^2 & =\|\mathbf{V}\|_F^2 \\
& =\operatorname{tr}\left(\mathbf{V} \mathbf{V}^{\top}\right) \\
& =\operatorname{tr}\left(\left(\mathbf{Z}(\mathbf{I}-\mathbf{W})^{\top}\right)\left(\mathbf{Z}(\mathbf{I}-\mathbf{W})^{\top}\right)^{\top}\right) \\
& =\operatorname{tr}\left(\mathbf{Z}(\mathbf{I}-\mathbf{W})^{\top}(\mathbf{I}-\mathbf{W}) \mathbf{Z}^{\top}\right) \\
& =\operatorname{tr}\left(\mathbf{Z}\mathbf{M}\mathbf{Z}^{\top}\right)
\end{aligned}
$$



接下来求解式(10.31)。

参考式(10.17)的推导, 应用拉格朗日乘子法, 先写出拉格朗日函数


$$
L(\mathbf{Z}, \boldsymbol{\Lambda})=\operatorname{tr}\left(\mathbf{Z M Z}^{\top}\right)+\left(\mathbf{Z Z}^{\top}-\mathbf{I}\right) \boldsymbol{\Lambda}
$$



令 $\mathbf{P}=\mathbf{Z}^{\top}$ (否则有点别扭), 则拉格朗日函数变为


$$
L(\mathbf{P}, \boldsymbol{\Lambda})=\operatorname{tr}\left(\mathbf{P}^{\top} \mathbf{M} \mathbf{P}\right)+\left(\mathbf{P}^{\top} \mathbf{P}-\mathbf{I}\right) \mathbf{\Lambda}
$$



求导并令导数等于 0 ： 

$$
\begin{aligned}
\frac{\partial L(\mathbf{P}, \boldsymbol{\Lambda})}{\partial \mathbf{P}} & =\frac{\partial \operatorname{tr}\left(\mathbf{P}^{\top} \mathbf{M} \mathbf{P}\right)}{\partial \mathbf{P}}+\frac{\partial\left(\mathbf{P}^{\top} \mathbf{P}-\mathbf{I}\right)}{\partial \mathbf{P}} \boldsymbol{\Lambda} \\
& =2 \mathbf{M} \mathbf{P}-2 \mathbf{P} \boldsymbol{\Lambda}=\mathbf{0}
\end{aligned}
$$



特征值对角阵; 然后两边再同时左乘 $\mathbf{P}^{\top}$ 并取矩阵的迹, 注意
$\mathbf{P}^{\top} \mathbf{P}=\mathbf{I} \in \mathbb{R}^{d^{\prime} \times d^{\prime}}$,
得
$\operatorname{tr}\left(\mathbf{P}^{\top} \mathbf{M} \mathbf{P}\right)=\operatorname{tr}\left(\mathbf{P}^{\top} \mathbf{P} \boldsymbol{\Lambda}\right)=\operatorname{tr}(\boldsymbol{\Lambda})$
因此, $\mathbf{P}=\mathbf{Z}^{\top}$ 是由
$\mathrm{M} \in \mathbb{R}^{m \times m}$ 最小的 $d^{\prime}$
个特征值对应的特征向量组成的矩阵。

## 10.8 度量学习

回忆10.5.1节的Isomap算法相比与10.2节的MDS算法的区别在于距离矩阵的计算方法不同，Isomap算法在计算样本间距离时使用的（近似）测地线距离，而MDS算法使用的是欧氏距离，也就是说二者的距离度量不同。

### 10.8.1 式(10.34)的解释

为了推导方便, 令
$\boldsymbol{u}=\left(u_1 ; u_2 ; \ldots ; u_d\right)=\boldsymbol{x}_i-\boldsymbol{x}_j \in \mathbb{R}^{d \times 1}$,
其中 $u_k=x_{i k}-x_{j k}$, 则式(10.34)重写为
$\boldsymbol{u}^{\top} \mathbf{M} \boldsymbol{u}=\|\boldsymbol{u}\|_{\mathbf{M}}^2$,
其中 $\mathbf{M} \in \mathbb{R}^{d \times d}$, 具体到元素级别的表达：


$$
\begin{aligned}
\boldsymbol{u}^{\top} \mathbf{M} \boldsymbol{u} & =\left[\begin{array}{llll}
u_1 & u_2 & \ldots & u_d
\end{array}\right]\left[\begin{array}{cccc}
m_{11} & m_{12} & \ldots & m_{1 d} \\
m_{21} & m_{22} & \ldots & m_{2 d} \\
\vdots & \vdots & \ddots & \vdots \\
m_{d 1} & m_{d 2} & \ldots & m_{d d}
\end{array}\right]\left[\begin{array}{c}
u_1 \\
u_2 \\
\vdots \\
u_d
\end{array}\right] \\
& =\left[\begin{array}{llll}
u_1 & u_2 & \ldots & u_d
\end{array}\right]\left[\begin{array}{c}
u_1 m_{11}+u_2 m_{12}+\ldots+u_d m_{1 d} \\
u_1 m_{21}+u_2 m_{22}+\ldots+u_d m_{2 d} \\
\vdots \\
u_1 m_{d 1}+u_2 m_{d 2}+\ldots+u_d m_{d d}
\end{array}\right] \\
& =u_1 u_1 m_{11}+u_1 u_2 m_{12}+\ldots+u_1 u_d m_{1 d} \\
& +u_2 u_1 m_{21}+u_2 u_2 m_{22}+\ldots+u_2 u_d m_{2 d} \\
& \ldots \\
& +u_d u_1 m_{d 1}+u_d u_2 m_{d 2}+\ldots+u_d u_d m_{d d}
\end{aligned}
$$

 注意, 对应到本式符号,
式(10.33)的结果即为上面最后一个等式的对角线部分, 即


$$
u_1 u_1 m_{11}+u_2 u_2 m_{22}+\ldots+u_d u_d m_{d d}
$$


而式(10.32)的结果则要更进一步, 去除对角线部分中的权重
$m_{i i}(1 \leqslant i \leqslant d)$ 部分, 即


$$
u_1 u_1+u_2 u_2+\ldots+u_d u_d
$$

 对比以上三个结果,
即式(10.32)的平方欧氏距离, 式(10.33)的加权平方欧氏距离, 式(10.34)
的马氏距离, 可以细细体会度量矩阵究竟带来了什么。

因此, 所谓 "度量学习", 即将系统中的平方欧氏距离换为式(10.34)的马氏距离,
通过 优化某个目标函数, 得到最恰当的度量矩阵 $\mathbf{M}$
（新的距离度量计算方法）的过程。书中在 式(10.34) (10.38)介绍的 NCA
即为一个具体的例子, 可以从中品味 "度量学习"的本质。

对于度量矩阵 $\mathbf{M}$ 要求半正定, 文中提到必有正交基 $\mathbf{P}$
使得 $\mathbf{M}$ 能写为 $\mathbf{M}=\mathbf{P P}^{\top}$, 此时 马氏距离
$\boldsymbol{u}^{\top} \mathbf{M} \boldsymbol{u}=\boldsymbol{u}^{\top} \mathbf{P} \mathbf{P}^{\top} \boldsymbol{u}=\left\|\mathbf{P}^{\top} \boldsymbol{u}\right\|_2^2$
。

### 10.8.2 式(10.35)的解释

这就是一种定义而已，没什么别的意思。传统近邻分类器使用多数投票法，有投票权的样本为
$\boldsymbol{x}_i$ 最近的 $\mathrm{K}$ 个近邻, 即 $\mathrm{KNN}$;
但也可以将投票范围扩大到整个样本集, 但每个样本 的投票权重不一样，距离
$\boldsymbol{x}_i$ 越近的样本投票权重越大，例如可取为第 5 章式(5.19)当
$\beta_i=1$ 时的高斯径向基函数
$\exp \left(-\left\|\boldsymbol{x}_i-\boldsymbol{x}_j\right\|^2\right)$
。从式中可以看出, 若 $\boldsymbol{x}_j$ 与 $\boldsymbol{x}_i$ 重合,
则投票权重 为 1 ,
距离越大该值越小。式(10.35)的分母是对所有投票值规一化至 $[0,1]$ 范围,
使之为概率。

可能会有疑问： 式(10.35)分母求和变量 $l$ 是否应该包含 $\boldsymbol{x}_i$
的下标即 $l=i$ ? 其实无所谓, 进一步说其实是否进行规一化也无所谓, 熟悉
$\mathrm{KNN}$ 的话就知道, 在预测时是比较各类投票 数的相对大小,
各类样本对 $\boldsymbol{x}_i$ 的投票权重的分母在式(10.35)中相同,
因此不影响相对大小。

注意啊, 这里有计算投票权重时用到了距离度量,
所以可以进一步将其换为马氏距离, 通过优化某个目标
(如式(10.38)）得到最优的度量矩阵 $\mathbf{M}$。

### 10.8.3 式(10.36)的解释

先简单解释留一法(LOO), $\mathrm{KNN}$ 是选出样本 $\boldsymbol{x}_i$
的在样本集中最近的 $\mathrm{K}$ 个近邻, 而现在 将范围扩大,
使用样本集中的所有样本进行投票, 每个样本的投票权重为式(10.35), 将各类
样本的投票权重分别求和, 注意 $\boldsymbol{x}_i$
自己的类别肯定与自己相同（现在是训练阶段, 还没到 对末见样本的预测阶段,
训练集样本的类别信息均已知), 但自己不能为自己投票吧, 所以 要将自己除外,
即留一法。

假设训练集共有 $N$ 个类别, $\Omega_n$ 表示第 $n$ 类样本的下标集合
$(1 \leqslant n \leqslant N)$, 对于样本 $\boldsymbol{x}_i$ 来说,
可以分别计算 $N$ 个概率：


$$
p_n^{\boldsymbol{x}_i}=\sum_{j \in \Omega_n} p_{i j}, 1 \leqslant n \leqslant N
$$


注意, 若样本 $\boldsymbol{x}_i$ 的类别为 $n_*$, 则在根据上式计算
$p_{n_*}^{\boldsymbol{x}_i}$ 时要将 $\boldsymbol{x}_i$ 的下标去除,
即刚刚解释的留 一法 (自己不能为自己投票)。 $p_{n_*}^{\boldsymbol{x}_i}$
即为训练集将样本 $\boldsymbol{x}_i$ 预测为第 $n_*$ 类的概率, 若
$p_{n_*}^{\boldsymbol{x}_i}$ 在所有的
$p_n^{\boldsymbol{x}_i}(1 \leqslant n \leqslant N)$ 中最大, 则预测正确,
反之预测错误。

其中$p_{n_*}^{\boldsymbol{x}_i}$即为式(10.36)。

### 10.8.4 式(10.37)的解释

换为刚才式(10.36)的符号, 式(10.37)即为 $\sum_{i=1}^m p_{n_*}^{x_i}$,
也就是所有训练样本被训练集预测
正确的概率之和。我们当然希望这个概率和最大, 但若采用平方欧氏距离时,
对于某个训练 集来说这个概率和是固定的; 但若采用了马氏距离,
这个概率和与度量矩阵 $M$ 有关。

### 10.8.5 式(10.38)的解释

刚才式(10.37)中提到希望寻找一个度量矩阵 $\mathrm{M}$
使训练样本被训练集预测正确的概率之 和最大, 即
$\max _{\mathrm{M}} \sum_{i=1}^m p_{n_*}^{\boldsymbol{x}_i}$,
但优化问题习惯是最小化, 所以改为
$\min _{\mathrm{M}}-\sum_{i=1}^m p_{n_*}^{\boldsymbol{x}_i}$ 即可,
而式(10.38)目标函数中的常数 1 并不影响优化结果, 有没有无所谓的。

式(10.38)中有关将 $\mathbf{M}=\mathbf{P} \mathbf{P}^{\top}$
代入的形式参见前面式(10.34)的解释最后一段。

### 10.8.6 式(10.39)的解释

式(10.39)是本节第二个 "度量学习"
的具体例子。优化目标函数是要求必连约束集合 $\mathcal{M}$
中的样本对之间的距离之和尽可能的小，而约束条件则是要求勿连约束集合
$\mathcal{C}$ 中的样本对之 间的距离之和大于 1 。

这里的 "1"应该类似于第 6 章 SVM 中间隔大于 "1", 纯属约定, 没有推导。

# 参考文献
[1] Michael Grant. Lagrangian optimization with matrix constrains, 2015.

[2] Wikipedia contributors. Frobenius inner product, 2020.

[3] Bernhard Schölkopf, Alexander Smola, and Klaus-Robert Müller. Kernel principal component analy-
sis. In Artificial Neural Networks—ICANN’97: 7th International Conference Lausanne, Switzerland,
October 8–10, 1997 Proceeedings, pages 583–588. Springer, 2005.
