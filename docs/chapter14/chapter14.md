## 14.26

$$p(x^t)T(x^{t-1}|x^t)=p(x^{t-1})T(x^t|x^{t-1})$$

[解析]：假设变量$x$所在的空间有$n$个状态($s_1,s_2,..,s_n$), 定义在该空间上的一个转移矩阵$T(n\times n)$如果满足一定的条件则该马尔可夫过程存在一个稳态分布$\pi$, 使得
$$
\begin{aligned}
\pi T=\pi
\end{aligned}
\tag{1}
$$
其中, $\pi$是一个是一个$n$维向量，代表​$s_1,s_2,..,s_n$对应的概率. 反过来, 如果我们希望采样得到符合某个分布​$\pi$的一系列变量​$x_1,x_2,..,x_t$, 应当采用哪一个转移矩阵​$T(n\times n)​$呢？

事实上，转移矩阵只需要满足马尔可夫细致平稳条件
$$
\begin{aligned}
\pi (i)T(i,j)=\pi (j)T(j,i)
\end{aligned}
\tag{2}
$$
即公式$14.26​$，这里采用的符号与西瓜书略有区别以便于理解.  证明如下
$$
\begin{aligned}
\pi T(j) = \sum _i \pi (i)T(i,j) = \sum _i \pi (j)T(j,i) = \pi(j)
\end{aligned} 
\tag{3}
$$
假设采样得到的序列为$x_1,x_2,..,x_{t-1},x_t$，则可以使用$MH$算法来使得$x_{t-1}$(假设为状态$s_i$)转移到$x_t$(假设为状态$s_j$)的概率满足式$(2)$.

## 14.28

$$A(x^* | x^{t-1}) = \min\left ( 1,\frac{p(x^*)Q(x^{t-1} | x^*) }{p(x^{t-1})Q(x^* | x^{t-1})} \right )$$

[推导]：这个公式其实是拒绝采样的一个trick，因为基于式$14.27​$只需要
$$
\begin{aligned}
  A(x^* | x^{t-1}) &= p(x^*)Q(x^{t-1} | x^*)  \\
  A(x^{t-1} | x^*) &= p(x^{t-1})Q(x^* | x^{t-1})
 \end{aligned} 
 \tag{4}
$$
即可满足式$14.26$，但是实际上等号右边的数值可能比较小，比如各为0.1和0.2，那么好不容易才到的样本只有百分之十几得到利用，所以不妨将接受率设为0.5和1，则细致平稳分布条件依然满足，样本利用率大大提高, 所以可以将$(4)$改进为
$$
\begin{aligned} 
A(x^* | x^{t-1}) &=  \frac{p(x^*)Q(x^{t-1} | x^*)}{norm}  \\  
A(x^{t-1} | x^*) &= \frac{p(x^{t-1})Q(x^* | x^{t-1}) }{norm}
\end{aligned}  
\tag{5}
$$
其中
$$
\begin{aligned} 
norm = \max\left (p(x^{t-1})Q(x^* | x^{t-1}),p(x^*)Q(x^{t-1} | x^*) \right )
\end{aligned}  
\tag{6}
$$
即教材的$14.28​$.

## 14.32

$${\rm ln}p(x)=\mathcal{L}(q)+{\rm KL}(q \parallel p)$$ 

[推导]：根据条件概率公式$p(x,z)=p(z|x)*p(x)$，可以得到$p(x)=\frac{p(x,z)}{p(z|x)}$

然后两边同时作用${\rm ln}$函数，可得${\rm ln}p(x)={\rm ln}\frac{p(x,z)}{p(z|x)}$    (1)

因为$q(z)$是概率密度函数，所以$1=\int q(z)dz$

等式两边同时乘以${\rm ln}p(x)$，因为${\rm ln}p(x)$是不关于变量$z$的函数，所以${\rm ln}p(x)$可以拿进积分里面，得到${\rm ln}p(x)=\int q(z){\rm ln}p(x)dz$
$$
\begin{aligned}
{\rm ln}p(x)&=\int q(z){\rm ln}p(x) \\
 &=\int q(z){\rm ln}\frac{p(x,z)}{p(z|x)}\qquad(带入公式(1))\\
 &=\int q(z){\rm ln}\bigg\{\frac{p(x,z)}{q(z)}\cdot\frac{q(z)}{p(z|x)}\bigg\} \\
 &=\int q(z)\bigg({\rm ln}\frac{p(x,z)}{q(z)}-{\rm ln}\frac{p(z|x)}{q(z)}\bigg) \\
  &=\int q(z){\rm ln}\bigg\{\frac{p(x,z)}{q(z)}\bigg\}-\int q(z){\rm ln}\frac{p(z|x)}{q(z)} \\
  &=\mathcal{L}(q)+{\rm KL}(q \parallel p)\qquad(根据\mathcal{L}和{\rm KL}的定义)
