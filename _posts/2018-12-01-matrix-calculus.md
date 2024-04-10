---
layout: post
comments: false
title: "Matrix Calculus"
date: 2018-12-01 12:00:00
tags: foundation random-topic
---

> Matrix Calculus.

<!--more-->

{: class="table-of-content"}
* TOC
{:toc}

The [pdf-version]({{ '/assets/pdfs/matrix-calculus4ml.pdf' | relative_url }}) is also available.

## Inner product notation

We will use $$\langle \cdot, \cdot \rangle$$ to denote inner product for vector or Frobenius inner product for matrix, and use $$\odot$$ to denote Hadamard product.

For any $$X \in \mathbb{R}^{m \times n}, Y \in \mathbb{R}^{m \times n}, Z \in \mathbb{R}^{m \times n}, a \in \mathbb{R}$$,

* $$\langle X, Y \rangle = \langle Y, X \rangle$$.
* $$\langle a X, Y \rangle = \langle X, a Y \rangle = a \langle X, Y \rangle$$.
* $$\langle X + Z, Y \rangle = \langle X, Y \rangle + \langle Z, Y \rangle$$.
* $$\langle X, Y \odot Z \rangle = \langle X \odot Y, Z \rangle$$.

Suppose $$A \in \mathbb{R}^{m \times \ell_1}, C \in \mathbb{R}^{\ell_1 \times n}, B \in \mathbb{R}^{m \times \ell_2}, D \in \mathbb{R}^{\ell_2 \times n}$$, then we have

$$
\begin{align}
\langle AC, BD \rangle &= \langle B^\top AC, D \rangle = \langle C, A^\top BD \rangle \\
&= \langle A C D^\top, B \rangle = \langle A, B D C^\top \rangle
\end{align}
$$

*Proof*  The first two equations are obvious and the last two equations use the fact that $$\operatorname{tr}(XY) = \operatorname{tr}(YX)$$ holds for any two matrix $$X$$, $$Y$$ such that $$X^\top$$ has the same size with $$Y$$.


In particular when the matrices $$C, D$$ degrade to vector  $$\mathbf{c} \in \mathbb{R}^{\ell_1}, \mathbf{d} \in \mathbb{R}^{\ell_2}$$, we have

$$
\langle A \mathbf{c}, B \mathbf{d} \rangle = \langle B^\top A \mathbf{c}, \mathbf{d} \rangle
= \langle \mathbf{c}, A^\top B \mathbf{d} \rangle,
$$

$$
\langle A \mathbf{c}, B \mathbf{d} \rangle = \langle A \mathbf{c} \mathbf{d}^\top, B \rangle.
$$

Note that the second one states that the inner product and Frobenius inner product are compatible.

## The scalar function

Let us denote $$f = f(\mathbf{x}) \in \mathbb{R}$$ or $$f = f({X}) \in \mathbb{R}$$.

First, let us consider the the case when the input is vector $$\mathbf{x} \in \mathbb{R}^n$$,

$$
df = \sum_{i=1}^n \frac{\partial f}{\partial x_i} dx_i = \left \langle \nabla_{\mathbf{x}}f, d \mathbf{x} \right \rangle.
$$

When the input is the matrix $${X} \in \mathbb{R}^{m, n}$$,

$$
df = \sum_{i=1}^m \sum_{j=1}^n \frac{\partial f}{\partial X_{ij}} d X_{ij} = \left \langle \nabla_X f, d {X} \right\rangle.
$$

### The matrix differentiation rules

* $$d ({X} \pm {Y}) = d {X} \pm d{Y}$$, $$d({X} {Y}) = (d {X}) {Y} + {X} d{Y}$$.
* $$d ({X}^\top) = (d {X})^\top$$, transpose.
* $$d \operatorname{tr}({X}) = \operatorname{tr}(d {X})$$, trace.
* $$d {X} ^ {-1} = - {X}^{-1} (d {X}) {X}^{-1}$$, which can be obtained by differentiating $${X} {X}^{-1} = I$$.
* $$d \vert{X}\vert = \operatorname{tr} (\operatorname{adj}(X) d {X})$$, when $${X}$$ is invertible $$d \vert{X}\vert = \vert{X}\vert\operatorname{tr} ({X}^{-1} d {X})$$.
* $$d({X} \odot {Y})=(d {X}) \odot {Y}+{X} \odot d {Y}$$, Hadamard product.
* $$d \sigma({X}) = \sigma ' ({X}) \odot d {X}$$, where $$\sigma(\cdot)$$ is an element-wise function such as $$\operatorname{sigmoid}$$, $$\sin$$, etc.

where $$\operatorname{adj}(X)$$ is the adjoint matrix of $${X}$$


### Trace Trick

