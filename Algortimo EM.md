# Algortimo EM
$Def:$ La verosimilitud completa es $L(\theta|\underline{x},\underline{z})$ y se construye con datos observados $\underline{x}$ y faltantes $\underline{z}$

$$
L(\theta|\underline{x},\underline{z})=f(\underline{x},\underline{z}|\theta)
$$

Nota: La verosimilitud marginal

$$
L(\theta|\underline{x})=f(\underline{x}|\theta)=\int f(\underline{x},\underline{z}|\theta)dz=\int L(\theta|\underline{x},\underline{z})dz
$$

La logverosimilitud tiene la siguiente cota

$$
\ell(\theta|\underline{x})=log(L(\theta|\underline{x}))=log(\int f(\underline{x},\underline{z}|\theta)dz)
$$

$$
=log\int \frac{f(\underline{x},\underline{z}|\theta)}{q(\underline{z}|\underline{x},\theta_0)}q(\underline{z}|\underline{x},\theta_0)dz=log\mathbb{E}_{\underline{z}\sim q}[\frac{f(\underline{x},\underline{Z}|\theta)}{q(\underline{Z}|\underline{x},\theta_0)}]
$$

$$
\geq\mathbb{E}_{\underline{z}}log(\frac{f(\underline{x},\underline{z}|\theta)}{q(\underline{Z}|\underline{x},\theta_0)})=\mathbb{E}_{\underline{z}}log(q(\underline{Z}|\underline{x},\theta_0))
$$

Con $q$ arcbitraria.

Con esto en mente tomemos a $q(\underline{z}|\underline{x},\theta_0)=f(\underline{z}|\underline{x},\theta_0)$

$$
\ell(\theta|\underline{x})\geq \mathbb{E}_{\underline{z}}[\ell(\theta|\underline{x},\underline{z})]-\mathbb{E}_{\underline{z}}[f(\underline{z}|\underline{x},\theta_0)]
$$

Donde la esperanza es sobre la densidad $f(\underline{z}|\underline{x},\theta_0)$

$$
\mathbb{E}_{\underline{z}}[\ell(\theta_1|\underline{x},\underline{z})]-\mathbb{E}_{\underline{z}}[\ell(\theta_0|\underline{x},\underline{z})]=Q(\theta_1|\theta_0)-Q(\theta_0|\theta_0)\ge0
$$

Es decir, $\theta_1$ maximiza $Q(\theta|\theta_0)\implies\ell(\theta_1|\underline{x})\geq\ell(\theta_0|\underline{x})$

con $\theta^{(t+1)}=argmaxQ(\theta|\theta^{(t)})\implies \ell(\theta^{(t+1)}|\underline{x})\ge \ell(\theta^{(t)}|\underline{x})$

Algoritmo EM

Inicialice $\theta^{(0)}$ y $t=0$

Repita hasta converger:

(E) Obtenga $Q(\theta|\theta^{(t)})=\mathbb{E}_{\underline{z}}[\ell(\theta|\underline{x},\underline{Z})]$ donde $\underline{Z}\sim f(z|x,\theta^{(t)})$

(M) Obtenga $\theta^{(t+1)}=argmaxQ(\theta|\theta^{(t)})$

Ventajas

Suponiendo que calcular $Q(\theta|\theta^{(t)})$ es sencillo, no se requiere mayor cálculo.

Cada iteracion garantizamos que no empeora la estimación

Muy útil cuando trabajamos con la familia exponencial

No requerimos derivadas para maximizar

Desventajas

Si no podemos calcular $Q(\theta|\theta^{(t)})$ entonces valio pa pura berga

Pero si le sabes a Monte Carlo, tonces se juega

No hay garantía de llegar al máximo global, todo depende del valor inicial $\theta^{(0)}$

En dimensiones altas de $\theta$, la convergencia puede ser lenta así como el caso cuando se tienen muchas variables latentes $\underline{Z}$

Ej. (Datos censurados)

n observaciones $x_1,…,x_n$ con datos censurados $z_1,…,z_m$ de los cuales sabemos que

$$
z_j\ge a\quad \forall j
$$

Los datos provienen de una distribución $N(\theta,1)$

La verosimilitud completa

$$
L(\theta|\underline x, \underline z)=\Pi_{i=1}^{n}f(x_i|\theta)\cdot\Pi_{i=1}^{m}f(z_j|\theta)\mathbb{I}_{z_j\ge a}
$$

además,

$$
L(\theta|\underline x)=\int L(\theta|\underline x, \underline z) dz=\Pi f(x_i|\theta)\cdot \Pi_{i=1}^{m}\int_{a}^{\infty}f(z_j|\theta)dz_j
$$

$$
=\Pi f(x_i|\theta)(\mathbb{P}_\theta[Z-\theta>a-\theta])^m=\Pi f(x_i)(1-\Phi(a-\theta))^m
$$

aparte, sabemos que,

$$
\ell(\theta|\underline x, \underline z)=\sum log((2\pi)^{-\frac{1}{2}}e^{-\frac{1}{2}(x_i-\theta)^2}+\sum_{j=1}^{m}log((2\pi)^{-\frac{1}{2}}e^{-\frac{1}{2}(z_j-\theta)^2}+\sum_{j=1}^{m}log(\mathbb{I}_{z_j\ge a})
$$

también

$$
f(\underline z|\underline x, \theta)=\frac{f(\underline x, \underline z|\theta)}{f(\underline x| \theta)}=\frac{\Pi_{j=1}^{m}f(z_j|\theta)\mathbb{I}_{z_j\ge a}}{(1-\Phi(a-\theta))^m}
$$

$$
\mathbb{E}[\ell(\theta|\underline x, \underline Z)]=\sum [-\frac{1}{2}log(2\pi){-\frac{1}{2}(x_i-\theta)^2}]+\sum [-\frac{1}{2}log(2\pi){-\frac{1}{2}\mathbb{E[}(z_i-\theta)^2}]]+\sum_{j=1}^{m}\mathbb{E}[log(\mathbb{I}_{z_j\ge a})]
$$

veamos que,

$$
Z_j \sim \frac{f(z|\theta)\mathbb{I}_{z\ge a}}{1-\Phi (a-\theta)}
$$

Maximizando,

$$
\frac{\partial}{\partial\theta}Q(\theta|\theta_{0})=\sum_{i=1}^{n}\frac{2}{2}(x_i-\theta)+\sum_{j=1}^{m}\frac{2}{2}\int_{a}^{\infty}(z_j-\theta)\frac{e^{-(z_j-\theta_0)^2}}{\sqrt{2\pi}(1-\Phi(a-\theta_0)}dz=\theta
$$

…

Despejando $\theta$,

$$
\theta_1=argmaxQ(\theta|\theta_0)=\frac{m}{n+m}\frac{f(a|\theta_0)}{1-\Phi(\theta_0-a)}+\frac{n}{n+m}\overline x+\frac{m}{n+m}\theta_0
$$

$$
\theta^{t}=\frac{m}{n+m}\frac{f(a|\theta^{(t-1)})}{1-\Phi(\theta^{(t-1)}-a)}+\frac{n}{n+m}\overline x+\frac{m}{n+m}\theta^{(t-1)}
$$

$$
\theta^{(\infty)}=(...)=\frac{m}{n}\frac{f(a|\theta^{(\infty)})}{1-\Phi(\theta^{(\infty)}-a)}+\overline x
$$

ola
