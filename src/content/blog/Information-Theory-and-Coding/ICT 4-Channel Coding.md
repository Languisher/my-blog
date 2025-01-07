## Table of contents

**Channel coding** is the process of ensuring reliable data transmission over noisy communication channels. The key problem in this chapter is _to reduce the error rate as small as possible_.

## Error Rate

Notations that relate to a decoder:
- The original symbols sent by the source is $x \in A = \{a_{1}, \dots, a_{n}\}$
- The decoder receives a symbol $y \in B = \{b_{1},\dots,b_{m}\}$.
- The decoding mapping function $F:B \to A$. Specifically, $F(b_{j}) = a_{i}$.

The error rate is the possibilities of other cases except for the "correct" code (according to our mapping function).

The error rate of decoding the received symbol $b_{j}$:
$$
P_{E}(b_{j}) =p(e|b_{j}) =  1-p(F(b_{j})|b_{j}) = 1 - p(a_{i}|b_{j})
$$
The average decoding error rate $P_{E}$ is a function of the decoding mapping function $F$:
$$
P_{E} = \mathbb{E}_{B}[P_{E}(b_{j})] = \sum_{j=1}^m p(b_{j}) p(e|b_{j}) 
$$
## How to Decode 

Our goal is to find the optimal decoding mapping function to minimize the decoding error rate, in other words, to find
$$
F = \arg \min_{F:B \to A} P_{E}
$$
### Optimal Encoding or Maximum Error Probability Criterion

To minimize $P_{E}$, is equivalent to minimize $p(e|b_{j})$ and thus to maximize $p(F(b_{j})|b_{j})$. Therefore, we choose the optimal $a^*$ for each $b_{j}$ as:
$$
p(a^*|b_{j}) \geq p(a_{i}|b_{j}), \quad a_{i} \ne a^*
$$
The $p(a_{i}|b_{j})$ is _a posterior_ probability which could not be known in advance. Thus we use the joint possibility to select the optimal $a^*$ for each $b_{j}$ (i.e. select the line for each column that has the highest probability)
$$
p(a^*|b_{j})p(b_{j}) \leq p(a_{i}|b_{j}) p(b_{j}) \implies p(a^* b_{j}) \leq  p(a_{i}b_{j}), \quad a_{i} \ne a^*
$$
The average decoding error rate is:
$$
P_{E} = \sum_{j=1}^m p(b_{j})(1- p(a^*|b_{j})) = 1 - \sum_{j=1}^m p(a^*b_{j})
$$
### Maximum Likelihood Decoding Criterion

This is a straightforward approach: for each $b_{j}$, it is encoded to the symbol with the maximum value of the $j$-th column of the _transition probability matrix of the channel_:
$$
p(b_{j}|a^*) \geq p(b_{j}|a_{i}), \quad a_{i}\ne a^*
$$

According to the Bayes' theorem, the average decoding error is:
$$
P_{E} = \sum_{j=1}^m p(b_{j})p(e|b_{j}) = \sum_{a_{i} \ne a^*} p(a_{i})\sum_{j=1}^m p(b_{j}|a_{i})
$$

### Minimum Hamming Distance Decoding

The **Hamming Distance** between two codewords defined as the number of positions on which the corresponding bits are different:
$$
D(c_{i},c_{j}) = \sum_{k=1}^nc_{i k} \oplus c_{j k}
$$

> Here, $\oplus$ represents XOR operation.

Example:
- For codewords **11000** and **00000**, the Hamming distance is 2 because they differ in the second and third positions.
- For codewords **11000** and **11111**, the Hamming distance is 3 because they differ in the second, fourth, and fifth positions.

It is decoded to the codeword that has the minimum Hamming distance to the received word.

## How to Encode

A proper coding method can also decrease the probability rate.

### Example: Three-times Extended Channel of the BSC

> BSC is short for Binary Symmetric Channel

For example, suppose a binary symmetric channel that:
$$
p(A=0,B=0)  =\bar{p}, \quad p(A=0, B=1) = p
$$
Therefore, by duplicating the symbol three times, coding $[0,1]$ as $[000, 111]$, the probability transition matrix of the destination receiving $[000, 001, 010, 011, 100, 101, 110, 111]$ is
$$
P = \begin{bmatrix}
\bar{p}^3 & \bar{p}^2 p & \bar{p}p^2 & \bar{p}p^2 & \bar{p}p^2 & \bar{p}p^2 & \bar{p}p^2 & p^3  \\
p^3  & \bar{p}p^2 & \bar{p}p^2 & \bar{p}^2p & \bar{p}p^2 & \bar{p}^2p & \bar{p}^2p & \bar{p}^3
\end{bmatrix}
$$

Using the maximum likelihood decoding method, the error rate is
$$
P_{E} = p(000) (\bar{p}p^2 + \bar{p}p^2+\bar{p}p^2 + p^3) + p(111)(p^3 + \bar{p}p^2 + \bar{p}p^2 +\bar{p}p^2) = 3\bar{p}p^2 + p^3 \ll P_{E}'= p
$$

Worthy to notice that the information transmission rate decreases.

## Linear Block Code

**Linear block code** is a type of code that 
- Allows for _error detection_ and _correction_ during transmission.
- Processed in _fixed-size blocks_.
- The encoding process satisfies the probabilities of _linear algebra_.

A $(n,k)$ **linear block code** implies that
- The data is divided into blocks of fixed-length $k$
- Mapped into a codeword of length $n$, with $k$ bits identical to the original data
- **Parity bits** has length $n-k$

> We use $(n,k) =(6,3)$ linear block code for example.
### Encoding

Suppose the original data is $(m_{2}m_{1}m_{0})$ and the output codeword is $(c_{5}c_{4}c_{3}c_{2}c_{1}c_{0})$.

We encode as follows: ($c_{3}$ to $c_{5}$ correspond to the original data and $c_{0}$ to $c_{2}$ are parity bits)
$$
\begin{cases}
c_{5} &= m_{2}  \\
c_{4} &= m_{1} \\
c_{3} &= m_{0} \\
c_{2} &= m_{2} + m_{1} \\
c_{1} &= m_{2}+ m_{1} +m_{0} \\
c_{0} &= m_{2}+m_{0}
\end{cases}
$$
which could be transformed using linear algebra where $G = (g_{ij})_{i \in [\![2, 0]], j \in [\![5, 0]\!]}$: 
$$
C = mG \iff \begin{bmatrix}
c_{5}c_{4}\dots c_{0}
\end{bmatrix}= \begin{bmatrix}
m_{2}m_{1}  m_{0}
\end{bmatrix}  \begin{bmatrix}
1 & 0 & 0 & 1  & 1 & 1 \\
0 & 1 & 0 & 1 & 1 & 0  \\
0 & 0 & 1 & 0 & 1 & 1
\end{bmatrix}
$$

> Be careful with the indices, I write it this way on purpose. The row starts from $m_{2}$ to $m_{0}$, and the array from left to right is $c_{5}$ to $c_{0}$.

Here, $G$ is the **generating matrix**:
$$
G = \begin{bmatrix}
I_{3} & P 
\end{bmatrix} \quad \text{where} \quad P = \{0, 1\}_{3, 3}
$$

### Verify a Codeword

By the linearity of the relationships, by constructing linear equations between symbols of the codewords, we can verify a codeword is valid or not.

Construct the **check matrix** as:
$$
H = \begin{bmatrix}
P^T & I_{3}
\end{bmatrix}
$$

If the received codeword $R$ satisfies 
$$
RH^T = 0
$$
Then it is valid.

