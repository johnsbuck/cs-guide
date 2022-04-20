---
tags:
  - numpy
  - python
  - data-processing
  - matrix
---

# NumPy
> [!INFO]- References
> [[References#^refbetaveroscheatsheet]]

`import numpy as np` is standard. `np.` is implied in most names below...

## Array construction
-   `array([[1, 2, 3], [4, 5, 6]])`
-   `eye(n)`, `identity(n)`
-   `ones(shape)`, `zeros(shape)`, `full(shape, fill_values)`
-   `arange([start,] stop, [step])` (basically just python range, except returns ndarray)
-   `linspace(start, stop, num=50, endpoint=True)` (50 evenly spaced samples in inclusive interval \[start, stop\], or half-open \[start, stop) if endpoint is False)
-   The above can receive a `dtype` argument which is like `numpy.int64`, `numpy.float_`, or `numpy.bool_`. If you just use Python's `int`, `float`, or `bool`, it will Just Work. You can also make your own by writing something in a string somehow.
-   `fromfunction(fn, shape)`

## Array properties
-   `arr.shape` is a tuple of the dimensions
-   `arr.ndim` is the number of dimensions (so, the length of arr.shape)
-   `arr.size` is a single number, the number of elements (so, the product of arr.shape)
-   `arr.dtype` is the type of the array's elements

## Matrix operations
Note that `*` (along with most other operations) acts element-wise!

-   `_.astype('float32')`
-   `np.transpose(_)` or `_.T` (_identity on 1D arrays!_ may need to reshape)
-   `np.linalg.det(_)`
-   `np.dot(_, _)`, or `_ @ _` (_Python 3 only_: `__matmul__` operator)
-   `np.linalg.inv(_)`

## Indexing, Slicing
-   Index and/or slice like `arr[1,2,3:4]`. You can use bare colons to slice everything along a dimension. (`arr[1][2][3:4]` still works, for the reasons you'd expect.)
-   You can also index with one tuple (but not other sequence types) like `arr[(1,2,3)] == arr[1,2,3]`.
-   Basic slices return views, so they're fast. `arr[::-1]` is a good way to reverse.
-   You can also do "advanced indexing" with one or many non-tuple sequences of indices (crudely, it zips them and indexes by each one to produce a new array). So `arr[[1,2,3]]` (unlike the above) will index by 1, then by 2, then by 3, and return the results in one array; and `arr[[1,2,3],[4,5,6]]` will index by (1,4), (2,5), and (3,6).
-   Or "advanced indexing" with an array of booleans (it filters for the true ones). Example: you can filter by `arr[arr % 2 == 0]`.
-   You can combine different indexing modes along different dimensions.
-   `numpy.nonzero(arr)` returns the indices of elements that are nonzero, but kind of strangely; watch out! It returns a tuple of arrays, one for each dimension of `arr`, so `arr[nonzero(arr)]` gives you an array of the nonzero values. If you want to iterate over indexes and then unpack them, do `transpose(nonzero(arr))`.

## Iterating
-   Iterating over an array directly indexes along the first (index 0) dimension, e.g. iterating over a 2D array gives you the rows.
-   `a.flat` yields all values (in row-major order).
-   `np.ndenumerate(a)` yields (index, value) pairs; the index is a tuple of ints.

## Folds
-   `np.sum`, `np.prod`, `np.min`, `np.max`, `np.all`, `np.any`, `np.count_nonzero` fold the matrix to one scalar result. Specify an `axis` argument, a dimension or tuple of dimensions, to fold along only certain axes. `np.sum` usefully promotes to bigger int types on booleans. Also all these functions can be called as methods on an array. (I think the modern name under `np` is `np.amin/amax`, but the above seem to work too, and methods are still `max/min`.)
-   (Don't confuse these folds with `np.minimum` and `np.maximum`, which take element-wise min/max of arrays, like the pairs of elements adding two arrays would operate on.)

## Array manipulation
-   `np.append(arr1, arr2, axis=0)` e.g. `np.append(x, [[1]], axis=0)` (you almost certainly want an axis; otherwise it flattens everything, ew)
-   `np.concatenate([arr1, arr2, ...], axis=0)` (requires arrays have same dimensions except along axis, returns an array also with same dimensions except along axis; axis is 0 by default)
-   `np.stack([arr1, arr2, ...], axis=0)` (requires arrays have the exact same shape, returns an array with an additional axis specified by the parameter)
-   `arr.reshape(shape)` e.g. `arr.reshape((-1, 1))` converts to a column vector; `-1` is the unknown dimension you can specify once
