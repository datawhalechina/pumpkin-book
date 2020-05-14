## 11.1

$$
\operatorname{Gain}(A)=\operatorname{Ent}(D)-\sum_{v=1}^{V} \frac{\left|D^{v}\right|}{|D|} \operatorname{Ent}\left(D^{v}\right)
$$

[解析]：此为信息增益的定义式，对数据集$D$和属性子集$A$，假设根据$A$的取值将$D$分为了$V$个子集$\{D^1,D^2,\dots,D^V\}$，那么信息增益的定义为划分之前数据集$D$的信息熵和划分之后每个子数据集$D^v$的信息熵的差。熵用来衡量一个系统的混乱程度，因此划分前和划分后熵的差越大，表示划分越有效，划分带来的”信息增益“越大。

## 11.2

$$
\operatorname{Ent}(D)=-\sum_{i=1}^{| \mathcal{Y |}} p_{k} \log _{2} p_{k}
$$

[解析]：此为信息熵的定义式，其中$p_k, k=1, 2, \dots \vert\mathcal{Y}\vert$表示$D$中第$i$类样本所占的比例。可以看出，样本越纯，即$p_k\rightarrow 0$或$p_k\rightarrow 1$时，$\mathrm{Ent}(D)$越小，其最小值为0。此时必有$p_i=1, p_{\backslash i}=0, i=1, 2, \dots, \vert\mathcal{Y}\vert$。

## 11.5

$$
\min _{\boldsymbol{w}} \sum_{i=1}^{m}\left(y_{i}-\boldsymbol{w}^{\mathrm{T}} \boldsymbol{x}_{i}\right)^{2}
$$

[解析]：该式为线性回归的优化目标式，$y_i$表示样本$i$的真实值，而$w^\top x_i$表示其预测值，这里使用预测值和真实值差的平方衡量预测值偏离真实值的大小。

## 11.6

$$
\min _{\boldsymbol{w}} \sum_{i=1}^{m}\left(y_{i}-\boldsymbol{w}^{\mathrm{T}} \boldsymbol{x}_{i}\right)^{2}+\lambda\|\boldsymbol{w}\|_{2}^{2}
$$

[解析]：该式为加入了$\mathrm{L}_2$正规化项的优化目标，也叫"岭回归"，$\lambda$用来调节误差项和正规化项的相对重要性，引入正规化项的目的是为了防止$w$的分量过太而导致过拟合的风险。

## 11.7

$$
\min _{\boldsymbol{w}} \sum_{i=1}^{m}\left(y_{i}-\boldsymbol{w}^{\mathrm{T}} \boldsymbol{x}_{i}\right)^{2}+\lambda\|\boldsymbol{w}\|_{1}
$$

[解析]：该式将11.6中的$\mathrm{L}_2$正规化项替换成了$\mathrm{L}_1$正规化项，也叫LASSO回归。关于$\mathrm{L}_2$和$\mathrm{L}_1$两个正规化项的区别，原书图11.2给出了很形象的解释。具体来说，结合$\mathrm{L}_1$范数优化的模型参数分量更偏向于取0，因此更容易取得稀疏解。

## 11.10

$$
\begin{aligned}
\hat{f}(\boldsymbol{x}) & \simeq f\left(\boldsymbol{x}_{k}\right)+\left\langle\nabla f\left(\boldsymbol{x}_{k}\right), \boldsymbol{x}-\boldsymbol{x}_{k}\right\rangle+\frac{L}{2}\left\|\boldsymbol{x}-\boldsymbol{x}_{k}\right\|^{2} \\
&=\frac{L}{2}\left\|\boldsymbol{x}-\left(\boldsymbol{x}_{k}-\frac{1}{L} \nabla f\left(\boldsymbol{x}_{k}\right)\right)\right\|_{2}^{2}+\mathrm{const}
\end{aligned}
$$

