---
tags:
  - scikit-learn
  - python
  - ml
  - machine-learning
  - data-processing
  - modelling
---

# scikit-learn
## [Install](https://scikit-learn.org/stable/install.html)
### Pip
```shell
pip install scikit-learn
```

In order to check your installation you can use

```shell
python -m pip show scikit-learn  # to see which version and where scikit-learn is installed
python -m pip freeze  # to see all packages installed in the active virtualenv
python -c "import sklearn; sklearn.show_versions()"
```

### Virtualenv
```shell
python -m venv sklearn-venvsklearn-venv\Scripts\activatepip install -U scikit-learn
```

In order to check your installation you can use

```shell
python -m pip show scikit-learn  # to see which version and where scikit-learn is installed
python -m pip freeze  # to see all packages installed in the active virtualenv
python -c "import sklearn; sklearn.show_versions()"
```

### Anaconda
```bash
conda create -n sklearn-env -c conda-forge scikit-learnconda activate sklearn-env
````

In order to check your installation you can use

```bash
conda list scikit-learn  # to see which scikit-learn version is installed
conda list  # to see all packages installed in the active conda environment
python -c "import sklearn; sklearn.show_versions()"
```

## Dimensionality Reduction
> [!INFO]- Tags
> #dimensionality-reduction #manifold #learning

### [PCA](https://scikit-learn.org/stable/modules/generated/sklearn.decomposition.PCA.html)
> [!INFO]- Tags
> #pca #principal-component-analysis

`sklearn.decomposition.PCA(n_components=None, ...)`

Principal Component Analysis (PCA)

Linear dimensionality reduction using Singular Value Decomposition of the data to project it to a lower dimensional space. The input data is centered but not scaled for each feature before applying the SVD.

#### Parameters
* n_components: int, float, or 'mle', default=None
	* Number of components to keep. if n_components is not set all components are kept:
	* `n_components == min(n_samples, n_features)`

#### Methods
`fit(X, y=None)`
Fit the model with X.

`transform(X)`
Apply dimensionality reduction to X. Returns the embedding of training data in low-dimensional space.

`fit_transform(X, y=None)`
Fit X into an embedded space and return that transformed output.

#### Example (-> [source](https://scikit-learn.org/stable/auto_examples/decomposition/plot_pca_vs_lda.html#sphx-glr-auto-examples-decomposition-plot-pca-vs-lda-py))
```python
from sklearn import datasets
from sklearn.decomposition import PCA

iris = datasets.load_iris()

X = iris.data
y = iris.target
target_names = iris.target_names

pca = PCA(n_components=2)
X_r = pca.fit(X).transform(X)
```

### [MDS](https://scikit-learn.org/stable/modules/generated/sklearn.manifold.MDS.html?highlight=mds#sklearn.manifold.MDS)
> [!INFO]- Tags
> #mds #multidimensional-scaling

`sklearn.manifold.MDS(n_components=2, metric=True, ...)`

Multidimensional Scaling (MDS)

#### Parameters
* n_components: int, default=2
	* Number of dimensions in which to immerse the dissimilarities.
* metric: bool, default=`True`
	* If `True`, perform metric MDS; otherwise, perform nonmetric MDS.

#### Methods
`fit(X, y=None)`
Fit the model with X.

`transform(X)`
Apply dimensionality reduction to X. Returns the embedding of training data in low-dimensional space.

`fit_transform(X, y=None)`
Fit X into an embedded space and return that transformed output.

#### Example (-> [source](https://scikit-learn.org/stable/auto_examples/manifold/plot_compare_methods.html#sphx-glr-auto-examples-manifold-plot-compare-methods-py))
```python
from functools import partial
from time import time

from sklearn import manifold, datasets

n_points = 1000
X, color = datasets.make_s_curve(n_points, random_state=0)
n_neighbors = 10
n_components = 2

MDS = manifold.MDS(n_components)

MDS.fit(X)
Y = MDS.transform(X)
```

### [Isomap](https://scikit-learn.org/stable/modules/generated/sklearn.manifold.Isomap.html?highlight=isomap#sklearn.manifold.Isomap)
> [!INFO]- Tags
> #isomap

`sklearn.manifold.Isomap(n_neighbors=5, n_components=2, ...)`

Isomap Embedding

#### Parameters
* n_neighbors: int, default=5
	* Number of neighbors to consider for each point.
* n_components: int, default=2
	* Number of coordinates to consider for each point.

#### Methods
`fit(X, y=None)`
Fit the model with X.

`transform(X)`
Apply dimensionality reduction to X. Returns the embedding of training data in low-dimensional space.

`fit_transform(X, y=None)`
Fit X into an embedded space and return that transformed output.

#### Example (-> [source](https://scikit-learn.org/stable/auto_examples/manifold/plot_compare_methods.html#sphx-glr-auto-examples-manifold-plot-compare-methods-py))
```python
from functools import partial
from time import time

