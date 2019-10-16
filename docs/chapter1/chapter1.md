## 1.2
$$\begin{aligned}
\sum_{f}E_{ote}(\mathfrak{L}_a\vert X,f) &= \sum_f\sum_h\sum_{x\in\mathcal{X}-X}P(x)\mathbb{I}(h(x)\neq f(x))P(h\vert X,\mathfrak{L}_a) \\
&=\sum_{x\in\mathcal{X}-X}P(x) \sum_hP(h\vert X,\mathfrak{L}_a)\sum_f\mathbb{I}(h(x)\neq f(x)) \\
&=\sum_{x\in\mathcal{X}-X}P(x) \sum_hP(h\vert X,\mathfrak{L}_a)\cfrac{1}{2}2^{\vert \mathcal{X} \vert} \\
&=\cfrac{1}{2}2^{\vert \mathcal{X} \vert}\sum_{x\in\mathcal{X}-X}P(x) \sum_hP(h\vert X,\mathfrak{L}_a) \\
&=2^{\vert \mathcal{X} \vert-1}\sum_{x\in\mathcal{X}-X}P(x) \cdot 1\\
\end{aligned}$$

[解析]：第一步到第二步是因为$\sum_i^m\sum_j^n\sum_k^o a_ib_jc_k=\sum_i^m a_i \cdot \sum_j^n b_j \cdot \sum_k^o c_k$；第二步到第三步：首先要知道此时$f$的定义为**任何能将样本映射到{0,1}的函数+均匀分布**，也即不止一个$f$且每个$f$出现的概率相等，例如样本空间只有两个样本时：$ \mathcal{X}=\{x_1,x_2\},\vert \mathcal{X} \vert=2$，那么所有的真实目标函数$f$为：
$$\begin{aligned}
f_1:f_1(x_1)=0,f_1(x_2)=0;\\
f_2:f_2(x_1)=0,f_2(x_2)=1;\\
f_3:f_3(x_1)=1,f_3(x_2)=0;\\
f_4:f_4(x_1)=1,f_4(x_2)=1;
\end{aligned}$$
一共$2^{\vert \mathcal{X} \vert}=2^2=4$个真实目标函数。所以此时通过算法$\mathfrak{L}_a$学习出来的模型$h(x)$对每个样本无论预测值为0还是1必然有一半的$f$与之预测值相等，例如，现在学出来的模型$h(x)$对$x_1$的预测值为1，也即$h(x_1)=1$，那么有且只有$f_3$和$f_4$与$h(x)$的预测值相等，也就是有且只有一半的$f$与它预测值相等，所以$\sum_f\mathbb{I}(h(x)\neq f(x)) = \cfrac{1}{2}2^{\vert \mathcal{X} \vert} $；第三步一直到最后显然成立。值得一提的是，在这里我们定义真实的目标函数为**“任何能将样本映射到{0,1}的函数+均匀分布”**，但是实际情形并非如此，通常我们只认为能高度拟合已有样本数据的函数才是真实目标函数，例如，现在已有的样本数据为$\{(x_1,0),(x_2,1)\}$，那么此时$f_2$才是我们认为的真实目标函数，由于没有收集到或者压根不存在$\{(x_1,0),(x_2,0)\},\{(x_1,1),(x_2,0)\},\{(x_1,1),(x_2,1)\}$这类样本，所以$f_1,f_3,f_4$都不算是真实目标函数。