## 7.5
$$R(c|\boldsymbol x)=1−P(c|\boldsymbol x)$$
[推导]：由式7.1和式7.4可得：
$$R(c_i|\boldsymbol x)=1*P(c_1|\boldsymbol x)+1*P(c_2|\boldsymbol x)+...+0*P(c_i|\boldsymbol x)+...+1*P(c_N|\boldsymbol x)$$
又$\sum_{j=1}^{N}P(c_j|\boldsymbol x)=1$，则：
$$R(c_i|\boldsymbol x)=1-P(c_i|\boldsymbol x)$$
此即为式7.5

## 7.17-7.18
$$P_{(\boldsymbol x_{i}|c)}\in[0,1]$$
$$p_{(\boldsymbol x_{i}| c)}$$
[解析]：式(7.17)所得$P_{(\boldsymbol x_{i}|c)}\in[0,1]$为条件概率，但式(7.18)所得$p_{(\boldsymbol x_{i}| c)}$为条件概率密度而非概率，其值并不在局限于区间[0,1]之内。

## 7.19
$$\hat{P}(c)=\frac{\left|D_{c}\right|+1}{|D|+N}$$
[解析]：从贝叶斯估计（参见附录①）的角度来说，拉普拉斯修正就等价于先验概率为Dirichlet分布（参见附录③）的后验期望值估计。为了接下来的叙述方便，我们重新定义一下相关数学符号。设包含$m$个独立同分布样本的训练集为$D$，$D$中可能的类别数为$k$，其类别的具体取值范围为$\{c_1,c_2,...,c_k\}$。若令随机变量$C$表示样本所属的类别，且$C$取到每个值的概率分别为$p(C=c_1)=\theta_1,p(C=c_2)=\theta_2,...,p(C=c_k)=\theta_k$，那么显然$C$服从参数为$\boldsymbol{\theta}=(\theta_1,\theta_2,...,\theta_k)\in\mathbb{R}^{k}$的Categorical分布（参见附录②），其概率质量函数为
$$p(C=c_i)=p(c_i)=\theta_1^{\mathbb{I}(C=c_1)}\ldots\theta_i^{\mathbb{I}(C=c_i)}\ldots\theta_k^{\mathbb{I}(C=c_k)}$$
其中$p(c_i)=\theta_i$就是公式(7.9)所要求解的$\hat{P}(c)$，下面我们用贝叶斯估计中的后验期望值估计来估计$\theta_i$。根据贝叶斯估计的原理可知，在进行参数估计之前，需要先主观预设一个先验概率$p(\boldsymbol{\theta})$，通常为了方便计算后验概率$p(\boldsymbol{\theta}|D)$，我们会用似然函数$p(D|\boldsymbol{\theta})$的共轭先验<sup>[6]</sup>作为我们的先验概率<sup>[7]</sup>。显然，此时的似然函数$p(D|\boldsymbol{\theta})$是一个基于Categorical分布的似然函数，而Categorical分布的共轭先验为Dirichlet分布，所以此时只需要预设先验概率$p(\boldsymbol{\theta})$为Dirichlet分布，然后使用后验期望值估计就能估计出$\theta_i$。具体地，记$D$中样本类别取值为$c_i$的样本个数为$y_i$，则似然函数$p(D|\boldsymbol{\theta})$可展开为
$$p(D|\boldsymbol{\theta})=\theta_1^{y_1}\ldots\theta_k^{y_k}=\prod_{i=1}^{k}\theta_i^{y_i}$$
那么后验概率$p(D|\boldsymbol{\theta})$为
$$\begin{aligned}
p(\boldsymbol{\theta}|D)&=\frac{p(D|\boldsymbol{\theta})p(\boldsymbol{\theta})}{p(D)}\\
&=\frac{p(D|\boldsymbol{\theta})p(\boldsymbol{\theta})}{\sum_{\boldsymbol{\theta}}p(D|\boldsymbol{\theta})p(\boldsymbol{\theta})}\\
&=\frac{\prod_{i=1}^{k}\theta_i^{y_i}\cdot p(\boldsymbol{\theta})}{\sum_{\boldsymbol{\theta}}\left[\prod_{i=1}^{k}\theta_i^{y_i}\cdot p(\boldsymbol{\theta})\right]}
\end{aligned}$$
假设此时先验概率$p(\boldsymbol{\theta})$是参数为$\boldsymbol{\alpha}=(\alpha_1,\alpha_2,...,\alpha_k)\in \mathbb{R}^{k}$的Dirichlet分布，则$p(\boldsymbol{\theta})$可写为
$$p(\boldsymbol{\boldsymbol{\theta}};\boldsymbol{\alpha})=\frac{\Gamma \left(\sum _{i=1}^{k}\alpha _{i}\right)}{\prod _{i=1}^{k}\Gamma (\alpha _{i})}\prod _{i=1}^{k}\theta_{i}^{\alpha _{i}-1}$$
将其代入$p(D|\boldsymbol{\theta})$可得
$$\begin{aligned}
p(\boldsymbol{\theta}|D)&=\frac{\prod_{i=1}^{k}\theta_i^{y_i}\cdot p(\boldsymbol{\theta})}{\sum_{\boldsymbol{\theta}}\left[\prod_{i=1}^{k}\theta_i^{y_i}\cdot p(\boldsymbol{\theta})\right]} \\
&=\frac{\prod_{i=1}^{k}\theta_i^{y_i}\cdot \frac{\Gamma \left(\sum _{i=1}^{k}\alpha _{i}\right)}{\prod _{i=1}^{k}\Gamma (\alpha _{i})}\prod _{i=1}^{k}\theta_{i}^{\alpha _{i}-1}}{\sum_{\boldsymbol{\theta}}\left[\prod_{i=1}^{k}\theta_i^{y_i}\cdot \frac{\Gamma \left(\sum _{i=1}^{k}\alpha _{i}\right)}{\prod _{i=1}^{k}\Gamma (\alpha _{i})}\prod _{i=1}^{k}\theta_{i}^{\alpha _{i}-1}\right]} \\
&=\frac{\prod_{i=1}^{k}\theta_i^{y_i}\cdot \frac{\Gamma \left(\sum _{i=1}^{k}\alpha _{i}\right)}{\prod _{i=1}^{k}\Gamma (\alpha _{i})}\prod _{i=1}^{k}\theta_{i}^{\alpha _{i}-1}}{\sum_{\boldsymbol{\theta}}\left[\prod_{i=1}^{k}\theta_i^{y_i}\cdot \prod _{i=1}^{k}\theta_{i}^{\alpha _{i}-1}\right]\cdot \frac{\Gamma \left(\sum _{i=1}^{k}\alpha _{i}\right)}{\prod _{i=1}^{k}\Gamma (\alpha _{i})}} \\
&=\frac{\prod_{i=1}^{k}\theta_i^{y_i}\cdot \prod _{i=1}^{k}\theta_{i}^{\alpha _{i}-1}}{\sum_{\boldsymbol{\theta}}\left[\prod_{i=1}^{k}\theta_i^{y_i}\cdot \prod _{i=1}^{k}\theta_{i}^{\alpha _{i}-1}\right]} \\
&=\frac{\prod_{i=1}^{k}\theta_i^{\alpha_{i}+y_i-1}}{\sum_{\boldsymbol{\theta}}\left[\prod_{i=1}^{k}\theta_i^{\alpha_{i}+y_i-1}\right]}
\end{aligned}$$
此时若设$\boldsymbol{\alpha}+\boldsymbol{y}=(\alpha_1+y_1,\alpha_2+y_2,...,\alpha_k+y_k)\in \mathbb{R}^{k}$，则根据Dirichlet分布的定义可知
$$\begin{aligned}
p(\boldsymbol{\theta};\boldsymbol{\alpha}+\boldsymbol{y})&=\frac{\Gamma \left(\sum _{i=1}^{k}(\alpha_{i}+y_i)\right)}{\prod _{i=1}^{k}\Gamma (\alpha_{i}+y_i)}\prod _{i=1}^{k}\theta_{i}^{\alpha_{i}+y_i-1} \\
\sum_{\boldsymbol{\theta}}p(\boldsymbol{\theta};\boldsymbol{\alpha}+\boldsymbol{y})&=\sum_{\boldsymbol{\theta}}\frac{\Gamma \left(\sum _{i=1}^{k}(\alpha_{i}+y_i)\right)}{\prod _{i=1}^{k}\Gamma (\alpha_{i}+y_i)}\prod _{i=1}^{k}\theta_{i}^{\alpha_{i}+y_i-1}\\
1&=\sum_{\boldsymbol{\theta}}\frac{\Gamma \left(\sum _{i=1}^{k}(\alpha_{i}+y_i)\right)}{\prod _{i=1}^{k}\Gamma (\alpha_{i}+y_i)}\prod _{i=1}^{k}\theta_{i}^{\alpha_{i}+y_i-1} \\
1&=\frac{\Gamma \left(\sum _{i=1}^{k}(\alpha_{i}+y_i)\right)}{\prod _{i=1}^{k}\Gamma (\alpha_{i}+y_i)}\sum_{\boldsymbol{\theta}}\left[\prod _{i=1}^{k}\theta_{i}^{\alpha_{i}+y_i-1}\right] \\
\frac{1}{\sum_{\boldsymbol{\theta}}\left[\prod _{i=1}^{k}\theta_{i}^{\alpha_{i}+y_i-1}\right]}&=\frac{\Gamma \left(\sum _{i=1}^{k}(\alpha_{i}+y_i)\right)}{\prod _{i=1}^{k}\Gamma (\alpha_{i}+y_i)} \\
\end{aligned}$$
将此结论代入$p(D|\boldsymbol{\theta})$可得
$$\begin{aligned}
p(\boldsymbol{\theta}|D)&=\frac{\prod_{i=1}^{k}\theta_i^{\alpha_{i}+y_i-1}}{\sum_{\boldsymbol{\theta}}\left[\prod_{i=1}^{k}\theta_i^{\alpha_{i}+y_i-1}\right]} \\
&=\frac{\Gamma \left(\sum _{i=1}^{k}(\alpha_{i}+y_i)\right)}{\prod _{i=1}^{k}\Gamma (\alpha_{i}+y_i)}\prod _{i=1}^{k}\theta_{i}^{\alpha _{i}+y_i-1} \\
&=p(\boldsymbol{\theta};\boldsymbol{\alpha}+\boldsymbol{y})
\end{aligned}$$
综上可知，对于服从Categorical分布的$\boldsymbol{\theta}$来说，假设其先验概率$p(\boldsymbol{\theta})$是参数为$\boldsymbol{\alpha}$的Dirichlet分布时，得到的后验概率$p(\boldsymbol{\theta}|D)$是参数为$\boldsymbol{\alpha}+\boldsymbol{y}$的Dirichlet分布，通常我们称这种先验概率分布和后验概率分布形式相同的这对分布为共轭分布<sup>[6]</sup>。在推得后验概率$p(\boldsymbol{\theta}|D)$的具体形式以后，根据后验期望值估计可得$\theta_i$的估计值为
$$\begin{aligned}
\theta_i&=E_{p(\boldsymbol{\theta}|D)}[\theta_i]\\
&=E_{p(\boldsymbol{\theta};\boldsymbol{\alpha}+\boldsymbol{y})}[\theta_i]\\
&=\frac{\alpha_i+y_i}{\sum_{j=1}^k(\alpha_j+y_j)}\\
&=\frac{\alpha_i+y_i}{\sum_{j=1}^k\alpha_j+\sum_{j=1}^ky_j}\\
&=\frac{\alpha_i+y_i}{\sum_{j=1}^k\alpha_j+m}\\
\end{aligned}$$
显然，公式(7.9)是当$\boldsymbol{\alpha}=(1,1,...,1)$时推得的具体结果，此时等价于我们主观预设的先验概率$p(\boldsymbol{\theta})$服从均匀分布，此即为拉普拉斯修正。同理，当我们调整$\boldsymbol{\alpha}$的取值后，即可推得其他数据平滑的公式。

