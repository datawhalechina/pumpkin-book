```{important}
参与组队学习的同学须知：

本章学习时间：3天

本章配套视频教程：

支持向量机：https://www.bilibili.com/video/BV1Mh411e7VU?p=9

软间隔与支持向量回归：https://www.bilibili.com/video/BV1Mh411e7VU?p=10
```

# 第6章 支持向量机

在深度学习流行之前，支持向量机及其核方法一直是机器学习领域中的主流算法，尤其是核方法至今都仍有相关学者在持续研究。

## 6.1 间隔与支持向量

### 6.1.1 图6.1的解释

回顾第5章5.2节的感知机模型可知，图6.1中的黑色直线均可作为感知机模型的解，因为感知机模型求解的是能将正负样本完全正确划分的超平面，因此解不唯一。而支持向量机想要求解的则是离正负样本都尽可能远且刚好位于"正中间"的划分超平面，因为这样的超平面理论上泛化性能更好。

### 6.1.2 式(6.1)的解释

$n$维空间的超平面定义为$\boldsymbol{w}^\mathrm{T}\boldsymbol{x}+b=0$，其中$\boldsymbol{w},\boldsymbol{x}\in\mathbb{R}^{n}$，$\boldsymbol{w}=(w_1;w_2;...;w_n)$称为法向量，$b$称为位移项。超平面具有以下性质：

(1)法向量$\boldsymbol{w}$和位移项$b$确定一个唯一超平面；

(2)超平面方程不唯一，因为当等倍缩放$\boldsymbol{w}$和$b$时（假设缩放倍数为$\alpha$），所得的新超平面方程$\alpha\boldsymbol{w}^\mathrm{T}\boldsymbol{x}+\alpha b=0$和$\boldsymbol{w}^\mathrm{T}\boldsymbol{x}+b=0$的解完全相同，因此超平面不变，仅超平面方程有变；

(3)法向量$\boldsymbol{w}$垂直于超平面；

(4)超平面将$n$维空间切割为两半，其中法向量$\boldsymbol{w}$指向的那一半空间称为正空间，另一半称为负空间，正空间中的点$\boldsymbol{x}^{+}$代入进方程$\boldsymbol{w}^\mathrm{T}\boldsymbol{x}^{+}+b$其计算结果大于0，反之负空间中的点代入进方程其计算结果小于0；

(5)$n$维空间中的任意点$\boldsymbol{x}$到超平面的距离公式为$r=\frac{\left|\boldsymbol{w}^{\mathrm{T}}\boldsymbol{x}+b\right|}{\|\boldsymbol{w}\|}$，其中$\|\boldsymbol{w}\|$表示向量$\boldsymbol{w}$的模。

### 6.1.3 式(6.2)的推导

对于任意一点$\boldsymbol{x}_0=(x^0_1;x^0_2;...;x^0_n)$，设其在超平面$\boldsymbol{w}^{\mathrm{T}} \boldsymbol{x}+b=0$上的投影点为$\boldsymbol{x}_1=(x^1_1;x^1_2;...;x^1_n)$，则$\boldsymbol{w}^{\mathrm{T}} \boldsymbol{x}_1+b=0$。根据超平面的性质(3)可知，此时向量$\overrightarrow{\boldsymbol{x}_1\boldsymbol{x}_0}$与法向量$\boldsymbol{w}$平行，因此


$$
|\boldsymbol{w}\cdot\overrightarrow{\boldsymbol{x}_1\boldsymbol{x}_0}|=|\|\boldsymbol{w}\|\cdot\cos\pi\cdot\|\overrightarrow{\boldsymbol{x}_1\boldsymbol{x}_0}\||=\|\boldsymbol{w}\|\cdot \|\overrightarrow{\boldsymbol{x}_1\boldsymbol{x}_0}\|=\|\boldsymbol{w}\|\cdot r
$$


又 

$$
\begin{aligned}
\boldsymbol{w}\cdot\overrightarrow{\boldsymbol{x}_1\boldsymbol{x}_0}&=w_1(x^0_1-x^1_1)+w_2(x^0_2-x^1_2)+...+w_n(x^0_n-x^1_n) \\
&=w_1x^0_1+w_2x^0_2+...+w_nx^0_n-(w_1x^1_1+w_2x^1_2+...+w_nx^1_n) \\
&=\boldsymbol{w}^{\mathrm{T}}\boldsymbol{x}_0-\boldsymbol{w}^{\mathrm{T}}\boldsymbol{x}_1 \\
&=\boldsymbol{w}^{\mathrm{T}}\boldsymbol{x}_0+b\\
\end{aligned}
$$

 所以


$$
|\boldsymbol{w}^{\mathrm{T}}\boldsymbol{x}_0+b|=\|\boldsymbol{w}\|\cdot r
$$




$$
r=\frac{\left|\boldsymbol{w}^{\mathrm{T}} \boldsymbol{x}+b\right|}{\|\boldsymbol{w}\|}
$$



### 6.1.4 式(6.3)的推导

支持向量机所要求的超平面需要满足三个条件，第一个是能正确划分正负样本，第二个是要位于正负样本正中间，第三个是离正负样本都尽可能远。式(6.3)仅满足前两个条件，第三个条件由式(6.5)来满足，因此下面仅基于前两个条件来进行推导。

对于第一个条件，当超平面满足该条件时，根据超平面的性质(4)可知，若$y_i=+1$的正样本被划分到正空间（当然也可以将其划分到负空间），$y_i=-1$的负样本被划分到负空间，以下不等式成立


