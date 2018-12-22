### 3.7

$$ w=\cfrac{\sum_{i=1}^{m}y_i(x_i-\bar{x})}{\sum_{i=1}^{m}x_i^2-\cfrac{1}{m}(\sum_{i=1}^{m}x_i)^2} $$

令式（3.5）等于0：
$$ 0 = w\sum_{i=1}^{m}x_i^2-\sum_{i=1}^{m}(y_i-b)x_i $$
$$ w\sum_{i=1}^{m}x_i^2 = \sum_{i=1}^{m}y_ix_i-\sum_{i=1}^{m}bx_i $$
由于令式（3.6）等于0可得$ b=\cfrac{1}{m}\sum_{i=1}^{m}(y_i-wx_i) $，又$ \cfrac{1}{m}\sum_{i=1}^{m}y_i=\bar{y} $，$ \cfrac{1}{m}\sum_{i=1}^{m}x_i=\bar{x} $，则$ b=\bar{y}-w\bar{x} $，代入上式可得：
$$ 
\begin{aligned}	 
    w\sum_{i=1}^{m}x_i^2 & = \sum_{i=1}^{m}y_ix_i-\sum_{i=1}^{m}(\bar{y}-w\bar{x})x_i \\\\
    w\sum_{i=1}^{m}x_i^2 & = \sum_{i=1}^{m}y_ix_i-\bar{y}\sum_{i=1}^{m}x_i+w\bar{x}\sum_{i=1}^{m}x_i \\\\
    w(\sum_{i=1}^{m}x_i^2-\bar{x}\sum_{i=1}^{m}x_i) & = \sum_{i=1}^{m}y_ix_i-\bar{y}\sum_{i=1}^{m}x_i \\\\
    w & = \cfrac{\sum_{i=1}^{m}y_ix_i-\bar{y}\sum_{i=1}^{m}x_i}{\sum_{i=1}^{m}x_i^2-\bar{x}\sum_{i=1}^{m}x_i}
\end{aligned}
$$
又$ \bar{y}\sum_{i=1}^{m}x_i=\cfrac{1}{m}\sum_{i=1}^{m}y_i\sum_{i=1}^{m}x_i=\bar{x}\sum_{i=1}^{m}y_i $，$ \bar{x}\sum_{i=1}^{m}x_i=\cfrac{1}{m}\sum_{i=1}^{m}x_i\sum_{i=1}^{m}x_i=\cfrac{1}{m}(\sum_{i=1}^{m}x_i)^2 $，代入上式即可得式（3.7）：
$$ w=\cfrac{\sum_{i=1}^{m}y_i(x_i-\bar{x})}{\sum_{i=1}^{m}x_i^2-\cfrac{1}{m}(\sum_{i=1}^{m}x_i)^2} $$

【注】：式（3.7）还可以进一步化简为能用向量表达的形式，将$ \cfrac{1}{m}(\sum_{i=1}^{m}x_i)^2=\bar{x}\sum_{i=1}^{m}x_i $代入分母可得：
$$ 
\begin{aligned}	  
     w & = \cfrac{\sum_{i=1}^{m}y_i(x_i-\bar{x})}{\sum_{i=1}^{m}x_i^2-\bar{x}\sum_{i=1}^{m}x_i} \\\\
     & = \cfrac{\sum_{i=1}^{m}(y_ix_i-y_i\bar{x})}{\sum_{i=1}^{m}(x_i^2-x_i\bar{x})}
\end{aligned}
$$
又$ \bar{y}\sum_{i=1}^{m}x_i=\bar{x}\sum_{i=1}^{m}y_i=\sum_{i=1}^{m}\bar{y}x_i=\sum_{i=1}^{m}\bar{x}y_i=m\bar{x}\bar{y}=\sum_{i=1}^{m}\bar{x}\bar{y} $，则上式可化为：
$$ 
\begin{aligned}
    w & = \cfrac{\sum_{i=1}^{m}(y_ix_i-y_i\bar{x}-x_i\bar{y}+\bar{x}\bar{y})}{\sum_{i=1}^{m}(x_i^2-x_i\bar{x}-x_i\bar{x}+\bar{x}^2)} \\\\
    & = \cfrac{\sum_{i=1}^{m}(x_i-\bar{x})(y_i-\bar{y})}{\sum_{i=1}^{m}(x_i-\bar{x})^2} 
\end{aligned}
$$
若令$ \mathbf{X}=(x_1,x_2,...,x_m) $，$\mathbf{X}\_{demean}$为去均值后的$ \mathbf{X} $，$ \mathbf{y}=(y_1,y_2,...,y_m) $，$ \mathbf{y}\_{demean} $为去均值后的$ \mathbf{y} $，其中$ \mathbf{X} $、$ \mathbf{X}\_{demean} $、$ \mathbf{y} $、$ \mathbf{y}\_{demean} $均为m行1列的列向量，代入上式可得：
$$ w=\cfrac{\mathbf{X}\_{demean}\mathbf{y}\_{demean}^T}{\mathbf{X}\_{demean}\mathbf{X}\_{demean}^T}$$