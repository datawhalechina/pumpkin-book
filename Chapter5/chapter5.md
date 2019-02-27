### 5.2
$$\Delta w\_i = \eta(y-\hat{y})x\_i$$
[推导]：此处感知机的模型为：
$$y=f(\sum\_{i} w\_i x\_i - \theta)$$
将$\theta$看成哑结点后，模型可化简为：
$$y=f(\sum\_{i} w\_i x\_i)=f(\boldsymbol w^T \boldsymbol x)$$
其中$f$为阶跃函数。<br>根据《统计学习方法》§2可知，假设误分类点集合为$M$，$\boldsymbol x\_i \in M$为误分类点，$\boldsymbol x\_i$的真实标签为$y\_i$,模型的预测值为$\hat{y\_i}$,对于误分类点$\boldsymbol x\_i$来说，此时$\boldsymbol w^T \boldsymbol x\_i \gt 0,\hat{y\_i}=1,y\_i=0$或$\boldsymbol w^T \boldsymbol x\_i \lt 0,\hat{y\_i}=0,y\_i=1$,综合考虑两种情形可得：
$$(\hat{y\_i}-y\_i)\boldsymbol w \boldsymbol x\_i>0$$
所以可以推得损失函数为：
$$L(\boldsymbol w)=\sum\_{\boldsymbol x\_i \in M} (\hat{y\_i}-y\_i)\boldsymbol w \boldsymbol x\_i$$
损失函数的梯度为：
$$\nabla\_w L(\boldsymbol w)=\sum\_{\boldsymbol x\_i \in M} (\hat{y\_i}-y\_i)\boldsymbol x\_i$$
随机选取一个误分类点$(\boldsymbol x\_i,y\_i)$，对$\boldsymbol w$进行更新：
$$\boldsymbol w \leftarrow \boldsymbol w-\eta(\hat{y\_i}-y\_i)\boldsymbol x\_i=\boldsymbol w+\eta(y\_i-\hat{y\_i})\boldsymbol x\_i$$
显然式5.2为$\boldsymbol w$的第$i$个分量$w\_i$的变化情况
### 5.12
$$\Delta \theta\_j = -\eta g\_j$$
[推导]：因为
$$\Delta \theta\_j = -\eta \cfrac{\partial E\_k}{\partial \theta\_j}$$
又
$$
\begin{aligned}	
\cfrac{\partial E\_k}{\partial \theta\_j} &= \cfrac{\partial E\_k}{\partial \hat{y}\_j^k} \cdot\cfrac{\partial \hat{y}\_j^k}{\partial \theta\_j} \\\\
&= (\hat{y}\_j^k-y\_j^k) \cdot f’(\beta\_j-\theta\_j) \cdot (-1) \\\\
&= -(\hat{y}\_j^k-y\_j^k)f’(\beta\_j-\theta\_j) \\\\
&= g\_j
\end{aligned}
$$
所以
$$\Delta \theta\_j = -\eta \cfrac{\partial E\_k}{\partial \theta\_j}=-\eta g\_j$$
### 5.13
$$\Delta v\_{ih} = \eta e\_h x\_i$$
[推导]：因为
$$\Delta v\_{ih} = -\eta \cfrac{\partial E\_k}{\partial v\_{ih}}$$
又
$$
\begin{aligned}	
\cfrac{\partial E\_k}{\partial v\_{ih}} &= \sum\_{j=1}^{l} \cfrac{\partial E\_k}{\partial \hat{y}\_j^k} \cdot \cfrac{\partial \hat{y}\_j^k}{\partial \beta\_j} \cdot \cfrac{\partial \beta\_j}{\partial b\_h} \cdot \cfrac{\partial b\_h}{\partial \alpha\_h} \cdot \cfrac{\partial \alpha\_h}{\partial v\_{ih}} \\\\
&= \sum\_{j=1}^{l} \cfrac{\partial E\_k}{\partial \hat{y}\_j^k} \cdot \cfrac{\partial \hat{y}\_j^k}{\partial \beta\_j} \cdot \cfrac{\partial \beta\_j}{\partial b\_h} \cdot \cfrac{\partial b\_h}{\partial \alpha\_h} \cdot x\_i \\\\ 
&= \sum\_{j=1}^{l} \cfrac{\partial E\_k}{\partial \hat{y}\_j^k} \cdot \cfrac{\partial \hat{y}\_j^k}{\partial \beta\_j} \cdot \cfrac{\partial \beta\_j}{\partial b\_h} \cdot f’(\alpha\_h-\gamma\_h) \cdot x\_i \\\\
&= \sum\_{j=1}^{l} \cfrac{\partial E\_k}{\partial \hat{y}\_j^k} \cdot \cfrac{\partial \hat{y}\_j^k}{\partial \beta\_j} \cdot w\_{hj} \cdot f’(\alpha\_h-\gamma\_h) \cdot x\_i \\\\
&= \sum\_{j=1}^{l} (-g\_j) \cdot w\_{hj} \cdot f’(\alpha\_h-\gamma\_h) \cdot x\_i \\\\
&= -f’(\alpha\_h-\gamma\_h) \cdot \sum\_{j=1}^{l} g\_j \cdot w\_{hj}  \cdot x\_i\\\\
&= -b\_h(1-b\_h) \cdot \sum\_{j=1}^{l} g\_j \cdot w\_{hj}  \cdot x\_i \\\\
&= -e\_h \cdot x\_i
\end{aligned}
$$
所以
$$\Delta v\_{ih} = -\eta \cdot -e\_h \cdot x\_i=\eta e\_h x\_i$$
### 5.14
$$\Delta \gamma\_h= -\eta e\_h$$
[推导]：因为
$$\Delta \gamma\_h = -\eta \cfrac{\partial E\_k}{\partial \gamma\_h}$$
又
$$
\begin{aligned}	
\cfrac{\partial E\_k}{\partial \gamma\_h} &= \sum\_{j=1}^{l} \cfrac{\partial E\_k}{\partial \hat{y}\_j^k} \cdot \cfrac{\partial \hat{y}\_j^k}{\partial \beta\_j} \cdot \cfrac{\partial \beta\_j}{\partial b\_h} \cdot \cfrac{\partial b\_h}{\partial \gamma\_h} \\\\
&= \sum\_{j=1}^{l} \cfrac{\partial E\_k}{\partial \hat{y}\_j^k} \cdot \cfrac{\partial \hat{y}\_j^k}{\partial \beta\_j} \cdot \cfrac{\partial \beta\_j}{\partial b\_h} \cdot f’(\alpha\_h-\gamma\_h) \cdot (-1) \\\\
&= -\sum\_{j=1}^{l} \cfrac{\partial E\_k}{\partial \hat{y}\_j^k} \cdot \cfrac{\partial \hat{y}\_j^k}{\partial \beta\_j} \cdot w\_{hj} \cdot f’(\alpha\_h-\gamma\_h)\\\\
&=e\_h
\end{aligned}
$$
所以
$$\Delta \gamma\_h= -\eta e\_h$$