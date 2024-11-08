```{important}
参与组队学习的同学须知：

本章学习时间：3天

本章配套视频教程：

https://www.bilibili.com/video/BV1Mh411e7VU?p=12

https://www.bilibili.com/video/BV1Mh411e7VU?p=13
```

# 第8章 集成学习

集成学习(ensemble
learning)描述的是组合多个基础的学习器（模型）的结果以达到更加鲁棒、效果更好的学习器。在"西瓜书"作者周志华教授的谷歌学术主页的top引用文章（图8-1）中，很大一部分都和集成学习有关。

![图8-1 周志华教授谷歌学术top10引用文章(截止到2023-02-19)](https://datawhale-business.oss-cn-hangzhou.aliyuncs.com/35577be0-f0ae-4875-9251-35eaa0c731b2-zhou_citation.svg)

在引用次数前10的文章中，第1名"Top 10 algorithms in data
mining"是在ICDM'06中投票选出的数据挖掘十大算法，每个提名算法均由业内专家代表去阐述，然后进行投票，其中最终得票排名第7位的"Adaboost"即由周志华教授作为代表进行阐述；第2名"Isolation
forest"是通过集成学习的技术用来做异常检测。第3名的"Ensemble Methods:
Foundations and
Algorithms"则是周志华教授所著的集成学习专著。第6名"Ensembing neural
networks: many could be better than
all"催生了基于优化的集成修剪(ensemble pruning)技术；第7名的"Exploratory
undersampling for class-imbalance
learning"是以集成学习技术解决类别不平衡问题。

毫不夸张的说，周志华教授在集成学习领域深耕了很多年，是绝对的权威。而集成学习也是经受了时间考验的非常有效的算法，常常被各位竞赛同学作为涨点提分的致胜法宝。下面，让我们一起认真享受"西瓜书"作者最拿手的集成学习章节吧。

## 8.1 个体与集成

基学习器(base
learner)的概念在论文中经常出现，可留意一下；另外，本节提到的投票法有两种，除了本节的多数投票(majority
voting)，还有概率投票(probability
voting)，这两点在8.4节中均会提及，即硬投票和软投票。

### 8.1.1 式(8.1)的解释

$h_{i}(\boldsymbol{x})$是编号为$i$的基分类器给$x$的预测标记，$f(\boldsymbol{x})$是$x$的真实标记，它们之间不一致的概率记为$\epsilon$。

### 8.1.2 式(8.2)的解释

注意到当前仅针对二分类问题 $y \in\{-1,+1\}$, 即预测标记
$h_i(\boldsymbol{x}) \in\{-1,+1\}$
。各个基分类器$h_i$的分类结果求和之后结果的正、负或0，代表投票法产生的结果，即"少数服从多数"，符号函数$\operatorname{sign}$，将正数变成1，负数变成-1，0仍然是0，所以$H(\boldsymbol{x})$是由投票法产生的分类结果。

### 8.1.3 式(8.3)的推导

由基分类器相互独立，假设随机变量$X$为$T$个基分类器分类正确的次数，因此随机变量$\mathrm{X}$服从二项分布：$\mathrm{X} \sim \mathcal{B}(\mathrm{T}, 1-\mathrm{\epsilon})$，设$x_i$为每一个分类器分类正确的次数，则$x_i\sim \mathcal{B}(1, 1-\mathrm{\epsilon})（i=1，2，3，...，\mathrm{T}）$，那么有


$$
\begin{aligned}
\mathrm{X}&=\sum_{i=1}^{\mathrm{T}} x_i\\
\mathbb{E}(X)&=\sum_{i=1}^{\mathrm{T}}\mathbb{E}(x_i)=(1-\epsilon)T
\end{aligned}
$$

 证明过程如下：


$$
\begin{aligned} P(H(x) \neq f(x))=& P(X \leq\lfloor T / 2\rfloor) \\ & \leqslant P(X \leq T / 2)
\\ & =P\left[X-(1-\epsilon) T \leqslant \frac{T}{2}-(1-\epsilon) T\right] 
\\ & =P\left[X-
(1-\epsilon) T \leqslant -\frac{T}{2}\left(1-2\epsilon\right)]\right]
\\ &=P\left[\sum_{i=1}^{\mathrm{T}} x_i-
\sum_{i=1}^{\mathrm{T}}\mathbb{E}(x_i) \leqslant -\frac{T}{2}\left(1-2\epsilon\right)]\right]
\\ &=P\left[\frac{1}{\mathrm{T}}\sum_{i=1}^{\mathrm{T}} x_i-\frac{1}{\mathrm{T}}
\sum_{i=1}^{\mathrm{T}}\mathbb{E}(x_i) \leqslant -\frac{1}{2}\left(1-2\epsilon\right)]\right]
 \end{aligned}
$$

 根据Hoeffding不等式知


$$
P\left(\frac{1}{m} \sum_{i=1}^{m} x_{i}-\frac{1}{m} \sum_{i=1}^{m} \mathbb{E}\left(x_{i}\right) \leqslant -\delta\right) \leqslant  \exp \left(-2 m \delta^{2}\right)
$$


令$\delta=\frac {(1-2\epsilon)}{2},m=T$得


$$
\begin{aligned} P(H(\boldsymbol{x}) \neq f(\boldsymbol{x})) &=\sum_{k=0}^{\lfloor T / 2\rfloor} \left( \begin{array}{c}{T} \\ {k}\end{array}\right)(1-\epsilon)^{k} \epsilon^{T-k} \\ & \leqslant \exp \left(-\frac{1}{2} T(1-2 \epsilon)^{2}\right) \end{aligned}
$$



## 8.2 Boosting

注意8.1节最后一段提到：根据个体学习器的生成方式，目前的集成学习方法大致可分为两大类，即个体学习器间存在强依赖关系、必须串行生成的序列化方法，以及个体学习器间不存在强依赖关系、可同时生成的并行化方法。

本节Boosting为前者的代表，Adaboost又是Boosting族算法的代表。

### 8.2.1 式(8.4)的解释

这个式子是集成学习的加性模型，加性模型不采用梯度下降的思想，而是$H(\boldsymbol{x})=\sum_{t=1}^{T-1} \alpha_{t} h_{t}(\boldsymbol{x})+\alpha_{T}h_{T}(\boldsymbol{x})$，共迭代$T$次，每次更新求解一个理论上最优的$h_T$和$\alpha_T$。**（$h_T$和$\alpha_T$的定义参见式(8.18)和式(8.11)）**

### 8.2.2 式(8.5)的解释

先考虑指数损失函数$e^{-f(x) H(x)}$的含义 **（参见"西瓜书"图6.5）**：$f$为真实函数，对于样本$x$来说，$f(\boldsymbol{x}) \in\{+1,-1\}$只能取$+1$和$-1$，而$H(\boldsymbol{x})$是一个实数。

当$H(\boldsymbol{x})$的符号与$f(x)$一致时，$f(\boldsymbol{x}) H(\boldsymbol{x})>0$，因此$e^{-f(\boldsymbol{x}) H(\boldsymbol{x})}=e^{-|H(\boldsymbol{x})|}<1$，且$|H(\boldsymbol{x})|$越大指数损失函数$e^{-f(\boldsymbol{x}) H(\boldsymbol{x})}$越小。这很合理：此时$|H(\boldsymbol{x})|$越大意味着分类器本身对预测结果的信心越大，损失应该越小；若$|H(\boldsymbol{x})|$在零附近，虽然预测正确，但表示分类器本身对预测结果信心很小，损失应该较大；

当$H(\boldsymbol{x})$的符号与$f(\boldsymbol{x})$不一致时，$f(\boldsymbol{x}) H(\boldsymbol{x})<0$，因此$e^{-f(\boldsymbol{x}) H(\boldsymbol{x})}=e^{|H(\boldsymbol{x})|}>1$，且$| H(\boldsymbol{x}) |$越大指数损失函数越大。这很合理：此时$| H(\boldsymbol{x}) |$越大意味着分类器本身对预测结果的信心越大，但预测结果是错的，因此损失应该越大；若$| H(\boldsymbol{x}) |$在零附近，虽然预测错误，但表示分类器本身对预测结果信心很小，虽然错了，损失应该较小。

再解释符号$\mathbb{E}_{\boldsymbol{x} \sim \mathcal{D}}[\cdot]$的含义：$\mathcal{D}$为概率分布，可简单理解为在数据集$D$中进行一次随机抽样，每个样本被取到的概率；$\mathbb{E}[\cdot]$为经典的期望，则综合起来$\mathbb{E}_{\boldsymbol{x} \sim \mathcal{D}}[\cdot]$表示在概率分布$\mathcal{D}$上的期望，可简单理解为对数据集$D$以概率$\mathcal{D}$进行加权后的期望。

综上所述, 若数据集 $D$ 中样本 $\boldsymbol{x}$ 的权值分布为
$\mathcal{D}(\boldsymbol{x})$, 则式(8.5)可写为: 

$$
\begin{aligned}
\ell_{\exp }(H \mid \mathcal{D}) & =\mathbb{E}_{\boldsymbol{x} \sim \mathcal{D}}\left[e^{-f(\boldsymbol{x}) H(\boldsymbol{x})}\right] \\
& =\sum_{\boldsymbol{x} \in D} \mathcal{D}(\boldsymbol{x}) e^{-f(\boldsymbol{x}) H(\boldsymbol{x})} \\
& =\sum_{\boldsymbol{x} \in D} \mathcal{D}(\boldsymbol{x})\left(e^{-H(\boldsymbol{x})} \mathbb{I}(f(\boldsymbol{x})=1)+e^{H(\boldsymbol{x})} \mathbb{I}(f(\boldsymbol{x})=-1)\right)
\end{aligned}
$$



特别地, 若针对任意样本 $\boldsymbol{x}$, 若分布
$\mathcal{D}(\boldsymbol{x})=\frac{1}{|D|}$, 其中 $|D|$ 为数据集 $D$
样本个数, 则


$$
\ell_{\exp }(H \mid \mathcal{D})=\mathbb{E}_{\boldsymbol{x} \sim \mathcal{D}}\left[e^{-f(\boldsymbol{x}) H(\boldsymbol{x})}\right]=\frac{1}{|D|} \sum_{\boldsymbol{x} \in D} e^{-f(\boldsymbol{x}) H(\boldsymbol{x})}
$$



而这就是在求传统平均值。

### 8.2.3 式(8.6)的推导

由式(8.5)中对于符号$\mathbb{E}_{\boldsymbol{x} \sim \mathcal{D}}[\cdot]$的解释可知


$$
\begin{aligned}
\ell_{\exp }(H | \mathcal{D}) &=\mathbb{E}_{\boldsymbol{x} \sim \mathcal{D}}\left[e^{-f(\boldsymbol{x}) H(\boldsymbol{x})}\right] \\
&=\sum_{\boldsymbol{x} \in D} \mathcal{D}(\boldsymbol{x}) e^{-f(\boldsymbol{x}) H(\boldsymbol{x})} \\
&=\sum_{i=1}^{|D|} \mathcal{D}\left(\boldsymbol{x}_{i}\right)\left(e^{-H\left(\boldsymbol{x}_{i}\right)} \mathbb{I}\left(f\left(\boldsymbol{x}_{i}\right)=1\right)+e^{H\left(\boldsymbol{x}_{i}\right)} \mathbb{I}\left(f\left(\boldsymbol{x}_{i}\right)=-1\right)\right)\\
&=\sum_{i=1}^{|D|} \left(e^{-H\left(\boldsymbol{x}_{i}\right)} \mathcal{D}\left(\boldsymbol{x}_{i}\right)\mathbb{I}\left(f\left(\boldsymbol{x}_{i}\right)=1\right)+e^{H\left(\boldsymbol{x}_{i}\right)} \mathcal{D}\left(\boldsymbol{x}_{i}\right)\mathbb{I}\left(f\left(\boldsymbol{x}_{i}\right)=-1\right)\right)\\
&=\sum_{i=1}^{|D|} \left(e^{-H\left(\boldsymbol{x}_{i}\right)} P\left(f\left(\boldsymbol{x}_{i}\right)=1 \mid \boldsymbol{x}_{i}\right)+e^{H\left(\boldsymbol{x}_{i}\right)} P\left(f\left(\boldsymbol{x}_{i}\right)=-1 \mid \boldsymbol{x}_{i}\right)\right)
\end{aligned}
$$


其中$\mathcal{D}\left(\boldsymbol{x}_{i}\right)\mathbb{I}\left(f\left(\boldsymbol{x}_{i}\right)=1\right)=P\left(f\left(\boldsymbol{x}_{i}\right)=1 \mid \boldsymbol{x}_{i}\right)$可以这样理解：
$\mathcal{D}(x_i)$表示在数据集$D$中进行一次随机抽样，样本$x_i$被取到的概率，$\mathcal{D}\left(\boldsymbol{x}_{i}\right)\mathbb{I}\left(f\left(\boldsymbol{x}_{i}\right)=1\right)$表示在数据集$D$中进行一次随机抽样，使得$f(x_i)=1$的样本$x_i$被抽到的概率，即为$P\left(f\left(\boldsymbol{x}_{i}\right)=1 \mid \boldsymbol{x}_{i}\right)$。

当对$H(x_i)$求导时，求和号中只有含$x_i$项不为0，由求导公式


$$
\frac{\partial e^{-H(\boldsymbol{x})}}{\partial H(\boldsymbol{x})}=-e^{-H(\boldsymbol{x})}\qquad \frac{\partial e^{H(\boldsymbol{x})}}{\partial H(\boldsymbol{x})}=e^{H(\boldsymbol{x})}
$$


有


$$
\frac{\partial \ell_{\exp }(H | \mathcal{D})}{\partial H(\boldsymbol{x})}=-e^{-H(\boldsymbol{x})} P(f(\boldsymbol{x})=1 | \boldsymbol{x})+e^{H(\boldsymbol{x})} P(f(\boldsymbol{x})=-1 | \boldsymbol{x})
$$



### 8.2.4 式(8.7)的推导

令式(8.6)等于零:


$$
\quad-e^{-H(\boldsymbol{x})} P(f(\boldsymbol{x})=1 \mid \boldsymbol{x})+e^{H(\boldsymbol{x})} P(f(\boldsymbol{x})=-1 \mid \boldsymbol{x})=0
$$


移项:


$$
\quad e^{H(\boldsymbol{x})} P(f(\boldsymbol{x})=-1 \mid \boldsymbol{x})=e^{-H(\boldsymbol{x})} P(f(\boldsymbol{x})=1 \mid \boldsymbol{x})
$$


两边同乘
$\frac{e^{H(\boldsymbol{x})}}{P(f(\boldsymbol{x})=-1 \mid \boldsymbol{x})}$:


$$
\quad e^{2 H(\boldsymbol{x})}=\frac{P(f(\boldsymbol{x})=1 \mid \boldsymbol{x})}{P(f(\boldsymbol{x})=-1 \mid \boldsymbol{x})}
$$


取 $\ln (\cdot)$:


$$
\quad 2 H(\boldsymbol{x})=\ln \frac{P(f(\boldsymbol{x})=1 \mid \boldsymbol{x})}{P(f(\boldsymbol{x})=-1 \mid \boldsymbol{x})}
$$


