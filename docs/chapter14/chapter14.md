# 第14章 概率图模型

本章介绍概率图模型，前三节分别介绍了有向图模型之隐马尔可夫模型以及无向图模型之马尔可夫随机场和条件随机场；接下来两节分别介绍精确推断和近似推断；最后一节简单介绍了话题模型的典型代表隐狄利克雷分配模型(LDA)。

## 14.1 隐马尔可夫模型

本节前三段内容实际上是本章的概述，从第四段才开始介绍"隐马尔可夫模型"。马尔可夫的大名相信很多人听说过，比如马尔可夫链；虽然隐马尔可夫模型与马尔可夫链并非同一人提出，但其中关键字"马尔可夫"蕴含的概念是相同的，即系统下一时刻的状态仅由当前状态决定。

### 14.1.1 生成式模型和判别式模型

一般来说, 机器学习的任务是根据输入特征 $\boldsymbol{x}$ 预测输出变量
$y$; 生成式模型最终求得联 合概率 $P(\boldsymbol{x}, y)$,
而判别式模型最终求得条件概率 $P(y \mid \boldsymbol{x})$ 。

统计机器学习算法都是基于样本独立同分布(independent and identically
distributed, 简 称 $i . i . d$. .)的假设, 也就是说,
假设样本空间中全体样本服从一个末知的 "分布" $\mathcal{D}$, 我们获
得的每个样本都是独立地从这个分布上采样获得的。

对于一个样本 $(\boldsymbol{x}, y)$, 联合概率 $P(\boldsymbol{x}, y)$
表示从样本空间中采样得到该样本的概率; 因 为 $P(\boldsymbol{x}, y)$ 表示
"生成" 样本本身的概率, 故称之为 "生成式模型"。而条件概率
$P(y \mid \boldsymbol{x})$ 则 表示已知 $\boldsymbol{x}$ 的条件下输出为
$y$ 的概率, 即根据 $\boldsymbol{x}$ "判别" $y$, 因此称为 "判别式模型"。

常见的对率回归、支持向量机等都属于判别式模型,
而朴素贝叶斯则属于生成式模型。

### 14.1.2 式(14.1)的推导

由概率公式 $P(A B)=P(A \mid B) \cdot P(B)$ 可得:


$$
P\left(x_1, y_1, \ldots, x_n, y_n\right)=P\left(x_1, \ldots, x_n \mid y_1, \ldots, y_n\right) \cdot P\left(y_1, \ldots, y_n\right)
$$



其中, 进一步可将 $P\left(y_1, \ldots, y_n\right)$ 做如下变换:


$$
\begin{aligned}
P\left(y_1, \ldots, y_n\right) & =P\left(y_n \mid y_1, \ldots, y_{n-1}\right) \cdot P\left(y_1, \ldots, y_{n-1}\right) \\
& =P\left(y_n \mid y_1, \ldots, y_{n-1}\right) \cdot P\left(y_{n-1} \mid y_1, \ldots, y_{n-2}\right) \cdot P\left(y_1, \ldots, y_{n-2}\right) \\
& =\ldots \ldots \\
& =P\left(y_n \mid y_1, \ldots, y_{n-1}\right) \cdot P\left(y_{n-1} \mid y_1, \ldots, y_{n-2}\right) \cdot \ldots \cdot P\left(y_2 \mid y_1\right) \cdot P\left(y_1\right)
\end{aligned}
$$



由于状态 $y_1, \ldots, y_n$ 构成马尔可夫链, 即 $y_t$ 仅由 $y_{t-1}$
决定; 基于这种依赖关系, 有 

$$
\begin{aligned}
P\left(y_n \mid y_1, \ldots, y_{n-1}\right) & =P\left(y_n \mid y_{n-1}\right) \\
P\left(y_{n-1} \mid y_1, \ldots, y_{n-2}\right) & =P\left(y_{n-1} \mid y_{n-2}\right) \\
P\left(y_{n-2} \mid y_1, \ldots, y_{n-3}\right) & =P\left(y_{n-2} \mid y_{n-3}\right)
\end{aligned}
$$



因此 $P\left(y_1, \ldots, y_n\right)$ 可化简为 

$$
\begin{aligned}
P\left(y_1, \ldots, y_n\right) & =P\left(y_n \mid y_{n-1}\right) \cdot P\left(y_{n-1} \mid y_{n-2}\right) \cdot \ldots \cdot P\left(y_2 \mid y_1\right) \cdot P\left(y_1\right) \\
& =P\left(y_1\right) \prod_{i=2}^n P\left(y_i \mid y_{i-1}\right)
\end{aligned}
$$



而根据"西瓜书"图14.1表示的变量间的依赖关系: 在任一时刻,
观测变量的取值仅依赖于状态 变量, 即 $x_t$ 由 $y_t$ 确定,
与其它状态变量及观测变量的取值无关。因此 

$$
\begin{aligned}
P\left(x_1, \ldots, x_n \mid y_1, \ldots, y_n\right) & =P\left(x_1 \mid y_1, \ldots, y_n\right) \cdot \ldots \cdot P\left(x_n \mid y_1, \ldots, y_n\right) \\
& =P\left(x_1 \mid y_1\right) \cdot \ldots \cdot P\left(x_n \mid y_n\right) \\
& =\prod_{i=1}^n P\left(x_i \mid y_i\right)
\end{aligned}
$$



综上所述, 可得 

$$
\begin{aligned}
P\left(x_1, y_1, \ldots, x_n, y_n\right) & =P\left(x_1, \ldots, x_n \mid y_1, \ldots, y_n\right) \cdot P\left(y_1, \ldots, y_n\right) \\
& =\left(\prod_{i=1}^n P\left(x_i \mid y_i\right)\right) \cdot\left(P\left(y_1\right) \prod_{i=2}^n P\left(y_i \mid y_{i-1}\right)\right) \\
& =P\left(y_1\right) P\left(x_1 \mid y_1\right) \prod_{i=2}^n P\left(y_i \mid y_{i-1}\right) P\left(x_i \mid y_i\right)
\end{aligned}
$$



### 14.1.3 隐马尔可夫模型的三组参数

状态转移概率和输出观测概率都容易理解, 简单解释一下初始状态概率。
特别注意, 初始状态概率中
$\pi_i=P\left(y_1 \mid s_i\right), 1 \leqslant i \leqslant N$, 这里只有
$y_1$, 因为 $y_2$ 及以 后的其它状态是由状态转移概率和 $y_1$ 确定的,
具体参见课本第 321 页 "给定隐马尔可夫模型 $\lambda$,
它按如下过程产生观测序列 $\left\{x_1, x_2, \ldots, x_n\right\} ”$
的四个步骤。

## 14.2 马尔可夫随机场

本节介绍无向图模型的著名代表之一：马尔可夫随机场。本节的部分概念（例如势函数、极大团等）比较抽象，我亦无好办法，只能建议多读几遍，从心里接受这些概念就好。另外，从因果关系角度来讲，首先是因为满足全局、局部或成对马尔可夫性的无向图模型称为马尔可夫随机场，所以马尔可夫随机场才具有全局、局部或成对马尔可夫性。

### 14.2.1 式(14.2)和式(14.3)的解释

注意式(14.2)之前的介绍是 "则联合概率 $P(\mathbf{x})$ 定义为",
而在式(14.3)之前也有类似的描 述。因此,
可以将式(14.2)和式(14.3)理解为一种定义,
记住并接受这个定义就好了。实际上, 该定义是根据 Hammersley-Clifford
定理而得, 可以具体了解一下该定理, 这里不再赘述。

值得一提的是, 在接下来讨论 "条件独立性" 时, 即式(14.4)
式(14.7)的推导过程直接 使用了该定义。注意：在有了式(14.3)的定义后,
式(14.2)已作废, 不再使用。

### 14.2.2 式(14.4)到式(14.7)的推导

