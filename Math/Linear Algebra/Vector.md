---
tags:
  - vector
  - linear-algebra
  - dot-product
  - cross-product
  - distance
  - norm
---

# Vector
> [!INFO]- References
> [MIT OCW Lecture](https://ocw.mit.edu/courses/18-02-multivariable-calculus-fall-2007/649253ba60d11b0598cc58e9dcf58142_lec_week1.pdf)

## Definition
$$
\overrightarrow{X} = <x_{1}, x_{2}, ..., x_{n}>
$$

## Addition
$$
\overrightarrow{A} + \overrightarrow{B} = <a_{1} + b_{1}, a_{2} + b_{2}, ..., a_{n} + b_{n}>
$$

## Dot Product
$$
\overrightarrow{A} \cdot \overrightarrow{B} = \sum_{i=1}^{n}a_{i}b_{i}
$$

#### Law of Cosines
$$
\overrightarrow{A}\cdot\overrightarrow{B} = |\overrightarrow{A}|\;|\overrightarrow{B}|\;cos(\theta)
$$

## Determinant
Determinants are only defined for **square matrices**.

$$
det(\overrightarrow{A}, \overrightarrow{B}) = \begin{vmatrix}
a_{1} & a_{2} \\
b_{1} & b_{2}
\end{vmatrix}
= a_{1}b_{2} - a_{2}b_{1}
$$

$$
det(\overrightarrow{A}, \overrightarrow{B}, \overrightarrow{C}) = \begin{vmatrix}
a_{1} & a_{2} & a_{3} \\
b_{1} & b_{2} & b_{3} \\
c_{1} & c_{2} & c_{3}
\end{vmatrix}
= a_{1} \begin{vmatrix}
b_{2} & b_{3} \\
c_{2} & c_{3}
\end{vmatrix}
- a_{2} \begin{vmatrix}
b_{1} & b_{3} \\
c_{1} & c_{3}
\end{vmatrix}
+ a_{3} \begin{vmatrix}
b_{1} & b_{2} \\
c_{1} & c_{2}
\end{vmatrix}
$$

## Unit Vector
### Definition
A vector of length 1. 

A unit vector can be found from any non-unit vector $u$ by performing the following operation: 
$$
\hat{u} = \frac{u}{|u|}
$$
Where $\hat{u}$ is the unit vector and $|u|$ is the vector norm.

## Cross Product
> [!INFO]- References
> [Wikipedia](https://en.wikipedia.org/wiki/Cross_product)

### Definition
The cross product is defined by:
$$
\overrightarrow{A} \times \overrightarrow{B} = |\overrightarrow{A}|\;|\overrightarrow{B}|\;sin(\theta)\;\hat{N}
$$
Where the terms are the following:
  * $|\overrightarrow{A}|$ and $|\overrightarrow{B}|$ are the norms of vectors $\overrightarrow{A}$ and $\overrightarrow{B}$.
  * $\hat{N}$ is the unit vector perpendicular to the plane containing $\overrightarrow{A}$ and $\overrightarrow{B}$.

> [!NOTE]
> If $\overrightarrow{A}$ and $\overrightarrow{B}$ are parallel ($\theta \in \{0, \pi\}$), the cross product is a zero vector.
> 
> If $\overrightarrow{A}$ and $\overrightarrow{B}$ are perpendicular ($\theta \in \{\frac{\pi}{2}, \frac{3\pi}{2}\}$), the cross product is $|\overrightarrow{A}|\;|\overrightarrow{B}|$. 

$$
\overrightarrow{A} \times \overrightarrow{B} = -\overrightarrow{B} \times \overrightarrow{A}
$$

$$
\overrightarrow{A} \times \overrightarrow{A} = 0
$$

### Visual Aid
| ![[right-hand-rule.png]] | ![[cross_product.gif]] |
| ------------------------ | ---------------------- |
| Right-Hand Rule          | Cross-Product Example  |

### Matrix Notation
In $\mathbb{R}^{3}$, the cross product is defined by:
$$
\overrightarrow{A} \times \overrightarrow{B} = \begin{vmatrix}
\hat{i} & \hat{j} & \hat{k} \\
a_{1} & a_{2} & a_{3} \\
b_{1} & b_{2} & b_{3}
\end{vmatrix}
=
\hat{i} \begin{vmatrix}
a_{2} & a_{3} \\
b_{2} & b_{3}
\end{vmatrix}
- \hat{j} \begin{vmatrix}
a_{1} & a_{3} \\
b_{1} & b_{3} \\
\end{vmatrix}
+ \hat{k} \begin{vmatrix}
a_{1} & a_{2} \\
b_{1} & b_{2}
\end{vmatrix}
$$

In $\mathbb{R}^{2}$, the cross product of $u=(u_{x}, u_{y})$ and $v = (v_{x}, v_{y})$ is:

$$
u \times v = det(u, v)
$$

## Distances 
### Manhattan (L1-Distance)
$$
D_{1}(X, Y) = \sum_{i=1}^{n} |x_{i} - y_{i}|
$$
### Euclidean (L2-Distance)
$$
D_{2}(X, Y) = \sqrt{\sum_{i=1}^{n}|x_{i} - y_{i}|^{2}}
$$
### P-Distance
$$
D_{p}(X, Y) = (\sum_{i=1}^{n}|x_{i} - y_{i}|^{p})^{\frac{1}{p}}
$$

### Chebyshev (L$\infty$-Distance)
$$
D_{\infty}(X,Y) = \underset{p\rightarrow{}\infty}{lim} (\sum_{i=1}^{n}|x_{i} - y_{i}|^{p})^{\frac{1}{p}}
$$

$$
D_{\infty}(X,Y) = \underset{i}{max}(|x_{i} - y_{i}|)
$$

## Norms
### Definition
The **norm** is the length of a given vector. It can also be defined as the distance between a vector and a 0-vector.

$$
|X|_{p} = D_{p}(X, 0) = (\sum_{i=1}^{n}|x_{i} - 0|^{p})^{\frac{1}{p}}
$$

> [!NOTE] Denotation
> Typically, the default norm is L2-Norm (Euclidean), which can have several denotations.
> This includes $|X|$, $||X||$, $|X|_{2}$, and $||X||_{2}$.
> 
> We will be using $|X|$, as well as $|X|_{p}$ for any p-norm.

### Manhattan (L1-Norm)
$$
|X|_{1} = \sum_{i=1}^{n} |x_{i}|
$$
### Euclidean (L2-Norm)
$$
|X|_{2} = \sqrt{\sum_{i=1}^{n}x_{i}^{2}}
$$
### P-Norm
$$
|X|_{p} = (\sum_{i=1}^{n}|x_{i}|^{p})^{\frac{1}{p}}
$$

### Chebyshev (L$\infty$-Norm)
$$
|X|_{\infty} = \underset{p\rightarrow{}\infty}{lim} (\sum_{i=1}^{n}|x_{i}|^{p})^{\frac{1}{p}}
$$

$$
|X|_{\infty} = \underset{i}{max}(|x_{i}|)
$$