两边同乘 $\frac{1}{2}$ 即得式(8.7)。

### 8.2.5 式(8.8)的推导



$$
\begin{aligned}
\operatorname{sign}(H(\boldsymbol{x}))&=\operatorname{sign}\left(\frac{1}{2} \ln \frac{P(f(x)=1 | \boldsymbol{x})}{P(f(x)=-1 | \boldsymbol{x})}\right)
\\ & =\left\{\begin{array}{ll}{1,} & {P(f(x)=1 | \boldsymbol{x})>P(f(x)=-1 | \boldsymbol{x})} \\ {-1,} & {P(f(x)=1 | \boldsymbol{x})<P(f(x)=-1 | \boldsymbol{x})}\end{array}\right.
\\ & =\underset{y \in\{-1,1\}}{\arg \max } P(f(x)=y | \boldsymbol{x})
\end{aligned}
$$


第一行到第二行显然成立，第二行到第三行是利用了$\arg\max$函数的定义。$\underset{y \in\{-1,1\}}{\arg \max } P(f(x)=y | \boldsymbol{x})$表示使得函数$P(f(x)=y | \boldsymbol{x}$)取得最大值的$y$的值，展开刚好是第二行的式子。

这里解释一下贝叶斯错误率的概念。这来源于"西瓜书"P148的式(7.6)表示的贝叶斯最优分类器，可以发现式(8.8)的最终结果是式(7.6)的二分类特殊形式。

到此为止，本节证明了指数损失函数是分类任务原本0/1损失函数的一致的替代损失函数，而指数损失函数有更好的数学性质，例如它是连续可微函数，因此接下来的式(8.9)至式(8.19)基于指数损失函数推导AdaBoost的理论细节。**（替代损失函数参见"西瓜书"P131图6.5）**

### 8.2.6 式(8.9)的推导



$$
\begin{aligned}
\ell_{\exp }\left(\alpha_{t} h_{t} | \mathcal{D}_{t}\right) &=\mathbb{E}_{\boldsymbol{x} \sim \mathcal{D}_{t}}\left[e^{-f(\boldsymbol{x}) \alpha_{t} h_{t}(\boldsymbol{x})}\right] &\textcircled{1}\\
&=\mathbb{E}_{\boldsymbol{x} \sim \mathcal{D}_{t}}\left[e^{-\alpha_{t}} \mathbb{I}\left(f(\boldsymbol{x})=h_{t}(\boldsymbol{x})\right)+e^{\alpha_{t}} \mathbb{I}\left(f(\boldsymbol{x}) \neq h_{t}(\boldsymbol{x})\right)\right] &\textcircled{2}\\
&=e^{-\alpha_{t}} P_{\boldsymbol{x} \sim \mathcal{D}_{t}}\left(f(\boldsymbol{x})=h_{t}(\boldsymbol{x})\right)+e^{\alpha_{t}} P_{\boldsymbol{x} \sim \mathcal{D}_{t}}\left(f(\boldsymbol{x}) \neq h_{t}(\boldsymbol{x})\right) &\textcircled{3}\\
&=e^{-\alpha_{t}}\left(1-\epsilon_{t}\right)+e^{\alpha_{t}} \epsilon_{t} &\textcircled{4}
\end{aligned}
$$



乍一看本式有些问题, 为什么要最小化
$\ell_{\exp }\left(\alpha_t h_t \mid \mathcal{D}_t\right)$ ?
"西瓜书"图8.3中的第 3 行的 表达式
$h_t=\mathfrak{L}\left(D, \mathcal{D}_t\right)$ 不是代表着应该最小化
$\ell_{\exp }\left(h_t \mid \mathcal{D}_t\right)$ 么? 或者从整体来看, 第
$t$ 轮迭代 也应该最小化
$\ell_{\exp }\left(H_t \mid \mathcal{D}\right)=\ell_{\exp }\left(H_{t-1}+\alpha_t h_t \mid \mathcal{D}\right)$,
这样最终 $T$ 轮迭代结束后得到的式 (8.4)就可以最小化
$\ell_{\exp }(H \mid \mathcal{D})$ 了。实际上, 理解了 AdaBoost
之后就会发现, $\ell_{\exp }\left(\alpha_t h_t \mid \mathcal{D}_t\right)$
与 $\ell_{\exp }\left(H_t \mid \mathcal{D}\right)$ 是等价的, 详见后面的
"AdaBoost 的个人推导"。另外,
$h_t=\mathfrak{L}\left(D, \mathcal{D}_t\right)$ 也 是推导的结论之一,
即式(8.18), 而不是无缘无故靠直觉用
$\mathfrak{L}\left(D, \mathcal{D}_t\right)$ 得到 $h_t$ 。

暂且不管以上疑问, 权且按作者思路推导一下：

$\textcircled{1}$与式(8.5)的区别仅在于到底针对
$\alpha_t h_t(\boldsymbol{x})$ 还是 $H(\boldsymbol{x})$, 代入即可;

$\textcircled{2}$是考虑到 $h_t(\boldsymbol{x})$ 和 $f(\boldsymbol{x})$
均只能取 $-1$ 和 $+1$ 两个值, 其中 $\mathbb{I}(\cdot)$ 为指示函数;

$\textcircled{3}$对中括号的两项分别求
$\mathbb{E}_{\boldsymbol{x} \sim \mathcal{D}_t}[\cdot]$, 而
$e^{\alpha_t}$ 和 $e^{-\alpha_t}$ 与 $\boldsymbol{x}$ 无关,
可以作为常数项拿到 $\mathbb{E}_{\boldsymbol{x} \sim \mathcal{D}_t}[$.$]$
外面, 而
$\mathbb{E}_{\boldsymbol{x}\sim\mathcal{D}_t}\left[\mathbb{I}\left(f(\boldsymbol{x})=h_t(\boldsymbol{x})\right)\right]$
表示在数据集 $D$ 上、样本权值分布为 $\mathcal{D}_t$ 时
$f(\boldsymbol{x})$ 和 $h_t(\boldsymbol{x})$ 相等次数的期望, 即
$P_{\boldsymbol{x} \sim \mathcal{D}_t}\left(f(\boldsymbol{x})=h_t(\boldsymbol{x})\right)$,
也就是正确率, 即 $\left(1-\epsilon_t\right)$; 同理,
$\mathbb{E}_{\boldsymbol{x} \sim \mathcal{D}_t}\left[\mathbb{I}\left(f(\boldsymbol{x}) \neq h_t(\boldsymbol{x})\right)\right]$
表示在数据集 $D$ 上、样本权值分布为 $\mathcal{D}_t$ 时
$f(\boldsymbol{x})$ 和 $h_t(\boldsymbol{x})$ 不相等次 数的期望, 即
$P_{\boldsymbol{x} \sim \mathcal{D}_t}\left(f(\boldsymbol{x}) \neq h_t(\boldsymbol{x})\right)$,
也就是错误率 $\epsilon_t$;

$\textcircled{4}$即为将
$P_{\boldsymbol{x} \sim \mathcal{D}_t}\left(f(\boldsymbol{x})=h_t(\boldsymbol{x})\right)$
替换为 $\left(1-\epsilon_t\right)$ 、将
$P_{\boldsymbol{x} \sim \mathcal{D}_t}\left(f(\boldsymbol{x}) \neq h_t(\boldsymbol{x})\right)$
替 换为 $\epsilon_t$ 的结果。

注意本节符号略有混乱, 如前所述式(8.4)的 $H(\boldsymbol{x})$
是连续实值函数, 但在"西瓜书"图8.3最后 一行的输出 $H(\boldsymbol{x})$
明显只能取 $-1$ 和 $+1$ 两个值 (与式(8.2)相同),
本节除了"西瓜书"图8.3最后一行的 输出之外, $H(\boldsymbol{x})$
均以式(8.4)的连续实值函数为准。

### 8.2.7 式(8.10)的解释

指数损失函数对$\alpha_t$求偏导，为了得到使得损失函数取最小值时$\alpha_t$的值。

### 8.2.8 式(8.11)的推导

令公式(8.10)等于0移项即得到的该式。此时$\alpha_t$的取值使得该基分类器经$\alpha_t$加权后的损失函数最小。

### 8.2.9 式(8.12)的解释

本式的推导和原始论文[1]的推导略有差异，虽然并不影响后面式(8.18)以及式(8.19)的推导结果。AdaBoost
第 $t$ 轮 迭代应该求解如下优化问题从而得到 $\alpha_t$ 和
$h_t(\boldsymbol{x})$ :


$$
\left(\alpha_t, h_t(\boldsymbol{x})\right)=\underset{\alpha, h}{\arg \min } \ell_{\exp }\left(H_{t-1}+\alpha h \mid \mathcal{D}\right)
$$


对于该问题, 先对于固定的任意 $\alpha>0$, 求解 $h_t(\boldsymbol{x})$;
得到 $h_t(\boldsymbol{x})$ 后再求 $\alpha_{t^{\circ}}$。

在原始论文的第346页，对式(8.12)的推导如图8-2所示，可以发现原文献中保留了参数 $c$
(即 $\alpha$ )。当然, 对于任意 $\alpha>0$, 并不影响推导结果。

![图8-2 原始论文对式(8.12)的相关推导](https://datawhale-business.oss-cn-hangzhou.aliyuncs.com/c43a3a78-5e8c-4c90-bc61-00aa0a0f55b1-adaboost_proof_1.svg)

如果暂且不管以上的差异，我们按照作者的思路推导的话，将$H_{t}(\boldsymbol{x})=H_{t-1}(\boldsymbol{x})+h_{t}(\boldsymbol{x})$带入公式(8.5)即可，因为理想的$h_t$可以纠正$H_{t-1}$的全部错误，所以这里指定$h_t$其权重系数$\alpha_t$为1。如果权重系数$\alpha_t$是个常数的话，对后续结果也没有影响。

### 8.2.10 式(8.13)的推导

由$e^x$的二阶泰勒展开为$1+x+\frac{x^2}{2}+o(x^2)$得: 

$$
\begin{aligned}
\ell_{\exp }\left(H_{t-1}+h_{t} | \mathcal{D}\right) &=\mathbb{E}_{\boldsymbol{x} \sim \mathcal{D}}\left[e^{-f(\boldsymbol{x}) H_{t-1}(\boldsymbol{x})} e^{-f(\boldsymbol{x}) h_{t}(\boldsymbol{x})}\right]
\\ & \simeq \mathbb{E}_{\boldsymbol{x} \sim \mathcal{D}}\left[e^{-f(\boldsymbol{x}) H_{t-1}(\boldsymbol{x})}\left(1-f(\boldsymbol{x}) h_{t}(\boldsymbol{x})+\frac{f^{2}(\boldsymbol{x}) h_{t}^{2}(\boldsymbol{x})}{2}\right)\right]
\end{aligned}
$$


因为$f(\boldsymbol{x})$与$h_t(\boldsymbol{x})$取值都为1或-1，所以$f^2(\boldsymbol{x})=h_t^2(\boldsymbol{x})=1$，所以得:


$$
\ell_{\exp }\left(H_{t-1}+h_{t} | \mathcal{D}\right)= \mathbb{E}_{\boldsymbol{x} \sim \mathcal{D}}\left[e^{-f(\boldsymbol{x}) H_{t-1}(\boldsymbol{x})}\left(1-f(\boldsymbol{x}) h_{t}(\boldsymbol{x})+\frac{1}{2}\right)\right]
$$



实际上，此处保留一阶泰勒展开项即可，后面提到的Gradient
Boosting理论框架就是只使用了一阶泰勒展开；当然二阶项为常数，也并不影响推导结果，原文献[1]中也保留了二阶项。

### 8.2.11 式(8.14)的推导



$$
\begin{aligned}
h_{t}(\boldsymbol{x})&=\underset{h}{\arg \min } \ell_{\exp }\left(H_{t-1}+h | \mathcal{D}\right)&\textcircled{1}\\
&=\underset{h}{\arg \min } \mathbb{E}_{\boldsymbol{x} \sim \mathcal{D}}\left[e^{-f(\boldsymbol{x}) H_{t-1}(\boldsymbol{x})}\left(1-f(\boldsymbol{x}) h(\boldsymbol{x})+\frac{1}{2}\right)\right]&\textcircled{2}\\
&=\underset{h}{\arg \max } \mathbb{E}_{\boldsymbol{x} \sim \mathcal{D}}\left[e^{-f(\boldsymbol{x}) H_{t-1}(\boldsymbol{x})} f(\boldsymbol{x}) h(\boldsymbol{x})\right]&\textcircled{3}\\
&=\underset{h}{\arg \max } \mathbb{E}_{\boldsymbol{x} \sim \mathcal{D}}\left[\frac{e^{-f(\boldsymbol{x}) H_{t-1}(\boldsymbol{x})}}{\mathbb{E}_{\boldsymbol{x} \sim \mathcal{D}}\left[e^{-f(\boldsymbol{x}) H_{t-1}(\boldsymbol{x})}\right]} f(\boldsymbol{x}) h(\boldsymbol{x})\right]&\textcircled{4}
\end{aligned}
$$


理想的$h_t(\boldsymbol{x})$是使得$H_{t}(\boldsymbol{x})$的指数损失函数取得最小值时的$h_t(\boldsymbol{x})$，该式将此转化成某个期望的最大值，其中：

$\textcircled{2}$是将式(8.13)代入；

$\textcircled{3}$是因为 

$$
\begin{aligned}
& \mathbb{E}_{\boldsymbol{x} \sim \mathcal{D}}\left[e^{-f(\boldsymbol{x}) H_{t-1}(\boldsymbol{x})}\left(1-f(\boldsymbol{x}) h(\boldsymbol{x})+\frac{1}{2}\right)\right] \\
= & \mathbb{E}_{\boldsymbol{x} \sim \mathcal{D}}\left[\frac{3}{2} e^{-f(\boldsymbol{x}) H_{t-1}(\boldsymbol{x})}-e^{-f(\boldsymbol{x}) H_{t-1}(\boldsymbol{x})} f(\boldsymbol{x}) h(\boldsymbol{x})\right] \\
= & \mathbb{E}_{\boldsymbol{x} \sim \mathcal{D}}\left[\frac{3}{2} e^{-f(\boldsymbol{x}) H_{t-1}(\boldsymbol{x})}\right]-\mathbb{E}_{\boldsymbol{x} \sim \mathcal{D}}\left[e^{-f(\boldsymbol{x}) H_{t-1}(\boldsymbol{x})} f(\boldsymbol{x}) h(\boldsymbol{x})\right]
\end{aligned}
$$

 本式自变量为 $h(\boldsymbol{x})$, 而
$\mathbb{E}_{\boldsymbol{x} \sim \mathcal{D}}\left[\frac{3}{2} e^{-f(\boldsymbol{x}) H_{t-1}(\boldsymbol{x})}\right]$
与 $h(\boldsymbol{x})$ 无关, 也就是一个常数，因此只需最小大化第二项


$$
-\mathbb{E}_{\boldsymbol{x} \sim \mathcal{D}}\left[e^{-f(\boldsymbol{x}) H_{t-1}(\boldsymbol{x})} f(\boldsymbol{x}) h(\boldsymbol{x})\right]
$$


将负号去掉, 原最小化问题变为最大化问题；

$\textcircled{4}$是因为
$\mathbb{E}_{\boldsymbol{x} \sim \mathcal{D}}\left[e^{-f(\boldsymbol{x}) H_{t-1}(\boldsymbol{x})}\right]$
是与自变量 $h(\boldsymbol{x})$ 无关的正常数 (因为指数函
数与原问题等价，例如 $\arg \max _x\left(1-x^2\right)$ 与
$\arg \max _x 2\left(1-x^2\right)$ 的结果均为 $\left.x=0\right)$。

### 8.2.12 式(8.16)的推导

首先解释下符号$\mathbb{E}_{\boldsymbol{x} \sim \mathcal{D}}$的含义，注意在本章中有两个符号$D$和$\mathcal{D}$，其中$D$表示数据集，而$\mathcal{D}$表示数据集$D$的样本分布，可以理解为在数据集$D$上进行一次随机采样，样本$x$被抽到的概率是$\mathcal{D}(x)$，那么符号$\mathbb{E}_{\boldsymbol{x} \sim \mathcal{D}}$表示的是在概率分布$\mathcal{D}$上的期望，可以简单地理解为对数据及$D$以概率$\mathcal{D}$加权之后的期望，因此有：


$$
\mathbb{E}(g(\boldsymbol{x}))=\sum_{i=1}^{|D|}f(\boldsymbol{x}_i)g(\boldsymbol{x}_i)
$$


故可得


$$
\mathbb{E}_{\boldsymbol{x} \sim \mathcal{D}}\left[e^{-f(\boldsymbol{x}) H(\boldsymbol{x})}\right]=\sum_{i=1}^{|D|} \mathcal{D}\left(\boldsymbol{x}_{i}\right) e^{-f\left(\boldsymbol{x}_{i}\right) H\left(\boldsymbol{x}_{i}\right)}
$$


由式(8.15)可知


$$
\mathcal{D}_{t}\left(\boldsymbol{x}_{i}\right)=\mathcal{D}\left(\boldsymbol{x}_{i}\right) \frac{e^{-f\left(\boldsymbol{x}_{i}\right) H_{t-1}\left(\boldsymbol{x}_{i}\right)}}{\mathbb{E}_{\boldsymbol{x} \sim \mathcal{D}}\left[e^{-f(\boldsymbol{x}) H_{t-1}(\boldsymbol{x})}\right]}
$$


所以式(8.16)可以表示为


$$
\begin{aligned} & \mathbb{E}_{\boldsymbol{x} \sim \mathcal{D}}\left[\frac{e^{-f(\boldsymbol{x}) H_{t-1}(\boldsymbol{x})}}{\mathbb{E}_{\boldsymbol{x} \sim \mathcal{D}}\left[e^{-f(\boldsymbol{x}) H_{t-1}(\boldsymbol{x})}\right]} f(\boldsymbol{x}) h(\boldsymbol{x})\right] \\=& \sum_{i=1}^{|D|} \mathcal{D}\left(\boldsymbol{x}_{i}\right) \frac{e^{-f\left(\boldsymbol{x}_{i}\right) H_{t-1}\left(\boldsymbol{x}_{i}\right)}}{\mathbb{E}_{\boldsymbol{x} \sim \mathcal{D}}\left[e^{-f(\boldsymbol{x}) H_{t-1}(\boldsymbol{x}) }]  \right.}f(x_i)h(x_i) \\=& \sum_{i=1}^{|D|} \mathcal{D}_{t}\left(\boldsymbol{x}_{i}\right) f\left(\boldsymbol{x}_{i}\right) h\left(\boldsymbol{x}_{i}\right) \\=& \mathbb{E}_{\boldsymbol{x} \sim \mathcal{D}_{t}}[f(\boldsymbol{x}) h(\boldsymbol{x})] \end{aligned}
$$



### 8.2.13 式(8.17)的推导

当$f(\boldsymbol{x})=h(\boldsymbol{x})$时，$\mathbb{I}(f(\boldsymbol{x}) \neq h(\boldsymbol{x}))=0$，$f(\boldsymbol{x}) h(\boldsymbol{x})=1$，$1-2\mathbb{I}(f(\boldsymbol{x}) \neq h(\boldsymbol{x}))=1$；

当$f(\boldsymbol{x})\neq h(\boldsymbol{x})$时，$\mathbb{I}(f(\boldsymbol{x}) \neq h(\boldsymbol{x}))=1$，$f(\boldsymbol{x}) h(\boldsymbol{x})=-1$，$1-2\mathbb{I}(f(\boldsymbol{x}) \neq h(\boldsymbol{x}))=-1$。

综上，左右两式相等。

### 8.2.14 式(8.18)的推导

本式基于式(8.17)的恒等关系，由式(8.16)推导而来。


$$
\begin{aligned} \mathbb{E}_{\boldsymbol{x} \sim \mathcal{D}_t}[f(\boldsymbol{x}) h(\boldsymbol{x})] & =\mathbb{E}_{\boldsymbol{x} \sim \mathcal{D}_t}[1-2 \mathbb{I}(f(\boldsymbol{x}) \neq h(\boldsymbol{x}))] \\ & =\mathbb{E}_{\boldsymbol{x} \sim \mathcal{D}_t}[1]-2 \mathbb{E}_{\boldsymbol{x} \sim \mathcal{D}_t}[\mathbb{I}(f(\boldsymbol{x}) \neq h(\boldsymbol{x}))] \\ & =1-2 \mathbb{E}_{\boldsymbol{x} \sim \mathcal{D}_t}[\mathbb{I}(f(\boldsymbol{x}) \neq h(\boldsymbol{x}))]\end{aligned}
$$



类似于式(8.14)的第3个和第4个等号，由式(8.16)的结果开始推导：


$$
\begin{aligned}
h_{t}(\boldsymbol{x}) &=\arg \max _{h} \mathbb{E}_{\boldsymbol{x} \sim \mathcal{D}_{t}}[f(\boldsymbol{x}) h(\boldsymbol{x})] \\
&=\arg \max _{h}\left(1-2 \mathbb{E}_{\boldsymbol{x} \sim \mathcal{D}_{t}}[\mathbb{I}(f(\boldsymbol{x}) \neq h(\boldsymbol{x}))]\right) \\
&=\underset{h}{\arg \max }\left(-2 \mathbb{E}_{\boldsymbol{x} \sim \mathcal{D}_{t}}[\mathbb{I}(f(\boldsymbol{x}) \neq h(\boldsymbol{x}))]\right) \\
&=\arg \min \mathbb{E}_{\boldsymbol{x} \sim \mathcal{D}_{t}}[\mathbb{I}(f(\boldsymbol{x}) \neq h(\boldsymbol{x}))]
\end{aligned}
$$



此式表示理想的 $h_t(\boldsymbol{x})$ 在分布 $\mathcal{D}_t$
下最小化分类误差, 因此有"西瓜书"图 8.3第 3 行
$h_t(\boldsymbol{x})=\mathfrak{L}\left(D, \mathcal{D}_t\right)$,
即分类器 $h_t(\boldsymbol{x})$ 可以基于分布 $\mathcal{D}_t$ 从数据集 $D$
中训练而得, 而我们在训练分类器时, 一般来说最小化的损失函数就是分类误差。

### 8.2.15 式(8.19)的推导



$$
\begin{aligned}
\mathcal{D}_{t+1}(\boldsymbol{x}) &=\frac{\mathcal{D}(\boldsymbol{x}) e^{-f(\boldsymbol{x}) H_{t}(\boldsymbol{x})}}{\mathbb{E}_{\boldsymbol{x} \sim \mathcal{D}}\left[e^{-f(\boldsymbol{x}) H_{t}(\boldsymbol{x})}\right]} \\
&=\frac{\mathcal{D}(\boldsymbol{x}) e^{-f(\boldsymbol{x}) H_{t-1}(\boldsymbol{x})} e^{-f(\boldsymbol{x}) \alpha_{t} h_{t}(\boldsymbol{x})}}{\mathbb{E}_{\boldsymbol{x} \sim \mathcal{D}}\left[e^{-f(\boldsymbol{x}) H_{t}(\boldsymbol{x})}\right]} \\
&=\mathcal{D}_{t}(\boldsymbol{x}) \cdot e^{-f(\boldsymbol{x}) \alpha_{t} h_{t}(\boldsymbol{x})} \frac{\mathbb{E}_{\boldsymbol{x} \sim \mathcal{D}}\left[e^{-f(\boldsymbol{x}) H_{t-1}(\boldsymbol{x})}\right]}{\mathbb{E}_{\boldsymbol{x} \sim \mathcal{D}}\left[e^{-f(\boldsymbol{x}) H_{t}(\boldsymbol{x})}\right]}
\end{aligned}
$$



第 1 个等号是将式(8.15)中的 $t$ 换为 $t+1$ (同时 $t-1$ 换为 $t)$;

第 2 个等号是将
$H_t(\boldsymbol{x})=H_{t-1}(\boldsymbol{x})+\alpha_t h_t(\boldsymbol{x})$
代入分子即可;

第 3 个等号是乘以
$\frac{\mathbb{E}_{\boldsymbol{x} \sim \mathcal{D}}\left[e^{-f(\boldsymbol{x}) H_{t-1}(\boldsymbol{x})}\right]}{\mathbb{E}_{\boldsymbol{x} \sim \mathcal{D}}\left[e^{-f(\boldsymbol{x}) H_{t-1}(\boldsymbol{x})}\right]}$
后, 凑出式(8.15)的 $\mathcal{D}_t(\boldsymbol{x})$ 表达式, 以符号
$\mathcal{D}_t(\boldsymbol{x})$ 替 换即得。到此之后, 得到
$\mathcal{D}_{t+1}(\boldsymbol{x})$ 与 $\mathcal{D}_t(\boldsymbol{x})$
的关系, 但为了确保 $\mathcal{D}_{t+1}(\boldsymbol{x})$ 是一个分布, 需要
对得到的 $\mathcal{D}_{t+1}(\boldsymbol{x})$ 进行规范化,
即"西瓜书"图8.3第 7 行的 $Z_t$ 。式(8.19)第 3 行最后一个分式将在规
范化过程被吸收。

boosting算法是根据调整后的样本再去训练下一个基分类器，这就是"重赋权法"的样本分布的调整公式。

### 8.2.16 AdaBoost的个人推导

西瓜书中对AdaBoost的推导和原论文[1]上有些地方有差异，综合原论文和一些参考资料，这里给出一版更易于理解的推导，亦可参见我们的视频教程。

AdaBoost 的目标是学得 $T$ 个 $h_t(\boldsymbol{x})$ 和相应的 $T$ 个
$\alpha_t$, 得到式(8.4)的 $H(\boldsymbol{x})$, 使式(8.5)指数 损失函数
$\ell_{\exp }(H \mid \mathcal{D})$ 最小, 这就是求解所谓的
"加性模型"。特别强调一下, 分类器 $h_t(\boldsymbol{x})$
如何得到及其相应的权重 $\alpha_t$ 等于多少都是需要求解的
$\left(h_t(\boldsymbol{x})=\mathfrak{L}\left(D, \mathcal{D}_t\right)\right.$,
即基于分布 $\mathcal{D}_t$ 从数据集 $D$ 中经过最小化训练误差训练出分类器
$h_t$, 也就是式(8.18), $\alpha_t$ 参见式(8.11)。

"通常这是一个复杂的优化问题（同时学得 $T$ 个 $h_t(\boldsymbol{x})$
和相应的 $T$ 个 $\alpha_t$ 很困难)。前向分
步算法求解这一优化问题的想法是：因为学习的是加法模型, 如果能够从前向后,
每一步只 学习一个基函数 $h_t(\boldsymbol{x})$ 及其系数 $\alpha_t$,
逐步逼近最小化指数损失函数 $\ell_{\exp }(H \mid \mathcal{D})$,
那么就可以简化优化的复杂度。" **（摘自李航 《统计学习方法》[2] 第 144
页，略有改动）**

因此, AdaBoost 每轮迭代只需要得到一个基分类器和其投票权重, 设第 $t$
轮迭代需得到 基分类器 $h_t(\boldsymbol{x})$, 对应的投票权重为
$\alpha_t$, 则集成分类器
$H_t(\boldsymbol{x})=H_{t-1}(\boldsymbol{x})+\alpha_t h_t(\boldsymbol{x})$,
其中 $H_0(\boldsymbol{x})=0$ 。为表达式简洁, 常常将
$h_t(\boldsymbol{x})$ 简写为 $h_t, H_t(\boldsymbol{x})$ 简写为 $H_t$
。则第 $t$
轮实际为如下优化问题（本节式(8.4)到式(8.8)已经证明了指数损失函数是分类任务原本
$0 / 1$ 损失函数的 一致替代损失函数）:


$$
\left(\alpha_t, h_t\right)=\underset{\alpha, h}{\arg \min } \ell_{\exp }\left(H_{t-1}+\alpha h \mid \mathcal{D}\right)
$$


表示每轮得到的基分类器 $h_t(\boldsymbol{x})$ 和对应的权重 $\alpha_t$
是最小化集成分类器 $H_t=H_{t-1}+\alpha_t h_t$ 在 数据集 $D$
上、样本权值分布为 $\mathcal{D}$ (即初始化样本权值分布, 也就是
$\mathcal{D}_1$ ) 时的指数损失函数
$\ell_{\exp }\left(H_{t-1}+\alpha h \mid \mathcal{D}\right)$
的结果。这就是前向分步算法求解加性模型的思路。
根据式(8.5)将指数损失函数表达式代入, 则 

$$
\begin{aligned}
\ell_{\exp }\left(H_{t-1}+\alpha h \mid \mathcal{D}\right) & =\mathbb{E}_{\boldsymbol{x} \sim \mathcal{D}}\left[e^{-f(\boldsymbol{x})\left(H_{t-1}(\boldsymbol{x})+\alpha h(\boldsymbol{x})\right)}\right] \\
& =\sum_{i=1}^{|D|} \mathcal{D}\left(\boldsymbol{x}_i\right) e^{-f\left(\boldsymbol{x}_i\right)\left(H_{t-1}\left(\boldsymbol{x}_i\right)+\alpha h\left(\boldsymbol{x}_i\right)\right)} \\
& =\sum_{i=1}^{|D|} \mathcal{D}\left(\boldsymbol{x}_i\right) e^{-f\left(\boldsymbol{x}_i\right) H_{t-1}\left(\boldsymbol{x}_i\right)} e^{-f\left(\boldsymbol{x}_i\right) \alpha h\left(\boldsymbol{x}_i\right)} \\
& =\sum_{i=1}^{|D|} \mathcal{D}\left(\boldsymbol{x}_i\right) e^{-f\left(\boldsymbol{x}_i\right) H_{t-1}\left(\boldsymbol{x}_i\right)}\left(e^{-\alpha} \mathbb{I}\left(f\left(\boldsymbol{x}_i\right)=h\left(\boldsymbol{x}_i\right)\right)+e^\alpha \mathbb{I}\left(f\left(\boldsymbol{x}_i\right) \neq h\left(\boldsymbol{x}_i\right)\right)\right)
\end{aligned}
$$

 上式推导中, 由于 $f\left(\boldsymbol{x}_i\right)$ 和
$h\left(\boldsymbol{x}_i\right)$ 均只能取 $-1,+1$ 两个值, 因此当
$f\left(\boldsymbol{x}_i\right)=h\left(\boldsymbol{x}_i\right)$ 时,
$f\left(\boldsymbol{x}_i\right) h\left(\boldsymbol{x}_i\right)=1$, 当
$f\left(\boldsymbol{x}_i\right) \neq h\left(\boldsymbol{x}_i\right)$ 时,
$f\left(\boldsymbol{x}_i\right) h\left(\boldsymbol{x}_i\right)=-1$
。另外, $f\left(\boldsymbol{x}_i\right)$ 和
$h\left(\boldsymbol{x}_i\right)$ 要么相 等, 要么不相等,
二者只能有一个为真, 因此以下等式恒成立:


$$
\mathbb{I}\left(f\left(\boldsymbol{x}_i\right)=h\left(\boldsymbol{x}_i\right)\right)+\mathbb{I}\left(f\left(\boldsymbol{x}_i\right) \neq h\left(\boldsymbol{x}_i\right)\right)=1
$$


所以 

$$
\begin{aligned}
& e^{-\alpha} \mathbb{I}\left(f\left(\boldsymbol{x}_i\right)=h\left(\boldsymbol{x}_i\right)\right)+e^\alpha \mathbb{I}\left(f\left(\boldsymbol{x}_i\right) \neq h\left(\boldsymbol{x}_i\right)\right) \\
= & e^{-\alpha} \mathbb{I}\left(f\left(\boldsymbol{x}_i\right)=h\left(\boldsymbol{x}_i\right)\right)+e^{-\alpha} \mathbb{I}\left(f\left(\boldsymbol{x}_i\right) \neq h\left(\boldsymbol{x}_i\right)\right)-e^{-\alpha} \mathbb{I}\left(f\left(\boldsymbol{x}_i\right) \neq h\left(\boldsymbol{x}_i\right)\right)+e^\alpha \mathbb{I}\left(f\left(\boldsymbol{x}_i\right) \neq h\left(\boldsymbol{x}_i\right)\right) \\
= & e^{-\alpha}\left(\mathbb{I}\left(f\left(\boldsymbol{x}_i\right)=h\left(\boldsymbol{x}_i\right)\right)+\mathbb{I}\left(f\left(\boldsymbol{x}_i\right) \neq h\left(\boldsymbol{x}_i\right)\right)\right)+\left(e^\alpha-e^{-\alpha}\right) \mathbb{I}\left(f\left(\boldsymbol{x}_i\right) \neq h\left(\boldsymbol{x}_i\right)\right) \\
= & e^{-\alpha}+\left(e^\alpha-e^{-\alpha}\right) \mathbb{I}\left(f\left(\boldsymbol{x}_i\right) \neq h\left(\boldsymbol{x}_i\right)\right)
\end{aligned}
$$

 将此结果代入
$\ell_{\exp }\left(H_{t-1}+\alpha h \mid \mathcal{D}\right)$, 得 **（注:
以下表达式后面求解权重 $\alpha_t$
时仍会使用）**

$$
\begin{aligned}
\ell_{\exp }\left(H_{t-1}+\alpha h \mid \mathcal{D}\right) & =\sum_{i=1}^{|D|} \mathcal{D}\left(\boldsymbol{x}_i\right) e^{-f\left(\boldsymbol{x}_i\right) H_{t-1}\left(\boldsymbol{x}_i\right)}\left(e^{-\alpha}+\left(e^\alpha-e^{-\alpha}\right) \mathbb{I}\left(f\left(\boldsymbol{x}_i\right) \neq h\left(\boldsymbol{x}_i\right)\right)\right) \\
& =\sum_{i=1}^{|D|} \mathcal{D}\left(\boldsymbol{x}_i\right) e^{-f\left(\boldsymbol{x}_i\right) H_{t-1}\left(\boldsymbol{x}_i\right)} e^{-\alpha}+\sum_{i=1}^{|D|} \mathcal{D}\left(\boldsymbol{x}_i\right) e^{-f\left(\boldsymbol{x}_i\right) H_{t-1}\left(\boldsymbol{x}_i\right)}\left(e^\alpha-e^{-\alpha}\right) \mathbb{I}\left(f\left(\boldsymbol{x}_i\right) \neq h\left(\boldsymbol{x}_i\right)\right) \\
& =e^{-\alpha} \sum_{i=1}^{|D|} \mathcal{D}_t^{\prime}\left(\boldsymbol{x}_i\right)+\left(e^\alpha-e^{-\alpha}\right) \sum_{i=1}^{|D|} \mathcal{D}_t^{\prime}\left(\boldsymbol{x}_i\right) \mathbb{I}\left(f\left(\boldsymbol{x}_i\right) \neq h\left(\boldsymbol{x}_i\right)\right)
\end{aligned}
$$

 外面; 第一项
$e^{-\alpha} \sum_{i=1}^{|D|} \mathcal{D}_t^{\prime}\left(\boldsymbol{x}_i\right)$
与 $h(\boldsymbol{x})$ 无关, 因此对于任意 $\alpha>0$, 使
$\ell_{\exp }\left(H_{t-1}+\alpha h \mid \mathcal{D}\right)$ 最小的
$h(\boldsymbol{x})$ 只需要使第二项最小即可, 即


$$
h_t=\underset{h}{\arg \min }\left(e^\alpha-e^{-\alpha}\right) \sum_{i=1}^{|D|} \mathcal{D}_t^{\prime}\left(\boldsymbol{x}_i\right) \mathbb{I}\left(f\left(\boldsymbol{x}_i\right) \neq h\left(\boldsymbol{x}_i\right)\right)
$$


对于任意 $\alpha>0$, 有 $e^\alpha-e^{-\alpha}>0$, 所以上式中与
$h(\boldsymbol{x})$ 无关的正系数可以省略:


$$
h_t=\underset{h}{\arg \min } \sum_{i=1}^{|D|} \mathcal{D}_t^{\prime}\left(\boldsymbol{x}_i\right) \mathbb{I}\left(f\left(\boldsymbol{x}_i\right) \neq h\left(\boldsymbol{x}_i\right)\right)
$$


此即式(8.18)另一种表达形式。注意, 为了确保
$\mathcal{D}_t^{\prime}(\boldsymbol{x})$ 是一个分布, 需要对其进行规范化,
即
$\mathcal{D}_t(\boldsymbol{x})=\frac{\mathcal{D}_t^{\prime}(\boldsymbol{x})}{Z_t}$,
然而规范化因子
$Z_t=\sum_{i=1}^{|D|} \mathcal{D}_t^{\prime}\left(\boldsymbol{x}_i\right)$
为常数, 并不影响最小化的求解。 正是基于此结论, AdaBoost 通过
$h_t=\mathfrak{L}\left(D, \mathcal{D}_t\right.$ )得到第 $t$
轮的基分类器。 **（"西瓜书"图 8.3 的第 3
行）**

$$
\begin{aligned}
\mathcal{D}_{t+1}\left(\boldsymbol{x}_i\right) & =\mathcal{D}\left(\boldsymbol{x}_i\right) e^{-f\left(\boldsymbol{x}_i\right) H_t\left(\boldsymbol{x}_i\right)} \\
& =\mathcal{D}\left(\boldsymbol{x}_i\right) e^{-f\left(\boldsymbol{x}_i\right)\left(H_{t-1}\left(\boldsymbol{x}_i\right)+\alpha_t h_t\left(\boldsymbol{x}_i\right)\right)} \\
& =\mathcal{D}\left(\boldsymbol{x}_i\right) e^{-f\left(\boldsymbol{x}_i\right) H_{t-1}\left(\boldsymbol{x}_i\right)} e^{-f\left(\boldsymbol{x}_i\right) \alpha_t h_t\left(\boldsymbol{x}_i\right)} \\
& =\mathcal{D}_t\left(\boldsymbol{x}_i\right) e^{-f\left(\boldsymbol{x}_i\right) \alpha_t h_t\left(\boldsymbol{x}_i\right)}
\end{aligned}
$$

 此即类似式(8.19)的分布权重更新公式。

现在只差权重 $\alpha_t$ 表达式待求。对指数损失函数
$\ell_{\exp }\left(H_{t-1}+\alpha h_t \mid \mathcal{D}\right)$ 求导, 得


$$
\begin{aligned}
\frac{\partial \ell_{\exp }\left(H_{t-1}+\alpha h_t \mid \mathcal{D}\right)}{\partial \alpha} & =\frac{\partial\left(e^{-\alpha} \sum_{i=1}^{|D|} \mathcal{D}_t^{\prime}\left(\boldsymbol{x}_i\right)+\left(e^\alpha-e^{-\alpha}\right) \sum_{i=1}^{|D|} \mathcal{D}_t^{\prime}\left(\boldsymbol{x}_i\right) \mathbb{I}\left(f\left(\boldsymbol{x}_i\right) \neq h\left(\boldsymbol{x}_i\right)\right)\right)}{\partial \alpha} \\
& =-e^{-\alpha} \sum_{i=1}^{|D|} \mathcal{D}_t^{\prime}\left(\boldsymbol{x}_i\right)+\left(e^\alpha+e^{-\alpha}\right) \sum_{i=1}^{|D|} \mathcal{D}_t^{\prime}\left(\boldsymbol{x}_i\right) \mathbb{I}\left(f\left(\boldsymbol{x}_i\right) \neq h\left(\boldsymbol{x}_i\right)\right)
\end{aligned}
$$

 令导数等于零, 得 

$$
\begin{aligned}
\frac{e^{-\alpha}}{e^\alpha+e^{-\alpha}} & =\frac{\sum_{i=1}^{|D|} \mathcal{D}_t^{\prime}\left(\boldsymbol{x}_i\right) \mathbb{I}\left(f\left(\boldsymbol{x}_i\right) \neq h\left(\boldsymbol{x}_i\right)\right)}{\sum_{i=1}^{|D|} \mathcal{D}_t^{\prime}\left(\boldsymbol{x}_i\right)}=\sum_{i=1}^{|D|} \frac{\mathcal{D}_t^{\prime}\left(\boldsymbol{x}_i\right)}{Z_t} \mathbb{I}\left(f\left(\boldsymbol{x}_i\right) \neq h\left(\boldsymbol{x}_i\right)\right) \\
& =\sum_{i=1}^{|D|} \mathcal{D}_t\left(\boldsymbol{x}_i\right) \mathbb{I}\left(f\left(\boldsymbol{x}_i\right) \neq h\left(\boldsymbol{x}_i\right)\right)=\mathbb{E}_{\boldsymbol{x} \sim \mathcal{D}_t}\left[\mathbb{I}\left(f\left(\boldsymbol{x}_i\right) \neq h\left(\boldsymbol{x}_i\right)\right)\right] \\
& =\epsilon_t
\end{aligned}
$$

 对上述等式化简, 得 

$$
\begin{aligned}
\frac{e^{-\alpha}}{e^\alpha+e^{-\alpha}}=\frac{1}{e^{2 \alpha}+1} & \Rightarrow e^{2 \alpha}+1=\frac{1}{\epsilon_t} \Rightarrow e^{2 \alpha}=\frac{1-\epsilon_t}{\epsilon_t} \Rightarrow 2 \alpha=\ln \left(\frac{1-\epsilon_t}{\epsilon_t}\right) \\
& \Rightarrow \alpha_t=\frac{1}{2} \ln \left(\frac{1-\epsilon_t}{\epsilon_t}\right)
\end{aligned}
$$

 即式(8.11)。 从该式可以发现, 当 $\epsilon_t=1$ 时,
$\alpha_t \rightarrow \infty$, 此时集成分类器将由基分类器 $h_t$ 决定,
而这很可能是由于过拟合产生的结果, 例如不前枝决策树, 如果一直分下去,
一般情况下总 能得到在训练集上分类误差很小甚至为 0 的分类器,
但这并没有什么意义。所以一般在 AdaBoost 中使用弱分类器, 如决策树桩
(即单层决策树)。

另外, 由以上指数损失函数
$\ell_{\exp }\left(H_{t-1}+\alpha h \mid \mathcal{D}\right)$
的推导可以发现 

$$
\begin{aligned}
\ell_{\exp }\left(H_{t-1}+\alpha h \mid \mathcal{D}\right) & =\sum_{i=1}^{|D|} \mathcal{D}\left(\boldsymbol{x}_i\right) e^{-f\left(\boldsymbol{x}_i\right) H_{t-1}\left(\boldsymbol{x}_i\right)} e^{-f\left(\boldsymbol{x}_i\right) \alpha h\left(\boldsymbol{x}_i\right)} \\
& =\sum_{i=1}^{|D|} \mathcal{D}_t^{\prime}\left(\boldsymbol{x}_i\right) e^{-f\left(\boldsymbol{x}_i\right) \alpha h\left(\boldsymbol{x}_i\right)}
\end{aligned}
$$

 这与指数损失函数
$\ell_{\exp }\left(\alpha_t h_t \mid \mathcal{D}_t\right)$
的表达式基本一致: 

$$
\begin{aligned}
\ell_{\exp }\left(\alpha_t h_t \mid \mathcal{D}_t\right) & =\mathbb{E}_{\boldsymbol{x} \sim \mathcal{D}_t}\left[e^{-f(\boldsymbol{x}) \alpha_t h_t(\boldsymbol{x})}\right] \\
& =\sum_{i=1}^{|D|} \mathcal{D}_t\left(\boldsymbol{x}_i\right) e^{-f\left(\boldsymbol{x}_i\right) \alpha_t h_t\left(\boldsymbol{x}_t\right)}
\end{aligned}
$$

 而 $\mathcal{D}_t^{\prime}(\boldsymbol{x})$
的规范化过程并不影响对
$\ell_{\exp }\left(H_{t-1}+\alpha h \mid \mathcal{D}\right)$
求最小化操作, 因此最小化式(8.9) 等价于最小化
$\ell_{\exp }\left(H_{t-1}+\alpha h \mid \mathcal{D}\right)$,
这就是式(8.9)的来历，故并无问题。

到此为止, 就逐一完成了"西瓜书"图8.3中第 3 行的 $h_t$ 的训练
(并计算训练误差)、第 6 行的权 重 $\alpha_t$ 计算公式以及第 7 行的分布
$\mathcal{D}_t$ 更新公式来历的理论推导。

### 8.2.17 进一步理解权重更新公式

Adaboost原始文献[1]第 12 页(pdf显示第348页)有如下推论，如图8-3所示:

![图8-3 Adaboost原始文献推论2](https://datawhale-business.oss-cn-hangzhou.aliyuncs.com/c4f146c8-cf68-4469-bfc0-94f9361acd09-adaboost_corollary_2.svg)

即
$P_{\boldsymbol{x} \sim \mathcal{D}_t}\left(h_{t-1}(\boldsymbol{x}) \neq f(\boldsymbol{x})\right)=0.5$
。用通俗的话来说就是, $h_{t-1}$ 在数据集 $D$ 上、 分布为 $\mathcal{D}_t$
时 的分类误差为 $0.5$, 即相当于随机猜测 (最糟糕的二分类器是分类误差为
$0.5$, 当二分类器分 类误差为 1 时相当于分类误差为 0 ,
因为将预测结果反过来用就是了)。而 $h_t$ 由式(8.18)得到


$$
h_t=\underset{h}{\arg \min } \mathbb{E}_{\boldsymbol{x} \sim \mathcal{D}_t}[\mathbb{I}(f(\boldsymbol{x}) \neq h(\boldsymbol{x}))]=\underset{h}{\arg \min } P_{\boldsymbol{x} \sim \mathcal{D}_t}(h(\boldsymbol{x}) \neq f(\boldsymbol{x}))
$$


即 $h_t$ 是在数据集 $D$ 上、分布为 $\mathcal{D}_t$
时分类误差最小的分类器, 因此在数据集 $D$ 上、分布为 $\mathcal{D}_t$ 时,
$h_t$ 是最好的分类器, 而 $h_{t-1}$ 是最差的分类器,
故二者差别最大。"西瓜书"第8.1节的图8.2形象的说 明了 "集成个体应
'好而不同'", 此时可以说 $h_{t-1}$ 和 $h_t$ 非常 "不同"。证明如下:

对于 $h_{t-1}$ 来说, 分类误差 $\epsilon_{t-1}$ 为 

$$
\begin{aligned}
\epsilon_{t-1} & =P_{\boldsymbol{x} \sim \mathcal{D}_{t-1}}\left(h_{t-1}(\boldsymbol{x}) \neq f(\boldsymbol{x})\right)=\mathbb{E}_{\boldsymbol{x} \sim \mathcal{D}_{t-1}}\left[\mathbb{I}\left(h_{t-1}(\boldsymbol{x}) \neq f(\boldsymbol{x})\right)\right] \\
& =\sum_{i=1}^{|D|} \mathcal{D}_{t-1}\left(\boldsymbol{x}_i\right) \mathbb{I}\left(h_{t-1}(\boldsymbol{x}) \neq f(\boldsymbol{x})\right) \\
& =\frac{\sum_{i=1}^{|D|} \mathcal{D}_{t-1}\left(\boldsymbol{x}_i\right) \mathbb{I}\left(h_{t-1}(\boldsymbol{x}) \neq f(\boldsymbol{x})\right)}{\sum_{i=1}^{|D|} \mathcal{D}_{t-1}\left(\boldsymbol{x}_i\right) \mathbb{I}\left(h_{t-1}(\boldsymbol{x})=f(\boldsymbol{x})\right)+\sum_{i=1}^{|D|} \mathcal{D}_{t-1}\left(\boldsymbol{x}_i\right) \mathbb{I}\left(h_{t-1}(\boldsymbol{x}) \neq f(\boldsymbol{x})\right)}
\end{aligned}
$$

 在第 $t$ 轮, 根据分布更新公式(8.19)或"西瓜书"图8.3第7行
(规范化因子 $Z_{t-1}$ 为常量):


$$
\mathcal{D}_t=\frac{\mathcal{D}_{t-1}}{Z_{t-1}} e^{-f(\boldsymbol{x}) \alpha_{t-1} h_{t-1}(\boldsymbol{x})}
$$


其中根据式(8.11), 第 $t-1$ 轮的权重


$$
\alpha_{t-1}=\frac{1}{2} \ln \frac{1-\epsilon_{t-1}}{\epsilon_{t-1}}=\ln \sqrt{\frac{1-\epsilon_{t-1}}{\epsilon_{t-1}}}
$$


代入 $\mathcal{D}_t$ 的表达式, 则


$$
\mathcal{D}_t= \begin{cases}\frac{\mathcal{D}_{t-1}}{Z_{t-1}} \cdot \sqrt{\frac{\epsilon_{t-1}}{1-\epsilon_{t-1}}} & \text {, if } h_{t-1}(\boldsymbol{x})=f(\boldsymbol{x}) \\ \frac{\mathcal{D}_{t-1}}{Z_{t-1}} \cdot \sqrt{\frac{1-\epsilon_{t-1}}{\epsilon_{t-1}}} & \text {, if } h_{t-1}(\boldsymbol{x}) \neq f(\boldsymbol{x})\end{cases}
$$


那么 $h_{t-1}$ 在数据集 $D$ 上、分布为 $\mathcal{D}_t$ 时的分类误差
$P_{\boldsymbol{x} \sim \mathcal{D}_t}\left(h_{t-1}(\boldsymbol{x}) \neq f(\boldsymbol{x})\right.$
)为 (注意, 下式 第二行的分母等于 1, 因为
$\mathbb{I}\left(h_{t-1}(\boldsymbol{x})=f(\boldsymbol{x})\right)+\mathbb{I}\left(h_{t-1}(\boldsymbol{x}) \neq f(\boldsymbol{x})\right)=1$
) 

$$
\begin{aligned}
& P_{\boldsymbol{x} \sim \mathcal{D}_t}\left(h_{t-1}(\boldsymbol{x}) \neq f(\boldsymbol{x})\right)=\mathbb{E}_{\boldsymbol{x} \sim \mathcal{D}_t}\left[\mathbb{I}\left(h_{t-1}(\boldsymbol{x}) \neq f(\boldsymbol{x})\right)\right] \\
& =\frac{\sum_{i=1}^{|D|} \mathcal{D}_t\left(\boldsymbol{x}_i\right) \mathbb{I}\left(h_{t-1}\left(\boldsymbol{x}_i\right) \neq f\left(\boldsymbol{x}_i\right)\right)}{\sum_{i=1}^{|D|} \mathcal{D}_t\left(\boldsymbol{x}_i\right) \mathbb{I}\left(h_{t-1}\left(\boldsymbol{x}_i\right)=f\left(\boldsymbol{x}_i\right)\right)+\sum_{i=1}^{|D|} \mathcal{D}_t\left(\boldsymbol{x}_i\right) \mathbb{I}\left(h_{t-1}\left(\boldsymbol{x}_i\right) \neq f\left(\boldsymbol{x}_i\right)\right)} \\
& =\frac{\sum_{i=1}^{|D|} \frac{\mathcal{D}_{t-1}\left(\boldsymbol{x}_i\right)}{Z_{t-1}} \cdot \sqrt{\frac{1-\epsilon_{t-1}}{\epsilon_{t-1}}} \mathbb{I}\left(h_{t-1}\left(\boldsymbol{x}_i\right) \neq f\left(\boldsymbol{x}_i\right)\right)}{\sum_{i=1}^{|D|} \frac{\mathcal{D}_{t-1}\left(\boldsymbol{x}_i\right)}{Z_{t-1}} \cdot \sqrt{\frac{\epsilon_{t-1}}{1-\epsilon_{t-1}}} \mathbb{I}\left(h_{t-1}\left(\boldsymbol{x}_i\right)=f\left(\boldsymbol{x}_i\right)\right)+\sum_{i=1}^{|D|} \frac{\mathcal{D}_{t-1}\left(\boldsymbol{x}_i\right)}{Z_{t-1}} \cdot \sqrt{\frac{1-\epsilon_{t-1}}{\epsilon_{t-1}}} \mathbb{I}\left(h_{t-1}\left(\boldsymbol{x}_i\right) \neq f\left(\boldsymbol{x}_i\right)\right)} \\
& =\frac{\sqrt{\frac{1-\epsilon_{t-1}}{\epsilon_{t-1}}} \cdot \sum_{i=1}^{|D|} \mathcal{D}_{t-1}\left(\boldsymbol{x}_i\right) \mathbb{I}\left(h_{t-1}\left(\boldsymbol{x}_i\right) \neq f\left(\boldsymbol{x}_i\right)\right)}{\sqrt{\frac{\epsilon_{t-1}}{1-\epsilon_{t-1}}} \cdot \sum_{i=1}^{|D|} \mathcal{D}_{t-1}\left(\boldsymbol{x}_i\right) \mathbb{I}\left(h_{t-1}\left(\boldsymbol{x}_i\right)=f\left(\boldsymbol{x}_i\right)\right)+\sqrt{\frac{1-\epsilon_{t-1}}{\epsilon_{t-1}}} \cdot \sum_{i=1}^{|D|} \mathcal{D}_{t-1}\left(\boldsymbol{x}_i\right) \mathbb{I}\left(h_{t-1}\left(\boldsymbol{x}_i\right) \neq f\left(\boldsymbol{x}_i\right)\right)} \\
& =\frac{\sqrt{\frac{1-\epsilon_{t-1}}{\epsilon_{t-1}}} \cdot \epsilon_{t-1}}{\sqrt{\frac{\epsilon_{t-1}}{1-\epsilon_{t-1}}} \cdot\left(1-\epsilon_{t-1}\right)+\sqrt{\frac{1-\epsilon_{t-1}}{\epsilon_{t-1}}} \cdot \epsilon_{t-1}}=\frac{1}{2} \\
&
\end{aligned}
$$



### 8.2.18 能够接受带权样本的基学习算法

在Adaboost算法的推导过程中，我们发现能够接受并利用带权样本的算法才能很好的嵌入到Adaboost的框架中作为基学习器。因此这里举一些能够接受带权样本的基学习算法的例子，分别是SVM和基于随机梯度下降(SGD)的对率回归：

其实原理很简单: 对于 SVM 来说, 针对"西瓜书" P130
页的优化目标式(6.29)来说, 第二项为损失项, 此时每个样本的损失
$\ell_{0 / 1}\left(y_i\left(\boldsymbol{w}^{\mathrm{T}} \boldsymbol{x}_i+b\right)-1\right)$
直接相加, 即样本权值分布为
$\mathcal{D}\left(\boldsymbol{x}_i\right)=\frac{1}{m}$, 其中 $m$
为数据集 $D$ 样本个数; 若样本权值更新为
$\mathcal{D}_t\left(\boldsymbol{x}_i\right)$, 则此时损失求和项应该变为


$$
\sum_{i=1}^m m \mathcal{D}_t\left(\boldsymbol{x}_i\right) \cdot \ell_{0 / 1}\left(y_i\left(\boldsymbol{w}^{\mathrm{T}} \boldsymbol{x}_i+b\right)-1\right)
$$


若将 $\mathcal{D}\left(\boldsymbol{x}_i\right)=\frac{1}{m}$ 替换
$\mathcal{D}_t\left(\boldsymbol{x}_i\right)$, 则就是每个样本的损失
$\ell_{0 / 1}\left(y_i\left(\boldsymbol{w}^{\mathrm{T}} \boldsymbol{x}_i+b\right)-1\right)$
直接相加。 如此更改后, 最后推导结果影响的是式(6.39), 将由
$C=\alpha_i+\mu_i$ 变为


$$
C \cdot m \mathcal{D}_t\left(\boldsymbol{x}_i\right)=\alpha_i+\mu_i
$$


进而由 $\alpha_i, \mu_i \geq 0$ 导出
$0 \leq \alpha_i \leq C \cdot m \mathcal{D}_t\left(\boldsymbol{x}_i\right)$
。

对于基于随机梯度下降(SGD)的对率回归, 每次随机选择一个样本进行梯度下降,
总体 上的期望损失即为式(3.27), 此时每个样本被选到的概率相同, 相当于
$\mathcal{D}\left(\boldsymbol{x}_i\right)=\frac{1}{m}$ 。若样本
权值更新为 $\mathcal{D}_t\left(\boldsymbol{x}_i\right)$, 则类似于
$\mathrm{SVM}$, 针对式 (3.27) 只需要给第 $i$ 项乘以
$m \mathcal{D}_t\left(\boldsymbol{x}_i\right)$ 即可,
相当于每次随机梯度下降选择样本时以概率
$\mathcal{D}_t\left(\boldsymbol{x}_i\right)$ 选择样本 $\boldsymbol{x}_i$
即可。

注意, 这里总的损失中出现了样本个数 $m$ 。这是因为在定义损失时末求均值,
若对式(6.29)的第二项和式(3.27)乘以 $\frac{1}{m}$ 则可以将 $m$
抵消掉。然而常数项在最小化式(3.27)实际上并不影响什么,
对于式(6.29)来说只要选择平衡参数 $C$ 时选为原来的 $m$ 倍即可。

当然, 正如"西瓜书" P177 第三段中所说, "对无法接受带权样本的基学习算法,
则可通过 "重采样法' 来处理, 即在每一轮学习中,
根据样本分布对训练集重新进行采样,
再用重采样而得的样本集对基学习器进行训练"。

## 8.3 Bagging与随机森林

### 8.3.1 式(8.20)的解释

$\mathbb{I}\left(h_{t}(\boldsymbol{x})=y\right)$表示对$\mathrm{T}$个基学习器，每一个都判断结果是否与$y$一致，$y$的取值一般是$-1$和$1$，如果基学习器结果与$y$一致，则$\mathbb{I}\left(h_{t}(\boldsymbol{x})=y\right)=1$，如果样本不在训练集内，则$\mathbb{I}\left(\boldsymbol{x} \notin D_{t}\right)=1$，综合起来看就是，对包外的数据，用"投票法"选择包外估计的结果，即1或-1。

### 8.3.2 式(8.21)的推导

由式(8.20)知，$H^{\mathrm{oob}}(\boldsymbol{x})$是对包外的估计，该式表示估计错误的个数除以总的个数，得到泛化误差的包外估计。注意在本式直接除以
$D \mid$ (训练集 $D$ 样本个数), 也就是说此处假设 $T$
个基分类器的各自的包外样本的并集一定为训练集 $D$ 。实际上,
这个事实成立的概率也是比较大的, 可以计算一下: 样本属于包内的概率为
$0.632$, 那么 $T$ 次独立的 随机采样均属于包内的概率为 $0.632^T$, 当
$T=5$ 时, $0.632^T \approx 0.1$, 当 $T=10$ 时, $0.632^T \approx 0.01$,
这么来看的话 $T$ 个基分类器的各自的包外样本的并集为训练集 $D$
的概率的确实比较大。

### 8.3.3 随机森林的解释

在8.3.2节开篇第一句话就解释了随机森林的概念：随机森林是Bagging的一个扩展变体，是以决策树为基学习器构建Bagging集成的基础上，进一步在决策树的训练过程中引入了随机属性选择。

完整版随机森林当然更复杂，这时只须知道两个重点：(1)
以决策树为基学习器；(2)在基学习器训练过程中，选择划分属性时只使用当前结点属性集合的一个子集。

## 8.4 结合策略

### 8.4.1 式(8.22)的解释



$$
H(\boldsymbol{x})=\frac{1}{T} \sum_{i=1}^{T} h_{i}(\boldsymbol{x})
$$


对基分类器的结果进行简单的平均。

### 8.4.2 式(8.23)的解释



$$
H(\boldsymbol{x})=\sum_{i=1}^{T} w_{i} h_{i}(\boldsymbol{x})
$$


对基分类器的结果进行加权平均。

### 8.4.3 硬投票和软投票的解释

"西瓜书"中第183页提到了硬投票(hard voting)和软投票(soft
voting)，本页左侧注释也提到多数投票法的英文术语使用不太一致，有文献称为majority
voting。本人看到有些文献中，硬投票使用majority
voting（多数投票），软投票使用probability
voting（概率投票），所以还是具体问题具体分析比较稳妥。

### 8.4.4 式(8.24)的解释



$$
H(\boldsymbol{x})=\left\{\begin{array}{ll}
{c_{j},} & {\text { if } \sum_{i=1}^{T} h_{i}^{j}(\boldsymbol{x})>0.5 \sum_{k=1}^{N} \sum_{i=1}^{T} h_{i}^{k}(\boldsymbol{x})} \\
{\text { reject, }} & {\text { otherwise. }}
\end{array}\right.
$$


当某一个类别$j$的基分类器的结果之和，大于所有结果之和的$\frac {1}{2}$，则选择该类别$j$为最终结果。

### 8.4.5 式(8.25)的解释



$$
H(\boldsymbol{x})=c_{\underset{j}{ \arg \max} \sum_{i=1}^{T} h_{i}^{j}(\boldsymbol{x})}
$$


相比于其他类别，该类别$j$的基分类器的结果之和最大，则选择类别$j$为最终结果。

### 8.4.6 式(8.26)的解释



$$
H(\boldsymbol{x})=c_{\underset{j}{ \arg \max} \sum_{i=1}^{T} w_i h_{i}^{j}(\boldsymbol{x})}
$$


相比于其他类别，该类别$j$的基分类器的结果之和最大，则选择类别$j$为最终结果，与式(8.25)不同的是，该式在基分类器前面乘上一个权重系数，该系数大于等于0，且T个权重之和为1。

### 8.4.7 元学习器(meta-learner)的解释

书中第183页最后一行提到了元学习器(meta-learner)，简单解释一下，因为理解meta的含义有时对于理解论文中的核心思想很有帮助。

元(meta)，非常抽象，例如此处的含义，即次级学习器，或者说基于学习器结果的学习器；另外还有元语言，就是描述计算机语言的语言，还有元数学，研究数学的数学等等；

另外，论文中经常出现的还有meta-strategy，即元策略或元方法，比如说你的研究问题是多分类问题，那么你提出了一种方法，例如对输入特征进行变换（或对输出类别做某种变换），然后再基于普通的多分类方法进行预测，这时你的方法可以看成是一种通用的框架，它虽然针对多分类问题开发，但它需要某个具体多分类方法配合才能实现，那么这样的方法是一种更高层级的方法，可以称为是一种meta-strategy。

### 8.4.8 Stacking算法的解释

该算法其实非常简单，对于数据集，试想你现在有了
个基分类器预测结果，也就是说数据集中的每个样本均有
个预测结果，那么怎么结合这 个预测结果呢？

本节名为"结合策略"，告诉你各种结合方法，但其实最简单的方法就是基于这
个预测结果再进行一次学习，即针对每个样本，将这
个预测结果作为输入特征，类别仍为原来的类别，既然无法抉择如何将这些结果进行结合，那么就"学习"一下吧。

"西瓜书"图8.9伪代码第9行中将第个样本进行变换，特征为个基学习器的输出，类别标记仍为原来的
，将所有训练集中的样本进行转换得到新的数据集后，再基于进行一次学习即可，也就是Stacking算法。

至于说"西瓜书"图8.9中伪代码第1行到第3行使用的数据集与第5行到第10行使用的数据集之间的关系，在"西瓜书"图8.9下方的一段话有详细的讨论，不再赘述。

## 8.5 多样性

### 8.5.1 式(8.27)的解释



$$
A\left(h_{i} | \boldsymbol{x}\right)=\left(h_{i}(\boldsymbol{x})-H(\boldsymbol{x})\right)^{2}
$$


该式表示个体学习器结果与预测结果的差值的平方，即为个体学习器的"分歧"。

### 8.5.2 式(8.28)的解释



$$
\begin{aligned}
\bar{A}(h | \boldsymbol{x}) &=\sum_{i=1}^{T} w_{i} A\left(h_{i} | \boldsymbol{x}\right) \\
&=\sum_{i=1}^{T} w_{i}\left(h_{i}(\boldsymbol{x})-H(\boldsymbol{x})\right)^{2}
\end{aligned}
$$


该式表示对各个个体学习器的"分歧"加权平均的结果，即集成的"分歧"。

### 8.5.3 式(8.29)的解释



$$
E\left(h_{i} | \boldsymbol{x}\right)=\left(f(\boldsymbol{x})-h_{i}(\boldsymbol{x})\right)^{2}
$$


该式表示个体学习器与真实值之间差值的平方，即个体学习器的平方误差。

### 8.5.4 式(8.30)的解释



$$
E(H | \boldsymbol{x})=(f(\boldsymbol{x})-H(\boldsymbol{x}))^{2}
$$


该式表示集成与真实值之间差值的平方，即集成的平方误差。

### 8.5.5 式(8.31)的推导

由(8.28)知 

$$
\begin{aligned}
\bar{A}(h | \boldsymbol{x})&=\sum_{i=1}^{T} w_{i}\left(h_{i}(\boldsymbol{x})-H(\boldsymbol{x})\right)^{2}\\
&=\sum_{i=1}^{T} w_{i}(h_i(\boldsymbol{x})^2-2h_i(\boldsymbol{x})H(\boldsymbol{x})+H(\boldsymbol{x})^2)\\
&=\sum_{i=1}^{T} w_{i}h_i(\boldsymbol{x})^2-H(\boldsymbol{x})^2
\end{aligned}
$$

 又因为 

$$
\begin{aligned}
& \sum_{i=1}^{T} w_{i} E\left(h_{i} | \boldsymbol{x}\right)-E(H | \boldsymbol{x})\\
&=\sum_{i=1}^{T} w_{i}\left(f(\boldsymbol{x})-h_{i}(\boldsymbol{x})\right)^{2}-(f(\boldsymbol{x})-H(\boldsymbol{x}))^{2}\\
&=\sum_{i=1}^{T} w_{i}h_i(\boldsymbol{x})^2-H(\boldsymbol{x})^{2}
\end{aligned}
$$

 所以


$$
\bar{A}(h | \boldsymbol{x}) =\sum_{i=1}^{T} w_{i} E\left(h_{i} | \boldsymbol{x}\right)-E(H | \boldsymbol{x})
$$



### 8.5.6 式(8.32)的解释



$$
\sum_{i=1}^{T} w_{i} \int A\left(h_{i} | \boldsymbol{x}\right) p(\boldsymbol{x}) d \boldsymbol{x}=\sum_{i=1}^{T} w_{i} \int E\left(h_{i} | \boldsymbol{x}\right) p(\boldsymbol{x}) d \boldsymbol{x}-\int E(H | \boldsymbol{x}) p(\boldsymbol{x}) d \boldsymbol{x}
$$


$\int A\left(h_{i} | \boldsymbol{x}\right) p(\boldsymbol{x}) d \boldsymbol{x}$表示个体学习器在全样本上的"分歧"，$\sum_{i=1}^{T} w_{i} \int A\left(h_{i} | \boldsymbol{x}\right) p(\boldsymbol{x}) d \boldsymbol{x}$表示集成在全样本上的"分歧"。
式(8.31)的意义在于, 对于示例 $\boldsymbol{x}$ 有
$\bar{A}(h \mid \boldsymbol{x})=\bar{E}(h \mid \boldsymbol{x})-E(H \mid \boldsymbol{x})$
成立, 即个体学习器分歧的加权均值等于个体学习器误差的加权均值减去集成
$H(\boldsymbol{x})$ 的误差。

将这个结论应用于全样本上, 即为式(8.32)。

例如
$A_i=\int A\left(h_i \mid \boldsymbol{x}\right) p(\boldsymbol{x}) d \boldsymbol{x}$,
这是将 $\boldsymbol{x}$ 作为连续变量来处理的, 所以这里是概率密度
$p(\boldsymbol{x})$ 和积分号; 若按离散变量来处理, 则变为
$A_i=\sum_{\boldsymbol{x} \in D} A\left(h_i \mid \boldsymbol{x}\right) p_{\boldsymbol{x}}$;
其实高等数学中讲过, 积分就是连续求和。

### 8.5.7 式(8.33)的解释



$$
E_{i}=\int E\left(h_{i} | \boldsymbol{x}\right) p(\boldsymbol{x}) d \boldsymbol{x}
$$


表示个体学习器在全样本上的泛化误差。

### 8.5.8 式(8.34)的解释



$$
A_{i}=\int A\left(h_{i} | \boldsymbol{x}\right) p(\boldsymbol{x}) d \boldsymbol{x}
$$


表示个体学习器在全样本上的分歧。

### 8.5.9 式(8.35)的解释



$$
E=\int E(H | \boldsymbol{x}) p(\boldsymbol{x}) d \boldsymbol{x}
$$


表示集成在全样本上的泛化误差。

### 8.5.10 式(8.36)的解释



$$
E=\bar{E}-\bar{A}
$$


$\bar{E}$表示个体学习器泛化误差的加权均值，$\bar{A}$表示个体学习器分歧项的加权均值，该式称为"误差-分歧分解"。

### 8.5.11 式(8.40)的解释

当 $p_1=p_2$ 时, $\kappa=0$; 当 $p_1=1$ 时, $\kappa=1$; 一般来说
$p_1 \geqslant p_2$, 即 $\kappa \geqslant 0$, 但偶尔也 有 $p_1<p_2$
的情况, 此时 $\kappa<0$ 。 有关 $p_1, p_2$
的意义参见式(8.41)和式(8.42)的解释。

### 8.5.12 式(8.41)的解释

分子 $a+d$ 为分类器 $h_i$ 与 $h_j$ 在数据集 $D$
上预测结果相同的样本数目, 分母为数据集 $D$ 总 样本数目, 因此 $p_1$
为两个分类器 $h_i$ 与 $h_j$ 预测结果相同的概率。 若 $a+d=m$, 即分类器
$h_i$ 与 $h_j$ 对数据集 $D$ 所有样本预测结果均相同, 此时 $p_1=1$ 。

### 8.5.13 式(8.42)的解释

将式(8.42)拆分为如下形式，将会很容易理解其含义：


$$
p_2=\frac{a+b}{m} \cdot \frac{a+c}{m}+\frac{c+d}{m} \cdot \frac{b+d}{m}
$$


其中 $\frac{a+b}{m}$ 为分类器 $h_i$ 将样本预测为 $+1$ 的概率,
$\frac{a+c}{m}$ 为分类器 $h_j$ 将样本预测为 $+1$ 的概率, 二者 相乘
$\frac{a+b}{m} \cdot \frac{a+c}{m}$ 可理解为分类器 $h_i$ 与 $h_j$
将样本预测为 $+1$ 的概率; $\frac{c+d}{m}$ 为分类器 $h_i$ 将样本预测为
$-1$ 的概率, $\frac{b+d}{m}$ 为分类器 $h_j$ 将样本预测为 $-1$ 的概率,
二者相乘 $\frac{c+d}{m} \cdot \frac{b+d}{m}$ 可理解为分类器 $h_i$ 与
$h_j$ 将样本预测为 $-1$ 的概率。

注意 $\frac{a+b}{m} \cdot \frac{a+c}{m}$ 与 $\frac{a}{m}$ 的不同,
$\frac{c+d}{m} \cdot \frac{b+d}{m}$ 与 $\frac{d}{m}$ 的不同:


$$
\begin{aligned}
& \frac{a+b}{m} \cdot \frac{a+c}{m}=p\left(h_i=+1\right) p\left(h_j=+1\right), \frac{a}{m}=p\left(h_i=+1, h_j=+1\right) \\
& \frac{c+d}{m} \cdot \frac{b+d}{m}=p\left(h_i=-1\right) p\left(h_j=-1\right), \frac{d}{m}=p\left(h_i=-1, h_j=-1\right)
\end{aligned}
$$

 即 $\frac{a+b}{m} \cdot \frac{a+c}{m}$ 和
$\frac{c+d}{m} \cdot \frac{b+d}{m}$ 是分别考虑分类器 $h_i$ 与 $h_j$
时的概率 ( $h_i$ 与 $h_j$ 独立), 而 $\frac{a}{m}$ 和 $\frac{d}{m}$
是同时考 虑 $h_i$ 与 $h_j$ 时的概率 (联合概率)。

### 8.5.14 多样性增强的解释

在 8.5.3 节介绍了四种多样性增强的方法, 通俗易懂, 几乎不需要什么注解,
仅强调几 个概念:

(1)数据样本扰动中提到了 "不稳定基学习器" (例如决策树、神经网络等) 和
"稳定基 学习器" (例如线性学习器、支持向量机、朴素贝叶斯、 $k$
近邻学习器等), 对稳定基学习器
进行集成时数据样本扰动技巧效果有限。这也就可以解释为什么随机森林和 GBDT
等以决 策树为基分学习器的集成方法很成功吧, Gradient Boosting 和 Bagging
都是以数据样本扰动 来增强多样性的; 而且,
掌握这个经验后在实际工程应用中就可以排除一些候选基分类器,
但论文中的确经常见到以支持向量机为基分类器 Bagging 实现, 这可能是由于
LIBSVM 简单易用的原因吧。

(2)"西瓜书"图8.11随机子空间算法, 针对每个基分类器 $h_t$
在训练时使用了原数据集的部分输入属性（末必是初始属性, 详见第 189
页左上注释), 因此在最终集成时 **（"西瓜书"图 8.11
最后一行）** 也要使用相同的部分属性。

(3)输出表示扰动中提到了 "翻转法" (Flipping Output),
看起来是一个并没有道理的技巧, 为什么要将训练样本的标记改变呢?
若认为原训练样本标记是完全可靠的, 这不是人为地加入噪声么? 但西瓜书作者
2017 年提出的深度森林[3]模型中也用到了该技巧,
正如本小节名为"多样性增强", 虽然从局部来看引入了标记噪声,
但从模型集成的角度来说却是有益的。

## 8.6 Gradient Boosting/GBDT/XGBoost联系与区别

在集成学习中，梯度提升(Gradient Boosting, GB)、梯度提升树(GB Decision
Tree,
GBDT)很常见，尤其是近几年非常流行的XGBoost很是耀眼，此处单独介绍对比这些概念。

### 8.6.1 梯度下降法
**（本部分内容参考了孙文瑜教授的最优化方法[4]）**
设目标函数 $f(\boldsymbol{x})$ 在 $\boldsymbol{x}_k$ 附近连续可微, 且
$\nabla f\left(\boldsymbol{x}_k\right)=\left.\frac{\nabla f(\boldsymbol{x})}{\nabla \boldsymbol{x}}\right|_{\boldsymbol{x}=\boldsymbol{x}_k} \neq 0$
。将 $f(\boldsymbol{x})$ 在 $\boldsymbol{x}_k$ 处进 行一阶 Taylor 展开


$$
f(\boldsymbol{x}) \approx f\left(\boldsymbol{x}_k\right)+\nabla f\left(\boldsymbol{x}_k\right)^{\mathrm{T}}\left(\boldsymbol{x}-\boldsymbol{x}_k\right)
$$


记 $\boldsymbol{x}-\boldsymbol{x}_k=\Delta \boldsymbol{x}$, 则上式可写为


$$
f\left(\boldsymbol{x}_k+\Delta \boldsymbol{x}\right) \approx f\left(\boldsymbol{x}_k\right)+\nabla f\left(\boldsymbol{x}_k\right)^{\mathrm{T}} \Delta \boldsymbol{x}
$$


显然, 若
$\nabla f\left(\boldsymbol{x}_k\right)^{\mathrm{T}} \Delta \boldsymbol{x}<0$
则有
$f\left(\boldsymbol{x}_k+\Delta \boldsymbol{x}\right)<f\left(\boldsymbol{x}_k\right)$,
即相比于 $f\left(\boldsymbol{x}_k\right)$, 自变量增量
$\Delta \boldsymbol{x}$ 会 使 $f(\boldsymbol{x})$ 函数值下降; 若要使
$f(\boldsymbol{x})=f\left(\boldsymbol{x}_k+\Delta \boldsymbol{x}\right)$
下降最快, 只要选择 $\Delta \boldsymbol{x}$ 使
$\nabla f\left(\boldsymbol{x}_k\right)^{\mathrm{T}} \Delta \boldsymbol{x}$
最 小即可, 而此时
$\nabla f\left(\boldsymbol{x}_k\right)^{\mathrm{T}} \Delta \boldsymbol{x}<0$,
因此使绝对值$| f\left(\boldsymbol{x}_k\right)^{\mathrm{T}} \Delta \boldsymbol{x}|$
最大即可。将 $\Delta \boldsymbol{x}$ 分成两 部分:
$\Delta \boldsymbol{x}=\alpha_k \boldsymbol{d}_k$, 其中
$\boldsymbol{d}_k$ 为待求单位向量, $\alpha_k>0$ 为待解常量;
$\boldsymbol{d}_k$ 表示往哪个方向改 变 $\boldsymbol{x}$ 函数值下降最快,
而 $\alpha_k$ 表示沿这个方向的步长。因此, 求解 $\Delta \boldsymbol{x}$
的问题变为


$$
\left(\alpha_k, \boldsymbol{d}_k\right)=\underset{\alpha, \boldsymbol{d}}{\arg \min } \nabla f\left(\boldsymbol{x}_k\right)^{\mathrm{T}} \alpha \boldsymbol{d}
$$


将以上优化问题分为两步求解, 即 

$$
\begin{gathered}
\boldsymbol{d}_k=\underset{\boldsymbol{d}}{\arg \min } \nabla f\left(\boldsymbol{x}_k\right)^{\mathrm{T}} \boldsymbol{d} \quad \text { s.t. }\|\boldsymbol{d}\|_2=1 \\
\alpha_k=\underset{\alpha}{\arg \min } \nabla f\left(\boldsymbol{x}_k\right)^{\mathrm{T}} \boldsymbol{d}_k \alpha
\end{gathered}
$$

 以上求解 $\alpha_k$ 的优化问题明显有问题, 因为对于
$\nabla f\left(\boldsymbol{x}_k\right)^{\mathrm{T}} \boldsymbol{d}_k<0$
来说, 显然 $\alpha_k=+\infty$ 时取 的最小值, 求解 $\alpha_k$
应该求解如下优化问题:


$$
\alpha_k=\underset{\alpha}{\arg \min } f\left(\boldsymbol{x}_k+\alpha \boldsymbol{d}_k\right)
$$


对于凸函数来说, 以上两步可以得到最优解; 但对于非凸函数来说, 联合求解得到
$\boldsymbol{d}_k$ 和 $\alpha_k$, 与先求 $\boldsymbol{d}_k$
然后基于此再求 $\alpha_k$ 的结果应该有时是不同的。 由 Cauchy-Schwartz
不等式


$$
\left|\nabla f\left(\boldsymbol{x}_k\right)^{\mathrm{T}} \boldsymbol{d}_k\right| \leq\left\|\nabla f\left(\boldsymbol{x}_k\right)\right\|_2\left\|\boldsymbol{d}_k\right\|_2
$$


可知, 当且仅当
$\boldsymbol{d}_k=-\frac{\nabla f\left(\boldsymbol{x}_k\right)}{\left\|\nabla f\left(\boldsymbol{x}_k\right)\right\|_2}$
时,
$\nabla f\left(\boldsymbol{x}_k\right)^{\mathrm{T}} \boldsymbol{d}_k$
最小,
$-\nabla f\left(\boldsymbol{x}_k\right)^{\mathrm{T}} \boldsymbol{d}_k$
最大。 对于 $\alpha_k$, 若
$f\left(\boldsymbol{x}_k+\alpha \boldsymbol{d}_k\right)$ 对 $\alpha$
的导数存在, 则可简单求解如下单变量方程即可:


$$
\frac{\partial f\left(\boldsymbol{x}_k+\alpha \boldsymbol{d}_k\right)}{\partial \alpha}=0
$$


例 1: 试求 $f(x)=x^2$ 在 $x_k=2$ 处的梯度方向 $d_k$ 和步长 $\alpha_k$ 。
解: 对 $f(x)$ 在 $x_k=2$ 处进行一阶 Taylor 展开: 

$$
\begin{aligned}
f(x) & =f\left(x_k\right)+f^{\prime}\left(x_k\right)\left(x-x_k\right) \\
& =x_k^2+2 x_k\left(x-x_k\right) \\
& =x_k^2+2 x_k \alpha d
\end{aligned}
$$

 由于此时自变量为一维, 因此只有两个方向可选, 要么正方向,
要么负方向。此时 $f^{\prime}\left(x_k\right)=4$, 因此
$d_k=-\frac{f^{\prime}\left(x_k\right)}{\left|f^{\prime}\left(x_k\right)\right|}=-1$
。接下来求 $\alpha_k$, 将 $x_k$ 和 $d_k$ 代入:


$$
f\left(x_k+\alpha d_k\right)=f(2-\alpha)=(2-\alpha)^2
$$

 进而有


$$
\frac{\partial f\left(x_k+\alpha d_k\right)}{\partial \alpha}=-2(2-\alpha)
$$


令导数等于 0 , 得 $\alpha_k=2$ 。此时 

$$
\Delta x=\alpha_k d_k=-2
$$

 则
$x_k+\Delta x=0$, 函数值 $f\left(x_k+\Delta x\right)=0$ 。 例 2: 试求
$f(\boldsymbol{x})=\|\boldsymbol{x}\|_2^2=\boldsymbol{x}^{\mathrm{T}} \boldsymbol{x}$
在
$\boldsymbol{x}_k=\left[x_k^1, x_k^2\right]^{\mathrm{T}}=[3,4]^{\mathrm{T}}$
处的梯度方向 $\boldsymbol{d}_k$ 和步长 $\alpha_k$ 。 解: 对
$f(\boldsymbol{x})$ 在
$\boldsymbol{x}_k=\left[x_k^1, x_k^2\right]^{\mathrm{T}}=[3,4]^{\mathrm{T}}$
处进行一阶 Taylor 展开: 

$$
\begin{aligned}
f(\boldsymbol{x}) & =f\left(\boldsymbol{x}_k\right)+\nabla f\left(\boldsymbol{x}_k\right)^{\mathrm{T}}\left(\boldsymbol{x}-\boldsymbol{x}_k\right) \\
& =\|\boldsymbol{x}\|_2^2+2 \boldsymbol{x}_k^{\mathrm{T}}\left(\boldsymbol{x}-\boldsymbol{x}_k\right) \\
& =\|\boldsymbol{x}\|_2^2+2 \boldsymbol{x}_k^{\mathrm{T}} \alpha \boldsymbol{d}
\end{aligned}
$$

 此时
$\nabla f\left(\boldsymbol{x}_k\right)=[6,8]^{\mathrm{T}}$, 因此
$\boldsymbol{d}_k=-\frac{\nabla f\left(\boldsymbol{x}_k\right)}{\left\|\nabla f\left(\boldsymbol{x}_k\right)\right\|_2}=[-0.6,-0.8]^{\mathrm{T}}$
。接下来求 $\alpha_k$, 将 $\boldsymbol{x}_k$ 和 $\boldsymbol{d}_k$ 代入:


$$
\begin{aligned}
f\left(\boldsymbol{x}_k+\alpha \boldsymbol{d}_k\right) & =(3-0.6 \alpha)^2+(4-0.8 \alpha)^2 \\
& =\alpha^2-10 \alpha+25 \\
& =(\alpha-5)^2
\end{aligned}
$$

 因此可得 $\alpha_k=5$ (或对 $\alpha$ 求导, 再令导数等于
0 )。此时


$$
\Delta \boldsymbol{x}=\alpha_k \boldsymbol{d}_k=[-3,-4]^{\mathrm{T}}
$$


则 $\boldsymbol{x}_k+\Delta \boldsymbol{x}=[0,0]^{\mathrm{T}}$, 函数值
$f\left(\boldsymbol{x}_k+\Delta \boldsymbol{x}\right)=0$ 。
通过以上分析, 只想强调两点: (1)梯度下降法求解下降最快的方向
$\boldsymbol{d}_k$ 时应该求解如下优化问题:


$$
\boldsymbol{d}_k=\underset{\boldsymbol{d}}{\arg \min } \nabla f\left(\boldsymbol{x}_k\right)^{\mathrm{T}} \boldsymbol{d} \text { s.t. }\|\boldsymbol{d}\|_2=C
$$


其中 $C$ 为常量, 即不必严格限定 $\left\|\boldsymbol{d}_k\right\|_2=1$,
只要固定向量长度, 与 $\alpha_k$ 搭配即可。 (2)梯度下降法求解步长
$\alpha_k$ 应该求解如下优化问题:


$$
\alpha_k=\underset{\alpha}{\arg \min } f\left(\boldsymbol{x}_k+\alpha \boldsymbol{d}_k\right)
$$


实际应用中, 很多时候不会去求最优的 $\alpha_k$, 而是靠经验设置一个步长。

### 8.6.2 从梯度下降的角度解释AdaBoost

AdaBoost 第 $t$ 轮迭代时最小化式(8.5)的指数损失函数


$$
\ell_{\exp }\left(H_t \mid \mathcal{D}\right)=\mathbb{E}_{\boldsymbol{x} \sim \mathcal{D}}\left[e^{-f(\boldsymbol{x}) H_t(\boldsymbol{x})}\right]=\sum_{\boldsymbol{x} \in D} \mathcal{D}(\boldsymbol{x}) e^{-f(\boldsymbol{x}) H_t(\boldsymbol{x})}
$$


对 $\ell_{\exp }\left(H_t \mid \mathcal{D}\right)$ 每一项在 $H_{t-1}$
处泰勒展开 

$$
\begin{aligned}
\ell_{\exp }\left(H_t \mid \mathcal{D}\right) & \approx \sum_{\boldsymbol{x} \in D} \mathcal{D}(\boldsymbol{x})\left(e^{-f(\boldsymbol{x}) H_{t-1}(\boldsymbol{x})}-f(\boldsymbol{x}) e^{-f(\boldsymbol{x}) H_{t-1}(\boldsymbol{x})}\left(H_t(\boldsymbol{x})-H_{t-1}(\boldsymbol{x})\right)\right) \\
& =\sum_{\boldsymbol{x} \in D} \mathcal{D}(\boldsymbol{x})\left(e^{-f(\boldsymbol{x}) H_{t-1}(\boldsymbol{x})}-e^{-f(\boldsymbol{x}) H_{t-1}(\boldsymbol{x})} f(\boldsymbol{x}) \alpha_t h_t(\boldsymbol{x})\right) \\
& =\mathbb{E}_{\boldsymbol{x} \sim \mathcal{D}}\left[e^{-f(\boldsymbol{x}) H_{t-1}(\boldsymbol{x})}-e^{-f(\boldsymbol{x}) H_{t-1}(\boldsymbol{x})} f(\boldsymbol{x}) \alpha_t h_t(\boldsymbol{x})\right]
\end{aligned}
$$

 其中 $H_t=H_{t-1}+\alpha_t h_t$ 。注意: $\alpha_t, h_t$
是第 $t$ 轮待解的变量。 另外补充一下, 在上式展开中的变量为
$H_t(\boldsymbol{x})$, 在 $H_{t-1}$ 处一阶导数为


$$
\left.\frac{\partial e^{-f(\boldsymbol{x}) H_t(\boldsymbol{x})}}{\partial H_t(\boldsymbol{x})}\right|_{H_t(\boldsymbol{x})=H_{t-1}(\boldsymbol{x})}=-f(\boldsymbol{x}) e^{-f(\boldsymbol{x}) H_{t-1}(\boldsymbol{x})}
$$


如果看不习惯上述泰勒展开过程, 可令变量 $z=H_t(\boldsymbol{x})$ 和函数
$g(z)=e^{-f(\boldsymbol{x}) z}$, 对 $g(z)$ 在
$z_0=H_{t-1}(\boldsymbol{x})$ 处泰勒展开, 得 

$$
\begin{aligned}
g(z) & \approx g\left(z_0\right)+g^{\prime}\left(z_0\right)\left(z-z_0\right) \\
& =g\left(z_0\right)-f(\boldsymbol{x}) e^{-f(\boldsymbol{x}) z_0}\left(z-z_0\right) \\
& =e^{-f(\boldsymbol{x}) H_{t-1}(\boldsymbol{x})}-e^{-f(\boldsymbol{x}) H_{t-1}(\boldsymbol{x})} f(\boldsymbol{x})\left(H_t(\boldsymbol{x})-H_{t-1}(\boldsymbol{x})\right) \\
& =e^{-f(\boldsymbol{x}) H_{t-1}(\boldsymbol{x})}-e^{-f(\boldsymbol{x}) H_{t-1}(\boldsymbol{x})} f(\boldsymbol{x}) \alpha_t h_t(\boldsymbol{x})
\end{aligned}
$$



注意此处 $h_t(\boldsymbol{x}) \in\{-1,+1\}$,
类似于3.3.2节梯度下降法中的约束 $\left\|\boldsymbol{d}^t\right\|=1$ 。
类似于使用梯度下降法求解下降最快的方向$\boldsymbol{d}^t$, 此处先求 $h_t$
(先不管 $\alpha_t$ ):


$$
h_t=\underset{h}{\arg \min } \sum_{\boldsymbol{x} \in D} \mathcal{D}(\boldsymbol{x})\left(-e^{-f(\boldsymbol{x}) H_{t-1}(\boldsymbol{x})} f(\boldsymbol{x}) h(\boldsymbol{x})\right) \quad \text { s.t. } h(\boldsymbol{x}) \in\{-1,+1\}
$$


将负号去掉, 最小化变为最大化问题 

$$
\begin{aligned}
h_t & =\underset{h}{\arg \max } \sum_{\boldsymbol{x} \in D} \mathcal{D}(\boldsymbol{x})\left(e^{-f(\boldsymbol{x}) H_{t-1}(\boldsymbol{x})} f(\boldsymbol{x}) h(\boldsymbol{x})\right) \\
& =\underset{h}{\arg \max } \mathbb{E}_{\boldsymbol{x} \sim \mathcal{D}}\left[e^{-f(\boldsymbol{x}) H_{t-1}(\boldsymbol{x})} f(\boldsymbol{x}) h(\boldsymbol{x})\right] \quad \text { s.t. } h(\boldsymbol{x}) \in\{-1,+1\}
\end{aligned}
$$

 这就是式(8.14)的第 3 个等号的结果,
因此其余推导参见8.2.16节即可。 由于这里的 $h(\boldsymbol{x})$
约束较强, 因此不能直接取负梯度方向, 书中经过推导得到了
$h_t(\boldsymbol{x})$ 的 表达式, 即式(8.18)。实际上,
可以将此结果理解为满足约束条件的最快下降方向。 求得
$h_t(\boldsymbol{x})$ 之后再求 $\alpha_t$
(8.2.16节 "AdaBoost的个人推导"
注解中已经写过一遍, 此处仅粘贴至此,
具体参见8.2.16节注解，尤其是
$\ell_{\exp }\left(H_{t-1}+\alpha h_t \mid \mathcal{D}\right)$
表达式的由来):


$$
\alpha_k=\underset{\alpha}{\arg \min } \ell_{\exp }\left(H_{t-1}+\alpha h_t \mid \mathcal{D}\right)
$$


对指数损失函数
$\ell_{\exp }\left(H_{t-1}+\alpha h_t \mid \mathcal{D}\right)$ 求导, 得


$$
\begin{aligned}
\frac{\partial \ell_{\exp }\left(H_{t-1}+\alpha h_t \mid \mathcal{D}\right)}{\partial \alpha} & =\frac{\partial\left(e^{-\alpha} \sum_{i=1}^{|D|} \mathcal{D}_t^{\prime}\left(\boldsymbol{x}_i\right)+\left(e^\alpha-e^{-\alpha}\right) \sum_{i=1}^{|D|} \mathcal{D}_t^{\prime}\left(\boldsymbol{x}_i\right) \mathbb{I}\left(f\left(\boldsymbol{x}_i\right) \neq h\left(\boldsymbol{x}_i\right)\right)\right)}{\partial \alpha} \\
& =-e^{-\alpha} \sum_{i=1}^{|D|} \mathcal{D}_t^{\prime}\left(\boldsymbol{x}_i\right)+\left(e^\alpha+e^{-\alpha}\right) \sum_{i=1}^{|D|} \mathcal{D}_t^{\prime}\left(\boldsymbol{x}_i\right) \mathbb{I}\left(f\left(\boldsymbol{x}_i\right) \neq h\left(\boldsymbol{x}_i\right)\right)
\end{aligned}
$$

 令导数等于零, 得 

$$
\begin{aligned}
\frac{e^{-\alpha}}{e^\alpha+e^{-\alpha}} & =\frac{\sum_{i=1}^{|D|} \mathcal{D}_t^{\prime}\left(\boldsymbol{x}_i\right) \mathbb{I}\left(f\left(\boldsymbol{x}_i\right) \neq h\left(\boldsymbol{x}_i\right)\right)}{\sum_{i=1}^{|D|} \mathcal{D}_t^{\prime}\left(\boldsymbol{x}_i\right)}=\sum_{i=1}^{|D|} \frac{\mathcal{D}_t^{\prime}\left(\boldsymbol{x}_i\right)}{Z_t} \mathbb{I}\left(f\left(\boldsymbol{x}_i\right) \neq h\left(\boldsymbol{x}_i\right)\right) \\
& =\sum_{i=1}^{|D|} \mathcal{D}_t\left(\boldsymbol{x}_i\right) \mathbb{I}\left(f\left(\boldsymbol{x}_i\right) \neq h\left(\boldsymbol{x}_i\right)\right)=\mathbb{E}_{\boldsymbol{x} \sim \mathcal{D}_t}\left[\mathbb{I}\left(f\left(\boldsymbol{x}_i\right) \neq h\left(\boldsymbol{x}_i\right)\right)\right] \\
& =\epsilon_t
\end{aligned}
$$

 对上述等式化简, 得 

$$
\begin{aligned}
\frac{e^{-\alpha}}{e^\alpha+e^{-\alpha}}=\frac{1}{e^{2 \alpha}+1} & \Rightarrow e^{2 \alpha}+1=\frac{1}{\epsilon_t} \Rightarrow e^{2 \alpha}=\frac{1-\epsilon_t}{\epsilon_t} \Rightarrow 2 \alpha=\ln \left(\frac{1-\epsilon_t}{\epsilon_t}\right) \\
& \Rightarrow \alpha_t=\frac{1}{2} \ln \left(\frac{1-\epsilon_t}{\epsilon_t}\right)
\end{aligned}
$$

 即式(8.11)。 通过以上推导可以发现: AdaBoost
每一轮的迭代就是基于梯度下降法求解损失函数为 指数损失函数的二分类问题。
**（约束条件
$h_t(\boldsymbol{x}) \in\{-1,+1\}$）**

### 8.6.3 梯度提升(Gradient Boosting)

将 AdaBoost 的问题一般化, 即不限定损失函数为指数损失函数,
也不局限于二分类问 题, 则可以将式(8.5)写为更一般化的形式


$$
\begin{aligned}
\ell\left(H_t \mid \mathcal{D}\right) & =\mathbb{E}_{\boldsymbol{x} \sim \mathcal{D}}\left[\operatorname{err}\left(H_t(\boldsymbol{x}), f(\boldsymbol{x})\right)\right] \\
& =\mathbb{E}_{\boldsymbol{x} \sim \mathcal{D}}\left[\operatorname{err}\left(H_{t-1}(\boldsymbol{x})+\alpha_t h_t(\boldsymbol{x}), f(\boldsymbol{x})\right)\right]
\end{aligned}
$$

 问题时, $f(\boldsymbol{x}) \in \mathbb{R}$,
损失函数可使用平方损失
$\operatorname{err}\left(H_t(\boldsymbol{x}), f(\boldsymbol{x})\right)=\left(H_t(\boldsymbol{x})-f(\boldsymbol{x})\right)^2$
。 针对该一般化的损失函数和一般的学习问题, 要通过 $T$ 轮迭代得到学习器


$$
H(\boldsymbol{x})=\sum_{t=1}^T \alpha_t h_t(\boldsymbol{x})
$$

 类似于
AdaBoost, 第 $t$ 轮得到 $\alpha_t, h_t(\boldsymbol{x})$,
可先对损失函数在 $H_{t-1}(\boldsymbol{x})$ 处进行泰勒展开:


$$
\begin{aligned}
\ell\left(H_t \mid \mathcal{D}\right) & \approx \mathbb{E}_{\boldsymbol{x} \sim \mathcal{D}}\left[\operatorname{err}\left(H_{t-1}(\boldsymbol{x}), f(\boldsymbol{x})\right)+\left.\frac{\partial \operatorname{err}\left(H_t(\boldsymbol{x}), f(\boldsymbol{x})\right)}{\partial H_t(\boldsymbol{x})}\right|_{H_t(\boldsymbol{x})=H_{t-1}(\boldsymbol{x})}\left(H_t(\boldsymbol{x})-H_{t-1}(\boldsymbol{x})\right)\right] \\
& =\mathbb{E}_{\boldsymbol{x} \sim \mathcal{D}}\left[\operatorname{err}\left(H_{t-1}(\boldsymbol{x}), f(\boldsymbol{x})\right)+\left.\frac{\partial \operatorname{err}\left(H_t(\boldsymbol{x}), f(\boldsymbol{x})\right)}{\partial H_t(\boldsymbol{x})}\right|_{H_t(\boldsymbol{x})=H_{t-1}(\boldsymbol{x})} \alpha_t h_t(\boldsymbol{x})\right] \\
& =\mathbb{E}_{\boldsymbol{x} \sim \mathcal{D}}\left[\operatorname{err}\left(H_{t-1}(\boldsymbol{x}), f(\boldsymbol{x})\right)\right]+\mathbb{E}_{\boldsymbol{x} \sim \mathcal{D}}\left[\left.\frac{\partial \operatorname{err}\left(H_t(\boldsymbol{x}), f(\boldsymbol{x})\right)}{\partial H_t(\boldsymbol{x})}\right|_{H_t(\boldsymbol{x})=H_{t-1}(\boldsymbol{x})} \alpha_t h_t(\boldsymbol{x})\right]
\end{aligned}
$$

 注意, 在上式展开中的变量为 $H_t(\boldsymbol{x})$, 且有
$H_t(\boldsymbol{x})=H_{t-1}(\boldsymbol{x})+\alpha_t h_t(\boldsymbol{x})$
(类似于梯度下降法中
$\left.\boldsymbol{x}=\boldsymbol{x}_k+\alpha_k \boldsymbol{d}_k\right)$
。上式中括号内第 1 项为常量 $\ell\left(H_{t-1} \mid \mathcal{D}\right)$,
最小化 $\ell\left(H_t \mid \mathcal{D}\right)$ 只须最小化第 2
项即可。先不考虑权重 $\alpha_t$, 求解如下优化问题可得
$h_t(\boldsymbol{x})$ :


$$
h_t(\boldsymbol{x})=\underset{h}{\arg \min } \mathbb{E}_{\boldsymbol{x} \sim \mathcal{D}}\left[\left.\frac{\partial \operatorname{err}\left(H_t(\boldsymbol{x}), f(\boldsymbol{x})\right)}{\partial H_t(\boldsymbol{x})}\right|_{H_t(\boldsymbol{x})=H_{t-1}(\boldsymbol{x})} h(\boldsymbol{x})\right] \quad \text { s.t. constraints for } h(\boldsymbol{x})
$$


解得 $h_t(\boldsymbol{x})$ 之后, 再求解如下优化问题可得权重 $\alpha_t$ :


$$
\alpha_t=\underset{\alpha}{\arg \min } \mathbb{E}_{\boldsymbol{x} \sim \mathcal{D}}\left[\operatorname{err}\left(H_{t-1}(\boldsymbol{x})+\alpha h_t(\boldsymbol{x}), f(\boldsymbol{x})\right)\right]
$$


以上就是梯度提升(Gradient Boosting)的理论框架,
即每轮通过梯度(Gradient)下降的方式将 $T$
个弱学习器提升(Boosting)为强学习器。可以看出 AdaBoost 是其特殊形式。

Gradient Boosting 算法的官方版本参见[5]第 5-6 页，其中算法伪代码部分如下

![gradient_boost](https://datawhale-business.oss-cn-hangzhou.aliyuncs.com/94043087-7ecb-4a1b-a06d-0a729c0989b3-gradient_boost.svg)

感觉该伪代码针对的还是在任意损失函数
$L\left(y_i, F\left(\boldsymbol{x}_i\right)\right)$
下的回归问题。Algorithm 1 中第 3 步 和第 4 步意思是用
$\beta h\left(\boldsymbol{x}_i, \boldsymbol{a}\right)$ 拟合
$F(\boldsymbol{x})=F_{m-1}(\boldsymbol{x})$ 处负梯度, 但第 4
步表示只求参数 $\boldsymbol{a}_m$, 第 5 步单独求解参数 $\rho_m$,
这里的疑问是为什么第 4 步要用最小二乘法（即 $3.2$ 节的线性回
归）去拟合负梯度（又称伪残差）?

简单理解如下: 第 4 步要解的
$h\left(\boldsymbol{x}_i, \boldsymbol{a}\right)$
相当于梯度下降法中的待解的下降方向 $\boldsymbol{d}$,
在梯度下降法中也已提到不必严格限制 $\|\boldsymbol{d}\|_2=1$,
长度可以由步长 $\alpha$ 调节 (例如前面梯度下降方解释中的例 1 , 若直接取
$d_k=-f^{\prime}\left(x_k\right)=-4$, 则可得 $\alpha_k=0.5$, 仍有
$\left.\Delta x=\alpha_k d_k=-2\right)$, 因此第 4 步直接用
$h\left(\boldsymbol{x}_i, \boldsymbol{a}\right)$ 拟合负梯度,
与梯度下降中约束 $\|\boldsymbol{d}\|_2=1$ 的区别在于末对负梯度
除以其模值进行归一化而已。

那为什么不是直接令 $h\left(\boldsymbol{x}_i, \boldsymbol{a}\right)$
等于负梯度呢? 因为这里实际是求假设函数 $h$, 将数据集 中所有的
$\boldsymbol{x}_i$ 经假设函数 $h$ 映射到对应的伪残差 (负梯度)
$\tilde{y}_i$, 所以只能做线性回归了。

李航《统计学习方法》[2] 第 8.4.3 节中的算法 $8.4$ 并末显式体现参数
$\rho_m$, 这应该是第 2 步 的(c)步完成的, 因为(b)步只是拟合一棵回归树
(相当于 Algorithm 1 第 4 步解得
$h\left(\boldsymbol{x}_i, \boldsymbol{a}\right)$ ), 而 (c)
步才确定每个叶结点的取值 (相当于 Algorithm 1 第 5 步解得 $\rho_m$,
只是每个叶结点均对应一个 $\left.\rho_m\right)$;
而且回归问题中基函数为实值函数，可以将参数 $\rho_m$ 吸收到基函数中。

### 8.6.4 梯度提升树(GBDT)

本部分无实质GBDT内容，仅为梳理GBDT的概念，具体可参考给出的资源链接。

对于GBDT，一般资料是按Gradient
Boosting+CART处理回归问题讲解的，如林轩田《机器学习技法》课程第11讲。
但是，分类问题也可以用回归来处理，例如3.3节的对数几率回归，只需将平方损失换为对率损失（参见式(3.27)和式(6.33)，二者关系可参见第3章注解中有关式(3.27)的推导）即可。细节可以搜索林轩田老师的《机器学习基石》和《机器学习技法》两门课程以及配套的视频。

### 8.6.5 XGBoost

本部分无实质XGBoost内容，仅为梳理XGBoost的概念，具体可参考给出的资源链接。

首先，XGBoost 是eXtreme Gradient Boosting的简称。
其次，XGBoost与GBDT的关系，可大致类比为LIBSVM与SVM（或SMO算法）的关系。LIBSVM是SVM算法的一种高效实现软件包，XGBoost是GBDT的一种高效实现；在实现层面，LIBSVM对SMO算法进行了许多改进，XGBoost也对GBDT进行了许多改进；另外，LIBSVM扩展了许多SVM变体，XGBoost也不再仅仅是标准的GBDT，也扩展了一些其它功能。
最后，XGBoost是由陈天奇开发的；XGBoost
论文可以参考[6]，XGBoost工具包、文档和源码等均可以在Github上搜索到。

## 参考文献
[1] Jerome Friedman, Trevor Hastie, and Robert Tibshirani. Additive logistic regression: a statistical view
of boosting (with discussion and a rejoinder by the authors). The annals of statistics, 28(2):337–407,
2000.

[2] 李航. 统计学习方法. 清华大学出版社, 2012.

[3] Zhi-Hua Zhou and Ji Feng. Deep forest: Towards an alternative to deep neural networks. In IJCAI,
pages 3553–3559, 2017.

[4] 朱德通孙文瑜, 徐成贤. 最优化方法. 最优化方法, 2010.

[5] Jerome H Friedman. Greedy function approximation: a gradient boosting machine. Annals of statistics,
pages 1189–1232, 2001.

[6] Tianqi Chen and Carlos Guestrin. Xgboost: A scalable tree boosting system. In Proceedings of the
22nd acm sigkdd international conference on knowledge discovery and data mining, pages 785–794,
2016.
