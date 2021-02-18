---
layout: post
comments: false
title: "Towards Principled Methods for Training Generative Adversarial Networks"
date: 2019-06-03 12:00:00
tags: implicit-generative optimal-transport paper-notes
---

> Towards Principled Methods for Training Generative Adversarial Networks@ICLR17.

<!--more-->

{: class="table-of-content"}
* TOC
{:toc}


Traditional approaches to genrative modeling relied on maximizing likelihood, or equivalently minimizing the Kullback-Leibler divergence between empirical data distribution $$\mathbb{P}_r$$ and our generator's distribution $$\mathbb{P}_g$$,

$$
K L \left( \mathbb { P } _ { r } \| \mathbb { P } _ { g } \right) = \int _ { \mathcal { X } } p _ { r } ( x ) \log \frac { p _ { r } ( x ) } { p _ { g } ( x ) } \mathrm { d } x = \mathbb{E}_{\mathbb{P}_r} \left[\log \frac { p _ { r } ( x ) } { p _ { g } ( x ) } \right].
$$

[Linking the maximum likelihood and KL divergence]({{ site.baseurl }}{% post_url 2018-01-01-exponential-family%}#kl-divergence-and-maximum-likelihood)

Two interesting points to note:

* If $$p_r(x) > p_g(x)$$, then $$x$$ is a point with higher probability of coming from the data than being a generated sample. KL divergence assigns high cost to these points.
* If $$p_g(x) > p_r(x)$$, then $$x$$ is a point more likely being generated. But KL divergence pays low cost to such points. In other words, the trained generators will tend to produce fake looking samples.



Generative adversarial networks have been shown to optimize the Jensen-Shannon divergence, 

$$
J S \left( \mathbb { P } _ { r } , \mathbb { P } _ { g } \right) = K L \left( \mathbb { P } _ { r } \| \mathbb { P } _ { m } \right) + K L \left( \mathbb { P } _ { g } \| \mathbb { P } _ { m } \right)
$$

where $$\mathbb{P}_m$$ is the average distribution with probability density $$(p_r + p_g)/2$$.

The training procedure is shown as follows,

* We first train a discriminator $$D$$ to maximize $$L(D, g_\theta)$$,

    $$
    L \left( D , g _ { \theta } \right) = \mathbb { E } _ { x \sim \mathbb { P } _ { r } } [ \log D ( x ) ] + \mathbb { E } _ { x \sim \mathbb { P } _ { g } } [ \log ( 1 - D ( x ) ) ].
    $$
    The optimal discriminator has the form,
    $$
    D ^ { * } ( x ) = \frac { p _ { r } ( x ) } { p _ { r } ( x ) + p _ { g } ( x ) },
    $$
    and $$L \left( D ^ { * } , g _ { \theta } \right) = 2 J S D \left( \mathbb { P } _ { r } \| \mathbb { P } _ { g } \right) - 2 \log 2$$.

* Once we get the optimal discriminator $$D^*$$, we minimize $$L \left( D ^ { * } , g _ { \theta } \right)$$ with repsect to $$\theta$$, which is equivalent to minimizing the Jensen-Shanon divergence $$J S D \left( \mathbb { P } _ { r } \| \mathbb { P } _ { g } \right)$$.

In theory, one would expect we first train the discriminator to be as close as to $$D^*$$, and then do gradient update on $$\theta$$, alternating these two steps until the convergence. In practice, as the discriminator gets better, the updates to the generator get consistently worse and unstable (so, very often, we update $$\theta$$ every time after we have updated $$D$$). 

The theory tells us that the trained discriminator will have cost at most $$2 \log2 - 2 JSD(\mathbb{P}_r \| \mathbb{P}_g)$$. In practice, we often observe that the error goes to 0, i.e., the JSD is maxed out. The only way this can happen is if the distributions are not continuous, or they have disjoint supports. One possible cause for the distributions not to be continuous is if their supports lie on low dimensional manifolds.

[The example of disjoint supports]({{ site.baseurl }}{% post_url 2019-06-05-variational-discriminator-bottleneck%}#analysis).

[Example 1 shows the case when JSD is maxed out]({{ site.baseurl }}{% post_url 2019-06-04-wgans%}#example-1).


