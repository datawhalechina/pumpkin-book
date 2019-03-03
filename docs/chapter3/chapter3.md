## 3.7

$$ w=\cfrac{\sum_{i=1}^{m}y_i(x_i-\bar{x})}{\sum_{i=1}^{m}x_i^2-\cfrac{1}{m}(\sum_{i=1}^{m}x_i)^2} $$

[推导]：令式（3.5）等于0：
$$ 0 = w\sum_{i=1}^{m}x_i^2-\sum_{i=1}^{m}(y_i-b)x_i $$
$$ w\sum_{i=1}^{m}x_i^2 = \sum_{i=1}^{m}y_ix_i-\sum_{i=1}^{m}bx_i $$
由于令式（3.6）等于0可得$ b=\cfrac{1}{m}\sum_{i=1}^{m}(y_i-wx_i) $，又$ \cfrac{1}{m}\sum_{i=1}^{m}y_i=\bar{y} $，$ \cfrac{1}{m}\sum_{i=1}^{m}x_i=\bar{x} $，则$ b=\bar{y}-w\bar{x} $，代入上式可得：
$$ 
\begin{aligned}	 
    w\sum_{i=1}^{m}x_i^2 & = \sum_{i=1}^{m}y_ix_i-\sum_{i=1}^{m}(\bar{y}-w\bar{x})x_i \\
    w\sum_{i=1}^{m}x_i^2 & = \sum_{i=1}^{m}y_ix_i-\bar{y}\sum_{i=1}^{m}x_i+w\bar{x}\sum_{i=1}^{m}x_i \\
    w(\sum_{i=1}^{m}x_i^2-\bar{x}\sum_{i=1}^{m}x_i) & = \sum_{i=1}^{m}y_ix_i-\bar{y}\sum_{i=1}^{m}x_i \\
    w & = \cfrac{\sum_{i=1}^{m}y_ix_i-\bar{y}\sum_{i=1}^{m}x_i}{\sum_{i=1}^{m}x_i^2-\bar{x}\sum_{i=1}^{m}x_i}
\end{aligned}
$$
又$ \bar{y}\sum_{i=1}^{m}x_i=\cfrac{1}{m}\sum_{i=1}^{m}y_i\sum_{i=1}^{m}x_i=\bar{x}\sum_{i=1}^{m}y_i $，$ \bar{x}\sum_{i=1}^{m}x_i=\cfrac{1}{m}\sum_{i=1}^{m}x_i\sum_{i=1}^{m}x_i=\cfrac{1}{m}(\sum_{i=1}^{m}x_i)^2 $，代入上式即可得式（3.7）：
$$ w=\cfrac{\sum_{i=1}^{m}y_i(x_i-\bar{x})}{\sum_{i=1}^{m}x_i^2-\cfrac{1}{m}(\sum_{i=1}^{m}x_i)^2} $$

【注】：式（3.7）还可以进一步化简为能用向量表达的形式，将$ \cfrac{1}{m}(\sum_{i=1}^{m}x_i)^2=\bar{x}\sum_{i=1}^{m}x_i $代入分母可得：
$$ 
\begin{aligned}	  
     w & = \cfrac{\sum_{i=1}^{m}y_i(x_i-\bar{x})}{\sum_{i=1}^{m}x_i^2-\bar{x}\sum_{i=1}^{m}x_i} \\
     & = \cfrac{\sum_{i=1}^{m}(y_ix_i-y_i\bar{x})}{\sum_{i=1}^{m}(x_i^2-x_i\bar{x})}
\end{aligned}
$$
又$ \bar{y}\sum_{i=1}^{m}x_i=\bar{x}\sum_{i=1}^{m}y_i=\sum_{i=1}^{m}\bar{y}x_i=\sum_{i=1}^{m}\bar{x}y_i=m\bar{x}\bar{y}=\sum_{i=1}^{m}\bar{x}\bar{y} $，则上式可化为：
$$ 
\begin{aligned}
    w & = \cfrac{\sum_{i=1}^{m}(y_ix_i-y_i\bar{x}-x_i\bar{y}+\bar{x}\bar{y})}{\sum_{i=1}^{m}(x_i^2-x_i\bar{x}-x_i\bar{x}+\bar{x}^2)} \\
    & = \cfrac{\sum_{i=1}^{m}(x_i-\bar{x})(y_i-\bar{y})}{\sum_{i=1}^{m}(x_i-\bar{x})^2} 
