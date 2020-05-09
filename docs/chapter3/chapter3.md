## 3.5
$$\cfrac{\partial E_{(w, b)}}{\partial w}=2\left(w \sum_{i=1}^{m} x_{i}^{2}-\sum_{i=1}^{m}\left(y_{i}-b\right) x_{i}\right)$$
[推导]：已知$E_{(w, b)}=\sum\limits_{i=1}^{m}\left(y_{i}-w x_{i}-b\right)^{2}$，所以
$$\begin{aligned}
\cfrac{\partial E_{(w, b)}}{\partial w}&=\cfrac{\partial}{\partial w} \left[\sum_{i=1}^{m}\left(y_{i}-w x_{i}-b\right)^{2}\right] \\
&= \sum_{i=1}^{m}\cfrac{\partial}{\partial w} \left[\left(y_{i}-w x_{i}-b\right)^{2}\right] \\
&= \sum_{i=1}^{m}\left[2\cdot\left(y_{i}-w x_{i}-b\right)\cdot (-x_i)\right] \\
&= \sum_{i=1}^{m}\left[2\cdot\left(w x_{i}^2-y_i x_i +bx_i\right)\right] \\
&= 2\cdot\left(w\sum_{i=1}^{m} x_{i}^2-\sum_{i=1}^{m}y_i x_i +b\sum_{i=1}^{m}x_i\right) \\
&=2\left(w \sum_{i=1}^{m} x_{i}^{2}-\sum_{i=1}^{m}\left(y_{i}-b\right) x_{i}\right)
\end{aligned}$$

## 3.6
$$\cfrac{\partial E_{(w, b)}}{\partial b}=2\left(m b-\sum_{i=1}^{m}\left(y_{i}-w x_{i}\right)\right)$$
[推导]：已知$E_{(w, b)}=\sum\limits_{i=1}^{m}\left(y_{i}-w x_{i}-b\right)^{2}$，所以
$$\begin{aligned}
\cfrac{\partial E_{(w, b)}}{\partial b}&=\cfrac{\partial}{\partial b} \left[\sum_{i=1}^{m}\left(y_{i}-w x_{i}-b\right)^{2}\right] \\
&=\sum_{i=1}^{m}\cfrac{\partial}{\partial b} \left[\left(y_{i}-w x_{i}-b\right)^{2}\right] \\
&=\sum_{i=1}^{m}\left[2\cdot\left(y_{i}-w x_{i}-b\right)\cdot (-1)\right] \\
&=\sum_{i=1}^{m}\left[2\cdot\left(b-y_{i}+w x_{i}\right)\right] \\
&=2\cdot\left[\sum_{i=1}^{m}b-\sum_{i=1}^{m}y_{i}+\sum_{i=1}^{m}w x_{i}\right] \\
&=2\left(m b-\sum_{i=1}^{m}\left(y_{i}-w x_{i}\right)\right)
\end{aligned}$$