首先, 式(14.4)直接使用了式(14.3)有关联合概率的定义。

对于式(14.5), 第一行两个等号变形就是概率论中的知识;
第二行的变形直接使用了式 (14.3)有关联合概率的定义; 第三行中, 由于
$\psi_{A C}\left(x_A^{\prime}, x_C\right)$ 与变量 $x_B^{\prime}$ 无关,
因此可以拿到求 和号 $\sum_{x_B^{\prime}}$ 外面, 即


$$
\sum_{x_A^{\prime}} \sum_{x_B^{\prime}} \psi_{A C}\left(x_A^{\prime}, x_C\right) \psi_{B C}\left(x_B^{\prime}, x_C\right)=\sum_{x_A^{\prime}} \psi_{A C}\left(x_A^{\prime}, x_C\right) \sum_{x_B^{\prime}} \psi_{B C}\left(x_B^{\prime}, x_C\right)
$$



举个例子, 假设
$\mathbf{x}=\left\{x_1, x_2, x_3\right\}, \mathbf{y}=\left\{y_1, y_2, y_3\right\}$,
则 

$$
\begin{aligned}
\sum_{i=1}^3 \sum_{j=1}^3 x_i y_j & =x_1 y_1+x_1 y_2+x_1 y_3+x_2 y_1+x_2 y_2+x_2 y_3+x_3 y_1+x_3 y_2+x_3 y_3 \\
& =x_1 \times\left(y_1+y_2+y_3\right)+x_2 \times\left(y_1+y_2+y_3\right)+x_3 \times\left(y_1+y_2+y_3\right) \\
& =\left(x_1+x_2+x_3\right) \times\left(y_1+y_2+y_3\right)=\left(\sum_{i=1}^3 x_i\right)\left(\sum_{j=1}^3 y_j\right)
\end{aligned}
$$



同理可得式(14.6)。类似于式(14.6), 还可以得到
$P\left(x_B \mid x_C\right)=\frac{\psi_{B C}\left(x_B, x_C\right)}{\sum_{x_B^{\prime}} \psi_{B C}\left(x_B^{\prime}, x_C\right)}$

最后, 综合可得式(14.7)成立, 即马尔可夫随机场 "条件独立性" 得证。

### 14.2.3 马尔可夫毯(Markov blanket)

本节共提到三个性质, 分别是全局马尔可夫性、局部马尔可夫性和成对马
尔可夫性, 三者本质上是一样的, 只是适用场景略有差异。

在"西瓜书"第 325 页左上角边注提到 "马尔可夫㲜" 的概念,
专门提一下这个概念主要是其名字
与马尔可夫链、隐马尔可夫模型、马尔可夫随机场等很像; 但实际上,
马尔可夫㲜是一个局 部的概念,
而马尔可夫链、隐马尔可夫模型、马尔可夫随机场则是整体模型级别的概念。

对于某变量, 当它的马尔可夫㲜 (即其所有邻接变量,
包含父变量、子变量、子变量的 其他父变量等组成的集合）确定时,
则该变量条件独立于其它变量, 即局部马尔可夫性。

### 14.2.4 势函数(potential function)

势函数贯穿本节，但却一直以抽象函数符号形式出现，直到本节最后才简单介绍势函数的具体形式，个人感觉这为理解本节内容增加不少难度。
具体来说，若已知势函数，例如以"西瓜书"图14.4为例的 和
取值，则可以根据式(14.3)基于最大团势函数定义的联合概率公式解得各种可能变量值指派的联合概率，进而完成一些预测工作；若势函数未知，在假定势函数的形式之后，应该就需要根据数据去学习势函数的参数。

### 14.2.5 式(14.8)的解释

此为势函数的定义式，即将势函数写作指数函数的形式。指数函数满足非负性，且便于求导，因此在机器学习中具有广泛应用，例如西瓜书式(8.5)和式(13.11)。

### 14.2.6 式(14.9)的解释

此为定义在变量$\mathbf{x}_{Q}$上的函数$H_{Q}\left(\cdot\right)$的定义式，第二项考虑单节点，第一项考虑每一对节点之间的关系。

## 14.3 条件随机场

条件随机场是给定一组输入随机变量 $\mathbf{x}$ 条件下, 另一组输出随机变量
$\mathbf{y}$ 构成的马尔可夫 随机场, 即本页边注中所说
"条件随机场可看作给定观测值的马尔可夫随机场", 条件随机 场的 "条件"
应该就来源于此吧, 因为需要求解的概率为条件联合概率
$P(\mathbf{y} \mid \mathbf{x})$, 因此它 是一种判别式模型, 参见"西瓜书"图
14.6。

### 14.3.1 式(14.10)的解释



$$
P\left(y_{v} | \mathbf{x}, \mathbf{y}_{V \backslash\{v\}}\right)=P\left(y_{v} | \mathbf{x}, \mathbf{y}_{n(v)}\right)
$$


\[解析\]：根据局部马尔科夫性，给定某变量的邻接变量，则该变量独立与其他变量，即该变量只与其邻接变量有关，所以式(14.10)中给定变量$v$
以外的所有变量与仅给定变量$v$的邻接变量是等价的。

特别注意, 本式下方写到 "则 $(\mathbf{y}, \mathbf{x})$
构成一个条件随机场"; 也就是说, 因为 $(\mathbf{y}, \mathbf{x})$ 满足 式
(14.10), 所以 $(\mathbf{y}, \mathbf{x})$ 构成一个条件随机场,
类似马尔可夫随机场与马尔可夫性的因果关系。

### 14.3.2 式(14.11)的解释

注意本式前面的话："条件概率被定义为"。至于式中使用的转移特征函数
和状态特征函数
，一般这两个函数取值为1或0，当满足特征条件时取值为1，否则为0。

### 14.3.3 学习与推断

本节前4段内容（标题"14.4.1
变量消去"之前）至关重要，可以看作是14.4节和14.5节的引言，为后面这两节内容做铺垫，因此一定要反复研读几遍，因为这几段内容告诉你接下来两节要解决什么问题，心中装着问题再去看书会事半功倍，否则即使推明白了公式也不知道为什么要去推这些公式。本节介绍两种精确推断方法，下一节则介绍两种近似推断方法。

### 14.3.4 式(14.14)的推导

该式本身的含义很容易理解, 即为了求 $P\left(x_5\right)$
对联合分布中其他无关变量（即 $x_1, x_2$, $x_3, x_4$ ）进行积分 (或求和)
的过程, 也就是 "边际化" (marginalization)。

关键在于为什么从第 1 个等号可以得到第 2 个等号, 边注中提到
"基于有向图模型所描 述的条件独立性", 此即第 7
章式(7.26)。这里的变换类似于式(7.27)的推导过程, 不再赘述。

总之，在消去变量的过程中，在消去每一个变量时需要保证其依赖的变量已经消去，因此消去顺序应该是有向概率图中的一条以目标节点为终点的拓扑序列。

### 14.3.5 式(14.15)和式(14.16)的推导

这里定义新符号 $m_{i j}\left(x_j\right)$,
请一定理解并记住其含义。依次推导如下: 

$$
\begin{aligned}
& m_{12}\left(x_2\right)=\sum_{x_1} P\left(x_1\right) P\left(x_2 \mid x_1\right)=\sum_{x_1} P\left(x_2, x_1\right)=P\left(x_2\right) \\
& m_{23}\left(x_3\right)=\sum_{x_2} P\left(x_3 \mid x_2\right) m_{12}\left(x_2\right)=\sum_{x_2} P\left(x_3, x_2\right)=P\left(x_3\right) \\
& \left.m_{43}\left(x_3\right)=\sum_{x_4} P\left(x_4 \mid x_3\right) m_{23}\left(x_3\right)=\sum_{x_4} P\left(x_4, x_3\right)=P\left(x_3\right) \text { (这里与书中不一样 }\right) \\
& m_{35}\left(x_5\right)=\sum_{x_3} P\left(x_5 \mid x_3\right) m_{43}\left(x_3\right)=\sum_{x_3} P\left(x_5, x_3\right)=P\left(x_5\right)
\end{aligned}
$$

 注意: 这里的过程与"西瓜书"中不太一样, 但本质一样, 因为
$m_{43}\left(x_3\right)=\sum_{x_4} P\left(x_4 \mid x_3\right)=1$ 。

### 14.3.6 式(14.17)的解释

忽略图14.7(a)中的箭头，然后把无向图中的每条边的两个端点作为一个团将其分解为四个团因子的乘积。$Z$为规范化因子确保所有可能性的概率之和为$1$。本式就是基于极大团定义的联合概率分布，参见式(14.3)。

### 14.3.7 式(14.18)的推导

原理同式$14.15$,
区别在于把条件概率替换为势函数。由于势函数的定义是抽象的, 无法类似于
$\sum_{x_4} P\left(x_4 \mid x_3\right)=1$ 去处理
$\sum_{x_4} \psi\left(x_3, x_4\right)$ 。

但根据边际化运算规则，可以知道：

$m_{12}\left(x_2\right)=\sum_{x_1} \psi_{12}\left(x_1, x_2\right)$ 只含
$x_2$ 不含 $x_1$;

$m_{23}\left(x_3\right)=\sum_{x_2} \psi_{23}\left(x_2, x_3\right) m_{12}\left(x_2\right)$
只含 $x_3$ 不含 $x_2$;

$m_{43}\left(x_3\right)=\sum_{x_4} \psi_{34}\left(x_3, x_4\right) m_{23}\left(x_3\right)$
只含 $x_3$ 不含 $x_4$;

$m_{35}\left(x_5\right)=\sum_{x_3} \psi_{35}\left(x_3, x_5\right) m_{43}\left(x_3\right)$
只含 $x_5$ 不含 $x_3$, 即最后得到 $P\left(x_5\right)$ 。

### 14.3.8 式(14.19)的解释

首先解释符号含义, $k \in n(i) \backslash j$ 表示 $k$ 属于除去 $j$ 之外的
$x_i$ 的邻接结点, 例如 $n(1) \backslash 2$ 为 空集 (因为 $x_1$
只有邻接结点 2 ), $n(2) \backslash 3=\{1\}$ (因为 $x_2$ 有邻接结点 1 和
3 )，n(4) $n 3$ 为空 集 (因为 $x_4$ 只有邻接结点 3 ),
$n(3) \backslash 5=\{2,4\}$ （因为 $x_3$ 有邻接结点 2,4 和 5 )。

