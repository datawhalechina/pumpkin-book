```{important}
参与组队学习的同学须知：

本章学习时间：线性回归3天+对数几率回归3天+线性判别分析3天，共计9天

本章配套视频教程：

一元线性回归：https://www.bilibili.com/video/BV1Mh411e7VU?p=3

多元线性回归：https://www.bilibili.com/video/BV1Mh411e7VU?p=4

对数几率回归：https://www.bilibili.com/video/BV1Mh411e7VU?p=5

线性判别分析：https://www.bilibili.com/video/BV1Mh411e7VU?p=6

本章配套代码：https://github.com/datawhalechina/machine-learning-toy-code/blob/main/%E8%A5%BF%E7%93%9C%E4%B9%A6%E4%BB%A3%E7%A0%81%E5%AE%9E%E6%88%98.md

本章配套代码视频教程：https://space.bilibili.com/431850986/lists/3884942
```

# 第3章 线性模型

作为"西瓜书"介绍机器学习模型的开篇，线性模型也是机器学习中最为基础的模型，很多复杂模型均可认为由线性模型衍生而得，无论是曾经红极一时的支持向量机还是如今万众瞩目的神经网络，其中都有线性模型的影子。

本章的线性回归和对数几率回归分别是回归和分类任务上常用的算法，因此属于重点内容，线性判别分析不常用，但是其核心思路和后续第10章将会讲到的经典降维算法主成分分析相同，因此也属于重点内容，且两者结合在一起看理解会更深刻。

## 3.1 基本形式

第1章的1.2基本术语中讲述样本的定义时，我们说明了"西瓜书"和本书中向量的写法，当向量中的元素用分号";"分隔时表示此向量为列向量，用逗号","分隔时表示为行向量。因此，式(3.2)中$\boldsymbol{w}=(w_{1};w_{2};...;w_{d})$和$\boldsymbol{x}=(x_{1};x_{2};...;x_{d})$均为$d$行1列的列向量。

## 3.2 线性回归

### 3.2.1 属性数值化

为了能进行数学运算，样本中的非数值类属性都需要进行数值化。对于存在"序"关系的属性，可通过连续化将其转化为带有相对大小关系的连续值；对于不存在"序"关系的属性，可根据属性取值将其拆解为多个属性，例如"西瓜书"中所说的"瓜类"属性，可将其拆解为"是否是西瓜"、"是否是南瓜"、"是否是黄瓜"3个属性，其中每个属性的取值为1或0，1表示"是"，0表示"否"。具体地，假如现有3个瓜类样本：$\boldsymbol{x}_1=(\text{甜度}=\text{高};\text{瓜类}=\text{西瓜}),\boldsymbol{x}_2=(\text{甜度}=\text{中};\text{瓜类}=\text{南瓜}),\boldsymbol{x}_3=(\text{甜度}=\text{低};\text{瓜类}=\text{黄瓜})$，其中"甜度"属性存在序关系，因此可将"高"、"中"、"低"转化为$\{1.0,0.5,0.0\}$，"瓜类"属性不存在序关系，则按照上述方法进行拆解，3个瓜类样本数值化后的结果为：$\boldsymbol{x}_1=(1.0;1;0;0),\boldsymbol{x}_1=(0.5;0;1;0),\boldsymbol{x}_1=(0.0;0;0;1)$。

以上针对样本属性所进行的处理工作便是第1章1.2基本术语中提到的"特征工程"范畴，完成属性数值化以后通常还会进行缺失值处理、规范化、降维等一系列处理工作。由于特征工程属于算法实践过程中需要掌握的内容，待学完机器学习算法以后，再进一步学习特征工程相关知识即可，在此先不展开。

### 3.2.2 式(3.4)的解释

下面仅针对式(3.4)中的数学符号进行解释。首先解释一下符号"$\arg \min$"，其中"arg"是"argument"（参数）的前三个字母，"min"
是"minimum"（最小值）的前三个字母，该符号表示求使目标函数达到最小值的参数取值。例如式(3.4)表示求出使目标函数$\sum_{i=1}^{m}\left(y_{i}-w x_{i}-b\right)^{2}$达到最小值的参数取值$(w^{*},b^{*})$，注意目标函数是以$(w,b)$为自变量的函数，$(x_{i},y_{i})$均是已知常量，即训练集中的样本数据。

类似的符号还有"$\min$"，例如将式(3.4)改为

$$
\underset{(w,b)}{\min}\sum_{i=1}^{m}\left(y_{i}-w x_{i}-b\right)^{2}
$$

则表示求目标函数的最小值。对比知道，"$\min$"和"$\arg \min$"的区别在于，前者输出目标函数的最小值，而后者输出使得目标函数达到最小值时的参数取值。

若进一步修改式(3.4)为

$$
\begin{aligned}
\underset{(w,b)}{\min} & \sum_{i=1}^{m}\left(y_{i}-w x_{i}-b\right)^{2} \\ 
\text{s.t.} & w > 0, \\
& b < 0.
\end{aligned}
$$

则表示在$w>0,b<0$范围内寻找目标函数的最小值，"$\text{s.t.}$"是"subject
to"的简写，意思是"受约束于"，即为约束条件。

以上介绍的符号都是应用数学领域的一个分支------"最优化"中的内容，若想进一步了解可找一本最优化的教材（例如参考文献[1]）进行系统性地学习。

### 3.2.3 式(3.5)的推导

"西瓜书"在式(3.5)左侧给出的凸函数的定义是最优化中的定义，与高等数学中的定义不同，本书也默认采用此种定义。由于一元线性回归可以看作是多元线性回归中元的个数为1时的情形，所以此处暂不给出$E_{(w, b)}$是关于$w$和$b$的凸函数的证明，在推导式(3.11)时一并给出，下面开始推导式(3.5)。

已知$E_{(w, b)}=\sum\limits_{i=1}^{m}\left(y_{i}-w x_{i}-b\right)^{2}$，所以

$$
\begin{aligned}
\frac{\partial E_{(w, b)}}{\partial w}&=\frac{\partial}{\partial w} \left[\sum_{i=1}^{m}\left(y_{i}-w x_{i}-b\right)^{2}\right] \\
&= \sum_{i=1}^{m}\frac{\partial}{\partial w} \left[\left(y_{i}-w x_{i}-b\right)^{2}\right] \\
&= \sum_{i=1}^{m}\left[2\cdot\left(y_{i}-w x_{i}-b\right)\cdot (-x_i)\right] \\
&= \sum_{i=1}^{m}\left[2\cdot\left(w x_{i}^2-y_i x_i +bx_i\right)\right] \\
&= 2\cdot\left(w\sum_{i=1}^{m} x_{i}^2-\sum_{i=1}^{m}y_i x_i +b\sum_{i=1}^{m}x_i\right) \\
&=2\left(w \sum_{i=1}^{m} x_{i}^{2}-\sum_{i=1}^{m}\left(y_{i}-b\right) x_{i}\right)
\end{aligned}
$$


### 3.2.4 式(3.6)的推导

已知$E_{(w, b)}=\sum\limits_{i=1}^{m}\left(y_{i}-w x_{i}-b\right)^{2}$，所以

$$
\begin{aligned}
\frac{\partial E_{(w, b)}}{\partial b}&=\frac{\partial}{\partial b} \left[\sum_{i=1}^{m}\left(y_{i}-w x_{i}-b\right)^{2}\right] \\
&=\sum_{i=1}^{m}\frac{\partial}{\partial b} \left[\left(y_{i}-w x_{i}-b\right)^{2}\right] \\
&=\sum_{i=1}^{m}\left[2\cdot\left(y_{i}-w x_{i}-b\right)\cdot (-1)\right] \\
&=\sum_{i=1}^{m}\left[2\cdot\left(b-y_{i}+w x_{i}\right)\right] \\
&=2\cdot\left[\sum_{i=1}^{m}b-\sum_{i=1}^{m}y_{i}+\sum_{i=1}^{m}w x_{i}\right] \\
&=2\left(m b-\sum_{i=1}^{m}\left(y_{i}-w x_{i}\right)\right)
\end{aligned}
$$


### 3.2.5 式(3.7)的推导

推导之前先重点说明一下"闭式解"或称为"解析解"。闭式解是指可以通过具体的表达式解出待解参数，例如可根据式(3.7)直接解得$w$。机器学习算法很少有闭式解，线性回归是一个特例，接下来推导式(3.7)。

令式(3.5)等于0

$$
0 = w\sum_{i=1}^{m}x_i^2-\sum_{i=1}^{m}(y_i-b)x_i
$$


$$
w\sum_{i=1}^{m}x_i^2 = \sum_{i=1}^{m}y_ix_i-\sum_{i=1}^{m}bx_i
$$

由于令式(3.6)等于0可得$b=\frac{1}{m}\sum_{i=1}^{m}(y_i-wx_i)$，又因为$\frac{1}{m}\sum_{i=1}^{m}y_i=\bar{y}$，$\frac{1}{m}\sum_{i=1}^{m}x_i=\bar{x}$，则$b=\bar{y}-w\bar{x}$，代入上式可得

$$
\begin{aligned}    
w\sum_{i=1}^{m}x_i^2 & = \sum_{i=1}^{m}y_ix_i-\sum_{i=1}^{m}(\bar{y}-w\bar{x})x_i \\
w\sum_{i=1}^{m}x_i^2 & = \sum_{i=1}^{m}y_ix_i-\bar{y}\sum_{i=1}^{m}x_i+w\bar{x}\sum_{i=1}^{m}x_i \\
w(\sum_{i=1}^{m}x_i^2-\bar{x}\sum_{i=1}^{m}x_i) & = \sum_{i=1}^{m}y_ix_i-\bar{y}\sum_{i=1}^{m}x_i \\
w & = \frac{\sum_{i=1}^{m}y_ix_i-\bar{y}\sum_{i=1}^{m}x_i}{\sum_{i=1}^{m}x_i^2-\bar{x}\sum_{i=1}^{m}x_i}
\end{aligned}
$$

将$\bar{y}\sum_{i=1}^{m}x_i=\frac{1}{m}\sum_{i=1}^{m}y_i\sum_{i=1}^{m}x_i=\bar{x}\sum_{i=1}^{m}y_i$和$\bar{x}\sum_{i=1}^{m}x_i=\frac{1}{m}\sum_{i=1}^{m}x_i\sum_{i=1}^{m}x_i=\frac{1}{m}(\sum_{i=1}^{m}x_i)^2$代入上式，即可得式(3.7)：

$$
w=\frac{\sum_{i=1}^{m}y_i(x_i-\bar{x})}{\sum_{i=1}^{m}x_i^2-\frac{1}{m}(\sum_{i=1}^{m}x_i)^2}
$$

如果要想用Python来实现上式的话，上式中的求和运算只能用循环来实现。但是如果能将上式向量化，也就是转换成矩阵（即向量）运算的话，我们就可以利用诸如NumPy这种专门加速矩阵运算的类库来进行编写。下面我们就尝试将上式进行向量化。

将$\frac{1}{m}(\sum_{i=1}^{m}x_i)^2=\bar{x}\sum_{i=1}^{m}x_i$代入分母可得