## 3.7
$$ w=\cfrac{\sum_{i=1}^{m}y_i(x_i-\bar{x})}{\sum_{i=1}^{m}x_i^2-\cfrac{1}{m}(\sum_{i=1}^{m}x_i)^2} $$
[推导]：令公式(3.5)等于0
$$ 0 = w\sum_{i=1}^{m}x_i^2-\sum_{i=1}^{m}(y_i-b)x_i $$
$$ w\sum_{i=1}^{m}x_i^2 = \sum_{i=1}^{m}y_ix_i-\sum_{i=1}^{m}bx_i $$
由于令公式(3.6)等于0可得$b=\cfrac{1}{m}\sum_{i=1}^{m}(y_i-wx_i)$，又因为$\cfrac{1}{m}\sum_{i=1}^{m}y_i=\bar{y}$，$\cfrac{1}{m}\sum_{i=1}^{m}x_i=\bar{x}$，则$b=\bar{y}-w\bar{x}$，代入上式可得
$$\begin{aligned}	 
w\sum_{i=1}^{m}x_i^2 & = \sum_{i=1}^{m}y_ix_i-\sum_{i=1}^{m}(\bar{y}-w\bar{x})x_i \\
w\sum_{i=1}^{m}x_i^2 & = \sum_{i=1}^{m}y_ix_i-\bar{y}\sum_{i=1}^{m}x_i+w\bar{x}\sum_{i=1}^{m}x_i \\
w(\sum_{i=1}^{m}x_i^2-\bar{x}\sum_{i=1}^{m}x_i) & = \sum_{i=1}^{m}y_ix_i-\bar{y}\sum_{i=1}^{m}x_i \\
w & = \cfrac{\sum_{i=1}^{m}y_ix_i-\bar{y}\sum_{i=1}^{m}x_i}{\sum_{i=1}^{m}x_i^2-\bar{x}\sum_{i=1}^{m}x_i}
\end{aligned}$$
由于$\bar{y}\sum_{i=1}^{m}x_i=\cfrac{1}{m}\sum_{i=1}^{m}y_i\sum_{i=1}^{m}x_i=\bar{x}\sum_{i=1}^{m}y_i$，$\bar{x}\sum_{i=1}^{m}x_i=\cfrac{1}{m}\sum_{i=1}^{m}x_i\sum_{i=1}^{m}x_i=\cfrac{1}{m}(\sum_{i=1}^{m}x_i)^2$，代入上式即可得公式(3.7)
$$ w=\cfrac{\sum_{i=1}^{m}y_i(x_i-\bar{x})}{\sum_{i=1}^{m}x_i^2-\cfrac{1}{m}(\sum_{i=1}^{m}x_i)^2} $$
如果要想用Python来实现上式的话，上式中的求和运算只能用循环来实现，但是如果我们能将上式给向量化，也就是转换成矩阵（向量）运算的话，那么我们就可以利用诸如NumPy这种专门加速矩阵运算的类库来进行编写。下面我们就尝试将上式进行向量化，将$ \cfrac{1}{m}(\sum_{i=1}^{m}x_i)^2=\bar{x}\sum_{i=1}^{m}x_i $代入分母可得
$$\begin{aligned}	  
w & = \cfrac{\sum_{i=1}^{m}y_i(x_i-\bar{x})}{\sum_{i=1}^{m}x_i^2-\bar{x}\sum_{i=1}^{m}x_i} \\
& = \cfrac{\sum_{i=1}^{m}(y_ix_i-y_i\bar{x})}{\sum_{i=1}^{m}(x_i^2-x_i\bar{x})}
\end{aligned}$$
又因为$ \bar{y}\sum_{i=1}^{m}x_i=\bar{x}\sum_{i=1}^{m}y_i=\sum_{i=1}^{m}\bar{y}x_i=\sum_{i=1}^{m}\bar{x}y_i=m\bar{x}\bar{y}=\sum_{i=1}^{m}\bar{x}\bar{y} $，$\sum_{i=1}^{m}x_i\bar{x}=\bar{x}\sum_{i=1}^{m}x_i=\bar{x}\cdot m \cdot\frac{1}{m}\cdot\sum_{i=1}^{m}x_i=m\bar{x}^2=\sum_{i=1}^{m}\bar{x}^2$，则上式可化为
$$\begin{aligned}
w & = \cfrac{\sum_{i=1}^{m}(y_ix_i-y_i\bar{x}-x_i\bar{y}+\bar{x}\bar{y})}{\sum_{i=1}^{m}(x_i^2-x_i\bar{x}-x_i\bar{x}+\bar{x}^2)} \\
& = \cfrac{\sum_{i=1}^{m}(x_i-\bar{x})(y_i-\bar{y})}{\sum_{i=1}^{m}(x_i-\bar{x})^2} 
\end{aligned}$$
若令$\boldsymbol{x}=(x_1,x_2,...,x_m)^T$，$\boldsymbol{x}_{d}=(x_1-\bar{x},x_2-\bar{x},...,x_m-\bar{x})^T$为去均值后的$\boldsymbol{x}$，$\boldsymbol{y}=(y_1,y_2,...,y_m)^T$，$\boldsymbol{y}_{d}=(y_1-\bar{y},y_2-\bar{y},...,y_m-\bar{y})^T$为去均值后的$\boldsymbol{y}$，其中$\boldsymbol{x}$、$\boldsymbol{x}_{d}$、$\boldsymbol{y}$、$\boldsymbol{y}_{d}$均为m行1列的列向量，代入上式可得
$$w=\cfrac{\boldsymbol{x}_{d}^T\boldsymbol{y}_{d}}{\boldsymbol{x}_d^T\boldsymbol{x}_{d}}$$

