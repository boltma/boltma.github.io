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

费希尔信息的三种观点：

- 得分（对数似然函数的偏导）的方差
- 对数似然函数负二阶偏导的期望
- 最大似然估计渐近分布的方差的倒数

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

# 与其它散度测度的关系

## 与熵的关系

以$f(x)$为密度函数的随机变量$X$的微分熵（differential entropy）定义为：

$$h(x)=-\int_{S}f(x)\log f(x)dx$$
对$\epsilon>0$及任意$n$，定义$f(x)$的典型集$A_{\epsilon}^{(n)}$如下：

$$\begin{aligned} A_{\epsilon}^{(n)}=\{&(x_1,x_2,\cdots,x_n)\in S^n: \\ &\left|-\frac{1}{n}\log f(x_1,x_2,\cdots,x_n)-h(X)\right|\leq\epsilon \} \end{aligned}$$
这里$f(x_1,x_2,\cdots,x_n) = \prod\limits_{i=1}^{n}f(x_i)$

典型集是所有概率$\geq1-\epsilon$的集合中体积最小的。

熵表征了典型集的体积（典型集体积渐近趋于$2^{nh}$，其中$h$为熵），而费希尔信息与典型集的表面积相关。

## de Bruijn恒等式

设$X$为任一随机变量，其密度函数为$f(x)$且方差有限。令$Z$是与$X$独立的正态分布的随机变量，均值为0，方差为1。则：

$$\frac{\partial}{\partial t}h_e(X+\sqrt{t}Z)=\frac{1}{2}\mathcal{I}(X+\sqrt{t}Z)$$

此处$h_e$表明微分熵公式中底数为$e$，费希尔信息是关于随机变量分布的费希尔信息。特别地，若$t\rightarrow 0$时极限存在，则$\frac{\partial}{\partial t}h_e(X+\sqrt{t}Z)\big|_{t=0}=\frac{1}{2}\mathcal{I}(X)$

通常$t$被视作一种扰动。

## 卷积不等式

$$\frac{1}{\mathcal{I}(X+Y)} \geq \frac{1}{\mathcal{I}(X)} + \frac{1}{\mathcal{I}(Y)}$$

## 熵幂不等式

设$\mathbf{X}$和$\mathbf{Y}$为相互独立的$n$维随机向量，它们的密度函数已知，则：

$$2^{\frac{2}{n}h(\mathbf{X}+\mathbf{Y})}\geq 2^{\frac{2}{n}h(\mathbf{X})}+2^{\frac{2}{n}h(\mathbf{Y})}$$

对于两个独立的随机变量$X$和$Y$，$h(X+Y)\geq h(X'+Y')$，这里$X'$和$Y'$是相互独立的正态分布的随机变量，且满足$h(X')=h(X)$且$h(Y')=h(Y)$。

# Fisher Information & Natural Gradient

在概率分布函数具备良好性质时，Fisher信息矩阵和KL散度的Hesse矩阵的相反数相等。因而在牛顿迭代法中，使用Fisher信息矩阵代替Hesse矩阵，有时更易求解。

> **Natural Gradient（自然梯度法）**
>
> $${\displaystyle \theta _{m+1}=\theta _{m}+\eta_m{\mathcal {I}}^{-1}(\theta _{m})V(\theta _{m})}$$
>
> （$V$为关于$\theta$的得分函数）
>

自然梯度法考虑了参数的不同维度对目标函数不同的影响，加速了梯度下降法的收敛。

SGD（随机梯度下降）中，假设中心极限定理，随机梯度作为一个随机变量，满足正态分布。那么迭代中，相当于使用全部数据计算的梯度，附加上协方差矩阵，即：（$t$为mini-batch的规模）

$${\displaystyle \theta _{m+1}=\theta _{m}+\eta_m V_0(\theta _{m})}+\frac{\eta_m}{\sqrt{t}}\epsilon_m, \epsilon_m\sim\mathcal{N}(0,\hat{\Sigma}(\theta_m))$$

上式中，协方差与目标函数的曲率在真实参数上相等。而在迭代过程中也近似相等。

这表明这样的SGD中的噪声遵循目标函数的曲率，使得迭代时更容易逃离局部的sharp minima，进入flat minima，从而得到泛化能力更强的解。因而使用自然梯度的随机梯度下降能获得更强的泛化能力，也通常能加快迭代速度。

# References

1. Wikipedia contributors, "Fisher information —— Wikipedia, the free encyclopedia," 2019.
   [Online; accessed 24-May-2019].

2. T. M. Cover and J. A. Thomas, *Elements of information theory.*
   John Wiley & Sons, 2012.

3. J. Duchi, "Lecture notes for statistics 311/electrical engineering 377," URL:[https://stanford.edu/class/stats311/Lectures/full_notes.pdf](https://stanford.edu/class/stats311/Lectures/full_notes.pdf) .
   Last visited on, vol. 2, p. 23, 2016.