---
layout: post
comments: false
title: "Machine Learning Foundations"
date: 2015-01-01 12:00:00
tags: edx
---


> The course notes of machine learning foundations enrolled in Coursera.

<!--more-->

{: class="table-of-content"}
* TOC
{:toc}


## Week-01

本周介绍了机器学习的基本形式，从已知的 training examples $$\mathcal{D}:(x_1, y_1), \ldots, (x_N, y_N)$$ 学习出一个函数 $$g$$ 来近似未知的目标函数 $$f$$。

最简单的学习算法 Perceptron 

$$
\begin{align}
\text{positive } +1 \quad & \sum_{i=1}^d w_i x_i > \text{threshold} \\
\text{negative } -1 \quad & \sum_{i=1}^d w_i x_i < \text{threshold}
\end{align}
$$

$$
\begin{align}
h(x) &= \text{sign}\left(\left(\sum_{i=1}^d w_i x_i\right) - \text{threshold}\right) \\
&= \text{sign}\left(\sum_{i=0}^d w_i x_i\right) \\
&= \text{sign} (w^T x)
\end{align}
$$

Perceptron Learning Algorithm 的直观解释就是用预测出错的实例 $$(x_{n(t)}, y_{n(t)})$$ 更新 $$w$$, 

$$w_{t+1} \leftarrow w_{t} + y_{n(t)}x_{n(t)}$$

对于 $$y_n = +1$$ 的实例来说，如果 Perceptron 预测出错，那么说明 $$w^T x_n$$ 为负数，即 $$w$$ 与 $$x$$ 夹角过大，那么应该将 
$$w_{t+1} \leftarrow w_{t} + x_{n(t)}$$
对于 $$y_n = -1$$ 的实例来说，如果 Perceptron 预测出错，那么说明 $$w^T x_n$$ 为正数，即 $$w$$ 与 $$x$$ 夹角过小，那么应该将 
$$w_{t+1} \leftarrow w_{t} - x_{n(t)}$$



对于线性可分的训练数据，PLA 经过若干步迭代后一定会找到一个线性可分的超平面 $$w_f$$。

* claim 1: $$w_f^T w_t$$ is increasing by updating with any $$(x_{n(t)}, y_{n(t)})$$
    
    $$
    \begin{align}
    w_f^T w_{t+1} &= w_f^T (w_t + y_{n(t)} x_{n(t)}) \\
    & \ge w_f^T w_t + \min _n  y_n w_f^T x_n \\
    & >  w_f^T w_t, \\
    \frac{w_f^T w_{T}}{\|w_f\|} &\ge \rho T, \quad \rho = \min_n \frac{y_n w_f^T x_n}{\|w_f\|}.
    \end{align}
    $$
  

* claim 2: $$\|w_T\|^2 \leq T \cdot \text{constant}$$ 

    $$
    \begin{align}
    \|w_{t+1} \|^2 &= \| w_t + y_{n(t)} x_{n(t)} \|^2 \\
    &= \|w_t\|^2 + 2 y_{n(t)} w_t^T x_{n(t)} + \| y_{n(t)} x_{n(t)}\|^2 \\
    &\leq \|w_t\|^2 + \max_{n} \| x_n\|^2,\\
    \|w_{T} \|^2 &\leq T  R^2, \quad R^2 = \max_n \| x_n \|^2.
    \end{align}
    $$



综上述两点，我们得出以下结论

$$
\begin{align}
\frac{w_f^T  w_{T}}{\|w_f\| \| w_T \|} &\ge \frac{\rho}{R}  \sqrt{T}.
\end{align}
$$

由于上式左边单位向量内积最大值为 1， 因此我们有 $$T \leq \frac{R^2}{\rho^2}$$，即经过 $$T$$ 次迭代后算法收敛。

Perceptron 优化的一般形式

$$w_g = \text{argmin} \sum_{n=1}^N I\{y_n \neq \text{sign}(w^T x_n) \}.$$

PLA 的迭代更新也可以通过求解如下代价函数的梯度得到

$$\text{cost}(w) = \sum_{n=1}^N \max(0, -y_n w^T x_n).$$

## Week-02

这周的内容主要包括两部分，从 deterministic 角度看学习问题可能是不可行的，但是从 probabilistic 角度看学习的问题又是可行的，这主要由 Hoeffding Inequality 保证。几个概念:

$$
f: \mathcal{X} \rightarrow \mathcal{Y}
$$