接下来, 仍然以图14.7 计算 $P\left(x_5\right)$ 为例: 

$$
\begin{aligned}
& m_{12}\left(x_2\right)=\sum_{x_1} \psi_{12}\left(x_1, x_2\right) \prod_{k \in n(1) \backslash 2} m_{k 1}\left(x_1\right)=\sum_{x_1} \psi_{12}\left(x_1, x_2\right) \\
& m_{23}\left(x_3\right)=\sum_{x_2} \psi_{23}\left(x_2, x_3\right) \prod_{k \in n(2) \backslash 3} m_{k 2}\left(x_2\right)=\sum_{x_1} \psi_{12}\left(x_1, x_2\right) m_{12}\left(x_2\right) \\
& m_{43}\left(x_3\right)=\sum_{x_4} \psi_{34}\left(x_3, x_4\right) \prod_{k \in n(4) \backslash 3} m_{k 4}\left(x_4\right)=\sum_{x_4} \psi_{34}\left(x_3, x_4\right) \\
& m_{35}\left(x_5\right)=\sum_{x_3} \psi_{35}\left(x_3, x_5\right) \prod_{k \in n(3) \backslash 5} m_{k 3}\left(x_3\right)=\sum_{x_3} \psi_{35}\left(x_3, x_5\right) m_{23}\left(x_3\right) m_{43}\left(x_3\right)
\end{aligned}
$$



该式表示从节点$i$传递到节点$j$的过程，求和号表示要考虑节点$i$的所有可能取值。连乘号解释见式$14.20$。应当注意这里连乘号的下标不包括节点$j$，节点$i$只需要把自己知道的关于$j$以外的消息告诉节点$j$即可。

### 14.3.9 式(14.20)的解释

应当注意这里是正比于而不是等于，因为涉及到概率的规范化。可以这么解释，每个变量可以看作一个有一些邻居的房子，每个邻居根据其自己的见闻告诉你一些事情(消息)，任何一条消息的可信度应当与所有邻居都有相关性，此处这种相关性用乘积来表达。

### 14.3.10 式(14.22)的推导

假设$x$有M种不同的取值，$x_i$的采样数量为$m_i$(连续取值可以采用微积分的方法分割为离散的取值)，则


$$
\begin{aligned}
\hat{f}&=\frac{1}{N} \sum_{j=1}^{M} f\left(x_{j}\right) \cdot m_j \\
&= \sum_{j=1}^{M} f\left(x_{j}\right)\cdot \frac{m_j}{N} \\
&\approx \sum_{j=1}^{M} f\left(x_{j}\right)\cdot p(x_j)  \\
&\approx \int f(x) p(x) dx
\end{aligned}
$$



### 14.3.11 图14.8的解释

图(a)表示信念传播算法的第 1 步, 即指定一个根结点,
从所有叶结点开始向根结点传 递消息, 直到根结点收到所有邻接结点的消息;
图(b)表示信念传播算法的第 2 步, 即从根 结点开始向叶结点传递消息,
直到所有叶结点均收到消息。

本图并不难理解, 接下来思考如下两个问题:

【思考 1】如何编程实现本图信念传播的过程? 这其中涉及到很多问题,
例如从叶结点 $x_4$ 向根结点传递消息时, 当传递到 $x_3$ 时如何判 断应该向
$x_2$ 传递还是向 $x_5$ 传递? 当然, 你可能感觉 $x_5$ 是叶结点,
所以肯定是向 $x_2$ 传递, 那 是因为这个无向图模型很简单, 如果 $x_5$ 和
$x_3$ 之间还有很多个结点呢?

【思考 2】14.4.2 节开头就说到
"信念传播\...\...较好地解决了求解多个边际分布时的重 复计算问题",
但如果图模型很复杂而我本身只需要计算少量边际分布, 是否还应该使用信
念传播呢? 其实计算边际分布类似于第 $10.1$ 节提到的 "懒惰学习",
只有在计算边际分布时
才需要计算某些"消息"。这可能要根据实际情况在变量消去和信念传播两种方法之间取舍。

## 14.4 近似推断

本节介绍两种近似推断方法：MCMC采样和变分推断。提到推断，一般是为了求解某个概率分布（参见上一节的例子），但需要特别说明的是，本节将要介绍的MCMC采样并不是为了求解某个概率分布，而是在已知某个概率分布的前提下去构造服从该分布的独立同分布的样本集合，理解这一点对于读懂14.5.1节的内容非常关键，即14.5.1节中的$p(\mathbf{x})$是已知的；变分推断是概率图模型常用的推断方法，要尽可能理解并掌握其中的细节。

### 14.4.1 式(14.21)到式(14.25)的解释

这五个公式都是概率论课程中的基本公式, 很容易理解; 从 14.5.1
节开始到式(14.25), 实际都在为 MCMC 采样做铺垫, 即为什么要做 MCMC 采样?
以下分三点说明:

\(1\) 若已知概率密度函数 $p(x)$, 则可通过式(14.21)计算函数 $f(x)$
在该概率密度函数 $p(x)$ 下的期望; 这个过程也可以先根据 $p(x)$
抽取一组样本再通过式(14.22)近似完成。

