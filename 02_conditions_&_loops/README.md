# Python Conditions & Loops

A structured guide to Python control flow, covering conditional statements, iteration, loop control, nested loops, and practical applications in data processing and automation.

## 🎯 Learning Objectives

- Understand conditional statements and Boolean expressions.
- Use `if`, `elif`, and `else` to control program execution.
- Iterate over sequences and collections using `for` loops.
- Generate numeric sequences using `range()`.
- Control loop execution using `break`, `continue`, and `pass`.
- Understand the `else` clause associated with loops.
- Use `while` loops for condition-based repetition.
- Apply conditions and loops to practical programming scenarios.

## 📚 Table of Contents

1. [Introduction to Control Flow](#1-introduction-to-control-flow)
2. [Conditional Statements](#2-conditional-statements)
3. [The `for` Loop](#3-the-for-loop)
4. [The `range()` Function](#4-the-range-function)
5. [Loop Control Statements](#5-loop-control-statements)
6. [The `else` Clause in Loops](#6-the-else-clause-in-loops)
7. [Nested Loops](#7-nested-loops)
8. [The `while` Loop](#8-the-while-loop)
9. [The `while True` Pattern](#9-the-while-true-pattern)
10. [`for` vs. `while`](#10-for-vs-while)
11. [Practical Examples](#11-practical-examples)
12. [Applications in Data Processing](#12-applications-in-data-processing)
13. [Best Practices](#13-best-practices)
14. [Key Takeaways](#14-key-takeaways)
15. [References](#15-references)

---

## 1. Introduction to Control Flow

Control flow determines the order in which statements execute in a program.

Python provides two fundamental mechanisms for controlling execution:

- **Conditional statements:** Execute code depending on whether a condition is satisfied.
- **Loops:** Repeat a block of code over a sequence or while a condition remains true.

These mechanisms allow programs to make decisions, process collections, validate inputs, and implement business rules.

## 2. Conditional Statements

Conditional statements evaluate expressions and execute code based on their truth values.

### 2.1 The `if` Statement

Executes a block only when the condition evaluates to `True`.

```python
age = 20

if age >= 18:
    print("Eligible")
```

Output:

```text
Eligible
```

### 2.2 The `if-else` Statement

Provides two alternative execution paths.

```python
age = 16

if age >= 18:
    print("Eligible")
else:
    print("Not eligible")
```

Output:

```text
Not eligible
```

### 2.3 The `if-elif-else` Statement

Checks multiple conditions in order. The first matching branch executes.

```python
score = 75

if score >= 90:
    print("A")
elif score >= 60:
    print("B")
else:
    print("C")
```

Output:

```text
B
```

### Key Points

- Conditions are evaluated from top to bottom.
- `elif` allows additional conditions.
- `else` handles cases where none of the preceding conditions is true.
- Indentation determines which statements belong to each block.

## 3. The `for` Loop

A `for` loop iterates over the items of an iterable, such as a list, tuple, string, or range.

### 3.1 Iterating Over a Sequence

```python
numbers = (1, 2, 3)

for number in numbers:
    print(number)
```

Output:

```text
1
2
3
```

The loop retrieves each item from the sequence and executes the loop body.

### 3.2 Iterating Over a List

```python
names = ["John", "Maria", "Kumar"]

for name in names:
    print(name)
```

Output:

```text
John
Maria
Kumar
```

The same approach works with other iterable objects.

## 4. The `range()` Function

The built-in `range()` function produces a sequence of integers commonly used for iteration.

### 4.1 Specifying the Stop Value

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

`range(3)` generates integers starting at `0` and stopping before `3`.

### 4.2 Specifying Start and Stop

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

The stop value is exclusive.

### 4.3 Specifying a Step

```python
for i in range(2, 11, 2):
    print(i)
```

Output:

```text
2
4
6
8
10
```

The general syntax is:

```python
range(start, stop, step)
```

- `start`: The starting value; defaults to `0`.
- `stop`: The exclusive stopping value.
- `step`: The increment or decrement; defaults to `1`.

### 4.4 Nested Iteration Using `range()`

```python
for year in range(2026, 2028):
    for month in range(1, 3):
        for day in range(1, 4):
            print(year, month, day)
```

This generates combinations of years, months, and days within the specified ranges. It is an illustration of nested iteration, not a complete calendar implementation.

## 5. Loop Control Statements

Python provides `break`, `continue`, and `pass` for different control-flow requirements.

### 5.1 The `break` Statement

`break` immediately terminates the innermost enclosing loop.

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

Execution stops when `i` becomes `2`.

**Use case:** Stop searching once the required item is found.

### 5.2 The `continue` Statement

`continue` skips the remaining statements in the current iteration and proceeds to the next iteration.

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

**Use case:** Skip invalid or unwanted records while continuing to process the remaining items.

### 5.3 The `pass` Statement

`pass` performs no operation. It is useful when Python syntax requires a statement but the implementation is intentionally left empty.

```python
for i in (1, 2, 3):
    if i == 2:
        pass

    print(i)
```

Output:

```text
1
2
3
```

Unlike `continue`, `pass` does not skip the remaining statements in the iteration.

### Comparison

| Statement | Behavior |
|---|---|
| `break` | Terminates the innermost enclosing loop. |
| `continue` | Skips the rest of the current iteration. |
| `pass` | Does nothing; execution continues normally. |

## 6. The `else` Clause in Loops

Python supports an `else` clause with both `for` and `while` loops.

The loop's `else` block executes when the loop finishes normally, including when a `while` condition becomes false. It does not execute if the loop terminates through `break`.

### 6.1 Basic Example

```python
for number in (1, 2, 3):
    if number == 5:
        break

    print(number)
else:
    print("Loop completed without break")
```

Output:

```text
1
2
3
Loop completed without break
```

The loop finishes without encountering `break`, so the `else` block executes.

### 6.2 Searching for an Item

```python
files = ["report.csv", "summary.docx", "customers.csv"]
target = "report.csv"

for file in files:
    if file == target:
        print("File found")
        break
else:
    print("File not found")
```

Output:

```text
File found
```

If the target is absent, the loop completes without `break` and prints `"File not found"`.

**Key distinction:** A loop's `else` clause indicates that the loop completed without a `break`; it does not simply mean that the loop has an alternative branch.

## 7. Nested Loops

A nested loop is a loop placed inside another loop.

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

For every iteration of the outer loop, the inner loop completes its iterations.

### Common Applications

- Processing rows and columns.
- Iterating through nested collections.
- Generating combinations.
- Comparing pairs of items.
- Traversing hierarchical data.

**Performance consideration:** Nested loops can become expensive when both loops process large collections. Always consider whether a more efficient approach is available.

## 8. The `while` Loop

A `while` loop repeatedly executes a block as long as its condition evaluates to `True`.

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

### How It Works

1. Initialize the variable used by the loop.
2. Evaluate the condition.
3. Execute the loop body if the condition is true.
4. Update the relevant state.
5. Evaluate the condition again.

When the condition becomes false, the loop terminates.

### Avoiding Infinite Loops

A loop can run indefinitely if its condition never becomes false and no statement terminates it.

For example, forgetting `i += 1` in the preceding example would prevent `i` from reaching a value greater than `4`.

Ensure that the loop has a valid termination condition or an explicit exit mechanism.

## 9. The `while True` Pattern

`while True` creates a loop that continues until an explicit exit condition is encountered.

```python
while True:
    answer = input("Do you agree? (y/n): ").strip().lower()

    if answer == "y":
        break

    if answer == "n":
        break

print("Thank you")
```

This example continues prompting until the user enters `y` or `n`.

The `.strip()` method removes surrounding whitespace, while `.lower()` normalizes the input to lowercase.

**Use case:** Input validation, interactive menus, and situations where the number of iterations is not known in advance.

## 10. `for` vs. `while`

Both loops repeat code, but they are suited to different situations.

| Aspect | `for` Loop | `while` Loop |
|---|---|---|
| Typical use | Iterating over an iterable | Repeating while a condition is true |
| Iteration control | Driven by the iterable | Driven by a Boolean condition |
| State updates | Often handled by the iterator | Frequently handled explicitly |
| Common risk | Incorrect iterable or iteration logic | Accidental infinite loops |

### `for` Example

```python
items = ["sales", "customers", "products"]

for item in items:
    print(item)
```

### `while` Example

```python
attempts = 0

while attempts < 3:
    print("Attempt:", attempts + 1)
    attempts += 1
```

**Mental model:**

- `for`: For each item in an iterable, execute the code.
- `while`: While a condition remains true, execute the code.

## 11. Practical Examples

### 11.1 Find the First Odd Number

```python
numbers = [2, 4, 6, 7, 8, 10]

for number in numbers:
    if number % 2 != 0:
        print("First odd number:", number)
        break
```

Output:

```text
First odd number: 7
```

The modulo operator (`%`) identifies whether a number is odd. `break` stops the search after the first match.

### 11.2 Skip Missing Values

```python
numbers = [10, 20, None, 30]

for number in numbers:
    if number is None:
        continue

    print(number)
```

Output:

```text
10
20
30
```

The `is None` check identifies missing values represented by `None` and skips them.

### 11.3 Retry with a Limited Number of Attempts

```python
attempts = 0
max_attempts = 3

while attempts < max_attempts:
    answer = input("Do you agree? (y/n): ").strip().lower()

    if answer == "y":
        print("Confirmed")
        break

    attempts += 1
else:
    print("Maximum attempts reached")
```

This example permits up to three attempts. The loop's `else` clause executes only if all attempts are exhausted without a successful response.

## 12. Applications in Data Processing

Conditions and loops are commonly used to implement logic in data-processing and automation workflows.

Examples include:

- Applying business rules to records.
- Skipping missing or invalid values.
- Searching for files or specific records.
- Processing collections of files.
- Checking threshold conditions.
- Retrying operations with a limit.
- Iterating through nested data structures.

### Example: Searching for a Required File

```python
files = ["sales.csv", "customers.csv", "inventory.csv"]
target = "sales.csv"

for file in files:
    if file == target:
        print(f"Required file found: {file}")
        break
else:
    print(f"Required file not found: {target}")
```

This demonstrates basic control flow. In production data pipelines, file discovery and validation may be handled more efficiently with dedicated filesystem tools rather than manually iterating over hard-coded lists.

## 13. Best Practices

- Keep conditional expressions clear and readable.
- Use descriptive variable names.
- Prefer `for` loops when iterating over an iterable.
- Use `while` loops when repetition depends on a condition.
- Ensure that condition-based loops have a reliable termination mechanism.
- Use `break` and `continue` only when they make the control flow easier to understand.
- Use nested loops thoughtfully, especially with large collections.
- Test boundary conditions and unexpected inputs.
- Avoid unnecessary loops when Python's built-in functions or suitable libraries provide a clearer solution.

## 14. Key Takeaways

- `if`, `elif`, and `else` implement conditional decision-making.
- `for` loops iterate over items in an iterable.
- `range()` generates integer sequences for iteration.
- `break` terminates the innermost enclosing loop.
- `continue` skips the remaining statements in the current iteration.
- `pass` is a no-operation placeholder.
- A loop's `else` clause executes when the loop finishes without `break`.
- Nested loops support multi-level iteration.
- `while` loops repeat while their conditions remain true.
- `while True` is useful when an explicit exit condition controls termination.
- Choosing the appropriate control-flow structure improves readability and maintainability.

## 15. References

- [Python Tutorial — More Control Flow Tools](https://docs.python.org/3/tutorial/controlflow.html)
- [Python Documentation — `range()`](https://docs.python.org/3/library/stdtypes.html#range)
- [Python Documentation — Built-in Functions](https://docs.python.org/3/library/functions.html)

---

*Part of the Python Learning Journey — a structured collection of concepts, examples, and practical programming notes.*