\end{aligned}
$$


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
&= \int q_{j}\bigg\{\int{\rm ln}p({\rm \mathbf{x},\mathbf{z}})\prod_{i\ne j}q_{i}d{\rm\mathbf{z_{i}}}\bigg\}d{\rm\mathbf{z_{j}}}\qquad (先对{\rm\mathbf{z_{j}}}求积分，再对{\rm\mathbf{z_{i}}}求积分)
\end{aligned}
$$
这个就是教材中的$14.36$左边的积分部分。

我们现在看下右边积分的推导$\int\prod_{i}q_{i}\sum_{i}{\rm ln}q_{i}d{\rm\mathbf{z}}$的推导。

在此之前我们看下$\int\prod_{i}q_{i}{\rm ln}q_{k}d{\rm\mathbf{z}}$的计算
$$
\begin{aligned}
\int\prod_{i}q_{i}{\rm ln}q_{k}d{\rm\mathbf{z}}&= \int q_{i^{\prime}}\prod_{i\ne i^{\prime}}q_{i}{\rm ln}q_{k}d{\rm\mathbf{z}}\qquad (选取一个变量q_{i^{\prime}}, i^{\prime}\ne k) \\
&=\int q_{i^{\prime}}\bigg\{\int\prod_{i\ne i^{\prime}}q_{i}{\rm ln}q_{k}d{\rm\mathbf{z_{i}}}\bigg\}d{\rm\mathbf{z_{i^{\prime}}}}
\end{aligned}
$$
$\bigg\{\int\prod_{i\ne i^{\prime}}q_{i}{\rm ln}q_{k}d{\rm\mathbf{z_{i}}}\bigg\}$部分与变量$q_{i^{\prime}}$无关，所以可以拿到积分外面。又因为$\int q_{i^{\prime}}d{\rm\mathbf{z_{i^{\prime}}}}=1$，所以
$$
\begin{aligned}
\int\prod_{i}q_{i}{\rm ln}q_{k}d{\rm\mathbf{z}}&=\int\prod_{i\ne i^{\prime}}q_{i}{\rm ln}q_{k}d{\rm\mathbf{z_{i}}} \\
&= \int q_{k}{\rm ln}q_{k}d{\rm\mathbf{z_k}}\qquad (所有k以外的变量都可以通过上面的方式消除)
\end{aligned}
$$
有了这个结论，我们再来看公式
$$
\begin{aligned}
\int\prod_{i}q_{i}\sum_{i}{\rm ln}q_{i}d{\rm\mathbf{z}}&= \int\prod_{i}q_{i}{\rm ln}q_{j}d{\rm\mathbf{z}} + \sum_{k\ne j}\int\prod_{i}q_{i}{\rm ln}q_{k}d{\rm\mathbf{z}} \\
&= \int q_{j}{\rm ln}q_{j}d{\rm\mathbf{z_j}} + \sum_{z\ne j}\int q_{k}{\rm ln}q_{k}d{\rm\mathbf{z_k}}\qquad (根据上面结论) \\
&= \int q_{j}{\rm ln}q_{j}d{\rm\mathbf{z_j}} + {\rm const} \qquad (这里我们关心的是q_{j}，其他变量可以视为{\rm const})
\end{aligned}
$$
这个就是$14.36$右边的积分部分。

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
 \tag{7}
$$
所以
$$
\exp(const)  = \dfrac{1}{\int \exp\left ( \mathbb{E}_{i\neq j}[\ln (p(\mathbf{x},\mathbf{z}))] \right ) \, \mathrm{d}\mathbf{z}_j}  \\
\tag{8}
$$

$$
\begin{aligned} 
  q_j^*(\mathbf{z}_j) &= \exp\left ( \mathbb{E}_{i\neq j}[\ln (p(\mathbf{x},\mathbf{z}))] \right )\cdot\exp(const) \, \mathrm{d}\mathbf{z}_j \\
 &= \frac{ \exp\left ( \mathbb{E}_{i\neq j}[\ln (p(\mathbf{x},\mathbf{z}))] \right ) }{\int \exp\left ( \mathbb{E}_{i\neq j}[\ln (p(\mathbf{x},\mathbf{z}))] \right ) \mathrm{d}\mathbf{z}_j}
 \end{aligned}
 \tag{9}
$$
