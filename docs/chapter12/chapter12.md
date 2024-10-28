# 第12章 计算学习理论

正如本章开篇所述，计算学习理论研究目的是分析学习任务的困难本质，为学习算法提供理论保证，并根据分析结果指导算法设计。例如，"西瓜书"定理12.1、定理12.3、定理12.6所表达意思的共同点是，泛化误差与经验误差之差的绝对值以很大概率($1-\delta$)很小，且这个差的绝对值随着训练样本个数($m$)的增加而减小，随着模型复杂度（定理12.1为假设空间包含的假设个数$\vert\mathcal{H}\vert$，定理12.3中为假设空间的VC维，定理12.6中为(经验)Rademacher复杂度）的减小而减小。因此，若想要得到一个泛化误差很小的模型，足够的训练样本是前提，最小化经验误差是实现途径，另外还要选择性能相同的模型中模型复杂度最低的那一个；"最小化经验误差"即常说的经验风险最小化，"选择模型复杂度最低的那一个"即结构风险最小化，可以参见"西瓜书"6.4节最后一段的描述，尤其是式(6.42)所表达的含义。

## 12.1 基础知识

统计学中有总体集合和样本集合之分,
比如要统计国内本科生对机器学习的掌握情况,
此时全国所有的本科生就是总体集合,
但总体集合往往太大而不具有实际可操作性, 一般都 是取总体集合的一部分,
比如从双一流 $\mathrm{A}$ 类、双一流 $\mathrm{B}$
类、一流学科建设高校、普通高校 中各找一部分学生 (即样本集合)进行调研,
以此来了解国内本科生对机器学习的掌握情况。 在机器学习中, 样本空间 (参见
$1.2$ 节) 对应总体集合, 而我们手头上的样例集 $D$ 对应 样本集合, 样例集
$D$ 是从样本空间中采样而得, 分布 $\mathcal{D}$
可理解为当从样本空间采样获得样例 集 $D$ 时每个样本被采到的概率, 我们用
$\mathcal{D}(t)$ 表示样本空间第 $t$ 个样本被采到的概率。

### 12.1.1 式(12.1)的解释

该式为泛化误差的定义式，所谓泛化误差，是指当样本$x$从真实的样本分布$\mathcal{D}$中采样后其预测值$h(\boldsymbol{x})$不等于真实值$y$的概率。在现实世界中，我们很难获得样本分布$\mathcal{D}$，我们拿到的数据集可以看做是从样本分布$\mathcal{D}$中独立同分布采样得到的。在西瓜书中，我们拿到的数据集，称为样例集$D$\[也叫观测集、样本集，注意与花体$\mathcal{D}$的区别\]。

### 12.1.2 式(12.2)的解释

该式为经验误差的定义式，所谓经验误差，是指观测集$D$中的样本$x_i, i=1,2,\cdots,m$的预测值$h(\boldsymbol{x}_i)$和真实值$y_i$的期望误差。

### 12.1.3 式(12.3)的解释

假设我们有两个模型$h_1$和$h_2$，将它们同时作用于样本$\boldsymbol{x}$上，那么他们的"不合"度定义为这两个模型预测值不相同的概率。

### 12.1.4 式(12.4)的解释

Jensen不等式：这个式子可以做很直观的理解，比如说在二维空间上，凸函数可以想象成开口向上的抛物线，假如我们有两个点$x_1, x_2$，那么$f(\mathbb{E}(x))$表示的是两个点的均值的纵坐标，而$\mathbb{E}(f(x))$表示的是两个点纵坐标的均值，因为两个点的均值落在抛物线的凹处，所以均值的纵坐标会小一些。

### 12.1.5 式(12.5)的解释

随机变量的观测值是随机的, 进一步地, 随机过程的每个时刻都是一个随机变量。

式中, $\frac{1}{m} \sum_{i=1}^m x_i$ 表示 $m$
个独立随机变量各自的某次观测值的平均,
$\frac{1}{m} \sum_{i=1}^m \mathbb{E}\left(x_i\right)$ 表示 $m$
个独立随机变量各自的期望的平均。

式(12.5)表示事件
$\frac{1}{m} \sum_{i=1}^m x_i-\frac{1}{m} \sum_{i=1}^m \mathbb{E}\left(x_i\right) \geqslant \epsilon$
出现的概率不大于 (i.e., $\left.\leqslant\right) e^{-2 m \epsilon^2}$;
式(12.6)的事件
$\left|\frac{1}{m} \sum_{i=1}^m x_i-\frac{1}{m} \sum_{i=1}^m \mathbb{E}\left(x_i\right)\right| \geqslant \epsilon$
等价于以下事件:


$$
\frac{1}{m} \sum_{i=1}^m x_i-\frac{1}{m} \sum_{i=1}^m \mathbb{E}\left(x_i\right) \geqslant \epsilon \quad \vee \quad \frac{1}{m} \sum_{i=1}^m x_i-\frac{1}{m} \sum_{i=1}^m \mathbb{E}\left(x_i\right) \leqslant-\epsilon
$$




其中, $\vee$ 表示逻辑或
(以上其实就是将绝对值表达式拆成两部分而已)。这两个子事件并无 交集,
因此总概率等于两个子事件概率之和; 而
$\frac{1}{m} \sum_{i=1}^m x_i-\frac{1}{m} \sum_{i=1}^m \mathbb{E}\left(x_i\right) \leqslant-\epsilon$
与式(12.5) 表达的事情对称, 因此概率相同。

Hoeffding 不等式表达的意思是 $\frac{1}{m} \sum_{i=1}^m x_i$ 和
$\frac{1}{m} \sum_{i=1}^m \mathbb{E}\left(x_i\right)$
两个值应该比较接近, 二者 之差大于 $\epsilon$ 的概率很小 (不大于
$2 e^{-2 m \epsilon^2}$ )。

如果对Hoeffding不等式的证明感兴趣，可以参考Hoeffding在1963年发表的论文[1]，这篇文章也被引用了逾万次。

### 12.1.6 式(12.7)的解释

McDiarmid不等式：首先解释下前提条件：

$$
\sup _{x_{1}, \ldots, x_{m}, x_{i}^{\prime}}\left|f\left(x_{1}, \ldots, x_{m}\right)-f\left(x_{1}, \ldots, x_{i-1}, x_{i}^{\prime}, x_{i+1}, \ldots, x_{m}\right)\right| \leqslant c_{i}
$$



表示当函数$f$某个输入$x_i$变到$x_i^\prime$的时候，其变化的上确$\sup$仍满足不大于$c_i$。所谓上确界sup可以理解成变化的极限最大值，可能取到也可能无穷逼近。当满足这个条件时，McDiarmid不等式指出：函数值$f(x_1,\dots,x_m)$和其期望值$\mathbb{E}\left(f(x_1,\dots,x_m)\right)$也相近，从概率的角度描述是：它们之间差值不小于$\epsilon$这样的事件出现的概率不大于$\exp \left(\frac{-2 \epsilon^{2}}{\sum_{i} c_{i}^{2}}\right)$，可以看出当每次变量改动带来函数值改动的上限越小，函数值和其期望越相近。

## 12.2 PAC学习

本节内容几乎都是概念, 建议多读几遍，仔细琢磨一下。

概率近似正确(Probably Approximately Correct, PAC)学习, 可以读为
\[pæk\]学习。

本节第 2 段讨论的目标概念, 可简单理解为真实的映射函数;

本节第 3 段讨论的假设空间, 可简单理解为学习算法不同参数时的存在,
例如线性分类 超平面
$f(\boldsymbol{x})=\boldsymbol{w}^{\top} \boldsymbol{x}+b$, 每一组
$(\boldsymbol{w}, b)$ 取值就是一个假设;

本节第 4 段讨论的可分的(separable)和不可分的(non-separable),
例如西瓜书第 100 页的 图 5.4, 若假设空间是线性分类器,
则(a)(b)(c)是可分的, 而(d)是不可分的; 当然, 若假设空 间为椭圆分类器
(分类边界为椭圆), 则(d)也是可分的;

本节第 5 段提到的 "等效的假设"指的是第 7 页图 $1.3$ 中的 $\mathrm{A}$ 和
$\mathrm{B}$ 两条曲线都可以完 美拟合有限的样本点, 故称之为 "等效"
的假设; 另外本段最后还给出了概率近似正确的 含义, 即
"以较大概率学得误差满足预设上限的模型"。

定义 12.1 PAC 辨识的式(12.9)表示输出假设 $h$ 的泛化误差
$E(h) \leqslant \epsilon$ 的概率不小于 $1-\delta$; 即 "学习算法
$\mathfrak{L}$ 能以较大概率 (至少 $1-\delta$ ) 学得目标概念 $c$ 的近似
(误差最多为 $\epsilon$ )"。

定义 12.2 PAC 可学习的核心在于, 需要的样本数目 $m$ 是
$1 / \epsilon, 1 / \delta, \operatorname{size}(\boldsymbol{x}), \operatorname{size}(\mathrm{c})$
的多 项式函数。

定义 12.3 PAC 学习算法的核心在于, 完成 PAC 学习所需要的时间是
$1 / \epsilon, 1 / \delta, \operatorname{size}(\boldsymbol{x})$,
$\operatorname{size}(\mathrm{c})$ 的多项式函数。

定义 12.4 样本复杂度指完成 PAC 学习过程需要的最少的样本数量,
而在实际中当然也 希望用最少的样本数量完成学习过程。

在定义 12.4 之后, 抛出来三个问题:

- 研究某任务在什么样的条件下可学得较好的模型？（定义 12.2）

- 某算法在什么样的条件下可进行有效的学习?（定义 12.3）

- 需多少训练样例才能获得较好的模型？（定义 12.4）

