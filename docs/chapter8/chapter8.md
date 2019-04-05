## 8.3
$$
\begin{aligned} P(H(\boldsymbol{x}) \neq f(\boldsymbol{x})) &=\sum_{k=0}^{\lfloor T / 2\rfloor} \left( \begin{array}{c}{T} \\ {k}\end{array}\right)(1-\epsilon)^{k} \epsilon^{T-k} \\ & \leqslant \exp \left(-\frac{1}{2} T(1-2 \epsilon)^{2}\right) \end{aligned}
$$

$$
\begin{aligned} P(H(x) \neq f(x))=& P(x \leq\lfloor T / 2\rfloor) \\ & \leqslant P(x \leq T / 2) \end{aligned}
$$

Hoeffding's inequality
$$
\mathrm{P}(H(n) \leq(p-\varepsilon) n) \leq \exp \left(-2 \varepsilon^{2} n\right)
$$
$\newline$

$$
\begin{aligned} & P\left(x \leq \frac{T}{2}\right) \\=& P\left[x-(1-\varepsilon) T \leqslant \frac{T}{2}-(1-\varepsilon) T\right] 
\\=&
P\left[x-
(1-\varepsilon) T \leqslant -\frac{1}{2}T\left(1-2\varepsilon\right)]\right.
\end{aligned}
$$
根据Hoeffding's inequality
$$
\begin{aligned} P(H(\boldsymbol{x}) \neq f(\boldsymbol{x})) &=\sum_{k=0}^{\lfloor T / 2\rfloor} \left( \begin{array}{c}{T} \\ {k}\end{array}\right)(1-\epsilon)^{k} \epsilon^{T-k} \\ & \leqslant \exp \left(-\frac{1}{2} T(1-2 \epsilon)^{2}\right) \end{aligned}
$$
