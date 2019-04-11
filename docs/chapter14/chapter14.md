## 14.26

$$
\begin{aligned}
 p(x^t)T(x^{t-1}|x^t)=p(x^{t-1})T(x^t|x^{t-1})
 \end{aligned}
 \tag{14.26}
$$

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

$$
\begin{aligned} 
A(x^* | x^{t-1}) = \min\left ( 1,\frac{p(x^*)Q(x^{t-1} | x^*) }{p(x^{t-1})Q(x^* | x^{t-1})} \right )
\end{aligned} 
\tag{14.28}
$$

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

## 14.40

$$
\begin{aligned} 
q_j^*(\mathbf{z}_j) = \frac{ \exp\left ( \mathbb{E}_{i\neq j}[\ln (p(\mathbf{x},\mathbf{z}))] \right ) }{\int \exp\left ( \mathbb{E}_{i\neq j}[\ln (p(\mathbf{x},\mathbf{z}))] \right ) \mathrm{d}\mathbf{z}_j}
\end{aligned}
$$

[推导]：由$14.39$去对数直接可得
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
  q_j^*(\mathbf{z}_j)\mathrm{d}\mathbf{z}_j &= \exp\left ( \mathbb{E}_{i\neq j}[\ln (p(\mathbf{x},\mathbf{z}))] \right )\cdot\exp(const) \, \mathrm{d}\mathbf{z}_j \\
 &= \frac{ \exp\left ( \mathbb{E}_{i\neq j}[\ln (p(\mathbf{x},\mathbf{z}))] \right ) }{\int \exp\left ( \mathbb{E}_{i\neq j}[\ln (p(\mathbf{x},\mathbf{z}))] \right ) \mathrm{d}\mathbf{z}_j}
 \end{aligned}
 \tag{9}
$$