$$f$$ 为待学习的未知函数，input space is $$ \mathcal{X}$$, output space $$ \mathcal{Y} \in \{-1, +1\}$$,因此对于有 $$N$$ 个样本的输入可能的 $$f$$ 会有 $$2^N$$ 个。

$$
(x_1, y_1), (x_2, y_2), \ldots, (x_N, y_N)
$$

为训练样本集合 (training examples), 也被称为 $$\mathcal{D}$$，其大小为 $$N$$。

$$
\mathcal{H} = \{h_1, h_2, \ldots, h_M\}
$$

为 Hypothesis Set，我们最终就是要从 $$\mathcal{H}$$ 中选出一个函数 $$g$$ 来近似未知函数 $$f$$。


$$
E_{\text{in}} = \frac{1}{N} \sum_{n=1}^N I \{h(x_n) \neq f(x_n) \}
$$

被称为 in-sample error, based on $$\mathcal{D}$$。

$$
E_{\text{out}} = \mathbb{P} [h(x) \neq f(x) ]
$$

被称为 out-sample error, based on the distribution $$\mathbb{P}$$ over $$\mathcal{X}$$。

Hoeffding Inequality 是关于，从随机抽样的样本与数据的整体分布偏差的上界估计。设数据的整体分布 (bin) 为 $$\mu$$，从数据中随机独立抽样 $$N$$ 个样本计算出分布为 $$\nu$$，那么

$$
\mathbb{P}[\vert \nu - \mu \vert > \epsilon] \leq 2 e^{-2 \epsilon^2 N} \quad \text{for any}~ \epsilon > 0
$$

我们可以看出随机抽样的样本数量 $$N$$ 越大，则估计出的分布与真实分布的偏差在概率的角度下越小。


本周的作业题中有一道解释了一个现象，在 PLA 的更新规则中

$$
w(t+1) \leftarrow w(t) + \eta \cdot y(t) x(t)
$$

如果 $$w$$ 从零向量开始迭代更新，那么对于任意 $$\eta > 0$$， 最后迭代停止的步数都相同。这是因为，$$\eta$$ 仅仅对最后的 $$w_T$$ 进行了常数倍 $$\eta$$ 的缩放，而不会改变方向。


## Week-03

对于拥有 $$M$$ 个 Hypothesis 的误差上界估计

$$
\mathbb{P}[\vert E_\text{in}(g) - E_\text{out}(g) \vert > \epsilon] \leq 2 M \exp(-2 \epsilon^2 N) \quad \text{for any}~ \epsilon > 0
$$

$$M$$ is $$\vert \mathcal{H} \vert$$，因为不等式的右端是 $$M$$ 个 Hypothesis 上界的简单叠加，因此过于 loose，我们希望寻求更紧致的上界。

由于给定 $$\mathcal{H}$$ 之后，我们可以 $$2^N$$ 个可能中去除一些不可能的 $$h$$，这样可以定义出这种去除不可能 $$h$$ 后 Hypothesis 的大小，Growth Function

$$
m_\mathcal{H}(N) = \max_{x_1, x_2, \ldots, \in \mathcal{X}} \vert \mathcal{H} (x_1, x_2, \ldots, x_N) \vert
$$

很显然 $$m_\mathcal{H}(N) \leq 2^N$$，几个简单的例子，

* Positive Rays

  $$\mathcal{H}$$ contains h, where each $$h(x) = \text{sign}(x-a)$$
  此时所有在 $$a$$ 右侧的点为 +1，左侧的为 -1，对于 $$N$$ 个点来说共有 $$N+1$$ 个可能的 $$h$$，即 $$m_\mathcal{H}(N) = N + 1$$
  
* Positive Intervals
  
  $$\mathcal{H}$$ contains h, where each $$h(x) = +1 ~\text{iff}~ x \in [\ell, r), -1$$ otherwise
  对于区间内的点为 +1，区间外的点为 -1，此时 $$m_\mathcal{H}(N) = {N+1 \choose 2} + 1$$

* Convex Sets

  $$\mathcal{H}$$ contains h, where each $$h(x) = +1 ~\text{iff}~ x \text{ in a convex set}, -1$$ otherwise 将 $$N$$ 个点排成一个圆，并将所有的正例顺次连接，构成一个凸多边形，这个凸多边形的边界即构成了满足条件的 $$h$$，由于每个点有两个选择，因此 $$m_\mathcal{H}(N) = 2^N$$
  
  
**Break Point** $$m_\mathcal{H}(k) < 2^k$$ and if $$k$$ is break point $$k+1, k+2, \ldots$$ also break points.

