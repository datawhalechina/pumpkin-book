## 2.20

$$ AUC=\cfrac{1}{2}\sum_{i=1}^{m-1}(x_{i+1} - x_i)\cdot(y_i + y_{i+1}) $$

[解析]：由于图2.4(b)中给出的ROC曲线为横平竖直的标准折线，所以乍一看这个式子的时候很不理解其中的$ \cfrac{1}{2} $和$ (y_i + y_{i+1}) $代表着什么，因为对于横平竖直的标准折线用$ AUC=\sum_{i=1}^{m-1}(x_{i+1} - x_i) \cdot y_i $就可以求出AUC了，但是图2.4(b)中的ROC曲线只是个特例罢了，因为此图是所有样例的预测值均不相同时的情形，也就是说每次分类阈值变化的时候只会划分新增**1个**样例为正例，所以下一个点的坐标为$ (x+\cfrac{1}{m^-},y) $或$ (x,y+\cfrac{1}{m^+}) $，然而当模型对某个正样例和某个反样例给出的预测值相同时，便会划分新增**两个**样例为正例，于是其中一个分类正确一个分类错误，那么下一个点的坐标为$ (x+\cfrac{1}{m^-},y+\cfrac{1}{m^+}) $（当没有预测值相同的样例时，若采取按固定梯度改变分类阈值，也会出现一下划分新增两个甚至多个正例的情形，但是此种阈值选取方案画出的ROC曲线AUC值更小，不建议使用），此时ROC曲线中便会出现斜线，而不再是只有横平竖直的折线，所以用**梯形面积公式**就能完美兼容这两种分类阈值选取方案，也即 **(上底+下底)\*高\*$ \cfrac{1}{2} $**

## 2.21

$$ l_{rank}=\cfrac{1}{m^+m^-}\sum_{x^+ \in D^+}\sum_{x^- \in D^-}(\mathbb{I}(f(x^+)<f(x^-))+\cfrac{1}{2}\mathbb{I}(f(x^+)=f(x^-))) $$

[解析]：

此公式正如书上所说，$ l_{rank} $为ROC曲线**之上**的面积，假设某ROC曲线如下图所示：

![avatar ROC曲线](resources/images/lrank.png? "ROC曲线")

观察ROC曲线易知：

- 每增加一条绿色线段对应着有**1个**正样例（$ x^+_i $）被模型正确判别为正例，且该线段在Y轴的投影长度恒为$ \cfrac{1}{m^+} $；
- 每增加一条红色线段对应着有**1个**反样例（$ x^-_i $）被模型错误判别为正例，且该线段在X轴的投影长度恒为$ \cfrac{1}{m^-} $；
- 每增加一条蓝色线段对应着有a个正样例和b个反样例**同时**被判别为正例，且该线段在X轴上的投影长度=$ b * \cfrac{1}{m^-} $，在Y轴上的投影长度=$ a * \cfrac{1}{m^+} $；
- 任何一条线段所对应的样例的预测值一定**小于**其左边和下边的线段所对应的样例的预测值，其中蓝色线段所对应的a+b个样例的预测值相等。

公式里的$ \sum_{x^+ \in D^+} $可以看成一个遍历$ x^+_i $的循环：

for $ x^+_i $ in $ D^+ $:

　　　　$ \cfrac{1}{m^+}\cdot\cfrac{1}{m^-}\cdot\sum_{x^- \in D^-}(\mathbb{I}(f(x^+_i)<f(x^-))+\cfrac{1}{2}\mathbb{I}(f(x^+_i)=f(x^-))) $ #记为式S

由于每个$ x^+_i $都对应着一条绿色或蓝色线段，所以遍历$ x^+_i $可以看成是在遍历每条绿色和蓝色线段，并用式S来求出每条绿色线段与Y轴构成的面积（例如上图中的m1)或者蓝色线段与Y轴构成的面积（例如上图中的m2+m3）。

