```{important}
参与组队学习的同学须知：

本章学习时间：3天

本章配套视频教程：https://www.bilibili.com/video/BV1Mh411e7VU?p=8

本章配套代码：https://github.com/datawhalechina/machine-learning-toy-code/blob/main/%E8%A5%BF%E7%93%9C%E4%B9%A6%E4%BB%A3%E7%A0%81%E5%AE%9E%E6%88%98.md

本章配套代码视频教程：https://space.bilibili.com/431850986/lists/3884942
```

# 第5章 神经网络

神经网络类算法可以堪称当今最主流的一类机器学习算法，其本质上和前几章讲到的线性回归、对数几率回归、决策树等算法一样均属于机器学习算法，也是被发明用来完成分类和回归等任务。不过由于神经网络类算法在如今超强算力的加持下效果表现极其出色，且从理论角度来说神经网络层堆叠得越深其效果越好，因此也单独称用深层神经网络类算法所做的机器学习为深度学习，属于机器学习的子集。

## 5.1 神经元模型

本节对神经元模型的介绍通俗易懂，在此不再赘述。
本节第2段提到"阈值"(threshold)的概念时，"西瓜书"左侧边注特意强调是"阈(yù)"而不是"阀(fá)"，这是因为该字确实很容易认错，读者注意一下即可。

图5.1所示的M-P神经元模型，其中的"M-P"便是两位作者McCulloch和Pitts的首字母简写。

## 5.2 感知机与多层网络

### 5.2.1 式(5.1)和式(5.2)的推导

此式是感知机学习算法中的参数更新公式，下面依次给出感知机模型、学习策略和学习算法的具体介绍[1]：

**感知机模型**：已知感知机由两层神经元组成，故感知机模型的公式可表示为


$$
y=f\left(\sum\limits_{i=1}^{n}w_ix_i-\theta\right)=f(\boldsymbol{w}^{\mathrm{T}}\boldsymbol{x}-\theta)
$$


其中，$\boldsymbol{x}\in\mathbb{R}^n$，为样本的特征向量，是感知机模型的输入；$\boldsymbol{w},\theta$是感知机模型的参数，$\boldsymbol{w}\in\mathbb{R}^n$，为权重，$\theta$为阈值。假定$f$为阶跃函数，那么感知机模型的公式可进一步表示为 **（用$\varepsilon(\cdot)$代表阶跃函数）**