有限假设空间指 $\mathcal{H}$ 中包含的假设个数是有限的,
反之则为无限假设空间; 无限假设空间 更为常见, 例如能够将图
5.4(a)(b)(c)中的正例和反例样本分开的线性超平面个数是无限多的。

### 12.2.1 式(12.9)的解释

PAC辨识的定义：$E(h)$表示算法$\mathcal{L}$在用观测集$D$训练后输出的假设函数$h$，它的泛化误差(见公式12.1)。这个概率定义指出，如果$h$的泛化误差不大于$\epsilon$的概率不小于$1-\delta$，那么我们称学习算法$\mathcal{L}$能从假设空间$\mathcal{H}$中PAC辨识概念类$\mathcal{C}$。

## 12.3 有限假设空间

本节内容分两部分, 第 1 部分 "可分情形" 时, 可以达到经验误差
$\widehat{E}(h)=0$, 做的事 情是以 $1-\delta$ 概率学得目标概念的
$\epsilon$ 近似, 即式(12.12); 第 2 部分 "不可分情形" 时, 无法达
到经验误差 $\widehat{E}(h)=0$, 做的事情是以 $1-\delta$ 概率学得
$\min _{h \in \mathcal{H}} E(h)$ 的 $\epsilon$ 近似, 即式(12.20)。无
论哪种情形, 对于 $h \in \mathcal{H}$, 可以得到该假设的泛化误差 $E(h)$
与经验误差 $\widehat{E}(h)$ 的关系, 即 "当 样例数目 $m$ 较大时, $h$
的经验误差是泛化误差很好的近似", 即式(12.18); 实际研究中经常需
要推导类似的泛化误差上下界。

从式12.10到式12.14的公式是为了回答一个问题：到底需要多少样例才能学得目标概念$c$的有效近似。只要训练集$D$的规模能使学习算法$\mathcal{L}$以概率$1-\delta$找到目标假设的$\epsilon$近似即可。下面就是用数学公式进行抽象。

### 12.3.1 式(12.10)的解释

$P(h(\boldsymbol{x})=y) =1-P(h(\boldsymbol{x}) \neq y)$
因为它们是对立事件，$P(h(x)\neq y)=E(h)$是泛化误差的定义(见12.1)，由于我们假定了泛化误差$E(h)>\epsilon$，因此有$1-E(h)<1-\epsilon$。

### 12.3.2 式(12.11)的解释

先解释什么是$h$与$D$"表现一致"，12.2节开头阐述了这样的概念，如果$h$能将$D$中所有样本按与真实标记一致的方式完全分开，我们称问题对学习算法是一致的。即$\left(h\left(\boldsymbol{x}_{1}\right)=y_{1}\right) \wedge \ldots \wedge\left(h\left(\boldsymbol{x}_{m}\right)=y_{m}\right)$为True。因为每个事件是独立的，所以上式可以写成$P\left(\left(h\left(\boldsymbol{x}_{1}\right)=y_{1}\right) \wedge \ldots \wedge\left(h\left(\boldsymbol{x}_{m}\right)=y_{m}\right)\right)=\prod_{i=1}^{m} P\left(h\left(\boldsymbol{x}_{i}\right)=y_{i}\right)$。根据对立事件的定义有：$\prod_{i=1}^{m} P\left(h\left(\boldsymbol{x}_{i}\right)=y_{i}\right)=\prod_{i=1}^{m}\left(1-P\left(h\left(\boldsymbol{x}_{i}\right) \neq y_{i}\right)\right)$，又根据公式(12.10)，有

$$
\prod_{i=1}^{m}\left(1-P\left(h\left(\boldsymbol{x}_{i}\right) \neq y_{i}\right)\right)<\prod_{i=1}^{m}(1-\epsilon)=(1-\epsilon)^{m}
$$




### 12.3.3 式(12.12)的推导

首先解释为什么"我们事先并不知道学习算法$\mathcal{L}$会输出$\mathcal{H}$中的哪个假设"，因为一些学习算法对用一个观察集$D$的输出结果是非确定的，比如感知机就是个典型的例子，训练样本的顺序也会影响感知机学习到的假设$h$参数的值。泛化误差大于$\epsilon$且经验误差为0的假设(即在训练集上表现完美的假设)出现的概率可以表示为$P(h \in \mathcal{H}: E(h)>\epsilon \wedge \widehat{E}(h)=0)$，根据式12.11，每一个这样的假设$h$都满足$P(E(h)>\epsilon \wedge \widehat{E}(h)=0)<\left(1-\epsilon \right)^m$，假设一共有$\vert\mathcal{H}\vert$这么多个这样的假设$h$，因为每个假设$h$满足$E(h)>\epsilon$且$\widehat{E}(h)=0$是互斥的，因此总的概率$P(h \in \mathcal{H}: E(h)>\epsilon \wedge \widehat{E}(h)=0)$就是这些互斥事件之和，即


$$
\begin{aligned}P\left(h \in \mathcal{H}: E(h)>\epsilon \wedge \widehat{E}(h)=0\right) &=\sum_i^{\mathcal{\vert H\vert}}P\left(E(h_i)>\epsilon \wedge \widehat{E}(h_i)=0\right)\\&<|\mathcal{H}|(1-\epsilon)^{m}\end{aligned}
$$




小于号依据公式(12.11)。

第二个小于号实际上是要证明$\vert\mathcal{H}\vert(1-\epsilon)^m < \vert\mathcal{H}\vert e^{-m\epsilon}$，即证明$(1-\epsilon)^m < e^{-m\epsilon}$，其中$\epsilon\in(0,1]$，$m$是正整数，推导如下：

当$\epsilon=1$时，显然成立，当$\epsilon\in(0, 1)$时，因为左式和右式的值域均大于0，所以可以左右两边同时取对数，又因为对数函数是单调递增函数，所以即证明$m\ln(1-\epsilon) < -m\epsilon$，即证明$\ln(1-\epsilon)<-\epsilon$，这个式子很容易证明：令$f(\epsilon)=\ln(1-\epsilon) + \epsilon$，其中$\epsilon\in(0,1)$，$f^\prime(\epsilon)=1-\frac{1}{1-\epsilon}=0 \Rightarrow \epsilon=0$
取极大值0，因此$ln(1-\epsilon)<-\epsilon$
也即$\vert\mathcal{H}\vert(1-\epsilon)^m < \vert\mathcal{H}\vert e^{-m\epsilon}$成立。

### 12.3.4 式(12.13)的解释

回到我们要回答的问题：到底需要多少样例才能学得目标概念$c$的有效近似。只要训练集$D$的规模能使学习算法$\mathcal{L}$以概率$1-\delta$找到目标假设的$\epsilon$近似即可。根据式12.12，学习算法$\mathcal{L}$生成的假设大于目标假设的$\epsilon$近似的概率为$P\left(h \in \mathcal{H}: E(h)>\epsilon \wedge \widehat{E}(h)=0\right)<\vert\mathcal{H}\vert e^{-m\epsilon}$，因此学习算法$\mathcal{L}$生成的假设落在目标假设的$\epsilon$近似的概率为$1-P\left(h \in \mathcal{H}: E(h)>\epsilon \wedge \widehat{E}(h)=0\right)\ge 1-\vert\mathcal{H}\vert e^{-m\epsilon}$，这个概率我们希望
至少是$1-\delta$，因此$1-\delta\leqslant 1-\vert\mathcal{H}\vert e^{-m\epsilon}\Rightarrow\vert\mathcal{H}\vert e^{-m\epsilon}\leqslant\delta$

### 12.3.5 式(12.14)的推导



$$
\begin{aligned}
\vert\mathcal{H}\vert e^{-m \epsilon} &\leqslant \delta\\
e^{-m \epsilon} &\leqslant \frac{\delta}{\vert\mathcal{H}\vert}\\
-m \epsilon &\leqslant \ln\delta-\ln\vert\mathcal{H}\vert\\
m &\geqslant \frac{1}{\epsilon}\left(\ln |\mathcal{H}|+\ln \frac{1}{\delta}\right)
\end{aligned}
$$




这个式子告诉我们，在假设空间$\mathcal{H}$是PAC可学习的情况下，输出假设$h$的泛化误差$\epsilon$随样本数目$m$增大而收敛到0，收敛速率为$O(\frac{1}{m})$。这也是我们在机器学习中的一个共识，即可供模型训练的观测集样本数量越多，机器学习模型的泛化性能越好。

### 12.3.6 引理12.1的解释

根据式(12.2),
$\widehat{E}(h)=\frac{1}{m} \sum_{i=1}^m \mathbb{I}\left(h\left(\boldsymbol{x}_i\right) \neq y_i\right)$,
而指示函数 $\mathbb{I}(\cdot)$ 取值非 0 即 1 , 也就是 说
$0 \leq \mathbb{I}\left(h\left(\boldsymbol{x}_i\right) \neq y_i\right) \leq 1$;
对于式(12.1) 的 $E(h)$ 实际上表示
$\mathbb{I}\left(h\left(\boldsymbol{x}_i\right) \neq y_i\right)$ 为 1
的期望
$\mathbb{E}\left(\mathbb{I}\left(h\left(\boldsymbol{x}_i\right) \neq y_i\right)\right)$
(泛化误差表示样本空间中任取一个样本, 其预测类别不等于真实类别的 概率),
当假设 $h$ 确定时, 泛化误差固定不变, 因此可记为
$E(h)=\frac{1}{m} \sum_{i=1}^m \mathbb{E}\left(\mathbb{I}\left(h\left(\boldsymbol{x}_i\right) \neq y_i\right)\right)$
。

