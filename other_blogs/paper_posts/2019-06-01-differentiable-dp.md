---
layout: post
comments: false
title: "Differentiable Dynamic Programming"
date: 2019-06-01 12:00:00
tags: differentiable-relax paper-notes
---


> Differentiable Dynamic Programming@ICML18.

<!--more-->

{: class="table-of-content"}
* TOC
{:toc}


## Background

**Notation**: Denoting standard simplex in $$\mathbb{R}^D$$ as $$\triangle _ { D } \triangleq \left\{ \boldsymbol { \lambda } \in \mathbb { R } _ { + } ^ { D } : \| \boldsymbol { \lambda } \| _ { 1 } = 1 \right\}$$.

## Dynamic programming

Every problem sovled by dynamic programming reduces to finding the highest-scoring path between a start node 1 and an end node $$N$$, on a weighted directed acyclic graph (DAG) $$G$$.  Every directed edge $$(i, j)$$ from a parent node $$j$$ to a child node $$i$$ has a weighted $$\theta_{i,j} \in \mathbb{R}$$, let us denote the weighted matrix as $$\boldsymbol{\theta} \in \mathbb{R}^{N \times N}$$ and all possible paths in DAG from node 1 to node $$N$$ as $$\mathcal{Y}$$. Any path $$\boldsymbol{Y} \in \mathcal{Y}$$ is a $$N \times N$$ binary matrix with $$y_{i,j} = 1$$ if the path goes through the edge $$(i,j)$$ and $$y_{i,j} = 0$$ otherwise. The computation of the highest score among all paths amounts to the following linear programming problem

$$
\operatorname { LP } ( \boldsymbol { \theta } ) \triangleq \max _ { \boldsymbol { Y } \in \mathcal { Y } } \langle \boldsymbol { Y } , \boldsymbol { \theta } \rangle \in \mathbb { R }.
$$

The size of $$\mathcal{Y}$$ is exponential in $$N$$, but $$\operatorname { LP } ( \boldsymbol { \theta } )$$ can be computed in one topologically-ordered pass over $$G$$ using dynamic programming as follows:

$$
\begin{aligned}   v _ { 1 } ( \boldsymbol { \theta } ) & \triangleq 0  \\  v _ { i } ( \boldsymbol { \theta } ) & \triangleq \max _ { j \in \mathcal { P } _ { i } } \theta _ { i , j } + v _ { j } ( \boldsymbol { \theta } )  \end{aligned}
$$

where $$\mathcal { P } _ { i }$$ indicates the parents of node $$i$$.

Very often, we are interested in the arguments that achieve the maximum, i.e., 

$$
\boldsymbol { Y } ^ { \star } ( \boldsymbol { \theta } ) \in \underset { \boldsymbol { Y } \in \mathcal { Y } } { \operatorname { argmax } } \langle \boldsymbol { Y } , \boldsymbol { \theta } \rangle.
$$

Note that there may exist multiple $$\boldsymbol { Y } ^ { \star } ( \boldsymbol { \theta } )$$. When $$\boldsymbol { Y } ^ { \star } ( \boldsymbol { \theta } )$$ is unique, it is equal to the gradient of $$\operatorname { LP } ( \boldsymbol { \theta } )$$, i.e., $$\nabla_{\boldsymbol{\theta}} \operatorname { LP } ( \boldsymbol { \theta } )$$. 

**Remark**: It is desirable if we could smooth the objective and calculate the gradient $$\nabla_{\boldsymbol{\theta}} \operatorname { LP } ( \boldsymbol { \theta } )$$, because we can then learn this $$\boldsymbol{\theta}$$ from data. For instance, in the traditional CRF for Name Entity Recogition, $$\boldsymbol{\theta}$$ corresponds to hand-crafted features; if we have a differentiable dynamic programming layer the features $$\boldsymbol{\theta}$$ can then be learned in an end-to-end manner.

## Smoothed max operator

Let $$\Omega: \mathbb{R}^D \rightarrow \mathbb{R}$$ be a strongly convex regularizer on $$\triangle _ { D }$$ and let $$\mathbf{x} \in \mathbb{R}^D$$. The max operator smoothed by $$\Omega$$ is defined as:

$$
\max{ \Omega } ( \boldsymbol { x } ) \triangleq \sup _ { \boldsymbol { q } \in \triangle _ { D } } \langle \boldsymbol { x }, \boldsymbol { q } \rangle - \Omega ( \boldsymbol { q } ).
$$