$$
y=\varepsilon(\boldsymbol{w}^{\mathrm{T}}\boldsymbol{x}-\theta)=\left\{\begin{array}{rcl}
1,& {\boldsymbol{w}^{\mathrm{T}}\boldsymbol{x} -\theta\geqslant 0};\\
0,& {\boldsymbol{w}^{\mathrm{T}}\boldsymbol{x} -\theta < 0}.\\
\end{array} \right.
$$

 由于$n$维空间中的超平面方程为


$$
w_1x_1+w_2x_2+\cdots+w_nx_n+b  =\boldsymbol{w}^{\mathrm{T}}\boldsymbol{x} +b=0
$$


所以此时感知机模型公式中的$\boldsymbol{w}^{\mathrm{T}}\boldsymbol{x}-\theta$可以看作是$n$维空间中的一个超平面，将$n$维空间划分为$\boldsymbol{w}^{\mathrm{T}}\boldsymbol{x}-\theta\geqslant 0$和$\boldsymbol{w}^{\mathrm{T}}\boldsymbol{x}-\theta<0$两个子空间，落在前一个子空间的样本对应的模型输出值为1，落在后一个子空间的样本对应的模型输出值为0，如此便实现了分类功能。

**感知机学习策略**：给定一个数据集


$$
T=\{(\boldsymbol{x}_1,y_1),(\boldsymbol{x}_2,y_2),\cdots,(\boldsymbol{x}_N,y_N)\}
$$


其中$\boldsymbol{x}_i\in\mathbb{R}^n,y_i\in\{0,1\},i=1,2,\cdots,N$。如果存在某个超平面


$$
\boldsymbol{w}^{\mathrm{T}}\boldsymbol{x}+b=0
$$


能将数据集$T$中的正样本和负样本完全正确地划分到超平面两侧，即对所有$y_i=1$的样本$\boldsymbol{x}_i$有$\boldsymbol{w}^{\mathrm{T}}\boldsymbol{x}_i+b\geqslant0$，对所有$y_i=0$的样本$\boldsymbol{x}_i$有$\boldsymbol{w}^{\mathrm{T}}\boldsymbol{x}_i+b<0$，则称数据集$T$线性可分，否则称数据集$T$线性不可分。

现给定一个线性可分的数据集$T$，感知机的学习目标是求得能对数据集$T$中的正负样本完全正确划分的分离超平面


$$
\boldsymbol{w}^{\mathrm{T}}\boldsymbol{x}-\theta=0
$$


假设此时误分类样本集合为$M\subseteq T$，对任意一个误分类样本$(\boldsymbol{x},y)\in M$来说，当$\boldsymbol{w}^\mathrm{T}\boldsymbol{x}-\theta\geqslant0$时，模型输出值为$\hat{y}=1$，样本真实标记为$y=0$；反之，当$\boldsymbol{w}^\mathrm{T}\boldsymbol{x}-\theta<0$时，模型输出值为$\hat{y}=0$，样本真实标记为$y=1$。综合两种情形可知，以下公式恒成立：


$$
(\hat{y}-y)\left(\boldsymbol{w}^\mathrm{T}\boldsymbol{x}-\theta\right)\geqslant0
$$


所以，给定数据集$T$，其损失函数可以定义为


$$
L(\boldsymbol{w},\theta)=\sum_{\boldsymbol{x}\in M}(\hat{y}-y)
\left(\boldsymbol{w}^\mathrm{T}\boldsymbol{x}-\theta\right)
$$


显然，此损失函数是非负的。如果没有误分类点，则损失函数值为0。而且，误分类点越少，误分类点离超平面越近（超平面相关知识参见本书6.1.2节），损失函数值就越小。因此，给定数据集$T$，损失函数$L(\boldsymbol{w},\theta)$是关于$\boldsymbol{w},\theta$的连续可导函数。

**感知机学习算法**：感知机模型的学习问题可以转化为求解损失函数的最优化问题，具体地，给定数据集


$$
T=\{(\boldsymbol{x}_1,y_1),(\boldsymbol{x}_2,y_2),\cdots,(\boldsymbol{x}_N,y_N)\}
$$


其中$\boldsymbol{x}_i \in \mathbb{R}^n,y_i \in\{0,1\}$，求参数$\boldsymbol{w},\theta$，使其为极小化损失函数的解：


$$
\min\limits_{\boldsymbol{w},\theta}L(\boldsymbol{w},\theta)=\min\limits_{\boldsymbol{w},\theta}\sum_{\boldsymbol{x_i}\in M}(\hat{y}_i-y_i)(\boldsymbol{w}^\mathrm{T}\boldsymbol{x}_i-\theta)
$$


其中$M\subseteq T$为误分类样本集合。若将阈值$\theta$看作一个固定输入为$-1$的"哑节点"，即


$$
-\theta=-1\cdot w_{n+1}=x_{n+1}\cdot w_{n+1}
$$


那么$\boldsymbol{w}^\mathrm{T}\boldsymbol{x}_i-\theta$可化简为


$$
\begin{aligned}
\boldsymbol{w}^\mathrm{T}\boldsymbol{x_i}-\theta&=\sum
\limits_{j=1}^n w_jx_j+x_{n+1}\cdot w_{n+1}\\ 
&=\sum\limits_{j=1}^{n+1}w_jx_j\\
&=\boldsymbol{w}^{\mathrm{T}}\boldsymbol{x_i}
\end{aligned}
$$


其中$\boldsymbol{x_i} \in \mathbb{R}^{n+1},\boldsymbol{w} \in
\mathbb{R}^{n+1}$。根据该公式，可将要求解的极小化问题进一步简化为


$$
\min\limits_{\boldsymbol{w}}L(\boldsymbol{w})=\min\limits_{\boldsymbol{w}}\sum_{\boldsymbol{x_i}\in M}(\hat{y}_i-y_i)\boldsymbol{w}^\mathrm{T}\boldsymbol{x_i}
$$


假设误分类样本集合$M$固定，那么可以求得损失函数$L(\boldsymbol{w})$的梯度


$$
\nabla_{\boldsymbol{w}}L(\boldsymbol{w})=\sum_{\boldsymbol{x_i}\in M}(\hat{y}_i-y_i)\boldsymbol{x_i}
$$


感知机的学习算法具体采用的是随机梯度下降法，即在极小化过程中，不是一次使$M$中所有误分类点的梯度下降，而是一次随机选取一个误分类点并使其梯度下降。
所以权重$\boldsymbol{w}$的更新公式为


$$
\boldsymbol w \leftarrow \boldsymbol w+\Delta \boldsymbol w
$$




$$
\Delta \boldsymbol w=-\eta(\hat{y}_i-y_i)\boldsymbol x_i=\eta(y_i-\hat{y}_i)\boldsymbol x_i
$$


相应地，$\boldsymbol{w}$中的某个分量$w_i$的更新公式即式(5.2)。

实践中常用的求解方法是先随机初始化一个模型权重$\boldsymbol{w}_{0}$，此时将训练集中的样本一一代入模型便可确定误分类点集合$M$，然后从$M$中随机抽选取一个误分类点计算得到$\Delta \boldsymbol{w}$，接着按照上述权重更新公式计算得到新的权重$\boldsymbol{w}_{1}=\boldsymbol{w}_{0}+\Delta \boldsymbol{w}$，并重新确定误分类点集合，如此迭代直至误分类点集合为空，即训练样本中的样本均完全正确分类。显然，随机初始化的$\boldsymbol{w}_{0}$不同，每次选取的误分类点不同，最后都有可能导致求解出的模型不同，因此感知模型的解不唯一。

### 5.2.2 图5.5的解释

图5.5中$(0,0),(0,1),(1,0),(1,1)$这4个样本点实现"异或"计算的过程如下：


$$
(x_1,x_2)\rightarrow h_1=\varepsilon(x_1-x_2-0.5),h_2=\varepsilon(x_2-x_1-0.5)\rightarrow y=\varepsilon(h_1+h_2-0.5)
$$


以$(0,1)$为例，首先求得$h_1=\varepsilon(0-1-0.5)=0,h_2=\varepsilon(1-0-0.5)=1$，然后求得$y=\varepsilon(0+1-0.5)=1$。

## 5.3 误差逆传播算法

### 5.3.1 式(5.10)的推导

参见式(5.12)的推导

### 5.3.2 式(5.12)的推导

因为 

$$
\Delta \theta_j = -\eta \cfrac{\partial E_k}{\partial \theta_j}
$$


又 

$$
\begin{aligned} 
\cfrac{\partial E_k}{\partial \theta_j} &= \cfrac{\partial E_k}{\partial \hat{y}_j^k} \cdot\cfrac{\partial \hat{y}_j^k}{\partial \theta_j} \\
&= \cfrac{\partial E_k}{\partial \hat{y}_j^k} \cdot\cfrac{\partial [f(\beta_j-\theta_j)]}{\partial \theta_j} \\
&=\cfrac{\partial E_k}{\partial \hat{y}_j^k} \cdot f^{\prime}(\beta_j-\theta_j) \times (-1) \\
&=\cfrac{\partial E_k}{\partial \hat{y}_j^k} \cdot f\left(\beta_{j}-\theta_{j}\right)\times\left[1-f\left(\beta_{j}-\theta_{j}\right)\right]  \times (-1) \\
&=\cfrac{\partial E_k}{\partial \hat{y}_j^k} \cdot \hat{y}_j^k\left(1-\hat{y}_j^k\right)  \times (-1) \\
&=\cfrac{\partial\left[ \cfrac{1}{2} \sum\limits_{j=1}^{l}\left(\hat{y}_{j}^{k}-y_{j}^{k}\right)^{2}\right]}{\partial \hat{y}_{j}^{k}} \cdot \hat{y}_j^k\left(1-\hat{y}_j^k\right) \times (-1)  \\
&=\cfrac{1}{2}\times 2(\hat{y}_j^k-y_j^k)\times 1 \cdot\hat{y}_j^k\left(1-\hat{y}_j^k\right)  \times (-1) \\
&=(y_j^k-\hat{y}_j^k)\hat{y}_j^k\left(1-\hat{y}_j^k\right)  \\
&= g_j
\end{aligned}
$$

 所以


$$
\Delta \theta_j = -\eta \cfrac{\partial E_k}{\partial \theta_j}=-\eta g_j
$$



### 5.3.3 式(5.13)的推导

因为 

$$
\Delta v_{ih} = -\eta \cfrac{\partial E_k}{\partial v_{ih}}
$$

 又


$$
\begin{aligned} 
\cfrac{\partial E_k}{\partial v_{ih}} &= \sum_{j=1}^{l} \cfrac{\partial E_k}{\partial \hat{y}_j^k} \cdot \cfrac{\partial \hat{y}_j^k}{\partial \beta_j} \cdot \cfrac{\partial \beta_j}{\partial b_h} \cdot \cfrac{\partial b_h}{\partial \alpha_h} \cdot \cfrac{\partial \alpha_h}{\partial v_{ih}} \\
&= \sum_{j=1}^{l} \cfrac{\partial E_k}{\partial \hat{y}_j^k} \cdot \cfrac{\partial \hat{y}_j^k}{\partial \beta_j} \cdot \cfrac{\partial \beta_j}{\partial b_h} \cdot \cfrac{\partial b_h}{\partial \alpha_h} \cdot x_i \\ 
&= \sum_{j=1}^{l} \cfrac{\partial E_k}{\partial \hat{y}_j^k} \cdot \cfrac{\partial \hat{y}_j^k}{\partial \beta_j} \cdot \cfrac{\partial \beta_j}{\partial b_h} \cdot f^{\prime}(\alpha_h-\gamma_h) \cdot x_i \\
&= \sum_{j=1}^{l} \cfrac{\partial E_k}{\partial \hat{y}_j^k} \cdot \cfrac{\partial \hat{y}_j^k}{\partial \beta_j} \cdot w_{hj} \cdot f^{\prime}(\alpha_h-\gamma_h) \cdot x_i \\
&= \sum_{j=1}^{l} (-g_j) \cdot w_{hj} \cdot f^{\prime}(\alpha_h-\gamma_h) \cdot x_i \\
&= -f^{\prime}(\alpha_h-\gamma_h) \cdot \sum_{j=1}^{l} g_j \cdot w_{hj}  \cdot x_i\\
&= -b_h(1-b_h) \cdot \sum_{j=1}^{l} g_j \cdot w_{hj}  \cdot x_i \\
&= -e_h \cdot x_i
\end{aligned}
$$

 所以


$$
\Delta v_{ih} =-\eta \cfrac{\partial E_k}{\partial v_{ih}} =\eta e_h x_i
$$



### 5.3.4 式(5.14)的推导

因为 

$$
\Delta \gamma_h = -\eta \cfrac{\partial E_k}{\partial \gamma_h}
$$


又 

$$
\begin{aligned} 
\cfrac{\partial E_k}{\partial \gamma_h} &= \sum_{j=1}^{l} \cfrac{\partial E_k}{\partial \hat{y}_j^k} \cdot \cfrac{\partial \hat{y}_j^k}{\partial \beta_j} \cdot \cfrac{\partial \beta_j}{\partial b_h} \cdot \cfrac{\partial b_h}{\partial \gamma_h} \\
&= \sum_{j=1}^{l} \cfrac{\partial E_k}{\partial \hat{y}_j^k} \cdot \cfrac{\partial \hat{y}_j^k}{\partial \beta_j} \cdot \cfrac{\partial \beta_j}{\partial b_h} \cdot f^{\prime}(\alpha_h-\gamma_h) \cdot (-1) \\
&= -\sum_{j=1}^{l} \cfrac{\partial E_k}{\partial \hat{y}_j^k} \cdot \cfrac{\partial \hat{y}_j^k}{\partial \beta_j} \cdot w_{hj} \cdot f^{\prime}(\alpha_h-\gamma_h)\\
&= -\sum_{j=1}^{l} \cfrac{\partial E_k}{\partial \hat{y}_j^k} \cdot \cfrac{\partial \hat{y}_j^k}{\partial \beta_j} \cdot w_{hj} \cdot b_h(1-b_h)\\
&= \sum_{j=1}^{l}g_j\cdot w_{hj} \cdot b_h(1-b_h)\\
&=e_h
\end{aligned}
$$

 所以


$$
\Delta \gamma_h=-\eta\cfrac{\partial E_k}{\partial \gamma_h} = -\eta e_h
$$



### 5.3.5 式(5.15)的推导

参见式(5.13)的推导

## 5.4 全局最小与局部极小

由图5.10可以直观理解局部极小和全局最小的概念，其余概念如模拟退火、遗传算法、启发式等，则需要查阅专业资料系统化学习。

## 5.5 其他常见神经网络

本节所提到的神经网络其实如今已不太常见，更为常见的神经网络是下一节深度学习里提到的卷积神经网络、循环神经网络等。

### 5.5.1 式(5.18)的解释

从式(5.18)可以看出，对于样本$\boldsymbol{x}$来说，RBF网络的输出为$q$个$\rho(\boldsymbol{x},\boldsymbol{c}_{i})$的线性组合。若换个角度来看这个问题，将$q$个$\rho(\boldsymbol{x},\boldsymbol{c}_{i})$当作是将$d$维向量$\boldsymbol{x}$基于式(5.19)进行特征转换后所得的$q$维特征，即$\tilde{\boldsymbol{x}}=\left(\rho(\boldsymbol{x},\boldsymbol{c}_{1});\rho(\boldsymbol{x},\boldsymbol{c}_{2});...;\rho(\boldsymbol{x},\boldsymbol{c}_{q})\right)$，则式(5.18)求线性加权系数$w_{i}$相当于求解第3.2节的线性回归$f(\tilde{\boldsymbol{x}})=\boldsymbol{w}^{\mathrm{T}}\tilde{\boldsymbol{x}}+b$，对于仅有的差别$b$来说，当然可以在式(5.18)中补加一个$b$。因此，RBF网络在确定$q$个神经元中心$\boldsymbol{c}_{i}$之后，接下来要做的就是线性回归。

### 5.5.2 式(5.20)的解释

Boltzmann机（Restricted Boltzmann
Machine，简称RBM）本质上是一个引入了隐变量的无向图模型，其能量可理解为


$$
E_{\rm graph}=E_{\rm edges}+E_{\rm nodes}
$$


其中，$E_{\rm graph}$表示图的能量，$E_{\rm edges}$表示图中边的能量，$E_{\rm nodes}$表示图中结点的能量。边能量由两连接结点的值及其权重的乘积确定，即$E_{{\rm edge}_{ij}}=-w_{ij}s_is_j$；结点能量由结点的值及其阈值的乘积确定，即$E_{{\rm node}_i}=-\theta_is_i$。图中边的能量为所有边能量之和为


$$
E_{\rm edges}=\sum_{i=1}^{n-1}\sum_{j=i+1}^{n}E_{{\rm edge}_{ij}}=-\sum_{i=1}^{n-1}\sum_{j=i+1}^{n}w_{ij}s_is_j
$$


图中结点的能量为所有结点能量之和


$$
E_{\rm nodes}=\sum_{i=1}^nE_{{\rm node}_i}=-\sum_{i=1}^n\theta_is_i
$$


故状态向量$\boldsymbol{s}$所对应的Boltzmann机能量


$$
E_{\rm graph}=E_{\rm edges}+E_{\rm nodes}=-\sum_{i=1}^{n-1}\sum_{j=i+1}^{n}w_{ij}s_is_j-\sum_{i=1}^n\theta_is_i
$$



### 5.5.3 式(5.22)的解释

受限Boltzmann机仅保留显层与隐层之间的连接。显层状态向量$\boldsymbol{v}=(v_1;v_2;...;v_d)$，隐层状态向量$\boldsymbol{h}=(h_1;h_2;...;h_q)$。显层状态向量$\boldsymbol{v}$中的变量$v_i$仅与隐层状态向量$\boldsymbol{h}$有关，所以给定隐层状态向量$\boldsymbol{h}$，有$v_1,v_2,...,v_d$相互独立。

### 5.5.4 式(5.23)的解释

由式(5.22)的解释同理可得，给定显层状态向量$\boldsymbol{v}$，有$h_1,h_2,...,h_q$相互独立。

## 5.6 深度学习

"西瓜书"在本节并未对如今深度学习领域的诸多经典神经网络作展开介绍，而是从更宏观的角度详细解释了应该如何理解深度学习。因此，本书也顺着"西瓜书"的思路对深度学习相关概念作进一步说明，对深度学习的经典神经网络感兴趣的读者可查阅其他相关书籍进行系统性学习。

### 5.6.1 什么是深度学习

深度学习就是很深层的神经网络，而神经网络属于机器学习算法的范畴，因此深度学习是机器学习的子集。

### 5.6.2 深度学习的起源

深度学习中的经典神经网络以及用于训练神经网络的BP算法其实在很早就已经被提出，例如卷积神经网络[2]是在1989提出，BP算法[3]是在1986年提出，但是在当时的计算机算力水平下，其他非神经网络类算法（例如当时红极一时的支持向量机算法）的效果优于神经网络类算法，因此神经网络类算法进入瓶颈期。随着计算机算力的不断提升，以及2012年Hinton和他的学生提出了AlexNet并在ImageNet评测中以明显优于第二名的成绩夺冠后，引起了学术界和工业界的广泛关注，紧接着三位深度学习之父LeCun、Bengio和Hinton在2015年正式提出深度学习的概念，自此深度学习开始成为机器学习的主流研究方向。

### 5.6.3 怎么理解特征学习

举例来说，用非深度学习算法做西瓜分类时，首先需要人工设计西瓜的各个特征，比如根蒂、色泽等，然后将其表示为数学向量，这些过程统称为"特征工程"，完成特征工程后用算法分类即可，其分类效果很大程度上取决于特征工程做得是否够好。而对于深度学习算法来说，只需将西瓜的图片表示为数学向量输入，输出层设置为想要的分类结果即可（例如二分类通常设置为对数几率回归），之前的"特征工程"交由神经网络来自动完成，即让神经网络进行"特征学习"，通过在输出层约束分类结果，神经网络会自动从西瓜的图片上提取出有助于西瓜分类的特征。

因此，如果分别用对数几率回归和卷积神经网络来做西瓜分类，其算法运行流程分别是"人工特征工程$\rightarrow$对数几率回归分类"和"卷积神经网络特征学习$\rightarrow$对数几率回归分类"。

## 参考文献
[1] 李航. 统计学习方法. 清华大学出版社, 2012.

[2] Yann LeCun, Bernhard Boser, John S Denker, Donnie Henderson, Richard E Howard, Wayne Hub-
bard, and Lawrence D Jackel. Backpropagation applied to handwritten zip code recognition. Neural
computation, 1(4):541–551, 1989.

[3] David E Rumelhart, Geoffrey E Hinton, and Ronald J Williams. Learning representations by back-
propagating errors. nature, 323(6088):533–536, 1986.
