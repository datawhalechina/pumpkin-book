# 5.1 神经元模型
# 5.2 感知机与多层网络
# 5.3 误差逆传播算法
对于训练例$(x_k,y_k)$, 假定神经网络输出为 $\hat{y}_k = (\hat{y}_1^k , \hat{y}_2^k,...,\hat{y}_l^k)$
$$ \hat{y}_j^k = f(\beta_j - \theta_j) $$ (5.3)
设网络在 $(x_k,y_k)$的均方误差为
 $$ E_k = \frac{1}{2} \sum^l_{j=1}(\hat{y}_j^k -y_j^k)^2$$  (5.4)

> 这里 1/2是为了求导方便消去系数，只是个规定

设 学习率为 $\eta$, 以目标的负梯度方向对参数进行调整的含义是：
目标函数（均方误差 $E_k$)在 $w_{hj}$处的负梯度对参数进行调整：
$$ \Delta w_{hj} = - \eta\frac{\partial E_k}{\partial w_{hj}}$$ (5.6)

根据链式法则：
$$ \frac{\partial E_k}{\partial w_{hj}} = \frac{\partial E_k}{\partial \hat{y}_j^k}\frac{\partial \hat{y}_j^k}{\partial \beta_j}\frac{\partial \beta_j}{\partial w_{hj}}$$ (5.7)

根据 $\beta_j$ 的定义，
$$ \beta_j = \sum^q_{h=1} w_{hj}b_h$$

有
$$\frac{\partial \beta_j}{\partial w_{hj}} = b_h$$ (5.8)
图 5.2 的Sigmoid 函数为
$$ sigmoid(x) = \frac{1}{1+e^{-x}}$$
所以
$$ f' = \frac{e^{-x}}{(1+e^{-x})^2} = f(x)(1-f(x))$$ （5.9）
根据 5.3和 5.4
$$  - \frac{\partial E_k}{\partial \hat{y}_j^k}\frac{\partial \hat{y}_j^k}{\partial \beta_j} = - (\hat{y}_j^k - y_j^k)f'(\beta_j - \theta_j) = - (\hat{y}_j^k - y_j^k)[\hat{y}_j^k(1-\hat{y}_j^k)] = \hat{y}_j^k(1-\hat{y}_j^k)(y_j^k - \hat{y}_j^k) = g_j$$  
（5.10）
>$g_j$ 是设出来的变量

将 (5.10)和（5.8）带入(5.7)再带入5.6，可以得到BP算法中关于$w_{hj}$的更新公式
$$ \Delta w_{hj} = \eta g_j b_h$$ (5.11)

同理有:

$$\Delta\theta_j = -\eta g_j $$ (5.12)

> 注意这里是把 $\theta_j$ 看做为输入为 -1，系数或者说权重为 $\theta$ 的w, 和$\Delta w_{hj}$ 具有等价的地位， 取 $b_h = -1$ 带入(5.11)得到的
$$\Delta v_{ih} = -\eta \frac{\partial E_k}{\partial v_{ih}}  $$
$$\frac{\partial E_k}{\partial v_{ih}} = \frac{E_k}{\partial{b_h}}\frac{\partial b_h}{\partial \alpha_h}\frac{\partial \alpha_h}{\partial x_i} = e_hx_i$$

所以 
$$\Delta v_{ih} = -\eta e_h x_i  $$ (5.13)
这里的
 $$e_h = \frac{E_k}{\partial{b_h}}\frac{\partial b_h}{\partial \alpha_h} = -\sum_{j=1}^l \frac{\partial E_k}{\partial \beta_j}\frac{\partial \beta_j}{\partial b_h}f'(\alpha - \gamma_h) = \sum_{j=1}^lw_{hj}g_jf'(\alpha_h - \gamma_h) = b_h(1-b_h)\sum_{j=1}^lw_{hj}g_j$$ 
同(5.12)理得到
$$\Delta \gamma_h = -\eta e_h$$ (5.14)