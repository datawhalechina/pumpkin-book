```{important}
参与组队学习的同学须知：

本章学习时间：1.5天

本章配套视频教程正在赶制中，先自行看书学习，第2章严格来说是在学完具体机器学习算法（第3章及其以后章节的内容）后再来学的，因此本章能看懂多少就看多少，只需看到2.3.2即可，【2.3.3-ROC与AUC】及其以后的暂时都可以跳过，等学完后面的算法再回来认真研读。

本章配套代码：https://github.com/datawhalechina/machine-learning-toy-code/blob/main/%E8%A5%BF%E7%93%9C%E4%B9%A6%E4%BB%A3%E7%A0%81%E5%AE%9E%E6%88%98.md

本章配套代码视频教程：https://space.bilibili.com/431850986/lists/3884942
```

# 第2章 模型评估与选择

如"西瓜书"前言所述，本章仍属于机器学习基础知识，如果说第1章介绍了什么是机器学习及机器学习的相关数学符号，那么本章则进一步介绍机器学习的相关概念。具体来说，介绍内容正如本章名称"模型评估与选择"所述，讲述的是如何评估模型的优劣和选择最适合自己业务场景的模型。

由于"模型评估与选择"是在模型产出以后进行的下游工作，要想完全吸收本章内容需要读者对模型有一些基本的认知，因此零基础的读者直接看本章会很吃力，实属正常，在此建议零基础的读者可以简单泛读本章，仅看能看懂的部分即可，或者直接跳过本章从第3章开始看，直至看完第6章以后再回头来看本章便会轻松许多。

## 2.1 经验误差与过拟合

梳理本节的几个概念。

**错误率**：$E=\frac{a}{m}$，其中$m$为样本个数，$a$为分类错误样本个数。

**精度**：精度=1-错误率。

**误差**：学习器的实际预测输出与样本的真实输出之间的差异。

**经验误差**：学习器在训练集上的误差，又称为"训练误差"。

**泛化误差**：学习器在新样本上的误差。

经验误差和泛化误差用于分类问题的定义式可参见"西瓜书"第12章的式(12.1)和式(12.2)，接下来辨析一下以上几个概念。

错误率和精度很容易理解，而且很明显是针对分类问题的。误差的概念更适用于回归问题，但是，根据"西瓜书"第12章的式(12.1)和式(12.2)的定义可以看出，在分类问题中也会使用误差的概念，此时的"差异"指的是学习器的实际预测输出的类别与样本真实的类别是否一致，若一致则"差异"为0，若不一致则"差异"为1，训练误差是在训练集上差异的平均值，而泛化误差则是在新样本（训练集中未出现过的样本）上差异的平均值。

**过拟合**是由于模型的学习能力相对于数据来说过于强大，反过来说，**欠拟合**是因为模型的学习能力相对于数据来说过于低下。暂且抛开"没有免费的午餐"定理不谈，例如对于"西瓜书"第1章图1.4中的训练样本（黑点）来说，用类似于抛物线的曲线A去拟合则较为合理，而比较崎岖的曲线B相对于训练样本来说学习能力过于强大，但若仅用一条直线去训练则相对于训练样本来说直线的学习能力过于低下。

## 2.2 评估方法

本节介绍了3种模型评估方法：留出法、交叉验证法、自助法。留出法由于操作简单，因此最常用；交叉验证法常用于对比同一算法的不同参数配置之间的效果，以及对比不同算法之间的效果；自助法常用于集成学习（详见"西瓜书"第8章的8.2节和8.3节）产生基分类器。留出法和自助法简单易懂，在此不再赘述，下面举例说明交叉验证法的常用方式。

**对比同一算法的不同参数配置之间的效果**：假设现有数据集$D$，且有一个被评估认为适合用于数据集$D$的算法$\mathfrak{L}$，该算法有可配置的参数，假设备选的参数配置方案有两套：方案$a$，方案$b$。下面通过交叉验证法为算法$\mathfrak{L}$筛选出在数据集$D$上效果最好的参数配置方案。以3折交叉验证为例，首先按照"西瓜书"中所说的方法，通过分层采样将数据集$D$划分为3个大小相似的互斥子集：$D_{1},D_{2},D_{3}$，然后分别用其中1个子集作为测试集，其他子集作为训练集，这样就可获得3组训练集和测试集：

训练集1：$D_{1}\cup D_{2}$，测试集1:$D_{3}$

训练集2：$D_{1}\cup D_{3}$，测试集2:$D_{2}$

训练集3：$D_{2}\cup D_{3}$，测试集3:$D_{1}$

接下来用算法$\mathfrak{L}$搭配方案$a$在训练集1上进行训练，训练结束后将训练得到的模型在测试集1上进行测试，得到测试结果1，依此方法再分别通过训练集2和测试集2、训练集3和测试集3得到测试结果2和测试结果3，最后将3次测试结果求平均即可得到算法$\mathfrak{L}$搭配方案$a$在数据集$D$上的最终效果，记为$Score_{a}$。同理，按照以上方法也可得到算法$\mathfrak{L}$搭配方案$b$在数据集$D$上的最终效果$Score_{b}$，最后通过比较$Score_{a}$和$Score_{b}$之间的优劣来确定算法$\mathfrak{L}$在数据集$D$上效果最好的参数配置方案。

**对比不同算法之间的效果**：同上述"对比同一算法的不同参数配置之间的效果"中所讲的方法一样，只需将其中的"算法$\mathfrak{L}$搭配方案$a$"和"算法$\mathfrak{L}$搭配方案$b$"分别换成需要对比的算法$\alpha$和算法$\beta$即可。

从以上的举例可以看出，交叉验证法本质上是在进行多次留出法，且每次都换不同的子集做测试集，最终让所有样本均至少做1次测试样本。这样做的理由其实很简单，因为一般的留出法只会划分出1组训练集和测试集，仅依靠1组训练集和测试集去对比不同算法之间的效果显然不够置信，偶然性太强，因此要想基于固定的数据集产生多组不同的训练集和测试集，则只有进行多次划分，每次采用不同的子集作为测试集，也即为交叉验证法。

### 2.2.1 算法参数（超参数）与模型参数

算法参数是指算法本身的一些参数（也称超参数），例如$k$近邻的近邻个数$k$、支持向量机的参数$C$（详见"西瓜书"第6章式(6.29)）。算法配置好相应参数后进行训练，训练结束会得到一个模型，例如支持向量机最终会得到$\boldsymbol{w}$和$b$的具体数值（此处不考虑核函数），这就是模型参数，模型配置好相应模型参数后即可对新样本做预测。

### 2.2.2 验证集

带有参数的算法一般需要从候选参数配置方案中选择相对于当前数据集的最优参数配置方案，例如支持向量机的参数$C$，一般采用的是前面讲到的交叉验证法，但是交叉验证法操作起来较为复杂，实际中更多采用的是：先用留出法将数据集划分出训练集和测试集，然后再对训练集采用留出法划分出训练集和新的测试集，称新的测试集为验证集，接着基于验证集的测试结果来调参选出最优参数配置方案，最后将验证集合并进训练集（训练集数据量够的话也可不合并），用选出的最优参数配置在合并后的训练集上重新训练，再用测试集来评估训练得到的模型的性能。

## 2.3 性能度量

本节性能度量指标较多，但是一般常用的只有错误率、精度、查准率、查全率、F1、ROC和AUC。

### 2.3.1 式(2.2)到式(2.7)的解释

这几个公式简单易懂，几乎不需要额外解释，但是需要补充说明的是式(2.2)、式(2.4)和式(2.5)假设了数据分布为均匀分布，即每个样本出现的概率相同，而式(2.3)、式(2.6)和式(2.7)则为更一般的表达式。此外，在无特别说明的情况下，2.3节所有公式中的"样例集$D$"均默认为非训练集（测试集、验证集或其他未用于训练的样例集）。

### 2.3.2 式(2.8)和式(2.9)的解释

查准率$P$：被学习器预测为**正例**的样例中有多大比例是**真正例**。

