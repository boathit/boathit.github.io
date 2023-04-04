---
layout: post
comments: false
title: "Variational Inference with Normalizing Flows"
date: 2019-06-10 12:00:00
tags: normalizing-flow variational-inference bayes paper-notes
---

> Variational Inference with Normalizing Flows@ICML15.

<!--more-->

{: class="table-of-content"}
* TOC
{:toc}

## Background

### The Evidence Lower Bound

The log-likelihood function with latent variable $$\mathbf{z}$$ can be written as

$$
\begin{aligned}\log p(x) &= \mathrm{D_{KL}} [q(\mathbf{z\vert x}) \|  p(\mathbf{z\vert x})] + \mathbb{E}_{q}\left[\log p(\mathbf{x, z}) - \log q(\mathbf{z \vert  x})\right]
\end{aligned}
$$

where the second term

$$
\mathbb{E}_{q}[\log p(\mathbf{x, z}) - \log q(\mathbf{z \vert  x})] =  -\mathrm{D_{KL}} [q(\mathbf{z\vert x}) \|  p(\mathbf{z})] + \mathbb{E}_q[\log p(\mathbf{x \vert  z})]
$$

is called the variational lower bound or evidence lower bound. 

### Variational Auto-Encoders

Current best practice in variational inference is stochastic gradient variational inference or variational auto-encoder, which scales to problems with very large data sets. Two techniques lie at the core of variational auto-encoder: reparameterization and Monte Carlo gradient estimation. 

* Reparameterization 
  
  $$
  z \sim \mathcal { N } ( z \vert  \mu , \sigma ^ { 2 } ) \Leftrightarrow z = \mu + \sigma \epsilon , \quad \epsilon \sim \mathcal { N } ( 0,1 )
  $$

* Backpropagation with Monte Carlo
  
  $$
  \nabla _ { \phi } \mathbb { E } _ { q _ { \phi } ( z ) } \left[ f _ { \theta } ( z ) \right] \Leftrightarrow \mathbb { E } _ { \mathcal { N } ( \epsilon \vert  0,1 ) } \left[ \nabla _ { \phi } f _ { \theta } ( \mu + \sigma \epsilon ) \right]
  $$

The simplest inference models that we can use are diagonal Gaussian densities, 

$$
q _ { \phi } ( \mathbf { z } \vert  \mathbf { x } ) = \mathcal { N } \left( \mathbf { z } \vert  \boldsymbol { \mu } _ { \phi } ( \mathbf { x } ) , \operatorname { diag } \left( \boldsymbol { \sigma } _ { \phi } ^ { 2 } ( \mathbf { x } ) \right) \right)
$$

which admits the simple form of reparameterization. 

However, the true posterior distributions in applications may be very complex that cannot be recovered by Gaussian distribution, and the choice of Gaussian distribution as posterior approximation will result in a very loose evidence lower bound.

If we use a Gaussian distribution to approximate the two components mixture distribution by minimizing the KL divergence, no solution is ever able to resemble the true data distribution.

![]({{ '/assets/images/kld-fit-mg.png' | relative_url }})
{: style="width: 65%;" class="center"}
*Figure KL Divergence Fitted MvGaussian*
{:.image-caption}


Let $$\mathbf{z}_1 \sim p(\mathbf{z}_1)$$ and $$f: \mathbb{R}^d \rightarrow \mathbb{R}^d$$ be an invertible, smooth transformation $$f = \mathbf{z}_1 \mapsto \mathbf{z}_2$$, then

$$
p(\mathbf{z}_2) = p(\mathbf{z}_1) \left\vert  \det \left(\frac{\partial f^{-1}}{\partial \mathbf{z}_2}\right)\right\vert  = p(\mathbf{z}_1) \left\vert  \det \left(\frac{\partial f}{\partial \mathbf{z}_1}\right)\right\vert ^{-1}
$$

where $$\frac{\partial f^{-1}}{\partial \mathbf{z}_2 }$$ and  $$\frac{\partial f}{\partial \mathbf{z}_1}$$ is the Jacobian matrix of the transformations $$f^{-1}$$ and $$f$$ respectively.

## Normalizing Flows

A normalizing flow describes the transformation of a probability density through a sequence of invertible mappings. We can construct arbitrarily complex densities by composing several simple maps and successively applying the probability density transformation:

$$
p(\mathbf{z}_2) = p(\mathbf{z}_1) \left\vert  \det \left(\frac{\partial f^{-1}}{\partial \mathbf{z}_2}\right)\right\vert  = p(\mathbf{z}_1) \left\vert  \det \left(\frac{\partial f}{\partial \mathbf{z}_1}\right)\right\vert ^{-1}
$$

The density $$q_K(\mathbf{z})$$ can be obtained by successively transforming a random variable $$\mathbf{z}_0$$ through a chain of $$K$$ transformations $$f_k$$:

$$
\left.\begin{aligned} \mathbf { z } _ { K } & = f _ { K } \circ \ldots \circ f _ { 2 } \circ f _ { 1 } \left( \mathbf { z } _ { 0 } \right) \\ \ln q _ { K } \left( \mathbf { z } _ { K } \right) & = \ln q _ { 0 } \left( \mathbf { z } _ { 0 } \right) - \sum _ { k = 1 } ^ { K } \ln \left\vert  \operatorname { det } \frac { \partial f _ { k } } { \partial \mathbf { z } _ { k - 1 } } \right\vert  \end{aligned} \right.
$$

The law of the unconscious statistician:

$$
\mathbb { E } _ { q _ { K } } [ h ( \mathbf { z } ) ] = \mathbb { E } _ { q _ { 0 } } \left[ h \left( f _ { K } \circ f _ { K - 1 } \circ \ldots \circ f _ { 1 } \left( \mathbf { z } _ { 0 } \right) \right) \right]
$$

which does not require computation of the logdet-Jacobian terms when $$h(\mathbf{z})$$ does not depend on $$q_K$$.

We can understand the effect of invertible flows as a sequence of expansions or contractions on the initial density. For an expansion, the map $$\mathbf { z } ^ { \prime } = f ( \mathbf { z } )$$ pulls the points $$\mathbf{z}$$ away from a region in $$\mathbb{R}^d$$, reducing the density in that region while increasing the density outside the region. Conversely, for a contraction, the map pushes points towards the interior of a region, increasing the density in its interior while reducing the density outside. 

**Remark**: consider the relative volume between the region and the region outside.				

### Deep Latent Gaussian Models

$$
p \left( \mathbf { x } , \mathbf { z } _ { 1 } , \ldots , \mathbf { z } _ { L } \right) = p ( \mathbf { x } \vert  f _ { 0 } \left( \mathbf { z } _ { 1 } \right) ) \prod _ { l = 1 } ^ { L } p \left( \mathbf { z } _ { l } \vert  f _ { l } \left( \mathbf { z } _ { l + 1 } \right) \right)
$$

The observation likelihood $$p(\mathbf{x} \vert  \mathbf{z})$$ is any appropriate distribution that is conditioned on $$\mathbf{z}_1$$. This model class includes factor analysis and PCA, non-linear factor analysis, and non-linear Gaussian belief networks as special cases.

## Inference

![]({{ '/assets/images/vae-nf.png' | relative_url }})
{: style="width: 65%;" class="center"}
*Figure Inference Framework*
{:.image-caption}

Specifying the transformations

$$
f ( \mathbf { z } ) = \mathbf { z } + \mathbf { u } h \left( \mathbf { w } ^ { \top } \mathbf { z } + b \right)
$$

The logdet-Jacobian term:

$$
\left. \begin{array} { c } { \left\vert  \operatorname { det } \frac { \partial f } { \partial \mathbf { z } } \right\vert  = \left\vert  \operatorname { det } \left( \mathbf { I } + \mathbf { u } \psi ( \mathbf { z } ) ^ { \top } \right) \right\vert  = \left\vert  1 + \mathbf { u } ^ { \top } \psi ( \mathbf { z } ) \right\vert  }\\
{ \psi ( \mathbf { z } ) = h ^ { \prime } \left( \mathbf { w } ^ { \top } \mathbf { z } + b \right) \mathbf { w } } \end{array} \right.
$$

and 

$$
\begin{aligned}   \mathbf { z } _ { K } &= f _ { K } \circ f _ { K - 1 } \circ \ldots \circ f _ { 1 } ( \mathbf { z } )  \\ 
 \ln q _ { K } \left( \mathbf { z } _ { K } \right) &= \ln q _ { 0 } ( \mathbf { z } ) - \sum _ { k = 1 } ^ { K } \ln \left\vert  1 + \mathbf { u } _ { k } ^ { \top } \psi _ { k } \left( \mathbf { z } _ { k - 1 } \right) \right\vert   \end{aligned}
$$

Evidence Low Bound

$$
\left.\begin{aligned} \mathcal { L } ( \mathrm { x } )  =&\ \mathbb { E } _ { q _ { \phi } ( z | x ) } \left[ \log q _ { \phi } ( \mathbf { z } | \mathbf { x } ) - \log p ( \mathbf { x } , \mathbf { z } ) \right] \\  =&\ \mathbb { E } _ { q _ { 0 } \left( z _ { 0 } \right) } \left[ \ln q _ { K } \left( \mathbf { z } _ { K } \right) - \log p \left( \mathbf { x } , \mathbf { z } _ { K } \right) \right] \\ =&\ \mathbb { E } _ { q _ { 0 } \left( z _ { 0 } \right) } \left[ \ln q _ { 0 } \left( \mathbf { z } _ { 0 } \right) \right] - \mathbb { E } _ { q _ { 0 } \left( z _ { 0 } \right) } \left[ \log p \left( \mathbf { x } , \mathbf { z } _ { K } \right) \right] \\  &- \mathbb { E } _ { q _ { 0 } \left( z _ { 0 } \right) } \left[ \sum _ { k = 1 } ^ { K } \ln \left\vert 1 + \mathbf { u } _ { k } ^ { \top } \psi _ { k } \left( \mathbf { z } _ { k - 1 } \right) \right\vert \right] \end{aligned} \right.
$$
