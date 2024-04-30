---
layout: post
comments: false
title: "Legendre Transformation"
date: 2018-12-03 12:00:00
tags: convex foundation random-topic
---


> Legendre transformation, its geometry intuition, and applications in machine learning.

<!--more-->

{: class="table-of-content"}
* TOC
{:toc}


## Legendre conjugate

For a strictly convex function $$f: \Omega \subset \mathbb{R}^n \mapsto \mathbb{R}$$ with $$\nabla^2f \succ 0$$, its Legendre conjugate is defined as

$$
f^* (\mathbf{y}) = \max_{\mathbf{x} \in \Omega}\ \langle \mathbf{y}, \mathbf{x} \rangle - f(\mathbf{x}), \quad  \mathbf{y} \in \mathbb{R}^n.
$$

As $$\mathbf{x} \mapsto \max_{\mathbf{x} \in \Omega}\ \langle \mathbf{y}, \mathbf{x} \rangle - f(\mathbf{x})$$ is a strictly concave function of $$\mathbf{x}$$ (linear function + concave function) it has unique point $$\mathbf{x}^*$$ that attains the maximum value

$$
\langle \mathbf{y}, \mathbf{x}^* \rangle - f(\mathbf{x^*}) \quad \mathrm{with}\quad \nabla f(\mathbf{x}^*) = \mathbf{y}.
$$

Hence, we can explain $$f^*(\mathbf{y})$$ like this, which point $$\mathbf{x} \in \Omega$$ has a gradient $$\nabla f(\mathbf{x})$$ equals to the given $$\mathbf{y}$$.

## Geometry explaination

Let us approach Legendre conjugate from the geometry perspective to gain more intuition. 

Recall that the epigraph for a convex function $$f$$ is 

$$
\mathcal{O} = \{ (\mathbf{x}, z) \mid \mathbf{x} \in \Omega, z \geq f(\mathbf{x}) \}.
$$

How can we describe its boundary $$\partial \mathcal{O}$$ ?

Of course, given a $$\mathbf{x} \in \Omega$$ we have $$(\mathbf{x}, z=f(\mathbf{x})) \in \partial \mathcal{O}$$, representing $$\partial \mathcal{O}$$ from $$\mathbf{x}$$-coordinate.

Let us consider the tangent hyperplane passing a fixed point $$(\mathbf{x}_0, f(\mathbf{x}_0)) \in \Omega$$: 

$$
z = \langle \nabla f(\mathbf{x}_0), \mathbf{x} - \mathbf{x}_0 \rangle + f(\mathbf{x}_0).
$$

The key point here is that there is only one point of $$\partial \mathcal{O}$$ that admits a tangent hyperplane with slope $$\nabla f(\mathbf{x}_0),$$ since $$\nabla f(\mathbf{x})$$ is a monotonous increasing function w.r.t $$\mathbf{x}$$ ($$\nabla^2f \succ 0$$). Therefore, we may also represent the boundary $$\partial \mathcal{O}$$ with the parameter $$\mathbf{y} = \nabla f(\mathbf{x})$$, representing $$\partial \mathcal{O}$$ from $$\mathbf{y}$$-coordinate. In summary, we can identify a point $$(\mathbf{x}, z) \in \partial \mathcal{O}$$, either by its $$\mathbf{x}$$-coordinate or its $$\mathbf{y}$$-coordinate ($$\mathbf{y} = \nabla f(\mathbf{x})$$). Namely, we have exhibited a dual coordinate system.

How can we write the boundary $$\mathcal{O}$$ in terms of $$\mathbf{y}$$-coordinate? Note that any hyperplane with a given slope $$\nabla f(\mathbf{x_0})$$ passing through $$\partial \mathcal{O}$$ has a unique point on $$z$$-axis. These lines admit one equation:

$$
z = \langle \nabla f(\mathbf{x_0}), \mathbf{p} - \mathbf{x} \rangle + f(\mathbf{x}),
$$

where $$(\mathbf{x}, f(\mathbf{x}))$$ is the intersection point between the hyperplane and $$\partial \mathcal{O}$$ (there may be multiple intersection points, and any of them is valid).  Among these parallel lines, $$z = \langle \nabla f(\mathbf{x_0}), \mathbf{p} - \mathbf{x}_0 \rangle + f(\mathbf{x}_0)$$ is the unique one that attains the minimized $$z$$-intersection value

$$
-\langle \nabla f(\mathbf{x}_0), \mathbf{x}_0 \rangle + f(\mathbf{x}_0).
$$

**Remark**: This is because $$f$$ is a convex function and $$f(\mathbf{x}) \geq \langle \nabla f(\mathbf{x_0}), \mathbf{x} - \mathbf{x}_0 \rangle + f(\mathbf{x}_0)$$, which concludes