## 3.10
$$\cfrac{\partial E_{\hat{\boldsymbol w}}}{\partial \hat{\boldsymbol w}}=2\mathbf{X}^{\mathrm{T}}(\mathbf{X}\hat{\boldsymbol w}-\boldsymbol{y})$$
[推导]：将$E_{\hat{\boldsymbol w}}=(\boldsymbol{y}-\mathbf{X}\hat{\boldsymbol w})^{\mathrm{T}}(\boldsymbol{y}-\mathbf{X}\hat{\boldsymbol w})$展开可得
$$E_{\hat{\boldsymbol w}}= \boldsymbol{y}^{\mathrm{T}}\boldsymbol{y}-\boldsymbol{y}^{\mathrm{T}}\mathbf{X}\hat{\boldsymbol w}-\hat{\boldsymbol w}^{\mathrm{T}}\mathbf{X}^{\mathrm{T}}\boldsymbol{y}+\hat{\boldsymbol w}^{\mathrm{T}}\mathbf{X}^{\mathrm{T}}\mathbf{X}\hat{\boldsymbol w}$$
对$\hat{\boldsymbol w}$求导可得
$$\cfrac{\partial E_{\hat{\boldsymbol w}}}{\partial \hat{\boldsymbol w}}= \cfrac{\partial \boldsymbol{y}^{\mathrm{T}}\boldsymbol{y}}{\partial \hat{\boldsymbol w}}-\cfrac{\partial \boldsymbol{y}^{\mathrm{T}}\mathbf{X}\hat{\boldsymbol w}}{\partial \hat{\boldsymbol w}}-\cfrac{\partial \hat{\boldsymbol w}^{\mathrm{T}}\mathbf{X}^{\mathrm{T}}\boldsymbol{y}}{\partial \hat{\boldsymbol w}}+\cfrac{\partial \hat{\boldsymbol w}^{\mathrm{T}}\mathbf{X}^{\mathrm{T}}\mathbf{X}\hat{\boldsymbol w}}{\partial \hat{\boldsymbol w}}$$
由矩阵微分公式$\cfrac{\partial\boldsymbol{a}^{\mathrm{T}}\boldsymbol{x}}{\partial\boldsymbol{x}}=\cfrac{\partial\boldsymbol{x}^{\mathrm{T}}\boldsymbol{a}}{\partial\boldsymbol{x}}=\boldsymbol{a},\cfrac{\partial\boldsymbol{x}^{\mathrm{T}}\mathbf{A}\boldsymbol{x}}{\partial\boldsymbol{x}}=(\mathbf{A}+\mathbf{A}^{\mathrm{T}})\boldsymbol{x}$可得
$$\cfrac{\partial E_{\hat{\boldsymbol w}}}{\partial \hat{\boldsymbol w}}= 0-\mathbf{X}^{\mathrm{T}}\boldsymbol{y}-\mathbf{X}^{\mathrm{T}}\boldsymbol{y}+(\mathbf{X}^{\mathrm{T}}\mathbf{X}+\mathbf{X}^{\mathrm{T}}\mathbf{X})\hat{\boldsymbol w}$$
$$\cfrac{\partial E_{\hat{\boldsymbol w}}}{\partial \hat{\boldsymbol w}}=2\mathbf{X}^{\mathrm{T}}(\mathbf{X}\hat{\boldsymbol w}-\boldsymbol{y})$$

