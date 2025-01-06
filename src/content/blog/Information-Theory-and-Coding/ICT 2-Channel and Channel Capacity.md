---
author: Nan Lin
pubDatetime: 2025-01-02T08:00:00Z
modDatetime: 2025-01-04T18:00:00.000Z
title: ICT 2-Channel and Channel Capacity
slug: information-and-coding-theory-2
featured: false
draft: false
tags:
  - Information-and-Coding-Theory
description: Note of Course ICE4411P-Information Theory and Coding (2/4)
---
## Table of contents

Message transmission Situation: Source $x_i$ ----Channel----> Destination $y_j$.
We want to figure out what is the symbol or the symbol sequence the source sends.

From an overall perspective:
- How much information could be transmitted from src to dst? (*Channel capacity*)
- Is the message highly effective? (*How to evaluate effectiveness?*)
- Has the message correctly arrived at the destination? (*How to evaluate correctness?*)

## Mutual Information and Average Mutual Information

### Definition of the Mutual Information

**Mutual information**: Information obtained (or "delivered") through the transmission of the symbol.
- Source sent $X = x_i$, Destination received $Y = y_j$.
- The receiver wants to deduce $X = ?$ based on the served message symbol $y_j$. Ideally, $x_{i}= y_{j}$.
- Information obtained = Uncertainty reduced.
- Amount of uncertainty reduced: Prior uncertainty about $X = x_i$ - Uncertainty about $X = x_i$ left after receiving $Y =y_j$

The mutual information of symbol $x_i$ and $y_j$:
$$
I(x_{i};y_{j}) = I(x_{i})-  I(x_{i}|y_{j}) = \log \frac{p(x_{i}|y_{j})}{p(x_{i})}
$$

### Properties of the Mutual Information

*Ideal case: Precise transmission*: 
- 100% correctness.
- $p(x_{i}| y_{j}) = 1$ therefore uncertainty is zero.
- Thus $I(x_{i};y_{j})=I(x_{i})$.

*Common case*:
- $p(x_{i}|y_{j}) < 1$.
- Thus $I(x_{i};y_{j}) < I(x_{i})$.
- Receive partial information about $x_i$.

*Case when the quality of the channel is very bad*:
- Uncertainty even increases after transmission (due to noise, interrupts , ...)
- $I(x_{i}|y_{j})> I(x_{i})$.
- Thus $I(x_{i};y_{j})<0$.
- No valuable information gained about $x_i$.

Relationship of mutual information and other self information:
$$
I(x_{i};y_{j}) = I(x_{i})- I(x_{i}|y_{j}) =  I(y_{j})-I(y_{j}|x_{i})  = I(x_{i}) + I(y_{j}) - I(x_{i}y_{j})
$$


### Average Mutual Information

**Average Mutual Information**: Quantify the amount of information passing through the channel.
- Evaluate the average amount of information delivered from src to dst.
- Mathematical expectation of overall mutual information between symbols of two sources.
- Depend on the transmission probability matrix.

> Note: Average of what? Mutual information of each possible symbol pair.

The average mutual information between $X$ and $Y$:
$$
I(X;Y) = \mathbb{E}_{X, Y}[I(x_{i};y_{j})] = \sum_{x_{i}} \sum_{y_{j}} p(x_{i}y_{j}) I(x_{i};y_{j}) = \sum_{x_{i}}\sum_{y_{j}} p(x_{i}y_{j}) \log \frac{p(x_{i}|y_{j})}{p(x_{i})}
$$

> Other formulas are similar.


### Conclusion

Identify: Mutual information, noise entropy and loss entropy during the transmission process.
![](attachments/Relationship%20Between%20Different%20Kinds%20of%20Entropies.png)
## Channel

### Mathematical Model of a Discrete Channel

A channel could be modeled as $\{ X, p(Y|X), Y \}$, which consists of three parts:
- The source, sends $X$
- The destination, receives $Y$
- The transition matrix between $X$ and $Y$: $[p(Y|X)]_{X, Y}$


### Special Types of Channel


A channel is **lossless** when the loss entropy $H(X|Y)$ is zero. (All information of the source is transmitted to the source)

A channel is **noiseless** when the noise entropy $H(Y|X)$ is zero. (The destination only accepts messages from a unique source.)

Ideal situation: A channel is simultaneously lossless and noiseless.

![](attachments/Lossless%20and%20Noiseless%20Channel.png)
### Channel Capacity

**Channel capacity**: measures the maximum amount of information could be transmitted from the source to the destination. It is ...
- Maximum of the transmission rate.
- Quantified by the average delivered information per time.
- The _average mutual information_ divided by time.

Thus, 
$$
C = \max_{p(x)} I(X;Y)
$$

How to calculate the channel's capacity? 
- Find the maximum of the mutual information between the source and the destination. 
- The maximum of the mutual information usually is reached when the symbols are equally distributed according to the maximum discrete entropy theorem.

Method 1: Calculate the derivative of $I(X;Y)$ to the unknown variable defining the probability distribution of $X$. (e.g. binary channel $p$ and $1-p$)
Method 2: For a lossless channel, the maximum is reached when the symbols of the source is equally distributed and $C= \max_{p(x)} H(X)$. For a noiseless channel, the maximum is reached when the symbols of the _destination_ is equally distributed and $C = \max_{p(x)} H(Y)$.
Method 3: Formula for special symmetric channels.
- Symmetric: 
	- Conditions: (1) The elements composing each row are identical and (2) The elements composing each column are identical
	- Capacity: $$C = \max H(Y) - H(p_{i_{1}}, \dots, p_{i_{m}}) = \log m - H(p_{i_{1}},\dots,p_{i_{m}})$$
- Quasi symmetric:
	- Conditions: After reorganization, could be divided into multiple subsets (in total $K$ subsets) which each of the subset is symmetric. Denote in each subset, the sum of each _row_ is $N_k$ and the sum of each _column_ is $M_k$.
	- Capacity: $$C= \log n - H(p_{i_{1}},\dots,p_{i_{m}})- \sum_{k=1}^K N_{k}\log M_k$$
Method 4: Lagrangian method for the general situations. We have $m$ unknowns denoted as $\beta_{1}, \dots, \beta_{m}$. *NSC of maximum mutual information is:*
$$
\forall i \in [\![1, n]\!], \; \sum_{j=1}^m p(y_{j}|x_{i})\log p(y_{j}|x_{i}) = \sum_{j=1}^m p(y_{j}|x_{i}) \beta_{j}
$$
(For each row, construct an equation: find the probability in each column and fill in the equation)
