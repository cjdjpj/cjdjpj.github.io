---
layout: post
title: What is the most concise representation of data?
---

{{ page.title }}
================

<p class="meta">{{ page.date | date: "%d %B %Y" }}</p>

Many compression schemes work by finding patterns in the data which can be more concisely represented.
For example, if an image has a large block of same colored pixels, rather than storing all the pixels individually, it would be more efficient to simply store one hex value and add a note specifying the shape and size of this block.

Kolmogorov complexity takes this to the extreme, asking what is the smallest program that can reproduce some piece of data in full.
In essence, how much actual "information" is contained within the data.

To formalize this idea, we need a model of general purpose computers—the [Turing machine](https://en.wikipedia.org/wiki/Turing_machine).

A Turing machine $$M$$ with input $$w$$ describes a string $$x$$  if $$\langle M, w \rangle$$ outputs $$x$$ ($$x$$ is left on the tape of the TM after halting).

The **Kolmogorov complexity** of $$x$$ is therefore the length $$K(x)$$ of the smallest possible $$\langle M, w \rangle$$ which outputs $$x$$, called $$d(x)$$.

$$
K(x) = |d(x)|
$$

First, we must deal with how to represent $$\langle M, w \rangle$$ as a binary string, such that we can measure it's size.

$$M$$ and $$w$$ can each be trivially encoded on their own. But how do we encode them together?
Our encoding of $$\langle M, w \rangle$$ will be to simply $$w$$ appended to $$M$$. But to identify when $$w$$ starts, we need a prefix-free encoding. Our choice will be to double the bits of $$M$$, add a `01` separator, and then just concatenate $$w$$. This encoding works, although is not the absolute shortest.

$$
\langle M, w \rangle = \text{DOUBLE}(M)01w
$$

(An easy way for a slightly better encoding is to double the length of $$M$$ rather than $$M$$ itself. This way a decoder can simply count the number of characters to know when $$w$$ starts, reducing our overhead from $$O(n)$$ to $$O(\log n)$$)

Kolmogorov compression is therefore defined as $$f(x) = d(x) = \langle M, w \rangle$$

### Properties of Kolmogorov complexity

> Although some of these kind of already make sense intuitively

**1\.** Kolmogorov complexity is at most the length of the string (plus a constant)

$$K(x) \leq |x| + c$$

A machine $$M$$ that simply accepts regardless of $$w$$ can be created.
Then, $$\langle M, x \rangle$$ describes $$x$$.
This means $$K(x) = | \langle M \rangle | + |x|$$.
Since a machine that just accepts is trivial and of constant length, 
$$K(x) = | x | + c$$.

**2\.** Kolmogorov compression is at least as good as any other compression scheme (plus a constant)

$$K(x) \leq f(x) + c$$

for any compression scheme $$f$$.

Any compression scheme with a computable decompression function $$g$$ (by some Turing machine $$M_g$$) can be a Kolmogorov compression by the machine-input pair - $$\langle M_g, f(x) \rangle$$. 
Therefore, $$K(x) \leq f(x) + c$$.
You can replicate any compression scheme with a Turing machine and input string.

**3\.** $$K(xx) \leq K(x) + c$$

A description for $$xx$$ can be made by simply wrapping $$d(x)$$ (the optimal description of $$x$$), with a machine $$M$$ that duplicates its output. 
This machine is 

$$K(xx) = |\langle M \rangle| +  |d(x)| = K(x) + c$$

In fact, in general, any computable function of $$x$$ can be computed by a constant length machine, and so the Kolmogorov complexity of any computable function of a string is $$\lvert x\rvert + c$$

**4\.** $$K(xy) \leq 2K(x) + K(y) + c$$

A rather weak theorem, but the best bound because we need to DOUBLE the description of $$d(x)$$ in order to recognize when the description for $$y$$ begins.
Better coefficients than 2 can be reached with better encodings than DOUBLE, but can never be 1, we need more than a constant amount of data to separate TM and input string.

# $$K(x)$$ is not computable

So the answer to the question in the title is...we can't never know?

> Intuition: We can find a string $$x$$ that we are certain that has no minimal representation $$K(x) < n$$. If $$K(x)$$ were computable, then we could use that machine to make another machine which describes $$x$$ in terms of its Kolmogorov complexity, which would be a shorter description than $$n$$, giving rise to a contradiction.

Suppose $$M_k$$ computes $$K(x)$$ for any string $$x$$.

Then we can build $$C$$ which finds a string where $$K(x) \geq n$$:
1. Takes an integer $$n$$ as input
2. For each string $$x$$, computes $$K(x)$$ using $$M_k$$.
3. Halts and outputs $$x$$ when $$K(x) \geq n$$

$$C$$ must halt because there are a finite number of strings where $$K(x) < n$$. $$n$$, being an integer, in binary is of length $$\lceil \lg (n+1) \rceil$$.

The outputted $$x$$ has complexity $$K(x) \geq n$$ (by definition). It also has complexity 

$$
\begin{align*}
K(x) &\leq |\langle C, \langle n \rangle \rangle| \leq |\langle C \rangle| + |\langle n \rangle|\\
&\leq \lceil \lg (n+1) \rceil + c
\end{align*}
$$

Thus, we have the inequality

$$
n \leq K(x) \leq \lceil \lg (n+1) \rceil + c
$$

At high enough values of $$c$$, this inequality is false. Therefore, $$M_k$$ cannot exist and $$K(x)$$ cannot  be computable.

# Compressibility

Given a randomly generated string, we are pretty certain probabilistically that any given string cannot be compressed to be very small, i.e. most strings are quite incompressible.
(If you generate a image by randomly setting pixels, in most cases you will get a picture of noise, and there is not much we can compress from noise)

Yet mathematically, it is also impossible that $$K(x) > c$$ for any string $$x$$, i.e. there must be some string that is compressible.

To define compressibility, define $$x$$ as $$c$$-compressible if 

$$K(x) \leq |x| - c$$

for $$c \geq 1$$.
$$x$$ is incompressible if it is not 1-compressible, or $$K(x) \geq |x|$$

**For every $$n$$, there is some string that is incompressible.** 

By the pigeonhole principle, there are $$2^n$$ strings of length $$n$$, and $$2^{n}-1$$ strings of length $$n-1$$ or less. 
This means there must be some string of length $n$ that cannot be 1-compressible.

**Most strings are not very compressible.**

Picking a binary string uniformly at random,

$$
\Pr[x \text{ is c-compressible}] \leq \frac{2^{n-c+1}-1}{2^n} =2^{1-c} 
$$

It is also impossible to prove a string $$x$$ has high Kolmogorov complexity ($$K(x) \geq k$$) using any machine checkable math proof system.

---

### References

1. Sipser, Michael. Introduction to the Theory of Computation. Boston, Course Technology, 2020.

<br>

