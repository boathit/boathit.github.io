---
layout: post
comments: false
title: "Introductory Lectures on Convex Optimization"
date: 2017-02-01 12:00:00
tags: convex foundation nesterov-lectures book-notes
---


> Chapter 1 and Chapter 2 of Introductory Lectures on Convex Optimization.

<!--more-->

{: class="table-of-content"}
* TOC
{:toc}

## 0 Notations

$$C_L^{k,p}(Q)$$ the functions that are $$k$$-order differentiable and $$p$$-order gradient Lipschitz continuous on set $$Q$$. 

$$\mathcal{F}_L^{k,p}(Q)$$ the convex functions that are $$k$$-order differentiable and $$p$$-order gradient Lipschitz continuous on set $$Q$$.

$$\mathcal{S}_{\mu, L}^{k,p}(Q)$$ the strongly convex functions that are $$k$$-order differentiable and $$p$$-order gradient Lipschitz continuous on set $$Q$$ and $$\mu$$.

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

since $$\langle A x ,x \rangle = x_1^2 + x_1 x_2 + x_2^2 = (x_1 + x_2)^2 - x_1 x_2 \geq 0 \quad \forall x \in \mathbb{R}^2$$.

2. When $$A$$ is not symmetric, even when its singular values are all nonnegative we cannot conclude that $$\langle A x ,x \rangle \geq 0  \quad \forall x \in \mathbb{R}^n$$ such as 
   
   $$
   A = \left[
   \begin{array}{cc}
     0&1\\
     1&1
   \end{array}
   \right]
   $$
   Its two singular values are $$1.61803$$ and $$ 0.61803$$ but for $$x = [-10, 1]^\top, \langle A x ,x \rangle = -19 < 0$$.

3. In the definition of (semi-) positive definite matrix, we explicitly require the matrix must be symmetric.

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

**Remark.** We can ignore the term $$o \left( \| y - x ^ { * } \| ^ { 2} \right)$$ in the computation of $$\left\langle f ^ { \prime \prime } \left( x ^ { * } \right) \left( y - x ^ { * } \right) ,y - x ^ { * } \right\rangle + o \left( \| y - x ^ { * } \| ^ { 2} \right)$$.

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

Proof. For any $$x, y \in \mathbb{R}^n$$ we have 

$$
\left.\begin{aligned} f ^ { \prime } ( y ) & = f ^ { \prime } ( x ) + \int _ { 0} ^ { 1} f ^ { \prime \prime } ( x + \tau ( y - x ) ) ( y - x ) d \tau \\ & = f ^ { \prime } ( x ) + \left( \int _ { 0} ^ { 1} f ^ { \prime \prime } ( x + \tau ( y - x ) ) d \tau \right) \cdot ( y - x ) \end{aligned} \right.
$$

