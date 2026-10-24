# Python Data Structures

A practical guide to Python's four core built-in data structures: **lists, tuples, sets, and dictionaries**. This section covers their characteristics, operations, comprehensions, selection criteria, and applications in data processing.

## 🎯 Learning Objectives

- Understand the characteristics of Python's built-in data structures.
- Create, access, update, and remove collection elements.
- Use commonly used methods for lists, sets, and dictionaries.
- Apply list and dictionary comprehensions.
- Perform mathematical set operations.
- Understand dictionary keys, values, and membership.
- Select appropriate data structures for different requirements.
- Apply these concepts to practical data-processing scenarios.

## 📚 Table of Contents

1. [Introduction](#1-introduction)
2. [Lists](#2-lists)
3. [List Comprehensions](#3-list-comprehensions)
4. [Tuples](#4-tuples)
5. [Sets](#5-sets)
6. [Set Operations](#6-set-operations)
7. [Dictionaries](#7-dictionaries)
8. [Dictionary Comprehensions](#8-dictionary-comprehensions)
9. [Comparing Data Structures](#9-comparing-data-structures)
10. [Choosing the Right Data Structure](#10-choosing-the-right-data-structure)
11. [Practical Examples](#11-practical-examples)
12. [Common Interview Questions](#12-common-interview-questions)
13. [Best Practices](#13-best-practices)
14. [Key Takeaways](#14-key-takeaways)
15. [References](#15-references)

---

## 1. Introduction

Data structures organize data so that it can be accessed, processed, and modified efficiently.

Python provides four commonly used built-in collection types:

| Data structure | Primary purpose | Example |
|---|---|---|
| List | Ordered, changeable collection | `[10, 20, 30]` |
| Tuple | Ordered, immutable collection | `(10, 20, 30)` |
| Set | Collection of unique elements | `{10, 20, 30}` |
| Dictionary | Key-value mapping | `{"id": 101, "name": "Priya"}` |

The right choice depends on the operations you need to perform, whether duplicates are allowed, and whether the collection needs to change.

## 2. Lists

A **list** is an ordered, mutable collection that supports indexing and allows duplicate values.

### 2.1 Creating a List

```python
fruits = ["apple", "banana", "cherry"]

mixed_values = ["apple", 10, True, 3.14]

print(fruits)
print(mixed_values)
```

Lists can contain elements of different data types, although using a consistent type is often preferable when processing structured data.

### 2.2 Indexing and Slicing

Python lists use zero-based indexing.

```python
fruits = ["apple", "banana", "cherry"]

print(fruits[0])    # apple
print(fruits[1])    # banana
print(fruits[-1])   # cherry
print(fruits[0:2])  # ['apple', 'banana']
```

- Positive indexes access elements from the beginning.
- Negative indexes access elements from the end.
- Slicing extracts a portion of a list; its stop index is excluded.

### 2.3 Common List Methods

| Method | Purpose |
|---|---|
| `append(x)` | Adds an element to the end. |
| `insert(i, x)` | Inserts an element at an index. |
| `remove(x)` | Removes the first matching value. |
| `pop(i)` | Removes and returns an element; defaults to the last. |
| `clear()` | Removes all elements. |
| `sort()` | Sorts the list in place. |
| `reverse()` | Reverses the list in place. |

Examples:

```python
numbers = [1, 2, 3]

numbers.append(4)
print(numbers)  # [1, 2, 3, 4]

numbers.insert(1, 10)
print(numbers)  # [1, 10, 2, 3, 4]

numbers.remove(10)
print(numbers)  # [1, 2, 3, 4]

last_value = numbers.pop()
print(last_value)  # 4
print(numbers)     # [1, 2, 3]
```

Sorting:

```python
numbers = [30, 10, 20]

numbers.sort()
print(numbers)  # [10, 20, 30]

numbers.sort(reverse=True)
print(numbers)  # [30, 20, 10]
```

**Important:** Methods such as `append()`, `sort()`, and `reverse()` modify the existing list. They generally return `None`, rather than a new list.

## 3. List Comprehensions

A list comprehension creates a new list from an iterable using a concise expression.

### 3.1 Basic Syntax

```python
[expression for item in iterable]
```

Example:

```python
numbers = [1, 2, 3, 4, 5]

squares = [number ** 2 for number in numbers]

print(squares)
# [1, 4, 9, 16, 25]
```

### 3.2 Filtering Values

```python
numbers = [10, 20, 60, 80, 30]

high_values = [number for number in numbers if number > 50]

print(high_values)
# [60, 80]
```

### 3.3 Transforming Values

```python
prices = [100, 200, 300]

updated_prices = [price * 2 for price in prices]

print(updated_prices)
# [200, 400, 600]
```

### 3.4 Traditional Loop vs. List Comprehension

Traditional approach:

```python
numbers = [1, 2, 3, 4]
squares = []

for number in numbers:
    squares.append(number ** 2)
```

List comprehension:

```python
numbers = [1, 2, 3, 4]
squares = [number ** 2 for number in numbers]
```

List comprehensions are particularly useful for simple transformations and filtering. For complex logic, a regular loop may be easier to read.

## 4. Tuples

A **tuple** is an ordered collection that cannot be structurally modified after creation.

### 4.1 Creating and Accessing Tuples

```python
coordinates = (10, 20)
values = (1, 2, 2, 3)

print(coordinates[0])  # 10
print(values[-1])      # 3
```

Tuples support indexing, slicing, and duplicate values.

### 4.2 Immutability

```python
coordinates = (10, 20)

# Raises TypeError:
# coordinates[0] = 100
```

A tuple's elements cannot be reassigned or removed. However, if a tuple contains a mutable object such as a list, that nested object may still be modified.

### 4.3 Single-Element Tuples

A trailing comma is required to create a single-element tuple.

```python
value = (10,)

print(type(value))  # <class 'tuple'>
```

Without the comma, `(10)` is simply an integer expression.

### 4.4 When to Use Tuples

Tuples are useful for:

- Fixed groups of related values.
- Coordinates and other structured records.
- Returning multiple values from a function.
- Representing collections that should not be structurally modified.

Example:

```python
coordinates = (17.3850, 78.4867)
```

## 5. Sets

A **set** is a mutable collection of unique, hashable elements. Sets do not support positional indexing and do not guarantee a meaningful iteration order.

### 5.1 Creating a Set

```python
numbers = {1, 2, 2, 3, 3, 4}

print(numbers)
# {1, 2, 3, 4}
```

Duplicate values are eliminated.

An empty set must be created using `set()` because `{}` creates an empty dictionary.

```python
empty_set = set()
```

### 5.2 Common Set Methods

| Method | Purpose |
|---|---|
| `add(x)` | Adds one element. |
| `update(iterable)` | Adds elements from an iterable. |
| `remove(x)` | Removes an element; raises `KeyError` if absent. |
| `discard(x)` | Removes an element if present without raising an error if absent. |
| `pop()` | Removes and returns an arbitrary element. |
| `clear()` | Removes all elements. |

Examples:

```python
numbers = {1, 2, 3}

numbers.add(4)
numbers.update([5, 6])

numbers.discard(10)  # No error if 10 is absent

print(numbers)
```

Because sets are unordered, do not rely on a particular element being returned by `pop()`.

### 5.3 Membership Testing

```python
fruits = {"apple", "banana", "orange"}

print("apple" in fruits)  # True
print("grape" in fruits)  # False
```

Set membership testing is generally efficient, making sets useful for checking whether a value has already been encountered.

## 6. Set Operations

Sets support mathematical operations for comparing collections.

Consider:

```python
A = {1, 2, 3, 4}
B = {3, 4, 5, 6}
```

### 6.1 Union

Returns all unique elements present in either set.

```python
print(A | B)
# {1, 2, 3, 4, 5, 6}
```

Equivalent method: `A.union(B)`.

### 6.2 Intersection

Returns elements common to both sets.

```python
print(A & B)
# {3, 4}
```

Equivalent method: `A.intersection(B)`.

### 6.3 Difference

Returns elements present in the first set but absent from the second.

```python
print(A - B)
# {1, 2}
```

Equivalent method: `A.difference(B)`.

Note that `A - B` and `B - A` can produce different results.

### 6.4 Symmetric Difference

Returns elements present in either set, but not in both.

```python
print(A ^ B)
# {1, 2, 5, 6}
```

Equivalent method: `A.symmetric_difference(B)`.

**Application:** These operations can help compare customer lists, identify overlapping categories, and find differences between groups of records.

## 7. Dictionaries

A **dictionary** stores data as key-value pairs. Keys are unique and must be hashable.

### 7.1 Creating a Dictionary

```python
customer = {
    "id": 101,
    "name": "Priya",
    "city": "Kurnool"
}
```

Each key maps to a corresponding value.

### 7.2 Dictionary Characteristics

- Mutable: entries can be added, updated, and removed.
- Insertion-ordered: entries retain their insertion order.
- Unique keys: assigning an existing key updates its value.
- Key-based access: values are retrieved using keys, not positional indexes.
- Hashable keys: lists and dictionaries cannot be used directly as keys.

### 7.3 Accessing Values

```python
customer = {
    "name": "John",
    "age": 25
}

print(customer["name"])  # John
print(customer["age"])   # 25
```

Accessing a missing key using square brackets raises `KeyError`.

Use `get()` when a missing key should return a default value instead.

```python
print(customer.get("city"))       # None
print(customer.get("city", "N/A"))  # N/A
```

### 7.4 Adding and Updating Values

```python
customer = {"name": "John"}

customer["age"] = 25       # Add a new key
customer["age"] = 26       # Update an existing key

print(customer)
# {'name': 'John', 'age': 26}
```

### 7.5 Removing Values

```python
customer = {"name": "John", "age": 25, "city": "Berlin"}

age = customer.pop("age")
print(age)       # 25
print(customer)
```

`pop()` removes the specified key and returns its value.

`popitem()` removes and returns the last inserted key-value pair.

```python
customer = {"name": "John", "city": "Berlin"}

last_item = customer.popitem()
print(last_item)  # ('city', 'Berlin')
```

### 7.6 Common Dictionary Methods

| Method | Purpose |
|---|---|
| `keys()` | Returns a view of the dictionary's keys. |
| `values()` | Returns a view of the dictionary's values. |
| `items()` | Returns a view of key-value pairs. |
| `get(key, default)` | Retrieves a value with an optional default. |
| `update()` | Adds or updates entries. |
| `pop(key)` | Removes a specified key and returns its value. |
| `popitem()` | Removes and returns the last inserted pair. |
| `clear()` | Removes all entries. |

### 7.7 Iterating Through a Dictionary

```python
customer = {
    "name": "John",
    "age": 25,
    "city": "Berlin"
}

for key, value in customer.items():
    print(key, value)
```

Output:

```text
name John
age 25
city Berlin
```

Use `items()` when both the key and value are needed.

### 7.8 Dictionary Membership

The `in` operator checks dictionary keys by default.

```python
customer = {"name": "John", "age": 25}

print("name" in customer)  # True
print("John" in customer)  # False
```

To check values, use:

```python
print("John" in customer.values())  # True
```

### 7.9 Valid Dictionary Keys

Dictionary keys must be hashable.

```python
record = {
    "name": "John",
    1: "one",
    (10, 20): "coordinates"
}
```

A tuple can be a key if all its elements are hashable. A list cannot be used as a key because lists are mutable and unhashable.

## 8. Dictionary Comprehensions

Dictionary comprehensions provide a concise way to construct dictionaries from iterables.

### 8.1 Basic Syntax

```python
{key_expression: value_expression for item in iterable}
```

Example:

```python
numbers = [1, 2, 3, 4]

squares = {number: number ** 2 for number in numbers}

print(squares)
# {1: 1, 2: 4, 3: 9, 4: 16}
```

### 8.2 Dictionary Comprehension with a Condition

```python
numbers = [1, 2, 3, 4, 5]

squares = {
    number: number ** 2
    for number in numbers
    if number % 2 == 0
}

print(squares)
# {2: 4, 4: 16}
```

Dictionary comprehensions are useful for creating mappings, transforming data, and filtering key-value relationships.

## 9. Comparing Data Structures

| Feature | List | Tuple | Set | Dictionary |
|---|---|---|---|---|
| Ordered | Yes | Yes | No guaranteed order | Yes, insertion order |
| Mutable | Yes | No | Yes | Yes |
| Allows duplicates | Yes | Yes | No duplicate elements | Keys are unique |
| Index-based access | Yes | Yes | No | No |
| Key-value pairs | No | No | No | Yes |
| Main purpose | Ordered collection | Fixed collection | Unique elements | Key-value mapping |

## 10. Choosing the Right Data Structure

### Use a List When

You need an ordered, mutable collection that permits duplicates.

```python
orders = [101, 102, 103, 101]
```

### Use a Tuple When

You need an ordered collection whose structure should remain unchanged.

```python
coordinates = (10, 20)
```

### Use a Set When

You need unique elements, membership checks, or mathematical set operations.

```python
unique_customer_ids = {101, 102, 103}
```

### Use a Dictionary When

You need to associate values with meaningful keys.

```python
customer = {
    "id": 101,
    "name": "John",
    "city": "Berlin"
}
```

### A Simple Decision Process

1. Need key-value mapping? Use a dictionary.
2. Need unique elements or set operations? Use a set.
3. Need an ordered collection that should not be structurally modified? Consider a tuple.
4. Otherwise, a list is often a suitable starting point.

This is a guideline, not an absolute rule. Consider the operations, constraints, and readability requirements of the actual problem.

## 11. Practical Examples

### 11.1 Identifying Unique Customer IDs

```python
customer_ids = [101, 102, 103, 101, 104, 102]

unique_customer_ids = set(customer_ids)

print(unique_customer_ids)
```

The set removes duplicate IDs. Since sets do not guarantee a particular display order, do not rely on the order of the printed result.

If you need unique IDs while preserving their first-occurrence order, use:

```python
unique_customer_ids = list(dict.fromkeys(customer_ids))

print(unique_customer_ids)
# [101, 102, 103, 104]
```

### 11.2 Mapping Status Codes to Labels

```python
status_codes = {
    "01": "Approved",
    "02": "In Progress",
    "03": "Rejected"
}

status = status_codes.get("01", "Unknown")

print(status)
# Approved
```

This pattern is useful when translating coded values into readable labels.

### 11.3 Transforming Abbreviated Column Names

```python
column_mapping = {
    "cust": "customer",
    "qty": "quantity",
    "amt": "amount"
}

for abbreviation, full_name in column_mapping.items():
    print(abbreviation, "->", full_name)
```

Mappings like this can support consistent naming during data-cleaning workflows.

### 11.4 Processing Structured Customer Data

```python
customers = [
    {"id": 101, "name": "John", "city": "Berlin"},
    {"id": 102, "name": "Maria", "city": "London"},
    {"id": 103, "name": "Kumar", "city": "Hyderabad"}
]

for customer in customers:
    print(customer["id"], customer["name"])
```

This combines a list of dictionaries, a common structure for representing collections of records.

## 12. Common Interview Questions

### Q1. What is the difference between a list and a tuple?

A list is mutable, whereas a tuple is immutable at the structural level. Both are ordered and support indexing and duplicate values.

### Q2. Why use a set?

Sets are useful for unique values, membership testing, and operations such as union, intersection, and difference.

### Q3. Can a set contain duplicate values?

No. Duplicate elements are not retained in a set.

### Q4. Can a dictionary contain duplicate keys?

No. Assigning the same key more than once updates the value associated with that key.

```python
user = {
    "name": "John",
    "name": "David"
}

print(user)
# {'name': 'David'}
```

### Q5. Can a tuple be used as a dictionary key?

Yes, provided all its elements are hashable.

### Q6. What is the difference between `remove()` and `discard()` for sets?

`remove()` raises `KeyError` if the element is absent. `discard()` does not raise an error when the element is missing.

### Q7. What is the difference between `dictionary[key]` and `dictionary.get(key)`?

Square-bracket access raises `KeyError` for a missing key. `get()` returns `None` by default or a supplied default value.

### Q8. What is a comprehension?

A comprehension constructs a collection concisely from an iterable, optionally applying a transformation or condition.

## 13. Best Practices

- Choose data structures based on the operations and constraints of your problem.
- Use descriptive variable names.
- Prefer comprehensions for straightforward transformations, not complicated multi-step logic.
- Use `dict.get()` when missing keys are expected and a default is appropriate.
- Use `discard()` when removing an optional set element.
- Remember that set iteration order is not guaranteed.
- Avoid modifying a collection while iterating over it unless the behavior is carefully controlled.
- Consider memory usage and lookup requirements when processing large datasets.
- Use consistent dictionary keys when representing structured records.

## 14. Key Takeaways

- **List:** Ordered, mutable, and supports duplicates.
- **Tuple:** Ordered, structurally immutable, and supports duplicates.
- **Set:** Stores unique, hashable elements and supports mathematical set operations.
- **Dictionary:** Stores insertion-ordered key-value pairs with unique, hashable keys.
- **List comprehension:** Creates lists using concise transformation and filtering expressions.
- **Dictionary comprehension:** Builds mappings from iterables.
- **Membership:** Use `in` to check for an element in a set or a key in a dictionary.
- **Selection:** Choose the data structure that best matches the required operations and constraints.

The key skill is not memorizing every method. It is understanding the shape of your data and selecting the structure that makes the intended operations clear and efficient.

## 15. References

- [Python Documentation — Built-in Types](https://docs.python.org/3/library/stdtypes.html)
- [Python Documentation — Data Structures](https://docs.python.org/3/tutorial/datastructures.html)
- [Python Documentation — Dictionaries](https://docs.python.org/3/tutorial/datastructures.html#dictionaries)
- [Python Documentation — Set Types](https://docs.python.org/3/library/stdtypes.html#set-types-set-frozenset)

---

*Part of the Python Learning Journey — a structured collection of concepts, examples, and practical programming notes.*
