---
layout: post
comments: false
title: "Conjugate Prior"
date: 2018-02-01 12:00:00
tags: bayes book-notes
---

> Conjugate Prior.

<!--more-->

{: class="table-of-content"}
* TOC
{:toc}

## Bernoulli distribution and Beta prior

The likelihood function

$$
p(x \vert \theta) = \theta^x (1 - \theta)^{1-x}
$$

The beta distribution is parameterized using $$\alpha_i -1$$

$$
p(\theta \vert \alpha) = K(\alpha) \theta^{\alpha_1 -1} (1- \theta)^{\alpha_2-1}
$$

where the normalization factor $$K(\alpha)$$ can be obtained analytically:

$$
\left.\begin{aligned} K ( \alpha ) & = \left( \int \theta ^ { \alpha _ { 1} - 1} ( 1- \theta ) ^ { \alpha _ { 2} - 1} d \theta \right ) ^ { - 1} \\ & = \frac { \Gamma ( \alpha _ { 1} + \alpha _ { 2} ) } { \Gamma ( \alpha _ { 1} ) \Gamma ( \alpha _ { 2} ) } \end{aligned} \right.
$$

Consider in particular $$N$$ i.i.d Bernoulli observations, $$x = (x_1, \ldots, x_N)^\top$$:

$$
\begin{align}
p ( \theta \vert \mathbf { x } ,\alpha ) &\propto \left( \prod _ { n = 1} ^ { N } \theta ^ { x _ { n } } ( 1- \theta ) ^ { 1- x _ { n } } \right) \theta ^ { \alpha _ { 1} - 1} ( 1- \theta ) ^ { \alpha _ { 2} - 1} \\
&= \theta^ {\sum _ { n = 1} ^ { N } x _ { n } + \alpha _ { 1} - 1} ( 1- \theta ) ^ { N - \sum _ { n = 1} ^ { N } x _ { n } + \alpha _ { 2} - 1}.
\end{align}
$$

In particular, it is a $$\text{Beta}(\theta \vert \sum _ { n = 1} ^ { N } x _ { n } + \alpha _ { 1},  N - \sum _ { n = 1} ^ { N } x _ { n } + \alpha _ { 2})$$.

$$
\mathbb{E}[\theta \vert \alpha] = \int \theta \cdot \text{Beta}(\theta \vert \alpha_1, \alpha_2) \text{d}\theta = \frac{\alpha_1}{\alpha_1 + \alpha_2}.
$$

A similar calculation ($$\mathbb{E}[\theta^2 \vert \alpha]$$) yields the variance:

$$
\operatorname{Var} [ \theta \vert \alpha ] = \frac { \alpha _ { 1} \alpha _ { 2} } { \left( \alpha _ { 1} + \alpha _ { 2} + 1\right) \left( \alpha _ { 1} + \alpha _ { 2} \right) ^ { 2} }.
$$

Applying the results to $$\text{Beta}(\theta \vert \sum _ { n = 1} ^ { N } x _ { n } + \alpha _ { 1},  N - \sum _ { n = 1} ^ { N } x _ { n } + \alpha _ { 2})$$ we obtain

$$
\begin{align}
\mathbb { E } [ \theta \vert \mathbf { x } ,\alpha ] &= \frac { \sum _ { n = 1} ^ { N } x _ { n } + \alpha _ { 1} } { N + \alpha _ { 1} + \alpha _ { 2} } \\
\operatorname{Var} [ \theta \vert \mathbf { x } ,\alpha ] &= \frac { \left( \sum _ { n = 1} ^ { N } x _ { n } + \alpha _ { 1} \right) \left( N - \sum _ { n = 1} ^ { N } x _ { n } + \alpha _ { 2} \right) } { \left( N + \alpha _ { 1} + \alpha _ { 2} + 1\right) \left( N + \alpha _ { 1} + \alpha _ { 2} \right) ^ { 2} }.
\end{align}
$$

The predictive probability of a new data point $$X$$ is calculated as follows:

$$
p(X = 1\vertx, \alpha) = \int p(X=1 \vert \theta) p(\theta \vert x, \alpha) \text{d} \theta = \int \theta p(\theta \vert x, \alpha) \text{d} \theta = \mathbb{E}[\theta \vert x, \alpha].
$$

## Categorical distribution and Dirichlet prior

In this case the likelihood takes the form

$$
p ( x \vert \theta ) = \theta _ { 1} ^ { x _ { 1} } \theta _ { 2} ^ { x _ { 2} } \dots \theta _ { K } ^ { x _ { K } }
$$

where $$x_k \in \{0, 1\}, \sum_k x_k = 1$$.

This yields the conjugate prior:

$$
p ( \theta \vert \alpha ) = K ( \alpha ) \theta _ { 1} ^ { \alpha _ { 1} - 1} \theta _ { 2} ^ { \alpha _ { 2} - 1} \dots \theta _ { K } ^ { \alpha _ { K } - 1}
$$

where $$\alpha_i > 0$$ and

$$
K ( \alpha ) = \frac { \Gamma \left( \sum _ { k = 1} ^ { K } \alpha _ { k } \right) } { \prod _ { k = 1} ^ { K } \Gamma \left( \alpha _ { k } \right) }
$$

The posterior distribution of $$N$$ i.i.d observations, $$x = (x_1, \ldots, x_N)^\top$$

$$
p ( \theta \vert x ,\alpha ) \quad \propto \quad \theta _ { 1} ^ { \sum _ { n = 1} ^ { N } x _ { n 1} + \alpha _ { 1} - 1} \theta _ { 2} ^ { \sum _ { n = 1} ^ { N } x _ { n 2} + \alpha _ { 2} - 1} \ldots \theta _ { K } ^ { \sum _ { n = 1} ^ { N } x _ { n K } + \alpha _ { K } - 1}
$$

which is $$\text{Dir} (\theta \vert \sum_{n} x_n + \alpha)$$ and note that $$x_n = (x_{n1}, \ldots, x_{nK})$$.

The mean and variance are 

$$
\begin{align}
\mathbb { E } \left[ \theta _ { i } \vert \alpha \right] &= \frac { \alpha _ { i } } { \alpha. } \\
\operatorname{Var} \left[ \theta _ { i } \vert \alpha \right] &= \frac { \alpha _ { i } \left( \alpha .- \alpha _ { i } \right) } { \alpha. ^ { 2} ( \alpha. + 1) }
\end{align}
$$

where $$\alpha. = \sum_{i} \alpha_i$$.



## Poisson distribution and Gamma prior

Consider the Poisson distribution:

$$
p ( x \vert \theta ) = \frac { \theta ^ { x } e ^ { - \theta } } { x ! }
$$

The corresponding conjugate prior retains the shape of the likelihood:

$$
p(\theta \vert \alpha) = K(\alpha) \theta^{\alpha_1 - 1} e^{-\alpha_2 \theta}
$$

where the normalization factor $$K(\alpha) = \frac{\alpha_2 ^{\alpha_1}}{\Gamma(\alpha_1)}$$.

The posterior distribution of $$N$$ i.i.d Poisson observations is:

$$
p(\theta \vert \alpha) \propto \theta^{\alpha_1 -1 + \sum_{n=1}^N x_n} e^{-(N+\alpha_2) \theta}
$$

which is $$\text{Gamma}(\theta \vert \alpha_1 + \sum_n x_n, \alpha_2 + N)$$.

The mean and variance are readily computed as:

$$
\begin{align}
\mathbb { E } [ \theta \vert \alpha ] &= \frac { \alpha _ { 1} } { \alpha _ { 2} } \\
\operatorname{Var} \left[ \theta _ { i } \vert \alpha \right] &= \frac { \alpha _ { 1} } { \alpha _ { 2} ^ { 2} }
\end{align}
$$

In the distribution $$\text{Gamma}(\theta \vert \alpha_1, \alpha_2)$$, $$\alpha_1$$ is known as the shape parameter and $$\alpha_2$$ is called the rate parameter ($$1/\alpha_2$$ is called the scale parameter).

