## 7.5
$$R(c|\boldsymbol x)=1−P(c|\boldsymbol x)$$
[推导]：由式7.1和式7.4可得：
$$R(c_i|\boldsymbol x)=1*P(c_1|\boldsymbol x)+1*P(c_2|\boldsymbol x)+...+0*P(c_i|\boldsymbol x)+...+1*P(c_N|\boldsymbol x)$$
又$\sum_{j=1}^{N}P(c_j|\boldsymbol x)=1$，则：
$$R(c_i|\boldsymbol x)=1-P(c_i|\boldsymbol x)$$
此即为式7.5

## 7.17-7.18
$$P_{(\boldsymbol x_{i}|c)}\in[0,1]$$
$$p_{(\boldsymbol x_{i}| c)}$$
[解析]：式(7.17)所得$P_{(\boldsymbol x_{i}|c)}\in[0,1]$为条件概率，但式(7.18)所得$p_{(\boldsymbol x_{i}| c)}$为条件概率密度而非概率，其值并不在局限于区间[0,1]之内。

## 7.23
$$P(c|\boldsymbol x)\propto{\sum_{i=1 \atop |D_{x_{i}}|\geq m'}^{d}}P(c,x_{i})\prod_{j=1}^{d}P(x_j|c,x_i)$$
[推导]：
$$\begin{aligned}
P(c|\boldsymbol x)&=\cfrac{P(\boldsymbol x,c)}{P(\boldsymbol x)}\\
&=\cfrac{P\left(x_{1}, x_{2}, \ldots, x_{d}, c\right)}{P(\boldsymbol x)}\\
&=\cfrac{P\left(x_{1}, x_{2}, \ldots, x_{d} | c\right) P(c)}{P(\boldsymbol x)} \\
&=\cfrac{P\left(x_{1}, \ldots, x_{i-1}, x_{i+1}, \ldots, x_{d} | c, x_{i}\right) P\left(c, x_{i}\right)}{P(\boldsymbol x)} \\
\end{aligned}$$
$$\begin{aligned}
P(c|\boldsymbol x)&\propto P(c,x_{i})P(x_{1},…,x_{i-1},x_{i+1},…,x_{d}|c,x_{i}) \\
&=P(c,x_{i})\prod _{j=1}^{d}P(x_j|c,x_i)
\end{aligned}$$
$$P(c|\boldsymbol x)\propto{\sum_{i=1 \atop |D_{x_{i}}|\geq m'}^{d}}P(c,x_{i})\prod_{j=1}^{d}P(x_j|c,x_i)$$
此即为式7.23，由于式(7.24)和式(7.25)的使用到了$|D_{c,x_{i}}|$与$|D_{c,x_{i},x_{j}}|$，若$|D_{x_{i}}|$集合中样本数量过少，则$|D_{c,x_{i}}|$与$|D_{c,x_{i},x_{j}}|$将会更小，因此在式(7.23)中要求$|D_{x_{i}}|$集合中样本数量不少于$m'$。




## 附录
### 勘误
7.3节例子计算有笔误，第152页的第9个等式应为$P_{(凹陷|\boldsymbol 是)}=\cfrac{5}{8}$。截止到目前（第28次印刷），西瓜书官方勘误修订仅在第8次印刷时修正了第3个等式$P_{(蜷缩|\boldsymbol 是)}$，但第9个等式仍未修正（若修正第84页的西瓜数据集3.0也可，但亦未修正）。