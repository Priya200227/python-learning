# Python Data Structures

Python provides several built-in data structures for storing and organizing collections of data.

The four core data structures covered here are:

1. **List**
2. **Tuple**
3. **Set**
4. **Dictionary**

---

# 1. List

A **list** is an ordered and mutable collection of elements.

```python
my_list = ["apple", "banana", "cherry"]
```

## Key Characteristics

| Property                    | List  |
| --------------------------- | ----- |
| Ordered                     | ✅ Yes |
| Allows duplicates           | ✅ Yes |
| Indexed                     | ✅ Yes |
| Mutable                     | ✅ Yes |
| Allows different data types | ✅ Yes |

Example:

```python
my_list = ["apple", "banana", "apple", 10, True]

print(my_list)
```

---

## List Indexing

Lists use zero-based indexing.

```python
my_list = ["apple", "banana", "cherry"]

print(my_list[0])
# apple

print(my_list[1])
# banana

print(my_list[2])
# cherry
```

Negative indexing can be used to access elements from the end.

```python
print(my_list[-1])
# cherry
```

---

## List Methods

### `append()`

Adds an element to the end of the list.

```python
numbers = [1, 2, 3]

numbers.append(4)

print(numbers)
# [1, 2, 3, 4]
```

### `insert()`

Adds an element at a specific position.

```python
numbers = [1, 2, 3]

numbers.insert(1, 10)

print(numbers)
# [1, 10, 2, 3]
```

### `remove()`

Removes the first matching value.

```python
numbers = [1, 2, 3, 2]

numbers.remove(2)

print(numbers)
# [1, 3, 2]
```

### `pop()`

Removes and returns an element.

```python
numbers = [10, 20, 30]

x = numbers.pop()

print(x)
# 30

print(numbers)
# [10, 20]
```

By default, `pop()` removes the last element.

You can also provide an index:

```python
numbers = [10, 20, 30]

numbers.pop(1)

print(numbers)
# [10, 30]
```

### `clear()`

Removes all elements.

```python
numbers = [1, 2, 3]

numbers.clear()

print(numbers)
# []
```

### `sort()`

Sorts the list.

```python
numbers = [30, 10, 20]

numbers.sort()

print(numbers)
# [10, 20, 30]
```

Descending order:

```python
numbers.sort(reverse=True)

print(numbers)
# [30, 20, 10]
```

### `reverse()`

Reverses the order of elements.

```python
numbers = [1, 2, 3]

numbers.reverse()

print(numbers)
# [3, 2, 1]
```

---

# 2. List Comprehension

List comprehension provides a concise way to create a new list.

### Basic Syntax

```python
[expression for item in iterable]
```

Example:

```python
numbers = [1, 2, 3, 4, 5]

squares = [x ** 2 for x in numbers]

print(squares)
# [1, 4, 9, 16, 25]
```

---

## List Comprehension with Condition

```python
[expression for item in iterable if condition]
```

Example:

```python
numbers = [10, 20, 60, 80, 30]

high_values = [x for x in numbers if x > 50]

print(high_values)
# [60, 80]
```

---

## List Comprehension for Data Transformation

List comprehension can be used to transform values.

Example:

```python
prices = [100, 200, 300]

new_prices = [price * 2 for price in prices]

print(new_prices)
# [200, 400, 600]
```

This can be thought of as:

> Take every value → apply an operation → create a new list.

---

## Traditional Loop vs List Comprehension

### Traditional Loop

```python
numbers = [1, 2, 3, 4]

squares = []

for num in numbers:
    squares.append(num ** 2)
```

### List Comprehension

```python
numbers = [1, 2, 3, 4]

squares = [num ** 2 for num in numbers]
```

List comprehension is shorter and often easier to read when the logic is simple.

---

# 3. Tuple

A **tuple** is an ordered collection that cannot be changed after it is created.

```python
my_tuple = (1, 2, 3)
```

## Key Characteristics

| Property          | Tuple |
| ----------------- | ----- |
| Ordered           | ✅ Yes |
| Allows duplicates | ✅ Yes |
| Indexed           | ✅ Yes |
| Mutable           | ❌ No  |
| Immutable         | ✅ Yes |

---

## Example

```python
my_tuple = ("apple", "banana", "cherry")

print(my_tuple[0])
# apple
```

---

## Tuple Immutability

