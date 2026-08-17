# Python Conditions & Loops

This section covers how Python makes decisions and repeats operations using conditional statements and loops.
These concepts are essential for controlling program flow and are commonly used in data processing, automation, and application logic.

---

## 1. Conditional Statements

Conditional statements allow a program to make decisions based on whether a condition evaluates to `True` or `False`.

### Basic `if`

```python
age = 20

if age >= 18:
    print("Eligible")
```

### `if-else`

Used when there are two possible outcomes.

```python
age = 16

if age >= 18:
    print("Eligible")
else:
    print("Not eligible")
```

### `if-elif-else`

Used when there are multiple conditions to check.

```python
score = 75

if score >= 90:
    print("A")
elif score >= 60:
    print("B")
else:
    print("C")
```

---

## 2. `for` Loop

A `for` loop is used to iterate through a sequence or collection.

```python
for i in (1, 2, 3):
    print(i)
```

Output:

```text
1
2
3
```

The loop processes each item one by one.

### Iterating Through Names

```python
names = ["John", "Maria", "Kumar"]

for name in names:
    print(name)
```

---

## 3. `range()`

`range()` is commonly used with `for` loops to generate a sequence of numbers.

```python
for i in range(3):
    print(i)
```

Output:

```text
0
1
2
```

### Range with Start and Stop

```python
for i in range(1, 4):
    print(i)
```

Output:

```text
1
2
3
```

### Nested Use of `range()`

```python
for year in range(2026, 2028):
    for month in range(1, 3):
        for day in range(1, 4):
            print(year, month, day)
```

This demonstrates how multiple loops can be used to navigate through hierarchical data.

---

## 4. `break`

`break` immediately stops the loop.

```python
for i in (1, 2, 3):
    if i == 2:
        break

    print(i)
```

Output:

```text
1
```

### Key Idea

> `break` means: **Stop the loop completely.**

It is useful when the required item has been found or a stopping condition has been satisfied.

Example:

```python
names = ["John", "Maria", "Kumar"]

for name in names:
    if name == "Kumar":
        break
```

---

## 5. `continue`

`continue` skips the current iteration and moves to the next iteration.

```python
for i in (1, 2, 3):
    if i == 2:
        continue

    print(i)
```

Output:

```text
1
3
```

### Key Idea

> `continue` means: **Skip this iteration, but keep the loop running.**

This is useful when certain records should be skipped while continuing to process the remaining data.

---

## 6. `pass`

`pass` is a placeholder statement.

It does nothing when executed.

```python
for i in (1, 2, 3):
    if i == 2:
        pass

    print(i)
```

`pass` can be used when a block is intentionally left empty and will be implemented later.

### Difference

* `break` → stops the loop
* `continue` → skips the current iteration
* `pass` → does nothing; execution continues normally

---

## 7. `else` with a `for` Loop

Python allows an `else` block with loops.

The `else` block executes when the loop finishes normally **without encountering a `break`**.

```python
for i in (1, 2, 3):
    if i == 5:
        break

    print(i)
else:
    print("Loop completed")
```

Output:

```text
1
2
3
Loop completed
```

If `break` occurs, the loop's `else` block does not execute.

### Practical Example

Checking whether a duplicate exists:

```python
files = ["report.csv", "summary.docx", "report.csv"]

for file in files:
    if file == "report.csv":
        print("Duplicate found")
        break
else:
    print("All files are unique")
```

The idea is:

* `break` → the condition was found
* `else` → the loop completed without encountering `break`

---

## 8. Nested Loops

A loop inside another loop is called a **nested loop**.

```python
for x in (1, 2, 3):
    for y in (1, 2):
        print(x, y)
```

Output:

```text
1 1
1 2
2 1
2 2
3 1
3 2
```

The inner loop completes its iterations for every iteration of the outer loop.

### Common Use Cases

Nested loops can be useful for:

* Working with rows and columns
* Navigating hierarchical data
* Comparing combinations
* Processing tables
* Working with nested structures

---

## 9. `while` Loop

A `while` loop repeatedly executes code as long as its condition remains `True`.

```python
i = 1

while i <= 4:
    print(i)
    i += 1
```

Output:

```text
1
2
3
4
```

A `while` loop generally involves:

1. Initialization
2. Condition
3. Work to be performed
4. Update

Conceptually:

```text
Initialization
      ↓
  Condition
   ↙      ↘
True     False
 ↓          ↓
Work       End
 ↓
Update
 ↓
Condition
```

---

## 10. `while` Loop with `break`

A `while` loop can also be stopped using `break`.

```python
i = 1

while i <= 4:
    print(i)

    if i == 2:
        break

    i += 1
```

Output:

```text
1
2
```

---

## 11. `while True`

`while True` creates a loop that continues indefinitely unless something inside the loop stops it.

```python
while True:
    answer = input("Do you agree? (y/n): ")

    if answer == "y":
        break

print("Thank you")
```

This pattern is useful when the number of iterations is not known beforehand and the loop should continue until a specific condition is met.

---

## 12. `break` vs `continue` vs `pass`

| Statement  | Purpose                             |
| ---------- | ----------------------------------- |
| `break`    | Completely stops the loop           |
| `continue` | Skips the current iteration         |
| `pass`     | Does nothing; acts as a placeholder |

---

## 13. `for` vs `while`

### `for` Loop

Useful when iterating through a known sequence or collection.

```python
for item in items:
    print(item)
```

### `while` Loop

Useful when repetition depends on a condition and the number of iterations may not be known beforehand.

```python
while condition:
    do_something()
```

### Mental Model

```text
for   → "For each item, do something."

while → "While this condition is true, keep doing something."
```

---

## 14. Practical Examples

### Find the First Odd Number

```python
numbers = [2, 4, 6, 7, 8, 10]

for number in numbers:
    if number % 2 != 0:
        print(number)
        break
```

### Skip Invalid Values

```python
numbers = [10, 20, None, 30]

for number in numbers:
    if number is None:
        continue

    print(number)
```

### Retry with a Limited Number of Attempts

```python
attempts = 0

while attempts < 3:
    answer = input("Do you agree? (y/n): ")

    if answer == "y":
        print("Glad we're on the same page!")
        break

    attempts += 1
```

---

## 15. Business/Data Analysis Applications

Loops and conditions are useful for understanding program logic behind tasks such as:

* Checking whether duplicate files exist
* Identifying missing values
* Skipping invalid records
* Processing multiple files
* Iterating through rows or columns
* Navigating hierarchical data
* Applying conditional business rules
* Retrying an operation a limited number of times
* Comparing possible combinations

Example:

```python
files = ["sales.csv", "customers.csv", "sales.csv"]

for file in files:
    if file == "sales.csv":
        print("Sales file found")
        break
else:
    print("Sales file not found")
```

---

## 16. Key Takeaways

* `if`, `elif`, and `else` control decision-making.
* `for` loops iterate through sequences and collections.
* `while` loops repeat while a condition remains `True`.
* `range()` generates sequences of numbers for iteration.
* `break` completely stops a loop.
* `continue` skips the current iteration.
* `pass` is a placeholder and does not affect loop execution.
* Loop `else` executes when the loop finishes without encountering `break`.
* Nested loops allow iteration through multiple levels of data.
* `while True` can be used when the stopping condition is determined inside the loop.
* Choosing between `for` and `while` depends on whether iteration is sequence-based or condition-based.

---

## Why This Matters

Conditions and loops form the core of Python's **control flow**.

They allow programs to:

* Make decisions
* Repeat operations
* Process collections
* Skip unwanted data
* Stop processing when a condition is met
* Apply business rules

These concepts are fundamental for later topics such as **data structures, functions, file handling, automation, data processing, and AI-related development**.

