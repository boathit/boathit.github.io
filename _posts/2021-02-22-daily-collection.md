---
layout: post
comments: false
title: "Daily Collection"
date: 2021-02-22 09:00:00
tags: foundation random-topic
---

> This note collects the fundamental concepts, theorems or interesting facts that come into mind in my daily learning. It will be updated frequently and put in top, so its url is not permanent and changing dynamically.

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


#### Polynomial representation and FFT

A polynomial of degree $$n-1$$ can be represented by $$n$$ coefficient $$c_0, c_1, \ldots, c_{n-1}$$ or its evaluated values at $$n$$ distinct points.

$$
\left[
\begin{array}{c}
p(1)\\
p(x_1)\\
p(x_2)\\
\vdots\\
p(x_{n-1})
\end{array}
\right] = \left[ \begin{array}{lllll} 
1& 1& 1& \ldots& 1\\
1& x_1& x_1^2&\ldots &x_1^{n-1}\\
1& x_2& x_2^2&\ldots &x_2^{n-1}\\
\vdots&\vdots & \vdots& &\vdots\\
1& x_{n-1}& x_{n-1}^2 & & x_{n-1}^{n-1}
\end{array} \right] \left[
\begin{array}{c}
c_0\\
c_1\\
c_2\\
\vdots\\
c_{n-1}
\end{array}
\right]
$$

Given two polynomials 

$$
\begin{aligned}
p_1(x) &= a_0 + a_1 x + \ldots + a_{n-1} x^{n-1}, \\
p_2(x) &= b_0 + b_1 x + \ldots + b_{n-1} x^{n-1}.
\end{aligned}
$$ 

The product of $$p_1$$ and $$p_2$$ is a polynomial of $$2n-2$$ degree with $$2n-1$$ coefficients $$c_0, c_1, \ldots, c_{2n-2}$$ and $$c_k = \sum_{i=1}^k a_i b_{k-i}$$. In other words, $$\mathbf{c} = \mathbf{a} \circledast \mathrm{reverse}(\mathbf{b})$$, the convolution of $$a$$ and reverse of $$b$$.

The convolution operation of two length $$n$$ vectors is of complexity $$O(n^2)$$. However, if we could represent $$p_1, p_2$$ using their value representations, we only need to do elementwise multiplication of their values evaluated at $$2n-1$$ distinct points and the result uniquely determines the polynomial $$p_1(x) \cdot p_2(x)$$. In this case, we need to pad $$a$$ and $$b$$ with zeros to be length of $$2n-1$$ like $$[0, \ldots, 0, a_0, \ldots, a_{n-1}]^\top$$ and $$[b_{n-1}, \ldots, b_0, 0, \ldots, 0]^\top$$.

$$ 
\left[
\begin{array}{c}
0\\
\vdots\\
0\\
a_0\\
\vdots\\
a_{n-1}
\end{array}
\right] \odot 
\left[
\begin{array}{c}
b_{n-1}\\
\vdots\\
b_0\\
0\\
\vdots\\
0
\end{array}
\right].
$$

Hence, we could first transform the coefficient representation into the value representation by a linear map, and we can do elementwise product in the value representation to get $$p_1(x) \cdot p_2(x)$$. Then we can get back to the coefficient representation by the inverse linear map. However, the linear map (inverse linear map) itself is very expensive $$O(n^2)$$, so can we do better by carefully choosing the linear map?

The FFT (Fast Fourier Transform) choose the $$n$$ distinct points to be $$\omega^0, \omega^1, \ldots, \omega^{n-1}$$ where $$\omega = e^{2 \pi i/n}$$, that is, the $$n$$ unitary roots in the unit circle and the linear map is defined by matrix

$$
\Omega = \frac{1}{\sqrt{n}}\left[\begin{array}{lllll} 
1& 1& 1& \ldots& 1\\
1& \omega^ {1 \cdot 1} & \omega^{1\cdot 2}&\ldots &\omega^{1\cdot (n-1)}\\
1& \omega^{2 \cdot 1}& \omega^{2 \cdot 2}&\ldots &\omega^{2\cdot (n-1)}\\
\vdots&\vdots & \vdots& &\vdots\\
1& \omega^{(n-1) \cdot 1}& \omega^{(n-1) \cdot 2} & & \omega^{(n-1)\cdot (n-1)}
\end{array} \right]
$$

where we put a normalization factor $$\frac{1}{\sqrt{n}}$$ such that $$\Omega$$ is a unitary matrix, $$\Omega \Omega^* = I$$.

Note the origin-symmetry pairs $$(\omega^k, \omega^{k + n/2})$$ in the unit circle,

$$
- \omega^k = e^{2\pi i k / n  + \pi i} = e^{2\pi i (k + n/2)/n} = \omega^{k + n/2}.
$$

FFT exploits such symmetry by reusing the intermediate computed results to accelerate the transformation. 

#### Hermitian and Spectral Theorem

(**Definition.**) Let $$V$$ be a finite-dimensional vector space, and $$A: V \rightarrow V$$ be a linear operator. The adjoint of $$A$$ is the linear operator $$A^*: V \rightarrow V$$ characterized by

$$
\langle A^* v, w \rangle = \langle v, A w \rangle \quad \forall v, w \in V.
$$

(**Proposition.**) The adjoint of $$A^*$$ is $$A$$.

*Proof:* By the definition of adjoint $$\langle A^{**} v, w \rangle = \langle v, A^{*} w \rangle$$, thus it is sufficient to show $$\langle v, A^{*} w \rangle = \langle A v, w \rangle$$.