The first equality holds as $$f'(y) - f'(x) = \int_0^1 \frac{\partial f'(x + \tau (y-x))}{\partial \tau} d\tau$$.

Therefore, if condition  $$\| f ^ { \prime \prime } ( x ) \| \leq L$$ holds then

$$
\left.\begin{aligned} \| f ^ { \prime } ( y ) - f ^ { \prime } ( x ) \| & = \left\| \left( \int _ { 0} ^ { 1} f ^ { \prime \prime } ( x + \tau ( y - x ) ) d \tau \right) \cdot ( y - x ) \right\| \\ & \leq \left\| \int _ { 0} ^ { 1} f ^ { \prime \prime } ( x + \tau ( y - x ) ) d \tau \right\| \cdot \| y - x \| \\ & \leq \int _ { 0} ^ { 1} \| f ^ { \prime \prime } ( x + \tau ( y - x ) ) \| d \tau \cdot \| y - x \| \\ & \leq L \| y - x \| \end{aligned} \right.
$$

On the other hand, if $$\| f ^ { \prime } ( x ) - f ^ { \prime } ( y ) \| \leq L \| x - y \|$$ holds then for any $$s \in \mathbb{R}^n$$ and $$\alpha > 0$$ we have

$$
\left\| \left( \int _ { 0} ^ { \alpha } f ^ { \prime \prime } ( x + \tau s ) d \tau \right) \cdot s  \right\| = \left \| \int_0^{\alpha} \frac{\partial f'(x + \tau s)}{\partial \tau} d\tau \right\| = \| f ^ { \prime } ( x + \alpha s ) - f ^ { \prime } ( x ) \| \leq \alpha L \| s \|
$$

Dividing this inequality by $$\alpha$$ and tending $$\alpha \rightarrow 0$$ we obtain $$\| f ^ { \prime \prime } ( x )  \cdot s\| \leq L\|s\| \quad \forall s \in \mathbb{R}^n$$ which we can conclude $$\| f ^ { \prime \prime } ( x ) \| \leq L$$.

**Lemma 1.2.3** Let $$f \in C_L^{1,1}(\mathbb{R}^n)$$. Then for any $$x, y$$ from $$\mathbb{R}^n$$ we have

$$
\vert f ( y ) - f ( x ) - \left\langle f ^ { \prime } ( x ) ,y - x \right\rangle \vert \leq \frac { L } { 2} \| y - x \| ^ { 2}
$$

Proof: Note that 

$$
\left.\begin{aligned} f ( y ) & = f ( x ) + \int _ { 0} ^ { 1} \left\langle f ^ { \prime } ( x + \tau ( y - x ) ) ,y - x \right) d \tau \\ & = f ( x ) + \left\langle f ^ { \prime } ( x ) ,y - x \right\rangle + \int _ { 0} ^ { 1} \left( f ^ { \prime } ( x + \tau ( y - x ) ) - f ^ { \prime } ( x ) ,y - x \right) d \tau. \end{aligned} \right.
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


## 2 Smooth convex optimization

We have seen that, in general, the gradient method converges only to a stationary point of function $$f$$, which motivates us to find a class of function $$\mathcal{F}$$ such that 

**Assumption 2.1.1** For any $$f \in \mathcal{F}$$ the first-order optimality condition, $$f'(x)=0$$, is sufficient for a point to be a global solution.

We introduce another invariant operation for the hypothetical class $$\mathcal{F}$$.

**Assumption 2.1.2** If $$f_1, f_2 \in \mathcal{F}$$ and $$\alpha, \beta \geq 0$$, then $$\alpha f_1 + \beta f_2 \in \mathcal{F}$$.

Let's introduce one basic element

**Assumption 2.1.3** Any linear function $$f(x) = \alpha + \langle a , x \rangle$$ belongs to $$\mathcal{F}$$.

The reason is obvious, because for a linear function, $$f'(x) = 0$$ implies that it must be a constant function, thus any point in its domain is the optimal solution.


Now let's see what kind of conclusions we can make about our hypothetical class $$\mathcal{F}$$. Consider $$f \in \mathcal{F}$$ and for a fixed point $$x_0 \in \mathbb{R}^n$$ the function

$$
\phi ( y ) = f ( y ) - \left\langle f ^ { \prime } \left( x _ { 0} \right) ,y \right\rangle
$$

is a non-negative linear combination of $$f(y)$$ and linear function $$-\langle f ^ { \prime } \left( x _ { 0} \right) ,y \rangle$$ thus $$\phi \in \mathcal{F}$$. Note that 

$$
\phi ^ { \prime } ( y ) \vert _ { y = x _ { 0} } = f ^ { \prime } \left( x _ { 0} \right) - f ^ { \prime } \left( x _ { 0} \right) = 0
$$

Hence, in view of assumtion 2.1.1 we have for any $$y \in \mathbb{R}^n$$ we have $$\phi ( y ) \geq \phi \left( x _ { 0} \right) = f \left( x _ { 0} \right) - \left\langle f ^ { \prime } \left( x _ { 0} \right) ,x _ { 0} \right\rangle$$ 

Hence we conclude that

$$
f ( y ) \geq f \left( x _ { 0} \right) + \left\langle f ^ { \prime } \left( x _ { 0} \right) ,y - x _ { 0} \right\rangle
$$

This inequality defines the class of differentiable convex functions.

**Defintion 2.1.1** A continuously differentiable function $$f(x)$$ is called convex on $$\mathbb{R}^n$$ ($$f \in \mathcal { F } ^ { 1} \left( \mathbb{R} ^ { n } \right)$$) if for any $$x, y \in \mathbb{R}^n$$ we have 

$$
f ( y ) \geq f ( x ) + \left\langle f ^ { \prime } ( x ) ,y - x \right\rangle
$$

**Theorem 2.1.1** If $$f \in \mathcal { F } ^ { 1} \left( \mathbb{R} ^ { n } \right)$$ and $$f'(x^*) = 0$$ then $$x^*$$ is the global minimum of $$f(x)$$ on $$\mathbb{R}^n$$.

**Lemma 2.1.1** If $$f_1, f_2 \in \mathcal { F } ^ { 1} \left( \mathbb{R} ^ { n } \right)$$ and $$\alpha, \beta \geq 0$$ then function $$f = \alpha f_1 + \beta f_2$$ also belongs to $$\mathcal { F } ^ { 1} \left( \mathbb{R} ^ { n } \right)$$.

**Lemma 2.1.2** If $$f \in \mathcal { F } ^ { 1} \left( \mathbb{R} ^ { m } \right), b \in  \mathbb{R} ^ { m }$$ and $$A : \mathbb{R}^n \rightarrow \mathbb{R}^m$$ then 

$$
\phi ( x ) = f ( A x + b ) \in \mathcal { F } ^ { 1} \left( R ^ { n } \right)
$$

Proof: Let $$x, y \in \mathbb{R}^n$$ and denote $$\bar{x} = Ax + b, \bar{y} = Ay + b$$. We have $$f ( \overline { y } ) \geq f ( \overline { x } ) + \left\langle f ^ { \prime } ( \overline { x } ) ,\overline { y } - \overline { x } \right\rangle$$. Also note that $$\phi(y) = f(\bar{y}), \phi(x) = f(\bar{x})$$, what remains is to show that $$\phi(y) \geq \phi(x) + \langle \phi(x)', y-x \rangle$$. 

:black_medium_square:

We provide several equivalent definitions of convex function.

**Theorem 2.1.2** Continously differentiable function $$f$$ belongs to the class $$\mathcal { F } ^ { 1} \left( \mathbb{R} ^ { n } \right)$$ if and only if for any $$x, y \in \mathbb{R}^n$$ and $$\alpha \in [0, 1]$$ we have

$$
f ( \alpha x + ( 1- \alpha ) y ) \leq \alpha f ( x ) + ( 1- \alpha ) f ( y ).
$$

Proof:  Denote $$x _ { \alpha } = \alpha x + ( 1- \alpha ) y$$ and let $$f \in \mathcal { F } ^ { 1} \left( \mathbb{R} ^ { n } \right)$$. Then 

$$
\left.\begin{array} { l l } { f \left( x _ { \alpha } \right) } & { \leq f ( y ) + \left\langle f ^ { \prime } \left( x _ { \alpha } \right) ,y - x _ { \alpha } \right\rangle } & { = f ( y ) + \alpha \left\langle f ^ { \prime } \left( x _ { \alpha } \right) ,y - x \right\rangle } \\ { f \left( x _ { \alpha } \right) } & { \leq f ( x ) + \left\langle f ^ { \prime } \left( x _ { \alpha } \right) ,x - x _ { \alpha } \right\rangle } & { = f ( x ) - ( 1- \alpha ) \left\langle f ^ { \prime } \left( x _ { \alpha } \right) ,y - x \right\rangle } \end{array} \right.
$$

Multiplying first inequality by $$1 - \alpha$$ and the second one by $$\alpha$$ and adding the results, we get $$f ( \alpha x + ( 1- \alpha ) y ) \leq \alpha f ( x ) + ( 1- \alpha ) f ( y ).$$

Let $$f ( \alpha x + ( 1- \alpha ) y ) \leq \alpha f ( x ) + ( 1- \alpha ) f ( y )$$ be true for all $$x, y \in \mathbb{R}^n$$ and $$\alpha \in [0, 1)$$. Then

$$
\left.\begin{aligned} f ( y ) & \geq \frac { 1} { 1- \alpha } \left[ f \left( x _ { \alpha } \right) - \alpha f ( x ) \right] = f ( x ) + \frac { 1} { 1- \alpha } \left[ f \left( x _ { \alpha } \right) - f ( x ) \right] \\ & = f ( x ) + \frac { 1} { 1- \alpha } [ f ( x + ( 1- \alpha ) ( y - x ) ) - f ( x ) ]
\end{aligned} \right.
$$

using first order Taylor expansion and tending $$ \alpha $$ to 1, we get $$f ( y ) \geq f ( x ) + \left\langle f ^ { \prime } ( x ) ,y - x \right\rangle$$.

**It is worthwhile mentioning that even without the differentiability assumption, the definition still holds.**

**Theorem 2.1.3** Continuously differentiable function $$f$$ belongs to the class $$\mathcal { F } ^ { 1} \left( \mathbb{R} ^ { n } \right)$$ if and only if we have

$$
\left\langle f ^ { \prime } ( x ) - f ^ { \prime } ( y ) ,x - y \right\rangle \geq 0
$$

Proof: $$\mathcal { F } ^ { 1} \left( \mathbb{R} ^ { n } \right) \rightarrow$$ $$\left\langle f ^ { \prime } ( x ) - f ^ { \prime } ( y ) ,x - y \right\rangle \geq 0$$ is straightforward.

Let $$\left\langle f ^ { \prime } ( x ) - f ^ { \prime } ( y ) ,x - y \right\rangle \geq 0$$ hold for all $$x, y \in \mathbb{R}^n$$ and denote $$x _ { \tau } = x + \tau ( y - x )$$. Then

$$
\left.\begin{aligned} f ( y ) & = f ( x ) + \int _ { 0} ^ { 1} \left\langle f ^ { \prime } ( x + \tau ( y - x ) ) ,y - x \right) d \tau \\ & = f ( x ) + \left\langle f ^ { \prime } ( x ) ,y - x \right\rangle + \int _ { 0} ^ { 1} \left\langle f ^ { \prime } \left( x _ { \tau } \right) - f ^ { \prime } ( x ) ,y - x \right\rangle d \tau \\ & = f ( x ) + \left\langle f ^ { \prime } ( x ) ,y - x \right\rangle + \int _ { 0} ^ { 1} \frac { 1} { \tau } \left\langle f ^ { \prime } \left( x _ { \tau } \right) - f ^ { \prime } ( x ) ,x _ { \tau } - x \right) d \tau \\ & \geq f ( x ) + \left\langle f ^ { \prime } ( x ) ,y - x \right\rangle \end{aligned} \right.
$$