(2)为什么要通过式(14.22)近似完成呢? 这是因为 "若 $x$
不是单变量而是一个高维多元变 量 $\mathbf{x}$, 且服从一个非常复杂的分布,
则对式(14.24)求积分通常很困难"。

\(3\) "然而, 若概率密度函数 $p(\mathbf{x})$ 很复杂, 则构造服从 $p$
分布的独立同分布样本也很困难", 这时可以使用 MCMC 采样技术完成采样过程。

式(14.23)就是在区间 $A$ 中的概率计算公式,
而式(14.24)与式(14.21)的区别也就在于式 (14.24)限定了积分变量 $x$ 的区间
(可能写成定积分形式可能更容易理解)。

### 14.4.2 式(14.26)的解释

假设变量$\mathbf{x}$所在的空间有$n$个状态($s_1,s_2,..,s_n$),
定义在该空间上的一个转移矩阵$\mathbf{T}\in\mathbb{R}^{n\times n}$满足一定的条件则该马尔可夫过程存在一个稳态分布$\boldsymbol{\pi}$,
使得 

$$
\begin{aligned}
\boldsymbol{\pi} \mathbf{T}=\boldsymbol{\pi}
\end{aligned}
$$

 其中,
$\boldsymbol{\pi}$是一个是一个$n$维向量，代表$s_1,s_2,..,s_n$对应的概率.
反过来,
如果我们希望采样得到符合某个分布$\boldsymbol{\pi}$的一系列变量$\mathbf{x}^1,\mathbf{x}^2,..,\mathbf{x}^t$,
应当采用哪一个转移矩阵$\mathbf{T}\in\mathbb{R}^{n\times n}$呢？

事实上，转移矩阵只需要满足马尔可夫细致平稳条件 

$$
\begin{aligned}
\pi_i \mathbf{T}_{ij}=\pi_j \mathbf{T}_{ji}
\end{aligned}
$$

 即式(14.26)，这里采用的符号与西瓜书略有区别以便于理解.
证明如下 

$$
\begin{aligned}
\boldsymbol{\pi} \mathbf{T}_{j\cdot} = \sum _i \pi_i\mathbf{T}_{ij} = \sum _i \pi_j\mathbf{T}_{ji} = \pi_j
\end{aligned}
$$


假设采样得到的序列为$\mathbf{x}^1,\mathbf{x}^2,..,\mathbf{x}^{t-1},\mathbf{x}^t$，则可以使用$MH$算法来使得$\mathbf{x}^{t-1}$(假设为状态$s_i$)转移到$\mathbf{x}^t$(假设为状态$s_j$)的概率满足式。

本式为某个时刻马尔可夫链平稳的条件, 注意式中的
$p\left(\mathbf{x}^t\right)$ 和 $p\left(\mathbf{x}^{t-1}\right)$ 已知,
但状态转 移概率 $T\left(\mathbf{x}^{t-1} \mid \mathbf{x}^t\right)$ 和
$T\left(\mathbf{x}^t \mid \mathbf{x}^{t-1}\right)$
末知。如何构建马尔可夫链转移概率至关重要, 不同的 构造方法将产生不同的
MCMC 算法（可以认为 MCMC 算法是一个大的框架或一种思想, 即 "MCMC
方法先设法构造一条马尔可夫链, 使其收玫至平稳分布恰为待估计参数的后验
分布, 然后通过这条马尔可夫链来产生符合后验分布的样本,
并基于这些样本来进行估计", 具体如何构建马尔可夫链有多种实现途径,
接下来介绍的 $\mathrm{MH}$ 算法就是其中一种)。

### 14.4.3 式(14.27)的解释

若将本式 $\mathbf{x}^{t-1}$ 和 $\mathbf{x}^*$ 分别对应式(14.27)的
$\mathbf{x}^t$ 和 $\mathbf{x}^{t-1}$, 则本式与式(14.27)区别仅在于状态
转移概率 $T\left(\mathbf{x}^* \mid \mathbf{x}^{t-1}\right)$ 由先验概率
$Q\left(\mathbf{x}^* \mid \mathbf{x}^{t-1}\right)$ 和被接受的概率
$A\left(\mathbf{x}^* \mid \mathbf{x}^{t-1}\right)$ 的乘积表示。

### 14.4.4 式(14.28)的推导

注意, 本式中的概率分布 $p(\mathbf{x})$ 和先验转移概率 $Q$ 均为已知,
因此可计算出接受概率。
将本式代入式(14.27)可以验证本式是正确的。具体来说,
式(14.27)等号左边将变为: 

$$
\begin{aligned}
& p\left(\mathbf{x}^{t-1}\right) Q\left(\mathbf{x}^* \mid \mathbf{x}^{t-1}\right) A\left(\mathbf{x}^* \mid \mathbf{x}^{t-1}\right) \\
= & p\left(\mathbf{x}^{t-1}\right) Q\left(\mathbf{x}^* \mid \mathbf{x}^{t-1}\right) \min \left(1, \frac{p\left(\mathbf{x}^*\right) Q\left(\mathbf{x}^{t-1} \mid \mathbf{x}^*\right)}{p\left(\mathbf{x}^{t-1}\right) Q\left(\mathbf{x}^* \mid \mathbf{x}^{t-1}\right)}\right) \\
= & \min \left(p\left(\mathbf{x}^{t-1}\right) Q\left(\mathbf{x}^* \mid \mathbf{x}^{t-1}\right), p\left(\mathbf{x}^{t-1}\right) Q\left(\mathbf{x}^* \mid \mathbf{x}^{t-1}\right) \frac{p\left(\mathbf{x}^*\right) Q\left(\mathbf{x}^{t-1} \mid \mathbf{x}^*\right)}{p\left(\mathbf{x}^{t-1}\right) Q\left(\mathbf{x}^* \mid \mathbf{x}^{t-1}\right)}\right) \\
= & \min \left(p\left(\mathbf{x}^{t-1}\right) Q\left(\mathbf{x}^* \mid \mathbf{x}^{t-1}\right), p\left(\mathbf{x}^*\right) Q\left(\mathbf{x}^{t-1} \mid \mathbf{x}^*\right)\right)
\end{aligned}
$$



将 $A\left(\mathbf{x}^{t-1} \mid \mathbf{x}^*\right)$ 代入右边 (符号式
$\mathbf{x}^{t-1}$ 和 $\mathbf{x}^*$ 调换位置), 同理可得如上结果,
即本式的 接受概率形式可保证式(14.27)成立。

验证完毕之后可以再做一个简单的推导。其实若想要式(14.27)成立, 简单令:


$$
A\left(\mathbf{x}^* \mid \mathbf{x}^{t-1}\right)=C \cdot p\left(\mathbf{x}^*\right) Q\left(\mathbf{x}^{t-1} \mid \mathbf{x}^*\right)
$$



(则等号右则的
$A\left(\mathbf{x}^{t-1} \mid \mathbf{x}^*\right)=C \cdot p\left(\mathbf{x}^{t-1}\right) Q\left(\mathbf{x}^* \mid \mathbf{x}^{t-1}\right)$
)

即可, 其中 $C$ 为大于零的常数, 且不能使
$A\left(\mathbf{x}^* \mid \mathbf{x}^{t-1}\right)$ 和
$A\left(\mathbf{x}^{t-1} \mid \mathbf{x}^*\right)$ 大于 1 （因为它们是
概率)。注意待解 $A\left(\mathbf{x}^* \mid \mathbf{x}^{t-1}\right)$
为接受概率, 在保证式(14.27)成立的基础上, 其值应该尽可能大一些
(但概率值不会超过 1 ), 否则在图 $14.9$ 描述的 $\mathrm{MH}$
算法中采样出的候选样本将会 有大部分会被拒绝。所以, 常数 $C$
尽可能大一些, 那么 $C$ 最大可以为多少呢?

