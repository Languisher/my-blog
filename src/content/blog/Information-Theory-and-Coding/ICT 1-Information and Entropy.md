---
author: Nan Lin
pubDatetime: 2025-01-01T23:00:00Z
modDatetime: 2025-01-07T08:00:00.000Z
title: ICT 1-Information and Entropy
slug: information-and-coding-theory-1
featured: false
draft: false
tags:
  - Information-and-Coding-Theory
description: "Course Notes: ICE4411P-Information Theory and Coding (1/4)"
---
## Table of contents

## Mathematic Model of the Source

*The process of the source sending the "message" could be modeled as a random variable*:
- The output of the source cannot be 100% determined beforehand. For example, we cannot predict the exact outcome of the event before it occurs, making it inherently stochastic.
- We can use a probabilistic framework to describe the likelihood of different possible outcomes by treating the source as a random variable.

### Memoryless Source

Below is a revision of the mathematical models of random variables based on the essence of the source.

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

**Multiple-symbol discrete**: Model the source as $X^N = (\alpha_{i})_{i \in [\![1, n^N]\!]}$ where $\alpha_i = (x_1 x_2 \dots x_N)$ and $x_i \in \{ x_1, \dots, x_n \}$
$$
\begin{bmatrix}
X^N \\ P
\end{bmatrix} =
\begin{bmatrix}
\alpha_1 & \alpha_2 & \dots & \alpha_n \\
p(\alpha_1) & p(\alpha_2) & \dots & p(\alpha_n)
\end{bmatrix}
$$
> Note that $p(\alpha_{i}) = p(x_{i_{1}}x_{i_{2}} \dots x_{i_{N}})= \prod_{k=1}^N p(x_{i_{k}})$ if the source is memoryless.


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
where $$p(x_{i}|x_{i-1}\dots x_{i-m}) = (p_{\alpha,i})_{\alpha \in n^m, \; \beta \in n} = \begin{bmatrix}
p(x_{1}x_{1}\dots x_{1}\to x_{1}) & \dots & p(x_{1}x_{1}\dots x_{1}\to x_{n}) \\
\vdots  & \ddots &\vdots  \\
p(x_{n}x_{n}\dots x_{n}\to x_{1}) & \dots & p(x_{n}x_{n}\dots x_{n } \to x_{n})
\end{bmatrix}$$ 

> Note that the $p$ is the probability matrix with dimension $n^m \times n$.

## Information of a Random Event: its Mathematic Model

### (Un)-Certainty of an Event: Probability of Happening

**(Un)-certainty** of an event: Describes the possibility of such event actually happens in the view of the receiver.

Here, *the term uncertainty measures the observer's lack of confidence or predictability regarding its occurrence.*

The term "uncertainty" doesn't imply the certainty that the event will not happen; it refers to the observer's inability to confidently predict the outcome or assign high certainty to either possibility.

Uncertainty is highest not just when probabilities are low but when they are neither high nor low (close to 0.5).

Uncertainty also reflects the extent of surprise of the observer when the event happens.

### Self-Information of a Random Event: Mathematic Modeling of Uncertainty

The process of obtaining information could be interpreted as:
- Formally, there is little confidence in the occurrence of the symbol the source sends.
- After observation, we know that the source is more likely to send certain variables than others.

**Self-information** about a specific event is a mathematical interpretation of "uncertainty." The self-information of a random event $X = x_i$ is:
$$
I(x_i) = - \log p(x_i)
$$
Properties of self-information:
- *Is non-negative.* For every event $X = x_i$, $I(x_i) \geq 0$.
- *Is monotone decreasing.* If $p(X = x_i) > p(X= x_2)$ then $I(x_1) < I(x_2)$
- *Certainty*: If the event has probability 1, the event will certainly happen; thus, the self-information is 0 bit. 
- *Extreme surprise*: If the event has a probability very close to 0, we feel exceedingly astonished if the event happens. Its self-information is infinity.

