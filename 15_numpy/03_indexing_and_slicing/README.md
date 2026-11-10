# NumPy Indexing and Slicing

Indexing and slicing let you access individual elements or subsets of an array.

## 1. Basic indexing

Access an element by its position. NumPy uses zero-based indexing.

```python
arr = np.array([10, 20, 30, 40, 50])

arr[3]
# 40
```

An invalid index raises an `IndexError`.

For multidimensional arrays, provide an index for each axis:

```python
matrix = np.array([[1, 2, 3], [4, 5, 6]])

matrix[1, 2]
# 6
```

## 2. Slicing

Slicing selects a range of elements.

```python
arr[start:stop]
```

The `stop` position is excluded.

For a 2D array:

```python
matrix[row_start:row_stop, col_start:col_stop]
```

Example:

```python
matrix[:, 1:3]  # all rows, columns at positions 1 and 2
```

**Important:** Basic slicing usually returns a view of the original array, not an independent copy. Changes to the view can affect the original array.

## 3. Boolean indexing

Select elements using a condition.

```python
arr = np.array([10, 20, 30, 40, 50])

arr[arr > 30]
# array([40, 50])
```

Useful for filtering values, selecting records that meet conditions, and masking data.

## 4. Fancy indexing

Select specific, potentially non-consecutive positions using an integer list or array.

```python
arr = np.array([10, 20, 30, 40, 50])
indices = [0, 2, 4]

arr[indices]
# array([10, 30, 50])
```

Fancy indexing returns a copy rather than a view.

## Quick comparison

| Method | Use it when |
|---|---|
| Basic indexing | You need a particular element |
| Slicing | You need a continuous range |
| Boolean indexing | You need values matching a condition |
| Fancy indexing | You need specific non-consecutive positions |

## Handwritten notes

[Open Indexing and Slicing mind map](../notes/03_indexing_and_slicing_mindmap.png)

## Remember

For a diagonal from a 2D array, use `np.diag()` or advanced indexing such as `arr[i, i]` when appropriate.

