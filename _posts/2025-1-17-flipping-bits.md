---
layout: post
title: Flipping bits
---

{{ page.title }}
================

> Consider an array of $$L$$ binary values all initially set as False.
> We want to flip them all to True by doing a $$T$$-block flip, where $$T$$ consecutive values are flipped to be True (already True values stay True). 
> What is the expected number of $$T$$-block flips until the entire array is True (assuming the position of each $$T$$-block flip is chosen randomly with equal probability for each site)?

Example: $$L = 10$$, $$T = 4$$: $$FFFFFFFFFF$$. Flip $$1$$ at position $$3$$: $$FFTTTTFFFF$$. Flip $$2$$ at position $$5$$: $$FFTTTTTTFF$$, and so on.

## Solution-ish

Upper bound: If $$T = 1$$, it is the [coupon's collectors problem](https://en.wikipedia.org/wiki/Coupon_collector%27s_problem), which has the solution 
$$
E(X) = LH_L \approx L \ln(L) + \gamma + \frac{1}{2L}
$$

Lower bound: If $$T = L$$, then we simply need one $$T$$-block flip at position 1, which has
$$
E(X) = L
$$

## How to answer these?
1. Instead of expected number events until fully flipped, how about until some proportion of $$L$$ is flipped?
2. Instead of expected number events until fully flipped, how about until there is no unflipped segment longer than some length?

---

### References

1. https://math.stackexchange.com/q/5027343/1548676
