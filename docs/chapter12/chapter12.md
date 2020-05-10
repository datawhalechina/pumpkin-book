## 12.1

$$
E(h ; \mathcal{D})=P_{\boldsymbol{x} \sim \mathcal{D}}(h(\boldsymbol{x}) \neq y)
$$

[解析]：该式为泛化误差的定义式，所谓泛化误差，是指当样本$x$从真实的样本分布$\mathcal{D}$中采样后其预测值$h(\boldsymbol{x})$不等于真实值$y$的概率。在现实世界中，我们很难获得样本分布$\mathcal{D}$，我们拿到的数据集可以看做是从样本分布$\mathcal{D}$中独立同分布采样得到的。在西瓜书中，我们拿到的数据集，称为样例集$D$[也叫观测集、样本集，注意与花体$\mathcal{D}$的区别]。



## 12.2

$$
\widehat{E}(h ; D)=\frac{1}{m} \sum_{i=1}^{m} \mathbb{I}\left(h\left(\boldsymbol{x}_{i}\right) \neq y_{i}\right)
$$

[解析]：该式为经验误差的定义式，所谓经验误差，是指观测集$D$中的样本$x_i, i=1,2,\cdots,m$的预测值$h(\boldsymbol{x}_i)$和真实值$y_i$的期望误差。



## 12.3

$$
d\left(h_{1}, h_{2}\right)=P_{\boldsymbol{x} \sim \mathcal{D}}\left(h_{1}(\boldsymbol{x}) \neq h_{2}(\boldsymbol{x})\right)
$$



[解析]：假设我们有两个模型$h_1$和$h_2$，将它们同时作用于样本$\boldsymbol{x}$上，那么他们的”不合“度定义为这两个模型预测值不相同的概率。



## 12.4

$$
f(\mathbb{E}(x)) \leqslant \mathbb{E}(f(x))
$$

[解析]：Jensen不等式：这个式子可以做很直观的理解，比如说在二维空间上，凸函数可以想象成开口向上的抛物线，假如我们有两个点$x_1, x_2$，那么$f(\mathbb{E}(x))$表示的是两个点的均值的纵坐标，而$\mathbb{E}(f(x))$表示的是两个点纵坐标的均值，因为两个点的均值落在抛物线的凹处，所以均值的纵坐标会小一些。



## 12.5

$$
P\left(\frac{1}{m} \sum_{i=1}^{m} x_{i}-\frac{1}{m} \sum_{i=1}^{m} \mathbb{E}\left(x_{i}\right) \geqslant \epsilon\right) \leqslant \exp \left(-2 m \epsilon^{2}\right)
$$

[解析]：Hoeffding 不等式：对于独立随机变量$x_1, x_2, \cdots, x_m$来说，他们观测值$x_i$的均值$\frac{1}{m} \sum_{i=1}^{m} x_{i}$总是和他们期望$\mathbb{E}(x_i)$的均值$\frac{1}{m} \sum_{i=1}^{m} \mathbb{E}\left(x_{i}\right)$相近，上式从概率的角度对这样一个结论进行了描述：即**它们之间差值不小于$\epsilon$**这样的事件出现的概率不大于$\exp \left(-2 m \epsilon^{2}\right)$，可以看出当观测到的变量越多，观测值的均值越逼近期望的均值。



## 12.6

$$
P\left(\left\vert\frac{1}{m} \sum_{i=1}^{m} x_{i}-\frac{1}{m} \sum_{i=1}^{m} \mathbb{E}\left(x_{i}\right)\right\vert \geqslant \epsilon\right) \leqslant 2 \exp \left(-2 m \epsilon^{2}\right)
$$

[解析]：略



## 12.7

$$
P\left(f\left(x_{1}, \ldots, x_{m}\right)-\mathbb{E}\left(f\left(x_{1}, \ldots, x_{m}\right)\right) \geqslant \epsilon\right) \leqslant \exp \left(\frac{-2 \epsilon^{2}}{\sum_{i} c_{i}^{2}}\right)
$$

[解析]：McDiarmid不等式：首先解释下前提条件：$$
\sup _{x_{1}, \ldots, x_{m}, x_{i}^{\prime}}\left|f\left(x_{1}, \ldots, x_{m}\right)-f\left(x_{1}, \ldots, x_{i-1}, x_{i}^{\prime}, x_{i+1}, \ldots, x_{m}\right)\right| \leqslant c_{i}
$$ 表示当函数$f$某个输入$x_i$变到$x_i^\prime$的时候，其变化的上确$\sup$仍满足不大于$c_i$。所谓上确界sup可以理解成变化的极限最大值，可能取到也可能无穷逼近。当满足这个条件时，McDiarmid不等式指出：函数值$f(x_1,\dots,x_m)$和其期望值$\mathbb{E}\left(f(x_1,\dots,x_m)\right)$也相近，从概率的角度描述是：它们之间差值不小于$\epsilon$这样的事件出现的概率不大于$
\exp \left(\frac{-2 \epsilon^{2}}{\sum_{i} c_{i}^{2}}\right)
$，可以看出当每次变量改动带来函数值改动的上限越小，函数值和其期望越相近。

## 12.8

$$
P\left(\left|f\left(x_{1}, \ldots, x_{m}\right)-\mathbb{E}\left(f\left(x_{1}, \ldots, x_{m}\right)\right)\right| \geqslant \epsilon\right) \leqslant 2 \exp \left(\frac{-2 \epsilon^{2}}{\sum_{i} c_{i}^{2}}\right)
$$

[解析]：略



## 12.9

$$
P(E(h)\le\epsilon)\ge 1-\delta
$$

[解析]：PAC辨识的定义：$E(h)$表示算法$\mathcal{L}$在用观测集$D$训练后输出的假设函数$h$，它的泛化误差(见公式12.1)。这个概率定义指出，如果$h$的泛化误差不大于$\epsilon$的概率不小于$1-\delta$，那么我们称学习算法$\mathcal{L}$能从假设空间$\mathcal{H}$中PAC辨识概念类$\mathcal{C}$。



