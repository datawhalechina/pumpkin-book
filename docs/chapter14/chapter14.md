## 14.1

$$
\begin{aligned}
P\left(x_{1}, y_{1}, \ldots, x_{n}, y_{n}\right)=P\left(y_{1}\right) P\left(x_{1} | y_{1}\right) \prod_{i=2}^{n} P\left(y_{i} | y_{i-1}\right) P\left(x_{i} | y_{i}\right)
\end{aligned}
$$

[解析]：所有的相乘关系都表示概率的相互独立。三种概率$P\left(y_{i}\right), P\left(x_{i} | y_{i}\right), P\left(x_{i} | y_{i}\right)$ 分别表示初始状态概率，输出观测概率和条件转移概率。

## 14.2

$$
P(\mathbf{x})=\frac{1}{Z} \prod_{Q \in C} \psi_{Q}\left(\mathbf{x}_{Q}\right)
$$

[解析]：连乘号都表示各个团之间概率分布相互独立。

## 14.3

$$
P(\mathbf{x})=\frac{1}{Z^*} \prod_{Q \in C*} \psi_{Q}\left(\mathbf{x}_{Q}\right)
$$

[解析]：意义同式$14.2$, 区别在于此处的团为极大团。

## 14.4

$$
P\left(x_{A}, x_{B}, x_{C}\right)=\frac{1}{Z} \psi_{A C}\left(x_{A}, x_{C}\right) \psi_{B C}\left(x_{B}, x_{C}\right)
$$

[解析]：将图$14.3$分解成$x_{A}, x_{C}$ 和 $x_{B}, x_{C}$ 两个团。

## 14.5

$$
P\left(x_{A}, x_{B} | x_{C}\right) =\frac{\psi_{A C}\left(x_{A}, x_{C}\right)}{\sum_{x_{A}^{\prime}} \psi_{A C}\left(x_{A}^{\prime}, x_{C}\right)} \cdot \frac{\psi_{B C}\left(x_{B}, x_{C}\right)}{\sum_{x_{B}^{\prime}} \psi_{B C}\left(x_{B}^{\prime}, x_{C}\right)}
$$

[推导]：参见原书推导。

## 14.6

$$
P\left(x_{A} | x_{C}\right) =\frac{\psi_{A C}\left(x_{A,} x_{C}\right)}{\sum_{x_{A}} \psi_{A C}\left(x_{A}^{\prime}, x_{C}\right)}
$$

[推导]：参见原书推导。

## 14.7

$$
P\left(x_{A}, x_{B} | x_{C}\right)=P\left(x_{A} | x_{C}\right) P\left(x_{B} | x_{C}\right)
$$

[解析]：可由14.5、14.6联立可得。

## 14.8

$$
\psi_{Q}\left(\mathbf{x}_{Q}\right)=e^{-H_{Q}\left(\mathbf{x}_{Q}\right)}
$$

[解析]：此为势函数的定义式，即将势函数写作指数函数的形式。指数函数满足非负性，且便于求导，因此在机器学习中具有广泛应用，例如西瓜书公式8.5和13.11。

## 14.9

$$
H_{Q}\left(\mathbf{x}_{Q}\right)=\sum_{u, v \in Q, u \neq v} \alpha_{u v} x_{u} x_{v}+\sum_{v \in Q} \beta_{v} x_{v}
$$

[解析]：此为定义在变量$\mathbf{x}_{Q}$上的函数$H_{Q}\left(\cdot\right)$的定义式，第二项考虑单节点，第一项考虑每一对节点之间的关系。

## 14.10

$$
P\left(y_{v} | \mathbf{x}, \mathbf{y}_{V \backslash\{v\}}\right)=P\left(y_{v} | \mathbf{x}, \mathbf{y}_{n(v)}\right)
$$

[解析]：根据局部马尔科夫性，给定某变量的邻接变量，则该变量独立与其他变量，即该变量只与其邻接变量有关，所以式$14.10$中给定变量$v$ 以外的所有变量与仅给定变量$v$的邻接变量是等价的。

## 14.14

