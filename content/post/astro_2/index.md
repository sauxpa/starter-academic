---
title: Gambler's ruin in Astro and the accuracy of Gaussian approximation [2]
subtitle:

# Summary for listings and search engines
summary: Some vacation fun on gambling and martingales.

# Link this post with a project
projects: [astro]

# Date published
date: "2022-09-18T00:00:00Z"

# Date updated
lastmod: "2022-09-18T00:00:00Z"

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

## Assessing the quality of the Gaussian approximation

In the previous post, we have calculated the distribution of the flight time of the cumulative Gaussian loss process $Z_t=\mu t + \sigma W_t$, that is $\tau_{\alpha} = \inf \left\lbrace t\in\mathbb{N}, Z_t \geq \alpha \right\rbrace$. In the game of Astro though, the loss distribution for scratching a card is far from being Gaussian: it is discrete, asymmetrical, with pronounced skewness towards small losses. The figure below shows the difference between the true Astro distribution (in green) and its Gaussian approximation $\mathcal{N}(\mu, \sigma^2)$ (in blue), with $\mu=0.63$ and $\sigma=20.67$.

{{< figure src="astro_vs_gaussian_distributions.png" >}}

It is insightful to look at the empirical estimation of $\mu$ and $\sigma$ under both the true Astro model and its Gaussian approximation. We define the standard (unbiased) sample estimators
\begin{align*}
&\widehat{\mu}_t = \frac{1}{t}\sum_{s=1}^t X_s \\,,\\\\
&\widehat{\sigma}^2_t = \frac{1}{t-1}\sum_{s=1}^t \left(X_s - \widehat{\mu}_t\right)^2 \\,,
\end{align*}
and report below the median (25th - 75th percentile in shaded area) of 10 independent replications of $\widehat{\mu}_t$ and $\widehat{\sigma}_t$ for $t$ ranging from 2 to 4,500,000, where $X$ is sampled from the Astro distribution (in green) and the Gaussian approximation (in blue).

{{< figure src="mu_sigma_astro_vs_gaussian_distributions.png" >}}

For the mean estimation, as expected, $\widehat{\mu}_t$ converges to the true expected value $\mu=0.63$, with higher dispersion in the Gaussian case. For the variance estimation, $\widehat{\sigma}_t$ seems to converge quite fast to the true value $\sigma=20.67$ in the Gaussian case, but remains much lower in the Astro model until $t\approx 10^5$, before suddenly jumping to values around $\widehat{\sigma}_t\approx 20$. This is actually quite intuitive: the high variance of the Astro distribution is driven by the outlier gains, in particular 1,000 and 25,000, which occur with very small probability. Before $t\approx 10^5$, the samples $X_1, \dots, X_t$ used in $\widehat{\sigma}_t$ do not contain such outliers, thus estimating a seemingly low variance. In other words, the sample variance in the Astro model is subject to higher variability than in the Gaussian case, which hints at larger higher order moments (skewness, kurtosis...) in the Astro case. We confirm this intuition with a direct calculation, based on the formulae:

\begin{align*}
&skewness = \frac{\sum_{x} (x - \mu)^3\mathbb{P}(gain=x)}{\sigma^{\frac{3}{2}}}\\,,\\\\
&kurtosis = \frac{\sum_{x} (x - \mu)^4\mathbb{P}(gain=x)}{\sigma^{2}} - 3\\,.
\end{align*}

|          | $\mu$ | $\sigma$ | skewness | kurtosis |
|----------|-------|----------|----------|----------|
| Astro    | 0.63  | 20.67    | -1180    | 1426486  |
| Gaussian | 0.63  | 20.67    | 0        | 0        |


### TBC

## License

Copyright 2022-present [Patrick Saux](https://sauxpa.github.io/).
