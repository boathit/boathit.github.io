---
layout: post
comments: false
title: "Introductory Lectures on Convex Optimization 1"
date: 2017-02-01 12:00:00
tags: convex foundation nesterov-lectures book-notes
---


> Chapter 1 of Introductory Lectures on Convex Optimization.

<!--more-->

{: class="table-of-content"}
* TOC
{:toc}

## 0 Notations

$$C_L^{k,p}(Q)$$ the functions that are $$k$$-order differentiable and $$p$$-order gradient Lipschitz continuous on set $$Q$$. 

$$\mathcal{F}_L^{k,p}(Q)$$ the convex functions that are $$k$$-order differentiable and $$p$$-order gradient Lipschitz continuous on set $$Q$$.

$$\mathcal{S}_{\mu, L}^{k,p}(Q)$$ the strongly convex functions that are $$k$$-order differentiable and $$p$$-order gradient Lipschitz continuous on set $$Q$$ and $$\mu$$ is the convexity parameter.



## 1 Nonlinear Optimization

### 1.2 Local methods in unconstrained minimization

#### 1.2.1 Relaxation and approximation

> To approximate means to replace an initial complex objective by a simplified one, which is close by its properties to the original.

In nonlinear optimization we usually apply local approximations based on derivatives of nonlinear functions, such as linear and quadratic approximations.

Let $$f(x)$$ be differentiable at $$\bar{x}$$ then for $$y \in \mathbb{R}^n$$ we have 

$$
f ( y ) = f ( \bar { x } ) + \left\langle f ^ { \prime } ( \bar { x } ) ,y - \bar { x } \right\rangle + o ( \| y - \bar { x } \| )
$$

where $$o(r)$$ is some function of $$r \geq 0$$ such that

$$
\lim _ { r \rightarrow 0} \frac { 1} { r } o ( r ) = 0,\quad o ( 0) = 0
$$

Let $$s$$ be a direction in $$\mathbb{R}^n$$ with $$\|s\| = 1$$. Consider the local decrease of $$f(x)$$ along $$s$$:

$$
\Delta ( s ) = \lim _ { \alpha \rightarrow 0} \frac { 1} { \alpha } [ f ( \bar { x } + \alpha s ) - f ( \bar { x } ) ] = \langle f'(x), s \rangle
$$

Using the Cauchy-Schwartz inequality:

$$
- \| x \| \cdot \| y \| \leq \langle x ,y \rangle \leq \| x \| \cdot \| y \|
$$

we obtain $$\Delta(s) = \langle f'(x), s \rangle \geq - \|f'(x)\|$$. Let us take

$$
\bar { s } = - f ^ { \prime } ( \overline { x } ) / \| f ^ { \prime } (\overline { x }) \|
$$

Then
$$
\Delta ( \bar { s } ) = - \left\langle f ^ { \prime } ( \bar { x } ) ,f ^ { \prime } ( \bar { x } ) \right\rangle / \| f ^ { \prime } ( \bar { x } ) \| = - \| f ^ { \prime } ( \bar { x } ) \|
$$

Thus the direction $$-f(\bar{x})$$ is the direction of the fastest local decrease of $$f(x)$$ at point $$\bar{x}$$.

**Theorem 1.2.1 (First-order optimality condition)**

Let $$x^*$$ be a local minimum of differentiable function $$f(x)$$. Then 

$$
f'(x^*) = 0
$$

Proof: Since $$x^*$$ is a local minimum of $$f(x)$$, then there exists $$r > 0$$ such that for all $$y$$ with $$\|y-x\| \leq r$$, we have $$f(y) \geq f(x^*)$$. Since $$f(x)$$ is differentiable, this implies that

$$
f ( y ) = f \left( x ^ { * } \right) + \left\langle f ^ { \prime } \left( x ^ { * } \right) ,y - x ^ { * } \right\rangle + o \left( \| y - x ^ { * } \| \right) \geq f \left( x ^ { * } \right)
$$

Thus for all $$s, \|s\|=1$$ we have $$\left\langle f ^ { \prime } \left( x ^ { * } \right) ,s \right\rangle \geq 0$$. Consider the directions $$s$$ and $$-s$$, we get $$f'(x) = 0$$.


**Remark.** The points satisfying Theorem 1.2.1 are called the stationary points of function $$f$$.


Let us introduce the second order approximation. Let $$f(x)$$ be twice differentiable at $$\bar{x}$$. Then

$$
f ( y ) = f ( \bar { x } ) + \left\langle f ^ { \prime } ( \bar { x } ) ,y - \bar { x } \right\rangle + \frac { 1} { 2} \left\langle f ^ { \prime \prime } ( \bar { x } ) ( y - \bar { x } ) ,y - \bar { x } \right\rangle + o \left( \| y - \bar { x } \| ^ { 2} \right)
$$