$$
\begin{aligned}
P\left(x_{5}\right) &=\sum_{x_{4}} \sum_{x_{3}} \sum_{x_{2}} \sum_{x_{1}} P\left(x_{1}, x_{2}, x_{3}, x_{4}, x_{5}\right) \\
&=\sum_{x_{4}} \sum_{x_{3}} \sum_{x_{2}} \sum_{x_{1}} P\left(x_{1}\right) P\left(x_{2} | x_{1}\right) P\left(x_{3} | x_{2}\right) P\left(x_{4} | x_{3}\right) P\left(x_{5} | x_{3}\right)
\end{aligned}
$$

[解析]：在消去变量的过程中，在消去每一个变量时需要保证其依赖的变量已经消去，因此消去顺序应该是有向概率图中的一条以目标节点为终点的拓扑序列。

## 14.15

$$
\begin{aligned}
P\left(x_{5}\right) &=\sum_{x_{3}} P\left(x_{5} | x_{3}\right) \sum_{x_{4}} P\left(x_{4} | x_{3}\right) \sum_{x_{2}} P\left(x_{3} | x_{2}\right) \sum_{x_{1}} P\left(x_{1}\right) P\left(x_{2} | x_{1}\right)\\
&=\sum_{x_{3}} P\left(x_{5} | x_{3}\right) \sum_{x_{4}} P\left(x_{4} | x_{3}\right) \sum_{x_{2}} P\left(x_{3} | x_{2}\right) m_{12}\left(x_{2}\right)
\end{aligned}
$$

[解析]：变量消去的顺序为从右至左求和号的下标，应当注意$x_4$与$x_5$相互独立，因此可与$x_3$的消去顺序互换，对最终结果无影响。

## 14.16

$$
\begin{aligned}
P\left(x_{5}\right) &=\sum_{x_{3}} P\left(x_{5} | x_{3}\right) \sum_{x_{4}} P\left(x_{4} | x_{3}\right) m_{23}\left(x_{3}\right) \\
&=\sum_{x_{3}} P\left(x_{5} | x_{3}\right) m_{23}\left(x_{3}\right) \sum_{x_{4}} P\left(x_{4} | x_{3}\right) \\
&=\sum_{x_{3}} P\left(x_{5} | x_{3}\right) m_{23}\left(x_{3}\right) \\
&=m_{35}\left(x_{5}\right)
\end{aligned}
$$

[解析]：注意到$\sum_{x_{4}} P\left(x_{4} | x_{3}\right) = 1$。

## 14.17

$$
P\left(x_{1}, x_{2}, x_{3}, x_{4}, x_{5}\right)=\frac{1}{Z} \psi_{12}\left(x_{1}, x_{2}\right) \psi_{23}\left(x_{2}, x_{3}\right) \psi_{34}\left(x_{3}, x_{4}\right) \psi_{35}\left(x_{3}, x_{5}\right)
$$

[解析]：忽略图$14.7(a)$中的箭头，然后把无向图中的每条边的两个端点作为一个团将其分解为四个团因子的乘积。$Z$为规范化因子确保所有可能性的概率之和为$1$。

## 14.18

$$
\begin{aligned}
P\left(x_{5}\right) &=\frac{1}{Z} \sum_{x_{3}} \psi_{35}\left(x_{3}, x_{5}\right) \sum_{x_{4}} \psi_{34}\left(x_{3}, x_{4}\right) \sum_{x_{2}} \psi_{23}\left(x_{2}, x_{3}\right) \sum_{x_{1}} \psi_{12}\left(x_{1}, x_{2}\right) \\
&=\frac{1}{Z} \sum_{x_{3}} \psi_{35}\left(x_{3}, x_{5}\right) \sum_{x_{4}} \psi_{34}\left(x_{3}, x_{4}\right) \sum_{x_{2}} \psi_{23}\left(x_{2}, x_{3}\right) m_{12}\left(x_{2}\right) \\
&=\cdots \\
&=\frac{1}{Z} m_{35}\left(x_{5}\right)
\end{aligned}
$$