**从式12.10到式12.14的公式是为了回答一个问题：到底需要多少样例才能学得目标概念$c$的有效近似。只要训练集$D$的规模能使学习算法$\mathcal{L}$以概率$1-\delta$找到目标假设的$\epsilon$近似即可。下面就是用数学公式进行抽象**

## 12.10

$$
\begin{aligned} P(h(\boldsymbol{x})=y) &=1-P(h(\boldsymbol{x}) \neq y) \\ &=1-E(h) \\ &<1-\epsilon \end{aligned}
$$

[解析]：$P(h(\boldsymbol{x})=y) =1-P(h(\boldsymbol{x}) \neq y)$ 因为它们是对立事件，$P(h(x)\neq y)=E(h)$是泛化误差的定义(见12.1)，由于我们假定了泛化误差$E(h)>\epsilon$，因此有$1-E(h)<1-\epsilon$。



## 12.11

$$
\begin{aligned} P\left(\left(h\left(\boldsymbol{x}_{1}\right)=y_{1}\right) \wedge \ldots \wedge\left(h\left(\boldsymbol{x}_{m}\right)=y_{m}\right)\right) &=(1-P(h(\boldsymbol{x}) \neq y))^{m} \\ &<(1-\epsilon)^{m} \end{aligned}
$$



[解析]：先解释什么是$h$与$D$“表现一致”，12.2节开头阐述了这样的概念，如果$h$能将$D$中所有样本按与真实标记一致的方式完全分开，我们则称$h$对$D$是一致的。即$\left(h\left(\boldsymbol{x}_{1}\right)=y_{1}\right) \wedge \ldots \wedge\left(h\left(\boldsymbol{x}_{m}\right)=y_{m}\right)$为True。因为每个事件是独立的，所以上式可以写成$P\left(\left(h\left(\boldsymbol{x}_{1}\right)=y_{1}\right) \wedge \ldots \wedge\left(h\left(\boldsymbol{x}_{m}\right)=y_{m}\right)\right)=\prod_{i=1}^{m} P\left(h\left(\boldsymbol{x}_{i}\right)=y_{i}\right)$。根据对立事件的定义有：$\prod_{i=1}^{m} P\left(h\left(\boldsymbol{x}_{i}\right)=y_{i}\right)=\prod_{i=1}^{m}\left(1-P\left(h\left(\boldsymbol{x}_{i}\right) \neq y_{i}\right)\right)$，又根据公式(12.10)，有$$\prod_{i=1}^{m}\left(1-P\left(h\left(\boldsymbol{x}_{i}\right) \neq y_{i}\right)\right)<\prod_{i=1}^{m}(1-\epsilon)=(1-\epsilon)^{m}$$



## 12.12

$$
\begin{aligned} P(h \in \mathcal{H}: E(h)>\epsilon \wedge \widehat{E}(h)=0) &<|\mathcal{H}|(1-\epsilon)^{m} \\ &<|\mathcal{H}| e^{-m \epsilon} \end{aligned}
$$

[解析]：首先解释为什么”我们事先并不知道学习算法$\mathcal{L}$会输出$\mathcal{H}$中的哪个假设“，因为一些学习算法对用一个观察集$D$的输出结果是非确定的，比如感知机就是个典型的例子，训练样本的顺序也会影响感知机学习到的假设$h$参数的值。泛化误差大于$\epsilon$且经验误差为0的假设(即在训练集上表现完美的假设)出现的概率可以表示为$P(h \in \mathcal{H}: E(h)>\epsilon \wedge \widehat{E}(h)=0)$，根据式12.11，每一个这样的假设$h$都满足$P(E(h)>\epsilon \wedge \widehat{E}(h)=0)<\left(1-\epsilon \right)^m$，假设一共有$\vert\mathcal{H}\vert$这么多个这样的假设$h$，因为每个假设$h$满足$E(h)>\epsilon$和$\widehat{E}(h)=0$成立的事件是互斥的，因此总的概率$P(h \in \mathcal{H}: E(h)>\epsilon \wedge \widehat{E}(h)=0)$就是这些互斥事件之和即
$$
\begin{aligned}P\left(h \in \mathcal{H}: E(h)>\epsilon \wedge \widehat{E}(h)=0\right) &=\sum_i^{\mathcal{\vert H\vert}}P\left(E(h_i)>\epsilon \wedge \widehat{E}(h_i)=0\right)\\&<|\mathcal{H}|(1-\epsilon)^{m}\end{aligned}
$$
小于号依据公式(12.11)。

第二个小于号实际上是要证明$\vert\mathcal{H}\vert(1-\epsilon)^m < \vert\mathcal{H}\vert e^{-m\epsilon}$，即证明$(1-\epsilon)^m < e^{-m\epsilon}$，其中$\epsilon\in(0,1]$，$m$是正整数，推导如下：

[推导]：当$\epsilon=1$时，显然成立，当$\epsilon\in(0, 1)$时，因为左式和右式的值域均大于0，所以可以左右两边同时取对数，又因为对数函数是单调递增函数，所以即证明$m\ln(1-\epsilon) < -m\epsilon$，即证明$\ln(1-\epsilon)<-\epsilon$，这个式子很容易证明：令$f(\epsilon)=\ln(1-\epsilon) + \epsilon$，其中$\epsilon\in(0,1)$，$f^\prime(\epsilon)=1-\frac{1}{1-\epsilon}=0 \Rightarrow \epsilon=0$ 取极大值0，因此$ln(1-\epsilon)<-\epsilon$ 也即$\vert\mathcal{H}\vert(1-\epsilon)^m < \vert\mathcal{H}\vert e^{-m\epsilon}$成立。



## 12.13

$$
|\mathcal{H}| e^{-m \epsilon} \leqslant \delta
$$