In what follows notation $$A \succeq 0$$ used for a symmetric matrix $$A$$ means that $$A$$ is positive semidefinite:

$$
\langle A x ,x \rangle \geq 0  \quad \forall x \in \mathbb{R}^n
$$

**Remark**: 

1. $$A$$ may satisfy $$\langle A x ,x \rangle \geq 0  \quad \forall x \in \mathbb{R}^n$$ even if it is not symmetric such as

    $$
    A = \left[
    \begin{array}{cc}
    1&0\\
    1&1
    \end{array}
    \right]
    $$

    since $$\langle A x ,x \rangle = x_1^2 + x_1 x_2 + x_2^2 = \frac{1}{2} (x_1 + x_2)^2 + \frac{1}{2}(x_1^2 + x_2^2) \geq 0 \quad \forall x \in \mathbb{R}^2$$.

2. In the definition of (semi-) positive definite matrix, we explicitly require the matrix must be symmetric.

**Theorem 1.2.2 (Second-order optimality condition)**

Let $$x^*$$ be a local minimum of twice differentiable function $$f(x)$$. Then 

$$
f'(x^*) = 0, f''(x^*) \succeq 0.
$$

Proof: Since $$x^*$$ is a local minimum of function $$f(x)$$,  there exists $$r > 0$$ such that for all $$y$$ with $$\|y - x^*\| \leq r$$ we have 

$$
f(y) \geq f(x^*)
$$

In view of Theorem 1.2.1, $$f'(x) = 0$$. Therefore for any such $$y$$,

$$
f ( y ) = f \left( x ^ { * } \right) + \left\langle f ^ { \prime \prime } \left( x ^ { * } \right) \left( y - x ^ { * } \right) ,y - x ^ { * } \right\rangle + o \left( \| y - x ^ { * } \| ^ { 2} \right) \geq f \left( x ^ { * } \right)
$$

Thus, $$\left\langle f ^ { \prime \prime } \left( x ^ { * } \right) s ,s \right\rangle \geq 0$$ for all $$s$$ with $$\|s\| = 1$$.

**Remark.** We can ignore the term $$o \left( \| y - x ^ { * } \| ^ { 2} \right)$$ in the computation since it can be arbitrarily small.

**Theorem 1.2.3**. Let function $$f(x)$$ be twice differentiable on $$\mathbb{R}^n$$ and let $$x^*$$ satisfy the following conditions:

$$
f'(x^*) = 0, \quad f''(x^*) \succ 0.
$$

Then $$x^*$$ is a strict local minimum of $$f(x)$$.

Proof: Note that in a small neighborbood of point $$x^*$$ function $$f(x)$$ can be represented as 

$$
f ( y ) = f \left( x ^ { * } \right) + \frac { 1} { 2} \left\langle f ^ { \prime \prime } \left( x ^ { * } \right) \left( y - x ^ { * } \right) ,y - x ^ { * } \right\rangle + o \left( \| y - x ^ { * } \| ^ { 2} \right)
$$

Since $$\frac { o ( r ) } { r } \rightarrow 0$$ there exists a value $$\bar{r}$$ such that for all $$r \in [0, \bar{r}]$$ we have

$$
\vert o ( r ) \vert \leq \frac { r } { 4} \lambda_\text{ min} \left( f ^ { \prime \prime } \left( x ^ { * } \right) \right)
$$

also recall that $$f''(x^*) \succ 0$$, therefore for any $$y$$ with $$\|y-x^*\| \leq \bar{r}$$ we have

$$
\left.\begin{aligned} f ( y ) & \geq f \left( x ^ { * } \right) + \frac { 1} { 2} \lambda _ \text{min} \left( f ^ { \prime \prime } \left( x ^ { * } \right) \right) \| y - x ^ { * } \| ^ { 2} + o \left( \| y - x ^ { * } \| ^ { 2} \right) \\ & \geq f \left( x ^ { * } \right) + \frac { 1} { 4} \lambda _ \text{min } \left( f ^ { \prime \prime } \left( x ^ { * } \right) \right) \| y - x ^ { * } \| ^ { 2} > f \left( x ^ { * } \right). \end{aligned} \right.
$$

**Remark**. 

* The unitary matrix $$U$$ only changes the direction of a vector, i.e., $$\langle Ux, Ux \rangle = \|x\|_2^2$$, therefore we have $$ \left\langle f ^ { \prime \prime } \left( x ^ { * } \right) \left( y - x ^ { * } \right) ,y - x ^ { * } \right\rangle \geq \lambda _ \text{min} \left( f ^ { \prime \prime } \left( x ^ { * } \right) \right) \| y - x ^ { * } \| ^ { 2}$$.
* We can always find a value $$\bar{r}$$ such that for all $$r \in [0, \bar{r}]$$, the sign of $$c \cdot r \pm o(r)$$ is dominated by $$c \cdot r$$ where $$c$$ is a constant.

