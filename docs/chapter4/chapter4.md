## 4.1
$$\operatorname{Ent}(D)=-\sum_{k=1}^{|\mathcal{Y}|}p_k\log_{2}{p_k}$$
[解析]：证明$0\leq\operatorname{Ent}(D)\leq\log_{2}|\mathcal{Y}|$：
已知集合$D$的信息熵的定义为
$$\operatorname{Ent}(D)=-\sum_{k=1}^{|\mathcal{Y}|} p_{k} \log _{2} p_{k}$$
其中，$|\mathcal{Y}|$表示样本类别总数，$p_k$表示第$k$类样本所占的比例，且$0 \leq p_k \leq 1,\sum_{k=1}^{n}p_k=1$。若令$|\mathcal{Y}|=n,p_k=x_k$，那么信息熵$\operatorname{Ent}(D)$就可以看作一个$n$元实值函数，也即
$$\operatorname{Ent}(D)=f(x_1,...,x_n)=-\sum_{k=1}^{n} x_{k} \log _{2} x_{k} $$
其中，$0 \leq x_k \leq 1,\sum_{k=1}^{n}x_k=1$，下面考虑求该多元函数的最值。首先我们先来求最大值，如果不考虑约束$0 \leq x_k \leq 1$，仅考虑$\sum_{k=1}^{n}x_k=1$的话，对$f(x_1,...,x_n)$求最大值等价于如下最小化问题
$$\begin{array}{ll}{
\operatorname{min}} & {\sum\limits_{k=1}^{n} x_{k} \log _{2} x_{k} } \\ 
{\text { s.t. }} & {\sum\limits_{k=1}^{n}x_k=1} 
\end{array}$$
显然，在$0 \leq x_k \leq 1$时，此问题为凸优化问题，而对于凸优化问题来说，能令其拉格朗日函数的一阶偏导数等于0的点即为最优解。根据拉格朗日乘子法可知，该优化问题的拉格朗日函数为
$$L(x_1,...,x_n,\lambda)=\sum_{k=1}^{n} x_{k} \log _{2} x_{k}+\lambda(\sum_{k=1}^{n}x_k-1)$$
其中，$\lambda$为拉格朗日乘子。对$L(x_1,...,x_n,\lambda)$分别关于$x_1,...,x_n,\lambda$求一阶偏导数，并令偏导数等于0可得
$$\begin{aligned}
\cfrac{\partial L(x_1,...,x_n,\lambda)}{\partial x_1}&=\cfrac{\partial }{\partial x_1}\left[\sum_{k=1}^{n} x_{k} \log _{2} x_{k}+\lambda(\sum_{k=1}^{n}x_k-1)\right]=0\\
&=\log _{2} x_{1}+x_1\cdot \cfrac{1}{x_1\ln2}+\lambda=0 \\
&=\log _{2} x_{1}+\cfrac{1}{\ln2}+\lambda=0 \\
&\Rightarrow \lambda=-\log _{2} x_{1}-\cfrac{1}{\ln2}\\
\cfrac{\partial L(x_1,...,x_n,\lambda)}{\partial x_2}&=\cfrac{\partial }{\partial x_2}\left[\sum_{k=1}^{n} x_{k} \log _{2} x_{k}+\lambda(\sum_{k=1}^{n}x_k-1)\right]=0\\
&\Rightarrow \lambda=-\log _{2} x_{2}-\cfrac{1}{\ln2}\\
\vdots\\
\cfrac{\partial L(x_1,...,x_n,\lambda)}{\partial x_n}&=\cfrac{\partial }{\partial x_n}\left[\sum_{k=1}^{n} x_{k} \log _{2} x_{k}+\lambda(\sum_{k=1}^{n}x_k-1)\right]=0\\
&\Rightarrow \lambda=-\log _{2} x_{n}-\cfrac{1}{\ln2}\\
\cfrac{\partial L(x_1,...,x_n,\lambda)}{\partial \lambda}&=\cfrac{\partial }{\partial \lambda}\left[\sum_{k=1}^{n} x_{k} \log _{2} x_{k}+\lambda(\sum_{k=1}^{n}x_k-1)\right]=0\\
&\Rightarrow \sum_{k=1}^{n}x_k=1\\
\end{aligned}$$
整理一下可得
$$\left\{ \begin{array}{lr}
\lambda=-\log _{2} x_{1}-\cfrac{1}{\ln2}=-\log _{2} x_{2}-\cfrac{1}{\ln2}=...=-\log _{2} x_{n}-\cfrac{1}{\ln2} \\
\sum\limits_{k=1}^{n}x_k=1
\end{array}\right.$$
由以上两个方程可以解得
$$x_1=x_2=...=x_n=\cfrac{1}{n}$$
又因为$x_k$还需满足约束$0 \leq x_k \leq 1$，显然$0 \leq\cfrac{1}{n}\leq 1$，所以$x_1=x_2=...=x_n=\cfrac{1}{n}$是满足所有约束的最优解，也即为当前最小化问题的最小值点，同时也是$f(x_1,...,x_n)$的最大值点。将$x_1=x_2=...=x_n=\cfrac{1}{n}$代入$f(x_1,...,x_n)$中可得
$$f(\cfrac{1}{n},...,\cfrac{1}{n})=-\sum_{k=1}^{n} \cfrac{1}{n} \log _{2} \cfrac{1}{n}=-n\cdot\cfrac{1}{n} \log _{2} \cfrac{1}{n}=\log _{2} n$$
所以$f(x_1,...,x_n)$在满足约束$0 \leq x_k \leq 1,\sum_{k=1}^{n}x_k=1$时的最大值为$\log _{2} n$。求完最大值后下面我们再来求最小值，如果不考虑约束$\sum_{k=1}^{n}x_k=1$，仅考虑$0 \leq x_k \leq 1$的话，$f(x_1,...,x_n)$可以看做是$n$个互不相关的一元函数的加和，也即
$$f(x_1,...,x_n)=\sum_{k=1}^{n} g(x_k) $$
其中，$g(x_k)=-x_{k} \log _{2} x_{k},0 \leq x_k \leq 1$。那么当$g(x_1),g(x_2),...,g(x_n)$分别取到其最小值时，$f(x_1,...,x_n)$也就取到了最小值。所以接下来考虑分别求$g(x_1),g(x_2),...,g(x_n)$各自的最小值，由于$g(x_1),g(x_2),...,g(x_n)$的定义域和函数表达式均相同，所以只需求出$g(x_1)$的最小值也就求出了$g(x_2),...,g(x_n)$的最小值。下面考虑求$g(x_1)$的最小值，首先对$g(x_1)$关于$x_1$求一阶和二阶导数
$$g^{\prime}(x_1)=\cfrac{d(-x_{1} \log _{2} x_{1})}{d x_1}=-\log _{2} x_{1}-x_1\cdot \cfrac{1}{x_1\ln2}=-\log _{2} x_{1}-\cfrac{1}{\ln2}$$
$$g^{\prime\prime}(x_1)=\cfrac{d\left(g^{\prime}(x_1)\right)}{d x_1}=\cfrac{d\left(-\log _{2} x_{1}-\cfrac{1}{\ln2}\right)}{d x_1}=-\cfrac{1}{x_{1}\ln2}$$
显然，当$0 \leq x_k \leq 1$时$g^{\prime\prime}(x_1)=-\cfrac{1}{x_{1}\ln2}$恒小于0，所以$g(x_1)$是一个在其定义域范围内开口向下的凹函数，那么其最小值必然在边界取，于是分别取$x_1=0$和$x_1=1$，代入$g(x_1)$可得
$$g(0)=-0\log _{2} 0=0$$
$$g(1)=-1\log _{2} 1=0$$
所以，$g(x_1)$的最小值为0，同理可得$g(x_2),...,g(x_n)$的最小值也为0，那么$f(x_1,...,x_n)$的最小值此时也为0。但是，此时是不考虑约束$\sum_{k=1}^{n}x_k=1$，仅考虑$0 \leq x_k \leq 1$时取到的最小值，若考虑约束$\sum_{k=1}^{n}x_k=1$的话，那么$f(x_1,...,x_n)$的最小值一定大于等于0。如果令某个$x_k=1$，那么根据约束$\sum_{k=1}^{n}x_k=1$可知$x_1=x_2=...=x_{k-1}=x_{k+1}=...=x_n=0$，将其代入$f(x_1,...,x_n)$可得
$$f(0,0,...,0,1,0,...,0)=-0 \log _{2}0-0 \log _{2}0...-0 \log _{2}0-1 \log _{2}1-0 \log _{2}0...-0 \log _{2}0=0 $$
所以$x_k=1,x_1=x_2=...=x_{k-1}=x_{k+1}=...=x_n=0$一定是$f(x_1,...,x_n)$在满足约束$\sum_{k=1}^{n}x_k=1$和$0 \leq x_k \leq 1$的条件下的最小值点，其最小值为0。<br>
综上可知，当$f(x_1,...,x_n)$取到最大值时：$x_1=x_2=...=x_n=\cfrac{1}{n}$，此时样本集合纯度最低；当$f(x_1,...,x_n)$取到最小值时：$x_k=1,x_1=x_2=...=x_{k-1}=x_{k+1}=...=x_n=0$，此时样本集合纯度最高。
## 4.2
$$\operatorname{Gain}(D,a) = \operatorname{Ent}(D) - \sum_{v=1}^{V}\frac{|D^v|}{|D|}\operatorname{Ent}({D^v})$$
[解析]：这个是信息增益的定义公式，在信息论中信息增益也称为互信息（参见附录①），其表示已知一个随机变量的信息后使得另一个随机变量的不确定性减少的程度。所以在这里，这个公式可以理解为在属性$a$的取值已知后数据集$D$中类别$k$的不确定性减小的程度。若根据某个属性计算得到的信息增益越大，则说明在知道其取值后样本集的不确定性减小的程度越大，也即为书上所说的“纯度提升”越大。

## 4.6
$$\operatorname{Gini\_index}(D,a) = \sum_{v=1}^{V}\frac{|D^v|}{|D|}\operatorname{Gini}(D^v)$$
[解析]：这个是数据集$D$中属性$a$的基尼指数的定义，它表示在属性$a$的取值已知的条件下，数据集$D$按照属性$a$的所有可能取值划分后的纯度，不过在构造CART分类树时并不会严格按照此公式来选择最优划分属性，主要是因为CART分类树是一颗二叉树，如果用上面的公式去选出最优划分属性，无法进一步选出最优划分属性的最优划分点。CART分类树的构造算法如下：
- 首先，对每个属性$a$的每个可能取值$v$，将数据集$D$分为$a=v$和$a\neq v$两部分来计算基尼指数，即
$$\operatorname{Gini\_index}(D,a) = \frac{|D^{a=v}|}{|D|}\operatorname{Gini}(D^{a=v})+\frac{|D^{a\neq v}|}{|D|}\operatorname{Gini}(D^{a\neq v})$$
- 然后，选择基尼指数最小的属性及其对应取值作为最优划分属性和最优划分点；
- 最后，重复以上两步，直至满足停止条件。

下面以西瓜书中表4.2中西瓜数据集2.0为例来构造CART分类树，其中第一个最优划分属性和最优划分点的计算过程如下：
以属性“色泽”为例，它有3个可能的取值：$\{\text{青绿}，\text{乌黑}，\text{浅白}\}$， 若使用该属性的属性值是否等于“青绿”对数据集$D$进行划分，则可得到2个子集，分别记为$D^1(\text{色泽}=\text{青绿}),D^2(\text{色泽}\not=\text{青绿})$。子集$D^1$包含编号$\{1,4,6,10,13,17\}$共6个样例，其中正例占$p_1=\frac{3}{6}$，反例占$p_2=\frac{3}{6}$；子集$D^2$包含编号$\{2,3,5,7,8,9,11,12,14,15,16\}$共11个样例，其中正例占$p_1=\frac{5}{11}$，反例占$p_2=\frac{6}{11}$，根据公式（4.5）可计算出用“色泽=青绿”划分之后得到基尼指数为
$$\operatorname{Gini\_index}(D,\text{色泽}=\text{青绿}) = \frac{6}{17}\times\left(1-(\frac{3}{6})^2-(\frac{3}{6})^2\right)+\frac{11}{17}\times\left(1-(\frac{5}{11})^2-(\frac{6}{11})^2\right) 
= 0.497$$
类似的，可以计算出以下不同属性取不同值的基尼指数
$$\begin{aligned}
\operatorname{Gini\_index}(D,\text{色泽}=\text{乌黑}) = \frac{6}{17}\times\left(1-(\frac{4}{6})^2-(\frac{2}{6})^2\right)+\frac{11}{17}\times\left(1-(\frac{4}{11})^2-(\frac{7}{11})^2\right) 
= 0.456\\
\operatorname{Gini\_index}(D,\text{色泽}=\text{浅白}) = \frac{5}{17}\times\left(1-(\frac{1}{5})^2-(\frac{4}{5})^2\right)+\frac{12}{17}\times\left(1-(\frac{7}{12})^2-(\frac{5}{12})^2\right) 
= 0.426
\end{aligned}$$
$$\begin{aligned}
\operatorname{Gini\_index}(D,\text{根蒂}=\text{蜷缩}) = 0.456\\
\operatorname{Gini\_index}(D,\text{根蒂}=\text{稍蜷}) = 0.496\\
\operatorname{Gini\_index}(D,\text{根蒂}=\text{硬挺}) = 0.439\\
\operatorname{Gini\_index}(D,\text{敲声}=\text{浊响}) = 0.450\\
\operatorname{Gini\_index}(D,\text{敲声}=\text{沉闷}) = 0.494\\
\operatorname{Gini\_index}(D,\text{敲声}=\text{清脆}) = 0.439\\
\operatorname{Gini\_index}(D,\text{纹理}=\text{清晰}) = 0.286\\
\operatorname{Gini\_index}(D,\text{纹理}=\text{稍稀}) = 0.437\\
\operatorname{Gini\_index}(D,\text{纹理}=\text{模糊}) = 0.403\\
\operatorname{Gini\_index}(D,\text{脐部}=\text{凹陷}) = 0.415\\
\operatorname{Gini\_index}(D,\text{脐部}=\text{稍凹}) = 0.497\\
\operatorname{Gini\_index}(D,\text{脐部}=\text{平坦}) = 0.362\\
\operatorname{Gini\_index}(D,\text{触感}=\text{硬挺}) = 0.494\\
\operatorname{Gini\_index}(D,\text{触感}=\text{软粘}) = 0.494
\end{aligned}$$ 
特别地，对于属性“触感”，由于它的可取值个数为2，所以其实只需计算其中一个取值的基尼指数即可。根据上面的计算结果可知$\operatorname{Gini\_index}(D,\text{纹理}=\text{清晰}) = 0.286$最小，所以选择属性“纹理”为最优划分属性并生成根节点，接着以“纹理=清晰”为最优划分点生成$D^1(\text{纹理}=\text{清晰}),D^2(\text{纹理}\not=\text{清晰})$两个子节点，对于两个子节点分别重复上述步骤继续生成下一层子节点，直至满足停止条件。以上便是CART分类树的构建过程，从构建过程中可以看出，CART分类树最终构造出来的是一颗二叉树。CART决策树除了能处理分类问题以外，它还可以处理回归问题，附录②中给出了CART回归树的构造算法。

## 4.7
$$T_a={\lbrace{\frac{a^i+a^{i+1}}{2}|1\leq{i}\leq{n-1}}\rbrace}$$
[解析]：这个公式所表达的思想很简单，就是以每两个相邻取值的中点作为划分点，下面以西瓜书中表4.3中西瓜数据集3.0为例来说明此公式的用法。对于“密度”这个连续属性，已观测到的可能取值为$\{0.243,0.245,0.343,0.360,0.403,0.437,0.481,0.556,0.593,0.608,0.634,0.639,0.657,0.666,0.697,0.719,0.774\}$共17个值，根据公式（4.7）可知，此时$i$依次取1到16，那么“密度”这个属性的候选划分点集合为
$T_{a} = \{\frac{(0.243+0.245)}{2}, \frac{(0.245+0.343)}{2},\frac{(0.343+0.360)}{2},\frac{(0.360+0.403)}{2},\frac{(0.403+0.437)}{2},\frac{(0.437+0.481)}{2},\frac{(0.481+0.556)}{2},\\\frac{(0.556+0.593)}{2},\frac{(0.593+0.608)}{2},\frac{(0.608+0.634)}{2},\frac{(0.634+0.639)}{2},\frac{(0.639+0.657)}{2},\frac{(0.657+0.666)}{2},\frac{(0.666+0.697)}{2},\frac{(0.697+0.719)}{2},\frac{(0.719+0.774)}{2}\}$

## 4.8
$$\begin{aligned}
\operatorname{Gain}(D,a) &= \max_{t\in{T_a}}\operatorname{Gain}(D,a,t)\\
&=
\max_{t\in{T_a}}\operatorname{Ent}(D)-\sum_{\lambda\in\{-,+\}}\frac{|D_t^{\lambda}|}
{|D|}\operatorname{Ent}(D_t^{\lambda})
\end{aligned}$$
[解析]：此公式是公式（4.2）用于离散化后的连续属性的版本，其中$T_a$由公式（4.7）计算得来，$\lambda\in\{-,+\}$表示属性$a$的取值分别小于等于和大于候选划分点$t$时的情形，也即当$\lambda=-$时：$D^{\lambda}_t=D^{a\leq t}_t$，当$\lambda=+$时：$D^{\lambda}_t=D^{a> t}_t$。
           
## 附录
### ①互信息<sup>[1]</sup>
在解释互信息之前，需要先解释一下什么是条件熵。条件熵表示的是在已知一个随机变量的条件下，另一个随机变量的不确定性。具体地，假设有随机变量$X$和$Y$，且它们服从以下联合概率分布
$$P(X = x_{i},Y = y_{j}) = p_{ij}\quad i = 1,2,....,n;j = 1,2,...,m$$
那么在已知$X$的条件下，随机变量$Y$的条件熵为
$$\operatorname{Ent}(Y|X) =  \sum_{i=1}^np_i \operatorname{Ent}(Y|X = x_i)$$
其中，$ p_i = P(X = x_i) ，i =1,2,...,n$。互信息定义为信息熵和条件熵的差，它表示的是已知一个随机变量的信息后使得另一个随机变量的不确定性减少的程度。具体地，假设有随机变量$X$和$Y$，那么在已知$X$的信息后，$Y$的不确定性减少的程度为
$$\operatorname{I}(Y;X) = \operatorname{Ent}(Y) - \operatorname{Ent}(Y|X)$$
此即为互信息的数学定义。

### ②CART回归树<sup>[1]</sup>
假设给定数据集
$$D = {(\boldsymbol{x}_1,y_1),(\boldsymbol{x}_2,y_2)...,(\boldsymbol{x}_N,y_N)}$$
其中$\boldsymbol{x}\in \mathbb{R}^d$为$d$维特征向量，$y\in \mathbb{R}$是连续型随机变量，这是一个标准的回归问题的数据集。若把每个属性视为坐标空间中的一个坐标轴，则$d$个属性就构成了一个$d$维的特征空间，而每个$d$维特征向量$\boldsymbol{x}$就对应了$d$维的特征空间中的一个数据点。CART回归树的目标是将特征空间划分成若干个子空间，每个子空间都有一个固定的输出值，也就是凡是落在同一个子空间内的数据点$\boldsymbol{x}_i$，他们所对应的输出值$y_i$恒相等，且都为该子空间的输出值。那么如何划分出若干个子空间呢？这里采用一种启发式的方法：
- 任意选择一个属性$a$，遍历其所有可能取值，根据如下公式找出属性$a$最优划分点$v^*$：
$$v^* = \arg\min_{v}\left[\min_{c_1}\sum_{\boldsymbol {x}_i\in{R_1(a,v)}}{(y_i - c_1)}^2 + \min_{c_2}\sum_{\boldsymbol {x}_i\in{R_2(a,v)}}{(y_i - c_2)}^2 \right]$$
其中，$R_1(a,v)=\{\boldsymbol {x}|\boldsymbol {x}\in D^{a\leq v}\},R_2(a,v)=\{\boldsymbol {x}|\boldsymbol {x}\in D^{a > v}\}$，$c_1$和$c_2$分别为集合$R_1(a,v)$和$R_2(a,v)$中的样本$\boldsymbol {x}_i$对应的输出值$y_i$的均值，也即
$$c_1=\operatorname{ave}(y_i | x\in R_1(a,v))=\frac{1}{|R_1(a,v)|}\sum_{\boldsymbol {x}_i\in{R_1(a,v)}}y_i$$
$$c_2=\operatorname{ave}(y_i | x\in R_2(a,v))=\frac{1}{|R_2(a,v)|}\sum_{\boldsymbol {x}_i\in{R_2(a,v)}}y_i$$
- 遍历所有属性，找到最优划分属性$a^*$，然后根据$a^*$的最优划分点$v^*$将特征空间划分为两个子空间，接着对每个子空间重复上述步骤，直至满足停止条件。这样就生成了一颗CART回归树，假设最终将特征空间被划分为了$M$个子空间$R_1,R_2,...,R_M$，那么CART回归树的模型公式可以表示为
$$f(\boldsymbol {x}) = \sum_{m=1}^{M}c_m\mathbb{I}(x\in{R_m})$$
同理，其中的$c_m$表示的也是集合$R_m$中的样本$\boldsymbol {x}_i$对应的输出值$y_i$的均值。此公式直观上的理解就是，对于一个给定的样本$\boldsymbol {x}_i$，首先判断其属于哪个子空间，然后将其所属的子空间对应的输出值作为该样本的预测值$y_i$。

## 参考文献
[1]李航编著.统计学习方法[M].清华大学出版社,2012.

