---
layout: post
comments: false
title: "The Exponential Family"
date: 2018-01-01 12:00:00
tags: bayes book-notes
---

> The exponential family.

<!--more-->

{: class="table-of-content"}
* TOC
{:toc}

## Definition


$$
p(x\vert\eta) = h(x) \exp\{\langle \eta, t(x) \rangle - a(\eta)\}
$$

$$\eta$$ is known as natural parameter and the function $$\Psi(\eta) = \theta$$ is called the canonical response function, it maps the $$\eta$$ from the natural parameter space to $$\theta$$ in the parameter space. $$a(\eta)$$ is known as the cumulant function which can be viewed as the logarithm of a normalization as we have

$$
a(\eta) = \log \int h(x) \exp\{ \langle \eta, t(x) \rangle \} \text{d}x
$$

$$t(x)$$ is the sufficient statistic for $$\eta$$.



## Examples

### The Bernoulli distribution

$$
\begin{align}
p(x \vert \theta) &= \theta^x (1 - \theta)^{1-x} \\
              &= \exp \left \{\log\left(\frac{\theta}{1 - \theta}\right) x + \log(1 - \theta) \right \}
\end{align}
$$

thus we have

$$
\begin{align}
\eta &= \log\left(\frac{\theta}{1 - \theta}\right) \\
 t(x) &= x \\
 a(\eta) &= -\log(1 - \theta) = \log(1 + e^{\eta}) \\
 h(x) &= 1.
\end{align}
$$


Note that the canonical response function $$\Psi(\eta)$$ is the logistic function

$$
\theta = \frac{1}{1 + e^{-\eta}}.
$$


### The multinomial distribution

$$
p ( x \vert \theta ) = \frac { M ! } { x _ { 1} ! x _ { 2} ! \dots x _ { k } ! } \exp \left\{ \sum _ { k = 1} ^ { K } x _ { k } \log \theta _ { k } \right\}
$$

When $$M$$ is constant the multinomial distribution also falls into the exponential family, and now we only consider the kernel part of the distribution. Note that in such case, $$\sum_{k}^K x_k = M$$.

$$
\left.\begin{aligned} p ( x \vert \theta ) & = \exp \left\{ \sum _ { k = 1} ^ { K } x _ { k } \log \theta _ { k } \right\} \\ & = \exp \left\{ \sum _ { k = 1} ^ { K -1} x _ { k } \log \theta _ { k } + \left( M- \sum _ { k = 1} ^ { K - 1} x _ { k } \right) \log \left( 1- \sum _ { k = 1} ^ { K - 1} \theta _ { k } \right) \right \} \\ & = \exp \left\{ \sum _ { k = 1} ^ { K - 1} \log\left ( \frac { \theta _ { k } } { 1- \sum _ { k = 1} ^ { K - 1} \theta _ { k } } \right) x _ { k } + M\log \left( 1- \sum _ { k = 1} ^ { K - 1} \theta _ { k } \right) \right\} \\
& = \exp \left\{\left \langle \log\left ( \theta/ \theta_K \right), x  \right\rangle + M\log \left( 1- \sum _ { k = 1} ^ { K - 1} \theta _ { k } \right) \right\}
\end{aligned} \right.
$$

then we get

$$
\begin{align}
\eta_k &= \log\left ( \frac { \theta _ { k } } { 1- \sum _ { k = 1} ^ { K - 1} \theta _ { k } } \right) = \log \left( \frac{\theta_k}{\theta_K}\right)  \\
t(x) & = x \\
a(\eta) &= -M \log \left( 1 - \sum_{k=1}^{K-1} \theta_k \right) = M \log\left(\sum_{k=1}^K e^{\eta_k} \right)\\
\end{align}
$$

the canonical response function $$\theta = \Psi (\eta)$$ is the softmax function (also known as the multinomial logit function)

$$
\theta = \frac{e^{\eta}}{\sum_{j=1}^K e^{\eta_j}} = \text{softmax}(\eta).
$$




### The Poisson distribution

$$
p(x\vert\lambda) = \frac{\lambda ^x e^{-\lambda}}{x!} = \frac{1}{x!} \exp\{ x \log \lambda - \lambda \}
$$

we have

$$
\begin{align}
\eta &= \log \lambda \\
t(x) &= x \\
a(\eta) &=  \exp(\eta) \\
h(x) &= \frac{1}{x!}
\end{align}
$$

The canonical response function $$\Psi(\eta)$$ is 

$$
\lambda = e^{\eta}.
$$

### The Gaussian distribution