[解析]：原理同式$14.15$, 区别在于把条件概率替换为势函数。

## 14.19

$$
m_{i j}\left(x_{j}\right)=\sum_{x_{i}} \psi\left(x_{i}, x_{j}\right) \prod_{k \in n(i) \backslash j} m_{k i}\left(x_{i}\right)
$$

[解析]：该式表示从节点$i$传递到节点$j$的过程，求和号表示要考虑节点$i$的所有可能取值。连乘号解释见式$14.20$。应当注意这里连乘号的下标不包括节点$j$，节点$i$只需要把自己知道的关于$j$以外的消息告诉节点$j$即可。

## 14.20

$$
P\left(x_{i}\right) \propto \prod_{k \in n(i)} m_{k i}\left(x_{i}\right)
$$

[解析]：应当注意这里是正比于而不是等于，因为涉及到概率的规范化。可以这么解释，每个变量可以看作一个有一些邻居的房子，每个邻居根据其自己的见闻告诉你一些事情(消息)，任何一条消息的可信度应当与所有邻居都有相关性，此处这种相关性用乘积来表达。【引用http://helper.ipam.ucla.edu/publications/gss2013/gss2013_11344.pdf】

## 14.22

$$
\hat{f}=\frac{1}{N} \sum_{i=1}^{N} f\left(x_{i}\right)
$$

[推导]：假设$x$有M种不同的取值，$x_i$的采样数量为$m_i$(连续取值可以采用微积分的方法分割为离散的取值)，则
$$
\begin{aligned}
\hat{f}&=\frac{1}{N} \sum_{j=1}^{M} f\left(x_{j}\right) \cdot m_j \\
&= \sum_{j=1}^{M} f\left(x_{j}\right)\cdot \frac{m_j}{N} \\
&\approx \sum_{j=1}^{M} f\left(x_{j}\right)\cdot p(x_j)  \\
&\approx \int f(x) p(x) dx
\end{aligned}
$$

## 14.26

$$p(x^t)T(x^{t-1}|x^t)=p(x^{t-1})T(x^t|x^{t-1})$$

[解析]：假设变量$x$所在的空间有$n$个状态($s_1,s_2,..,s_n$), 定义在该空间上的一个转移矩阵$T(n\times n)$如果满足一定的条件则该马尔可夫过程存在一个稳态分布$\pi$, 使得
$$
\begin{aligned}
\pi T=\pi
\end{aligned}
$$
其中, $\pi$是一个是一个$n$维向量，代表$s_1,s_2,..,s_n$对应的概率. 反过来, 如果我们希望采样得到符合某个分布$\pi$的一系列变量$x_1,x_2,..,x_t$, 应当采用哪一个转移矩阵$T(n\times n)$呢？

事实上，转移矩阵只需要满足马尔可夫细致平稳条件
$$
\begin{aligned}
\pi (i)T(i,j)=\pi (j)T(j,i)
\end{aligned}
$$
即公式$14.26$，这里采用的符号与西瓜书略有区别以便于理解.  证明如下
$$
\begin{aligned}
\pi T(j) = \sum _i \pi (i)T(i,j) = \sum _i \pi (j)T(j,i) = \pi(j)
\end{aligned} 
$$
假设采样得到的序列为$x_1,x_2,..,x_{t-1},x_t$，则可以使用$MH$算法来使得$x_{t-1}$(假设为状态$s_i$)转移到$x_t$(假设为状态$s_j$)的概率满足式。

## 14.27

$$
p\left(\mathbf{x}^{t-1}\right) Q\left(\mathbf{x}^{*} | \mathbf{x}^{t-1}\right) A\left(\mathbf{x}^{*} | \mathbf{x}^{t-1}\right)=p\left(\mathbf{x}^{*}\right) Q\left(\mathbf{x}^{t-1} | \mathbf{x}^{*}\right) A\left(\mathbf{x}^{t-1} | \mathbf{x}^{*}\right)
$$

[解析]：这里把式$14.26$中的函数$T$ 拆分为两个函数$Q$和$A$之积，即先验概率和接受概率，便于实际算法的实现。

