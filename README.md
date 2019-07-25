# 南瓜书PumpkinBook

周志华老师的《机器学习》（西瓜书）是机器学习领域的经典入门教材之一，周老师为了使尽可能多的读者通过西瓜书对机器学习有所了解, 所以在书中对部分公式的推导细节没有详述，但是这对那些想深究公式推导细节的读者来说可能“不太友好”，本书旨在对西瓜书里比较难理解的公式加以解析，以及对部分公式补充具体的推导细节，诚挚欢迎每一位西瓜书读者前来参与完善本书：一个人可以走的很快，但是一群人却可以走的更远。

## 使用说明

南瓜书仅仅是西瓜书的一些细微补充而已，里面的内容都是以西瓜书的内容为前置知识进行表述的，所以南瓜书的最佳使用方法是以西瓜书为主线，遇到自己推导不出来或者看不懂的公式时再来查阅南瓜书。若南瓜书里没有你想要查阅的公式，可以[点击这里](https://github.com/datawhalechina/pumpkin-book/issues)提交你希望补充推导或者解析的公式编号，我们看到后会尽快进行补充。

### 在线阅读地址

在线阅读地址：https://datawhalechina.github.io/pumpkin-book

## 目录

- 第1章 [绪论](https://datawhalechina.github.io/pumpkin-book/#/chapter1/chapter1)
- 第2章 [模型评估与选择](https://datawhalechina.github.io/pumpkin-book/#/chapter2/chapter2)
- 第3章 [线性模型](https://datawhalechina.github.io/pumpkin-book/#/chapter3/chapter3)
- 第4章 [决策树](https://datawhalechina.github.io/pumpkin-book/#/chapter4/chapter4)
- 第5章 [神经网络](https://datawhalechina.github.io/pumpkin-book/#/chapter5/chapter5)
- 第6章 [支持向量机](https://datawhalechina.github.io/pumpkin-book/#/chapter6/chapter6)
- 第7章 [贝叶斯分类器](https://datawhalechina.github.io/pumpkin-book/#/chapter7/chapter7)
- 第8章 [集成学习](https://datawhalechina.github.io/pumpkin-book/#/chapter8/chapter8)
- 第9章 [聚类](https://datawhalechina.github.io/pumpkin-book/#/chapter9/chapter9)
- 第10章 [降维与度量学习](https://datawhalechina.github.io/pumpkin-book/#/chapter10/chapter10)
- 第11章 [特征选择与稀疏学习](https://datawhalechina.github.io/pumpkin-book/#/chapter11/chapter11)
- 第12章 [计算学习理论](https://datawhalechina.github.io/pumpkin-book/#/chapter12/chapter12)
- 第13章 [半监督学习](https://datawhalechina.github.io/pumpkin-book/#/chapter13/chapter13)
- 第14章 [概率图模型](https://datawhalechina.github.io/pumpkin-book/#/chapter14/chapter14)
- 第15章 规则学习（正在完成中）
- 第16章 [强化学习](https://datawhalechina.github.io/pumpkin-book/#/chapter16/chapter16)

## 选用的西瓜书版本

<img src="https://raw.githubusercontent.com/datawhalechina/pumpkin-book/master/res/xigua.jpg" width="300" height= "350">

> 书名：机器学习<br>
> 作者：周志华<br>
> 出版社：清华大学出版社<br>
> 版次：2016年1月第1版<br>
> 勘误表：http://cs.nju.edu.cn/zhouzh/zhouzh.files/publication/MLbook2016.htm<br>

## 协作规范

### 文档书写规范：

文档采用```Markdown```语法编写，数学公式采用```LaTeX```语法编写，数学符号规范参见西瓜书目录前一页《主要符号表》。<br>
编辑器推荐：[马克飞象](https://maxiang.io)

### 目录结构规范：

```markdown
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

```markdown
## 公式编号

$$（公式的LaTeX表达式）$$

[推导]：（公式推导步骤） or [解析]：（公式解析说明）

## 附录（可选）

（附录内容）
```

例如：
<img src="https://raw.githubusercontent.com/datawhalechina/pumpkin-book/master/res/example.png">

### 主要贡献者（按首字母排名）

[@awyd234](https://github.com/awyd234)
[@feijuan](https://github.com/feijuan)
[@Ggmatch](https://github.com/Ggmatch)
[@Heitao5200](https://github.com/Heitao5200)
[@huaqing89](https://github.com/huaqing89)
[@juxiao](https://github.com/juxiao)
[@jbb0523](https://blog.csdn.net/jbb0523)
[@LongJH](https://github.com/LongJH)
[@LilRachel](https://github.com/LilRachel)
[@LeoLRH](https://github.com/LeoLRH)
[@Majingmin](https://github.com/Majingmin)
[@MrBigFan](https://github.com/MrBigFan)
[@Nono17](https://github.com/Nono17)
[@spareribs](https://github.com/spareribs)
[@sunchaothu](https://github.com/sunchaothu)
[@StevenLzq](https://github.com/StevenLzq)
[@Sm1les](https://github.com/Sm1les)
[@shanry](https://github.com/shanry)
[@Ye980226](https://github.com/Ye980226)


## 关注我们

<div align=center><img src="https://raw.githubusercontent.com/datawhalechina/pumpkin-book/master/res/qrcode.jpeg" width = "250" height = "270" alt="Datawhale，一个专注于AI领域的学习圈子。初衷是for the learner，和学习者一起成长。目前加入学习社群的人数已经数千人，组织了机器学习，深度学习，数据分析，数据挖掘，爬虫，编程，统计学，Mysql，数据竞赛等多个领域的内容学习，微信搜索公众号Datawhale可以加入我们。"></div>

## LICENSE

[GNU General Public License v3.0](https://github.com/datawhalechina/pumpkin-book/blob/master/LICENSE)