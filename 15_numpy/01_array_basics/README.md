# NumPy Array Basics

The main NumPy data structure is `numpy.ndarray`, an N-dimensional array.

## Creating an array

```python
import numpy as np

arr = np.array([1, 2, 3])

print(arr)
print(type(arr))
```

Output:

```text
[1 2 3]
<class 'numpy.ndarray'>
```

`np.array()` converts a Python sequence, such as a list, into a NumPy array.

## Inspecting an array

| Attribute | Meaning |
|---|---|
| `arr.ndim` | Number of dimensions (axes) |
| `arr.shape` | Size along each dimension, returned as a tuple |
| `arr.size` | Total number of elements |
| `arr.dtype` | Data type of the array elements |

Example:

```python
arr = np.array([[1, 2, 3], [4, 5, 6]])

print(arr.ndim)   # 2
print(arr.shape)  # (2, 3)
print(arr.size)   # 6
print(arr.dtype)  # e.g. int64, depending on the platform
```

A 1D array is a sequence. A 2D array is commonly used to represent a matrix or table-like structure.

## Handwritten notes

[Open NumPy Array Basics mind map](notes/01_array_basics_mindmap.png)

## Remember

- `shape` is not the same as `size`.
- `ndim` tells you how many axes the array has.
- `dtype` describes the type used to store its elements.