[解析]：回到我们要回答的问题：到底需要多少样例才能学得目标概念$c$的有效近似。只要训练集$D$的规模能使学习算法$\mathcal{L}$以概率$1-\delta$找到目标假设的$\epsilon$近似即可。根据式12.12，学习算法$\mathcal{L}$生成的假设大于目标假设的$\epsilon$近似的概率为$P\left(h \in \mathcal{H}: E(h)>\epsilon \wedge \widehat{E}(h)=0\right)<\vert\mathcal{H}\vert e^{-m\epsilon}$，因此学习算法$\mathcal{L}$生成的假设落在目标假设的$\epsilon$近似的概率为$1-P\left(h \in \mathcal{H}: E(h)>\epsilon \wedge \widehat{E}(h)=0\right)\ge 1-\vert\mathcal{H}\vert e^{-m\epsilon}$，这个概率我们希望是$1-\delta$，因此$1-\delta\geqslant 1-\vert\mathcal{H}\vert e^{-m\epsilon}\Rightarrow\vert\mathcal{H}\vert e^{-m\epsilon}\leqslant\delta$

## 12.14

$$
m \geqslant \frac{1}{\epsilon}\left(\ln |\mathcal{H}|+\ln \frac{1}{\delta}\right)
$$

[推导]：
$$
\begin{aligned}
\vert\mathcal{H}\vert e^{-m \epsilon} &\leqslant \delta\\
e^{-m \epsilon} &\leqslant \frac{\delta}{\vert\mathcal{H}\vert}\\
-m \epsilon &\leqslant \ln\delta-\ln\vert\mathcal{H}\vert\\
m &\geqslant \frac{1}{\epsilon}\left(\ln |\mathcal{H}|+\ln \frac{1}{\delta}\right)
\end{aligned}
$$
[解析]：这个式子告诉我们，在假设空间$\mathcal{H}$是PAC可学习的情况下，输出假设$h$的泛化误差$\epsilon$随样本数目$m$增大而收敛到0，收敛速率为$O(\frac{1}{m})$。这也是我们在机器学习中的一个共识，即可供模型训练的观测集样本数量越多，机器学习模型的泛化性能越好。



## 12.15

$$
P(\widehat{E}(h)-E(h) \geqslant \epsilon) \leqslant \exp \left(-2 m \epsilon^{2}\right)
$$



[解析]：参见12.5



## 12.16

$$
P(E(h)-\widehat{E}(h) \geqslant \epsilon) \leqslant \exp \left(-2 m \epsilon^{2}\right)
$$



[解析]：参见12.5



## 12.17

$$
P(|E(h)-\widehat{E}(h)| \geqslant \epsilon) \leqslant 2 \exp \left(-2 m \epsilon^{2}\right)
$$



[解析]：参见12.6



## 12.18

$$
\widehat{E}(h)-\sqrt{\frac{\ln (2 / \delta)}{2 m}} \leqslant E(h) \leqslant \widehat{E}(h)+\sqrt{\frac{\ln (2 / \delta)}{2 m}}
$$

[推导]：令$\delta=2e^{-2m\epsilon^2}$，则$\epsilon=\sqrt{\frac{\ln(2/\delta)}{2m}}$，由式12.17
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



## 12.19

$$
P\left(|E(h)-\widehat{E}(h)| \leqslant \sqrt{\frac{\ln |\mathcal{H}|+\ln (2 / \delta)}{2 m}}\right) \geqslant 1-\delta
$$

[推导]：

令$h_1,h_2,\dots,h_{\vert\mathcal{H}\vert}$表示假设空间$\mathcal{H}$中的假设，有
$$
\begin{aligned} 
& P(\exists h \in \mathcal{H}:|E(h)-\widehat{E}(h)|>\epsilon) \\
=& P\left(\left(\left|E_{h_{1}}-\widehat{E}_{h_{1}}\right|>\epsilon\right) \vee \ldots \vee\left(| E_{h_{|\mathcal{H}|}}-\widehat{E}_{h_{|\mathcal{H}|} |>\epsilon}\right)\right) \\ \leqslant & \sum_{h \in \mathcal{H}} P(|E(h)-\widehat{E}(h)|>\epsilon) 
\end{aligned}
$$
这一步是很好理解的，存在一个假设$h$使得$|E(h)-\widehat{E}(h)|>\epsilon$的概率可以表示为对假设空间内所有的假设$h_i, i\in 1,\dots,\vert\mathcal{H}\vert$，使得$\left|E_{h_{i}}-\widehat{E}_{h_{i}}\right|>\epsilon$这个事件的"或"事件。因为$P(A\vee B)=P(A) + P(B) - P(A\wedge B)$，而$P(A\wedge B)\geqslant 0$，所以最后一行的不等式成立。

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



## 12.20

$$
P\left(E(h)-\min _{h^{\prime} \in \mathcal{H}} E\left(h^{\prime}\right) \leqslant \epsilon\right) \geqslant 1-\delta
$$

[解析]：这个式子是”不可知PAC可学习“的定义式，不可知是指当目标概念$c$不在算法$\mathcal{L}$所能生成的假设空间$\mathcal{H}$里。可学习是指如果$\mathcal{H}$中泛化误差最小的假设是$\arg\min_{h\in \mathcal{H}}E(h)$，且这个假设的泛化误差满足其与目标概念的泛化误差的差值不大于$\epsilon$的概率不小于$1-\delta$。我们称这样的假设空间$\mathcal{H}$是不可知PAC可学习的。



## 12.21

$$
\Pi_{\mathcal{H}}(m)=\max _{\left\{\boldsymbol{x}_{1}, \ldots, \boldsymbol{x}_{m}\right\} \subseteq \mathcal{X}}\left|\left\{\left(h\left(\boldsymbol{x}_{1}\right), \ldots, h\left(\boldsymbol{x}_{m}\right)\right) | h \in \mathcal{H}\right\}\right|
$$

[解析]：这个是增长函数的定义式。增长函数$\Pi_{\mathcal{H}}(m)$表示假设空间$\mathcal{H}$对m个样本所能赋予标签的最大可能的结果数。比如对于两个样本的二分类问题，一共有4中可能的标签组合$[[0, 0], [0, 1], [1, 0], [1, 1]]$，如果假设空间$\mathcal{H}_1$能赋予这两个样本两种标签组合$[[0, 0], [1, 1]]$，则$\Pi_{\mathcal{H}_1}(2)=2$。显然，$\mathcal{H}$对样本所能赋予标签的可能结果数越多，$\mathcal{H}$的表示能力就越强。增长函数可以用来反映假设空间$\mathcal{H}$的复杂度。



