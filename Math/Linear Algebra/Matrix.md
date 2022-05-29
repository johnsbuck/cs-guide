---
tags:
  - matrix
  - linear-algebra
  - inverse
  - transpose
---

# Matrix
## Definition
A **matrix** is typically define as a 2-dimensional, rectangular array or table, arranged in *rows* or *columns*.

$$
X = \begin{bmatrix}
x_{11} & x_{12} & ... & x_{1m} \\
x_{21} & x_{22} & ... & x_{2m} \\
... & ... & ... & ... \\
x_{n1} & x_{n2} & ... & x_{nm}
\end{bmatrix}
$$

## Matrix Multiplication
$$
\displaylines{
AX = Y
\\
A = \begin{bmatrix}
a_{11} & a_{12} & a_{13} \\
a_{21} & a_{22} & a_{23} \\
a_{31} & a_{32} & a_{33}
\end{bmatrix}
\\
X = \begin{bmatrix}
x_{1} \\
x_{2} \\
x_{3}
\end{bmatrix}
\\
Y = \begin{bmatrix}
a_{11}x_{1} + a_{12}x_{2} + a_{13}x_{3} \\
a_{21}x_{1} + a_{22}x_{2} + a_{23}x_{3} \\
a_{31}x_{1} + a_{32}x_{2} + a_{33}x_{3} \\
\end{bmatrix}
}
$$

> [!NOTE]
> Can only multiply $(n \times b)$ matrices to $(b \times m)$ matrices.
> 
> $AX = (n \times b) (b \times m) = (n \times m)$

## Identity Matrix
$$
I_{3\times3} = \begin{bmatrix}
1 & 0 & 0 \\
0 & 1 & 0 \\
0 & 0 & 1 \\
\end{bmatrix}
$$

### Identity Transformation
$$
IX = X
$$

## Transpose
$$
\displaylines{
X = \begin{bmatrix}
x_{11} & x_{12} & ... & x_{1m} \\
x_{21} & x_{22} & ... & x_{2m} \\
... & ... & ... & ... \\
x_{n1} & x_{n2} & ... & x_{nm}
\end{bmatrix}
\\
X^{T} = \begin{bmatrix}
x_{11} & x_{21} & ... & x_{m1} \\
x_{12} & x_{22} & ... & x_{m2} \\
... & ... & ... & ... \\
x_{1n} & x_{2n} & ... & x_{mn} \\
\end{bmatrix}
}
$$

$$
(X^{T})^{T} = X
$$

## Inverse Matrix
### Cramer's Rule
For $Ax =b$, where the $n \times n$ matrix $A$ has a non-zero determinant and vector $x=(x_{1},...,x_{n})^{T}$ is the column vector of variables, the system of linear equations has a unique solution given by:

$$
x_{i} = \frac{det(A_{i})}{det(A)} \quad i=1,...,n
$$

### Cofactor Matrix
A **minor** of a matrix $X$ is the determinant of some smaller square matrix by removing one or more of $X$'s columns and rows.

A **cofactor** is the minor of a matrix, multiplied by $(-1)^{x+y}$ where $x$ is the row number and $y$ is the column number.

To find the **cofactor matrix**, we eliminate row $x$ and row $y$ for each element in the cofactor matrix.

$$
C = \begin{bmatrix}
C_{11} & C_{12} & ... & C_{1n} \\
C_{21} & C_{22} & ... & C_{2n} \\
... & ... & ... & ... \\
C_{n1} & C_{n2} & ... & C_{nn} \\
\end{bmatrix}
$$

#### Cofactor Example
$$
\displaylines{
A = \begin{bmatrix}
1 & 4 & 7 \\
3 & 0 & 5 \\
-1 & 9 & 11 \\
\end{bmatrix}
\\
M_{2,3} = det\begin{bmatrix}
1 & 4 & \bbox[5px, border: 1px solid white]{} \\
\bbox[5px, border: 1px solid white]{} & \bbox[5px, border: 1px solid white]{} & \bbox[5px, border: 1px solid white]{} \\
-1 & 9 & \bbox[5px, border: 1px solid white]{} \\
\end{bmatrix}
= det\begin{bmatrix}
1 & 4 \\
-1 & 9 \\
\end{bmatrix}
= 9 - (-4) = 13
\\
C_{2,3} = (-1)^{2+3}(M_{2,3}) = -13
}
$$

### Adjunct Matrix
> [!INFO]- Tags
> #adjugate #adjoint

The **adjunct matrix** (sometimes referred as the **classical adjoint matrix** or **adjugate**) is the transpose of the cofactor matrix of the same referred matrix 

(i.e the adjugate of $A$ is the transpose of the cofactor matrix of $A$).

$$
adj(A) = C^{T}
$$

### Definition
The **inverse matrix** is the inverse of matrix $X$ such that:

$$
XX^{-1} = X^{-1}X = I_{n}
$$

> [!NOTE]
> Only applies to square ($n \times n$) matrices with a non-zero determinant.

You can find the inverse matrix using the following equation:
$$
X^{-1} = \frac{1}{det(X)}(adj(X))
$$

## Determinant
> [!NOTE]
> *Review [[Vector#Determinant]] for more information.*

$$
det(X) = \begin{vmatrix}
a & b \\
c & d \\
\end{vmatrix}
= ab - dc
$$

$$
det(X) = \begin{vmatrix}
x_{11} & x_{12} & x_{13} \\
x_{21} & x_{22} & x_{23} \\
x_{31} & x_{32} & x_{33}
\end{vmatrix}
=
x_{11} \begin{vmatrix}
x_{22} & x_{23} \\
x_{32} & x_{33}
\end{vmatrix}
- x_{12} \begin{vmatrix}
x_{21} & x_{23} \\
x_{31} & x_{33}
\end{vmatrix}
+ x_{13} \begin{vmatrix}
x_{21} & x_{22} \\
x_{31} & x_{32}
\end{vmatrix}
$$

> [!NOTE] Denotation
> The determinat can be denoted as $det(A)$, $det A$, or $|A|$.
> For clarity and consistency, we will be using $det(A)$ as the determinant of A. 

## Cross-Product
> [!NOTE]
> *Review* [[Vector#Cross Product]] for more information.

$$
A \times B = -B \times A
$$

$$
A \times A = 0
$$

## Rotation Matrix
> [!INFO]+ Reference
> [Freya Holmér Explanation](https://www.youtube.com/watch?v=7j5yW5QDC2U)

$$
\displaylines{
V = \begin{bmatrix}
x \\
y
\end{bmatrix}
\\
V^{'} = \begin{bmatrix}
cos\theta & -sin\theta \\
sin\theta & cos\theta
\end{bmatrix}
\begin{bmatrix}
x \\
y
\end{bmatrix}
}
$$
## Line/Circle Intersection
> [!INFO]+ References
> [Freya Holmér Explanation](https://acegikmo.notion.site/Line-Circle-Intersection-0d259641b1b544e88557a61a9bd85d90)