#### 1.2.2 Classes of differentiable functions

**Lipschitz condition.**

We denote $$C_L^{k,p}(Q)$$ the class of functions with the following properties:

* any $$f\in C_L^{k,p}(Q)$$ is $$k$$ times continuously differentiable on $$Q$$.

* Its $$p$$th derivative is Lipschitz continuous on $$Q$$ with the constant $$L$$:
  
  $$
  \| f ^ { ( p ) } ( x ) - f ^ { ( p ) } ( y ) \| \leq L \| x - y \| \quad \forall x, y \in Q
  $$

For us the most important class of functions will be $$C_L^{1,1}(\mathbb{R}^n)$$.

**Lemma 1.2.2** For $$f(x)$$ that is twice differentiable we have

$$
\| f ^ { \prime \prime } ( x ) \| \leq L \Longleftrightarrow \| f ^ { \prime } ( x ) - f ^ { \prime } ( y ) \| \leq L \| x - y \|
$$

Proof. $$\Longrightarrow$$ For any $$x, y \in \mathbb{R}^n$$ we have 

$$
\left.\begin{aligned} f ^ { \prime } ( y ) & = f ^ { \prime } ( x ) + \int _ { 0} ^ { 1} f ^ { \prime \prime } ( x + \tau ( y - x ) ) ( y - x ) d \tau \\ & = f ^ { \prime } ( x ) + \left( \int _ { 0} ^ { 1} f ^ { \prime \prime } ( x + \tau ( y - x ) ) d \tau \right) \cdot ( y - x ) \end{aligned} \right.
$$

The first equality holds as $$f'(y) - f'(x) = \int_0^1  d f'(x + \tau (y-x))$$.

Therefore, if condition  $$\| f ^ { \prime \prime } ( x ) \| \leq L$$ holds then

$$
\left.\begin{aligned} \| f ^ { \prime } ( y ) - f ^ { \prime } ( x ) \| & = \left\| \left( \int _ { 0} ^ { 1} f ^ { \prime \prime } ( x + \tau ( y - x ) ) d \tau \right) \cdot ( y - x ) \right\| \\ & \leq \left\| \int _ { 0} ^ { 1} f ^ { \prime \prime } ( x + \tau ( y - x ) ) d \tau \right\| \cdot \| y - x \| \\ & \leq \int _ { 0} ^ { 1} \| f ^ { \prime \prime } ( x + \tau ( y - x ) ) \| d \tau \cdot \| y - x \| \\ & \leq L \| y - x \| \end{aligned} \right.
$$

 $$\Longleftarrow$$ On the other hand, if $$\| f ^ { \prime } ( x ) - f ^ { \prime } ( y ) \| \leq L \| x - y \|$$ holds then for any $$s \in \mathbb{R}^n$$ and $$\alpha > 0$$ we have

$$
\left\| \left( \int _ { 0} ^ { \alpha } f ^ { \prime \prime } ( x + \tau s ) d \tau \right) \cdot s  \right\| = \left \| \int_{0}^{\alpha} d f'(x + \tau s) \right\| = \| f ^ { \prime } ( x + \alpha s ) - f ^ { \prime } ( x ) \| \leq \alpha L \| s \|
$$

Dividing this inequality by $$\alpha$$ and tending $$\alpha \rightarrow 0$$,

$$
\lim_{\alpha \rightarrow 0} \left\| \frac{f ^ { \prime } ( x + \alpha s ) - f ^ { \prime } ( x )}{\alpha} \right\| \leq L \| s \|
$$

(the l.h.s is directional derivative of $$f'$$ at point $$x$$ along $$s$$) we obtain $$\| f ^ { \prime \prime } ( x )  \cdot s\| \leq L\|s\| \quad \forall s \in \mathbb{R}^n$$ which we can conclude $$\| f ^ { \prime \prime } ( x ) \| \leq L$$.

**Lemma 1.2.3** Let $$f \in C_L^{1,1}(\mathbb{R}^n)$$. Then for any $$x, y$$ from $$\mathbb{R}^n$$ we have

$$
\vert f ( y ) - f ( x ) - \left\langle f ^ { \prime } ( x ) ,y - x \right\rangle \vert \leq \frac { L } { 2} \| y - x \| ^ { 2}
$$

Proof: Note that 

$$
\left.\begin{aligned} f ( y ) & = f ( x ) + \int _ { 0} ^ { 1} \left\langle f ^ { \prime } ( x + \tau ( y - x ) ) ,y - x \right) d \tau \\ & = f ( x ) + \left\langle f ^ { \prime } ( x ) ,y - x \right\rangle + \int _ { 0} ^ { 1} \left\langle f ^ { \prime } ( x + \tau ( y - x ) ) - f ^ { \prime } ( x ) ,y - x \right\rangle d \tau. \end{aligned} \right.
$$