此时, 将 $\widehat{E}(h)$ 和 $E(h)$ 代入式(12.15)到式(12.17),
对比式(12.5)和式(12.6)的 Hoeffding 不等式可知, 式(12.15)对应式(12.5),
式(12.16)与式(12.15)对称, 式(12.17)对应式(12.6)。

### 12.3.7 式(12.18)的推导

令$\delta=2e^{-2m\epsilon^2}$，则$\epsilon=\sqrt{\frac{\ln(2/\delta)}{2m}}$，由式(12.17)


$$
\begin{aligned}
P(|E(h)-\widehat{E}(h)| \geqslant \epsilon) &\leqslant 2 \exp \left(-2 m \epsilon^{2}\right)\\
P(|E(h)-\widehat{E}(h)| \geqslant \epsilon) &\leqslant \delta\\
P(|E(h)-\widehat{E}(h)| \leqslant \epsilon) &\geqslant 1 - \delta\\
P(-\epsilon \leqslant E(h)-\widehat{E}(h) \leqslant \epsilon) &\geqslant 1 - \delta\\
P(\widehat{E}(h) -\epsilon \leqslant E(h) \leqslant \widehat{E}(h)+\epsilon) &\geqslant 1 - \delta\\
\end{aligned}
$$




带入 $\epsilon=\sqrt{\frac{\ln(2/\delta)}{2m}}$得证。

这个式子进一步阐明了当观测集样本数量足够大的时候，$h$的经验误差是其泛化误差很好的近似。

### 12.3.8 式(12.19)的推导

令$h_1,h_2,\dots,h_{\vert\mathcal{H}\vert}$表示假设空间$\mathcal{H}$中的假设，有


$$
\begin{aligned} 
& P(\exists h \in \mathcal{H}:|E(h)-\widehat{E}(h)|>\epsilon) \\
=& P\left(\left(\left|E_{h_{1}}-\widehat{E}_{h_{1}}\right|>\epsilon\right) \vee \ldots \vee\left(| E_{h_{|\mathcal{H}|}}-\widehat{E}_{h_{|\mathcal{H}|} |>\epsilon}\right)\right) \\ \leqslant & \sum_{h \in \mathcal{H}} P(|E(h)-\widehat{E}(h)|>\epsilon) 
\end{aligned}
$$




这一步是很好理解的，存在一个假设$h$使得$|E(h)-\widehat{E}(h)|>\epsilon$的概率可以表示为对假设空间内所有的假设$h_i, i\in 1,\dots,\vert\mathcal{H}\vert$，使得$\left|E_{h_{i}}-\widehat{E}_{h_{i}}\right|>\epsilon$这个事件成立的\"或\"事件。因为$P(A\vee B)=P(A) + P(B) - P(A\wedge B)$，而$P(A\wedge B)\geqslant 0$，所以最后一行的不等式成立。

由式12.17： 

$$
\begin{aligned}
&P(|E(h)-\widehat{E}(h)| \geqslant \epsilon) \leqslant 2 \exp \left(-2 m \epsilon^{2}\right)\\
&\Rightarrow \sum_{h \in \mathcal{H}} P(|E(h)-\widehat{E}(h)|>\epsilon) \leqslant 2|\mathcal{H}| \exp \left(-2 m \epsilon^{2}\right)
\end{aligned}
$$




因此： 

$$
\begin{aligned}
P(\exists h \in \mathcal{H}:|E(h)-\widehat{E}(h)|>\epsilon) 
&\leqslant  \sum_{h \in \mathcal{H}} P(|E(h)-\widehat{E}(h)|>\epsilon)\\
&\leqslant 2|\mathcal{H}| \exp \left(-2 m \epsilon^{2}\right)
\end{aligned}
$$




其对立事件： 

$$
\begin{aligned}
P(\forall h\in\mathcal{H}:\vert E(h)-\widehat{E}(h)\vert\leqslant\epsilon)&=1-P(\exists h \in \mathcal{H}:|E(h)-\widehat{E}(h)|>\epsilon)\\ &\geqslant 1- 2|\mathcal{H}| \exp \left(-2 m \epsilon^{2}\right)
\end{aligned}
$$




令$\delta=2\vert\mathcal{H}\vert e^{-2m\epsilon^2}$，则$\epsilon=\sqrt{\frac{\ln |\mathcal{H}|+\ln (2 / \delta)}{2 m}}$，带入上式中即可得到


$$
P\left(\forall h\in\mathcal{H}:\vert E(h)-\widehat{E}(h)\vert\leqslant\sqrt{\frac{\ln |\mathcal{H}|+\ln (2 / \delta)}{2 m}}\right)\geqslant 1- \delta
$$




其中$\forall h\in\mathcal{H}$这个前置条件可以省略。

### 12.3.9 式(12.20)的解释

这个式子是"不可知PAC可学习"的定义式，不可知是指当目标概念$c$不在算法$\mathcal{L}$所能生成的假设空间$\mathcal{H}$里。可学习是指如果$\mathcal{H}$中泛化误差最小的假设是$\arg\min_{h\in \mathcal{H}}E(h)$，且这个假设的泛化误差满足其与目标概念的泛化误差的差值不大于$\epsilon$的概率不小于$1-\delta$。我们称这样的假设空间$\mathcal{H}$是不可知PAC可学习的。

## 12.4 VC维

不同于12.3节的有限假设空间，从本节开始，本章剩余内容均针对无限假设空间。

### 12.4.1 式(12.21)的解释

这个是增长函数的定义式。增长函数$\Pi_{\mathcal{H}}(m)$表示假设空间$\mathcal{H}$对m个样本所能赋予标签的最大可能的结果数。比如对于两个样本的二分类问题，一共有4中可能的标签组合$[[0, 0], [0, 1], [1, 0], [1, 1]]$，如果假设空间$\mathcal{H}_1$能赋予这两个样本两种标签组合$[[0, 0], [1, 1]]$，则$\Pi_{\mathcal{H}_1}(2)=2$。显然，$\mathcal{H}$对样本所能赋予标签的可能结果数越多，$\mathcal{H}$的表示能力就越强。增长函数可以用来反映假设空间$\mathcal{H}$的复杂度。

### 12.4.2 式(12.22)的解释

值得指出的是，这个式子的前提假设有误，应当写成对假设空间$\mathcal{H}$，$m\in\mathbb{N}$，$0<\epsilon<1$，存在$h\in\mathcal{H}$
详细证明参见原论文 On the uniform convergence of relative frequencies of
events to their probabilities
[2]，在该论文中，定理的形式如下：

Theorem 2 The probability that the relative frequency of at least one
event in class $S$ differs from its probability in an experiment of size
l by more then $\varepsilon$, for $l \geqq 2 / \varepsilon^2$, satisfies
the inequality


$$
\mathbf{P}\left(\pi^{(l)}>\varepsilon\right) \leqq 4 m^S(2 l) e^{-\varepsilon^2 l / 8} .
$$




注意定理描述中使用的是"at least one event in class S", 因此应该是 class
S 中"存在"one event 而不是 class S 中的 "任意" event。

另外, 该定理为基于增长函数对无限假设空间的泛化误差分析,
与上一节有限假设空间 的定理 12.1。在证明定理 $12.1$ 的式(12.19)过程中,
实际证明的结论是


$$
P(\exists h \in \mathcal{H}:|E(h)-\widehat{E}(h)|>\epsilon) \leqslant 2|\mathcal{H}| e^{-2 m \epsilon^2}
$$




根据该结论可得式(12.19)的原型（式(12.19)就是将 $\epsilon$ 用 $\delta$
表示):


$$
P(\forall h \in \mathcal{H}:|E(h)-\widehat{E}(h)| \leqslant \epsilon) \leqslant 1-2|\mathcal{H}| e^{-2 m \epsilon^2}
$$




这是因为事件 $\exists h \in \mathcal{H}:|E(h)-\widehat{E}(h)|>\epsilon$
与事件
$\forall h \in \mathcal{H}:|E(h)-\widehat{E}(h)| \leqslant \epsilon$
为对立事件。

注意到当使用 $|E(h)-\widehat{E}(h)|>\epsilon$ 表达时对应于 "存在",
当使用 $|E(h)-\widehat{E}(h)| \leqslant \epsilon$ 表达时 则对应于
"任意"。

综上所述, 式(12.22)使用 $|E(h)-\widehat{E}(h)|>\epsilon$,
所以这里应该对应于 "存在"。

### 12.4.3 式(12.23)的解释

这是VC维的定义式：VC维的定义是能被$\mathcal{H}$打散的最大示例集的大小。"西瓜书"中例12.1和例12.2
给出了形象的例子。

式(12.23)中的 $\left\{m: \Pi_{\mathcal{H}}(m)=2^m\right\}$ 表示一个集合,
集合的元素是能使 $\Pi_{\mathcal{H}}(m)=2^m$ 成立 的所有 $m$; 最外层的
max表示取集合的最大值。注意,
这里仅讨论二分类问题。注意，VC维的定义式上的底数2表示这个问题是2分类的问题。如果是$n$分类的问题，那么定义式中底数需要变为$n$。

$\mathrm{VC}$ 维的概念还是很容易理解的,
有个常见的思维误区西瓜书也指出来了, 即 "这并不 意味着所有大小为 $d$
的示例集都能被假设空间 $\mathcal{H}$ 打散", 也就是说只要 "存在大小为 $d$
的示例 集能被假设空间 H打散" 即可, 这里的区别与前面 "定理 $12.2$ 的解释"
中提到的 "任意" 与 "存在" 的关系一样。

### 12.4.4 引理12.2的解释