$$
\begin{aligned}     
w & = \frac{\sum_{i=1}^{m}y_i(x_i-\bar{x})}{\sum_{i=1}^{m}x_i^2-\bar{x}\sum_{i=1}^{m}x_i} \\
& = \frac{\sum_{i=1}^{m}(y_ix_i-y_i\bar{x})}{\sum_{i=1}^{m}(x_i^2-x_i\bar{x})}
\end{aligned}
$$

又因为$\bar{y}\sum_{i=1}^{m}x_i=\bar{x}\sum_{i=1}^{m}y_i=\sum_{i=1}^{m}\bar{y}x_i=\sum_{i=1}^{m}\bar{x}y_i=m\bar{x}\bar{y}=\sum_{i=1}^{m}\bar{x}\bar{y}$且$\sum_{i=1}^{m}x_i\bar{x}=\bar{x}\sum_{i=1}^{m}x_i=\bar{x}\cdot m \cdot\frac{1}{m}\cdot\sum_{i=1}^{m}x_i=m\bar{x}^2=\sum_{i=1}^{m}\bar{x}^2$，则有

$$
\begin{aligned}
w & = \frac{\sum_{i=1}^{m}(y_ix_i-y_i\bar{x}-x_i\bar{y}+\bar{x}\bar{y})}{\sum_{i=1}^{m}(x_i^2-x_i\bar{x}-x_i\bar{x}+\bar{x}^2)} \\
& = \frac{\sum_{i=1}^{m}(x_i-\bar{x})(y_i-\bar{y})}{\sum_{i=1}^{m}(x_i-\bar{x})^2} 
\end{aligned}
$$

若令$\boldsymbol{x}=(x_1;x_2;...;x_m)$，$\boldsymbol{x}_{d}=(x_1-\bar{x};x_2-\bar{x};...;x_m-\bar{x})$为去均值后的$\boldsymbol{x}$；$\boldsymbol{y}=(y_1;y_2;...;y_m)$，$\boldsymbol{y}_{d}=(y_1-\bar{y};y_2-\bar{y};...;y_m-\bar{y})$为去均值后的$\boldsymbol{y}$， **（$\boldsymbol{x}$、$\boldsymbol{x}_{d}$、$\boldsymbol{y}$、$\boldsymbol{y}_{d}$均为m行1列的列向量）** 代入上式可得

$$
w=\frac{\boldsymbol{x}_{d}^\mathrm{T}\boldsymbol{y}_{d}}{\boldsymbol{x}_d^\mathrm{T}\boldsymbol{x}_{d}}
$$


### 3.2.6 式(3.9)的推导

式(3.4)是最小二乘法运用在一元线性回归上的情形，那么对于多元线性回归来说，我们可以类似得到

$$
\begin{aligned}
    \left(\boldsymbol{w}^{*}, b^{*}\right)&=\underset{(\boldsymbol{w}, b)}{\arg \min } \sum_{i=1}^{m}\left(f\left(\boldsymbol{x}_{i}\right)-y_{i}\right)^{2} \\
    &=\underset{(\boldsymbol{w}, b)}{\arg \min } \sum_{i=1}^{m}\left(y_{i}-f\left(\boldsymbol{x}_{i}\right)\right)^{2}\\
    &=\underset{(\boldsymbol{w}, b)}{\arg \min } \sum_{i=1}^{m}\left(y_{i}-\left(\boldsymbol{w}^\mathrm{T}\boldsymbol{x}_{i}+b\right)\right)^{2}
\end{aligned}
$$

为便于讨论，我们令$\hat{\boldsymbol{w}}=(\boldsymbol{w};b)=(w_1;...;w_d;b)\in\mathbb{R}^{(d+1)\times 1},\hat{\boldsymbol{x}}_i=(x_{i1};...;x_{id};1)\in\mathbb{R}^{(d+1)\times 1}$，那么上式可以简化为

$$
\begin{aligned}
    \hat{\boldsymbol{w}}^{*}&=\underset{\hat{\boldsymbol{w}}}{\arg \min } \sum_{i=1}^{m}\left(y_{i}-\hat{\boldsymbol{w}}^\mathrm{T}\hat{\boldsymbol{x}}_{i}\right)^{2} \\
    &=\underset{\hat{\boldsymbol{w}}}{\arg \min } \sum_{i=1}^{m}\left(y_{i}-\hat{\boldsymbol{x}}_{i}^\mathrm{T}\hat{\boldsymbol{w}}\right)^{2} \\
\end{aligned}
$$

根据向量内积的定义可知，上式可以写成如下向量内积的形式

$$
\begin{aligned}
    \hat{\boldsymbol{w}}^{*}&=\underset{\hat{\boldsymbol{w}}}{\arg \min } \begin{bmatrix}
    y_{1}-\hat{\boldsymbol{x}}_{1}^\mathrm{T}\hat{\boldsymbol{w}} & \cdots & y_{m}-\hat{\boldsymbol{x}}_{m}^\mathrm{T}\hat{\boldsymbol{w}} \\
    \end{bmatrix}
    \begin{bmatrix}
    y_{1}-\hat{\boldsymbol{x}}_{1}^\mathrm{T}\hat{\boldsymbol{w}} \\
    \vdots \\
    y_{m}-\hat{\boldsymbol{x}}_{m}^\mathrm{T}\hat{\boldsymbol{w}}
    \end{bmatrix} \\
\end{aligned}
$$

其中 

$$
\begin{aligned}
\begin{bmatrix}
    y_{1}-\hat{\boldsymbol{x}}_{1}^\mathrm{T}\hat{\boldsymbol{w}} \\
    \vdots \\
    y_{m}-\hat{\boldsymbol{x}}_{m}^\mathrm{T}\hat{\boldsymbol{w}}
\end{bmatrix}&=\begin{bmatrix}
    y_{1} \\
    \vdots \\
    y_{m}
\end{bmatrix}-\begin{bmatrix}
    \hat{\boldsymbol{x}}_{1}^\mathrm{T}\hat{\boldsymbol{w}} \\
    \vdots \\
    \hat{\boldsymbol{x}}_{m}^\mathrm{T}\hat{\boldsymbol{w}}
\end{bmatrix}\\
&=\boldsymbol{y}-\begin{bmatrix}
    \hat{\boldsymbol{x}}_{1}^\mathrm{T} \\
    \vdots \\
    \hat{\boldsymbol{x}}_{m}^\mathrm{T}
\end{bmatrix}\cdot\hat{\boldsymbol{w}}\\
&=\boldsymbol{y}-\mathbf{X}\hat{\boldsymbol{w}}
\end{aligned}
$$

所以

$$
\hat{\boldsymbol{w}}^{*}=\underset{\hat{\boldsymbol{w}}}{\arg \min }(\boldsymbol{y}-\mathbf{X} \hat{\boldsymbol{w}})^{\mathrm{T}}(\boldsymbol{y}-\mathbf{X} \hat{\boldsymbol{w}})
$$


### 3.2.7 式(3.10)的推导

将$E_{\hat{\boldsymbol w}}=(\boldsymbol{y}-\mathbf{X}\hat{\boldsymbol w})^{\mathrm{T}}(\boldsymbol{y}-\mathbf{X}\hat{\boldsymbol w})$展开可得

$$
E_{\hat{\boldsymbol w}}= \boldsymbol{y}^{\mathrm{T}}\boldsymbol{y}-\boldsymbol{y}^{\mathrm{T}}\mathbf{X}\hat{\boldsymbol w}-\hat{\boldsymbol w}^{\mathrm{T}}\mathbf{X}^{\mathrm{T}}\boldsymbol{y}+\hat{\boldsymbol w}^{\mathrm{T}}\mathbf{X}^{\mathrm{T}}\mathbf{X}\hat{\boldsymbol w}
$$

对$\hat{\boldsymbol w}$求导可得

$$
\frac{\partial E_{\hat{\boldsymbol w}}}{\partial \hat{\boldsymbol w}}= \frac{\partial \boldsymbol{y}^{\mathrm{T}}\boldsymbol{y}}{\partial \hat{\boldsymbol w}}-\frac{\partial \boldsymbol{y}^{\mathrm{T}}\mathbf{X}\hat{\boldsymbol w}}{\partial \hat{\boldsymbol w}}-\frac{\partial \hat{\boldsymbol w}^{\mathrm{T}}\mathbf{X}^{\mathrm{T}}\boldsymbol{y}}{\partial \hat{\boldsymbol w}}+\frac{\partial \hat{\boldsymbol w}^{\mathrm{T}}\mathbf{X}^{\mathrm{T}}\mathbf{X}\hat{\boldsymbol w}}{\partial \hat{\boldsymbol w}}
$$

由矩阵微分公式$\frac{\partial\boldsymbol{a}^{\mathrm{T}}\boldsymbol{x}}{\partial\boldsymbol{x}}=\frac{\partial\boldsymbol{x}^{\mathrm{T}}\boldsymbol{a}}{\partial\boldsymbol{x}}=\boldsymbol{a},\frac{\partial\boldsymbol{x}^{\mathrm{T}}\mathbf{A}\boldsymbol{x}}{\partial\boldsymbol{x}}=(\mathbf{A}+\mathbf{A}^{\mathrm{T}})\boldsymbol{x}$ **（更多矩阵微分公式可查阅[2]，矩阵微分原理可查阅[3]）** 可得

$$
\begin{aligned}
\frac{\partial E_{\hat{\boldsymbol w}}}{\partial \hat{\boldsymbol w}} &= 0-\mathbf{X}^{\mathrm{T}}\boldsymbol{y}-\mathbf{X}^{\mathrm{T}}\boldsymbol{y}+(\mathbf{X}^{\mathrm{T}}\mathbf{X}+\mathbf{X}^{\mathrm{T}}\mathbf{X})\hat{\boldsymbol w} \\
&=2\mathbf{X}^{\mathrm{T}}(\mathbf{X}\hat{\boldsymbol w}-\boldsymbol{y})
\end{aligned}
$$


### 3.2.8 式(3.11)的推导

首先铺垫讲解接下来以及后续内容将会用到的多元函数相关基础知识[1]。

**$n$元实值函数**：含$n$个自变量，值域为实数域$\mathbb{R}$的函数称为$n$元实值函数，记为$f(\boldsymbol{x})$，其中$\boldsymbol{x}=(x_{1};x_{2};...;x_{n})$为$n$维向量。"西瓜书"和本书中的多元函数未加特殊说明均为实值函数。

**凸集**：设集合$D\subset\mathbb{R}^n$为$n$维欧式空间中的子集，如果对$D$中任意的$n$维向量$\boldsymbol{x}\in D$和$\boldsymbol{y}\in D$与任意的$\alpha\in[0,1]$，有

$$
\alpha\boldsymbol{x}+(1-\alpha)\boldsymbol{y}\in D
$$

则称集合$D$是凸集。凸集的几何意义是：若两个点属于此集合，则这两点连线上的任意一点均属于此集合。常见的凸集有空集$\varnothing$，整个$n$维欧式空间$\mathbb{R}^n$。

**凸函数**：设$D\subset\mathbb{R}^n$是非空凸集，$f$是定义在$D$上的函数，如果对任意的$\boldsymbol{x}^1,\boldsymbol{x}^2\in D,\alpha\in(0,1)$，均有

