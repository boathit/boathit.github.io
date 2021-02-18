---
layout: post
comments: false
title: "Variational Discriminator Bottleneck"
date: 2019-06-05 12:00:00
tags: implicit-generative information-theory paper-notes
---

> Variational Discriminator Bottleneck@ICLR19.

<!--more-->

{: class="table-of-content"}
* TOC
{:toc}


In this work, we propose a simple regularization technique for adversarial learning, which constrains the information flow from the inputs to the discriminator using a variational approximation to the information bottleneck.

Given a dataset $$\{\mathbf{x}_i, y_i\}$$ with features $$\mathbf{x}_i$$ and labels $$y_i$$, the standard maximum likelihood estimate $$q(y\vert\mathbf{x})$$ can be determined according to

$$
\min_{q}\quad \mathbb{E}_{\mathbf{x}, y \sim p(\mathbf{x}, y)} \left[-\log q(y \vert \mathbf{x})\right]
$$

where $$p ( \mathbf{x} , y ) = \frac { 1 } { N } \sum _ { n = 1 } ^ { N } \delta _ { \mathbf{x}_ { n } } ( \mathbf{x} ) \delta _ { y _ { n } } ( y )$$ is the empirical data distribution. This estimate is prone to overfitting, and the resulting model often exploit idiosyncrasies in the data. VIB (Variational Information Bottleneck) proposes regularizing the model using an information bottleneck to encourage the model to focus only on the most discriminative features. VIB first maps the features $$\mathbf{x}$$ to a latent random variable $$Z$$, defined by $$p(\mathbf{z} \vert \mathbf{x})$$, and then enforces an upper bound on the mutual information between the encoding and the original features $$I(X, Z)$$. This results in the following regularized objective $$J(q(y \vert \mathbf{x}),  p_E(\mathbf{z} \vert \mathbf{x}))$$

$$
\begin{aligned}
J(q(y \vert \mathbf{x}),  p(\mathbf{z} \vert \mathbf{x})) = &\min_{q,  p} \mathbb{E}_{\mathbf{x}, y \sim p(\mathbf{x}, y)} \left[\mathbb{E}_{\mathbf{z} \sim p_E(\mathbf{z} \vert \mathbf{x})} [-\log q(y \vert \mathbf{z})] \right] \\
&\mathrm{s.t.}\quad I(X, Z) \leq I_c.
\end{aligned}
$$

## The upper bound of $$I(Z, X)$$

$$
\begin{aligned}
I(Z, X) &= \int \int p(z, x) \log \frac{p(z\vert x)}{p(z)}\ dz\ dx \\
        &= \int \int p(z, x) \log p(z \vert x)\ dz\ dx - \int \int p(z, x) \log p(z)\ dz\ dx \\
\end{aligned}
$$

As $$\int p(z\vert x) \log p(z\vert x)\ dz \geq \int p(z\vert x) \log r(z)\ dz$$  $$\Longrightarrow \int p(z\vert x) \log p(z)\ dz - \int p(z\vert x) \log p(x)\ dz \geq \int p(z\vert x) \log r(z)\ dz$$ $$\Longrightarrow \int p(z\vert x) \log p(z)\ dz \geq \int p(z\vert x) \log r(z)\ dz + \log p(x)$$, and hence we have

$$
\begin{aligned}
I(Z, X) &\leq \int \int p(z, x) \log p(z \vert x)\ dz\ dx - \int p(x) \left[\int p(z\vert x) \log r(z)\ dz + \log p(x) \right] \ dx \\
&= \int \int p(z, x) \log p(z \vert x)\ dz\ dx - \int \int  p(x) p(z\vert x) \log r(z)\ dz\ dx + H(X)\\
&= \int \int p(z, x) \log \frac{p(z\vert x)}{r(z)}\ dz\ dx + H(X)\\
&= \mathbb{E}_{p(x)} [\mathrm{KL}[p(z\vert x) \| r(z)]] + H(X)
\end{aligned}
$$

$$H(X)$$ is a constant w.r.t the parameters we aim to optimize, and thus we have the upper bound,

$$
I(Z, X) \leq \mathbb{E}_{p(x)} [\mathrm{KL}[p(z\vert x) \| r(z)]] + \mathrm{const}
$$

The above analysis is also available at [DVIB](/deep-VIB.html).

## VIB objective

Replacing the $$I(Z, X)$$ with the upper bound we get 

$$
\begin{aligned}
\tilde{J}(q(y \vert \mathbf{x}),  p(\mathbf{z} \vert \mathbf{x})) = &\min_{q,  p} \mathbb{E}_{\mathbf{x}, y \sim p(\mathbf{x}, y)} \left[\mathbb{E}_{\mathbf{z} \sim p_E(\mathbf{z} \vert \mathbf{x})} [-\log q(y \vert \mathbf{z})] \right] \\
&\mathrm{s.t.}\quad \mathbb{E}_{\mathbf{x} \sim p(\mathbf{x})} [\mathrm{KL}[p_E(\mathbf{z}\vert\mathbf{x}) \| r(\mathbf{z})]] \leq I_c.
\end{aligned}
$$