首先解释下数学归纳法的起始条件\"当$m=1, d=0$或$d=1$时，定理成立\"，当$m=1,d=0$时，由VC维的定义(式12.23)
$\mathrm{VC}(\mathcal{H})=\max \left\{m: \Pi_{\mathcal{H}}(m)=2^{m}\right\}=0$
可知$\Pi_{\mathcal{H}}(1)<2$，否则$d$可以取到1，又因为$\Pi_{\mathcal{H}}(m)$为整数，所以$\Pi_{\mathcal{H}}(1)\in[0, 1]$，式12.24右边为$\sum_{i=0}^{0}\left(\begin{array}{c}{1} \\ {i}\end{array}\right)=1$，因此不等式成立。当$m=1,d=1$时，因为一个样本最多只能有两个类别，所以$\Pi_\mathcal{H}(1)=2$，不等式右边为$\sum_{i=0}^{1}\left(\begin{array}{c}{1} \\ {i}\end{array}\right)=2$，因此不等式成立。

再介绍归纳过程，这里采样的归纳方法是假设式(12.24)对$(m-1, d-1)$和$(m-1, d)$成立，推导出其对$(m,d)$也成立。证明过程中引入观测集$D=\left\{\boldsymbol{x}_{1}, \boldsymbol{x}_{2}, \ldots, \boldsymbol{x}_{m}\right\}$
和观测集$D^\prime=\left\{\boldsymbol{x}_{1}, \boldsymbol{x}_{2}, \ldots, \boldsymbol{x}_{m-1}\right\}$，其中$D$比$D^\prime$多一个样本$x_m$，它们对应的假设空间可以表示为：


$$
\begin{array}{l}{\mathcal{H}_{| D}=\left\{\left(h\left(\boldsymbol{x}_{1}\right), h\left(\boldsymbol{x}_{2}\right), \ldots, h\left(\boldsymbol{x}_{m}\right)\right) | h \in \mathcal{H}\right\}} \\ {\mathcal{H}_{| D^{\prime}}=\left\{\left(h\left(\boldsymbol{x}_{1}\right), h\left(\boldsymbol{x}_{2}\right), \ldots, h\left(\boldsymbol{x}_{m-1}\right)\right) | h \in \mathcal{H}\right\}}\end{array}
$$




如果假设$h\in\mathcal{H}$对$x_m$的分类结果为$+1$，或为$-1$，那么任何出现在$\mathcal{H}_{\vert D^\prime}$中的串都会在$\mathcal{H}_{\vert D}$中出现一次或者两次。这里举个例子就很容易理解了，假设$m=3$：


$$
\begin{aligned}
\mathcal{H}_{\vert D}&=\{(+,-,-),(+,+,-),(+,+,+),(-,+,-),(-,-,+)\}\\
\mathcal{H}_{\vert D^\prime}&=\{(+,+),(+,-),(-,+),(-,-)\}\\
\end{aligned}
$$




其中串$(+,+)$在$\mathcal{H}_{\vert D}$中出现了两次$(+, +, +), (+, +, -)$，$\mathcal{H}_{\vert D^\prime}$中得其他串$(+,-), (-, +), (-, -)$均只在$\mathcal{H}_{\vert D}$中出现了一次。这里的原因是每个样本是二分类的，所以多出的样本$x_m$要么取$+$，要么取$-$，要么都取到(至少两个假设$h$对$x_m$做出了不一致的判断)。
记号$\mathcal{H}_{D^\prime\vert D}$表示在$\mathcal{H}_{\vert D}$中出现了两次的$\mathcal{H}_{\vert D^\prime}$组成的集合，比如在上例中$\mathcal{H}_{D^\prime\vert D}=\{(+,+)\}$，有


$$
\left|\mathcal{H}_{| D}\right|=\left|\mathcal{H}_{| D^{\prime}}\right|+\left|\mathcal{H}_{D^{\prime} | D}\right|
$$




由于$\mathcal{H}_{\vert D^\prime}$表示限制在样本集$D^\prime$上的假设空间$\mathcal{H}$的表达能力(即所有假设对样本集$D^\prime$所能赋予的标记种类数)，样本集$D^\prime$的数目为$m-1$，根据增长函数的定义，假设空间$\mathcal{H}$对包含$m-1$个样本的集合所能赋予的最大标记种类数为$\Pi_{\mathcal{H}}(m-1)$，因此$\vert\mathcal{H}_{\vert D^\prime}\vert \leqslant \Pi_\mathcal{H}(m-1)$。又根据数学归纳法的前提假设，有：


$$
\left|\mathcal{H}_{| D^{\prime}}\right| \leqslant \Pi_{\mathcal{H}}(m-1) \leqslant \sum_{i=0}^{d}\left(\begin{array}{c}{m-1} \\ {i}\end{array}\right)
$$




由记号$\mathcal{H}_{\vert D^\prime}$的定义可知，$\vert\mathcal{H}_{\vert D^\prime}\vert \geqslant \left\lfloor\frac{\vert\mathcal{H}_{\vert D}\vert}{2}\right\rfloor$，又由于$\vert\mathcal{H}_{\vert D^\prime}\vert$和$\vert\mathcal{H}_{D^\prime\vert D}\vert$均为整数，因此$\vert\mathcal{H}_{D^\prime\vert D}\vert \leqslant \left\lfloor\frac{\vert\mathcal{H}_{\vert D}\vert}{2}\right\rfloor$，由于样本集$D$的大小为$m$，根据增长函数的概念，有$\left|\mathcal{H}_{D^{\prime}| D}\right| \leqslant \left\lfloor\frac{\vert\mathcal{H}_{\vert D}\vert}{2}\right\rfloor\leqslant \Pi_{\mathcal{H}}(m-1)$。
假设$Q$表示能被$\mathcal{H}_{D^\prime\vert D}$打散的集合，因为根据$\mathcal{H}_{D^\prime\vert D}$的定义，$H_{D}$必对元素$x_m$给定了不一致的判定，因此$Q \cup\left\{\boldsymbol{x}_{m}\right\}$必能被$\mathcal{H}_{\vert D}$打散，由前提假设$\mathcal{H}$的VC维为$d$，因此$\mathcal{H}_{D^\prime\vert D}$的VC维最大为$d-1$，综上有


$$
\left|\mathcal{H}_{D^{\prime}| D}\right| \leqslant \Pi_{\mathcal{H}}(m-1) \leqslant \sum_{i=0}^{d-1}\left(\begin{array}{c}{m-1} \\ {i}\end{array}\right)
$$




因此： 

$$
\begin{aligned}
\left|\mathcal{H}_{| D}\right|&=\left|\mathcal{H}_{| D^{\prime}}\right|+\left|\mathcal{H}_{D^{\prime} | D}\right|\\
&\leqslant \sum_{i=0}^{d}\left(\begin{array}{c}{m-1} \\ {i}\end{array}\right) + \sum_{i=0}^{d+1}\left(\begin{array}{c}{m-1} \\ {i}\end{array}\right)\\
&=\sum_{i=0}^d \left(\left(\begin{array}{c}{m-1} \\ {i}\end{array}\right) + \left(\begin{array}{c}{m-1} \\ {i-1}\end{array}\right)\right)\\
&=\sum_{i=0}^{d}\left(\begin{array}{c}{m} \\ {i}\end{array}\right)
\end{aligned}
$$




注：最后一步依据组合公式，推导如下：


$$
\begin{aligned}\left(\begin{array}{c}{m-1} \\ {i}\end{array}\right)+\left(\begin{array}{c}{m-1} \\ {i-1}\end{array}\right) &=\frac{(m-1) !}{(m-1-i) ! i !}+\frac{(m-1) !}{(m-1-i+1) !(i-1) !} \\ &=\frac{(m-1) !(m-i)}{(m-i)(m-1-i) ! i !}+\frac{(m-1) ! i}{(m-i) !(i-1) ! i} \\ &=\frac{(m-1) !(m-i)+(m-1) ! i}{(m-i) ! i !} \\ &=\frac{(m-1) !(m-i+i)}{(m-i) ! i !}=\frac{(m-1) ! m}{(m-i) ! i !} \\ &=\frac{m !}{(m-i) ! i !}=\left(\begin{array}{c}{m} \\ {i}\end{array}\right) \end{aligned}
$$




### 12.4.5 式(12.28)的解释



$$
\begin{aligned} \Pi_{\mathcal{H}}(m) & \leqslant \sum_{i=0}^{d}\left(\begin{array}{c}{m} \\ {i}\end{array}\right) \\ & \leqslant \sum_{i=0}^{d}\left(\begin{array}{c}{m} \\ {i}\end{array}\right)\left(\frac{m}{d}\right)^{d-i} \\ &=\left(\frac{m}{d}\right)^{d} \sum_{i=0}^{d}\left(\begin{array}{c}{m} \\ {i}\end{array}\right)\left(\frac{d}{m}\right)^{i} \\ & \leqslant\left(\frac{m}{d}\right)^{d} \sum_{i=0}^{m}\left(\begin{array}{c}{m} \\ {i}\end{array}\right)\left(\frac{d}{m}\right)^{i} \\ 
&={\left(\frac{m}{d}\right)}^d{\left(1+\frac{d}{m}\right)}^m\\
&<\left(\frac{e \cdot m}{d}\right)^{d} \end{aligned}
$$




第一步到第二步和第三步到第四步均因为$m\geqslant d$，第四步到第五步是由于二项式定理[3]：$(x+y)^{n}=\sum_{k=0}^{n}\left(\begin{array}{l}{n} \\ {k}\end{array}\right) x^{n-k} y^{k}$，其中令$k=i, n=m, x=1, y = \frac{d}{m}$得$\left(\frac{m}{d}\right)^{d} \sum_{i=0}^{m}\left(\begin{array}{c}{m} \\ {i}\end{array}\right)\left(\frac{d}{m}\right)^{i}=\left(\frac{m}{d}\right)^{d} (1+\frac{d}{m})^m$，最后一步的不等式即需证明${\left(1+\frac{d}{m}\right)}^m\leqslant e^d$，因为${\left(1+\frac{d}{m}\right)}^m={\left(1+\frac{d}{m}\right)}^{\frac{m}{d}d}$，根据自然对数底数$e$的定义[4]，${\left(1+\frac{d}{m}\right)}^{\frac{m}{d}d}< e^d$，注意原文中用的是$\leqslant$，但是由于$e=\lim _{\frac{d}{m} \rightarrow 0}\left(1+\frac{d}{m}\right)^{\frac{m}{d}}$的定义是一个极限，所以应该是用$<$。