**Remark.** This is the geometric interpretation of functions $$f \in C_L^{1,1}(\mathbb{R}^n)$$. $$f(x)$$ is bounded by two quadratic functions.

**Lemma 1.2.4** Let $$f \in C_L^{2,2}(\mathbb{R}^n)$$. Then for any $$x, y$$ from $$\mathbb{R}^n$$ we have 

$$
\| f ^ { \prime } ( y ) - f ^ { \prime } ( x ) - f ^ { \prime \prime } ( x ) ( y - x ) \| \leq \frac { M } { 2} \| y - x \| ^ { 2}
$$

and 

$$
\left.\begin{array} { r l } { \vert f ( y ) - f ( x ) - \left\langle f ^ { \prime } ( x ) ,y - x \right\rangle - \frac { 1} { 2} \left\langle f ^ { \prime \prime } ( x ) ( y - x ) ,y - x \right\rangle \vert }  { \leq \frac { M } { 6} \| y - x \| ^ { 3} } \end{array} \right.
$$

Proof: Let us fix some $$x, y \in \mathbb{R}^n$$. Then

$$
\left.\begin{aligned} f ^ { \prime } ( y ) & = f ^ { \prime } ( x ) + \int _ { 0} ^ { 1} f ^ { \prime \prime } ( x + \tau ( y - x ) ) ( y - x ) d \tau \\ & = f ^ { \prime } ( x ) + f ^ { \prime \prime } ( x ) ( y - x ) + \int _ { 0} ^ { 1} \left( f ^ { \prime \prime } ( x + \tau ( y - x ) ) - f ^ { \prime \prime } ( x ) \right) ( y - x ) d \tau \end{aligned} \right.
$$

Therefore

$$
\left.\begin{array} { c c} 
 \| f ^ { \prime } ( y ) - f ^ { \prime } ( x ) - f ^ { \prime \prime } ( x ) ( y - x ) \|  \\  
 \begin{aligned}
 &= \left\| \int _ { 0} ^ { 1} \left( f ^ { \prime \prime } ( x + \tau ( y - x ) ) - f ^ { \prime \prime } ( x ) \right) ( y - x ) d \tau \right\|  \\
 &\leq \int _ { 0} ^ { 1} \| \left( f ^ { \prime \prime } ( x + \tau ( y - x ) ) - f ^ { \prime \prime } ( x ) \right) ( y - x ) \| d \tau \\
 &\leq \int _ { 0} ^ { 1} \| f ^ { \prime \prime } ( x + \tau ( y - x ) ) - f ^ { \prime \prime } ( x ) \| \cdot \| y - x \| d \tau \\
 &\leq \int _ { 0} ^ { 1} \tau M \| y - x \| ^ { 2} d \tau = \frac { M } { 2} \| y - x \| ^ { 2}
 \end{aligned}
\end{array} \right.
$$

For the other inequality since

$$
f(y) - f(x) - \langle f'(x), y-x \rangle = \int_0^1 \left\langle f ^ { \prime } ( x + \tau_1 ( y - x ) ) - f'(x) ,y - x \right\rangle d \tau_1 =:A
$$

and we have

$$
\begin{array}{ll}
f'(x + \tau_1 (y-x)) - f'(x) &= &\int_0^1 f''(x + \tau_2 \tau_1 (y-x))\tau_1 (y-x) d\tau_2 \\
&= &\int_0^1 f''(x + \tau_2 \tau_1 (y-x) - f''(x))\tau_1 (y-x) d\tau_2\\ 
&  &+ f''(x)\tau_1 (y-x)
\end{array}
$$

therefore