查全率$R$：所有**正例**当中有多大比例被学习器预测为**正例**。

### 2.3.3 图2.3的解释

P-R曲线的画法与ROC曲线的画法类似，也是通过依次改变模型阈值，然后计算出查准率和查全率并画出相应坐标点，具体参见"式(2.20)的推导"部分的讲解。这里需要说明的是，"西瓜书"中的图2.3仅仅是示意图，除了图左侧提到的"现实任务中的P-R曲线常是非单调、不平滑的，在很多局部有上下波动"以外，通常不会取到$(1,0)$点。当取到$(1,0)$点时，就会将所有样本均判为正例，此时$FN=0$，根据式(2.9)可算得查全率为1，但是此时$TP+FP$为样本总数，根据式(2.8)可算得查准率此时为正例在全体样本中的占比，显然在现实任务中正例的占比通常不为0，因此P-R曲线在现实任务中通常不会取到$(1,0)$点。

### 2.3.4 式(2.10)的推导

将式(2.8)和式(2.9)代入式(2.10)，得

$$\begin{aligned}
F1 &=\frac{2 \times P \times R}{P+R}=\frac{2 \times \frac{TP}{TP+FP} \times \frac{TP}{TP+FN}}{\frac{TP}{TP+FP}+\frac{TP}{TP+FN}} \\
&=\frac{2 \times TP \times TP}{TP(TP+FN)+T P(TP+FP)} \\
&=\frac{2 \times TP}{(TP+FN)+(TP+FP)}\\
&=\frac{2 \times TP}{(TP+FN+FP+TN)+TP-TN}\\
&=\frac{2 \times TP}{\text{样例总数}+TP-TN}\\
\end{aligned}$$

若现有数据集$D=\left\{\left(\boldsymbol{x}_{i}, y_{i}\right) \mid 1 \leqslant i \leqslant m\right\}$，其中标记$y_{i}\in\{0,1\}$（1表示正例，0表示反例），假设模型$f(\boldsymbol{x})$对$\boldsymbol{x}_{i}$的预测结果为$h_i\in\{0,1\}$，则模型$f(\boldsymbol{x})$在数据集$D$上的$F1$为

$$F1=\frac{2 \sum_{i=1}^{m} y_{i} h_{i}}{\sum_{i=1}^{m} y_{i}+\sum_{i=1}^{m} h_{i}}$$

不难看出上式的本质为

$$F1=\frac{2 \times TP}{(TP+FN)+(TP+FP)}$$

### 2.3.5 式(2.11)的解释

"西瓜书"在式(2.11)左侧提到$F_{\beta}$本质是加权调和平均，且和常用的算数平均相比，其更重视较小值，在此举例说明。例如a同学有两门课的成绩分别为100分和60分，b同学相应的成绩为80分和80分，此时若计算a同学和b同学的算数平均分则均为80分，无法判断两位同学成绩的优劣，但是若计算加权调和平均，当$\beta=1$时，a同学的加权调和平均为$\frac{2\times 100 \times 60}{100+60}=75$，b同学的加权调和平均为$\frac{2\times 80 \times 80}{80+80}=80$，此时b同学的平均成绩更优，原因是a同学由于偏科导致其中一门成绩过低，而调和平均更重视较小值，所以a同学的偏科便被凸显出来。

式(2.11)下方有提到"$\beta>1$时查全率有更大影响；$\beta<1$时查准率有更大影响"，下面解释其原因。将式(2.11)恒等变形为如下形式

$$F_{\beta}=\frac{1}{\frac{1}{1+\beta^{2}}\cdot\frac{1}{P}+\frac{\beta^{2}}{1+\beta^{2}}\cdot\frac{1}{R}}$$

从上式可以看出，当$\beta>1$时$\frac{\beta^{2}}{1+\beta^{2}}>\frac{1}{1+\beta^{2}}$，所以$\frac{1}{R}$的权重比$\frac{1}{P}$的权重高，因此查全率$R$对$F_{\beta}$的影响更大，反之查准率$P$对$F_{\beta}$的影响更大。

### 2.3.6 式(2.12)到式(2.17)的解释

式(2.12)的$\text{macro-}P$和式(2.13)的$\text{macro-}R$是基于各个二分类问题的$P$和$R$计算而得的；式(2.15)的$\text{micro-}P$和式(2.16)的$\text{micro-}R$是基于各个二分类问题的$TP$、$FP$、$TN$、$FN$计算而得的；"宏"可以认为是只关注宏观而不看具体细节，而"微"可以认为是要从具体细节做起，因为相比于$P$和$R$指标来说，$TP$、$FP$、$TN$、$FN$更微观，毕竟$P$和$R$是基于$TP$、$FP$、$TN$、$FN$计算而得。

从"宏"和"微"的计算方式可以看出，"宏"没有考虑每个类别下的的样本数量，所以平等看待每个类别，因此会受到高$P$和高$R$类别的影响，而"微"则考虑到了每个类别的样本数量，因为样本数量多的类相应的$TP$、$FP$、$TN$、$FN$也会占比更多，所以在各类别样本数量极度不平衡的情况下，数量较多的类别会主导最终结果。

式(2.14)的$\text{macro-}F1$是将$\text{macro-}P$和$\text{macro-}R$代入式(2.10)所得；式(2.17)的$\text{micro-}F1$是将$\text{micro-}P$和$\text{micro-}R$代入式(2.10)所得。值得一提的是，以上只是$\text{macro-}F1$和$\text{micro-}F1$的常用计算方式之一，如若在查阅资料的过程中看到其他的计算方式也属正常。

### 2.3.7 式(2.18)和式(2.19)的解释

式(2.18)定义了真正例率TPR。先解释公式中出现的真正例和假反例，真正例即实际为正例预测结果也为正例，假反例即实际为正例但预测结果为反例，式(2.18)分子为真正例，分母为真正例和假反例之和（即实际的正例个数），因此式(2.18)的含义是所有**正例**当中有多大比例被预测为**正例**（即查全率Recall）。

式(2.19)定义了假正例率FPR。先解释式子中出现的假正例和真反例，假正例即实际为反例但预测结果为正例，真反例即实际为反例预测结果也为反例，式(2.19)分子为假正例，分母为真反例和假正例之和（即实际的反例个数），因此式(2.19)的含义是所有**反例**当中有多大比例被预测为**正例**。

除了真正例率TPR和假正例率FPR，还有真反例率TNR和假反例率FNR：

$$\begin{aligned}
    \mathrm{TNR}=\frac{TN}{FP+TN} \\
    \mathrm{FNR}=\frac{FN}{TP+FN}
\end{aligned}$$

### 2.3.8 式(2.20)的推导

在推导式(2.20)之前，需要先弄清楚$\text{ROC}$曲线的具体绘制过程。下面我们就举个例子，按照"西瓜书"图2.4下方给出的绘制方法来讲解一下$\text{ROC}$曲线的具体绘制过程。

假设我们已经训练得到一个学习器$f(\boldsymbol{s})$，现在用该学习器来对8个测试样本（4个正例，4个反例，即$m^+=m^-=4$）进行预测，预测结果为 **（此处用$\boldsymbol{s}$表示样本，以和坐标$(x,y)$作出区分）**：

$$\begin{aligned}
&(\boldsymbol{s}_1,0.77,+),(\boldsymbol{s}_2,0.62,-),(\boldsymbol{s}_3,0.58,+),(\boldsymbol{s}_4,0.47,+),\\
&(\boldsymbol{s}_5,0.47,-),(\boldsymbol{s}_6,0.33,-),(\boldsymbol{s}_7,0.23,+),(\boldsymbol{s}_8,0.15,-)
\end{aligned}$$

其中，$+$和$-$分别表示样本为正例和为反例，数字表示学习器$f$预测该样本为正例的概率，例如对于反例$\boldsymbol{s}_2$来说，当前学习器$f(\boldsymbol{s})$预测它是正例的概率为$0.62$。

