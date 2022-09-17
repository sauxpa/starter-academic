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

## License

Copyright 2022-present [Patrick Saux](https://sauxpa.github.io/).
