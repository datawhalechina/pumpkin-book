# 第15章 规则学习

规则学习是"符号主义学习"的代表性方法，用来从训练数据中学到一组能对未见示例进行判别的规则，形如"如果A或B，并且C的条件下，D满足"这样的形式。因为这种学习方法更加贴合人类从数据中学到经验的描述，具有非常良好的可解释性，是最早开始研究机器学习的技术之一。

## 15.1 剪枝优化

### 15.1.1 式(15.2)和式(15.3)的解释

似然率统计量LRS定义为：

$$\mathrm{LRS}=2 \cdot\left(\hat{m}_{+} \log _{2} \frac{\left(\frac{\hat{m}_{+}}{\hat{m}_{+}+\hat{m}_{-}}\right)}{\left(\frac{m_{+}}{m_{+}+m_{-}}\right)}+\hat{m}_{-} \log _{2} \frac{\left(\frac{\hat{m}_{-}}{\hat{m}_{+}+\hat{m}_{-}}\right)}{\left(\frac{m_{-}}{m_{+}+m_{-}}\right)}\right)$$

同时，根据对数函数的定义，我们可以对式(15.3)进行化简： 

$$\begin{aligned}
\mathrm{F}_{-} \text {Gain }&=\hat{m}_{+} \times\left(\log _{2} \frac{\hat{m}_{+}}{\hat{m}_{+}+\hat{m}_{-}}-\log _{2} \frac{m_{+}}{m_{+}+m_{-}}\right)\\
&=\hat{m}_{+}\left(\log_{2}\frac{\frac{\hat{m}_{+}}{\hat{m}_{+}+\hat{m}_{-}}}{\frac{m_{+}}{m_{+}+m_{-}}}\right)
\end{aligned}$$

可以观察到F_Gain即为式(15.2)中LRS求和项中的第一项。这里"西瓜书"中做了详细的解释，FOIL仅考虑正例的信息量，由于关系数据中正例数旺旺远少于反例数，因此通常对正例应该赋予更多的关注。

## 15.2 归纳逻辑程序设计

### 15.2.1 式(15.6)的解释

定义析合范式的删除操作符为"$-$"，表示在$A$和$B$的析合式中删除成分$B$，得到成分$A$。

### 15.2.2 式(15.7)的推导

$C=A\vee B$，把$A=C_1 - \{L\}$和$L=C_2-\{\neg L\}$带入即得。

### 15.2.3 式(15.9)的推导

根据式(15.7) $C=\left(C_1-\{L\}\right) \vee\left(C_2-\{\neg L\}\right)$
和析合范式的删除操作，等式两边同时删除析合项$C_2-\{\neg L\}$有：

$$C - (C_1 - \{L\}) = C_2-\{\neg L\}$$

再次运用析合范式删除操作符的逆定义，等式两边同时加上析合项$\{\neg L\}$有：

$$C_2=\left(C-\left(C_1-\{L\}\right)\right) \vee\{\neg L\}$$

### 15.2.4 式(15.10)的解释

该式是吸收(absorption)操作的定义。注意作者在文章中所用的符号定义，用
$\frac{X}{Y}$ 表示 $X$ 蕴含
$Y$，${X}$的子句或是${Y}$的归结项，或是$Y$中某个子句的等价项。所谓吸收，是指替换部分逻辑子句（大写字母），生成一个新的逻辑文字（小写字母）用于定义这些被替换的逻辑子句。在式(15.10)中，逻辑子句$A$被逻辑文字$q$替换。

### 15.2.5 式(15.11)的解释

该式是辨识(identification)操作的定义。辨识操作依据已知的逻辑文字，构造新的逻辑子句和文字的关系。在式(15.11)中，已知$p \leftarrow A \wedge B$和$p \leftarrow A \wedge q$，构造的新逻辑文字为$q \leftarrow B$。

### 15.2.6 式(15.12)的解释

该式是内构(intra-construction)操作的定义。内构操作找到关于同一逻辑文字中的共同逻辑子句部分，并且提取其中不同的部分作为新的逻辑文字。在式(15.12)中，逻辑文字$p \leftarrow A \wedge B$和$p \leftarrow A \wedge C$的共同部分为$p \leftarrow A \wedge q$，其中新逻辑文字$q \leftarrow B \quad q \leftarrow C$。

### 15.2.7 式(15.13)的解释

该式是互构(inter-construction)操作的定义。互构操作找到不同逻辑文字中的共同逻辑子句部分，并定义新的逻辑文字已描述这个共同的逻辑子句。在式(15.13)中，逻辑文字$p \leftarrow A \wedge B$
和
$q \leftarrow A \wedge C$的共同逻辑子句$A$提取出来，并用逻辑文字定义为$r \leftarrow A$。逻辑文字$p$和$q$的定义也用$r$做相应的替换得到$p \leftarrow r \wedge B$与$q \leftarrow r \wedge C$。

### 15.2.8 式(15.16)的推导

$\theta_1$为作者笔误，由15.9

$$\begin{aligned}
C_{2}&=\left(C-\left(C_{1}-\{L_1\}\right)\right) \vee\{L_2\}\\
\end{aligned}$$

因为 $L_2=(\neg L_1\theta_1)\theta_2^{-1}$，替换得证。