### Joint Self-Information and Condition Self-Information

$(X= x|Y=y)$ and $(X = x, Y= y)$ are random events, the self-information of these events are defined as follows:

**Joint self-information**: Information of the event that the symbols appear simultaneously: 
$$
I(x_{i}y_{j}) = -\log p(x_{i }y_{j})
$$

**Condition self-information**: Information of the event that the symbol appears under certain condition: 
$$
I(x_{i}|y_{j}) = - \log p(x_{i}|y_{j})
$$


### Self-Information in the Case of S-Independent Events

When $X$ and $Y$ are s-independent, $$I(x_{i}y_{j}) = I(x_{i})+ I(y_{j})$$

### Self-Information of a Sequence

**Single source, multiple symbols**: Source sending a sequence of $N$ symbols, where each symbol appears $m_{i}$ times.
$$
I = - \sum_{x_i \in X} m_i \log p(x_i) \quad \text{where} \quad \sum_{x_i \in X} m_i = N
$$

## Mutual Information: Relation in Two Random Events

### Mutual Information: Acquire Information about one Random Event from Another Random Event

In the communication end, we want to deduce what is the original message $X$ is after obtaining the received message $Y$. In other words, we want to acquire information about $X$ when observing $Y$.

This is described as the *shared information* between the two random variables, and is defined as **mutual information**.

Interpretation of mutual information:
- Source sent $x_i$ and Receiver correspondingly received $y_j$.
- We know in the beforehand the probability model of sending $x_i$.
- We want to deduce what the source really send after observing the received $y_j$.
- The information received (i.e. uncertainty reduced) in this process = uncertainty pre-known about $x_{i}$ - uncertainty still exists after receiving $y_j$.

![](attachments/Mutual%20Information.png)

Thus, denote the mutual information (i.e. the amount of uncertainty reduced) between $x_{i}$ and $y_j$ is $I(x_{i};y_{j})$, it could be calculated as: 
$$
I(x_{i};y_{j}) = I(x_{i}) - I(x_{i}|y_{j}) =  \log \frac{p(x_{i}|y_{j})}{p(x_{i})} = \log \frac{p(x_{i}y_{j})}{p(x_{i})p(y_{j})}
$$

Therefore,
$$
I(x_{i};y_{j}) = I(x_{i})- I(x_{i}|y_{j}) = I(y_{j})- I(y_{j}|x_{i}) = I(x_{i})+ I(y_{j}) -  I(x_{i}y_{j})
$$
## Information Entropy of Single-Symbol Discrete Memoryless Sources

### Information Entropy: The Average Amount of Information Contained in Each Symbol of a Sequence

Revision: Information of a sequence of $N$ symbols:
$$
I = -\sum_{i=1}^n m_{i} \log p(x_{i})
$$

When $N$ is very large, it is difficult to count $m_i$, thus we use the *average amount of information per symbol (of the message)*: 
$$
\bar{I} = - \sum_{i=1}^n \frac{m_{i}}{N} \log p(x_{i}) \approx - \sum_{i=1}^n p(x_{i})\log p(x_{i})
$$


**Information Entropy** of a discrete source (described as a random variable), denoted as $H(X)$, is:
- The average amount of information per symbol of a sequence after the source sends the message.
- Expectation of the self-information (of the message sent by the source).
- _Measure of the average self-information (uncertainty or disorderness) of the memoryless source._
$$
H(X) = \mathbb{E}[I(X=x_{i})] = -\sum_{i=1}^n p(x_{i}) \log p(x_{i})
$$

> Note that $p(x_{i}) \log p(x_{i}) = 0$ if $p(x_{i}) = 0$.

### Joint Entropy and Condition Entropy: Between Different Sources

> For every random variable, we define mathematically the entropy as the expectation of the information of each event.

