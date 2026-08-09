---
layout: page
title: MIT 6.S183 Homeworks
description: "Homeworks in MIT 6.S183: A Practical Introduction to Diffusion Models"
category: Diffusion
toc:
  sidebar: left
---

## Problem Set1

### Question1

**a.**
Prove that for any distribution over random variables $$X$$ and $$Y$$ we have

$$
\arg\min_f \mathbb{E}_{x,y} \big[ \|f(y) - x\|^2 \big] = \mathbb{E}[x \vert y].
$$

Hint: Solve the optimization problem pointwise for fixed $$y$$.

**Proof**

With the [law of total expectation](https://en.wikipedia.org/wiki/Law_of_total_expectation), we have 

$$
\mathbb{E} \big[ \|f(y) - x\|^2 \big] = \mathbb{E}_Y \bigg[ \mathbb{E}_X \big[ \|f(y) - x\|^2 \vert Y = y \big] \bigg]
$$

For any fixed $$y$$, $$f(y)$$ is nothing but a constant. Let $$c = f(y)$$, the inner expectation could be expressed as

$$
\begin{aligned}
\mathbb{E}_X \big[ \|f(y) - x\|^2 \vert Y = y \big] &= \mathbb{E}_X [ \| c - x\|^2 \vert Y=y ] \\
&= \mathbb{E}_X [ c^2 - 2 c x + x^2 \vert Y=y ] \\
&= c^2 - 2c \mathbb{E} [X \vert Y=y] + \mathbb{E} [X^2 \vert Y=y]
\end{aligned}
$$

Obviously, the minimizer of $$c$$ is

$$
c^* = f(y) = \mathbb{E} [X \vert Y=y]
$$

This is also known as the [MMSE estimator](https://en.wikipedia.org/wiki/Minimum_mean_square_error_estimator).∎

**b.**
Let $$\mu$$ be the density function of a data distribution, so that $$\mu(x_0) \geq 0$$ for all $$x_0 \in \mathbb{R}^n$$ and $$\int_{\mathbb{R}^n} \mu(x_0) \mathrm{d} x_0 = 1$$. Consider the following loss function for fixed $$\sigma$$.

$$
\mathcal{L}_{\sigma}(\epsilon_{\sigma}) = \mathbb{E}_{x_0 \sim \mu, \epsilon \sim N(0, I_n)} \left[ \|\epsilon_{\sigma}(x_0 + \sigma \epsilon) - \epsilon\|^2 \right]
$$

Write down the exact minimizer $$\epsilon_{\sigma}^*(x_{\sigma})$$ in terms of the data distribution $$\mu(x_0)$$ and input $$x_{\sigma}$$. 

Hint: use part **a**, Bayes' rule, as well as the probability density function of $$N(x_0, \sigma^2 I_n)$$.

**c.**
Let $$x_0$$ be a random variable distributed according to some continuous density $$p_0(x)$$. Define $$x_t := x_0 + \sigma(t)\epsilon$$ for some function $$\sigma : [0, 1] \rightarrow \mathbb{R}_{\geq 0}$$ and let $$p_t(x)$$ be the probability density associated with $$x_t$$.
Show that $$v(x, t) = \mathbb{E}[\frac{d\sigma(t)}{dt} \mid x_t = x]$$ and $$p_t(x)$$ satisfy the *transport equation* governing the evolution of the density $$p_t$$ subject to a velocity field $$v$$:

$$
\partial_t p_t(x) + \nabla \cdot (v(x, t) p_t(x)) = 0
$$

Hint: Consider the time derivative of the *characteristic function* $$g(t, k) = \mathbb{E}[e^{ik \cdot x_t}]$$. You may also use the fact from Fourier analysis that

$$
\int e^{ik \cdot x} 
\nabla \cdot f(x) \mathrm{d} x = -ik \cdot \int e^{ik \cdot x} f(x) \mathrm{d} x
$$

and, for continuous functions $$f$$ and $$g$$, $$\int e^{ik \cdot x} f(x) \mathrm{d} x = \int e^{ik \cdot x} g(x) \mathrm{d} x$$ iff $$f = g$$.