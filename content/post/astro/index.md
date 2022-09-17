---
title: Gambler's ruin in Astro and the accuracy of Gaussian approximation
subtitle:

# Summary for listings and search engines
summary: Some vacation fun on gambling and martingales.

# Link this post with a project
projects: [astro]

# Date published
date: "2022-09-17T00:00:00Z"

# Date updated
lastmod: "2022-09-17T00:00:00Z"

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
Recently, I was on a holiday trip with a friend, let's call him Nathan. While not exactly a gambling addict, Nathan likes to buy scratchcards every now and then. One of his favourites is called Astro; most newspapers shops in France sell them for €2 a piece, and there exists 12 different versions, one for each astrological sign. While the sign itself does not affect the odds (at least I don't believe so), it is a brilliant marketing strategy as it encourages you to buy a few more tickets and distribute them to your friends born under a different star than yours. Nathan is a Sagittarius, and that day he acquired a set of tickets, including an extra Aquarius one that he gifted to me. As it turned out, all but one of the set were loosers, the Aquarius one being the sole winner of a modest €6. Emboldened by this unexpected luck, I exchanged my gains for three more tickets to distribute among friends, keeping one Aquarius for myself. Again, all loosers but mine. This time I cashed out and stopped the gambling streak, much to Nathan's dismay. I admit this is not a particularly exciting vacation story, but it got me thinking: what if I did not cashed out? How long could I have last by just reinvesting my gains, flying off Nathan's initial €2 ticket?  


## The Astro distribution

Astro is a fairly transparent game: whoever takes the time to read the back of a ticket can learn all there is to know about the odds and possible profits. On mine, it read that 4,500,000 tickets were issued, among which 661,500 are winners for €2, 476,000 for €4, 150,000 for €6, 101,350 for €10, 45,000 for €20, 415 for €17T00, 8 for €1,000 and finally 3 extra lucky ones for €25,000. The figure below shows this gain distribution, with probabilities displayed in logarithmic scale for readability.

{{< figure src="astro_distribution.png" >}}

The expected gain of a ticket chosen uniformly at random (which I assume is how tickets are assigned to vendors) can be easily calculated from the above information and reads $\mathbb{E}[gain] = \sum_{x} x\mathbb{P}(gain=x) = 1.37$. After substracting the €2 cost of a ticket, the expected net payoff of a single Astro ticket is then $-0.63$. This makes perfect sense: the company that issues these tickets makes 63 cents on each, i.e about €2.3m in total.

In addition, we can also compute the variance with the formula $\sigma^2 = \mathbb{E}[gain^2] - \mathbb{E}[gain]^2$, which gives $\sigma\approx 20.67$.

## Flight time


Going back to our initial question: how long can we defy the odds and keep "flying" from only the initial investment of €2? Let's introduce some notations. We denote by $\left(g_t\right)_{t\in\mathbb{N}}$ a sequence of i.i.d samples drawn from the Astro distribution and let $\xi_t = 2 - g_t$ be the corresponding net *loss*, including the ticket price, and
$$
X_t = \sum_{s=1}^t \xi_s
$$
be the cumulative loss process.

Note that the i.i.d assumption does not exactly hold: once a ticket is bought and scratched, it is removed from the pool of available tickets and cannot be acquired again, i.e. the true sampling is *without replacement*. We will see later that this only has a limited impact on the final result, which is quite intuitive: if you draw only a few samples, say a few dozens at most, with replacement, it is quite unlikely that you would sample twice the same among the $N=4,500,000$.

Our problem of "flight time" corresponds to studying the following random time:
$$
\tau_{\alpha} = \inf \left\lbrace t\in\mathbb{N}, X_t \geq \alpha \right\rbrace,
$$
which is a stopping time with respect to the natural filtration $\left(\mathcal{F}_t\right)_{t\in\mathbb{N}}$ induced by the process $X$, i.e. $\mathcal{F}_t=\sigma\left( X_s, 0\leq s\leq t\right)$. Indeed, reaching the threshold $\alpha=2$ for the cumulative loss $X_t$ is equivalent to reaching €0 wealth for a gambler who starts at €2 and keeps scratching Astro at every round, which we informally defined as the "flight time" above.

## The Gaussian case

The reader versed in martingales will almost surely recognise this problem as an instance of gambler's ruin. Indeed, the distribution of $\tau_{\alpha}$ can be fully explicited when the increments $\xi$ are Gaussian. For completeness, we derive below this standard result using stochastic calculus.

For convenience, we consider the continuous time setting, i.e. $t\in\mathbb{R}_+$ instead of $t\in\mathbb{N}$ and define the process $Z$ as
\begin{align*}
&dZ_t = \mu dt + \sigma dW_t\,,\\
&Z_0=0\,,\\
\end{align*}
where $\mu=0.63$, $\sigma=20.67$ and $W$ is a standard Brownian motion with respect to the filtration $\mathcal{F}$. Intuitively, we retrieve the discrete time model by the correspondence $dt\approx 1$ and $dW_t \approx \xi_t$. The loss process $Z$ can be more directly expressed as
$$
Z_t = \mu t + \sigma W_t\,.
$$

