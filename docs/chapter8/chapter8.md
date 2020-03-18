## 8.1

$$
P\left(h_{i}(\boldsymbol{x}) \neq f(\boldsymbol{x})\right)=\epsilon
$$

[解析]：$h_{i}(\boldsymbol{x})$是编号为$i$的基分类器，$f(\boldsymbol{x})$是真实函数，取值时-1或1，当基学习器分类错误时的概率是$\epsilon$

## 8.2

$$
H(\boldsymbol{x})=\operatorname{sign}\left(\sum_{i=1}^{T} h_{i}(\boldsymbol{x})\right)
$$

[解析]：$h_i(\boldsymbol{x})$当把$\boldsymbol{x}$分成1时，$h_i(\boldsymbol{x})=1$，否则$h_i(\boldsymbol{x})=-1$各个分类器的结果求和之后数字的正、负或0，代表投票法产生的结果，即“少数服从多数”，符号函数$\operatorname{sign}$，将正数变成1，负数变成-1，0仍然是0，所以$H(\boldsymbol{x})$是由投票法产生的分类结果。

## 8.3

$$
\begin{aligned} P(H(\boldsymbol{x}) \neq f(\boldsymbol{x})) &=\sum_{k=0}^{\lfloor T / 2\rfloor} \left( \begin{array}{c}{T} \\ {k}\end{array}\right)(1-\epsilon)^{k} \epsilon^{T-k} \\ & \leqslant \exp \left(-\frac{1}{2} T(1-2 \epsilon)^{2}\right) \end{aligned}
$$

[推导]:由基分类器相互独立，设X为T个基分类器分类正确的次数，因此$\mathrm{X} \sim \mathrm{B}(\mathrm{T}, 1-\mathrm{\epsilon})$，设$x_i$为每一个分类器分类正确的次数，则$x_i\sim \mathrm{B}(1, 1-\mathrm{\epsilon})（i=1，2，3，...，\mathrm{T}）$，那么$$\mathrm{X}=\sum_{i=1}^{\mathrm{T}} x_i，\mathbb{E}(X)=\sum_{i=1}^{\mathrm{T}}\mathbb{E}(x_i)$$
证明过程如下：
$$
\begin{aligned} P(H(x) \neq f(x))=& P(X \leq\lfloor T / 2\rfloor) \\ & \leqslant P(X \leq T / 2)
\\ & =P\left[X-(1-\varepsilon) T \leqslant \frac{T}{2}-(1-\varepsilon) T\right] 
\\ & =P\left[X-
(1-\varepsilon) T \leqslant -\frac{T}{2}\left(1-2\varepsilon\right)]\right]
\\ &=P\left[\sum_{i=1}^{\mathrm{T}} x_i-
\sum_{i=1}^{\mathrm{T}}\mathbb{E}(x_i) \leqslant -\frac{T}{2}\left(1-2\varepsilon\right)]\right]
\\ &=P\left[\frac{1}{\mathrm{T}}\sum_{i=1}^{\mathrm{T}} x_i-\frac{1}{\mathrm{T}}
\sum_{i=1}^{\mathrm{T}}\mathbb{E}(x_i) \leqslant -\frac{1}{2}\left(1-2\varepsilon\right)]\right]
 \end{aligned}
$$
根据Hoeffding不等式知
$$
P\left(\frac{1}{m} \sum_{i=1}^{m} x_{i}-\frac{1}{m} \sum_{i=1}^{m} \mathbb{E}\left(x_{i}\right) \leqslant -\epsilon\right) \leqslant  \exp \left(-2 m \epsilon^{2}\right)
$$
令$\varepsilon=\frac {(1-2\epsilon)}{2},m=T$得
$$
\begin{aligned} P(H(\boldsymbol{x}) \neq f(\boldsymbol{x})) &=\sum_{k=0}^{\lfloor T / 2\rfloor} \left( \begin{array}{c}{T} \\ {k}\end{array}\right)(1-\epsilon)^{k} \epsilon^{T-k} \\ & \leqslant \exp \left(-\frac{1}{2} T(1-2 \epsilon)^{2}\right) \end{aligned}
$$