## 14.28

$$A(x^* | x^{t-1}) = \min\left ( 1,\frac{p(x^*)Q(x^{t-1} | x^*) }{p(x^{t-1})Q(x^* | x^{t-1})} \right )$$

[推导]：这个公式其实是拒绝采样的一个trick，因为基于式$14.27$只需要
$$
\begin{aligned}
  A(x^* | x^{t-1}) &= p(x^*)Q(x^{t-1} | x^*)  \\
  A(x^{t-1} | x^*) &= p(x^{t-1})Q(x^* | x^{t-1})
 \end{aligned} 
$$
即可满足式$14.26$，但是实际上等号右边的数值可能比较小，比如各为0.1和0.2，那么好不容易才到的样本只有百分之十几得到利用，所以不妨将接受率设为0.5和1，则细致平稳分布条件依然满足，样本利用率大大提高, 所以可以改进为
$$
\begin{aligned} 
A(x^* | x^{t-1}) &=  \frac{p(x^*)Q(x^{t-1} | x^*)}{norm}  \\  
A(x^{t-1} | x^*) &= \frac{p(x^{t-1})Q(x^* | x^{t-1}) }{norm}
\end{aligned} 
$$
其中
$$
\begin{aligned} 
norm = \max\left (p(x^{t-1})Q(x^* | x^{t-1}),p(x^*)Q(x^{t-1} | x^*) \right )
\end{aligned}  
$$
即西瓜书中的$14.28$。

## 14.29

$$
p(\mathbf{x} | \Theta)=\prod_{i=1}^{N} \sum_{\mathbf{z}} p\left(x_{i}, \mathbf{z} | \Theta\right)
$$

[解析]：连乘号是因为$N$个变量的生成过程相互独立。求和号是因为每个变量的生成过程需要考虑中间隐变量的所有可能性，类似于边际分布的计算方式。

## 14.30

$$
\ln p(\mathbf{x} | \Theta)=\sum_{i=1}^{N} \ln \left\{\sum_{\mathbf{z}} p\left(x_{i}, \mathbf{z} | \Theta\right)\right\}
$$

[解析]：对式$14.29$取对数。

## 14.31

$$
\begin{aligned}
\Theta^{t+1} &=\underset{\Theta}{\arg \max } \mathcal{Q}\left(\Theta ; \Theta^{t}\right) \\
&=\underset{\Theta}{\arg \max } \sum_{\mathbf{z}} p\left(\mathbf{z} | \mathbf{x}, \Theta^{t}\right) \ln p(\mathbf{x}, \mathbf{z} | \Theta)
\end{aligned}
$$

[解析]：EM算法中的M步，参见$7.6$节。

## 14.32

$${\rm ln}p(x)=\mathcal{L}(q)+{\rm KL}(q \parallel p)$$ 

[推导]：根据条件概率公式$p(x,z)=p(z|x)*p(x)$，可以得到$p(x)=\frac{p(x,z)}{p(z|x)}$

然后两边同时作用${\rm ln}$函数，可得${\rm ln}p(x)={\rm ln}\frac{p(x,z)}{p(z|x)}$    (1)

因为$q(z)$是概率密度函数，所以$1=\int q(z)dz$

等式两边同时乘以${\rm ln}p(x)$，因为${\rm ln}p(x)$是不关于变量$z$的函数，所以${\rm ln}p(x)$可以拿进积分里面，得到${\rm ln}p(x)=\int q(z){\rm ln}p(x)dz$
$$
\begin{aligned}
{\rm ln}p(x)&=\int q(z){\rm ln}p(x)dz \\
 &=\int q(z){\rm ln}\frac{p(x,z)}{p(z|x)}\\
 &=\int q(z){\rm ln}\bigg\{\frac{p(x,z)}{q(z)}\cdot\frac{q(z)}{p(z|x)}\bigg\} \\
 &=\int q(z)\bigg({\rm ln}\frac{p(x,z)}{q(z)}-{\rm ln}\frac{p(z|x)}{q(z)}\bigg) \\
  &=\int q(z){\rm ln}\bigg\{\frac{p(x,z)}{q(z)}\bigg\}-\int q(z){\rm ln}\frac{p(z|x)}{q(z)} \\
  &=\mathcal{L}(q)+{\rm KL}(q \parallel p)\qquad