$$
f\left(\alpha\boldsymbol{x}^1+(1-\alpha)\boldsymbol{x}^2\right)\leqslant \alpha f(\boldsymbol{x}^1)+(1-\alpha)f(\boldsymbol{x}^2)
$$

则称$f$为$D$上的凸函数。若其中的$\leqslant$改为$<$也恒成立，则称$f$为$D$上的严格凸函数。

**梯度**：若$n$元函数$f(\boldsymbol{x})$对$\boldsymbol{x}=(x_{1};x_{2};...;x_{n})$中各分量$x_{i}$的偏导数$\frac{\partial f(\boldsymbol{x})}{\partial x_{i}}(i=1,2,...,n)$都存在，则称函数$f(\boldsymbol{x})$在$\boldsymbol{x}$处一阶可导，并称以下列向量

$$
\nabla f(\boldsymbol{x})=\frac{\partial f(\boldsymbol{x})}{\partial \boldsymbol{x}}=\left[ \begin{array}{c}{\frac{\partial f(\boldsymbol{x})}{\partial x_{1}}} \\ {\frac{\partial f(\boldsymbol{x})}{\partial x_{2}}} \\ {\vdots} \\ {\frac{\partial f(\boldsymbol{x})}{\partial x_{n}}}\end{array}\right]
$$

为函数$f(\boldsymbol{x})$在$\boldsymbol{x}$处的一阶导数或梯度，易证梯度指向的方向是函数值增大速度最快的方向。$\nabla f(\boldsymbol{x})$也可写成行向量形式

$$
\nabla f(\boldsymbol{x})=\frac{\partial f(\boldsymbol{x})}{\partial \boldsymbol{x}^\mathrm{T}}=\left[{\frac{\partial f(\boldsymbol{x})}{\partial x_{1}}}, {\frac{\partial f(\boldsymbol{x})}{\partial x_{2}}},{\cdots},{\frac{\partial f(\boldsymbol{x})}{\partial x_{n}}}\right]
$$

我们称列向量形式为"分母布局"，行向量形式为"分子布局"，由于在最优化中习惯采用分母布局，因此"西瓜书"以及本书中也采用分母布局。为了便于区分当前采用何种布局，通常在采用分母布局时偏导符号$\partial$后接的是$\boldsymbol{x}$，采用分子布局时后接的是$\boldsymbol{x}^\mathrm{T}$。

**Hessian矩阵**：若$n$元函数$f(\boldsymbol{x})$对$\boldsymbol{x}=(x_{1};x_{2};...;x_{n})$中各分量$x_{i}$的二阶偏导数$\frac{\partial^{2} f(\boldsymbol{x})}{\partial x_{i} \partial x_{j}}(i=1,2,...,n;j=1,2,...,n)$都存在，则称函数$f(\boldsymbol{x})$在$\boldsymbol{x}$处二阶阶可导，并称以下矩阵

$$
\nabla^2f(\boldsymbol x)=\frac{\partial^{2}f(\boldsymbol{x})}{\partial\boldsymbol{x}\partial\boldsymbol{x}^\mathrm{T}}=\left[ \begin{array}{cccc}{\frac{\partial^{2} f(\boldsymbol x)}{\partial x_{1}^{2}}} & {\frac{\partial^{2} f(\boldsymbol x)}{\partial x_{1} \partial x_{2}}} & {\cdots} & {\frac{\partial^{2} f(\boldsymbol x)}{\partial x_{1} \partial x_{n}}} \\ {\frac{\partial^{2} f(\boldsymbol x)}{\partial x_{2} \partial x_{1}}} & {\frac{\partial^{2} f(\boldsymbol x)}{\partial x_{2}^{2}}} & {\cdots} & {\frac{\partial^{2} f(\boldsymbol x)}{\partial x_{2} \partial x_{n}}} \\ {\vdots} & {\vdots} & {\ddots} & {\vdots} \\ {\frac{\partial^{2} f(\boldsymbol x)}{\partial x_{n} \partial x_{1}}} & {\frac{\partial^{2} f(\boldsymbol x)}{\partial x_{n} \partial x_{2}}} & {\cdots} & {\frac{\partial^{2} f(\boldsymbol x)}{\partial x_{n}^{2}}}\end{array}\right]
$$

为函数$f(\boldsymbol{x})$在$\boldsymbol{x}$处的二阶导数或Hessian矩阵。若其中的二阶偏导数均连续，则

$$
\frac{\partial^{2} f(\boldsymbol{x})}{\partial x_{i} \partial x_{j}}=\frac{\partial^{2} f(\boldsymbol{x})}{\partial x_{j} \partial x_{i}}
$$

此时Hessian矩阵为对称矩阵。

**定理3.1**：设$D\subset\mathbb{R}^n$是非空开凸集，$f(\boldsymbol{x})$是定义在$D$上的实值函数，且$f(\boldsymbol{x})$在$D$上二阶连续可微，如果$f(\boldsymbol{x})$的Hessian矩阵$\nabla^2f(\boldsymbol x)$在$D$上是半正定的，则$f(\boldsymbol{x})$是$D$上的凸函数；如果$\nabla^2f(\boldsymbol x)$在$D$上是正定的，则$f(\boldsymbol{x})$是$D$上的严格凸函数。

**定理3.2**：若$f(\boldsymbol{x})$是凸函数，且$f(\boldsymbol{x})$一阶连续可微，则$\boldsymbol{x}^*$是全局解的充分必要条件是其梯度等于零向量，即$\nabla f(\boldsymbol x^*)=\boldsymbol{0}$。

式(3.11)的推导思路如下：首先根据定理3.1推导出$E_{\hat{\boldsymbol{w}}}$是$\hat{\boldsymbol{w}}$的凸函数，接着根据定理3.2推导出式(3.11)。下面按照此思路进行推导。

由于式(3.10)已推导出$E_{\hat{\boldsymbol{w}}}$关于$\hat{\boldsymbol{w}}$的一阶导数，接着基于此进一步推导出二阶导数，即Hessian矩阵。推导过程如下：

$$
\begin{aligned}
\nabla^2E_{\hat{\boldsymbol{w}}} &= \frac{\partial}{\partial \hat{\boldsymbol w}^\mathrm{T}}\left(\frac{\partial E_{\hat{\boldsymbol w}}}{\partial \hat{\boldsymbol w}}\right)\\
&=\frac{\partial}{\partial \hat{\boldsymbol w}^\mathrm{T}}\left[2\mathbf{X}^\mathrm{T}(\mathbf{X}\hat{\boldsymbol{w}}-\boldsymbol{y})\right]\\
&=\frac{\partial}{\partial \hat{\boldsymbol{w}}^\mathrm{T}}\left( 2\mathbf{X}^\mathrm{T}\mathbf{X}\hat{\boldsymbol{w}}-2\mathbf{X}^\mathrm{T}\boldsymbol{y}\right)
\end{aligned}
$$

由矩阵微分公式$\frac{\partial \mathbf{A}\boldsymbol{x}}{\boldsymbol{x}^\mathrm{T}}=\mathbf{A}$可得

$$
\nabla^2E_{\hat{\boldsymbol{w}}}=2\mathbf{X}^\mathrm{T}\mathbf{X}
$$


如"西瓜书"中式(3.11)上方的一段话所说，假定$\mathbf{X}^\mathrm{T}\mathbf{X}$为正定矩阵，根据定理3.1可知此时$E_{\hat{\boldsymbol{w}}}$是$\hat{\boldsymbol{w}}$的严格凸函数，接着根据定理3.2可知只需令$E_{\hat{\boldsymbol{w}}}$关于$\hat{\boldsymbol{w}}$的一阶导数等于零向量，即令式(3.10)等于零向量即可求得全局最优解$\hat{\boldsymbol{w}}^{*}$，具体求解过程如下：

$$
\frac{\partial E_{\hat{\boldsymbol w}}}{\partial \hat{\boldsymbol w}}=2\mathbf{X}^\mathrm{T}(\mathbf{X}\hat{\boldsymbol{w}}-\boldsymbol{y})=\boldsymbol{0}
$$


$$
2\mathbf{X}^\mathrm{T}\mathbf{X}\hat{\boldsymbol{w}}-2\mathbf{X}^\mathrm{T}\boldsymbol{y}=\boldsymbol{0}
$$


$$
2\mathbf{X}^\mathrm{T}\mathbf{X}\hat{\boldsymbol{w}}=2\mathbf{X}^\mathrm{T}\boldsymbol{y}
$$


$$
\hat{\boldsymbol{w}}=(\mathbf{X}^\mathrm{T}\mathbf{X})^{-1}\mathbf{X}^\mathrm{T}\boldsymbol{y}
$$

令其为$\hat{\boldsymbol{w}}^{*}$即为式(3.11)。

由于$\mathbf{X}$是由样本构成的矩阵，而样本是千变万化的，因此无法保证$\mathbf{X}^\mathrm{T}\mathbf{X}$一定是正定矩阵，极易出现非正定的情形。当$\mathbf{X}^\mathrm{T}\mathbf{X}$非正定矩阵时，除了"西瓜书"中所说的引入正则化外，也可用$\mathbf{X}^\mathrm{T}\mathbf{X}$的伪逆矩阵代入式(3.11)求解出$\hat{\boldsymbol{w}}^{*}$，只是此时并不保证求解得到的$\hat{\boldsymbol{w}}^{*}$一定是全局最优解。除此之外，也可用下一节将会讲到的"梯度下降法"求解，同样也不保证求得全局最优解。

## 3.3 对数几率回归

对数几率回归的一般使用流程如下：首先在训练集上学得模型

$$
y=\frac{1}{1+e^{-\left(\boldsymbol{w}^\mathrm{T}\boldsymbol{x}+b\right)}}
$$

然后对于新的测试样本$\boldsymbol{x}_{i}$，将其代入模型得到预测结果$y_{i}$，接着自行设定阈值$\theta$，通常设为$\theta=0.5$，如果$y_{i}\geqslant\theta$则判$\boldsymbol{x}_{i}$为正例，反之判为反例。

### 3.3.1 式(3.27)的推导

将式(3.26)代入式(3.25)可得

$$
\ell(\boldsymbol{\beta})=\sum_{i=1}^{m}\ln\left(y_ip_1(\hat{\boldsymbol x}_i;\boldsymbol{\beta})+(1-y_i)p_0(\hat{\boldsymbol x}_i;\boldsymbol{\beta})\right)
$$

其中$p_1(\hat{\boldsymbol x}_i;\boldsymbol{\beta})=\frac{e^{\boldsymbol{\beta}^{\mathrm{T}}\hat{\boldsymbol x}_i}}{1+e^{\boldsymbol{\beta}^{\mathrm{T}}\hat{\boldsymbol x}_i}},p_0(\hat{\boldsymbol x}_i;\boldsymbol{\beta})=\frac{1}{1+e^{\boldsymbol{\beta}^{\mathrm{T}}\hat{\boldsymbol x}_i}}$，代入上式可得

$$
\begin{aligned} 
\ell(\boldsymbol{\beta})&=\sum_{i=1}^{m}\ln\left(\frac{y_ie^{\boldsymbol{\beta}^{\mathrm{T}}\hat{\boldsymbol x}_i}+1-y_i}{1+e^{\boldsymbol{\beta}^{\mathrm{T}}\hat{\boldsymbol x}_i}}\right) \\
&=\sum_{i=1}^{m}\left(\ln(y_ie^{\boldsymbol{\beta}^{\mathrm{T}}\hat{\boldsymbol x}_i}+1-y_i)-\ln(1+e^{\boldsymbol{\beta}^{\mathrm{T}}\hat{\boldsymbol x}_i})\right) 
\end{aligned}
$$