* $$a = \operatorname{tr} (a)$$ for $$a \in \mathbb{R}$$.
* $$\operatorname{tr}(A^\top) = \operatorname{tr}(A)$$, null.
* $$\operatorname{tr} (A \pm B) = \operatorname{tr}(A) \pm \operatorname{tr}(B)$$, null.
* $$\operatorname{tr} (AB) = \operatorname{tr}(BA)$$ where $$A$$ and $$B^\top$$ have the same size.
* $$\operatorname{tr}(A^\top (B \odot C)) = \operatorname{tr}((A \odot B)^\top C)$$ or $$\langle A, B \odot C \rangle = \langle A \odot B, C \rangle$$, $$A,B,C$$ are compatible.



In the following examples, we try to use the matrix differentiation rules and trace trick to write it as

$$
df = \left \langle \nabla_X f, d {X} \right\rangle.
$$


### Examples

#### Example 1

$$f(X) =  \mathbf{a}^\top X \mathbf{b}$$.

Using differentiation rules

$$
\begin{align}
d f &= \mathbf{a}^\top d (X \mathbf{b}) = \mathbf{a}^\top (d X) \mathbf{b}.
\end{align}
$$

Using the trace trick

$$
df = \operatorname{tr} (\mathbf{a}^\top (d X) \mathbf{b}) = \operatorname{tr} (\mathbf{b} \mathbf{a}^\top d X) = \langle \mathbf{a} \mathbf{b}^\top,  d X\rangle.
$$

Hence,

$$
\nabla_X f = \mathbf{a} \mathbf{b}^\top.
$$

#### Example 2

$$f(X) = \mathbf{a}^\top \exp(X \mathbf{b})$$.

Using the differentiation rules

$$
d f = \mathbf{a}^\top d \exp (X \mathbf{b}) =  \mathbf{a}^\top \exp (X \mathbf{b}) \odot d( X \mathbf{b}) = \mathbf{a}^\top \exp (X \mathbf{b}) \odot d(X) \mathbf{b}.
$$

Using the trace trick

$$
\begin{align}
d f &= \operatorname{tr}(\mathbf{a}^\top \exp (X \mathbf{b}) \odot d(X) \mathbf{b}) = \operatorname{tr}((\mathbf{a} \odot \exp (X \mathbf{b}))^\top d(X) \mathbf{b}) = \operatorname{tr}(\mathbf{b} (\mathbf{a} \odot \exp (X \mathbf{b}))^\top dX)\\
&= \left\langle (\mathbf{a} \odot \exp (X \mathbf{b})) \mathbf{b}^\top,  dX \right\rangle.
\end{align}
$$

Hence, 
$$
\nabla_X f = (\mathbf{a} \odot \exp (X \mathbf{b})) \mathbf{b}^\top.
$$

#### Example 3

$$f(X) = \langle Y, MY \rangle$$, $$Y = \sigma(WX)$$.

Using the differentiation rules and trace trick,

