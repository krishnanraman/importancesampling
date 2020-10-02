# 1-line Importance Sampler

```{R}

Suppose you'd like to estimate the probability of drawing 5 or more from a standard normal.
Exact: Integral of p = exp(-x^2)/sqrt(2*pi), from 5 to infinity
In R: 1 - pnorm(5) = 2.866516e-07

Instead, consider proposal distribution q = N(5,1) ie. Gaussian centered at 5.
Integral(p) = Integral(pq/q) = Integral(p/q)*q = Expectation(p/q) under q.
We've changed measure from p to q.
Simply sample from q & compute p/q whenever samples are greater than 5.
Mean of above samples is the desired probability.

So what is p/q ?
p = density of N(0,1) = exp(-x^2/2)/sqrt(2*pi)
q = density of N(5,1) = exp(-(x-5)^2/2)/sqrt(2*pi)
=> p/q = exp((25-10x)/2)

# 1 line Importance Sampling, with Gaussian proposal
mean(sapply(rnorm(100000,5,1), function(x) { ifelse(x>5, exp((25-10*x)/2),0) }))
= 2.856604e-07

Consider proposal distribution of Uniform centered at 5 with width 20, ie. U[-5,15]
p/q = 20*exp(-x^2/2)/sqrt(2*pi)

# 1 line Importance Sampling, with Uniform proposal
mean(sapply(runif(1000,-5,15), function(x) { ifelse(x>5, 20*exp(-x^2/2)/sqrt(2*pi),0) }))
[1] 2.6877e-07
```