## Univariate Gaussian distribution and Normal-Gamma Priors



Recall that the Gaussian distribution is a two-parameter exponential family of the following form:

$$
p(x \mid \mu, \sigma^2) \propto (\sigma^2)^{-1/2} \exp\left\{- \frac { 1} { 2\sigma ^ { 2} } \left( x - \mu \right) ^ { 2} \right\}
$$

### Conjugacy for the mean

Inspecting the above density form we see that the exponent is the negative of a quadratic form in $$\mu$$. Thus we assume

$$
p \left( \mu \vert \mu _ { 0} ,\sigma _ { 0} ^ { 2} \right) \propto \left( \sigma _ { 0} ^ { 2} \right) ^ { - 1/ 2} \exp \left\{ - \frac { 1} { 2\sigma _ { 0} ^ { 2} } \left( \mu - \mu _ { 0} \right) ^ { 2} \right\}
$$

where $$\mu_0$$ and $$\sigma_0^2$$ are the mean and variance of $$\mu$$.

Before deriving the posterior distribution $$p(\mu \vert x)$$, let us introduce some useful properties of Gaussian variable.

$$
\begin{align}
\mathbb { E } \left[ Z _ { 1} \vert Z _ { 2} \right] &= \mathbb { E } \left[ Z _ { 1} \right] + \frac { \operatorname{Cov} \left[ Z _ { 1} ,Z _ { 2} \right] } { \operatorname{Var} \left[ Z _ { 2} \right] } \left( Z _ { 2} - \mathbb { E } \left[ Z _ { 2} \right] \right) \\
\operatorname{Var} \left[ Z _ { 1} \vert Z _ { 2} \right] &= \operatorname{Var} \left[ Z _ { 1} \right] - \frac { \operatorname{Cov} ^ { 2} \left[ Z _ { 1} ,Z _ { 2} \right] } { \operatorname{Var} \left[ Z _ { 2} \right] }
\end{align}
$$

Consider that we only have one observation $$x$$, we reparameterize $$X$$ and $$\mu$$ using the Normal random variable $$\epsilon, \delta \sim N(0, 1)$$ as

$$
\left.\begin{array} { l } { X = \mu + \sigma \epsilon } \\ { \mu = \mu _ { 0} + \sigma _ { 0} \delta } \end{array} \right.
$$

We can now easily calculate:

$$
\begin{align}
\mathbb{E}[X] &= \mathbb{E}[\mu] + \sigma\mathbb{E}[\epsilon] = \mu_0 \\
\mathbb{V}[X] &= \mathbb{V}[\mu] + \sigma^2 \mathbb{V}[\epsilon] = \sigma^2_0 + \sigma^2 \\
\text{Cov}(X, \mu) &= \mathbb { E } \left[ \left( X - \mu _ { 0} \right) \left( \mu - \mu _ { 0} \right) \right] = \mathbb { E } \left[ \left( \mu + \sigma \epsilon - \mu _ { 0}  \right) \left( \mu - \mu _ { 0} \right) \right] = \sigma _ { 0} ^ { 2}
\end{align}
$$

Treating $$\mu$$ as $$Z_1$$ and $$X$$ as $$Z_2$$ we obtain:

$$
\begin{align}
\mathbb{E}[\mu\vert X = x ] &= \mu _ { 0} + \frac { \sigma _ { 0} ^ { 2} } { \sigma ^ { 2} + \sigma _ { 0} ^ { 2} } \left( x - \mu _ { 0} \right) = \frac { \sigma _ { 0} ^ { 2} } { \sigma ^ { 2} + \sigma _ { 0} ^ { 2} } x + \frac { \sigma ^ { 2} } { \sigma ^ { 2} + \sigma _ { 0} ^ { 2} } \mu _ { 0} \\
\text{Var}(\mu \vert X=x) &= \sigma _ { 0} ^ { 2} - \frac { \sigma _ { 0} ^ { 4} } { \sigma ^ { 2} + \sigma _ { 0} ^ { 2} } = \frac { \sigma ^ { 2}} { \sigma ^ { 2} + \sigma _ { 0} ^ { 2} } \sigma _ { 0} ^ { 2}.
\end{align}
$$