$$
\begin{align}
p(x \vert \mu, \sigma^2) &= \frac{1}{\sqrt{2\pi} \sigma} \exp\left\{ - \frac{1}{2\sigma^2} (x - \mu)^2 \right\} \\
                                     &= \frac{1}{\sqrt{2\pi} \sigma} \exp\left\{ \frac{\mu}{\sigma^2} x - \frac{1}{2\sigma^2}x^2 
                                                          - \frac{1}{2\sigma^2} \mu^2 - \log\sigma \right\}.
\end{align}
$$

then we have 

$$
\begin{align}
\eta &= \left[
\begin{array}{c}
  \mu / \sigma^2 \\
  - 1 / 2\sigma^2
\end{array}
\right] \\

t(x) &= \left[
\begin{array}{c}
  x \\
  x^2
\end{array}
\right] \\

a(\eta) &= \frac { \mu ^ { 2} } { 2\sigma ^ { 2} } + \log \sigma = - \frac { \eta _ { 1} ^ { 2} } { 4\eta _ { 2} } - \frac { 1} { 2} \log \left( - 2\eta _ { 2} \right) \\

h ( x ) &= \frac { 1} { \sqrt { 2\pi } }.
\end{align}
$$


## Convexity

**Theorem**. The natural parameter space $$\mathcal{N}$$ is convex (convex set) and the cumulant function $$a(\eta)$$ is convex (convex function). If the family is minimal then $$a(\eta)$$ is strictly convex.

Proof. Consider two distinct parameters $$\eta_1 \in \mathcal{N}, \eta_2 \in \mathcal{N}$$ and let $$\eta = \lambda \eta_1 + (1-\lambda) \eta_2$$ for $$0 < \eta < 1$$. Then we have:

$$
\begin{align}
\exp ( a ( \eta ) ) &= \int e ^ { \langle \lambda \eta _ { 1} + ( 1- \lambda ) \eta _ { 2},  t ( x )  \rangle } h ( x )  \text{d} x \quad \text{(Definition of } a(\eta))\\
&= \int e ^ { \langle \lambda \eta _ { 1} ,  t( x ) \rangle } h(x)^{\lambda}   \cdot  e ^ {  \langle (1- \lambda) \eta _ { 2} ,  t( x ) \rangle } h(x)^{1-\lambda} \text{d} x \\
 &\leq \left( \int \left( e ^ { \langle \lambda \eta _ { 1} ,  t( x ) \rangle }  \right) ^ {\frac { 1} { \lambda }} h ( x )  \text{d} x \right) ^ { \lambda }
 \left( \int  \left( e ^ {  \langle (1- \lambda) \eta _ { 2} ,  t( x ) \rangle } \right)^{\frac { 1} { 1- \lambda  }}  h ( x )  \text{d} x \right)^{1-\lambda} \quad \text{(Holder's inequality)}\\
 &= \left( \int e ^ { \langle \eta _ { 1},  t ( x ) \rangle  }  h ( x )  \text{d} x  \right) ^ { \lambda } \left( \int  e ^ { \langle \eta _ { 2},  t ( x ) \rangle }   h ( x ) \text{d} x  \right) ^{1-\lambda}
\end{align}
$$

It shows that the left side is finite if both integrals on the right are finte, thus $$\eta \in \mathcal{N}$$, which implies that $$\mathcal{N}$$

is a convex set.

Taking logarithms on both sides yields:

$$
a \left( \lambda \eta _ { 1} + ( 1- \lambda ) \eta _ { 2} \right) \leq \lambda a \left( \eta _ { 1} \right) + ( 1- \lambda ) a \left( \eta _ { 2} \right)
$$

which establishes the convexity of $$a(\eta)$$. :black_large_square:



## Mean, variance

One desirable property of exponential family is that we can get the expectation and variance of the sufficient statistics by calculating the gradients of the cumulant of function respect to the natural parameters:

$$
\begin{align}
\nabla_\eta a(\eta) &= \mathbb{E}[t(x)] \\
\nabla_\eta^2 a(\eta) &= \text{Var}[t(x)] 
\end{align}
$$

### Examples

Recall that in the case of Bernoulli distribution we have $$a(\eta) = \log (1 + e^\eta)$$.  Taking a first derivative yields:

$$
\frac{d a}{d\eta} = \frac{e^\eta}{1 + e^\eta} = \theta
$$

Taking a second derivative yields:

$$
\frac{d^2 a}{d\eta^2} = \theta (1 - \theta)
$$

Recall the Gaussian distribution $$a(\eta) = - \frac { \eta _ { 1} ^ { 2} } { 4\eta _ { 2} } - \frac { 1} { 2} \log \left( - 2\eta _ { 2} \right)$$ and we have:

$$
\left.\begin{aligned} \mathbb{E}[X] =  \frac { \partial a } { \partial \eta _ { 1} }  = \frac { \eta _ { 1} } { 2\eta _ { 2} } = \frac { \mu / \sigma ^ { 2} } { 1/ \sigma ^ { 2} } = \mu \end{aligned} \right.
$$

and 

$$
\left.\begin{aligned} \text{Var}(X) =  \frac { \partial ^ { 2} a } { \partial \eta _ { 1} ^ { 2} }  = - \frac { 1} { 2\eta _ { 2} }  = \sigma ^ { 2} \end{aligned} \right.
$$

Also note that $$\eta_2$$ corresponds to $$X^2$$ in the sufficient statistics $$t(x)$$ thus we have 

$$
\mathbb{E}[X^2]  = \frac { \partial a } { \partial \eta _ { 2} } = \mu^2 + \sigma^2.
$$

## Sufficiency and maximum likelihood estimates

$$t(x)$$ contains all of the essential information in $$X$$ regarding $$\theta$$.

Suppose that we have a collection of $$N$$ independent random variables sampled from the same exponential family distribution. Taking the product, we obtain the joint density:

$$
p ( x \vert \eta ) = \prod _ { n = 1} ^ { N } h \left( x _ { n } \right) \exp \left\{ \langle \eta,  t \left( x _ { n } \right) \rangle  - a ( \eta ) \right\} = \left( \prod _ { n = 1} ^ { N } h \left( x _ { n } \right) \right) \exp \left\{ \left \langle \eta, \sum _ { n = 1} ^ { N } t \left( x _ { n } \right) \right\rangle - N a ( \eta ) \right\}
$$

For Bernoulli, multinomial, Poisson distributions, it suffices to maintain a single scalar or vector, the sum of the observations. For Gaussian distribution, it suffices to maintain two numbers: the sum $$\sum_{n=1}^N x_n$$ and the sum of squares $$\sum_{n=1}^N x_n^2$$.



Take the logarithm of the joint density we obtain the log-likelihood:

$$
\ell(\eta \vert D) = \log \left( \prod _ { n = 1} ^ { N } h \left( x _ { n } \right) \right) + \left\langle \eta, \sum _ { n = 1} ^ { N } t \left( x _ { n } \right) \right\rangle - N a ( \eta )
$$


Taking the gradient with respect to $$\eta$$ and setting to zero gives:

$$
\nabla _ { \eta } \ell = \sum _ { n = 1} ^ { N } t \left( x _ { n } \right) - N \nabla _ { \eta } a ( \eta ) = 0 \\
\Rightarrow \mathbb{E}[t(x)] = \nabla _ { \eta } a ( \eta ) = \frac { 1} { N } \sum _ { n = 1} ^ { N } t \left( x _ { n } \right)
$$

## KL divergence and maximum likelihood 

To link the KL divergence and maximum likelihood, let us first define the empirical distribution

$$
\tilde { p } ( x ) : = \frac { 1} { N } \sum _ { n = 1} ^ { N } \delta \left( x ,x _ { n } \right)
$$

where $$\delta(\cdot, \cdot)$$ is a Kronecker delta function.

In the discrete case we compute the cross-entropy between the empirical distribution and the model, we obtain the log likelihood scaled by the constant $$1/N$$,

$$
\left.\begin{aligned} \sum_x \tilde { p } ( x ) \log p ( x \vert \theta ) & = \sum _ { x } \frac { 1} { N } \sum _ { n = 1} ^ { N } \delta ( x ,x _ { n } ) \log p ( x \vert \theta ) \\ & = \frac { 1} { N } \sum _ { n = 1} ^ { N } \sum _ { x } \delta ( x ,x _ { n } ) \log p ( x \vert \theta ) \\ & = \frac { 1} { N } \sum _ { n = 1} ^ { N } \log p ( x_n \vert \theta ) \\ & = \frac { 1} { N } \ell ( \theta \vert \mathcal { D } ) \end{aligned} \right.
$$

Let us now calculate the KL divergence between empirical distribution and the model,

$$
\left.\begin{aligned} D ( \tilde { p } ( x ) \| p ( x \vert \theta ) ) & = \sum _ { x } \tilde { p } ( x ) \log \frac { \tilde { p } ( x ) } { p ( x \vert \theta ) } \\ & = \sum _ { x } \tilde { p } ( x ) \log \tilde { p } ( x ) - \sum _ { x } \tilde { p } ( x ) \log p ( x \vert \theta ) \\ & =  \sum _ { x } \tilde { p } ( x ) \log \tilde { p } ( x ) - \frac { 1} { N } \ell ( \theta \vert \mathcal { D } ) \end{aligned} \right.
$$

It shows clearly that minimizing the KL divergence between the empirical distribution and the model is equivalent to maximizing the likelihood.