from sklearn import manifold, datasets

n_points = 1000
X, color = datasets.make_s_curve(n_points, random_state=0)
n_neighbors = 10
n_components = 2

iso = manifold.Isomap(n_neighbors=n_neighbors, n_components=n_components)

iso.fit(X)
Y = iso.transform(X)
```

### [t-SNE](https://scikit-learn.org/stable/modules/generated/sklearn.manifold.TSNE.html?highlight=tsne#sklearn.manifold.TSNE)
> [!INFO]- Tags
> #t-sne

`sklearn.manifold.TSNE(n_components=2, perplexity=30.0, init="random", ...)`

T-distributed Stochastic Neighbor Embedding

#### Parameters
* n_components: int, default=2
	* Dimension of embedded space
* perplexity: float, default=30.0
	* The **perplexity** is related to the number of nearest neighbors that is used in other manifold learning algorithms.
	* Larger datasets usually require a larger perplexity. Consider selecting a value between 5 and 50.
	* Different values can result in significantly different results.
* init: {‘random’, ‘pca’} or ndarray of shape (n_samples, n_components), default=’random’
	* Initialization of embedding. Possible options are ‘random’, ‘pca’, and a numpy array of shape (n_samples, n_components).
	* PCA initialization cannot be used with precomputed distances and is usually more globally stable than random initialization.
	* `init='pca'` will become default in 1.2.

#### Methods
`fit(X, y=None)`
Fit the model with X.

`transform(X)`
Apply dimensionality reduction to X. Returns the embedding of training data in low-dimensional space.

`fit_transform(X, y=None)`
Fit X into an embedded space and return that transformed output.

#### Example (-> [source](https://scikit-learn.org/stable/auto_examples/manifold/plot_compare_methods.html#sphx-glr-auto-examples-manifold-plot-compare-methods-py))
```python
from functools import partial
from time import time

from sklearn import manifold, datasets

n_points = 1000
X, color = datasets.make_s_curve(n_points, random_state=0)
n_neighbors = 10
n_components = 2

tsne = manifold.TSNE(n_components=n_components, init="pca", random_state=0)