### 12.4.6 式(12.29)的解释

这里应该是作者的笔误，根据式12.22，$E(h)-\widehat{E}(h)$应当被绝对值符号包裹。将式12.28带入式12.22得


$$
P\left(\vert 
E(h)-\widehat{E}(h) \vert> \epsilon
\right) 
\leqslant 4{\left(\frac{2em}{d}\right)}^d\exp\left(-\frac{m\epsilon^2}{8}\right)
$$



令$4{\left(\frac{2em}{d}\right)}^d\exp\left(-\frac{m\epsilon^2}{8}\right)=\delta$可解得


$$
\delta=\sqrt{
\frac{8d\ln\frac{2em}{d}+8\ln\frac{4}{\delta}}{m}
}
$$



带入式12.22，则定理得证。这个式子是用VC维表示泛化界，可以看出，泛化误差界只与样本数量$m$有关，收敛速率为$\sqrt{\frac{\ln m}{m}}$
(书上简化为$\frac{1}{\sqrt{m}}$)。

### 12.4.7 式(12.30)的解释

这个是经验风险最小化的定义式。即从假设空间中找出能使经验风险最小的假设。

### 12.4.8 定理12.4的解释

首先回忆PAC可学习的概念，见定义12.2，而可知/不可知PAC可学习之间的区别仅仅在于概念类$c$是否包含于假设空间$\mathcal{H}$中。令


$$
\begin{aligned}
\delta^\prime = \frac{\delta}{2} \\
\sqrt{\frac{\left(\ln 2 / \delta^{\prime}\right)}{2 m}}=\frac{\epsilon}{2}
\end{aligned}
$$


 结合这两个标记的转换，由推论12.1可知：


$$
\widehat{E}(g)-\frac{\epsilon}{2} \leqslant E(g) \leqslant \widehat{E}(g)+\frac{\epsilon}{2}
$$



至少以$1-\delta/2$的概率成立。写成概率的形式即：


$$
P\left(|E(g)-\widehat{E}(g)| \leqslant \frac{\epsilon}{2}\right) \geqslant 1-\delta / 2
$$



即$P\left(\left(E(g)-\widehat{E}(g) \leqslant \frac{\epsilon}{2}\right) \wedge\left(E(g)-\widehat{E}(g) \geqslant-\frac{\epsilon}{2}\right)\right) \geqslant 1-\delta / 2$，因此$P\left(E(g)-\widehat{E}(g) \leqslant \frac{\epsilon}{2}\right) \geqslant 1-\delta / 2$且$P\left(E(g)-\widehat{E}(g) \geqslant -\frac{\epsilon}{2}\right) \geqslant 1-\delta / 2$成立。
再令


$$
\sqrt{\frac{8 d \ln \frac{2 e m}{d}+8 \ln \frac{4}{\delta^{\prime}}}{m}}=\frac{\epsilon}{2}
$$



由式12.29可知 

$$
P\left(\left\vert 
E(h)-\widehat{E}(h) \right\vert\leqslant \frac{\epsilon}{2}
\right)
\geqslant 1-\frac{\delta}{2}
$$



同理，$P\left(E(h)-\widehat{E}(h) \leqslant \frac{\epsilon}{2}\right) \geqslant 1-\delta / 2$且$P\left(E(h)-\widehat{E}(h) \geqslant -\frac{\epsilon}{2}\right) \geqslant 1-\delta / 2$成立。
由$P\left(E(g)-\widehat{E}(g) \geqslant - \frac{\epsilon}{2}\right) \geqslant 1-\delta / 2$和$P\left(E(h)-\widehat{E}(h) \leqslant \frac{\epsilon}{2}\right) \geqslant 1-\delta / 2$均成立可知
则事件$E(g)-\widehat{E}(g) \geqslant -\frac{\epsilon}{2}$和事件$E(h)-\widehat{E}(h) \leqslant \frac{\epsilon}{2}$同时成立的概率为：


$$
\begin{aligned}
&P\left(
\left(E(g)-\widehat{E}(g) \geqslant -\frac{\epsilon}{2} \right)\wedge\left(E(h)-\widehat{E}(h) \leqslant \frac{\epsilon}{2}
\right)\right) 
\\= &
P\left(E(g)-\widehat{E}(g) \geqslant -\frac{\epsilon}{2}\right) +
P\left(E(h)-\widehat{E}(h) \leqslant \frac{\epsilon}{2}\right)
- P\left(\left(E(g)-\widehat{E}(g) \geqslant -\frac{\epsilon}{2} \right)\vee\left(E(h)-\widehat{E}(h) \leqslant \frac{\epsilon}{2}
\right)\right) 
\\\geqslant &1 - \delta/2 + 1 - \delta/2 - 1
\\=& 1-\delta
\end{aligned}
$$


 即 

$$
P\left(
\left(E(g)-\widehat{E}(g) \geqslant -\frac{\epsilon}{2} \right)\wedge\left(E(h)-\widehat{E}(h) \leqslant \frac{\epsilon}{2}
\right)\right) \geqslant 1-\delta
$$


 因此 

$$
P\left(
\widehat{E}(g)-E(g)+E(h)-\widehat{E}(h)\leqslant\frac{\epsilon}{2} + \frac{\epsilon}{2} 
\right) = P\left(E(h)-E(g)\leqslant\widehat{E}(h)-\widehat{E}(g)+\epsilon\right)
\geqslant 1 - \delta
$$



再由$h$和$g$的定义，$h$表示假设空间中经验误差最小的假设，$g$表示泛化误差最小的假设，将这两个假设共用作用于样本集$D$，则一定有$\widehat{E}(h)\leqslant\widehat{E}(g)$，因此上式可以简化为：


$$
P\left(E(h)-E(g)\leqslant\epsilon\right)
\geqslant 1 - \delta
$$



根据式12.32和式12.34，可以求出$m$为关于$\left(1/\epsilon,1/\delta,\text{size}(x),\text{size}(c)\right)$的多项式，因此根据定理12.2，定理12.5，得到结论任何VC维有限的假设空间$\mathcal{H}$都是(不可知)PAC可学习的。

## 12.5 Rademacher复杂度

上一节中介绍的基于VC维的泛化误差界是分布无关、数据独立的，本节将要介绍的Rademacher复杂度则在一定程度上考虑了数据分布。

### 12.5.1 式(12.36)的解释

这里解释从第一步到第二步的推导，因为前提假设是2分类问题，$y_k\in\{-1, +1\}$，因此$\mathbb{I}\left(h(x_i)\neq y_i\right)\equiv \frac{1-y_i h(x_i)}{2}$。这是因为假如$y_i=+1, h(x_i)=+1$或$y_i=-1, h(x_i)=-1$，有$\mathbb{I}\left(h(x_i)\neq y_i\right)=0= \frac{1-y_i h(x_i)}{2}$；反之，假如$y_i=-1, h(x_i)=+1$或$y_i=+1, h(x_i)=-1$，有$\mathbb{I}\left(h(x_i)\neq y_i\right)=1= \frac{1-y_i h(x_i)}{2}$。

### 12.5.2 式(12.37)的解释

由公式12.36可知，经验误差$\widehat{E}(h)$和$\frac{1}{m} \sum_{i=1}^{m} y_{i} h\left(\boldsymbol{x}_{i}\right)$呈反比的关系，因此假设空间中能使经验误差最小的假设$h$即是使$\frac{1}{m} \sum_{i=1}^{m} y_{i} h\left(\boldsymbol{x}_{i}\right)$最大的$h$。

### 12.5.3 式(12.38)的解释

上确界$\sup$这个概念前面已经解释过，见式(12.7)的解析。相比于式(12.37),
样例真实标记 $y_i$ 换为了 Rademacher 随机变量
$\sigma_i, \arg \max _{h \in \mathcal{H}}$ 换为了 上确界
$\sup _{h \in \mathcal{H}^{\circ}}$ 该式表示, 对于样例集
$D=\left\{\boldsymbol{x}_1, \boldsymbol{x}_2, \ldots, \boldsymbol{x}_m\right\}$,
假设空间 $\mathcal{H}$ 中的假设对其预 测结果
$\left\{h\left(\boldsymbol{x}_1\right), h\left(\boldsymbol{x}_2\right), \ldots, h\left(\boldsymbol{x}_m\right)\right\}$
与随机变量集合
$\boldsymbol{\sigma}=\left\{\sigma_1, \sigma_2, \ldots, \sigma_m\right\}$
的契合程度。接下 来解释一下该式的含义。
$\frac{1}{m} \sum_{i=1}^m \sigma_i h\left(\boldsymbol{x}_i\right)$ 中的
$\boldsymbol{\sigma}=\left\{\sigma_1, \sigma_2, \ldots, \sigma_m\right\}$
表示单次随机生成的结果（生成后就固定不 动 ), 而
$\left\{h\left(\boldsymbol{x}_1\right), h\left(\boldsymbol{x}_2\right), \ldots, h\left(\boldsymbol{x}_m\right)\right\}$
表示某个假设 $h \in \mathcal{H}$ 的预测结果, 至于
$\frac{1}{m} \sum_{i=1}^m \sigma_i h\left(\boldsymbol{x}_i\right)$ 的
取值则取决于本次随机生成的 $\sigma$ 和假设 $h$ 的预测结果的契合程度。

