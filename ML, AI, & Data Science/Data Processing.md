---
tags:
  - data-processing
  - normalization
  - standardization
  - scaling
  - feature
  - clustering
---

#  Data Processing Techniques

## Basic Techniques
### Normalization
$$
X' = \frac{X - \mu}{\sigma}
$$

The equation above takes the mean, $\mu$, and standard deviation, $\sigma$, of the data matrix $X$. It then subtracts by the $\mu$ and divides by $\sigma$.

### Scaling
$$
X' = \frac{X - X_{min}}{X_{max} - X_{min}}
$$

The equation above finds the min and max of the data matrix $X$. It then subtracts each value by the min and divides by the difference between the minimum and maximum.

This is useful for scaling the data within a specific range (default: \[0, 1\]). To scale to different ranges, you can multiply by some scalar and subtract to fit within different ranges.

#### Example: Scaling from \[-1, 1\]
$$
X' = 2 \times (\frac{X - X_{min}}{X_{max} - X_{min}}) - 1
$$

## Feature Processing Techniques
Instead of considering the general min/max for our data matrix/series, we can consider each feature (*column*) as a separate entity for processing.

### Feature Centering
$$
X^{'(f)} = X^{(f)} - \mu^{(f)}
$$

The equation above subtracts the mean of each feature from each feature of $X$. This makes each feature *centered*. This is commonly used in *Principal Component Analysis* ([[Principal Component Analysis#Center Each Feature]]).

### Feature Normalization
$$
X^{'(f)} = \frac{X^{(f)} - \mu^{(f)}}{\sigma^{(f)}}
$$

### Feature Scaling
$$
X^{'(f)} = \frac{X^{(f)} - X_{min}^{(f)}}{X_{max}^{(f)} - X_{min}^{(f)}}
$$

## Clustering
Clustering is useful in separating into separate groups, or *clusters*. Typically technqiues for clustering include ***K-Means***, ***K-Nearest Neighbor***, and when the number of clusters is unknown, we can use ***Affinity Propogation***.

## Dimension Reduction
*Refer to [[Manifold Learning & Dimensionality Reduction]] for more information.*
