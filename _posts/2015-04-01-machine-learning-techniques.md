---
layout: post
comments: false
title: "Machine Learning Techniques"
date: 2015-04-01 12:00:00
tags: edx
---


> The course notes of machine learning techniques enrolled in Coursera.

<!--more-->

{: class="table-of-content"}
* TOC
{:toc}

## Week-1 

### Linear Support Vector Machine 

Recall **PLA** $$h(x) = \text{sign}(w^T x)$$ and $$E_\text{out}(w) \leq E_\text{in}(w) + \Omega(\mathcal{H})$$.

我们希望在所有的 seperating hyperplane 中选择最 fat 的，即距离所有点最远的，因为这样的 hyperplane 有更好的 robustness，tolerate more noise. 将这个 idea 进行 formulate 可以得到如下的表示

$$
\begin{align}
\max_{w}\quad  &\text{margin}(w) \\
\text{subject to}\quad &\text{every } y_n (w^T x_n + b) > 0 \\
&\text{margin}(w) = \min_{n} \text{distance}(x_n, w)
\end{align}
$$


其中 $$\text{distance}(x_n, w) = \frac{1}{\|w\|_2}|w^T x_n + b|$$ 为点 $$x_n$$ 到 seperating hyperplane $$y = w^T x + b$$ 的距离。设 $$x'$$ 为 $$y = w^T x + b$$ 上任意一点，那么 $$x_n - x'$$ 在法向量 $$w$$ 上的投影即为 $$x_n$$ 到 seperating hyperplane 的距离，即
$$\text{distance}(x_n, w) = \frac{w^T}{\|w\|_2} (x_n - x')$$
as $$x'$$ locates in the seperating hyperplane, thus $$w^T x' + b = 0$$ and substitute $$b = -w^T x'$$ in the above equation.

注意到我们可以 scale up $$w, b$$ 使得 $$\min \ w^T x_n + b$$ 取任意值，同时不影响最终的最优解，因此令 $$\min_n w^T x_n + b = 1$$ 我们可以得到

$$
\begin{align}
\max_{w, b}\quad  &\frac{1}{\|w\|_2} \\
\text{subject to}\quad & y_n(w^T x_n + b) > 0,\quad \text{for all } n \\
& \min_n |w^T x_n + b| = 1.
\end{align}
$$


进一步简化得到，

$$
\begin{align}
\min_{w, b}\quad  &\frac{1}{2}\|w\|_2^2 \\
\text{subject to}\quad & y_n(w^T x_n + b) \geq 1,\quad \text{for all } n.
\end{align}
$$


### Dual Support Vector Machine

Lagrange Function 

$$\mathcal{L}(b, w, \alpha) = \frac{1}{2} w^T w + \sum_{n=1}^N \alpha_n(1 - y_n(w^T z_n + b))$$

Primal Problem and Dual Problem

$$\min_{b, w}(\max_{\alpha_n} \mathcal{L}(b, w, \alpha)) \geq \min_{b,w} \mathcal{L}(b, w, \alpha)$$

because $$\max \geq$$ any

$$\min_{b, w}(\max_{\alpha_n} \mathcal{L}(b, w, \alpha)) \geq \max_{\alpha_n} \min_{b,w} \mathcal{L}(b, w, \alpha)$$

The Lagrange Dual Form of SVM

$$\max_{\alpha_n \geq 0}\left( \min_{b, w}\frac{1}{2}w^Tw + \sum_{n=1}^N \alpha_n (1 - y_n(w^T z_n + b))\right)$$

根据 KKT condition 倒数为 0，有

$$
\begin{align}
\nabla_b \mathcal{L}(b, w, \alpha) &= 0 = - \sum_{n=1}^N \alpha_n y_n \\
\nabla_w \mathcal{L}(b, w, \alpha) &= 0 = w - \sum_{n=1}^N \alpha_n y_n z_n
\end{align}
$$


带回 Dual Form 有

$$\max_{\alpha_n \geq 0, \sum y_n \alpha_n = 0, w = \sum \alpha_n y_n z_n} -\frac{1}{2} \|\sum_{n=1}^N \alpha_n y_n z_n\|^2_2 + \sum_{n=1}^N \alpha_n$$

根据 KKT condition complementary slack 有，

$$\alpha_n (1 - y_n (w^T z_n + b)) = 0$$ 

当 $$\alpha_n > 0$$, 则 $$1 - y_n (w^T z_n + b) = 0$$，也就是说 $$\alpha_n$$ 对应的点 $$z_n$$ 一定位于 support boundary 上，我们将这样的点称为 support vector，同时有 $$w = \sum_{n=1}^N \alpha_n y_n z_n$$，由于 $$\alpha_n \geq 0$$, 因此 only support vectors (SVs) contribute to $$w$$，换句话说 $$w$$ can be represented as the linear combination of support vectors。与此同时使用任意 support vector 可以求解 $$b = y_n - w^T z_n$$, $$ w  \sum_{n=1}^N \alpha_n y_n z_n$$。

Dual Form

$$\min_{\alpha_n \geq 0,\, \sum y_n \alpha_n = 0} \frac{1}{2} \|\sum_{n=1}^N \alpha_n y_n z_n\|^2_2 - \sum_{n=1}^N \alpha_n$$

or 

$$
\begin{eqnarray*}
  \min_{\boldsymbol\alpha} && 
      \frac{1}{2} \sum_{n=1}^N \sum_{m=1}^N \alpha_n \alpha_m y_n y_m \mathbf{z}_n^T \mathbf{z}_m
      - \sum_{n=1}^N \alpha_n \\
  \mathrm{s.t.}        && \sum_{n=1}^N y_n \alpha_n = 0 \\ 
                       && \alpha_n \geq 0  \ \ \ n=1,\cdots,N
\end{eqnarray*}
$$


在求解得到 $$\alpha$$ 后，使用任意 support vector $$(z_s, y_s)$$

$$
\begin{aligned}
w &= \sum_{n=1}^N \alpha_n y_n z_n \\
b &= y_s - w^T z_s = y_s - \sum_{n=1}^N\alpha_n y_n (z_n^T z_s)
\end{aligned}
$$

---

## Week-2

### Kernel Support Vector Machine

在 Dual Support Vector Machine 中讲到了求解过程中需要计算 $$z_n^T z_m = \Phi(x_n)^T \Phi(x_m)$$。现在考虑二次多项式变换
$$\Phi_2(x) = (1, x_1, x_2, \ldots, x_d,x_1^2, x_1 x_2, \ldots, x_1 x_d, x_2 x_1, x_2^2, \ldots,  x_2 x_d, \ldots, x_d^2)$$
那么

$$
\begin{aligned}
\Phi_2(x)^T \Phi_2(x') &= 1 + \sum_{i=1}^d x_i x_i' + \sum_{i=1}^d \sum_{j=1}^d x_i x_j x_i' x_j' \\
&= 1 + \sum_{i=1}^d x_i x_i' + \sum_{i=1}^d  x_i x_i' \sum_{j=1}^d x_j x_j' \\
&= 1 + x^T x' + (x^T x') (x^T x')
\end{aligned}
$$


考虑更一般的二次 Kernel 

$$\Phi_2(x) = (1, \sqrt{2\gamma}x_1, \sqrt{2\gamma}x_2, \ldots,\sqrt{2\gamma} x_d, \gamma x_1^2,\gamma x_1 x_2, \ldots,\gamma x_1 x_d,\gamma x_2 x_1,\gamma x_2^2, \ldots, \gamma x_2 x_d, \ldots,\gamma x_d^2)$$

$$
\begin{aligned}
K_{\Phi_2}(x, x') =& 1 + 2 \gamma x^T x' + \gamma^2 (x^T x')^2 \\
 =& (1 + \gamma x^T x')^2 \text{ with } \gamma > 0
\end{aligned}
$$

Recall 

$$b = y_s - w^T z_s = y_s - \left(\sum_{n=1}^N \alpha_n y_n z_n\right)^T z_s  = y_s - \sum_{n=1}^N \alpha_n y_n K(x_n, x_s)$$

其中 $$x_s$$ 为任意 support vector。

那么对任意 test input $$x$$

$$g_\text{SVM}(x) = \text{sign}(w^T \Phi(x) + b) = \text{sign}\left(\sum_{n=1}^N \alpha_n y_n K(x_n, x) + b\right)$$

Gaussian Kernel and Infinite Dimensional Transform
考虑一维的 $$x$$, $$\Phi(x) = \exp(-x^2)\left(1, \sqrt{\frac{2}{1!}} x, \sqrt{\frac{2^2}{2!}} x^2, \ldots \right)$$, $$K(x, x') = \Phi(x)^T \Phi(x')$$

More generally, Gaussian kernel

$$K(x, x') = \exp(-\gamma \| x - x' \|_2^2) \text{ with }\gamma > 0$$

also called Radial Basis Function (RBF) kernel.

$$\gamma$$ 越大越容易 overfitting, the extreme case $$K_{\gamma \rightarrow \infty}(x, x') = I\{x = x'\}$$

其对偶形式为 (Dual Form)

$$
\begin{eqnarray*}
  \min_{\boldsymbol\alpha} && 
      \frac{1}{2} \sum_{n=1}^N \sum_{m=1}^N \alpha_n \alpha_m y_n y_m K(\mathbf{x}_n, \mathbf{x}_m)
      - \sum_{n=1}^N \alpha_n \\
  \mathrm{s.t.}        && \sum_{n=1}^N y_n \alpha_n = 0 \\ 
                       && \alpha_n \geq 0  \ \ \ n=1,\cdots,N
\end{eqnarray*}
$$


在求解得到 $$\alpha$$ 后，使用任意 support vector $$(z_s, y_s)$$

$$
\begin{aligned}
w &= \sum_{n=1}^N \alpha_n y_n z_n \\
b &= y_s - w^T z_s = y_s - \sum_{n=1}^N\alpha_n y_n K(x_n, x_s)
\end{aligned}
$$


### Soft Margin Support Vector Machine

Recall pocket 
$$\min_{b,w} \sum_{n=1}^N I\{y_n \neq \text{sign}(w^T z_n + b)\}$$

combine pocket with hard margin svm

$$
\begin{aligned}
\max_{w, b}\quad  &\frac{1}{2}\|w\|_2^2 + C \cdot \sum_{n=1}^N I\{y_n \neq \text{sign}(w^T z_n + b)\}\\
\text{subject to}\quad & y_n(w^T x_n + b) \geq 1 \quad \text{for correct } n \\
& y_n(w^T z_n + b) \geq -\infty \quad \text{for incorret } n
\end{aligned}
$$


但是每个点的犯错误程度不同，为了体现出这种错误程度，可以引入变量 $$\xi_n$$

$$
\begin{align}
\max_{w, b}\quad  &\frac{1}{2}\|w\|_2^2 + C \cdot \sum_{n=1}^N \xi_n \\
\text{subject to}\quad & y_n(w^T x_n + b) \geq 1 - \xi_n \quad \text{for all } n\\
& \xi_n \geq 0 
\end{align}
$$


其 Lagrange function 为

$$
\begin{aligned}\mathcal{L}(b, w, \alpha) = &\frac{1}{2} w^T w + C\cdot \sum_{n=1}^N \xi_n \\
&+ \sum_{n=1}^N \alpha_n(1 - \xi_n - y_n(w^T z_n + b)) + \sum_{n=1}^N \beta_n \cdot (-\xi_n)\end{aligned}
$$


根据 KKT condition

$$\nabla_{\xi_n} \mathcal{L} = 0 = C - \alpha_n - \beta_n$$ 因为 $$\beta_n \geq 0$$ 故 $$0 \leq \alpha_n \leq C$$

根据 KKT complementary slackness condition

$$
\begin{aligned}
\alpha_n(1 - \xi_n - y_n(w^T z_n + b)) &= 0 \\
(C - \alpha_n)\xi_n &= 0
\end{aligned}
$$

对于 $$0 < \alpha_n < C$$ 我们可以根据此  $$y_s, x_s$$ 计算得到 $$b = y_s - \sum_{n} \alpha_n y_n K(x_n, x_s)$$

其对偶形式为 (Dual Form)

$$
\begin{eqnarray*}
  \min_{\boldsymbol\alpha} && 
      \frac{1}{2} \sum_{n=1}^N \sum_{m=1}^N \alpha_n \alpha_m y_n y_m K(\mathbf{x}_n, \mathbf{x}_m)
      - \sum_{n=1}^N \alpha_n \\
  \mathrm{s.t.}        && \sum_{n=1}^N y_n \alpha_n = 0 \\ 
                       && 0 \leq \alpha_n \leq C \ \ \ n=1,\cdots,N
\end{eqnarray*}
$$


根据 $$\alpha_n$$ 的取值，可以对 $$(x_n, y_n)$$ 做如下分类

* $$\alpha_n = 0$$ 这类点为正确分类点，且在支撑边界之外
* $$0 < \alpha_n < C$$ 这类点为 support vectors 
* $$\alpha_n = C$$ 这类点为越过了支撑边界的点，其中可能存在被错误分类的点，也可能存在被正确分类的点，$$\xi_n = $$ violation amount

$$C$$ 越大两个支撑边界间距 ($$1/\|w\|$$) 越 thin ，越容易 overfitting
在使用 Gaussian Kernel 时，$$\gamma$$ 越大越容易 overfitting

在使用 kernel trick 时 $$w$$ 可能无法算出来，但是 $$\|w\|_2$$ 是可以计算的 $$\|w\|_2^2 = \sum_{n=1}^N \sum_{m=1}^N \alpha_n \alpha_m y_n y_m K(\mathbf{x}_n, \mathbf{x}_m)$$， $$C$$ 增大 $$\|w\|_2$$ 也随之增大，求解过程也更久了，从对偶问题的形式来看，需要搜索的空间更大了

---

## Week-3 

### Kernel Logistic Regression

回忆 SVM 的对偶形式

$$
\begin{array}{ll} 
\text{minimize } & \frac{1}{2}w^T w + C\sum_{n=1}^N\xi_n \\ 
\text{subject to} & y_n(w^T z_n + b) \geq 1 - \xi_n \\ 
& \xi_n \geq 0,\quad n=1,2,\ldots,N
\end{array}
$$


最后所有的点所对应的 $$\xi_n$$ 可以分成两类 

$$
\xi_n =
\begin{cases}
1-y_n(w^T z_n + b),  & \text{if }\, y_n(w^T z_n + b) < 1 \\
0, & \text{if }\,  y_n(w^T z_n + b) \geq 1
\end{cases}
$$

which means $$\xi_n = \max(1-y_n(w^T z_n + b), 0)$$ so we can cast the constrained optimization problem into an unconstrained one as

$$
\text{minimize } \frac{1}{2}w^T w + C\sum_{n=1}^N  \max(1-y_n(w^T z_n + b), 0).
$$


而 $$\max(1 - y \cdot s, 0)$$-hinge error message 为 $$\text{err}_{0/1}$$ 的 upper bound，如果我们将 error 换成另一个 $$\text{err}_{0/1}$$ 的 upper bound $$\log_2(1 + \exp(-y s))$$，我们将得到 L2-regularized logistic regression

在 SVM 的最终解中 $$w = \sum (\alpha_n y_n) z_n$$，现在我们
claim:

$$\min_{w} \frac{\lambda}{N} w^T w + \frac{1}{N} \sum_{n=1}^N \text{err} (y_n, w^T z_n)$$

最优解 $$w_* = \sum_{n=1}^{N} \beta_n z_n$$，我们可以直接将此式带入

$$\min_{w} \frac{\lambda}{N} w^T w + \frac{1}{N} \sum_{n=1}^N \log(1 + \exp(-y_n w^T z_n))$$

可以得到 kernel logistic regression

$$\min_{\beta} \frac{\lambda}{N} \sum_{n=1}^N \sum_{m=1}^N \beta_n \beta_m K(x_n, x_m) + \frac{1}{N} \sum_{n=1}^N \log\left(1 + \exp\left(- y_n \sum_{m=1}^N \beta_m K(x_m, x_n)\right)\right)$$

可以直接通过 SGD 求解 $$\beta$$

### Support Vector Regression

对于任意 L2-regularized linear model

$$\min_{w} \frac{\lambda}{N} w^T w + \frac{1}{N} \sum_{n=1}^N \text{err} (y_n, w^T z_n)$$

最优解 $$w_* = \sum_{n=1}^{N} \beta_n z_n$$

$$\text{err}(y, w^T z) = (y - w^T z)^2$$

带入上式得

$$\min_{w} \frac{\lambda}{N} w^T w + \frac{1}{N} \sum_{n=1}^N (y_n - w^T z_n)^2$$

我们可以将对 $$w$$ 的求解转变为对 $$\beta$$ 的求解

$$
\min_{\beta} \frac{\lambda}{N} \sum_{n=1}^N \sum_{m=1}^N \beta_n \beta_m K(x_n, x_m) + \frac{1}{N}\sum_{n=1}^N\left(y_n - \sum_{m=1}^N\beta_m K(x_n, x_m) \right)^2 \\
= \frac{\lambda}{N} \beta^T K \beta + \frac{1}{N}(\beta^T K^T K \beta - 2 \beta^T K^T y + y^T y)
$$

$$\nabla_{\beta} E = 0$$ 得到 $$\beta = (\lambda I + K)^{-1} y$$

Tube Regression

$$\text{err}(y, s) = \max(0, |s-y| - \epsilon)$$

$$\min_{\beta} \frac{\lambda}{N} \sum_{n=1}^N \sum_{m=1}^N \beta_n \beta_m K(x_n, x_m) + \frac{1}{N}\sum_{n=1}^N\max(0, |w^T z_n - y| - \epsilon)$$

mimic standard SVM 

$$\min_{b, w} \frac{1}{2} w^T w + C \sum_{n=1}^N \max(0, |w^T z_n + b - y_n| - \epsilon)$$

变换成带有 linear constraints version

$$
\begin{align}
\min_{w, b, \xi} & \frac{1}{2} w^T w + C \sum_{n=1}^N (\xi_n^- + \xi_n^+) \\
& -\epsilon - \xi_n^- \leq y_n - w^T z_n - b \leq \epsilon + \xi^+_n \\
& \xi_n^- \geq 0, \xi_n^+ \geq 0
\end{align}
$$


使用 KKT conditions

$$
\begin{align}
\min &\frac{1}{2} \sum_{n=1}^N \sum_{m=1}^N(\alpha_n^+ - \alpha_n^-)(\alpha_m^+ - \alpha_m^-) k_{n,m}\\
&+ \sum_{n=1}^N ((\epsilon - y_n)\alpha_n^+ + (\epsilon + y_n) \alpha_n^-) \\
s.t. & \sum_{n=1}^N 1 \cdot (\alpha_n^+ - \alpha_n^-) = 0 \\
& 0 \leq \alpha_n^+ \leq C, 0 \leq \alpha_n^- \leq C
\end{align}
$$


求解对偶问题后可得 $$w = \sum_{n=1}^N (\alpha_n^+ - \alpha_n^-)z_n$$

$$
\begin{align}
\alpha_n^+(\epsilon + \xi_n^+ - y_n + w^T z_n + b) = 0 \\
\alpha_n^-(\epsilon + \xi_n^- + y_n - w^T z_n - b) = 0
\end{align}
$$


对于那些严格在 tube 内的点有 $$\alpha_n^+ = 0, \alpha_n^- = 0$$ 以及 $$\beta_n = 0$$

---

## Week-4

### Blending and Bagging

Why might aggregation work:

* feature transform
* regularization

Uniform blending

$$G(x) = \text{sign}\left(\sum_{t=1}^T 1 \cdot g_t(x)\right)$$

Linear Blending

$$G(x) = \text{sign}\left(\sum_{t=1}^T \alpha_t \cdot g_t(x) \right)$$ with $$\alpha_t \geq 0$$

### Adaptive Boosting

error rate $$\epsilon_t = \frac{\sum_{n=1}^N u_n^{(t)} I\{y_n \neq g_t(x_n)\}}{\sum_{n=1}^N u_n^{(t)}}$$

multiply incorrect $$\propto (1-\epsilon_t)$$; multiply correct $$\propto \epsilon_t$$

define scaling factor $$\blacklozenge_t = \sqrt{\frac{1-\epsilon_t}{\epsilon_t}}$$

$$
\begin{align}
\text{incorrect}(u_n) &\leftarrow \text{incorrect} \cdot \blacklozenge_t \\
\text{correct}(u_n) &\leftarrow \text{correct} \cdot 1/\blacklozenge_t
\end{align}
$$


if $$\epsilon_t \leq 1/2$$, $$\blacklozenge_t \geq 1$$, scale up incorrect

$$\alpha_t = \ln(\blacklozenge_t)$$

$$G(x) = \text{sign}\left(\sum_{t=1}^T \alpha_t g_t(x) \right)$$

在使用 decision stump 实现 adaboost 时，每轮 $$g_t$$ 都试图最小化

$$E_{\text{in}}^u(h) = \frac{1}{N} \sum_{n=1}^N u_n \cdot I\{y_n \neq h(x_n)\}$$

其中 

$$h_{s,i,\theta} = s \cdot \mbox{sign}(x_i - \theta)$$

每次都要扫描所有的维度，从中选出最优的 $$h_{s,i,\theta}$$。每次迭代之后会发现 $$\sum_{i=1}^N u_n$$ 都在减小。$$T$$ 过大可能会导致 overfitting.

## Week 5

### Decision Tree

Recursively build the tree

$$G(x) = \sum_{c=1}^C I\{b(x) = c\} G_c(x)$$

其中在选择划分标准时使用划分后的 impurity 作为标准

Gini index

$$1 - \sum_{k=1}^K \left(\frac{\sum_{n=1}^N I\{y_n = k\}}{N} \right)^2 = 1 - \sum_{k=1}^K p_k^2$$

$$p_k$$ is the frequency of the category $$k$$.

选择 Gini index 最小的，因为 Gini index 越小，划分后的集合纯度越大或者说混乱程度越低。Gini index 与 entropy 有相似的效果。

Entropy

$$-\sum_{k=1}^K p_k \log p_k$$

### Random Forest

```julia
function RandomForest(D)
    for t = 1:T
        request size-M data D_t by bootstrapping with D
        obtain tree g_t by DTree(D_t)
    end
	G = Uniform({g_t})
end
```

一对训练数据 $$(x_n, y_n)$$ 经过了 $$N$$ 次采样后没有被选到的概率 $$(1 - {\frac 1 N})^N = \frac{1}{e}$$，对每个 $$g_t$$ out of bag 的样本数量近似为 $${\frac N e}$$.

OOB 为训练样本，如果训练 $$g_t$$ 时没有使用 $$x_n$$ 那么 $$x_n$$ 就是 $$g_t$$ 的 OOB.

|   nil      |$$g_1$$  |$$g_2$$ |$$g_3$$ |$$\ldots$$ |$$g_T$$ |
|------------|-------|------|------|---------|------|
|$$(x_1,y_1)$$ |$$\tilde{D}_1$$| *|$$\tilde{D}_3$$ |$$\ldots$$ |$$\tilde{D}_T$$|
|$$(x_2,y_2)$$ |*| *|$$\tilde{D}_3$$ |$$\ldots$$ |$$\tilde{D}_T$$|
|$$\ldots$$    | blank| blank |blank |blank |blank|
|$$(x_N,y_N)$$ |$$\tilde{D}_1$$| *|* |$$\ldots$$ |* |



$$G_n^-$$ contains only trees that $$x_n$$ is OOB of the trees, e.g. in the table $$G_N^-(x_N) = \mbox{average}(g_2, g_3, g_T)$$, $$E_\text{oob}(G) = \frac{1}{N} \sum_{n=1}^N \mbox{err}(y_n, G_n^-(x_n))$$.

Feature selection: $$\mbox{importance}(i) = E_\text{oob}(G) - E_\text{oob}^{(p)}(G)$$, $$E_\text{oob}^{(p)}$$ comes from replacing each request of $$x_{n,i}$$ by a permuted OOB value. 

Assume both $$x_n$$ and $$x_{n'}$$ is the OOB of $$g_t$$, in $$E_\text{OOB}$$ we use $$g_t([x_{n,1}, \ldots, x_{n,i}, \ldots, x_{n,d}])$$ but in $$E_\text{OOB}^{(p)}$$ we replace $$x_{n, i}$$ with $$x_{n', i}$$, i.e. we do  $$g_t([x_{n,1}, \ldots, x_{n',i}, \ldots, x_{n,d}])$$.

---

## Week 6

### Gradient Boost

$$
u_n^{(t+1)} = \begin{cases}
u_n^{(t)} \cdot \blacklozenge_t \quad \text{incorrect} \\
u_n^{(t)} / \blacklozenge_t \quad \text{correct}
\end{cases}
$$



$$u_n^{(t+1)} = u_n^{(t)} \cdot \blacklozenge_t ^ {-y_n g_t(x_n)} = u_n^{(t)} \cdot \exp(-y_n \alpha_t g_t(x_n))$$

$$u_n^{(T+1)} = u_n^{(1)} \cdot \prod_{t=1}^T \exp(-y_n \alpha_t g_t(x_n)) = \frac{1}{N} \cdot \exp\left(-y_n \sum_{t=1}^T \alpha_t g_t(x_n)\right)$$

其中 $$\sum_{t=1}^T \alpha_t g_t(x_n)$$ 可以看成投票的 score 我们希望 $$y_n \cdot s_n$$ positive and large.
因此我们可以将 AdaBoost 看成如下的优化问题

$$\sum_{n=1}^N u_n ^ {(T+1)} = {\frac 1 N} \sum_{n=1}^N \exp\left(-y_n \sum_{t=1}^T \alpha_t g_t(x_n)\right)$$

$$\exp(-y\cdot s)$$ 称为 exp-loss 

Gradient Boosting for Arbitrary Error Function 

AdaBoost

$$\min_\eta \min_h {\frac 1 N} \sum_{n=1}^N \exp\left(-y_n \left(\sum_{\tau = 1}^{t-1} \alpha_\tau g_\tau (x_n) + \eta h(x_n)\right)\right)$$

GradientBoost

$$\min_\eta \min_h {\frac 1 N} \sum_{n=1}^N \text{err}\left(\sum_{\tau = 1}^{t-1} \alpha_\tau g_\tau (x_n) + \eta h(x_n), y_n\right)$$

$$h(x_n)$$ 可以看成 Hilbert Space 中的一个点 （向量），$$\eta$$ 为步长。

### Neural Network

设输入层为第 0 层， $$w^{(i)}$$ 为连接 $$i-1$$ 层和 $$i$$ 层的 weight.

$$e_n = (y_n - s_1^{(L)})^2 = \left(y_n - \sum_{i=0}^{d^{(L-1)}} w_{i1}^{(L)} x_i^{(L-1)}\right)^2$$

设 $$\frac{\partial e_n}{\partial s^{(\ell)}} = \delta^{(\ell)}$$, 

* 对于最后一层 

    $$\delta_1^{(L)} = -2(y_n - s_1^{(L)})$$

* 对于其他层有 

    $$\begin{align}\delta^{(\ell)}_j = \frac{\partial e_n}{\partial s_j^{(\ell)}} &= \sum_k \frac{\partial e_n}{\partial s_k^{(\ell+1)}} \frac{\partial s_k^{(\ell+1)}}{\partial x_j^{(\ell)}} \frac{\partial x_j^{(\ell)}}{\partial s_j^{(\ell)}} \\ &= \sum_k\left(\delta_k^{(\ell+1)}\right)\left(w_{jk}^{(\ell+1)}\right)\left(\tanh'(s_j^{(\ell)})\right)\end{align}$$

SGD:
- randomly pick $$n \in \{1,2,\ldots,N\}$$
- forward compute all $$x_i^{(\ell)}$$ with $$x^{(0)}=x_n$$
- backward compute all $$\delta_j^{(\ell)}$$
- gradient descent $$w_{ij}^{(\ell)} \leftarrow w_{ij}^{(\ell)} - \eta x_i^{(\ell-1)} \delta_j^{(\ell)} $$

sometimes 1 - 3 is parallelly done many times and average $$x_i^{(\ell-1)} \delta_j^{(\ell)}$$ taken for update in 4  (mini-batch gradient descent)

设 $$W$$ 为连接 layer $$\ell$$ 和 layer $$\ell ＋ 1$$ 的矩阵，$$x$$ 为 layer  $$\ell$$ 的输出， $$\delta^{(\ell+1)}$$ 为 layer $$\ell+1$$ 的误差，那么 layer $$\ell$$ 的误差可以使用矩阵和向量乘法描述为

$$\delta^{(\ell)} = W \cdot \delta^{(\ell+1)} \odot (1 - x^2)$$

其中 $$\odot$$ 为 element-wise multiply operation

而 $$W$$ 的梯度可以通过 vector outer product 计算得到, 

$$x \cdot (\delta^{(\ell + 1)})^T $$

如果使用 numpy 的话可以用 `np.outer()` 完成 outer product operation

---

## Week-7

### Deep Learning

Linear autoencoder or PCA

$${\frac 1 N} \sum_{n=1}^N \|x_n - W W^T x_n \|^2$$

- let $$\bar{x} = {\frac 1 N} \sum_{n=1}^N x_n$$, and let $$x_n \leftarrow x_n - {\bar x}$$
- calculate $${\tilde d}$$ top eigenvectors $$w_1, w_2, \ldots, w_{\tilde d}$$ of $$X^T X$$
- return feature transform $$\Phi(x) = W (x - {\bar x})$$

### Radial Basis Network

Radial Basis Function

$$\phi(x, \mu) = \exp(-\gamma \| x - \mu\|^2)$$

Full RBF Network

$$h(x) = \mbox{Output}\left(\sum_{m=1}^M \beta_m \mbox{RBF}(x, \mu_m)\right)$$

k-means

$$E_\text{in}(S_1, \ldots, S_M; \mu_1, \ldots, \mu_M) = \sum_{n=1}^N \sum_{m=1}^M I(x_n \in S_m)\|x_n - \mu_m \|^2$$

### Matrix Factorization

$$R \approx W^T V$$

$$\sum(r_{nm} - w_m^T v_n)^2$$ movie m and user n

$$v_n$$ 维度 $$\tilde{d} \times 1$$，第 n 个用户的 topic 分布
$$w_m$$ 维度 $$\tilde{d} \times 1$$，第 m 部电影的 topic 分布

SGD for Matrix Factorization

- initialize $$\tilde{d}$$ dimension vectors $$\{w_m\}, \{v_n\}$$ randomly
- for t = 0, 1, ..., T
 * randomly pick $$(n,m)$$ within all known $$r_{nm}$$
 * calculate residual $$\tilde{r}_{nm} = (r_{nm} - w_m^T v_n)$$
 * $$v_n \leftarrow v_n + \eta \cdot \tilde{r}_{nm} w_m$$
 * $$w_m \leftarrow w_m + \eta \cdot \tilde{r}_{nm} v_n$$.