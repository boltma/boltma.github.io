---
title: Fisher Information
date: 2019-05-02 22:49:24
tags:
---

# Fisher Information

**Fisher Information（费希尔信息）**是用以衡量观测数据所蕴含的数据量，具体说来是指观测所得的随机变量$X$携带的关于未知参数$\theta$的信息量，其中$X$的概率分布依赖于$\theta$，通常记作${\mathcal {I}}_{X}(\theta)$。

对于概率分布$f(X;\theta)$（满足$f(x;\theta )\geq0, \int f(x;\theta )dx=1,\quad\forall\theta\in\Theta$，$\Theta$为参数集），称对数似然函数关于$\theta$的偏导$\frac {\partial }{\partial \theta }\log f(X;\theta )$为其**得分**（Score function），对于真实的参数$\theta$，则有此得分的期望为0：

${\displaystyle {\begin{aligned}\operatorname {E} \left[\left.{\frac {\partial }{\partial \theta }}\log f(X;\theta )\right|\theta \right]&=\int {\frac { {\frac {\partial }{\partial \theta }}f(x;\theta )}{f(x;\theta )}}f(x;\theta )\,dx\\&={\frac {\partial }{\partial \theta }}\int f(x;\theta )\,dx\\&={\frac {\partial }{\partial \theta }}1=0.\end{aligned}}}$

并将得分的方差定义为**费希尔信息**：

${\displaystyle {\mathcal {I}}(\theta )=\operatorname {E} \left[\left.\left({\frac {\partial }{\partial \theta }}\log f(X;\theta )\right)^{2}\right|\theta \right]=\int \left({\frac {\partial }{\partial \theta }}\log f(x;\theta )\right)^{2}f(x;\theta )\,dx}$

并且在密度函数具有良好性质的情况下，可以用分部积分很容易证明：

${\mathcal {I}}(\theta )=-\operatorname {E} \left[\left.{\frac {\partial ^{2}}{\partial \theta ^{2}}}\log f(X;\theta )\right|\theta \right]$

这个表达式以如下方式表达了观测所携带的信息量：若对数似然函数较为平坦，则我们对$\theta$的估计较差，反之，若对数似然函数有高而窄的峰，则我们可以得到对$\theta$的较好估计，而这个性状由负二阶导数表征。

由于n个样本的对数似然函数为单个似然函数之和，容易证明，n个独立同分布样本的费希尔信息是单个样本的费希尔信息的n倍。

# Cramér-Rao Bound

费希尔信息的重要性在于**Cramér-Rao不等式**，或**Cramér-Rao Bound（克拉美罗界）**：

费希尔信息的倒数是参数$\theta$的任何无偏估计$\hat\theta$的方差的下界，即$\operatorname {var} ({\hat {\theta }})\geq {\frac {1}{\mathcal {I}(\theta )}}$

> 关于参数$\theta$的估计$\hat\theta$的偏定义为估计的误差的期望值，即$\operatorname {Bias} _{\theta }[\,{\hat {\theta }}\,]=\operatorname {E} _{\theta }[\,{\hat {\theta }}\,]-\theta =\operatorname {E} _{\theta }[\,{\hat {\theta }}-\theta \,]$，其中$\operatorname {E} _{\theta }$表示期望是相对密度函数$f(X;\theta)$而言的。
>
> 若对于所有$\theta\in\Theta$，偏为0，则称此估计为无偏估计。
>
> 例如样本均值是总体均值的无偏估计量，样本方差是总体方差的无偏估计量，而标准差是总体标准差的有偏估计量。

估计无偏并不能保证误差以极大的概率是低的。例如对于正态分布${\mathcal {N}}(\theta ,1)$，设$X_1,X_2,\cdots,X_n$是抽自它的独立同分布样本。估计$\theta$时，$X_1$与$\bar {X_n}$均为无偏估计，然而显然使用更多数据会得到更好的估计，事实上也可以证明$\bar {X_n}$是最小方差无偏估计量，也即达到了**克拉美罗界**。

达到克拉美罗界的无偏估计量优越于其他所有估计，也即${\displaystyle \operatorname{E}[(\hat\theta_1(X_1,X_2,\cdots,X_n)-\theta)^{2}]\leq\operatorname{E}[(\hat\theta_2(X_1,X_2,\cdots,X_n)-\theta)^{2}]}$，若$\hat\theta_1$达到了克拉美罗界。

**Cramér-Rao不等式的证明：**

设$V$是得分函数，$\hat\theta$是估计量。由Cauchy-Schwartz不等式，可得

$$\operatorname{E}_\theta[(V-\operatorname{E}_\theta V)(\hat\theta-\operatorname{E}_\theta\hat\theta)] \leq \operatorname{E}_\theta(V-\operatorname{E}_\theta V)^2\operatorname{E}_\theta(\hat\theta-\operatorname{E}_\theta \hat\theta)^2$$

由于$\hat\theta$是无偏估计，所以对于任意$\theta$均有$\operatorname{E}_\theta \hat\theta=\theta$，同时得分函数的期望也为零（见上），并且结合费希尔信息的定义（${\mathcal {I}}(\theta )=\operatorname {E}[V^2]$），代入上式有$\operatorname {E}_\theta[V\hat\theta]\leq\mathcal{I}(\theta)\operatorname{var}(\hat\theta).$

而

$${\displaystyle \begin{aligned} \operatorname {E}_\theta[V\hat\theta]&=\int\frac{\frac{\partial}{\partial\theta}f(x;\theta)}{f(x;\theta)}\hat\theta(x)f(x;\theta)dx \\ &= \int \frac{\partial}{\partial\theta} f(x;\theta)\hat\theta(x)dx \\ &= \frac{\partial}{\partial\theta}\int f(x;\theta)\hat\theta(x)dx \\ &= \frac{\partial}{\partial\theta} \operatorname{E}_\theta[\hat\theta] \\ &= \frac{\partial}{\partial\theta}\theta = 1\end{aligned}}$$

（这里能交换积分与微分号是假定密度函数具有良好性质，上同）

代入即得到Cramér-Rao不等式。																																$\square$ 

以相同的证明方式可以得到对于任意估计量有$\operatorname {var} \left({\hat {\theta }}\right)\geq {\frac {[1+b'(\theta )]^{2}}{\mathcal{I}(\theta )}}$，此处$b(\theta)=\operatorname{E}_\theta[\hat\theta]-\theta.$

# 多参数情形的Fisher Information

多参数下有**费希尔信息矩阵**$\mathcal{I}(\theta)$，其中元素为$\mathcal{I}_{m,k}=\operatorname {E} \left[{\frac {\partial }{\partial \theta _{m}}}\log f\left(x;\theta\right){\frac {\partial }{\partial \theta _{k}}}\log f\left(x;\theta\right)\right]=-\operatorname {E} \left[{\frac {\partial ^{2}}{\partial \theta _{m}\partial \theta _{k}}}\log f\left(x;\theta\right)\right]$

**Cramér-Rao不等式**变为：$\Sigma\geq\mathcal{I}^{-1}(\theta)$，这里矩阵不小于号指差是半正定的，$\Sigma$是关于$\theta$的一组无偏估计量的协方差矩阵。