Suppose that $X$ send a sequence which is a subset of $\{{x_{1}, x_{2}, \dots, x_{n}}\}$ and $Y$ received a sequence which is a subset of $\{y_{1}, y_{2}, \dots, y_{m}\}$.

The **joint entropy** $H(XY)$ could be defined as:
$$
H(XY) = \mathbb{E}_{X, Y}[I(x_{i} y_{j})] = - \sum_{i=1}^n \sum_{j=1}^m p(x_{i}y_{j}) \log p(x_{i}y_{j})
$$


Similarly, the **condition entropy** could be defined as:
$$
H(Y|X) = \mathbb{E}_{X, Y}[I(y_{j}|x_{i})] = \sum_{i=1}^n p(x_{i}) H(Y | X = x_{i}) = - \sum_{i=1}^n \sum_{j=1}^m p(x_{i}y_{j}) \log p(y_{j}|x_{i})
$$
### Properties of the Entropy Function

1. *Non-negativity*.
2. *Symmetry*. Only how the probabilities is distributed really matters to the entropy of the random variable. We don't care which event $x_{i}$ that the probability $p_{i}$ corresponds to.
3. *Extensibility*. Symbols with very low probabilities have little contribution to the entropy of the source. (Hint: $\lim_{ \epsilon \to 0 } \epsilon \log \epsilon = 0$)
4. *Additivity*. $H(X Y) = H(X) + H(Y | X)$, when $Y$ and $X$ are s-independent, $H(XY) = H(X) + H(Y)$.
5. *Increasing*. If divide one single symbol into several symbols, the overall entropy will increase.
6. *Upper convexity*, shape like $\cap$.
7. *Extremum property*: $$H(p_{1}, p_{2}, \dots, p_{n}) \leq H\left( \frac{1}{n}, \frac{1}{n}, \dots, \frac{1}{n} \right)$$
> Imagine $H$ as $\log p$ when calculating $H(X Y)$.
### Maximum Discrete Entropy Theorem
**Maximum discrete entropy theorem**: For a discrete memoryless source $X$ whose sample space contains $n$ symbols, the entropy arrives at its maximum when all of the occurrence of the symbols is identical, i.e.
$$
H(X) \leq H_{\max} = H\left( \frac{1}{n}, \frac{1}{n}, \dots, \frac{1}{n} \right) = \log n
$$

> Note: Equal distribution signifies maximum randomness. We cannot gain any prior knowledge from the event.

### Loss Entropy and Noise Entropy: Their Practical Meanings

**Loss entropy** is $H(X|Y)$ since it is the information "lost", i.e. uncertainty left about the message of the source (sent to other destinations) during transmission.

> Notice the word "after". This signifies the deduction is divided into two steps in sequence: (1) We first observe the random variable $Y$ (2) We then want to deduce $X$ based on the fact we received $Y$.

**Noise Entropy** is $H(Y|X)$ since it is the uncertainty "added" to the observed message due to noise (which is invariant from the message of the source).

> Similarly, (1) We know we sent random variable $X$ (2) We want to figure out what $Y$ would be under the condition we sent $X$.

Therefore, the information provided by the event that simultaneously $X = x_i$ and $Y= y_j$ is:
$$
H(XY) = H(X) + H(Y|X) = H(Y) + H(X|Y)
$$
> Note: $H(XY) = H(X) + H(Y)$ if $X$ and $Y$ are s-independent.

![](attachments/Relationship%20Between%20Different%20Kinds%20of%20Entropies.png)

### Relationship Between Different Kinds of Entropies
![](attachments/Relationship.png)

![](attachments/CleanShot%202025-01-04%20at%2019.30.01@2x.png)

## Information Entropy of Multiple-Symbol Discrete Sources

### Memoryless Source and Memoryless Stationary Source