\end{aligned}
$$
最后一行是根据$\mathcal{L}$和${\rm KL}$的定义。
## 14.33

$$
\mathcal{L}(q)=\int q(\mathbf{z}) \ln \left\{\frac{p(\mathbf{x}, \mathbf{z})}{q(\mathbf{z})}\right\} \mathrm{d} \mathbf{z}
$$



[解析]：见$14.32$解析。

## 14.34

$$
\mathrm{KL}(q \| p)=-\int q(\mathrm{z}) \ln \frac{p(\mathrm{z} | \mathrm{x})}{q(\mathrm{z})} \mathrm{d} \mathrm{z}
$$



[解析]：见$14.32$解析。

## 14.35

$$
q(\mathbf{z})=\prod_{i=1}^{M} q_{i}\left(\mathbf{z}_{i}\right)
$$

[解析]：再一次，条件独立的假设。可以看到，当问题复杂是往往简化问题到最简单最容易计算的局面，实际上往往效果不错。

## 14.36
$$
\begin{aligned}
\mathcal{L}(q)&=\int \prod_{i}q_{i}\bigg\{ {\rm ln}p({\rm \mathbf{x},\mathbf{z}})-\sum_{i}{\rm ln}q_{i}\bigg\}d{\rm\mathbf{z}} \\
&=\int q_{j}\bigg\{\int p(x,z)\prod_{i\ne j}q_{i}d{\rm\mathbf{z_{i}}}\bigg\}d{\rm\mathbf{z_{j}}}-\int q_{j}{\rm ln}q_{j}d{\rm\mathbf{z_{j}}}+{\rm const} \\
&=\int q_{j}{\rm ln}\tilde{p}({\rm \mathbf{x},\mathbf{z_{j}}})d{\rm\mathbf{z_{j}}}-\int q_{j}{\rm ln}q_{j}d{\rm\mathbf{z_{j}}}+{\rm const}
\end{aligned}
$$
[推导]：
$$
\mathcal{L}(q)=\int \prod_{i}q_{i}\bigg\{ {\rm ln}p({\rm \mathbf{x},\mathbf{z}})-\sum_{i}{\rm ln}q_{i}\bigg\}d{\rm\mathbf{z}}=\int\prod_{i}q_{i}{\rm ln}p({\rm \mathbf{x},\mathbf{z}})d{\rm\mathbf{z}}-\int\prod_{i}q_{i}\sum_{i}{\rm ln}q_{i}d{\rm\mathbf{z}}
$$
公式可以看做两个积分相减，我们先来看左边积分$\int\prod_{i}q_{i}{\rm ln}p({\rm \mathbf{x},\mathbf{z}})d{\rm\mathbf{z}}$的推导。
$$
\begin{aligned}
\int\prod_{i}q_{i}{\rm ln}p({\rm \mathbf{x},\mathbf{z}})d{\rm\mathbf{z}} &= \int q_{j}\prod_{i\ne j}q_{i}{\rm ln}p({\rm \mathbf{x},\mathbf{z}})d{\rm\mathbf{z}} \\
&= \int q_{j}\bigg\{\int{\rm ln}p({\rm \mathbf{x},\mathbf{z}})\prod_{i\ne j}q_{i}d{\rm\mathbf{z_{i}}}\bigg\}d{\rm\mathbf{z_{j}}}\qquad 
\end{aligned}
$$
即先对$\rm\mathbf{z_{j}}$求积分，再对$\rm\mathbf{z_{i}}$求积分，这个就是教材中的$14.36$左边的积分部分。
我们现在看下右边积分的推导$\int\prod_{i}q_{i}\sum_{i}{\rm ln}q_{i}d{\rm\mathbf{z}}$的推导。
在此之前我们看下$\int\prod_{i}q_{i}{\rm ln}q_{k}d{\rm\mathbf{z}}$的计算
$$
\begin{aligned}
\int\prod_{i}q_{i}{\rm ln}q_{k}d{\rm\mathbf{z}}&= \int q_{i^{\prime}}\prod_{i\ne i^{\prime}}q_{i}{\rm ln}q_{k}d{\rm\mathbf{z}}\qquad  \\
&=\int q_{i^{\prime}}\bigg\{\int\prod_{i\ne i^{\prime}}q_{i}{\rm ln}q_{k}d{\rm\mathbf{z_{i}}}\bigg\}d{\rm\mathbf{z_{i^{\prime}}}}
\end{aligned}
$$
第一个等式是一个展开项，选取一个变量$q_{i^{\prime}}, i^{\prime}\ne k$，由于
$\bigg\{\int\prod_{i\ne i^{\prime}}q_{i}{\rm ln}q_{k}d{\rm\mathbf{z_{i}}}\bigg\}$部分与变量$q_{i^{\prime}}$无关，所以可以拿到积分外面。又因为$\int q_{i^{\prime}}d{\rm\mathbf{z_{i^{\prime}}}}=1$，所以
$$
\begin{aligned}
\int\prod_{i}q_{i}{\rm ln}q_{k}d{\rm\mathbf{z}}&=\int\prod_{i\ne i^{\prime}}q_{i}{\rm ln}q_{k}d{\rm\mathbf{z_{i}}} \\
&= \int q_{k}{\rm ln}q_{k}d{\rm\mathbf{z_k}}\qquad 
\end{aligned}
$$
即所有$k$以外的变量都可以通过上面的方式消除,有了这个结论，我们再来看公式
$$
\begin{aligned}
\int\prod_{i}q_{i}\sum_{i}{\rm ln}q_{i}d{\rm\mathbf{z}}&= \int\prod_{i}q_{i}{\rm ln}q_{j}d{\rm\mathbf{z}} + \sum_{k\ne j}\int\prod_{i}q_{i}{\rm ln}q_{k}d{\rm\mathbf{z}} \\
&= \int q_{j}{\rm ln}q_{j}d{\rm\mathbf{z_j}} + \sum_{z\ne j}\int q_{k}{\rm ln}q_{k}d{\rm\mathbf{z_k}}\qquad \\
&= \int q_{j}{\rm ln}q_{j}d{\rm\mathbf{z_j}} + {\rm const} \qquad
\end{aligned}
$$
其中第二个等式是依据上述规律进行消除，最后将与$q_j$无关的部分写作$\rm const$，这个就是$14.36$右边的积分部分。
## 14.37