Now we consider the twice differentiable function class $$\mathcal { F } ^ {2} \left( \mathbb{R} ^ { n } \right)$$.

**Theorem 2.1.4** Twice continuously differentiable function $$f$$ belongs to $$\mathcal { F } ^ { 2} \left( \mathbb{R} ^ { n } \right)$$ if and only if for any $$x \in \mathbb{R}^n$$ we have 

$$
f''(x) \succeq 0
$$

Proof: Let $$f \in \mathcal { F } ^ { 2} \left( \mathbb{R} ^ { n } \right)$$ and $$x_\tau = x + \tau s$$, $$\tau > 0$$. Then in the view of **Theorem 2.1.3**, we have

$$
\left.\begin{aligned} 0& \leq \frac { 1} { \tau } \left\langle f ^ { \prime } \left( x _ { \tau } \right) - f ^ { \prime } ( x ) ,x _ { \tau } - x \right\rangle = \left\langle f ^ { \prime } \left( x _ { \tau } \right) - f ^ { \prime } ( x ) ,s \right\rangle \\ & = 
  \left\langle \int_0 ^\tau d f^{\prime}(x + \lambda s), s \right\rangle \\
  &= \int_0 ^\tau \langle f ^ { \prime \prime } ( x + \lambda s ) d(x + \lambda s), s \rangle\\