由于$y_i$=0或1，则 

$$
\ell(\boldsymbol{\beta}) =
\begin{cases} 
\sum_{i=1}^{m}(-\ln(1+e^{\boldsymbol{\beta}^{\mathrm{T}}\hat{\boldsymbol x}_i})),  & y_i=0 \\
\sum_{i=1}^{m}(\boldsymbol{\beta}^{\mathrm{T}}\hat{\boldsymbol x}_i-\ln(1+e^{\boldsymbol{\beta}^{\mathrm{T}}\hat{\boldsymbol x}_i})), & y_i=1
\end{cases}
$$

两式综合可得

$$
\ell(\boldsymbol{\beta})=\sum_{i=1}^{m}\left(y_i\boldsymbol{\beta}^{\mathrm{T}}\hat{\boldsymbol x}_i-\ln(1+e^{\boldsymbol{\beta}^{\mathrm{T}}\hat{\boldsymbol x}_i})\right)
$$

由于此式仍为极大似然估计的似然函数，所以最大化似然函数等价于最小化似然函数的相反数，即在似然函数前添加负号即可得式(3.27)。值得一提的是，若将式(3.26)改写为$p(y_i|\boldsymbol x_i;\boldsymbol w,b)=[p_1(\hat{\boldsymbol x}_i;\boldsymbol{\beta})]^{y_i}[p_0(\hat{\boldsymbol x}_i;\boldsymbol{\beta})]^{1-y_i}$，再代入式(3.25)可得

$$
\begin{aligned}
 \ell(\boldsymbol{\beta})&=\sum_{i=1}^{m}\ln\left([p_1(\hat{\boldsymbol x}_i;\boldsymbol{\beta})]^{y_i}[p_0(\hat{\boldsymbol x}_i;\boldsymbol{\beta})]^{1-y_i}\right) \\
&=\sum_{i=1}^{m}\left[y_i\ln\left(p_1(\hat{\boldsymbol x}_i;\boldsymbol{\beta})\right)+(1-y_i)\ln\left(p_0(\hat{\boldsymbol x}_i;\boldsymbol{\beta})\right)\right] \\
&=\sum_{i=1}^{m} \left \{ y_i\left[\ln\left(p_1(\hat{\boldsymbol x}_i;\boldsymbol{\beta})\right)-\ln\left(p_0(\hat{\boldsymbol x}_i;\boldsymbol{\beta})\right)\right]+\ln\left(p_0(\hat{\boldsymbol x}_i;\boldsymbol{\beta})\right)\right\} \\
&=\sum_{i=1}^{m}\left[y_i\ln\left(\frac{p_1(\hat{\boldsymbol x}_i;\boldsymbol{\beta})}{p_0(\hat{\boldsymbol x}_i;\boldsymbol{\beta})}\right)+\ln\left(p_0(\hat{\boldsymbol x}_i;\boldsymbol{\beta})\right)\right] \\
&=\sum_{i=1}^{m}\left[y_i\ln\left(e^{\boldsymbol{\beta}^{\mathrm{T}}\hat{\boldsymbol x}_i}\right)+\ln\left(\frac{1}{1+e^{\boldsymbol{\beta}^{\mathrm{T}}\hat{\boldsymbol x}_i}}\right)\right] \\
&=\sum_{i=1}^{m}\left(y_i\boldsymbol{\beta}^{\mathrm{T}}\hat{\boldsymbol x}_i-\ln(1+e^{\boldsymbol{\beta}^{\mathrm{T}}\hat{\boldsymbol x}_i})\right) 
\end{aligned}
$$

显然，此种方式更易推导出式(3.27)。

"西瓜书"在式(3.27)下方有提到式(3.27)是关于$\boldsymbol{\beta}$的凸函数，其证明过程如下：由于若干半正定矩阵的加和仍为半正定矩阵，则根据定理3.1可知，若干凸函数的加和仍为凸函数。因此，只需证明式(3.27)求和符号后的式子$-y_i\boldsymbol{\beta}^{\mathrm{T}}\hat{\boldsymbol x}_i+\ln(1+e^{\boldsymbol{\beta}^{\mathrm{T}}\hat{\boldsymbol x}_i})$（记为$f(\boldsymbol{\beta})$）为凸函数即可。根据式(3.31)可知，$f(\boldsymbol{\beta})$的二阶导数，即Hessian矩阵为

$$
\hat{\boldsymbol{x}}_{i} \hat{\boldsymbol{x}}_{i}^\mathrm{T} p_{1}\left(\hat{\boldsymbol{x}}_{i};\boldsymbol{\beta}\right)\left(1-p_{1}\left(\hat{\boldsymbol{x}}_{i};\boldsymbol{\beta}\right)\right)
$$

对于任意非零向量$\boldsymbol{y}\in\mathbb{R}^{d+1}$，恒有

$$
\boldsymbol{y}^\mathrm{T}\cdot\hat{\boldsymbol{x}}_{i} \hat{\boldsymbol{x}}_{i}^\mathrm{T} p_{1}\left(\hat{\boldsymbol{x}}_{i};\boldsymbol{\beta}\right)\left(1-p_{1}\left(\hat{\boldsymbol{x}}_{i};\boldsymbol{\beta}\right)\right)\cdot\boldsymbol{y}
$$


$$
\boldsymbol{y}^\mathrm{T}\hat{\boldsymbol{x}}_{i} \hat{\boldsymbol{x}}_{i}^\mathrm{T}\boldsymbol{y} p_{1}\left(\hat{\boldsymbol{x}}_{i};\boldsymbol{\beta}\right)\left(1-p_{1}\left(\hat{\boldsymbol{x}}_{i};\boldsymbol{\beta}\right)\right)
$$


$$
\left(\boldsymbol{y}^\mathrm{T}\hat{\boldsymbol{x}}_{i}\right)^2 p_{1}\left(\hat{\boldsymbol{x}}_{i};\boldsymbol{\beta}\right)\left(1-p_{1}\left(\hat{\boldsymbol{x}}_{i};\boldsymbol{\beta}\right)\right)
$$

由于$p_{1}\left(\hat{\boldsymbol{x}}_{i};\boldsymbol{\beta}\right)>0$，因此上式恒大于等于0，根据半正定矩阵的定义可知此时$f(\boldsymbol{\beta})$的Hessian矩阵为半正定矩阵，所以$f(\boldsymbol{\beta})$是关于$\boldsymbol{\beta}$的凸函数。

### 3.3.2 梯度下降法

不同于式(3.7)可求得闭式解，式(3.27)中的$\boldsymbol{\beta}$没有闭式解，因此需要借助其他工具进行求解。求解使得式(3.27)取到最小值的$\boldsymbol{\beta}$属于最优化中的"无约束优化问题"，在无约束优化问题中最常用的求解算法有"梯度下降法"和"牛顿法"[1]，下面分别展开讲解。

梯度下降法是一种迭代求解算法，其基本思路如下：先在定义域中随机选取一个点$\boldsymbol{x}^{0}$，将其代入函数$f(\boldsymbol{x})$并判断此时$f(\boldsymbol{x}^{0})$是否是最小值，如果不是的话，则找下一个点$\boldsymbol{x}^{1}$，且保证$f(\boldsymbol{x}^{1})<f(\boldsymbol{x}^{0})$，然后接着判断$f(\boldsymbol{x}^{1})$是否是最小值，如果不是的话则重复上述步骤继续迭代寻找$\boldsymbol{x}^{2}$、$\boldsymbol{x}^{3}$、\...\...直到找到使得$f(\boldsymbol{x})$取到最小值的$\boldsymbol{x}^{*}$。

显然，此算法要想行得通就必须解决在找到第$t$个点$\boldsymbol{x}^{t}$时，能进一步找到第$t+1$个点$\boldsymbol{x}^{t+1}$，且保证$f(\boldsymbol{x}^{t+1})<f(\boldsymbol{x}^{t})$。梯度下降法利用"梯度指向的方向是函数值增大速度最快的方向"这一特性，每次迭代时朝着梯度的反方向进行，进而实现函数值越迭代越小，下面给出完整的数学推导过程。

根据泰勒公式可知，当函数$f(\boldsymbol{x})$在$\boldsymbol{x}^{t}$处一阶可导时，在其邻域内进行一阶泰勒展开恒有

$$
f(\boldsymbol{x})=f\left(\boldsymbol{x}^{t}\right)+\nabla f\left(\boldsymbol{x}^{t}\right)^{\mathrm{T}}\left(\boldsymbol{x}-\boldsymbol{x}^{t}\right)+o\left(\left\|\boldsymbol{x}-\boldsymbol{x}^{t}\right\|\right)
$$

其中$\nabla f\left(\boldsymbol{x}^{t}\right)$是函数$f(\boldsymbol{x})$在点$\boldsymbol{x}^{t}$处的梯度，$\left\|\boldsymbol{x}-\boldsymbol{x}^{t}\right\|$是指向量$\boldsymbol{x}-\boldsymbol{x}^{t}$的模。若令$\boldsymbol{x}-\boldsymbol{x}^{t}=a\boldsymbol{d}^{t}$，其中$a>0$，$\boldsymbol{d}^{t}$是模长为1的单位向量，则上式可改写为

$$
f(\boldsymbol{x}^{t}+a\boldsymbol{d}^{t})=f\left(\boldsymbol{x}^{t}\right)+a\nabla f\left(\boldsymbol{x}^{t}\right)^{\mathrm{T}}\boldsymbol{d}^{t}+o\left(\left\|\boldsymbol{d}^{t}\right\|\right)
$$


$$
f(\boldsymbol{x}^{t}+a\boldsymbol{d}^{t})-f\left(\boldsymbol{x}^{t}\right)=a\nabla f\left(\boldsymbol{x}^{t}\right)^{\mathrm{T}}\boldsymbol{d}^{t}+o\left(\left\|\boldsymbol{d}^{t}\right\|\right)
$$

观察上式可知，如果能保证$a\nabla f\left(\boldsymbol{x}^{t}\right)^{\mathrm{T}}\boldsymbol{d}^{t}<0$，则一定能保证$f(\boldsymbol{x}^{t}+a\boldsymbol{d}^{t})<f\left(\boldsymbol{x}^{t}\right)$，此时再令$\boldsymbol{x}^{t+1}=\boldsymbol{x}^{t}+a\boldsymbol{d}^{t}$，即可推得我们想要的$f(\boldsymbol{x}^{t+1})<f(\boldsymbol{x}^{t})$。所以，此时问题转化为了求解能使得$a\nabla f\left(\boldsymbol{x}^{t}\right)^{\mathrm{T}}\boldsymbol{d}^{t}<0$的$\boldsymbol{d}^t$，且$a\nabla f\left(\boldsymbol{x}^{t}\right)^{\mathrm{T}}\boldsymbol{d}^{t}$比0越小，相应地$f(\boldsymbol{x}^{t+1})$也会比$f(\boldsymbol{x}^{t})$越小，也更接近最小值。