$$
\left\{\begin{array}{ll}
\boldsymbol{w}^{\mathrm{T}} \boldsymbol{x}_i+b \geqslant 0, & y_i=+1 \\
\boldsymbol{w}^{\mathrm{T}} \boldsymbol{x}_i+b \leqslant 0, & y_i=-1
\end{array}\right.
$$



对于第二个条件，首先设离超平面最近的正样本为$\boldsymbol{x}^{+}_{*}$，离超平面最近的负样本为$\boldsymbol{x}^{-}_{*}$，由于这两样本是离超平面最近的点，所以其他样本到超平面的距离均大于等于它们，即


$$
\left\{\begin{array}{ll}
\frac{\left|\boldsymbol{w}^{\mathrm{T}}\boldsymbol{x}_i+b\right|}{\|\boldsymbol{w}\|} \geqslant \frac{\left|\boldsymbol{w}^{\mathrm{T}}\boldsymbol{x}^{+}_{*}+b\right|}{\|\boldsymbol{w}\|}, & y_i=+1 \\
\frac{\left|\boldsymbol{w}^{\mathrm{T}}\boldsymbol{x}_i+b\right|}{\|\boldsymbol{w}\|} \geqslant \frac{\left|\boldsymbol{w}^{\mathrm{T}}\boldsymbol{x}^{-}_{*}+b\right|}{\|\boldsymbol{w}\|}, & y_i=-1
\end{array}\right.
$$


结合第一个条件中推导出的不等式，可将上式中的绝对值符号去掉并推得


$$
\left\{\begin{array}{ll}
\frac{\boldsymbol{w}^{\mathrm{T}}\boldsymbol{x}_i+b}{\|\boldsymbol{w}\|} \geqslant \frac{\boldsymbol{w}^{\mathrm{T}}\boldsymbol{x}^{+}_{*}+b}{\|\boldsymbol{w}\|}, & y_i=+1 \\
\frac{\boldsymbol{w}^{\mathrm{T}}\boldsymbol{x}_i+b}{\|\boldsymbol{w}\|} \leqslant \frac{\boldsymbol{w}^{\mathrm{T}}\boldsymbol{x}^{-}_{*}+b}{\|\boldsymbol{w}\|}, & y_i=-1
\end{array}\right.
$$


基于此再考虑第二个条件，"位于正负样本正中间"等价于要求超平面到$\boldsymbol{x}^{+}_{*}$和$\boldsymbol{x}^{-}_{*}$这两点的距离相等，即


$$
\frac{\left|\boldsymbol{w}^{\mathrm{T}}\boldsymbol{x}^{+}_{*}+b\right|}{\|\boldsymbol{w}\|}=\frac{\left|\boldsymbol{w}^{\mathrm{T}}\boldsymbol{x}^{-}_{*}+b\right|}{\|\boldsymbol{w}\|}
$$



综上，支持向量机所要求的超平面所需要满足的条件如下


$$
\left\{\begin{array}{ll}
\frac{\boldsymbol{w}^{\mathrm{T}}\boldsymbol{x}_i+b}{\|\boldsymbol{w}\|} \geqslant \frac{\boldsymbol{w}^{\mathrm{T}}\boldsymbol{x}^{+}_{*}+b}{\|\boldsymbol{w}\|}, & y_i=+1 \\
\frac{\boldsymbol{w}^{\mathrm{T}}\boldsymbol{x}_i+b}{\|\boldsymbol{w}\|} \leqslant \frac{\boldsymbol{w}^{\mathrm{T}}\boldsymbol{x}^{-}_{*}+b}{\|\boldsymbol{w}\|}, & y_i=-1 \\
\frac{\left|\boldsymbol{w}^{\mathrm{T}}\boldsymbol{x}^{+}_{*}+b\right|}{\|\boldsymbol{w}\|}=\frac{\left|\boldsymbol{w}^{\mathrm{T}}\boldsymbol{x}^{-}_{*}+b\right|}{\|\boldsymbol{w}\|}
\end{array}\right.
$$



但是根据超平面的性质(2)可知，当等倍缩放法向量$\boldsymbol{w}$和位移项$b$时，超平面不变，且上式也恒成立，因此会导致所求的超平面的参数$\boldsymbol{w}$和$b$有无穷多解。因此为了保证每个超平面的参数只有唯一解，不妨再额外施加一些约束，例如约束$\boldsymbol{x}^{+}_{*}$和$\boldsymbol{x}^{-}_{*}$代入进超平面方程后的绝对值为1，也就是令$\boldsymbol{w}^{\mathrm{T}}\boldsymbol{x}^{+}_{*}+b=1,\boldsymbol{w}^{\mathrm{T}}\boldsymbol{x}^{-}_{*}+b=-1$。此时支持向量机所要求的超平面所需要满足的条件变为


$$
\left\{\begin{array}{ll}
\frac{\boldsymbol{w}^{\mathrm{T}}\boldsymbol{x}_i+b}{\|\boldsymbol{w}\|} \geqslant \frac{+1}{\|\boldsymbol{w}\|}, & y_i=+1 \\
\frac{\boldsymbol{w}^{\mathrm{T}}\boldsymbol{x}_i+b}{\|\boldsymbol{w}\|} \leqslant \frac{-1}{\|\boldsymbol{w}\|}, & y_i=-1 \\
\end{array}\right.
$$


由于$\|\boldsymbol{w}\|$恒大于0，因此上式可进一步化简为


$$
\left\{\begin{array}{ll}
\boldsymbol{w}^{\mathrm{T}}\boldsymbol{x}_i+b \geqslant +1, & y_i=+1 \\
\boldsymbol{w}^{\mathrm{T}}\boldsymbol{x}_i+b \leqslant -1, & y_i=-1 \\
\end{array}\right.
$$



### 6.1.5 式(6.4)的推导

根据式(6.3)的推导可知，$\boldsymbol{x}^{+}_{*}$和$\boldsymbol{x}^{-}_{*}$便是"支持向量"，因此支持向量到超平面的距离已经被约束为$\frac{1}{\|\boldsymbol{w}\|}$，所以两个异类支持向量到超平面的距离之和为$\frac{2}{\|\boldsymbol{w}\|}$。

### 6.1.6 式(6.5)的解释

式(6.5)是通过"最大化间隔"来保证超平面离正负样本都尽可能远，且该超平面有且仅有一个，因此可以解出唯一解。

## 6.2 对偶问题

### 6.2.1 凸优化问题

考虑一般地约束优化问题 

$$
\begin{array}{ll}
{\min } & {f(\boldsymbol x)} \\
{\text {s.t.}} & {g_{i}(\boldsymbol x) \leqslant 0, \quad i=1,2,..., m} \\
{} & {h_{j}(\boldsymbol x)=0, \quad j=1,2,...,n}
\end{array}
$$


若目标函数$f(\boldsymbol x)$是凸函数，不等式约束$g_i(\boldsymbol{x})$是凸函数，等式约束$h_{j}(\boldsymbol x)$是仿射函数，则称该优化问题为凸优化问题。

由于$\frac{1}{2}\|\boldsymbol{w}\|^{2}$和$1-y_i\left(\boldsymbol{w}^{\mathrm{T}}\boldsymbol{x}_i+b\right)$均是关于$\boldsymbol{w}$和$b$的凸函数，所以式(6.6)是凸优化问题。凸优化问题是最优化里比较易解的一类优化问题，因为其拥有诸多良好的数学性质和现成的数学工具，因此如果非凸优化问题能等价转化为凸优化问题，其求解难度通常也会减小。

### 6.2.2 KKT条件

考虑一般的约束优化问题 

$$
\begin{array}{ll}
{\min } & {f(\boldsymbol x)} \\
{\text {s.t.}} & {g_{i}(\boldsymbol x) \leqslant 0, \quad i=1,2,..., m} \\
{} & {h_{j}(\boldsymbol x)=0, \quad j=1,2,...,n}
\end{array}
$$

 若$f(\boldsymbol x),g_i(\boldsymbol x),h_j(\boldsymbol
x)$的一阶偏导连续，$\boldsymbol x^*$是优化问题的局部解，$\boldsymbol \mu=(\mu_1;\mu_2;...;\mu_m),\boldsymbol \lambda=(\lambda_1;\lambda_2;...;\lambda_n)$为拉格朗日乘子向量，$L(\boldsymbol x,\boldsymbol \mu,\boldsymbol \lambda)=f(\boldsymbol x)+\sum_{i=1}^{m}\mu_i g_i(\boldsymbol x)+\sum_{j=1}^{n}\lambda_j h_j(\boldsymbol x)$为拉格朗日函数，且该优化问题满足任何一个特定的约束限制条件，则一定存在$\boldsymbol \mu^*=(\mu_1^*;
\mu_2^*;...; \mu_m^*), \boldsymbol \lambda^*=(\lambda_1^*;
\lambda_2^*;...; \lambda_n^*)$，使得：

\(1\)
$\nabla_{\boldsymbol x} L(\boldsymbol x^* ,\boldsymbol \mu^* ,\boldsymbol \lambda^* )=\nabla f(\boldsymbol  x^*)+\sum_{i=1}^{m}\mu_i^* \nabla g_i(\boldsymbol x^*)+\sum_{j=1}^{n}\lambda_j^* \nabla h_j(\boldsymbol x^*)=0$；

\(2\) $h_j(\boldsymbol x^*)=0, \quad j=1,2,...,n$；

\(3\) $g_i(\boldsymbol x^*) \leqslant 0, \quad i=1,2,..., m$；

\(4\) $\mu_i^* \geqslant 0, \quad i=1,2,..., m$；

\(5\) $\mu_i^* g_i(\boldsymbol x^*)=0, \quad i=1,2,..., m$。

以上5条便是Karush--Kuhn--Tucker
Conditions（简称KKT条件）。KKT条件是局部解的必要条件，也就是说只要该优化问题满足任何一个特定的约束限制条件，局部解就一定会满足以上5个条件。常用的约束限制条件可查阅维基百科"Karush--Kuhn--Tucker
Conditions"词条以及查阅参考文献[1]的第4.2.2节，若对KKT条件的数学证明感兴趣可查阅参考文献[1]的第4.2.1节。

### 6.2.3 拉格朗日对偶函数

考虑一般地约束优化问题 

$$
\begin{array}{ll}
{\min } & {f(\boldsymbol x)} \\
{\text {s.t.}} & {g_{i}(\boldsymbol x) \leqslant 0, \quad i=1,2,..., m} \\
{} & {h_{j}(\boldsymbol x)=0, \quad j=1,2,...,n}
\end{array}
$$


设上述优化问题的定义域为$D=\operatorname{dom} \ f \cap \bigcap\limits_{i=1}^{m}\operatorname{dom} \ g_i \cap \bigcap\limits_{j=1}^{n}\operatorname{dom} \ h_j$，可行集为$\tilde{D}=\{\boldsymbol x|\boldsymbol x\in D,g_i(\boldsymbol x) \leqslant 0,h_j(\boldsymbol x) = 0\}$（显然$\tilde{D}$是$D$的子集），最优值为$p^*=\min\{f(\tilde{\boldsymbol x})\},\tilde{\boldsymbol x}\in\tilde{D}$。上述优化问题的拉格朗日函数定义为


$$
L(\boldsymbol x,\boldsymbol \mu,\boldsymbol \lambda)=f(\boldsymbol x)+\sum_{i=1}^{m}\mu_i g_i(\boldsymbol x)+\sum_{j=1}^{n}\lambda_j h_j(\boldsymbol x)
$$


其中$\boldsymbol \mu=(\mu_1;\mu_2;...;\mu_m),\boldsymbol \lambda=(\lambda_1;\lambda_2;...;\lambda_n)$为拉格朗日乘子向量。相应地拉格朗日对偶函数$\Gamma (\boldsymbol \mu,\boldsymbol \lambda)$（简称对偶函数）定义为$L(\boldsymbol x,\boldsymbol \mu,\boldsymbol \lambda)$关于$\boldsymbol x$的下确界，即


$$
\Gamma (\boldsymbol \mu,\boldsymbol \lambda)=\mathop{\inf}\limits_{\boldsymbol x\in D}L(\boldsymbol x,\boldsymbol \mu,\boldsymbol \lambda)=\mathop{\inf}\limits_{\boldsymbol x\in D} \left( f(\boldsymbol x)+\sum_{i=1}^{m}\mu_i g_i(\boldsymbol x)+\sum_{j=1}^{n}\lambda_j h_j(\boldsymbol x) \right)
$$



对偶函数有如下性质：

(1)无论上述优化问题是否为凸优化问题，其对偶函数$\Gamma (\boldsymbol \mu,\boldsymbol \lambda)$恒为凹函数，详细证明可查阅参考文献[2]的第5.1.2和3.2.3节；

(2)当$\boldsymbol{\mu} \succeq 0$时（$\boldsymbol{\mu} \succeq 0$表示$\boldsymbol{\mu}$的分量均为非负），$\Gamma (\boldsymbol \mu,\boldsymbol \lambda)$构成了上述优化问题最优值$p^*$的下界，即


$$
\Gamma (\boldsymbol \mu,\boldsymbol \lambda)\leqslant p^*
$$


其推导过程如下：

设$\tilde{\boldsymbol x}\in\tilde{D}$是优化问题的可行点，则$g_i(\tilde{\boldsymbol x})\leqslant 0,h_j(\tilde{\boldsymbol x})=0$，因此，当$\boldsymbol \mu \succeq 0$时，$\mu_i g_i(\tilde{\boldsymbol x})\leqslant 0,\lambda_j h_j(\tilde{\boldsymbol x})=0$恒成立，所以


$$
\sum_{i=1}^{m}\mu_i g_i(\tilde{\boldsymbol x})+\sum_{j=1}^{n}\lambda_j h_j(\tilde{\boldsymbol x}) \leqslant 0
$$


根据上述不等式可以推得


$$
L(\tilde{\boldsymbol x},\boldsymbol \mu,\boldsymbol \lambda)=f(\tilde{\boldsymbol x})+\sum_{i=1}^{m}\mu_i g_i(\tilde{\boldsymbol x})+\sum_{j=1}^{n}\lambda_j h_j(\tilde{\boldsymbol x}) \leqslant f(\tilde{\boldsymbol x})
$$


又


$$
\Gamma (\boldsymbol \mu,\boldsymbol \lambda)=\mathop{\inf}\limits_{\boldsymbol x\in D}L(\boldsymbol x,\boldsymbol \mu,\boldsymbol \lambda) \leqslant L(\tilde{\boldsymbol x},\boldsymbol \mu,\boldsymbol \lambda)
$$


所以


$$
\Gamma (\boldsymbol \mu,\boldsymbol \lambda) \leqslant L(\tilde{\boldsymbol x},\boldsymbol \mu,\boldsymbol \lambda) \leqslant f(\tilde{\boldsymbol x})
$$


进一步地


$$
\Gamma (\boldsymbol \mu,\boldsymbol \lambda) \leqslant \min\{f(\tilde{\boldsymbol x})\}=p^*
$$



### 6.2.4 拉格朗日对偶问题

在$\boldsymbol \mu \succeq 0$的约束下求对偶函数最大值的优化问题称为拉格朗日对偶问题（简称对偶问题）


$$
\begin{array}{ll}
{\max } & {\Gamma (\boldsymbol \mu,\boldsymbol \lambda)} \\ 
{\text {s.t.}} & {\boldsymbol \mu \succeq 0} 
\end{array}
$$

 上一节的优化问题称为主问题或原问题。

设对偶问题的最优值为$d^*=\max \{\Gamma (\boldsymbol \mu,\boldsymbol \lambda)\},\boldsymbol \mu \succeq 0$，根据对偶函数的性质（2）可知$d^* \leqslant p^*$，此时称为"弱对偶性"成立，若$d^* = p^*$，则称为"强对偶性"成立。由此可以看出，当主问题较难求解时，如果强对偶性成立，则可以通过求解对偶问题来间接求解主问题。由于约束条件$\boldsymbol \mu \succeq 0$是凸集，且根据对偶函数的性质（1）可知$\Gamma (\boldsymbol \mu,\boldsymbol \lambda)$恒为凹函数，其加个负号即为凸函数，所以无论主问题是否为凸优化问题，对偶问题恒为凸优化问题。

一般情况下，强对偶性并不成立，只有当主问题满足特定的约束限制条件（不同于KKT条件中的约束限制条件）时，强对偶性才成立，常见的有"Slater条件"。Slater条件指出，当主问题是凸优化问题，且存在一点$\boldsymbol x\in\operatorname{relint}D$能使得所有等式约束成立，除仿射函数以外的不等式约束严格成立，则强对偶性成立。由于式(6.6)是凸优化问题，且不等式约束均为仿射函数，所以式(6.6)强对偶性成立。

对于凸优化问题，还可以通过KKT条件来间接推导出强对偶性，并同时求解出主问题和对偶问题的最优解。具体地，若主问题为凸优化问题，目标函数$f(\boldsymbol x)$和约束函数$g_{i}(\boldsymbol x),h_{j}(\boldsymbol x)$的一阶偏导连续，主问题满足KKT条件中任何一个特定的约束限制条件，则满足KKT条件的点$\boldsymbol{x}^{*}$和$(\boldsymbol{\mu}^{*},\boldsymbol{\lambda}^{*})$分别是主问题和对偶问题的最优解，且此时强对偶性成立。下面给出具体的推导过程。

设$\boldsymbol x^* ,\boldsymbol \mu^* ,\boldsymbol \lambda^*$是任意满足KKT条件的点，即


$$
\left\{\begin{array}{ll}
\nabla_{\boldsymbol x} L(\boldsymbol x^* ,\boldsymbol \mu^* ,\boldsymbol \lambda^* )=\nabla f(\boldsymbol  x^*)+\sum_{i=1}^{m}\mu_i^* \nabla g_i(\boldsymbol x^*)+\sum_{j=1}^{n}\lambda_j^* \nabla h_j(\boldsymbol x^*)=0 \\
h_j(\boldsymbol x^*)=0, \quad j=1,2,...,n\\
g_i(\boldsymbol x^*) \leqslant 0, \quad i=1,2,..., m\\
\mu_i^* \geqslant 0, \quad i=1,2,..., m\\
\mu_i^* g_i(\boldsymbol x^*)=0, \quad i=1,2,..., m
\end{array}\right.
$$


由于主问题是凸优化问题，所以$f(\boldsymbol{x})$和$g_i(\boldsymbol{x})$是凸函数，$h_j(\boldsymbol{x})$是仿射函数，又因为此时$\mu_i^* \geqslant 0$，所以$L(\boldsymbol x,\boldsymbol \mu^*,\boldsymbol \lambda^*)$是关于$\boldsymbol{x}$的凸函数。根据$\nabla_{\boldsymbol x} L(\boldsymbol x^* ,\boldsymbol \mu^* ,\boldsymbol \lambda^* )=0$可知，此时$\boldsymbol{x}^{*}$是$L(\boldsymbol x,\boldsymbol \mu^*,\boldsymbol \lambda^*)$的极值点，而凸函数的极值点也是最值点，所以$\boldsymbol{x}^{*}$是最小值点，因此可以进一步推得


$$
\begin{aligned}
L(\boldsymbol x^* ,\boldsymbol \mu^* ,\boldsymbol \lambda^* )&=\min\{L(\boldsymbol x,\boldsymbol \mu^*,\boldsymbol \lambda^*)\} \\
&=\mathop{\inf}_{\boldsymbol x\in D} \left( f(\boldsymbol x)+\sum_{i=1}^{m}\mu_i^* g_i(\boldsymbol x)+\sum_{j=1}^{n}\lambda_j^* h_j(\boldsymbol x) \right)\\
&=\Gamma(\boldsymbol \mu^*,\boldsymbol \lambda^*) \\
&=f(\boldsymbol x^*)+\sum_{i=1}^{m}\mu_i^* g_i(\boldsymbol x^*)+\sum_{j=1}^{n}\lambda_j^* h_j(\boldsymbol x^*)\\
&=f(\boldsymbol x^*)
\end{aligned}
$$


其中第二个等式是根据下确界函数的性质推得，第三个等式是根据对偶函数的定义推得，第四个等式是$L(\boldsymbol x^* ,\boldsymbol \mu^* ,\boldsymbol \lambda^* )$的展开形式，最后一个等式是因为$\mu_i^* g_i(\boldsymbol x^*)=0,h_j(\boldsymbol x^*)=0$。

由于$\boldsymbol{x}^{*}$和$(\boldsymbol{\mu}^{*},\boldsymbol{\lambda}^{*})$仅是满足KKT条件的点，并不一定是$f(\boldsymbol x)$和$\Gamma(\boldsymbol \mu,\boldsymbol \lambda)$的最值点，所以$f(\boldsymbol x^*)\geqslant p^*\geqslant d^{*}\geqslant\Gamma(\boldsymbol \mu^*,\boldsymbol \lambda^*)$，但是上式又推得$f(\boldsymbol x^*)=\Gamma(\boldsymbol \mu^*,\boldsymbol \lambda^*)$，所以$p^{*}=d^{*}$，因此推得强对偶性成立，且$\boldsymbol{x}^{*}$和$(\boldsymbol{\mu}^{*},\boldsymbol{\lambda}^{*})$分别是主问题和对偶问题的最优解。

Slater条件恰巧也是KKT条件中特定的约束限制条件之一，所以式(6.6)不仅强对偶性成立，而且可以通过求解满足KKT条件的点来求解出最优解。

KKT条件除了可以作为凸优化问题强对偶性成立的充分条件以外，其实对于任意优化问题（并不一定是凸优化问题），若其强对偶性成立，KKT条件也是主问题和对偶问题最优解的必要条件，而且此时并不要求主问题满足KKT条件中任何一个特定的约束限制条件。下面同样给出具体的推导过程。

设主问题的最优解为$\boldsymbol{x}^{*}$，对偶问题的最优解为$(\boldsymbol{\mu}^{*},\boldsymbol{\lambda}^{*})$，目标函数$f(\boldsymbol x)$和约束函数$g_{i}(\boldsymbol x),h_{j}(\boldsymbol x)$的一阶偏导连续，当强对偶性成立时，可以推得


$$
\begin{aligned}
f(\boldsymbol x^*)&=\Gamma(\boldsymbol \mu^*,\boldsymbol \lambda^*)\\
&= \mathop{\inf}_{\boldsymbol x\in D} L(\boldsymbol x,\boldsymbol \mu^*,\boldsymbol \lambda^*) \\
&=\mathop{\inf}_{\boldsymbol x\in D} \left( f(\boldsymbol x)+\sum_{i=1}^{m}\mu_i^* g_i(\boldsymbol x)+\sum_{j=1}^{n}\lambda_j^* h_j(\boldsymbol x) \right) \\
&\leqslant f(\boldsymbol x^*)+\sum_{i=1}^{m}\mu_i^* g_i(\boldsymbol x^*)+\sum_{j=1}^{n}\lambda_j^* h_j(\boldsymbol x^*) \\
&\leqslant f(\boldsymbol x^*)
\end{aligned}
$$


其中，第一个等式是因为强对偶性成立时$p^{*}=d^{*}$，第二和第三个等式是对偶函数的定义，第四个不等式是根据下确界的性质推得，最后一个不等式成立是因为$\mu_i^*\geqslant 0,g_i(\boldsymbol x^*)\leqslant 0,h_j(\boldsymbol x^*)=0$。

由于$f(\boldsymbol x^*)=f(\boldsymbol x^*)$，所以上式中的不等式均可化为等式。第四个不等式可化为等式，说明$L(\boldsymbol x,\boldsymbol \mu^*,\boldsymbol \lambda^*)$在$\boldsymbol x^*$处取得最小值，所以根据极值的性质可知在$\boldsymbol x^*$处一阶导$\nabla_{\boldsymbol x} L(\boldsymbol x^* ,\boldsymbol \mu^* ,\boldsymbol \lambda^*)=0$。最后一个不等式可化为等式，说明$\mu_i^* g_i(\boldsymbol x^*)=0$。此时再结合主问题和对偶问题原有的约束条件$\mu_i^*\geqslant 0,g_i(\boldsymbol x^*)\leqslant 0,h_j(\boldsymbol x^*)=0$便凑齐了KKT条件。

### 6.2.5 式(6.9)和式(6.10)的推导



$$
\begin{aligned}
L(\boldsymbol{w},b,\boldsymbol{\alpha}) &= \frac{1}{2}\|\boldsymbol{w}\|^2+\sum_{i=1}^m\alpha_i(1-y_i(\boldsymbol{w}^{\mathrm{T}}\boldsymbol{x}_i+b)) \\
& =\frac{1}{2}\|\boldsymbol{w}\|^2+\sum_{i=1}^m(\alpha_i-\alpha_iy_i \boldsymbol{w}^{\mathrm{T}}\boldsymbol{x}_i-\alpha_iy_ib)\\
& =\frac{1}{2}\boldsymbol{w}^{\mathrm{T}}\boldsymbol{w}+\sum_{i=1}^m\alpha_i -\sum_{i=1}^m\alpha_iy_i\boldsymbol{w}^{\mathrm{T}}\boldsymbol{x}_i-\sum_{i=1}^m\alpha_iy_ib
\end{aligned}
$$

 对$\boldsymbol{w}$和$b$分别求偏导数并令其为零


$$
\frac {\partial L}{\partial \boldsymbol{w}}=\frac{1}{2}\times2\times\boldsymbol{w} + 0 - \sum_{i=1}^{m}\alpha_iy_i \boldsymbol{x}_i-0= 0 \Longrightarrow \boldsymbol{w}=\sum_{i=1}^{m}\alpha_iy_i \boldsymbol{x}_i
$$




$$
\frac {\partial L}{\partial b}=0+0-0-\sum_{i=1}^{m}\alpha_iy_i=0  \Longrightarrow  \sum_{i=1}^{m}\alpha_iy_i=0
$$



### 6.2.6 式(6.11)的推导

因为$\alpha_{i}\geqslant0$，且$\frac{1}{2}\|\boldsymbol{w}\|^{2}$和$1-y_i\left(\boldsymbol{w}^{\mathrm{T}}\boldsymbol{x}_i+b\right)$均是关于$\boldsymbol{w}$和$b$的凸函数，所以式(6.8)也是关于$\boldsymbol{w}$和$b$的凸函数。根据凸函数的性质可知，其极值点就是最值点，所以一阶导为零的点就是最小值点，因此将式(6.9)和式(6.10)代入式(6.8)后即可得式(6.8)的最小值（等价于下确界），再根据对偶问题的定义加上约束$\alpha_{i}\geqslant 0$，就得到了式(6.6)的对偶问题。由于式(6.10)也是$\alpha{i}$必须满足的条件，且不含有$\boldsymbol{w}$和$b$，因此也需要纳入对偶问题的约束条件。根据以上思路进行推导的过程如下：


$$
\begin{aligned}
\inf_{\boldsymbol{w},b} L(\boldsymbol{w},b,\boldsymbol{\alpha})  &=\frac{1}{2}\boldsymbol{w}^{\mathrm{T}}\boldsymbol{w}+\sum_{i=1}^m\alpha_i -\sum_{i=1}^m\alpha_iy_i\boldsymbol{w}^{\mathrm{T}}\boldsymbol{x}_i-\sum_{i=1}^m\alpha_iy_ib \\
&=\frac {1}{2}\boldsymbol{w}^{\mathrm{T}}\sum _{i=1}^m\alpha_iy_i\boldsymbol{x}_i-\boldsymbol{w}^{\mathrm{T}}\sum _{i=1}^m\alpha_iy_i\boldsymbol{x}_i+\sum _{i=1}^m\alpha_
i -b\sum _{i=1}^m\alpha_iy_i \\
& = -\frac {1}{2}\boldsymbol{w}^{\mathrm{T}}\sum _{i=1}^m\alpha_iy_i\boldsymbol{x}_i+\sum _{i=1}^m\alpha_i -b\sum _{i=1}^m\alpha_iy_i \\
&= -\frac {1}{2}\boldsymbol{w}^{\mathrm{T}}\sum _{i=1}^m\alpha_iy_i\boldsymbol{x}_i+\sum _{i=1}^m\alpha_i \\
&=-\frac {1}{2}(\sum_{i=1}^{m}\alpha_iy_i\boldsymbol{x}_i)^{\mathrm{T}}(\sum _{i=1}^m\alpha_iy_i\boldsymbol{x}_i)+\sum _{i=1}^m\alpha_i \\
&=-\frac {1}{2}\sum_{i=1}^{m}\alpha_iy_i\boldsymbol{x}_i^{\mathrm{T}}\sum _{i=1}^m\alpha_iy_i\boldsymbol{x}_i+\sum _{i=1}^m\alpha_i \\
&=\sum _{i=1}^m\alpha_i-\frac {1}{2}\sum_{i=1 }^{m}\sum_{j=1}^{m}\alpha_i\alpha_jy_iy_j\boldsymbol{x}_i^{\mathrm{T}}\boldsymbol{x}_j
\end{aligned}
$$

 所以


$$
\max_{\boldsymbol{\alpha}}\inf_{\boldsymbol{w},b} L(\boldsymbol{w},b,\boldsymbol{\alpha})=\max_{\boldsymbol{\alpha}} \sum_{i=1}^m\alpha_i - \frac{1}{2}\sum_{i = 1}^m\sum_{j=1}^m\alpha_i \alpha_j y_iy_j\boldsymbol{x}_i^{\mathrm{T}}\boldsymbol{x}_j
$$


最后将$\alpha_{i}\geqslant 0$和式(6.10)作为约束条件即可得式(6.11)。

式(6.6)之所以要转化为式(6.11)来求解，其主要有以下两点理由：

(1)式(6.6)中的未知数是$\boldsymbol{w}$和$b$，式(6.11)中的未知数是$\boldsymbol{\alpha}$，$\boldsymbol{w}$的维度$d$对应样本特征个数，$\boldsymbol{\alpha}$的维度$m$对应训练样本个数，通常$m\ll d$，所以求解式(6.11)更高效，反之求解式(6.6)更高效；

(2)式(6.11)中有样本内积$\boldsymbol{x}_i^{\mathrm{T}}\boldsymbol{x}_j$这一项，后续可以很自然地引入核函数，进而使得支持向量机也能对在原始特征空间线性不可分的数据进行分类。

### 6.2.7 式(6.13)的解释

因为式(6.6)满足Slater条件，所以强对偶性成立，进而最优解满足KKT条件。

## 6.3 核函数

### 6.3.1 式(6.22)的解释

此即核函数的定义，即核函数可以分解成两个向量的内积。要想了解某个核函数是如何将原始特征空间映射到更高维的特征空间的，只需要分解为两个表达形式完全一样的向量内积即可。

## 6.4 软间隔与正则化

### 6.4.1 式(6.35)的推导

令


$$
\max \left(0,1-y_{i}\left(\boldsymbol{w}^{\mathrm{T}} \boldsymbol{x}_{i}+b\right)\right)=\xi_{i}
$$


显然$\xi_i\geq 0$，且当$1-y_{i}\left(\boldsymbol{w}^{\mathrm{T}}\boldsymbol{x}_{i}+b\right)>0$时有


$$
1-y_{i}\left(\boldsymbol{w}^{\mathrm{T}} \boldsymbol{x}_{i}+b\right)=\xi_i
$$


当$1-y_{i}\left(\boldsymbol{w}^{\mathrm{T}}\boldsymbol{x}_{i}+b\right)\leq 0$时有


$$
\xi_i = 0
$$

 综上可得


$$
1-y_{i}\left(\boldsymbol{w}^{\mathrm{T}} \boldsymbol{x}_{i}+b\right)\leqslant\xi_i\Rightarrow y_{i}\left(\boldsymbol{w}^{\mathrm{T}} \boldsymbol{x}_{i}+b\right) \geqslant 1-\xi_{i}
$$



### 6.4.2 式(6.37)和式(6.38)的推导

类比式(6.9)和式(6.10)的推导

### 6.4.3 式(6.39)的推导

式(6.36)关于$\xi_i$求偏导数并令其为零


$$
\frac{\partial L}{\partial \xi_i}=0+C \times 1 - \alpha_i \times 1-\mu_i
\times 1 =0\Rightarrow C=\alpha_i +\mu_i
$$



### 6.4.4 式(6.40)的推导

将式(6.37)、式(6.38)和(6.39)代入式(6.36)可以得到式(6.35)的对偶问题，有


$$
\begin{aligned}
&\frac{1}{2}\left\|\boldsymbol{w}\right\|^2+C\sum_{i=1}^m \xi_i+\sum_{i=1}^m\alpha_i\left(1-\xi_i-y_i\left(\boldsymbol{w}^{\mathrm{T}}\boldsymbol{x}_i+b\right)\right)-\sum_{i=1}^m\mu_i \xi_i  \\
=&\frac{1}{2}\left\|\boldsymbol{w}\right\|^2+\sum_{i=1}^m\alpha_i
\left(1-y_i\left(\boldsymbol{w}^{\mathrm{T}}\boldsymbol{x}_i+b\right)\right)+C\sum_{i=1}^m \xi_i-\sum_{i=1}^m \alpha_i \xi_i-\sum_{i=1}^m\mu_i \xi_i \\
=&-\frac {1}{2}\sum_{i=1}^{m}\alpha_iy_i\boldsymbol{x}_i^{\mathrm{T}}\sum _{i=1}^m\alpha_iy_i\boldsymbol{x}_i+\sum _{i=1}^m\alpha_i +\sum_{i=1}^m C\xi_i-\sum_{i=1}^m \alpha_i \xi_i-\sum_{i=1}^m\mu_i \xi_i \\
=&-\frac {1}{2}\sum_{i=1}^{m}\alpha_iy_i\boldsymbol{x}_i^{\mathrm{T}}\sum _{i=1}^m\alpha_iy_i\boldsymbol{x}_i+\sum _{i=1}^m\alpha_i +\sum_{i=1}^m (C-\alpha_i-\mu_i)\xi_i \\
=&\sum _{i=1}^m\alpha_i-\frac {1}{2}\sum_{i=1 }^{m}\sum_{j=1}^{m}\alpha_i\alpha_jy_iy_j\boldsymbol{x}_i^{\mathrm{T}}\boldsymbol{x}_j\\
=&\min_{\boldsymbol{w},b,\boldsymbol{\xi}}L(\boldsymbol{w},b,\boldsymbol{\alpha},\boldsymbol{\xi},\boldsymbol{\mu})
\end{aligned}
$$

 所以 

$$
\begin{aligned}
\max_{\boldsymbol{\alpha},\boldsymbol{\mu}}\min_{\boldsymbol{w},b,\boldsymbol{\xi}}L(\boldsymbol{w},b,\boldsymbol{\alpha},\boldsymbol{\xi},\boldsymbol{\mu})&=\max_{\boldsymbol{\alpha},\boldsymbol{\mu}}\quad\sum_{i=1}^m\alpha_i-\frac {1}{2}\sum_{i=1}^{m}\sum_{j=1}^{m}
\alpha_i\alpha_jy_iy_j\boldsymbol{x}_i^{\mathrm{T}}\boldsymbol{x}_j \\
&=\max_{\boldsymbol{\alpha}}\quad\sum _{i=1}^m\alpha_i-\frac
{1}{2}\sum_{i=1}^{m}\sum_{j=1}^{m}\alpha_i\alpha_jy_iy_j\boldsymbol{x}_i^{\mathrm{T}}\boldsymbol{x}_j
\end{aligned}
$$

 又因为 $\alpha_i \geq 0$，$\mu_i \geq 0$，
$C = \alpha_i+\mu_i$，消去$\mu_i$可得等价约束条件


$$
0 \leqslant\alpha_i \leqslant C, \quad i=1,2,...,m
$$



### 6.4.5 对数几率回归与支持向量机的关系

在"西瓜书"本节的倒数第二段开头，其讨论了对数几率回归与支持向量机的关系，提到"如果使用对率损失函数$\ell_{\log}$来替代式(6.29)中的$0/1$损失函数，则几乎就得到了对率回归模型(3.27)"，但式(6.29)与式(3.27)形式上相差甚远。为了更清晰的说明对数几率回归与软间隔支持向量机的关系，以下先对式(3.27)的形式进行变化。

将$\boldsymbol{\beta}=(\boldsymbol{w};b)$和$\hat{\boldsymbol{x}}=(\boldsymbol{x};1)$代入式(3.27)可得


$$
\begin{aligned}
\ell(\boldsymbol{w}, b) & =\sum_{i=1}^m\left(-y_i\left(\boldsymbol{w}^{\mathrm{T}}\boldsymbol{x}_i+b\right)+\ln \left(1+e^{\boldsymbol{w}^{\mathrm{T}}\boldsymbol{x}_i+b}\right)\right) \\
& =\sum_{i=1}^m\left(\ln \frac{1}{e^{y_i\left(\boldsymbol{w}^{\mathrm{T}}\boldsymbol{x}_i+b\right)}}+\ln \left(1+e^{\boldsymbol{w}^{\mathrm{T}}\boldsymbol{x}_i+b}\right)\right) \\
& =\sum_{i=1}^m \ln \frac{1+e^{\boldsymbol{w}^{\mathrm{T}}\boldsymbol{x}_i+b}}{e^{y_i\left(\boldsymbol{w}^{\mathrm{T}}\boldsymbol{x}_i+b\right)}} \\
& =\left\{\begin{array}{cl}
\sum_{i=1}^m \ln \left(1+e^{-\left(\boldsymbol{w}^{\mathrm{T}}\boldsymbol{x}_i+b\right)}\right),&\quad y_i=1 \\
\sum_{i=1}^m \ln \left(1+e^{\boldsymbol{w}^{\mathrm{T}}\boldsymbol{x}_i+b}\right),&\quad y_i=0
\end{array}\right.
\end{aligned}
$$


上式中正例和反例分别用$y_i=1$和$y_i=0$表示，这是对数几率回归常用的方式，而在支持向量机中正例和反例习惯用$y_i=+1$和$y_i=-1$表示。实际上，若上式也换用$y_i=+1$和$y_i=-1$分别表示正例和反例，则上式可改写为


$$
\begin{aligned}
\ell(\boldsymbol{w}, b) & =\left\{\begin{array}{cl}
\sum_{i=1}^m \ln \left(1+e^{-\left(\boldsymbol{w}^{\mathrm{T}} \boldsymbol{x}_i+b\right)}\right),&\quad y_i=+1 \\
\sum_{i=1}^m \ln \left(1+e^{\boldsymbol{w}^{\mathrm{T}} \boldsymbol{x}_i+b}\right),&\quad y_i=-1
\end{array}\right. \\
&=\sum_{i=1}^m \ln \left(1+e^{-y_i\left(\boldsymbol{w}^{\mathrm{T}} \boldsymbol{x}_i+b\right)}\right) \\
\end{aligned}
$$

 此时上式的求和项正是式(6.33)所表述的对率损失。

### 6.4.6 式(6.41)的解释

参见式(6.13)的解释

## 6.5 支持向量回归

### 6.5.1 式(6.43)的解释

相比于线性回归用一条线来拟合训练样本，支持向量回归而是采用一个以$f(\boldsymbol x)=\boldsymbol{w}^\mathrm{T}\boldsymbol{x}+b$为中心，宽度为$2\epsilon$的间隔带，来拟合训练样本。

落在带子上的样本不计算损失（类比线性回归在线上的点预测误差为0），不在带子上的则以偏离带子的距离作为损失（类比线性回归的均方误差），然后以最小化损失的方式迫使间隔带从样本最密集的地方穿过，进而达到拟合训练样本的目的。因此支持向量回归的优化问题可以写为


$$
\min _{\boldsymbol{w}, b} \frac{1}{2}\|\boldsymbol{w}\|^{2}+C \sum_{i=1}^{m} \ell_{\epsilon}\left(f\left(\boldsymbol{x}_{i}\right)-y_{i}\right)
$$


其中$\ell_{\epsilon}(z)$为"$\epsilon$不敏感损失函数"（类比线性回归的均方误差损失）


$$
\ell_{\epsilon}(z)=\left\{\begin{array}{cc}{0,} & {\text { if }|z| \leqslant \epsilon} \\ {|z|-\epsilon,} & {\text { if }|z| > \epsilon}\end{array}\right.
$$


$\frac{1}{2}\|\boldsymbol{w}\|^{2}$为L2正则项，此处引入正则项除了起正则化本身的作用外，也是为了和软间隔支持向量机的优化目标保持形式上的一致，这样就可以导出对偶问题引入核函数，$C$为用来调节损失权重的正则化常数。

### 6.5.2 式(6.45)的推导

同软间隔支持向量机，引入松弛变量$\xi_i$，令


$$
\ell_{\epsilon}\left(f\left(\boldsymbol{x}_{i}\right)-y_{i}\right)=\xi_i
$$


显然$\xi_i\geqslant 0$，并且当$|f\left(\boldsymbol{x}_{i}\right)-y_{i}|\leqslant\epsilon$时，$\xi_i=0$，当$|f\left(\boldsymbol{x}_{i}\right)-y_{i}|>\epsilon$时，$\xi_i=|f\left(\boldsymbol{x}_{i}\right)-y_{i}|-\epsilon$，所以


$$
|f\left(\boldsymbol{x}_{i}\right)-y_{i}|-\epsilon\leqslant\xi_i
$$




$$
|f\left(\boldsymbol{x}_{i}\right)-y_{i}|\leqslant\epsilon+\xi_i
$$




$$
-\epsilon-\xi_i\leqslant f\left(\boldsymbol{x}_{i}\right)-y_{i}\leqslant\epsilon+\xi_i
$$


因此支持向量回归的优化问题可以化为


$$
\min _{\boldsymbol{w}, b, \xi_{i}} \frac{1}{2}\|\boldsymbol{w}\|^{2}+C \sum_{i=1}^{m}\xi_{i}
$$




$$
\begin{array}{ll}{\text { s.t. }} & {-\epsilon-\xi_i\leqslant f\left(\boldsymbol{x}_{i}\right)-y_{i}\leqslant\epsilon+\xi_i} \\  {} & {\xi_{i} \geqslant 0, i=1,2, \ldots, m}\end{array}
$$


如果考虑两边采用不同的松弛程度，则有


$$
\min _{\boldsymbol{w}, b, \xi_{i}, \hat{\xi}_{i}} \frac{1}{2}\|\boldsymbol{w}\|^{2}+C \sum_{i=1}^{m}\left(\xi_{i}+\hat{\xi}_{i}\right)
$$




$$
\begin{array}{ll}{\text { s.t. }} & {-\epsilon-\hat{\xi}_i\leqslant f\left(\boldsymbol{x}_{i}\right)-y_{i}\leqslant\epsilon+\xi_i} \\  {} & {\xi_{i} \geqslant 0, \hat{\xi}_{i} \geqslant 0, i=1,2, \ldots, m}\end{array}
$$



### 6.5.3 式(6.52)的推导

将式(6.45)的约束条件全部恒等变形为小于等于0的形式可得


$$
\left\{\begin{array}{l}
{f\left(\boldsymbol{x}_{i}\right)-y_{i}-\epsilon-\xi_{i} \leq 0 } \\
{y_{i}-f\left(\boldsymbol{x}_{i}\right)-\epsilon-\hat{\xi}_{i} \leq 0 } \\
{-\xi_{i} \leq 0} \\
{-\hat{\xi}_{i} \leq 0}
\end{array}\right.
$$


由于以上四个约束条件的拉格朗日乘子分别为$\alpha_i,\hat{\alpha}_i,\mu_i,\hat{\mu}_i$，所以其对应的KKT条件为


$$
\left\{\begin{array}{l}
{\alpha_i\left(f\left(\boldsymbol{x}_{i}\right)-y_{i}-\epsilon-\xi_{i} \right) = 0 } \\
{\hat{\alpha}_i\left(y_{i}-f\left(\boldsymbol{x}_{i}\right)-\epsilon-\hat{\xi}_{i} \right) = 0 } \\
{-\mu_i\xi_{i} = 0 \Rightarrow \mu_i\xi_{i} = 0 } \\
{-\hat{\mu}_i \hat{\xi}_{i} = 0  \Rightarrow \hat{\mu}_i
\hat{\xi}_{i} = 0}
\end{array}\right.
$$

 又由式(6.49)和式(6.50)有 

$$
\left\{\begin{aligned}
\mu_i=C-\alpha_i\\
\hat{\mu}_i=C-\hat{\alpha}_i
\end{aligned}\right.
$$

 所以上述KKT条件可以进一步变形为


$$
\left\{\begin{array}{l}
{\alpha_i\left(f\left(\boldsymbol{x}_{i}\right)-y_{i}-\epsilon-\xi_{i} \right) = 0 } \\
{\hat{\alpha}_i\left(y_{i}-f\left(\boldsymbol{x}_{i}\right)-\epsilon-\hat{\xi}_{i} \right) = 0 } \\
{(C-\alpha_i)\xi_{i} = 0 }  \\
{(C-\hat{\alpha}_i) \hat{\xi}_{i} = 0 }
\end{array}\right.
$$


又因为样本$(\boldsymbol{x}_i,y_i)$只可能处在间隔带的某一侧，即约束条件$f\left(\boldsymbol{x}_{i}\right)-y_{i}-\epsilon-\xi_{i}=0$和$y_{i}-f\left(\boldsymbol{x}_{i}\right)-\epsilon-\hat{\xi}_{i}=0$不可能同时成立，所以$\alpha_i$和$\hat{\alpha}_i$中至少有一个为0，即$\alpha_i\hat{\alpha}_i=0$。

在此基础上再进一步分析可知，如果$\alpha_i=0$，则根据约束$(C-\alpha_i)\xi_{i} = 0$可知此时$\xi_i=0$。同理，如果$\hat{\alpha}_i=0$，则根据约束$(C-\hat{\alpha}_i)\hat{\xi}_{i} =
0$可知此时$\hat{\xi}_i=0$。所以$\xi_i$和$\hat{\xi}_i$中也是至少有一个为0，即$\xi_{i}\hat{\xi}_{i}=0$。将$\alpha_i\hat{\alpha}_i=0,\xi_{i}\hat{\xi}_{i}=0$整合进上述KKT条件中即可得到式(6.52)。

## 6.6 核方法

### 6.6.1 式(6.57)和式(6.58)的解释

式(6.24)是式(6.20)的解；式(6.56)是式(6.43)的解。对应到表示定理式(6.57)当中，式(6.20)和式(6.43)均为$\Omega\left(\|h\|_{\mathbb{H}}\right)=\frac{1}{2}\|\boldsymbol{w}\|^2$，式(6.20)的$\ell\left(h(\boldsymbol{x}_1),h(\boldsymbol{x}_2),...,h(\boldsymbol{x}_m)\right)=0$，而式(6.43)的$\ell\left(h(\boldsymbol{x}_1),h(\boldsymbol{x}_2),...,h(\boldsymbol{x}_m)\right)=C\sum_{i=1}^{m}\ell_{\epsilon}(f(\boldsymbol{x}_i)-y_i)$，均满足式(6.57)的要求，式(6.20)和式(6.43)的解均为$\kappa(\boldsymbol{x},\boldsymbol{x}_i)$的线性组合，即式(6.58)。

### 6.6.2 式(6.65)的推导

由表示定理可知，此时二分类KLDA最终求得的投影直线方程总可以写成如下形式：


$$
h(\boldsymbol{x})=\sum_{i=1}^{m} \alpha_{i} \kappa\left(\boldsymbol{x}, \boldsymbol{x}_{i}\right)
$$


又因为直线方程的固定形式为


$$
h(\boldsymbol{x})=\boldsymbol{w}^{\mathrm{T}}\phi(\boldsymbol{x})
$$


所以


$$
\boldsymbol{w}^{\mathrm{T}}\phi(\boldsymbol{x})=\sum_{i=1}^{m} \alpha_{i} \kappa\left(\boldsymbol{x}, \boldsymbol{x}_{i}\right)
$$


将$\kappa\left(\boldsymbol{x},
\boldsymbol{x}_{i}\right)=\phi(\boldsymbol{x})^{\mathrm{T}}\phi(\boldsymbol{x}_i)$代入可得


$$
\begin{aligned}
\boldsymbol{w}^{\mathrm{T}}\phi(\boldsymbol{x})&=\sum_{i=1}^{m} \alpha_{i}
\phi(\boldsymbol{x})^{\mathrm{T}}\phi(\boldsymbol{x}_i)\\
&=\phi(\boldsymbol{x})^{\mathrm{T}}\cdot\sum_{i=1}^{m} \alpha_{i}
\phi(\boldsymbol{x}_i)
\end{aligned}
$$


由于$\boldsymbol{w}^{\mathrm{T}}\phi(\boldsymbol{x})$的计算结果为标量，而标量的转置等于其本身，所以


$$
\boldsymbol{w}^{\mathrm{T}}\phi(\boldsymbol{x})=
\left(\boldsymbol{w}^{\mathrm{T}}\phi(\boldsymbol{x})\right)^{\mathrm{T}}
=\phi(\boldsymbol{x})^{\mathrm{T}}\boldsymbol{w}
=\phi(\boldsymbol{x})^{\mathrm{T}}\sum_{i=1}^{m} \alpha_{i}
\phi(\boldsymbol{x}_i)
$$

 即


$$
\boldsymbol{w}=\sum_{i=1}^{m} \alpha_{i} \phi(\boldsymbol{x}_i)
$$



### 6.6.3 式(6.66)和式(6.67)的解释

为了详细地说明此式的计算原理，下面首先举例说明，然后再在例子的基础上延展出其一般形式。
假设此时仅有4个样本，其中第1和第3个样本的标记为0，第2和第4个样本的标记为1，那么此时有


$$
m=4
$$

 

$$
m_0=2,m_1=2
$$




$$
X_0=\{\boldsymbol{x}_1,\boldsymbol{x}_3\},X_1=\{\boldsymbol{x}_2,\boldsymbol{x}_4\}
$$




$$
\mathbf{K}=\left[ \begin{array}{cccc}
\kappa\left(\boldsymbol{x}_1, \boldsymbol{x}_1\right) & \kappa\left(\boldsymbol{x}_1, \boldsymbol{x}_2\right) & \kappa\left(\boldsymbol{x}_1, \boldsymbol{x}_3\right) & \kappa\left(\boldsymbol{x}_1, \boldsymbol{x}_4\right)\\ 
\kappa\left(\boldsymbol{x}_2, \boldsymbol{x}_1\right) & \kappa\left(\boldsymbol{x}_2, \boldsymbol{x}_2\right) & \kappa\left(\boldsymbol{x}_2, \boldsymbol{x}_3\right) & \kappa\left(\boldsymbol{x}_2, \boldsymbol{x}_4\right)\\ 
\kappa\left(\boldsymbol{x}_3, \boldsymbol{x}_1\right) & \kappa\left(\boldsymbol{x}_3, \boldsymbol{x}_2\right) & \kappa\left(\boldsymbol{x}_3, \boldsymbol{x}_3\right) & \kappa\left(\boldsymbol{x}_3, \boldsymbol{x}_4\right)\\ 
\kappa\left(\boldsymbol{x}_4, \boldsymbol{x}_1\right) & \kappa\left(\boldsymbol{x}_4, \boldsymbol{x}_2\right) & \kappa\left(\boldsymbol{x}_4, \boldsymbol{x}_3\right) & \kappa\left(\boldsymbol{x}_4, \boldsymbol{x}_4\right)\\ 
\end{array} \right]\in \mathbb{R}^{4\times 4}
$$




$$
\mathbf{1}_{0}=\left[ \begin{array}{c}
1\\ 
0\\ 
1\\ 
0\\ 
\end{array} \right]\in \mathbb{R}^{4\times 1}
$$




$$
\mathbf{1}_{1}=\left[ \begin{array}{c}
0\\ 
1\\ 
0\\ 
1\\ 
\end{array} \right]\in \mathbb{R}^{4\times 1}
$$

 所以


$$
\hat{\boldsymbol{\mu}}_{0}=\frac{1}{m_{0}} \mathbf{K} \mathbf{1}_{0}=\frac{1}{2}\left[ \begin{array}{c}
\kappa\left(\boldsymbol{x}_1, \boldsymbol{x}_1\right)+\kappa\left(\boldsymbol{x}_1, \boldsymbol{x}_3\right)\\ 
\kappa\left(\boldsymbol{x}_2, \boldsymbol{x}_1\right)+\kappa\left(\boldsymbol{x}_2, \boldsymbol{x}_3\right)\\ 
\kappa\left(\boldsymbol{x}_3, \boldsymbol{x}_1\right)+\kappa\left(\boldsymbol{x}_3, \boldsymbol{x}_3\right)\\ 
\kappa\left(\boldsymbol{x}_4, \boldsymbol{x}_1\right)+\kappa\left(\boldsymbol{x}_4, \boldsymbol{x}_3\right)\\ 
\end{array} \right]\in \mathbb{R}^{4\times 1}
$$




$$
\hat{\boldsymbol{\mu}}_{1}=\frac{1}{m_{1}} \mathbf{K} \mathbf{1}_{1}=\frac{1}{2}\left[ \begin{array}{c}
\kappa\left(\boldsymbol{x}_1, \boldsymbol{x}_2\right)+\kappa\left(\boldsymbol{x}_1, \boldsymbol{x}_4\right)\\ 
\kappa\left(\boldsymbol{x}_2, \boldsymbol{x}_2\right)+\kappa\left(\boldsymbol{x}_2, \boldsymbol{x}_4\right)\\ 
\kappa\left(\boldsymbol{x}_3, \boldsymbol{x}_2\right)+\kappa\left(\boldsymbol{x}_3, \boldsymbol{x}_4\right)\\ 
\kappa\left(\boldsymbol{x}_4, \boldsymbol{x}_2\right)+\kappa\left(\boldsymbol{x}_4, \boldsymbol{x}_4\right)\\ 
\end{array} \right]\in \mathbb{R}^{4\times 1}
$$


根据此结果易得$\hat{\boldsymbol{\mu}}_{0},\hat{\boldsymbol{\mu}}_{1}$的一般形式为


$$
\hat{\boldsymbol{\mu}}_{0}=\frac{1}{m_{0}} \mathbf{K} \mathbf{1}_{0}=\frac{1}{m_{0}}\left[ \begin{array}{c}
\sum_{\boldsymbol{x} \in X_{0}}\kappa\left(\boldsymbol{x}_1, \boldsymbol{x}\right)\\ 
\sum_{\boldsymbol{x} \in X_{0}}\kappa\left(\boldsymbol{x}_2, \boldsymbol{x}\right)\\ 
\vdots\\ 
\sum_{\boldsymbol{x} \in X_{0}}\kappa\left(\boldsymbol{x}_m, \boldsymbol{x}\right)\\ 
\end{array} \right]\in \mathbb{R}^{m\times 1}
$$




$$
\hat{\boldsymbol{\mu}}_{1}=\frac{1}{m_{1}} \mathbf{K} \mathbf{1}_{1}=\frac{1}{m_{1}}\left[ \begin{array}{c}
\sum_{\boldsymbol{x} \in X_{1}}\kappa\left(\boldsymbol{x}_1, \boldsymbol{x}\right)\\ 
\sum_{\boldsymbol{x} \in X_{1}}\kappa\left(\boldsymbol{x}_2, \boldsymbol{x}\right)\\ 
\vdots\\ 
\sum_{\boldsymbol{x} \in X_{1}}\kappa\left(\boldsymbol{x}_m, \boldsymbol{x}\right)\\ 
\end{array} \right]\in \mathbb{R}^{m\times 1}
$$



### 6.6.4 式(6.70)的推导

此式是将式(6.65)代入式(6.60)后推得而来的，下面给出详细地推导过程。

首先将式(6.65)代入式(6.60)的分子可得 

$$
\begin{aligned}
\boldsymbol{w}^{\mathrm{T}} \mathbf{S}_{b}^{\phi} \boldsymbol{w}&=\left(\sum_{i=1}^{m} \alpha_{i} \phi\left(\boldsymbol{x}_{i}\right)\right)^{\mathrm{T}}\cdot\mathbf{S}_{b}^{\phi}\cdot \sum_{i=1}^{m} \alpha_{i} \phi\left(\boldsymbol{x}_{i}\right) \\
&=\sum_{i=1}^{m} \alpha_{i} \phi\left(\boldsymbol{x}_{i}\right)^{\mathrm{T}}\cdot\mathbf{S}_{b}^{\phi}\cdot \sum_{i=1}^{m} \alpha_{i} \phi\left(\boldsymbol{x}_{i}\right) \\
\end{aligned}
$$

 其中 

$$
\begin{aligned}
\mathbf{S}_{b}^{\phi} &=\left(\boldsymbol{\mu}_{1}^{\phi}-\boldsymbol{\mu}_{0}^{\phi}\right)\left(\boldsymbol{\mu}_{1}^{\phi}-\boldsymbol{\mu}_{0}^{\phi}\right)^{\mathrm{T}} \\
&=\left(\frac{1}{m_{1}} \sum_{\boldsymbol{x} \in X_{1}} \phi(\boldsymbol{x})-\frac{1}{m_{0}} \sum_{\boldsymbol{x} \in X_{0}} \phi(\boldsymbol{x})\right)\left(\frac{1}{m_{1}} \sum_{\boldsymbol{x} \in X_{1}} \phi(\boldsymbol{x})-\frac{1}{m_{0}} \sum_{\boldsymbol{x} \in X_{0}} \phi(\boldsymbol{x})\right)^{\mathrm{T}} \\
&=\left(\frac{1}{m_{1}} \sum_{\boldsymbol{x} \in X_{1}} \phi(\boldsymbol{x})-\frac{1}{m_{0}} \sum_{\boldsymbol{x} \in X_{0}} \phi(\boldsymbol{x})\right)\left(\frac{1}{m_{1}} \sum_{\boldsymbol{x} \in X_{1}} \phi(\boldsymbol{x})^{\mathrm{T}}-\frac{1}{m_{0}} \sum_{\boldsymbol{x} \in X_{0}} \phi(\boldsymbol{x})^{\mathrm{T}}\right) \\
\end{aligned}
$$

 将其代入上式可得 

$$
\begin{aligned}
\boldsymbol{w}^{\mathrm{T}} \mathbf{S}_{b}^{\phi} \boldsymbol{w}=&\sum_{i=1}^{m} \alpha_{i} \phi\left(\boldsymbol{x}_{i}\right)^{\mathrm{T}}\cdot\left(\frac{1}{m_{1}} \sum_{\boldsymbol{x} \in X_{1}} \phi(\boldsymbol{x})-\frac{1}{m_{0}} \sum_{\boldsymbol{x} \in X_{0}} \phi(\boldsymbol{x})\right)\cdot\left(\frac{1}{m_{1}} \sum_{\boldsymbol{x} \in X_{1}} \phi(\boldsymbol{x})^{\mathrm{T}}-\frac{1}{m_{0}} \sum_{\boldsymbol{x} \in X_{0}} \phi(\boldsymbol{x})^{\mathrm{T}}\right)\cdot \sum_{i=1}^{m} \alpha_{i} \phi\left(\boldsymbol{x}_{i}\right) \\
=&\left(\frac{1}{m_{1}} \sum_{\boldsymbol{x} \in X_{1}}\sum_{i=1}^{m} \alpha_{i} \phi\left(\boldsymbol{x}_{i}\right)^{\mathrm{T}} \phi(\boldsymbol{x})-\frac{1}{m_{0}} \sum_{\boldsymbol{x} \in X_{0}} \sum_{i=1}^{m} \alpha_{i} \phi\left(\boldsymbol{x}_{i}\right)^{\mathrm{T}}\phi(\boldsymbol{x})\right)\\
&\cdot\left(\frac{1}{m_{1}} \sum_{\boldsymbol{x} \in X_{1}} \sum_{i=1}^{m} \alpha_{i} \phi(\boldsymbol{x})^{\mathrm{T}}\phi\left(\boldsymbol{x}_{i}\right)-\frac{1}{m_{0}} \sum_{\boldsymbol{x} \in X_{0}} \sum_{i=1}^{m} \alpha_{i} \phi(\boldsymbol{x})^{\mathrm{T}}\phi\left(\boldsymbol{x}_{i}\right)\right) \\
\end{aligned}
$$


由于$\kappa\left(\boldsymbol{x}_i, \boldsymbol{x}\right)=\phi(\boldsymbol{x}_i)^{\mathrm{T}}\phi(\boldsymbol{x})$为标量，所以其转置等于本身，即$\kappa\left(\boldsymbol{x}_i, \boldsymbol{x}\right)=\phi(\boldsymbol{x}_i)^{\mathrm{T}}\phi(\boldsymbol{x})=\left(\phi(\boldsymbol{x}_i)^{\mathrm{T}}\phi(\boldsymbol{x})\right)^{\mathrm{T}}=\phi(\boldsymbol{x})^{\mathrm{T}}\phi(\boldsymbol{x}_i)=\kappa\left(\boldsymbol{x}_i, \boldsymbol{x}\right)^{\mathrm{T}}$，将其代入上式可得


$$
\begin{aligned}
\boldsymbol{w}^{\mathrm{T}} \mathbf{S}_{b}^{\phi} \boldsymbol{w}=&\left(\frac{1}{m_{1}} \sum_{i=1}^{m}\sum_{\boldsymbol{x} \in X_{1}}\alpha_{i} \kappa\left(\boldsymbol{x}_i, \boldsymbol{x}\right)-\frac{1}{m_{0}} \sum_{i=1}^{m} \sum_{\boldsymbol{x} \in X_{0}}  \alpha_{i} \kappa\left(\boldsymbol{x}_i, \boldsymbol{x}\right)\right)\\
&\cdot\left(\frac{1}{m_{1}} \sum_{i=1}^{m}\sum_{\boldsymbol{x} \in X_{1}} \alpha_{i} \kappa\left(\boldsymbol{x}_i, \boldsymbol{x}\right)-\frac{1}{m_{0}}\sum_{i=1}^{m}  \sum_{\boldsymbol{x} \in X_{0}} \alpha_{i} \kappa\left(\boldsymbol{x}_i, \boldsymbol{x}\right)\right)
\end{aligned}
$$


设$\boldsymbol{\alpha}=(\alpha_1;\alpha_2;...;\alpha_m)^{\mathrm{T}}\in \mathbb{R}^{m\times 1}$，同时结合式(6.66)的解释可得到$\hat{\boldsymbol{\mu}}_{0},\hat{\boldsymbol{\mu}}_{1}$的一般形式，上式可以化简为


$$
\begin{aligned}
\boldsymbol{w}^{\mathrm{T}} \mathbf{S}_{b}^{\phi} \boldsymbol{w}&=\left(\boldsymbol{\alpha}^{\mathrm{T}}\hat{\boldsymbol{\mu}}_{1}-\boldsymbol{\alpha}^{\mathrm{T}}\hat{\boldsymbol{\mu}}_{0}\right)\cdot\left(\hat{\boldsymbol{\mu}}_{1}^{\mathrm{T}}\boldsymbol{\alpha}-\hat{\boldsymbol{\mu}}_{0}^{\mathrm{T}}\boldsymbol{\alpha}\right)\\
&=\boldsymbol{\alpha}^{\mathrm{T}}\cdot\left(\hat{\boldsymbol{\mu}}_{1}-\hat{\boldsymbol{\mu}}_{0}\right)\cdot\left(\hat{\boldsymbol{\mu}}_{1}^{\mathrm{T}}-\hat{\boldsymbol{\mu}}_{0}^{\mathrm{T}}\right)\cdot\boldsymbol{\alpha}\\
&=\boldsymbol{\alpha}^{\mathrm{T}}\cdot\left(\hat{\boldsymbol{\mu}}_{1}-\hat{\boldsymbol{\mu}}_{0}\right)\cdot\left(\hat{\boldsymbol{\mu}}_{1}-\hat{\boldsymbol{\mu}}_{0}\right)^{\mathrm{T}}\cdot\boldsymbol{\alpha}\\
&=\boldsymbol{\alpha}^{\mathrm{T}} \mathbf{M} \boldsymbol{\alpha}\\
\end{aligned}
$$


以上便是式(6.70)分子部分的推导，下面继续推导式(6.70)的分母部分。将式(6.65)代入式(6.60)的分母可得：


$$
\begin{aligned}
\boldsymbol{w}^{\mathrm{T}} \mathbf{S}_{w}^{\phi} \boldsymbol{w}&=\left(\sum_{i=1}^{m} \alpha_{i} \phi\left(\boldsymbol{x}_{i}\right)\right)^{\mathrm{T}}\cdot\mathbf{S}_{w}^{\phi}\cdot \sum_{i=1}^{m} \alpha_{i} \phi\left(\boldsymbol{x}_{i}\right) \\
&=\sum_{i=1}^{m} \alpha_{i} \phi\left(\boldsymbol{x}_{i}\right)^{\mathrm{T}}\cdot\mathbf{S}_{w}^{\phi}\cdot \sum_{i=1}^{m} \alpha_{i} \phi\left(\boldsymbol{x}_{i}\right) \\
\end{aligned}
$$

 其中 

$$
\begin{aligned}
\mathbf{S}_{w}^{\phi}&=\sum_{i=0}^{1} \sum_{\boldsymbol{x} \in X_{i}}\left(\phi(\boldsymbol{x})-\boldsymbol{\mu}_{i}^{\phi}\right)\left(\phi(\boldsymbol{x})-\boldsymbol{\mu}_{i}^{\phi}\right)^{\mathrm{T}} \\
&=\sum_{i=0}^{1} \sum_{\boldsymbol{x} \in X_{i}}\left(\phi(\boldsymbol{x})-\boldsymbol{\mu}_{i}^{\phi}\right)\left(\phi(\boldsymbol{x})^{\mathrm{T}}-\left(\boldsymbol{\mu}_{i}^{\phi}\right)^{\mathrm{T}}\right) \\
&=\sum_{i=0}^{1} \sum_{\boldsymbol{x} \in X_{i}}\left(\phi(\boldsymbol{x})\phi(\boldsymbol{x})^{\mathrm{T}}-\phi(\boldsymbol{x})\left(\boldsymbol{\mu}_{i}^{\phi}\right)^{\mathrm{T}}-\boldsymbol{\mu}_{i}^{\phi}\phi(\boldsymbol{x})^{\mathrm{T}}+\boldsymbol{\mu}_{i}^{\phi}\left(\boldsymbol{\mu}_{i}^{\phi}\right)^{\mathrm{T}}\right) \\
&=\sum_{i=0}^{1} \sum_{\boldsymbol{x} \in X_{i}}\phi(\boldsymbol{x})\phi(\boldsymbol{x})^{\mathrm{T}}-\sum_{i=0}^{1} \sum_{\boldsymbol{x} \in X_{i}}\phi(\boldsymbol{x})\left(\boldsymbol{\mu}_{i}^{\phi}\right)^{\mathrm{T}}-\sum_{i=0}^{1} \sum_{\boldsymbol{x} \in X_{i}}\boldsymbol{\mu}_{i}^{\phi}\phi(\boldsymbol{x})^{\mathrm{T}}+\sum_{i=0}^{1} \sum_{\boldsymbol{x} \in X_{i}}\boldsymbol{\mu}_{i}^{\phi}\left(\boldsymbol{\mu}_{i}^{\phi}\right)^{\mathrm{T}} \\
\end{aligned}
$$

 由于 

$$
\begin{aligned}
\sum_{i=0}^{1} \sum_{\boldsymbol{x} \in X_{i}} \phi(\boldsymbol{x})\left(\boldsymbol{\mu}_{i}^{\phi}\right)^{\mathrm{T}} &=\sum_{\boldsymbol{x} \in X_{0}} \phi(\boldsymbol{x})\left(\boldsymbol{\mu}_{0}^{\phi}\right)^{\mathrm{T}}+\sum_{\boldsymbol{x} \in X_{1}} \phi(\boldsymbol{x})\left(\boldsymbol{\mu}_{1}^{\phi}\right)^{\mathrm{T}} \\
 &=m_{0} \boldsymbol{\mu}_{0}^{\phi}\left(\boldsymbol{\mu}_{0}^{\phi}\right)^{\mathrm{T}}+m_{1} \boldsymbol{\mu}_{1}^{\phi}\left(\boldsymbol{\mu}_{1}^{\phi}\right)^{\mathrm{T}}
\end{aligned}
$$

 且 

$$
\begin{aligned}
\sum_{i=0}^{1} \sum_{\boldsymbol{x} \in X_{i}} \boldsymbol{\mu}_{i}^{\phi} \phi(\boldsymbol{x})^{\mathrm{T}} &=\sum_{i=0}^{1} \boldsymbol{\mu}_{i}^{\phi} \sum_{\boldsymbol{x} \in X_{i}} \phi(\boldsymbol{x})^{\mathrm{T}} \\ &=\boldsymbol{\mu}_{0}^{\phi} \sum_{\boldsymbol{x} \in X_{0}} \phi(\boldsymbol{x})^{\mathrm{T}}+\boldsymbol{\mu}_{1}^{\phi} \sum_{\boldsymbol{x} \in X_{1}} \phi(\boldsymbol{x})^{\mathrm{T}} \\ &=m_{0} \boldsymbol{\mu}_{0}^{\phi}\left(\boldsymbol{\mu}_{0}^{\phi}\right)^{\mathrm{T}}+m_{1} \boldsymbol{\mu}_{1}^{\phi}\left(\boldsymbol{\mu}_{1}^{\phi}\right)^{\mathrm{T}}
\end{aligned}
$$

 所以 

$$
\begin{aligned}
\mathbf{S}_{w}^{\phi}&=\sum_{\boldsymbol{x} \in  D}\phi(\boldsymbol{x})\phi(\boldsymbol{x})^{\mathrm{T}}-2\left[m_0\boldsymbol{\mu}_{0}^{\phi}\left(\boldsymbol{\mu}_{0}^{\phi}\right)^{\mathrm{T}}+m_1\boldsymbol{\mu}_{1}^{\phi}\left(\boldsymbol{\mu}_{1}^{\phi}\right)^{\mathrm{T}}\right]+m_0 \boldsymbol{\mu}_{0}^{\phi}\left(\boldsymbol{\mu}_{0}^{\phi}\right)^{\mathrm{T}}+m_1 \boldsymbol{\mu}_{1}^{\phi}\left(\boldsymbol{\mu}_{1}^{\phi}\right)^{\mathrm{T}} \\
&=\sum_{\boldsymbol{x} \in  D}\phi(\boldsymbol{x})\phi(\boldsymbol{x})^{\mathrm{T}}-m_0\boldsymbol{\mu}_{0}^{\phi}\left(\boldsymbol{\mu}_{0}^{\phi}\right)^{\mathrm{T}}-m_1\boldsymbol{\mu}_{1}^{\phi}\left(\boldsymbol{\mu}_{1}^{\phi}\right)^{\mathrm{T}}\\
\end{aligned}
$$


再将此式代回$\boldsymbol{w}^{\mathrm{T}} \mathbf{S}_{w}^{\phi} \boldsymbol{w}$可得


$$
\begin{aligned}
\boldsymbol{w}^{\mathrm{T}} \mathbf{S}_{w}^{\phi} \boldsymbol{w}=&\sum_{i=1}^{m} \alpha_{i} \phi\left(\boldsymbol{x}_{i}\right)^{\mathrm{T}}\cdot\mathbf{S}_{w}^{\phi}\cdot \sum_{i=1}^{m} \alpha_{i} \phi\left(\boldsymbol{x}_{i}\right) \\
=&\sum_{i=1}^{m} \alpha_{i} \phi\left(\boldsymbol{x}_{i}\right)^{\mathrm{T}}\cdot\left(\sum_{\boldsymbol{x} \in  D}\phi(\boldsymbol{x})\phi(\boldsymbol{x})^{\mathrm{T}}-m_0\boldsymbol{\mu}_{0}^{\phi}\left(\boldsymbol{\mu}_{0}^{\phi}\right)^{\mathrm{T}}-m_1\boldsymbol{\mu}_{1}^{\phi}\left(\boldsymbol{\mu}_{1}^{\phi}\right)^{\mathrm{T}}\right)\cdot \sum_{i=1}^{m} \alpha_{i} \phi\left(\boldsymbol{x}_{i}\right) \\
=&\sum_{i=1}^{m}\sum_{j=1}^{m}\sum_{\boldsymbol{x} \in  D}\alpha_{i} \phi\left(\boldsymbol{x}_{i}\right)^{\mathrm{T}}\phi(\boldsymbol{x})\phi(\boldsymbol{x})^{\mathrm{T}}\alpha_{j} \phi\left(\boldsymbol{x}_{j}\right)-\sum_{i=1}^{m}\sum_{j=1}^{m}\alpha_{i} \phi\left(\boldsymbol{x}_{i}\right)^{\mathrm{T}}m_0\boldsymbol{\mu}_{0}^{\phi}\left(\boldsymbol{\mu}_{0}^{\phi}\right)^{\mathrm{T}}\alpha_{j} \phi\left(\boldsymbol{x}_{j}\right)\\
&-\sum_{i=1}^{m}\sum_{j=1}^{m}\alpha_{i} \phi\left(\boldsymbol{x}_{i}\right)^{\mathrm{T}}m_1\boldsymbol{\mu}_{1}^{\phi}\left(\boldsymbol{\mu}_{1}^{\phi}\right)^{\mathrm{T}}\alpha_{j} \phi\left(\boldsymbol{x}_{j}\right) \\
\end{aligned}
$$

 其中，第1项 

$$
\begin{aligned}
\sum_{i=1}^{m}\sum_{j=1}^{m}\sum_{\boldsymbol{x} \in  D}\alpha_{i} \phi\left(\boldsymbol{x}_{i}\right)^{\mathrm{T}}\phi(\boldsymbol{x})\phi(\boldsymbol{x})^{\mathrm{T}}\alpha_{j} \phi\left(\boldsymbol{x}_{j}\right)&=\sum_{i=1}^{m}\sum_{j=1}^{m}\sum_{\boldsymbol{x} \in  D}\alpha_{i} \alpha_{j}\kappa\left(\boldsymbol{x}_i, \boldsymbol{x}\right)\kappa\left(\boldsymbol{x}_j, \boldsymbol{x}\right)\\
&=\boldsymbol{\alpha}^{\mathrm{T}} \mathbf{K} \mathbf{K}^{\mathrm{T}} \boldsymbol{\alpha}
\end{aligned}
$$

 第2项 

$$
\begin{aligned}
\sum_{i=1}^{m}\sum_{j=1}^{m}\alpha_{i} \phi\left(\boldsymbol{x}_{i}\right)^{\mathrm{T}}m_0\boldsymbol{\mu}_{0}^{\phi}\left(\boldsymbol{\mu}_{0}^{\phi}\right)^{\mathrm{T}}\alpha_{j} \phi\left(\boldsymbol{x}_{j}\right)&=m_0\sum_{i=1}^{m}\sum_{j=1}^{m}\alpha_{i}\alpha_{j}\phi\left(\boldsymbol{x}_{i}\right)^{\mathrm{T}}\boldsymbol{\mu}_{0}^{\phi}\left(\boldsymbol{\mu}_{0}^{\phi}\right)^{\mathrm{T}} \phi\left(\boldsymbol{x}_{j}\right)\\
&=m_0\sum_{i=1}^{m}\sum_{j=1}^{m}\alpha_{i}\alpha_{j}\phi\left(\boldsymbol{x}_{i}\right)^{\mathrm{T}}\left[\frac{1}{m_{0}} \sum_{\boldsymbol{x} \in X_{0}} \phi(\boldsymbol{x})\right]\left[\frac{1}{m_{0}} \sum_{\boldsymbol{x} \in X_{0}} \phi(\boldsymbol{x})\right]^{\mathrm{T}} \phi\left(\boldsymbol{x}_{j}\right)\\
&=m_0\sum_{i=1}^{m}\sum_{j=1}^{m}\alpha_{i}\alpha_{j}\left[\frac{1}{m_{0}} \sum_{\boldsymbol{x} \in X_{0}} \phi\left(\boldsymbol{x}_{i}\right)^{\mathrm{T}}\phi(\boldsymbol{x})\right]\left[\frac{1}{m_{0}} \sum_{\boldsymbol{x} \in X_{0}} \phi(\boldsymbol{x})^{\mathrm{T}}\phi\left(\boldsymbol{x}_{j}\right)\right] \\
&=m_0\sum_{i=1}^{m}\sum_{j=1}^{m}\alpha_{i}\alpha_{j}\left[\frac{1}{m_{0}} \sum_{\boldsymbol{x} \in X_{0}} \kappa\left(\boldsymbol{x}_i, \boldsymbol{x}\right)\right]\left[\frac{1}{m_{0}} \sum_{\boldsymbol{x} \in X_{0}} \kappa\left(\boldsymbol{x}_j, \boldsymbol{x}\right)\right] \\
&=m_0\boldsymbol{\alpha}^{\mathrm{T}} \hat{\boldsymbol{\mu}}_{0} \hat{\boldsymbol{\mu}}_{0}^{\mathrm{T}} \boldsymbol{\alpha}
\end{aligned}
$$

 同理，有第3项


$$
\sum_{i=1}^{m}\sum_{j=1}^{m}\alpha_{i} \phi\left(\boldsymbol{x}_{i}\right)^{\mathrm{T}}m_1\boldsymbol{\mu}_{1}^{\phi}\left(\boldsymbol{\mu}_{1}^{\phi}\right)^{\mathrm{T}}\alpha_{j} \phi\left(\boldsymbol{x}_{j}\right)=m_1\boldsymbol{\alpha}^{\mathrm{T}} \hat{\boldsymbol{\mu}}_{1} \hat{\boldsymbol{\mu}}_{1}^{\mathrm{T}} \boldsymbol{\alpha}
$$


将上述三项的化简结果代回再将此式代回$\boldsymbol{w}^{\mathrm{T}} \mathbf{S}_{w}^{\phi} \boldsymbol{w}$可得


$$
\begin{aligned}
\boldsymbol{w}^{\mathrm{T}} \mathbf{S}_{w}^{\phi} \boldsymbol{w}&=\boldsymbol{\alpha}^{\mathrm{T}} \mathbf{K} \mathbf{K}^{\mathrm{T}} \boldsymbol{\alpha}-m_0\boldsymbol{\alpha}^{\mathrm{T}} \hat{\boldsymbol{\mu}}_{0} \hat{\boldsymbol{\mu}}_{0}^{\mathrm{T}} \boldsymbol{\alpha}-m_1\boldsymbol{\alpha}^{\mathrm{T}} \hat{\boldsymbol{\mu}}_{1} \hat{\boldsymbol{\mu}}_{1}^{\mathrm{T}} \boldsymbol{\alpha}\\
&=\boldsymbol{\alpha}^{\mathrm{T}} \cdot\left(\mathbf{K} \mathbf{K}^{\mathrm{T}} -m_0\hat{\boldsymbol{\mu}}_{0} \hat{\boldsymbol{\mu}}_{0}^{\mathrm{T}} -m_1\hat{\boldsymbol{\mu}}_{1} \hat{\boldsymbol{\mu}}_{1}^{\mathrm{T}} \right)\cdot\boldsymbol{\alpha}\\
&=\boldsymbol{\alpha}^{\mathrm{T}} \cdot\left(\mathbf{K} \mathbf{K}^{\mathrm{T}}-\sum_{i=0}^{1} m_{i} \hat{\boldsymbol{\mu}}_{i} \hat{\boldsymbol{\mu}}_{i}^{\mathrm{T}} \right)\cdot\boldsymbol{\alpha}\\
&=\boldsymbol{\alpha}^{\mathrm{T}} \mathbf{N}\boldsymbol{\alpha}\\
\end{aligned}
$$



### 6.6.5 核对数几率回归

将"对数几率回归与支持向量机的关系"中最后得到的对数几率回归重写为如下形式


$$
\min_{\boldsymbol{w}, b} \frac{1}{m} \sum_{i=1}^m \log \left(1+e^{-y_i\left(\boldsymbol{w}^{\mathrm{T}} \boldsymbol{x}_i+b\right)}\right)+\frac{\lambda}{2 m}\|\boldsymbol{w}\|^2
$$


其中$\lambda$是用来调整正则项权重的正则化常数。假设$\boldsymbol{z}_i=\phi(\boldsymbol{x}_i)$是由原始空间经核函数映射到高维空间的特征向量，则


$$
\min_{\boldsymbol{w}, b} \frac{1}{m} \sum_{i=1}^m \log \left(1+e^{-y_i\left(\boldsymbol{w}^{\mathrm{T}} \boldsymbol{z}_i+b\right)}\right)+\frac{\lambda}{2 m}\|\boldsymbol{w}\|^2
$$


注意，以上两式中的$\boldsymbol{w}$维度是不同的，其分别与$\boldsymbol{x}_i$和$\boldsymbol{z}_i$的维度一致。根据表示定理，上式的解可以写为


$$
\boldsymbol{w}=\sum_{j=1}^{m}\alpha_j\boldsymbol{z}_j
$$


将$\boldsymbol{w}$代入对数几率回归可得


$$
\min _{\boldsymbol{w}, b} \frac{1}{m} \sum_{i=1}^m \log \left(1+e^{-y_i\left(\sum_{j=1}^m \alpha_j \boldsymbol{z}_j^{\mathrm{T}} \boldsymbol{z}_i+b\right)}\right)+\frac{\lambda}{2 m} \sum_{i=1}^m \sum_{j=1}^m \alpha_i \alpha_j \boldsymbol{z}_i^{\mathrm{T}} \boldsymbol{z}_j
$$


用核函数$\kappa\left(\boldsymbol{x}_i, \boldsymbol{x}_j\right)=\boldsymbol{z}_i^{\mathrm{T}} \boldsymbol{z}_j=\phi\left(\boldsymbol{x}_i\right)^{\mathrm{T}} \phi\left(\boldsymbol{x}_j\right)$替换上式中的内积运算


$$
\min _{\boldsymbol{w}, b} \frac{1}{m} \sum_{i=1}^m \log \left(1+e^{-y_i\left(\sum_{j=1}^m \alpha_j \kappa\left(\boldsymbol{x}_i, \boldsymbol{x}_j\right)+b\right)}\right)+\frac{\lambda}{2 m} \sum_{i=1}^m \sum_{j=1}^m \alpha_i \alpha_j \kappa\left(\boldsymbol{x}_i, \boldsymbol{x}_j\right)
$$


解出$\boldsymbol{\alpha}=(\alpha_1,\alpha_2,...,\alpha_m)$和$b$后，即可得$f(\boldsymbol{x})=\sum_{i=1}^{m}\alpha_i\kappa(\boldsymbol{x},\boldsymbol{x}_i)+b$。


## 参考文献

[1] 王燕军. 最优化基础理论与方法. 复旦大学出版社, 2011.

[2] 王书宁. 凸优化. 清华大学出版社, 2013.
