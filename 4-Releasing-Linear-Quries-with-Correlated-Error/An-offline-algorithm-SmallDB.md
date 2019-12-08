# 4.1 SmallDB：离线算法

在本节中，我们给出一种基于使用指数机制对小型数据库进行采样的思想的算法。 我们将表明，对于计数查询，只需考虑较小的数据库即可：它们的大小仅是查询类的函数以及我们所需的近似精度$\alpha$，而私有数据库大小 $\Vert x\Vert _1$ 不是关键。这很重要，因为它将允许我们同时为所有足够大的数据库保证在指数机制范围内至少有一个数据库，这个数据库在查询 $\mathcal{Q}$ 中可以很好地近似于 $x$，并且还能保证在数据库中没有太多数据库消除“好”数据库上的概率质量。

![SmallDB](/4-Releasing-Linear-Quries-with-Correlated-Error/img/SmallDB.png)

我们首先认识到 **SmallDB** 机制是 $\varepsilon$- 差分隐私的。

**命题 4.1** **SmallDB** 机制是 $(\varepsilon,0)$- 差分隐私的。

**【证明】** **SmallDB** 机制是指数机制的一个简单实例，因此其隐私性服从 [**定理3.10**](/3-Basic-Techniques-and-Composition-Theorems/The-exponential-mechanism.html)

我们可以类似地使用指数机制分析方式，对 **SmallDB** 机制进行分析，以了解其效用保证。但是首先，我们必须证明我们选择范围 $\mathcal{R}=\{y \in \mathbb{N}^{|\mathcal{X}|}:\Vert y\Vert _1 = \frac{\log |\mathcal{Q}|}{\alpha^2}\}$，其输出的所有数据库集合的大小为：$\log |\mathcal{Q}|/\alpha^2$。

**定理 4.2** 对于线性查询 $\mathcal{Q}$ 的任何有限类，如果 $\mathcal{R}=\{y \in \mathbb{N}^{|\mathcal{X}|}:\Vert y\Vert _1 = \frac{\log |\mathcal{Q}|}{\alpha^2}\}$ 则对于所有 $x \in \mathbb{N}^{|\mathcal{X}|}$，存在一个 $y \in \mathcal{R}$ 使得：

$$
\max_{f\in \mathcal{Q}}|f(x)-f(y)|\leq \alpha
$$

成立。

换句话说，我们将证明对于线性查询的任何集合 $\mathcal{Q}$，对于任何数据库 $x$，都有一个“小”数据库 $y$，其大小为 $\Vert y\Vert _1 = \frac{\log |\mathcal{Q}|}{\alpha^2}$ ，这个“小”数据库能将 $\mathcal{Q}$ 中每个查询的答案编码，编码误差最大为
$\alpha$。

**引理 4.3（采样边界）** 对于任何 $x \in \mathbb{N}^{|\mathcal{X}|}$ 和 任何
线性查询集合 $\mathcal{Q}$ ，存在一个数据库 $y$，其大小为：

$$
\Vert y\Vert _1=\frac{\log|\mathcal{Q}|}{\alpha^2}
$$

使得：

$$
\max_{f\in \mathcal{Q}}|f(x)-f(y)|\leq \alpha
$$

**【证明】** 令 $m=\frac{\log|\mathcal{Q}|}{\alpha^2}$ 。 我们将从 $x$ 的元素中提取 $m$ 个均匀随机的样本来构建数据库 $y$。 具体来说，对于 $i \in \{1,...,m\}$，令 $X_i$ 为一个以概率  $x_j/\Vert x \Vert_1$ 从 $\chi_j \in \mathcal{X}$ 取值的随机变量，令 $y$ 为包含元素 $X_1,...,X_m$ 的数据库。现在固定任何 $f\in \mathcal{Q}$ 并考虑概率 $f(y)$。我们有：

$$
f(y) = \frac{1}{\Vert y \Vert_1}\sum_{i=1}^{|\mathcal{X}|}y_i\cdot f(\chi_i) = \frac{1}{m} \sum_{i=1}^m f(X_i)
$$

我们注意到和的每个项 $f(X_i)$ 是有界随机变量，取值为 $0 \leq f(X_i) \leq 1$，其期望为：

$$
\mathbb{E}[f(X_i)] = \sum_{j=1}^{|\mathcal{X}|}\frac{x_j}{\Vert x \Vert_1}f(\chi_j) = f(x)
$$

$f(y)$ 的期望为：

$$
\mathbb{E}[f(y)] = \frac{1}{m}\sum_{i=1}^{m}\mathbb{E}[f(X_i)] = f(x)
$$

因此，我们可以应用 [**定理3.1**](/3-Basic-Techniques-and-Composition-Theorems/Useful-probabilistic-tools.html) 中所述的切尔诺夫不等式，这使：

$$
\text{Pr}[|f(y)-f(x)|>\alpha]\leq 2e^{-2m\alpha^2}
$$

在所有线性查询 $f \in \mathcal{Q}$ 上使用布尔不等式，我们得到：

$$
\text{Pr}\big[\max_{f \in \mathcal{Q}}|f(y)-f(x)|>\alpha\big]\leq 2|\mathcal{Q}|e^{-2m\alpha^2}
$$

当 $m=\frac{\log|\mathcal{Q}|}{\alpha^2}$ 使等式右边小于 $1$（只要 $\mathcal{Q}>2$ ），z这证明了存在一个满足上述界限的大小为 $m$ 的数据库，从而完成了引理的证明。证毕。

因为 $\mathcal{R}$ 必包含所有大小为 $\frac{\log|\mathcal{Q}|}{\alpha^2}$ 的数据库，可以简单地得出 **定理4.2** 的证明。

**命题4.4** 令 $\mathcal{Q}$ 为线性查询的任何类别。 令 $y$ 为
$\text{SmallDB}(x,\mathcal{Q},\varepsilon,\alpha)$输出的数据库。然后以 $1-\beta$ 的概率有：

$$
\max_{f\in\mathcal{Q}}|f(x)-f(y)|\leq \alpha + \frac{2\Big(\frac{\log|\mathcal{X}|\log|\mathcal{Q}|}{\alpha^2}+\log(\frac{1}{\beta})\Big)}{\varepsilon\Vert x \Vert_1}
$$