$$
\ln \tilde{p}\left(\mathbf{x}, \mathbf{z}_{j}\right)=\mathbb{E}_{i \neq j}[\ln p(\mathbf{x}, \mathbf{z})]+\text { const }
$$



[解析]：参见14.36

## 14.38

$$
\mathbb{E}_{i \neq j}[\ln p(\mathbf{x}, \mathbf{z})]=\int \ln p(\mathbf{x}, \mathbf{z}) \prod_{i \neq j} q_{i} \mathrm{d} \mathbf{z}_{i}
$$



[解析]：参见14.36

## 14.39

$$
\ln q_{j}^{*}\left(\mathbf{z}_{j}\right)=\mathbb{E}_{i \neq j}[\ln p(\mathbf{x}, \mathbf{z})]+\mathrm{const}
$$

[解析]：散度取得极值的条件是两个概率分布相同，见附录$C.3$。

## 14.40

$$
\begin{aligned} 
q_j^*(\mathbf{z}_j) = \frac{ \exp\left ( \mathbb{E}_{i\neq j}[\ln (p(\mathbf{x},\mathbf{z}))] \right ) }{\int \exp\left ( \mathbb{E}_{i\neq j}[\ln (p(\mathbf{x},\mathbf{z}))] \right ) \mathrm{d}\mathbf{z}_j}
\end{aligned}
$$