对于
$A\left(\mathrm{x}^* \mid \mathrm{x}^{t-1}\right)=C \cdot p\left(\mathrm{x}^*\right) Q\left(\mathrm{x}^{t-1} \mid \mathrm{x}^*\right)$,
易知 $C$ 最大可以取值
$\frac{1}{p\left(\mathrm{x}^*\right) Q\left(\mathrm{x}^{t-1} \mid \mathrm{x}^*\right)}$,
再大则会使 $A\left(\mathbf{x}^* \mid \mathbf{x}^{t-1}\right)$ 大于 1 ;
对于
$A\left(\mathbf{x}^{t-1} \mid \mathbf{x}^*\right)=C \cdot p\left(\mathbf{x}^{t-1}\right) Q\left(\mathbf{x}^* \mid \mathbf{x}^{t-1}\right)$,
易知 $C$ 最 大可以取值
$\frac{1}{p\left(\mathbf{x}^{t-1}\right) Q\left(\mathbf{x}^* \mid \mathbf{x}^{t-1}\right)}$;
常数 $C$ 的取值需要同时满足两个约束, 因此


$$
C=\min \left(\frac{1}{\cdot p\left(\mathbf{x}^*\right) Q\left(\mathbf{x}^{t-1} \mid \mathbf{x}^*\right)}, \frac{1}{p\left(\mathbf{x}^{t-1}\right) Q\left(\mathbf{x}^* \mid \mathbf{x}^{t-1}\right)}\right)
$$



将这个常数 $C$ 的表达式代入
$A\left(\mathrm{x}^* \mid \mathrm{x}^{t-1}\right)=C \cdot p\left(\mathrm{x}^*\right) Q\left(\mathrm{x}^{t-1} \mid \mathrm{x}^*\right)$
即得式(14.28)。

### 14.4.5 吉布斯采样与MH算法

这里解释一下为什么说吉布斯采样是 MH 算法的特例。

吉布斯采样算法如下 ("西瓜书"第 334 页):

\(1\) 随机或以某个次序选取某变量 $x_i$;

\(2\) 根据 $\mathbf{x}$ 中除 $x_i$ 外的变量的现有取值, 计算条件概率
$p\left(x_i \mid \mathbf{x}_{\bar{i}}\right)$, 其中
$\mathbf{x}_{\bar{i}}=\left\{x_1, x_2, \ldots, x_{i-1}, x_{i+1}, \ldots, x_N\right\}$;

\(3\) 根据 $p\left(x_i \mid \mathbf{x}_{\bar{i}}\right)$ 对变量 $x_i$
采样, 用采样值代替原值。

对应到式(14.27)和式(14.28)表示的 MH 采样, 候选样本 $\mathbf{x}^*$ 与
$t-1$ 时刻样本 $\mathbf{x}^{t-1}$ 的区别 仅在于第 $i$ 个变量的取值不同,
即 $\mathbf{x}_{\bar{i}}^*$ 与 $\mathbf{x}_{\bar{i}}^{t-1}$
相同。先给几个概率等式:

\(1\)
$Q\left(\mathbf{x}^* \mid \mathbf{x}^{t-1}\right)=p\left(x_i^* \mid \mathbf{x}_{\bar{i}}^{t-1}\right)$；

\(2\)
$Q\left(\mathbf{x}^{t-1} \mid \mathbf{x}^*\right)=p\left(x_i^{t-1} \mid \mathbf{x}_{\bar{i}}^*\right)$；

\(3\)
$p\left(\mathbf{x}^*\right)=p\left(x_i^*, \mathbf{x}_{\bar{i}}^*\right)=p\left(x_i^* \mid \mathbf{x}_{\bar{i}}^*\right) p\left(\mathbf{x}_{\bar{i}}^*\right)$；

\(4\)
$p\left(\mathbf{x}^{t-1}\right)=p\left(x_i^{t-1}, \mathbf{x}_{\bar{i}}^{t-1}\right)=p\left(x_i^{t-1} \mid \mathbf{x}_{\bar{i}}^{t-1}\right) p\left(\mathbf{x}_{\bar{i}}^{t-1}\right)$。

其中等式(1)是由于吉布斯采样中 "根据
$p\left(x_i \mid \mathbf{x}_i\right)$ 对变量 $x_i$ 采样" (参见以上第 (3)
步), 即用 户给定的先验概率为
$p\left(x_i \mid \mathbf{x}_{\bar{i}}\right)$, 同理得等式(2);
等式(3)就是将联合概率 $p\left(\mathbf{x}^*\right)$ 换了种形式,
然写成了条件概率和先验概率乘积, 同理得等式(4)。

对于式(14.28)来说 (注意:
$\mathbf{x}_{\bar{i}}^*=\mathbf{x}_{\bar{i}}^{t-1}$ )


$$
\frac{p\left(\mathbf{x}^*\right) Q\left(\mathbf{x}^{t-1} \mid \mathbf{x}^*\right)}{p\left(\mathbf{x}^{t-1}\right) Q\left(\mathbf{x}^* \mid \mathbf{x}^{t-1}\right)}=\frac{p\left(x_i^* \mid \mathbf{x}_i^*\right) p\left(\mathbf{x}_i^*\right) p\left(x_i^{t-1} \mid \mathbf{x}_{\bar{i}}^*\right)}{p\left(x_i^{t-1} \mid \mathbf{x}_{\bar{i}}^{t-1}\right) p\left(\mathbf{x}_{\bar{i}}^{t-1}\right) p\left(x_i^* \mid \mathbf{x}_{\bar{i}}^{t-1}\right)}=1
$$



即在吉布斯采样中接受概率恒等于 1 , 也就是说吉布斯采样是接受概率为 1 的
MH 采样。

该推导参考了PRML[1]第 544 页。

### 14.4.6 式(14.29)的解释

连乘号是因为$N$个变量的生成过程相互独立。求和号是因为每个变量的生成过程需要考虑中间隐变量的所有可能性，类似于边际分布的计算方式。

### 14.4.7 式(14.30)的解释

对式(14.29)取对数。本式就是求对数后, 原来的连乘变为了连加, 即性质
$\ln (a b)=\ln a+\ln b$ 。

接下来提到"图14.10所对应的推断和学习任务主要是由观察到的变量
$\mathbf{x}$ 来估计隐变量 $\mathbf{Z}$ 和分布参数变量 $\Theta$, 即求解
$p(\mathbf{z} \mid \mathbf{x}, \Theta)$ 和 $\Theta$ ",
这里可以对应式(3.26)来这样不严谨理解: $\Theta$ 对应式(3.26)的
$\boldsymbol{w}, b$, 而 $\mathbf{z}$ 对应式(3.26)的 $y$ 。

### 14.4.8 式(14.31)的解释

对应7.6节EM算法中的M步，参见第163页的式(7.36)和式(7.37)。

### 14.4.9 式(14.32)到式(14.34)的推导

从式(14.31)到式(14.32)之间的跳跃比较大, 接下来为了方便忽略分布参数变量
$\Theta$ 。 这里的主要问题是后验概率 $p(\mathbf{z} \mid \mathbf{x})$
难于获得, 进而使用一个已知简单分布 $q(\mathbf{z})$ 去近似
需要推导的复杂分布 $p(\mathbf{z} \mid \mathbf{x})$,
这就是变分推断的核心思想。

根据概率论公式
$p(\mathbf{x}, \mathbf{z})=p(\mathbf{z} \mid \mathbf{x}) p(\mathbf{x})$,
得:


$$
p(\mathbf{x})=\frac{p(\mathbf{x}, \mathbf{z})}{p(\mathbf{z} \mid \mathbf{x})}
$$



分子分母同时除以 $q(\mathbf{z})$, 得:


$$
p(\mathbf{x})=\frac{p(\mathbf{x}, \mathbf{z}) / q(\mathbf{z})}{p(\mathbf{z} \mid \mathbf{x}) / q(\mathbf{z})}
$$



等号两边同时取自然对数, 得:


