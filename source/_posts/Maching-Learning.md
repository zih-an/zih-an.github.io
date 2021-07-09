---
title: 机器学习知识点
tags: [ML]
categories:
  - [ML]
date: 2021-01-05 19:21:53
cover: bg.jpg
katex: true
top_img: false
---

> 写在前面：
> 1. 所有向量如不加描述均视为列向量
> 2. 数据集为m个数据，n个特征，第i个数据表示为：$(x^{(i)}, y^{(i)})$


## What is ML?
A computer program is said to learn from experience E with respect to some class of tasks T and performance measure P, if its performance at tasks in T, as measured by P, improves with experience E.

## Linear Regression
- **Hypothesis:** $h_{\theta}(x)=X{\theta}$ ($x_0^{(i)}=1$)
- **Cost Function:** $J(\theta)=\frac{1}{2}\sum_{i=1}^{m} (h_{\theta}(x^{(i)})-y^{(i)})^2=\frac{1}{2}(X\theta-Y)^T(X\theta-Y)$
- **Derivatives:** $\nabla J(\theta) = \sum_{i=1}^{m}(h_{\theta}(x^{(i)})-y^{(i)})x^{(i)}= X^TX\theta-X^Ty$
- **Objective:** $\mathop{min}\limits_{\theta} J(\theta)$

### Gradient Descent Algorithm
寻找最小值$\mathop{min}\limits_{\theta} J(\theta)$ => 找到一阶导数为0的点$\nabla J(\theta)=0$：
```pseudo-code
repeat:
    计算J(θ)梯度J'
    θ = θ - α*J'
until convergence criterion
```
Convergence Criterion: $\left\| \nabla J(\theta) \right\|_2 \le \epsilon$
> - 对应的梯度上升算法(Gradient Ascent): 用于最大化maximize
>   + $\theta_j=\theta_j + \alpha \frac{\partial J(\theta)}{\partial \theta_j}$
> - 随机梯度下降(SGD)
>   + 每次随机打散数据
>   + 更新$\theta_i$时，仅使用第i组数据计算$J(\theta)$

### Matrix Derivatives
$n \times n$的矩阵的迹(trace): $trM = \sum_{i=1}{n}M_{ii}$，即对角线值之和。

$n \times n$的矩阵的性质：
- $\mathop{tr}ABCD = \mathop{tr}DABC = \mathop{tr}CDAB = \mathop{tr}BCDA$
- 非梯度类
    + $\mathop{tr}A = \mathop{tr}A^T$
    + $\mathop{tr}(A+B) = \mathop{tr}A + \mathop{tr}B$
    + $\mathop{tr}aA = a\mathop{tr}A$
    + $\nabla_{A^T}f(A)=(\nabla_A f(A))^T$
- 含梯度类
    + $\nabla_A \mathop{tr}AB = B^T$
    + $\nabla_A|A| = |A|(A^{-1})^T$
    + $\nabla_{A}ABA^TC = CAB + C^TAB^T$
        * 与上式转置关系：$\nabla_{A^T}ABA^TC = B^TA^TC^T + BA^TC$

#### 推导梯度公式
$$\begin{aligned}\nabla_{\theta} J(\theta) &= \nabla_{\theta} \frac{1}{2}(X\theta-Y)^T(X\theta-Y)\\
&= \nabla_{\theta} \frac{1}{2}\mathop{tr}(\theta^TX^TX\theta - \theta^TX^TY - Y^TX\theta + Y^TY)\\
&= \nabla_{\theta} \frac{1}{2} [\mathop{tr}(\theta^TX^TX\theta) - \mathop{tr}(\theta^TX^TY + Y^TX\theta)]\\
&= \nabla_{\theta} \frac{1}{2} \mathop{tr}(\theta^TX^TX\theta) - X^TY\\
&= X^TX\theta - X^TY
\end{aligned}
$$
> 因为实数即为$1 \times 1$矩阵
> - 3->4：利用公式
>   + $\mathop{tr}A=\mathop{tr}A^T$
>   + $\mathop{tr}ABCD = \mathop{tr}DABC = \mathop{tr}CDAB = \mathop{tr}BCDA$
>   + $\nabla_A \mathop{tr}AB = B^T$
> - 4->5：利用公式
>   + $\nabla_{A^T}ABA^TC = B^TA^TC^T + BA^TC$
>       * 此处将$B=X^TX$

#### 正规方程Normal Equation
由上述梯度公式，并求解$\nabla J(\theta)=0$，得到：
$$\theta = (X^TX)^{-1}X^TY$$

## Logistic Regression
- **sigmoid**: $g(z)=\frac{1}{1+e^{-z}}$
    + bound: $g(z) \in (0,1)$
    + symmetric: $g(-z)=1-g(z)$
    + gradient: $g'(z) = g(z)(1-g(z))$
