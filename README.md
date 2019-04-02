# 南瓜书PumpkinBook
周志华老师的《机器学习》（西瓜书）是机器学习领域的经典入门教材之一，周老师为了使尽可能多的读者通过西瓜书对机器学习有所了解, 所以在书中对部分公式的推导细节没有详述，但是这对那些想深究公式推导细节的读者来说可能“不太友好”，本书旨在对西瓜书里比较难理解的公式加以解析，以及对部分公式补充具体的推导细节，诚挚欢迎每一位西瓜书读者前来参与完善本书：一个人可以走的很快，但是一群人却可以走的更远。

# 使用说明
南瓜书仅仅是西瓜书的一些细微补充而已，里面的内容都是以西瓜书的内容为前置知识进行表述的，所以南瓜书的最佳使用方法是以西瓜书为主线，遇到自己推导不出来或者看不懂的公式时再来查阅南瓜书。若南瓜书里没有你想要查阅的公式，可以[点击这里](https://github.com/datawhalechina/pumpkin-book/issues/1)提交你希望补充推导或者解析的公式编号，我们看到后会尽快进行补充。

# 在线阅读地址
https://datawhalechina.github.io/pumpkin-book/

# 目录

- 第1章 [绪论](https://datawhalechina.github.io/pumpkin-book/#/chapter1/chapter1)
- 第2章 [模型评估与选择](https://datawhalechina.github.io/pumpkin-book/#/chapter2/chapter2)
- 第3章 [线性模型](https://datawhalechina.github.io/pumpkin-book/#/chapter3/chapter3)
- 第4章 [决策树](https://datawhalechina.github.io/pumpkin-book/#/chapter4/chapter4)
- 第5章 [神经网络](https://datawhalechina.github.io/pumpkin-book/#/chapter5/chapter5)
- 第6章 [支持向量机](https://datawhalechina.github.io/pumpkin-book/#/chapter6/chapter6)
- 第7章 [贝叶斯分类器](https://datawhalechina.github.io/pumpkin-book/#/chapter7/chapter7)
- 第8章 集成学习
- 第9章 聚类
- 第10章 降维与度量学习
- 第11章 特征选择与稀疏学习
- 第12章 计算学习理论
- 第13章 半监督学习
- 第14章 概率图模型
- 第15章 规则学习
- 第16章 强化学习

# 选用的西瓜书版本
<img src="https://raw.githubusercontent.com/datawhalechina/pumpkin-book/master/res/xigua.jpg" width = "476.7" height = "555.3">

> 书名：机器学习<br>
> 作者：周志华<br>
> 出版社：清华大学出版社<br>
> 版次：2016年1月第1版<br>
> 勘误表：http://cs.nju.edu.cn/zhouzh/zhouzh.files/publication/MLbook2016.htm

#  协作规范

### 文档书写规范：
文档采用Markdown语法编写，数学公式采用LaTeX语法编写，数学符号规范参见西瓜书《主要符号表》。

|          | 格式     | 参考资料                                                     |
| :------: | :------- | :----------------------------------------------------------- |
| 文档 | Markdown | 1. CSDN Markdown 使用教程 http://t.cn/E4699cO<br>2. 简书 Markdown 使用教程 https://www.jianshu.com/p/q81RER<br>3. 编辑软件推荐 Typora https://typora.io/ |
| 数学公式 | LaTeX    | 1. CSDN Latex语法编写数学公式 http://t.cn/E469pdI<br>2.Latex 在线编辑工具 http://latex.codecogs.com/eqneditor/editor.php |


### 目录结构规范：

```
pumpkin-book
├─docs
|  ├─chapter1  # 第1章
|  |  ├─resources  # 资源文件夹
|  |  |  └─images  # 图片资源
|  |  └─chapter1.md # 第1章公式全解
|  ├─chapter2
...
```
### 公式全解文档规范：
```
## 公式编号
$$（公式的LaTeX表达式）$$
[推导]：（公式推导步骤） or [解析]：（公式解析说明）
## 附录
（附录内容）
```
样例参见`docs/chapter2/chapter2.md`和`docs/chapter3/chapter3.md`

## 关注我们

<div align=center><img src="https://raw.githubusercontent.com/datawhalechina/pumpkin-book/master/res/qrcode.jpeg" width = "250" height = "270"></div>

# LICENSE
[GNU General Public License v3.0](https://github.com/datawhalechina/pumpkin-book/blob/master/LICENSE)

