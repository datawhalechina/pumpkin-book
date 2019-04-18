## 11.10

$$
\hat{f}(x) \simeq f(x_{k})+\langle \nabla f(x_{k}),x-x_{k} \rangle + \frac{L}{2}\left \| x-x_{k} \right\|^{2}
$$

[推导]：
$$
\begin{aligned}
\hat{f}(x) &\simeq f(x_{k})+\langle \nabla f(x_{k}),x-x_{k} \rangle + \frac{L}{2}\left \| x-x_{k} \right\|^{2} \\
&= f(x_{k})+\langle \nabla f(x_{k}),x-x_{k} \rangle + \langle\frac{L}{2}(x-x_{k}),x-x_{k}\rangle \\
&= f(x_{k})+\langle \nabla f(x_{k})+\frac{L}{2}(x-x_{k}),x-x_{k} \rangle \\
&= f(x_{k})+\frac{L}{2}\langle\frac{2}{L}\nabla f(x_{k})+(x-x_{k}),x-x_{k} \rangle \\
&= f(x_{k})+\frac{L}{2}\langle x-x_{k}+\frac{1}{L}\nabla f(x_{k})+\frac{1}{L}\nabla f(x_{k}),x-x_{k}+\frac{1}{L}\nabla f(x_{k})-\frac{1}{L}\nabla f(x_{k}) \rangle \\
&= f(x_{k})+\frac{L}{2}\left\| x-x_{k}+\frac{1}{L}\nabla f(x_{k}) \right\|_{2}^{2} -\frac{1}{2L}\left\|\nabla f(x_{k})\right\|_{2}^{2} \\
&= \frac{L}{2}\left\| x-(x_{k}-\frac{1}{L}\nabla f(x_{k})) \right\|_{2}^{2} + const \qquad (因为f(x_{k})和\nabla f(x_{k})是常数)
\end{aligned}
$$