进一步地,
$\sup _{h \in \mathcal{H}} \frac{1}{m} \sum_{i=1}^m \sigma_i h\left(\boldsymbol{x}_i\right)$
中的
$\boldsymbol{\sigma}=\left\{\sigma_1, \sigma_2, \ldots, \sigma_m\right\}$
仍表示单次随机生成的结 果 (生成后就固定不动), 但此时需求解的是假设空间
$\mathcal{H}$ 中所有假设与 $\sigma$ 最契合的那个 $h$ 。

例如, $\boldsymbol{\sigma}=\{-1,+1,-1,+1\}$ （即 $m=4$, 这里
$\boldsymbol{\sigma}$ 仅为本次随机生成结果而已, 下次生
成结果可能是另一组结果), 假设空间
$\mathcal{H}=\left\{h_1, h_2, h_3\right\}$, 其中 

$$
\begin{aligned}
& \left\{h_1\left(\boldsymbol{x}_1\right), h_1\left(\boldsymbol{x}_2\right), h_1\left(\boldsymbol{x}_3\right), h_1\left(\boldsymbol{x}_4\right)\right\}=\{-1,-1,-1,-1\} \\
& \left\{h_2\left(\boldsymbol{x}_1\right), h_2\left(\boldsymbol{x}_2\right), h_2\left(\boldsymbol{x}_3\right), h_2\left(\boldsymbol{x}_4\right)\right\}=\{-1,+1,-1,-1\} \\
& \left\{h_3\left(\boldsymbol{x}_1\right), h_3\left(\boldsymbol{x}_2\right), h_3\left(\boldsymbol{x}_3\right), h_3\left(\boldsymbol{x}_4\right)\right\}=\{+1,+1,+1,+1\}
\end{aligned}
$$


 易知
$\frac{1}{m} \sum_{i=1}^m \sigma_i h_1\left(\boldsymbol{x}_i\right)=0, \frac{1}{m} \sum_{i=1}^m \sigma_i h_2\left(\boldsymbol{x}_i\right)=\frac{2}{4}, \frac{1}{m} \sum_{i=1}^m \sigma_i h_3\left(\boldsymbol{x}_i\right)=0$,
因此


$$
\sup _{h \in \mathcal{H}} \frac{1}{m} \sum_{i=1}^m \sigma_i h\left(\boldsymbol{x}_i\right)=\frac{2}{4}
$$




### 12.5.4 式(12.39)的解释



$$
\mathbb{E}_{\boldsymbol{\sigma}}\left[\sup _{h \in \mathcal{H}} \frac{1}{m} \sum_{i=1}^{m} \sigma_{i} h\left(\boldsymbol{x}_{i}\right)\right]
$$



\[解析\]：这个式子可以用来衡量假设空间$\mathcal{H}$的表达能力，对变量$\sigma$求期望可以理解为当变量$\sigma$包含所有可能的结果时，假设空间$\mathcal{H}$中最契合的假设$h$和变量的平均契合程度。因为前提假设是2分类的问题，因此$\sigma_i$一共有$2^m$种，这些不同的$\sigma_i$构成了数据集$D=\{(x_1, y_1), (x_2, y_2),\dots, (x_m, y_m)\}$的"对分"(12.4节)，如果一个假设空间的表达能力越强，那么就越有可能对于每一种$\sigma_i$，假设空间中都存在一个$h$使得$h(x_i)$和$\sigma_i$非常接近甚至相同，对所有可能的$\sigma_i$取期望即可衡量假设空间的整体表达能力，这就是这个式子的含义。

### 12.5.5 式(12.40)的解释

对比式12.39，这里使用函数空间$\mathcal{F}$代替了假设空间$\mathcal{H}$，函数$f$代替了假设$h$，很容易理解，因为假设$h$即可以看做是作用在数据$x_i$上的一个映射，通过这个映射可以得到标签$y_i$。注意前提假设实值函数空间$\mathcal{F}:\mathcal{Z}\rightarrow\mathbb{R}$，即映射$f$将样本$z_i$映射到了实数空间，这个时候所有的$\sigma_i$将是一个标量即$\sigma_i\in\{+1, -1\}$。

### 12.5.6 式(12.41)的解释

这里所要求的是$\mathcal{F}$关于分布$\mathcal{D}$的Rademacher复杂度，因此从$\mathcal{D}$中采出不同的样本$Z$，计算这些样本对应的Rademacher复杂度的期望。

### 12.5.7 定理12.5的解释

首先令记号


$$
\begin{aligned} \widehat{E}_{Z}(f) &=\frac{1}{m} \sum_{i=1}^{m} f\left(\boldsymbol{z}_{i}\right) \\ \Phi(Z) &=\sup _{f \in \mathcal{F}} \left(\mathbb{E}[f]-\widehat{E}_{Z}(f)\right) \end{aligned}
$$




即$\widehat{E}_{Z}(f)$表示函数$f$作为假设下的经验误差，$\Phi(Z)$表示泛化误差和经验误差的差的上确界。再令$Z^\prime$为只与$Z$有一个示例(样本)不同的训练集，不妨设$z_m\in Z$和$z^\prime_m\in Z^\prime$为不同的示例，那么有


$$
\begin{aligned} \Phi\left(Z^{\prime}\right)-\Phi(Z) &=\sup _{f \in \mathcal{F}} \left(\mathbb{E}[f]-\widehat{E}_{Z^{\prime}}(f)\right)-\sup _{f \in \mathcal{F}} \left(\mathbb{E}[f]-\widehat{E}_{Z}(f)\right) \\ & \leqslant \sup _{f \in \mathcal{F}} \left(\widehat{E}_{Z}(f)-\widehat{E}_{Z^{\prime}}(f)\right) \\ &=\sup_{f\in\mathcal{F}}\frac{\sum^m_{i=1}f(z_i)-\sum^m_{i=1}f(z^\prime_i)}{m}\\&=\sup _{f \in \mathcal{F}} \frac{f\left(z_{m}\right)-f\left(z_{m}^{\prime}\right)}{m} \\ & \leqslant \frac{1}{m} \end{aligned}
$$




第一个不等式是因为上确界的差不大于差的上确界[5]，第四行的等号由于$Z^\prime$与$Z$只有$z_m$不相同，最后一行的不等式是因为前提假设$\mathcal{F}:\mathcal{Z}\rightarrow [0,1]$，即$f(z_m),f(z_m^\prime)\in[0,1]$。
同理


$$
\Phi(Z)-\Phi\left(Z^{\prime}\right) =\sup _{f \in \mathcal{F}} \frac{f\left(z_{m}^\prime\right)-f\left(z_{m}\right)}{m} \leqslant \frac{1}{m}
$$




综上二式有：


$$
\left\vert \Phi(Z)-\Phi\left(Z^{\prime}\right)\right\vert \leqslant \frac{1}{m}
$$




将$\Phi$看做函数$f$(注意这里的$f$不是$\Phi$定义里的$f$)，那么可以套用McDiarmid不等式的结论式12.7


$$
P\left(\Phi(Z)-\mathbb{E}_{Z}[\Phi(Z)] \geqslant \epsilon\right) \leqslant \exp \left(\frac{-2 \epsilon^{2}}{\sum_{i} c_{i}^{2}}\right)
$$




令$\exp \left(\frac{-2 \epsilon^{2}}{\sum_{i} c_{i}^{2}}\right)=\delta$可以求得$\epsilon=\sqrt{\frac{\ln (1 / \delta)}{2 m}}$，所以


$$
P\left(\Phi(Z)-\mathbb{E}_{Z}[\Phi(Z)] \geqslant \sqrt{\frac{\ln (1 / \delta)}{2 m}}\right) \leqslant \delta
$$




由逆事件的概率定义得


$$
P\left(\Phi(Z)-\mathbb{E}_{Z}[\Phi(Z)] \leqslant \sqrt{\frac{\ln (1 / \delta)}{2 m}}\right) \geqslant 1-\delta
$$




即书中式12.44的结论。下面来估计$\mathbb{E}_{Z}[\Phi(Z)]$的上界：


$$
\begin{aligned} \mathbb{E}_{Z}[\Phi(Z)] &=\mathbb{E}_{Z}\left[\sup _{f \in \mathcal{F}} \left(\mathbb{E}[f]-\widehat{E}_{Z}(f)\right)\right] \\ &=\mathbb{E}_{Z}\left[\sup _{f \in \mathcal{F}} \mathbb{E}_{Z^{\prime}}\left[\widehat{E}_{Z^{\prime}}(f)-\widehat{E}_{Z}(f)\right]\right] \\ & \leqslant \mathbb{E}_{Z, Z^{\prime}}\left[\sup _{f \in \mathcal{F}}\left( \widehat{E}_{Z^{\prime}}(f)-\widehat{E}_{Z}(f)\right)\right] \\ &=\mathbb{E}_{Z, Z^{\prime}}\left[\sup _{f \in \mathcal{F}} \frac{1}{m} \sum_{i=1}^{m}\left(f\left(\boldsymbol{z}_{i}^{\prime}\right)-f\left(\boldsymbol{z}_{i}\right)\right)\right] \\ &=\mathbb{E}_{\boldsymbol{\sigma}, Z,Z^{\prime}}\left[\sup _{f \in \mathcal{F}} \frac{1}{m} \sum_{i=1}^{m} \sigma_{i}\left(f\left(\boldsymbol{z}_{i}^{\prime}\right)-f\left(\boldsymbol{z}_{i}\right)\right)\right] \\ &\leqslant \mathbb{E}_{\boldsymbol{\sigma}, Z^{\prime}}\left[\sup _{f \in \mathcal{F}} \frac{1}{m} \sum_{i=1}^{m} \sigma_{i} f\left(\boldsymbol{z}_{i}^{\prime}\right)\right]+\mathbb{E}_{\boldsymbol{\sigma}, Z}\left[\sup _{f \in \mathcal{F}} \frac{1}{m} \sum_{i=1}^{m}-\sigma_{i} f\left(\boldsymbol{z}_{i}\right)\right] \\ &=2 \mathbb{E}_{\boldsymbol{\sigma}, Z}\left[\sup _{f \in \mathcal{F}} \frac{1}{m} \sum_{i=1}^{m} \sigma_{i} f\left(\boldsymbol{z}_{i}\right)\right] \\ &=2 R_{m}(\mathcal{F}) \end{aligned}
$$




