---
layout: post
comments: false
title: "Functional Analysis, Sobolev Spaces"
date: 2021-03-03 00:00:00
tags: foundation book-notes
---

> Functional Analysis, Sobolev Spaces and Partial Differential Equations.

<!--more-->

{: class="table-of-content"}
* TOC
{:toc}

## Chapter 1 The Hahn-Banach Theorems.

### 1.1 The Analytic Form of the Hahn-Banach Theorem

Suppose $$E$$ is a vector space, a functional is a function defined on $$E$$ or on some subspace of $$E$$ with values in $$\mathbb{R}$$. $$E^*$$ denotes the dual space of $$E$$, that is, the space of all continuous linear functionals on $$E$$. The dual norm on $$E^*$$ is defined by

$$
\| f \|_{E^*} = \sup_{\|x\| \leq 1 \\ x \in E} \vert f(x) \vert = \sup_{\|x\| \leq 1 \\ x \in E} f(x).
$$

Given $$f \in E^*$$ and $$x \in E$$ we often write $$ \langle f, x \rangle$$ instead of $$f(x)$$; it reads the scalar product for the duality $$E^*, E$$.

**(Corollary 1.3.)** 

*Proof.* Note that $$G = \{x_0\}$$ and $$f_0$$ is equivalent to $$g$$ on $$G$$, hence,

$$
\begin{aligned}
t \| x_0 \|^2 &= \sup_{x_0 \in G} t \| x_0 \|^2 = \sup_{x_0 \in G} g(t x_0) = \sup_{x_0 \in G} g\left(t \|x_0\| \frac{x_0}{\| x_0 \|}\right) \\
&= t \|x_0\| \sup_{x_0 \in G} g\left(\frac{x_0}{\| x_0 \|}\right) = t \|x_0\| \| g \|_{G^*},
\end{aligned}
$$

which leads to $$\| g \|_{G^*} = \| x_0 \|$$.

$$
\begin{aligned}
\langle f_0, x_0 \rangle = \sup_{x_0 \in G} \langle f_0, x_0 \rangle &= \sup_{x_0 \in G} \| x_0 \| \left\langle f_0, \frac{x_0}{\| x_0 \|} \right\rangle \\
&= \| x_0 \| \sup_{x_0 \in G} \left\langle f_0, \frac{x_0}{\| x_0 \|} \right\rangle \\
&= \| x_0 \| \| g \|_{G^*} = \| x_0 \|^2.
\end{aligned}
$$

**(Corollary 1.4.)** 

*Proof.* 

$$
\sup_{f \in E^* \\ \|f\| \leq 1} \vert \langle f, x \rangle \vert = 
\sup_{f \in E^* \\ \|f\| \leq 1} \|x\| \left\vert \left\langle f, x / {\|x\|} \right\rangle \right\vert \leq \|x\| \sup_{f \in E^* \\ \|f\| \leq 1} \| f \| \leq \| x\|.
$$

### 1.3 The Bidual $$E^{**}$$.

$$J: E \longrightarrow E^{**}$$, given $$x \in E$$ then $$Jx \in E^{**}$$. It is defined as follows: given $$x \in E$$, the map $$f \mapsto \langle f, x \rangle$$ is a continuous linear functional on $$E^*$$; thus it is an element of $$E^**$$, which is denoted by $$Jx$$.

$$
\langle J x, f\rangle_{E^{\star \star}, E^{\star}}=\langle f, x\rangle_{E^{\star}, E} \quad \forall x \in E, \quad \forall f \in E^{\star}
$$

**Remark.** We need both $$f$$ and $$x$$ to define bidual norm.

### 1.4 Theory of Conjugate Convex Functions

For fixed $$x \in E$$, the function $$f \mapsto \langle f, x \rangle - \varphi(x)$$ is convex where $$f \in E^*$$. Let $$T(f) = \langle f, x \rangle - \varphi(x)$$,

$$
\begin{aligned}
T(t f_1 + (1-t) f_2) &= \langle t f_1, x \rangle - \varphi(x) + \langle (1-t) f_2, x \rangle - \varphi(x)\\ 
&= t \langle f_1, x \rangle - \varphi(x) +  (1-t) \langle f_2, x \rangle - \varphi(x)\\
&= t T(f_1) + (1 - t) T(f_2),
\end{aligned}
$$

which is ture for all $$f_1, f_2 \in E^*, t \in (0, 1)$$.

**Proposition 1.10.** 

$$
\begin{aligned}
\Phi([x, \lambda]) &= \Phi([x, 0] + [0, \lambda])\\
&= \Phi([x, 0]) + \lambda \Phi([0, 1])\\
&= \langle f, x \rangle + \lambda k.
\end{aligned}
$$

**Theorem 1.11.**

**Step 1:**

$$
\begin{aligned}
\left\langle \frac{f}{k + \epsilon}, x \right\rangle + \varphi(x) &\geq \frac{\alpha}{k + \epsilon} \Rightarrow \\
\left\langle -\frac{f}{k + \epsilon}, x \right\rangle - \varphi(x) &\leq -\frac{\alpha}{k + \epsilon} \Rightarrow \\
\varphi^*\left(- \frac{f}{k+\epsilon}\right) &\leq - \frac{\alpha}{k + \epsilon}.
\end{aligned}
$$

**Step 2:**

$$
\begin{aligned}
(\bar{\varphi})^*(f) &= \sup_{x \in E}\left\{ \langle f, x \rangle - \bar{\varphi}(x) \right\}\\
&= \sup_{x \in E}\left\{ \langle f + f_0, x \rangle - \bar{\varphi}(x) - \varphi^*(f_0)\right\}\\
&= \varphi^*(f + f_0) - \varphi^*(f_0).
\end{aligned}
$$

$$
\begin{aligned}
(\bar{\varphi})^{**}(x) &= \sup_{f \in E^*}\left\{ \langle f, x \rangle - \bar{\varphi}^*(f) \right\}\\
&= \sup_{f \in E^*}\left\{ \langle f + f_0, x \rangle - \varphi^*(f+f_0) - \langle f_0, x \rangle + \varphi^*(f_0)\right\}\\
&= \varphi^{**}(x) - \langle f_0, x \rangle + \varphi^*(f_0).
\end{aligned}
$$

Example 1.

$$
\begin{aligned}
\varphi^*(f) &= \sup_{x \in E}\left\{ \langle f, x \rangle - \| x \| \right\} \\
&= \sup_{x \in E}\left\{ \|x\| \left(\left\langle f, \frac{x}{\| x \|} \right\rangle - 1\right) \right\}.
\end{aligned}
$$