tsne.fit(X)
Y = tsne.transform(X)
```

## A Basic Example
> [!INFO]- References
> [datacamp.com](https://www.datacamp.com/cheat-sheet/scikit-learn-cheat-sheet-python-machine-learning) [[datacamp-scikit-learn.pdf]]

```python
from sklearn import neighbors, datasets, preprocessing
from sklearn.model_selection import train_test_split
from sklearn.metrics import accuracy_score
iris = datasets.load_iris()
X, y = iris.data[:, :2], iris.target
X_train, X_test, y_train, y_test = train_test_split(X, y, random_state=33)
scaler = preprocessing.StandardScaler().fit(X_train)
X_train = scaler.transform(X_train)
X_test = scaler.transform(X_test)
knn = neighbors.KNeighborsClassifier(n_neighbors=5)
knn.fit(X_train, y_train)
y_pred = knn.predict(X_test)
accuracy_score(y_test, y_pred)
```

### Loading The Data

Your data needs to be numeric and stored as #NumPy arrays or #SciPy sparse matrices. Other types that are convertible to numeric arrays, such as #Pandas DataFrame, are also acceptable.

```python
import numpy as np
X = np.random.random((10,5))
y = np.array(['M','M','F','F','M','F','M','M','F','F','F'])
X[X < 0.7] = 0
```

### Preprocessing The Data

#### Standardization

```python
from sklearn.preprocessing import StandardScaler
scaler = StandardScaler().fit(X_train)
standardized_X = scaler.transform(X_train)
standardized_X_test = scaler.transform(X_test)
```

#### Normalization

```python
from sklearn.preprocessing import Normalizer
scaler = Normalizer().fit(X_train)
normalized_X = scaler.transform(X_train)
normalized_X_test = scaler.transform(X_test)
```

#### Binarization

```python
from sklearn.preprocessing import Binarizer
binarizer = Binarizer(threshold=0.0).fit(X)
binary_X = binarizer.transform(X)
```

#### Encoding Categorical Features

```python
from sklearn.preprocessing import LabelEncoder
enc = LabelEncoder()
y = enc.fit_transform(y)
```

#### Imputing Missing Values

```python
from sklearn.preprocessing import Imputer
imp = Imputer(missing_values=0, strategy='mean', axis=0)
imp.fit_transform(X_train)
```

#### Generating Polynomial Features

```python
from sklearn.preprocessing import PolynomialFeatures
poly = PolynomialFeatures(5)
oly.fit_transform(X)
```

### Training And Test Data

```python
from sklearn.model_selection import train_test_split
X_train, X_test, y_train, y_test = train_test_split(X,y,random_state=0)
```

### Create Your Model

#### Supervised Learning Estimators

Linear Regression

```python
from sklearn.linear_model import LinearRegression
lr = LinearRegression(normalize=True)
```

Support Vector Machines (SVM)

```python
from sklearn.svm import SVC
svc = SVC(kernel='linear')
```

Naive Bayes

```python
from sklearn.naive_bayes import GaussianNB
gnb = GaussianNB()
```

KNN

```python
from sklearn import neighbors
knn = neighbors.KNeighborsClassifier(n_neighbors=5)
```

Unsupervised Learning Estimators

Principal Component Analysis (PCA)

```python
from sklearn.decomposition import PCA
pca = PCA(n_components=0.95)
```

K Means

```python
from sklearn.cluster import KMeans
k_means = KMeans(n_clusters=3, random_state=0)
```

### Model Fitting

#### Supervised learning

```python
lr.fit(X, y)
knn.fit(X_train, y_train)
svc.fit(X_train, y_train)
```

Unsupervised Learning

```python
k_means.fit(X_train)
pca_model = pca.fit_transform(X_train)
```

### Prediction

Supervised Estimators

```python
y_pred = svc.predict(np.random.random((2,5)))
y_pred = lr.predict(X_test)
y_pred = knn.predict_proba(X_test))
```

Unsupervised Estimators

```python
y_pred = k_means.predict(X_test)
```

### Evaluate Your Model's Performance

#### Classification Metrics

Accuracy Score

```python
knn.score(X_test, y_test)
from sklearn.metrics import accuracy_score
accuracy_score(y_test, y_pred)
```

Classification Report

```python
from sklearn.metrics import classification_report
print(classification_report(y_test, y_pred)))
```

Confusion Matrix

```python
from sklearn.metrics import confusion_matrix
print(confusion_matrix(y_test, y_pred)))
```

#### Regression Metrics

Mean Absolute Error

```python
from sklearn.metrics import mean_absolute_error
y_true = [3, -0.5, 2])
mean_absolute_error(y_true, y_pred))
```

Mean Squared Error

```python
from sklearn.metrics import mean_squared_error
mean_squared_error(y_test, y_pred))
```

R2 Score

```python
from sklearn.metrics import r2_score
r2_score(y_true, y_pred))
```

#### Clustering Metrics

Adjusted Rand Index

```python
from sklearn.metrics import adjusted_rand_score
adjusted_rand_score(y_true, y_pred))
```

Homogeneity

```python
from sklearn.metrics import homogeneity_score
homogeneity_score(y_true, y_pred))
```

V-measure

```python
from sklearn.metrics import v_measure_score
metrics.v_measure_score(y_true, y_pred))
```

#### Cross-Validation

```python
print(cross_val_score(knn, X_train, y_train, cv=4))
print(cross_val_score(lr, X, y, cv=2))
```

### Tune Your Model

#### Grid Search

```python
from sklearn.grid_search import GridSearchCV
params = {"n_neighbors": np.arange(1,3), "metric": ["euclidean", "cityblock"]}
grid = GridSearchCV(estimator=knn,param_grid=params)
grid.fit(X_train, y_train)
print(grid.best_score_)
print(grid.best_estimator_.n_neighbors)
```

#### Randomized Parameter Optimization

```python
from sklearn.grid_search import RandomizedSearchCV
params = {"n_neighbors": range(1,5), "weights": ["uniform", "distance"]}
rsearch = RandomizedSearchCV(estimator=knn,
   param_distributions=params,
   cv=4,
   n_iter=8,
   random_state=5)
rsearch.fit(X_train, y_train)
print(rsearch.best_score_)
```

### Going Further
Begin with [our scikit-learn tutorial for beginners](https://www.datacamp.com/community/tutorials/machine-learning-python/), in which you'll learn in an easy, step-by-step way how to explore handwritten digits data, how to create a model for it, how to fit your data to your model and how to predict target values. In addition, you'll make use of Python's data visualization library matplotlib to visualize your results.