根据向量的内积公式可知

$$
a\nabla f\left(\boldsymbol{x}^{t}\right)^{\mathrm{T}}\boldsymbol{d}^{t}=a\times\|\nabla f\left(\boldsymbol{x}^{t}\right)\|\times\|\boldsymbol{d}^{t}\|\times\cos\theta^{t}
$$

其中$\theta^{t}$是向量$\nabla f\left(\boldsymbol{x}^{t}\right)$与向量$\boldsymbol{d}^{t}$之间的夹角。观察上式易知，此时$\|\nabla f\left(\boldsymbol{x}^{t}\right)\|$是固定常量，$\|\boldsymbol{d}^{t}\|=1$，所以当$a$也固定时，取$\theta^{t}=\pi$，即向量$\boldsymbol{d}^t$与向量$\nabla f\left(\boldsymbol{x}^{t}\right)$的方向刚好相反时，上式取到最小值。通常为了精简计算步骤，可直接令$\boldsymbol{d}^t=-\nabla f\left(\boldsymbol{x}^{t}\right)$，因此便得到了第$t+1$个点$\boldsymbol{x}^{t+1}$的迭代公式

$$
\boldsymbol{x}^{t+1}=\boldsymbol{x}^{t}-a\nabla f\left(\boldsymbol{x}^{t}\right)
$$

其中$a$也称为"步长"或"学习率"，是需要自行设定的参数，且每次迭代时可取不同值。

除了需要解决如何找到$\boldsymbol{x}^{t+1}$以外，梯度下降法通常还需要解决如何判断当前点是否使得函数取到了最小值，否则的话迭代过程便可能会无休止进行。常用的做法是预先设定一个极小的阈值$\epsilon$，当某次迭代造成的函数值波动已经小于$\epsilon$时，即$|f(\boldsymbol{x}^{t+1})-f(\boldsymbol{x}^{t})|<\epsilon$，我们便近似地认为此时$f(\boldsymbol{x}^{t+1})$取到了最小值。

### 3.3.3  牛顿法

同梯度下降法，牛顿法也是一种迭代求解算法，其基本思路和梯度下降法一致，只是在选取第$t+1$个点$\boldsymbol{x}^{t+1}$时所采用的策略有所不同，即迭代公式不同。梯度下降法每次选取$\boldsymbol{x}^{t+1}$时，只要求通过泰勒公式在$\boldsymbol{x}^{t}$的邻域内找到一个函数值比其更小的点即可，而牛顿法则期望在此基础之上，$\boldsymbol{x}^{t+1}$还必须是$\boldsymbol{x}^{t}$的邻域内的极小值点。

类似一元函数取到极值点的必要条件是一阶导数等于0，多元函数取到极值点的必要条件是其梯度等于零向量$\boldsymbol{0}$，为了能求解出$\boldsymbol{x}^{t}$的邻域内梯度等于$\boldsymbol{0}$的点，需要进行二阶泰勒展开，其展开式如下

$$
f(\boldsymbol{x}) = f\left(\boldsymbol{x}^{t}\right)+\nabla f\left(\boldsymbol{x}^{t}\right)^{\mathrm{T}}\left(\boldsymbol{x}-\boldsymbol{x}^{t}\right)+\frac{1}{2}\left(\boldsymbol{x}-\boldsymbol{x}^{t}\right)^{\mathrm{T}} \nabla^{2} f\left(\boldsymbol{x}^{t}\right)\left(\boldsymbol{x}-\boldsymbol{x}^{t}\right)+o\left(\left\|\boldsymbol{x}-\boldsymbol{x}^{t}\right\|\right)
$$

为了后续计算方便，我们取其近似形式

$$
f(\boldsymbol{x}) \approx f\left(\boldsymbol{x}^{t}\right)+\nabla f\left(\boldsymbol{x}^{t}\right)^{\mathrm{T}}\left(\boldsymbol{x}-\boldsymbol{x}^{t}\right)+\frac{1}{2}\left(\boldsymbol{x}-\boldsymbol{x}^{t}\right)^{\mathrm{T}} \nabla^{2} f\left(\boldsymbol{x}^{t}\right)\left(\boldsymbol{x}-\boldsymbol{x}^{t}\right)
$$

首先对上式求导 

$$
\begin{aligned}
\frac{\partial f(\boldsymbol{x})}{\partial \boldsymbol{x}}&=\frac{\partial f\left(\boldsymbol{x}^{t}\right)}{\partial \boldsymbol{x}}+\frac{\partial\nabla f\left(\boldsymbol{x}^{t}\right)^{\mathrm{T}}\left(\boldsymbol{x}-\boldsymbol{x}^{t}\right)}{\partial \boldsymbol{x}}+\frac{1}{2}\frac{\partial\left(\boldsymbol{x}-\boldsymbol{x}^{t}\right)^{\mathrm{T}} \nabla^{2} f\left(\boldsymbol{x}^{t}\right)\left(\boldsymbol{x}-\boldsymbol{x}^{t}\right)}{\partial \boldsymbol{x}} \\
&=0+\nabla f\left(\boldsymbol{x}^{t}\right)+\frac{1}{2}\left(\nabla^{2} f\left(\boldsymbol{x}^{t}\right)+\nabla^{2} f\left(\boldsymbol{x}^{t}\right)^\mathrm{T}\right)\left(\boldsymbol{x}-\boldsymbol{x}^{t}\right)
\end{aligned}
$$

假设函数$f(\boldsymbol{x})$在$\boldsymbol{x}^{t}$处二阶可导，且偏导数连续，则$\nabla^{2} f\left(\boldsymbol{x}^{t}\right)$是对称矩阵，上式可写为

$$
\begin{aligned}
\frac{\partial f(\boldsymbol{x})}{\partial \boldsymbol{x}}&=0+\nabla f\left(\boldsymbol{x}^{t}\right)+\frac{1}{2}\times2\times\nabla^{2} f\left(\boldsymbol{x}^{t}\right)\left(\boldsymbol{x}-\boldsymbol{x}^{t}\right)\\
&=\nabla f\left(\boldsymbol{x}^{t}\right)+\nabla^{2} f\left(\boldsymbol{x}^{t}\right)\left(\boldsymbol{x}-\boldsymbol{x}^{t}\right)
\end{aligned}
$$

令上式等于$\boldsymbol{0}$

$$
\nabla f\left(\boldsymbol{x}^{t}\right)+\nabla^{2} f\left(\boldsymbol{x}^{t}\right)\left(\boldsymbol{x}-\boldsymbol{x}^{t}\right)=\boldsymbol{0}
$$

当$\nabla^{2} f\left(\boldsymbol{x}^{t}\right)$是可逆矩阵时，解得

$$
\boldsymbol{x}=\boldsymbol{x}^{t}-\left[\nabla^{2} f\left(\boldsymbol{x}^{t}\right)\right]^{-1}\nabla f\left(\boldsymbol{x}^{t}\right)
$$

令上式为$\boldsymbol{x}^{t+1}$即可得到牛顿法的迭代公式

$$
\boldsymbol{x}^{t+1}=\boldsymbol{x}^{t}-\left[\nabla^{2} f\left(\boldsymbol{x}^{t}\right)\right]^{-1}\nabla f\left(\boldsymbol{x}^{t}\right)
$$

通过上述推导可知，牛顿法每次迭代时需要求解Hessian矩阵的逆矩阵，该步骤计算量通常较大，因此有人基于牛顿法，将其中求Hessian矩阵的逆矩阵改为求计算量更低的近似逆矩阵，我们称此类算法为"拟牛顿法"。

牛顿法虽然期望在每次迭代时能取到极小值点，但是通过上述推导可知，迭代公式是根据极值点的必要条件推导而得，因此并不保证一定是极小值点。

无论是梯度下降法还是牛顿法，根据其终止迭代的条件可知，其都是近似求解算法，即使$f(\boldsymbol{x})$是凸函数，也并不一定保证最终求得的是全局最优解，仅能保证其接近全局最优解。不过在解决实际问题时，并不一定苛求解得全局最优解，在能接近全局最优甚至局部最优时通常也能很好地解决问题。

### 3.3.4  式(3.29)的解释

根据上述牛顿法的迭代公式可知，此式为式(3.27)应用牛顿法时的迭代公式。

### 3.3.5 式(3.30)的推导


$$
\begin{aligned}
\frac{\partial \ell(\boldsymbol{\beta})}{\partial \boldsymbol{\beta}} &=\frac{\partial \sum_{i=1}^{m}\left(-y_{i} \boldsymbol{\beta}^\mathrm{T} \hat{\boldsymbol{x}}_{i}+\ln \left(1+e^{\boldsymbol{\beta}^\mathrm{T} \hat{\boldsymbol{x}}_{i}}\right)\right)}{\partial \boldsymbol{\beta}} \\
&=\sum_{i=1}^{m}\left(\frac{\partial\left(-y_{i} \boldsymbol{\beta}^\mathrm{T} \hat{\boldsymbol{x}}_{i}\right)}{\partial \boldsymbol{\beta}}+\frac{\partial \ln \left(1+e^{\boldsymbol{\beta}^\mathrm{T} \hat{\boldsymbol{x}}_{i}}\right)}{\partial \boldsymbol{\beta}}\right) \\
&=\sum_{i=1}^{m}\left(-y_{i} \hat{\boldsymbol{x}}_{i}+\frac{1}{1+e^{\boldsymbol{\beta}^\mathrm{T} \hat{\boldsymbol{x}}_{i}}} \cdot \hat{\boldsymbol{x}}_{i} e^{\boldsymbol{\beta}^\mathrm{T} \hat{\boldsymbol{x}}_{i}}\right) \\
&=-\sum_{i=1}^{m} \hat{\boldsymbol{x}}_{i}\left(y_{i}-\frac{e^{\boldsymbol{\beta}^\mathrm{T} \hat{\boldsymbol{x}}_{i}}}{1+e^{\boldsymbol{\beta}^\mathrm{T} \hat{\boldsymbol{x}}_{i}}}\right) \\
&=-\sum_{i=1}^{m} \hat{\boldsymbol{x}}_{i}\left(y_{i}-p_{1}\left(\hat{\boldsymbol{x}}_{i} ; \boldsymbol{\beta}\right)\right)
\end{aligned}
$$

此式也可以进行向量化，令$p_1(\hat{\boldsymbol{x}}_i;\boldsymbol{\beta})=\hat{y}_i$，代入上式得

$$
\begin{aligned}
\frac{\partial \ell(\boldsymbol{\beta})}{\partial \boldsymbol{\beta}} &= -\sum_{i=1}^{m}\hat{\boldsymbol{x}}_i(y_i-\hat{y}_i) \\
& =\sum_{i=1}^{m}\hat{\boldsymbol{x}}_i(\hat{y}_i-y_i) \\
& ={\mathbf{X}^{\mathrm{T}}}(\hat{\boldsymbol{y}}-\boldsymbol{y}) \\
\end{aligned}
$$

其中$\hat{\boldsymbol{y}}=(\hat{y}_{1};\hat{y}_{2};...;\hat{y}_{m}),\boldsymbol{y}=(y_{1};y_{2};...;y_{m})$。

### 3.3.6 式(3.31)的推导