$$
\ln p(\mathbf{x})=\ln \frac{p(\mathbf{x}, \mathbf{z}) / q(\mathbf{z})}{p(\mathbf{z} \mid \mathbf{x}) / q(\mathbf{z})}=\ln \frac{p(\mathbf{x}, \mathbf{z})}{q(\mathbf{z})}-\ln \frac{p(\mathbf{z} \mid \mathbf{x})}{q(\mathbf{z})}
$$



等号两边同时乘以 $q(\mathbf{z})$ 并积分, 得:


$$
\int q(\mathbf{z}) \ln p(\mathbf{x}) \mathrm{d} \mathbf{z}=\int q(\mathbf{z}) \ln \frac{p(\mathbf{x}, \mathbf{z})}{q(\mathbf{z})} \mathrm{d} \mathbf{z}-\int q(\mathbf{z}) \ln \frac{p(\mathbf{z} \mid \mathbf{x})}{q(\mathbf{z})} \mathrm{d} \mathbf{z}
$$



对于等号左边的积分, 由于 $p(\mathbf{x})$ 与变量 $\mathbf{z}$ 无关,
因此可以当作常数拿到积分号外面:


$$
\int q(\mathbf{z}) \ln p(\mathbf{x}) \mathrm{d} \mathbf{z}=\ln p(\mathbf{x}) \int q(\mathbf{z}) \mathrm{d} \mathbf{z}=\ln p(\mathbf{x})
$$



其中 $q(\mathbf{z})$ 为一个概率分布, 所以积分等于 1 。至此,
前面式子变为:


$$
\ln p(\mathbf{x})=\int q(\mathbf{z}) \ln \frac{p(\mathbf{x}, \mathbf{z})}{q(\mathbf{z})} \mathrm{d} \mathbf{z}-\int q(\mathbf{z}) \ln \frac{p(\mathbf{z} \mid \mathbf{x})}{q(\mathbf{z})} \mathrm{d} \mathbf{z}
$$



此即式(14.32), 等号右边第 1 项即式(14.33)称为 Evidence Lower Bound
(ELBO), 等号右边 第 2 项即式(14.34)为 $K L$ 散度（参见附录 C.3）。
我们的目标是用分布 $q(\mathbf{z})$ 去近似后验概率
$p(\mathbf{z} \mid \mathbf{x})$, 而 $\mathrm{KL}$
散度用于度量两个概率分布 之间的差异, 其中 KL
散度越小表示两个分布差异越小, 因此可以最小化式(14.34):


$$
\min _{q(\mathbf{z})} \operatorname{KL}(q(\mathbf{z}) \| p(\mathbf{z} \mid \mathbf{x}))
$$



但这并没有什么意义, 因为 $p(\mathbf{z} \mid \mathbf{x})$ 末知。注意,
式(14.32)恒等于常数 $\ln p(\mathbf{x})$, 因此最小化
式(14.34)等价于最大化式(14.33)的 ELBO。 在本节接下来的推导中,
就是通过最大化式(14.33)来求解 $p(\mathbf{z} \mid \mathbf{x})$ 的近似
$q(\mathbf{z})$ 。

### 14.4.10 式(14.35)的解释

在"西瓜书" 14.5.2 节开篇提到,
"变分推断通过使用已知简单分布来逼近需推断的复杂分布", 这 里我们使用
$q(\mathbf{z})$ 去近似后验分布 $p(\mathbf{z} \mid \mathbf{x})$
。而本式进一步假设复杂的多变量 $\mathbf{z}$ 可拆解为一系
列相互独立的多变量 $\mathbf{z}_i$, 进而有
$q(\mathbf{z})=\prod_{i=1}^M q_i\left(\mathbf{z}_i\right)$,
以便于后面简化求解。

### 14.4.11 式(14.36)的推导

将式(14.35)代入式(14.33), 得: 

$$
\begin{aligned}
\mathcal{L}(q) & =\int q(\mathbf{z}) \ln \frac{p(\mathbf{x}, \mathbf{z})}{q(\mathbf{z})} \mathrm{d} \mathbf{z}=\int q(\mathbf{z})\{\ln p(\mathbf{x}, \mathbf{z})-\ln q(\mathbf{z})\} \mathrm{d} \mathbf{z} \\
& =\int \prod_{i=1}^M q_i\left(\mathbf{z}_i\right)\left\{\ln p(\mathbf{x}, \mathbf{z})-\ln \prod_{i=1}^M q_i\left(\mathbf{z}_i\right)\right\} \mathrm{d} \mathbf{z} \\
& =\int \prod_{i=1}^M q_i\left(\mathbf{z}_i\right) \ln p(\mathbf{x}, \mathbf{z}) \mathrm{d} \mathbf{z}-\int \prod_{i=1}^M q_i\left(\mathbf{z}_i\right) \ln \prod_{i=1}^M q_i\left(\mathbf{z}_i\right) \mathrm{d} \mathbf{z} \triangleq \mathcal{L}_1(q)-\mathcal{L}_2(q)
\end{aligned}
$$



接下来推导中大量使用交换积分号次序, 记积分项为
$Q(\mathbf{x}, \mathbf{z})$, 则上式可变形为:


$$
\mathcal{L}(q)=\int Q(\mathbf{x}, \mathbf{z}) \mathrm{d} \mathbf{z}=\int \cdots \int Q(\mathbf{x}, \mathbf{z}) \mathrm{d} \mathbf{z}_1 \mathrm{~d} \mathbf{z}_2 \cdots \mathrm{d} \mathbf{z}_M
$$



根据积分相关知识, 在满足某种条件下, 积分号的次序可以任意交换。

对于第 1 项 $\mathcal{L}_1(q)$, 交换积分号次序, 得:


$$
\mathcal{L}_1(q)=\int \prod_{i=1}^M q_i\left(\mathbf{z}_i\right) \ln p(\mathbf{x}, \mathbf{z}) \mathrm{d} \mathbf{z}=\int q_j\left\{\int \ln p(\mathbf{x}, \mathbf{z}) \prod_{i \neq j}^M\left(q_i\left(\mathbf{z}_i\right) \mathrm{d} \mathbf{z}_i\right)\right\} \mathrm{d} \mathbf{z}_j
$$



令
$\ln \tilde{p}\left(\mathbf{x}, \mathbf{z}_j\right)=\int \ln p(\mathbf{x}, \mathbf{z}) \prod_{i \neq j}^M\left(q_i\left(\mathbf{z}_i\right) \mathrm{d} \mathbf{z}_i\right)$
（这里与式(14.37)略有不同, 具体参见接下 来一条的解释), 代入, 得:


$$
\mathcal{L}_1(q)=\int q_j \ln \tilde{p}\left(\mathbf{x}, \mathbf{z}_j\right) \mathrm{d} \mathbf{z}_j
$$



对于第 2 项 $\mathcal{L}_2(q):$ 

$$
\begin{aligned}
\mathcal{L}_2(q) & =\int \prod_{i=1}^M q_i\left(\mathbf{z}_i\right) \ln \prod_{i=1}^M q_i\left(\mathbf{z}_i\right) \mathrm{d} \mathbf{z}=\int \prod_{i=1}^M q_i\left(\mathbf{z}_i\right) \sum_{i=1}^M \ln q_i\left(\mathbf{z}_i\right) \mathrm{d} \mathbf{z} \\
& =\sum_{i=1}^M \int \prod_{i=1}^M q_i\left(\mathbf{z}_i\right) \ln q_i\left(\mathbf{z}_i\right) \mathrm{d} \mathbf{z}=\sum_{i_1=1}^M \int \prod_{i_2=1}^M q_{i_2}\left(\mathbf{z}_{i_2}\right) \ln q_{i_1}\left(\mathbf{z}_{i_1}\right) \mathrm{d} \mathbf{z}
\end{aligned}
$$