[解析]：首先注意优化目标式和11.7 LASSO回归的联系和区别，该式中的$x$对应到式11.7的$w$，即我们优化的目标。再解释下什么是[$L\mathrm{-Lipschitz}$条件](https://zh.wikipedia.org/wiki/利普希茨連續)，根据维基百科的定义：它是一个比通常[连续](https://zh.wikipedia.org/wiki/連續函數)更强的光滑性条件。直觉上，利普希茨连续函数限制了函数改变的速度，符合利普希茨条件的函数的斜率，必小于一个称为利普希茨常数的实数（该常数依函数而定）。

注意这里可能存在一个笔误，在wiki百科的定义中，式11.7应该写成
$$
\left\vert\nabla f\left(\boldsymbol{x}^{\prime}\right)-\nabla f(\boldsymbol{x})\right\vert \leqslant L\left\vert\boldsymbol{x}^{\prime}-\boldsymbol{x}\right\vert \quad\left(\forall \boldsymbol{x}, \boldsymbol{x}^{\prime}\right)
$$
移项得
$$
\frac{\left|\nabla f\left(\boldsymbol{x}^{\prime}\right)-\nabla f(\boldsymbol{x})\right|}{\vert x^\prime - x\vert}\leqslant L \quad\left(\forall \boldsymbol{x}, \boldsymbol{x}^{\prime}\right)
$$
由于上式对所有的$x, x^\prime$都成立，由[导数的定义](https://zh.wikipedia.org/wiki/导数)，上式可以看成是$f(x)$的二阶导数恒不大于$L$。即
$$
\nabla^2f(x)\leqslant L
$$
得到这个结论之后，我们来推导式11.10。

由[泰勒公式](https://zh.wikipedia.org/wiki/泰勒公式)，$x_k$附近的$f(x)$通过二阶泰勒展开式可近似为
$$
\begin{aligned}
\hat{f}(\boldsymbol{x}) & \simeq f\left(\boldsymbol{x}_{k}\right)+\left\langle\nabla f\left(\boldsymbol{x}_{k}\right), \boldsymbol{x}-\boldsymbol{x}_{k}\right\rangle+\frac{\nabla^2f(x_k)}{2}\left\|\boldsymbol{x}-\boldsymbol{x}_{k}\right\|^{2} \\
&\leqslant
 f\left(\boldsymbol{x}_{k}\right)+\left\langle\nabla f\left(\boldsymbol{x}_{k}\right), \boldsymbol{x}-\boldsymbol{x}_{k}\right\rangle+\frac{L}{2}\left\|\boldsymbol{x}-\boldsymbol{x}_{k}\right\|^{2} \\
&= f\left(\boldsymbol{x}_{k}\right)+\nabla f\left(\boldsymbol{x}_{k}\right)^{\top}\left(\boldsymbol{x}-\boldsymbol{x}_{k}\right)+\frac{L}{2}\left(\boldsymbol{x}-\boldsymbol{x}_{k}\right)^{\top}\left(\boldsymbol{x}-\boldsymbol{x}_{k}\right)\\
&=f(x_k)+\frac{L}{2}\left(\left(\boldsymbol{x}-\boldsymbol{x}_{k}\right)^{\top}\left(\boldsymbol{x}-\boldsymbol{x}_{k}\right)+\frac{2}{L}\nabla f\left(\boldsymbol{x}_{k}\right)^{\top}\left(\boldsymbol{x}-\boldsymbol{x}_{k}\right)\right)\\
&=f(x_k)+\frac{L}{2}\left(\left(\boldsymbol{x}-\boldsymbol{x}_{k}\right)^{\top}\left(\boldsymbol{x}-\boldsymbol{x}_{k}\right)+\frac{2}{L}\nabla f\left(\boldsymbol{x}_{k}\right)^{\top}\left(\boldsymbol{x}-\boldsymbol{x}_{k}\right)+\frac{1}{L^2}\nabla f(x_k)^\top\nabla f(x_k)\right) -\frac{1}{2L}\nabla f(x_k)^\top\nabla f(x_k)\\
&=f(x_k)+\frac{L}{2}\left(\left(\boldsymbol{x}-\boldsymbol{x}_{k}\right)+\frac{1}{L} \nabla f\left(\boldsymbol{x}_{k}\right)\right)^{\top}\left(\left(\boldsymbol{x}-\boldsymbol{x}_{k}\right)+\frac{1}{L} \nabla f\left(\boldsymbol{x}_{k}\right)\right)-\frac{1}{2L}\nabla f(x_k)^\top\nabla f(x_k)\\
&=\frac{L}{2}\left\|\boldsymbol{x}-\left(\boldsymbol{x}_{k}-\frac{1}{L} \nabla f\left(\boldsymbol{x}_{k}\right)\right)\right\|_{2}^{2}+\mathrm{const}
\end{aligned}
$$
其中$\mathrm{const}=f(x_k)--\frac{1}{2 L} \nabla f\left(x_{k}\right)^{\top} \nabla f\left(x_{k}\right)$



## 11.11

$$
\boldsymbol{x}_{k+1}=\boldsymbol{x}_{k}-\frac{1}{L} \nabla f\left(\boldsymbol{x}_{k}\right)
$$
[解析]：这个很容易理解，因为2范数的最小值为0，当$\boldsymbol{x}_{k+1}=\boldsymbol{x}_{k}-\frac{1}{L} \nabla f\left(\boldsymbol{x}_{k}\right)$时，$\hat{f}(x_{k+1})\leqslant\hat{f}(x_k)$恒成立，同理$\hat{f}(x_{k+2})\leqslant\hat{f}(x_{k+1}), \cdots$，因此反复迭代能够使$\hat{f}(x)$的值不断下降。

## 11.12

$$
\boldsymbol{x}_{k+1}=\underset{\boldsymbol{x}}{\arg \min } \frac{L}{2}\left\|\boldsymbol{x}-\left(\boldsymbol{x}_{k}-\frac{1}{L} \nabla f\left(\boldsymbol{x}_{k}\right)\right)\right\|_{2}^{2}+\lambda\|\boldsymbol{x}\|_{1}
$$

[解析]：式11.11是用来优化$\hat{f}(x)$的，而对于式11.8，优化的函数为$f(x)+\lambda\left\Vert x\right\Vert_1$，由泰勒展开公式，优化的目标可近似为$\hat{f}(x)+\lambda\Vert x\Vert_1$，根据式11.10可知，$x$的更新由式11.12决定。

## 11.13

$$
\boldsymbol{x}_{k+1}=\underset{\boldsymbol{x}}{\arg \min } \frac{L}{2}\|\boldsymbol{x}-\boldsymbol{z}\|_{2}^{2}+\lambda\|\boldsymbol{x}\|
$$

[解析]：这里将式11.12的优化步骤拆分成了两步，首先令$z=x_{k}-\frac{1}{L} \nabla f\left(x_{k}\right)$以计算$z$，然后再求解式11.13，得到的结果是一致的。

## 11.14

$$
x_{k+1}^{i}=\left\{\begin{array}{ll}
{z^{i}-\lambda / L,} & {\lambda / L<z^{i}} \\
{0,} & {\left|z^{i}\right| \leqslant \lambda / L} \\
{z^{i}+\lambda / L,} & {z^{i}<-\lambda / L}
\end{array}\right.
$$

[解析]：令优化函数
$$
\begin{aligned}
g(\boldsymbol{x}) &=\frac{L}{2}\|\boldsymbol{x}-\boldsymbol{z}\|_{2}^{2}+\lambda\|\boldsymbol{x}\|_{1} \\
&=\frac{L}{2} \sum_{i=1}^{d}\left\|x^{i}-z^{i}\right\|_{2}^{2}+\lambda \sum_{i=1}^{d}\left\|x^{i}\right\|_{1} \\
&=\sum_{i=1}^{d}\left(\frac{L}{2}\left(x^{i}-z^{i}\right)^{2}+\lambda\left|x^{i}\right|\right)
\end{aligned}
$$

这个式子表明优化$g(\boldsymbol{x})$可以被拆解成优化$\boldsymbol{x}$的各个分量的形式，对分量$x_i$，其优化函数
$$
g\left(x^{i}\right)=\frac{L}{2}\left(x^{i}-z^{i}\right)^{2}+\lambda\left|x^{i}\right|
$$
求导得
$$
\frac{d g\left(x^{i}\right)}{d x^{i}}=L\left(x^{i}-z^{i}\right)+\lambda s g n\left(x^{i}\right)
$$
其中
$$
\operatorname{sign}\left(x^{i}\right)=\left\{\begin{array}{ll}
{1,} & {x^{i}>0} \\
{-1,} & {x^{i}<0}
\end{array}\right.
$$
称为[符号函数](https://en.wikipedia.org/wiki/Sign_function)，对于$x_i=0$的特殊情况，由于$\vert x_i \vert$在$x_i=0$点出不光滑，所以其不可导，需单独讨论。令$\frac{d g\left(x^{i}\right)}{d x^{i}}=0$有
$$
x^{i}=z^{i}-\frac{\lambda}{L} \operatorname{sign}\left(x^{i}\right)
$$
此式的解即为优化目标$g(x^i)$的极值点，因为等式两端均含有未知变量$x^i$，故分情况讨论。

1. 当$z^i>\frac{\lambda}{L}$时：

   a. 假设$x^i<0$，则$\operatorname{sign}(x^i)=-1$，那么有$x^i=z^i+\frac{\lambda}{L}>0$与假设矛盾；

   b. 假设$x^i>0$，则$\operatorname{sign}(x^i)=1$，那么有$x^i=z^i-\frac{\lambda}{L}<0$和假设相符和，下面来检验$x^i=z^i-\frac{\lambda}{L}$是否是使函数$g(x^i)$的取得最小值。当$x^i<0$时，
   $$
   \frac{d g\left(x^{i}\right)}{d x^{i}}=L\left(x^{i}-z^{i}\right)-\lambda
   $$
   在定义域内连续可导，则$g(x^i)$的二阶导数
   $$
   \frac{d^2 g\left(x^{i}\right)}{{d x^{i}}^2}=L
   $$
   由于$L$是Lipschitz常数恒大于0，因为$x^i=z^i-\frac{\lambda}{L}$是函数$g(x^i)$的最小值。

2. 当$z_i<-\frac{\lambda}{L}$时：

   a. 假设$x^i>0$，则$\operatorname{sign}(x^i)=1$，那么有$x^i=z^i-\frac{\lambda}{L}<0$与假设矛盾；

   b. 假设$x^i<0$，则$\operatorname{sign}(x^i)=-1$，那么有$x^i=z^i+\frac{\lambda}{L}<0$与假设相符，由上述二阶导数恒大于0可知，$x^i=z^i+\frac{\lambda}{L}$是$g(x^i)$的最小值。

3. 当$-\frac{\lambda}{L} \leqslant z_i \leqslant \frac{\lambda}{L}$时：

   a. 假设$x^i>0$，则$\operatorname{sign}(x^i)=1$，那么有$x^i=z^i-\frac{\lambda}{L}\leqslant 0$与假设矛盾；

   b. 假设$x^i<0$，则$\operatorname{sign}(x^i)=-1$，那么有$x^i=z^i+\frac{\lambda}{L}\geqslant 0$与假设矛盾。

4. 最后讨论$x_i=0$的情况，此时$g(x^i)=\frac{L}{2}\left({z^i}\right)^2$

   a. 当$\vert z^i\vert>\frac{\lambda}{L}$时，由上述推导可知$g(x_i)$的最小值在$x^i=z^i-\frac{\lambda}{L}$处取得，令
   $$
   \begin{aligned}
   f(x^i)&=g(x^i)\vert_{x^i=0}-g(x^i)\vert_{x_i=z^i-\frac{\lambda}{L}}\\
   &=\frac{L}{2}\left({z^i}\right)^2 - \left(\lambda z^i-\frac{\lambda^2}{2L}\right)\\
   &=\frac{L}{2}\left(z^i-\frac{\lambda}{L}\right)^2\\
   &>0
   \end{aligned}
   $$
   

   因此当$\vert z^i\vert>\frac{\lambda}{L}$时，$x_i=0$不会是函数$g(x_i)$的最小值。

   b. 当$-\frac{\lambda}{L} \leqslant z_i \leqslant \frac{\lambda}{L}$时，对于任何$\Delta x\neq 0$有
   $$
   \begin{aligned}
   g(\Delta x) &=\frac{L}{2}\left(\Delta x-z^{i}\right)^{2}+\lambda|\Delta x| \\
   &=\frac{L}{2}\left((\Delta x)^{2}-2 \Delta x \cdot z^{i}+\frac{2 \lambda}{L}|\Delta x|\right)+\frac{L}{2}\left(z^{i}\right)^{2} \\
   &>\frac{L}{2}\left((\Delta x)^{2}-2 \Delta x \cdot z^{i}+\frac{2 \lambda}{L}\Delta x\right)+\frac{L}{2}\left(z^{i}\right)^{2}\\
   &>\frac{L}{2}\left(\Delta x\right)^2+\frac{L}{2}\left(z^{i}\right)^{2}\\
   &>g(x^i)\vert_{x^i=0}
   \end{aligned}
   $$
   因此$x^i=0$是$g(x^i)$的最小值点。

5. 综上所述，11.14成立

   

## 11.15

$$
\min _{\mathbf{B}, \boldsymbol{\alpha}_{i}} \sum_{i=1}^{m}\left\|\boldsymbol{x}_{i}-\mathbf{B} \boldsymbol{\alpha}_{i}\right\|_{2}^{2}+\lambda \sum_{i=1}^{m}\left\|\boldsymbol{\alpha}_{i}\right\|_{1}
$$
[解析]：这个式子表达的意思很容易理解，即希望样本$x_i$的稀疏表示$\boldsymbol{\alpha}_i$通过字典$\mathbf{B}$重构后和样本$x_i$的原始表示尽量相似，如果满足这个条件，那么稀疏表示$\boldsymbol{\alpha}_i$是比较好的。后面的1范数项是为了使表示更加稀疏。

## 11.16

$$
\min _{\boldsymbol{\alpha}_{i}}\left\|\boldsymbol{x}_{i}-\mathbf{B} \boldsymbol{\alpha}_{i}\right\|_{2}^{2}+\lambda\left\|\boldsymbol{\alpha}_{i}\right\|_{1}
$$

[解析]：为了优化11.15，我们采用变量交替优化的方式(有点类似EM算法)，首先固定变量$\mathbf{B}$，则11.15求解的是$m$个样本相加的最小值，因为公式里没有样本之间的交互(即文中所述$\alpha_{i}^{u} \alpha_{i}^{v}(u \neq v)$这样的形式)，因此可以对每个变量做分别的优化求出$\boldsymbol{\alpha}_i$，求解方法见11.13，11.14。

## 11.17

$$
\min _{\mathbf{B}}\|\mathbf{X}-\mathbf{B} \mathbf{A}\|_{F}^{2}
$$

[解析]：这是优化11.15的第二步，固定住$\boldsymbol{\alpha}_i, i=1, 2,\dots,m$，此时式11.15的第二项为一个常数，优化11.15即优化$\min _{\mathbf{B}} \sum_{i=1}^{m}\left\|\boldsymbol{x}_{i}-\mathbf{B} \boldsymbol{\alpha}_{i}\right\|_{2}^{2}$。其写成矩阵相乘的形式为$\min _{\mathbf{B}}\|\mathbf{X}-\mathbf{B} \mathbf{A}\|_{2}^{2}$，将2范数扩展到$F$范数即得优化目标为$\min _{\mathbf{B}}\|\mathbf{X}-\mathbf{B} \mathbf{A}\|_{F}^{2}$。

## 11.18

$$
\begin{aligned}
\min _{\mathbf{B}}\|\mathbf{X}-\mathbf{B} \mathbf{A}\|_{F}^{2} &=\min _{\boldsymbol{b}_{i}}\left\|\mathbf{X}-\sum_{j=1}^{k} \boldsymbol{b}_{j} \boldsymbol{\alpha}^{j}\right\|_{F}^{2} \\
&=\min _{\boldsymbol{b}_{i}}\left\|\left(\mathbf{X}-\sum_{j \neq i} \boldsymbol{b}_{j} \boldsymbol{\alpha}^{j}\right)-\boldsymbol{b}_{i} \boldsymbol{\alpha}^{i}\right\| _{F}^{2} \\
&=\min _{\boldsymbol{b}_{i}}\left\|\mathbf{E}_{i}-\boldsymbol{b}_{i} \boldsymbol{\alpha}^{i}\right\|_{F}^{2}
\end{aligned}
$$
[解析]：这个公式难点在于推导$\mathbf{B}\mathbf{A}=\sum_{j=1}^k\boldsymbol{b}_j\boldsymbol{\alpha}^j$。大致的思路是$\boldsymbol{b}_{j} \boldsymbol{\alpha}^{j}$会生成和矩阵$\mathbf{B}\mathbf{A}$同样维度的矩阵，这个矩阵对应位置的元素是$\mathbf{B}\mathbf{A}$中对应位置元素的一个分量，这样的分量矩阵一共有$k$个，把所有分量矩阵加起来就得到了最终结果。推导过程如下：
$$
\begin{aligned}
\boldsymbol B\boldsymbol A
& =\begin{bmatrix}
b_{1}^{1} &b_{2}^{1}  & \cdot  & \cdot  & \cdot  & b_{k}^{1}\\ 
b_{1}^{2} &b_{2}^{2}  & \cdot  & \cdot  & \cdot  & b_{k}^{2}\\ 
\cdot  & \cdot  & \cdot  &  &  & \cdot \\ 
\cdot  &  \cdot &  & \cdot  &  &\cdot  \\ 
 \cdot & \cdot  &  &  & \cdot  & \cdot \\ 
 b_{1}^{d}& b_{2}^{d}  & \cdot  & \cdot  &\cdot   &  b_{k}^{d}
\end{bmatrix}_{d\times k}\cdot 
\begin{bmatrix}
\alpha_{1}^{1} &\alpha_{2}^{1}  & \cdot  & \cdot  & \cdot  & \alpha_{m}^{1}\\ 
\alpha_{1}^{2} &\alpha_{2}^{2}  & \cdot  & \cdot  & \cdot  & \alpha_{m}^{2}\\ 
\cdot  & \cdot  & \cdot  &  &  & \cdot \\ 
\cdot  &  \cdot &  & \cdot  &  &\cdot  \\ 
 \cdot & \cdot  &  &  & \cdot  & \cdot \\ 
 \alpha_{1}^{k}& \alpha_{2}^{k}  & \cdot  & \cdot  &\cdot   &  \alpha_{m}^{k}
\end{bmatrix}_{k\times m} \\
& =\begin{bmatrix}
\sum_{j=1}^{k}b_{j}^{1}\alpha _{1}^{j} &\sum_{j=1}^{k}b_{j}^{1}\alpha _{2}^{j} & \cdot  & \cdot  & \cdot  & \sum_{j=1}^{k}b_{j}^{1}\alpha _{m}^{j}\\ 
\sum_{j=1}^{k}b_{j}^{2}\alpha _{1}^{j} &\sum_{j=1}^{k}b_{j}^{2}\alpha _{2}^{j}  & \cdot  & \cdot  & \cdot  & \sum_{j=1}^{k}b_{j}^{2}\alpha _{m}^{j}\\ 
\cdot  & \cdot  & \cdot  &  &  & \cdot \\ 
\cdot  &  \cdot &  & \cdot  &  &\cdot  \\ 
 \cdot & \cdot  &  &  & \cdot  & \cdot \\ 
\sum_{j=1}^{k}b_{j}^{d}\alpha _{1}^{j}& \sum_{j=1}^{k}b_{j}^{d}\alpha _{2}^{j}  & \cdot  & \cdot  &\cdot   &  \sum_{j=1}^{k}b_{j}^{d}\alpha _{m}^{j}
\end{bmatrix}_{d\times m} &
\end{aligned}
$$

$$
\begin{aligned}
\boldsymbol b_{\boldsymbol j}\boldsymbol \alpha ^{\boldsymbol j}
& =\begin{bmatrix}
b_{j}^{1}\\ b_{j}^{2}
\\ \cdot 
\\ \cdot 
\\ \cdot 
\\ b_{j}^{d}
\end{bmatrix}\cdot 
\begin{bmatrix}
 \alpha _{1}^{j}& \alpha _{2}^{j} & \cdot  & \cdot  & \cdot  & \alpha _{m}^{j}
\end{bmatrix}\\
& =\begin{bmatrix}
b_{j}^{1}\alpha _{1}^{j} &b_{j}^{1}\alpha _{2}^{j} & \cdot  & \cdot  & \cdot  & b_{j}^{1}\alpha _{m}^{j}\\ 
b_{j}^{2}\alpha _{1}^{j} &b_{j}^{2}\alpha _{2}^{j}  & \cdot  & \cdot  & \cdot  & b_{j}^{2}\alpha _{m}^{j}\\ 
\cdot  & \cdot  & \cdot  &  &  & \cdot \\ 
\cdot  &  \cdot &  & \cdot  &  &\cdot  \\ 
 \cdot & \cdot  &  &  & \cdot  & \cdot \\ 
b_{j}^{d}\alpha _{1}^{j}& b_{j}^{d}\alpha _{2}^{j}  & \cdot  & \cdot  &\cdot   &  b_{j}^{d}\alpha _{m}^{j}
\end{bmatrix}_{d\times m} &
\end{aligned}
$$

求和可得：
$$
\begin{aligned}
\sum_{j=1}^{k}\boldsymbol b_{\boldsymbol j}\boldsymbol \alpha ^{\boldsymbol j} 
& = \sum_{j=1}^{k}\left (\begin{bmatrix}
b_{1}^{j}\\ b_{w}^{j}
\\ \cdot 
\\ \cdot 
\\ \cdot 
\\ b_{d}^{j}
\end{bmatrix}\cdot 
\begin{bmatrix}
 \alpha _{1}^{j}& \alpha _{2}^{j} & \cdot  & \cdot  & \cdot  & \alpha _{m}^{j}
\end{bmatrix} \right )\\
& =\begin{bmatrix}
\sum_{j=1}^{k}b_{j}^{1}\alpha _{1}^{j} &\sum_{j=1}^{k}b_{j}^{1}\alpha _{2}^{j} & \cdot  & \cdot  & \cdot  & \sum_{j=1}^{k}b_{j}^{1}\alpha _{m}^{j}\\ 
\sum_{j=1}^{k}b_{j}^{2}\alpha _{1}^{j} &\sum_{j=1}^{k}b_{j}^{2}\alpha _{2}^{j}  & \cdot  & \cdot  & \cdot  & \sum_{j=1}^{k}b_{j}^{2}\alpha _{m}^{j}\\ 
\cdot  & \cdot  & \cdot  &  &  & \cdot \\ 
\cdot  &  \cdot &  & \cdot  &  &\cdot  \\ 
 \cdot & \cdot  &  &  & \cdot  & \cdot \\ 
\sum_{j=1}^{k}b_{j}^{d}\alpha _{1}^{j}& \sum_{j=1}^{k}b_{j}^{d}\alpha _{2}^{j}  & \cdot  & \cdot  &\cdot   &  \sum_{j=1}^{k}b_{j}^{d}\alpha _{m}^{j}
\end{bmatrix}_{d\times m} &
\end{aligned}
$$
得证。

将矩阵$\mathbf{B}$分解成矩阵列$\boldsymbol{b}_j,j=1,2,\dots,k$带来一个好处，即和11.16的原理相同，矩阵列与列之间无关，因此可以分别优化各个列，即将$\min_\mathbf{B}\Vert\dots\mathbf{B}\dots\Vert^2_F$转化成了$\min_{b_i}\Vert\cdots\boldsymbol{b}_i\cdots\Vert^2_F$，得到第三行的等式之后，再利用文中介绍的KSVD算法求解即可。