根据"西瓜书"上给出的绘制方法，首先需要对所有测试样本按照学习器给出的预测结果进行排序 **（上面给出的预测结果已经按照预测值从大到小排序）** ，接着将分类阈值设为一个不可能取到的超大值，例如设为1。显然，此时所有样本预测为正例的概率都一定小于分类阈值，那么预测为正例的样本个数为0，相应的真正例率和假正例率也都为0，所以我们可以在坐标$(0,0)$处标记一个点。接下来需要把分类阈值从大到小依次设为每个
样本的预测值，也就是依次设为0.77, 0.62, 0.58, 0.47, 0.33,
0.23,0.15，然后分别计算真正例率和假正例率，再在相应的坐标上标记点，最后再将各个点用直线连接,
即可得到$\text{ROC}$曲线。需要注意的是，在统计预测结果时，预测值等于分类阈值的样本也被算作预测为正例。例如，当分类阈值为$0.77$时，测试样本
$\boldsymbol{s}_{1}$被预测为正例，由于它的真实标记也是正例，所以此时$\boldsymbol{s}_{1}$是一个真正例。为了便于绘图，我们将$x$轴（假正例率轴）的"步长"定为$\frac{1}{m^-}$，$y$轴（真正例率轴）的"步长"定为$\frac{1}{m^+}$。根据真正例率和假正例率的定义可知，每次变动分类阈值时，若新增$i$个假正例，那么相应的$x$轴坐标也就增加$\frac{i}{m^-}$；若新增$j$个真正例，那么相应的$y$轴坐标也就增加$\frac{j}{m^+}$。按照以上讲述的绘制流程，最终我们可以绘制出如图2-1所示的$\text{ROC}$曲线。

