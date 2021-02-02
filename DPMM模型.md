## DPMM模型

全称是Dirichlet Process Multinomial Mixture.是DMM(Dirichlet Multinomial Mixture)模型的一个升级，主要是不用预先指定聚类的类别数，默认为无穷。

DPMM模型的规范化。
$$
\Theta|\alpha \sim Dir(\alpha) \\
\Zeta _d | \Theta \sim Mult(\Theta) d=1,...,D\\
\Phi _k |\beta \sim Dir(\beta)  \ k=1,...,K \\
d|Z_d,\{\Phi _k\}^{K}_{k=1} \sim p(d|\Phi _{Z_d})
$$

> dirichlet 分布，是beta 分布的多元推广，
>
> 要理解beta 分布的作用，需要理解伯努利过程(Bernoulli process)。
>
> 伯努利过程是从重复的独立实验（此实验只有两个结果）里面抽样，得到random sample X = (x_1,x_2,...,x_n)，基于这n次观察，估计一下q(伯努利实验取值为正的概率)的分布（由于做的实验是有限的，所以True/N不是太靠谱，所以只能估计一个大概的范围。）也就是f(q|X)，通过一系列推导，可以得到，$f(q|x) \sim P(X=x|q)f(q)$,$f(q|x) \sim q^k(1-q)^{n-k}$，$q^k(1-q)^{n-q}$乘上[0,1]的均匀分布就是一个beta 分布。
>
> 那么，将结果只有两个结果的实验拓展到n个结果，求出来的beta 分布的拓广，就是Dirichlet 分布，