## 7.20
$$\hat{P}\left(x_{i} | c\right)=\frac{\left|D_{c, x_{i}}\right|+1}{\left|D_{c}\right|+N_{i}}$$
[解析]：参见公式(7.19)

## 7.23
$$P(c|\boldsymbol x)\propto{\sum_{i=1 \atop |D_{x_{i}}|\geq m'}^{d}}P(c,x_{i})\prod_{j=1}^{d}P(x_j|c,x_i)$$
[推导]：
$$\begin{aligned}
P(c|\boldsymbol x)&=\cfrac{P(\boldsymbol x,c)}{P(\boldsymbol x)}\\
&=\cfrac{P\left(x_{1}, x_{2}, \ldots, x_{d}, c\right)}{P(\boldsymbol x)}\\
&=\cfrac{P\left(x_{1}, x_{2}, \ldots, x_{d} | c\right) P(c)}{P(\boldsymbol x)} \\
&=\cfrac{P\left(x_{1}, \ldots, x_{i-1}, x_{i+1}, \ldots, x_{d} | c, x_{i}\right) P\left(c, x_{i}\right)}{P(\boldsymbol x)} \\
\end{aligned}$$
$$\begin{aligned}
P(c|\boldsymbol x)&\propto P(c,x_{i})P(x_{1},…,x_{i-1},x_{i+1},…,x_{d}|c,x_{i}) \\
&=P(c,x_{i})\prod _{j=1}^{d}P(x_j|c,x_i)
\end{aligned}$$
$$P(c|\boldsymbol x)\propto{\sum_{i=1 \atop |D_{x_{i}}|\geq m'}^{d}}P(c,x_{i})\prod_{j=1}^{d}P(x_j|c,x_i)$$
此即为式7.23，由于式(7.24)和式(7.25)的使用到了$|D_{c,x_{i}}|$与$|D_{c,x_{i},x_{j}}|$，若$|D_{x_{i}}|$集合中样本数量过少，则$|D_{c,x_{i}}|$与$|D_{c,x_{i},x_{j}}|$将会更小，因此在式(7.23)中要求$|D_{x_{i}}|$集合中样本数量不少于$m'$。

## 附录
### ①贝叶斯估计<sup>[1]</sup>
贝叶斯学派视角下的一类点估计法称为贝叶斯估计，常用的贝叶斯估计有最大后验估计（Maximum A Posteriori Estimation，简称MAP）、后验中位数估计和后验期望值估计这3种参数估计方法，下面给出这3种方法的具体定义。设总体的概率质量函数（若总体的分布为连续型时则改为概率密度函数，此处以离散型为例）为$p(x|\theta)$，从该总体中抽取出的$n$个独立同分布的样本构成的样本集为$D=\{x_1,x_2,...,x_n\}$，则根据贝叶斯公式可得在给定样本集$D$的条件下，$\theta$的条件概率为
$$p(\theta|D)=\frac{p(D|\theta)p(\theta)}{p(D)}=\frac{p(D|\theta)p(\theta)}{\sum_{\theta}p(D|\theta)p(\theta)}$$
其中$p(D|\theta)$为似然函数，由于样本集$D$中的样本是独立同分布的，所以似然函数可以进一步展开
$$p(\theta|D)=\frac{p(D|\theta)p(\theta)}{\sum_{\theta}p(D|\theta)p(\theta)}=\frac{\prod_{i=1}^{n}p(x_i|\theta) p(\theta)}{\sum_{\theta}\prod_{i=1}^{n}p(x_i|\theta)p(\theta)}$$
根据贝叶斯学派的观点，此条件概率代表了我们在已知样本集$D$后对$\theta$产生的新的认识，它综合了我们对$\theta$主观预设的先验概率$p(\theta)$和样本集$D$带来的信息，通常称其为$\theta$的后验概率。贝叶斯学派认为，在得到$p(\theta|D)$以后，对参数$\theta$的任何统计推断，都只能基于$p(\theta|D)$。至于具体如何去使用它，可以结合某种准则一起去进行，统计学家也有一定的自由度。对于点估计来说，求使得$p(\theta|D)$达到最大值的$\hat{\theta}_{MAP}$作为$\theta$的估计称为最大后验估计；求$p(\theta|D)$的中位数$\hat{\theta}_{Median}$作为$\theta$的估计称为后验中位数估计；求$p(\theta|D)$的期望值（均值）$\hat{\theta}_{Mean}$作为$\theta$的估计称为后验期望值估计。

### ②Categorical分布<sup>[2]</sup>
Categorical分布又称为广义伯努利分布，是将伯努利分布中的随机变量可取值个数由两个泛化为多个得到的分布。具体地，设离散型随机变量$X$共有$k$种可能的取值$\{x_1,x_2,...,x_k\}$，且$X$取到每个值的概率分别为$p(X=x_1)=\theta_1,p(X=x_2)=\theta_2,...,p(X=x_k)=\theta_k$，则称随机变量$X$服从参数为$\theta_1,\theta_2,...,\theta_k$的Categorical分布，其概率质量函数为
$$p(X=x_i)=p(x_i)=\theta_1^{\mathbb{I}(X=x_1)}\ldots\theta_i^{\mathbb{I}(X=x_i)}\ldots\theta_k^{\mathbb{I}(X=x_k)}$$
其中$\mathbb{I}(\cdot)$是指示函数，若$\cdot$为真则取值1，否则取值0。

### ③Dirichlet分布<sup>[3]</sup>
类似于Categorical分布是伯努利分布的泛化形式，Dirichlet分布是Beta分布<sup>[4]</sup>的泛化形式。对于一个$k$维随机变量$\boldsymbol{x}=(x_1,x_2,...,x_k)\in \mathbb{R}^{k}$，其中$x_i(i=1,2,...,k)$满足$0\leqslant x_i \leqslant 1,\sum_{i=1}^{k}x_i=1$，若$\boldsymbol{x}$服从参数为$\boldsymbol{\alpha}=(\alpha_1,\alpha_2,...,\alpha_k)\in \mathbb{R}^{k}$的Dirichlet分布，则其概率密度函数为
$$p(\boldsymbol{x};\boldsymbol{\alpha})=\frac{\Gamma \left(\sum _{i=1}^{k}\alpha _{i}\right)}{\prod _{i=1}^{k}\Gamma (\alpha _{i})}\prod _{i=1}^{k}x_{i}^{\alpha _{i}-1}$$
其中$\Gamma (z)=\int _{0}^{\infty }x^{z-1}e^{-x}dx$为Gamma函数<sup>[5]</sup>，当$\boldsymbol{\alpha}=(1,1,...,1)$时，Dirichlet分布等价于均匀分布。

## 参考文献
[1]陈希孺编著.概率论与数理统计[M].中国科学技术大学出版社,2009. <br>
[2]https://en.wikipedia.org/wiki/Categorical_distribution <br>
[3]https://en.wikipedia.org/wiki/Dirichlet_distribution <br>
[4]https://en.wikipedia.org/wiki/Beta_distribution <br>
[5]https://en.wikipedia.org/wiki/Gamma_function <br>
[6]https://en.wikipedia.org/wiki/Conjugate_prior <br>
[7]https://baike.baidu.com/item/%E5%85%B1%E8%BD%AD%E5%85%88%E9%AA%8C%E5%88%86%E5%B8%83 <br>