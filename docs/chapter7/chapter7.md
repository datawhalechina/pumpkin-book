## 7.5
$$R(c|\boldsymbol x)=1−P(c|\boldsymbol x)$$
[推导]：由式7.1和式7.4可得：
$$R(c_i|\boldsymbol x)=1*P(c_1|\boldsymbol x)+1*P(c_2|\boldsymbol x)+...+0*P(c_i|\boldsymbol x)+...+1*P(c_N|\boldsymbol x)$$
又$\sum_{j=1}^{N}P(c_j|\boldsymbol x)=1$，则：
$$R(c_i|\boldsymbol x)=1-P(c_i|\boldsymbol x)$$
此即为式7.5
## 7.8
$$P(c|\boldsymbol x)=\cfrac{P(c)P(\boldsymbol x|c)}{P(\boldsymbol x)}$$
[解析]：最小化误差，也就是最大化P(c|x)，但由于P(c|x)属于后验概率无法直接计算，由贝叶斯公式可计算出:
$$P(c|\boldsymbol x)=\cfrac{P(c)P(\boldsymbol x|c)}{P(\boldsymbol x)}$$
$P(\boldsymbol x)$可以省略，因为我们比较的时候$P(\boldsymbol x)$一定是相同的，所以我们就是用历史数据计算出$P(c)$和$P(\boldsymbol x|c)$。
1. $P(c)$根据大数定律，当样本量到了一定程度且服从独立同分布，c的出现的频率就是c的概率。
2. $P(\boldsymbol x|c)$，因为$\boldsymbol x$在这里不对单一元素是个矩阵，涉及n个元素，不太好直接统计分类为c时，$\boldsymbol x$的概率，所以我们根据假设独立同分布，对每个$\boldsymbol x$的每个特征分别求概率
$$P(\boldsymbol x|c)=P(x_1|c)*P(x_2|c)*P(x_3|c)...*P(x_n|c)$$
这个式子就可以很方便的通过历史数据去统计了,比如特征n，就是在分类为c时特征n出现的概率，在数据集中应该是用1显示。
但是当某一概率为0时会导致整个式子概率为0，所以采用拉普拉斯修正

当样本属性独依赖时，也就是除了c多加一个依赖条件，式子变成了
$$∏_{i=1}^n P(x_i|c,p_i)$$
$p_i$是$x_i$所依赖的属性

当样本属性相关性未知时,我们采用贝叶斯网的算法，对相关性进行评估，以找出一个最佳的分类模型。

当遇到不完整的训练样本时，可通过使用EM算法对模型参数进行评估来解决。

## 附录
#### sklearn调包

```python
 import numpy as np
 X = np.array([[-1, -1], [-2, -1], [-3, -2], [1, 1], [2, 1], [3, 2]])
 Y = np.array([1, 1, 1, 2, 2, 2])
from sklearn.naive_bayes import GaussianNB
 clf = GaussianNB()
clf.fit(X, Y)
GaussianNB(priors=None, var_smoothing=1e-09)
print(clf.predict([[-0.8, -1]]))
```
#### 参数:	
priors : array-like, shape (n_classes,)
Prior probabilities of the classes. If specified the priors are not adjusted according to the data.

var_smoothing : float, optional (default=1e-9)
Portion of the largest variance of all features that is added to variances for calculation stability.
#### 贝叶斯应用

1. 中文分词
分词后，得分的假设是基于两词之间是独立的，后词的出现与前词无关
2. 统计机器翻译
统计机器翻译因为其简单，无需手动添加规则，迅速成为了机器翻译的事实标准。
3. 贝叶斯图像识别
首先是视觉系统提取图形的边角特征，然后使用这些特征自底向上地激活高层的抽象概念，然后使用一个自顶向下的验证来比较到底哪个概念最佳地解释了观察到的图像。