第二行等式是外面套了一个对服从分布$\mathcal{D}$的示例集$Z^\prime$求期望，因为$\mathbb{E}_{Z^\prime\sim\mathcal{D}}[\widehat{E}_{Z^\prime}(f)]=\mathbb{E}(f)$，而采样出来的$Z^\prime$和$Z$相互独立，因此有$\mathbb{E}_{Z^\prime\sim\mathcal{D}}[\widehat{E}_{Z}(f)]=\widehat{E}_{Z}(f)$。

第三行不等式基于上确界函数$\sup$是个凸函数，将$\sup_{f\in\mathcal{F}}$看做是凸函数$f$，将$\widehat{E}_{Z^{\prime}}(f)-\widehat{E}_{Z}(f)$看做变量$x$根据Jesen不等式(式12.4)，有$\mathbb{E}_{Z}\left[\sup _{f \in \mathcal{F}} \mathbb{E}_{Z^{\prime}}\left[\widehat{E}_{Z^{\prime}}(f)-\widehat{E}_{Z}(f)\right]\right] \leqslant \mathbb{E}_{Z, Z^{\prime}}\left[\sup _{f \in \mathcal{F}}\left( \widehat{E}_{Z^{\prime}}(f)-\widehat{E}_{Z}(f)\right)\right]$，其中$\mathbb{E}_{Z, Z^{\prime}}[\cdot]$是$\mathbb{E}_{Z}[\mathbb{E}_{Z^\prime}[\cdot]]$的简写形式。

第五行引入对Rademacher随机变量的期望，由于函数值空间是标量，因为$\sigma_i$也是标量，即$\sigma_i\in\{-1, +1\}$，且$\sigma_i$总以相同概率可以取到这两个值，因此可以引入$\mathbb{E}_{\sigma}$而不影响最终结果。

第六行利用了上确界的和不小于和的上确界[5]，因为第一项中只含有变量$z^\prime$，所以可以将$\mathbb{E}_Z$去掉，因为第二项中只含有变量$z$，所以可以将$\mathbb{E}_{Z^\prime}$去掉。

第七行利用$\sigma$是对称的，所以$-\sigma$的分布和$\sigma$完全一致，所以可以将第二项中的负号去除，又因为$Z$和$Z^\prime$均是从$\mathcal{D}$中$i.i.d.$采样得到的数据，因此可以将第一项中的$z^\prime_i$替换成$z$，将$Z^\prime$替换成$Z$。

最后根据定义式12.41可得$\mathbb{E}_{Z}[\Phi(Z)]=2\mathcal{R}_m(\mathcal{F})$，式(12.42)得证。

## 12.6 定理12.6的解释

针对二分类问题, 定理 $12.5$ 给出了 "泛化误差" 和 "经验误差" 的关系, 即:

-   式(12.47)基于 Rademacher 复杂度 $R_m(\mathcal{H})$ 给出了泛化误差
    $E(h)$ 的上界;

-   式(12.48)基于经验 Rademacher 复杂度 $\widehat{R}_D(\mathcal{H})$
    给出了泛化误差 $E(h)$ 的上界。

可能大家都会有疑问：定理12.6的设定其实也适用于定理12.5, 即值域为二 值的
$\{-1,+1\}$ 也属于值域为连续值的 $[0,1]$ 的一种特殊情况,
这一点从接下来的式(12.49)的 转换可以看出。那么,
为什么还要针对二分类问题专门给出定理12.6呢?

根据(经验)Rademacher 复杂度的定义可以知道, $R_m(\mathcal{H})$ 和
$\widehat{R}_D(\mathcal{H})$ 均大于零 (参见前面 有关式(12.39)的解释,
书中式(12.39)下面的一行也提到该式取值范围是 $[0,1])$; 因此, 相比
于定理12.5来说, 定理12.6的上界更紧,
因为二者的界只有中间一项关于(经验)Rademacher 复杂度的部分不同,
在定理12.5中是两倍的(经验)Rademacher 复杂度, 而在定理 12.6中是
一倍的(经验)Rademacher 复杂度, 而(经验)Rademacher 复杂度大于零。

因此, 为二分类问题量身定制的定理12.6相比于通用的定理12.5来说,
二者的区别在 于定理12.6考虑了二分类的特殊情况,
得到了比定理12.5更紧的泛化误差界, 仅此而已。

下面做一些证明：

(1)首先通过式(12.49)将值域为 $\{-1,+1\}$ 的假设空间 $\mathcal{H}$
转化为值域为 $[0,1]$ 的函数空间 $\mathcal{F}_{\mathcal{H}}$ ;

(2)接下来是该证明最核心部分, 即证明式(12.50)的结论
$\widehat{R}_Z\left(\mathcal{F}_{\mathcal{H}}\right)=\frac{1}{2} \widehat{R}_D(\mathcal{H})$
: 第 1 行等号就是定义 $12.8$; 第 2 行等号就是根据式(12.49)将
$f_h\left(\boldsymbol{x}_i, y_i\right)$ 换为
$\mathbb{I}\left(h\left(\boldsymbol{x}_i\right) \neq y_i\right)$; 第 3
行等号类似于式(12.36)的第 2 个等号; 第 4 行等号说明如下:


$$
\sup _{h \in \mathcal{H}} \frac{1}{m} \sum_{i=1}^m \sigma_i \frac{1-y_i h\left(\boldsymbol{x}_i\right)}{2}=\sup _{h \in \mathcal{H}} \frac{1}{2 m} \sum_{i=1}^m \sigma_i+\sup _{h \in \mathcal{H}} \frac{1}{2 m} \sum_{i=1}^m \frac{-y_i \sigma_i h\left(\boldsymbol{x}_i\right)}{2}
$$



其中 $\sup _{h \in \mathcal{H}} \frac{1}{2 m} \sum_{i=1}^m \sigma_i$ 与
$h$ 无关, 所以
$\sup _{h \in \mathcal{H}} \frac{1}{2 m} \sum_{i=1}^m \sigma_i=\frac{1}{2 m} \sum_{i=1}^m \sigma_i$,
即第 4 行等号; 第 5 行等号是由于
$\mathbb{E}_{\boldsymbol{\sigma}}\left[\frac{1}{m} \sum_{i=1}^m \sigma_i\right]=0$,
例如当 $m=2$ 时, 所有可能得 $\boldsymbol{\sigma}$ 包括 $(-1,-1)$,
$(-1,+1),(+1,-1)$ 和 $(+1,+1)$, 求期望后显然结果等于 0 ; 第 6
行等号正如边注所说, " $-y_i \sigma_i$ 与 $\sigma_i$ 分布相同"
(原因跟定理12.5中证明
$\mathbb{E}_Z[\Phi(Z)] \leqslant 2 R_m(\mathcal{F})$ 相同,
即求期望时要针对所 有可能的 $\sigma$ 参见"西瓜书"第 282 页第 8 行); 第 7
行等号再次使用了定义12.8。

(3)关于式(12.51), 根据式(12.50)的结论, 可证明如下:


$$
R_m\left(\mathcal{F}_{\mathcal{H}}\right)=\mathbb{E}_Z\left[\widehat{R}_Z\left(\mathcal{F}_{\mathcal{H}}\right)\right]=\mathbb{E}_D\left[\frac{1}{2} \widehat{R}_D(\mathcal{H})\right]=\frac{1}{2} \mathbb{E}_D\left[\widehat{R}_D(\mathcal{H})\right]=\frac{1}{2} R_m(\mathcal{H})
$$



其中第 2 个等号由 $Z$ 变为 $D$ 只是符号根据具体情况的适时变化而已。

(4)最后, 将式(12.49)定义的 $f_h$ 替换定理 $12.5$ 中的函数 $f$, 则


$$
\begin{gathered}
\mathbb{E}[f(\boldsymbol{z})]=\mathbb{E}[\mathbb{I}(h(\boldsymbol{x}) \neq y)]=E(h) \\
\frac{1}{m} \sum_{i=1}^m f\left(\boldsymbol{z}_i\right)=\frac{1}{m} \sum_{i=1}^m \mathbb{I}\left(h\left(\boldsymbol{x}_i\right) \neq y_i\right)=\widehat{E}(h)
\end{gathered}
$$


 将式(12.51)代入式(12.42), 即用
$\frac{1}{2} R_m(\mathcal{H})$ 替换式(12.42)的 $R_m(\mathcal{F})$,
式(12.47)得证;

将式(12.50)代入式(12.43), 即用 $\frac{1}{2} \widehat{R}_D(\mathcal{H})$
替换式(12.43)的 $\widehat{R}_Z(\mathcal{F})$, 式(12.48)得证。

这里有个疑问在于，定理 $12.5$ 的前提是 "实值函数空间
$\mathcal{F}: \mathcal{Z} \rightarrow[0,1]$ ", 而式(12.49) 得到的函数
$f_h(z)$ 的值域实际为 $\{0,1\}$, 仍是离散的而非实值的; 当然, 定理 $12.5$
的证明也 只需要其函数值在 $[0,1]$ 范围内即可, 并不需要其连续。