Studying a stopping time can be quite hard, and coupling it with the right martingale is often the easiest approach. For $\lambda\in\mathbb{R}$, we define
$$
M^\lambda_t = \exp\left( \lambda W_t - \frac{\lambda^2}{2}t\right)\,.
$$
It is a standard result that $M^\lambda$ defines a $\mathcal{F}$-martingale. Moreover, it can be rewritten as $M^\lambda_t = \exp\left( \frac{\lambda}{\sigma} Z_t - (\frac{\lambda^2}{2} + \frac{\lambda \mu}{\sigma})t\right)$. Note that $t\mapsto X_t$ is (almost-surely) continuous (since $t\mapsto W_t$ is), and therefore $Z_{\tau_{\alpha}}=\alpha$. Doob's optional stopping theorem yields that the stopped martingale $\left(M^\lambda_{t \wedge \tau_{\alpha}}\right)_{t\in\mathbb{N}}$ is also a $\mathcal{F}$-martingale, and thus in particular
$$
\mathbb{E}[M^\lambda_{t \wedge \tau_{\alpha}}] = \mathbb{E}[M^\lambda_0] = 1\,.
$$
It is now tempting to consider the limit $t\rightarrow +\infty$ to obtain $\mathbb{E}[M^\lambda_{\tau_{\alpha}}]=\mathbb{E}[e^{\frac{\lambda \alpha}{\sigma} - (\frac{\lambda^2}{2} + \frac{\lambda \mu}{\sigma})\tau_{\alpha}}]=1$. This is justified since
$$
\lvert M^\lambda_{t\wedge \tau_{\alpha}} \rvert \leq e^{\frac{\lambda}{\sigma} \max(Z_{t\wedge \tau_{\alpha}}, 0) } \leq e^{\frac{\lambda \alpha}{\sigma}}\,,
$$
which allows to apply the dominated convergence theorem. Rearranging termes, we obtain the identity
$$
\mathbb{E}[e^{-(\frac{\lambda^2}{2} + \frac{\lambda \mu}{\sigma})\tau_{\alpha}}] = e^{-\frac{\lambda \alpha}{\sigma}}\,.
$$
This is almost the Laplace transform of $\tau_{\alpha}$, which we define for $\beta>0$ as $\mathbb{E}[e^{-\beta \tau_{\alpha}}]$. Since $\tau_{\alpha}$ is nonnegative, this Laplace transform fully characterises its distribution. Solving $\beta = \frac{\lambda^2}{2} + \frac{\lambda \mu}{\sigma}$ in terms of $\lambda$ yields $\lambda=-\frac{\mu}{\sigma} + \sqrt{\frac{\mu^2}{\sigma^2} + 2\beta}$ for $\lambda\geq 0$, i.e.
$$
\mathbb{E}[e^{-\beta \tau_{\alpha}}] = e^{\frac{\alpha \mu}{\sigma^2}\left(1 - \sqrt{1 + \frac{2\sigma^2 \beta}{\mu^2}}\right)} \,.
$$

Besides characterising the distribution, the Laplace transform is also useful to compute the moments. Indeed, by differentiating w.r.t. $\beta$, we have that
$$
-\frac{\partial}{\partial \beta} \mathbb{E}[e^{-\beta \tau_{\alpha}}]\bigg|_{\beta=0} = \mathbb{E}[\tau_{\alpha}] = \frac{\alpha}{\mu}\,.
$$

After all these calculations, let's take a step back to reflect on what this result means. On average, a gambler reaches the critical threshold $\alpha=2$ after $\frac{\alpha}{\mu}=\frac{2}{0.63}\approx 3.17$ rounds. This is, perhaps quite surprisingly, very intuitive: if you start from €2 and loose on average $63$ cents each time you play, it should take you a bit more than 3 rounds to consume your initial €2. The above shows that this back-of-the-envelope calculation is exact in the continuous time Gaussian case. Moreover, this result is independent of the variance $\sigma^2$.

### Nice! Does this mean that I can play thrice at the cost of a single ticket most of the time?

Not at all! It is important to make the distinction here between *mean* and *median* flight times. Intuitively, the distribution of flight time $\tau_{\alpha}$ should be quite asymmetrical around its mean: events where large gains are made early (e.g. first ticket yields €100) will result in very large flight times but remain quite rare, while events of "early crashes" (the few tickets are loosers) are much more frequent. Therefore, the median flight time, i.e. the majority of flight times are below it, should be less than the mean flight time, which is driven upwards by the "early luck" outliers. This type of distribution is called *right-skewed*.

### Flight time distribution in the Gaussian case

The expression of the Laplace transform $\mathbb{E}[e^{-\beta \tau_{\alpha}}]$ reveals that $\tau_{\alpha}$ follows an Inverse Gaussian distribution $IG\left(\frac{\alpha}{\mu}, \frac{\alpha^2}{\sigma^2}\right)$, the density of which is given by:
$$
p_{\tau_{\alpha}}(t) = \sqrt{\frac{\alpha^2}{2\pi \sigma^2 t^3}} \exp\left(-\frac{\left(\mu t - \alpha\right)^2}{2\sigma^2 t} \right)\,.
$$

We plot below this density for the Astro parameters ($\mu=0.63, \sigma=20.67, \alpha=2$), as well as the mean, the median, and the 5th, 25th, 75th and 95th percentiles. This distribution appears to be significantly right-skewed: even the 95th percentile is below the mean ($2.14$ versus $3.17$). In other words, according to the Gaussian approximation, it is really unlikely (less than 5% chance) that your flight time will be above 2 rounds, even though the mean flight time is above 3!

{{< figure src="ig_distribution.png" >}}

## TBC
Next time, we'll talk about the flight time for the true Astro distribution, rather than under the Gaussian approximation. Closed-form expression of $\tau_{\alpha}$ are not available in this case, so simulations will prove handy.

## License

Copyright 2022-present [Patrick Saux](https://sauxpa.github.io/).