$$
-\langle \nabla f(\mathbf{x_0}), \mathbf{x} \rangle + f(\mathbf{x})  \geq -\langle \nabla f(\mathbf{x_0}) , \mathbf{x}_0\rangle + f(\mathbf{x}_0).
$$

We can also imagine this visually. The hyperplane $$z = \langle \nabla f(\mathbf{x}_0), \mathbf{x} - \mathbf{x}_0 \rangle + f(\mathbf{x}_0)$$ only has one intersection with $$\partial \mathcal{O}$$ wheras other lines $$z = \langle \nabla f(\mathbf{x}_0), \mathbf{p} - \mathbf{x} \rangle + f(\mathbf{x})$$ with $$\mathbf{x} \ne \mathbf{x}_0$$ all have mutiple intersections with $$\partial \mathcal{O}$$; we can move any of the later lines to get the former by decreasing the $$z$$-axis intersection. 

The tangent hyperlane equation $$z = \langle \nabla f(\mathbf{x}_0), \mathbf{x} - \mathbf{x}_0 \rangle + f(\mathbf{x}_0)$$ can also be writen as

$$
\left\langle \begin{bmatrix}\nabla f(\mathbf{x}_0) \\ -1 \end{bmatrix}, \begin{bmatrix}\mathbf{x} \\ z \end{bmatrix} - \begin{bmatrix}\mathbf{x}_0 \\ f(\mathbf{x}_0) \end{bmatrix}\right\rangle = 0
$$

This form reveals that $$[\mathbf{x}, z]^\top \in \mathbb{R}^{n+1}$$ is any point in the hyperplane and $$[f(\mathbf{x}_0), -1]^\top \in \mathbb{R}^{n+1}$$ is the normal vector perpendicular to the hyperplane at point $$[\mathbf{x}_0, f(\mathbf{x}_0)]^\top$$.


So we can represent $$(\mathbf{x}_0, z=f(\mathbf{x}_0)) \in \partial\mathcal{O}$$ with $$\mathbf{y} := \nabla f(\mathbf{x_0})$$ as 

$$
\mathbf{x}_0 = \operatorname{argmin}_{\mathbf{x}} -\langle \mathbf{y}, \mathbf{x} \rangle + f(\mathbf{x}) = \operatorname{argmax}_{\mathbf{x}} \langle \mathbf{y}, \mathbf{x} \rangle - f(\mathbf{x}).
$$

In other words, finding a point $$\mathbf{x} \in \Omega$$ that has a gradient $$\nabla f(\mathbf{x})$$ equals to the given $$\mathbf{y}$$, which is equivalent to

$$
\max_{\mathbf{x} \in \Omega}\ \langle \mathbf{y}, \mathbf{x} \rangle - f(\mathbf{x}), \quad  \mathbf{y} \in \mathbb{R}^n.
$$

**Remark**: $$\langle \mathbf{y}, 0 - \mathbf{x} \rangle$$ describes the change of $$z$$ value of the hyperplane $$z = \langle \mathbf{y}, \mathbf{p} - \mathbf{x} \rangle + f(\mathbf{x})$$ when $$\mathbf{p}$$ goes from $$\mathbf{x}$$ to $$0$$. Given the slope $$\mathbf{y}$$, the hyperplane passing the point $$(\mathbf{x}, f(\mathbf{x}))$$ with $$\nabla f(\mathbf{x}) = \mathbf{y}$$ achieves the minimum value on $$z$$-axis. 

Interpreting Legendre transformation in one sentence:

$$
\max_{\mathbf{x} \in \Omega}\ \langle \mathbf{y}, \mathbf{x} \rangle - f(\mathbf{x}), \quad  \mathbf{y} \in \mathbb{R}^n
$$

is a linear function of $$\mathbf{y}$$ and thus it amounts to

$$
\min_{\mathbf{x} \in \Omega}\ \langle \mathbf{y}, -\mathbf{x} \rangle + f(\mathbf{x}), \quad  \mathbf{y} \in \mathbb{R}^n,
$$

which is the minimized $$z$$-intersection value that hyperplane

$$
z = \langle \mathbf{y}, \mathbf{p} -\mathbf{x} \rangle + f(\mathbf{x}), \quad  \mathbf{p} \in \mathbb{R}^n
$$

could attain.


## Properties

* $$f^* (\mathbf{y}) = \max_{\mathbf{x} \in \Omega}\ \langle \mathbf{y}, \mathbf{x} \rangle - f(\mathbf{x})$$ is a convex function, since it is a linear function of $$\mathbf{y}$$.
* $$f^{**} = f$$.
* $$\nabla f^* = (\nabla f)^{-1}$$ or $$(\nabla f^*)^{-1} = \nabla f$$ if $$\nabla f$$ is inversible.
* $$f(\mathbf{x}) + f^*(\mathbf{y}) \geq \langle \mathbf{y}, \mathbf{x} \rangle$$.

## Applications

[Smoothed max operator](https://arxiv.org/abs/1802.03676).




















