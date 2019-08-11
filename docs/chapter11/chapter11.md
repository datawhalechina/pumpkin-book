## 11.9
$$\left \| \nabla f(\boldsymbol x{}')-\nabla  f(\boldsymbol x) \right \|_{2}^{2} \leqslant L\left \| \boldsymbol x{}'-\boldsymbol x \right \|_{2}^{2} 　(\forall \boldsymbol x,\boldsymbol x{}'),$$
[解析]：*L-Lipschitz*条件定义为：对于函数$f(\boldsymbol x)$,若其任意定义域中的**x1**,**x2**，都存在L>0，使得$|f(\boldsymbol x1)-f(\boldsymbol x2)|≤L|\boldsymbol x1-\boldsymbol x2|$。通俗理解就是，对于函数上的每一对点，都存在一个实数L，使得它们连线斜率的绝对值不大于这个L，其中最小的L称为*Lipschitz*常数。
　　　将公式变形可以更好的理解：$$\frac{\left \| \nabla f(\boldsymbol x{}')-\nabla f(\boldsymbol x) \right \|_{2}^{2}}{\left \| \boldsymbol x{}'-\boldsymbol x \right \|_{2}^{2}}\leqslant L 　(\forall \boldsymbol x,\boldsymbol x{}'),$$
　　　进一步，如果$\boldsymbol x{}'\to  \boldsymbol x$，即$$\lim_{\boldsymbol x{}'\to \boldsymbol x}\frac{\left \| \nabla f(\boldsymbol x{}')-\nabla f(\boldsymbol x)\right \|_{2}^{2}}{\left \| \boldsymbol x{}'-\boldsymbol x \right \|_{2}^{2}}$$
　　　“ *Lipschitz*连续”很常见，知乎有一个问答(https://www.zhihu.com/question/51809602) 对*Lipschitz*连续的解释很形象：以陆地为例， 连续就是说这块地上没有特别陡的坡；其中最陡的地方有多陡呢？这就是所谓的*Lipschitz*常数。 
   
## 11.10

$$
\hat{f}(x) \simeq f(x_{k})+\langle \nabla f(x_{k}),x-x_{k} \rangle + \frac{L}{2}\left \| x-x_{k} \right\|^{2}
$$

[推导]：
$$
\begin{aligned}
\hat{f}(x) &\simeq f(x_{k})+\langle \nabla f(x_{k}),x-x_{k} \rangle + \frac{L}{2}\left \| x-x_{k} \right\|^{2} \\
&= f(x_{k})+\langle \nabla f(x_{k}),x-x_{k} \rangle + \langle\frac{L}{2}(x-x_{k}),x-x_{k}\rangle \\
&= f(x_{k})+\langle \nabla f(x_{k})+\frac{L}{2}(x-x_{k}),x-x_{k} \rangle \\
&= f(x_{k})+\frac{L}{2}\langle\frac{2}{L}\nabla f(x_{k})+(x-x_{k}),x-x_{k} \rangle \\
&= f(x_{k})+\frac{L}{2}\langle x-x_{k}+\frac{1}{L}\nabla f(x_{k})+\frac{1}{L}\nabla f(x_{k}),x-x_{k}+\frac{1}{L}\nabla f(x_{k})-\frac{1}{L}\nabla f(x_{k}) \rangle \\
&= f(x_{k})+\frac{L}{2}\left\| x-x_{k}+\frac{1}{L}\nabla f(x_{k}) \right\|_{2}^{2} -\frac{1}{2L}\left\|\nabla f(x_{k})\right\|_{2}^{2} \\
&= \frac{L}{2}\left\| x-(x_{k}-\frac{1}{L}\nabla f(x_{k})) \right\|_{2}^{2} + const \qquad (因为f(x_{k})和\nabla f(x_{k})是常数)
\end{aligned}
$$

## 11.13
$$\boldsymbol x_{\boldsymbol k+\boldsymbol 1}=\underset{\boldsymbol x}{argmin}\frac{L}{2}\left \| \boldsymbol x -\boldsymbol z\right \|_{2}^{2}+\lambda \left \| \boldsymbol x \right \|_{1}$$
[推导]：假设目标函数为$g(\boldsymbol x)$，则
$$
\begin{aligned}
g(\boldsymbol x)
& =\frac{L}{2}\left \|\boldsymbol  x \boldsymbol -\boldsymbol z\right \|_{2}^{2}+\lambda \left \| \boldsymbol x \right \|_{1}\\
& =\frac{L}{2}\sum_{i=1}^{d}\left \| x^{i} -z^{i}\right \|_{2}^{2}+\lambda \sum_{i=1}^{d}\left \| x^{i} \right \|_{1} \\
& =\sum_{i=1}^{d}(\frac{L}{2}(x^{i}-z^{i})^{2}+\lambda \left | x^{i}\right |)&
\end{aligned}
$$
由上式可见， $g(\boldsymbol x)$可以拆成 d个独立的函 数，求解式(11.13)可以分别求解d个独立的目标函数。 
针对目标函数$g(x^{i})=\frac{L}{2}(x^{i}-z^{i})^{2}+\lambda \left | x^{i}\right |$，通过求导求解极值：
$$\frac{dg(x^{i})}{dx^{i}}=L(x^{i}-z^{i})+\lambda sgn(x^{i})$$
其中$$sgn(x^{i})=\left\{\begin{matrix}
1, &x^{i}>0\\ 
 -1,& x^{i}<0
\end{matrix}\right.$$
令导数为0，可得：$$x^{i}=z^{i}-\frac{\lambda }{L}sgn(x^{i})$$可分为三种情况：
1. 当$z^{i}>\frac{\lambda }{L}$时：
    (1）假设此时的根$x^{i}<0$，则$sgn(x^{i})=-1$，所以$x^{i}=z^{i}+\frac{\lambda }{L}>0$，与假设矛盾。
    (2）假设此时的根$x^{i}>0$，则$sgn(x^{i})=1$，所以$x^{i}=z^{i}-\frac{\lambda }{L}>0$，成立。
2. 当$z^{i}<-\frac{\lambda }{L}$时：
    (1）假设此时的根$x^{i}>0$，则$sgn(x^{i})=1$，所以$x^{i}=z^{i}-\frac{\lambda }{L}<0$，与假设矛盾。
    (2）假设此时的根$x^{i}<0$，则$sgn(x^{i})=-1$，所以$x^{i}=z^{i}+\frac{\lambda }{L}<0$，成立。
3. 当$\left |z^{i}  \right |<\frac{\lambda }{L}$时：
    (1）假设此时的根$x^{i}>0$，则$sgn(x^{i})=1$，所以$x^{i}=z^{i}-\frac{\lambda }{L}<0$，与假设矛盾。
    (2）假设此时的根$x^{i}<0$，则$sgn(x^{i})=-1$，所以$x^{i}=z^{i}+\frac{\lambda }{L}>0$，与假设矛盾，此时$x^{i}=0$为函数的极小值。
综上所述可得函数闭式解如下：
$$x_{k+1}^{i}=\left\{\begin{matrix}
z^{i}-\frac{\lambda }{L}, &\frac{\lambda }{L}< z^{i}\\ 
0, & \left |z^{i}  \right |\leqslant \frac{\lambda }{L}\\ 
z^{i}+\frac{\lambda }{L}, & z^{i}<-\frac{\lambda }{L}
\end{matrix}\right.$$

## 11.18
$$\begin{aligned}
\underset{\boldsymbol B}{min}\left \|\boldsymbol  X-\boldsymbol B\boldsymbol A \right \|_{F}^{2}
& =\underset{b_{i}}{min}\left \| \boldsymbol X-\sum_{j=1}^{k}b_{j}\alpha ^{j} \right \|_{F}^{2}\\
& =\underset{b_{i}}{min}\left \| \left (\boldsymbol X-\sum_{j\neq i}b_{j}\alpha ^{j} \right )- b_{i}\alpha ^{i}\right \|_{F}^{2} \\
& =\underset{b_{i}}{min}\left \|\boldsymbol  E_{\boldsymbol i}-b_{i}\alpha ^{i} \right \|_{F}^{2} &
\end{aligned}
$$
[推导]：此处只推导一下$BA=\sum_{j=1}^{k}\boldsymbol b_{\boldsymbol j}\boldsymbol \alpha ^{\boldsymbol j}$，其中$\boldsymbol b_{\boldsymbol j}$表示**B**的第j列，$\boldsymbol \alpha ^{\boldsymbol j}$表示**A**的第j行。
然后，用$b_{j}^{i}$，$\alpha _{j}^{i}$分别表示**B**和**A**的第i行第j列的元素，首先计算**BA**:
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
然后计算$\boldsymbol b_{\boldsymbol j}\boldsymbol \alpha ^{\boldsymbol j}$:
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
