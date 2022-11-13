---
title: Gambler's ruin in Astro and the accuracy of Gaussian approximation [3]
subtitle:

# Summary for listings and search engines
summary: Some vacation fun on gambling and martingales.

# Link this post with a project
projects: [astro]

# Date published
date: "2022-11-12T00:00:00Z"

# Date updated
lastmod: "2022-11-12T00:00:00Z"

# Is this an unpublished draft?
draft: false

# Show this page in the Featured widget?
featured: false

# Featured image
# Place an image named `featured.jpg/png` in this page's folder and customize its options here.
image:
  caption: ""
  focal_point: ""
  placement: 2
  preview_only: false

authors:
- Patrick Saux

tags:
-

categories:
-
---
In the previous posts, we calculated the distribution of the so-called "flight time" $\tau\_{\alpha}$, interpreted as the (random) number of Astro card a player can scratch before loosing more than their initial investment $\alpha$ (typically $\alpha$ is €2), both in closed-form under a continuous time Gaussian setting and empirically using random simulations of the exact Astro loss random walk. These two approaches showed that $\frac{\alpha}{\mu}$, where $\mu$ is the average loss of random walk step, was a good estimation of $\mathbb{E}\left[ \tau\_{\alpha}\right]$; in the case of Astro, this estimate is $\mathbb{E}\left[ \tau\_{\alpha}\right]\approx 3.17$. We now show that this estimation is actually correct. We will actually prove a more general result, namely a closed-form expression of the Laplace transform of $\tau\_{\alpha}$, based on the method of martingales already used in the Gaussian approximation.

We recall that $\left(\xi_t\right)\_{t\in\mathbb{N}}$ denotes a i.i.d sequence of random Astro losses and let
$$
X_t = \sum_{s=1}^t \xi_s
$$
be the cumulative loss after $t\in\mathbb{N}$ steps, and that the flight time is defined by the equation
$$
\tau\_{\alpha} = \inf\left\lbrace t\in\mathbb{N}, X_t \geq \alpha\right\rbrace.
$$

## What goes wrong in the method of martingales for discrete random walks?

In the continuous time Gaussian setting, we replaced $\tau\_{\alpha}$ and $\left(X_t\right)\_{t\in\mathbb{N}}$ with $\bar{\tau}\_{\alpha} = \left\lbrace t\in\mathbb{R}\_+, Z\_t \geq \alpha\right\rbrace$ and $\left(Z_t\right)\_{t\in\mathbb{R}_+}$ satisfying the stochastic differential equation $dZ_t = \mu dt + \sigma dW_t$, driven by a Brownian motion $W$. Then for any $\lambda\in\mathbb{R}$,
$$
M^\lambda_t = \exp\left( \frac{\lambda}{\sigma} Z_t - (\frac{\lambda^2}{2} + \frac{\lambda \mu}{\sigma})t\right)
$$
defines a martingale and a standard application of Doob's optional stopping theorem showed that $\mathbb{E}\left[M^\lambda\_{\bar{\tau}\_{\alpha}}\right]=1$. Now in general, this gives information on two random variables simultaneously: $\bar{\tau}\_{\alpha}$ and $Z\_{\bar{\tau}\_{\alpha}}$. The whole trick is to disentangle the contribution of each of these variables and extract the Laplace transform of $\bar{\tau}\_{\alpha}$, which is $\mathbb{E}\left[e^{-\beta \bar{\tau}\_{\alpha}}\right]$ for $\beta>0$. In the continuous time setting, the almost sure continuity of $t\mapsto Z_t$ and the definition of $\bar{\tau}\_{\alpha}$ shows that $Z\_{\tau\_{\alpha}}=\alpha$ (almost surely), which is not random at all, thus revealing information directly on $\bar{\tau}\_{\alpha}$.

By contrast, the discrete time counterpart $X\_{\tau\_\alpha}$ does not satisfy a similar equality in general, but only the inequality $X\_{\tau\_\alpha} \geq \alpha$. Worse, the distribution of $X_{\tau\_\alpha}$ can be quite difficult to characterise as there are potentially many different sequences of steps $\xi_1, \dots, \xi\_{\tau\_\alpha}$ that result in crossing the barrier $\alpha$. In a special case of step distribution however, $X\_{\tau\_\alpha}$ takes a simple form, namely if the process $X$ can only increase by a fixed step size, or decrease by multiples of the same step size. More precisely, let $\Delta>0$, $(p_k)_{k\in\mathbb{N}}$ such that $p_k\in[0, 1]$ for all $k\in\mathbb{N}$ and $\sum_{k\in\mathbb{N}} p_k < 1$, and define the  i.i.d. step sequence $(\xi)_{t\in\mathbb{N}}$ by
$$
\xi_t = \begin{cases}
-k\Delta& \text{with probability $p_k$}\\,,\\\\
\Delta& \text{with probability $1-\sum_{k\in\mathbb{N}} p_k$}\\,.\\\\
\end{cases}
$$
Indeed, this forces the process $X$ to move over the grid $\Delta \mathbb{Z}$, and since $\alpha>0$ and $X$ can only increase by $\Delta$, this implies that $X\_{\tau\_{\alpha}}$ can only take a single value corresponding to $X\_{\tau\_{\alpha}-1}<\alpha$ and $\xi\_{\tau\_{\alpha}}=\Delta$, i.e. $X\_{\tau\_{\alpha}}=\lceil \frac{\alpha}{\Delta}\rceil\Delta =: \alpha^+$, where $\lceil x \rceil$ denotes the ceil operator (smallest integer larger than $x$).