$$
\begin{align}
d f &= \langle d Y, M Y \rangle + \langle Y, M d Y \rangle \\
&= \langle M Y, d Y \rangle + \langle M^\top Y,  d Y \rangle \\
&= \langle (M + M^\top) Y, dY \rangle \\
&= \langle (M + M^\top) Y, \sigma'(WX) \odot (W d X) \rangle \\
&= \langle (M + M^\top) Y \odot \sigma'(WX), W d X \rangle \\
&= \left\langle W^\top \left((M + M^\top) Y \odot \sigma'(WX)\right), d X \right\rangle \\
&= \left\langle W^\top \left((M + M^\top) \sigma(WX) \odot \sigma'(WX)\right), d X \right\rangle
\end{align}
$$

Hence,

$$
\nabla_X f = W^\top  \left( (M^\top + M) \sigma(WX) \odot \sigma'(WX)\right).
$$


#### Example 4

$$f(\mathbf{w}) = \langle X \mathbf{w} - \mathbf{y}, X \mathbf{w} - \mathbf{y} \rangle$$.

Using the differentiation rules,

$$
\begin{align}
d f &= \langle d (X \mathbf{w} - \mathbf{y}), X \mathbf{w} - \mathbf{y} \rangle + \langle X \mathbf{w} - \mathbf{y}, d(X \mathbf{w} - \mathbf{y}) \rangle \\
&= 2 \langle X \mathbf{w} - \mathbf{y}, d(X \mathbf{w} - \mathbf{y}) \rangle \\
&= 2 \langle X \mathbf{w} - \mathbf{y}, X d \mathbf{w} \rangle = 2 \langle X^\top (X \mathbf{w} - \mathbf{y}), d \mathbf{w} \rangle. 
\end{align}
$$

Hence, 

$$
\nabla_{\mathbf{w}} f = 2 X^\top (X \mathbf{w} - \mathbf{y}).
$$


#### Example 5

$$f(\Sigma) = \log \vert\Sigma\vert + \frac{1}{N} \sum_{i=1}^N\left\langle\mathbf{x}_i - \boldsymbol{\mu},  \Sigma^{-1}  (\mathbf{x}_i - \boldsymbol{\mu}) \right\rangle$$.

Consider the first term

$$
d \log \vert\Sigma\vert = \vert\Sigma\vert^{-1} d \vert\Sigma\vert = \langle \Sigma^{-1}, d \Sigma\rangle
$$

Consider the second term

$$
\begin{align}
d\frac{1}{N} \sum_{i=1}^N\left\langle\mathbf{x}_i - \boldsymbol{\mu},  \Sigma^{-1}  (\mathbf{x}_i - \boldsymbol{\mu}) \right\rangle &= \frac{1}{N}\sum_{i=1}^N \left\langle\mathbf{x}_i - \boldsymbol{\mu},  d\Sigma^{-1}  (\mathbf{x}_i - \boldsymbol{\mu}) \right\rangle \\
&= \frac{1}{N} \sum_{i=1}^N\left\langle\mathbf{x}_i - \boldsymbol{\mu},  \Sigma^{-1} (d\Sigma) \Sigma^{-1}  (\mathbf{x}_i - \boldsymbol{\mu}) \right\rangle \\
&= \frac{1}{N} \sum_{i=1}^N\left\langle (\mathbf{x}_i - \boldsymbol{\mu})(\mathbf{x}_i - \boldsymbol{\mu})^\top\Sigma^{-1},  \Sigma^{-1} d\Sigma  \right\rangle \\
&= \frac{1}{N} \sum_{i=1}^N \left\langle \Sigma^{-1} (\mathbf{x}_i - \boldsymbol{\mu})(\mathbf{x}_i - \boldsymbol{\mu})^\top\Sigma^{-1},  d\Sigma  \right\rangle.
\end{align}
$$

Let $$S = \frac{1}{N}\sum_{i=1}^N (\mathbf{x}_i - \bar{\mathbf{x}}) (\mathbf{x}_i - \bar{\mathbf{x}})^\top$$ then

$$
d f = \left\langle\Sigma^{-1} - \Sigma^{-1} S \Sigma^{-1}, d \Sigma \right\rangle.
$$

Thus,

$$
\nabla_{\Sigma} f = (\Sigma^{-1} - \Sigma^{-1} S \Sigma^{-1})^\top.
$$


#### Example 6

$$f(\mathbf{w}) = - \langle \mathbf{y}, \log \operatorname{softmax}(X \mathbf{w}) \rangle$$, the cross entropy loss with prediction distribution $$\operatorname{softmax}(X \mathbf{w}) $$ and ground truth distribution $$\mathbf{y}$$.

$$
\begin{align*}
         f(\mathbf{w}) &= - \langle \mathbf{y}, \log \operatorname{softmax}(X \mathbf{w})\\
         &= -\left\langle \mathbf{y}, \log \frac{\exp (X \mathbf{w})}{\langle\mathbf{1}, \exp (X \mathbf{w}) \rangle} \right\rangle \\
         &= -\left\langle \mathbf{y}, X \mathbf{w} - \mathbf{1}\log \langle\mathbf{1}, \exp (X \mathbf{w}) \rangle \right\rangle \\
         &= -\left\langle \mathbf{y}, X \mathbf{w} \right\rangle + \log \langle\mathbf{1}, \exp (X \mathbf{w}) \rangle  \left\langle \mathbf{y}, \mathbf{1}\right\rangle \\
         &= -\left\langle \mathbf{y}, X \mathbf{w} \right\rangle + \log \langle\mathbf{1}, \exp (X \mathbf{w}) \rangle,
     \end{align*}
$$

note that $$\left\langle \mathbf{y}, \mathbf{1}\right\rangle  = 1$$.

$$
\begin{align*}
         d f &= -\left\langle \mathbf{y}, X d \mathbf{w} \right\rangle + \frac{d \langle\mathbf{1}, \exp (X \mathbf{w}) \rangle}{\langle\mathbf{1}, \exp (X \mathbf{w}) \rangle} \\
            &= -\left\langle \mathbf{y}, X d \mathbf{w} \right\rangle + \left\langle \frac{\mathbf{1}}{\langle\mathbf{1}, \exp (X \mathbf{w}) \rangle}, \exp (X \mathbf{w}) \odot d  (X\mathbf{w}) \right\rangle \\
            &= -\left\langle \mathbf{y}, X d \mathbf{w} \right\rangle + \left\langle \frac{\mathbf{1} \odot \exp (X \mathbf{w})}{\langle\mathbf{1}, \exp (X \mathbf{w}) \rangle}, d (X \mathbf{w}) \right\rangle \\
            &= -\left\langle \mathbf{y}, X d \mathbf{w} \right\rangle + \left\langle \frac{\exp (X \mathbf{w})}{\langle\mathbf{1}, \exp (X \mathbf{w}) \rangle}, Xd \mathbf{w} \right\rangle \\
            %&= -\left\langle \mathbf{y} - \frac{\exp (X \mathbf{w})}{\langle\mathbf{1}, \exp (X \mathbf{w}) \rangle}, X d \mathbf{w} \right\rangle \\
            &= -\left\langle X^\top \left(\mathbf{y} - \frac{\exp (X \mathbf{w})}{\langle\mathbf{1}, \exp (X \mathbf{w}) \rangle} \right), d \mathbf{w} \right\rangle
\end{align*}
$$

Hence,

$$
\nabla_{\mathbf{w}} f = X^\top \frac{\exp (X \mathbf{w})}{\langle\mathbf{1}, \exp (X \mathbf{w}) \rangle} - X^\top \mathbf{y}.
$$

#### Example 7

Let $$L = f(Y), Y = WX$$ where $$f$$ maps a matrix to a scalar,

$$
\begin{align}
d L = d f(Y) &= \left\langle \nabla_Y f, d Y \right\rangle \\
&= \left\langle \nabla_Y f, d (W X) \right\rangle \\
&= \left\langle \nabla_Y f, W dX + (dW)X\right\rangle \qquad (\text{using product rule of differentiation})\\
&= \left\langle \nabla_Y f, W dX \right\rangle + \left\langle \nabla_Y f, (dW)X \right\rangle
\end{align}
$$

* $$W$$ is constant, then $$d L = \left\langle \nabla_Y f, W dX \right\rangle = \left\langle W^\top \nabla_Y f, dX \right\rangle$$ thus $$\frac{\partial L}{\partial X} = W^\top \nabla_Y f$$.
* $$X$$ is constant, then $$d L = \left\langle \nabla_Y f, (dW)X \right\rangle = \left\langle \nabla_Y f X^\top, dW \right\rangle$$ thus $$\frac{\partial L}{\partial W} = \nabla_Y f X^\top$$.

## The non-scalar function

We wish to compute the derivative of $$\frac{\partial F}{\partial X}$$, where $$F \in \mathbb{R}^{p \times q}$$ and $$X \in \mathbb{R}^{m \times n}$$. Consider the vector-output function $$\boldsymbol{f}(\mathbf{x}) \in \mathbb{R}^p, \mathbf{x} \in \mathbb{R}^{m}$$, the derivative of $$\boldsymbol{f}$$ w.r.t $$\mathbf{x}$$ can be defined as

$$
\frac{\partial \boldsymbol{f}}{\partial \mathbf{x}}=\left[\begin{array}{cccc}{\frac{\partial f_{1}}{\partial x_{1}}} & {\frac{\partial f_{2}}{\partial x_{1}}} & {\cdots} & {\frac{\partial f_{p}}{\partial x_{1}}} \\ {\frac{\partial f_{1}}{\partial x_{2}}} & {\frac{\partial f_{2}}{\partial x_{2}}} & {\cdots} & {\frac{\partial f_{p}}{\partial x_{2}}} \\ {\vdots} & {\vdots} & {\ddots} & {\vdots} \\ {\frac{\partial f_{1}}{\partial x_{m}}} & {\frac{\partial f_{2}}{\partial x_{m}}} & {\cdots} & {\frac{\partial f_{p}}{\partial x_{m}}}\end{array}\right] \in \mathbb{R}^{m \times p}
$$

which is the transpose of the Jacobian matrix of $$\boldsymbol{f}$$ w.r.t $$\mathbf{x}$$. Then we have

$$
d \boldsymbol{f} = \left \langle \frac{\partial \boldsymbol{f}}{\partial \mathbf{x}}, d \mathbf{x} \right \rangle \in \mathbb{R}^p.
$$

Let us define the vectorization operator as $$\lambda(X) = [X_{11}, X_{12}, \ldots, X_{mn}]^\top \in \mathbb{R}^{mn}$$. Then 

$$
\begin{align}
\frac{\partial F}{\partial X} &= \frac{\partial \lambda(F)}{\partial \lambda(X)} \in \mathbb{R}^{mn \times pq}, \\
\end{align}
$$

and

$$
\lambda (d F) = \left\langle \frac{\partial F}{\partial X}, \lambda(dX) \right\rangle.
$$

---

The content of this post is largely borrowed from this [blog](<https://zhuanlan.zhihu.com/p/24709748>).










