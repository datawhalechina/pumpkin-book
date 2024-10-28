# 第16章 强化学习

强化学习作为机器学习的子领域，其本身拥有一套完整的理论体系，以及诸多经典和最新前沿算法，"西瓜书"该章内容仅可作为综述查阅，若想深究建议查阅其他相关书籍（例如《Easy
RL：强化学习教程》[1]）进行系统性学习。

## 16.1 任务与奖赏

本节理解强化学习的定义和相关术语的含义即可。

## 16.2 K-摇臂赌博机

### 16.2.1 式(16.2)和式(16.3)的推导

$$\begin{aligned}
Q_{n}(k)&=\frac{1}{n}\sum_{i=1}^{n}v_{i}\\
&=\frac{1}{n}\left(\sum_{i=1}^{n-1}v_{i}+v_{n}\right)\\
&=\frac{1}{n}\left((n-1)\times Q_{n-1}(k)+v_{n}\right)\\
&=Q_{n-1}(k)+\frac{1}{n}\left(v_n-Q_{n-1}(k)\right)
\end{aligned}$$

### 16.2.2 式(16.4)的解释

$$P(k)=\frac{e^{\frac{Q(k)}{\tau }}}{\sum_{i=1}^{K}e^{\frac{Q(i)}{\tau}}}\propto e^{\frac{Q(k)}{\tau }}\propto\frac{Q(k)}{\tau }\propto\frac{1}{\tau}$$

如果$\tau$很大，所有动作几乎以等概率选择（探索）；如果
$\tau$很小，$Q$值大的动作更容易被选中（利用）。

## 16.3 有模型学习

### 16.3.1 式(16.7)的解释

因为 

$$\pi(x,a)=P(action=a|state=x)$$

表示在状态$x$下选择动作$a$的概率，又因为动作事件之间两两互斥且和为动作空间，由全概率展开公式

$$P(A)=\sum_{i=1}^{\infty}P(B_{i})P(A\mid B_{i})$$

可得

$$\begin{aligned}
&\mathbb{E}_{\pi}[\frac{1}{T}r_{1}+\frac{T-1}{T}\frac{1}{T-1}\sum_{t=2}^{T}r_{t}\mid x_{0}=x]\\
&=\sum_{a\in A}\pi(x,a)\sum_{x{}'\in X}P_{x\rightarrow x{}'}^{a}(\frac{1}{T}R_{x\rightarrow x{}'}^{a}+\frac{T-1}{T}\mathbb{E}_{\pi}[\frac{1}{T-1}\sum_{t=1}^{T-1}r_{t}\mid x_{0}=x{}'])
\end{aligned}$$

其中

$$r_{1}=\pi(x,a)P_{x\rightarrow x{}'}^{a}R_{x\rightarrow x{}'}^{a}$$

最后一个等式用到了递归形式。

Bellman等式定义了当前状态与未来状态之间的关系，表示当前状态的价值函数可以通过下个状态的价值函数来计算。

### 16.3.2 式(16.8)的推导

$$\begin{aligned}
V_{\gamma }^{\pi}(x)&=\mathbb{E}_{\pi}[\sum_{t=0}^{\infty }\gamma^{t}r_{t+1}\mid x_{0}=x]\\
&=\mathbb{E}_{\pi}[r_{1}+\sum_{t=1}^{\infty}\gamma^{t}r_{t+1}\mid x_{0}=x]\\
&=\mathbb{E}_{\pi}[r_{1}+\gamma\sum_{t=1}^{\infty}\gamma^{t-1}r_{t+1}\mid x_{0}=x]\\
&=\sum _{a\in A}\pi(x,a)\sum_{x{}'\in X}P_{x\rightarrow x{}'}^{a}(R_{x\rightarrow x{}'}^{a}+\gamma \mathbb{E}_{\pi}[\sum_{t=0}^{\infty }\gamma^{t}r_{t+1}\mid x_{0}=x{}'])\\
&=\sum _{a\in A}\pi(x,a)\sum_{x{}'\in X}P_{x\rightarrow x{}'}^{a}(R_{x\rightarrow x{}'}^{a}+\gamma V_{\gamma }^{\pi}(x{}'))
\end{aligned}$$

### 16.3.3 式(16.10)的推导

参见式(16.7)和式(16.8)的推导

### 16.3.4 式(16.14)的解释