\end{aligned}
$$
若令$ \mathbf{X}=(x_1,x_2,...,x_m) $，$\mathbf{X}_{demean}$为去均值后的$ \mathbf{X} $，$ \mathbf{y}=(y_1,y_2,...,y_m) $，$ \mathbf{y}_{demean} $为去均值后的$ \mathbf{y} $，其中$ \mathbf{X} $、$ \mathbf{X}_{demean} $、$ \mathbf{y} $、$ \mathbf{y}_{demean} $均为m行1列的列向量，代入上式可得：
$$ w=\cfrac{\mathbf{X}_{demean}\mathbf{y}_{demean}^T}{\mathbf{X}_{demean}\mathbf{X}_{demean}^T}$$
## 3.10

$$ \cfrac{\partial E_{\hat{w}}}{\partial \hat{w}}=2\mathbf{X}^T(\mathbf{X}\hat{w}-\mathbf{y}) $$

[推导]：将$ E_{\hat{w}}=(\mathbf{y}-\mathbf{X}\hat{w})^T(\mathbf{y}-\mathbf{X}\hat{w}) $展开可得：
$$ E_{\hat{w}}= \mathbf{y}^T\mathbf{y}-\mathbf{y}^T\mathbf{X}\hat{w}-\hat{w}^T\mathbf{X}^T\mathbf{y}+\hat{w}^T\mathbf{X}^T\mathbf{X}\hat{w} $$
对$ \hat{w} $求导可得：
$$ \cfrac{\partial E_{\hat{w}}}{\partial \hat{w}}= \cfrac{\partial \mathbf{y}^T\mathbf{y}}{\partial \hat{w}}-\cfrac{\partial \mathbf{y}^T\mathbf{X}\hat{w}}{\partial \hat{w}}-\cfrac{\partial \hat{w}^T\mathbf{X}^T\mathbf{y}}{\partial \hat{w}}+\cfrac{\partial \hat{w}^T\mathbf{X}^T\mathbf{X}\hat{w}}{\partial \hat{w}} $$
由向量的求导公式可得：
$$ \cfrac{\partial E_{\hat{w}}}{\partial \hat{w}}= 0-\mathbf{X}^T\mathbf{y}-\mathbf{X}^T\mathbf{y}+(\mathbf{X}^T\mathbf{X}+\mathbf{X}^T\mathbf{X})\hat{w} $$
$$ \cfrac{\partial E_{\hat{w}}}{\partial \hat{w}}=2\mathbf{X}^T(\mathbf{X}\hat{w}-\mathbf{y}) $$

## 3.27

$$ l(β)=\sum_{i=1}^{m}(-y_iβ^T\hat{\boldsymbol x_i}+\ln(1+e^{β^T\hat{\boldsymbol x_i}})) $$

[推导]：将式（3.26）代入式（3.25）可得：
$$ l(β,b)=\sum_{i=1}^{m}\ln(y_ip_1(\boldsymbol{\hat{x_i}};β)+(1-y_i)p_0(\boldsymbol{\hat{x_i}};β)) $$
其中$ p_1(\boldsymbol{\hat{x_i}};β)=\cfrac{e^{β^T\hat{\boldsymbol x_i}}}{1+e^{β^T\hat{\boldsymbol x_i}}},p_0(\boldsymbol{\hat{x_i}};β)=\cfrac{1}{1+e^{β^T\hat{\boldsymbol x_i}}} $，代入上式可得：
$$ l(β,b)=\sum_{i=1}^{m}\ln(\cfrac{y_ie^{β^T\hat{\boldsymbol x_i}}+1-y_i}{1+e^{β^T\hat{\boldsymbol x_i}}}) $$
$$ l(β,b)=\sum_{i=1}^{m}(\ln(y_ie^{β^T\hat{\boldsymbol x_i}}+1-y_i)-\ln(1+e^{β^T\hat{\boldsymbol x_i}})) $$
又$ y_i $=0或1，则：
$$ l(β,b) =
\begin{cases} 
\sum_{i=1}^{m}(-\ln(1+e^{β^T\hat{\boldsymbol x_i}})),  & y_i=0 \\
\sum_{i=1}^{m}(β^T\hat{\boldsymbol x_i}-\ln(1+e^{β^T\hat{\boldsymbol x_i}})), & y_i=1
\end{cases} $$
两式综合可得：
$$ l(β)=\sum_{i=1}^{m}(y_iβ^T\hat{\boldsymbol x_i}-\ln(1+e^{β^T\hat{\boldsymbol x_i}})) $$
由于此式仍为极大似然估计的似然函数，所以最大化似然函数等价于最小化似然函数的相反数，也即在似然函数前添加负号即可得式（3.27）。

