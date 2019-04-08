## 9.33

$$
\sum_{j=1}^m \frac{\alpha_{i}\cdot p\left(\boldsymbol{x_{j}}|\boldsymbol\mu _{i},\boldsymbol\Sigma_{i}\right)}{\sum_{l=1}^k \alpha_{l}\cdot p(\boldsymbol{x_{j}}|\boldsymbol\mu_{l},\boldsymbol\Sigma_{l})}(\boldsymbol{x_{j}-\mu_{i}})=0
$$

[推导]：根据公式(9.28)可知：
$$
p(\boldsymbol{x_{j}|\boldsymbol\mu_{i},\boldsymbol\Sigma_{i}})=\frac{1}{(2\pi)^\frac{n}{2}\left| \boldsymbol\Sigma_{i}\right |^\frac{1}{2}}e^{-\frac{1}{2}(\boldsymbol{x_{j}}-\boldsymbol\mu_{i})^T\boldsymbol\Sigma_{i}^{-1}(\boldsymbol{x_{j}-\mu_{i}})}
$$


又根据公式(9.32)，由
$$
\frac {\partial LL(D)}{\partial \boldsymbol\mu_{i}}=0
$$
可得
$$\begin{aligned}
\frac {\partial LL(D)}{\partial\boldsymbol\mu_{i}}&=\frac {\partial}{\partial \boldsymbol\mu_{i}}\sum_{j=1}^mln\Bigg(\sum_{i=1}^k \alpha_{i}\cdot p(\boldsymbol{x_{j}}|\boldsymbol\mu_{i},\boldsymbol\Sigma_{i})\Bigg) \\
&=\sum_{j=1}^m\frac{\partial}{\partial\boldsymbol\mu_{i}}ln\Bigg(\sum_{i=1}^k \alpha_{i}\cdot p(\boldsymbol{x_{j}}|\boldsymbol\mu_{i},\boldsymbol\Sigma_{i})\Bigg) \\
&=\sum_{j=1}^m\frac{\alpha_{i}\cdot \frac{\partial}{\partial\boldsymbol{\mu_{i}}}(p(\boldsymbol x_{j}|\boldsymbol{\mu_{i},\Sigma_{i}}))}{\sum_{l=1}^k\alpha_{l}\cdot p(\boldsymbol x_{j}|\boldsymbol{\mu_{l},\Sigma_{l}})} \\
&=\sum_{j=1}^m\frac{\alpha_{i}\cdot \frac{1}{(2\pi)^\frac{n}{2}\left| \boldsymbol\Sigma_{i}\right |^\frac{1}{2}}e^{-\frac{1}{2}(\boldsymbol{x_{j}}-\boldsymbol\mu_{i})^T\boldsymbol\Sigma_{i}^{-1}(\boldsymbol{x_{j}-\mu_{i}})}}{\sum_{l=1}^k\alpha_{l}\cdot p(\boldsymbol x_{j}|\boldsymbol{\mu_{l},\Sigma_{l}})}\frac{\partial}{\partial \boldsymbol\mu_{i}}\left(-\frac{1}{2}\left(\boldsymbol{x_{j}-\mu_{i}}\right)^T\boldsymbol\Sigma_{i}^{-1}\left(\boldsymbol{x_{j}-\mu_{i}}\right)\right) \\
&=\sum_{j=1}^m\frac{\alpha_{i}\cdot p(\boldsymbol x_{j}|\boldsymbol{\mu_{i},\Sigma_{i}})}{\sum_{l=1}^k\alpha_{l}\cdot p(\boldsymbol x_{j}|\boldsymbol{\mu_{l},\Sigma_{l}})}(-\frac{1}{2})\left(\left(\boldsymbol\Sigma_{i}^{-1}+\left(\boldsymbol\Sigma_{i}^{-1}\right)^T\right)\cdot\left(\boldsymbol{x_{j}-\mu_{i}}\right)\cdot(-1)\right) \\
&=\sum_{j=1}^m\frac{\alpha_{i}\cdot p(\boldsymbol x_{j}|\boldsymbol{\mu_{i},\Sigma_{i}})}{\sum_{l=1}^k\alpha_{l}\cdot p(\boldsymbol x_{j}|\boldsymbol{\mu_{l},\Sigma_{l}})}(-\frac{1}{2})\left(-\boldsymbol\Sigma_{i}^{-1}\left(\boldsymbol{x_{j}-\mu_{i}}\right)-\left(\boldsymbol\Sigma_{i}^{-1}\right)^T\left(\boldsymbol{x_{j}-\mu_{i}}\right)\right)=0 \\
&=\sum_{j=1}^m\frac{\alpha_{i}\cdot p(\boldsymbol x_{j}|\boldsymbol{\mu_{i},\Sigma_{i}})}{\sum_{l=1}^k\alpha_{l}\cdot p(\boldsymbol x_{j}|\boldsymbol{\mu_{l},\Sigma_{l}})}(-\frac{1}{2})\left(-\boldsymbol\Sigma_{i}^{-1}\left(\boldsymbol{x_{j}-\mu_{i}}\right)-\left(\boldsymbol\Sigma_{i}^T\right)^{-1}\left(\boldsymbol{x_{j}-\mu_{i}}\right)\right) \\
&=\sum_{j=1}^m\frac{\alpha_{i}\cdot p(\boldsymbol x_{j}|\boldsymbol{\mu_{i},\Sigma_{i}})}{\sum_{l=1}^k\alpha_{l}\cdot p(\boldsymbol x_{j}|\boldsymbol{\mu_{l},\Sigma_{l}})}(-\frac{1}{2})\left(-\boldsymbol\Sigma_{i}^{-1}\left(\boldsymbol{x_{j}-\mu_{i}}\right)-\boldsymbol\Sigma_{i}^{-1}\left(\boldsymbol{x_{j}-\mu_{i}}\right)\right) \\
&=\sum_{j=1}^m\frac{\alpha_{i}\cdot p(\boldsymbol x_{j}|\boldsymbol{\mu_{i},\Sigma_{i}})}{\sum_{l=1}^k\alpha_{l}\cdot p(\boldsymbol x_{j}|\boldsymbol{\mu_{l},\Sigma_{l}})}(-\frac{1}{2})\left(-2\cdot\boldsymbol\Sigma_{i}^{-1}\left(\boldsymbol{x_{j}-\mu_{i}}\right)\right) \\
&=\sum_{j=1}^m \frac{\alpha_{i}\cdot p\left(\boldsymbol{x_{j}}|\boldsymbol\mu _{i},\boldsymbol\Sigma_{i}\right)}{\sum_{l=1}^k \alpha_{l}\cdot p(\boldsymbol{x_{j}}|\boldsymbol\mu_{l},\boldsymbol\Sigma_{l})}\boldsymbol\Sigma_{i}^{-1}(\boldsymbol{x_{j}-\mu_{i}}) \\
&=\sum_{j=1}^m \frac{\alpha_{i}\cdot p\left(\boldsymbol{x_{j}}|\boldsymbol\mu _{i},\boldsymbol\Sigma_{i}\right)}{\sum_{l=1}^k \alpha_{l}\cdot p(\boldsymbol{x_{j}}|\boldsymbol\mu_{l},\boldsymbol\Sigma_{l})}(\boldsymbol{x_{j}-\mu_{i}})=0
\end{aligned}$$