## 3.27
$$ \ell(\boldsymbol{\beta})=\sum_{i=1}^{m}(-y_i\boldsymbol{\beta}^{\mathrm{T}}\hat{\boldsymbol x}_i+\ln(1+e^{\boldsymbol{\beta}^{\mathrm{T}}\hat{\boldsymbol x}_i})) $$
[推导]：将公式(3.26)代入公式(3.25)可得
$$ \ell(\boldsymbol{\beta})=\sum_{i=1}^{m}\ln\left(y_ip_1(\hat{\boldsymbol x}_i;\boldsymbol{\beta})+(1-y_i)p_0(\hat{\boldsymbol x}_i;\boldsymbol{\beta})\right) $$
其中$ p_1(\hat{\boldsymbol x}_i;\boldsymbol{\beta})=\cfrac{e^{\boldsymbol{\beta}^{\mathrm{T}}\hat{\boldsymbol x}_i}}{1+e^{\boldsymbol{\beta}^{\mathrm{T}}\hat{\boldsymbol x}_i}},p_0(\hat{\boldsymbol x}_i;\boldsymbol{\beta})=\cfrac{1}{1+e^{\boldsymbol{\beta}^{\mathrm{T}}\hat{\boldsymbol x}_i}} $，代入上式可得
$$\begin{aligned} 
\ell(\boldsymbol{\beta})&=\sum_{i=1}^{m}\ln\left(\cfrac{y_ie^{\boldsymbol{\beta}^{\mathrm{T}}\hat{\boldsymbol x}_i}+1-y_i}{1+e^{\boldsymbol{\beta}^{\mathrm{T}}\hat{\boldsymbol x}_i}}\right) \\
&=\sum_{i=1}^{m}\left(\ln(y_ie^{\boldsymbol{\beta}^{\mathrm{T}}\hat{\boldsymbol x}_i}+1-y_i)-\ln(1+e^{\boldsymbol{\beta}^{\mathrm{T}}\hat{\boldsymbol x}_i})\right) 
\end{aligned}$$
由于$ y_i $=0或1，则
$$ \ell(\boldsymbol{\beta}) =
\begin{cases} 
\sum_{i=1}^{m}(-\ln(1+e^{\boldsymbol{\beta}^{\mathrm{T}}\hat{\boldsymbol x}_i})),  & y_i=0 \\
\sum_{i=1}^{m}(\boldsymbol{\beta}^{\mathrm{T}}\hat{\boldsymbol x}_i-\ln(1+e^{\boldsymbol{\beta}^{\mathrm{T}}\hat{\boldsymbol x}_i})), & y_i=1
\end{cases} $$
两式综合可得
$$ \ell(\boldsymbol{\beta})=\sum_{i=1}^{m}\left(y_i\boldsymbol{\beta}^{\mathrm{T}}\hat{\boldsymbol x}_i-\ln(1+e^{\boldsymbol{\beta}^{\mathrm{T}}\hat{\boldsymbol x}_i})\right) $$
由于此式仍为极大似然估计的似然函数，所以最大化似然函数等价于最小化似然函数的相反数，也即在似然函数前添加负号即可得公式(3.27)。值得一提的是，若将公式(3.26)这个似然项改写为$p(y_i|\boldsymbol x_i;\boldsymbol w,b)=[p_1(\hat{\boldsymbol x}_i;\boldsymbol{\beta})]^{y_i}[p_0(\hat{\boldsymbol x}_i;\boldsymbol{\beta})]^{1-y_i}$，再将其代入公式(3.25)可得
$$\begin{aligned}
 \ell(\boldsymbol{\beta})&=\sum_{i=1}^{m}\ln\left([p_1(\hat{\boldsymbol x}_i;\boldsymbol{\beta})]^{y_i}[p_0(\hat{\boldsymbol x}_i;\boldsymbol{\beta})]^{1-y_i}\right) \\
&=\sum_{i=1}^{m}\left[y_i\ln\left(p_1(\hat{\boldsymbol x}_i;\boldsymbol{\beta})\right)+(1-y_i)\ln\left(p_0(\hat{\boldsymbol x}_i;\boldsymbol{\beta})\right)\right] \\
&=\sum_{i=1}^{m} \left \{ y_i\left[\ln\left(p_1(\hat{\boldsymbol x}_i;\boldsymbol{\beta})\right)-\ln\left(p_0(\hat{\boldsymbol x}_i;\boldsymbol{\beta})\right)\right]+\ln\left(p_0(\hat{\boldsymbol x}_i;\boldsymbol{\beta})\right)\right\} \\
&=\sum_{i=1}^{m}\left[y_i\ln\left(\cfrac{p_1(\hat{\boldsymbol x}_i;\boldsymbol{\beta})}{p_0(\hat{\boldsymbol x}_i;\boldsymbol{\beta})}\right)+\ln\left(p_0(\hat{\boldsymbol x}_i;\boldsymbol{\beta})\right)\right] \\
&=\sum_{i=1}^{m}\left[y_i\ln\left(e^{\boldsymbol{\beta}^{\mathrm{T}}\hat{\boldsymbol x}_i}\right)+\ln\left(\cfrac{1}{1+e^{\boldsymbol{\beta}^{\mathrm{T}}\hat{\boldsymbol x}_i}}\right)\right] \\
&=\sum_{i=1}^{m}\left(y_i\boldsymbol{\beta}^{\mathrm{T}}\hat{\boldsymbol x}_i-\ln(1+e^{\boldsymbol{\beta}^{\mathrm{T}}\hat{\boldsymbol x}_i})\right) 
\end{aligned}$$
显然，此种方式更易推导出公式(3.27)