【注】：若式（3.26）中的似然项改写方式为$ p(y_i|\boldsymbol x_i;\boldsymbol w,b)=[p_1(\boldsymbol{\hat{x_i}};β)]^{y_i}[p_0(\boldsymbol{\hat{x_i}};β)]^{1-y_i} $，再将其代入式（3.25）可得：
$$ l(β)=\sum_{i=1}^{m}(y_i\ln(p_1(\boldsymbol{\hat{x_i}};β))+(1-y_i)\ln(p_0(\boldsymbol{\hat{x_i}};β))) $$
此式显然更易推导出式（3.27）

## 3.30

$$\frac{\partial l(β)}{\partial β}=-\sum_{i=1}^{m}\hat{\boldsymbol x_i}(y_i-p_1(\hat{\boldsymbol x_i};β))$$

[解析]：此式可以进行向量化，令$p_1(\hat{\boldsymbol x_i};β)=\hat{y_i}$，代入上式得：
$$\begin{aligned}
	\frac{\partial l(β)}{\partial β} &= -\sum_{i=1}^{m}\hat{\boldsymbol x_i}(y_i-\hat{y_i}) \\
	& =\sum_{i=1}^{m}\hat{\boldsymbol x_i}(\hat{y_i}-y_i) \\
	& ={\boldsymbol X^T}(\hat{\boldsymbol y}-\boldsymbol{y}) \\
	& ={\boldsymbol X^T}(p_1(\boldsymbol X;β)-\boldsymbol{y}) \\
\end{aligned}$$

## 3.32

$$J=\cfrac{\boldsymbol w^T(\mu_0-\mu_1)(\mu_0-\mu_1)^T\boldsymbol w}{\boldsymbol w^T(\Sigma_0+\Sigma_1)\boldsymbol w}$$

[推导]：
$$\begin{aligned}
	J &= \cfrac{\big|\big|\boldsymbol w^T\mu_0-\boldsymbol w^T\mu_1\big|\big|_2^2}{\boldsymbol w^T(\Sigma_0+\Sigma_1)\boldsymbol w} \\
	&= \cfrac{\big|\big|(\boldsymbol w^T\mu_0-\boldsymbol w^T\mu_1)^T\big|\big|_2^2}{\boldsymbol w^T(\Sigma_0+\Sigma_1)\boldsymbol w} \\
	&= \cfrac{\big|\big|(\mu_0-\mu_1)^T\boldsymbol w\big|\big|_2^2}{\boldsymbol w^T(\Sigma_0+\Sigma_1)\boldsymbol w} \\
	&= \cfrac{[(\mu_0-\mu_1)^T\boldsymbol w]^T(\mu_0-\mu_1)^T\boldsymbol w}{\boldsymbol w^T(\Sigma_0+\Sigma_1)\boldsymbol w} \\
	&= \cfrac{\boldsymbol w^T(\mu_0-\mu_1)(\mu_0-\mu_1)^T\boldsymbol w}{\boldsymbol w^T(\Sigma_0+\Sigma_1)\boldsymbol w}
\end{aligned}$$

## 3.37

$$\boldsymbol S_b\boldsymbol w=\lambda\boldsymbol S_w\boldsymbol w$$

[推导]：由3.36可列拉格朗日函数：
$$l(\boldsymbol w)=-\boldsymbol w^T\boldsymbol S_b\boldsymbol w+\lambda(\boldsymbol w^T\boldsymbol S_w\boldsymbol w-1)$$
对$\boldsymbol w$求偏导可得：
$$\begin{aligned}
\cfrac{\partial l(\boldsymbol w)}{\partial \boldsymbol w} &= -\cfrac{\partial(\boldsymbol w^T\boldsymbol S_b\boldsymbol w)}{\partial \boldsymbol w}+\lambda \cfrac{(\boldsymbol w^T\boldsymbol S_w\boldsymbol w-1)}{\partial \boldsymbol w} \\
	&= -(\boldsymbol S_b+\boldsymbol S_b^T)\boldsymbol w+\lambda(\boldsymbol S_w+\boldsymbol S_w^T)\boldsymbol w
\end{aligned}$$
又$\boldsymbol S_b=\boldsymbol S_b^T,\boldsymbol S_w=\boldsymbol S_w^T$，则：
$$\cfrac{\partial l(\boldsymbol w)}{\partial \boldsymbol w} = -2\boldsymbol S_b\boldsymbol w+2\lambda\boldsymbol S_w\boldsymbol w$$
令导函数等于0即可得式3.37。
