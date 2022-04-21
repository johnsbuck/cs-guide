---
tags:
  - pca
  - principal-component-analysis
  - dimensionality-reduction
---

# Principal Component Analysis
**Principal Component Analysis (PCA)** is a form of *linear* dimensionality reduction of a dataset that is incredibly useful in data analytics, computer vision, and is sometimes paired with other techniques as a form of *relaxant*, or pre-processing. A clear example of this is when using [t-SNE](t-SNE), you can use PCA to lower the initial number of dimensions to simplify computation.

## Explanation
> [!NOTE]+
> [StackExchange Answer](https://stats.stackexchange.com/a/140579)
> Easily the best explanation for PCA.
> 

### Math Overview
> [!INFO]- Related Material
> [[References#^refdeeplearningbook]]
> [Wikipedia](https://en.wikipedia.org/wiki/Principal_component_analysis)

#### Define the Dataset
Let $X$ be the following:

$$
X = \begin{bmatrix}
a_{11} & a_{12} & ... & a_{1p} \\
a_{21} & a_{22} & ... & a_{2p} \\
... & ... & ... & ... \\
a_{n1} & a_{n2} & ... & a_{np} \\
\end{bmatrix}
$$
Where $n$ is the number of samples (or rows) and $p$ is the number of features/dimensions (or columns).

#### Center Each Feature
Find the mean, $C_{j}$, for each feature and subtract each value, $X_{ij}$, in $X$ to center each feature.
$$
C = [\overline{X^{T}_{1}}, \overline{X^{T}_{2}}, ..., \overline{X^{T}_{p}}]
$$
$$
X_{ij} = X_{ij} - C_{j}
$$

#### Find the Principal Component
>[!QUOTE]
> PCA is defined as an [orthogonal](https://en.wikipedia.org/wiki/Orthogonal_transformation "Orthogonal transformation") [linear transformation](https://en.wikipedia.org/wiki/Linear_transformation "Linear transformation") that transforms the data to a new [coordinate system](https://en.wikipedia.org/wiki/Coordinate_system "Coordinate system") such that the greatest variance by some scalar projection of the data comes to lie on the first coordinate (called the first principal component), the second greatest variance on the second coordinate, and so on.[\[cite\]](https://en.wikipedia.org/wiki/Principal_component_analysis#cite_note-Jolliffe2002-10)

To perform a linear transformation, we need at least one $p$-dimensional vector of weights, $w$.
$$
w = \begin{pmatrix}
w_{1}, w_{2}, ..., w_{p}
\end{pmatrix}
$$
$w$ is also constrained to be a *unit vector*, or *normalized vector*.

We can give each each row vector, $x_{i}$, of $X$ a *principal component score*, $t_{i}$.
$$
t_{i} = x_{i} \cdot w 
$$
Now that we have a score to use, we can focus on maximizing the variance of our linear transformation. In order to do this, $w$ has to satisfy the following:

$$
w = arg \underset{||w||=1}{max} \sum_{i}(t)^{2}_{i}
$$
$$
= arg\underset{||w||=1}{max} \sum_{i} (x_{i} \cdot w)^{2}
$$
$$
= arg\underset{||w||=1}{max} ||Xw||^{2}
$$
$$
= arg\underset{||w||=1}{max} w^{T}X^{T}Xw
$$
Because $w$ is a unit vector, it also satisfies the following:
$$
= argmax \frac{w^{T}X^{T}Xw}{w^Tw}
$$
The optimal $w$ for this equation is the largest eigenvector of the cross-product matrix:
$$
X^TX
$$

>[!NOTE]
>The covariance matrix is considered to be $\frac{X^{T}X}{n}$.

#### Further Components
Imagine instead of one vector of weights, we have a set of weights. Let the size of this set be size $l$ and the *k-th* vector be $w_{(k)}$.

The *k-th* principal component can be found by subtracting the previous principal components from $X$.
$$
\hat{X}_{k} = X - \sum_{s=1}^{k-1} Xw_{(s)}w_{(s)}^{T}
$$
We can repeat the process for $\hat{X}_{k}$ as we did with the first principal component.
$$
= argmax \frac{w^{T}\hat{X}^{T}_{k}\hat{X}_{k}w}{w^Tw}
$$