Since the $$\boldsymbol { q } \in \triangle _ { D } $$ that attains the maximum on the right hand side is unique, according to Danskin's theorem it is equal to the gradient of $$\max{\Omega}$$ at $$\boldsymbol{x}$$, let $$\phi(\boldsymbol{x}, \boldsymbol{q}) \triangleq \sup _ { \boldsymbol { q } \in \triangle _ { D } } \langle \boldsymbol { x }, \boldsymbol { q } \rangle - \Omega ( \boldsymbol { q } )$$:

$$
\nabla_{\boldsymbol{x}} \max{ \Omega } ( \boldsymbol { x } ) = \nabla_{\boldsymbol{x}} \phi(\boldsymbol{x}, \boldsymbol{q}^*) = \boldsymbol{q}^*.
$$

where $$\boldsymbol{q}^* \triangleq \underset { \boldsymbol { q } \in \triangle _ { D } } { \operatorname { argmax } } \left(\langle \boldsymbol { x }, \boldsymbol { q }  \rangle - \Omega ( \boldsymbol { q } )\right)$$.

**Remark**: 

* $$\max{ \Omega } ( \boldsymbol { x } )$$ actually is the [Legendre conjugate]({{ site.baseurl }}{% post_url 2018-12-03-legendra-transformation%}) of $$\Omega ( \boldsymbol { q } )$$.
* When choosing $$\Omega$$ as [negative-entropy]({{ site.baseurl }}{% post_url 2018-12-02-negative-entropy%}), $$\max_{\Omega}$$ becomes the $$\operatorname{log-sum-exp}$$ and $$\nabla\max_{\Omega}$$ becomes $$\operatorname{softmax}$$.
* Negative-entropy is strongly convex function, and thus $$\sup _ { \boldsymbol { q } \in \triangle _ { D } } \langle \boldsymbol { x }, \boldsymbol { q }  \rangle - \Omega ( \boldsymbol { q } )$$ is a strongly-concave optimization problem.


## Differentiable dynamic programming

$$
\operatorname { LP } _ { \Omega } ( \boldsymbol { \theta } )  \triangleq \max \Omega _ { \boldsymbol { Y } \in \mathcal { Y } } \langle \boldsymbol { Y } , \boldsymbol { \theta } \rangle.
$$

$$\operatorname { LP } _ { \Omega } ( \boldsymbol { \theta } )$$ is generally intractable when $$\mathcal{Y}$$ has an exponential size.

The smoothing dynamic programming replaces $$\max$$ with $$\max_{\Omega} $$ wthin the DP recursion, which leads to the $$\mathrm { DP } _ { \Omega } ( \boldsymbol { \theta } )$$ as follows

$$
\begin{aligned}   v _ { 1 } ( \boldsymbol { \theta } ) & \triangleq 0  \\  v _ { i } ( \boldsymbol { \theta } ) & \triangleq  \max{\Omega} _ { j \in \mathcal { P } _ { i } } \theta _ { i , j } + v _ { j } ( \boldsymbol { \theta } ).  \end{aligned}
$$

$$\operatorname { DP } _ { \Omega } ( \boldsymbol { \theta } )$$ is a sensible approximation of $$\operatorname { LP }  ( \boldsymbol { \theta } )$$, it has the following properties:

1. $$\operatorname { DP } _ { \Omega } ( \boldsymbol { \theta } )$$ is convex.
2. $$\operatorname { LP } ( \boldsymbol { \theta } ) - \operatorname { DP } _ { \Omega } ( \boldsymbol { \theta } )$$ is bounded above and below.
3. When $$\Omega$$ is seperable, $$\operatorname { DP } _ { \Omega } ( \boldsymbol { \theta } ) = \operatorname { LP } _ { \Omega } ( \boldsymbol { \theta } )$$ if and only if $$\Omega = -\gamma H$$, where $$\gamma \geq 0$$.

The usage of $$\operatorname { DP } _ { \Omega } ( \boldsymbol { \theta } )$$ and $$\nabla_{\boldsymbol{\theta}} \mathrm { DP } _ { \Omega } ( \boldsymbol { \theta } )$$:

* $$\mathrm { DP } _ { \Omega } ( \boldsymbol { \theta } )$$ can be used as a layer producing a real value loss.
* $$\nabla_{\boldsymbol{\theta}} \mathrm { DP } _ { \Omega } ( \boldsymbol { \theta } )$$ can be used as a layer, which outputs an optimal alignment $$\boldsymbol{Y}^*$$ given $$\boldsymbol{\theta}$$.



**Instantiations of $$\mathrm { DP } _ { \Omega } ( \boldsymbol { \theta } )$$**. In the above discussion $$\max{\Omega}$$ represents an abstract operator, we can instantiate it with negative-entropy or $$\ell_2$$ regularization. 

