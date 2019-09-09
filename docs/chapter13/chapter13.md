## 13.1

$$p(\boldsymbol{x})=\sum_{i=1}^{N} \alpha_{i} \cdot p\left(\boldsymbol{x} | \boldsymbol{\mu}_{i}, \mathbf{\Sigma}_{i}\right)$$
[解析]： 该式即为 9.4.3 节的式(9.29)，式(9.29)中的$k$个混合成分对应于此处的$N$个可能的类别

## 13.2
$$
\begin{aligned} f(\boldsymbol{x}) &=\underset{j \in \mathcal{Y}}{\arg \max } p(y=j | \boldsymbol{x}) \\ &=\underset{j \in \mathcal{Y}}{\arg \max } \sum_{i=1}^{N} p(y=j, \Theta=i | \boldsymbol{x}) \\ &=\underset{j \in \mathcal{Y}}{\arg \max } \sum_{i=1}^{N} p(y=j | \Theta=i, \boldsymbol{x}) \cdot p(\Theta=i | \boldsymbol{x}) \end{aligned}
$$
[解析]：
首先，该式的变量$\theta \in \{1,2,...,N\}$即为 9.4.3 节的式(9.30)中的 $\ z_j\in\{1,2,...k\}$
从公式第 1 行到第 2 行是做了边际化（marginalization）；具体来说第 2 行比第 1 行多了$\theta$为了消掉$\theta$对其进行求和（若是连续变量则为积分）$\sum_{i=1}^N$
[推导]：从公式第 2 行到第 3 行推导如下
$$\begin{aligned} p(y = j,\theta = i \vert x) &= \cfrac {p(y=j, \theta=i,x)} {p(x)} \\
&=\cfrac{p(y=j ,\theta=i,x)}{p(\theta=i,x)}\cdot \cfrac{p(\theta=i,x)}{p(x)} \\
&=p(y=j\vert \theta=i,x)\cdot p(\theta=i\vert x)\end{aligned}$$
[解析]：
其中$p(y=j\vert x)$表示$x$的类别$y$为第$j$个类别标记的后验概率（注意条件是已知$x$）； 
$p(y=j,\theta=i\vert x)$表示$x$的类别$y$为第$j$个类别标记且由第$i$个高斯混合成分生成的后验概率（注意条件是已知$x$ ）； 
$p(y=j,\theta=i,x)$表示第$i$个高斯混合成分生成的$x$其类别$y$为第$j$个类别标记的概率（注意条件是已知$\theta$和$x$，这里修改了西瓜书式(13.3)下方对$p(y=j\vert\theta=i,x)$的表述；
$p(\theta=i \vert x)$表示$x$由第$i$个高斯混合成分生成的后验概率（注意条件是已知$x$）；
西瓜书第 296 页第 2 行提到“假设样本由高斯混合模型生成，且每个类别对应一个高斯混合成分”，也就是说，如果已知$x$是由哪个高斯混合成分生成的，也就知道了其类别。而$p(y=j,\theta=i\vert x)$表示已知$\theta$和$x$ 的条件概率（其实已知$\theta$就足够，不需$x$的信息），因此
$$p(y=j\vert \theta=i,x)=
\begin{cases}
1,&i=j \\
0,&i\not=j
\end{cases}$$
## 13.3
$$
p(\Theta=i | \boldsymbol{x})=\frac{\alpha_{i} \cdot p\left(\boldsymbol{x} | \boldsymbol{\mu}_{i}, \mathbf{\Sigma}_{i}\right)}{\sum_{i=1}^{N} \alpha_{i} \cdot p\left(\boldsymbol{x} | \boldsymbol{\mu}_{i}, \mathbf{\Sigma}_{i}\right)}
$$
[解析]：该式即为 9.4.3 节的式(9.30)，具体推导参见有关式(9.30)的解释。
## 13.4
$$
\begin{aligned} L L\left(D_{l} \cup D_{u}\right)=& \sum_{\left(x_{j}, y_{j}\right) \in D_{l}} \ln \left(\sum_{i=1}^{N} \alpha_{i} \cdot p\left(\boldsymbol{x}_{j} | \boldsymbol{\mu}_{i}, \mathbf{\Sigma}_{i}\right) \cdot p\left(y_{j} | \Theta=i, \boldsymbol{x}_{j}\right)\right) \\ &+\sum_{x_{j} \in D_{u}} \ln \left(\sum_{i=1}^{N} \alpha_{i} \cdot p\left(\boldsymbol{x}_{j} | \boldsymbol{\mu}_{i}, \mathbf{\Sigma}_{i}\right)\right) \end{aligned}
$$
[解析]：由式(13.2)对概率$p(y=j\vert\theta =i,x)=$的分析，式中第 1 项中的$p(y_j\vert\theta =i,x_j)$ 为
$$p(y_j\vert \theta=i,x_j)=
\begin{cases}
1,&y_i=i \\
0,&y_i\not=i
\end{cases}$$
该式第 1 项针对有标记样本$(x_i,y_i) \in D_i$来说，因为有标记样本的类别是确定的，因此在计算它的对数似然时，它只可能来自$N$个高斯混合成分中的一个（西瓜书第 296 页第 2 行提到“假设样本由高斯混合模型生成，且每个类别对应一个高斯混合成分”），所以计算第 1 项计算有标记样本似然时乘以了$p(y_j\vert\theta =i,x_j)$ ；
该式第 2 项针对未标记样本$x_j\in D_u$；来说的，因为未标记样本的类别不确定，即它可能来自$N$个高斯混合成分中的任何一个，所以第 1 项使用了式(13.1)。
## 13.5
$$
\gamma_{j i}=\frac{\alpha_{i} \cdot p\left(\boldsymbol{x}_{j} | \boldsymbol{\mu}_{i}, \mathbf{\Sigma}_{i}\right)}{\sum_{i=1}^{N} \alpha_{i} \cdot p\left(\boldsymbol{x}_{j} | \boldsymbol{\mu}_{i}, \mathbf{\Sigma}_{i}\right)}
$$
[解析]：该式与式(13.3)相同，即后验概率。 可通过有标记数据对模型参数$(\alpha_i,\mu_i,\Sigma_i)$进行初始化，具体来说：
$$\alpha_i = \cfrac{l_i}{|D_l|},where |D_l| = \sum_{i=1}^N l_i$$
$$\mu_i = \cfrac{1}{l_i}\sum_{(x_j,y_j) \in D_l\wedge y_i=i}(x_j-\mu_j)(x_j-\mu_j)^T$$
$$
\Sigma_{i}=\frac{1}{l_{i}} \sum_{\left(x_{j}, y_{j}\right) \in D_{l} \wedge y_{j}=i}\left( x_{j}- \mu_{i}\right)\left( x_{j}-\mu_{i}\right)^{\top}
$$
其中$l_i$表示第$i$类样本的有标记样本数目，$|D_l|$为有标记样本集样本总数，$\wedge$为“逻辑与”。
## 13.6
$$
\boldsymbol{\mu}_{i}=\frac{1}{\sum_{\boldsymbol{x}_{j} \in D_{u}} \gamma_{j i}+l_{i}}\left(\sum_{\boldsymbol{x}_{j} \in D_{u}} \gamma_{j i} \boldsymbol{x}_{j}+\sum_{\left(\boldsymbol{x}_{j}, y_{j}\right) \in D_{l} \wedge y_{j}=i} \boldsymbol{x}_{j}\right)
$$
[推导]：类似于式(9.34)该式由$\cfrac{\partial LL(D_l \cup D_u) }{\partial \mu_i}=0$而得，将式(13.4)的两项分别记为：
$$LL(D_l)=\sum_{(x_j,y_j \in D_l)}ln(\sum_{s=1}^{N}\alpha_s \cdot p(x_j \vert \mu_s,\Sigma_s) \cdot p(y_i|\theta = s,x_j)$$
$$LL(D_u)=\sum_{x_j \in D_u} ln(\sum_{s=1}^N \alpha_s \cdot p(x_j | \mu_s,\Sigma_s))$$
对于式(13.4)中的第 1 项$LL(D_l)$，由于$p(y_j\vert \theta=i,x_j)$取值非1即0（详见13.2,13.4分析），因此
$$LL(D_l)=\sum_{(x_j,y_j)\in D_l} ln(\alpha_{y_j} \cdot p(x_j|\mu_{y_j}, \Sigma_{y_j}))$$
若求$LL(D_l)$对$\mu_i$的偏导，则$LL(D_l)$求和号中只有$y_j=i$ 的项能留下来，即

$$\begin{aligned}
\cfrac{\partial LL(D_l) }{\partial \mu_i} &=
\sum_{(x_i,y_i)\in D_l \wedge y_j=i} \cfrac{\partial ln(\alpha_i \cdot p(x_j| \mu_i,\Sigma_i))}{\partial\mu_i}\\
&=\sum_{(x_i,y_i)\in D_l \wedge y_j=i}\cfrac{1}{p(x_j|\mu_i,\Sigma_i) }\cdot  \cfrac{\partial p(x_j|\mu_i,\Sigma_i)}{\partial\mu_i}\\
&=\sum_{(x_i,y_i)\in D_l \wedge y_j=i}\cfrac{1}{p(x_j|\mu_i,\Sigma_i) }\cdot  p(x_j|\mu_i,\Sigma_i)  \cdot \Sigma_i^{-1}(x_j-\mu_i)\\
&=\sum_{\left(\boldsymbol{x}_{j}, y_{j}\right) \in D_{l} \wedge y_{j}=i}
\boldsymbol{\Sigma}_{i}^{-1}\left(\boldsymbol{x}_{j}-\boldsymbol{\mu}_{i}\right)
\end{aligned}$$

对于式(13.4)中的第 2 项$LL(D_u)$，求导结果与式(9.33)的推导过程一样
$$\cfrac{\partial LL(D_l \cup D_u) }{\partial \mu_i}=\sum_{x_j \in {D_u}} \cfrac{\alpha_i}{\sum_{s=1}^N \alpha_s \cdotp(x_j|\mu_s,\Sigma_s)} \cdot p(x_j|\mu_i,\Sigma_i )\cdot \Sigma_i^{-1}(x_j-\mu_i)$$
$$=\sum_{x_j \in D_u }\gamma_{ji} \cdot  \Sigma_i^{-1}(x_j-\mu_i)$$
综合两项结果，则$\cfrac{\partial LL(D_l \cup D_u) }{\partial \mu_i}$为
$$
\begin{aligned} \frac{\partial L L\left(D_{l} \cup D_{u}\right)}{\partial \mu_{i}} &=\sum_{\left(x_{j}, y_{j}\right) \in D_{t} \wedge y_{j}=i} \Sigma_{i}^{-1}\left(x_{j}-\mu_{i}\right)+\sum_{x_{j} \in D_{u}} \gamma_{j i} \cdot \Sigma_{i}^{-1}\left(x_{j}-\mu_{i}\right) \\ &=\Sigma_{i}^{-1}\left(\sum_{\left(x_{j}, y_{j}\right) \in D_{l} \wedge y_{j}=i}\left(x_{j}-\mu_{i}\right)+\sum_{x_{j} \in D_{u}} \gamma_{j i} \cdot\left(x_{j}-\mu_{i}\right)\right) \\ &=\Sigma_{i}^{-1}\left(\sum_{\left(x_{j}, y_{j}\right) \in D_{l} \wedge y_{j}=i} x_{j}+\sum_{x_{j} \in D_{u}} \gamma_{j i} \cdot x_{j}-\sum_{\left(x_{j}, y_{j}\right) \in D_{l} \wedge y_{j}=i} \mu_{i}-\sum_{x_{j} \in D_{u}} \gamma_{j i} \cdot \mu_{i}\right) \end{aligned}
$$
令$\frac{\partial L L\left(D_{l} \cup D_{u}\right)}{\partial \boldsymbol{\mu}_{i}}=0$，两边同时左乘$\Sigma_i$可将$\Sigma_i^{-1}$消掉，移项即得
$$
\sum_{x_{j} \in D_{u}} \gamma_{j i} \cdot \mu_{i}+\sum_{\left(x_{j}, y_{j}\right) \in D_{t} \wedge y_{j}=i} \mu_{i}=\sum_{x_{j} \in D_{u}} \gamma_{j i} \cdot x_{j}+\sum_{\left(x_{j}, y_{j}\right) \in D_{l} \wedge y_{j}=i} x_{j}
$$
上式中， 可以作为常量提到求和号外面，而$\sum_{\left(x_{j}, y_{j}\right) \in D_{l} \wedge y_{j}=i} 1=l_{i}$，即第 类样本的有标记 样本数目，因此
$$
\left(\sum_{x_{j} \in D_{u}} \gamma_{j i}+\sum_{\left(x_{j}, y_{j}\right) \in D_{l} \backslash y_{j}=i}  \mu_{i} \right) =\sum_{x_{j} \in D_{u}} \gamma_{j i} \cdot x_{j}+\sum_{\left(x_{j}, y_{j}\right) \in D_{l} \wedge y_{j}=i} x_{j}
$$
即得式(13.6)；
## 13.7
$$
\begin{aligned} \boldsymbol{\Sigma}_{i}=& \frac{1}{\sum_{\boldsymbol{x}_{j} \in D_{u}} \gamma_{j i}+l_{i}}\left(\sum_{\boldsymbol{x}_{j} \in D_{u}} \gamma_{j i}\left(\boldsymbol{x}_{j}-\boldsymbol{\mu}_{i}\right)\left(\boldsymbol{x}_{j}-\boldsymbol{\mu}_{i}\right)^{\mathrm{T}}\right.\\+& \sum_{\left(\boldsymbol{x}_{j}, y_{j}\right) \in D_{l} \wedge y_{j}=i}\left(\boldsymbol{x}_{j}-\boldsymbol{\mu}_{i}\right)\left(\boldsymbol{x}_{j}-\boldsymbol{\mu}_{i}\right)^{\mathrm{T}} ) \end{aligned}
$$
[推导]：类似于13.6 由$\cfrac{\partial LL(D_l \cup D_u) }{\partial \Sigma_i}=0$得，化简过程同13.6过程类似
对于式(13.4)中的第 1 项$LL(D_l)$ ，类似于刚才式(13.6)的推导过程;
$$
\begin{aligned} \frac{\partial L L\left(D_{l}\right)}{\partial \boldsymbol{\Sigma}_{i}} &=\sum_{\left(\boldsymbol{x}_{j}, y_{j}\right) \in D_{l} \wedge y_{j}=i} \frac{\partial \ln \left(\alpha_{i} \cdot p\left(\boldsymbol{x}_{j} | \boldsymbol{\mu}_{i}, \boldsymbol{\Sigma}_{i}\right)\right)}{\partial \boldsymbol{\Sigma}_{i}} \\ &=\sum_{\left(\boldsymbol{x}_{j}, y_{j}\right) \in D_{l} \wedge y_{j}=i} \frac{1}{p\left(\boldsymbol{x}_{j} | \boldsymbol{\mu}_{i}, \mathbf{\Sigma}_{i}\right)} \cdot \frac{\partial p\left(\boldsymbol{x}_{j} | \boldsymbol{\mu}_{i}, \boldsymbol{\Sigma}_{i}\right)}{\partial \boldsymbol{\Sigma}_{i}} \\
&=\sum_{\left(\boldsymbol{x}_{j}, y_{j}\right) \in D_{l} \wedge y_{j}=i} \frac{1}{p\left(\boldsymbol{x}_{j} | \boldsymbol{\mu}_{i}, \mathbf{\Sigma}_{i}\right)} \cdot p\left(\boldsymbol{x}_{j} | \boldsymbol{\mu}_{i}, \boldsymbol{\Sigma}_{i}\right) \cdot\left(\boldsymbol{\Sigma}_{i}^{-1}\left(\boldsymbol{x}_{j}-\boldsymbol{\mu}_{i}\right)\left(\boldsymbol{x}_{j}-\boldsymbol{\mu}_{i}\right)^{\top}-\boldsymbol{I}\right) \cdot \frac{1}{2} \boldsymbol{\Sigma}_{i}^{-1}\\
&=\sum_{\left(\boldsymbol{x}_{j}, y_{j}\right) \in D_{l} \wedge y_{j}=i}\left(\mathbf{\Sigma}_{i}^{-1}\left(\boldsymbol{x}_{j}-\boldsymbol{\mu}_{i}\right)\left(\boldsymbol{x}_{j}-\boldsymbol{\mu}_{i}\right)^{\top}-\boldsymbol{I}\right) \cdot \frac{1}{2} \boldsymbol{\Sigma}_{i}^{-1}
\end{aligned}
$$
对于式(13.4)中的第 2 项$LL(D_u)$ ，求导结果与式(9.35)的推导过程一样；
$$
\frac{\partial L L\left(D_{u}\right)}{\partial \boldsymbol{\Sigma}_{i}}=\sum_{\boldsymbol{x}_{j} \in D_{u}} \gamma_{j i} \cdot\left(\boldsymbol{\Sigma}_{i}^{-1}\left(\boldsymbol{x}_{j}-\boldsymbol{\mu}_{i}\right)\left(\boldsymbol{x}_{j}-\boldsymbol{\mu}_{i}\right)^{\top}-\boldsymbol{I}\right) \cdot \frac{1}{2} \boldsymbol{\Sigma}_{i}^{-1}
$$
综合两项结果，则$\cfrac{\partial LL(D_l \cup D_u) }{\partial \Sigma_i}$为
$$\begin{aligned} \frac{\partial L L\left(D_{l} \cup D_{u}\right)}{\partial \boldsymbol{\mu}_{i}}=& \sum_{\boldsymbol{x}_{j} \in D_{u}} \gamma_{j i} \cdot\left(\boldsymbol{\Sigma}_{i}^{-1}\left(\boldsymbol{x}_{j}-\boldsymbol{\mu}_{i}\right)\left(\boldsymbol{x}_{j}-\boldsymbol{\mu}_{i}\right)^{\top}-\boldsymbol{I}\right) \cdot \frac{1}{2} \boldsymbol{\Sigma}_{i}^{-1} \\ &+\sum_{\left(\boldsymbol{x}_{j}, y_{j}\right) \in D_{l} \wedge y_{j}=i}\left(\boldsymbol{\Sigma}_{i}^{-1}\left(\boldsymbol{x}_{j}-\boldsymbol{\mu}_{i}\right)\left(\boldsymbol{x}_{j}-\boldsymbol{\mu}_{i}\right)^{\top}-\boldsymbol{I}\right) \cdot \frac{1}{2} \boldsymbol{\Sigma}_{i}^{-1} \\
&=\left(\sum_{\boldsymbol{x}_{j} \in D_{u}} \gamma_{j i} \cdot\left(\boldsymbol{\Sigma}_{i}^{-1}\left(\boldsymbol{x}_{j}-\boldsymbol{\mu}_{i}\right)\left(\boldsymbol{x}_{j}-\boldsymbol{\mu}_{i}\right)^{\top}-\boldsymbol{I}\right)\right.\\ &+\sum_{\left(\boldsymbol{x}_{j}, y_{j}\right) \in D_{l} \wedge y_{j}=i}\left(\boldsymbol{\Sigma}_{i}^{-1}\left(\boldsymbol{x}_{j}-\boldsymbol{\mu}_{i}\right)\left(\boldsymbol{x}_{j}-\boldsymbol{\mu}_{i}\right)^{\top}-\boldsymbol{I}\right) ) \cdot \frac{1}{2} \boldsymbol{\Sigma}_{i}^{-1}
\end{aligned}
$$
令$\frac{\partial L L\left(D_{l} \cup D_{u}\right)}{\partial \boldsymbol{\Sigma}_{i}}=0$，两边同时右乘$2\Sigma_i$可将 $\cfrac{1}{2}\Sigma_i^{-1}$消掉，移项即得
$$
\begin{aligned} \sum_{\boldsymbol{x}_{j} \in D_{u}} \gamma_{j i} \cdot \boldsymbol{\Sigma}_{i}^{-1}\left(\boldsymbol{x}_{j}-\boldsymbol{\mu}_{i}\right)\left(\boldsymbol{x}_{j}-\boldsymbol{\mu}_{i}\right)^{\top}+& \sum_{\left(\boldsymbol{x}_{j}, y_{j} \in D_{l} \wedge y_{j}=i\right.} \boldsymbol{\Sigma}_{i}^{-1}\left(\boldsymbol{x}_{j}-\boldsymbol{\mu}_{i}\right)\left(\boldsymbol{x}_{j}-\boldsymbol{\mu}_{i}\right)^{\top} \\=& \sum_{\boldsymbol{x}_{j} \in D_{u}} \gamma_{j i} \cdot \boldsymbol{I}+\sum_{\left(\boldsymbol{x}_{j}, y_{j}\right) \in D_{l} \wedge y_{j}=i} \boldsymbol{I} \\ &=\left(\sum_{\boldsymbol{x}_{j} \in D_{u}} \gamma_{j i}+l_{i}\right) \boldsymbol{I} \end{aligned}
$$
两边同时左乘以$\Sigma_i$，上式变为
$$
\sum_{\boldsymbol{x}_{j} \in D_{u}} \gamma_{j i} \cdot\left(\boldsymbol{x}_{j}-\boldsymbol{\mu}_{i}\right)\left(\boldsymbol{x}_{j}-\boldsymbol{\mu}_{i}\right)^{\top}+\sum_{\left(\boldsymbol{x}_{j}, y_{j}\right) \in D_{l} \wedge y_{j}=i}\left(\boldsymbol{x}_{j}-\boldsymbol{\mu}_{i}\right)\left(\boldsymbol{x}_{j}-\boldsymbol{\mu}_{i}\right)^{\top}=\left(\sum_{\boldsymbol{x}_{j} \in D_{u}} \gamma_{j i}+l_{i}\right) \boldsymbol{\Sigma}_{i}
$$
即得式(13.7)；
## 13.8
$$
\alpha_{i}=\frac{1}{m}\left(\sum_{\boldsymbol{x}_{j} \in D_{u}} \gamma_{j i}+l_{i}\right)
$$
[推导]：类似于式(9.36)，写出$LL(D_l \cup D_u)$的拉格朗日形式
$$\begin{aligned}
\mathcal{L}(D_l \cup D_u,\lambda) &= LL(D_l \cup D_u)+\lambda(\sum_{s=1}^N \alpha_s -1)\\
& =LL(D_l)+LL(D_u)+\lambda(\sum_{s=1}^N \alpha_s - 1)\\
\end{aligned}$$
类似于式(9.37)，对$\alpha_i$求偏导。对于LL(D_u)，求导结果与式(9.37)的推导过程一样：
$$\cfrac{\partial LL(D_u)}{\partial\alpha_i} = \sum_{x_j \in D_u} \cfrac{1}{\Sigma_{s=1}^N \alpha_s \cdot p(x_j|\mu_s,\Sigma_s)} \cdot p(x_j|\mu_i,\Sigma_i)$$
对于$LL(D_l)$，类似于类似于(13.6)和(13.7)的推导过程
$$\begin{aligned}
\cfrac{\partial LL(D_l)}{\partial\alpha_i} &= \sum_{(x_i,y_i)\in D_l \wedge y_j=i} \cfrac{\partial ln(\alpha_i \cdot p(x_j| \mu_i,\Sigma_i))}{\partial\alpha_i}\\
&=\sum_{(x_i,y_i)\in D_l \wedge y_j=i}\cfrac{1}{ \alpha_i \cdot p(x_j|\mu_i,\Sigma_i) }\cdot  \cfrac{\partial (\alpha_i \cdot  p(x_j|\mu_i,\Sigma_i))}{\partial \alpha_i}\\
&=\sum_{(x_i,y_i)\in D_l \wedge y_j=i}\cfrac{1}{\alpha_i \cdot p(x_j|\mu_i,\Sigma_i) }\cdot  p(x_j|\mu_i,\Sigma_i)  \\
&=\cfrac{1}{\alpha_i} \cdot \sum_{(x_i,y_i)\in D_l \wedge y_j=i} 1 \\
&=\cfrac{l_i}{\alpha_i}
\end{aligned}$$
上式推导过程中，重点注意变量是$\alpha_i$ ，$p(x_j|\mu_i,\Sigma_i)$是常量；最后一行$\alpha_i$相对于求和变量为常量，因此作为公因子提到求和号外面； 为第$i$类样本的有标记样本数目。
综合两项结果，则$\cfrac{\partial LL(D_l \cup D_u) }{\partial \alpha_i}$为
$$\cfrac{\partial LL(D_l \cup D_u) }{\partial \mu_i} = \cfrac{l_i}{\alpha_i} + \sum_{x_j \in D_u} \cfrac{p(x_j|\mu_i,\Sigma_i)}{\Sigma_{s=1}^N \alpha_s \cdot p(x_j| \mu_s, \Sigma_s)}+\lambda$$
令$\cfrac{\partial LL(D_l \cup D_u) }{\partial \alpha_i}=0$并且两边同乘以$\alpha_i$,得
$$ \alpha_i \cdot \cfrac{l_i}{\alpha_i} + \sum_{x_j \in D_u} \cfrac{\alpha_i \cdot  p(x_j|\mu_i,\Sigma_i)}{\Sigma_{s=1}^N \alpha_s \cdot p(x_j| \mu_s, \Sigma_s)}+\lambda \cdot \alpha_i=0$$
结合式(9.30)发现，求和号内即为后验概率$\gamma_{ji}$,即
$$l_i+\sum_{x_i \in D_u} \gamma_{ji}+\lambda \alpha_i = 0$$
对所有混合成分求和，得
$$\sum_{i=1}^N l_i+\sum_{i=1}^N  \sum_{x_i \in D_u} \gamma_{ji}+\sum_{i=1}^N \lambda \alpha_i = 0$$
这里$\Sigma_{i=1}^N \alpha_i =1$ ，因此$\sum_{i=1}^N \lambda \alpha_i=\lambda\sum_{i=1}^N \alpha_i=\lambda$
根据（9.30）中$\gamma_{ji}$表达式可知
$$\sum_{i=1}^N \gamma_{ji} =  \sum_{i =1}^{N} \cfrac{\alpha_i \cdot  p(x_j|\mu_i,\Sigma_i)}{\Sigma_{s=1}^N \alpha_s \cdot p(x_j| \mu_s, \Sigma_s)}=  \cfrac{\sum_{i =1}^{N}\alpha_i \cdot  p(x_j|\mu_i,\Sigma_i)}{\sum_{s=1}^N \alpha_s \cdot p(x_j| \mu_s, \Sigma_s)}=1$$
再结合加法满足交换律，所以
$$\sum_{i=1}^N  \sum_{x_i \in D_u} \gamma_{ji}=\sum_{x_i \in D_u} \sum_{i=1}^N  \gamma_{ji} =\sum_{x_i \in D_u} 1=u$$
以上分析过程中，$\sum_{x_j\in D_u}$ 形式与$\sum_{j=1}^u$等价，其中u为未标记样本集的样本个数； $\sum_{i=1}^Nl_i=l$其中$l$为有标记样本集的样本个数；将这些结果代入
$$\sum_{i=1}^N l_i+\sum_{i=1}^N  \sum_{x_i \in D_u} \gamma_{ji}+\sum_{i=1}^N \lambda \alpha_i = 0$$
解出$l+u+\lambda = 0$  且$l+u =m$ 其中$m$为样本总个数，移项即得$\lambda = -m$
最后带入整理解得
$$l_i + \Sigma_{X_j \in{D_u}} \gamma_{ji}-m \alpha_i = 0$$
整理即得式(13.8)；