## 9.35

$$
\boldsymbol\Sigma_{i}=\frac{\sum_{j=1}^m\gamma_{ji}(\boldsymbol{x_{j}-\mu_{i}})(\boldsymbol{x_{j}-\mu_{i}})^T}{\sum_{j=1}^m}\gamma_{ji}
$$

[推导]：根据公式(9.28)可知：
$$
p(\boldsymbol{x_{j}|\boldsymbol\mu_{i},\boldsymbol\Sigma_{i}})=\frac{1}{(2\pi)^\frac{n}{2}\left| \boldsymbol\Sigma_{i}\right |^\frac{1}{2}}e^{-\frac{1}{2}(\boldsymbol{x_{j}}-\boldsymbol\mu_{i})^T\boldsymbol\Sigma_{i}^{-1}(\boldsymbol{x_{j}-\mu_{i}})}
$$
又根据公式(9.32)，由
$$
\frac {\partial LL(D)}{\partial \boldsymbol\Sigma_{i}}=0
$$
可得
$$\begin{aligned}
\frac {\partial LL(D)}{\partial\boldsymbol\Sigma_{i}}&=\frac {\partial}{\partial \boldsymbol\Sigma_{i}}\sum_{j=1}^mln\Bigg(\sum_{i=1}^k \alpha_{i}\cdot p(\boldsymbol{x_{j}}|\boldsymbol\mu_{i},\boldsymbol\Sigma_{i})\Bigg) \\
&=\sum_{j=1}^m\frac{\partial}{\partial\boldsymbol\Sigma_{i}}ln\Bigg(\sum_{i=1}^k \alpha_{i}\cdot p(\boldsymbol{x_{j}}|\boldsymbol\mu_{i},\boldsymbol\Sigma_{i})\Bigg) \\
&=\sum_{j=1}^m \frac{\alpha_{i}\cdot \frac{\partial}{\partial\boldsymbol\Sigma_{i}}p(\boldsymbol x_{j}|\boldsymbol \mu_{i},\boldsymbol\Sigma_{i})}{\sum_{l=1}^k\alpha_{l}\cdot p(\boldsymbol x_{j},|\boldsymbol \mu_{l},\boldsymbol\Sigma_{l})} \\
&=\sum_{j=1}^m \frac{\alpha_{i}\cdot \frac{\partial}{\partial\boldsymbol\Sigma_{i}}\frac{1}{(2\pi)^\frac{n}{2}\left| \boldsymbol\Sigma_{i}\right |^\frac{1}{2}}e^{-\frac{1}{2}(\boldsymbol{x_{j}}-\boldsymbol\mu_{i})^T\boldsymbol\Sigma_{i}^{-1}(\boldsymbol{x_{j}-\mu_{i}})}}{\sum_{l=1}^k\alpha_{l}\cdot p(\boldsymbol x_{j},|\boldsymbol \mu_{l},\boldsymbol\Sigma_{l})}\\
&=\sum_{j=1}^m \frac{\alpha_{i}\cdot \frac{\partial}{\partial\boldsymbol\Sigma_{i}}e^{ln\left(\frac{1}{(2\pi)^\frac{n}{2}\left| \boldsymbol\Sigma_{i}\right |^\frac{1}{2}}e^{-\frac{1}{2}(\boldsymbol{x_{j}}-\boldsymbol\mu_{i})^T\boldsymbol\Sigma_{i}^{-1}(\boldsymbol{x_{j}-\mu_{i}})}\right)}}{\sum_{l=1}^k\alpha_{l}\cdot p(\boldsymbol x_{j},|\boldsymbol \mu_{l},\boldsymbol\Sigma_{l}} \\
&=\sum_{j=1}^m \frac{\alpha_{i}\cdot \frac{\partial}{\partial\boldsymbol\Sigma_{i}}e^{-\frac{n}{2}ln\left(2\pi\right)-\frac{1}{2}ln\left(|\boldsymbol\Sigma_{i}|\right)-\frac{1}{2}\left(\boldsymbol{x_{j}-\mu_{i}}\right)^T\boldsymbol\Sigma_{i}^{-1}\left(\boldsymbol{x_{j}-\mu_{i}}\right)}}{\sum_{l=1}^k\alpha_{l}\cdot p(\boldsymbol x_{j},|\boldsymbol \mu_{l},\boldsymbol\Sigma_{l}} \\
&=\sum_{j=1}^m \frac{\alpha_{i}\cdot p(\boldsymbol x_{j}|\boldsymbol \mu_{i},\boldsymbol\Sigma_{i})}{\sum_{l=1}^k\alpha_{l}\cdot p(\boldsymbol x_{j},|\boldsymbol \mu_{l},\boldsymbol\Sigma_{l})}\frac{\partial}{\partial\boldsymbol\Sigma_{i}}\left(-\frac{n}{2}ln\left(2\pi\right)-\frac{1}{2}ln\left(|\boldsymbol\Sigma_{i}|\right)-\frac{1}{2}\left(\boldsymbol{x_{j}-\mu_{i}}\right)^T\boldsymbol\Sigma_{i}^{-1}\left(\boldsymbol{x_{j}-\mu_{i}}\right)\right) \\
&=\sum_{j=1}^m \frac{\alpha_{i}\cdot p(\boldsymbol x_{j}|\boldsymbol \mu_{i},\boldsymbol\Sigma_{i})}{\sum_{l=1}^k\alpha_{l}\cdot p(\boldsymbol x_{j},|\boldsymbol \mu_{l},\boldsymbol\Sigma_{l})}\left(-\frac{1}{2}\left(\boldsymbol\Sigma_{i}^{-1}\right)^T-\frac{1}{2}\frac{\partial}{\partial\boldsymbol\Sigma_{i}}\left(\boldsymbol{x_{j}-\mu_{i}}\right)^T\boldsymbol\Sigma_{i}^{-1}\left(\boldsymbol{x_{j}-\mu_{i}}\right)\right)
\end{aligned}$$

为求得
$$
\frac{\partial}{\partial\boldsymbol\Sigma_{i}}\left(\boldsymbol{x_{j}-\mu_{i}}\right)^T\boldsymbol\Sigma_{i}^{-1}\left(\boldsymbol{x_{j}-\mu_{i}}\right)
$$

首先分析对$\boldsymbol \Sigma_{i}$中单一元素的求导，用$r$代表矩阵$\boldsymbol\Sigma_{i}$的行索引，$c$代表矩阵$\boldsymbol\Sigma_{i}$的列索引，则
$$\begin{aligned}
\frac{\partial}{\partial\Sigma_{i_{rc}}}\left(\boldsymbol{x_{j}-\mu_{i}}\right)^T\boldsymbol\Sigma_{i}^{-1}\left(\boldsymbol{x_{j}-\mu_{i}}\right)&=\left(\boldsymbol{x_{j}-\mu_{i}}\right)^T\frac{\partial\boldsymbol\Sigma_{i}^{-1}}{\partial\Sigma_{i_{rc}}}\left(\boldsymbol{x_{j}-\mu_{i}}\right) \\
&=-\left(\boldsymbol{x_{j}-\mu_{i}}\right)^T\boldsymbol\Sigma_{i}^{-1}\frac{\partial\boldsymbol\Sigma_{i}}{\partial\Sigma_{i_{rc}}}\boldsymbol\Sigma_{i}^{-1}\left(\boldsymbol{x_{j}-\mu_{i}}\right)
\end{aligned}$$
设$B=\boldsymbol\Sigma_{i}^{-1}\left(\boldsymbol{x_{j}-\mu_{i}}\right)$，则
$$\begin{aligned}
B^T&=\left(\boldsymbol\Sigma_{i}^{-1}\left(\boldsymbol{x_{j}-\mu_{i}}\right)\right)^T \\
&=\left(\boldsymbol{x_{j}-\mu_{i}}\right)^T\left(\boldsymbol\Sigma_{i}^{-1}\right)^T \\
&=\left(\boldsymbol{x_{j}-\mu_{i}}\right)^T\boldsymbol\Sigma_{i}^{-1}
\end{aligned}$$
所以
$$\begin{aligned}
\frac{\partial}{\partial\Sigma_{i_{rc}}}\left(\boldsymbol{x_{j}-\mu_{i}}\right)^T\boldsymbol\Sigma_{i}^{-1}\left(\boldsymbol{x_{j}-\mu_{i}}\right)=-B^T\frac{\partial\boldsymbol\Sigma_{i}}{\partial\Sigma_{i_{rc}}}B\end{aligned}$$
其中$B$为$n\times1$阶矩阵，$\frac{\partial\boldsymbol\Sigma_{i}}{\partial\Sigma_{i_{rc}}}$为$n$阶方阵，且$\frac{\partial\boldsymbol\Sigma_{i}}{\partial\Sigma_{i_{rc}}}$仅在$\left(r,c\right)$位置处的元素值为1，其它位置处的元素值均为$0$，所以
$$\begin{aligned}
\frac{\partial}{\partial\Sigma_{i_{rc}}}\left(\boldsymbol{x_{j}-\mu_{i}}\right)^T\boldsymbol\Sigma_{i}^{-1}\left(\boldsymbol{x_{j}-\mu_{i}}\right)=-B^T\frac{\partial\boldsymbol\Sigma_{i}}{\partial\Sigma_{i_{rc}}}B=-B_{r}\cdot B_{c}=-\left(B\cdot B^T\right)_{rc}=\left(-B\cdot B^T\right)_{rc}\end{aligned}$$
即对$\boldsymbol\Sigma_{i}$中特定位置的元素的求导结果对应于$\left(-B\cdot B^T\right)$中相同位置的元素值，所以
$$\begin{aligned}
\frac{\partial}{\partial\boldsymbol\Sigma_{i}}\left(\boldsymbol{x_{j}-\mu_{i}}\right)^T\boldsymbol\Sigma_{i}^{-1}\left(\boldsymbol{x_{j}-\mu_{i}}\right)&=-B\cdot B^T\\
&=-\boldsymbol\Sigma_{i}^{-1}\left(\boldsymbol{x_{j}-\mu_{i}}\right)\left(\boldsymbol\Sigma_{i}^{-1}\left(\boldsymbol{x_{j}-\mu_{i}}\right)\right)^T\\
&=-\boldsymbol\Sigma_{i}^{-1}\left(\boldsymbol{x_{j}-\mu_{i}}\right)\left(\boldsymbol{x_{j}-\mu_{i}}\right)^T\boldsymbol\Sigma_{i}^{-1}
\end{aligned}$$

因此最终结果为
$$
\frac {\partial LL(D)}{\partial \boldsymbol\Sigma_{i}}=\sum_{j=1}^m \frac{\alpha_{i}\cdot p(\boldsymbol x_{j}|\boldsymbol \mu_{i},\boldsymbol\Sigma_{i})}{\sum_{l=1}^k\alpha_{l}\cdot p(\boldsymbol x_{j},|\boldsymbol \mu_{l},\boldsymbol\Sigma_{l})}\left( -\frac{1}{2}\left(\boldsymbol\Sigma_{i}^{-1}-\boldsymbol\Sigma_{i}^{-1}\left(\boldsymbol{x_{j}-\mu_{i}}\right)\left(\boldsymbol{x_{j}-\mu_{i}}\right)^T\boldsymbol\Sigma_{i}^{-1}\right)\right)=0
$$

整理可得
$$
\boldsymbol\Sigma_{i}=\frac{\sum_{j=1}^m\gamma_{ji}(\boldsymbol{x_{j}-\mu_{i}})(\boldsymbol{x_{j}-\mu_{i}})^T}{\sum_{j=1}^m}\gamma_{ji}
$$

## 9.38

$$
\alpha_{i}=\frac{1}{m}\sum_{j=1}^m\gamma_{ji}
$$

[推导]：基于公式(9.37)进行恒等变形：
$$
\sum_{j=1}^m\frac{p(\boldsymbol x_{j}|\boldsymbol{\mu_{i},\Sigma_{i}})}{\sum_{l=1}^k\alpha_{l}\cdot p(\boldsymbol x_{j}|\boldsymbol{\mu_{l},\Sigma_{l}})}+\lambda=0
$$

$$
\Rightarrow\sum_{j=1}^m\frac{\alpha_{i}\cdot p(\boldsymbol x_{j}|\boldsymbol{\mu_{i},\Sigma_{i}})}{\sum_{l=1}^k\alpha_{l}\cdot p(\boldsymbol x_{j}|\boldsymbol{\mu_{l},\Sigma_{l}})}+\alpha_{i}\lambda=0
$$

对所有混合成分进行求和：
$$
\Rightarrow\sum_{i=1}^k\left(\sum_{j=1}^m\frac{\alpha_{i}\cdot p(\boldsymbol x_{j}|\boldsymbol{\mu_{i},\Sigma_{i}})}{\sum_{l=1}^k\alpha_{l}\cdot p(\boldsymbol x_{j}|\boldsymbol{\mu_{l},\Sigma_{l}})}+\alpha_{i}\lambda\right)=0
$$

$$
\Rightarrow\sum_{i=1}^k\sum_{j=1}^m\frac{\alpha_{i}\cdot p(\boldsymbol x_{j}|\boldsymbol{\mu_{i},\Sigma_{i}})}{\sum_{l=1}^k\alpha_{l}\cdot p(\boldsymbol x_{j}|\boldsymbol{\mu_{l},\Sigma_{l}})}+\sum_{i=1}^k\alpha_{i}\lambda=0
$$

$$
\Rightarrow\lambda=-\sum_{i=1}^k\sum_{j=1}^m\frac{\alpha_{i}\cdot p(\boldsymbol x_{j}|\boldsymbol{\mu_{i},\Sigma_{i}})}{\sum_{l=1}^k\alpha_{l}\cdot p(\boldsymbol x_{j}|\boldsymbol{\mu_{l},\Sigma_{l}})}=-m
$$

又
$$
\sum_{j=1}^m\frac{\alpha_{i}\cdot p(\boldsymbol x_{j}|\boldsymbol{\mu_{i},\Sigma_{i}})}{\sum_{l=1}^k\alpha_{l}\cdot p(\boldsymbol x_{j}|\boldsymbol{\mu_{l},\Sigma_{l}})}+\alpha_{i}\lambda=0
$$

$$
\Rightarrow\sum_{j=1}^m\gamma_{ji}+\alpha_{i}\lambda=0
$$

$$
\Rightarrow\alpha_{i}=-\frac{\sum_{j=1}^m\gamma_{ji}}{\lambda}=\frac{1}{m}\sum_{j=1}^m\gamma_{ji}
$$



## 附录
参考公式
$$
\frac{\partial\boldsymbol x^TB\boldsymbol x}{\partial\boldsymbol x}=\left(B+B^T\right)\boldsymbol x 
$$
$$
\frac{\partial}{\partial A}ln|A|=\left(A^{-1}\right)^T
$$
$$
\frac{\partial}{\partial x}\left(A^{-1}\right)=-A^{-1}\frac{\partial A}{\partial x}A^{-1}
$$
参考资料<br>
Petersen, K. B., & Pedersen, M. S. *The Matrix Cookbook*.<br>
Bishop, C. M. (2006). *Pattern recognition and machine learning*. springer.