![图2-1 ROC曲线示意](https://datawhale-business.oss-cn-hangzhou.aliyuncs.com/06419b28-ca3b-4719-a6df-60f81dc9d44b-roc.svg)

在这里，为了能在解释式(2.21)时复用此图，我们没有写上具体的数值，转而用其数学符号代替。其中绿色线段表示在分类阈值变动的过程中只新增了真正例，红色线段表示只新增了假正例，蓝色线段表示既新增了真正例也新增了假正例。根据$\text{AUC}$值的定义可知，此时的$\text{AUC}$值其实就是所有红色线段和蓝色线段与$x$轴围成的面积之和。观察图2-1可知，红色线段与$x$轴围成的图形恒为矩形，蓝色线段与$x$轴围成的图形恒为梯形。由于梯形面积式既能算梯形面积，也能算矩形面积，所以无论是红色线段还是蓝色线段，其与$x$轴围成的面积都能用梯形公式来计算：

$$\frac{1}{2}\cdot(x_{i+1} - x_{i})\cdot(y_{i} + y_{i+1})$$

其中，$(x_{i+1} - x_i)$为"高"，$y_i$为"上底"，$y_{i+1}$为"下底"。那么对所有红色线段和蓝色线段与$x$轴围成的面积进行求和，则有

$$\sum_{i=1}^{m-1}\left[\frac{1}{2}\cdot(x_{i+1} - x_{i})\cdot(y_{i} + y_{i+1})\right]$$

此即为$\text{AUC}$。

通过以上$\text{ROC}$曲线的绘制流程可以看出，$\text{ROC}$曲线上每一个点都表示学习器$f(s)$在特定阈值下构成的一个二分类器，越好的二分类器其假正例率（反例被预测错误的概率，横轴）越小，真正例率（正例被预测正确的概率，纵轴）越大，所以这个点越靠左上角（即点$(0,1)$）越好。因此，越好的学习器，其$\text{ROC}$曲线上的点越靠左上角，相应的$\text{ROC}$曲线下的面积也越大，即AUC也越大。

### 2.3.9 式(2.21)和式(2.22)的推导

下面针对"西瓜书"上所说的"$\ell_{rank}$对应的是ROC曲线之上的面积"进行推导。按照我们上述对式(2.20)的推导思路，$\ell_{rank}$可以看作是所有绿色线段和蓝色线段与$y$轴围成的面积之和，但从式(2.21)中很难一眼看出其面积的具体计算方式，因此我们进行恒等变形如下：

$$\begin{aligned}
\ell_{rank}&=\frac{1}{m^+m^-}\sum_{\boldsymbol{x}^+ \in D^+}\sum_{\boldsymbol{x}^- \in D^-}\left(\mathbb{I}\left(f(\boldsymbol{x}^+)<f(\boldsymbol{x}^-)\right)+\frac{1}{2}\mathbb{I}\left(f(\boldsymbol{x}^+)=f(\boldsymbol{x}^-)\right)\right) \\
&=\frac{1}{m^+m^-}\sum_{\boldsymbol{x}^+ \in D^+}\left[\sum_{\boldsymbol{x}^- \in D^-}\mathbb{I}\left(f(\boldsymbol{x}^+)<f(\boldsymbol{x}^-)\right)+\frac{1}{2}\cdot\sum_{\boldsymbol{x}^- \in D^-}\mathbb{I}\left(f(\boldsymbol{x}^+)=f(\boldsymbol{x}^-)\right)\right] \\
&=\sum_{\boldsymbol{x}^+ \in D^+}\left[\frac{1}{m^+}\cdot\frac{1}{m^-}\sum_{\boldsymbol{x}^- \in D^-}\mathbb{I}\left(f(\boldsymbol{x}^+)<f(\boldsymbol{x}^-)\right)+\frac{1}{2}\cdot\frac{1}{m^+}\cdot\frac{1}{m^-}\sum_{\boldsymbol{x}^- \in D^-}\mathbb{I}\left(f(\boldsymbol{x}^+)=f(\boldsymbol{x}^-)\right)\right] \\
&=\sum_{\boldsymbol{x}^+ \in D^+}\frac{1}{2}\cdot\frac{1}{m^+}\cdot\left[\frac{2}{m^-}\sum_{\boldsymbol{x}^- \in D^-}\mathbb{I}\left(f(\boldsymbol{x}^+)<f(\boldsymbol{x}^-)\right)+\frac{1}{m^-}\sum_{\boldsymbol{x}^- \in D^-}\mathbb{I}\left(f(\boldsymbol{x}^+)=f(\boldsymbol{x}^-)\right)\right] \\
\end{aligned}$$

在变动分类阈值的过程当中，如果有新增真正例，那么图2-1就会相应地增加一条绿色线段或蓝色线段，所以上式中的$\sum_{\boldsymbol{x}^+ \in D^+}$可以看作是在累加所有绿色和蓝色线段，相应地，$\sum_{\boldsymbol{x}^+
\in D^+}$后面的内容便是在求绿色线段或者蓝色线段与$y$轴围成的面积，即：

$$\frac{1}{2}\cdot\frac{1}{m^+}\cdot\left[\frac{2}{m^-}\sum_{\boldsymbol{x}^- \in D^-}\mathbb{I}\left(f(\boldsymbol{x}^+)<f(\boldsymbol{x}^-)\right)+\frac{1}{m^-}\sum_{\boldsymbol{x}^- \in D^-}\mathbb{I}\left(f(\boldsymbol{x}^+)=f(\boldsymbol{x}^-)\right)\right]$$

与式(2.20)中的推导思路相同，不论是绿色线段还是蓝色线段，其与$y$轴围成的图形面积都可以用梯形公式来进行计算，所以上式表示的依旧是一个梯形的面积公式。其中$\frac{1}{m^+}$即梯形的"高"，中括号内便是"上底+下底"，下面我们来分别推导一下"上底"（较短的底）
和"下底"（较长的底）。

由于在绘制$\text{ROC}$曲线的过程中，每新增一个假正例时$x$坐标也就新增一个步长，所以对于"上底"，也就是绿色或者蓝色线段的下端点到$y$轴的距离，长度就等于$\frac{1}{m^-}$乘以预测值大于$f(\boldsymbol{x}^+)$的假正例的个数，即

$$\frac{1}{m^-}\sum\limits_{\boldsymbol{x}^- \in D^-}\mathbb{I}\left(f(\boldsymbol{x}^+)<f(\boldsymbol{x}^-)\right)$$

而对于"下底"，长度就等于$\frac{1}{m^-}$乘以预测值大于等于$f(\boldsymbol{x}^+)$的假正例的个数，即

$$\frac{1}{m^-}\left(\sum_{\boldsymbol{x}^- \in D^-}\mathbb{I}\left(f(\boldsymbol{x}^+)<f(\boldsymbol{x}^-)\right)+\sum_{\boldsymbol{x}^- \in D^-}\mathbb{I}\left(f(\boldsymbol{x}^+)=f(\boldsymbol{x}^-)\right)\right)$$

到此，推导完毕。

若不考虑$f(\boldsymbol{x}^+)=f(\boldsymbol{x}^-)$，从直观上理解$\ell_{rank}$，其表示的是：对于待测试的模型$f(\boldsymbol{x})$，从测试集中随机抽取一个正反例对儿$\{\boldsymbol{x}^{+},\boldsymbol{x}^{-}\}$，模型$f(\boldsymbol{x})$对正例的打分$f(\boldsymbol{x}^{+})$小于对反例的打分$f(\boldsymbol{x}^{-})$的概率，即"排序错误"的概率。推导思路如下：采用频率近似概率的思路，组合出测试集中的所有正反例对儿，假设组合出来的正反例对儿的个数为$m$，用模型$f(\boldsymbol{x})$对所有正反例对儿打分并统计"排序错误"的正反例对儿个数$n$，然后计算出$\frac{n}{m}$即为模型$f(\boldsymbol{x})$"排序错误"的正反例对儿的占比，其可近似看作为$f(\boldsymbol{x})$在测试集上"排序错误"的概率。具体推导过程如下：测试集中的所有正反例对儿的个数为

$$m^+\times m^-$$ 

"排序错误"的正反例对儿个数为

$$\sum_{\boldsymbol{x}^+ \in D^+}\sum_{\boldsymbol{x}^- \in D^-}\left(\mathbb{I}\left(f(\boldsymbol{x}^+)<f(\boldsymbol{x}^-)\right)\right)$$

因此，"排序错误"的概率为

$$\frac{\sum_{\boldsymbol{x}^+ \in D^+}\sum_{\boldsymbol{x}^- \in D^-}\left(\mathbb{I}\left(f(\boldsymbol{x}^+)<f(\boldsymbol{x}^-)\right)\right)}{m^+\times m^-}$$

若再考虑$f(\boldsymbol{x}^+)=f(\boldsymbol{x}^-)$时算半个"排序错误"，则上式可进一步扩展为

$$\frac{\sum_{\boldsymbol{x}^+ \in D^+}\sum_{\boldsymbol{x}^- \in D^-}\left(\mathbb{I}\left(f(\boldsymbol{x}^+)<f(\boldsymbol{x}^-)+\frac{1}{2}\mathbb{I}\left(f(\boldsymbol{x}^+)=f(\boldsymbol{x}^-)\right)\right)\right)}{m^+\times m^-}$$

此即为$\ell_{rank}$。

如果说$\ell_{rank}$指的是从测试集中随机抽取正反例对儿，模型$f(\boldsymbol{x})$"排序错误"的概率，那么根据式(2.22)可知，AUC则指的是从测试集中随机抽取正反例对儿，模型$f(\boldsymbol{x})$"排序正确"的概率。显然，此概率越大越好。

### 2.3.10 式(2.23)的解释

本公式很容易理解，只是需要注意该公式上方交代了"若将表2.2中的第0类作为正类、第1类作为反类"，若不注意此条件，按习惯（0为反类、1为正类）会产生误解。为避免产生误解，在接下来的解释中将$cost_{01}$记为$cost_{+-}$，$cost_{10}$记为$cost_{-+}$。本公式还可以作如下恒等变形

$$\begin{aligned}
E(f;D;cost) &=\frac{1}{m}\left(m^{+} \times \frac{1}{m^{+}} \sum_{\boldsymbol{x}_{i} \in D^{+}} \mathbb{I}\left(f\left(\boldsymbol{x}_{i} \neq y_{i}\right)\right) \times cost_{+-}+m^{-} \times \frac{1}{m^{-}} \sum_{\boldsymbol{x}_{i} \in D^{-}} \mathbb{I}\left(f\left(\boldsymbol{x}_{i} \neq y_{i}\right)\right) \times cost_{-+}\right) \\
&=\frac{m^{+}}{m} \times \frac{1}{m^{+}} \sum_{\boldsymbol{x}_{i} \in D^{+}} \mathbb{I}\left(f\left(\boldsymbol{x}_{i} \neq y_{i}\right)\right) \times cost_{+-}+\frac{m^{-}}{m} \times \frac{1}{m^{-}} \sum_{\boldsymbol{x}_{i} \in D^{-}} \mathbb{I}\left(f\left(\boldsymbol{x}_{i} \neq y_{i}\right)\right) \times cost_{-+}
\end{aligned}$$

其中$m^{+}$和$m^{-}$分别表示正例集$D^{+}$和反例集$D^{-}$的样本个数。

$\frac{1}{m^{+}} \sum_{\boldsymbol{x}_{i} \in D^{+}} \mathbb{I}\left(f\left(\boldsymbol{x}_{i} \neq y_{i}\right)\right)$表示正例集$D^{+}$中预测错误样本所占比例，即假反例率FNR。

$\frac{1}{m^{-}} \sum_{\boldsymbol{x}_{i} \in D^{-}} \mathbb{I}\left(f\left(\boldsymbol{x}_{i} \neq y_{i}\right)\right)$表示反例集$D^{-}$中预测错误样本所占比例，即假正例率FPR。

$\frac{m^{+}}{m}$表示样例集$D$中正例所占比例，或理解为随机从$D$中取一个样例取到正例的概率。

$\frac{m^{-}}{m}$表示样例集$D$中反例所占比例，或理解为随机从$D$中取一个样例取到反例的概率。

因此，若将样例为正例的概率$\frac{m^{+}}{m}$记为$p$，则样例为f反例的概率$\frac{m^{-}}{m}$为$1-p$，上式可进一步写为

$$E(f;D;cost)=p \times \mathrm{FNR} \times cost_{+-}+(1-p) \times \mathrm{FPR} \times cost_{-+}$$

此公式在接下来式(2.25)的解释中会用到。

### 2.3.11 式(2.24)的解释

当$cost_{+-}=cost_{-+}$时，本公式可化简为

$$P(+)cost=\frac{p}{p+(1-p)}=p$$

其中$p$是样例为正例的概率（一般用正例在样例集中所占的比例近似代替）。因此，当代价不敏感时（也即$cost_{+-}=cost_{-+}$），$P(+)cost$就是正例在样例集中的占比。那么，当代价敏感时（也即$cost_{+-}\neq cost_{-+}$），$P(+)cost$即为正例在样例集中的加权占比。具体来说，对于样例集

$$D=\left\{\boldsymbol{x}_{1}^{+}, \boldsymbol{x}_{2}^{+}, \boldsymbol{x}_{3}^{-}, \boldsymbol{x}_{4}^{-}, \boldsymbol{x}_{5}^{-}, \boldsymbol{x}_{6}^{-}, \boldsymbol{x}_{7}^{-}, \boldsymbol{x}_{8}^{-}, \boldsymbol{x}_{9}^{-}, \boldsymbol{x}_{10}^{-}\right\}$$

其中$\boldsymbol{x}^{+}$表示正例，$\boldsymbol{x}^{-}$表示反例。可以看出$p=0.2$，若想让正例得到更多重视，考虑代价敏感$cost_{+-}=4$和$cost_{-+}=1$，这实际等价于在以下样例集上进行代价不敏感的正例概率代价计算

$$D^{\prime}=\left\{\boldsymbol{x}_{1}^{+}, \boldsymbol{x}_{1}^{+}, \boldsymbol{x}_{1}^{+}, \boldsymbol{x}_{1}^{+}, \boldsymbol{x}_{2}^{+}, \boldsymbol{x}_{2}^{+}, \boldsymbol{x}_{2}^{+}, \boldsymbol{x}_{2}^{+}, \boldsymbol{x}_{3}^{-}, \boldsymbol{x}_{4}^{-}, \boldsymbol{x}_{5}^{-}, \boldsymbol{x}_{6}^{-}, \boldsymbol{x}_{7}^{-}, \boldsymbol{x}_{8}^{-}, \boldsymbol{x}_{9}^{-}, \boldsymbol{x}_{10}^{-}\right\}$$

即将每个正例样本复制4份，若有1个出错，则有4个一起出错，代价为4。此时可计算出

$$\begin{aligned}
P(+) cost &=\frac{p \times cost_{+-}}{p \times cost_{+-}+(1-p) \times cost_{-+}} \\
&=\frac{0.2 \times 4}{0.2 \times 4+(1-0.2) \times 1}=0.5
\end{aligned}$$

也就是正例在等价的样例集$D^{\prime}$中的占比。所以，无论代价敏感还是不敏感，$P(+)cost$本质上表示的都是样例集中正例的占比。在实际应用过程中，如果由于某种原因无法将$cost_{+-}$和$cost_{-+}$设为不同取值，可以采用上述"复制样本"的方法间接实现将$cost_{+-}$和$cost_{-+}$设为不同取值。

对于不同的$cost_{+-}$和$cost_{-+}$取值，若二者的比值保持相同，则$P(+)cost$不变。例如，对于上面的例子，若设$cost_{+-}=40$和$cost_{-+}=10$，所得$P(+)cost$仍为0.5。

此外，根据此式还可以相应地推导出反例概率代价

$$P(-) cost=1-P(+) cost=\frac{(1-p) \times cost_{-+}}{p \times cost_{+-}+(1-p) \times cost_{-+}}$$

### 2.3.12 式(2.25)的解释

对于包含$m$个样本的样例集$D$，可以算出学习器$f(\boldsymbol{x})$总的代价是

$$\begin{aligned}
cost_{se} &=m \times p \times \mathrm{FNR} \times cost_{+-}+m \times(1-p) \times \mathrm{FPR} \times cost_{-+} \\
&+m \times p \times \mathrm{TPR} \times cost_{++}+m \times(1-p) \times \mathrm{TNR} \times cost_{--}
\end{aligned}$$

其中$p$是正例在样例集中所占的比例（或严格地称为样例为正例的概率），$cost_{se}$下标中的"se"表示sensitive，即代价敏感，根据前面讲述的FNR、FPR、TPR、TNR的定义可知：

$m \times p \times \mathrm{FNR}$表示正例被预测为反例（正例预测错误）的样本个数；

$m \times(1-p) \times \mathrm{FPR}$表示反例被预测为正例（反例预测错误）的样本个数；

$m \times p \times \mathrm{TPR}$表示正例被预测为正例（正例预测正确）的样本个数；

$m \times(1-p) \times \mathrm{TNR}$表示反例预测为反例（反例预测正确）的样本个数。

以上各种样本个数乘以相应的代价则得到总的代价$cost_{se}$。但是，按照此公式计算出的代价与样本个数$m$呈正比，显然不具有一般性，因此需要除以样本个数$m$，而且一般来说，预测出错才会产生代价，预测正确则没有代价，也即$cost_{++}=cost_{--}=0$，所以$cost_{se}$更为一般化的表达式为

$$cost_{se} = p \times \mathrm{FNR} \times cost_{+-}+(1-p) \times \mathrm{FPR} \times cost_{-+}$$

回顾式(2.23)的解释可知，此式即为式(2.23)的恒等变形，所以此式可以同式(2.23)一样理解为学习器$f(\boldsymbol{x})$在样例集$D$上的"代价敏感错误率"。显然，$cost_{se}$的取值范围并不在0到1之间，且$cost_{se}$在$\mathrm{FNR}=\mathrm{FPR}=1$时取到最大值，因为$\mathrm{FNR}=\mathrm{FPR}=1$时表示所有正例均被预测为反例，反例均被预测为正例，代价达到最大，即

$$max(cost_{se})=p \times cost_{+-}+(1-p) \times cost_{-+}$$

所以，如果要将$cost_{se}$的取值范围归一化到0到1之间，则只需将其除以其所能取到的最大值即可，也即

$$\frac{cost_{se}}{max(cost_{se})}=\frac{p \times \mathrm{FNR} \times cost_{+-}+(1-p) \times \mathrm{FPR} \times cost_{-+}}{p \times cost_{+-}+(1-p) \times cost_{-+}}$$

此即为式(2.25)，也即为$cost_{norm}$，其中下标"norm"表示normalization。

进一步地，根据式(2.24)中$P(+)cost$的定义可知，式(2.25)可以恒等变形为

$$cost_{norm}=\mathrm{FNR} \times P(+)cost+\mathrm{FPR} \times(1-P(+)cost)$$

对于二维直角坐标系中的两个点$(0,B)$和$(1,A)$以及实数$p\in[0,1]$，$(p,pA+(1-p)B)$一定是线段$A-B$上的点，且当$p$从0变到1时，点$(p,pA+(1-p)B)$的轨迹为从$(0,B)$到$(1,A)$，基于此，结合上述$cost_{norm}$的表达式可知：$(P(+)cost, cost_{norm})$即为线段$\mathrm{FPR}-\mathrm{FNR}$上的点，当$(P(+)cost$从0变到1时，$(P(+)cost, cost_{norm})$的轨迹为从$(0,\mathrm{FPR})$到$(1,\mathrm{FNR})$
，也即图2.5中的各条线段。需要注意的是，以上只是从数学逻辑自洽的角度对图2.5中的各条线段进行解释，实际中各条线段并非按照上述方法绘制。理由如下：

$P(+)cost$表示的是样例集中正例的占比，而在进行学习器的比较时，变动的只是训练学习器的算法或者算法的超参数，用来评估学习器性能的样例集是固定的（单一变量原则），所以$P(+)cost$是一个固定值，因此图2.5中的各条线段并不是通过变动$P(+)cost$然后计算$cost_{norm}$画出来的，而是按照"西瓜书"上式(2.25)下方所说对ROC曲线上每一点计算FPR和FNR，然后将点$(0,\mathrm{FPR})$和点$(1,\mathrm{FNR})$直接连成线段。

虽然图2.5中的各条线段并不是通过变动横轴表示的$P(+)cost$来进行绘制，但是横轴仍然有其他用处，例如用来找使学习器的归一化代价$cost_{norm}$达到最小的阈值（暂且称其为最佳阈值）。具体地，首先计算当前样例集的$P(+)cost$值，然后根据计算出来的值在横轴上标记出具体的点，再基于该点作一条垂直于横轴的垂线，与该垂线最先相交（从下往上看）的线段所对应的阈值（因为每条线段都对应ROC曲线上的点，ROC曲线上的点又对应着具体的阈值）即为最佳阈值。原因是与该垂线最先相交的线段必然最靠下，因此其交点的纵坐标最小，而纵轴表示的便是归一化代价$cost_{norm}$，所以此时归一化代价$cost_{norm}$达到最小。特别地，当$P(+)cost=0$时，即样例集中没有正例，全是负例，因此最佳阈值应该是学习器不可能取到的最大值，且按照此阈值计算出来出来的$\mathrm{FPR}=0,\mathrm{FNR}=1,cost_{norm}=0$。那么按照上述作垂线的方法去图2.5中进行实验，也即在横轴0刻度处作垂线，显然与该垂线最先相交的线段是点$(0,0)$和点$(1,1)$连成的线段，交点为$(0,0)$，此时对应的也为$\mathrm{FPR}=0,\mathrm{FNR}=1,cost_{norm}=0$，且该条线段所对应的阈值也确实为"学习器不可能取到的最大值"（因为该线段对应的是ROC曲线中的起始点）。

## 2.4 比较检验

为什么要做比较检验？"西瓜书"在本节开篇的两段话已经交代原由。简单来说，从统计学的角度，取得的性能度量的值本质上仍是一个随机变量，因此并不能简单用比较大小来直接判定算法（或者模型）之间的优劣，而需要更置信的方法来进行判定。

在此说明一下，如果不做算法理论研究，也不需要对算法（或模型）之间的优劣给出严谨的数学分析，本节可以暂时跳过。本节主要使用的数学知识是"统计假设检验"，该知识点在各个高校的概率论与数理统计教材（例如参考文献[1]）上均有讲解。此外，有关检验变量的公式，例如式(2.30)至式(2.36)，并不需要清楚是怎么来的（这是统计学家要做的事情），只需要会用即可。

### 2.4.1 式(2.26)的解释

理解本公式时需要明确的是：$\epsilon$是未知的，是当前希望估算出来的，$\hat{\epsilon}$是已知的，是已经用$m$个测试样本对学习器进行测试得到的。因此，本公式也可理解为：当学习器的泛化错误率为$\epsilon$时，被测得测试错误率为$\hat{\epsilon}$的条件概率。所以本公式可以改写为

$$P(\hat{\epsilon}|\epsilon)=\left(\begin{array}{c}
m \\
\hat{\epsilon} \times m
\end{array}\right) \epsilon^{\hat{\epsilon} \times m}(1-\epsilon)^{m-\hat{\epsilon} \times m}$$

其中

$$\left(\begin{array}{c}
m \\
\hat{\epsilon} \times m
\end{array}\right)=\frac{m !}{(\hat{\epsilon} \times m) !(m-\hat{\epsilon} \times m) !}$$

为中学时学的组合数，即$C_{m}^{\hat{\epsilon} \times m}$。

在已知$\hat{\varepsilon}$时，求使得条件概率$P(\hat{\epsilon}|\epsilon)$达到最大的$\epsilon$是概率论与数理统计中经典的极大似然估计问题。从极大似然估计的角度可知，由于$\hat{\epsilon},m$均为已知量，所以$P(\hat{\epsilon}|\epsilon)$可以看作为一个关于$\epsilon$的函数，称为似然函数，于是问题转化为求使得似然函数取到最大值的$\epsilon$，即

$$\epsilon=\mathop{\arg\max}_{\epsilon}P(\hat{\epsilon}|\epsilon)$$

首先对$\epsilon$求一阶导数

$$\begin{aligned}
\frac{\partial P(\hat{\epsilon} \mid \epsilon)}{\partial \epsilon} &=\left(\begin{array}{c}
m \\
\hat{\epsilon} \times m
\end{array}\right) \frac{\partial \epsilon^{\hat{\epsilon} \times m}(1-\epsilon)^{m-\hat{\epsilon} \times m}}{\partial \epsilon} \\
&=\left(\begin{array}{c}
m \\
\hat{\epsilon} \times m
\end{array}\right)\left(\hat{\epsilon} \times m \times \epsilon^{\hat{\epsilon} \times m-1}(1-\epsilon)^{m-\hat{\epsilon} \times m}+\epsilon^{\hat{\epsilon} \times m} \times(m-\hat{\epsilon} \times m) \times(1-\epsilon)^{m-\hat{\epsilon} \times m-1} \times(-1)\right) \\
&=\left(\begin{array}{c}
m \\
\hat{\epsilon} \times m
\end{array}\right) \epsilon^{\hat{\epsilon} \times m-1}(1-\epsilon)^{m-\hat{\epsilon} \times m-1}(\hat{\epsilon} \times m \times(1-\epsilon)-\epsilon \times(m-\hat{\epsilon} \times m)) \\
&=\left(\begin{array}{c}
m \\
\hat{\epsilon} \times m
\end{array}\right) \epsilon^{\hat{\epsilon} \times m-1}(1-\epsilon)^{m-\hat{\epsilon} \times m-1}(\hat{\epsilon} \times m-\epsilon \times m)
\end{aligned}$$

分析上式可知，其中$\left(\begin{array}{c}
m \\
\hat{\epsilon} \times m
\end{array}\right)$为常数，由于$\epsilon \in [0,1]$，所以$\epsilon^{\hat{\epsilon} \times m-1}(1-\epsilon)^{m-\hat{\epsilon} \times m-1}$恒大于0，$(\hat{\epsilon} \times m-\epsilon \times m)$在$0\leqslant\epsilon<\hat{\epsilon}$时大于0，在$\epsilon=\hat{\epsilon}$时等于0，在$\hat{\epsilon}\leqslant\epsilon<1$时小于0，因此$P(\hat{\epsilon} \mid \epsilon)$是关于$\epsilon$开口向下的凹函数（此处采用的是最优化中对凹凸函数的定义，"西瓜书"第3章3.2节左侧边注对凹凸函数的定义也是如此）。所以，当且仅当一阶导数$\frac{\partial P(\hat{\epsilon} \mid \epsilon)}{\partial \epsilon}=0$时$P(\hat{\epsilon} \mid \epsilon)$取到最大值，此时$\epsilon=\hat{\epsilon}$。

### 2.4.2 式(2.27)的推导

截至2021年5月，"西瓜书"第1版第36次印刷，式(2.27)应当勘误为

$$\overline{\epsilon}=\min \epsilon\quad\text { s.t. } \sum_{i=\epsilon\times m+1}^{m}\left(\begin{array}{c}{m} \\ {i}\end{array}\right) \epsilon_0^{i}(1-\epsilon_0)^{m-i}<\alpha$$

在推导此公式之前，先铺垫讲解一下"二项分布参数$p$的假设检验"[1]：

设某事件发生的概率为$p$，$p$未知。做$m$次独立试验，每次观察该事件是否发生，以$X$记该事件发生的次数，则$X$服从二项分布$B(m,p)$，现根据$X$检验如下假设：

$$\begin{aligned}
H_0:p\leqslant p_0\\
H_1:p > p_0
\end{aligned}$$

由二项分布本身的特性可知：$p$越小，$X$取到较小值的概率越大。因此，对于上述假设，一个直观上合理的检验为

$$\varphi:\text{当}X>C\text{时拒绝}H_0,\text{否则就接受}H_0\text{。}$$

其中，$C$表示事件最大发生次数。此检验对应的功效函数为

$$\begin{aligned}
\beta_{\varphi}(p)&=P(X>C)\\
&=1-P(X\leqslant C) \\
&=1-\sum_{i=0}^{C}\left(\begin{array}{c}{m} \\ {i}\end{array}\right) p^{i} (1-p)^{m-i} \\
&=\sum_{i=C+1}^{m}\left(\begin{array}{c}{m} \\ {i}\end{array}\right) p^{i} (1-p)^{m-i} \\
\end{aligned}$$

由于"$p$越小，$X$取到较小值的概率越大"可以等价表示为：$P(X\leqslant C)$是关于$p$的减函数，所以$\beta_{\varphi}(p)=P(X>C)=1-P(X\leqslant C)$是关于$p$的增函数，那么当$p\leqslant p_0$时，$\beta_{\varphi}(p_0)$即为$\beta_{\varphi}(p)$的上确界。 **（更为严格的数学证明参见参考文献[1]中第2章习题7）** 又根据参考文献[1]中5.1.3的定义1.2可知，在给定检验水平$\alpha$时，要想使得检验$\varphi$达到水平$\alpha$，则必须保证$\beta_{\varphi}(p)\leqslant\alpha$，因此可以通过如下方程解得使检验$\varphi$达到水平$\alpha$的整数$C$：

$$\alpha =\sup \left\{\beta_{\varphi}(p)\right\}$$

显然，当$p\leqslant p_0$时有

$$\begin{aligned}
\alpha &=\sup \left\{\beta_{\varphi}(p)\right\} \\
&=\beta_{\varphi}(p_0) \\
&=\sum_{i=C+1}^{m}\left(\begin{array}{c}{m} \\ {i}\end{array}\right) p_0^{i} (1-p_0)^{m-i}
\end{aligned}$$

对于此方程，通常不一定正好解得一个使得方程成立的整数$C$，较常见的情况是存在这样一个$\overline{C}$使得

$$\begin{aligned}
\sum_{i=\overline{C}+1}^{m}\left(\begin{array}{c}{m} \\ {i}\end{array}\right) p_0^{i} (1-p_0)^{m-i}<\alpha \\
\sum_{i=\overline{C}}^{m}\left(\begin{array}{c}{m} \\ {i}\end{array}\right) p_0^{i} (1-p_0)^{m-i}>\alpha
\end{aligned}$$

此时，$C$只能取$\overline{C}$或者$\overline{C}+1$。若$C$取$\overline{C}$，
则相当于升高了检验水平$\alpha$；若$C$取$\overline{C}+1$则相当于降低了检验水平$\alpha$。具体如何取舍需要结合实际情况，一般的做法是使$\alpha$尽可能小，因此倾向于令$C$取$\overline{C}+1$。

下面考虑如何求解$\overline{C}$。易证$\beta_{\varphi}(p_0)$是关于$C$的减函数，再结合上述关于$\overline{C}$的两个不等式易推得

$$\overline{C}=\min C\quad\text { s.t. } \sum_{i=C+1}^{m}\left(\begin{array}{c}{m} \\ {i}\end{array}\right) p_0^{i}(1-p_0)^{m-i}<\alpha$$

由"西瓜书"中的上下文可知，对$\epsilon\leqslant\epsilon_0$进行假设检验，等价于"二项分布参数$p$的假设检验"中所述的对$p\leqslant p_0$进行假设检验，所以在"西瓜书"中求解最大错误率$\overline{\epsilon}$等价于在"二项分布参数$p$的假设检验"中求解事件最大发生频率$\frac{\overline{C}}{m}$。由上述"二项分布参数$p$的假设检验"中的推导可知

$$\overline{C}=\min C\quad\text { s.t. } \sum_{i=C+1}^{m}\left(\begin{array}{c}{m} \\ {i}\end{array}\right) p_0^{i}(1-p_0)^{m-i}<\alpha$$

所以

$$\frac{\overline{C}}{m}=\min \frac{C}{m}\quad\text { s.t. } \sum_{i=C+1}^{m}\left(\begin{array}{c}{m} \\ {i}\end{array}\right) p_0^{i}(1-p_0)^{m-i}<\alpha$$

将上式中的$\frac{\overline{C}}{m},\frac{C}{m},p_0$等价替换为$\overline{\epsilon},\epsilon,\epsilon_0$可得

$$\overline{\epsilon}=\min \epsilon\quad\text { s.t. } \sum_{i=\epsilon\times m+1}^{m}\left(\begin{array}{c}{m} \\ {i}\end{array}\right) \epsilon_0^{i}(1-\epsilon_0)^{m-i}<\alpha$$

## 2.5 偏差与方差

### 2.5.1 式(2.37)到式(2.42)的推导

首先，梳理一下"西瓜书"中的符号，书中称$\boldsymbol{x}$为测试样本，但是书中又提到"令$y_{D}$为$\boldsymbol{x}$在数据集中的标记"，那么$\boldsymbol{x}$究竟是测试集中的样本还是训练集中的样本呢？这里暂且理解为$\boldsymbol{x}$为从训练集中抽取出来用于测试的样本。此外，"西瓜书"中左侧边注中提到"有可能出现噪声使得$y_{D}\neq y$"，其中所说的"噪声"通常是指人工标注数据时带来的误差，例如标注"身高"时，由于测量工具的精度等问题，测出来的数值必然与真实的"身高"之间存在一定误差，此即为"噪声"。

为了进一步解释式(2.37)、(2.38)和(2.39)，在这里设有$n$个训练集$D_{1},...,D_{n}$，这$n$个训练集都是以独立同分布的方式从样本空间中采样而得，并且恰好都包含测试样本$\boldsymbol{x}$，该样本在这$n$个训练集的标记分别为$y_{D_{1}},...,y_{D_{n}}$。书中已明确，此处以回归任务为例，也即$y_{D},y,f(\boldsymbol{x} ; D)$均为实值。

式(2.37)可理解为：

$$\bar{f}(\boldsymbol{x})=\mathbb{E}_{D}[f(\boldsymbol{x} ; D)]=\frac{1}{n}\left(f\left(\boldsymbol{x} ; D_{1}\right)+\ldots+f\left(\boldsymbol{x} ; D_{n}\right)\right)$$

式(2.38)可理解为： 

$$\begin{aligned}
\operatorname{var}(\boldsymbol{x}) &=\mathbb{E}_{D}\left[(f(\boldsymbol{x} ; D)-\bar{f}(\boldsymbol{x}))^{2}\right] \\
&=\frac{1}{n}\left(\left(f\left(\boldsymbol{x} ; D_{1}\right)-\bar{f}(\boldsymbol{x})\right)^{2}+\ldots+\left(f\left(\boldsymbol{x} ; D_{n}\right)-\bar{f}(\boldsymbol{x})\right)^{2}\right)
\end{aligned}$$

式(2.39)可理解为：

$$\varepsilon^{2}=\mathbb{E}_{D}\left[\left(y_{D}-y\right)^{2}\right]=\frac{1}{n}\left(\left(y_{D_{1}}-y\right)^{2}+\ldots+\left(y_{D_{n}}-y\right)^{2}\right)$$

最后，推导一下式(2.41)和式(2.42)，由于推导完式(2.41)自然就会得到式(2.42)，因此下面仅推导式(2.41)即可。

$$\begin{aligned} 
E(f ; D)=& \mathbb{E}_{D}\left[\left(f(\boldsymbol{x} ; D)-y_{D}\right)^{2}\right] &\textcircled{1}\\
=& \mathbb{E}_{D}\left[\left(f(\boldsymbol{x} ; D)-\bar{f}(\boldsymbol{x})+\bar{f}(\boldsymbol{x})-y_{D}\right)^{2}\right] &\textcircled{2}\\
=& \mathbb{E}_{D}\left[\left(f(\boldsymbol{x} ; D)-\bar{f}(\boldsymbol{x})\right)^{2}\right]+\mathbb{E}_{D}\left[\left(\bar{f}(\boldsymbol{x})-y_{D}\right)^{2}\right]+ \\ &\mathbb{E}_{D}\left[2\left(f(\boldsymbol{x} ; D)-\bar{f}(\boldsymbol{x})\right)\left(\bar{f}(\boldsymbol{x})-y_{D}\right)\right] &\textcircled{3}\\
=& \mathbb{E}_{D}\left[\left(f(\boldsymbol{x} ; D)-\bar{f}(\boldsymbol{x})\right)^{2}\right]+\mathbb{E}_{D}\left[\left(\bar{f}(\boldsymbol{x})-y_{D}\right)^{2}\right] &\textcircled{4}\\
=& \mathbb{E}_{D}\left[\left(f(\boldsymbol{x} ; D)-\bar{f}(\boldsymbol{x})\right)^{2}\right]+\mathbb{E}_{D}\left[\left(\bar{f}(\boldsymbol{x})-y+y-y_{D}\right)^{2}\right] &\textcircled{5}\\
=& \mathbb{E}_{D}\left[\left(f(\boldsymbol{x} ; D)-\bar{f}(\boldsymbol{x})\right)^{2}\right]+\mathbb{E}_{D}\left[\left(\bar{f}(\boldsymbol{x})-y\right)^{2}\right]+\mathbb{E}_{D}\left[\left(y-y_{D}\right)^{2}\right]+ \\ &2 \mathbb{E}_{D}\left[\left(\bar{f}(\boldsymbol{x})-y\right)\left(y-y_{D}\right)\right]&\textcircled{6}\\
=& \mathbb{E}_{D}\left[\left(f(\boldsymbol{x} ; D)-\bar{f}(\boldsymbol{x})\right)^{2}\right]+\left(\bar{f}(\boldsymbol{x})-y\right)^{2}+\mathbb{E}_{D}\left[\left(y_{D}-y\right)^{2}\right] &\textcircled{7}
\end{aligned}$$

上式即为式(2.41)，下面给出每一步的推导过程：

$\textcircled{1}\to\textcircled{2}$：减一个$\bar{f}(\boldsymbol{x})$再加一个$\bar{f}(\boldsymbol{x})$，属于简单的恒等变形。

$\textcircled{2}\to\textcircled{3}$：首先将中括号内的式子展开，有

$$\mathbb{E}_{D}\left[\left(f(\boldsymbol{x} ; D)-\bar{f}(\boldsymbol{x})\right)^{2}+\left(\bar{f}(\boldsymbol{x})-y_{D}\right)^{2}+2\left(f(\boldsymbol{x} ; D)-\bar{f}(\boldsymbol{x})\right)\left(\bar{f}(\boldsymbol{x})-y_{D}\right)\right]$$

然后根据期望的运算性质$\mathbb{E}[X+Y]=\mathbb{E}[X]+\mathbb{E}[Y]$可将上式化为

$$\mathbb{E}_{D}\left[\left(f(\boldsymbol{x} ; D)-\bar{f}(\boldsymbol{x})\right)^{2}\right]+\mathbb{E}_{D}\left[\left(\bar{f}(\boldsymbol{x})-y_{D}\right)^{2}\right] +\mathbb{E}_{D}\left[2\left(f(\boldsymbol{x} ; D)-\bar{f}(\boldsymbol{x})\right)\left(\bar{f}(\boldsymbol{x})-y_{D}\right)\right]$$

$\textcircled{3}\to\textcircled{4}$：再次利用期望的运算性质将$\textcircled{3}$的最后一项展开，有

$$\mathbb{E}_{D}\left[2\left(f(\boldsymbol{x} ; D)-\bar{f}(\boldsymbol{x})\right)\left(\bar{f}(\boldsymbol{x})-y_{D}\right)\right] = \mathbb{E}_{D}\left[2\left(f(\boldsymbol{x} ; D)-\bar{f}(\boldsymbol{x})\right)\cdot\bar{f}(\boldsymbol{x})\right] - \mathbb{E}_{D}\left[2\left(f(\boldsymbol{x} ; D)-\bar{f}(\boldsymbol{x})\right)\cdot y_{D}\right]$$

首先计算展开后得到的第1项，有

$$\mathbb{E}_{D}\left[2\left(f(\boldsymbol{x} ; D)-\bar{f}(\boldsymbol{x})\right)\cdot\bar{f}(\boldsymbol{x})\right] = \mathbb{E}_{D}\left[2f(\boldsymbol{x} ; D)\cdot\bar{f}(\boldsymbol{x})-2\bar{f}(\boldsymbol{x})\cdot\bar{f}(\boldsymbol{x})\right]$$

由于$\bar{f}(\boldsymbol{x})$是常量，所以由期望的运算性质：$\mathbb{E}[AX+B]=A\mathbb{E}[X]+B$（其中$A,B$均为常量）可得

$$\mathbb{E}_{D}\left[2\left(f(\boldsymbol{x} ; D)-\bar{f}(\boldsymbol{x})\right)\cdot\bar{f}(\boldsymbol{x})\right] = 2\bar{f}(\boldsymbol{x})\cdot\mathbb{E}_{D}\left[f(\boldsymbol{x} ; D)\right]-2\bar{f}(\boldsymbol{x})\cdot\bar{f}(\boldsymbol{x})$$

由式(2.37)可知$\mathbb{E}_{D}\left[f(\boldsymbol{x} ; D)\right]=\bar{f}(\boldsymbol{x})$，所以

$$\mathbb{E}_{D}\left[2\left(f(\boldsymbol{x} ; D)-\bar{f}(\boldsymbol{x})\right)\cdot\bar{f}(\boldsymbol{x})\right] = 2\bar{f}(\boldsymbol{x})\cdot\bar{f}(\boldsymbol{x})-2\bar{f}(\boldsymbol{x})\cdot\bar{f}(\boldsymbol{x})=0$$
接着计算展开后得到的第2项

$$
\mathbb{E}_{D}\left[2\left(f(\boldsymbol{x} ; D)-\bar{f}(\boldsymbol{x})\right)\cdot y_{D}\right]=2\mathbb{E}_{D}\left[f(\boldsymbol{x} ; D)\cdot y_{D}\right]-2\bar{f}(\boldsymbol{x})\cdot \mathbb{E}_{D}\left[y_{D}\right]
$$

由于噪声和$f$无关，所以$f(\boldsymbol{x} ; D)$和$y_D$是两个相互独立的随机变量。根据期望的运算性质$\mathbb{E}[XY]=\mathbb{E}[X]\mathbb{E}[Y]$（其中$X$和$Y$为相互独立的随机变量）可得

$$\begin{aligned} 
\mathbb{E}_{D}\left[2\left(f(\boldsymbol{x} ; D)-\bar{f}(\boldsymbol{x})\right)\cdot y_{D}\right]&=2\mathbb{E}_{D}\left[f(\boldsymbol{x} ; D)\cdot y_{D}\right]-2\bar{f}(\boldsymbol{x})\cdot \mathbb{E}_{D}\left[y_{D}\right]\\
&=2\mathbb{E}_{D}\left[f(\boldsymbol{x} ; D)\right]\cdot \mathbb{E}_{D}\left[y_{D}\right]-2\bar{f}(\boldsymbol{x})\cdot \mathbb{E}_{D}\left[y_{D}\right]\\
&=2\bar{f}(\boldsymbol{x})\cdot \mathbb{E}_{D}\left[y_{D}\right]-2\bar{f}(\boldsymbol{x})\cdot \mathbb{E}_{D}\left[y_{D}\right]\\
&= 0
\end{aligned}$$

所以

$$\begin{aligned} \mathbb{E}_{D}\left[2\left(f(\boldsymbol{x} ; D)-\bar{f}(\boldsymbol{x})\right)\left(\bar{f}(\boldsymbol{x})-y_{D}\right)\right] &= \mathbb{E}_{D}\left[2\left(f(\boldsymbol{x} ; D)-\bar{f}(\boldsymbol{x})\right)\cdot\bar{f}(\boldsymbol{x})\right] - \mathbb{E}_{D}\left[2\left(f(\boldsymbol{x} ; D)-\bar{f}(\boldsymbol{x})\right)\cdot y_{D}\right] \\
&= 0+0 \\
&=0
\end{aligned}$$

$\textcircled{4}\to\textcircled{5}$：同$\textcircled{1}\to\textcircled{2}$一样，减一个$y$再加一个$y$，属于简单的恒等变形。

$\textcircled{5}\to\textcircled{6}$：同$\textcircled{2}\to\textcircled{3}$一样，将最后一项利用期望的运算性质进行展开。

$\textcircled{6}\to\textcircled{7}$：因为$\bar{f}(\boldsymbol{x})$和$y$均为常量，根据期望的运算性质，$\textcircled{6}$中的第2项可化为

$$\mathbb{E}_{D}\left[\left(\bar{f}(\boldsymbol{x})-y\right)^{2}\right]=\left(\bar{f}(\boldsymbol{x})-y\right)^{2}$$

同理，$\textcircled{6}$中的最后一项可化为

$$2\mathbb{E}_{D}\left[\left(\bar{f}(\boldsymbol{x})-y\right)\left(y-y_{D}\right)\right]=2\left(\bar{f}(\boldsymbol{x})-y\right)\mathbb{E}_{D}\left[\left(y-y_{D}\right)\right]$$

由于此时假定噪声的期望为0，即$\mathbb{E}_{D}\left[\left(y-y_{D}\right)\right]=0$，所以

$$2\mathbb{E}_{D}\left[\left(\bar{f}(\boldsymbol{x})-y\right)\left(y-y_{D}\right)\right]=2\left(\bar{f}(\boldsymbol{x})-y\right)\cdot 0=0$$

## 参考文献
[1] 陈希孺. 概率论与数理统计. 中国科学技术大学出版社, 2009.