## 3.30
$$\frac{\partial \ell(\boldsymbol{\beta})}{\partial \boldsymbol{\beta}}=-\sum_{i=1}^{m}\hat{\boldsymbol x}_i(y_i-p_1(\hat{\boldsymbol x}_i;\boldsymbol{\beta}))$$
[解析]：此式可以进行向量化，令$p_1(\hat{\boldsymbol x}_i;\boldsymbol{\beta})=\hat{y}_i$，代入上式得
$$\begin{aligned}
\frac{\partial \ell(\boldsymbol{\beta})}{\partial \boldsymbol{\beta}} &= -\sum_{i=1}^{m}\hat{\boldsymbol x}_i(y_i-\hat{y}_i) \\
& =\sum_{i=1}^{m}\hat{\boldsymbol x}_i(\hat{y}_i-y_i) \\
& ={\mathbf{X}^{\mathrm{T}}}(\hat{\boldsymbol y}-\boldsymbol{y}) \\
& ={\mathbf{X}^{\mathrm{T}}}(p_1(\mathbf{X};\boldsymbol{\beta})-\boldsymbol{y}) \\
\end{aligned}$$

## 3.32
$$J=\cfrac{\boldsymbol w^{\mathrm{T}}(\boldsymbol{\mu}_{0}-\boldsymbol{\mu}_{1})(\boldsymbol{\mu}_{0}-\boldsymbol{\mu}_{1})^{\mathrm{T}}\boldsymbol w}{\boldsymbol w^{\mathrm{T}}(\boldsymbol{\Sigma}_{0}+\boldsymbol{\Sigma}_{1})\boldsymbol w}$$
[推导]：
$$\begin{aligned}
	J &= \cfrac{\|\boldsymbol w^{\mathrm{T}}\boldsymbol{\mu}_{0}-\boldsymbol w^{\mathrm{T}}\boldsymbol{\mu}_{1}\|_2^2}{\boldsymbol w^{\mathrm{T}}(\boldsymbol{\Sigma}_{0}+\boldsymbol{\Sigma}_{1})\boldsymbol w} \\
	&= \cfrac{\|(\boldsymbol w^{\mathrm{T}}\boldsymbol{\mu}_{0}-\boldsymbol w^{\mathrm{T}}\boldsymbol{\mu}_{1})^{\mathrm{T}}\|_2^2}{\boldsymbol w^{\mathrm{T}}(\boldsymbol{\Sigma}_{0}+\boldsymbol{\Sigma}_{1})\boldsymbol w} \\
	&= \cfrac{\|(\boldsymbol{\mu}_{0}-\boldsymbol{\mu}_{1})^{\mathrm{T}}\boldsymbol w\|_2^2}{\boldsymbol w^{\mathrm{T}}(\boldsymbol{\Sigma}_{0}+\boldsymbol{\Sigma}_{1})\boldsymbol w} \\
	&= \cfrac{\left[(\boldsymbol{\mu}_{0}-\boldsymbol{\mu}_{1})^{\mathrm{T}}\boldsymbol w\right]^{\mathrm{T}}(\boldsymbol{\mu}_{0}-\boldsymbol{\mu}_{1})^{\mathrm{T}}\boldsymbol w}{\boldsymbol w^{\mathrm{T}}(\boldsymbol{\Sigma}_{0}+\boldsymbol{\Sigma}_{1})\boldsymbol w} \\
	&= \cfrac{\boldsymbol w^{\mathrm{T}}(\boldsymbol{\mu}_{0}-\boldsymbol{\mu}_{1})(\boldsymbol{\mu}_{0}-\boldsymbol{\mu}_{1})^{\mathrm{T}}\boldsymbol w}{\boldsymbol w^{\mathrm{T}}(\boldsymbol{\Sigma}_{0}+\boldsymbol{\Sigma}_{1})\boldsymbol w}
\end{aligned}$$

