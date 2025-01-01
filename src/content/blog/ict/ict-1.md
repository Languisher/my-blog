---
author: Nan Lin # should be replaced
pubDatetime: 2025-01-01T15:00:00Z # should be replaced
modDatetime: 2025-01-01T15:00:00.000Z
title: Information and Entropy # should be replaced
slug: information-and-coding-theory-1 # should be replaced
featured: false # check
draft: false # check
tags:
  - Information # should be replaced
description:
  test description # should be replaced
---

## Table of contents

Wednesday, January 1, 2025
## Mathematic Model of the Source

*The source sending the "message" could be modelized as a random variable* since:
- The output of the source cannot be determined with certainty beforehand. For example, we cannot predict the exact outcome of the event before it occurs, making it inherently stochastic.
- We can use a probabilistic framework to describe the likelihood of different possible outcomes by treating the source as a random variable.

Below is a revision of the mathematical models of random variables based on the essence of the source.
### Memoryless Source

**Single-symbol discrete**:
$$
\begin{bmatrix}
X \\ P
\end{bmatrix} =
\begin{bmatrix}
x_1 & x_2 & \dots & x_n \\
p(x_1) & p(x_2) & \dots & p(x_n)
\end{bmatrix}
$$

**Multiple-symbol discrete**: Modelize the source as $X^N = (\alpha_{i})_{i \in [\![1, n^N]\!]}$ where $\alpha_i = (x_1 x_2 \dots x_N)$ and $x_i \in \{ x_1, \dots, x_n \}$
$$
\begin{bmatrix}
X^N \\ P
\end{bmatrix} =
\begin{bmatrix}
\alpha_1 & \alpha_2 & \dots & \alpha_n \\
p(\alpha_1) & p(\alpha_2) & \dots & p(\alpha_n)
\end{bmatrix}
$$
(Note that $p(\alpha_{i}) = p(x_{i_{1}}x_{i_{2}} \dots x_{i_{N}})= \prod_{k=1}^N p(x_{i_{k}})$ since the source is memoryless)


**Single-symbol continuous**:
$$
\begin{bmatrix}
X \\ P
\end{bmatrix} =
\begin{bmatrix}
\mathbb{R} \\ 
p_{X}(x)
\end{bmatrix}
$$

### Source with Memory

**Multiple-symbol discrete**: using the conditional probability, for a $m$-order Markovian source,
$$
\begin{bmatrix}
X \\ P
\end{bmatrix} = \begin{bmatrix}
x_{1} \ x_{2} \ \dots x_{n} \\
p(x_{i}|x_{i-1}x_{i-2}\dots x_{i-m})
\end{bmatrix}
$$

(Note that the $p$ is a probability matrix with dimension $n^m \times n$)

## Information of a Random Event: its Mathematic Model

### (Un)-Certainty of an Event: Probability of Happening

The term **(un)-certainty** of an event describes the possibility of such event actually happens in the view of the receiver. According to the definition, if the probability of such event will happen is high, the certainty is high and the uncertainty is low.

Here, *the term uncertainty measures the observer's lack of confidence or predictability regarding its occurrence.*

> Is the term "uncertainty" solely means the certainty that the event will not happen? No, it implies that neither the event happening nor not happening is clearly predictable.

Uncertainty also reflects the extent of surprise of the observer when the event actually happens.

Uncertainty arises when the probability of the event goes down.
### Self-Information: Mathematic Modeling of Uncertainty

(Here we only consider the case of single-symbol source)

**Self-information** about a specific event is a mathematical interpretation of the term "uncertainty". The self-information of a random event $X = x_i$ is:
$$
I(x_i) = - \log p(x_i)
$$
Properties of self-information:
- *Self-information is non-negative.* For every event $X = x_i$, $I(x_i) \geq 0$.
- *Self-information is monotone decreasing.* If $p(X = x_i) > p(X= x_2)$ then $I(x_1) < I(x_2)$
- *Certainty*: If the event has probability 1, it is certain that the event will happen; thus, the self-information is 0 bit. 
- *Extreme surprise*: If the event has a probability very close to 0, we feel exceedingly astonished if the event happens. Its self-information is infinity.

### Self-Information of Various Types of Source

**Single source, multiple symbols**: Source sending a sequence of $N$ symbols, where each symbol appears $m_{i} = m_{i}(x_{i})$ times.
$$
I = - \sum_{x_i \in X} m_i \log p(x_i) \quad \text{where} \quad \sum_{x_i \in X} m_i = N
$$

**Multiple sources**:
- **Joint self-information**: Information of the event that the symbols appears simultaneously: $I(x_{i}y_{j}) = -\log p(x_{i }y_{j})$
- **Condition self-information**: Information of the event that the symbol appears under certain condition: $I(x_{i}|y_{j}) = - \log p(x_{i}|y_{j})$

> Consider modelizing each case as a random event.

### Self-Information in the case of s-independent events

When $X$ and $Y$ are s-independent, $$I(x_{i}y_{j}) = I(x_{i})+ I(y_{j})$$
## Mutual Information: Relation in Two Random Events

### Mutual Information: Acquire Information about one Random Event from Another Random Event

In the communication end, we want to deduce what is the original message $X$ is after obtaining the received message $Y$. In other words, we want to acquire information about $X$ when observing $Y$.

This is described as the *shared information* between the two random variables, and is denoted as **mutual information**.

![](attachments/Mutual%20Information.png)