$$
\begin{array}{lc}
A &= &\int_0^1\int_0^1  \langle (f''(x + \tau_2 \tau_1 (y-x) - f''(x))\tau_1 (y-x), y-x \rangle d\tau_2 d\tau_1\\  
& &+ \int_0^1 \langle f''(x)\tau_1 (y-x), y-x \rangle d\tau_1 \\
&= &\int_0^1\int_0^1  \langle (f''(x + \tau_2 \tau_1 (y-x) - f''(x))\tau_1 (y-x), y-x \rangle d\tau_2 d\tau_1\\ 
& &+ \frac{1}{2} \langle f''(x)(y-x), y-x \rangle \\
\end{array}
$$

which implies

$$
\begin{array}{l}
&\left\| A - \frac{1}{2} \langle f''(x)(y-x), y-x \rangle \right\|\\ 
=& \left\| \int_0^1\int_0^1  \langle (f''(x + \tau_2 \tau_1 (y-x) - f''(x))\tau_1 (y-x), y-x \rangle d\tau_2 d\tau_1 \right\|  \\
\leq& M \frac{\|y-x\|^3}{6}.
\end{array}
$$

**Corollary 1.2.2** Let $$f \in C_M^{2,2}(\mathbb{R}^n)$$ and $$\|y-x\| = r$$. Then

$$
f ^ { \prime \prime } ( x ) - M r I _ { n } \leq f ^ { \prime \prime } ( y ) \leq f ^ { \prime \prime } ( x ) + M r I _ { n }
$$

where $$I_n$$ is the unit matrix in $$\mathbb{R}^n$$.

Proof: We have $$\| f''(y) - f''(x) \|_2 \leq Mr$$ which implies that all eigenvalues of the matrix $$f''(y) - f''(x)$$ is bounded by $$Mr$$. Hence, $$- M r I _ { n } \leq f ^ { \prime \prime } ( y ) - f ^ { \prime \prime } ( x ) \leq M r I _ { n }$$. 

#### 1.2.3 Gradient method

**Step-size strategies**:

1. constant step $$h_k = h > 0$$

2. full relaxation: $$h _ { k } = \arg \min _ { h \geq 0} f \left( x _ { k } - h f ^ { \prime } \left( x _ { k } \right) \right)$$

3. Goldstein-Armijo rule: find $$x_{k+1} = x_k - h f'(x_k)$$ such that
   
   $$
   \left.\begin{array} { l } { \alpha \left\langle f ^ { \prime } \left( x _ { k } \right) ,x _ { k } - x _ { k + 1} \right\rangle \leq f \left( x _ { k } \right) - f \left( x _ { k + 1} \right) } \\ { \beta \left\langle f ^ { \prime } \left( x _ { k } \right) ,x _ { k } - x _ { k + 1} \right\rangle \geq f \left( x _ { k } \right) - f \left( x _ { k + 1} \right) } \end{array} \right.
   $$
   
   where $$0 < \alpha < \beta < 1$$ are fixed parameters.

The first strategy is mainly used in the context of convex optimization.

The third one is used in majority of practical algorithm.

We analyze the convergence propertity of Goldstein-Armijo rule, in the view its condition we have

$$
f \left( x _ { k } \right) - f \left( x _ { k + 1} \right) \leq \beta \left\langle f ^ { \prime } \left( x _ { k } \right) ,x _ { k } - x _ { k + 1} \right\rangle = \beta h _ { k } \| f ^ { \prime } \left( x _ { k } \right) \| ^ { 2}
$$

From the Lipschitz continous we have

$$
f \left( x _ { k } \right) - f \left( x _ { k + 1} \right) \geq h _ { k } \left( 1- \frac { h _ { k } } { 2} L \right) \| f ^ { \prime } \left( x _ { k } \right) \| ^ { 2}
$$

Combining these two inequalities we obtain $$h _ { k } \geq \frac { 2} { L } ( 1- \beta )$$.

Again using the condition of Goldstein-Armijo rule we have

$$
f \left( x _ { k } \right) - f \left( x _ { k + 1} \right) \geq \alpha \left\langle f ^ { \prime } \left( x _ { k } \right) ,x _ { k } - x _ { k + 1} \right\rangle = \alpha h _ { k } \| f ^ { \prime } \left( x _ { k } \right) \| ^ { 2}
$$

Replacing $$h_k$$ with its lower bound we have

$$
f \left( x _ { k } \right) - f \left( x _ { k + 1} \right) \geq \frac { 2} { L } \alpha ( 1- \beta ) \| f ^ { \prime } \left( x _ { k } \right) \| ^ { 2}
$$

For all these strategies we have 

$$
f \left( x _ { k } \right) - f \left( x _ { k + 1} \right) \geq \frac { \omega } { L } \| f ^ { \prime } \left( x _ { k } \right) \| ^ { 2}
$$

where $$\omega$$ is a positive constant.

Summing up the inequalities for $$k = 0 \ldots N$$ we obtain

$$
\frac { \omega } { L } \sum _ { k = 0} ^ { N } \| f ^ { \prime } \left( x _ { k } \right) \| ^ { 2} \leq f \left( x _ { 0} \right) - f \left( x _ { N + 1} \right) \leq f \left( x _ { 0} \right) - f ^ { * }
$$

As $$f$$ is bounded below we have $$\| f ^ { \prime } \left( x _ { k } \right) \| \rightarrow 0\quad \text{ as } \quad k \rightarrow \infty$$.

Let us define $$g _ { N } ^ \text{min} = \min _ { 0\leq k \leq N } g _ { k }$$ where $$g _ { k } = \| f ^ { \prime } \left( x _ { k } \right) \|$$ then we immediately get the following inequality:

$$
g _ { N } ^ \text{min} \leq \frac { 1} { \sqrt { N + 1} } \left[ \frac { 1} { \omega } L \left( f \left( x _ { 0} \right) - f ^ { * } \right) \right] ^ { 1/ 2}
$$

By now we can say nothing about the rate of convergence of sequences $$\{f(x_k)\}$$ and $$\{x_k\}$$.

Note that if $$N + 1\geq \frac { L } { \omega \epsilon ^ { 2} } \left( f \left( x _ { 0} \right) - f ^ { * } \right)$$ we will get 

$$
g _ { N } ^ \text{min} \leq \frac { 1} { \sqrt { N + 1} } \left[ \frac { 1} { \omega } L \left( f \left( x _ { 0} \right) - f ^ { * } \right) \right] ^ { 1/ 2} \leq \epsilon
$$

Now we are ready to study the local convergence of the gradient method. Consider the unconstrained minimization problem

$$
\min_{x \in \mathbb{R}^n} f(x)
$$

under the **assumptions**:

1. $$f \in C _ { M } ^ { 2,2} \left( R ^ { n } \right)$$.

2. There exists a local minimum of function $$f$$ at which Hessian is postive definite.

3. The Hessian matrix $$f''(x^*)$$ is bounded by the constants $$0< l \leq L < \infty$$ as:
   
   $$
   l I _ { n } \leq f ^ { \prime \prime } \left( x ^ { * } \right) \leq L I _ { n }
   $$

4. Our starting point $$x_0$$ is close enough to $$x^*$$.

Consider the process: $$x _ { k + 1} = x _ { k } - h _ { k } f ^ { \prime } \left( x _ { k } \right)$$. We have

$$
\left.\begin{aligned} f ^ { \prime } \left( x _ { k } \right) & = f ^ { \prime } \left( x _ { k } \right) - f ^ { \prime } \left( x ^ { * } \right)  \quad (f'(x^*) = 0)\\ 
&= \int _ { 0} ^ { 1} f ^ { \prime \prime } \left( x ^ { * } + \tau \left( x _ { k } - x ^ { * } \right) \right) \left( x _ { k } - x ^ { * } \right) d \tau\\
& = G _ { k } \left( x _ { k } - x ^ { * } \right) \end{aligned} \right.
$$

where $$G_k = \int _ { 0} ^ { 1} f ^ { \prime \prime } \left( x ^ { * } + \tau \left( x _ { k } - x ^ { * } \right) \right) d \tau$$. Therefore

$$
x _ { k + 1} - x ^ { * } = x _ { k } - x ^ { * } - h _ { k } G _ { k } \left( x _ { k } - x ^ { * } \right) = \left( I - h _ { k } G _ { k } \right) \left( x _ { k } - x ^ { * } \right)
$$

We wish to prove the convergence of the sequence $$\{x_{k+1} - x^*\}$$. The standard technique for analyzing the process is called contracting mappings. The key idea is to bound the operator norm of the transformation matrix. Let sequence $$\{a_k\}$$ be defined as follows:

$$
a _ { 0} \in \mathbb{R} ^ { n } ,\quad a _ { k + 1} = A _ { k } a _ { k }
$$

where $$A_k$$ are $$n \times n$$ and $$\|A_k\| \leq 1 -q$$ where $$q \in (0, 1)$$. Then we have

$$
\| a _ { k + 1} \| \leq ( 1- q ) \| a _ { k } \| \leq ( 1- q ) ^ { k + 1} \| a _ { 0} \| \rightarrow 0
$$

In our case we need to bound $$\| I - h _ { k } G _ { k } \|$$. In view of Corollary 1.2.2 and $$r_k := \|x_k - x^*\|$$, we have

$$
f ^ { \prime \prime } \left( x ^ { * } \right) - \tau M r _ { k } I _ { n } \leq f ^ { \prime \prime } \left( x ^ { * } + \tau \left( x _ { k } - x ^ { * } \right) \right) \leq f ^ { \prime \prime } \left( x ^ { * } \right) + \tau M r _ { k } I _ { n }
$$

Combining the above inequality with assumption 3 we have

$$
\left( l - \frac { r _ { k } } { 2} M \right) I _ { n } \leq G _ { k } \leq \left( L + \frac { r _ { k } } { 2} M \right) I _ { n }
$$

Hence, $$\left( 1- h _ { k } \left( L + \frac { r _ { k } } { 2} M \right) \right) I _ { n } \leq I _ { n } - h _ { k } G _ { k } \leq \left( 1- h _ { k } \left( l - \frac { r _ { k } } { 2} M \right) \right) I _ { n }$$ and we conclude that

$$
\| I _ { n } - h _ { k } G _ { k } \| \leq \max \left\{ a _ { k } \left( h _ { k } \right) ,b _ { k } \left( h _ { k } \right) \right\}
$$

where $$a_k(h) = 1- h \left( l - \frac { r _ { k } } { 2} M \right)$$ and $$b _ { k } ( h ) = h \left( L + \frac { r _ { k } } { 2} M \right) - 1$$.  

Note that for a small $$h$$, $$\vert b_k(h)\vert < 1$$ and also if $$r_k < \frac{2l}{M} =: \bar{r}$$,   $$a_k(h)$$ is a strictly decreasing function of $$h$$; a small $$h$$ could also ensure $$\vert a_k(h)\vert < 1$$, i.e., 

$$
\| I _ { n } - h _ { k } G _ { k } \| < 1
$$

holds for small $$h_k$$(e.g., $$h_k = 1/L$$). In this case we have $$r_{k+1} < r_k$$. 

To minimize $$\max \left\{ a _ { k } ( h ) ,b _ { k } ( h ) \right\}$$ we have

$$
a _ { k } ( h ) = b _ { k } ( h ) \quad \Leftrightarrow \quad 1- h \left( l - \frac { r _ { k } } { 2} M \right) = h \left( L + \frac { r _ { k } } { 2} M \right) - 1
$$

and get $$h _ { k } ^ { * } = \frac { 2} { L + l }$$.

Under such a choice we obtain

$$
r _ { k + 1} \leq \frac { ( L - l ) r _ { k } } { L + l } + \frac { M r _ { k } ^ { 2} } { L + l }
$$

...

In the end, we will obtain $$\|x_k - x^* \| \leq c \rho^k$$ where $$0 < c, 0 < \rho < 1$$.

**Theorem 1.2.4** Let function $$f(x)$$ satisfy our assumptions and the starting point $$x_0$$ be close enough to a local minimum:

$$
r _ { 0} = \| x _ { 0} - x ^ { * } \| < \overline { r } = \frac { 2l } { M }
$$

Then the gradient methods with step size $$h _ { k } ^ { * } = \frac { 2} { L + l }$$ converges as follows:

$$
\| x _ { k } - x ^ { * } \| \leq \frac { \overline { r } r _ { 0} } { \tilde { r } - r _ { 0} } \left( 1- \frac { 2l } { L + 3l } \right) ^ { k }
$$

This rate of convergence is called linear.

**Remark.** When $$L = l$$,  $$ \left( 1- \frac { 2l } { L + 3l } \right) $$ gets its minimum value and we have the best convergence speed.

#### 1.2.4 Newton method

Consider the quadratic approximation of a function $$f$$ at point $$x_k$$:

$$
\phi ( x_k + \Delta x ) = f \left( x _ { k } \right) + \left\langle f ^ { \prime } \left( x _ { k } \right) ,\Delta x \right\rangle + \frac { 1} { 2} \left\langle f ^ { \prime \prime } \left( x _ { k } \right) \Delta x ,\Delta x \right\rangle
$$

Taking the derivative with $$\Delta x$$ and get:

$$
\phi ( x_k + \Delta x )' = f'(x_k) + f''(x_k)\Delta x
$$

Setting $$\phi ( x_k + \Delta x )' =0$$ to minimize $$\phi(x_k + \Delta x)$$ and get $$\Delta x = [f''(x_k)]^{-1} f'(x_k)$$.

Two drawbacks of Newton method: 

* It can break if $$f''(x_k)$$ is degenerate.
* It can diverge.

For example, let us apply Newton method to find a root of the function of one variable:

$$
\phi ( t ) = \frac { t } { \sqrt { 1+ t ^ { 2} } }
$$

Clearly, $$t^* = 0$$.

The Newton process looks as follows:

$$
t _ { k + 1} = t _ { k } - \frac { \phi \left( t _ { k } \right) } { \phi ^ { \prime } \left( t _ { k } \right) } = t _ { k } - \frac { t _ { k } } { \sqrt { 1+ t _ { k } ^ { 2} } } \cdot \left[ 1+ t _ { k } ^ { 2} \right] ^ { 3/ 2} = - t _ { k } ^ { 3}
$$

Thus, if $$\vert t_0 \vert < 1$$ then this method converges and the convergence speed is extremely fast. The points $$\pm 1$$ are oscillation points. If $$\vert t_0 \vert > 1$$ then the method diverges.

Let us now study the local convergence of the Newton method. Consider the problem

$$
\min_{x \in \mathbb{R}^n} f(x)
$$

under the assumptions:

1. $$f \in C _ { M } ^ { 2,2} \left( \mathbb{R} ^ { n } \right)$$ 

2. There exists a local minimum of function $$f$$ with positive definite Hessian:
   
   $$
   f ^ { \prime \prime } \left( x ^ { * } \right) \geq l I _ { n } ,\quad l > 0
   $$

3. The starting point $$x_0$$ is close enough to $$x^*$$.

Consider the process $$x _ { k + 1} = x _ { k } - \left[ f ^ { \prime \prime } \left( x _ { k } \right) \right] ^ { - 1} f ^ { \prime } \left( x _ { k } \right)$$.

$$
\left.\begin{aligned} x _ { k + 1} - x ^ { * } & = x _ { k } - x ^ { * } - \left[ f ^ { \prime \prime } \left( x _ { k } \right) \right] ^ { - 1} f ^ { \prime } \left( x _ { k } \right) \\ & = x _ { k } - x ^ { * } - \left[ f ^ { \prime \prime } \left( x _ { k } \right) \right] ^ { - 1} \int _ { 0} ^ { 1} f ^ { \prime \prime } \left( x ^ { * } + \tau \left( x _ { k } - x ^ { * } \right) \right) \left( x _ { k } - x ^ { * } \right) d \tau \\
&= \left[ f ^ { \prime \prime } \left( x _ { k } \right) \right] ^ { - 1} G _ { k } \left( x _ { k } - x ^ { * } \right)\end{aligned} \right.
$$

where $$G _ { k } = \int _ { 0} ^ { 1} \left[ f ^ { \prime \prime } \left( x _ { k } \right) - f ^ { \prime \prime } \left( x ^ { * } + \tau \left( x _ { k } - x ^ { * } \right) \right) \right] d \tau$$.

As before let $$r_k := \| x_k - x^* \|$$ we have

$$
\left.\begin{aligned} \| G _ { k } \| & = \left\| \int _ { 0} ^ { 1} \left[ f ^ { \prime \prime } \left( x _ { k } \right) - f ^ { \prime \prime } \left( x ^ { * } + \tau \left( x _ { k } - x ^ { * } \right) \right) \right] d \tau \right\| \\ & \leq \int _ { 0} ^ { 1} \| f ^ { \prime \prime } \left( x _ { k } \right) - f ^ { \prime \prime } \left( x ^ { * } + \tau \left( x _ { k } - x ^ { * } \right) \right) \| d \tau \\
&\leq \int _ { 0} ^ { 1} M ( 1- \tau ) r _ { k } d \tau = \frac { r _ { k } } { 2} M \end{aligned} \right.
$$

In view of Corollary 1.2.2 and assumption 2, we have

$$
f ^ { \prime \prime } \left( x _ { k } \right) \geq f ^ { \prime \prime } \left( x ^ { * } \right) - M r _ { k } I _ { n } \geq \left( l - M r _ { k } \right) I _ { n }
$$

Therefore if $$r_k < \frac{l}{M}$$ then $$f''(x_k)$$ is positive definite and 

$$
\| \left[ f ^ { \prime \prime } \left( x _ { k } \right) \right] ^ { - 1} \| \leq \left( l - M r _ { k } \right) ^ { - 1}
$$

Hence for $$r_k$$ small enough (e.g., $$r_k \leq \frac{2l}{3M}$$), we have

$$
r _ { k + 1} \leq \frac { M r _ { k } ^ { 2} } { 2\left( l - M r _ { k } \right) } \quad \left( < r _ { k } \right)
$$

The rate of convergence of this type is called quadratic.

**Theorem 1.2.5.** Let function $$f$$ satisfy our assumptions. Suppose that the initial starting point $$x_0$$ is close enough to $$x^{*}$$:

$$
\| x _ { 0} - x ^ { * } \| < \overline { r } = \frac { 2l } { 3M }
$$

Then $$\| x _ { k } - x ^ { * } \| < \bar { r }$$ for all $$k$$ and Newton method converges quadratically:

$$
\| x _ { k + 1} - x ^ { * } \| \leq \frac { M \| x _ { k } - x ^ { * } \| ^ { 2} } { 2\left( l - M \| x _ { k } - x ^ { * } \| \right) }.
$$

---

* **Sublinear rate.** The rate is described in terms of a power function of the iteration counter. For example, 

  $$
  r _ { k } \leq \frac { c } { \sqrt { k } }
  $$

* **Linear rate.** The rate is described in terms of an exponential function of the iteration counter. For example,

  $$
  r _ { k } \leq c ( 1- q ) ^ { k }
  $$

* **Quadratic rate.** The rate has a form of a double exponential function of the iteration counter. For example,

  $$
  r _ { k + 1} \leq c r _ { k } ^ { 2}
  $$

Quadratic rate is extremely fast.

### 1.3 First-order methods in nonlinear optimization