为了获得最优的状态值函数$V$，这里取了两层最优，分别是采用最优策略$\pi^{*}$和选取使得状态动作值函数$Q$最大的动作$\max_{a\in A}$。

### 16.3.5 式(16.15)的解释

最优 Bellman等式表明：最佳策略下的一个状态的价值必须等于在这个状态下采取最好动作得到的累积奖赏值的期望。

### 16.3.6 式(16.16)的推导

$$\begin{aligned}
V^{\pi}(x) & \leqslant Q^{\pi}\left(x, \pi^{\prime}(x)\right) \\
&=\sum_{x^{\prime} \in X} P_{x \rightarrow x^{\prime}}^{\pi^{\prime}(x)}\left(R_{x \rightarrow x^{\prime}}^{\pi^{\prime}(x)}+\gamma V^{\pi}\left(x^{\prime}\right)\right) \\
& \leqslant \sum_{x^{\prime} \in X} P_{x \rightarrow x^{\prime}}^{\pi^{\prime}(x)}\left(R_{x \rightarrow x^{\prime}}^{\pi^{\prime}(x)}+\gamma Q^{\pi}\left(x^{\prime}, \pi^{\prime}\left(x^{\prime}\right)\right)\right) \\
&= \sum_{x^{\prime} \in X} P_{x \rightarrow x^{\prime}}^{\pi^{\prime}(x)}\left(R_{x \rightarrow x^{\prime}}^{\pi^{\prime}(x)}+
\sum_{x'^{\prime} \in X} P_{x' \rightarrow x^{''}}^{\pi^{\prime}(x')}\left(\gamma R_{x' \rightarrow x^{\prime \prime}}^{\pi^{\prime}(x')}+
\gamma^2 V^{\pi}\left(x^{\prime \prime}\right)\right)\right)\\
& \leqslant \sum_{x^{\prime} \in X} P_{x \rightarrow x^{\prime}}^{\pi^{\prime}(x)}\left(R_{x \rightarrow x^{\prime}}^{\pi^{\prime}(x)}+ \sum_{x'^{\prime} \in X} P_{x' \rightarrow x^{''}}^{\pi^{\prime}(x')} \left( \gamma R_{x' \rightarrow x^{\prime \prime}}^{\pi^{\prime}(x')} +
\gamma^2 Q^{\pi}\left(x^{\prime \prime}, \pi^{\prime }\left(x^{\prime \prime}\right)\right)\right)\right) \\
&\leqslant \cdots \\
&\leqslant \sum_{x^{\prime} \in X} P_{x \rightarrow x^{\prime}}^{\pi^{\prime}(x)}\left(R_{x \rightarrow x^{\prime}}^{\pi^{\prime}(x)}+\sum_{x'^{\prime} \in X} P_{x' \rightarrow x^{''}}^{\pi^{\prime}(x')}\left(\gamma R_{x' \rightarrow x^{\prime \prime}}^{\pi^{\prime}(x')}+\sum_{x'^{\prime} \in X} P_{x'' \rightarrow x^{'''}}^{\pi^{\prime}(x'')} \left(\gamma^2 R_{x'' \rightarrow x^{\prime \prime \prime}}^{\pi^{\prime}(x'')}+\cdots \right)\right)\right) \\
&= V^{\pi'}(x) 
\end{aligned}$$

其中，使用了动作改变条件

$$Q^{\pi}(x,\pi{}'(x))\geqslant V^{\pi}(x)$$

以及状态-动作值函数

$$Q^{\pi}(x{}',\pi{}'(x{}'))=\sum_{x{}'\in X}P_{x{}'\rightarrow x{}'}^{\pi{}'(x{}')}(R_{x{}'\rightarrow x{}'}^{\pi{}'(x{}')}+\gamma V^{\pi}(x{}'))$$

于是，当前状态的最优值函数为

$$V^{\ast}(x)=V^{\pi{}'}(x)\geqslant V^{\pi}(x)$$

## 16.4 免模型学习

### 16.4.1 式(16.20)的解释

如果 $\epsilon_k=\frac{1}{k}$，并且其值随 $k$ 增大而主角趋于零，则$\epsilon-$ 贪心是在无限的探索中的极限贪心（Greedy in the Limit with Infinite Exploration，简称GLIE）。

### 16.4.2 式(16.23)的解释

$\frac{p(x)}{q(x)}$称为重要性权重（Importance
Weight），其用于修正两个分布的差异。

### 16.4.3 式(16.31)的推导

对比公式16.29

$$Q_{t+1}^{\pi}(x,a)=Q_{t}^{\pi}(x,a)+\frac{1}{t+1}(r_{t+1}-Q_{t}^{\pi}(x,a))$$

以及由

$$\frac{1}{t+1}=\alpha$$

可知，若下式成立，则公式16.31成立

$$r_{t+1}=R_{x\rightarrow x{}'}^{a}+\gamma Q_{t}^{\pi}(x{}',a{}')$$

而$r_{t+1}$表示$t+1$步的奖赏，即状态$x$变化到$x'$的奖赏加上前面$t$步奖赏总和$Q_{t}^{\pi}(x{}',a{}')$的$\gamma$折扣，因此这个式子成立。

## 16.5 值函数近似

### 16.5.1 式(16.33)的解释

古代汉语中"平方"称为"二乘"，此处的最小二乘误差也就是均方误差。

### 16.5.2 式(16.34)的推导

$$\begin{aligned}
-\frac{\partial E_{\boldsymbol{\theta}}}{\partial \boldsymbol{\theta}} & = -\frac{\partial \mathbb{E}_{\boldsymbol{x} \sim \pi}\left[\left(V^\pi(\boldsymbol{x})-V_{\boldsymbol{\theta}}(\boldsymbol{x})\right)^2\right]}{\partial \boldsymbol{\theta}}\\
\end{aligned}$$

将$V^\pi(\boldsymbol{x})-V_{\boldsymbol{\theta}}(\boldsymbol{x})$
看成一个整体，根据链式法则（chain rule）可知

$$-\frac{\partial \mathbb{E}_{\boldsymbol{x} \sim \pi}\left[\left(V^\pi(\boldsymbol{x})-V_{\boldsymbol{\theta}}(\boldsymbol{x})\right)^2\right]}{\partial \boldsymbol{\theta}}=\mathbb{E}_{\boldsymbol{x} \sim \pi}\left[2\left(V^\pi(\boldsymbol{x})-V_{\boldsymbol{\theta}}(\boldsymbol{x})\right) \frac{\partial V_{\boldsymbol{\theta}}(\boldsymbol{x})}{\partial \boldsymbol{\theta}}\right]$$

$V_{\boldsymbol{\theta}}(\boldsymbol{x})$
是一个标量，$\boldsymbol{\theta}$
是一个向量，$\frac{\partial V_{\boldsymbol{\theta}}(\boldsymbol{x})}{\partial \boldsymbol{\theta}}$
属于矩阵微积分中的标量对向量求偏导，因此

$$\begin{aligned}
\frac{\partial V_{\boldsymbol{\theta}}(\boldsymbol{x})}{\partial \boldsymbol{\theta}}&=
\frac{\partial \boldsymbol{\theta}^\mathrm{T}{\boldsymbol{x}}}{\partial \boldsymbol{\theta}} \\
& =\left[\frac{\partial \boldsymbol{\theta}^{\mathrm{T}} \boldsymbol{x}}{\partial \theta_1}, \frac{\partial \boldsymbol{\theta}^{\mathrm{T}} \boldsymbol{x}}{\partial \theta_2}, \cdots,\frac{\partial \boldsymbol{\theta}^{\mathrm{T}}\boldsymbol{x}}{\partial \theta_n}\right]^{\mathrm{T}} \\
& =\left[x_1, x_2, \cdots,x_m\right]^{\mathrm{T}} \\
& =\boldsymbol{x}
\end{aligned}$$

故

$$\begin{aligned}
-\frac{\partial E_{\boldsymbol{\theta}}}{\partial \boldsymbol{\theta}} & =\mathbb{E}_{\boldsymbol{x} \sim \pi}\left[2\left(V^\pi(\boldsymbol{x})-V_{\boldsymbol{\theta}}(\boldsymbol{x})\right) \frac{\partial V_{\boldsymbol{\theta}}(\boldsymbol{x})}{\partial \boldsymbol{\theta}}\right] \\
& =\mathbb{E}_{\boldsymbol{x} \sim \pi}\left[2\left(V^\pi(\boldsymbol{x})-V_{\boldsymbol{\theta}}(\boldsymbol{x})\right) \boldsymbol{x}\right]
\end{aligned}$$

## 参考文献
[1] 王琦，杨毅远，江季. Easy RL：强化学习教程. 人民邮电出版社, 2022.
