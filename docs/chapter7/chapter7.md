```{important}
参与组队学习的同学须知：

本章学习时间：3天

本章配套视频教程：https://www.bilibili.com/video/BV1Mh411e7VU?p=11
```

# 第7章 贝叶斯分类器

本章是从概率框架下的贝叶斯视角给出机器学习问题的建模方法，不同于前几章着重于算法具体实现，本章的理论性会更强。朴素贝叶斯算法常用于文本分类，例如用于广告邮件检测，贝叶斯网和EM算法均属于概率图模型的范畴，因此可合并至第14章一起学习。

## 7.1 贝叶斯决策论

### 7.1.1 式(7.5)的推导

由式(7.1)和式(7.4)可得


$$
R(c_i|\boldsymbol x)=1*P(c_1|\boldsymbol x)+...+1*P(c_{i-1}|\boldsymbol x)+0*P(c_i|\boldsymbol x)+1*P(c_{i+1}|\boldsymbol x)+...+1*P(c_N|\boldsymbol x)
$$


又$\sum_{j=1}^{N}P(c_j|\boldsymbol x)=1$，则


$$
R(c_i|\boldsymbol x)=1-P(c_i|\boldsymbol x)
$$

 此即式(7.5）。

### 7.1.2 式(7.6)的推导

将式(7.5)代入式(7.3)即可推得此式

### 7.1.3 判别式模型与生成式模型

对于判别式模型来说，就是在已知$\boldsymbol{x}$的条件下判别其类别标记$c$，即求后验概率$P(c|\boldsymbol{x})$，前几章介绍的模型都属于判别式模型的范畴，尤其是对数几率回归最为直接明了，式(3.23)和式(3.24)直接就是后验概率的形式。

对于生成式模型来说，理解起来比较抽象，但是可通过思考以下两个问题来理解。

(1)对于数据集来说，其中的样本是如何生成的？通常假设数据集中的样本服从独立同分布，即每个样本都是按照联合概率分布$P(\boldsymbol{x},c)$采样而得，也可以描述为根据$P(\boldsymbol{x},c)$生成的。

(2)若已知样本$\boldsymbol{x}$和联合概率分布$P(\boldsymbol{x},c)$，如何预测类别呢？若样本$\boldsymbol{x}$和联合概率分布$P(\boldsymbol{x},c)$已知，则可以分别求出$\boldsymbol{x}$属于各个类别的概率，即$P(\boldsymbol{x},c_1),P(\boldsymbol{x},c_2),...,P(\boldsymbol{x},c_N)$，然后选择概率最大的类别作为样本$\boldsymbol{x}$的预测结果。

因此，之所以称为"生成式"模型，是因为所求的概率$P(\boldsymbol{x},c)$是生成样本$\boldsymbol{x}$的概率。

## 7.2 极大似然估计

### 7.2.1 式(7.12)和(7.13)的推导

根据式(7.11)和式(7.10)可知参数求解式为 

$$
\begin{aligned}
\hat{\boldsymbol{\theta}}_{c}&=\underset{\boldsymbol{\theta}_{c}}{\arg \max } LL\left(\boldsymbol{\theta}_{c}\right) \\
&=\underset{\boldsymbol{\theta}_{c}}{\arg \min } -LL\left(\boldsymbol{\theta}_{c}\right) \\
&= \underset{\boldsymbol{\theta}_{c}}{\arg \min }-\sum_{\boldsymbol{x} \in D_{c}} \log P\left(\boldsymbol{x} | \boldsymbol{\theta}_{c}\right)
\end{aligned}
$$


由"西瓜书"上下文可知，此时假设概率密度函数$p(\boldsymbol{x} | c) \sim \mathcal{N}\left(\boldsymbol{\mu}_{c}, \boldsymbol{\sigma}_{c}^{2}\right)$，其等价于假设


$$
P\left(\boldsymbol{x} | \boldsymbol{\theta}_{c}\right)=P\left(\boldsymbol{x} | \boldsymbol{\mu}_{c}, \boldsymbol{\sigma}_{c}^{2}\right)=\frac{1}{\sqrt{(2 \pi)^{d}|\boldsymbol{\Sigma}_c|}} \exp \left(-\frac{1}{2}(\boldsymbol{x}-\boldsymbol{\mu}_c)^{\mathrm{T}} \boldsymbol{\Sigma}_c^{-1}(\boldsymbol{x}-\boldsymbol{\mu}_c)\right)
$$


其中，$d$表示$\boldsymbol{x}$的维数，$\boldsymbol{\Sigma}_c=\boldsymbol{\sigma}_{c}^{2}$为对称正定协方差矩阵，$|\boldsymbol{\Sigma}_c|$表示$\boldsymbol{\Sigma}_c$的行列式。将其代入参数求解式可得


$$
\begin{aligned}
(\hat{\boldsymbol{\mu}}_{c}, \hat{\boldsymbol{\Sigma}}_{c})&= \underset{(\boldsymbol{\mu}_{c},\boldsymbol{\Sigma}_c)}{\arg \min }-\sum_{\boldsymbol{x} \in D_{c}} \log\left[\frac{1}{\sqrt{(2 \pi)^{d}|\boldsymbol{\Sigma}_c|}} \exp \left(-\frac{1}{2}(\boldsymbol{x}-\boldsymbol{\mu}_c)^{\mathrm{T}} \boldsymbol{\Sigma}_c^{-1}(\boldsymbol{x}-\boldsymbol{\mu}_c)\right)\right] \\
&= \underset{(\boldsymbol{\mu}_{c},\boldsymbol{\Sigma}_c)}{\arg \min }-\sum_{\boldsymbol{x} \in D_{c}} \left[-\frac{d}{2}\log(2 \pi)-\frac{1}{2}\log|\boldsymbol{\Sigma}_c|-\frac{1}{2}(\boldsymbol{x}-\boldsymbol{\mu}_c)^{\mathrm{T}} \boldsymbol{\Sigma}_c^{-1}(\boldsymbol{x}-\boldsymbol{\mu}_c)\right] \\
&= \underset{(\boldsymbol{\mu}_{c},\boldsymbol{\Sigma}_c)}{\arg \min }\sum_{\boldsymbol{x} \in D_{c}} \left[\frac{d}{2}\log(2 \pi)+\frac{1}{2}\log|\boldsymbol{\Sigma}_c|+\frac{1}{2}(\boldsymbol{x}-\boldsymbol{\mu}_c)^{\mathrm{T}} \boldsymbol{\Sigma}_c^{-1}(\boldsymbol{x}-\boldsymbol{\mu}_c)\right] \\
&= \underset{(\boldsymbol{\mu}_{c},\boldsymbol{\Sigma}_c)}{\arg \min }\sum_{\boldsymbol{x} \in D_{c}} \left[\frac{1}{2}\log|\boldsymbol{\Sigma}_c|+\frac{1}{2}(\boldsymbol{x}-\boldsymbol{\mu}_c)^{\mathrm{T}} \boldsymbol{\Sigma}_c^{-1}(\boldsymbol{x}-\boldsymbol{\mu}_c)\right] \\
\end{aligned}
$$


假设此时数据集$D_c$中的样本个数为$n$，即$|D_c|=n$，则上式可以改写为


$$
\begin{aligned}
(\hat{\boldsymbol{\mu}}_{c}, \hat{\boldsymbol{\Sigma}}_{c})&=\underset{(\boldsymbol{\mu}_{c},\boldsymbol{\Sigma}_c)}{\arg \min }\sum_{i=1}^{n} \left[\frac{1}{2}\log|\boldsymbol{\Sigma}_c|+\frac{1}{2}(\boldsymbol{x}_{i}-\boldsymbol{\mu}_c)^{\mathrm{T}} \boldsymbol{\Sigma}_c^{-1}(\boldsymbol{x}_{i}-\boldsymbol{\mu}_c)\right]\\
&=\underset{(\boldsymbol{\mu}_{c},\boldsymbol{\Sigma}_c)}{\arg \min }\frac{n}{2}\log|\boldsymbol{\Sigma}_c|+\sum_{i=1}^{n}\frac{1}{2}(\boldsymbol{x}_i-\boldsymbol{\mu}_c)^{\mathrm{T}} \boldsymbol{\Sigma}_c^{-1}(\boldsymbol{x}_i-\boldsymbol{\mu}_c)\\
\end{aligned}
$$


为了便于分别求解$\hat{\boldsymbol{\mu}}_{c}$和$\hat{\boldsymbol{\Sigma}}_{c}$，在这里我们根据式$\boldsymbol{x}^{\mathrm{T}}\mathbf{A}\boldsymbol{x}=\operatorname{tr}(\mathbf{A}\boldsymbol{x}\boldsymbol{x}^{\mathrm{T}}),\bar{\boldsymbol{x}}=\frac{1}{n}\sum_{i=1}^{n}\boldsymbol{x}_i$将上式中的最后一项作如下恒等变形：


$$
\begin{aligned}
&\sum_{i=1}^{n}\frac{1}{2}(\boldsymbol{x}_i-\boldsymbol{\mu}_c)^{\mathrm{T}} \boldsymbol{\Sigma}_c^{-1}(\boldsymbol{x}_i-\boldsymbol{\mu}_c)\\
=&\frac{1}{2}\operatorname{tr}\left[\boldsymbol{\Sigma}_c^{-1}\sum_{i=1}^{n}(\boldsymbol{x}_i-\boldsymbol{\mu}_c)(\boldsymbol{x}_i-\boldsymbol{\mu}_c)^{\mathrm{T}}\right]\\
=&\frac{1}{2}\operatorname{tr}\left[\boldsymbol{\Sigma}_c^{-1}\sum_{i=1}^{n}\left(\boldsymbol{x}_i\boldsymbol{x}_i^{\mathrm{T}}-\boldsymbol{x}_i\boldsymbol{\mu}_c^{\mathrm{T}}-\boldsymbol{\mu}_c\boldsymbol{x}_i^{\mathrm{T}}+\boldsymbol{\mu}_c\boldsymbol{\mu}_c^{\mathrm{T}}\right)\right]\\
=&\frac{1}{2}\operatorname{tr}\left[\boldsymbol{\Sigma}_c^{-1}\left(\sum_{i=1}^{n}\boldsymbol{x}_i\boldsymbol{x}_i^{\mathrm{T}}-n\bar{\boldsymbol{x}}\boldsymbol{\mu}_c^{\mathrm{T}}-n\boldsymbol{\mu}_c\bar{\boldsymbol{x}}^{\mathrm{T}}+n\boldsymbol{\mu}_c\boldsymbol{\mu}_c^{\mathrm{T}}\right)\right]\\
=&\frac{1}{2}\operatorname{tr}\left[\boldsymbol{\Sigma}_c^{-1}\left(\sum_{i=1}^{n}\boldsymbol{x}_i\boldsymbol{x}_i^{\mathrm{T}}-2n\bar{\boldsymbol{x}}\boldsymbol{\mu}_c^{\mathrm{T}}+n\boldsymbol{\mu}_c\boldsymbol{\mu}_c^{\mathrm{T}}+2n\bar{\boldsymbol{x}}\bar{\boldsymbol{x}}^{\mathrm{T}}-2n\bar{\boldsymbol{x}}\bar{\boldsymbol{x}}^{\mathrm{T}}\right)\right]\\
=&\frac{1}{2}\operatorname{tr}\left[\boldsymbol{\Sigma}_c^{-1}\left(\left(\sum_{i=1}^{n}\boldsymbol{x}_i\boldsymbol{x}_i^{\mathrm{T}}-2n\bar{\boldsymbol{x}}\bar{\boldsymbol{x}}^{\mathrm{T}}+n\bar{\boldsymbol{x}}\bar{\boldsymbol{x}}^{\mathrm{T}}\right)+\left(n\boldsymbol{\mu}_c\boldsymbol{\mu}_c^{\mathrm{T}}-2n\bar{\boldsymbol{x}}\boldsymbol{\mu}_c^{\mathrm{T}}+n\bar{\boldsymbol{x}}\bar{\boldsymbol{x}}^{\mathrm{T}}\right)\right)\right]\\
=&\frac{1}{2}\operatorname{tr}\left[\boldsymbol{\Sigma}_c^{-1}\left(\sum_{i=1}^{n}(\boldsymbol{x}_i-\bar{\boldsymbol{x}})(\boldsymbol{x}_i-\bar{\boldsymbol{x}})^{\mathrm{T}}+\sum_{i=1}^{n}(\boldsymbol{\mu}_c-\bar{\boldsymbol{x}})(\boldsymbol{\mu}_c-\bar{\boldsymbol{x}})^{\mathrm{T}}\right)\right]\\
=&\frac{1}{2}\operatorname{tr}\left[\boldsymbol{\Sigma}_c^{-1}\sum_{i=1}^{n}(\boldsymbol{x}_i-\bar{\boldsymbol{x}})(\boldsymbol{x}_i-\bar{\boldsymbol{x}})^{\mathrm{T}}\right]+\frac{1}{2}\operatorname{tr}\left[\boldsymbol{\Sigma}_c^{-1}\sum_{i=1}^{n}(\boldsymbol{\mu}_c-\bar{\boldsymbol{x}})(\boldsymbol{\mu}_c-\bar{\boldsymbol{x}})^{\mathrm{T}}\right]\\
=&\frac{1}{2}\operatorname{tr}\left[\boldsymbol{\Sigma}_c^{-1}\sum_{i=1}^{n}(\boldsymbol{x}_i-\bar{\boldsymbol{x}})(\boldsymbol{x}_i-\bar{\boldsymbol{x}})^{\mathrm{T}}\right]+\frac{1}{2}\operatorname{tr}\left[n\cdot\boldsymbol{\Sigma}_c^{-1}(\boldsymbol{\mu}_c-\bar{\boldsymbol{x}})(\boldsymbol{\mu}_c-\bar{\boldsymbol{x}})^{\mathrm{T}}\right]\\
=&\frac{1}{2}\operatorname{tr}\left[\boldsymbol{\Sigma}_c^{-1}\sum_{i=1}^{n}(\boldsymbol{x}_i-\bar{\boldsymbol{x}})(\boldsymbol{x}_i-\bar{\boldsymbol{x}})^{\mathrm{T}}\right]+\frac{n}{2}\operatorname{tr}\left[\boldsymbol{\Sigma}_c^{-1}(\boldsymbol{\mu}_c-\bar{\boldsymbol{x}})(\boldsymbol{\mu}_c-\bar{\boldsymbol{x}})^{\mathrm{T}}\right]\\
=&\frac{1}{2}\operatorname{tr}\left[\boldsymbol{\Sigma}_c^{-1}\sum_{i=1}^{n}(\boldsymbol{x}_i-\bar{\boldsymbol{x}})(\boldsymbol{x}_i-\bar{\boldsymbol{x}})^{\mathrm{T}}\right]+\frac{n}{2}(\boldsymbol{\mu}_c-\bar{\boldsymbol{x}})^{\mathrm{T}} \boldsymbol{\Sigma}_c^{-1}(\boldsymbol{\mu}_c-\bar{\boldsymbol{x}})
\end{aligned}
$$

 所以


$$
(\hat{\boldsymbol{\mu}}_{c}, \hat{\boldsymbol{\Sigma}}_{c})=\underset{(\boldsymbol{\mu}_{c},\boldsymbol{\Sigma}_c)}{\arg \min }\frac{n}{2}\log|\boldsymbol{\Sigma}_c|+\frac{1}{2}\operatorname{tr}\left[\boldsymbol{\Sigma}_{c}^{-1}\sum_{i=1}^{n}(\boldsymbol{x}_i-\bar{\boldsymbol{x}})(\boldsymbol{x}_i-\bar{\boldsymbol{x}})^{\mathrm{T}}\right]+\frac{n}{2}(\boldsymbol{\mu}_c-\bar{\boldsymbol{x}})^{\mathrm{T}} \boldsymbol{\Sigma}_c^{-1}(\boldsymbol{\mu}_c-\bar{\boldsymbol{x}})
$$


观察上式可知，由于此时$\boldsymbol{\Sigma}_c^{-1}$和$\boldsymbol{\Sigma}_c$一样均为正定矩阵，所以当$\boldsymbol{\mu}_c-\bar{\boldsymbol{x}}\neq\boldsymbol{0}$时，上式最后一项为正定二次型。根据正定二次型的性质可知，此时上式最后一项的取值仅与$\boldsymbol{\mu}_c-\bar{\boldsymbol{x}}$相关，并有当且仅当$\boldsymbol{\mu}_c-\bar{\boldsymbol{x}}=\boldsymbol{0}$时，上式最后一项取最小值0，此时可以解得


$$
\hat{\boldsymbol{\mu}}_{c}=\bar{\boldsymbol{x}}=\frac{1}{n}\sum_{i=1}^{n}\boldsymbol{x}_i
$$


将求解出来的$\hat{\boldsymbol{\mu}}_{c}$代回参数求解式可得新的参数求解式，有


$$
\hat{\boldsymbol{\Sigma}}_{c}=\underset{\boldsymbol{\Sigma}_c}{\arg \min }\frac{n}{2}\log|\boldsymbol{\Sigma}_c|+\frac{1}{2}\operatorname{tr}\left[\boldsymbol{\Sigma}_{c}^{-1}\sum_{i=1}^{n}(\boldsymbol{x}_i-\bar{\boldsymbol{x}})(\boldsymbol{x}_i-\bar{\boldsymbol{x}})^{\mathrm{T}}\right]
$$


此时的参数求解式是仅与$\boldsymbol{\Sigma}_c$相关的函数。

为了求解$\hat{\boldsymbol{\Sigma}}_{c}$，在这里我们不加证明地给出一个引理：设$\mathbf{B}$为$p$阶正定矩阵，$n>0$为实数，在对所有$p$阶正定矩阵$\boldsymbol{\Sigma}$有


$$
\frac{n}{2}\log|\boldsymbol{\Sigma}|+\frac{1}{2}\operatorname{tr}\left[\boldsymbol{\Sigma}^{-1}\mathbf{B}\right]\geq\frac{n}{2}\log|\mathbf{B}|+\frac{pn}{2}(1-\log n)
$$


当且仅当$\boldsymbol{\Sigma}=\frac{1}{n}\mathbf{B}$时等号成立。**（引理的证明可搜索张伟平老师的"多元正态分布参数的估计和数据的清洁与变换"课件）**

根据此引理可知，当且仅当$\boldsymbol{\Sigma}_c=\frac{1}{n}\sum_{i=1}^{n}(\boldsymbol{x}_i-\bar{\boldsymbol{x}})(\boldsymbol{x}_i-\bar{\boldsymbol{x}})^{\mathrm{T}}$
时，上述参数求解式中$\arg \min$后面的式子取到最小值，那么此时的$\boldsymbol{\Sigma}_c$即我们想要求解的$\hat{\boldsymbol{\Sigma}}_{c}$。

## 7.3 朴素贝叶斯分类器

### 7.3.1 式(7.16)和式(7.17)的解释

该式是基于大数定律的频率近似概率的思路，而该思路的本质仍然是极大似然估计，下面举例说明。以掷硬币为例，假设投掷硬币5次，结果依次是正面、正面、反面、正面、反面，试基于此观察结果估计硬币正面朝上的概率。

设硬币正面朝上的概率为$\theta$，其服从伯努利分布，因此反面朝上的概率为$1-\theta$，同时设每次投掷结果相互独立，即独立同分布，则似然为


$$
\begin{aligned}
L(\theta)&=\theta\cdot\theta\cdot(1-\theta)\cdot\theta\cdot(1-\theta)\\
&=\theta^{3}(1-\theta)^2
\end{aligned}
$$

 对数似然为


$$
LL(\theta)=\ln L(\theta)=3\ln\theta+2\ln (1-\theta)
$$


易证$LL(\theta)$是关于$\theta$的凹函数，因此对其求一阶导并令导数等于零即可求出最大值点，具体地


$$
\begin{aligned}
    \frac{\partial LL(\theta)}{\partial\theta}&=\frac{\partial\left(3\ln\theta+2\ln (1-\theta)\right)}{\partial\theta}\\
    &=\frac{3}{\theta}-\frac{2}{1-\theta}\\
    &=\frac{3-5\theta}{\theta(1-\theta)}
\end{aligned}
$$


令上式等于0可解得$\theta=\frac{3}{5}$，显然$\frac{3}{5}$也是正面出现的频率。

### 7.3.2 式(7.18)的解释

该式所表示的正态分布并不一定是标准正态分布，因此$p(x_i|c)$的取值并不一定在$(0,1)$之间，但是仍然不妨碍其用作"概率"，因为根据朴素贝叶斯的算法原理可知，只需$p(x_i|c)$的值仅仅是用来比大小，因此只关心相对值而不关心绝对值。

### 7.3.3 贝叶斯估计

贝叶斯学派视角下的一类点估计法称为贝叶斯估计[1]，常用的贝叶斯估计有最大后验估计（Maximum
A Posteriori
Estimation，简称MAP）、后验中位数估计和后验期望值估计这3种参数估计方法，下面给出这3种方法的具体定义。

设总体的概率质量函数（若总体的分布为连续型时则改为概率密度函数，此处以离散型为例）
为$P(x|\theta)$，从该总体中抽取出的$n$个独立同分布的样本构成样本集
$D=\{x_1,x_2,\cdots,x_n\}$，则根据贝叶斯式可得，在给定样本集$D$的条件下，$\theta$的条件概率为


$$
P(\theta|D)=\frac{P(D|\theta)P(\theta)}{P(D)}=\frac{P(D|\theta)P(\theta)}
{\sum_{\theta}P(D|\theta)P(\theta)}
$$


其中$P(D|\theta)$为似然函数，由于样本集$D$中的样本是独立同分布的，所以似然函数可以进一步展开，有


$$
P(\theta|D)=\frac{P(D|\theta)P(\theta)}
{\sum_{\theta}P(D|\theta)P(\theta)}=\frac{\prod_{i=1}^{n}P(x_i|\theta)
P(\theta)}{\sum_{\theta}\prod_{i=1}^{n}P(x_i|\theta)P(\theta)}
$$



根据贝叶斯学派的观点，此条件概率代表了我们在已知样本集$D$后对$\theta$产生的新的认识，它综合了我们对$\theta$主观预设的先验概率$P(\theta)$和样本集$D$带来的信息，通常称其为$\theta$的后验概率。

贝叶斯学派认为，在得到$P(\theta|D)$以后，对参数$\theta$的任何统计推断，都只能基于$P(\theta|D)$。至于具体如何去使用它，可以结合某种准则一起去进行，统计学家也有一定的自由度。
对于点估计来说，
求使得$P(\theta|D)$达到最大值的$\hat{\theta}_{\mathrm{MAP}}$作为$\theta$的估计称为最大后验估计，求$P(\theta|D)$的中位数$\hat{\theta}_{\mathrm{Median}}$作为$\theta$的估计称为后验中位数估计，求$P(\theta|D)$的期望值（均值）$\hat{\theta}_{\mathrm{
Mean}}$作为$\theta$的估计称为后验期望值估计。

### 7.3.4 Categorical分布

Categorical分布又称为广义伯努利分布，是将伯努利分布中的随机变量可取值个数由两个泛化为多个得到的分布。具体地，设离散型随机变量$X$共有$k$种可能的取值$\{x_1,x_2,\cdots,x_k\}$，且$X$取到每个值的概率分别为$P(X=x_1)=\theta_1,P(X=x_2)=\theta_2,\cdots,P(X=x_k)=\theta_k$，则称随机变量$X$服从参数为$\theta_1,\theta_2,\cdots,\theta_k$的Categorical分布，其概率质量函数为


$$
P(X=x_i)=p(x_i)=\theta_i
$$



### 7.3.5 Dirichlet分布

类似于Categorical分布是伯努利分布的泛化形式，Dirichlet分布是Beta分布的泛化形式。对于一个$k$维随机变量$\boldsymbol{x}=(x_1,x_2,\cdots,x_k)\in
\mathbb{R}^{k}$，其中$x_i(i=1,2,\cdots,k)$满足$0\leqslant x_i
\leqslant
1,\sum_{i=1}^{k}x_i=1$，若$\boldsymbol{x}$服从参数为$\boldsymbol{\alpha}=(\alpha_1,\alpha_2,\cdots,\alpha_k)\in
\mathbb{R}^{k}$的Dirichlet分布，则其概率密度函数为


$$
p(\boldsymbol{x};\boldsymbol{\alpha})=\frac{\Gamma \left(\sum _{i=1}^{k}\alpha _{i}\right)}
{\prod _{i=1}^{k}\Gamma (\alpha _{i})}\prod
_{i=1}^{k}x_{i}^{\alpha _{i}-1}
$$

 其中$\Gamma (z)=\int
_{0}^{\infty }x^{z-1}e^{-x}{\rm
d}x$为Gamma函数，当$\boldsymbol{\alpha}=(1,1,\cdots,1)$时，Dirichlet分布等价于均匀分布。

### 7.3.6 式(7.19)和式(7.20)的推导

从贝叶斯估计的角度来说，拉普拉斯修正就等价于先验概率为Dirichlet分布的后验期望值估计。为了接下来的叙述方便，我们重新定义一下相关数学符号。

设有包含$m$个独立同分布样本的训练集$D$，$D$中可能的类别数为$k$，其类别的具体取值范围为$\{c_1,c_2,...,c_k\}$。若令随机变量$C$表示样本所属的类别，且$C$取到每个值的概率分别为$P(C=c_1)=\theta_1,P(C=c_2)=\theta_2,...,P(C=c_k)=\theta_k$，那么显然$C$服从参数为$\boldsymbol{\theta}=(\theta_1,\theta_2,...,\theta_k)\in\mathbb{R}^{k}$的Categorical分布，其概率质量函数为


$$
P(C=c_i)=P(c_i)=\theta_i
$$



其中$P(c_i)=\theta_i$就是式(7.9)所要求解的$\hat{P}(c)$，下面我们用贝叶斯估计中的后验期望值估计来估计$\theta_i$。根据贝叶斯估计的原理可知，在进行参数估计之前，需要先主观预设一个先验概率$P(\boldsymbol{\theta})$，通常为了方便计算后验概率$P(\boldsymbol{\theta}|D)$，我们会用似然函数$P(D|\boldsymbol{\theta})$的共轭先验作为我们的先验概率。显然，此时的似然函数$P(D|\boldsymbol{\theta})$是一个基于Categorical分布的似然函数，而Categorical分布的共轭先验为Dirichlet分布，所以只需要预设先验概率$P(\boldsymbol{\theta})$为Dirichlet分布，然后使用后验期望值估计就能估计出$\theta_i$。

具体地，记$D$中样本类别取值为$c_i$的样本个数为$y_i$，则似然函数$P(D|\boldsymbol{\theta})$可展开为


$$
P(D|\boldsymbol{\theta})=\theta_1^{y_1}...\theta_k^{y_k}=\prod_{i=1}^{k}\theta_i^{y_i}
$$


则有后验概率 

$$
\begin{aligned}
P(\boldsymbol{\theta}|D)&=\frac{P(D|\boldsymbol{\theta})P(\boldsymbol{\theta})}{P(D)}\\
&=\frac{P(D|\boldsymbol{\theta})P(\boldsymbol{\theta})}{\sum_{\boldsymbol{\theta}}
P(D|\boldsymbol{\theta})P(\boldsymbol{\theta})}\\
&=\frac{\prod_{i=1}^{k}\theta_i^{y_i}\cdot
P(\boldsymbol{\theta})}{\sum_{\boldsymbol{\theta}}\left[\prod_{i=1}^{k}\theta_i^{y_i}\cdot
P(\boldsymbol{\theta})\right]}
\end{aligned}
$$


假设此时先验概率$P(\boldsymbol{\theta})$是参数为$\boldsymbol{\alpha}=(\alpha_1,\alpha_2,...,\alpha_k)\in\mathbb{R}^{k}$的Dirichlet分布，则$P(\boldsymbol{\theta})$可写为


$$
P(\boldsymbol{\boldsymbol{\theta}};\boldsymbol{\alpha})=\frac{\Gamma \left(\sum_{i=1}^{k}\alpha_{i}\right)}{\prod_{i=1}^{k}\Gamma (\alpha_{i})}\prod_{i=1}^{k}\theta_{i}^{\alpha_{i}-1}
$$


将其代入$P(D|\boldsymbol{\theta})$可得 

$$
\begin{aligned}
P(\boldsymbol{\theta}|D)&=\dfrac{\prod_{i=1}^{k}\theta_i^{y_i}
\cdot P(\boldsymbol{\theta})}{\sum_{\boldsymbol{\theta}}
\left[\prod_{i=1}^{k}\theta_i^{y_i}\cdot
P(\boldsymbol{\theta})\right]} \\
&=\dfrac{\prod_{i=1}^{k}\theta_i^{y_i}\cdot \dfrac{\Gamma
\left(\sum _{i=1}^{k}\alpha _{i}\right)} {\prod _{i=1}^{k}\Gamma
(\alpha _{i})}\prod _{i=1}^{k} \theta_{i}^{\alpha
_{i}-1}}{\sum_{\boldsymbol{\theta}}\left[\prod_{i=1}^{k}\theta_i^{y_i}
\cdot \dfrac{\Gamma \left(\sum _{i=1}^{k}\alpha _{i}\right)}{\prod
_{i=1}^{k}
\Gamma (\alpha _{i})}\prod _{i=1}^{k}\theta_{i}^{\alpha _{i}-1}\right]}
\\
&=\dfrac{\prod_{i=1}^{k}\theta_i^{y_i}\cdot \dfrac{\Gamma
\left(\sum _{i=1}^{k}\alpha _{i}\right)} {\prod _{i=1}^{k}\Gamma
(\alpha _{i})}\prod _{i=1}^{k}\theta_{i}^{\alpha _{i}-1}}
{\sum_{\boldsymbol{\theta}}\left[\prod_{i=1}^{k}\theta_i^{y_i}\cdot
\prod _{i=1}^{k}\theta_{i}^{\alpha _{i}-1}\right]\cdot
\dfrac{\Gamma \left(\sum _{i=1}^{k}\alpha _{i}\right)}{\prod
_{i=1}^{k}\Gamma (\alpha _{i})}} \\
&=\dfrac{\prod_{i=1}^{k}\theta_i^{y_i}\cdot \prod
_{i=1}^{k}\theta_{i}^{\alpha _{i}-1}}
{\sum_{\boldsymbol{\theta}}\left[\prod_{i=1}^{k}\theta_i^{y_i}\cdot
\prod _{i=1}^{k}\theta_{i}^{\alpha _{i}-1}\right]} \\
&=\dfrac{\prod_{i=1}^{k}\theta_i^{\alpha_{i}+y_i-1}}{\sum_{\boldsymbol{\theta}}
\left[\prod_{i=1}^{k}\theta_i^{\alpha_{i}+y_i-1}\right]}
\end{aligned}
$$


此时若设$\boldsymbol{\alpha}+\boldsymbol{y}=(\alpha_1+y_1,\alpha_2+y_2,...,\alpha_k+y_k)\in
\mathbb{R}^{k}$，则根据Dirichlet分布的定义可知 

$$
\begin{aligned}
P(\boldsymbol{\theta};\boldsymbol{\alpha}+\boldsymbol{y})&=
\dfrac{\Gamma \left(\sum _{i=1}^{k}(\alpha_{i}+y_i)\right)}{\prod _{i=1}^{k}\Gamma (\alpha_{i}+y_i)}\prod _{i=1}^{k}\theta_{i}^{\alpha_{i}+y_i-1} \\
\sum_{\boldsymbol{\theta}}P(\boldsymbol{\theta};\boldsymbol{\alpha}+\boldsymbol{y})&=\sum_{\boldsymbol{\theta}}\frac{\Gamma
\left(\sum _{i=1}^{k}(\alpha_{i}+y_i)\right)}{\prod
_{i=1}^{k}\Gamma (\alpha_{i}+y_i)}\prod
_{i=1}^{k}\theta_{i}^{\alpha_{i}+y_i-1}\\
1&=\sum_{\boldsymbol{\theta}}\frac{\Gamma \left(\sum
_{i=1}^{k}(\alpha_{i}+y_i)\right)}{\prod _{i=1}^{k}\Gamma
(\alpha_{i}+y_i)}\prod _{i=1}^{k}\theta_{i}^{\alpha_{i}+y_i-1}
\\
1&=\frac{\Gamma \left(\sum
_{i=1}^{k}(\alpha_{i}+y_i)\right)}{\prod _{i=1}^{k}\Gamma
(\alpha_{i}+y_i)}\sum_{\boldsymbol{\theta}}\left[\prod
_{i=1}^{k}\theta_{i}^{\alpha_{i}+y_i-1}\right] \\
\frac{1}{\sum_{\boldsymbol{\theta}}\left[\prod _{i=1}^{k}\theta_{i}^{\alpha_{i}+y_i-1}\right]}&=\frac{\Gamma \left(\sum _{i=1}^{k}(\alpha_{i}+y_i)\right)}{\prod _{i=1}^{k}\Gamma (\alpha_{i}+y_i)} \\
\end{aligned}
$$

 将此结论代入$P(D|\boldsymbol{\theta})$可得


$$
\begin{aligned}
P(\boldsymbol{\theta}|D)&=\frac{\prod_{i=1}^{k}\theta_i^{\alpha_{i}+y_i-1}}{\sum_{\boldsymbol{\theta}}\left[\prod_{i=1}^{k}\theta_i^{\alpha_{i}+y_i-1}\right]}\\
&=\frac{\Gamma \left(\sum _{i=1}^{k}(\alpha_{i}+y_i)\right)}{\prod
_{i=1}^{k}\Gamma
(\alpha_{i}+y_i)}\prod _{i=1}^{k}\theta_{i}^{\alpha _{i}+y_i-1} \\
&=P(\boldsymbol{\theta};\boldsymbol{\alpha}+\boldsymbol{y})
\end{aligned}
$$


综上可知，对于服从Categorical分布的$\boldsymbol{\theta}$来说，假设其先验概率$P(\boldsymbol{\theta})$是参数为$\boldsymbol{\alpha}$的Dirichlet分布时，得到的后验概率$P(\boldsymbol{\theta}|D)$是参数为$\boldsymbol{\alpha}+\boldsymbol{y}$的Dirichlet分布，通常我们称这种先验概率分布和后验概率分布形式相同的这对分布为共轭分布。在推得后验概率$P(\boldsymbol{\theta}|D)$的具体形式以后，根据后验期望值估计可得$\theta_i$的估计值为


$$
\begin{aligned}
\theta_i&=\mathbb E_{P(\boldsymbol{\theta}|D)}[\theta_i]\\
&=\mathbb E_{P(\boldsymbol{\theta};\boldsymbol{\alpha}+\boldsymbol{y})}[\theta_i]\\
&=\frac{\alpha_i+y_i}{\sum_{j=1}^k(\alpha_j+y_j)}\\
&=\frac{\alpha_i+y_i}{\sum_{j=1}^k\alpha_j+\sum_{j=1}^ky_j}\\
&=\frac{\alpha_i+y_i}{\sum_{j=1}^k\alpha_j+m}\\
\end{aligned}
$$


显然，式(7.9)是当$\boldsymbol{\alpha}=(1,1,...,1)$时推得的具体结果，此时等价于我们主观预设的先验概率$P(\boldsymbol{\theta})$服从均匀分布，此即拉普拉斯修正。同理，当我们调整$\boldsymbol{\alpha}$的取值后，即可推得其他数据平滑的公式。

## 7.4 半朴素贝叶斯分类器

### 7.4.1 式(7.21)的解释

在朴素贝叶斯中求解$P(x_i|c)$时，先挑出类别为$c$的样本，若是离散属性则按大数定律估计$P(x_i|c)$，若是连续属性则求这些样本的均值和方差，接着按正态分布估计$P(x_i|c)$。现在估计$P(x_i|c,pa_i)$，则是先挑出类别为$c$且属性$x_i$所依赖的属性为$pa_i$的样本，剩下步骤与估计$P(x_i|c)$时相同。

### 7.4.2 式(7.22)的解释

该式写为如下形式可能更容易理解：


$$
I(x_i,x_j|y)=\sum_{n=1}^{N}P(x_i,x_j|c_n)\log\frac{P(x_i,x_j|c_n)}{P(x_i|c_n)P(x_j|c_n)}
$$


其中$i,j=1,2,...,d$且$i\neq j$，$N$为类别个数。该式共可得到$\frac{d(d-1)}{2}$个$I(x_i,x_j|y)$，即每对$(x_i,x_j)$均有一个条件互信息$I(x_i,x_j|y)$。

### 7.4.3 式(7.23)的推导

基于贝叶斯定理，式(7.8)将联合概率$P(\boldsymbol{x},c)$写为等价形式$P(\boldsymbol{x}|c)P(c)$，实际上，也可将向量$\boldsymbol{x}$拆开，把$P(\boldsymbol{x},c)$写为$P(x_1,x_2,...,x_d,c)$形式，然后利用概率公式$P(A,B)=P(A|B)P(B)$对其恒等变形


$$
\begin{aligned}
P(\boldsymbol{x}, c) & =P\left(x_1, x_2, \ldots, x_d, c\right) \\
& =P\left(x_1, x_2, \ldots, x_d \mid c\right) P(c) \\
& =P\left(x_1, \ldots, x_{i-1}, x_{i+1}, \ldots, x_d \mid c, x_i\right) P\left(c, x_i\right)
\end{aligned}
$$



类似式(7.14)采用属性条件独立性假设，则


$$
P(x_1,...,x_{i-1},x_{i+1},...,x_d|c,x_i)=\prod_{j=1\\j\neq i}^{d}P(x_j|c,x_i)
$$


根据式(7.25)可知，当$j=i$时，$|D_{c,x_i}|=|D_{c,x_i,x_j}|$，若不考虑平滑项，则此时$P(x_j|c,x_i)=1$，因此在上式的连乘项中可放开$j\neq i$的约束，即


$$
P(x_1,...,x_{i-1},x_{i+1},...,x_d|c,x_i)=\prod_{j=1}^{d}P(x_j|c,x_i)
$$


综上可得： 

$$
\begin{aligned}
P(c|\boldsymbol{x})&=\frac{P(\boldsymbol{x},c)}{P(\boldsymbol{x})}\\ 
&=\frac{P\left(c, x_i\right)P\left(x_1, \ldots, x_{i-1}, x_{i+1}, \ldots, x_d \mid c, x_i\right)}{P(\boldsymbol{x})}\\
&\propto P\left(c, x_i\right)P\left(x_1, \ldots, x_{i-1}, x_{i+1}, \ldots, x_d \mid c, x_i\right) \\
&=P\left(c, x_i\right)\prod_{j=1}^{d}P(x_j|c,x_i)
\end{aligned}
$$



上式是将属性$x_i$作为超父属性的，AODE尝试将每个属性作为超父来构建SPODE，然后将那些具有足够训练数据支撑的SPODE集成起来作为最终结果。具体来说，对于总共$d$个属性来说，共有$d$个不同的上式，集成直接求和即可，因为对于不同的类别标记$c$均有$d$个不同的上式，至于如何满足"足够训练数据支撑的SPODE"这个条件，注意式(7.24)和式(7.25)均使用到了$|D_{c,x_i}|$和$|D_{c,x_i,x_j}|$，若集合$D_{x_i}$中样本数量过少，则$|D_{c,x_i}|$和$|D_{c,x_i,x_j}|$将会更小，因此在式(7.23)中要求集合$D_{x_i}$中样本数量不少于$m^{\prime}$。

### 7.4.4 式(7.24)和式(7.25)的推导

类比式(7.19)和式(7.20)的推导。

## 7.5 贝叶斯网

### 7.5.1 式(7.27)的解释

在这里补充一下同父结构和顺序结构的推导。同父结构：在给定父节点$x_1$的条件下$x_3,x_4$独立


$$
\begin{aligned} 
P(x_3,x_4|x_1)&=\frac{P(x_1,x_3,x_4)}{P(x_1)} \\
&=\frac{P(x_1)P(x_3|x_1)P(x_4|x_1)}{P(x_1)} \\
&=P(x_3|x_1)P(x_4|x_1) \\
\end{aligned}
$$

 顺序结构：在给定节点$x$的条件下$y,z$独立


$$
\begin{aligned} 
P(y,z|x)&=\frac{P(x,y,z)}{P(x)} \\
&=\frac{P(z)P(x|z)P(y|x)}{P(x)} \\
&=\frac{P(z,x)P(y|x)}{P(x)} \\
&=P(z|x)P(y|x) \\
\end{aligned}
$$



## 7.6 EM算法

"西瓜书"中仅给出了EM算法的运算步骤，其原理并未展开讲解，下面补充EM算法的推导原理，以及所用到的相关数学知识。

### 7.6.1 Jensen不等式

若$f$是凸函数，则下式恒成立


$$
f\left(t x_1 + (1-t)x_2\right)\leqslant tf(x_1)+(1-t)f(x_2)
$$


其中$t\in [0,1]$，若将$x$推广到$n$个时同样成立，即


$$
f(t_1 x_1 + t_2x_2+...+t_nx_n)\leqslant t_1f(x_1)+t_2f(x_2)+...+t_nf(t_n)
$$


其中$t_1,t_2,...,t_n\in[0,1],\sum_{i=1}^{n}t_i=1$。此不等式在概率论中通常以如下形式出现


$$
\varphi(\mathbb{E}[X])\leqslant \mathbb{E}[\varphi(X)]
$$


其中$X$是随机变量，$\varphi$为凸函数，$\mathbb{E}[X]$为随机变量$X$的期望。显然，若$f$和$\varphi$是凹函数，则上述不等式中的$\leqslant$换成$\geqslant$也恒成立。

### 7.6.2 EM算法的推导

假设现有一批独立同分布的样本$\{x_1,x_2,...,x_m\}$，它们是由某个含有隐变量的概率分布$p(x,z;\theta)$生成，现尝试用极大似然估计法估计此概率分布的参数。为了便于讨论，此处假设$z$为离散型随机变量，则对数似然函数为


$$
\begin{aligned} 
LL(\theta) &=\sum_{i=1}^{m} \ln p(x_i; \theta) \\ 
&=\sum_{i=1}^{m} \ln \sum_{z_i} p(x_i, z_i; \theta) 
\end{aligned}
$$


显然，此时$LL(\theta)$里含有未知的隐变量$z$以及求和项的对数，相比于不含隐变量的对数似然函数，显然该似然函数的极大值点较难求解，而EM算法则给出了一种迭代的方法来完成对$LL(\theta)$的极大化。

下面给出两种推导方法，一个是出自李航老师的《统计学习方法》[2]，一个是出自吴恩达老师的CS229，两种推导方式虽然形式上有差异，但最终的$Q$函数相等，接下来先讲述两种推导方法，最后会给出$Q$函数是相等的证明。

首先给出《统计学习方法》中的推导方法，设$X=\{x_1,x_2,...,x_m\},Z=\{z_1,z_2,...,z_m\}$，则对数似然函数可以改写为


$$
\begin{aligned} 
LL(\theta)&=\ln P(X\vert \theta)\\
&=\ln \sum_Z P(X,Z\vert\theta)\\
&=\ln \left(\sum_Z P(X\vert Z,\theta)P(Z\vert \theta)\right)
\end{aligned}
$$


EM算法采用的是通过迭代逐步近似极大化$L(\theta)$：假设第$t$次迭代时$\theta$的估计值是$\theta^{(t)}$，我们希望第$t+1$次迭代时的$\theta$能使$LL(\theta)$增大，即$LL(\theta)>LL(\theta^{(t)})$。为此，考虑两者的差


$$
\begin{aligned}
LL(\theta)-LL(\theta^{(t)})&=\ln \left(\sum_Z P(X\vert Z,\theta)P(Z\vert \theta)\right)-\ln P(X\vert\theta^{(t)}) \\
&=\ln \left(\sum_Z P(Z\vert X,\theta^{(t)}) \cfrac{P(X\vert Z,\theta)P(Z\vert \theta)}{P(Z\vert X,\theta^{(t)})}\right)-\ln P(X\vert\theta^{(t)})
\end{aligned}
$$

 由上述Jensen不等式可得 

$$
\begin{aligned}
LL(\theta)-LL(\theta^{(t)})
&\geqslant \sum_Z P(Z\vert X,\theta^{(t)})\ln \cfrac{P(X\vert Z,\theta)P(Z\vert \theta)}{P(Z\vert X,\theta^{(t)})}-\ln P(X\vert\theta^{(t)}) \\
&= \sum_Z P(Z\vert X,\theta^{(t)})\ln \cfrac{P(X\vert Z,\theta)P(Z\vert \theta)}{P(Z\vert X,\theta^{(t)})}-1\cdot \ln P(X\vert\theta^{(t)}) \\
&= \sum_Z P(Z\vert X,\theta^{(t)})\ln \cfrac{P(X\vert Z,\theta)P(Z\vert \theta)}{P(Z\vert X,\theta^{(t)})}-\sum_Z P(Z\vert X,\theta^{(t)})\cdot \ln P(X\vert\theta^{(t)}) \\
&=\sum_Z P(Z\vert X,\theta^{(t)}) \left( \ln \cfrac{P(X\vert Z,\theta)P(Z\vert \theta)}{P(Z\vert X,\theta^{(t)})} - \ln P(X\vert\theta^{(t)}) \right)\\
&= \sum_Z P(Z\vert X,\theta^{(t)})\ln \cfrac{P(X\vert Z,\theta)P(Z\vert \theta)}{P(Z\vert X,\theta^{(t)})P(X\vert\theta^{(t)})}
\end{aligned}
$$

 令


$$
B(\theta,\theta^{(t)})=LL(\theta^{(t)})+\sum_Z P(Z\vert X,\theta^{(t)})\ln \cfrac{P(X\vert Z,\theta)P(Z\vert \theta)}{P(Z\vert X,\theta^{(t)})P(X\vert\theta^{(t)})}
$$


则 

$$
LL(\theta)\geqslant B(\theta,\theta^{(t)})
$$


即$B(\theta,\theta^{(t)})$是$LL(\theta)$的下界，此时若设$\theta^{(t+1)}$能使得$B(\theta,\theta^{(t)})$达到极大，即


$$
B(\theta^{(t+1)},\theta^{(t)}) \geqslant B(\theta,\theta^{(t)})
$$


由于$LL(\theta^{(t)})=B(\theta^{(t)},\theta^{(t)})$，那么可以进一步推得


$$
LL(\theta^{(t+1)})\geqslant B(\theta^{(t+1)},\theta^{(t)})\geqslant B(\theta^{(t)},\theta^{(t)})=LL(\theta^{(t)})
$$




$$
LL(\theta^{(t+1)})\geqslant LL(\theta^{(t)})
$$


因此，任何能使得$B(\theta,\theta^{(t)})$增大的$\theta$，也可以使得$LL(\theta)$增大，于是问题就转化为了求解能使得$B(\theta,\theta^{(t)})$达到极大的$\theta^{(t+1)}$，即


$$
\begin{aligned}
\theta^{(t+1)}&=\mathop{\arg\max}_{\theta}B(\theta,\theta^{(t)}) \\
&=\mathop{\arg\max}_{\theta}\left( LL(\theta^{(t)})+\sum_Z P(Z\vert X,\theta^{(t)})\ln \cfrac{P(X\vert Z,\theta)P(Z\vert \theta)}{P(Z\vert X,\theta^{(t)})P(X\vert\theta^{(t)})}\right)
\end{aligned}
$$

 略去对$\theta$极大化而言是常数的项 

$$
\begin{aligned}
\theta^{(t+1)}&=\mathop{\arg\max}_{\theta}\left(\sum_Z P(Z\vert X,\theta^{(t)})\ln\left( P(X\vert Z,\theta)P(Z\vert \theta)\right)\right) \\
&=\mathop{\arg\max}_{\theta}\left(\sum_Z P(Z\vert X,\theta^{(t)})\ln P(X,Z\vert \theta)\right) \\
&=\mathop{\arg\max}_{\theta}Q(\theta,\theta^{(t)})
\end{aligned}
$$


到此即完成了EM算法的一次迭代，求出的$\theta^{(t+1)}$作为下一次迭代的初始$\theta^{(t)}$。综上，EM算法的"E步"和"M步"可总结为以下两步。

E步：计算完全数据的对数似然函数$\ln P(X,Z\vert \theta)$关于在给定观测数据$X$和当前参数$\theta^{(t)}$下对未观测数据$Z$的条件概率分布$P(Z\vert X,\theta^{(t)})$的期望$Q(\theta,\theta^{(t)})$：


$$
Q(\theta,\theta^{(t)})=\mathbb{E}_Z[\ln P(X,Z\vert \theta)\vert X,\theta^{(t)}]=\sum_Z P(Z\vert X,\theta^{(t)})\ln P(X,Z\vert \theta)
$$



M步：求使得$Q(\theta,\theta^{(t)})$达到极大的$\theta^{(t+1)}$。

接下来给出CS229中的推导方法，设$z_i$的概率质量函数为$Q_i(z_i)$，则$LL(\theta)$可以作如下恒等变形


$$
\begin{aligned} 
LL(\theta) &=\sum_{i=1}^{m} \ln p(x_i; \theta) \\ 
&=\sum_{i=1}^{m} \ln \sum_{z_i} p(x_i, z_i; \theta) \\
&=\sum_{i=1}^{m} \ln \sum_{z_i} Q_i(z_i)\cfrac{p(x_i, z_i; \theta)}{Q_i(z_i)} \\
\end{aligned}
$$


其中$\sum_{z_i} Q_i(z_i)\cfrac{p(x_i, z_i; \theta)}{Q_i(z_i)}$可以看做是对$\cfrac{p(x_i, z_i; \theta)}{Q_i(z_i)}$关于$z_i$求期望，即


$$
\sum_{z_i} Q_i(z_i)\cfrac{p(x_i, z_i; \theta)}{Q_i(z_i)}=\mathbb{E}_{z_i}\left[\cfrac{p(x_i, z_i; \theta)}{Q_i(z_i)}\right]
$$


由Jensen不等式可得


$$
\ln\left(\mathbb{E}_{z_i}\left[\cfrac{p(x_i, z_i; \theta)}{Q_i(z_i)}\right]\right)\geqslant \mathbb{E}_{z_i}\left[\ln\left(\cfrac{p(x_i, z_i; \theta)}{Q_i(z_i)}\right)\right]
$$




$$
\ln\sum_{z_i} Q_i(z_i)\cfrac{p(x_i, z_i; \theta)}{Q_i(z_i)}\geqslant \sum_{z_i} Q_i(z_i)\ln\cfrac{p(x_i, z_i; \theta)}{Q_i(z_i)}
$$


将此式代入$LL(\theta)$可得 

$$
\begin{aligned} 
LL(\theta) &=\sum_{i=1}^{m} \ln \sum_{z_i} Q_i(z_i)\cfrac{p(x_i, z_i; \theta)}{Q_i(z_i)}\geqslant \sum_{i=1}^{m}\sum_{z_i} Q_i(z_i)\ln\cfrac{p(x_i, z_i; \theta)}{Q_i(z_i)} \quad \textcircled{1}
\end{aligned}
$$


若令$B(\theta)=\sum\limits_{i=1}^{m}\sum\limits_{z_i} Q_i(z_i)\ln\cfrac{p(x_i, z_i; \theta)}{Q_i(z_i)}$，则此时$B(\theta)$为$LL(\theta)$的下界函数，那么这个下界函数所能构成的最优下界是多少？即$B(\theta)$的最大值是多少？显然，$B(\theta)$是$LL(\theta)$的下界函数，反过来$LL(\theta)$是其上界函数，所以如果能使得$B(\theta)=LL(\theta)$，则此时的$B(\theta)$就取到了最大值。根据Jensen不等式的性质可知，如果能使得$\cfrac{p(x_i, z_i; \theta)}{Q_i(z_i)}$恒等于某个常量$c$，大于等于号便可以取到等号。因此，只需任意选取满足$\cfrac{p(x_i, z_i; \theta)}{Q_i(z_i)}=c$的$Q_i(z_i)$就能使得$B(\theta)$达到最大值。由于$Q_i(z_i)$是$z_i$的概率质量函数，所以$Q_i(z_i)$同时也满足约束$0\leqslant Q_i(z_i)\leqslant 1,\sum_{z_i} Q_i(z_i)=1$，结合$Q_i(z_i)$的所有约束可以推得


$$
\cfrac{p(x_i, z_i; \theta)}{Q_i(z_i)}=c
$$




$$
p(x_i, z_i; \theta)=c\cdot Q_i(z_i)
$$




$$
\sum_{z_i}p(x_i, z_i; \theta)=c\cdot \sum_{z_i}Q_i(z_i)
$$




$$
\sum_{z_i}p(x_i, z_i; \theta)=c
$$




$$
\cfrac{p(x_i, z_i; \theta)}{Q_i(z_i)}=\sum_{z_i}p(x_i, z_i; \theta)
$$




$$
Q_i(z_i)=\cfrac{p(x_i, z_i; \theta)}{\sum\limits_{z_i}p(x_i, z_i; \theta)}=\cfrac{p(x_i, z_i; \theta)}{p(x_i; \theta)}=p(z_i|x_i; \theta)
$$


所以，当且仅当$Q_i(z_i)=p(z_i|x_i; \theta)$时$B(\theta)$取到最大值，将$Q_i(z_i)=p(z_i|x_i; \theta)$代回$LL(\theta)$和$B(\theta)$可以推得


$$
\begin{aligned} 
LL(\theta) &=\sum_{i=1}^{m} \ln \sum_{z_i} Q_i(z_i)\cfrac{p(x_i, z_i; \theta)}{Q_i(z_i)} & \quad \textcircled{2}\\
&=\sum_{i=1}^{m} \ln \sum_{z_i}p(z_i|x_i; \theta)\cfrac{p(x_i, z_i; \theta)}{p(z_i|x_i; \theta)} & \quad \textcircled{3}\\
&=\sum_{i=1}^{m}\sum_{z_i} p(z_i|x_i; \theta)\ln\cfrac{p(x_i, z_i; \theta)}{p(z_i|x_i; \theta)} & \quad \textcircled{4}\\
&=\max\{B(\theta)\} & \quad \textcircled{5} \\
\end{aligned}
$$


其中式$\textcircled{4}$是式$\textcircled{1}$中不等式取等号时的情形。由以上推导可知，此时对数似然函数$LL(\theta)$等价于其下界函数的最大值$\max\{B(\theta)\}$，所以要想极大化$LL(\theta)$可以通过极大化$\max\{B(\theta)\}$来间接极大化$LL(\theta)$，因此，下面考虑如何极大化$\max\{B(\theta)\}$。假设已知第$t$次迭代的参数为$\theta^{(t)}$，而第$t+1$次迭代的参数$\theta^{(t+1)}$可通过如下方式求得


$$
\begin{aligned} 
\theta^{(t+1)}&=\arg\max_{\theta}\max\{B(\theta)\}  & \quad \textcircled{6}\\
&=\arg\max_{\theta}\sum_{i=1}^{m}\sum_{z_i} p(z_i|x_i;\theta^{(t)})\ln\cfrac{p(x_i, z_i; \theta)}{p(z_i|x_i; \theta^{(t)})}  & \quad \textcircled{7}\\
&=\arg\max_{\theta}\sum_{i=1}^{m}\sum_{z_i} p(z_i|x_i;\theta^{(t)})\ln p(x_i, z_i; \theta) & \quad \textcircled{8}
\end{aligned}
$$

 此时将$\theta^{(t+1)}$代入$LL(\theta)$可推得


$$
\begin{aligned} 
LL(\theta^{(t+1)}) &=\max\{B(\theta^{(t+1)})\} &\quad\textcircled{9} \\
&=\sum_{i=1}^{m}\sum_{z_i} p(z_i|x_i; \theta^{(t+1)})\ln\cfrac{p(x_i, z_i; \theta^{(t+1)})}{p(z_i|x_i; \theta^{(t+1)})} &\quad\textcircled{10}\\
&\geqslant \sum_{i=1}^{m}\sum_{z_i} p(z_i|x_i; \theta^{(t)})\ln\cfrac{p(x_i, z_i; \theta^{(t+1)})}{p(z_i|x_i; \theta^{(t)})} &\quad\textcircled{11}\\
&\geqslant \sum_{i=1}^{m}\sum_{z_i} p(z_i|x_i; \theta^{(t)})\ln\cfrac{p(x_i, z_i; \theta^{(t)})}{p(z_i|x_i; \theta^{(t)})} &\quad\textcircled{12}\\
&=\max\{B(\theta^{(t)})\} &\quad\textcircled{13} \\
&=LL(\theta^{(t)})&\quad\textcircled{14}
\end{aligned}
$$


其中，式$\textcircled{9}$和式$\textcircled{10}$分别由式$\textcircled{5}$和式$\textcircled{4}$推得，式$\textcircled{11}$由式$\textcircled{1}$推得，式$\textcircled{12}$由式$\textcircled{7}$推得，式$\textcircled{13}$和式$\textcircled{14}$由式$\textcircled{2}$至式$\textcircled{5}$推得。此时若令


$$
Q(\theta,\theta^{(t)})=\sum_{i=1}^{m}\sum_{z_i} p(z_i|x_i; \theta^{(t)})\ln p(x_i, z_i; \theta)
$$


由式$\textcircled{9}$至式$\textcircled{14}$可知，凡是能使得$Q(\theta,\theta^{(t)})$达到极大的$\theta^{(t+1)}$一定能使得$LL(\theta^{(t+1)})\geqslant LL(\theta^{(t)})$。综上，EM算法的"E步"和"M步"可总结为以下两步。

E步：令$Q_i(z_i)=p(z_i|x_i; \theta)$并写出$Q(\theta,\theta^{(t)})$；

M步：求使得$Q(\theta,\theta^{(t)})$到达极大的$\theta^{(t+1)}$。

以上便是EM算法的两种推导方法，下面证明两种推导方法中的$Q$函数相等。


$$
\begin{aligned} Q(\theta|\theta^{(t)})&=\sum_Z P(Z|X,\theta^{(t)})\ln P(X,Z|\theta) \\
&=\sum_{z_1,z_2,...,z_m}\left\{\prod_{i=1}^mP(z_i|x_i,\theta^{(t)})\ln\left[ \prod_{i=1}^m P(x_i,z_i|\theta) \right] \right\} \\
&=\sum_{z_1,z_2,...,z_m}\left\{\prod_{i=1}^mP(z_i|x_i,\theta^{(t)})\left[ \sum_{i=1}^m\ln P(x_i,z_i|\theta) \right] \right\} \\
&=\sum_{z_1,z_2,...,z_m}\left\{\prod_{i=1}^mP(z_i|x_i,\theta^{(t)})\left[\ln P(x_1,z_1|\theta) + \ln P(x_2,z_2|\theta) +...+ \ln P(x_m,z_m|\theta)\right] \right\} \\
&=\sum_{z_1,z_2,...,z_m}\left[\prod_{i=1}^mP(z_i|x_i,\theta^{(t)})\cdot\ln P(x_1,z_1|\theta) \right]+...+\sum_{z_1,z_2,...,z_m}\left[\prod_{i=1}^mP(z_i|x_i,\theta^{(t)})\cdot\ln P(x_m,z_m|\theta) \right]  \\
\end{aligned}
$$


其中$\sum\limits_{z_1,z_2,...,z_m}\left[\prod\limits_{i=1}^mP(z_i|x_i,\theta^{(t)})\cdot\ln P(x_1,z_1|\theta) \right]$可作如下恒等变形：


$$
\begin{aligned} 
&\sum\limits_{z_1,z_2,...,z_m}\left[\prod\limits_{i=1}^mP(z_i|x_i,\theta^{(t)})\cdot\ln P(x_1,z_1|\theta) \right] \\
=&\sum\limits_{z_1,z_2,...,z_m}\left[\prod_{i=2}^mP(z_i|x_i,\theta^{(t)})\cdot P(z_1|x_1,\theta^{(t)})\cdot\ln P(x_1,z_1|\theta) \right] \\
=&\sum_{z_1}\sum\limits_{z_2,...,z_m}\left[\prod_{i=2}^mP(z_i|x_i,\theta^{(t)})\cdot P(z_1|x_1,\theta^{(t)})\cdot\ln P(x_1,z_1|\theta) \right] \\
=&\sum_{z_1}P(z_1|x_1,\theta^{(t)})\ln P(x_1,z_1|\theta) \sum\limits_{z_2,...,z_m}\left[\prod_{i=2}^mP(z_i|x_i,\theta^{(t)}) \right] \\
=&\sum_{z_1}P(z_1|x_1,\theta^{(t)})\ln P(x_1,z_1|\theta)\sum\limits_{z_2,...,z_m}\left[\prod_{i=3}^mP(z_i|x_i,\theta^{(t)})\cdot P(z_2|x_2,\theta^{(t)}) \right] \\
=&\sum_{z_1}P(z_1|x_1,\theta^{(t)})\ln P(x_1,z_1|\theta) \left\{\sum\limits_{z_2}\sum\limits_{z_3,...,z_m}\left[\prod_{i=3}^mP(z_i|x_i,\theta^{(t)})\cdot P(z_2|x_2,\theta^{(t)}) \right]\right\} \\
=&\sum_{z_1}P(z_1|x_1,\theta^{(t)})\ln P(x_1,z_1|\theta) \left\{\sum_{z_2}P(z_2|x_2,\theta^{(t)}) \sum\limits_{z_3,...,z_m}\left[\prod_{i=3}^mP(z_i|x_i,\theta^{(t)})\right]\right\} \\
=&\sum_{z_1}P(z_1|x_1,\theta^{(t)})\ln P(x_1,z_1|\theta) \left\{\sum_{z_2}P(z_2|x_2,\theta^{(t)})\times\sum_{z_3}P(z_3|x_3,\theta^{(t)})\times...\times\sum_{z_m}P(z_m|x_m,\theta^{(t)})\right\} \\
=&\sum_{z_1}P(z_1|x_1,\theta^{(t)})\ln P(x_1,z_1|\theta)\times \left\{1\times1\times...\times1\right\} \\
=&\sum_{z_1}P(z_1|x_1,\theta^{(t)})\ln P(x_1,z_1|\theta)  \\
\end{aligned}
$$

 所以


$$
\sum\limits_{z_1,z_2,...,z_m}\left[\prod_{i=1}^mP(z_i|x_i,\theta^{(t)})\cdot\ln P(x_1,z_1|\theta) \right]=\sum_{z_1}P(z_1|x_1,\theta^{(t)})\ln P(x_1,z_1|\theta)
$$


同理可得 

$$
\begin{aligned} 
\sum\limits_{z_1,z_2,...,z_m}\left[\prod_{i=1}^mP(z_i|x_i,\theta^{(t)})\cdot\ln P(x_2,z_2|\theta) \right] &=\sum_{z_2}P(z_2|x_2,\theta^{(t)})\ln P(x_2,z_2|\theta) \\
&\vdots\\
\sum\limits_{z_1,z_2,...,z_m}\left[\prod_{i=1}^mP(z_i|x_i,\theta^{(t)})\cdot\ln P(x_m,z_m|\theta) \right] &=\sum_{z_m}P(z_m|x_m,\theta^{(t)})\ln P(x_m,z_m|\theta)
\end{aligned}
$$

 将上式代入$Q(\theta|\theta^{(t)})$可得


$$
\begin{aligned} 
Q(\theta|\theta^{(t)})&=\sum_{z_1,z_2,...,z_m}\left[\prod_{i=1}^mP(z_i|x_i,\theta^{(t)})\cdot\ln P(x_1,z_1|\theta) \right]+...+\sum_{z_1,z_2,...,z_m}\left[\prod_{i=1}^mP(z_i|x_i,\theta^{(t)})\cdot\ln P(x_m,z_m|\theta) \right]  \\
&=\sum_{z_1}P(z_1|x_1,\theta^{(t)})\ln P(x_1,z_1|\theta) +...+\sum_{z_m}P(z_m|x_m,\theta^{(t)})\ln P(x_m,z_m|\theta) \\
&=\sum_{i=1}^m\sum_{z_i}P(z_i|x_i,\theta^{(t)})\ln P(x_i,z_i|\theta)\\
\end{aligned}
$$


## 参考文献
[1] 陈希孺. 概率论与数理统计. 中国科学技术大学出版社, 2009.

[2] 李航. 统计学习方法. 清华大学出版社, 2012.