继续对上述式(3.30)中倒数第二个等号的结果求导

$$
\begin{aligned}
\frac{\partial^{2} \ell(\boldsymbol{\beta})}{\partial \boldsymbol{\beta} \partial \boldsymbol{\beta}^\mathrm{T}} &=-\frac{\partial \sum_{i=1}^{m} \hat{\boldsymbol{x}}_{i}\left(y_{i}-\frac{e^{\boldsymbol{\beta}^\mathrm{T} \hat{\boldsymbol{x}}_{i}}}{1+e^\mathrm{T} \hat{\boldsymbol{x}}_{i}}\right)}{\partial \boldsymbol{\beta}^\mathrm{T}} \\
&=-\sum_{i=1}^{m} \hat{\boldsymbol{x}}_{i} \frac{\partial\left(y_{i}-\frac{e^{\boldsymbol{\beta}^\mathrm{T} \hat{\boldsymbol{x}}_{i}}}{1+e^{\boldsymbol{\beta}^\mathrm{T} \hat{\boldsymbol{x}}_{i}}}\right)}{\partial \boldsymbol{\beta}^\mathrm{T}} \\
&=-\sum_{i=1}^{m} \hat{\boldsymbol{x}}_{i}\left(\frac{\partial y_{i}}{\partial \boldsymbol{\beta}^\mathrm{T}}-\frac{\partial\left(\frac{e^{\boldsymbol{\beta}^\mathrm{T} \hat{\boldsymbol{x}}_{i}}}{1+e^{\boldsymbol{\beta}^\mathrm{T} \hat{\boldsymbol{x}}_{i}}}\right)}{\partial \boldsymbol{\beta}^\mathrm{T}}\right) \\
&=\sum_{i=1}^{m} \hat{\boldsymbol{x}}_{i}\cdot\frac{\partial\left(\frac{e^{\boldsymbol{\beta}^\mathrm{T} \hat{\boldsymbol{x}}_{i}}}{1+e^{\boldsymbol{\beta}^\mathrm{T} \hat{\boldsymbol{x}}_{i}}}\right)}{\partial \boldsymbol{\beta}^\mathrm{T}} \\
\end{aligned}
$$

根据矩阵微分公式$\frac{\partial\boldsymbol{a}^{\mathrm{T}}\boldsymbol{x}}{\partial\boldsymbol{x}^\mathrm{T}}=\frac{\partial\boldsymbol{x}^{\mathrm{T}}\boldsymbol{a}}{\partial\boldsymbol{x}^\mathrm{T}}=\boldsymbol{a}^\mathrm{T}$，其中

$$
\begin{aligned}
\frac{\partial\left(\frac{e^{\boldsymbol{\beta}^\mathrm{T} \hat{\boldsymbol{x}}_{i}}}{1+e^{\boldsymbol{\beta}^\mathrm{T} \hat{\boldsymbol{x}}_{i}}}\right)}{\partial \boldsymbol{\beta}^\mathrm{T}} &=\frac{\frac{\partial e^{\boldsymbol{\beta}^\mathrm{T} \hat{\boldsymbol{x}}_{i}}}{\partial \boldsymbol{\beta}^\mathrm{T}} \cdot\left(1+e^{\boldsymbol{\beta}^\mathrm{T} \hat{\boldsymbol{x}}_{i}}\right)-e^{\boldsymbol{\beta}^\mathrm{T} \hat{\boldsymbol{x}}_{i}} \cdot \frac{\partial\left(1+e^{\boldsymbol{\beta}^\mathrm{T} \hat{\boldsymbol{x}}_{i}}\right)}{\partial \boldsymbol{\beta}^\mathrm{T}}}{\left(1+e^{\boldsymbol{\beta}^\mathrm{T} \hat{\boldsymbol{x}}_{i}}\right)^{2}} \\
&=\frac{\hat{\boldsymbol{x}}_{i}^\mathrm{T} e^{\boldsymbol{\beta}^\mathrm{T} \hat{\boldsymbol{x}}_{i}} \cdot\left(1+e^{\boldsymbol{\beta}^\mathrm{T} \hat{\boldsymbol{x}}_{i}}\right)-e^{\boldsymbol{\beta}^\mathrm{T} \hat{\boldsymbol{x}}_{i}} \cdot \hat{\boldsymbol{x}}_{i}^\mathrm{T} e^{\boldsymbol{\beta}^\mathrm{T} \hat{\boldsymbol{x}}_{i}}}{\left(1+e^{\boldsymbol{\beta}^\mathrm{T} \hat{\boldsymbol{x}}_{i}}\right)^{2}} \\
&=\hat{\boldsymbol{x}}_{i}^\mathrm{T} e^{\boldsymbol{\beta}^\mathrm{T} \hat{\boldsymbol{x}}_{i}}\cdot\frac{\left(1+e^{\boldsymbol{\beta}^\mathrm{T} \hat{\boldsymbol{x}}_{i}}\right)-e^{\boldsymbol{\beta}^\mathrm{T} \hat{\boldsymbol{x}}_{i}}}{\left(1+e^{\boldsymbol{\beta}^\mathrm{T} \hat{\boldsymbol{x}}_{i}}\right)^{2}} \\
&=\hat{\boldsymbol{x}}_{i}^\mathrm{T} e^{\boldsymbol{\beta}^\mathrm{T} \hat{\boldsymbol{x}}_{i}}\cdot\frac{1}{\left(1+e^{\boldsymbol{\beta}^\mathrm{T} \hat{\boldsymbol{x}}_{i}}\right)^{2}} \\
&=\hat{\boldsymbol{x}}_{i}^\mathrm{T} \cdot \frac{e^{\boldsymbol{\beta}^\mathrm{T} \hat{\boldsymbol{x}}_{i}}}{1+e^{\boldsymbol{\beta}^\mathrm{T} \hat{\boldsymbol{x}}_{i}}} \cdot \frac{1}{1+e^{\boldsymbol{\beta}^\mathrm{T} \hat{\boldsymbol{x}}_{i}}}
\end{aligned}
$$

所以 

$$
\begin{aligned}
\frac{\partial^{2} \ell(\boldsymbol{\beta})}{\partial \boldsymbol{\beta} \partial \boldsymbol{\beta}^\mathrm{T}} &=\sum_{i=1}^{m} \hat{\boldsymbol{x}}_{i}\cdot\hat{\boldsymbol{x}}_{i}^\mathrm{T} \cdot \frac{e^{\boldsymbol{\beta}^\mathrm{T} \hat{\boldsymbol{x}}_{i}}}{1+e^{\boldsymbol{\beta}^\mathrm{T} \hat{\boldsymbol{x}}_{i}}} \cdot \frac{1}{1+e^{\boldsymbol{\beta}^\mathrm{T} \hat{\boldsymbol{x}}_{i}}} \\
&=\sum_{i=1}^{m} \hat{\boldsymbol{x}}_{i} \hat{\boldsymbol{x}}_{i}^\mathrm{T} p_{1}\left(\hat{\boldsymbol{x}}_{i};\boldsymbol{\beta}\right)\left(1-p_{1}\left(\hat{\boldsymbol{x}}_{i};\boldsymbol{\beta}\right)\right)
\end{aligned}
$$


## 3.4 线性判别分析

线性判别分析的一般使用流程如下：首先在训练集上学得模型

$$
y=\boldsymbol{w}^\mathrm{T}\boldsymbol{x}
$$

由向量内积的几何意义可知，$y$可以看作是$\boldsymbol{x}$在$\boldsymbol{w}$上的投影，因此在训练集上学得的模型能够保证训练集中的同类样本在$\boldsymbol{w}$上的投影$y$很相近，而异类样本在$\boldsymbol{w}$上的投影$y$很疏远。然后对于新的测试样本$\boldsymbol{x}_{i}$，将其代入模型得到它在$\boldsymbol{w}$上的投影$y_{i}$，然后判别这个投影$y_{i}$与哪一类投影更近，则将其判为该类。

最后，线性判别分析也是一种降维方法，但不同于第10章介绍的无监督降维方法，线性判别分析是一种监督降维方法，即降维过程中需要用到样本类别标记信息。

### 3.4.1 式(3.32)的推导

式(3.32)中$\|\boldsymbol w^{\mathrm{T}}\boldsymbol{\mu}_{0}-\boldsymbol w^{\mathrm{T}}\boldsymbol{\mu}_{1}\|_2^2$右下角的"2"表示求"2范数"，向量的2范数即为模，右上角的"2"表示求平方数，基于此，下面推导式(3.32)。

$$
\begin{aligned}
    J &= \frac{\|\boldsymbol w^{\mathrm{T}}\boldsymbol{\mu}_{0}-\boldsymbol w^{\mathrm{T}}\boldsymbol{\mu}_{1}\|_2^2}{\boldsymbol w^{\mathrm{T}}(\boldsymbol{\Sigma}_{0}+\boldsymbol{\Sigma}_{1})\boldsymbol w} \\
    &= \frac{\|(\boldsymbol w^{\mathrm{T}}\boldsymbol{\mu}_{0}-\boldsymbol w^{\mathrm{T}}\boldsymbol{\mu}_{1})^{\mathrm{T}}\|_2^2}{\boldsymbol w^{\mathrm{T}}(\boldsymbol{\Sigma}_{0}+\boldsymbol{\Sigma}_{1})\boldsymbol w} \\
    &= \frac{\|(\boldsymbol{\mu}_{0}-\boldsymbol{\mu}_{1})^{\mathrm{T}}\boldsymbol w\|_2^2}{\boldsymbol w^{\mathrm{T}}(\boldsymbol{\Sigma}_{0}+\boldsymbol{\Sigma}_{1})\boldsymbol w} \\
    &= \frac{\left[(\boldsymbol{\mu}_{0}-\boldsymbol{\mu}_{1})^{\mathrm{T}}\boldsymbol w\right]^{\mathrm{T}}(\boldsymbol{\mu}_{0}-\boldsymbol{\mu}_{1})^{\mathrm{T}}\boldsymbol w}{\boldsymbol w^{\mathrm{T}}(\boldsymbol{\Sigma}_{0}+\boldsymbol{\Sigma}_{1})\boldsymbol w} \\
    &= \frac{\boldsymbol w^{\mathrm{T}}(\boldsymbol{\mu}_{0}-\boldsymbol{\mu}_{1})(\boldsymbol{\mu}_{0}-\boldsymbol{\mu}_{1})^{\mathrm{T}}\boldsymbol w}{\boldsymbol w^{\mathrm{T}}(\boldsymbol{\Sigma}_{0}+\boldsymbol{\Sigma}_{1})\boldsymbol w}
\end{aligned}
$$


### 3.4.2 式(3.37)到式(3.39)的推导

由式(3.36)，可定义拉格朗日函数为

$$
L(\boldsymbol w,\lambda)=-\boldsymbol w^{\mathrm{T}}\mathbf{S}_b\boldsymbol w+\lambda(\boldsymbol w^{\mathrm{T}}\mathbf{S}_w\boldsymbol w-1)
$$

对$\boldsymbol w$求偏导可得 