*If the source is memoryless, the entropy of the sequence is the sum of the entropy of all the elements in the sequence*: denote the sequence as $X^N = (X_1 \dots X_{N})$
$$
H(X^N) = H(X_{1}\dots X_{N}) = \sum_{i=1}^NH(X_{i})
$$
(Hint: $\log p(x_{i_{1}}\dots x_{i_{N}}) = \log p(x_{i_{1}}) + \dots + \log p(x_{i_{N}})$)

*Moreover, it the source is stationary (i.e. all probability distribution of the symbols are identical), the entropy of the sequence is the length of the sequence multiply by the entropy of a single symbol.* This is to say, 
$$
H(X_{1}) = \dots= H(X_{N}) \implies H(X^N) = \sum_{i=1}^N H(X_{i}) = NH(X)
$$


### Stationary Source with Memory

The **chain rule**:
$$
H(X^N) = H(X_{1})  + H(X_{2}|X_{1}) + \dots + H(X_{N}| X_{1}\dots X_{N-1}) = \sum_{i}^N H(X_{i}|X^{i-1})
$$

(Hint: proof is similar, use the property of $\log p$)

### Average Symbol Entropy

The **average symbol entropy** is the _amount of information contained (averagely) in each symbol of the symbol sequence with length $N$._ This is a _statistical average._

$$
H_{N}(X) = \frac{H(X^N)}{N}
$$

When the source is memoryless, clearly $H_{N}(X) = H(X)$.

The average symbol entropy has the following properties:
- *Decreases as $N$ increases*: Any new term is smaller than all of its former terms, thus the average is monotone decreasing. i.e. (Note: $H_{0}(X)$ is the entropy of a _single-symbol equal-probability distributed memoryless source_, deduced from the maximum discrete entropy theorem) $$H(X_{N}| X_{1}\dots X_{N-1}) \leq H(X_{N-1}| X_{1}\dots X_{N-2})\leq \dots \leq H(X_{2}|X_{1}) \leq H(X_{1})= H(X) \leq H_{0}(X)$$
- $N_{1} > N_{2}$, $H_{N_{1}}(X) < H_{N_{2}}(X)$
- *Existence of limit when the sequence is infinitely long*.

The first property implies that _more memory generally leads to lower average entropy per symbol._

The existence is defined as:

$$
H_{\infty}(X) = H_{N\to \infty} (X^N ) =H(X_{N}|X ^{N-1})
$$

Example: Calculate $H_{\infty}$ of a stationary first-order source:
$$
\begin{align}
H_{\infty}(X) = H(X_{N}|X^{N-1}) &= \sum_{X_{1}}\dots \sum_{X_{N}} p(x_{1}\dots x_{N}) \log p (x_{N}| x_{1}\dots x_{N-1})  \\
&= \sum _{X_{1}} \dots \sum_{X_{N}} p(x_N|x_{1}\dots x_{N-1}) p(x_{1}\dots x_{N-1}) \log p(x_{N}| x_{1}\dots x_{N-1})  \\
&= \sum_{X_{N}} \sum_{X_{N-1}} p(x_{N}|x_{N-1}) p(x_{N-1}) \log p(x_{N}|x_{N-1}) \sum_{X_{1}}\dots \sum_{X_{N-1}} p(x_{1}\dots x_{N-2}) \\
&=  \sum_{X_{N-1}} p(x_{N-1}) \sum_{X_{N}}p(x_{N}|x_{N-1})  \log p(x_{N}|x_{N-1})
\end{align}
$$
### Transmission Efficiency and Source Redundancy

**Transmission Efficiency** is 
- the efficiency of the source.
- (realistic case) the entropy needed to be transmitted divided by the (imagined case) maximum entropy could be delivered using the same sets of symbols: 
$$
\eta = \frac{H_{\infty}(X)}{H_{0}(X)}
$$

On the contrary, **Source redundancy** is the inefficiency of the source:
$$
\gamma = 1- \eta
$$
## Information Entropy of Continuous Sources

### Definitions of Entropies

Properties:
- The information entropy of any continuous sources is infinite and non-negative.
- But, between two sources, if they use the same sampling method, their difference is finite.