## 8.4

$$
H(\boldsymbol{x})=\sum_{t=1}^{T} \alpha_{t} h_{t}(\boldsymbol{x})
$$



[解析]：这个式子是集成学习的加性模型，加性模型不采用梯度下降的思想，而是$H(\boldsymbol{x})=\sum_{t=1}^{T-1} \alpha_{t} h_{t}(\boldsymbol{x})+\alpha_{T}h_{T}(\boldsymbol{x})$每次更新求解一个理论上最优的$h_T$（见式8.18）和$\alpha_T$（见式8.11）

## 8.5

$$
\begin{aligned}
\ell_{\mathrm{exp}}(H | \mathcal{D})=&\mathbb{E}_{\boldsymbol{x} \sim \mathcal{D}}\left[e^{-f(\boldsymbol{x}) H(\boldsymbol{x})}\right]
\\ =&P(f(x)=1|x)*e^{-H(x)}+P(f(x)=-1|x)*e^{H(x)}
\end{aligned}
$$

[解析]：由式(8.4)知
$$
H(\boldsymbol{x})=\sum_{t=1}^{T} \alpha_{t} h_{t}(\boldsymbol{x})
$$
又由式(8.11)可知
$$
\alpha_{t}=\frac{1}{2} \ln \left(\frac{1-\epsilon_{t}}{\epsilon_{t}}\right)
$$
该分类器的权重只与分类器的错误率负相关(即错误率越大，权重越低)

1. 先考虑指数损失函数$e^{-f(x) H(x)}$的含义：$f$为真实函数，对于样本$x$来说，$f(\boldsymbol{x}) \in\{+1,-1\}$只能取+1和-1，而$H(\boldsymbol{x})$是一个实数；
   当$H(\boldsymbol{x})$的符号与$f(x)$一致时，$f(\boldsymbol{x}) H(\boldsymbol{x})>0$，因此$e^{-f(\boldsymbol{x}) H(\boldsymbol{x})}=e^{-|H(\boldsymbol{x})|}<1$，且$|H(\boldsymbol{x})|$越大指数损失函数$e^{-f(\boldsymbol{x}) H(\boldsymbol{x})}$越小（这很合理：此时$|H(\boldsymbol{x})|$越大意味着分类器本身对预测结果的信心越大，损失应该越小；若$|H(\boldsymbol{x})|$在零附近，虽然预测正确，但表示分类器本身对预测结果信心很小，损失应该较大）；
   当$H(\boldsymbol{x})$的符号与$f(\boldsymbol{x})$不一致时，$f(\boldsymbol{x}) H(\boldsymbol{x})<0$，因此$e^{-f(\boldsymbol{x}) H(\boldsymbol{x})}=e^{|H(\boldsymbol{x})|}>1$，且$| H(\boldsymbol{x}) |$越大指数损失函数越大（这很合理：此时$| H(\boldsymbol{x}) |$越大意味着分类器本身对预测结果的信心越大，但预测结果是错的，因此损失应该越大；若$| H(\boldsymbol{x}) |$在零附近，虽然预测错误，但表示分类器本身对预测结果信心很小，虽然错了，损失应该较小）；
2. 符号$\mathbb{E}_{\boldsymbol{x} \sim \mathcal{D}}[\cdot]$的含义：$\mathcal{D}$为概率分布，可简单理解为在数据集$D$中进行一次随机抽样，每个样本被取到的概率；$\mathbb{E}[\cdot]$为经典的期望，则综合起来$\mathbb{E}_{\boldsymbol{x} \sim \mathcal{D}}[\cdot]$表示在概率分布$\mathcal{D}$上的期望，可简单理解为对数据集$D$以概率$\mathcal{D}$进行加权后的期望。

## 8.6

$$
\frac{\partial \ell_{\exp }(H | \mathcal{D})}{\partial H(\boldsymbol{x})}=-e^{-H(\boldsymbol{x})} P(f(\boldsymbol{x})=1 | \boldsymbol{x})+e^{H(\boldsymbol{x})} P(f(\boldsymbol{x})=-1 | \boldsymbol{x})
$$

