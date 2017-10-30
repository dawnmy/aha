
---
title: "贝叶斯统计与MCMC"
date: 2017-10-29T21:59:26+02:00
draft: false
---

>"When the facts change, I change my mind. What do you do, sir?"  - by John Maynard Keynes

贝叶斯思想的核心是将参数 $\theta$ 当成一个变量，我们不完全相信先验的信息而是基于先验信息并通过真实观测数据来不断更新参数值而得到参数的后验分布，这一点是与频率学派思想的最大区别。

### 贝叶斯定理

$P(Y|X)$ 为已知X发生后Y发生的概率，根据贝叶斯定理，
 
  $$P(
  P(Y|X) = \frac{P(X\bigcap{Y})}{P(X)}=\frac{P(X|Y)\times{P(Y)}}{P(X)}
  $$
 
 其中，
 
- $P(X\bigcap{Y})$ 就是联合概率（joint probability），表示 $X$ ， $Y$ 都发生的概率。
- $P(X)$ 是 $X$ 的先验概率（prior probability）或边缘概率（marginal probability）。
- $P(Y)$ 是 $Y$ 的先验概率或边缘概率。
- $P(Y|X)$ 也叫后验概率（posterior probability）。

假设$Y$是离散的变量， $Y \in \{y1,y2,...,yi,...,yn\}$ ，上式可以改写为

$$
  P(Y=yi|X) = \frac{P(X|Y=yi){P(Y=yi)}}{\sum_{i=1}^n{P(X|Y=yi)P(Y=yi)}}
$$

如果用常用的贝叶斯变量名写法， $x$ 表示观测到的数据， $\theta$ 表示参数：

 $$
  \pi(\theta{i}|x) = \frac{f(x|\theta{i}){\pi(\theta{i})}}{\sum_{i=1}^n{f(x|\theta{i})\pi(\theta{i})}}
  $$
  
其中，$\pi$ 表示参数的概率分布函数，$\pi(\theta{i})$ 是参数的先验概率，$\pi(\theta{i}|x)$ 表示后验概率，$f(x|\theta{i})$表示我们观测的样本数据的分布也就是似然函数（likelihood function）。

上面公式中的参数$\theta$为离散的变量，当参数$\theta$为连续时，我们就不是用求和 $\sum$ 而是积分$\int$了。

$$
\pi(\theta)=\int_\Theta{f(x|\theta)\pi(\theta)}d\theta
$$

于是：

 $$
  \pi(\theta|x) = \frac{f(x|\theta){\pi(\theta)}}{\int_{\Theta}{f(x|\theta)\pi(\theta)}d\theta}
  $$

对于多变量参数，假设 $\theta\in\{\theta_1, \theta_2\}$，则条件概率的计算如下：

$$
P(\theta_1|x)
$$

$$
P(\theta_1|x)=d\theta_2
$$

因为：

$$
P(\theta_1,\theta_2|y)=P(\theta_1|y,\theta_2)P(\theta_2|y)
$$

于是：

$$
P(\theta_1|x)=\int_{\Theta_2}{P(\theta_1|y,\theta_2)P(\theta_2|y)}d\theta_2
$$

### 共轭分布

- $\beta$分布

如果观测数据服从二项分布 $x \sim B(n,\theta)$，先验分布服从 $\beta$ 分布 $\pi(\theta)\sim Beta(a, b)$，则后验分布 $\pi(\theta|x) \sim Beta(x+a,n-x+b)$。这时候似然函数二项分布的共轭先验分布为$\beta$分布。在贝叶斯统计中，如果后验分布与先验分布属于同类，则先验分布与后验分布被称为共轭分布，而先验分布被称为似然函数的共轭先验。
共轭先验的好处是我们不需要去计算贝叶斯公式中的复杂的积分也就是式中的分母(归一化项)来计算后验分布。

- 常用共轭先验


| Likelihood| Conjugate prior distribution |
| --- | --- | --- |
| Binomial | Beta |
| Poisson  | Gamma |
| Negative binomial | Beta |
| Categorical | Dirichlet |
| Multinomial | Dirichlet |
| Hypergeometric | Beta-binomial |
| Normal | Normal |


### 非共轭先验时后验分布的获得

- 通过概率密度（PDF）函数的近似函数，变分推断（variational inference）
- 格点近似法（grid approximation）来计算积分，仅适用于变量少且样本空间小的情况。
- 基于反CDF (cumulative distribution function) 函数 (inverse CDF) 采样。
- 随机采样（stochastic sampling）


### Sampling methods

#### Rejection sampling

此方法最出名的例子是布丰投针估算圆周率，该方法的实现是首先构建一个建议分布$q(x)$，然后在建议分布中均匀取样，落在待估函数$p(x)$内的样本被接受，以外的被拒绝。该方法能工作，但是会导致大量无效采样，浪费计算资源，因此其改进版Adaptive Rejection Sampling（ARS）被开发出来，ARS通过对待估函数分段做切线，切线围成的区域为建议分布。ARS仅对于concave或者log-concave的函数十分有效。

#### Importance sampling and adaptive importance sampling

$x$是取自$p(x)$中的样本，$x \sim p(x)$，现在我们想求$f(x)$的期望，

$$
E(f(x))=\int_x{f(x)p(x)}dx \\
x^{(i)} \sim p(x^{(i)}) \\
\hat{E}(f(x))=\frac{1}{N}\sum_{i=1}^Nf(x^{(i)})
$$

而往往，从$p(x)$ 中采样是有困难的，尤其当密度函数非常复杂的时候，于是我们引入简单的建议密度函数$q(x)$，采样$x \sim q(x)$，
于是

$$
E(f(x))=\int_x{f(x)\frac{p(x)}{q(x)}q(x)}dx \\
x^{(i)} \sim q(x^{(i)}) \\
\hat{E}(f(x))=\frac{1}{N}\sum_{i=1}^Nf(x^{(i)}\frac{p(x^{(i)})}{q(x^{(i)})})
$$

其中$p(x^{(i)})/q(x^{(i)})=w(x^{(i)})$ 就是重要性权重。

#### CDF反函数均匀采样

如果已知后验分布的CDF的反函数$CDF^{-1}$，我们可以均匀采样 $CDF(x) = u \sim U(0,1)$，然后通过$CDF$的反函数得到对应的$x$的样本。

$$CDF(x)=\int_{-\infty}^{x}PDF(t)dt$$

$$
u \sim U(0,1)\\
x=CDF^{-1}(u)
$$

#### MCMC

前面三种采样方法都有蒙特卡洛（MC）的思想，接下来我们介绍的是MCMC方法，多了一个MC。MCMC全称马尔可夫链蒙特卡洛方法（Markov chain Monte Carlo, MCMC）是通过构建状态转移概率矩阵使其平稳分布为待估参数的后验分布。满足细致平衡条件的状态转移概率矩阵可以收敛到指定的概率分布。
MCMC方法满足细致平衡原则 （detailed balance principle），也就是$x$到$x^*$的概率等于$x^*$到$x$的概率：

$$\pi(x)k(x\to{x^*})=\pi(x^*)k(x^*\to{x})$$

其中，$\pi(x)$为出现样本$x$的概率，$k$为transition kernel函数表示样本间转移的概率。满足该原则的状态转移概率矩阵才能使得后验分布收敛。

- Metropolis-Hastings

取样（转移概率）: 

$$x^* \sim q(x^*|x)$$

接受概率：

$$P_{xx^*}=min(1,\frac{\pi(x^*)q(x|x^*)}{\pi(x)q(x^*|x)})$$

$u \sim U(0,1)$，如果 $u < P_{xx^*}$，则$X_{t+1}=x^*$，否则$X_{t+1}=x$

为了提高MH方法的效率，很多改进方法被提出来，其中很常见的是Hybrid MH，比如结合梯度信息来决定跳转的步长。

- Gibbs

Gibbs可能是我们最常听说的采样方法了，她是MH方法接受概率为1时的特例。这样收敛更快，效率更高。Gibbs采样最适合高维变量的后验分布采样。比如，服从$P(x,y,z)$。
举个栗子，
给出一个初始样本 $(x^{(1)}, y^{(1)}, z^{(1)})^T$，我们希望采样取自$P(x, y, z)$ 的样本$(x^{(2)}, y^{(2)}, z^{(2)})^T$，$ (x^{(3)}, y^{(3)}, z^{(3)})^T$ .. $(x^{(N)}, y^{(N)}, z^{(N)})^T$
我们定义

$$
x^{(2)} \sim P(x|y^{(1)},z^{(1)}) \\
y^{(2)} \sim P(y|x^{(2)},z^{(1)}) \\
z^{(2)} \sim P(z|x^{(2)},y^{(2)}) \\
x^{(3)} \sim P(x|y^{(2)},z^{(2)}) \\
y^{(3)} \sim P(y|x^{(3)},z^{(2)}) \\
z^{(3)} \sim P(z|x^{(3)},y^{(3)}) \\
$$

该定义符合细致平衡条件，这里就不详细证明了。

这种原始的Gibbs采样显然是不可能并行化的，因为下一步的采样依赖于上一步的结果。于是很多改进方法被开发出来，比如引入辅助变量$u$，使得概率密度函数为$P(x,y,z,u)$

$$ u \sim P(u|x,y,z)$$

于是，

$$ P(x,y,z|u)=P(x|u)P(y|u)P(z|u)$$ 

使得 $x,y,z$ 独立。

- slice sampling

其实slice sampling也属于MCMC，他将要采样的复杂分布分片成


#### other sampling methods

- nested sampling


### MCMC方法的使用

- PyMC3
- Stan
- winBUGS
- openBUGS


| left | center | right |
| :--- | :----: | ----: |
| aaaa | bbbbbb | ccccc |
| a    | b      | c     |


####参考资料：

+ Doing Bayesian Data Analysis (Book)
+ Bayesian Data Analysis (Book)
+ 徐亦达——机器学习 (课程)
+ [机器学习(一) —— 浅谈贝叶斯和MCMC](http://www.xuyankun.cn/2017/05/13/bayes/)
+ [Probabilistic Programming
and Bayesian Methods for Hackers] (http://camdavidsonpilon.github.io/Probabilistic-Programming-and-Bayesian-Methods-for-Hackers)