解释一下第 2 行的第 2 个等号后的结果,
这是因为课本在这里符号表示并不严谨, 求和变量 和连乘变量不能同时使用 $i$,
这里求和变量和连乘变量分布使用 $i_1$ 和 $i_2$ 表示。对于求和号内的
积分项，考虑当 $i_1=j$ 时: 

$$
\begin{aligned}
\int \prod_{i_2=1}^M q_{i_2}\left(\mathbf{z}_{i_2}\right) \ln q_j\left(\mathbf{z}_j\right) \mathrm{d} \mathbf{z} & =\int q_j\left(\mathbf{z}_j\right) \prod_{i_2 \neq j} q_{i_2}\left(\mathbf{z}_{i_2}\right) \ln q_j\left(\mathbf{z}_j\right) \mathrm{d} \mathbf{z} \\
& =\int q_j\left(\mathbf{z}_j\right) \ln q_j\left(\mathbf{z}_j\right)\left\{\int \prod_{i_2 \neq j} q_{i_2}\left(\mathbf{z}_{i_2}\right) \prod_{i_2 \neq j} \mathrm{~d} \mathbf{z}_{i_2}\right\} \mathrm{d} \mathbf{z}_j
\end{aligned}
$$



注意到
$\int \prod_{i_2 \neq j} q_{i_2}\left(\mathbf{z}_{i_2}\right) \prod_{i_2 \neq j} \mathrm{~d} \mathbf{z}_{i_2}=1$,
为了直观说明这个结论, 假设这里只有 $q_1\left(\mathbf{z}_1\right)$,
$q_2\left(\mathbf{z}_2\right)$ 和 $q_3\left(\mathbf{z}_3\right)$, 即:


$$
\iiint q_1\left(\mathbf{z}_1\right) q_2\left(\mathbf{z}_2\right) q_3\left(\mathbf{z}_3\right) \mathrm{d} \mathbf{z}_1 \mathrm{~d} \mathbf{z}_2 \mathrm{~d} \mathbf{z}_3=\int q_1\left(\mathbf{z}_1\right) \int q_2\left(\mathbf{z}_2\right) \int q_3\left(\mathbf{z}_3\right) \mathrm{d} \mathbf{z}_3 \mathrm{~d} \mathbf{z}_2 \mathrm{~d} \mathbf{z}_1
$$



对于概率分布, 我们有
$\int q_1\left(\mathbf{z}_1\right) \mathrm{d} \mathbf{z}_1=\int q_2\left(\mathbf{z}_2\right) \mathrm{d} \mathbf{z}_2=\int q_3\left(\mathbf{z}_3\right) \mathrm{d} \mathbf{z}_3=1$,
代入即得。因此:


$$
\int \prod_{i_2=1}^M q_{i_2}\left(\mathbf{z}_{i_2}\right) \ln q_j\left(\mathbf{z}_j\right) \mathrm{d} \mathbf{z}=\int q_j\left(\mathbf{z}_j\right) \ln q_j\left(\mathbf{z}_j\right) \mathrm{d} \mathbf{z}_j
$$



进而第 2 项可化简为: 

$$
\begin{aligned}
\mathcal{L}_2(q) & =\sum_{i_1=1}^M \int q_{i_1}\left(\mathbf{z}_{i_1}\right) \ln q_{i_1}\left(\mathbf{z}_{i_1}\right) \mathrm{d} \mathbf{z}_{i_1} \\
& =\int q_j\left(\mathbf{z}_j\right) \ln q_j\left(\mathbf{z}_j\right) \mathrm{d} \mathbf{z}_j+\sum_{i_1 \neq j}^M \int q_{i_1}\left(\mathbf{z}_{i_1}\right) \ln q_{i_1}\left(\mathbf{z}_{i_1}\right) \mathrm{d} \mathbf{z}_{i_1}
\end{aligned}
$$



由于这里只关注 $q_j$ （即固定 $q_{i \neq j}$ ）, 因此第 2
项进一步表示为第 $j$ 项加上一个常数:


$$
\mathcal{L}_2(q)=\int q_j\left(\mathbf{z}_j\right) \ln q_j\left(\mathbf{z}_j\right) \mathrm{d} \mathbf{z}_j+\text { const }
$$



综上所述, 可得式(14.36)的形式。

### 14.4.12 式(14.37)到式(14.38)的解释

首先解释式(14.38), 该式等号右侧就是式(14.36)第 2
个等号后面花括号中的内容, 之所 以这里写成了期望的形式, 这是将
$\prod_{i \neq j} q_i$ 看作为一个概率分布, 则该式表示函数
$\ln p(\mathbf{x}, \mathbf{z})$ 在概率分布 $\prod_{i \neq j} q_i$
下的期望, 类似于式(14.21)和式(14.24)。

然后解释式(14.37), 该式就是一个定义, 即令等号右侧的项为
$\ln \tilde{p}\left(\mathbf{x}, \mathbf{z}_j\right)$, 但该式却包
含一个常数项 const, 当然这并没有什么问题,
并不影响式(14.36)本身。具体来说, 将本项反代回式(14.36)第二个等号右侧第 1
项, 即: 

$$
\begin{aligned}
& \int q_j\left\{\int \ln p(\mathbf{x}, \mathbf{z}) \prod_{i \neq j}^M\left(q_i\left(\mathbf{z}_i\right) \mathrm{d} \mathbf{z}_i\right)\right\} \mathrm{d} \mathbf{z}_j=\int q_j \mathbb{E}_{i \neq j}[\ln p(\mathbf{x}, \mathbf{z})] \mathrm{d} \mathbf{z}_j \\
& =\int q_j\left(\ln \tilde{p}\left(\mathbf{x}, \mathbf{z}_j\right)-\text { const }\right) \mathrm{d} \mathbf{z}_j \\
& =\int q_j \ln \tilde{p}\left(\mathbf{x}, \mathbf{z}_j\right) \mathrm{d} \mathbf{z}_j-\int q_j \operatorname{constd} \mathbf{z}_j \\
& =\int q_j \ln \tilde{p}\left(\mathbf{x}, \mathbf{z}_j\right) \mathrm{d} \mathbf{z}_j-\text { const } \\
&
\end{aligned}
$$



注意, 加或减一个常数 const 实际等价, 只需 const
定义时添个符号即可。将这个 const 与式 (14.36)第 2 个等号后面的 const
合并（注意二者表示不同的值), 即式(14.36) 第 3 个等号后 面的 const。

### 14.4.13 式(14.39)的解释

对于式(14.36), 可继续变形为: 

$$
\begin{aligned}
\mathcal{L}(q) & =\int q_j \ln \tilde{p}\left(\mathbf{x}, \mathbf{z}_j\right) \mathrm{d} \mathbf{z}_j-\int q_j \ln q_j \mathrm{~d} \mathbf{z}_j+\mathrm{const} \\
& =\int q_j \ln \frac{\tilde{p}\left(\mathbf{x}, \mathbf{z}_j\right)}{q_j} \mathrm{~d} \mathbf{z}_j+\mathrm{const} \\
& =-\mathrm{KL}\left(q_j \| \tilde{p}\left(\mathbf{x}, \mathbf{z}_j\right)\right)+\mathrm{const}
\end{aligned}
$$

 注意, 在前面关于 "式(14.32) 式(14.34)的推导" 中提到,