**Negative Entropy**. If we choose $$\Omega$$ as the negative-entropy $$\Omega(\boldsymbol{q}) = \gamma \sum_{i=1}^D q_i \log q_i$$ where $$\gamma > 0$$, then we obtain

$$
\begin{aligned} \max{ \Omega } ( \boldsymbol { x } ) & = \gamma \log \left( \sum _ { i = 1 } ^ { D } \exp \left( x _ { i } / \gamma \right) \right) \\ \nabla \max  { \Omega } ( \boldsymbol { x } ) & = \exp ( \boldsymbol { x } / \gamma ) / \sum _ { i = 1 } ^ { D } \exp \left( x _ { i } / \gamma \right) \\ \nabla ^ { 2 } \max  { \Omega } ( \boldsymbol { x } ) & = \boldsymbol { J } _ { \Omega } \left( \nabla \max  { \Omega } ( \boldsymbol { x } ) \right) \end{aligned}
$$

where $$\boldsymbol { J } _ { \Omega } ( \boldsymbol { q } ) \triangleq \left( \operatorname { Diag } ( \boldsymbol { q } ) - \boldsymbol { q } \boldsymbol { q } ^ { \top } \right) / \gamma$$. 

**Squared $$\ell_2$$ norm**. When $$\Omega ( \boldsymbol { x } ) = \frac { \gamma } { 2 } \| \boldsymbol { x } \| _ { 2 } ^ { 2 }$$ with $$\gamma > 0$$, we obtain

$$
\begin{aligned} \max  { \Omega } ( \boldsymbol { x } ) & = \left\langle \boldsymbol { q } ^ { \star } , \boldsymbol { x } \right\rangle - \frac { \gamma } { 2 } \left\| \boldsymbol { q } ^ { \star } \right\| _ { 2 } ^ { 2 } \\ \nabla \max  { \Omega } ( \boldsymbol { x } ) & = \underset { \boldsymbol { q } \in \Delta ^ { D } } { \operatorname { argmin } } \| \boldsymbol { q } - \boldsymbol { x } / \gamma \| _ { 2 } ^ { 2 } = \boldsymbol { q } ^ { \star } \\ \nabla ^ { 2 } \max  { \Omega } ( \boldsymbol { x } ) & = \boldsymbol { J } _ { \Omega } \left( \nabla \max  { \Omega } ( \boldsymbol { x } ) \right) \end{aligned}
$$