## 3.37
$$\mathbf{S}_b\boldsymbol w=\lambda\mathbf{S}_w\boldsymbol w$$
[推导]：由公式(3.36)可得拉格朗日函数为
$$L(\boldsymbol w,\lambda)=-\boldsymbol w^{\mathrm{T}}\mathbf{S}_b\boldsymbol w+\lambda(\boldsymbol w^{\mathrm{T}}\mathbf{S}_w\boldsymbol w-1)$$
对$\boldsymbol w$求偏导可得
$$\begin{aligned}
\cfrac{\partial L(\boldsymbol w,\lambda)}{\partial \boldsymbol w} &= -\cfrac{\partial(\boldsymbol w^{\mathrm{T}}\mathbf{S}_b\boldsymbol w)}{\partial \boldsymbol w}+\lambda \cfrac{\partial(\boldsymbol w^{\mathrm{T}}\mathbf{S}_w\boldsymbol w-1)}{\partial \boldsymbol w} \\
&= -(\mathbf{S}_b+\mathbf{S}_b^{\mathrm{T}})\boldsymbol w+\lambda(\mathbf{S}_w+\mathbf{S}_w^{\mathrm{T}})\boldsymbol w
\end{aligned}$$
由于$\mathbf{S}_b=\mathbf{S}_b^{\mathrm{T}},\mathbf{S}_w=\mathbf{S}_w^{\mathrm{T}}$，所以
$$\cfrac{\partial L(\boldsymbol w,\lambda)}{\partial \boldsymbol w} = -2\mathbf{S}_b\boldsymbol w+2\lambda\mathbf{S}_w\boldsymbol w$$
令上式等于0即可得
$$-2\mathbf{S}_b\boldsymbol w+2\lambda\mathbf{S}_w\boldsymbol w=0$$
$$\mathbf{S}_b\boldsymbol w=\lambda\mathbf{S}_w\boldsymbol w$$
由于我们想要求解的只有$\boldsymbol{w}$，而$\lambda$这个拉格朗乘子具体取值多少都无所谓，因此我们可以任意设定$\lambda$来配合我们求解$\boldsymbol{w}$。我们注意到
$$\mathbf{S}_b\boldsymbol{w}=(\boldsymbol{\mu}_{0}-\boldsymbol{\mu}_{1})(\boldsymbol{\mu}_{0}-\boldsymbol{\mu}_{1})^{\mathrm{T}}\boldsymbol{w}$$
如果我们令$\lambda$恒等于$(\boldsymbol{\mu}_{0}-\boldsymbol{\mu}_{1})^{\mathrm{T}}\boldsymbol{w}$，那么上式即可改写为
$$\mathbf{S}_b\boldsymbol{w}=\lambda(\boldsymbol{\mu}_{0}-\boldsymbol{\mu}_{1})$$
将其代入$\mathbf{S}_b\boldsymbol w=\lambda\mathbf{S}_w\boldsymbol w$即可解得
$$\boldsymbol{w}=\mathbf{S}_{w}^{-1}(\boldsymbol{\mu}_{0}-\boldsymbol{\mu}_{1})$$

## 3.38
$$\mathbf{S}_b\boldsymbol{w}=\lambda(\boldsymbol{\mu}_{0}-\boldsymbol{\mu}_{1})$$
[推导]：参见公式(3.37)

## 3.39
$$\boldsymbol{w}=\mathbf{S}_{w}^{-1}(\boldsymbol{\mu}_{0}-\boldsymbol{\mu}_{1})$$
[推导]：参见公式(3.37)