As it turns out, this rather specific property is satisfied by the Astro step distribution: $\xi_t$ is equal to €2 minus the gain of the $t$-th ticket, and since all gains are multiple of €2, with a minimum of 0€, the above property holds with $\Delta$ equal to €2 and $(p_k)_{k\in\mathbb{N}}$ the corresponding gain probabilities.

## Exponential supermartingale and expected flight time

Using the property that $X\_{\tau\_\alpha}$, the martingale method developed in the first part for the Gaussian approximation can be applied to the process $X$ essentially unchanged. Let $\xi$ denote a generic random variable following the Astro step distribution and define
$$
\psi\colon \lambda\in\mathbb{R}\^\*\_+ \mapsto \log\mathbb{E}\left[ e^{\lambda \xi}\right]\\,.
$$
Then, for any $\lambda>0$, the process defined for $t\in\mathbb{N}$ by $M^\lambda_t = e^{X_t  - t\psi(\lambda)}$ is a martingale. Using the expression of the step size sequence, we have
$$
\psi(\lambda) = \lambda \Delta + \log\left(1 - \sum_{k\in\mathbb{N}}p_k\left(1 - e^{-(k+1)\Delta}\right)\right)\\,.
$$
Moreover, $e^{\psi(\lambda)} = \mathbb{E}\left[e^{\lambda \xi}\right] \geq e^{\lambda \mu}$ by convexity and Jensen's inequality, where
$$
\mu=\mathbb{E}\left[\xi\right] = \Delta \left( 1 - \sum_{k\in\mathbb{N}} (1+k)p_k\right)\\,.
$$
and hence $\psi(\lambda) \geq \lambda \mu >0$.

The same arguments as for the Gaussian approximation (dominated convergence, Doob's optional stopping theorem) apply and show that $\mathbb{E}\left[ e^{-\psi(\lambda) \tau_{\alpha}}\right] = e^{-\lambda \alpha^+}$. Noting that $\psi$ is invertible and using the change of variable $\beta = \psi(\lambda)$, we deduce that
$$
\mathbb{E}\left[ e^{-\beta \tau\_{\alpha}}\right] = e^{-\psi^{-1}(\beta) \alpha^+}\\,,
$$
which is the expression of the Laplace transform of $\tau\_{\alpha}$. To obtain the expected flight time $\mathbb{E}\left[ \tau\_{\alpha}\right]$, we differentiate with respect to $\beta$, which gives
$$
-\frac{\partial}{\partial \beta} \mathbb{E}\left[e^{-\beta \tau\_{\alpha}}\right] = \mathbb{E}\left[ \tau\_{\alpha}\right] = (\psi^{-1})'(0)\alpha^+ e^{-\psi^{-1}(0)\alpha^+} = (\psi^{-1})'(0)\alpha^+\\,,
$$
since $\psi(0)=0$.

The term $(\psi^{-1})'(0)$ can be calculated using the inverse function rule, which yields $(\psi^{-1})'(0)=\frac{1}{\psi'(0)}$. A direct calculation shows that
$$
\psi'(\lambda) = \Delta + \frac{-\Delta\sum_{k\in\mathbb{N}}(k+1)p_k e^{-(k+1) \Delta \lambda}}{1 - \sum_{k\in\mathbb{N}}(k+1)p_k}\\,,
$$
and thus $\psi'(0) = \Delta\left(1 - \sum_{k\in\mathbb{N}}(k+1)p_k\right) = \mu$. Going back to the expected flight time, we have
$$
\mathbb{E}\left[ \tau\_{\alpha} \right] = \frac{\alpha^+}{\mu}\\,.
$$
In particular for the Astro distribution, $\alpha=\Delta=\alpha^+$; in other words, $\mathbb{E}[\tau\_{\alpha}]=\mathbb{E}\left[\bar{\tau}\_{\alpha}\right]$, i.e. the Gaussian approximation was actually correct in expectation!

## License

Copyright 2022-present [Patrick Saux](https://sauxpa.github.io/).