&= \int_0^\tau \left\langle f ^ { \prime \prime } ( x + \lambda s ) s ,s \right\rangle d \lambda \end{aligned} \right.
$$

and tending $$\tau$$ to 0 we get $$f''(x) \succeq 0$$.

Let $$f''(x) \succeq 0$$ hold and $$s = y-x$$. Then

$$
\left.\begin{aligned} f ( y ) & = f ( x ) + \int _ { 0} ^ { 1} \left\langle f ^ { \prime } ( x + \tau ( y - x ) ) ,y - x \right) d \tau \\ & = f ( x ) + \left\langle f ^ { \prime } ( x ) ,y - x \right\rangle + \int _ { 0} ^ { 1} \left\langle f ^ { \prime } \left( x _ { \tau } \right) - f ^ { \prime } ( x ) ,y - x \right\rangle d \tau \\
&= f ( x ) + \left\langle f ^ { \prime } ( x ) ,y - x \right\rangle + \int _ { 0} ^ { 1} \int_0^\tau \left\langle f ^ { \prime \prime } ( x + \lambda s ) s ,s \right\rangle d \lambda d \tau \\
& \geq f(x) + \langle f'(x), y-x \rangle.
\end{aligned} \right.
$$

:black_medium_square:

**Examples of differentiable convex function.** 

