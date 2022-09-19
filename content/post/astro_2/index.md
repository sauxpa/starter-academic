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
&skewness = \frac{\mathbb{E}\left[\left(X-\mu\right)^3\right]}{\sigma^{\frac{3}{2}}}\\,,\\\\
&kurtosis = \frac{\mathbb{E}\left[\left(X-\mu\right)^4\right]}{\sigma^{2}} - 3\\,.
\end{align*}

|          | $\mu$ | $\sigma$ | skewness | kurtosis |
|----------|-------|----------|----------|----------|
| Astro    | 0.63  | 20.67    | -1180    | 1426486  |
| Gaussian | 0.63  | 20.67    | 0        | 0        |

## Simulated flight time

Contrary to the continuous time Gaussian case, computing $\tau_{\alpha}$ for the true Astro distrubtion is not possible in closed-form using similar martingale arguments as in the first post. However, it is straightforward to simulate random walks following the Astro distribution and therefore to estimate $\tau_{\alpha}$ by the Monte Carlo method. The result of 100 millions independent replications are reported below against the inverse Gaussian distribution coming from the Gaussian approximation model.

{{< figure src="astro_vs_gaussian_flight_time_distributions.png" >}}

The simulation reveals that the inverse Gaussian approximation underestimates the probability of small flight times and overestimates that of longer ($>100$) flight times. In other words, the inverse Gaussian approximation does not account properly for the high risk of early crash. We report mean, standard deviation and percentiles of $\tau_{\alpha}$ under both the simulated Astro and the inverse Gaussian models (95% confidence intervals for the simulated statistics are calculated by bootstrap with 1,000 independent replications).

|                   | $\mu$              | $\sigma$           | 5th             | 25th            | 50th            | 75th            | 95th               |
|-------------------|--------------------|--------------------|-----------------|-----------------|-----------------|-----------------|--------------------|
| Astro (simulated) | 3.20 <br />[3.19 ; 3.21] | 60.8 <br />[56.9 ; 64.1] | 1.0 <br />[1.0 ; 1.0] | 1.0 <br />[1.0 ; 1.0] | 1.0 <br />[1.0 ; 1.0] | 2.0 <br />[2.0 ; 2.0] | 11.0 <br />[11.0 ; 11.0] |
| IG approximation   | 3.17               | 20.67              | 0.00            | 0.01            | 0.02            | 0.09            | 2.14               |

The expectation of $\tau_{\alpha}$ is surprisingly well estimated in the inverse Gaussian model (3.17 versus 3.20 $\pm$ 0.01). This is not totally surprising: remember the expectation of $IG\left(\frac{\alpha}{\mu}, \frac{\alpha^2}{\sigma^2}\right)$ is $\frac{\alpha}{\mu}$, which depends only on the threshold $\alpha$ and the expectation $\mu$ of the Astro random walk, *not* on higher moments which are crucially underestimated by the Gaussian approximation. On the contrary, quantiles are poorly estimated by the Gaussian model, partly because of the continuous time (quantiles in the discrete time Astro model cannot be less than 1, whereas the inverse Gaussian distribution puts a lot of mass on $[0, 1]$).

### Conclusion

The inverse Gaussian approximation based on continuous time martingale argument offers an analytical expression that tracks quite accurately the expected flight time of scratching Astro cards. In a sense, this is an empirical manifestation of the ubiquitous property that Gaussian distributions are limits of distributions with finite variance (central limit theorem, Donsker's theorem on rnadom walks...). This sort of approximation is numerically consistent in expectation. but much less so for other statistics, e.g. the quantiles. We leave as an open problem whether this can be studied more formally (e.g. in a similar spirit to Glivenko-Cantolli classes). Moreover, let's emphasise again the difference in interpretation between mean and median: the expected flight time $\mathbb{E}\left[\tau_{\alpha}\right]\approw 3.20$ is driven upwards by unlikely events of large magnitude; the most likely outcome of scratching Astra cards is to loose 2€ all at once!

Finally, you may check out the code used for the simulation and figures in this [repo](https://github.com/sauxpa/astro).

Cheers!

## License

Copyright 2022-present [Patrick Saux](https://sauxpa.github.io/).