## 3.43
$$\begin{aligned}
\mathbf{S}_b &= \mathbf{S}_t - \mathbf{S}_w \\
&= \sum_{i=1}^N m_i(\boldsymbol\mu_i-\boldsymbol\mu)(\boldsymbol\mu_i-\boldsymbol\mu)^{\mathrm{T}}
\end{aligned}$$
[推导]：由公式(3.40)、公式(3.41)、公式(3.42)可得：
$$\begin{aligned}
\mathbf{S}_b &= \mathbf{S}_t - \mathbf{S}_w \\
&= \sum_{i=1}^m(\boldsymbol x_i-\boldsymbol\mu)(\boldsymbol x_i-\boldsymbol\mu)^{\mathrm{T}}-\sum_{i=1}^N\sum_{\boldsymbol x\in X_i}(\boldsymbol x-\boldsymbol\mu_i)(\boldsymbol x-\boldsymbol\mu_i)^{\mathrm{T}} \\
&= \sum_{i=1}^N\left(\sum_{\boldsymbol x\in X_i}\left((\boldsymbol x-\boldsymbol\mu)(\boldsymbol x-\boldsymbol\mu)^{\mathrm{T}}-(\boldsymbol x-\boldsymbol\mu_i)(\boldsymbol x-\boldsymbol\mu_i)^{\mathrm{T}}\right)\right) \\
&= \sum_{i=1}^N\left(\sum_{\boldsymbol x\in X_i}\left((\boldsymbol x-\boldsymbol\mu)(\boldsymbol x^{\mathrm{T}}-\boldsymbol\mu^{\mathrm{T}})-(\boldsymbol x-\boldsymbol\mu_i)(\boldsymbol x^{\mathrm{T}}-\boldsymbol\mu_i^{\mathrm{T}})\right)\right) \\
&= \sum_{i=1}^N\left(\sum_{\boldsymbol x\in X_i}\left(\boldsymbol x\boldsymbol x^{\mathrm{T}} - \boldsymbol x\boldsymbol\mu^{\mathrm{T}}-\boldsymbol\mu\boldsymbol x^{\mathrm{T}}+\boldsymbol\mu\boldsymbol\mu^{\mathrm{T}}-\boldsymbol x\boldsymbol x^{\mathrm{T}}+\boldsymbol x\boldsymbol\mu_i^{\mathrm{T}}+\boldsymbol\mu_i\boldsymbol x^{\mathrm{T}}-\boldsymbol\mu_i\boldsymbol\mu_i^{\mathrm{T}}\right)\right) \\
&= \sum_{i=1}^N\left(\sum_{\boldsymbol x\in X_i}\left(- \boldsymbol x\boldsymbol\mu^{\mathrm{T}}-\boldsymbol\mu\boldsymbol x^{\mathrm{T}}+\boldsymbol\mu\boldsymbol\mu^{\mathrm{T}}+\boldsymbol x\boldsymbol\mu_i^{\mathrm{T}}+\boldsymbol\mu_i\boldsymbol x^{\mathrm{T}}-\boldsymbol\mu_i\boldsymbol\mu_i^{\mathrm{T}}\right)\right) \\
&= \sum_{i=1}^N\left(-\sum_{\boldsymbol x\in X_i}\boldsymbol x\boldsymbol\mu^{\mathrm{T}}-\sum_{\boldsymbol x\in X_i}\boldsymbol\mu\boldsymbol x^{\mathrm{T}}+\sum_{\boldsymbol x\in X_i}\boldsymbol\mu\boldsymbol\mu^{\mathrm{T}}+\sum_{\boldsymbol x\in X_i}\boldsymbol x\boldsymbol\mu_i^{\mathrm{T}}+\sum_{\boldsymbol x\in X_i}\boldsymbol\mu_i\boldsymbol x^{\mathrm{T}}-\sum_{\boldsymbol x\in X_i}\boldsymbol\mu_i\boldsymbol\mu_i^{\mathrm{T}}\right) \\
&= \sum_{i=1}^N\left(-m_i\boldsymbol\mu_i\boldsymbol\mu^{\mathrm{T}}-m_i\boldsymbol\mu\boldsymbol\mu_i^{\mathrm{T}}+m_i\boldsymbol\mu\boldsymbol\mu^{\mathrm{T}}+m_i\boldsymbol\mu_i\boldsymbol\mu_i^{\mathrm{T}}+m_i\boldsymbol\mu_i\boldsymbol\mu_i^{\mathrm{T}}-m_i\boldsymbol\mu_i\boldsymbol\mu_i^{\mathrm{T}}\right) \\
&= \sum_{i=1}^N\left(-m_i\boldsymbol\mu_i\boldsymbol\mu^{\mathrm{T}}-m_i\boldsymbol\mu\boldsymbol\mu_i^{\mathrm{T}}+m_i\boldsymbol\mu\boldsymbol\mu^{\mathrm{T}}+m_i\boldsymbol\mu_i\boldsymbol\mu_i^{\mathrm{T}}\right) \\
&= \sum_{i=1}^Nm_i\left(-\boldsymbol\mu_i\boldsymbol\mu^{\mathrm{T}}-\boldsymbol\mu\boldsymbol\mu_i^{\mathrm{T}}+\boldsymbol\mu\boldsymbol\mu^{\mathrm{T}}+\boldsymbol\mu_i\boldsymbol\mu_i^{\mathrm{T}}\right) \\
&= \sum_{i=1}^N m_i(\boldsymbol\mu_i-\boldsymbol\mu)(\boldsymbol\mu_i-\boldsymbol\mu)^{\mathrm{T}}
\end{aligned}$$