$$
H(X) = - \int_{\mathbb{R}} p_{X}(x) \log p_{X}(x) \mathrm{d}x
$$

Example: A continuous source modeled by a normally distributed random variable: $X \sim N (\mu, \sigma^2)$ where 
$$
p_{X}(x) = \frac{1}{\sqrt{ 2 \pi \sigma^2 }} \exp\left( - \frac{(x- \mu)^2}{2 \sigma^2} \right)\text{ d}x
$$
Its entropy could be calculated as using integration by parts:

$$
\begin{align}
H(X) &= - \int _{- \infty}^{\infty} p_{X}(x) \log \left( \frac{1}{\sqrt{ 2 \pi \sigma^2 }} \exp\left( -\frac{(x-\mu)^2}{2 \sigma^2} \right) \right) \text{ d}x  \\
&= - \int _{- \infty}^{\infty} p_{X}(x) (- \log \sqrt{ 2 \pi \sigma^2 })\text{ d}x + \int _{-\infty}^{\infty} \log e \left(  \frac{(x-\mu)^2}{2\sigma^2} \right) p_{X}(x)\text{ d}x \\
&= \log \sqrt{ 2 \pi \sigma^2 } + \frac{\log e}{\sqrt{ 2 \pi \sigma^2 }}\int_{-\infty}^{\infty}  \left( \frac{(x-\mu)^2}{2 \sigma^2} \right) \exp\left( - \frac{(x-\mu)^2}{2 \sigma^2} \right)\text{ d}x  \\
&=^{y = \frac{(x- \mu)^2}{ \sqrt{ 2 } \sigma}} \log \sqrt{ 2 \pi \sigma^2 } + \frac{\log e}{\sqrt{ \pi }} \int_{- \infty}^{\infty} e^{-y^2} y^2\text{ d}y \\
&=^{u =  e^{-y^2}, \; v=- y/2}\log \sqrt{ 2 \pi \sigma^2 }  + \frac{\log e}{\sqrt{ \pi }} \left( \left[ - \frac{e^{-y^2}}{2} y \right]_{- \infty}^{\infty} -\int _{-\infty}^{\infty} \left( - \frac{e^{-y^2}}{2} \text{} \right) \text{ d}y \right) \\
&= \log \sqrt{ 2 \pi \sigma^2 } + \frac{\log e}{\sqrt{ \pi }} \frac{1}{2} \int_{- \infty}^\infty e^{-y^2}\text{ d}y = \frac{1}{2}\log 2 \pi e \sigma^2
\end{align}
$$

Correspondingly, **joint entropy** and **conditional entropy** is:
$$
H(X Y) = - \iint_{\mathbb{R}} p_{XY }(x y) \log p_{X Y}(xy)\mathrm{d}x\mathrm{d}y
$$
and 
$$
H(X| Y) = - \iint_{\mathbb{R}} p_{X Y}(xy) \log p_{X|Y}(x|y)\mathrm{d}x\mathrm{d}y
$$

### Maximum Continuous Entropy Sources

Reminder: For discrete sources, the entropy reaches at its maximum when the occurrence probabilities of all $n$ symbols are identical.

However,
- Since entropy is inherently infinite, there are no maximum entropies for continuous sources.
- Thus, we consider maximum entropy under certain conditions.

**Amplitude of the output of the source is limited**: Maximum reached when uniformly distributed: 
$$
\arg \max_{p_{X}(x)}H(X) = \begin{cases}
\frac{1}{b-a} \quad &a\leq x \leq b \\
0 \quad &\text{others}
\end{cases}
$$
**Average power of the output of the source is limited**: Maximum reached when normally distributed:
$$
\arg \max_{p_{X}(x)}H(X) = \frac{1}{\sqrt{ 2 \pi \sigma^2 }} \exp\left( - \frac{(x-\mu)^2}{2 \sigma^2} \right)
$$