**对于每条绿色线段：** 将其式S展开可得：
$$ \cfrac{1}{m^+}\cdot\cfrac{1}{m^-}\cdot\sum_{x^- \in D^-}\mathbb{I}(f(x^+_i)<f(x^-))+\cfrac{1}{m^+}\cdot\cfrac{1}{m^-}\cdot\sum_{x^- \in D^-}\cfrac{1}{2}\mathbb{I}(f(x^+_i)=f(x^-)) $$其中$ x^+_i $此时恒为该线段所对应的正样例，是一个定值。$ \sum_{x^- \in D^-}\cfrac{1}{2}\mathbb{I}(f(x^+_i)=f(x^-) $是在通过遍历所有反样例来统计和$ x^+_i $的预测值相等的反样例个数，由于没有反样例的预测值和$ x^+_i $的预测值相等，所以$ \sum_{x^- \in D^-}\cfrac{1}{2}\mathbb{I}(f(x^+_i)=f(x^-)) $此时恒为0，于是其式S可以化简为：$$ \cfrac{1}{m^+}\cdot\cfrac{1}{m^-}\cdot\sum_{x^- \in D^-}\mathbb{I}(f(x^+_i)<f(x^-)) $$其中$ \cfrac{1}{m^+} $为该线段在Y轴上的投影长度，$ \sum_{x^- \in D^-}\mathbb{I}(f(x^+_i)<f(x^-)) $同理是在通过遍历所有反样例来统计预测值大于$ x^+_i $的预测值的反样例个数，也即该线段左边和下边的红色线段个数+蓝色线段对应的反样例个数，所以$ \cfrac{1}{m^-}\cdot\sum_{x^- \in D^-}(\mathbb{I}(f(x^+)<f(x^-))) $便是该线段左边和下边的红色线段在X轴的投影长度+蓝色线段在X轴的投影长度，也就是该绿色线段在X轴的投影长度，观察ROC图像易知绿色线段与Y轴围成的面积=该线段在Y轴的投影长度 * 该线段在X轴的投影长度。

**对于每条蓝色线段：** 将其式S展开可得：
$$ \cfrac{1}{m^+}\cdot\cfrac{1}{m^-}\cdot\sum_{x^- \in D^-}\mathbb{I}(f(x^+_i)<f(x^-))+\cfrac{1}{m^+}\cdot\cfrac{1}{m^-}\cdot\sum_{x^- \in D^-}\cfrac{1}{2}\mathbb{I}(f(x^+_i)=f(x^-)) $$
其中前半部分表示的是蓝色线段和Y轴围成的图形里面矩形部分的面积，后半部分表示的便是剩下的三角形的面积，矩形部分的面积公式同绿色线段的面积公式一样很好理解，而三角形部分的面积公式里面的$ \cfrac{1}{m^+} $为底边长，$ \cfrac{1}{m^-}\cdot\sum_{x^- \in D^-}\mathbb{I}(f(x^+_i)=f(x^-)) $为高。

综上分析可知，式S既可以用来求绿色线段与Y轴构成的面积也能求蓝色线段与Y轴构成的面积，所以遍历完所有绿色和蓝色线段并将其与Y轴构成的面积累加起来即得$ l_{rank} $。

## 2.27

$$\overline{\epsilon}=\max \epsilon\quad \text { s.t. } \sum_{i= \epsilon_{0} \times m+1}^{m}\left(\begin{array}{c}{m} \\ {i}\end{array}\right) \epsilon^{i}(1-\epsilon)^{m-i}<\alpha$$

[推导]：截至2018年12月，第一版第30次印刷，公式（2.27）应当勘误为
$$\overline{\epsilon}=\min \epsilon\quad\text { s.t. } \sum_{i=\epsilon\times m+1}^{m}\left(\begin{array}{c}{m} \\ {i}\end{array}\right) \epsilon_0^{i}(1-\epsilon_0)^{m-i}<\alpha$$
具体推导过程如下：由西瓜书中的上下文可知，对$\epsilon\leq\epsilon_0$进行假设检验，等价于附录<a href="#add1">①</a>中所述的对$p\leq p_0$进行假设检验，所以在西瓜书中求解最大错误率$\overline{\epsilon}$等价于在附录<a href="#add1">①</a>中求解事件最大发生频率$\frac{\overline{C}}{m}$。由附录<a href="#add1">①</a>可知
$$\overline{C}=\min C\quad\text { s.t. } \sum_{i=C+1}^{m}\left(\begin{array}{c}{m} \\ {i}\end{array}\right) p_0^{i}(1-p_0)^{m-i}<\alpha$$
所以
$$\frac{\overline{C}}{m}=\min \frac{C}{m}\quad\text { s.t. } \sum_{i=C+1}^{m}\left(\begin{array}{c}{m} \\ {i}\end{array}\right) p_0^{i}(1-p_0)^{m-i}<\alpha$$
将上式中的$\frac{\overline{C}}{m},\frac{C}{m},p_0$等价替换为$\overline{\epsilon},\epsilon,\epsilon_0$可得
$$\overline{\epsilon}=\min \epsilon\quad\text { s.t. } \sum_{i=\epsilon\times m+1}^{m}\left(\begin{array}{c}{m} \\ {i}\end{array}\right) \epsilon_0^{i}(1-\epsilon_0)^{m-i}<\alpha$$

## 附录
<h3 id="add1">①二项分布参数$p$的检验<sup><a href="#ref1">[1]</a></sup></h3>
设某事件发生的概率为$p$，$p$未知，作$m$次独立试验，每次观察该事件是否发生，以$X$记该事件发生的次数，则$X$服从二项分布$B(m,p)$，现根据$X$检验如下假设：
$$H_0:p\leq p_0 \\ H_1:p > p_0$$
由二项分布本身的特性可知：$p$越小，$X$取到较小值的概率越大。因此，对于上述假设，一个直观上合理的检验为
$$\varphi:当X\leq C时接受H_0,否则就拒绝H_0$$
其中，$C\in N$表示事件最大发生次数。此检验对应的功效函数为
$$\begin{aligned}
\beta_{\varphi}(p)&=P(X>C)\\
&=1-P(X\leq C) \\
&=1-\sum_{i=0}^{C}\left(\begin{array}{c}{m} \\ {i}\end{array}\right) p^{i} (1-p)^{m-i} \\
&=\sum_{i=C+1}^{m}\left(\begin{array}{c}{m} \\ {i}\end{array}\right) p^{i} (1-p)^{m-i} \\
\end{aligned}$$
由于“$p$越小，$X$取到较小值的概率越大”可以等价表示为：$P(X\leq C)$是关于$p$的减函数（更为严格的数学证明参见<a href="#ref1">[1]</a>中第二章习题7），所以$\beta_{\varphi}(p)=P(X>C)=1-P(X\leq C)$是关于$p$的增函数，那么当$p\leq p_0$时，$\beta_{\varphi}(p_0)$即为$\beta_{\varphi}(p)$的上确界。又因为，根据<a href="#ref1">[1]</a>中5.1.3的定义1.2可知，检验水平$\alpha$默认取最小可能的水平，所以在给定检验水平$\alpha$时，可以通过如下方程解得满足检验水平$\alpha$的整数$C$：
$$\alpha =\sup \left\{\beta_{\varphi}(p)\right\}$$
显然，当$p\leq p_0$时：
$$\begin{aligned}
\alpha &=\sup \left\{\beta_{\varphi}(p)\right\} \\
&=\beta_{\varphi}(p_0) \\
&=\sum_{i=C+1}^{m}\left(\begin{array}{c}{m} \\ {i}\end{array}\right) p_0^{i} (1-p_0)^{m-i}
\end{aligned}$$
对于此方程，通常不一定正好解得一个整数$C$使得方程成立，较常见的情况是存在这样一个$\overline{C}$使得
$$\sum_{i=\overline{C}+1}^{m}\left(\begin{array}{c}{m} \\ {i}\end{array}\right) p_0^{i} (1-p_0)^{m-i}<\alpha \\
\sum_{i=\overline{C}}^{m}\left(\begin{array}{c}{m} \\ {i}\end{array}\right) p_0^{i} (1-p_0)^{m-i}>\alpha$$
此时，$C$只能取$\overline{C}$或者$\overline{C}+1$，若$C$取$\overline{C}$，则相当于升高了检验水平$\alpha$，若$C$取$\overline{C}+1$则相当于降低了检验水平$\alpha$，具体如何取舍需要结合实际情况，但是通常为了减小犯第一类错误的概率，会倾向于令$C$取$\overline{C}+1$。下面考虑如何求解$\overline{C}$：易证$\beta_{\varphi}(p_0)$是关于$C$的减函数，所以再结合上述关于$\overline{C}$的两个不等式易推得
$$\overline{C}=\min C\quad\text { s.t. } \sum_{i=C+1}^{m}\left(\begin{array}{c}{m} \\ {i}\end{array}\right) p_0^{i}(1-p_0)^{m-i}<\alpha$$

## 参考文献
<span id="ref1">[1]陈希孺编著.概率论与数理统计[M].中国科学技术大学出版社,2009.</span>