## 12.22

$$
P(|E(h)-\widehat{E}(h)|>\epsilon) \leqslant 4 \Pi_{\mathcal{H}}(2 m) \exp \left(-\frac{m \epsilon^{2}}{8}\right)
$$

[解析]：这个式子的前提假设有误，应当写成对假设空间$\mathcal{H}$，$m\in\mathbb{N}$，$0<\epsilon<1$，**存在**$h\in\mathcal{H}$

详细证明参见原论文 https://courses.engr.illinois.edu/ece544na/fa2014/vapnik71.pdf



## 12.23

$$
\mathrm{VC}(\mathcal{H})=\max \left\{m: \Pi_{\mathcal{H}}(m)=2^{m}\right\}
$$

[解析]：这是VC维的定义式：VC维的定义是能被$\mathcal{H}$打散的最大示例集的大小。西瓜书中例12.1和例12.2 给出了形象的例子。注意，VC维的定义式上的底数2表示这个问题是2分类的问题。如果是$n$分类的问题，那么定义式中底数需要变为$n$。



## 12.24

$$
\Pi_{\mathcal{H}}(m) \leqslant \sum_{i=0}^{d}\left(\begin{array}{c}{m} \\ {i}\end{array}\right)
$$
[解析]：首先解释下数学归纳法的起始条件"当$m=1, d=0$或$d=1$时，定理成立"，当$m=1,d=0$时，由VC维的定义(式12.23) $\mathrm{VC}(\mathcal{H})=\max \left\{m: \Pi_{\mathcal{H}}(m)=2^{m}\right\}=0$ 可知$\Pi_{\mathcal{H}}(1)<2$，否则$d$可以取到1，又因为$\Pi_{\mathcal{H}}(m)$为整数，所以$\Pi_{\mathcal{H}}(1)\in[0, 1]$，式12.24右边为$\sum_{i=0}^{0}\left(\begin{array}{c}{1} \\ {i}\end{array}\right)=1$，因此不等式成立。当$m=1,d=1$时，因为一个样本最多只能有两个类别，所以$\Pi_\mathcal{H}(1)=2$，不等式右边为$\sum_{i=0}^{1}\left(\begin{array}{c}{1} \\ {i}\end{array}\right)=2$，因此不等式成立。

再介绍归纳过程，这里采样的归纳方法是假设式12.24对$(m-1, d-1)$和$(m-1, d)$成立，推导出其对$(m,d)$也成立。证明过程中引入观测集$D=\left\{\boldsymbol{x}_{1}, \boldsymbol{x}_{2}, \ldots, \boldsymbol{x}_{m}\right\}$ 和观测集$D^\prime=\left\{\boldsymbol{x}_{1}, \boldsymbol{x}_{2}, \ldots, \boldsymbol{x}_{m-1}\right\}$，其中$D$比$D^\prime$多一个样本$x_m$，它们对应的假设空间可以表示为：
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
由记号$\mathcal{H}_{\vert D^\prime}$的定义可知，$\vert\mathcal{H}_{\vert D^\prime}\vert \geqslant \left\lfloor\frac{\vert\mathcal{H}_{\vert D}\vert}{2}\right\rfloor$，因此$\vert\mathcal{H}_{D\vert D^\prime}\vert \leqslant \left\lfloor\frac{\vert\mathcal{H}_{\vert D}\vert}{2}\right\rfloor$，由于样本集$D$的数量为$m$，根据增长函数的概念，有$\left|\mathcal{H}_{D| D^{\prime}}\right| \leqslant \left\lfloor\frac{\vert\mathcal{H}_{\vert D}\vert}{2}\right\rfloor\leqslant \Pi_{\mathcal{H}}(m-1)$。

假设$Q$表示能被$\mathcal{H}_{D^\prime\vert D}$打散的集合，因为根据$\mathcal{H}_{D^\prime\vert D}$的定义，$H_{D}$必对元素$x_m$给定了不一致的判定，因此$Q \cup\left\{\boldsymbol{x}_{m}\right\}$必能被$\mathcal{H}_{\vert D}$打散，由前提假设$\mathcal{H}$的VC维为$d$，因此$\mathcal{H}_{D^\prime\vert D}$的VC维最大为$d-1$，综上有
$$
\left|\mathcal{H}_{D| D^{\prime}}\right| \leqslant \Pi_{\mathcal{H}}(m-1) \leqslant \sum_{i=0}^{d-1}\left(\begin{array}{c}{m-1} \\ {i}\end{array}\right)
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

## 12.25

$$
\left|\mathcal{H}_{| D}\right|=\left|\mathcal{H}_{| D^{\prime}}\right|+\left|\mathcal{H}_{D^{\prime} | D}\right|
$$



[解析]：参见12.24



## 12.26

$$
\left|\mathcal{H}_{| D^{\prime}}\right| \leqslant \Pi_{\mathcal{H}}(m-1) \leqslant \sum_{i=0}^{d}\left(\begin{array}{c}
m-1 \\
i
\end{array}\right)
$$



[解析]：参见12.24



## 12.27

$$
\left|\mathcal{H}_{D^{\prime} | D}\right| \leqslant \Pi_{\mathcal{H}}(m-1) \leqslant \sum_{i=0}^{d-1}\left(\begin{array}{c}
m-1 \\
i
\end{array}\right)
$$



[解析]：参见12.24



## 12.28