[推导]：由$14.39$去对数并积分
$$
\begin{aligned} 
 \int q_j^*(\mathbf{z}_j)\mathrm{d}\mathbf{z}_j &=\int \exp\left ( \mathbb{E}_{i\neq j}[\ln (p(\mathbf{x},\mathbf{z}))] \right )\cdot\exp(const) \, \mathrm{d}\mathbf{z}_j \\
 &=\exp(const) \int \exp\left ( \mathbb{E}_{i\neq j}[\ln (p(\mathbf{x},\mathbf{z}))] \right ) \, \mathrm{d}\mathbf{z}_j \\
 &= 1
 \end{aligned}
$$
所以
$$
\exp(const)  = \dfrac{1}{\int \exp\left ( \mathbb{E}_{i\neq j}[\ln (p(\mathbf{x},\mathbf{z}))] \right ) \, \mathrm{d}\mathbf{z}_j}  \\
$$

$$
\begin{aligned} 
  q_j^*(\mathbf{z}_j) &= \exp\left ( \mathbb{E}_{i\neq j}[\ln (p(\mathbf{x},\mathbf{z}))] \right )\cdot\exp(const)  \\
 &= \frac{ \exp\left ( \mathbb{E}_{i\neq j}[\ln (p(\mathbf{x},\mathbf{z}))] \right ) }{\int \exp\left ( \mathbb{E}_{i\neq j}[\ln (p(\mathbf{x},\mathbf{z}))] \right ) \mathrm{d}\mathbf{z}_j}
 \end{aligned}
$$

## 14.41

$$
p(\boldsymbol W,\boldsymbol z,\boldsymbol \beta,\boldsymbol \theta | \boldsymbol \alpha,\boldsymbol \eta) =
\prod_{t=1}^{T}p(\boldsymbol \theta_t | \boldsymbol \alpha)
\prod_{k=1}^{K}p(\boldsymbol \beta_k | \boldsymbol \eta) 
(\prod_{n=1}^{N}P(w_{t,n} | z_{t,n}, \boldsymbol \beta_k)P( z_{t,n} | \boldsymbol \theta_t))
$$



[解析]：此式表示LDA模型下根据参数$\alpha, \eta$生成文档$W$的概率。其中$z, \beta, \theta$是生成过程的中间变量。具体的生成步骤可见概率图14.12，图中的箭头和式14.41中的条件概率中的因果项目一一对应。这里共有三个连乘符号，表示三个相互独立的概率关系。第一个连乘表示T个文档每个文档的话题分布都是相互独立的。第二个连乘表示K个话题每个话题下单词的分布是相互独立的。最后一个连乘号表示每篇文档中的所有单词的生成是相互独立的。

## 14.42

$$
p\left(\Theta_{t} | \boldsymbol{\alpha}\right)=\frac{\Gamma\left(\sum_{k} \alpha_{k}\right)}{\prod_{k} \Gamma\left(\alpha_{k}\right)} \prod_{k} \Theta_{t, k}^{\alpha_{k}-1}
$$

[解析]：参见附录$C1.6$。

## 14.43

$$
L L(\boldsymbol{\alpha}, \boldsymbol{\eta})=\sum_{t=1}^{T} \ln p\left(\boldsymbol{w}_{t} | \boldsymbol{\alpha}, \boldsymbol{\eta}\right)
$$

[解析]：对数似然函数。参见$7.2$极大似然估计。

## 14.44

$$
p(\mathbf{z}, \boldsymbol{\beta}, \Theta | \mathbf{W}, \boldsymbol{\alpha}, \boldsymbol{\eta})=\frac{p(\mathbf{W}, \mathbf{z}, \boldsymbol{\beta}, \boldsymbol{\Theta} | \boldsymbol{\alpha}, \boldsymbol{\eta})}{p(\mathbf{W} | \boldsymbol{\alpha}, \boldsymbol{\eta})}
$$

[解析]：分母为边际分布，需要对变量$\mathbf{z}, \boldsymbol{\beta}, \Theta$ 积分或者求和，所以往往难以直接求解。