## Dirichlet Process

形式化记作：$G \sim DP(\alpha, H)$

**dirichlet distribution**：$P(x_1...x_i...x_k) \sim DIR(\alpha _1...\alpha _i ... \alpha _k)$，则$E[x_i] = \frac{\alpha _i}{\sum \alpha}$，$Var[x_i] = \frac{\alpha _i (\sum \alpha - \alpha _i)}{(\sum \alpha)^2(\sum \alpha + 1)}$

**性质**：$G \sim DP(\alpha, H) \Leftrightarrow (G(a_1)...G(a_n)) \sim DIR(\alpha H(a_1)...\alpha H(a_n))$，也就是在一个区间$a_i$中，$G(a_i)$产生的离散分布$G$ 在这个区间内的概率和的均值是$H(a_i)$，可以代入上面**dirichlet distribution**来进行推导。

**Beta分布**：$X \sim Beta(a,b)$，则$E(X) = \frac{a}{a+b}$

**取样方式**：stick-breaking，每次取一个样本，样本的权重从**Beta 分布**得到。