According to **Lemma 2.1.2**, $$f ( x ) = \sum _ { i = 1 } ^ { m } e ^ { \alpha _ { i } + \left\langle a _ { i } , x \right\rangle }$$ and $$f ( x ) = \sum _ { i = 1 } ^ { m } \left\vert \left\langle a _ { i } , x \right\rangle - b _ { i } \right\vert ^ { p }$$ are all convex functions.

The differentiability iteself cannot ensure any special topological properties of convex functions. Therefore we need to consider the problem classes with Lipschitz continuous derivatives of a certain order.

**Theorem 2.1.5** All conditions below, holding for all $$x, y \in \mathbb{R}^n$$ and $$\alpha \in [0, 1]$$, are equivalent to inclusion $$f \in \mathcal { F } _ { L } ^ { 1,1 } \left( \mathbb{R} ^ { n } \right)$$:

$$
\text{(1)}\quad 0 \leq f ( y ) - f ( x ) - \left\langle f ^ { \prime } ( x ) , y - x \right\rangle \leq \frac { L } { 2 } \| x - y \| ^ { 2 }
$$

$$
\text{(2)}\quad f ( x ) + \left\langle f ^ { \prime } ( x ) , y - x \right\rangle + \frac { 1 } { 2 L } \left\| f ^ { \prime } ( x ) - f ^ { \prime } ( y ) \right\| ^ { 2 } \leq f ( y )
$$

$$
\text{(3)}\quad \frac { 1 } { L } \left\| f ^ { \prime } ( x ) - f ^ { \prime } ( y ) \right\| ^ { 2 } \leq \left\langle f ^ { \prime } ( x ) - f ^ { \prime } ( y ) , x - y \right\rangle
$$

$$
\text{(4)}\quad \left\langle f ^ { \prime } ( x ) - f ^ { \prime } ( y ) , x - y \right\rangle \leq L \| x - y \| ^ { 2 }
$$