- **hypothesis**: $h_{\theta}(x) = g(\theta^Tx)$
- **cost function(MLE)**: $l(\theta)=\sum_{i=1}^m (y^{(i)}\mathop{log} h_{\theta}(x^{(i)})+(1-y^{(i)})\mathop{log}(1-h_{\theta}(x^{(i)}))$
- **Objective:** $\mathop{max}\limits_{\theta} l(\theta)$
    + 注意此处是最大化，使用梯度上升
    + 若对cost function增加负号，即为最小化，使用梯度下降

### Cost Function推导
$h_{\theta}(x)$是1的概率。逻辑回归的概率表达$p(y|x;\theta)=p(Y=y|X=x;\theta)$：
- 1: $p(Y=1|X=x;\theta)=h_{\theta}(x)=g(\theta^T x)$
- 0： $p(Y=0|X=x;\theta)=1-h_{\theta}(x)=g(-\theta^Tx)$
- 综上，$p(y|x;\theta)=(h_{\theta}(x))^y (1-h_{\theta}(x))^{(1-y)}$

#### 极大似然估计MLE
> $y\in\{0,1\}$: 
> 
> - 使用当前概率表达式的似然函数：
> $$L(\theta) = \prod_{i=1}^m (h_{\theta}(x))^{y^{(i)}} (1-h_{\theta}(x))^{(1-y^{(i)})}$$
> 
> - 直接取对数，即为上述cost function：
> $$l(\theta) = \mathop{log}L(\theta) = \sum_{i=1}^m \big(y^{(i)}\mathop{log} h_{\theta}(x^{(i)})+(1-y^{(i)})\mathop{log}(1-h_{\theta}(x^{(i)}) \big)$$
> - 求导(梯度)：
> $$\frac{\partial l(\theta)}{\partial\theta} = \sum_{i=1}^{m} (y^{(i)}-h_{\theta}(x^{(i)}))x^{(i)}$$
> - 使用梯度上升求解：$\theta = \theta + \alpha\frac{\partial l(\theta)}{\partial\theta}$
>     + 提出负号，等同于线性回归中的梯度下降

设定$y\in\{-1,1\}$，并假设服从伯努利分布，改写概率表达式：
$$p(y|x;\theta)=\frac{1}{1+exp(-y\theta^Tx)}$$

新似然函数：
$$L(\theta) = \prod_{i=1}^m \frac{1}{1+exp(-y^{(i)}\theta^Tx)}$$

取对数：
$$l(\theta) = -\sum_{i=1}^m \mathop{log}\big(1+exp(-y^{(i)}\theta^Tx)\big)$$

MLE估计值为：
$$\begin{aligned}\theta_{MLE} &= arg\mathop{max}\limits_{\theta} -\sum_{i=1}^m \mathop{log}\big(1+exp(-y^{(i)}\theta^Tx)\big)\\
&= arg\mathop{min}\limits_{\theta} \sum_{i=1}^m \mathop{log}\big(1+exp(-y^{(i)}\theta^Tx)\big)
\end{aligned}
$$

### Newton's Method牛顿法
牛顿法目标：求解$f(x)=0$
<img src="fig1.jpg" width="400" height="250">
- 基本公式：$x_2 = x_1 - \frac{f(x_1)}{f'(x_1)}$
- 在LR模型中的最优化，目标是找到$f'(x)=0$
    + 转换公式为$x_2 = x_1 - \frac{f'(x_1)}{f''(x_1)}$
    + $\theta = \theta - H^{-1}\nabla_{\theta}l(\theta)$，其中$H$为hessian matrix

**牛顿法与梯度下降的对比：**
根据公式，将梯度下降法中的$\alpha$与牛顿法中的$\frac{1}{f'(x)}$对应（假设寻找$f(x)=0$的点）。因此，某点下降时所沿直线的斜率在梯度下降中为$\frac{1}{\alpha}$，在牛顿法中为$f'(x)$

<img src="fig2.jpg">

显然，在同一点，梯度下降法每次都是同样slope($\frac{1}{\alpha}$)，因此是同样的step size；而牛顿法每次都根据新的slope($f'(x)$)重新调整step size。在大部分点$x_n$上，牛顿法明显快于梯度下降法。
同时，此图也解释了梯度下降法中大$\alpha$速度快，但不能太大的原因。

### 多分类
#### One-vs.-All
思想：
- 将第k类赋值$y^{(k)}=1$，其余为0，分别训练m个分类器$f_k(x)$
- 对每一个分类器求$f_k(x)$的值，取最大值的分类结果

#### One-vs.-One
思想：
- 两两之间训练分类器，共$\frac{k(k-1)}{2}$个
- 对每个分类器求值，取最大值作为分类结果

## Regularization and Bayesian Statistics
为解决overfitting的问题，引入正则化Regularization
> - 在Cost Function中加入一个正则化项(惩罚项)
> - 正则化项(惩罚项)的$\lambda$值越大，越不容易overfitting，更容易underfitting

### Regularized Linear Regression
**Cost Function形式：**
$$J(\theta) = \frac{1}{2m} \sum_{i=1}^m\big(h_{\theta}(x^{(i)})-y^{(i)}\big)^2 + \frac{\lambda}{2m} \sum_{j=1}^n\theta_j^2$$
**Gradient Descent梯度下降：**
$$\begin{aligned}&\theta_0 = \theta_0 - \alpha\frac{1}{m}\sum_{i=1}^{m}(h_{\theta}(x^{(i)})-y^{(i)})x_0^{(i)} \\
&\theta_j = \theta_j - \alpha\big[\frac{1}{m}\sum_{i=1}^{m}(h_{\theta}(x^{(i)})-y^{(i)})x_j^{(i)} + \frac{\lambda}{m}\theta_j\big]
\end{aligned}$$
> $\theta_0$项不需要正则化惩罚

**Normal Equation正规方程：**
$$\theta = (X^TX+\lambda L)^{-1}X^Ty$$
其中，
$$L = \begin{bmatrix}0&&&\\&1&&\\&&\ddots&\\&&&1\end{bmatrix}$$
> 证明：
> 

### Regularized Logistic Regression
**Cost Function形式(此处对MLE结果增加负号以使用梯度下降)：**
$$l(\theta)=-\frac{1}{m}\sum_{i=1}^m (y^{(i)}\mathop{log} h_{\theta}(x^{(i)})+(1-y^{(i)})\mathop{log}(1-h_{\theta}(x^{(i)})) + \frac{\lambda}{2m}\sum_{j=1}^n\theta_j^2$$
> 对求和符号前常数系数的说明：是否有2、是否有m，只是为求导的方便增加，不改变结果和形式的本质

**Gradient Descent梯度下降(与线性回归的公式相同)：**
$$\begin{aligned}&\theta_0 = \theta_0 - \alpha\frac{1}{m}\sum_{i=1}^{m}(h_{\theta}(x^{(i)})-y^{(i)})x_0^{(i)} \\
&\theta_j = \theta_j - \alpha\big[\frac{1}{m}\sum_{i=1}^{m}(h_{\theta}(x^{(i)})-y^{(i)})x_j^{(i)} + \frac{\lambda}{m}\theta_j\big]
\end{aligned}$$

## 概率估计模型：MLE与MAP
### MLE与MAP基本
MLE：最大似然估计 -数据D未发生估计$\theta$
- 频率学派
- $p(D;\theta)$：联合概率，待估参数为$\theta$
    + $L(\theta)=p(D;\theta) = \prod_{i=1}^m p(d^{(i)}; \theta)$
    + $l(\theta)=\mathop{log} p(D;\theta) = \sum_{i=1}^m \mathop{log}p(d^{(i)}; \theta)$

MAP：最大后验估计 -数据D已发生估计$\theta$
- 概率学派
- $p(\theta|D)$：条件概率，使用贝叶斯定理估计$\theta$
    + $p(\theta|D) = \frac{p(\theta)p(D|\theta)}{p(D)}$
    + $p(\theta|D)\propto p(\theta)p(D|\theta)$

> 将MLE与MAP求出的公式看作cost function

高斯分布：
$$X\sim N(\mu,\sigma^2)\\
f(x) = \frac{1}{\sqrt{2\pi\sigma^2}}\mathop{e}^{-\frac{(x-\mu)^2}{2\sigma^2}}
$$
> 关于似然函数$l(\theta)=p(D;\theta)$的D(x,y)联合概率、条件概率问题:
> - $p(D)$表示某个观测事件的概率，需要最大化
> - $\theta$仅表示待估参数
> - 由于模型的假设和前提不同，对于$p(D)$取联合概率还是条件概率并不一定，比如：
>   + 在MLE/MAP中：$p(D)=p(y|x)$
>   + 在naive bayes中$p(D)=p(x,y)$
>       * 在naive bayes中目标也是求$p(D)=p(y|x)$，经公式转换后变为求$p(D)=p(x,y)$
>       * 因此目标变成$p(D)=p(x,y)$，但本质相同，目标还是求$p(D)=p(y|x)$

### MLE
$$\begin{aligned}\theta_{MLE} &= arg\mathop{max}\limits_{\theta} l(\theta)\\
&= arg\mathop{max}\limits_{\theta} \sum_{i=1}^m \mathop{log} p(d^{(i)}; \theta)
\end{aligned}
$$

#### MLE求解Linear Regression
假设服从分布：$y^{(i)}|x^{(i)} \sim N(\theta^Tx, \sigma^2)$
有：
$$\begin{aligned}p(d^{(i)};\theta) &= p(y^{(i)}|x^{(i)};\theta)\\
&= \frac{1}{\sqrt{2\pi\sigma^2}}\mathop{e}^{-\frac{(y^{(i)}-\theta^Tx)^2}{2\sigma^2}}
\end{aligned}
$$

求解：
$$\begin{aligned}l(\theta) &= \sum_{i=1}^m \mathop{log} p(d^{(i)};\theta)\\
&= \sum_{i=1}^m \mathop{log} \frac{1}{\sqrt{2\pi\sigma^2}}\mathop{e}^{-\frac{(y^{(i)}-\theta^Tx)^2}{2\sigma^2}}\\
&= m\mathop{log}\frac{1}{\sqrt{2\pi\sigma^2}} + \sum_{i=1}^m -\frac{(y^{(i)}-\theta^Tx)^2}{2\sigma^2}
\end{aligned}
$$

由上式得到目标：
$$\begin{aligned}& \mathop{max}\limits_{\theta} m\mathop{log}\frac{1}{\sqrt{2\pi\sigma^2}} + \sum_{i=1}^m -\frac{(y^{(i)}-\theta^Tx)^2}{2\sigma^2}\\
& \mathop{max}\limits_{\theta} \sum_{i=1}^m -\frac{(y^{(i)}-\theta^Tx)^2}{2\sigma^2}\\
& \mathop{max}\limits_{\theta} -\sum_{i=1}^m (y^{(i)}-\theta^Tx)^2 \\
& \mathop{min}\limits_{\theta} \sum_{i=1}^m (y^{(i)}-\theta^Tx)^2
\end{aligned}
$$
结果为(非正则化的Cost Function)：
$$\theta_{MLE} = arg\mathop{min}\limits_{\theta} \sum_{i=1}^m (h_{\theta}(x)-y^{(i)})^2$$

#### MLE求解Logistic Regression
假设$y\in\{-1,1\}$，则有概率表达式：
$$p(y|x;\theta)=\frac{1}{1+exp(-y\theta^Tx)}$$
$$\theta_{MLE} = arg\mathop{min}\limits_{\theta} \sum_{i=1}^m \mathop{log}\big(1+exp(-y^{(i)}\theta^Tx)\big)
$$
> 具体详见Logistic Regression部分


### MAP
$$\begin{aligned}\theta_{MAP} &= arg\mathop{max}\limits_{\theta}\frac{p(\theta)p(D|\theta)}{p(D)}\\
&= arg\mathop{max}\limits_{\theta} p(\theta)p(D|\theta)\\
&= arg\mathop{max}\limits_{\theta} \mathop{log} p(\theta)p(D|\theta)\\
&= arg\mathop{max}\limits_{\theta} (\mathop{log} p(\theta)+\mathop{log} p(D|\theta)) \\
&= arg\mathop{max}\limits_{\theta} \Big(\mathop{log} p(\theta)+\sum_{i=1}^m \mathop{log} p(d^{(i)}|\theta)\Big)
\end{aligned}
$$

#### MAP求解Linear Regression
假设服从分布：
- $y^{(i)}|x^{(i)} \sim N(\theta^Tx, \sigma^2)$
- $\theta \sim N(0, \lambda^2I)$

有：
$$\begin{aligned}& \begin{aligned}p(d^{(i)};\theta) &= p(y^{(i)}|x^{(i)};\theta)\\
&= \frac{1}{\sqrt{2\pi\sigma^2}}\mathop{e}^{-\frac{(y^{(i)}-\theta^Tx)^2}{2\sigma^2}}
\end{aligned}\\

& p(\theta) = \frac{1}{\sqrt{2\pi\lambda^2}}\mathop{e}^{-\frac{\theta^T\theta}{2\lambda^2}}
\end{aligned}
$$

求解：
$$\begin{aligned}l(\theta) &= \mathop{log}p(\theta) + \sum_{i=1}^m p(y^{(i)}|x^{(i)};\theta)\\
&= \frac{1}{\sqrt{2\pi\lambda^2}} -\frac{\theta^T\theta}{2\lambda^2} + m\mathop{log}\frac{1}{\sqrt{2\pi\sigma^2}} - \sum_{i=1}^m \frac{(y^{(i)}-\theta^Tx)^2}{2\sigma^2} \\
\end{aligned}
$$

由上式得到目标：
$$\begin{aligned}& \mathop{max}\limits_{\theta} \frac{1}{\sqrt{2\pi\lambda^2}} -\frac{\theta^T\theta}{2\lambda^2} + m\mathop{log}\frac{1}{\sqrt{2\pi\sigma^2}} - \sum_{i=1}^m \frac{(y^{(i)}-\theta^Tx)^2}{2\sigma^2}\\
& \mathop{max}\limits_{\theta} -\frac{\theta^T\theta}{2\lambda^2}- \sum_{i=1}^m \frac{(y^{(i)}-\theta^Tx)^2}{2\sigma^2}\\
& \mathop{min}\limits_{\theta} \sum_{i=1}^m \frac{(y^{(i)}-\theta^Tx)^2}{2\sigma^2} + \frac{\theta^T\theta}{2\lambda^2}
\end{aligned}
$$

结果为(正则化的Cost Function)：
$$\theta_{MAP} = arg\mathop{min}\limits_{\theta} \sum_{i=1}^m \frac{(y^{(i)}-\theta^Tx)^2}{2\sigma^2} + \frac{\theta^T\theta}{2\lambda^2}$$
> 注意与Regularization章的表达式对比，此处MAP的$\lambda$与其不同，但是整体表达式内涵相同

#### MAP求解Logistic Regression
假设服从分布：
- $\theta \sim N(0, \lambda^2I)$

有：
$$p(\theta) = \frac{1}{\sqrt{2\pi\lambda^2}}\mathop{e}^{-\frac{\theta^T\theta}{2\lambda^2}}
$$

根据MLE及线性回归MAP中的部分表达式，易得到如下结果：
$$\theta_{MAP} = arg\mathop{min}\limits_{\theta} \sum_{i=1}^m \mathop{log}\big(1+exp(-y^{(i)}\theta^Tx)\big) + \frac{\theta^T\theta}{2\lambda^2}
$$


## Naive Bayes and EM algorithm
### Naive Bayes
**目标：** $p(y|x)=\frac{p(x,y)}{p(x)}=\frac{p(y)p(x|y)}{p(x)}$
- 因为分类是为比较$p(y|x)$的大小，因此对于不同的类别$y^{(i)}$具有相同的$p(x)$，可以省略计算，由此转换目标式(详情参见MLE求解)
- **目标式转换为：** $p(x,y)=p(y)p(x|y)$

#### MLE求解朴素贝叶斯
**似然函数：**
$$L = p(y|x)=\frac{p(x,y)}{p(x)} \propto p(x,y)=\prod_{i=1}^mp(x^{(i)},y^{(i)})\\ 
\begin{aligned} l &= \mathop{log} p(x,y) \\
&= \mathop{log} \prod_{i=1}^m p(x^{(i)}, y^{(i)}) \\
&= \mathop{log} \prod_{i=1}^m p(y^{(i)}) p(x^{(i)}|y^{(i)}) \\
&= \sum_{i=1}^m \mathop{log} \Big( p(y^{(i)}) \prod_{j=1}^n p(x_j^{(i)}|y^{(i)})  \Big) \\
&= \sum_{i=1}^m \mathop{log}p(y^{(i)}) + \sum_{i=1}^m\sum_{j=1}^n\mathop{log} p(x_j^{(i)}|y^{(i)})
\end{aligned}
$$

**目标：**
$$\mathop{max} \sum_{i=1}^m \mathop{log}p(y^{(i)}) + \sum_{i=1}^m\sum_{j=1}^n\mathop{log} p(x_j^{(i)}|y^{(i)})$$

结合上式，根据已知，显然数数统计概率$p(x|y), p(y)$即可：
$$\begin{aligned}
& p(y) = \frac{\sum_{i=1}^m 1(y^{(i)}=y)}{m} \\
& p_j(x|y) = \frac{\sum_{i=1}^m 1(x_j^{(i)}=x \land y^{(i)}=y)}{\sum_{i=1}^m 1(y^{(i)}=y)}
\end{aligned}
$$

#### Laplace Smoothing
目的：防止除0问题的发生，人为在分子、分母加上一定的常数项
- 分子：+1
- 分母：+对应的类别数

### EM Algorithm
EM算法主要思想：解决观测数据有缺失情况下的参数估计问题，分为E步和M步
- E步(期望步)
    + $Q(z^{(i)}) = p(z^{(i)}|x^{(i)},\theta)$
- M步(极大步)
    + $\theta := arg\mathop{max}\limits_{\theta} \sum_{i=1}^m \sum_{y=1}^k Q(z^{(i)}) \mathop{log} \frac{p(x^{(i)},z^{(i)})}{Q(z^{(i)})}$

#### Convex Function凸函数
- 定义1：$H = \nabla^2f(x) \succeq 0$
    + 上式表示Hessian矩阵(二阶导数)是半正定矩阵，$f(x)$是凸函数
- 定义2：$f(y) \geq f(x) + \nabla f(x)^T(y-x)$
    + 上式表示$f(y)$值比$x$对应位置切线增长的值要大，$f(x)$是凸函数
    + <img src="fig3.jpg" width="600" height="200">
- 性质：$f((1-\lambda)x+\lambda y) \leq (1-\lambda)f(x) + \lambda f(y)$

#### Jensen's Inequality
对于凸函数，下式成立(若为凹函数，大小符号反向)：
- 形式1：$f(\sum_{i=1}^N \lambda_ix_i) \leq \sum_{i=1}^N\lambda_if(x_i)$
    + 其中,$\lambda_1,lambda_1,... >0$，且$\sum_{i=1}^N \lambda_i=1$
- 形式2：$f(E[X]) \leq E[f(x)]$
> 证明：对i使用归纳法，现证i=k时成立(第二步配比满足和=1):
> $$\begin{aligned}f(\sum_{i=1}^k \lambda_ix_i) &= f(\sum_{i=1}^{k-1} \lambda_ix_i + \lambda_k x_k)\\ 
&= f((1-\lambda_k) \sum_{i=1}^{k-1} \frac{\lambda_i}{1-\lambda_k}x_i + \lambda_k x_k)\\
&\leq (1-\lambda_k)f(\sum_{i=1}^{k-1} \frac{\lambda_i}{1-\lambda_k}x_i) + \lambda_k f(x_k)\\
&\leq (1-\lambda_k) \sum_{i=1}^{k-1} \frac{\lambda_i}{1-\lambda_k} f(x_i) +  \lambda_k f(x_k)\\
&= \sum_{i=1}^{k-1} \lambda_i f(x_i) +  \lambda_k f(x_k)\\
&= \sum_{i=1}^{k} \lambda_i f(x_i)
\end{aligned}
$$

#### EM算法的导出

#### EM算法与混合高斯

#### EM算法与Naive Bayes


## SVM
**SVM**：间隔、对偶、核技巧
**分类**：hard-margin, soft-margin
**最初为解决二分类问题**：找到一个超平面，正确分隔数据集且效果最好(最小距离最大间隔)

### Hard-Margin
使用硬间隔Hard-Margin导出SVM。硬间隔就是严格将样本点分隔，软间隔允许错误。

#### SVM问题导出
##### SVM分类函数
$$f(x) = \mathop{sign}(w^Tx+b)$$
> sign为符号函数，值大于0则为1，否则为-1

##### 基本模型：最大间隔分类器
根据SVM基本思想，写出目标；根据正确分类的标准，写出约束条件：
$$\mathop{max}\limits_{w,b}margin(w,b)\\
\mathop{s.t.} \begin{cases} w^Tx+b>0, &y=1\\
    w^Tx+b<0, &y=-1\end{cases}
$$
观察到上述约束条件表达的是$w^T+b$与$y$符号需要相同，因此改写为：
$$\mathop{max}\limits_{w,b}margin(w,b)\\
\mathop{s.t.} y^{(i)}(w^Tx^{(i)}+b)>0, \forall i=1,2,...
$$

##### margin间隔
- 函数间隔：点分类结果是否与真实结果一致(符号相同的值)
    + $\gamma=y^{(i)}(w^Tx^{(i)}+b)$
- 几何间隔：点到直线的几何距离，是SVM真正需要最大化的间隔
    + 点到直线的距离公式(初高中)：在此处，显然为$\frac{|w^Tx^{(i)}+b|}{\|w\|}$
    + 为去掉绝对值，采用$w^T+b$与$y$符号相同的思想，改写为$\frac{y^{(i)}(w^Tx^{(i)}+b)}{\|w\|}$
        * $\gamma = \frac{1}{\|w\|}y^{(i)}(w^Tx^{(i)}+b)$
        * 显然与函数间隔有$\frac{1}{\|w\|}$的关系

综上所述，将SVM模型转化为(最小距离最大间隔)：
$$\mathop{max}\limits_{w,b} \mathop{min}\limits_{i} \frac{1}{\|w\|}y^{(i)}(w^Tx^{(i)}+b)\\
\mathop{s.t.} y^{(i)}(w^Tx^{(i)}+b)>0, \forall i=1,2,... \\
\implies \\
\mathop{max}\limits_{w,b} \frac{1}{\|w\|} \mathop{min}\limits_{i} y^{(i)}(w^Tx^{(i)}+b)\\
\mathop{s.t.} y^{(i)}(w^Tx^{(i)}+b)>0, \forall i=1,2,... \\
$$

根据上式，令最小距离为1，即$\mathop{min}\limits_{i} y^{(i)}(w^Tx^{(i)}+b) = 1$，进一步转化模型为：
$$\mathop{max}\limits_{w,b} \frac{1}{\|w\|}\\
\mathop{s.t.} y^{(i)}(w^Tx^{(i)}+b) \ge 1, \forall i=1,2,... \\
\implies \\
\mathop{min}\limits_{w,b} \frac{1}{2}{\|w\|}\\
\mathop{s.t.} y^{(i)}(w^Tx^{(i)}+b) \ge 1, \forall i=1,2,... 
$$
> - 令最小距离为1，并修改约束条件为集合间隔大于等于最小距离，此举不改变表达式：
>   + 因按比例缩放，即使w,b按相同比例缩放也不改变
>   + 具体可以参考[链接](https://www.zhihu.com/question/325564129)
> - 在目标前加1/2是为方便求导，同时也有$\|w\| = w^Tw$

#### 凸优化问题(QP问题)及其对偶问题
SVM是一个凸优化问题，稍加转换，下式为符合形式的原问题(小于等于0)：
$$\mathop{min}\limits_{w,b} \frac{1}{2}{w^Tw}\\
\mathop{s.t.} 1-y^{(i)}(w^Tx^{(i)}+b) \le 0, \forall i=1,2,... 
$$

使用拉格朗日乘子法和KKT条件(在有不等式约束时使用KKT条件)，于是原问题可描述为：
$$\mathop{min}\limits_{w,b} \mathop{max}\limits_{\lambda} L(w,b,\lambda)\\
\mathop{s.t.} 1-y^{(i)}(w^Tx^{(i)}+b) \le 0, \forall i=1,2,... 
$$

上述问题的(强)对偶问题，即将min和max交换并使用KKT条件，描述如下：
$$\mathop{max}\limits_{\lambda} \mathop{min}\limits_{w,b} L(w,b,\lambda)\\
\begin{aligned}\mathop{s.t.} &(KKT condition:)\\
&\begin{cases}
\frac{\partial L}{\partial w} = 0 \\
\frac{\partial L}{\partial b} = 0 \\
\lambda_i\big(1-y^{(i)}(w^Tx^{(i)}+b)\big) = 0, &\forall i=1,2,...\\
\lambda_i \ge 0, &\forall i=1,2,... \\
1-y^{(i)}(w^Tx^{(i)}+b) \le 0, &\forall i=1,2,...
\end{cases}
\end{aligned}
$$

> 因L是凸函数，希望消除上述对偶问题中的min，即满足$\frac{\partial L}{\partial w} = 0, \frac{\partial L}{\partial b} = 0$，代入即可(KKT条件已包含)

#### KKT条件求解w,b
原问题：
$$\mathop{min}\limits_{w,b} \frac{1}{2}w^Tw\\
\mathop{s.t.} 1-y^{(i)}(w^Tx^{(i)}+b) \le 0, \forall i=1,2,... 
$$

$L(w,b,\lambda)$：
$$\begin{aligned}L(w,b,\lambda) &= \frac{1}{2}w^Tw + \sum_{i}\lambda_i \big(1-y^{(i)}(w^Tx^{(i)}+b) \big) \\
&= \frac{1}{2}w^Tw + \sum_{i}\lambda_i \big(-y^{(i)}(w^Tx^{(i)}+b) \big) + \sum_{i}\lambda_i
\end{aligned}
$$

对偶问题：
$$\mathop{max}\limits_{i} \mathop{min}\limits_{w,b} \frac{1}{2}w^Tw + \sum_{i}\lambda_i \big(-y^{(i)}(w^Tx^{(i)}+b) \big) + \sum_{i}\lambda_i \\
\mathop{s.t.} (KKT Condition)
$$

$\frac{\partial L}{\partial w}$：
$$\begin{aligned}& \frac{\partial L}{\partial w} = w - \sum_{i}\lambda_iy^{(i)}x^{(i)} = 0 \\
\implies & w^{*} = \sum_{i}\lambda_iy^{(i)}x^{(i)}
\end{aligned}
$$

$\frac{\partial L}{\partial b}$：
$$\begin{aligned}\frac{\partial L}{\partial b} &= -\sum_{i}\lambda_iy^{(i)} = 0 \\
\implies & \sum_{i}\lambda_iy^{(i)} = 0
\end{aligned}
$$

求解b：
$$\begin{aligned}&\because \begin{cases}\lambda_i\big(1-y^{(i)}(w^Tx^{(i)}+b)\big) = 0, &\forall i=1,2,...\\
\lambda_i \ge 0, &\forall i=1,2,... \\
1-y^{(i)}(w^Tx^{(i)}+b) \le 0, &\forall i=1,2,...\end{cases} \\
&\therefore \text{仅当} \lambda>0 \text{时，才有} 1-y^{(i)}(w^Tx^{(i)}+b)=0 \\
&\therefore y^{(i)}(w^Tx^{(i)}+b)=1 \\
&\because y^{(i)^2}(w^Tx^{(i)}+b)=y^{(i)}, \text{且}y^{(i)^2}=1\\
&\therefore b^{*} = y^{(i)} - w^Tx^{(i)}
\end{aligned}
$$

> 另附最终最优化问题描述：
> $$\mathop{max}\limits_{i} -\frac{1}{2}\sum_{i}\sum_{j}\lambda_i \lambda_j y^{(i)} y^{(j)} x^{(i)^T}x^{(j)} + \sum_{i}\lambda_i\\
\begin{aligned}\mathop{s.t.} &(KKT condition:)\\
&\begin{cases}
\frac{\partial L}{\partial w} = 0 \\
\frac{\partial L}{\partial b} = 0 \\
\lambda_i\big(1-y^{(i)}(w^Tx^{(i)}+b)\big) = 0, &\forall i=1,2,...\\
\lambda_i \ge 0, &\forall i=1,2,... \\
1-y^{(i)}(w^Tx^{(i)}+b) \le 0, &\forall i=1,2,...
\end{cases}
\end{aligned}
$$

#### 关于KKT条件
拉格朗日乘子法解决条件中仅包含等式的优化问题，而扩展出的KKT条件能求解条件中包含不等式的情况，以下是最优化问题使用KKT条件的表达：

原问题：
$$\mathop{min} f(x)\\
\mathop{s.t.} \begin{cases}h_j(x) = 0, &\forall j=1,2,... \\
g_k(x) \le 0, &\forall j=1,2,... \\
\end{cases}
$$

使用拉格朗日乘子，上述问题等价于(不等式，用KKT条件)：
$$\mathop{min} \mathop{max}\limits_{\lambda} f(x) + \sum_j \lambda_j h_j(x) + \sum_k \mu_k g_k(x)\\
\mathop{s.t.} \begin{cases} \frac{\partial L}{\partial X}=0 \\
\lambda_j \neq 0, &\forall j=1,2,...\\
\mu_k \ge 0, &\forall k=1,2,... \\
\mu_k g_k(x) = 0, &\forall k=1,2,... \\
h_j(x) = 0, &\forall j=1,2,... \\
g_k(x) \le 0, &\forall k=1,2,... \\
\end{cases}
$$

将上式转换为对偶问题(强对偶关系，问题等价)：
$$\mathop{max}\limits_{\lambda} \mathop{min} f(x) + \sum_j \lambda_j h_j(x) + \sum_k \mu_k g_k(x)\\
\mathop{s.t.} \begin{cases} \frac{\partial L}{\partial X}=0 \\
\lambda_j \neq 0, &\forall j=1,2,...\\
\mu_k \ge 0, &\forall k=1,2,... \\
\mu_k g_k(x) = 0, &\forall k=1,2,... \\
h_j(x) = 0, &\forall j=1,2,... \\
g_k(x) \le 0, &\forall k=1,2,... \\
\end{cases}
$$

> 对偶问题：实际上再次就是$min$和$max$的位置互换，形式上是一个弱对偶即取$\ge$号，但此处经证明是一个强对偶关系取$=$，因此问题等价，即：
> $$\mathop{min} \mathop{max}\limits_{\lambda} \ge \mathop{max}\limits_{\lambda} \mathop{min}\\
$$
> - 只要是不等式的约束使用拉格朗日乘子法，一定满足KKT条件，与是否有等价对偶问题无关。
> - 等价的对偶问题只与是否是强对偶关系有关，对偶问题是否等价，可进一步查找资料

### Soft-Margin
允许一点错误，类似于Regularization的思想，增加一个惩罚项：
$$\mathop{min}\limits_{w,b} \frac{1}{2}{w^Tw} + C\sum_i \xi_i \\
\mathop{s.t.} \begin{cases} y^{(i)}(w^Tx^{(i)}+b) \ge 1-\xi_i, &\forall i=1,2,... \\
\xi_i \ge0, &\forall i=1,2,...
\end{cases}
$$

对于soft-margin的推导与hard-margin相似，按其步骤推导即可.

### Kernel
对于线性不可分的数据，可通过核技巧解决：
- 其基本思想是一个feature mapping，即将原来的$x$映射到$\phi(x)$
- 使用核函数$K<x,z> = \phi(x)\phi(z)$更容易计算，替代对偶表达式中的优化目标
    + $K_{ij}=K<x_i,x_j> = \phi(x_i)^T\phi(x_j)$

使用核函数的对偶式：
$$\mathop{max}\limits_{i} -\frac{1}{2}\sum_{i}\sum_{j}\lambda_i \lambda_j y^{(i)} y^{(j)} K_{ij} + \sum_{i}\lambda_i\\
\mathop{s.t.} (KKT condition:)
$$

对应的，解为：
$$\begin{cases}w^* = \sum_{i;\lambda_i>0} \lambda_iy^{(i)}\phi(x^{(i)})  \\
\begin{aligned}b^* &= y^{(i)} - w^Tx^{(i)} \\
&= y^{(i)} - \sum_{j;\lambda_j>0} \lambda_jy^{(j)}\phi(x^{(j)})^T x^{(i)}\\
&= y^{(i)} - \sum_{j;\lambda_j>0} \lambda_jy^{(j)}K_{ji}
\end{aligned}
\end{cases}
$$

对应的分类函数：
$$\begin{aligned}y &= \mathop{sign}(w^Tx+b)\\
&= \mathop{sign}(\sum_{j;\lambda_j>0}  \lambda_j y^{(j)}\phi(x^{(j)})^T x+b)\\
&= \mathop{sign}(\sum_{j;\lambda_j>0} \lambda_j y^{(j)}K(x^{(j)},x)+b)
\end{aligned}
$$

显然因设定$K$为已知，而非$\phi(x)$函数已知，$w^*$无法使用上式求解。但是我们本质上分类时并不需要知道$w^*$的值，只需要知道它的类别。因此只需求出$b^*$，再直接使用上述分类函数的表达式计算所属类别。



## Clustering
通常是非监督学习问题
聚类的种类
- Flat or Partitional clustering: Partitions are independent of each other
    + 各分区独立
- Hierarchical clustering: Partitions can be visualized using a tree structure (a dendrogram)
    + 各分区呈现出树形结构

### K-Means
K-Means分类算法与所取的类别数k有关，还与初始化的中心有关。若希望选取最优的类别数k，可以多试几组不同的k，最终选择"elbow point"作为类别数
#### 算法伪代码
```
(初始化)随机指派k个数据点分别作为k类的中心
Repeat:
    分别计算各个点到k个类中心的距离，并选择最近的一个中心指派类别
    分别对各类数据计算平均值，作为新的类中心
until (收敛或达到停止条件)
```

#### 问题的基本结构
需要分出的类，每个数据都离它对应类别的中心点距离是最小的，数学语言表达如下式：
$$arg\mathop{min}\limits_{\{C_i\}} \sum_{i=1}^K \sum_{x\in C_i} \|x-\mu_i\|^2 $$
显然该问题是一个NP问题，因此使用上述伪代码所描述的迭代的启发式算法可以寻找到一个合适的解.

#### Loss Function
使用向量$z_i$代表第i个数据所属类别的one-hot编码(若属于第k维的类别，为1；否则为0)，由此导出损失函数L如下：
$$L(\mu,X,Z) = \sum_{i=1}^N \sum_{k=1}^K z_{i,k}\| x_i- \mu_k\|^2 \\
\implies L(\mu,X,Z) = \|X-Z\mu\|^2 \\
\text{where X is N} \times\text{D, Z is N} \times \text{K and }\times \text{is K }\times \text{D}
$$
利用Loss Function验证K-Means算法收敛性

#### Kernel K-Means
即使用某种核函数替代欧式距离的计算，对该核函数可作如下定义：
$$d(x_i, x_j) = \| \phi(x_i) - \phi(x_j)\|^2 \\
\begin{aligned}d(x_i, x_j)^2 &= \| \phi(x_i) - \phi(x_j)\|^2 \\
&= \phi(x_i)^2 - 2\phi(x_i)\phi(x_j) + \phi(x_j)^2 \\
&=k(i,i) - 2K(i,j) + K(j,j)
\end{aligned}
$$

其中$k(x,y)$即定义的核函数，因此与SVM核技巧一样，不需要明确映射$\phi$，可直接隐式确定值，直接计算距离$d$

### Hierachical Clustering
#### 算法伪代码
```
初始化为n个类别，每个数据单独一个类
Repeat:
    计算各个类别两两之间的距离(欧式)
    合并距离最小的2个类
until 只剩下一个类
```


## PCA
一个中心：一组可能线性相关的数据通过正交变换到线性无关的数据
- $z = U^Tx$ and $x = Uz$

两个基本点(2种目标描述，实则等价)：
- 最大投影方差：投影后变量的方差要最大(方差大，更无关)
- 最小重构距离：逆变换后与原值差异最小

### PCA基本步骤
1. 中心化：数据与均值作差，使均值$\mu=0$，方便计算
2. 计算协方差矩阵$S$
3. 计算协方差矩阵的特征值和特征向量
4. 排序特征值(由大到小)，即其对应的特征向量
5. 取前$k$个构成投影矩阵$U$
6. 使用变换$z=U^Tx$

### PCA数学基础
#### 样本均值
$$\mu = \frac{1}{N}\sum_{i=1}^N x^{(i)}$$

#### 协方差矩阵
基本定义：$S = \frac{1}{N}\sum_{i=1}^N (x^{(i)}-\mu)(x^{(i)}-\mu)^T$
表现形式(以$2\times2$为例)：
$$S=\begin{bmatrix}Cov(x_1,x_1) & Cov(x_1,x_2)\\
Cov(x_2,x_1) & Cov(x_2,x_2)
\end{bmatrix}
$$

#### 求解特征方程：求出特征值和特征向量
基本问题：$Ax=\lambda x$
问题转换：
- $(A-\lambda E)x=0$，在$x$向量非0时，有行列式：$|A-\lambda E|=0$
- 找到特征值之后，特征向量为$x=\frac{1}{\lambda}Ax$

### PCA问题的导出
- 最大投影方差：导出问题及其算法正确性
- 最小重构距离：导出问题本质与最大投影方差相同

#### 最大投影方差
目标：找到一个投影$U$使变换后方差最大
- 我们仅以一个维度$u_1$为例，其余维度类似
- 设原数据集的均值为$\mu$，方差为$S$

变换后均值：
$$\begin{aligned}\hat z &= \frac{1}{N}\sum_{i=1}^N u_1^Tx^{(i)}\\
&= u_1^T\mu
\end{aligned}
$$

协方差：
$$\begin{aligned}& \frac{1}{N}\sum_{i=1}^N (z^{(i)}-u_1^T\mu)(z^{(i)}-u_1^T\mu)^T\\
&= \frac{1}{N}\sum_{i=1}^N (u_1^Tx^{(i)}-u_1^T\mu)(u_1^Tx^{(i)}-u_1^T\mu)^T\\
&= \frac{1}{N}\sum_{i=1}^N u_1^T(x^{(i)}-\mu)(x^{(i)}-\mu)^Tu_1\\
&= u_1^T\big[\frac{1}{N}\sum_{i=1}^N (x^{(i)}-\mu)(x^{(i)}-\mu)^T\big]u_1\\
&= u_1^TSu_1
\end{aligned}
$$

设$u_1$为单位向量，即$u_1^Tu_1=1$，导出基本问题模型为：
$$\mathop{max}\limits_{u_1} u_1^TSu_1 \\
\mathop{s.t.} u_1^Tu_1=1
$$

使用拉格朗日乘子法，推导出本质是求原数据集协方差矩阵的特征值和特征向量($u_1$)，以及找出特征值最大的对应特征向量：
$$\begin{aligned} && L = u_1^TSu_1 + \lambda(1-u_1^Tu_1)\\
&\implies & \frac{\partial L}{\partial u_1} = 2Su_1 - 2\lambda u_1 = 0 \\
&\implies & Su_1 = \lambda u_1\\
&\because &u_1^TSu_1 = \lambda u_1^T u_1 = \lambda\\
&\therefore &\mathop{max} u_1^TSu_1 \iff \mathop{max} \lambda
\end{aligned}
$$

#### 最小重构距离
PCA的逆变换：$\hat x = Uz$
距离：
$$\begin{aligned}L &= \sum_{i=1}^N \|x^{(i)}-\hat x^{(i)}\|^2 \\
&= \sum_{i=1}^N \|x^{(i)}-u_1z^{(i)}\|^2 \\
&= \sum_{i=1}^N \|x^{(i)}-u_1u_1^Tx^{(i)}\|^2 \\
&= \sum_{i=1}^N (-u_1u_1^Tx^{(i)})^T x^{(i)}-u_1u_1^Tx^{(i)} \\
&= \sum_{i=1}^N (x^{(i)^T}x^{(i)} - x^{(i)^T}u_1u_1^Tx^{(i)}) \\
&= \sum_{i=1}^N (x^{(i)^T}x^{(i)} - u_1^Tx^{(i)}x^{(i)^T}u_1) \\
&= -u_1^TSu_1 + \sum_{i=1}^N x^{(i)^T}x^{(i)} \\
\end{aligned}
$$

因此优化问题为下，明显与最大投影方差等价：
$$\mathop{min} -u_1^TSu_1 + \sum_{i=1}^N x^{(i)^T}x^{(i)} \iff \mathop{max} u_1^TSu_1
$$


## Learning Theory
### Bias, Variance and Model Complexity

### Bias-Variance Decomposition

### The Gap Between Training Error and Generalization Error

### Selecting Right Model and Features