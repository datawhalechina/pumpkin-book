```{important}
参与组队学习的同学须知：

本章学习时间：3天

本章配套视频教程：https://www.bilibili.com/video/BV1Mh411e7VU?p=17
```

# 第11章 特征选择与稀疏学习

## 11.1 子集搜索与评价

开篇给出了 "特征选择" 的概念, 并谈到特征选择与第 10
章的降维有相似的动机。特征选择与降维的区别在于特征选择是从所有特征中简单地选出相关特征,
选择出来的特征就 是原来的特征; 降维则对原来的特征进行了映射变换,
降维后的特征均不再是原来的特征。

本节涉及 "子集评价" 的式(14.1)和式(14.2)与第 4 章的式(4.2)和式(4.1)相同,
这是因为 "决策树算法在构建树的同时也可看作进行了特征选择" (参见 "11.7
阅读材料")。 接下来在 11.2节、11.3 节、11.4
节分别介绍的三类特征选择方法: 过滤式(filter)、包裹
式(wrapper)和嵌入式(embedding)。

### 11.1.1 式(11.1)的解释

此为信息增益的定义式，对数据集 $D$ 和属性子集 $A$, 假设根据 $A$ 的取值将
$D$ 分为了 $V$ 个子集 $\left\{D^1, D^2, \ldots, D^V\right\}$,
那么信息增益的定义为划分之前数据集 $D$ 的信息樀和划分之后每个子数据集
$D^v$ 的信息樀的差。樀用来衡量一个系统的混舌程度,
因此划分前和划分后樀的差越大, 表示划分越有效, 划分带来的"信息增
益"越大。

### 11.1.2 式(11.2)的解释



$$
\operatorname{Ent}(D)=-\sum_{i=1}^{| \mathcal{Y |}} p_{k} \log _{2} p_{k}
$$



此为信息熵的定义式，其中$p_k, k=1, 2, \dots \vert\mathcal{Y}\vert$表示$D$中第$i$类样本所占的比例。可以看出，样本越纯，即$p_k\rightarrow 0$或$p_k\rightarrow 1$时，$\mathrm{Ent}(D)$越小，其最小值为0。此时必有$p_i=1, p_{\backslash i}=0, i=1, 2, \dots, \vert\mathcal{Y}\vert$。

## 11.2 过滤式选择

"过滤式方法先对数据集进行特征选择, 然后再训练学习器,
特征选择过程与后续学习 器无关。这相当于先用特征选择过程对初始特征进行
'过滤', 再用过滤后的特征来训练模 型。", 这是本节开篇第一段原话,
之所以重写于此, 是因为这段话里包含了 "过滤"的概念,
该概念并非仅针对特征选择, 那些所有先对数据集进行某些预处理,
然后基于预处理结果再 训练学习器的方法 (预处理过程独立于训练学习器过程)
均可以称之为 "过滤式算法"。特 别地, 本节介绍的 Relief
方法只是过滤式特征选择方法的其中一种而已。

从式(11.3)可以看出, Relief 方法本质上基于
"空间上相近的样本具有相近的类别标记" 假设。Relief
基于样本与同类和异类的最近邻之间的距离来计算相关统计量 $\delta^j$,
越是满足前 提假设, 原则上样本与同类最近邻之间的距离
$\operatorname{diff}\left(x_i^j, x_{i, \mathrm{nh}}^j\right)^2$ 会越小,
样本与异类最近邻之 间的距离
$\operatorname{diff}\left(x_i^j, x_{i, \mathrm{~nm}}^j\right)^2$
会越大，因此相关统计量 $\delta^j$ 越大，对应属性的分类能力就越强。

对于能处理多分类问题的扩展变量 Relief-F, 由于有多个异类,
因此对所有异类最近邻 进行加权平均,
各异类的权重为其在数据集中所占的比例。

### 11.2.1 包裹式选择

"与过滤式特征选择不考虑后续学习器不同,
包裹式特征选择直接把最终将要使用的学
习器的性能作为特征子集的评价准则。换言之,
包裹式特征选择的目的就是为给定学习器选 择最有利于其性能、"量身定做'
的特征子集。", 这是本节开篇第一段原话, 之所以重写于 此,
是因为这段话里包含了 "包裹" 的概念, 该概念并非仅针对特征选择,
那些所有基于学 习器的性能作为评价准则对数据集进行预处理的方法
(预处理过程依赖训练所得学习器的 测试性能）均可以称之为
"包裹式算法"。特别地, 本节介绍的 LVW 方法只是包裹式特征
选择方法的其中一种而已。

图 $11.1$ 中, 第 1 行 $E=\infty$ 表示初始化学习器误差为无穷大, 以至于第
1 轮迭代第 9 行 的条件就一定为真; 第 2 行 $d=|A|$ 中的 $|A|$ 表示特征集
$A$ 的包含的特征个数; 第 9 行 $E^{\prime}<E$ 表示学习器 $\mathfrak{L}$
在特征子集 $A^{\prime}$ 上的误差比当前特征子集 $A$ 上的误差更小,
$\left(E^{\prime}=E\right) \vee\left(d^{\prime}<d\right)$ 表示学习器
$\mathfrak{L}$ 在特征子集 $A^{\prime}$ 上的误差与当前特征子集 $A$
上的误差相当 但 $A^{\prime}$ 中包含的特征数更小; 表示 "逻辑与"， $V$
表示 "逻辑或"。注意到, 第 5 行至第 17 行的while循环中 $t$ 并非一直增加,
当第 9 行条件满足时 $t$ 会被清零。

最后, 本节 LVW 算法基于拉斯维加斯方法框架,
可以仔细琢磨体会拉斯维加斯方法和
蒙特卡罗方法的区别。一个通俗的解释如下：

蒙特卡罗算法------采样越多, 越近似最优解；

拉斯维加斯算法------采样越多, 越有机会找到最优解。

举个例子, 假如筐里有 100 个苹果, 让我每次闭眼拿 1 个,
挑出最大的。于是我随机拿1个，再随机拿1个跟它比，留下大的，再随机拿1个\...\...我每拿一次，留下的苹果都至少不比上次的小。拿的次数越多,
挑出的苹果就越大, 但我除非拿 100 次, 否则无法肯定挑出
了最大的。这个挑苹果的算法, 就属于蒙特卡罗算法一一尽量找好的,
但不保证是最好的。 而拉斯维加斯算法, 则是另一种情况。假如有一把锁, 给我
100 把钥匙, 只有 1 把是对 的。于是我每次随机拿 1 把钥匙去试,
打不开就再换 1 把。我试的次数越多, 打开 (最优解) 的机会就越大,
但在打开之前, 那些错的钥匙都是没有用的。这个试钥匙的算法,
就是拉斯维加斯的一一尽量找最好的, 但不保证能找到。

## 11.3 嵌入式选择与L1正则化

"嵌入式特征选择是将特征选择过程与学习器训练过程融为一体，两者在同一个优化过程中完成，即在学习器训练过程中自动地进行了特征选择。"，具体可以对比本节式(11.7)的例子与前两节方法的本质区别，细细体会本节第一段的这句有关"嵌入式"的概念描述。

### 11.3.1 式(11.5)的解释

该式为线性回归的优化目标式，$y_i$表示样本$i$的真实值，而$w^\top x_i$表示其预测值，这里使用预测值和真实值差的平方衡量预测值偏离真实值的大小。

### 11.3.2 式(11.6)的解释

该式为加入了$\mathrm{L}_2$正规化项的优化目标，也叫"岭回归"，$\lambda$用来调节误差项和正规化项的相对重要性，引入正规化项的目的是为了防止$w$的分量过大而导致过拟合的风险。

### 11.3.3 式(11.7)的解释

该式将11.6中的$\mathrm{L}_2$正规化项替换成了$\mathrm{L}_1$正规化项，也叫LASSO回归。关于$\mathrm{L}_2$和$\mathrm{L}_1$两个正规化项的区别，"西瓜书"图11.2给出了很形象的解释。具体来说，结合$\mathrm{L}_1$范数优化的模型参数分量取值尽量稀疏，即非零分量个数尽量小，因此更容易取得稀疏解。

### 11.3.4 式(11.8)的解释

从本式开始至本节结束, 都在介绍近端梯度下降求解 $L_1$
正则化问题。若将本式对应到 式(11.7), 则本式中
$f(\boldsymbol{w})=\sum_{i=1}^m\left(y^i-\boldsymbol{w}^{\top} \boldsymbol{x}_i\right)^2$,
注意变量为 $\boldsymbol{w}$ （若感觉不习惯就将其用 $\boldsymbol{x}$
替换好了)。最终推导结果仅含 $f(\boldsymbol{w})$ 的一阶导数
$\nabla f(\boldsymbol{w})=-\sum_{i=1}^m 2\left(y^i-\boldsymbol{w}^{\top} \boldsymbol{x}_i\right) \boldsymbol{x}_i$
。

### 11.3.5 式(11.9)的解释

该式即为 $L$-Lipschitz(利普希茨)条件的定义。简单来说,
该条件约束函数的变化不能太快。将式(11.9)变形则更为直观 (注: 式中应该是 2
范数, 而非 2 范数平方):


$$
\frac{\left\|\nabla f\left(\boldsymbol{x}^{\prime}\right)-\nabla f(\boldsymbol{x})\right\|_2}{\left\|\boldsymbol{x}^{\prime}-\boldsymbol{x}\right\|_2} \leqslant L, \quad\left(\forall \boldsymbol{x}, \boldsymbol{x}^{\prime}\right)
$$



进一步地, 若 $\boldsymbol{x}^{\prime} \rightarrow \boldsymbol{x}$, 即


$$
\lim _{\boldsymbol{x}^{\prime} \rightarrow \boldsymbol{x}} \frac{\left\|\nabla f\left(\boldsymbol{x}^{\prime}\right)-\nabla f(\boldsymbol{x})\right\|_2}{\left\|\boldsymbol{x}^{\prime}-\boldsymbol{x}\right\|_2}
$$



这明显是在求解函数 $\nabla f(\boldsymbol{x})$ 的导数绝对值（模值。 因此,
式(11.9)即要求 $f(\boldsymbol{x})$ 的二阶导数不大于 $L$, 其中 $L$
称为Lipschitz常数。

"Lipschitz连续" 可以形象得理解为: 以陆地为例,
Lipschitz连续就是说这块地上没有特别陡的坡; 其中最陡的地方有多陡呢?
这就是所谓的Lipschitz常数。

### 11.3.6 式(11.10)的推导

首先注意优化目标式和11.7
LASSO回归的联系和区别，该式中的$x$对应到式11.7的$w$，即我们优化的目标。再解释下什么是$L\mathrm{-Lipschitz}$条件，根据维基百科的定义：它是一个比通常连续更强的光滑性条件。直觉上，利普希茨连续函数限制了函数改变的速度，符合利普希茨条件的函数的斜率，必小于一个称为利普希茨常数的实数（该常数依函数而定）。
注意这里存在一个笔误，在wiki百科的定义中，式11.9应该写成


$$
\left\vert\nabla f\left(\boldsymbol{x}^{\prime}\right)-\nabla f(\boldsymbol{x})\right\vert \leqslant L\left\vert\boldsymbol{x}^{\prime}-\boldsymbol{x}\right\vert \quad\left(\forall \boldsymbol{x}, \boldsymbol{x}^{\prime}\right)
$$



移项得


$$
\frac{\left|\nabla f\left(\boldsymbol{x}^{\prime}\right)-\nabla f(\boldsymbol{x})\right|}{\vert x^\prime - x\vert}\leqslant L \quad\left(\forall \boldsymbol{x}, \boldsymbol{x}^{\prime}\right)
$$



由于上式对所有的$x, x^\prime$都成立，由导数的定义，上式可以看成是$f(x)$的二阶导数恒不大于$L$。即


$$
\nabla^2f(x)\leqslant L
$$


 得到这个结论之后，我们来推导式11.10。
由泰勒公式，$x_k$附近的$f(x)$通过二阶泰勒展开式可近似为


$$
\begin{aligned}
\hat{f}(\boldsymbol{x}) & \simeq f\left(\boldsymbol{x}_{k}\right)+\left\langle\nabla f\left(\boldsymbol{x}_{k}\right), \boldsymbol{x}-\boldsymbol{x}_{k}\right\rangle+\frac{\nabla^2f(x_k)}{2}\left\|\boldsymbol{x}-\boldsymbol{x}_{k}\right\|^{2} \\
&\leqslant
 f\left(\boldsymbol{x}_{k}\right)+\left\langle\nabla f\left(\boldsymbol{x}_{k}\right), \boldsymbol{x}-\boldsymbol{x}_{k}\right\rangle+\frac{L}{2}\left\|\boldsymbol{x}-\boldsymbol{x}_{k}\right\|^{2} \\
&= f\left(\boldsymbol{x}_{k}\right)+\nabla f\left(\boldsymbol{x}_{k}\right)^{\top}\left(\boldsymbol{x}-\boldsymbol{x}_{k}\right)+\frac{L}{2}\left(\boldsymbol{x}-\boldsymbol{x}_{k}\right)^{\top}\left(\boldsymbol{x}-\boldsymbol{x}_{k}\right)\\
&=f(x_k)+\frac{L}{2}\left(\left(\boldsymbol{x}-\boldsymbol{x}_{k}\right)^{\top}\left(\boldsymbol{x}-\boldsymbol{x}_{k}\right)+\frac{2}{L}\nabla f\left(\boldsymbol{x}_{k}\right)^{\top}\left(\boldsymbol{x}-\boldsymbol{x}_{k}\right)\right)\\
&=f(x_k)+\frac{L}{2}\left(\left(\boldsymbol{x}-\boldsymbol{x}_{k}\right)^{\top}\left(\boldsymbol{x}-\boldsymbol{x}_{k}\right)+\frac{2}{L}\nabla f\left(\boldsymbol{x}_{k}\right)^{\top}\left(\boldsymbol{x}-\boldsymbol{x}_{k}\right)+\frac{1}{L^2}\nabla f(x_k)^\top\nabla f(x_k)\right) -\frac{1}{2L}\nabla f(x_k)^\top\nabla f(x_k)\\
&=f(x_k)+\frac{L}{2}\left(\left(\boldsymbol{x}-\boldsymbol{x}_{k}\right)+\frac{1}{L} \nabla f\left(\boldsymbol{x}_{k}\right)\right)^{\top}\left(\left(\boldsymbol{x}-\boldsymbol{x}_{k}\right)+\frac{1}{L} \nabla f\left(\boldsymbol{x}_{k}\right)\right)-\frac{1}{2L}\nabla f(x_k)^\top\nabla f(x_k)\\
&=\frac{L}{2}\left\|\boldsymbol{x}-\left(\boldsymbol{x}_{k}-\frac{1}{L} \nabla f\left(\boldsymbol{x}_{k}\right)\right)\right\|_{2}^{2}+\mathrm{const}
\end{aligned}
$$



其中$\mathrm{const}=f(x_k)-\frac{1}{2 L} \nabla f\left(x_{k}\right)^{\top} \nabla f\left(x_{k}\right)$

### 11.3.7 式(11.11)的解释

这个很容易理解，因为2范数的最小值为0，当$\boldsymbol{x}_{k+1}=\boldsymbol{x}_{k}-\frac{1}{L} \nabla f\left(\boldsymbol{x}_{k}\right)$时，$\hat{f}(x_{k+1})\leqslant\hat{f}(x_k)$恒成立，同理$\hat{f}(x_{k+2})\leqslant\hat{f}(x_{k+1}), \cdots$，因此反复迭代能够使$\hat{f}(x)$的值不断下降。

### 11.3.8 式(11.12)的解释

注意 $\hat{f}(\boldsymbol{x})$ 在式(11.11)处取得最小值, 因此,
以下不等式肯定成立:


$$
\hat{f}\left(\boldsymbol{x}_k-\frac{1}{L} \nabla f\left(\boldsymbol{x}_k\right)\right) \leqslant \hat{f}\left(\boldsymbol{x}_k\right)
$$




在式(11.10)推导中有
$f(\boldsymbol{x}) \leqslant \hat{f}(\boldsymbol{x})$ 恒成立, 因此,
以下不等式肯定成立:


$$
f\left(\boldsymbol{x}_k-\frac{1}{L} \nabla f\left(\boldsymbol{x}_k\right)\right) \leqslant \hat{f}\left(\boldsymbol{x}_k-\frac{1}{L} \nabla f\left(\boldsymbol{x}_k\right)\right)
$$




在式(11.10)推导中还知道
$f\left(\boldsymbol{x}_k\right)=\hat{f}\left(\boldsymbol{x}_k\right)$,
因此


$$
f\left(\boldsymbol{x}_k-\frac{1}{L} \nabla f\left(\boldsymbol{x}_k\right)\right) \leqslant \hat{f}\left(\boldsymbol{x}_k-\frac{1}{L} \nabla f\left(\boldsymbol{x}_k\right)\right) \leqslant \hat{f}\left(\boldsymbol{x}_k\right)=f\left(\boldsymbol{x}_k\right)
$$




也就是说通过迭代
$\boldsymbol{x}_{k+1}=\boldsymbol{x}_k-\frac{1}{L} \nabla f\left(\boldsymbol{x}_k\right)$
可以使 $f(\boldsymbol{x})$ 的函数值逐步下降。

同理, 对于函数
$g(\boldsymbol{x})=f(\boldsymbol{x})+\lambda\|\boldsymbol{x}\|_1$,
可以通过最小化
$\hat{g}(\boldsymbol{x})=\hat{f}(\boldsymbol{x})+\lambda\|\boldsymbol{x}\|_1$
逐步 求解。式(11.12)就是在最小化
$\hat{g}(\boldsymbol{x})=\hat{f}(\boldsymbol{x})+\lambda\|\boldsymbol{x}\|_{1^{\circ}}$
。

以上优化方法被称为Majorization-Minimization。可以搜索相关资料做详细了解。

### 11.3.9 式(11.13)的解释

这里将式11.12的优化步骤拆分成了两步，首先令$z=x_{k}-\frac{1}{L} \nabla f\left(x_{k}\right)$以计算$z$，然后再求解式11.13，得到的结果是一致的。

### 11.3.10 式(11.14)的推导

令优化函数 

$$
\begin{aligned}
g(\boldsymbol{x}) &=\frac{L}{2}\|\boldsymbol{x}-\boldsymbol{z}\|_{2}^{2}+\lambda\|\boldsymbol{x}\|_{1} \\
&=\frac{L}{2} \sum_{i=1}^{d}\left\|x^{i}-z^{i}\right\|_{2}^{2}+\lambda \sum_{i=1}^{d}\left\|x^{i}\right\|_{1} \\
&=\sum_{i=1}^{d}\left(\frac{L}{2}\left(x^{i}-z^{i}\right)^{2}+\lambda\left|x^{i}\right|\right)
\end{aligned}
$$




这个式子表明优化$g(\boldsymbol{x})$可以被拆解成优化$\boldsymbol{x}$的各个分量的形式，对分量$x^i$，其优化函数


$$
g\left(x^{i}\right)=\frac{L}{2}\left(x^{i}-z^{i}\right)^{2}+\lambda\left|x^{i}\right|
$$




求导得


$$
\frac{d g\left(x^{i}\right)}{d x^{i}}=L\left(x^{i}-z^{i}\right)+\lambda s g n\left(x^{i}\right)
$$




其中 

$$
\operatorname{sign}\left(x^{i}\right)=\left\{\begin{array}{ll}
{1,} & {x^{i}>0} \\
{-1,} & {x^{i}<0}
\end{array}\right.
$$




称为符号函数[1]，对于$x^i=0$的特殊情况，由于$\vert x^i \vert$在$x^i=0$点出不光滑，所以其不可导，需单独讨论。令$\frac{d g\left(x^{i}\right)}{d x^{i}}=0$有


$$
x^{i}=z^{i}-\frac{\lambda}{L} \operatorname{sign}\left(x^{i}\right)
$$




此式的解即为优化目标$g(x^i)$的极值点，因为等式两端均含有未知变量$x^i$，故分情况讨论。

（1）当$z^i>\frac{\lambda}{L}$时：a. 假设$x^i<0$，则$\operatorname{sign}(x^i)=-1$，那么有$x^i=z^i+\frac{\lambda}{L}>0$与假设矛盾；b. 假设$x^i>0$，则$\operatorname{sign}(x^i)=1$，那么有$x^i=z^i-\frac{\lambda}{L}>0$和假设相符和，下面来检验$x^i=z^i-\frac{\lambda}{L}$是否是使函数$g(x^i)$的取得最小值。当$x^i>0$时，

$$
\frac{d g\left(x^{i}\right)}{d x^{i}}=L\left(x^{i}-z^{i}\right)+\lambda
$$
在定义域内连续可导，则$g(x^i)$的二阶导数
    

$$
\frac{d^2 g\left(x^{i}\right)}{{d x^{i}}^2}=L
$$


由于$L$是Lipschitz常数恒大于0，因为$x^i=z^i-\frac{\lambda}{L}$是函数$g(x^i)$的最小值。

（2）当$z^i<-\frac{\lambda}{L}$时：a. 假设$x^i>0$，则$\operatorname{sign}(x^i)=1$，那么有$x^i=z^i-\frac{\lambda}{L}<0$与假设矛盾；b. 假设$x^i<0$，则$\operatorname{sign}(x^i)=-1$，那么有$x^i=z^i+\frac{\lambda}{L}<0$与假设相符，由上述二阶导数恒大于0可知，$x^i=z^i+\frac{\lambda}{L}$是$g(x^i)$的最小值。

（3）当$-\frac{\lambda}{L} \leqslant z^i \leqslant \frac{\lambda}{L}$时：a. 假设$x^i>0$，则$\operatorname{sign}(x^i)=1$，那么有$x^i=z^i-\frac{\lambda}{L}\leqslant 0$与假设矛盾；b. 假设$x^i<0$，则$\operatorname{sign}(x^i)=-1$，那么有$x^i=z^i+\frac{\lambda}{L}\geqslant 0$与假设矛盾。

（4）最后讨论$x^i=0$的情况，此时$g(x^i)=\frac{L}{2}\left({z^i}\right)^2$。当$\vert z^i\vert>\frac{\lambda}{L}$时，由上述推导可知$g(x^i)$的最小值在$x^i=z^i-\frac{\lambda}{L}$处取得，因为
        
$$
\begin{aligned}
           g(x^i)\vert_{x^i=0}-g(x^i)\vert_{x^i=z^i-\frac{\lambda}{L}}
           &=\frac{L}{2}\left({z^i}\right)^2 - \left(\lambda z^i-\frac{\lambda^2}{2L}\right)\\
           &=\frac{L}{2}\left(z^i-\frac{\lambda}{L}\right)^2\\
           &>0
           \end{aligned}
$$

因此当$\vert z^i\vert>\frac{\lambda}{L}$时，$x^i=0$不会是函数$g(x^i)$的最小值。当$-\frac{\lambda}{L} \leqslant z^i \leqslant \frac{\lambda}{L}$时，对于任何$\Delta x\neq 0$有

$$
\begin{aligned}
           g(\Delta x) &=\frac{L}{2}\left(\Delta x-z^{i}\right)^{2}+\lambda|\Delta x| \\
           &=\frac{L}{2}\left((\Delta x)^{2}-2 \Delta x \cdot z^{i}+\frac{2 \lambda}{L}|\Delta x|\right)+\frac{L}{2}\left(z^{i}\right)^{2} \\
           &\ge\frac{L}{2}\left((\Delta x)^{2}-2 \Delta x \cdot z^{i}+\frac{2 \lambda}{L}\Delta x\right)+\frac{L}{2}\left(z^{i}\right)^{2}\\
           &\ge\frac{L}{2}\left(\Delta x\right)^2+\frac{L}{2}\left(z^{i}\right)^{2}\\
           &>g(x^i)\vert_{x^i=0}
           \end{aligned}
$$

因此$x^i=0$是$g(x^i)$的最小值点。

综上所述，11.14成立。

该式称为软阈值(Soft Thresholding)函数，很常见，建议掌握。另外，常见的变形是式(11.13)中的$L=1$或$L=2$时的形式，其解直接代入式(11.14)即可。与软阈值函数相对的是硬阈值函数，是将式(11.13)中的1范数替换为0范数的优化问题的闭式解。

## 11.4 稀疏表示与字典学习

稀疏表示与字典学习实际上是信号处理领域的概念。本节内容核心就是K-SVD算法。

### 11.4.1 式(11.15)的解释

这个式子表达的意思很容易理解，即希望样本$x_i$的稀疏表示$\boldsymbol{\alpha}_i$通过字典$\mathbf{B}$重构后和样本$x_i$的原始表示尽量相似，如果满足这个条件，那么稀疏表示$\boldsymbol{\alpha}_i$是比较好的。后面的1范数项是为了使表示更加稀疏。

### 11.4.2 式(11.16)的解释

为了优化11.15，我们采用变量交替优化的方式(有点类似EM算法)，首先固定变量$\mathbf{B}$，则11.15求解的是$m$个样本相加的最小值，因为公式里没有样本之间的交互(即文中所述$\alpha_{i}^{u} \alpha_{i}^{v}(u \neq v)$这样的形式)，因此可以对每个变量做分别的优化求出$\boldsymbol{\alpha}_i$，求解方法见式(11.13)，式(11.14)。

### 11.4.3 式(11.17)的推导

这是优化11.15的第二步，固定住$\boldsymbol{\alpha}_i, i=1, 2,\dots,m$，此时式11.15的第二项为一个常数，优化11.15即优化$\min _{\mathbf{B}} \sum_{i=1}^{m}\left\|\boldsymbol{x}_{i}-\mathbf{B} \boldsymbol{\alpha}_{i}\right\|_{2}^{2}$。其写成矩阵相乘的形式为$\min _{\mathbf{B}}\|\mathbf{X}-\mathbf{B} \mathbf{A}\|_{2}^{2}$，将2范数扩展到$F$范数即得优化目标为$\min _{\mathbf{B}}\|\mathbf{X}-\mathbf{B} \mathbf{A}\|_{F}^{2}$。

### 11.4.4 式(11.18)的推导

这个公式难点在于推导$\mathbf{B}\mathbf{A}=\sum_{j=1}^k\boldsymbol{b}_j\boldsymbol{\alpha}^j$。大致的思路是$\boldsymbol{b}_{j} \boldsymbol{\alpha}^{j}$会生成和矩阵$\mathbf{B}\mathbf{A}$同样维度的矩阵，这个矩阵对应位置的元素是$\mathbf{B}\mathbf{A}$中对应位置元素的一个分量，这样的分量矩阵一共有$k$个，把所有分量矩阵加起来就得到了最终结果。推导过程如下：


$$
\begin{aligned}
\boldsymbol B\boldsymbol A
& =\begin{bmatrix}
b_{1}^{1} &b_{2}^{1}  & \cdot  & \cdot  & \cdot  & b_{k}^{1}\\ 
b_{1}^{2} &b_{2}^{2}  & \cdot  & \cdot  & \cdot  & b_{k}^{2}\\ 
\cdot  & \cdot  & \cdot  &  &  & \cdot \\ 
\cdot  &  \cdot &  & \cdot  &  &\cdot  \\ 
 \cdot & \cdot  &  &  & \cdot  & \cdot \\ 
 b_{1}^{d}& b_{2}^{d}  & \cdot  & \cdot  &\cdot   &  b_{k}^{d}
\end{bmatrix}_{d\times k}\cdot 
\begin{bmatrix}
\alpha_{1}^{1} &\alpha_{2}^{1}  & \cdot  & \cdot  & \cdot  & \alpha_{m}^{1}\\ 
\alpha_{1}^{2} &\alpha_{2}^{2}  & \cdot  & \cdot  & \cdot  & \alpha_{m}^{2}\\ 
\cdot  & \cdot  & \cdot  &  &  & \cdot \\ 
\cdot  &  \cdot &  & \cdot  &  &\cdot  \\ 
 \cdot & \cdot  &  &  & \cdot  & \cdot \\ 
 \alpha_{1}^{k}& \alpha_{2}^{k}  & \cdot  & \cdot  &\cdot   &  \alpha_{m}^{k}
\end{bmatrix}_{k\times m} \\
& =\begin{bmatrix}
\sum_{j=1}^{k}b_{j}^{1}\alpha _{1}^{j} &\sum_{j=1}^{k}b_{j}^{1}\alpha _{2}^{j} & \cdot  & \cdot  & \cdot  & \sum_{j=1}^{k}b_{j}^{1}\alpha _{m}^{j}\\ 
\sum_{j=1}^{k}b_{j}^{2}\alpha _{1}^{j} &\sum_{j=1}^{k}b_{j}^{2}\alpha _{2}^{j}  & \cdot  & \cdot  & \cdot  & \sum_{j=1}^{k}b_{j}^{2}\alpha _{m}^{j}\\ 
\cdot  & \cdot  & \cdot  &  &  & \cdot \\ 
\cdot  &  \cdot &  & \cdot  &  &\cdot  \\ 
 \cdot & \cdot  &  &  & \cdot  & \cdot \\ 
\sum_{j=1}^{k}b_{j}^{d}\alpha _{1}^{j}& \sum_{j=1}^{k}b_{j}^{d}\alpha _{2}^{j}  & \cdot  & \cdot  &\cdot   &  \sum_{j=1}^{k}b_{j}^{d}\alpha _{m}^{j}
\end{bmatrix}_{d\times m} &
\end{aligned}
$$






$$
\begin{aligned}
\boldsymbol b_{\boldsymbol j}\boldsymbol \alpha ^{\boldsymbol j}
& =\begin{bmatrix}
b_{j}^{1}\\ b_{j}^{2}
\\ \cdot 
\\ \cdot 
\\ \cdot 
\\ b_{j}^{d}
\end{bmatrix}\cdot 
\begin{bmatrix}
 \alpha _{1}^{j}& \alpha _{2}^{j} & \cdot  & \cdot  & \cdot  & \alpha _{m}^{j}
\end{bmatrix}\\
& =\begin{bmatrix}
b_{j}^{1}\alpha _{1}^{j} &b_{j}^{1}\alpha _{2}^{j} & \cdot  & \cdot  & \cdot  & b_{j}^{1}\alpha _{m}^{j}\\ 
b_{j}^{2}\alpha _{1}^{j} &b_{j}^{2}\alpha _{2}^{j}  & \cdot  & \cdot  & \cdot  & b_{j}^{2}\alpha _{m}^{j}\\ 
\cdot  & \cdot  & \cdot  &  &  & \cdot \\ 
\cdot  &  \cdot &  & \cdot  &  &\cdot  \\ 
 \cdot & \cdot  &  &  & \cdot  & \cdot \\ 
b_{j}^{d}\alpha _{1}^{j}& b_{j}^{d}\alpha _{2}^{j}  & \cdot  & \cdot  &\cdot   &  b_{j}^{d}\alpha _{m}^{j}
\end{bmatrix}_{d\times m} &
\end{aligned}
$$




求和可得： 

$$
\begin{aligned}
\sum_{j=1}^{k}\boldsymbol b_{\boldsymbol j}\boldsymbol \alpha ^{\boldsymbol j} 
& = \sum_{j=1}^{k}\left (\begin{bmatrix}
b_{j}^{1}\\ b_{j}^{2}
\\ \cdot 
\\ \cdot 
\\ \cdot 
\\ b_{j}^{d}
\end{bmatrix}\cdot 
\begin{bmatrix}
 \alpha _{1}^{j}& \alpha _{2}^{j} & \cdot  & \cdot  & \cdot  & \alpha _{m}^{j}
\end{bmatrix} \right )\\
& =\begin{bmatrix}
\sum_{j=1}^{k}b_{j}^{1}\alpha _{1}^{j} &\sum_{j=1}^{k}b_{j}^{1}\alpha _{2}^{j} & \cdot  & \cdot  & \cdot  & \sum_{j=1}^{k}b_{j}^{1}\alpha _{m}^{j}\\ 
\sum_{j=1}^{k}b_{j}^{2}\alpha _{1}^{j} &\sum_{j=1}^{k}b_{j}^{2}\alpha _{2}^{j}  & \cdot  & \cdot  & \cdot  & \sum_{j=1}^{k}b_{j}^{2}\alpha _{m}^{j}\\ 
\cdot  & \cdot  & \cdot  &  &  & \cdot \\ 
\cdot  &  \cdot &  & \cdot  &  &\cdot  \\ 
 \cdot & \cdot  &  &  & \cdot  & \cdot \\ 
\sum_{j=1}^{k}b_{j}^{d}\alpha _{1}^{j}& \sum_{j=1}^{k}b_{j}^{d}\alpha _{2}^{j}  & \cdot  & \cdot  &\cdot   &  \sum_{j=1}^{k}b_{j}^{d}\alpha _{m}^{j}
\end{bmatrix}_{d\times m} &
\end{aligned}
$$


 得证。

将矩阵$\mathbf{B}$分解成矩阵列$\boldsymbol{b}_j,j=1,2,\dots,k$带来一个好处，即和11.16的原理相同，矩阵列与列之间无关，因此可以分别优化各个列，即将$\min\limits_\mathbf{B}\Vert\dots\mathbf{B}\dots\Vert^2_F$转化成了$\min\limits_{b_i}\Vert\dots\boldsymbol{b}_i\dots\Vert^2_F$，得到第三行的等式之后，再利用文中介绍的K-SVD算法求解即可。

## 11.5 K-SVD算法

本节前半部分铺垫概念，后半部分核心就是K-SVD。作为字典学习的最经典的算法，K-SVD[2]自2006年发表以来已逾万次引用。理解K-SVD的基础是SVD，即奇异值分解，参见"西瓜书"附录A.3。

对于任意实矩阵 $\mathbf{A} \in \mathbb{R}^{m \times n}$, 都可分解为
$\mathbf{A}=\mathbf{U} \boldsymbol{\Sigma} \mathbf{V}^{\top}$, 其中
$\mathbf{U} \in \mathbb{R}^{m \times m}, \mathbf{V} \in \mathbb{R}^{n \times n}$,
分别为 $m$ 阶和 $n$ 阶正交矩阵 (复数域时称为酉矩阵), 即
$\mathbf{U}^{\top} \mathbf{U}=\mathbf{I}, \mathbf{V}^{\top} \mathbf{V}=\mathbf{I}$
(逆矩阵等于 自身的转置),
$\boldsymbol{\Sigma} \in \mathbb{R}^{m \times n}$, 且除
$(\boldsymbol{\Sigma})_{i i}=\sigma_i$ 之外其它位置的元素均为零,
$\sigma_i$ 称为奇异值, 可以证明, 矩阵 $\mathrm{A}$
的秩等于非零奇异值的个数。

正如西瓜书附录 A.3 所述, K-SVD 分解中主要使用 SVD
解决低秩矩阵近似问题。之所 以称为 K-SVD, 原文献中专门有说明:

We shall call this algorithm \"K-SVD\" to parallel the name
$\mathrm{K}$-means. While K-means applies K computations of means to
update the codebook, K-SVD obtains the updated dictionary by K SVD
computations, each determining one column.

具体来说, 就是原文献中的字典共有 $\mathrm{K}$ 个原子 (列), 因此需要迭代
$\mathrm{K}$ 次，这类似于 $\mathrm{K}$均值算法欲将数据聚成 $\mathrm{K}$
个簇, 需要计算 $\mathrm{K}$ 次均值。

K-SVD算法伪代码详如图11-1所示, 其中符号与本节符号有差异。具体来说,
原文献中字典矩阵用 $\mathbf{D}$ 表示 (书中用 $\mathrm{B}$ ), 稀疏系数用
$\mathbf{x}_i$ 表示 (书中用 $\boldsymbol{\alpha}_i$ ), 数据集用
$\mathbf{Y}$ 表示 (书中用$\mathrm{X}$)。

![图11-1 K-SVD算法在论文中的描述](https://datawhale-business.oss-cn-hangzhou.aliyuncs.com/3659/dashboard/1729869698433/ksvd.svg)

在初始化字典矩阵D以后, K-SVD 算法迭代过程分两步: 第 1 步 Sparse Coding
Stage 就是普通的已知字典矩阵 $\mathbf{D}$ 的稀疏表示问题,
可以使用很多现成的算法完成此步, 不再详述; K-SVD 的核心创新点在第 2 步
Codebook Update Stage, 在该步骤中分 K 次分别更新字典矩 阵 $\mathbf{D}$
中每一列, 更新第 $k$ 列 $\mathbf{d}_k$ 时其它各列都是固定的,
如原文献式(21)所示: 

$$
\begin{aligned}
\|\mathbf{Y}-\mathbf{D X}\|_F^2 & =\left\|\mathbf{Y}-\sum_{j=1}^K \mathbf{d}_j \mathbf{x}_T^j\right\|_F^2 \\
& =\left\|\left(\mathbf{Y}-\sum_{j \neq k} \mathbf{d}_j \mathbf{x}_T^j\right)-\mathrm{d}_k \mathbf{x}_T^k\right\|_F^2 \\
& =\left\|\mathbf{E}_k-\mathbf{d}_k \mathrm{x}_T^k\right\|_F^2 .
\end{aligned}
$$




注意, 矩阵 $\mathbf{d}_k \mathbf{x}_T^k$ 的秩为 1 (其中,
$\mathbf{x}_T^k$ 表示稀疏系数矩阵 $\mathbf{X}$ 的第 $k$ 行, 以区别于其第
$k$ 列 $\mathbf{x}_k$ ), 对比西瓜书附录 A.3 中的式(A.34),
这就是一个低秩矩阵近似问题, 即对于给定矩阵 $\mathbf{E}_k$, 求 其最优 1
秩近似矩阵 $\mathbf{d}_k \mathbf{x}_T^k$; 此时可对 $\mathbf{E}_k$ 进行
SVD 分解, 类似于西瓜书附录式(A.35), 仅保 留最大的 1 个奇异值; 具体来说
$\mathbf{E}_k=\mathbf{U} \Delta \mathbf{V}^{\top}$, 仅保留 $\Delta$
中最大的奇异值 $\Delta(1,1)$, 则
$\mathbf{d}_k \mathbf{x}_T^k=\mathbf{U}_1 \boldsymbol{\Delta}(1,1) \mathbf{V}_1^{\top}$,
其 中 $\mathbf{U}_1, \mathbf{V}_1$ 分 别 为 $\mathbf{U}, \mathbf{V}$ 的
第 1 列, 此时 $\mathbf{d}_k=\mathbf{U}_1$,
$\mathbf{x}_T^k=\boldsymbol{\Delta}(1,1) \mathbf{V}_1^{\top}$
。但这样更新会破坏第 1 步中得到的稀疏系数的稀疏性 !

为了保证第 1 步中得到的稀疏系数的稀疏性, K-SVD 并不直接对 $\mathbf{E}_k$
进行 SVD 分解, 而是根据 $\mathbf{x}_T^k$ 仅取出与 $\mathrm{x}_T^k$
非零元素对应的部分列, 例如行向量 $\mathbf{x}_T^k$ 只有第 1、3、5、8、9
个元 素非零, 则仅取出 $\mathbf{E}_k$ 的第 1、3、5、8、9 列组成矩阵进行
$\operatorname{SVD}$ 分解
$\mathbf{E}_k^R=\mathbf{U} \Delta \mathrm{V}^{\top}$, 则


$$
\tilde{\mathbf{d}}_k=\mathbf{U}_1, \quad \tilde{\mathbf{x}}_T^k=\boldsymbol{\Delta}(1,1) \mathbf{V}_1^{\top}
$$




即得到更新后的 $\tilde{\mathbf{d}}_k$ 和 $\tilde{\mathbf{x}}_T^k$ (注意,
此时的行向量 $\tilde{\mathbf{x}}_T^k$ 长度仅为原 $\mathbf{x}_T^k$
非零元素个数, 需要按原 $\mathbf{x}_T^k$ 对其余位置填 0)。如此迭代 K
次即得更新后的字典矩阵 $\tilde{\mathbf{D}}$, 以供下一轮 Sparse Coding
使用。 $\mathrm{K}-\mathrm{SVD}$ 原文献中特意提到, 在 K
次迭代中要使用最新的稀疏系数 $\tilde{\mathbf{x}}_T^k$,
但并没有说是否要用 最新的 $\tilde{\mathbf{d}}_k$ (推测应该也要用最新的
$\tilde{\mathbf{d}}_k$ )。

## 11.6 压缩感知

虽然压缩感知与稀疏表示关系密切, 但它是彻彻底底的信号处理领域的概念。
"西瓜书"在本章有几个专业术语翻译与信号处理领域人士的习惯翻译略不一样：比如第
258 页的 Restricted Isometry Property (RIP)"西瓜书"翻译为 "限定等距性",
信号处理领域一般翻译为 "有限等距性质"; 第 259 页的 Basis Pursuit
De-Noising、第 261 页 的 Basis Pursuit 和 Matching Pursuit 中的
"Pursuit" "西瓜书"翻译为 "寻踪", 信号处理领域一般 翻译为 "追踪",
即基追踪降噪、基追踪、匹配追踪。

### 11.6.1 式(11.21)的解释

将式(11.21)进行变形


$$
\left(1-\delta_k\right) \leqslant \frac{\left\|\mathbf{A}_k \boldsymbol{s}\right\|_2^2}{\|\boldsymbol{s}\|_2^2} \leqslant\left(1+\delta_k\right)
$$



注意不等式中间, 若 $s$ 为输入信号, 则分母 $\|\boldsymbol{s}\|_2^2$
为输入信号的能量, 分子 $\left\|\mathrm{A}_k \boldsymbol{s}\right\|_2^2$
为对应的 观测信号的能量, 即 RIP
要求观测信号与输入信号的能量之比在一定的范围之内; 例如当 $\delta_k$ 等于
0 时, 观测信号与输入信号的能量相等,
即实现了等距变换，相关文献可以参考[3];
RIP 放松了对矩阵A 的约 束 (而且 $\mathrm{A}$ 并非方阵), 因此称为 "有限"
等距性质。

### 11.6.2 式(11.25)的解释

该式即为核范数定义: 矩阵的核范数（迹范数）为矩阵的奇异值之和。

有关 "凸包" 的概念, 引用百度百科里的两句原话: 在二维欧几里得空间中,
凸包可想 象为一条刚好包著所有点的橡皮圈; 用不严谨的话来讲,
给定二维平面上的点集, 凸包就是将最外层的点连接起来构成的凸多边形,
它能包含点集中所有的点。

个人理解, 将 $\operatorname{rank}(\mathbf{X})$ 的 "凸包" 是 $\mathbf{X}$
的核范数 $\|\mathbf{X}\|_*$ 这件事简单理解为 $\|\mathbf{X}\|_*$ 是
$\operatorname{rank}(\mathbf{X})$ 的上限即可, 即 $\|\mathbf{X}\|_*$
恒大于 $\operatorname{rank}(\mathbf{X})$, 类似于式(11.10)中的式子恒大于
$f(\boldsymbol{x})$ 。

## 参考文献
[1] Wikipedia contributors. Sign function, 2020.

[2] Michal Aharon, Michael Elad, and Alfred Bruckstein. K-svd: An algorithm for designing overcomplete
dictionaries for sparse representation. IEEE Transactions on signal processing, 54(11):4311–4322, 2006.

[3] 杨孝春. 欧氏空间中的等距变换与等距映射. 四川工业学院学报, 1999.