$$
\begin{aligned} \alpha f ( x ) + ( 1 - \alpha ) f ( y ) \geq & f ( \alpha x + ( 1 - \alpha ) y ) \\ & + \frac { \alpha ( 1 - \alpha ) } { 2 L } \left\| f ^ { \prime } ( x ) - f ^ { \prime } ( y ) \right\| ^ { 2 } \end{aligned}
$$

$$
\begin{aligned} \alpha f ( x ) + ( 1 - \alpha ) f ( y ) \leq & f ( \alpha x + ( 1 - \alpha ) y ) \\ & + \alpha ( 1 - \alpha ) \frac { L } { 2 } \| x - y \| ^ { 2 } \end{aligned}
$$

Proof: (1) follows from the definition of convex functions and **Lemma 1.2.3**. To prove (2), let us fix $$x$$ and consider the function

$$
\phi ( y ) = f ( y ) - \left\langle f ^ { \prime } \left( x \right) , y \right\rangle
$$

Note that $$\phi \in \mathcal { F } _ { L } ^ { 1,1 } \left( R ^ { n } \right)$$ and its optimal point is $$y^* = x$$ and in the view of (1), we have

$$
\begin{aligned}
\phi \left( x \right) = \min_{v} \phi(v) \leq \min_{v} \left[ \phi(y) + \langle \phi^{\prime}(y), v - y\rangle + \frac{1}{2} L \| v - y \|^2\right]
\end{aligned}
$$

where the right most function get its minimum at $$v = y - \frac { 1 } { L } \phi ^ { \prime } ( y )$$ and replacing $$v$$ we get (2).

We get (3) from (2) by adding two copies of it with $$x$$ and $$y$$ interchanged.

Applying to (3) Cauchy-Schwartz inequality we get $$\left\| f ^ { \prime } ( x ) - f ^ { \prime } ( y ) \right\| \leq L \| x - y \|$$, the definition of one order Lipschitz continuous gradient, i.e., (1).

We can get (4) from (1) by adding two copies of it with $$x$$ and $$y$$ interchanged.

To get (1) from (4) we apply integration:

$$
\begin{aligned} f ( y ) - f ( x ) - \left\langle f ^ { \prime } ( x ) , y - x \right\rangle & = \int _ { 0 } ^ { 1 } \left\langle f ^ { \prime } ( x + \tau ( y - x ) ) - f ^ { \prime } ( x ) , y - x \right\rangle d \tau \\ & \leq \frac { 1 } { 2 } L \| y - x \| ^ { 2 } \end{aligned}
$$


**Theorem 2.1.6** Two times continuously differentiable function $$f$$ belongs to $$\mathcal { F } _ { L } ^ { 2,1 } \left( R ^ { n } \right)$$ if and only if for any $$x \in R ^ { n }$$ we have 

$$
0 \preceq f ^ { \prime \prime } ( x ) \preceq L I _ { n }.
$$

Proof: According to (4) in **Theorem 2.1.5**,

$$
\left.\begin{aligned} \tau L \|s\|^2 &\geq \frac { 1} { \tau } \left\langle f ^ { \prime } \left( x + \tau s \right) - f ^ { \prime } ( x ) ,x + \tau s - x \right\rangle \\ &= \left\langle f ^ { \prime } \left( x _ { \tau } \right) - f ^ { \prime } ( x ) ,s \right\rangle \\ & = 
  \left\langle \int_0 ^\tau d f^{\prime}(x + \lambda s), s \right\rangle \\
  &= \int_0 ^\tau \langle f ^ { \prime \prime } ( x + \lambda s ) d(x + \lambda s), s \rangle\\
&= \int_0^\tau \left\langle f ^ { \prime \prime } ( x + \lambda s ) s ,s \right\rangle d \lambda \end{aligned} \right.
$$

hence we have

$$
L \|s\|^2 \geq \frac{1}{\tau} \int_0^\tau \left\langle f ^ { \prime \prime } ( x + \lambda s ) s ,s \right\rangle d \lambda
$$

i.e.