Once a tuple is created, its elements cannot be changed.

```python
my_tuple = (10, 20, 30)

# my_tuple[0] = 100
```

This produces an error because tuples are immutable.

---

## Tuple Use Case

One important use case for tuples is:

> **Protecting values from accidental modification.**

For example:

```python
coordinates = (10, 20)
```

If the values should remain fixed, a tuple can be appropriate.

---

# 4. Set

A **set** is an unordered collection of unique values.

```python
my_set = {"apple", "banana", "cherry"}
```

## Key Characteristics

| Property          | Set   |
| ----------------- | ----- |
| Ordered           | ❌ No  |
| Allows duplicates | ❌ No  |
| Indexed           | ❌ No  |
| Mutable           | ✅ Yes |
| Unique values     | ✅ Yes |

---

## Duplicate Values

Sets automatically remove duplicates.

```python
my_set = {1, 2, 2, 3, 3, 4}

print(my_set)
# {1, 2, 3, 4}
```

This makes sets useful when you need **unique values**.

---

# 5. Set Methods

## `add()`

Adds an element.

```python
my_set = {1, 2, 3}

my_set.add(4)

print(my_set)
```

## `update()`

Adds multiple elements from another iterable.

```python
my_set = {1, 2, 3}

my_set.update([4, 5, 6])

print(my_set)
```

## `remove()`

Removes an element.

```python
my_set = {1, 2, 3}

my_set.remove(2)

print(my_set)
```

If the value does not exist, `remove()` raises an error.

## `discard()`

Removes an element if it exists.

```python
my_set = {1, 2, 3}

my_set.discard(5)
```

Unlike `remove()`, `discard()` does not raise an error if the value is missing.

## `pop()`

Removes and returns an arbitrary element from a set.

```python
my_set = {1, 2, 3}

value = my_set.pop()

print(value)
```

Because sets are unordered, you should not assume which element will be removed.

## `clear()`

Removes all elements.

```python
my_set = {1, 2, 3}

my_set.clear()

print(my_set)
# set()
```

---

# 6. Set Mathematical Operations

Sets support useful mathematical operations.

Consider:

```python
A = {1, 2, 3, 4}
B = {3, 4, 5, 6}
```

## Union

Union combines all unique elements from both sets.

```python
A.union(B)
```

Result:

```text
{1, 2, 3, 4, 5, 6}
```

Shortcut:

```python
A | B
```

---

## Intersection

Intersection returns elements common to both sets.

```python
A.intersection(B)
```

Result:

```text
{3, 4}
```

Shortcut:

```python
A & B
```

---

## Difference

Difference returns elements that exist in the first set but not the second.

```python
A.difference(B)
```

Result:

```text
{1, 2}
```

Shortcut:

```python
A - B
```

---

## Symmetric Difference

Returns elements that exist in either set but not in both.

```python
A.symmetric_difference(B)
```

Result:

```text
{1, 2, 5, 6}
```

Shortcut:

```python
A ^ B
```

---

# 7. Set Membership

Sets are useful when checking whether a value exists in a collection.

```python
my_set = {"apple", "banana", "orange"}

print("apple" in my_set)
# True
```

Because sets are hash-based, membership checking is generally very efficient.

---

# 8. Dictionary

A **dictionary** stores data as **key-value pairs**.

```python
user = {
    "name": "Priya",
    "age": 24,
    "city": "Kurnool"
}
```

Conceptually:

```text
Key      → Value

name     → Priya
age      → 24
city     → Kurnool
```

---

## Dictionary Structure

```python
{
    key: value,
    key: value
}
```

Example:

```python
user = {
    "name": "John",
    "city": "Berlin"
}
```

---

# 9. Dictionary Characteristics

| Property               | Dictionary |
| ---------------------- | ---------- |
| Stores key-value pairs | ✅ Yes      |
| Ordered                | ✅ Yes      |
| Allows duplicate keys  | ❌ No       |
| Mutable                | ✅ Yes      |
| Indexed by position    | ❌ No       |
| Accessed using keys    | ✅ Yes      |

> **Note:** Python dictionaries preserve insertion order. However, they are accessed by keys rather than positional indexes.

---

# 10. Accessing Dictionary Values

```python
user = {
    "name": "John",
    "age": 25
}

print(user["name"])
# John

print(user["age"])
# 25
```

---

## `get()`