### 12.6.1 式(12.52)的证明

比较繁琐，同书上所示，参见Foundations of Machine
Learning[6]

### 12.6.2 式(12.53)的推导

根据式12.28有$\Pi_{\mathcal{H}}(m) \leqslant\left(\frac{e \cdot m}{d}\right)^{d}$，根据式12.52有$R_{m}(\mathcal{H}) \leqslant \sqrt{\frac{2 \ln \Pi_{\mathcal{H}}(m)}{m}}$，因此$\Pi_{\mathcal{H}}(m) \leqslant \sqrt{\frac{2 d \ln \frac{e m}{d}}{m}}$，再根据式12.47
$E(h) \leqslant \widehat{E}(h)+R_{m}(\mathcal{H})+\sqrt{\frac{\ln (1 / \delta)}{2 m}}$
即证。

## 12.7 稳定性

上上节中介绍的基于VC维的泛化误差界是分布无关、数据独立的，上一节介绍的Rademacher复杂度则在一定程度上考虑了数据分布，但二者得到的结果均与具体学习算法无关；本节将要介绍的稳定性分析可以获得与算法有关的分析结果。算法的"稳定性"考察的是算法在输入发生变化时，输出是否会随之发生较大的变化。

### 12.7.1 泛化/经验/留一损失的解释

根据式(12.54)上方关于损失函数的描述："刻画了假设 的预测标记 与真实标记
之间的差别"，这里针对的是二分类，预测标记和真实标记均只能取 和
两个值，它们之间的"差别"又能是什么呢？

因此，当"差别"取为
时，式(12.54)的泛化损失就是式(12.1)的泛化误差，式(12.55)的经验损失就是式(12.2)的经验误差，如果类似于式(12.1)和式(12.2)继续定义留一误差，那么式(12.56)就对应于留一误差。

### 12.7.2 式(12.57)的解释

根据三角不等式[7]，有$|a+b| \leq|a|+|b|$，将$a=\ell\left(\mathfrak{L}_{D}, \boldsymbol{z}\right)-\ell\left(\mathfrak{L}_{D^{i}}\right)$，$b=\ell\left(\mathfrak{L}_{D^{i}, \boldsymbol{z}}\right)-\ell\left(\mathfrak{L}_{D^{\backslash i}, \boldsymbol{z}}\right)$带入即可得出第一个不等式，根据$D^{\backslash i}$表示移除$D$中第$i$个样本，$D^i$表示替换$D$中第$i$个样本，那么$a,b$的变动均为一个样本，根据式12.57，$a\leqslant\beta, b\leqslant\beta$，因此$a +b \leqslant 2\beta$。

### 12.7.3 定理12.8的解释

西瓜书在该定理下方已明确给出该定理的意义, 即 "定理 $12.8$
给出了基于稳定性分析 推导出的学习算法 $\mathfrak{L}$
学得假设的泛化误差界", 式(12.58)和式(12.59)分别基于经验损失和留
一损失给出了泛化损失的上界。接下来讨论两个相关问题:

（1）定理 $12.8$ 的条件包括损失函数有界, 即
$0 \leqslant \ell\left(\mathfrak{L}_D, \boldsymbol{z}\right) \leqslant M$;
如本节第 1 条注解 "泛 化/经验/留一损失的解释" 中所述, 若 "差别" 取为
$\mathbb{I}\left(\mathfrak{L}_D(\boldsymbol{x}), y\right)$,
则泛化损失对应于泛化误 差, 此时上限 $M=1$ 。

（2）在前面泛化误差上界的推导中（例如定理 12.1、定理 12.3、定理 12.6、定理
12.7), 上界中与样本数 $m$ 有关的项收玫率均为 $O(1 / \sqrt{m})$,
但在该定理中却是 $O(\beta \sqrt{m})$; 一般来讲, 随着样本数 $m$ 的增加,
经验误差/损失应该收玫于泛化误差/损失, 因此这里假设 $\beta=1 / m$ (书
中式(12.59)下方第 3 行写为 $\beta=O(1 / m)$ ), 而在第 2 条注解 "定义
$12.10$ 的解释" 中已经提 到 $\beta$ 的取值的确会随着样本数 $m$
的增多会变小, 虽然书中并没有严格去讨论 $\beta$ 随 $m$ 增多的变 化规律,
但至少直觉上是对的。

### 12.7.4 式(12.60)的推导

将$\beta=\frac{1}{m}$带入至式(12.58)即得证。

### 12.7.5 经验损失最小化

顾名思义, "经验损失最小化" 指通过最小化经验损失来求得假设函数。

这里, "对于损失函数 $\ell$, 若学习算法 $\mathfrak{L}$
所输出的假设满足经验损失最小化, 则称算法 $\mathfrak{L}$
满足经验风险最小化原则, 简称算法是 ERM 的"。 在"西瓜书"第 278 页,
若学习算法 $\mathfrak{L}$ 输出的假设 $h$ 满足式(12.30), 则也称
$\mathfrak{L}$ 为满足经验风险最小 化原则的算法。而很明显,
式(12.30)是在最小化经验误差。

那么最小化经验误差和最小化经验损失有什么区别么?

在\"西瓜书"第 286 页左下角边注中提到,
"最小化经验误差和最小化经验损失有时并不相同, 这
是由于存在某些病态的损失函数 $\ell$
使得最小化经验损失并不是最小化经验误差"。

对于 "误差"、"损失"、"风险" 等概念的辨析，参见"西瓜书"第 2 章 $2.1$
节的注解。

### 12.7.6 定理(12.9)的证明的解释

首先明确几个概念，ERM表示算法$\mathcal{L}$满足经验风险最小化(Empirical
Risk
Minimization)。由于$\mathcal{L}$满足经验误差最小化，则可令$g$表示假设空间中具有最小泛化损失的假设，即


$$
\ell(g, \mathcal{D})=\min _{h \in \mathcal{H}} \ell(h, \mathcal{D})
$$




再令


$$
\begin{array}{l}{\epsilon^{\prime}=\frac{\epsilon}{2}} \\ {\frac{\delta}{2}=2 \exp \left(-2 m\left(\epsilon^{\prime}\right)^{2}\right)}\end{array}
$$




将$\epsilon^\prime=\frac{\epsilon}{2}$带入到${\frac{\delta}{2}=2 \exp \left(-2 m\left(\epsilon^{\prime}\right)^{2}\right)}$可以解得$m=\frac{2}{\epsilon^{2}} \ln \frac{4}{\delta}$，由Hoeffding不等式12.6，

$$
P\left(\left\vert\frac{1}{m} \sum_{i=1}^{m} x_{i}-\frac{1}{m} \sum_{i=1}^{m} \mathbb{E}\left(x_{i}\right)\right\vert \geqslant \epsilon\right) \leqslant 2 \exp \left(-2 m \epsilon^{2}\right)
$$



其中$\frac{1}{m} \sum_{i=1}^{m} \mathbb{E}\left(x_{i}\right)=\ell(g, \mathcal{D})$，$\frac{1}{m} \sum_{i=1}^{m} x_{i}=\widehat{\ell}(g, \mathcal{D})$，带入可得


$$
P(|\ell(g, \mathcal{D})-\widehat{\ell}(g, D)| \geqslant \frac{\epsilon}{2})\leqslant \frac{\delta}{2}
$$




根据逆事件的概率可得


$$
P(|\ell(g, \mathcal{D})-\widehat{\ell}(g, D)| \leqslant \frac{\epsilon}{2})\geqslant 1- \frac{\delta}{2}
$$




即文中$|\ell(g, \mathcal{D})-\widehat{\ell}(g, D)| \leqslant \frac{\epsilon}{2}$至少以$1-\delta/2$的概率成立。

由$\frac{2}{m}+(4+M) \sqrt{\frac{\ln (2 / \delta)}{2 m}}=\frac{\epsilon}{2}$可以求解出


$$
\sqrt{m}=\frac{(4+M) \sqrt{\frac{\ln (2 / \delta)}{2}}+\sqrt{(4+M)^{2} \frac{\ln (2 / \delta)}{2}-4 \times \frac{\epsilon}{2} \times(-2)}}{2 \times \frac{\epsilon}{2}}
$$




即$m=O\left(\frac{1}{\epsilon^{2}} \ln \frac{1}{\delta}\right)$。

由$P(|\ell(g, \mathcal{D})-\widehat{\ell}(g, D)| \leqslant \frac{\epsilon}{2})\geqslant 1- \frac{\delta}{2}$可以按照同公式12.31中介绍的相同的方法推导出


$$
P(\ell(\mathfrak{L}, \mathcal{D})-\ell(g, \mathcal{D})\leqslant\epsilon)\geqslant 1-\delta
$$




又因为$m$为与$\left(1/\epsilon,1/\delta,\text{size}(x),\text{size}(c)\right)$相关的多项式的值，因此根据定理12.2，定理12.5，得到结论$\mathcal{H}$是(不可知)PAC可学习的。

## 参考文献
[1] Wassily Hoeffding. Probability inequalities for sums of bounded random variables. Journal of the
American statistical association, 58(301):13–30, 1963.

[2] Vladimir N Vapnik and A Ya Chervonenkis. On the uniform convergence of relative frequencies of
events to their probabilities. In Measures of complexity, pages 11–30. Springer, 2015.

[3] Wikipedia contributors. Binomial theorem, 2020.

[4] Wikipedia contributors. E, 2020.

[5] robjohn. Supremum of the difference of two functions, 2013.

[6] Mehryar Mohri, Afshin Rostamizadeh, and Ameet Talwalkar. Foundations of machine learning. 2018.

[7] Wikipedia contributors. Triangle inequality, 2020.
