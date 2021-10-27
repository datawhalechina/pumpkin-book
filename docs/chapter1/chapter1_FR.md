## 1.1
$$E_{o t e}\left(\mathfrak{L}_{a} | X, f\right)=\sum_{h} \sum_{\boldsymbol{x} \in \mathcal{X}-X} P(\boldsymbol{x}) \mathbb{I}(h(\boldsymbol{x}) \neq f(\boldsymbol{x})) P\left(h | X, \mathfrak{L}_{a}\right)$$
[Analyse]: voir la formule(1.2)

## 1.2
$$\begin{aligned}
\sum_{f}E_{ote}(\mathfrak{L}_a\vert X,f) &= \sum_f\sum_h\sum_{\boldsymbol{x}\in\mathcal{X}-X}P(\boldsymbol{x})\mathbb{I}(h(\boldsymbol{x})\neq f(\boldsymbol{x}))P(h\vert X,\mathfrak{L}_a) \\
&=\sum_{\boldsymbol{x}\in\mathcal{X}-X}P(\boldsymbol{x}) \sum_hP(h\vert X,\mathfrak{L}_a)\sum_f\mathbb{I}(h(\boldsymbol{x})\neq f(\boldsymbol{x})) \\
&=\sum_{\boldsymbol{x}\in\mathcal{X}-X}P(\boldsymbol{x}) \sum_hP(h\vert X,\mathfrak{L}_a)\cfrac{1}{2}2^{\vert \mathcal{X} \vert} \\
&=\cfrac{1}{2}2^{\vert \mathcal{X} \vert}\sum_{\boldsymbol{x}\in\mathcal{X}-X}P(\boldsymbol{x}) \sum_hP(h\vert X,\mathfrak{L}_a) \\
&=2^{\vert \mathcal{X} \vert-1}\sum_{\boldsymbol{x}\in\mathcal{X}-X}P(\boldsymbol{x}) \cdot 1\\
\end{aligned}$$
[Analyse]：De l'étape 1 à 2：
$$\begin{aligned}
&\sum_f\sum_h\sum_{\boldsymbol{x}\in\mathcal{X}-X}P(\boldsymbol{x})\mathbb{I}(h(\boldsymbol{x})\neq f(\boldsymbol{x}))P(h\vert X,\mathfrak{L}_a) \\
&=\sum_{\boldsymbol{x}\in\mathcal{X}-X}P(\boldsymbol{x})\sum_f\sum_h\mathbb{I}(h(\boldsymbol{x})\neq f(\boldsymbol{x}))P(h\vert X,\mathfrak{L}_a) \\
&=\sum_{\boldsymbol{x}\in\mathcal{X}-X}P(\boldsymbol{x}) \sum_hP(h\vert X,\mathfrak{L}_a)\sum_f\mathbb{I}(h(\boldsymbol{x})\neq f(\boldsymbol{x})) \\
\end{aligned}$$
De l'étape 1 à 2：：Tout d'abord; on fait hypothèse que $f$ est fonction qui peut mapper l'échantillion de {0,1} et obéir à une distribution uniforme. C'est-à-dire qu'il exists plusieur $f$ et la probabilité de chaque $f$ procède la même probabilité，dans le cas où il y a que deux échantillions ：$ \mathcal{X}=\{\boldsymbol{x}_1,\boldsymbol{x}_2\},\vert \mathcal{X} \vert=2$，Toute les fonctions objectif $f$ seront egaux aux：
$$\begin{aligned}
f_1:f_1(\boldsymbol{x}_1)=0,f_1(\boldsymbol{x}_2)=0;\\
f_2:f_2(\boldsymbol{x}_1)=0,f_2(\boldsymbol{x}_2)=1;\\
f_3:f_3(\boldsymbol{x}_1)=1,f_3(\boldsymbol{x}_2)=0;\\
f_4:f_4(\boldsymbol{x}_1)=1,f_4(\boldsymbol{x}_2)=1;
\end{aligned}$$
En total, il y aura $2^{\vert \mathcal{X} \vert}=2^2=4$ fonctions objectifs. Le modèle $h(\boldsymbol{x})$ apris par l'algoritme $\mathfrak{L}_a$ contients des prédictions qui sont égaux aux $f$-la moitié de l'échantillion comprise entre 0 et 1. Par exemple, le modèle actuel$h(\boldsymbol{x})$ La prédiction de $\boldsymbol{x}_1$ dans le modèle actuel est égale à 1，donc $h(\boldsymbol{x}_1)=1$. Il n'y a que $f_3$ et $f_4$ qui sont égaux à la prédiction de $h(\boldsymbol{x})$. Autrementdit，la moitié de $f$ est égale à sa prédiction. Dans ce cas, \sum_f\mathbb{I}(h(\boldsymbol{x})\neq f(\boldsymbol{x})) = \cfrac{1}{2}2^{\vert \mathcal{X} \vert} $；les analyses de l'étape 3 jusqu'aux dernières étapes sont étabile de façon évident.Il est à noter que, notre hypothèses initiale est “La fonction peut mapper à l'échantillion de {0,1} et obéir à une distribution uniforme”，mais en réalité, on constate que la fonction qui peut parfaitement s'adapter aux données d'échantillon existantes est la véritable fonction objectif. Par exemple, l'échantillion existante est $\{(\boldsymbol{x}_1,0),(\boldsymbol{x}_2,1)\}$, et alors, $f_2$ est la véritable fonction objectif. Parce qu'aucun échantillon de ce type $\{(\boldsymbol{x}_1,0),(\boldsymbol{x}_2,0)\},\{(\boldsymbol{x}_1,1),(\boldsymbol{x}_2,0)\},\{(\boldsymbol{x}_1,1),(\boldsymbol{x}_2,1)\}$ n'a été collecté ou n'existait pas du tout. On constate que $f_1,f_3,f_4$ n'est pas la véritable fonction objectif。C'est donc l'expression "Cyclisme" de la formule (1.3) dans notre Pumpkin-book.