`get()` can be used to retrieve a value safely.

```python
user = {
    "name": "John"
}

print(user.get("name"))
# John
```

If the key does not exist:

```python
print(user.get("age"))
# None
```

You can also provide a default value:

```python
print(user.get("age", 0))
# 0
```

---

# 11. Adding and Updating Dictionary Values

```python
user = {
    "name": "John"
}

user["age"] = 25

print(user)
```

Updating an existing key:

```python
user["age"] = 26
```

---

# 12. Removing Dictionary Values

## `pop()`

Removes a key-value pair using the key.

```python
user = {
    "name": "John",
    "age": 25
}

user.pop("age")

print(user)
```

## `popitem()`

Removes and returns the last inserted key-value pair.

```python
user = {
    "name": "John",
    "age": 25
}

user.popitem()
```

---

# 13. Dictionary Methods

## `keys()`

Returns the dictionary's keys.

```python
user = {
    "name": "John",
    "age": 25
}

print(user.keys())
```

## `values()`

Returns the dictionary's values.

```python
print(user.values())
```

## `items()`

Returns key-value pairs.

```python
print(user.items())
```

Example:

```python
for key, value in user.items():
    print(key, value)
```

This is useful when you need both the key and value while looping.

---

# 14. Dictionary Membership

Dictionary membership checks keys.

```python
user = {
    "name": "John",
    "age": 25
}

print("name" in user)
# True
```

It checks whether the key exists.

---

# 15. Dictionary Keys

Dictionary keys must be **hashable**.

Common examples of valid keys:

```python
{
    "name": "John",
    1: "one",
    (1, 2): "coordinates"
}
```

Lists cannot be dictionary keys because lists are mutable and therefore unhashable.

```python
# Invalid

{
    [1, 2]: "value"
}
```

---

# 16. Dictionary Comprehension

Dictionary comprehension is used to create dictionaries in a concise way.

### Basic Syntax

```python
{key_expression: value_expression for item in iterable}
```

Example:

```python
numbers = [1, 2, 3, 4]

squares = {
    num: num ** 2
    for num in numbers
}

print(squares)
```

Output:

```text
{
    1: 1,
    2: 4,
    3: 9,
    4: 16
}
```

---

# 17. Dictionary Comprehension with Condition

```python
numbers = [1, 2, 3, 4, 5]

squares = {
    num: num ** 2
    for num in numbers
    if num % 2 == 0
}

print(squares)
```

Output:

```text
{
    2: 4,
    4: 16
}
```

---

# 18. Practical Dictionary Use Cases

Dictionaries are particularly useful when working with structured information.

## Use Case 1: API Data

API responses are commonly represented using dictionaries.

```python
user = {
    "id": 101,
    "name": "John",
    "city": "Berlin"
}
```

---

## Use Case 2: Mapping Codes to Labels

Suppose a dataset contains technical codes:

```python
status_codes = {
    "01": "Approved",
    "02": "In Progress",
    "03": "Rejected"
}
```

Instead of repeatedly working with codes, we can map them to meaningful labels.

```python
status_codes["01"]
# Approved
```

---

## Use Case 3: Mapping Abbreviations

```python
abbreviations = {
    "cust": "customer",
    "qty": "quantity",
    "amt": "amount"
}
```

This can be useful when transforming technical or abbreviated dataset columns into readable names.

---

# 19. Dictionary + Loop

Dictionaries can be processed using loops.

```python
user = {
    "name": "John",
    "age": 25,
    "city": "Berlin"
}

for key, value in user.items():
    print(key, value)
```

Output:

```text
name John
age 25
city Berlin
```

---

# 20. List vs Tuple vs Set vs Dictionary

| Feature                | List       | Tuple            | Set           | Dictionary |
| ---------------------- | ---------- | ---------------- | ------------- | ---------- |
| Ordered                | ✅          | ✅                | ❌             | ✅          |
| Mutable                | ✅          | ❌                | ✅             | ✅          |
| Duplicates             | ✅          | ✅                | ❌             | Keys ❌     |
| Indexed                | ✅          | ✅                | ❌             | By key     |
| Stores key-value pairs | ❌          | ❌                | ❌             | ✅          |
| Main purpose           | Collection | Fixed collection | Unique values | Mapping    |

---

# 21. When Should I Use Each Data Structure?

## Use a List When:

You need:

* An ordered collection
* Duplicate values
* Index-based access
* A collection that may change

Example:

```python
orders = [101, 102, 103, 104]
```

---

## Use a Tuple When:

You need:

* An ordered collection
* Values that should not change
* A fixed group of values

Example:

```python
coordinates = (10, 20)
```

---

## Use a Set When:

You need:

* Unique values
* Fast membership checking
* Set operations such as union/intersection

Example:

```python
unique_customers = {101, 102, 103}
```

---

## Use a Dictionary When:

You need:

* Key-value relationships
* Fast lookup using a key
* Structured records
* Mapping one value to another

Example:

```python
customer = {
    "id": 101,
    "name": "John",
    "city": "Berlin"
}
```

---

# 22. Quick Mental Model

Think of the four data structures like this:

```text
LIST
↓
Ordered + Changeable + Duplicates allowed


TUPLE
↓
Ordered + Fixed + Duplicates allowed


SET
↓
Unique values + Unordered + Changeable


DICTIONARY
↓
Key → Value mapping
```

---

# 23. Data Structure Selection

When deciding which data structure to use, ask:

### Question 1

Do I need key-value mapping?

```text
YES → Dictionary
NO  → Continue
```

### Question 2

Do I need unique values only?

```text
YES → Set
NO  → Continue
```

### Question 3

Should the collection be immutable?

```text
YES → Tuple
NO  → List
```

---

# 24. Practical Example

Suppose we are analyzing customers.

### List

Store all customer IDs:

```python
customers = [101, 102, 103, 104, 101]
```

Duplicates are allowed.

### Set

Find unique customer IDs:

```python
unique_customers = set(customers)

print(unique_customers)
```

### Tuple

Store a fixed coordinate:

```python
location = (17.3850, 78.4867)
```

### Dictionary

Store customer information:

```python
customer = {
    "id": 101,
    "name": "John",
    "city": "Berlin"
}
```

---

# 25. Important Interview Points

## List

* Ordered
* Mutable
* Allows duplicates
* Supports indexing
* Useful for collections that change

## Tuple

* Ordered
* Immutable
* Allows duplicates
* Supports indexing
* Useful when values should remain fixed

## Set

* Stores unique values
* Does not support positional indexing
* Mutable
* Useful for membership checking and mathematical set operations

## Dictionary

* Stores key-value pairs
* Keys must be hashable
* Mutable
* Accessed using keys
* Useful for mapping and lookups

---

# 26. Common Interview Questions

## Q1. What is the difference between List and Tuple?

**List:**

```python
[1, 2, 3]
```

Mutable.

**Tuple:**

```python
(1, 2, 3)
```

Immutable.

---

## Q2. Why use a Set?

When you need unique values or efficient membership checking.

```python
unique_values = set([1, 2, 2, 3])
```

---

## Q3. Why use a Dictionary?

When data has a meaningful key-value relationship.

```python
employee = {
    "id": 101,
    "name": "John"
}
```

---

## Q4. Can a Set contain duplicate values?

No.

```python
my_set = {1, 1, 2, 2, 3}

print(my_set)
# {1, 2, 3}
```

---

## Q5. Can a Tuple be modified?

No.

```python
my_tuple = (1, 2, 3)

# my_tuple[0] = 100
```

---

## Q6. Can a Dictionary have duplicate keys?

No.

If the same key is assigned again, the value is overwritten.

```python
user = {
    "name": "John",
    "name": "David"
}

print(user)
```

Result:

```python
{"name": "David"}
```

---

# 27. Key Takeaways

```text
LIST
→ Ordered
→ Mutable
→ Duplicates allowed
→ Indexed


TUPLE
→ Ordered
→ Immutable
→ Duplicates allowed
→ Indexed


SET
→ Unique values
→ No positional indexing
→ Mutable
→ Useful for membership and set operations


DICTIONARY
→ Key-value pairs
→ Mutable
→ Keys must be hashable
→ Access using keys
```

The most important skill is not memorizing methods.

The important question is:

> **"What kind of data do I have, and what operation do I need to perform?"**

Then choose the appropriate data structure.

---

## Why This Matters

Data structures determine **how you store, access, modify, and organize data** in Python.

Understanding when to use a list, tuple, set, or dictionary is essential before moving into topics such as functions, file handling, APIs, data processing, Pandas, automation, and AI-related development.

