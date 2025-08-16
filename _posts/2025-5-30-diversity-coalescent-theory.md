---
layout: post
title: How much diversity is maintained in a neutral population?
---

{{ page.title }}
================

> Motivating question: under the neutral Wright-Fischer, a population loses diversity over time due to drift, but at what rate?

We will look at $$H$$, which stands for heterozygosity. Heterozygosity is usually an exclusively diploid concept. However, here $$H$$ refers simply to the probability two random alleles (at a single locus) are different in a population. Rather than referring to when an individual carries two different alleles via separate chromosomes, here it is a measure of "allelic diversity" that works for haploids as well. In the haploid case, it is akin to looking at $$\pi$$ for a single locus.

The only other difference between haploids and diploids is that there are double ($$2N$$) the alleles for diploids. We will assume a haploid population ($$N$$).

Two alleles [**coalesce**](https://en.wikipedia.org/wiki/Coalescent_theory) when their lineages merge (i.e. they reach a common ancestor). There are two exhaustive possibilities for the ancestry of two alleles:

1. They coalesced in the previous generation. This occurs with probability $$\frac{1}{N}$$, since the first allele's parent can be anything, and the second allele must have the same parent allele ($$1 \times \frac{1}{N}$$). 
    - Without mutation, the alleles must be identical - meaning $$H = 0$$.

2. They did not coalesce in the previous generation. This occurs with probability $$1-\frac{1}{N}$$, by the complement rule.
	- Without mutation, the alleles are only different if the parents were different, which means $$H=H_{t-1}$$ (since the probability two parents are different is by definition $$H_{t-1}$$).

Therefore,

$$
H_t = \frac{1}{N} \times 0 + (1-\frac{1}{N}) \times H_{t-1} 
$$

Converting the recursive formula to an explicit formula,

$$
H_t = (1-\frac{1}{N})^t H_0 
$$

In other words, diversity at a locus decays geometrically as a factor of $$1-\frac{1}{N}$$ every generation. Since $$(1-x) \approx e^{-x}$$ for small $$x$$ (from the Taylor expansion of $$e^{-x}$$, and discarding order $$\geq 2$$ terms as part of the approximation), we can approximate this using exponential decay

$$
H_t = H_0e^{(-\frac{1}{N})^t} = H_0e^{-\frac{t}{N}}
$$


> At the same time, mutation introduces diversity into the population. How much diversity do we expect to see with mutations introducing it and drift removing it?


Under the infinite-alleles model, each new mutation creates a new allele. The probability of a mutation occurring each generation at an allele is $$\mu$$. The probability that no mutation occurs is $$(1-\mu)$$.

From this, the probability that two alleles coalesced $$t$$ generations ago and are still identical is

$$
\mathbb{P}(\text{coal. in } t,  \text{no mut}) = (1-\mu)^{2t}\frac{1}{N}(1-\frac{1}{N})^{t-1}
$$

We can extend this to calculate the probability that two alleles coalesced at all before they mutated

$$
\mathbb{P}(\text{coal. before mut}) = \sum_{t=0}^\infty \mathbb{P}(\text{coal. in } t, \text{no mut})
$$

From here we make some reasonable assumptions to allow for simplification:
1. $$t \approx t+1$$
2. $$(1-x) \approx e^{-x}$$
3. $$\frac{1}{N} \ll 1$$ (the population is large) and $$\mu \ll 1$$ (mutation is rare) so we can approximate the sum with an integral.

This allows us to simplify to:

$$
\begin{align*}
\mathbb{P}(\text{coal. before mut}) &= \sum_{t=0}^\infty \frac{1}{N}e^{-2t\mu}e^{-\frac{t}{N}} \\
&= \sum_{t=0}^\infty \frac{1}{N}e^{-t(2\mu + \frac{1}{N})} \\
&\approx \frac{1}{N}\int^\infty_0 e^{-t(2\mu + \frac{1}{N})}\\
&= \frac{\frac{1}{N}}{\frac{1}{N} + 2\mu}\\
&= \frac{1}{1+2N\mu}
\end{align*}
$$

The complement of this probability, that two alleles *mutate before coalescing*, or that two randomly sampled alleles are different (since if they coalesced, they must be identical), is $$H_e$$.

$$
H_e = 1-\frac{1}{1+2N\mu} = \frac{2N\mu}{1+2N\mu}
$$

We often call $$\theta = 2N\mu$$. It is the population scaled mutation rate and an important quantity of the population, but often difficult to know precisely.
One such way to estimate the $$\theta$$ of a population is by looking at the number of [segregating sites](https://en.wikipedia.org/wiki/Segregating_site) in a population ($$\theta_w$$ from Watterson 1975).

> What about the expected diversity across the genome ($$\pi$$) between two samples?

When trying to estimate $$\pi$$, it is important to note that we are now operating at the level of individuals with potentially many alleles. This means $$\pi$$ is a fundamentally different quantity from $$H$$.

Let's call $$T_2$$ the time until two samples coalesce.

Since the probability of two samples coalescing in any given generation is $$\frac{1}{N}$$, the probability that two samples coalesce in generation $$t+1$$ is

$$
\mathbb{P}(T_2 = t+1) = \frac{1}{N} (1-\frac{1}{N})^t
$$

This is a geometric distribution (where the last success is included) where $$T_2 \sim \text{Geo}(\frac{1}{N})$$, meaning as a property of the geometric distribution, we know the mean/expected time to coalescence is $$\mathbb{E}(T_2) = N$$.

Given two samples coalesce $$t$$ generations ago ($$t \approx t+1$$), the expected number of mutations they accumulate is $$2t\mu$$, since each sample is accumulating mutations at rate $$\mu$$ for time $$t$$.
Therefore,

$$
E(\pi) = 2t\mu = 2\mathbb{E}(T_2)\mu = 2N\mu
$$

This result is surprisingly simple - it means that a population under Wright-Fischer assumptions with a mutation rate of $$\mu$$ and a population size of $$N$$ would have an average $$\pi$$ of around $$2N\mu$$. 

When mutation is rare and $$\theta \ll 1$$, $$H_e \approx E(\pi)$$. We can see why by looking at the similar but subtle differences in their definitions. $$H_e$$ is the probability that two random alleles are different. $$\pi$$ is the expected number of differences between two samples. Because mutation is rare, it is unlikely for two samples to have more than one mutation. Each sample's genome is effectively an "allele" that becomes different after one mutation and the two quantities measure the same thing. If mutation is common however, this approximation no longer works.

We find that $$E(\pi) = \theta$$. In other words, by getting the hamming distance between individuals in a population and taking the average, we can get an empirical estimate of the population scaled mutation rate. This is very useful! It means that if we know we can assume the Wright-Fischer about a population, we can infer its mutation rate from its diversity. On the other hand, if we know the mutation rate, we can check if a population is deviating from Wright-Fischer assumptions by measuring its diversity. Indeed, another statistic - Tajima's D - tests if a population is evolving neutrally by comparing $$\pi$$ with $$\theta_w$$.

---

### References

1. Coop, Graham. (2021). Population and Quantitative Genetics. Version 1.2. https://github.com/cooplab/popgen-notes.

2. Wakeley, John. (2009). Coalescent theory : an introduction. Roberts & Co. Publishers.