where $$J _ { \Omega } ( q ) \triangleq \left( \operatorname { Diag } ( s ) - s s ^ { \top } / \| s \| _ { 1 } \right) / \gamma$$ and $$s \in \{ 0,1 \} ^ { D }$$ is the vector that indicates the support of $$q$$. In this  case, we are required to compute $$\boldsymbol{q}^*= \operatorname { argmin }  \| \boldsymbol { q } - \boldsymbol { x } / \gamma \| _ { 2 } ^ { 2 } $$ which is the Euclidean projection onto the simplex $$\boldsymbol{x}/\gamma$$ and can be computed in expected $$O(D)$$ using the [randomized pivot algorithm](https://stanford.edu/~jduchi/projects/DuchiShSiCh08.pdf).


## Concrete examples

We instantiate $$\operatorname{DP}_{\Omega}$$ with two examples.

### Smoothed Viterbi $$\operatorname{Vit}_\Omega$$

In Name Entity Recognition, we wish to tag a sequence of $$\boldsymbol{X} = (\boldsymbol{x}_1, \ldots, \boldsymbol{x}_T)$$ of vectors in $$\mathbb{R}^D$$ with the most probable output sequence $$\boldsymbol{y} = (y_1, \ldots, y_T)$$ with $$S$$ distincet tags. We represent $$\boldsymbol{Y}$$ with a $$T \times S \times S$$ binary tensor, such that $$y_{t, i, j} = 1$$ if $$\boldsymbol{y}$$ transits from node $$j$$ to node $$i$$ at time $$t$$, and 0 otherwise. The potentials can be organized as a $$T \times S \times S$$ real tensor, such that $$\theta_{t, i, j} = \phi_t(\boldsymbol{x}_t, i, j)$$. Note that, the potentials are traditionally human engineered.

Using above representations, the inner product $$\operatorname{Vit}_\Omega(\boldsymbol{\theta}) = \langle \boldsymbol{Y}, \boldsymbol{\theta} \rangle $$ is equal to $$\sum_{t=1}^T \phi_t(\boldsymbol{x}_t, y_t, y_{t-1})$$.  When $$\Omega = -H$$, we recover linear-chain CRFs and the probability of $$\boldsymbol{y}$$ given $$\boldsymbol{X}$$ is

$$
p( \boldsymbol { y } \vert \boldsymbol { X } ) \propto \exp ( \langle \boldsymbol { Y } , \boldsymbol { \theta } \rangle ) = \exp \left( \sum _ { t = 1 } ^ { T} \phi _ { t } \left( \boldsymbol { x } _ { t } , y _ { t } , y _ { t - 1 } \right) \right).
$$

The gradient $$\nabla_{\boldsymbol{\theta}} \operatorname{Vit}_\Omega (\boldsymbol{\theta}) = \boldsymbol{E} \in \mathbb{R}^{T \times S \times S}$$ with $$e _ { t , i , j }$$ equals to edge marginal $$p \left( y _ { t } = i , y _ { t - 1 } = j \vert \boldsymbol { X } \right)$$. The node marginal probability of state $$i$$ at time $$t$$ is simply $$p \left( y _ { t } = i \vert \boldsymbol { X } \right) = \sum _ { j = 1 } ^ { S } e _ { t , i , j }$$. The conditional probability $$p( \boldsymbol { y } \vert \boldsymbol { X } )$$ is associated with $$\Omega$$, using a different $$\Omega$$ simply changes the distribution over state transitions. $$\Omega = \| \cdot \|^2$$ typically results in sparse marginal probabilities.


$$
\begin{aligned}
\theta ( \boldsymbol { X } ) _ { t , i , j } & \triangleq \boldsymbol { w } _ { i } ^ { \top } \boldsymbol { x } _ { t } + b _ { i } + t _ { i , j }, \quad t > 1\\
\theta ( \boldsymbol { X } ) _ { 1 , i , j } &\triangleq \boldsymbol { w } _ { i } ^ { \top } \boldsymbol { x } _ { t } + b _ { i }
\end{aligned}
$$

where $$\left( \boldsymbol { w } _ { i } , b _ { i } \right) \in \mathbb { R } ^ { D } \times \mathbb { R }$$ is the linear classifier associated with tag $$i$$ and $$\boldsymbol { T } \in \mathbb { R } ^ { S \times S }$$ is a transition matrix. $$\boldsymbol{W}, \boldsymbol{b}$$ and $$\boldsymbol{T}$$ along with the parameters of the network that produces $$\boldsymbol{X}$$ are learned from data. The eventual loss is 

$$
\Delta \left( \boldsymbol { Y } _ { \text { true } } , \nabla_{\boldsymbol { \theta }} \mathrm { DP } _ { \Omega } ( \boldsymbol { \theta } ) \right).
$$

It is the KL divergence when $$\Omega = -H$$, applied to row-wise to the marginalization, $$p \left( y _ { t } = i \vert \boldsymbol { X } \right)$$, of $$\boldsymbol { Y } _ { \text { true } }$$ and $$\boldsymbol { Y }$$.

### Smoothed DTW $$\operatorname{DTW}_\Omega$$

Let $$N_A$$ and $$N_B$$ be the lengths of two time series $$\boldsymbol{A}$$ and $$\boldsymbol{B}$$, whose $$i$$-th ($$j$$-th) column $$\boldsymbol{a}_i \in \mathbb{R}^p$$ ( $$\boldsymbol{b}_j \in \mathbb{R}^p $$) indicates the $$i$$-th ($$j$$-th) observation. We represent $$\boldsymbol{\theta}$$ as a $$N_A \times N_B$$ matrix. $$\theta_{i, j} = \delta(\boldsymbol{a}_i, \boldsymbol{b}_j)$$, in which $$\delta(\cdot, \cdot)$$ is a differentiable discrepancy, in most cases, Euclidean distance. Analogously, $$\boldsymbol{Y}$$ is also a $$N_A \times N_B$$ alignment matrix.

$$\nabla_{\boldsymbol{\theta}} \operatorname{DTW}_\Omega (\boldsymbol{\theta}) = \boldsymbol{E} \in \mathbb{R}^{N_A \times N_B}$$ is an optimal soft alignment matrix given $$\boldsymbol{\theta}$$. 

More details is available at [Soft-DTW]({{ site.baseurl }}{% post_url 2019-06-02-soft-dtw%}).

## Algorithms

![Beam search probability]({{ '/assets/images/ddp-vit.png' | relative_url }})
{: style="width: 100%;" class="center"}
*Fig. Differentiable Viterbi*
{:.image-caption}


![Beam search probability]({{ '/assets/images/ddp-dtw.png' | relative_url }})
{: style="width: 100%;" class="center"}
*Fig. Differentiable DTW*
{:.image-caption}
