$$
\begin{aligned}
\frac{\partial L(\boldsymbol w,\lambda)}{\partial \boldsymbol w} &= -\frac{\partial(\boldsymbol w^{\mathrm{T}}\mathbf{S}_b\boldsymbol w)}{\partial \boldsymbol w}+\lambda \frac{\partial(\boldsymbol w^{\mathrm{T}}\mathbf{S}_w\boldsymbol w-1)}{\partial \boldsymbol w} \\
&= -(\mathbf{S}_b+\mathbf{S}_b^{\mathrm{T}})\boldsymbol w+\lambda(\mathbf{S}_w+\mathbf{S}_w^{\mathrm{T}})\boldsymbol w
\end{aligned}
$$

由于$\mathbf{S}_b=\mathbf{S}_b^{\mathrm{T}},\mathbf{S}_w=\mathbf{S}_w^{\mathrm{T}}$，所以

$$
\frac{\partial L(\boldsymbol w,\lambda)}{\partial \boldsymbol w} = -2\mathbf{S}_b\boldsymbol w+2\lambda\mathbf{S}_w\boldsymbol w
$$

令上式等于0即可得

$$
-2\mathbf{S}_b\boldsymbol w+2\lambda\mathbf{S}_w\boldsymbol w=0
$$


$$
\mathbf{S}_b\boldsymbol w=\lambda\mathbf{S}_w\boldsymbol w
$$


$$
(\boldsymbol{\mu}_{0}-\boldsymbol{\mu}_{1})(\boldsymbol{\mu}_{0}-\boldsymbol{\mu}_{1})^{\mathrm{T}}\boldsymbol{w}=\lambda\mathbf{S}_w\boldsymbol w
$$

若令$(\boldsymbol{\mu}_{0}-\boldsymbol{\mu}_{1})^{\mathrm{T}}\boldsymbol{w}=\gamma$，则有

$$
\gamma(\boldsymbol{\mu}_{0}-\boldsymbol{\mu}_{1})=\lambda\mathbf{S}_w\boldsymbol w
$$


$$
\boldsymbol{w}=\frac{\gamma}{\lambda}\mathbf{S}_{w}^{-1}(\boldsymbol{\mu}_{0}-\boldsymbol{\mu}_{1})
$$

由于最终要求解的$\boldsymbol{w}$不关心其大小，只关心其方向，所以其大小可以任意取值。又因为$\boldsymbol{\mu}_{0}$和$\boldsymbol{\mu}_{1}$的大小是固定的，所以$\gamma$的大小只受$\boldsymbol{w}$的大小影响，因此可以通过调整$\boldsymbol{w}$的大小使得$\gamma=\lambda$，西瓜书中所说的"不妨令$\mathbf{S}_b\boldsymbol w=\lambda(\boldsymbol{\mu}_{0}-\boldsymbol{\mu}_{1})$"也可等价理解为令$\gamma=\lambda$，因此，此时$\frac{\gamma}{\lambda}=1$，求解出的$\boldsymbol{w}$即为式(3.39)。

### 3.4.3 式(3.43)的推导

由式(3.40)、式(3.41)、式(3.42)可得 

$$
\begin{aligned}
\mathbf{S}_b &= \mathbf{S}_t - \mathbf{S}_w \\
&= \sum_{i=1}^m(\boldsymbol x_i-\boldsymbol\mu)(\boldsymbol x_i-\boldsymbol\mu)^{\mathrm{T}}-\sum_{i=1}^N\sum_{\boldsymbol x\in X_i}(\boldsymbol x-\boldsymbol\mu_i)(\boldsymbol x-\boldsymbol\mu_i)^{\mathrm{T}} \\
&= \sum_{i=1}^N\left(\sum_{\boldsymbol x\in X_i}\left((\boldsymbol x-\boldsymbol\mu)(\boldsymbol x-\boldsymbol\mu)^{\mathrm{T}}-(\boldsymbol x-\boldsymbol\mu_i)(\boldsymbol x-\boldsymbol\mu_i)^{\mathrm{T}}\right)\right) \\
&= \sum_{i=1}^N\left(\sum_{\boldsymbol x\in X_i}\left((\boldsymbol x-\boldsymbol\mu)(\boldsymbol x^{\mathrm{T}}-\boldsymbol\mu^{\mathrm{T}})-(\boldsymbol x-\boldsymbol\mu_i)(\boldsymbol x^{\mathrm{T}}-\boldsymbol\mu_i^{\mathrm{T}})\right)\right) \\
&= \sum_{i=1}^N\left(\sum_{\boldsymbol x\in X_i}\left(\boldsymbol x\boldsymbol x^{\mathrm{T}} - \boldsymbol x\boldsymbol\mu^{\mathrm{T}}-\boldsymbol\mu\boldsymbol x^{\mathrm{T}}+\boldsymbol\mu\boldsymbol\mu^{\mathrm{T}}-\boldsymbol x\boldsymbol x^{\mathrm{T}}+\boldsymbol x\boldsymbol\mu_i^{\mathrm{T}}+\boldsymbol\mu_i\boldsymbol x^{\mathrm{T}}-\boldsymbol\mu_i\boldsymbol\mu_i^{\mathrm{T}}\right)\right)\\
&=\sum_{i=1}^N\left(\sum_{\boldsymbol x\in X_i}\left(- \boldsymbol x\boldsymbol\mu^{\mathrm{T}}-\boldsymbol\mu\boldsymbol x^{\mathrm{T}}+\boldsymbol\mu\boldsymbol\mu^{\mathrm{T}}+\boldsymbol x\boldsymbol\mu_i^{\mathrm{T}}+\boldsymbol\mu_i\boldsymbol x^{\mathrm{T}}-\boldsymbol\mu_i\boldsymbol\mu_i^{\mathrm{T}}\right)\right) \\
&= \sum_{i=1}^N\left(-\sum_{\boldsymbol x\in X_i}\boldsymbol x\boldsymbol\mu^{\mathrm{T}}-\sum_{\boldsymbol x\in X_i}\boldsymbol\mu\boldsymbol x^{\mathrm{T}}+\sum_{\boldsymbol x\in X_i}\boldsymbol\mu\boldsymbol\mu^{\mathrm{T}}+\sum_{\boldsymbol x\in X_i}\boldsymbol x\boldsymbol\mu_i^{\mathrm{T}}+\sum_{\boldsymbol x\in X_i}\boldsymbol\mu_i\boldsymbol x^{\mathrm{T}}-\sum_{\boldsymbol x\in X_i}\boldsymbol\mu_i\boldsymbol\mu_i^{\mathrm{T}}\right) \\
&= \sum_{i=1}^N\left(-m_i\boldsymbol\mu_i\boldsymbol\mu^{\mathrm{T}}-m_i\boldsymbol\mu\boldsymbol\mu_i^{\mathrm{T}}+m_i\boldsymbol\mu\boldsymbol\mu^{\mathrm{T}}+m_i\boldsymbol\mu_i\boldsymbol\mu_i^{\mathrm{T}}+m_i\boldsymbol\mu_i\boldsymbol\mu_i^{\mathrm{T}}-m_i\boldsymbol\mu_i\boldsymbol\mu_i^{\mathrm{T}}\right) \\
&= \sum_{i=1}^N\left(-m_i\boldsymbol\mu_i\boldsymbol\mu^{\mathrm{T}}-m_i\boldsymbol\mu\boldsymbol\mu_i^{\mathrm{T}}+m_i\boldsymbol\mu\boldsymbol\mu^{\mathrm{T}}+m_i\boldsymbol\mu_i\boldsymbol\mu_i^{\mathrm{T}}\right) \\
&= \sum_{i=1}^Nm_i\left(-\boldsymbol\mu_i\boldsymbol\mu^{\mathrm{T}}-\boldsymbol\mu\boldsymbol\mu_i^{\mathrm{T}}+\boldsymbol\mu\boldsymbol\mu^{\mathrm{T}}+\boldsymbol\mu_i\boldsymbol\mu_i^{\mathrm{T}}\right) \\
&= \sum_{i=1}^N m_i(\boldsymbol\mu_i-\boldsymbol\mu)(\boldsymbol\mu_i-\boldsymbol\mu)^{\mathrm{T}}
\end{aligned}
$$


### 3.4.4 式(3.44)的推导

此式是式(3.35)的推广形式，证明如下。

设$\mathbf{W}=(\boldsymbol w_1,\boldsymbol w_2,...,\boldsymbol w_i,...,\boldsymbol w_{N-1})\in\mathbb{R}^{d\times(N-1)}$，其中$\boldsymbol w_i\in\mathbb{R}^{d\times 1}$为$d$行1列的列向量，则