$$
\Pi_{\mathcal{H}}(m) \leqslant\left(\frac{e \cdot m}{d}\right)^{d}
$$
[推导]：
$$
\begin{aligned} \Pi_{\mathcal{H}}(m) & \leqslant \sum_{i=0}^{d}\left(\begin{array}{c}{m} \\ {i}\end{array}\right) \\ & \leqslant \sum_{i=0}^{d}\left(\begin{array}{c}{m} \\ {i}\end{array}\right)\left(\frac{m}{d}\right)^{d-i} \\ &=\left(\frac{m}{d}\right)^{d} \sum_{i=0}^{d}\left(\begin{array}{c}{m} \\ {i}\end{array}\right)\left(\frac{d}{m}\right)^{i} \\ & \leqslant\left(\frac{m}{d}\right)^{d} \sum_{i=0}^{m}\left(\begin{array}{c}{m} \\ {i}\end{array}\right)\left(\frac{d}{m}\right)^{i} \\ 
&={\left(\frac{m}{d}\right)}^d{\left(1+\frac{d}{m}\right)}^m\\
&<\left(\frac{e \cdot m}{d}\right)^{d} \end{aligned}
$$
第一步到第二步和第三步到第四步均因为$m\geqslant d$，第四步到第五步是由于[二项式定理](https://zh.wikipedia.org/wiki/二项式定理)：$(x+y)^{n}=\sum_{k=0}^{n}\left(\begin{array}{l}{n} \\ {k}\end{array}\right) x^{n-k} y^{k}$，其中令$k=i, n=m, x=1, y = \frac{d}{m}$得$\left(\frac{m}{d}\right)^{d} \sum_{i=0}^{m}\left(\begin{array}{c}{m} \\ {i}\end{array}\right)\left(\frac{d}{m}\right)^{i}=\left(\frac{m}{d}\right)^{d} (1+\frac{d}{m})^m$，最后一步的不等式即需证明${\left(1+\frac{d}{m}\right)}^m\leqslant e^d$，因为${\left(1+\frac{d}{m}\right)}^m={\left(1+\frac{d}{m}\right)}^{\frac{m}{d}d}$，根据[自然对数底数$e$的定义](https://en.wikipedia.org/wiki/E_(mathematical_constant))，${\left(1+\frac{d}{m}\right)}^{\frac{m}{d}d}< e^d$，注意原文中用的是$\leqslant$，但是由于$e=\lim _{\frac{d}{m} \rightarrow 0}\left(1+\frac{d}{m}\right)^{\frac{m}{d}}$的定义是一个极限，所以应该是用$<$。



## 12.29

$$
P\left(
E(h)-\widehat{E}(h) \leqslant \sqrt{
\frac{8d\ln\frac{2em}{d}+8\ln\frac{4}{\delta}}{m}
}
\right)
\geqslant 1-\delta
$$


[推导]：这里应该是作者的笔误，根据式12.22，$E(h)-\widehat{E}(h)$应当被绝对值符号包裹。将式12.28带入式12.22得
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
带入式12.22，则定理得证。这个式子是用VC维表示泛化界，可以看出，泛化误差界只与样本数量$m$有关，收敛速率为$\sqrt{\frac{\ln m}{m}}$ (书上简化为$\frac{1}{\sqrt{m}}$)。



## 12.30

$$
\widehat{E}(h)=\min _{h^{\prime} \in \mathcal{H}} \widehat{E}\left(h^{\prime}\right)
$$

[解析]：这个是经验风险最小化的定义式。即从假设空间中找出能使经验风险最小的假设。



## 12.31

$$
E(g)=\min _{h \in \mathcal{H}} E(h)
$$



[解析]：首先回忆PAC可学习的概念，见定义12.2，而可知/不可知PAC可学习之间的区别仅仅在于概念类$c$是否包含于假设空间$\mathcal{H}$中。令
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



## 12.32

$$
\sqrt{\frac{\left(\ln 2 / \delta^{\prime}\right)}{2 m}}=\frac{\epsilon}{2}
$$

[解析]：参见12.31



## 12.34

$$
\sqrt{\frac{8 d \ln \frac{2 e m}{d}+8 \ln \frac{4}{\delta^{\prime}}}{m}}=\frac{\epsilon}{2}
$$



[解析]：参见12.31



## 12.36

$$
\begin{aligned} \widehat{E}(h) &=\frac{1}{m} \sum_{i=1}^{m} \mathbb{I}\left(h\left(\boldsymbol{x}_{i}\right) \neq y_{i}\right) \\ &=\frac{1}{m} \sum_{i=1}^{m} \frac{1-y_{i} h\left(\boldsymbol{x}_{i}\right)}{2} \\ &=\frac{1}{2}-\frac{1}{2 m} \sum_{i=1}^{m} y_{i} h\left(\boldsymbol{x}_{i}\right) \end{aligned}
$$
[解析]：这里解释从第一步到第二步的推导，因为前提假设是2分类问题，$y_k\in\{-1, +1\}$，因此$\mathbb{I}\left(h(x_i)\neq y_i\right)\equiv \frac{1-y_i h(x_i)}{2}$。这是因为假如$y_i=+1, h(x_i)=+1$或$y_i=-1, h(x_i)=-1$，有$\mathbb{I}\left(h(x_i)\neq y_i\right)=1= \frac{1-y_i h(x_i)}{2}$；反之，假如$y_i=-1, h(x_i)=+1$或$y_i=+1, h(x_i)=-1$，有$\mathbb{I}\left(h(x_i)\neq y_i\right)=0= \frac{1-y_i h(x_i)}{2}$。



## 12.37

$$
\underset{h \in \mathcal{H}}{\arg \max } \frac{1}{m} \sum_{i=1}^{m} y_{i} h\left(\boldsymbol{x}_{i}\right)
$$
[解析]：由公式12.36可知，经验误差$\widehat{E}(h)$和$\frac{1}{m} \sum_{i=1}^{m} y_{i} h\left(\boldsymbol{x}_{i}\right)$呈反比的关系，因此假设空间中能使经验误差最小的假设$h$即是使$\frac{1}{m} \sum_{i=1}^{m} y_{i} h\left(\boldsymbol{x}_{i}\right)$最大的$h$。



## 12.38

$$
\sup _{h \in \mathcal{H}} \frac{1}{m} \sum_{i=1}^{m} \sigma_{i} h\left(\boldsymbol{x}_{i}\right)
$$
[解析]：上确界$\sup$这个概念前面已经解释过，见式12.7的解析。由于$\sigma_i$是随机变量，因此这个式子可以理解为求解和随机生成的标签(即$\sigma$)最契合的假设(当$\sigma_i$和$h(\boldsymbol{x}_i)$完全一致时，他们的内积最大)。



## 12.39

$$
\mathbb{E}_{\boldsymbol{\sigma}}\left[\sup _{h \in \mathcal{H}} \frac{1}{m} \sum_{i=1}^{m} \sigma_{i} h\left(\boldsymbol{x}_{i}\right)\right]
$$
[解析]：这个式子可以用来衡量假设空间$\mathcal{H}$的表达能力，对变量$\sigma$求期望可以理解为当变量$\sigma$包含所有可能的结果时，假设空间$\mathcal{H}$中最契合的假设$h$和变量的平均契合程度。因为前提假设是2分类的问题，因此$\sigma_i$一共有$2^m$种，这些不同的$\sigma_i$构成了数据集$D=\{(x_1, y_1), (x_2, y_2),\dots, (x_m, y_m)\}$的”对分“(12.4节)，如果一个假设空间的表达能力越强，那么就越有可能对于每一种$\sigma_i$，假设空间中都存在一个$h$使得$h(x_i)$和$\sigma_i$非常接近甚至相同，对所有可能的$\sigma_i$取期望即可衡量假设空间的整体表达能力，这就是这个式子的含义。



## 12.40

$$
\widehat{R}_{Z}(\mathcal{F})=\mathbb{E}_{\boldsymbol{\sigma}}\left[\sup _{f \in \mathcal{F}} \frac{1}{m} \sum_{i=1}^{m} \sigma_{i} f\left(\boldsymbol{z}_{i}\right)\right]
$$
[解析]：对比式12.39，这里使用函数空间$\mathcal{F}$代替了假设空间$\mathcal{H}$，函数$f$代替了假设$h$，很容易理解，因为假设$h$即可以看做是作用在数据$x_i$上的一个映射，通过这个映射可以得到标签$y_i$。注意前提假设实值函数空间$\mathcal{F}:\mathcal{Z}\rightarrow\mathbb{R}$，即映射$f$将样本$z$隐射到了实数空间，这个时候所有的$\sigma_i$将是一个标量即$\sigma_i\in\{+1, -1\}$。



## 12.41

$$
R_{m}(\mathcal{F})=\mathbb{E}_{Z \subseteq \mathcal{Z}:|Z|=m}\left[\widehat{R}_{Z}(\mathcal{F})\right]
$$
[解析]：这里所要求的是$\mathcal{F}$关于分布$\mathcal{D}$的Rademacher复杂度，因此从$\mathcal{D}$中采出不同的样本$Z$，计算这些样本对应的Rademacher复杂度的期望。



## 12.42

$$
\begin{array}{l}{\mathbb{E}[f(\boldsymbol{z})] \leqslant \frac{1}{m} \sum_{i=1}^{m} f\left(\boldsymbol{z}_{i}\right)+2 R_{m}(\mathcal{F})+\sqrt{\frac{\ln (1 / \delta)}{2 m}}} \\ {\mathbb{E}[f(\boldsymbol{z})] \leqslant \frac{1}{m} \sum_{i=1}^{m} f\left(\boldsymbol{z}_{i}\right)+2 \widehat{R}_{Z}(\mathcal{F})+3 \sqrt{\frac{\ln (2 / \delta)}{2 m}}}\end{array}
$$
[解析]：首先令记号
$$
\begin{aligned} \widehat{E}_{Z}(f) &=\frac{1}{m} \sum_{i=1}^{m} f\left(\boldsymbol{z}_{i}\right) \\ \Phi(Z) &=\sup _{f \in \mathcal{F}} \left(\mathbb{E}[f]-\widehat{E}_{Z}(f)\right) \end{aligned}
$$
即$\widehat{E}_{Z}(f)$表示函数$f$作为假设下的经验误差，$\Phi(Z)$表示经验误差和泛化误差的上确界。再令$Z^\prime$为只与$Z$有一个示例(样本)不同的训练集，不妨设$z_m\in Z$和$z^\prime_m\in Z^\prime$为不同的示例，那么有
$$
\begin{aligned} \Phi\left(Z^{\prime}\right)-\Phi(Z) &=\sup _{f \in \mathcal{F}} \left(\mathbb{E}[f]-\widehat{E}_{Z^{\prime}}(f)\right)-\sup _{f \in \mathcal{F}} \left(\mathbb{E}[f]-\widehat{E}_{Z}(f)\right) \\ & \leqslant \sup _{f \in \mathcal{F}} \left(\widehat{E}_{Z}(f)-\widehat{E}_{Z^{\prime}}(f)\right) \\ &=\sup_{f\in\mathcal{F}}\frac{\sum^m_{i=1}f(z_i)-\sum^m_{i=1}f(z^\prime_i)}{m}\\&=\sup _{f \in \mathcal{F}} \frac{f\left(z_{m}\right)-f\left(z_{m}^{\prime}\right)}{m} \\ & \leqslant \frac{1}{m} \end{aligned}
$$
第一个不等式是因为[上确界的差不大于差的上确界](https://math.stackexchange.com/questions/246015/supremum-of-the-difference-of-two-functions)，第四行的等号由于$Z^\prime$与$Z$只有$z_m$不相同，最后一行的不等式是因为前提假设$\mathcal{F}:\mathcal{Z}\rightarrow [0,1]$，即$f(z_m),f(z_m^\prime)\in[0,1]$。

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
令$ \exp \left(\frac{-2 \epsilon^{2}}{\sum_{i} c_{i}^{2}}\right)=\delta$可以求得$\epsilon=\sqrt{\frac{\ln (1 / \delta)}{2 m}}$，所以
$$
P\left(\Phi(Z)-\mathbb{E}_{Z}[\Phi(Z)] \geqslant \sqrt{\frac{\ln (1 / \delta)}{2 m}}\right) \leqslant \delta
$$
由逆事件的概率定义得
$$
P\left(\Phi(Z)-\mathbb{E}_{Z}[\Phi(Z)] \leqslant \sqrt{\frac{\ln (1 / \delta)}{2 m}}\right) \geqslant 1-\delta
$$
即书中式12.44的结论。

下面来估计$\mathbb{E}_{Z}[\Phi(Z)]$的上界：
$$
\begin{aligned} \mathbb{E}_{Z}[\Phi(Z)] &=\mathbb{E}_{Z}\left[\sup _{f \in \mathcal{F}} \left(\mathbb{E}[f]-\widehat{E}_{Z}(f)\right)\right] \\ &=\mathbb{E}_{Z}\left[\sup _{f \in \mathcal{F}} \mathbb{E}_{Z^{\prime}}\left[\widehat{E}_{Z^{\prime}}(f)-\widehat{E}_{Z}(f)\right]\right] \\ & \leqslant \mathbb{E}_{Z, Z^{\prime}}\left[\sup _{f \in \mathcal{F}}\left( \widehat{E}_{Z^{\prime}}(f)-\widehat{E}_{Z}(f)\right)\right] \\ &=\mathbb{E}_{Z, Z^{\prime}}\left[\sup _{f \in \mathcal{F}} \frac{1}{m} \sum_{i=1}^{m}\left(f\left(\boldsymbol{z}_{i}^{\prime}\right)-f\left(\boldsymbol{z}_{i}\right)\right)\right] \\ &=\mathbb{E}_{\boldsymbol{\sigma}, Z,Z^{\prime}}\left[\sup _{f \in \mathcal{F}} \frac{1}{m} \sum_{i=1}^{m} \sigma_{i}\left(f\left(\boldsymbol{z}_{i}^{\prime}\right)-f\left(\boldsymbol{z}_{i}\right)\right)\right] \\ &\leqslant \mathbb{E}_{\boldsymbol{\sigma}, Z^{\prime}}\left[\sup _{f \in \mathcal{F}} \frac{1}{m} \sum_{i=1}^{m} \sigma_{i} f\left(\boldsymbol{z}_{i}^{\prime}\right)\right]+\mathbb{E}_{\boldsymbol{\sigma}, Z}\left[\sup _{f \in \mathcal{F}} \frac{1}{m} \sum_{i=1}^{m}-\sigma_{i} f\left(\boldsymbol{z}_{i}\right)\right] \\ &=2 \mathbb{E}_{\boldsymbol{\sigma}, Z}\left[\sup _{f \in \mathcal{F}} \frac{1}{m} \sum_{i=1}^{m} \sigma_{i} f\left(\boldsymbol{z}_{i}\right)\right] \\ &=2 R_{m}(\mathcal{F}) \end{aligned}
$$
第二行等式是外面套了一个对服从分布$\mathcal{D}$的示例集$Z^\prime$求期望，因为$\mathbb{E}_{Z^\prime\sim\mathcal{D}}[\widehat{E}_{Z^\prime}(f)]=\mathbb{E}(f)$，而采样出来的$Z^\prime$和$Z$相互独立，因此有$\mathbb{E}_{Z^\prime\sim\mathcal{D}}[\widehat{E}_{Z}(f)]=\widehat{E}_{Z}(f)$。

第三行不等式基于上确界函数$\sup$是个凸函数，将$\sup_{f\in\mathcal{F}}$看做是凸函数$f$，将$\widehat{E}_{Z^{\prime}}(f)-\widehat{E}_{Z}(f)$看做变量$x$根据Jesen不等式(式12.4)，有$\mathbb{E}_{Z}\left[\sup _{f \in \mathcal{F}} \mathbb{E}_{Z^{\prime}}\left[\widehat{E}_{Z^{\prime}}(f)-\widehat{E}_{Z}(f)\right]\right] \leqslant \mathbb{E}_{Z, Z^{\prime}}\left[\sup _{f \in \mathcal{F}}\left( \widehat{E}_{Z^{\prime}}(f)-\widehat{E}_{Z}(f)\right)\right] $，其中$\mathbb{E}_{Z, Z^{\prime}}[\cdot]$是$\mathbb{E}_{Z}[\mathbb{E}_{Z^\prime}[\cdot]]$的简写形式。

第五行引入对Rademacher随机变量的期望，由于函数值空间是标量，因为$\sigma_i$也是标量，即$\sigma_i\in\{-1, +1\}$，且$\sigma_i$总以相同概率可以取到这两个值，因此可以引入$\mathbb{E}_{\sigma}$而不影响最终结果。

第六行利用了[上确界的和不小于和的上确界](https://math.stackexchange.com/questions/246015/supremum-of-the-difference-of-two-functions)，因为第一项中只含有变量$z^\prime$，所以可以将$\mathbb{E}_Z$去掉，因为第二项中只含有变量$z$，所以可以将$\mathbb{E}_{Z^\prime}$去掉。

第七行利用$\sigma$是对称的，所以$-\sigma$的分布和$\sigma$完全一致，所以可以将第二项中的负号去除，又因为$Z$和$Z^\prime$均是从$\mathcal{D}$中$i.i.d.$采样得到的数据，因此可以将第一项中的$z^\prime_i$替换成$z$，将$Z^\prime$替换成$Z$。

最后根据定义式12.41可得$\mathbb{E}_{Z}[\Phi(Z)]=2\mathcal{R}_m(\mathcal{F})$，式12.24得证。

## 12.43

$$
\mathbb{E}[f(\boldsymbol{z})] \leqslant \frac{1}{m} \sum_{i=1}^{m} f\left(\boldsymbol{z}_{i}\right)+2 \widehat{R}_{Z}(\mathcal{F})+3 \sqrt{\frac{\ln (2 / \delta)}{2 m}}
$$



[解析]：参见12.42



## 12.44

$$
\Phi(Z) \leqslant \mathbb{E}_{Z}[\Phi(Z)]+\sqrt{\frac{\ln (1 / \delta)}{2 m}}
$$



[解析]：参见 12.42



## 12.45

$$
R_{m}(\mathcal{F}) \leqslant \widehat{R}_{Z}(\mathcal{F})+\sqrt{\frac{\ln (2 / \delta)}{2 m}}
$$



[解析]：参见12.42



## 12.46

$$
\Phi(Z) \leqslant 2 \widehat{R}_{Z}(\mathcal{F})+3 \sqrt{\frac{\ln (2 / \delta)}{2 m}}
$$



参见12.42



## 12.52

$$
R_{m}(\mathcal{H}) \leqslant \sqrt{\frac{2 \ln \Pi_{\mathcal{H}}(m)}{m}}
$$
[证明]：比较繁琐，同书上所示，参见[Mohri etc., 2012](https://cs.nyu.edu/~mohri/mlbook/)

## 12.53

$$
E(h) \leqslant \widehat{E}(h)+\sqrt{\frac{2 d \ln \frac{e m}{d}}{m}}+\sqrt{\frac{\ln (1 / \delta)}{2 m}}
$$

[解析]：根据式12.28有$\Pi_{\mathcal{H}}(m) \leqslant\left(\frac{e \cdot m}{d}\right)^{d}$，根据式12.52有$R_{m}(\mathcal{H}) \leqslant \sqrt{\frac{2 \ln \Pi_{\mathcal{H}}(m)}{m}}$，因此$\Pi_{\mathcal{H}}(m) \leqslant \sqrt{\frac{2 d \ln \frac{e m}{d}}{m}}$，再根据式12.47 $E(h) \leqslant \widehat{E}(h)+R_{m}(\mathcal{H})+\sqrt{\frac{\ln (1 / \delta)}{2 m}}$ 即证。



## 12.57

$$
\begin{array}{l}{\quad\left|\ell\left(\mathfrak{L}_{D}, \boldsymbol{z}\right)-\ell\left(\mathfrak{L}_{D^{i}}, \boldsymbol{z}\right)\right|} \\ {\leqslant\left|\ell\left(\mathfrak{L}_{D}, \boldsymbol{z}\right)-\ell\left(\mathfrak{L}_{D^{\backslash i}}, \boldsymbol{z}\right)\right|+\left|\ell\left(\mathfrak{L}_{D^{i}, \boldsymbol{z}}\right)-\ell\left(\mathfrak{L}_{D^{\backslash i}, \boldsymbol{z}}\right)\right|} \\ {\leqslant 2 \beta}\end{array}
$$

[解析]：根据[三角不等式]([https://zh.wikipedia.org/zh-hans/%E4%B8%89%E8%A7%92%E4%B8%8D%E7%AD%89%E5%BC%8F](https://zh.wikipedia.org/zh-hans/三角不等式))，有$|a+b| \leq|a|+|b|$，将$a=\ell\left(\mathfrak{L}_{D}, \boldsymbol{z}\right)-\ell\left(\mathfrak{L}_{D^{i}}\right)$，$b=\ell\left(\mathfrak{L}_{D^{i}, \boldsymbol{z}}\right)-\ell\left(\mathfrak{L}_{D^{\backslash i}, \boldsymbol{z}}\right)$带入即可得出第一个不等式，根据$D^{\backslash i}$表示移除$D$中第$i$个样本，$D^i$表示替换$D$中第$i$个样本，那么$a,b$的变动均为一个样本，根据式12.57，$a\leqslant\beta, b\leqslant\beta$，因此$a +b \leqslant 2\beta$。



## 12.58

$$
\ell(\mathfrak{L}, \mathcal{D}) \leqslant \widehat{\ell}(\mathfrak{L}, D)+2 \beta+(4 m \beta+M) \sqrt{\frac{\ln (1 / \delta)}{2 m}}
$$



[证明]：比较繁琐，同书上所示，参见[Mohri etc., 2012](https://cs.nyu.edu/~mohri/mlbook/)



## 12.59

$$
\ell(\mathfrak{L}, \mathcal{D}) \leqslant \ell_{l o o}(\mathfrak{L}, D)+\beta+(4 m \beta+M) \sqrt{\frac{\ln (1 / \delta)}{2 m}}
$$



[证明]：比较繁琐，同书上所示，参见[Mohri etc., 2012](https://cs.nyu.edu/~mohri/mlbook/)



## 12.60

$$
\ell(\mathfrak{L}, \mathcal{D}) \leqslant \widehat{\ell}(\mathfrak{L}, D)+\frac{2}{m}+(4+M) \sqrt{\frac{\ln (1 / \delta)}{2 m}}
$$



[证明]：将$\beta=\frac{1}{m}$带入至式12.58即得证。



## 定理12.9

若学习算法$\mathcal{L}$是ERM且是稳定的，则假设空间$\mathcal{H}$可学习。

[解析]：首先明确几个概念，ERM表示算法$\mathcal{L}$满足经验风险最小化(Empirical Risk Minimization)，学习算法稳定表示。由于$\mathcal{L}$满足经验误差最小化，则可

令$g$表示假设空间中具有最小泛化损失的假设，即
$$
\ell(g, \mathcal{D})=\min _{h \in \mathcal{H}} \ell(h, \mathcal{D})
$$
再令
$$
\begin{array}{l}{\epsilon^{\prime}=\frac{\epsilon}{2}} \\ {\frac{\delta}{2}=2 \exp \left(-2 m\left(\epsilon^{\prime}\right)^{2}\right)}\end{array}
$$
将$\epsilon^\prime=\frac{\epsilon}{2}$带入到${\frac{\delta}{2}=2 \exp \left(-2 m\left(\epsilon^{\prime}\right)^{2}\right)}$可以解得$m=\frac{2}{\epsilon^{2}} \ln \frac{4}{\delta}$，由Hoeffding不等式12.6，$$P\left(\left\vert\frac{1}{m} \sum_{i=1}^{m} x_{i}-\frac{1}{m} \sum_{i=1}^{m} \mathbb{E}\left(x_{i}\right)\right\vert \geqslant \epsilon\right) \leqslant 2 \exp \left(-2 m \epsilon^{2}\right)$$，其中$\frac{1}{m} \sum_{i=1}^{m} \mathbb{E}\left(x_{i}\right)=\ell(g, \mathcal{D})$，$\frac{1}{m} \sum_{i=1}^{m} x_{i}=\widehat{\ell}(g, \mathcal{D})$，带入可得
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
又因为$m$为关于$\left(1/\epsilon,1/\delta,\text{size}(x),\text{size}(c)\right)$的多项式，因此根据定理12.2，定理12.5，得到结论$\mathcal{H}$是(不可知)PAC可学习的。











