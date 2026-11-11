# NumPy Array Shape Manipulation

Array shape manipulation changes the dimensions or arrangement of an array. The goal is to reorganise its shape without changing its underlying values.

![Open Array Shape Manipulation mind map](array_shape_manipulation_mindmap.png)

## 1. Reshaping

`reshape()` changes an array's shape while keeping its elements in the same order.

```python
arr = np.arange(12)

arr.reshape(4, -1)
```

`-1` lets NumPy infer that dimension from the number of elements and the other dimensions.

## 2. Transposing

Transpose swaps or reorders an array's axes.

```python
arr_2d = np.array([[0, 1, 2], [3, 4, 5]])

arr_2d.T
arr_2d.transpose()
```

For arrays with more than two dimensions, use `transpose(axes_order)` to specify the new axis order.

Transpose generally returns a view when possible.

## 3. Flattening

Flattening converts a multidimensional array into a one-dimensional array.

| Method | Result |
|---|---|
| `ravel()` | Returns a flattened view when possible; otherwise, a copy |
| `flatten()` | Always returns a copy |

```python
arr = np.array([[1, 2, 3], [4, 5, 6]])

arr.ravel()
arr.flatten()
```

Both produce a one-dimensional sequence of the elements.

## 4. Squeezing

`np.squeeze()` removes axes whose length is 1.

```python
arr = np.array([[[1, 2, 3]]])

np.squeeze(arr)
# array([1, 2, 3])
```

The opposite operation is `np.expand_dims()`, which adds an axis of length 1.

```python
np.expand_dims(arr, axis=0)
```

## Quick comparison

| Operation | Purpose |
|---|---|
| `reshape()` | Change the array shape |
| `transpose()` / `.T` | Reorder axes |
| `ravel()` | Flatten; view if possible |
| `flatten()` | Flatten into a copy |
| `squeeze()` | Remove axes of length 1 |
| `expand_dims()` | Add an axis of length 1 |