$$
f ^ { \prime \prime } ( x ) \preceq L I _ { n }
$$

If we have

$$
f ^ { \prime \prime } ( x ) \preceq L I _ { n }
$$

then

$$
\left.\begin{aligned} L \|y - x\|^2 &\geq \int_0^1 \left\langle f ^ { \prime \prime } ( x + \lambda (y-x) ) (y-x) , y-x \right\rangle d \lambda \\ &=  \left\langle f ^ { \prime } \left(y \right) - f ^ { \prime } ( x ) , y-x \right\rangle.
\end{aligned}\right.
$$


**Remark**: This theorem states  

$$
f ^ { \prime \prime } ( x ) \preceq L I _ { n } \Leftrightarrow \left\langle f ^ { \prime } \left(y \right) - f ^ { \prime } ( x ) , y-x \right\rangle \leq L \|y - x\|^2
$$

Similarly, we could prove

$$
f ^ { \prime \prime } ( x ) \succeq \mu I _ { n } \Leftrightarrow \left\langle f ^ { \prime } \left(y \right) - f ^ { \prime } ( x ) , y-x \right\rangle \geq \mu \|y - x\|^2.
$$

### 2.1.2 Lower complexity bounds for $$\mathcal { F } _ { \boldsymbol { L } } ^ { \infty , \mathbf { 1 } } \left( \boldsymbol { R } ^ { n } \right)$$

**Assumption 2.1.4** An iterative method $$\mathscr{M}$$ generates a sequence of test points $$\{ x_k \}$$ such that 

$$
x_{k} \in x_{0}+\operatorname{Lin}\left\{\nabla f\left(x_{0}\right), \ldots, \nabla f\left(x_{k-1}\right)\right\}, \quad k \geq 1
$$

Consider the following family of quadratic functions with a fixed constant $$L > 0$$

$$
f_k (x) = \frac{L}{4}\left[ \langle x, A_k x \rangle - \langle x, e_1 \rangle \right]
$$

where 

![]({{ '/assets/images/convex-Ak.png' | relative_url }})
{: style="width: 55%;" class="center"}
*Fig. $$A_k$$*
{:.image-caption}

$$A_k$$ is a tri-diagonal matrix. Thus $$0 \preceq \nabla^{2} f_{k}(x) \preceq L I_{n}$$ and $$f_{k}(\cdot) \in \mathscr{F}_{L}^{\infty, 1}\left(\mathbb{R}^{n}\right), 1 \leq k \leq n$$. Therefore, the equation

$$
\nabla f_{k}(x)= 2A_{k} x-e_{1}=0
$$

has unique solution:

