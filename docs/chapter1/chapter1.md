## 1.2
$$\begin{aligned}
\sum_{f}E_{ote}(\mathfrak{L}_a\vert X,f) &= \sum_f\sum_h\sum_{x\in\mathcal{X}-X}P(x)\mathbb{I}(h(x)\neq f(x))P(h\vert X,\mathfrak{L}_a) \\
&=\sum_{x\in\mathcal{X}-X}P(x) \sum_hP(h\vert X,\mathfrak{L}_a)\sum_f\mathbb{I}(h(x)\neq f(x)) \\
&=\sum_{x\in\mathcal{X}-X}P(x) \sum_hP(h\vert X,\mathfrak{L}_a)\cfrac{1}{2}2^{\vert \mathcal{X} \vert} \\
&=\cfrac{1}{2}2^{\vert \mathcal{X} \vert}\sum_{x\in\mathcal{X}-X}P(x) \sum_hP(h\vert X,\mathfrak{L}_a) \\
&=2^{\vert \mathcal{X} \vert-1}\sum_{x\in\mathcal{X}-X}P(x) \cdot 1\\
\end{aligned}$$

[解析]：第一步到第二步是因为$\sum_i^m\sum_j^n\sum_k^o a_ib_jc_k=\sum_i^m a_i \cdot \sum_j^n b_j \cdot \sum_k^o c_k$；
第二步到第三步：首先要知道此时$f$的定义为**任何能将样本映射到{0,1}的函数+均匀分布**，也即不止一个$f$且每个$f$出现的概率相等，例如样本空间只有两个样本时：$ \mathcal{X}=\{x_1,x_2\},\vert \mathcal{X} \vert=2$，那么所有的真实目标函数$f$为：
$$\begin{aligned}
f_1:f_1(x_1)=0,f_1(x_2)=0;\\
f_2:f_2(x_1)=0,f_2(x_2)=1;\\
f_3:f_3(x_1)=1,f_3(x_2)=0;\\
f_4:f_4(x_1)=1,f_4(x_2)=1;
\end{aligned}$$
一共$2^{\vert \mathcal{X} \vert}=2^2=4$个真实目标函数。所以此时通过算法$\mathfrak{L}_a$学习出来的模型$h(x)$对每个样本无论预测值为0还是1必然有一半的$f$与之预测值相等，所以$\sum_f\mathbb{I}(h(x)\neq f(x)) = \cfrac{1}{2}2^{\vert \mathcal{X} \vert} $；
第三步一直到最后有点概率论的基础应该都能看懂了。