We can also express the results in terms of the precision. In particular, plugging $$\tau = 1/\sigma^2$$ and $$\tau_0 = 1 / \sigma_0^2$$ into the above equation yields:

$$
\begin{align}
\mathbb { E } [ \mu \vert X = x ] = \frac { \tau } { \tau + \tau _ { 0} } x + \frac { \tau _ { 0} } { \tau + \tau _ { 0} } \mu _ { 0}
\end{align}
$$

The posterior expectation is a convex combination of the observation $$x$$ and prior mean $$\mu_0$$. More precisely, the precision of the data multiplies the data $$x$$ and the precision of the prior multiplies the prior mean $$\mu_0$$. If the precision of the data is large relative to the precision of the prior, then the posterior mean is closer to $$x$$. Also note that the posterior precision $$\tau_\text{post} = \tau + \tau_0$$, which has a direct interpretation: precision add.

Let us now consider the posterior distribution in the case of involving multiple observed data points, we have:

$$
p ( \mathbf { x } \vert \mu ,\tau ) \propto \tau ^ { n/ 2} \exp\left \{ - \frac { \tau } { 2} \sum _ { i = 1} ^ { n } \left( x _ { i } - \mu \right) ^ { 2} \right\}
$$

We rewrite the exponent using a standard trick:

$$
\left.\begin{aligned} \sum _ { i = 1} ^ { n } \left( x _ { i } - \mu \right) ^ { 2} & = \sum _ { i = 1} ^ { n } \left( x _ { i } - \overline { x } + \overline { x } - \mu \right) ^ { 2} \\ &= \sum _ { i = 1} ^ { n } \left( x _ { i } - \overline { x } \right) ^ { 2} + n ( \overline { x } - \mu ) ^ { 2} \end{aligned} \right.
$$

The first term yields a constant factor, and we see the problem reduces to an equivalent problem involving only single random variable (treating $$\bar{x}$$ as random variable), in which $$\bar{X} \sim N(\mu, \sigma^2/n)$$.

$$
\bar{X} = \mu + \frac{\sigma^2}{n} \epsilon
$$

and we have 

$$
\begin{align}
\mathbb{E}[\mu\vertx_1,\ldots,x_n ] &=  \frac { \sigma _ { 0} ^ { 2} } { \sigma ^ { 2}/n + \sigma _ { 0} ^ { 2} } \bar{x} + \frac { \sigma ^ { 2} } { \sigma ^ { 2} / n + \sigma _ { 0} ^ { 2} } \mu _ { 0} \\
\text{Var}(\mu \vert x_1,\ldots,x_n) &= \frac { \sigma ^ { 2}/n} { \sigma ^ { 2}/n + \sigma _ { 0} ^ { 2} } \sigma _ { 0} ^ { 2}.
\end{align}
$$

$$\tau_\text{post} = n\tau + \tau_0$$.

Now consider an unseen data point $$Y \sim N(\mu, \sigma^2)$$, we reparameterize it as 

$$
\begin{align}
Y &= \mu + \sigma \epsilon \\
\mu &= \mu_\text{post} + \sigma_\text{post}\delta
\end{align}
$$

$$Y$$ again is a Gaussian random variable as it is the sum of two Gaussian random variables $$\mu$$ and $$\epsilon$$, its predictive mean and variance:

$$
\begin{align}
\mathbb{E}[Y] &= \mu_\text{post} \\
\text{Var}(Y) &= \sigma^2_\text{post} + \sigma^2.
\end{align}
$$


### Conjugacy for the variance

Let us now consider the Gaussian distribution with known mean $$\mu$$,

$$
p \left( \mathbf { x } \vert \mu ,\sigma ^ { 2} \right) \propto \left( \sigma ^ { 2} \right) ^ { a } e ^ { - b / \sigma ^ { 2} }
$$
where $$a = -1/2$$ and $$b = \frac { 1} { 2} \sum _ { i = 1} ^ { n } \left( x _ { i } - \mu \right) ^ { 2}$$. This has the flavor of the gamma distribution, but the random variable $$\sigma^2$$ is in the denominator. This is actually an inverse gamma distribution. We thus assume the prior distribution for the variance is an inverse gamma distribution:

$$
p \left( \sigma ^ { 2} \vert \alpha ,\beta \right) = \frac { \beta ^ { \alpha } } { \Gamma ( \alpha ) } \left( \sigma ^ { 2} \right) ^ { - \alpha - 1} e ^ { - \beta / \sigma ^ { 2} }
$$

The posterior distribution:

$$
\begin{align}
p \left( \sigma ^ { 2} \vert \mathbf { x } ,\mu ,\alpha ,\beta \right) \quad & \propto \quad p \left( \mathbf { x } \vert \mu ,\sigma ^ { 2} \right) p \left( \sigma ^ { 2} \vert \alpha ,\beta \right) \\
&\propto \quad \left( \sigma ^ { 2} \right) ^ { - n / 2} e ^ { - \frac { 1} { 2} \sum _ { i = 1} ^ { n } \left( x _ { i } - \mu \right) ^ { 2} / \sigma ^ { 2} } \left( \sigma ^ { 2} \right) ^ { - \alpha - 1} e ^ { - \beta / \sigma ^ { 2} }\\
&= \quad \left( \sigma ^ { 2} \right) ^ { - \left( \alpha + \frac { n } { 2} \right) - 1} e ^ { - \left( \beta + \frac { 1} { 2} \sum _ { i = 1} ^ { n } \left( x _ { i } - \mu \right) ^ { 2} \right) / \sigma ^ { 2} }
\end{align}
$$

which is an $$\text{Inv-Gamma}(\alpha + \frac{n}{2}, \beta + \frac { 1} { 2} \sum _ { i = 1} ^ { n } \left( x _ { i } - \mu \right) ^ { 2})$$.

If we derive the posetrior in terms of precision we would obtain $$\tau \sim \text{Gamma}(\alpha + \frac{n}{2}, \beta + \frac { 1} { 2} \sum _ { i = 1} ^ { n } \left( x _ { i } - \mu \right) ^ { 2})$$.

The predictive distribution for an unseen data point $$Y$$:

$$
\left.\begin{aligned} p \left( Y \vert \mathbf { x } ,\mu ,\alpha ,\beta \right) & = \int p \left( Y \vert \mathbf { x } ,\mu ,\tau \right) p ( \tau \vert \mathbf { x } ,\mu ,\alpha ,\beta ) d \tau \\ & = \int p \left(Y \vert \mu ,\tau \right) p ( \tau \vert \mathbf { x } ,\mu ,\alpha ,\beta ) d \tau \end{aligned} \right.
$$

which turns out to be a t-distribution:

$$
Y \sim \text{St} \left( \mu ,\frac { \alpha + n / 2} { \beta + \frac { 1} { 2} \sum _ { i = 1} ^ { n } \left( x _ { i } - \mu \right) ^ { 2} } ,2\alpha + n \right).
$$

### Conjugacy for the mean and variance

We make the following specifications:

$$
\begin{align}  
 X _ { i } &\sim N ( \mu ,\tau )\quad \quad i=1,\ldots,n  \\ 
\mu   &\sim N \left( \mu _ { 0} ,n _ { 0} \tau \right)  \\ 
\tau    &\sim \text{Gamma} ( \alpha ,\beta ) 
\end{align} 
$$

where the $$X_i$$ are assumed independent given $$\mu, \tau$$. We refer to this prior as normal-gamma distribution.

To compute the posterior distribution $$p(\mu, \tau \vert x)$$, we first compute $$p(\mu \vert \tau, x)$$ and then find $$p(\tau \vert x)$$.

$$
\begin{align}
\tau &\sim p(\tau \vert x) \\
\mu &\sim p(\mu \vert \tau, x)
\end{align}
$$

When $$\tau$$ is fixed, we are back in the setting of **conjugacy for the mean** and can simply use the results, plugging in $$n_0\tau$$ in place of $$\tau_0$$:

$$
\left.\begin{aligned} \mathbb { E } [ \mu \vert \mathbf { x } ] & = \frac { n \tau } { n \tau + n _ { 0} \tau } \overline { x } + \frac { n _ { 0} \tau } { n \tau + n _ { 0} \tau } \mu _ { 0} \\ & = \frac { n } { n + n _ { 0} } \overline { x } + \frac { n _ { 0} } { n + n _ { 0} } \mu _ { 0} \end{aligned} \right.
$$

and $$\tau_\text{post} = n\tau + n_0 \tau$$.



We proceed to working out the marginal posterior of $$\tau$$:

$$
\begin{align}
p \left( \tau ,\mu \vert \mathbf { x } ,\mu _ { 0} ,n _ { 0} ,\alpha ,\beta \right) &\propto p ( \tau \vert \alpha ,\beta ) p \left( \mu \vert \tau ,\mu _ { 0} ,n _ { 0} \right) p ( \mathbf { x } \vert \mu ,\tau ) \\
&\propto \left( \tau ^ { \alpha - 1} e ^ { - \beta \tau } \right) \left( \tau ^ { 1/ 2} e ^ { - \frac { n _ { 0} \tau } { 2} ( \mu - \mu 0) ^ { 2} } \right) \left( \tau ^ { n / 2} e ^ { - \frac { \tau } { 2} \sum _ { i = 1} ^ { n } \left( x _ { i } - \mu \right) ^ { 2} } \right) \\
& \propto \tau ^ { \alpha + n / 2- 1} e^{ - \left( \beta + \frac { 1} { 2} \sum _ { i = 1} ^ { n } \left( x _ { i } - \overline { x } \right) ^ { 2} \right) \tau }  \tau ^ { 1/ 2} e^{ - \frac { \tau } { 2} \left[ n _ { 0} \left( \mu - \mu _ { 0} \right) ^ { 2} + n ( \overline { x } - \mu ) ^ { 2} \right]}
\end{align}
$$

We want to integrate out $$\mu$$, and the last factor can be viewed as the joint probability distribution of two Gaussian random variables $$\mu$$ and $$\bar{x}$$ that own the following reparameterization form:

$$
\left.\begin{array} { l } { \bar { X } \sim \mu + \frac { 1} { \sqrt { n \tau } } \epsilon } \\ { \mu \sim \mu _ { 0} + \frac { 1} { \sqrt { n _ { 0} \tau } } \delta } \end{array} \right.
$$

Thus integrating out $$\mu$$ leaves us the marginal distribution of $$\bar{X}$$ which is still a Gaussian random variable with:

$$
\mathbb{E}[\bar{X}] = \mu_0, \text{Var}(\bar{X}) = \frac { 1} { n \tau } + \frac { 1} { n _ { 0} \tau }.
$$

The probability density function is 

$$
p(\bar{x}) \propto \tau^{1/2} \exp \left\{ - \frac { n n _ { 0} \tau } { 2\left( n + n _ { 0} \right) } \left( \overline { x } - \mu _ { 0} \right) ^ { 2} \right\}
$$

In the end, we have

$$
p \left( \tau \vert \mathbf { x } ,\mu _ { 0} ,n _ { 0} ,\mathcal { \beta } \right) \propto \tau ^ { \alpha + n / 2- 1} \exp \left \{ - \left( \beta + \frac { 1} { 2} \sum _ { i = 1} ^ { n } \left( x _ { i } - \overline { x } \right) ^ { 2} + \frac { n n _ { 0}} { 2\left( n + n _ { 0} \right) } \left( \overline { x } - \mu _ { 0} \right)^2 \right) \tau \right\}
$$

which is a gamma distribution $$\text{Gamma}(\alpha + n/2, \beta + \frac { 1} { 2} \sum _ { i = 1} ^ { n } \left( x _ { i } - \overline { x } \right) ^ { 2} + \frac { n n _ { 0}} { 2\left( n + n _ { 0} \right) } \left( \overline { x } - \mu _ { 0} \right)^2 )$$.





