* Positive rays:  break point at 2
* Positive intervals: break point at 3
* Convex sets: no break point
* 2D perceptrons: break point at 4   
  

**Bounding Function** $$B(N, k)$$: maximum possible $$m_\mathcal{H}(N)$$ when break point = $$k$$.

$$m_\mathcal{H}(N) < B(N, k)$$

$$B(N,k) \leq \sum_{i=0}^{k-1}{N \choose i}$$

由此可以推得，$$m_\mathcal{H}(N)$$ is $$poly(N)$$ if break point exists

最后可以得出以下结论，

$$\exists h \in \mathcal{H}\text{ s.t. }$$, 
$$\mathbb{P}\left[\vert E_\text{in}(h) - E_\text{out}(h)\vert > \epsilon   \right] \leq 4 m_\mathcal{H}(2N) \cdot \exp(-\frac{1}{8}\epsilon^2N)$$

这个上界相对于 $$2 M \exp(-2 \epsilon^2 N) $$ 要紧很多。

## Week-04

Vapnik-Chervonenkis (VC) Bound
For any $$g = \mathcal{A(D)} \in \mathcal{H}$$ and statistical large $$\mathcal{D}$$ 

$$
\begin{aligned} 
&\mathbb{P} \left[\vert E_\text{in}(h) - E_\text{out}(h)\vert > \epsilon   \right] \\
&\leq 4 m_\mathcal{H}(2N) \cdot \exp(-\frac{1}{8}\epsilon^2N)\\
&\leq 4 (2N)^{k-1} \cdot \exp(-\frac{1}{8}\epsilon^2N) \quad \text{if $k$ exists}
\end{aligned}
$$


VC dimension of $$\mathcal{H}$$ denoted as $$d_{VC}(\mathcal{H})$$ is largest $$N$$ for which $$m_\mathcal{H}(N) = 2^N$$

  * the most inputs $$\mathcal{H}$$ can shatter
  * $$d_{VC} = $$ minimum $$k$$ - 1
  * $$ N \leq d_{VC} \Longrightarrow \mathcal{H}$$ can shatter some $$N$$ input
  * $$k > d_{VC} \Longrightarrow$$ $$k$$ is a break point for $$\mathcal{H}$$

Examples:

* Positive rays $$d_{VC} = 1$$
* Positive intervals $$d_{VC} = 2$$
* Convex sets $$d_{VC} = \infty$$
* 2D perceptrons $$d_{VC} = 3$$

**VC Dimension of Perceptions**

$$d$$-D perceptrons $$d_{VC} = d + 1$$ 

* $$d_{VC} \geq d + 1$$  There are some $$d+1$$ inputs we can shatter
* $$d_{VC} \leq d + 1$$  We cannot shatter any set of $$d+2$$ inputs

从直观上讲，$$d_{VC}$$ 可以看作模型参数的自由度，即 $$d_{VC} \approx $$ # free parameters

令 $$4 (2N)^{d_{VC}} \cdot \exp(-\frac{1}{8}\epsilon^2N) = \delta$$ 可得 

$$\epsilon = \sqrt{\frac{8}{N} \ln \left(\frac{4(2N)^{d_{VC}}}{\delta}\right)}$$

我们可以 a high probability $$1 - \delta$$,

$$E_\text{out}(g) \leq E_\text{in}(g) + \sqrt{\frac{8}{N} \ln \left(\frac{4(2N)^{d_{VC}}}{\delta}\right)}$$

practice $$N \approx 10 \cdot d_{VC}$$

## Week-05

Pseudo-inverse $$X^{\dagger} = (X^TX)^{-1}X^T$$, each row of $$X$$ is a training case

$$w = X^{\dagger} y$$
$$\hat{y} = Xw = X X^{\dagger}y$$, thus the matrix $$XX^{\dagger}$$ is named hat matrix $$H$$ (project matrix, it projects $$y$$ to $$\hat{y}$$).

$$H$$ has the following properties:

 * $$H$$ is symmetric; 
 * $$H^2 = H$$, null;
 * $$(I - H)^2 = I-H$$, null;

Linear regression can be used for binary classification but $$\text{err}_\text{sqr}$$ is a loose upper bound to approximate $$\text{err}_{0/1}$$.

