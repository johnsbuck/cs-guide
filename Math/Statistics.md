# Statistics
## Mean
The **mean** is the average found in a data set.

The mean can be defined as:
$$
\overline{X} = \frac{1}{n}\sum_{i=1}^{n} x_{i}
$$
Where the terms are as followed:
  * $X$ is the data set.
  * $n$ is the number of values in vector $X$.
  * $x_{i}$ is the ith value in vector $X$.

## Standard Deviation
The **standard deviation** is a measure in the amount of variation or *dispersion* in a data set.

$$
\sigma = \sqrt{\frac{\sum_{i=1}^{n} (x_{i} - \overline{X})^{2}}{n}}
$$

## Variance
The **variance** is a measure in the amount of variation or *dispersion* in a data set. It is the square of the standard deviation, which has many advantages, including always being positive.

$$
\sigma^{2} = Var(X) = \frac{1}{n-1}\sum_{i=1}^{n}(x_{i} - \overline{X})^{2}
$$

## Covariance
**Covariance** is the measure of joint variability between two variables.
$$
\sigma_{XY} = Cov(X, Y) = \frac{1}{N}\sum_{i=1}^{n}(x_{i} - \overline{X})(y_{i} - \overline{Y})
$$

$$
Cov(X, X) = Var(X)
$$

### Covariance Matrix
$$
\displaylines{
X = (x_{1}, x_{2}, ..., x_{n})
\\
x_{i} = <a_{1}, a_{2}, ..., a_{p}>
\\
C = \begin{bmatrix}
Var(x_{1}) & Cov(x_{1}, x_{2}) & ... & Cov(x_{1}, x_{n}) \\
Cov(x_{2}, x_{1}) & Var(x_{2}) & ... & Cov(x_{n}, x_{n}) \\
... & ... & ... & ... \\
Cov(x_{n}, x_{1}) & Cov(x_{n}, x_{2}) & ... & Var(x_{n}) \\
\end{bmatrix}
}
$$

## Distributions
### Uniform
|                |                         |
| -------------- | ----------------------- |
| **Parameters** | $-\inf < a < b < \inf$  |
| **Support**    | $x \in [a, b]$          |
| **Mean**       | $\frac{1}{2}(a+b)$      |
| **Variance**   | $\frac{1}{12}(b-a)^{2}$ |
| **Median**     | $\frac{1}{2}(a+b)$      |
| **Mode**       | Any value in $(a,b)$    |

#### Probability Density Function
$$
f(x) = \begin{cases}
\frac{1}{b-a}\quad\text{for}\;a \leq x \leq b\\
0\quad\quad\;\text{for}\;x < a\;|\;x > b
\end{cases}
$$

#### Cumulative Density Function
$$
F(x) = \begin{cases}
0\quad\quad\;\text{for}\;x < a \\
\frac{x-a}{b-a}\quad\text{for}\;a \leq x \leq b \\
1\quad\quad\;\text{for}\;x > b
\end{cases}
$$

### Normal

|                |                                                                                                    |
| -------------- | -------------------------------------------------------------------------------------------------- |
| **Parameters** | $\displaylines{\mu \in \mathbb{R} = \text{mean}\\\sigma^{2}\in \mathbb{R}_{>0} = \text{variance}}$ |
| **Support**    | $x \in \mathbb{R}$                                                                                 |
| **Mean**       | $\mu$                                                                                              |
| **Variance**   | $\sigma$                                                                                           |

#### Probability Density Function
$$
f(x) = \frac{1}{\sigma\sqrt{2\pi}}e^{-\frac{1}{2}(\frac{x - \mu}{\sigma})^{2}}
$$

#### Cumulative Density Function
$$
\displaylines{
F(x) = \frac{1}{2}[1 + \text{erf}(\frac{x - \mu}{\sigma\sqrt{2}})]
\\
\text{erf}(x) = \frac{2}{\sqrt{\pi}}\int_{0}^{x}e^{-t^{2}/2}dt
}
$$

> [!NOTE]
> $\text{erf}(x)$ is the *Gauss Error Function*.

#### Central Limit Theorem
> [!INFO]- Reference
> [Wikipedia](https://en.wikipedia.org/wiki/Central_limit_theorem)

The **central limit theorem** (**CLT**) establishes that, in many situations, when [independent random variables](https://en.wikipedia.org/wiki/Statistical_independence "Statistical independence") are summed up, their properly [normalized](https://en.wikipedia.org/wiki/Normalization_(statistics) "Normalization (statistics)") sum tends toward a [normal distribution](https://en.wikipedia.org/wiki/Normal_distribution "Normal distribution") even if the original variables themselves are not normally distributed.

### Geometric
|                | Number of Trials                         | Number of Failures                       |
| -------------- | ---------------------------------------- | ---------------------------------------- |
| **Parameters** | $0 < p \leq 1$ success probability       | $0 < p \leq 1$ success probability       |
| **Support**    | $k$ trials where $k \in \{1, 2, 3,...\}$ | $k$ failures where $k \in {0,1,2,3,...}$ |
| **PMF**        | $(1-p)^{k-1}p$                           | $(1-p)^{k}p$                             |
| **CDF**        | $1-(1-p)^{k}$                            | $1-(1-p)^{k+1}$                          |
| **Mean**       | $1/p$                                    | $(1-p)/p$                                |
| **Variance**   | $(1-p)/(p^{2})$                          | <-                                       |
| **Mode**       | 1                                        | 0                                        |

### Binomial
|                |                                                                                                                    |
| -------------- | ------------------------------------------------------------------------------------------------------------------ |
| **Parameters** | $\displaylines{n \in \{0,1,2,...\} : \text{Number of trials}\\p \in [0, 1] : \text{Success prob. for each trial}}$ |
| **Support**    | $k \in \{0,1,...,n\} : \text{Number of successes}$                                                                 |
| **Mean**       | $np$                                                                                                               |
| **Variance**   | $np(1-p)$                                                                                                          |
| **Mode**               | $\lfloor(n+1)p\rfloor$                                                                                                                   |

#### Probability Mass Function
$$
Pr(k;n, p) = Pr(X=k) = {n \choose k}p^{k}(1-p)^{n-k} 
$$

#### Cumulative Density Function
$$
F(k;n, p) = Pr(X \leq k) = \sum_{i=0}^{\lfloor k \rfloor} {n \choose i} p^{i} (1-p)^{n-i}
$$