我们的目标是用分布 $q(\mathbf{z})$ 去 近似后验概率
$p(\mathbf{z} \mid \mathbf{x})$, 而 $\mathrm{KL}$
散度则用于度量两个概率分布之间的差异, 其中 $\mathrm{KL}$ 散度越
小表示两个分布差异越小, 因此可以最小化式(14.34), 但这并没有什么意义,
因为 $p(\mathbf{z} \mid \mathbf{x})$ 末知。又因为式(14.32)恒等于常数
$\ln p(\mathbf{x})$, 因此最小化式(14.34)等价于最大化式(14.33)。刚
刚又得到式(14.33)等于
$-\mathrm{KL}\left(q_j \| \tilde{p}\left(\mathbf{x}, \mathbf{z}_j\right)\right)+$
const, 因此最大化式(14.33)等价于最小化这 里的 $\mathrm{KL}$ 散度,
因此可知当 $q_j=\tilde{p}\left(\mathbf{x}, \mathbf{z}_j\right)$ 时这个
$\mathrm{KL}$ 散度最小, 即式(14.33)最大, 也就是分 布 $q(\mathbf{z})$
与后验概率 $p(\mathbf{z} \mid \mathbf{x})$ 最相似。

而根据式(14.37)有
$\ln \tilde{p}\left(\mathbf{x}, \mathbf{z}_j\right)=\mathbb{E}_{i \neq j}[\ln p(\mathbf{x}, \mathbf{z})]+\mathrm{const}$,
再结合 $q_j=\tilde{p}\left(\mathbf{x}, \mathbf{z}_j\right)$, 可 知
$\ln q_j=\mathbb{E}_{i \neq j}[\ln p(\mathbf{x}, \mathbf{z})]+\mathrm{const}$,
即本式。

### 14.4.14 式(14.40)的解释

对式(14.39)两边同时取 $\exp (\cdot)$ 操作, 得 

$$
\begin{aligned}
q_j^*\left(\mathbf{z}_j\right) & =\exp \left(\mathbb{E}_{i \neq j}[\ln p(\mathbf{x}, \mathbf{z})]+\text { const }\right) \\
& =\exp \left(\mathbb{E}_{i \neq j}[\ln p(\mathbf{x}, \mathbf{z})]\right) \cdot \exp (\text { const })
\end{aligned}
$$

 两边同时取积分 $\int(\cdot) \mathrm{d} \mathbf{z}_j$
操作, 由于 $q_j^*\left(\mathbf{z}_j\right)$ 为概率分布, 所以
$\int q_j^*\left(\mathbf{z}_j\right) \mathrm{d} \mathbf{z}_j=1$, 因此有


$$
\begin{aligned}
1 & =\int \exp \left(\mathbb{E}_{i \neq j}[\ln p(\mathbf{x}, \mathbf{z})]\right) \cdot \exp (\text { const }) \mathrm{d} \mathbf{z}_j \\
& =\exp (\text { const }) \int \exp \left(\mathbb{E}_{i \neq j}[\ln p(\mathbf{x}, \mathbf{z})]\right) \mathrm{d} \mathbf{z}_j
\end{aligned}
$$

 这里就是将常数拿到了积分号外面, 因此:


$$
\exp (\text { const })=\frac{1}{\int \exp \left(\mathbb{E}_{i \neq j}[\ln p(\mathbf{x}, \mathbf{z})]\right) \mathrm{d} \mathbf{z}_j}
$$


代入刚开始的表达式, 可得本式: 

$$
\begin{aligned}
q_j^*\left(\mathbf{z}_j\right) & =\exp \left(\mathbb{E}_{i \neq j}[\ln p(\mathbf{x}, \mathbf{z})]\right) \cdot \exp (\text { const }) \\
& =\frac{\exp \left(\mathbb{E}_{i \neq j}[\ln p(\mathbf{x}, \mathbf{z})]\right)}{\int \exp \left(\mathbb{E}_{i \neq j}[\ln p(\mathbf{x}, \mathbf{z})]\right) \mathrm{d} \mathbf{z}_j}
\end{aligned}
$$

 实际上, 本式的分母为归一化因子, 以保证
$q_j^*\left(\mathbf{z}_j\right)$ 为概率分布。

## 14.5 话题模型

本节介绍话题模型的概念及其典型代表：隐狄利克雷分配模型（LDA）。

概括来说，给定一组文档，话题模型可以告诉我们这组文档谈论了哪些话题，以及每篇文档与哪些话题有关。举个例子，社会中出现了一个热点事件，为了大致了解网民的思想动态，于是抓取了一组比较典型的网页（博客、评论等）；每个网页就是一篇文档，我们通过分析这组网页，可以大致了解到网民都从什么角度关注这件事情（每个角度可视为一个主题，其中LDA模型中主题个数
需要人工指定），并大致知道每个网页都涉及哪些角度；这里学得的主题类似于聚类（参见第9章）中所得的簇（没有标记），每个主题最终由一个词频向量表示（即本节），通过分析该主题下的高频词，就可对其有大致的了解。

### 14.5.1 式(14.41)的解释



$$
p(\boldsymbol W,\boldsymbol z,\boldsymbol \beta,\boldsymbol \theta | \boldsymbol \alpha,\boldsymbol \eta) =
\prod_{t=1}^{T}p(\boldsymbol \theta_t | \boldsymbol \alpha)
\prod_{k=1}^{K}p(\boldsymbol \beta_k | \boldsymbol \eta) 
(\prod_{n=1}^{N}P(w_{t,n} | z_{t,n}, \boldsymbol \beta_k)P( z_{t,n} | \boldsymbol \theta_t))
$$


此式表示LDA模型下根据参数$\alpha, \eta$生成文档$W$的概率。其中$z, \beta, \theta$是生成过程的中间变量。具体的生成步骤可见概率图14.12，图中的箭头和式14.41中的条件概率中的因果项目一一对应。这里共有三个连乘符号，表示三个相互独立的概率关系。第一个连乘表示T个文档每个文档的话题分布都是相互独立的。第二个连乘表示K个话题每个话题下单词的分布是相互独立的。最后一个连乘号表示每篇文档中的所有单词的生成是相互独立的。

### 14.5.2 式(14.42)的解释

本式就是狄利克雷分布的定义式, 参见"西瓜书"附录C1.6。

### 14.5.3 式(14.43)的解释

本式为对数似然, 其中
$p\left(\mathbf{w}_t \mid \boldsymbol{\alpha}, \boldsymbol{\eta}\right)=\iiint p\left(\mathbf{w}_t, \mathbf{z}, \boldsymbol{\beta}, \boldsymbol{\Theta} \mid \boldsymbol{\alpha}, \boldsymbol{\eta}\right) \mathrm{d} \mathbf{z} \mathrm{d} \boldsymbol{\beta} \mathrm{d} \boldsymbol{\Theta}$,
即通过边际化
$p\left(\mathbf{w}_t, \mathbf{z}, \boldsymbol{\beta}, \boldsymbol{\Theta} \mid \boldsymbol{\alpha}, \boldsymbol{\eta}\right)$
而得。

由于 $T$ 篇文档相互独立, 所以
$p(\mathbf{W}, \mathbf{z}, \boldsymbol{\beta}, \boldsymbol{\Theta} \mid \boldsymbol{\alpha}, \boldsymbol{\eta})=\prod_{t=1}^T p\left(\mathbf{w}_t, \mathbf{z}, \boldsymbol{\beta}, \boldsymbol{\Theta} \mid \boldsymbol{\alpha}, \boldsymbol{\eta}\right)$,
求对数似然后连乘变为了连加, 即得本式。参见$7.2$极大似然估计。

### 14.5.4 式(14.44)的解释

本式就是联合概率、先验概率、条件概率之间的关系,
换种表示方法可能更易理解:


$$
p_{\boldsymbol{\alpha}, \boldsymbol{\eta}}(\mathbf{z}, \boldsymbol{\beta}, \boldsymbol{\Theta} \mid \mathbf{W})=\frac{p_{\boldsymbol{\alpha}, \boldsymbol{\eta}}(\mathbf{W}, \mathbf{z}, \boldsymbol{\beta}, \boldsymbol{\Theta})}{p_{\boldsymbol{\alpha}, \boldsymbol{\eta}}(\mathbf{W})}
$$



## 参考文献

[1] Christopher M Bishop and Nasser M Nasrabadi. Pattern recognition and machine learning, volume 4.
Springer, 2006.
