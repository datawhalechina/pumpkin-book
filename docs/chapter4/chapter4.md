## 4.1
$$\operatorname{Ent}(D)=-\sum_{k=1}^{|\mathcal{Y}|}p_k\log_{2}{p_k}$$
[解析]：证明$0\leq\operatorname{Ent}(D)\leq\log_{2}|\mathcal{Y}|$：
已知集合$D$的信息熵的定义为
$$\operatorname{Ent}(D)=-\sum_{k=1}^{|\mathcal{Y}|} p_{k} \log _{2} p_{k}$$
其中，$|\mathcal{Y}|$表示样本类别总数，$p_k$表示第$k$类样本所占的比例，且$0 \leq p_k \leq 1,\sum_{k=1}^{n}p_k=1$。若令$|\mathcal{Y}|=n,p_k=x_k$，那么信息熵$\operatorname{Ent}(D)$就可以看作一个$n$元实值函数，也即
$$\operatorname{Ent}(D)=f(x_1,...,x_n)=-\sum_{k=1}^{n} x_{k} \log _{2} x_{k} $$
其中，$0 \leq x_k \leq 1,\sum_{k=1}^{n}x_k=1$，下面考虑求该多元函数的最值。<br>
**求最大值：**<br>
如果不考虑约束$0 \leq x_k \leq 1$，仅考虑$\sum_{k=1}^{n}x_k=1$的话，对$f(x_1,...,x_n)$求最大值等价于如下最小化问题
$$\begin{array}{ll}{
\operatorname{min}} & {\sum\limits_{k=1}^{n} x_{k} \log _{2} x_{k} } \\ 
{\text { s.t. }} & {\sum\limits_{k=1}^{n}x_k=1} 
\end{array}$$
显然，在$0 \leq x_k \leq 1$时，此问题为凸优化问题，而对于凸优化问题来说，满足KKT条件的点即为最优解。由于此最小化问题仅含等式约束，那么能令其拉格朗日函数的一阶偏导数等于0的点即为满足KKT条件的点。根据拉格朗日乘子法可知，该优化问题的拉格朗日函数为
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
所以$f(x_1,...,x_n)$在满足约束$0 \leq x_k \leq 1,\sum_{k=1}^{n}x_k=1$时的最大值为$\log _{2} n$。<br>
**求最小值：**<br>
如果不考虑约束$\sum_{k=1}^{n}x_k=1$，仅考虑$0 \leq x_k \leq 1$的话，$f(x_1,...,x_n)$可以看做是$n$个互不相关的一元函数的加和，也即
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