By introducing the Lagrange multiplier $$\beta > 0$$, we have an equivalent objective

$$
\min_{q,  p} \mathbb{E}_{\mathbf{x}, y \sim p(\mathbf{x}, y)} \left[\mathbb{E}_{\mathbf{z} \sim p_E(\mathbf{z} \vert \mathbf{x})} [-\log q(y \vert \mathbf{z})] \right] + \beta (\mathbb{E}_{\mathbf{x} \sim p(\mathbf{x})} [\mathrm{KL}[p_E(\mathbf{z}\vert\mathbf{x}) \| r(\mathbf{z})]] - I_c )
$$

## Variational discriminator bottleneck

Recall the standard optimization objective of GAN

$$
\max _ { G } \min _ { D } \mathbb { E } _ { \mathbf { x } \sim p _ { * } ( \mathbf { x } ) } [ - \log ( D ( \mathbf { x } ) ) ] + \mathbb { E } _ { \mathbf { x } \sim p_G ( \mathbf { x } ) } [ - \log ( 1 - D ( \mathbf { x } ) ) ]
$$

where $$p_*(\mathbf{x}), p_G(\mathbf{x})$$ denotes the real data distribution and generated data distribution respectively.

By introducing the encoder $$p_E(\mathbf{z} \vert \mathbf{x})$$, the regularized objective $$\mathcal{L}(D, E, \beta)$$ is 

$$
\begin{array} { r l } {\min _ { D , E } \max _ {\beta \geq 0 } } & { \mathbb { E } _ { \mathbf { x } \sim p _ { * } ( \mathbf { x } ) } \left[ \mathbb { E } _ { \mathbf { z } \sim p_E ( \mathbf { z } \vert \mathbf { x } ) } [ - \log ( D ( \mathbf { z } ) ) ] \right] + \mathbb { E } _ { \mathbf { x } \sim p_G ( \mathbf { x } ) } \left[ \mathbb { E } _ { \mathbf { z } \sim p_E ( \mathbf { z } \vert \mathbf { x } ) } [ - \log ( 1 - D ( \mathbf { z } ) ) ] \right] } \\ { } & { + \beta \left( \mathbb { E } _ { \mathbf { x } \sim \tilde { p } ( \mathbf { x } ) } [ \operatorname { KL } [ p_E ( \mathbf { z } \vert \mathbf { x } ) \| r ( \mathbf { z } ) ] ] - I _ { c } \right) } \end{array}
$$

in which $$\tilde { p } ( \mathbf { x } )  = \frac{1}{2}p_*(\mathbf{x}) + \frac{1}{2} p_G(\mathbf{x})$$.

In the paper, the prior $$r(\mathbf{z}) = \mathcal{N}(0, I)$$, the encoder $$p_E(\mathbf{z} \vert \mathbf{x}) = \mathcal{N}(\mu_E(\mathbf{x}), \Sigma_E(\mathcal{x}))$$ models a Gaussian distribution, with mean $$\mu_E(\mathbf{x})$$ and diagnoal covariance matrix $$\Sigma_E(\mathcal{x})$$. The discriminator is modeled with a single linear unit followed by a sigmoid $$D ( \mathbf { z } ) = \sigma \left( \mathbf { w } _ { D } ^ { T } \mathbf { z } + \mathbf { b } _ { D } \right)$$.

The objective function for the generator is 

$$
\max _ { G } \mathbb { E } _ { \mathbf { x } \sim p_G ( \mathbf { x } ) } \left[ - \log \left( 1 - D \left( \mu _ { E } ( \mathbf { x } ) \right) \right) \right] \quad \text{or} \quad \min _ { G } \mathbb { E } _ { \mathbf { x } \sim p_G ( \mathbf { x } ) } \left[ - \log D \left( \mu _ { E } ( \mathbf { x } ) \right)  \right].
$$

Note that they approximate the expectation by evaluating $$D$$ at $$\mu _ { E } ( \mathbf { x } )$$ instead of the encoder's distribution.

## Analysis

![WGAN-Algorithm]({{ '/assets/images/vdb.png' | relative_url }})
{: style="width: 85%;" class="center"}
*Figure VDB*
{:.image-caption}

The above figure visualizes the decision bounds of $$D(\mathbf{x})$$ learned with different bounds $$I_c$$. Without VDB, the discriminator learns a sharp decision boundary, resulting in vanishing gradients for much of the space. But as $$I_c$$ decreases, the decision boundary is smoothed, providing more informative gradients that can be leveraged by the generator. 