$$
\left\{
\begin{aligned}
\operatorname{tr}(\mathbf{W}^{\mathrm{T}}\mathbf{S}_b \mathbf{W})&=\sum_{i=1}^{N-1}\boldsymbol w_i^{\mathrm{T}}\mathbf{S}_b \boldsymbol w_i \\
\operatorname{tr}(\mathbf{W}^{\mathrm{T}}\mathbf{S}_w \mathbf{W})&=\sum_{i=1}^{N-1}\boldsymbol w_i^{\mathrm{T}}\mathbf{S}_w \boldsymbol w_i
\end{aligned}
\right.
$$

所以式(3.44)可变形为 

$$
\max\limits_{\mathbf{W}}\frac{
\sum_{i=1}^{N-1}\boldsymbol w_i^{\mathrm{T}}\mathbf{S}_b \boldsymbol w_i}{\sum_{i=1}^{N-1}\boldsymbol w_i^{\mathrm{T}}\mathbf{S}_w \boldsymbol w_i}
$$

对比式(3.35)易知，上式即式(3.35)的推广形式。

除了式(3.35)以外，还有一种常见的优化目标形式如下

$$
\max\limits_{\mathbf{W}}\frac{
\prod_{i=1}^{N-1}\boldsymbol w_i^{\mathrm{T}}\mathbf{S}_b \boldsymbol w_i}{\prod_{i=1}^{N-1}\boldsymbol w_i^{\mathrm{T}}\mathbf{S}_w \boldsymbol w_i}=\max\limits_{\mathbf{W}}\prod_{i=1}^{N-1}\frac{\boldsymbol w_i^{\mathrm{T}}\mathbf{S}_b \boldsymbol w_i}{\boldsymbol w_i^{\mathrm{T}}\mathbf{S}_w \boldsymbol w_i}
$$


无论是采用何种优化目标形式，其优化目标只要满足"同类样例的投影点尽可能接近，异类样例的投影点尽可能远离"即可。

### 3.4.5 式(3.45)的推导

同式(3.35)，此处也固定式(3.44)的分母为1，那么式(3.44)此时等价于如下优化问题

$$
\begin{array}{cl}\underset{\mathbf{w}}{\min} & -\operatorname{tr}(\mathbf{W}^{\mathrm{T}}\mathbf{S}_b \mathbf{W}) \\ 
\text { s.t. } & \operatorname{tr}(\mathbf{W}^{\mathrm{T}}\mathbf{S}_w \mathbf{W})=1\end{array}
$$

根据拉格朗日乘子法，可定义上述优化问题的拉格朗日函数

$$
L(\mathbf{W},\lambda)=-\operatorname{tr}(\mathbf{W}^{\mathrm{T}}\mathbf{S}_b \mathbf{W})+\lambda(\operatorname{tr}(\mathbf{W}^{\mathrm{T}}\mathbf{S}_w \mathbf{W})-1)
$$

根据矩阵微分公式$\frac{\partial}{\partial \mathbf{X}} \text { tr }(\mathbf{X}^{\mathrm{T}}  \mathbf{B} \mathbf{X})=(\mathbf{B}+\mathbf{B}^{\mathrm{T}})\mathbf{X}$对上式关于$\mathbf{W}$求偏导可得

$$
\begin{aligned}
\frac{\partial L(\mathbf{W},\lambda)}{\partial \mathbf{W}} &= -\frac{\partial\left(\operatorname{tr}(\mathbf{W}^{\mathrm{T}}\mathbf{S}_b \mathbf{W})\right)}{\partial \mathbf{W}}+\lambda \frac{\partial\left(\operatorname{tr}(\mathbf{W}^{\mathrm{T}}\mathbf{S}_w \mathbf{W})-1\right)}{\partial \mathbf{W}} \\
&= -(\mathbf{S}_b+\mathbf{S}_b^{\mathrm{T}})\mathbf{W}+\lambda(\mathbf{S}_w+\mathbf{S}_w^{\mathrm{T}})\mathbf{W}
\end{aligned}
$$

由于$\mathbf{S}_b=\mathbf{S}_b^{\mathrm{T}},\mathbf{S}_w=\mathbf{S}_w^{\mathrm{T}}$，所以

$$
\frac{\partial L(\mathbf{W},\lambda)}{\partial \mathbf{W}} = -2\mathbf{S}_b\mathbf{W}+2\lambda\mathbf{S}_w\mathbf{W}
$$

令上式等于$\mathbf{0}$即可得

$$
-2\mathbf{S}_b\mathbf{W}+2\lambda\mathbf{S}_w\mathbf{W}=\mathbf{0}
$$


$$
\mathbf{S}_b\mathbf{W}=\lambda\mathbf{S}_w\mathbf{W}
$$

此即为式(3.45)，但是此式在解释为何要取$N-1$个最大广义特征值所对应的特征向量来构成$\mathbf{W}$时不够直观。因此，我们换一种更为直观的方式求解式(3.44)，只需换一种方式构造拉格朗日函数即可。

重新定义上述优化问题的拉格朗日函数

$$
L(\mathbf{W},\boldsymbol{\Lambda})=-\operatorname{tr}(\mathbf{W}^{\mathrm{T}}\mathbf{S}_b \mathbf{W})+\operatorname{tr}\left(\boldsymbol{\Lambda}(\mathbf{W}^{\mathrm{T}}\mathbf{S}_w \mathbf{W}-\mathbf{I})\right)
$$

其中，$\mathbf{I} \in \mathbb{R}^{(N-1) \times (N-1)}$为单位矩阵，$\boldsymbol{\Lambda}=\operatorname{diag}(\lambda_1,\lambda_2,...,\lambda_{N-1})\in \mathbb{R}^{(N-1) \times (N-1)}$是由$N-1$个拉格朗日乘子构成的对角矩阵。根据矩阵微分公式$\frac{\partial}{\partial \mathbf{X}} \operatorname{tr}(\mathbf{X}^{\mathrm{T}}\mathbf{A}\mathbf{X})=(\mathbf{A}+\mathbf{A}^{\mathrm{T}})\mathbf{X},\frac{\partial}{\partial \mathbf{X}} \operatorname{tr}(\mathbf{X}\mathbf{A}\mathbf{X}^\mathrm{T}\mathbf{B})=\frac{\partial}{\partial \mathbf{X}} \operatorname{tr}(\mathbf{A}\mathbf{X}^\mathrm{T}\mathbf{B}\mathbf{X})=\mathbf{B}^\mathrm{T}\mathbf{X}\mathbf{A}^\mathrm{T}+\mathbf{B}\mathbf{X}\mathbf{A}$，对上式关于$\mathbf{W}$求偏导可得

$$
\begin{aligned}
\frac{\partial L(\mathbf{W},\boldsymbol{\Lambda})}{\partial \mathbf{W}} &= -\frac{\partial\left(\operatorname{tr}(\mathbf{W}^{\mathrm{T}}\mathbf{S}_b \mathbf{W})\right)}{\partial \mathbf{W}}+\frac{\partial\left(\operatorname{tr}\left(\boldsymbol{\Lambda}\mathbf{W}^{\mathrm{T}}\mathbf{S}_w \mathbf{W}-\boldsymbol{\Lambda}\mathbf{I}\right)\right)}{\partial \mathbf{W}} \\
&= -(\mathbf{S}_b+\mathbf{S}_b^{\mathrm{T}})\mathbf{W}+(\mathbf{S}_{w}^\mathrm{T}\mathbf{W}\boldsymbol{\Lambda}^\mathrm{T}+\mathbf{S}_w\mathbf{W}\boldsymbol{\Lambda})
\end{aligned}
$$

由于$\mathbf{S}_b=\mathbf{S}_b^{\mathrm{T}},\mathbf{S}_w=\mathbf{S}_w^{\mathrm{T}},\boldsymbol{\Lambda}^\mathrm{T}=\boldsymbol{\Lambda}$，所以

$$
\frac{\partial L(\mathbf{W},\boldsymbol{\Lambda})}{\partial \mathbf{W}} = -2\mathbf{S}_b\mathbf{W}+2\mathbf{S}_w\mathbf{W}\boldsymbol{\Lambda}
$$

令上式等于$\mathbf{0}$即可得

$$
-2\mathbf{S}_b\mathbf{W}+2\mathbf{S}_w\mathbf{W}\boldsymbol{\Lambda}=\mathbf{0}
$$


$$
\mathbf{S}_b\mathbf{W}=\mathbf{S}_w\mathbf{W}\boldsymbol{\Lambda}
$$

将$\mathbf{W}$和$\boldsymbol{\Lambda}$展开可得

$$
\mathbf{S}_b\boldsymbol{w}_i=\lambda_{i}\mathbf{S}_w\boldsymbol{w}_i, \quad i=1,2,...,N-1
$$

此时便得到了$N-1$个广义特征值问题。进一步地，将其代入优化问题的目标函数可得

$$
\begin{aligned}
\underset{\mathbf{W}}{\min}-\operatorname{ tr }(\mathbf W^{\mathrm{T}} \mathbf{S}_{b} \mathbf W)&=\max\limits_{\mathbf W}\operatorname{ tr }(\mathbf W^{\mathrm{T}} \mathbf{S}_{b} \mathbf W) \\
&=\max\limits_{\mathbf W}\sum_{i=1}^{N-1}\boldsymbol w_i^{\mathrm{T}}\mathbf{S}_{b} \boldsymbol w_i \\
&=\max\limits_{\mathbf W}\sum_{i=1}^{N-1}\lambda_{i}\boldsymbol w_i^{\mathrm{T}}\mathbf{S}_{w} \boldsymbol w_i
\end{aligned}
$$

由于存在约束$\operatorname{tr}(\mathbf{W}^{\mathrm{T}}\mathbf{S}_w \mathbf{W})=\sum\limits_{i=1}^{N-1}\boldsymbol w_i^{\mathrm{T}}\mathbf{S}_{w} \boldsymbol w_i=1$，所以欲使上式取到最大值，只需取$N-1$个最大的$\lambda_{i}$即可。根据$\mathbf{S}_b\boldsymbol{w}_i=\lambda_{i}\mathbf{S}_w\boldsymbol{w}_i$可知，$\lambda_{i}$对应的便是广义特征值，$\boldsymbol{w}_i$是$\lambda_{i}$所对应的特征向量。**（广义特征值的定义和常用求解方法可查阅[3]）**

对于$N$分类问题，一定要求出$N-1$个$\boldsymbol{w}_{i}$吗？其实不然。之所以将$\mathbf{W}$定义为$d\times (N-1)$维的矩阵是因为当$d>(N-1)$时，实对称矩阵$\mathbf{S}_w^{-1}\mathbf{S}_b$的秩至多为$N-1$，所以理论上至多能解出$N-1$个非零特征值$\lambda_{i}$及其对应的特征向量$\boldsymbol{w}_i$。但是$\mathbf{S}_w^{-1}\mathbf{S}_b$的秩是受当前训练集中的数据分布所影响的，因此并不一定为$N-1$。此外，当数据分布本身就足够理想时，即使能求解出多个$\boldsymbol{w}_i$，但是实际可能只需要求解出1个$\boldsymbol{w}_i$便可将同类样本聚集，异类样本完全分离。

当$d>(N-1)$时，实对称矩阵$\mathbf{S}_w^{-1}\mathbf{S}_b$的秩至多为$N-1$的证明过程如下：由于$\boldsymbol{\mu}=\frac{1}{N}\sum\limits_{i=1}^{N}m_i\boldsymbol{\mu}_i$，所以$\boldsymbol{\mu}_1-\boldsymbol{\mu}$一定可以由$\boldsymbol{\mu}$和$\boldsymbol{\mu}_2,...,\boldsymbol{\mu}_N$线性表示，因此矩阵$\mathbf{S}_b$中至多有$\boldsymbol{\mu}_2-\boldsymbol{\mu},...,\boldsymbol{\mu}_N-\boldsymbol{\mu}$共$N-1$个线性无关的向量，由于此时$d> (N-1)$，所以$\mathbf{S}_b$的秩$r(\mathbf{S}_b)$至多为$N-1$。同时假设矩阵$\mathbf{S}_w$满秩，即$r(\mathbf{S}_w)=r(\mathbf{S}_w^{-1})=d$，则根据矩阵秩的性质$r(\mathbf{A}\mathbf{B})\leqslant\min\{r(\mathbf{A}),r(\mathbf{B})\}$可知，$\mathbf{S}_w^{-1}\mathbf{S}_b$的秩也至多为$N-1$。

## 3.5 多分类学习

### 3.5.1 图3.5的解释

图3.5中所说的"海明距离"是指两个码对应位置不相同的个数，"欧式距离"则是指两个向量之间的欧氏距离，例如图3.5(a)中第1行的编码可以视作为向量$(-1, +1, -1, +1, +1)$，测试示例的编码则为$(-1, -1, +1, -1, +1)$，其中第2个、第3个、第4个元素不相同，所以它们的海明距离为3，欧氏距离为$\sqrt{(-1-(-1))^2+(1-(-1))^2+(-1-1)^2+(1-(-1))^2+(1-1)^2}=\sqrt{0+4+4+4+0}=2\sqrt{3}$。需要注意的是，在计算海明距离时，与"停用类"不同算作0.5，例如图3.5(b)中第2行的海明距离计算公式为$0.5+0.5+0.5+0.5=2$。

## 3.6 类别不平衡问题

对于类别不平衡问题，"西瓜书"2.3.1节中的"精度"通常无法满足该特殊任务的需求，例如"西瓜书"在本节第一段的举例：有998个反例和2个正例，若机器学习算法返回一个永远将新样本预测为反例的学习器则能达到99.8%的精度，显然虚高，因此在类别不平衡时常采用2.3节中的查准率、查全率和F1来度量学习器的性能。

## 参考文献
[1] 王燕军. 最优化基础理论与方法. 复旦大学出版社, 2011.

[2] Wikipedia contributors. Matrix calculus, 2022.

[3] 张贤达. 矩阵分析与应用. 第 2 版. 清华大学出版社, 2013.