[解析]：为求得$\ell_{\exp }(H | \mathcal{D})$损失函数的最小值，先对未知数$H$求偏导，$P(f(x)=1|x)和P(f(x)=-1|x)$为常数

故式(8.6)可轻易推知

$$
\frac{\partial \ell_{\exp }(H | \mathcal{D})}{\partial H(\boldsymbol{x})}=-e^{-H(\boldsymbol{x})} P(f(\boldsymbol{x})=1 | \boldsymbol{x})+e^{H(\boldsymbol{x})} P(f(\boldsymbol{x})=-1 | \boldsymbol{x})
$$

## 8.7

$$
H(\boldsymbol{x})=\frac{1}{2} \ln \frac{P(f(x)=1 | \boldsymbol{x})}{P(f(x)=-1 | \boldsymbol{x})}
$$

[解析]：令式(8.6)等于0，分离出来$H(\boldsymbol{x})$，即可得到式(8.7)。

## 8.8

$$
\begin{aligned}
\operatorname{sign}(H(\boldsymbol{x}))&=\operatorname{sign}\left(\frac{1}{2} \ln \frac{P(f(x)=1 | \boldsymbol{x})}{P(f(x)=-1 | \boldsymbol{x})}\right)
\\ & =\left\{\begin{array}{ll}{1,} & {P(f(x)=1 | \boldsymbol{x})>P(f(x)=-1 | \boldsymbol{x})} \\ {-1,} & {P(f(x)=1 | \boldsymbol{x})<P(f(x)=-1 | \boldsymbol{x})}\end{array}\right.
\\ & =\underset{y \in\{-1,1\}}{\arg \max } P(f(x)=y | \boldsymbol{x})
\end{aligned}
$$

[解析]：该式显然成立，由(8.6)知，假设$H(\boldsymbol{x})$可以使指数损失函数最小化，同时由该式可以看出来，也满足真实函数与分类器$H(\boldsymbol{x})$结果一致

## 8.9

$$
\begin{aligned}
\ell_{\exp }\left(\alpha_{t} h_{t} | \mathcal{D}_{t}\right) &=\mathbb{E}_{\boldsymbol{x} \sim \mathcal{D}_{t}}\left[e^{-f(\boldsymbol{x}) \alpha_{t} h_{t}(\boldsymbol{x})}\right] \\
&=\mathbb{E}_{\boldsymbol{x} \sim \mathcal{D}_{t}}\left[e^{-\alpha_{t}} \mathbb{I}\left(f(\boldsymbol{x})=h_{t}(\boldsymbol{x})\right)+e^{\alpha_{t}} \mathbb{I}\left(f(\boldsymbol{x}) \neq h_{t}(\boldsymbol{x})\right)\right] \\
&=e^{-\alpha_{t}} P_{\boldsymbol{x} \sim \mathcal{D}_{t}}\left(f(\boldsymbol{x})=h_{t}(\boldsymbol{x})\right)+e^{\alpha_{t}} P_{\boldsymbol{x} \sim \mathcal{D}_{t}}\left(f(\boldsymbol{x}) \neq h_{t}(\boldsymbol{x})\right) \\
&=e^{-\alpha_{t}}\left(1-\epsilon_{t}\right)+e^{\alpha_{t}} \epsilon_{t}
\end{aligned}
$$

[解析]：$\epsilon_t$与式(8.1)一致，为$h_t(\boldsymbol{x})$分类错误的概率

## 8.10

$$
\frac{\partial \ell_{\exp }\left(\alpha_{t} h_{t} | \mathcal{D}_{t}\right)}{\partial \alpha_{t}}=-e^{-\alpha_{t}}\left(1-\epsilon_{t}\right)+e^{\alpha_{t}} \epsilon_{t}
$$

[解析]：指数损失函数对$\alpha_t$求偏导，为了得到使得损失函数取最小值时$\alpha_t$的值

## 8.11

$$
\alpha_{t}=\frac{1}{2} \ln \left(\frac{1-\epsilon_{t}}{\epsilon_{t}}\right)
$$

[解析]：令偏导数等于0，得到的该式，此时$\alpha_t$的取值使得该基分类器经$\alpha_t$加权后的损失函数最小

## 8.12

$$
\begin{aligned}
\ell_{\exp }\left(H_{t-1}+h_{t} | \mathcal{D}\right) &=\mathbb{E}_{x \sim \mathcal{D}}\left[e^{-f(x)\left(H_{t-1}(x)+h_{t}(x)\right)}\right] \\
&=\mathbb{E}_{x \sim \mathcal{D}}\left[e^{-f(x) H_{t-1}(x)} e^{-f(x) h_{t}(x)}\right]
\end{aligned}
$$

[解析]：因为理想的$h_t(\boldsymbol{x})$可以纠正理想的$h_t$可以纠正$H_{t-1}$的全部错误，所以权重系数为1。如果权重系数$\alpha_t$是个常数的话，对后续结果也没有影响。

## 8.13

$$
\ell_{\exp }\left(H_{t-1}+h_{t} | \mathcal{D}\right)=\mathbb{E}_{\boldsymbol{x} \sim \mathcal{D}}\left[e^{-f(\boldsymbol{x}) H_{t-1}(\boldsymbol{x})}\left(1-f(\boldsymbol{x}) h_{t}(\boldsymbol{x})+\frac{1}{2}\right)\right]
$$

[推导]：由$e^x$的二阶泰勒展开为$1+x+\frac{x^2}{2}+o(x^2)$得:
$$
\begin{aligned}
\ell_{\exp }\left(H_{t-1}+h_{t} | \mathcal{D}\right) &=\mathbb{E}_{\boldsymbol{x} \sim \mathcal{D}}\left[e^{-f(\boldsymbol{x}) H_{t-1}(\boldsymbol{x})} e^{-f(\boldsymbol{x}) h_{t}(\boldsymbol{x})}\right]
\\ & \simeq \mathbb{E}_{\boldsymbol{x} \sim \mathcal{D}}\left[e^{-f(\boldsymbol{x}) H_{t-1}(\boldsymbol{x})}\left(1-f(\boldsymbol{x}) h_{t}(\boldsymbol{x})+\frac{f^{2}(\boldsymbol{x}) h_{t}^{2}(\boldsymbol{x})}{2}\right)\right]
\end{aligned}
$$
因为$f(\boldsymbol{x})$与$h_t(\boldsymbol{x})$取值都为1或-1，所以$f^2(\boldsymbol{x})=h_t^2(\boldsymbol{x})=1$，所以得:
$$
\ell_{\exp }\left(H_{t-1}+h_{t} | \mathcal{D}\right)= \mathbb{E}_{\boldsymbol{x} \sim \mathcal{D}}\left[e^{-f(\boldsymbol{x}) H_{t-1}(\boldsymbol{x})}\left(1-f(\boldsymbol{x}) h_{t}(\boldsymbol{x})+\frac{1}{2}\right)\right]
$$

## 8.14

$$
\begin{aligned}
h_{t}(\boldsymbol{x})&=\underset{h}{\arg \min } \ell_{\exp }\left(H_{t-1}+h | \mathcal{D}\right)\\
&=\underset{h}{\arg \min } \mathbb{E}_{\boldsymbol{x} \sim \mathcal{D}}\left[e^{-f(\boldsymbol{x}) H_{t-1}(\boldsymbol{x})}\left(1-f(\boldsymbol{x}) h(\boldsymbol{x})+\frac{1}{2}\right)\right]\\
&=\underset{h}{\arg \max } \mathbb{E}_{\boldsymbol{x} \sim \mathcal{D}}\left[e^{-f(\boldsymbol{x}) H_{t-1}(\boldsymbol{x})} f(\boldsymbol{x}) h(\boldsymbol{x})\right]\\
&=\underset{h}{\arg \max } \mathbb{E}_{\boldsymbol{x} \sim \mathcal{D}}\left[\frac{e^{-f(\boldsymbol{x}) H_{t-1}(\boldsymbol{x})}}{\mathbb{E}_{\boldsymbol{x} \sim \mathcal{D}}\left[e^{-f(\boldsymbol{x}) H_{t-1}(\boldsymbol{x})}\right]} f(\boldsymbol{x}) h(\boldsymbol{x})\right]
\end{aligned}
$$

[解析]：理想的$h_t(\boldsymbol{x})$是使得$H_{t}(\boldsymbol{x})$的指数损失函数取得最小值时的$h_t(\boldsymbol{x})$，该式将此转化成某个期望的最大值

## 8.16

$$
\begin{aligned} h_{t}(\boldsymbol{x}) &=\underset{h}{\arg \max } \mathbb{E}_{\boldsymbol{x} \sim \mathcal{D}}\left[\frac{e^{-f(\boldsymbol{x}) H_{t-1}(\boldsymbol{x})}}{\mathbb{E}_{\boldsymbol{x} \sim \mathcal{D}}\left[e^{-f(\boldsymbol{x}) H_{t-1}(\boldsymbol{x})}\right]} f(\boldsymbol{x}) h(\boldsymbol{x})\right] \\ &=\underset{\boldsymbol{h}}{\arg \max } \mathbb{E}_{\boldsymbol{x} \sim \mathcal{D}_{t}}[f(\boldsymbol{x}) h(\boldsymbol{x})] \end{aligned}
$$

[推导]：假设$\boldsymbol{x}$的概率分布是$f(\boldsymbol{x})$
(注:本书中概率分布全都是$\mathcal{D(\boldsymbol{x})}$)
$$
\mathbb{E(g(\boldsymbol{x}))}=\sum_{i=1}^{|D|}f(\boldsymbol{x}_i)g(\boldsymbol{x}_i)
$$
故可得

$$
\mathbb{E}_{\boldsymbol{x} \sim \mathcal{D}}\left[e^{-f(\boldsymbol{x}) H(\boldsymbol{x})}\right]=\sum_{i=1}^{|D|} \mathcal{D}\left(\boldsymbol{x}_{i}\right) e^{-f\left(\boldsymbol{x}_{i}\right) H\left(\boldsymbol{x}_{i}\right)}
$$
由式(8.15)可知
$$
\mathcal{D}_{t}\left(\boldsymbol{x}_{i}\right)=\mathcal{D}\left(\boldsymbol{x}_{i}\right) \frac{e^{-f\left(\boldsymbol{x}_{i}\right) H_{t-1}\left(\boldsymbol{x}_{i}\right)}}{\mathbb{E}_{\boldsymbol{x} \sim \mathcal{D}}\left[e^{-f(\boldsymbol{x}) H_{t-1}(\boldsymbol{x})}\right]}
$$

所以式(8.16)可以表示为
$$
\begin{aligned} & \mathbb{E}_{\boldsymbol{x} \sim \mathcal{D}}\left[\frac{e^{-f(\boldsymbol{x}) H_{t-1}(\boldsymbol{x})}}{\mathbb{E}_{\boldsymbol{x} \sim \mathcal{D}}\left[e^{-f(\boldsymbol{x}) H_{t-1}(\boldsymbol{x})}\right]} f(\boldsymbol{x}) h(\boldsymbol{x})\right] \\=& \sum_{i=1}^{|D|} \mathcal{D}\left(\boldsymbol{x}_{i}\right) \frac{e^{-f\left(\boldsymbol{x}_{i}\right) H_{t-1}\left(\boldsymbol{x}_{i}\right)}}{\mathbb{E}_{\boldsymbol{x} \sim \mathcal{D}}\left[e^{-f(\boldsymbol{x}) H_{t-1}(\boldsymbol{x}) }]  \right.}f(x_i)h(x_i) \\=& \sum_{i=1}^{|D|} \mathcal{D}_{t}\left(\boldsymbol{x}_{i}\right) f\left(\boldsymbol{x}_{i}\right) h\left(\boldsymbol{x}_{i}\right) \\=& \mathbb{E}_{\boldsymbol{x} \sim \mathcal{D}_{t}}[f(\boldsymbol{x}) h(\boldsymbol{x})] \end{aligned}
$$

## 8.17

$$
f(\boldsymbol{x}) h(\boldsymbol{x})=1-2 \mathbb{I}(f(\boldsymbol{x}) \neq h(\boldsymbol{x}))
$$

[解析]：当$f(\boldsymbol{x})=h(\boldsymbol{x})$时，$\mathbb{I}(f(\boldsymbol{x}) \neq h(\boldsymbol{x}))=0$，$f(\boldsymbol{x}) h(\boldsymbol{x})=1$，当$f(\boldsymbol{x})\neq h(\boldsymbol{x})$时，$\mathbb{I}(f(\boldsymbol{x}) \neq h(\boldsymbol{x}))=1$，$f(\boldsymbol{x}) h(\boldsymbol{x})=-1$

## 8.18

$$
h_{t}(\boldsymbol{x})=\underset{h}{\arg \min } \mathbb{E}_{\boldsymbol{x} \sim \mathcal{D}_{t}}[\mathbb{I}(f(\boldsymbol{x}) \neq h(\boldsymbol{x}))]
$$

[解析]：结合式(8.16)与(8.17)可知该式成立

## 8.19

$$
\begin{aligned}
\mathcal{D}_{t+1}(\boldsymbol{x}) &=\frac{\mathcal{D}(\boldsymbol{x}) e^{-f(\boldsymbol{x}) H_{t}(\boldsymbol{x})}}{\mathbb{E}_{\boldsymbol{x} \sim \mathcal{D}}\left[e^{-f(\boldsymbol{x}) H_{t}(\boldsymbol{x})}\right]} \\
&=\frac{\mathcal{D}(\boldsymbol{x}) e^{-f(\boldsymbol{x}) H_{t-1}(\boldsymbol{x})} e^{-f(\boldsymbol{x}) \alpha_{t} h_{t}(\boldsymbol{x})}}{\mathbb{E}_{\boldsymbol{x} \sim \mathcal{D}}\left[e^{-f(\boldsymbol{x}) H_{t}(\boldsymbol{x})}\right]} \\
&=\mathcal{D}_{t}(\boldsymbol{x}) \cdot e^{-f(\boldsymbol{x}) \alpha_{t} h_{t}(\boldsymbol{x})} \frac{\mathbb{E}_{\boldsymbol{x} \sim \mathcal{D}}\left[e^{-f(\boldsymbol{x}) H_{t-1}(\boldsymbol{x})}\right]}{\mathbb{E}_{\boldsymbol{x} \sim \mathcal{D}}\left[e^{-f(\boldsymbol{x}) H_{t}(\boldsymbol{x})}\right]}
\end{aligned}
$$

[解析]：boosting算法是根据调整后的样本再去训练下一个基分类器，这就是“重赋权法”的样本分布的调整公式


## 8.20

$$
H^{oob}(\boldsymbol{x})=\underset{y \in \mathcal{Y}}{\arg \max } \sum_{t=1}^{\mathrm{T}} \mathbb{I}\left(h_{t}(\boldsymbol{x})=y\right) \cdot \mathbb{I}\left(\boldsymbol{x} \notin D_{t}\right)
$$

[解析]：$\mathbb{I}\left(h_{t}(\boldsymbol{x})=y\right)$表示对$\mathrm{T}$个基学习器，每一个都判断结果是否与$y$一致，$y$的取值一般是$-1$和$1$，如果基学习器结果与$y$一致，则$\mathbb{I}\left(h_{t}(\boldsymbol{x})=y\right)=1$，如果样本不在训练集内，则$\mathbb{I}\left(\boldsymbol{x} \notin D_{t}\right)=1$，综合起来看就是，对包外的数据，用“投票法”选择包外估计的结果，即1或-1

## 8.21

$$
\epsilon^{o o b}=\frac{1}{|D|} \sum_{(\boldsymbol{x}, y) \in D} \mathbb{I}\left(H^{o o b}(\boldsymbol{x}) \neq y\right)
$$

[解析]：由8.20知，$H^{oob}(\boldsymbol{x})$是对包外的估计，该式表示估计错误的个数除以总的个数，得到泛化误差的包外估计

## 8.22

$$
H(\boldsymbol{x})=\frac{1}{T} \sum_{i=1}^{T} h_{i}(\boldsymbol{x})
$$

[解析]：对基分类器的结果进行简单的平均

## 8.23

$$
H(\boldsymbol{x})=\sum_{i=1}^{T} w_{i} h_{i}(\boldsymbol{x})
$$

[解析]：对基分类器的结果进行加权平均

## 8.24

$$
H(\boldsymbol{x})=\left\{\begin{array}{ll}
{c_{j},} & {\text { if } \sum_{i=1}^{T} h_{i}^{j}(\boldsymbol{x})>0.5 \sum_{k=1}^{N} \sum_{i=1}^{T} h_{i}^{k}(\boldsymbol{x})} \\
{\text { reject, }} & {\text { otherwise. }}
\end{array}\right.
$$

[解析]：当某一个类别$j$的基分类器的结果之和，大于所有结果之和的$\frac {1}{2}$，则选择该类别$j$为最终结果

## 8.25

$$
H(\boldsymbol{x})=c_{\underset{j}{ \arg \max} \sum_{i=1}^{T} h_{i}^{j}(\boldsymbol{x})}
$$

[解析]：相比于其他类别，该类别$j$的基分类器的结果之和最大，则选择类别$j$为最终结果

## 8.26

$$
H(\boldsymbol{x})=c_{\underset{j}{ \arg \max} \sum_{i=1}^{T} w_i h_{i}^{j}(\boldsymbol{x})}
$$

[解析]：相比于其他类别，该类别$j$的基分类器的结果之和最大，则选择类别$j$为最终结果，与式(8.25)不同的是，该式在基分类器前面乘上一个权重系数，该系数大于等于0，且T个权重之和为1

## 8.27

$$
A\left(h_{i} | \boldsymbol{x}\right)=\left(h_{i}(\boldsymbol{x})-H(\boldsymbol{x})\right)^{2}
$$

[解析]：该式表示个体学习器结果与预测结果的差值的平方，即为个体学习器的“分歧”

## 8.28

$$
\begin{aligned}
\bar{A}(h | \boldsymbol{x}) &=\sum_{i=1}^{T} w_{i} A\left(h_{i} | \boldsymbol{x}\right) \\
&=\sum_{i=1}^{T} w_{i}\left(h_{i}(\boldsymbol{x})-H(\boldsymbol{x})\right)^{2}
\end{aligned}
$$

[解析]：该式表示对各个个体学习器的“分歧”加权平均的结果，即集成的“分歧”

## 8.29

$$
E\left(h_{i} | \boldsymbol{x}\right)=\left(f(\boldsymbol{x})-h_{i}(\boldsymbol{x})\right)^{2}
$$

[解析]：该式表示个体学习器与真实值之间差值的平方，即个体学习器的平方误差

## 8.30

$$
E(H | \boldsymbol{x})=(f(\boldsymbol{x})-H(\boldsymbol{x}))^{2}
$$

[解析]：该式表示集成与真实值之间差值的平方，即集成的平方误差

## 8.31

$$
\bar{A}(h | \boldsymbol{x}) =\sum_{i=1}^{T} w_{i} E\left(h_{i} | \boldsymbol{x}\right)-E(H | \boldsymbol{x})
$$

[推导]：由(8.28)知
$$
\begin{aligned}
\bar{A}(h | \boldsymbol{x})&=\sum_{i=1}^{T} w_{i}\left(h_{i}(\boldsymbol{x})-H(\boldsymbol{x})\right)^{2}\\
&=\sum_{i=1}^{T} w_{i}(h_i(\boldsymbol{x})^2-2h_i(\boldsymbol{x})H(\boldsymbol{x})+H(\boldsymbol{x})^2)\\
&=\sum_{i=1}^{T} w_{i}h_i(\boldsymbol{x})^2-H(\boldsymbol{x})^2
\end{aligned}
$$

又因为
$$
\begin{aligned}
& \sum_{i=1}^{T} w_{i} E\left(h_{i} | \boldsymbol{x}\right)-E(H | \boldsymbol{x})\\
&=\sum_{i=1}^{T} w_{i}\left(f(\boldsymbol{x})-h_{i}(\boldsymbol{x})\right)^{2}-(f(\boldsymbol{x})-H(\boldsymbol{x}))^{2}\\
&=\sum_{i=1}^{T} w_{i}h_i(\boldsymbol{x})^2-H(\boldsymbol{x})^{2}
\end{aligned}
$$
所以
$$
\bar{A}(h | \boldsymbol{x}) =\sum_{i=1}^{T} w_{i} E\left(h_{i} | \boldsymbol{x}\right)-E(H | \boldsymbol{x})
$$

## 8.32

$$
\sum_{i=1}^{T} w_{i} \int A\left(h_{i} | \boldsymbol{x}\right) p(\boldsymbol{x}) d \boldsymbol{x}=\sum_{i=1}^{T} w_{i} \int E\left(h_{i} | \boldsymbol{x}\right) p(\boldsymbol{x}) d \boldsymbol{x}-\int E(H | \boldsymbol{x}) p(\boldsymbol{x}) d \boldsymbol{x}
$$

[解析]：$\int A\left(h_{i} | \boldsymbol{x}\right) p(\boldsymbol{x}) d \boldsymbol{x}$表示个体学习器在全样本上的“分歧”，$\sum_{i=1}^{T} w_{i} \int A\left(h_{i} | \boldsymbol{x}\right) p(\boldsymbol{x}) d \boldsymbol{x}$表示集成在全样本上的“分歧”，然后根据式(8.31)拆成误差的形式

## 8.33

$$
E_{i}=\int E\left(h_{i} | \boldsymbol{x}\right) p(\boldsymbol{x}) d \boldsymbol{x}
$$

[解析]：表示个体学习器在全样本上的泛化误差

## 8.34

$$
A_{i}=\int A\left(h_{i} | \boldsymbol{x}\right) p(\boldsymbol{x}) d \boldsymbol{x}
$$

[解析]：表示个体学习器在全样本上的分歧

## 8.35

$$
E=\int E(H | \boldsymbol{x}) p(\boldsymbol{x}) d \boldsymbol{x}
$$

[解析]：表示集成在全样本上的泛化误差

## 8.36

$$
E=\bar{E}-\bar{A}
$$

[解析]：$\bar{E}$表示个体学习器泛化误差的加权均值，$\bar{A}$表示个体学习器分歧项的加权均值，该式称为“误差-分歧分解”

## 8.37

$$
d i s_{i j}=\frac{b+c}{m}
$$

[解析]：这是不合度量的定义式，具体参考“西瓜书”

## 8.38

$$
\rho_{i j}=\frac{a d-b c}{\sqrt{(a+b)(a+c)(c+d)(b+d)}}
$$

[解析]：这是相关系数的定义式，具体参考“西瓜书”

## 8.39

$$
Q_{i j}=\frac{a d-b c}{a d+b c}
$$

[解析]：这是Q-统计量的定义式，具体参考“西瓜书”

## 8.40

$$
\kappa=\frac{p_{1}-p_{2}}{1-p_{2}}
$$

[解析]：这是$\kappa$-统计量的定义式，具体参考“西瓜书”

## 8.41

$$
p_{1}=\frac{a+d}{m}
$$

[解析]：这是两个个体学习器取得一致的概率，具体参考“西瓜书”

## 8.42

$$
p_{2}=\frac{(a+b)(a+c)+(c+d)(b+d)}{m^{2}}
$$

[解析]：这是两个个体学习器偶然达成一致的概率，具体参考“西瓜书”