## 3.44
$$\max\limits_{\mathbf{W}}\cfrac{
\operatorname{tr}(\mathbf{W}^{\mathrm{T}}\mathbf{S}_b \mathbf{W})}{\operatorname{tr}(\mathbf{W}^{\mathrm{T}}\mathbf{S}_w \mathbf{W})}$$
[解析]：此式是公式(3.35)的推广形式，证明如下：
设$\mathbf{W}=(\boldsymbol w_1,\boldsymbol w_2,...,\boldsymbol w_i,...,\boldsymbol w_{N-1})\in\mathbb{R}^{d\times(N-1)}$，其中$\boldsymbol w_i\in\mathbb{R}^{d\times 1}$为$d$行1列的列向量，则
$$\left\{
\begin{aligned}
\operatorname{tr}(\mathbf{W}^{\mathrm{T}}\mathbf{S}_b \mathbf{W})&=\sum_{i=1}^{N-1}\boldsymbol w_i^{\mathrm{T}}\mathbf{S}_b \boldsymbol w_i \\
\operatorname{tr}(\mathbf{W}^{\mathrm{T}}\mathbf{S}_w \mathbf{W})&=\sum_{i=1}^{N-1}\boldsymbol w_i^{\mathrm{T}}\mathbf{S}_w \boldsymbol w_i
\end{aligned}
\right.$$
所以公式(3.44)可变形为
$$\max\limits_{\mathbf{W}}\cfrac{
\sum_{i=1}^{N-1}\boldsymbol w_i^{\mathrm{T}}\mathbf{S}_b \boldsymbol w_i}{\sum_{i=1}^{N-1}\boldsymbol w_i^{\mathrm{T}}\mathbf{S}_w \boldsymbol w_i}$$
对比公式(3.35)易知上式即公式(3.35)的推广形式

## 3.45
$$\mathbf{S}_b\mathbf{W}=\lambda\mathbf{S}_w\mathbf{W}$$
[推导]：同公式(3.35)一样，我们在此处也固定公式(3.44)的分母为1，那么公式(3.44)此时等价于如下优化问题
$$\begin{array}{cl}\underset{\boldsymbol{w}}{\min} & -\operatorname{tr}(\mathbf{W}^{\mathrm{T}}\mathbf{S}_b \mathbf{W}) \\ 
\text { s.t. } & \operatorname{tr}(\mathbf{W}^{\mathrm{T}}\mathbf{S}_w \mathbf{W})=1\end{array}$$
根据拉格朗日乘子法可知，上述优化问题的拉格朗日函数为
$$L(\mathbf{W},\lambda)=-\operatorname{tr}(\mathbf{W}^{\mathrm{T}}\mathbf{S}_b \mathbf{W})+\lambda(\operatorname{tr}(\mathbf{W}^{\mathrm{T}}\mathbf{S}_w \mathbf{W})-1)$$
根据矩阵微分公式$\cfrac{\partial}{\partial \mathbf{X}} \text { tr }(\mathbf{X}^{\mathrm{T}}  \mathbf{B} \mathbf{X})=(\mathbf{B}+\mathbf{B}^{\mathrm{T}})\mathbf{X}$对上式关于$\mathbf{W}$求偏导可得
$$\begin{aligned}
\cfrac{\partial L(\mathbf{W},\lambda)}{\partial \mathbf{W}} &= -\cfrac{\partial\left(\operatorname{tr}(\mathbf{W}^{\mathrm{T}}\mathbf{S}_b \mathbf{W})\right)}{\partial \mathbf{W}}+\lambda \cfrac{\partial\left(\operatorname{tr}(\mathbf{W}^{\mathrm{T}}\mathbf{S}_w \mathbf{W})-1\right)}{\partial \mathbf{W}} \\
&= -(\mathbf{S}_b+\mathbf{S}_b^{\mathrm{T}})\mathbf{W}+\lambda(\mathbf{S}_w+\mathbf{S}_w^{\mathrm{T}})\mathbf{W}
\end{aligned}$$
由于$\mathbf{S}_b=\mathbf{S}_b^{\mathrm{T}},\mathbf{S}_w=\mathbf{S}_w^{\mathrm{T}}$，所以
$$\cfrac{\partial L(\mathbf{W},\lambda)}{\partial \mathbf{W}} = -2\mathbf{S}_b\mathbf{W}+2\lambda\mathbf{S}_w\mathbf{W}$$
令上式等于$\mathbf{0}$即可得
$$-2\mathbf{S}_b\mathbf{W}+2\lambda\mathbf{S}_w\mathbf{W}=\mathbf{0}$$
$$\mathbf{S}_b\mathbf{W}=\lambda\mathbf{S}_w\mathbf{W}$$