---
layout: post
comments: false
title: "Negative Entropy"
date: 2018-12-02 12:00:00
tags: convex foundation random-topic
---


> Negative entropy, its properties, and applications in machine learning.

<!--more-->

{: class="table-of-content"}
* TOC
{:toc}

### Background

**Notation**: Denoting standard simplex in $$\mathbb{R}^n$$ as $$\triangle_n \triangleq \left\{ \boldsymbol { \lambda } \in \mathbb { R } _ { + } ^ { n } : \langle \boldsymbol{1}, \boldsymbol { \lambda } \rangle = 1 \right\}$$.

### Strongly convex property

We consider the negative entropy defined as 

$$
\eta(\mathbf{x}) = \langle \mathbf{x}, \ln \mathbf{x} \rangle, \mathbf{x} \in \triangle_n.
$$

**Theorem 1**. The negative entropy is a strongly convex on $$\triangle_n$$ in the $$\ell_1$$-norm with convexity parameter 1, i.e., for any $$\mathbf{h} \in \mathbb{R}^n$$, the following inequality holds

$$
\left\langle \nabla^2 \eta(\mathbf{x}) \mathbf{h}, \mathbf{h}\right\rangle \leq \| \mathbf{h} \|_1^2.
$$

*Proof*. First note that $$\nabla \eta(\mathbf{x}) = \mathbf{1} + \ln \mathbf{x}$$ and $$\nabla^2 \eta(\mathbf{x}) = \operatorname{diag}(1/\mathbf{x})$$, and thus 

$$
\begin{align}
\left\langle \nabla^2 \eta(\mathbf{x}) \mathbf{h}, \mathbf{h}\right\rangle = \left\langle \mathbf{h}, \frac{\mathbf{h}}{\mathbf{x}} \right\rangle = \sum_{i=1}^n \frac{h_i^2}{x_i}. 
\end{align}
$$

Now we shall find

$$
\min f(\mathbf{x}) \triangleq \left\langle \mathbf{h}, \frac{\mathbf{h}}{\mathbf{x}} \right\rangle \quad \mathrm{s.t.} \quad \langle \mathbf{1}, \mathbf{x} \rangle = 1.
$$

According to the **Proposition 1** the optimal $$\mathbf{x}^*$$ and optimal dual multiplier $$\lambda^*$$ satisfy

$$
\nabla f(\mathbf{x}^*) = -\left(\frac{\mathbf{h}}{\mathbf{x}^*}\right)^2 = \mathbf{1} \lambda^*
\quad \Rightarrow \quad \left(\frac{h_i}{x^*_i}\right)^2 = \lambda^* \quad \Rightarrow \quad x_i^* = \frac{|h_i|}{\sqrt{\lambda^*}}
$$

in the first step, we have absorbed the minus operator into $$\lambda^*$$ since it is a free variable; the second step follows the fact that $$h_i \in \mathbb{R}$$ and $$x_i^* \in \mathbb{R}_+$$.

Taking $$x_i^* = \frac{\vert h_i \vert}{\sqrt{\lambda^*}}$$ along with $$\sum_{i=1}^n x_i^* = 1$$, we get 

$$
f(\mathbf{x}^*) = \left( \sum_{i}^n |h_i| \right)^2 = \| \mathbf{h} \|_1^2.
$$

**Proposition 1**. Let $$\mathbf{x}^*$$ be a local minimum of a differentiable function $$f$$ subject to the linear equality constraints 

$$
\mathbf{x} \in \mathcal{E} = \{ \mathbf{x} \in \mathbb{R}^n | A \mathbf{x} = \mathbf{b} \} \neq \emptyset,
$$

where $$A$$ is an $$m \times n$$-matrix with full row rank, and $$m < n$$. Then there exists a vector of multipliers $$\boldsymbol{\lambda}^*$$ such that

$$
\nabla f(\mathbf{x}^*) = A^\top \boldsymbol{\lambda}^*.
$$

*Proof*. Let $$\mathbf{u}_i \in \mathbb{R}^n, i = 1 \ldots k$$ be the basis of the null space of $$A$$ and $$g(\mathbf{v}) \triangleq \mathbf{x}^* + U \mathbf{v}$$ where $$\mathbf{v} \in \mathbb{R}^k$$, $$U = [\mathbf{u}_1, \ldots, \mathbf{u}_k]$$. We observe that $$g(\mathbf{v}) \in \mathcal{E}$$ for any $$\mathbf{v} \in \mathbb{R}^k$$, and let $$\phi(\mathbf{v}) = f(g(\mathbf{v}))$$. Since $$\phi(0)$$ is a local minimum of $$\phi$$ then 

$$
\nabla \phi(\mathbf{v})|_{\mathbf{v} = 0} = U^\top \nabla f(\mathbf{x}^*) = 0,
$$

i.e., $$\nabla f(\mathbf{x}^*)$$ is orthogonal to the null space of $$A$$, hence $$\nabla f(\mathbf{x}^*)$$ must stay in the row space of $$A$$ which completes the proof.

**Remark**: $$\mathrm{d} \phi(\mathbf{v}) = \mathrm{d} f(g(\mathbf{v})) = \langle \nabla f, \mathrm{d}  g(\mathbf{v}) \rangle = \langle \nabla f, U \mathrm{d}  \mathbf{v} \rangle = \langle U^\top \nabla f,  \mathrm{d}  \mathbf{v} \rangle$$.



### Applications

#### Differentiable dynamic programming

The $$\max$$ operator can be formulated as

$$
\max(\mathbf{x}) = \sup_{\mathbf{q} \in \triangle_+} \langle \mathbf{q}, \mathbf{x} \rangle
$$

However, it is not a smooth operator w.r.t $$\mathbf{x}$$ and we can smooth it with negative entropy to yield

$$
\operatorname{max}_{\eta}(\mathbf{x}) = \sup_{\mathbf{q} \in \triangle_+} \langle \mathbf{q}, \mathbf{x} \rangle - \eta(\mathbf{q})
$$

where the r.h.s is a strongly concave function w.r.t $$\mathbf{q}$$, more details is available in the [paper](https://arxiv.org/abs/1802.03676).

#### Smoothed Wasserstain distance

Recall the Wasserstain distance between two measure $$\mathbf{a}, \mathbf{b}$$:
$$
\mathrm{L}_{C}(\mathbf{a}, \mathbf{b}) \triangleq \min_{P \in U(\mathbf{a}, \mathbf{b})} \langle P, C \rangle
$$
the r.h.s is a convex function w.r.t $$P$$ and we can transform it to a $$\epsilon$$-strongly convex function with negative entropy
$$
\mathrm{L}_{C}^{\epsilon}(\mathbf{a}, \mathbf{b}) \triangleq \min_{P \in U(\mathbf{a}, \mathbf{b})} \langle P, C \rangle + \epsilon \eta(P).
$$