$$
\bar{x}_{k}^{(i)}=\left\{\begin{array}{cc}{1-\frac{i}{k+1},} & {i=1 \ldots k} \\ {0,} & {k+1 \leq i \leq n}\end{array}\right.
$$

Hence, the optimal value of the function $$f_k$$ is 

$$
\begin{align}
f_k^* &= \frac{L}{4}\left[ \langle \bar{x}, A_k \bar{x} \rangle - \langle \bar{x}, e_1 \rangle \right] \\
&= \frac{L}{4}\left[ \langle \bar{x}, \frac{1}{2} e_1 \rangle - \langle \bar{x}, e_1 \rangle \right] \\
&= \frac{L}{8}\left(-1+\frac{1}{k+1}\right)
\end{align}
$$

Note that

$$
\sum_{i=1}^{k} i^{2}=\frac{k(k+1)(2 k+1)}{6} \leq \frac{(k+1)^{3}}{3}
$$

Therefore,

$$
\begin{aligned}\left\|\bar{x}_{k}\right\|^{2} &=\sum_{i=1}^{n}\left(\bar{x}_{k}^{(i)}\right)^{2}=\sum_{i=1}^{k}\left(1-\frac{i}{k+1}\right)^{2} \\ &=k-\frac{2}{k+1} \sum_{i=1}^{k} i+\frac{1}{(k+1)^{2}} \sum_{i=1}^{k} i^{2} \\ & \leq k-\frac{2}{k+1} \cdot \frac{k(k+1)}{2}+\frac{1}{(k+1)^{2}} \cdot \frac{(k+1)^{3}}{3}=\frac{1}{3}(k+1) \end{aligned}
$$

Let $$\mathbb{R}^{k, n}=\left\{x \in \mathbb{R}^{n} \vert x^{(i)}=0, k+1 \leq i \leq n\right\}$$. Then for any $$x \in \mathbb{R}^{k, n}$$ we have 

$$
f_{p}(x) = f_{k}(x), \quad p=k, \ldots, n
$$

since $$\langle x, A_p x \rangle \in \mathbb{R}^{k, n}$$ for all $$x \in \mathbb{R}^{k, n}$$.

Now let us fix $$p, k \leq p \leq n$$.

**Lemma 2.1.5** Let $$x_0 = 0$$. Then for any sequence $$\left\{x_{k}\right\}_{k=0}^{p}$$ satisfying the condition 

$$
x_{k} \in \mathscr{L}_{k} \stackrel{\mathrm{def}}{=} \operatorname{Lin}\left\{\nabla f_{p}\left(x_{0}\right), \ldots, \nabla f_{p}\left(x_{k-1}\right)\right\}
$$

we have $$\mathscr{L}_{k} \subseteq \mathbb{R}^{k, n}$$.

*Proof* Since $$x_0 = 0$$, we have $$\nabla f_{p}\left(x_{0}\right)=-\frac{L}{4} e_{1} \in \mathbb{R}^{1, n}$$. Thus $$\mathscr{L}_1 = \mathbb{R}^{1,n}$$. Let $$\mathscr{L}_{k} \subseteq \mathbb{R}^{k, n}$$ for some $$k < p$$. Since $$A_p$$ is tri-diagonal for any $$x \in \mathbb{R}^{k,n}$$ we have $$\nabla f_{p}(x) \in \mathbb{R}^{k+1, n}$$.  Therefore $$\mathscr{L}_{k+1} \subseteq \mathbb{R}^{k+1, n}$$. $$\square$$ 

**Remark**: $$A_p x \in \mathbb{R}^{k+1, n}$$ for $$x \in \mathbb{R}^{k, n}$$ and $$p \geq k$$, just consider the element in $$k+1$$-row and $$k$$-column of $$A_p$$. 

**Corollary 2.1.1** For any sequence $$\{x_k\}_{k=0}^p$$ with $$x_0 = 0$$ and $$x_k \in \mathscr{L}_{k}$$, we have 

$$
f_{p}\left(x_{k}\right) \geq f_{k}^{*}.
$$

*Proof:* Since $$x_k \in \mathscr{L}_{k} \subseteq \mathbb{R}^{k, n}$$ and therefore $$f_{p}\left(x_{k}\right)=f_{k}\left(x_{k}\right) \geq f_{k}^{*}$$.

**Theorem 2.1.7** For any $$k, 1 \leq k \leq \frac{1}{2}(n-1)$$, and any $$x_{0} \in \mathbb{R}^{n}$$ there exists a function $$f \in \mathscr{F}_{L}^{\infty, 1}\left(\mathbb{R}^{n}\right)$$ such that for any first-order method $$\mathscr{M}$$ satisfying **Assumption 2.1.4** we have

$$
\begin{array}{l}{f\left(x_{k}\right)-f^{*} \geq \frac{3 L\left\|x_{0}-x^{*}\right\|^{2}}{32(k+1)^{2}}}, \\ {\left\|x_{k}-x^{*}\right\|^{2} \geq \frac{1}{8}\left\|x_{0}-x^{*}\right\|^{2}},\end{array}
$$

where $$x^*$$ is the minimum of the function $$f$$ and $$f^* = f(x^*)$$.

### 2.1.3 Strongly convex functions