Typical upper bound functions of $$\text{err}_{0/1}$$:

 * $$\exp(-y w^T x)$$, null;
 * $$\max(0, 1- y w^T x)$$, null;
 * $$\log_2(1 + \exp(- y w^T x)$$, null;

We often trade off between bound tightness and efficiency.

Risk score: $$s = \sum_{i=0}^d w_i x_i $$
Logisitic hypothesis: $$h(x) = \theta(w^T x)$$

$$\theta(s) =  \frac{e^s}{1 + e^s} = \frac{1}{1 + e^{-s}}$$

logistic regression:

  $$h(x) = \frac{1}{1 + \exp(-w^Tx)}$$

假设我们有数据集 $$\mathcal{D} = \{ (x_1, y_1), (x_2, y_2), \ldots \}$$ target function $$f(x) = \mathbb{P}(+1 \vert x)$$

$$
\mathbb{P}(y \vert x) = \begin{cases}f(x)\quad &y = +1 \\
1 - f(x)  \quad &y = -1 \end{cases}
$$


使用 $$h(x) = \theta(w^T x)$$ 来近似 $$f(x)$$, 似然函数 

$$
\begin{align} \text{likelihood}(h) &\propto h(x_1) \times (1 - h(x_2)) \times \ldots \\
&= h(x_1) \times h(-x_2) \times \ldots\\
&= \prod_{n=1}^N h(y_n x_n)\\
&= \prod_{n=1}^N \theta(y_n w^T x_n) \end{align}
$$


上式假设 $$y_1 = +1, y_2 = -1$$ 并利用了 $$1 - h(x) = h(-x)$$

Cross-Entropy Error 

$$-\frac{1}{N}\sum_{n=1}^N \log \theta(y_n w^T x_n) = \frac{1}{N}\sum_{n=1}^N\log(1 + \exp(-y_n w^T x_n))$$

$$\nabla E_\text{in}(w) = \frac{1}{N}\sum_{n=1}^{N} \theta(-y_n w^T x_n)(-y_n x_n) $$

$$E_\text{in}(w_t + \eta v) \approx E_\text{in}(w_t) + \eta\  v^T \nabla E_\text{in}(w_t) $$

$$w_{t+1} \leftarrow w_t +\eta \frac{1}{N}\sum_{n=1}^{N} \theta(-y_n w^T x_n)(y_n x_n) $$

$$\eta$$ is the fixed learning rate.



## Week-06

Linear scoring function $$s = w^T x$$
Linear classification

$$
\begin{align}
h(x) &= \text{sign}(s) \\
\text{error}(h, x, y) &= I\{h(x) \neq y\} \\
\text{error}_{0/1}(y, s) &= I\{\text{sign}(ys) \neq 1\}
\end{align}
$$


Linear Regression

$$
\begin{align}
h(x) &= s \\
\text{error}(h,y,x) &= (h(x)-y)^2 \\
\text{error}(y, s) &=  (s - y)^2
\end{align}
$$


Logistic Regression

$$
\begin{align}
h(x) &= \theta(s) \\
\text{error}(h,y,x) &= -\log h(yx) \\
\text{error}(y, s) &=  \log (1 + \exp(-ys))
\end{align}
$$


Stochastic Gradient Descent (SGD)

$$w_{t+1} \leftarrow w_t + \eta\ \theta(-y_n w_t^T x_n)(y_n x_n) $$

回忆在 PLA 更新中

$$w_{t+1} \leftarrow w_t + 1 \cdot I\{y_n \neq \text{sign}(w_t^T x_n)\} (y_n x_n)$$

因此对 SGD 中 $$\theta(-y_n w_t^T x_n)$$ 项可以有如下直观解释，当 $$y_n w_t^T x_n$$ 为一个 large positive number 时，$$\theta(-y_n w_t^T x_n)$$ 趋近于 0，也就是说当 $$(x_n, y_n)$$ 被正确分类时，$$w_{t+1}$$ 在更新时几乎不受其影响。否则，若 $$(x_n, y_n)$$ 被错误分类，$$y_n w_t^T x_n$$ 为一个 negative number 时，$$\theta(-y_n w_t^T x_n)$$ 趋近于 1, $$w_{t+1}$$ 受其影响较大。

Nonlinear Transform $$\Phi(x): \mathcal{X} \rightarrow \mathcal{Z}$$. In $$\mathcal{Z}$$-space we have data $$\{(z_n = \Phi(x_n), y_n \}$$.

当使用 Q-polynomial 进行数据变换时，数据在 $$\mathcal{Z}$$-space 中维度的上界函数为 $$O(Q^d)$$，此时 $$d_{VC} \approx c \cdot Q^d + 1$$