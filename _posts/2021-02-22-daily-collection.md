---
layout: post
comments: false
title: "Daily Collection"
date: 2021-02-21 12:00:00
tags: foundation random-topic
---

> Daily Collection.

<!--more-->

{: class="table-of-content"}
* TOC
{:toc}

## Analysis & Probability

#### Limit superior and limit inferior

For a collection of set $$\{E_n\}$$,

$$
\limsup E_n = \bigcap_{k=1}^\infty \bigcup_{n=k}^\infty E_n,
$$

$$
\liminf E_n = \bigcup_{k=1}^\infty \bigcap_{n=k}^\infty E_n.
$$

$$\limsup$$ on set collection means union starting from infinity $$\bigcup_{n \geq N}^\infty$$ and $$\liminf$$ means intersection starting from infinity $$\bigcap_{n \geq N}^\infty$$.

For a sequence of $$\{x_n\}$$,

$$
\limsup x_n = \inf_{k \geq 1} \sup_{n \geq k} x_n,
$$

$$
\liminf x_n = \sup_{k \geq 1} \inf_{n \geq k} x_n.
$$

#### Convergence in probability and distribution

A sequence of random variables $$X_1, X_2, \ldots,$$ converge in probability to a random variable
$$X$$, that is, $$X_n \stackrel{p}{\rightarrow} X$$, if

$$
\lim_{n \rightarrow \infty} \mathbb{P} (\vert X_n - X \vert \geq \epsilon) = 0 \quad \text{for all } \epsilon > 0.
$$

A sequence of random variables $$X_1, X_2, \ldots,$$ converge in distribution to a random variable
$$X$$, that is, $$X_n \stackrel{d}{\rightarrow} X$$, if

$$
\lim_{n \rightarrow \infty} F_{X_n}(x) = F_X(x),
$$

for all $$x$$ at which $$F_X(x)$$ is continuous.

**Example.** Let $$X_1, X_2, \ldots,$$ be a sequence of random variable such that 

$$
X_n \sim \mathrm{Binomial}(n, \frac{\lambda}{n}) \quad \text{for } n \in \mathbb{N}, n > \lambda > 0,
$$

then $$X_n$$ converges in distribution to $$\mathrm{Poisson}(\lambda)$$.

## Linear Algebra & Functional