$$
\begin{aligned}
\langle v, A^{*} w \rangle &= \overline{\langle A^{*} w, v \rangle} \quad \text{(conjugate symmetric)}\\
&= \overline{\langle w, A v \rangle} \quad (\text{definition of } A^*)\\
&= \langle A v, w \rangle. 
\end{aligned}
$$

Proof is completed. □

(**Definition.**) A linear operator $$A$$ is called self-adjoint or Hermitian if $$A^* = A$$.

(**Proposition.**) The eigenvalues of Hermtian are real and the eigenvectors corresponding to different eigenvalues are orthogonal.

*Proof:* Suppose $$\mathbf{v}$$ is the eigenvector corresponds to eigenvalue $$\lambda$$,

$$
\begin{aligned}
\lambda \| \mathbf{v} \|_2^2 &= \langle v, A v \rangle \\
&= \langle A v, v \rangle \quad (A \text{ is Hermitian})\\
&= \overline{\langle v, A v \rangle} \quad (\text{conjugate symmetric})\\
&= \overline{\lambda \| \mathbf{v} \|_2^2} \\
&= \overline{\lambda}  \| \mathbf{v} \|_2^2,
\end{aligned}
$$

thus we conclude $$\lambda$$ is real.

Now suppose $$A \mathbf{v}_1 = \lambda_1  \mathbf{v}_1$$, $$A \mathbf{v}_2 = \lambda_2  \mathbf{v}_2$$, and $$\lambda_1 \ne \lambda_2$$,

$$
\begin{aligned}
\lambda_1 \lambda_2 \langle \mathbf{v}_1, \mathbf{v}_2 \rangle &= \langle A  \mathbf{v}_1, A  \mathbf{v}_2 \rangle \\
&=  \langle \mathbf{v}_1, A^* A  \mathbf{v}_2 \rangle \\
&=  \langle \mathbf{v}_1, A^2  \mathbf{v}_2 \rangle \\
&=  \langle \mathbf{v}_1, \lambda_2^2  \mathbf{v}_2 \rangle
\end{aligned} \\ 
\Rightarrow 
(\lambda_1 \lambda_2 - \lambda_2^2) \langle \mathbf{v}_1, \mathbf{v}_2 \rangle = 0  \Rightarrow  \langle \mathbf{v}_1, \mathbf{v}_2 \rangle = 0.
$$

The proof is completed. □

(**Spectral Theorem.**) The Hermitian operator $$A$$ has a complete set of orthonormal eigenvectors. Each eigenvalue is real.

*Proof:* [ntu-note](http://spms.ntu.edu.sg/MathematicalSciences/Seminars/Documents/adams-lecture.pdf). □

#### Projector

![]({{ '/assets/images/2021-02-26-projector.png' | relative_url }})
{: style="width: 65%;" class="center"}
*Figure Projector*
{:.image-caption}

(**Proposition.**) Suppose $$P$$ is a projector that projects $$\mathbf{v}$$ onto $$P \mathbf{v}$$ along $$(I-P)\mathbf{v}$$. Let $$S_1 = \{P \mathbf{v} \mid \mathbf{v} \in \mathbb{C}^m\}$$ and $$S_2 = \{(I - P) \mathbf{v} \mid \mathbf{v} \in \mathbb{C}^m\}$$. Then $$P$$ seperates $$\mathbb{C}^m$$ into two space $$S_1$$ and $$S_2$$ such that $$S_1 \bigcap S_2 = \{0\}$$ and

$$
\begin{aligned}
S_1 &= \mathrm{range}(P) = \mathrm{null}(I - P),\\
S_2 &= \mathrm{range}(I - P) = \mathrm{null}(P).
\end{aligned}
$$

*Proof*: We shall prove $$\mathrm{range}(I-P)=\mathrm{null}(P)$$ as follows.

First, for any $$\mathbf{v} \in \mathrm{null}(P)$$ we have $$\mathbf{v} = \mathbf{v} - P \mathbf{v} = (I-P)\mathbf{v} \in \mathrm{range}(I-P)$$. Hence, $$\mathrm{null}(P) \subseteq \mathrm{range}(I-P)$$.

Next, for any $$(I - P) \mathbf{v} \in \mathrm{range}(I - P)$$ we have $$P(I-P)\mathbf{v} = (P - P^2) \mathbf{v} = 0$$. Thus, $$\mathrm{range}(I-P) \subseteq \mathrm{null}(P)$$.

Now, by writing $$P = I - (I - P)$$ we can obtain $$\mathrm{range}(P) = \mathrm{null}(I - P)$$.

$$\mathrm{null}(P) \bigcap \mathrm{null}(I - P) = \{0\}$$ because for any $$\mathbf{v} \in \mathbb{C}^m$$ in both of them satisfies 

$$
\mathbf{v} = \underbrace{\mathbf{v} - P \mathbf{v}}_{\text{in null}(P)} = \underbrace{ (I - P) \mathbf{v}}_{\text{in null}(I - P)} = 0.
$$

Proof is completed. □

(**Proposition.**) The eigenvalues of the projector is either 0 or 1.

*Proof:* Suppose $$P$$ is a projector and let $$\lambda$$ be its eigenvalue and $$\mathbf{v}$$ be the corresponding eigenvector then 

$$
\begin{array}{lll}
P \mathbf{v} &= \lambda \mathbf{v}  &\Rightarrow P^2 \mathbf{v} &= \lambda P \mathbf{v} \Rightarrow\\ 
P \mathbf{v} &= \lambda^2 \mathbf{v} &\Rightarrow \lambda \mathbf{v} &= \lambda^2 \mathbf{v}
\end{array}
$$

If $$\mathbf{v} \ne 0$$ then $$\lambda = 0$$ or $$\lambda = 1$$. □

