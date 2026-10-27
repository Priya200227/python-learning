# Iterators, Generators, Closures & Decorators

A practical reference to Python concepts that help you work with sequences, manage memory, preserve state between function calls, and extend function behavior without modifying the original function.

These notes cover the concepts studied through handwritten examples, including generators, closures, a timing decorator, and reading and writing CSV files with Python's built-in `csv` module.

## Learning Objectives

By the end of this section, you should be able to:

- Explain how iterators produce values one at a time.
- Distinguish an iterable, an iterator, and a generator.
- Use `yield` and `next()` to understand generator execution.
- Explain why generators can be more memory-efficient than building a complete list.
- Create closures that retain access to variables from an enclosing scope.
- Use decorators to wrap a function with additional behavior.
- Read and write CSV data using `csv.reader`, `csv.DictReader`, `csv.writer`, and `csv.DictWriter`.

## Topics Covered

| Topic | What it covers |
|---|---|
| Iterators | Iteration, `iter()`, `next()`, and the iterator protocol |
| Generators | Generator functions, `yield`, lazy evaluation, and memory efficiency |
| Closures | Nested functions and retaining values from an enclosing scope |
| Decorators | Wrapping functions to add reusable behavior, such as timing |
| CSV handling | Reading and writing tabular data with Python's `csv` module |

---

## 1. Iterators

An **iterable** is an object that can return its items one at a time, such as a list, tuple, string, or dictionary. An **iterator** is an object that produces the next value when requested.

The built-in functions `iter()` and `next()` make the iterator protocol visible:

```python
numbers = [10, 20, 30]

iterator = iter(numbers)

print(next(iterator))  # 10
print(next(iterator))  # 20
print(next(iterator))  # 30
```

When the iterator has no more values, `next()` raises `StopIteration`.

```python
numbers = [10, 20]
iterator = iter(numbers)

print(next(iterator))  # 10
print(next(iterator))  # 20
# next(iterator)       # Raises StopIteration
```

A `for` loop handles this stopping condition automatically.

```python
for number in numbers:
    print(number)
```

### Key distinction

- **Iterable:** Can provide an iterator.
- **Iterator:** Produces values one at a time and remembers its current position.
- **Generator:** A convenient kind of iterator, usually created with a generator function or generator expression.

---

## 2. Generators

A **generator** produces values on demand instead of building and storing the entire result up front. This is useful when processing large sequences or streams of data.

### `yield` versus `return`

- `return` ends a function and sends its result back to the caller.
- `yield` produces a value and pauses the generator function. Its local state is retained so execution can resume when another value is requested.

```python
def generate_values():
    yield 1
    yield 2
    yield 3


values = generate_values()  # Creates a generator object

print(next(values))  # 1
print(next(values))  # 2
print(next(values))  # 3
# next(values)       # Raises StopIteration
```

Calling a generator function creates a generator object; the function body begins running when values are requested.

You can also iterate over the generator with a `for` loop:

```python
for value in generate_values():
    print(value)
```

### Why use generators?

A list comprehension creates the complete result in memory:

```python
squares = [number * number for number in range(1_000_000)]
```

A generator expression produces values lazily:

```python
squares = (number * number for number in range(1_000_000))

for square in squares:
    # Process one value at a time
    pass
```

Generators can reduce memory use when values are consumed incrementally. They are generally **one-pass iterators**: after they are exhausted, you need to create a new generator to iterate again.

> **Important:** Generators do not automatically make every program faster. Their main advantage is lazy, incremental processing and potentially lower memory consumption.

---

## 3. Closures

A **closure** is a function that retains access to variables from its enclosing scope, even after the outer function has finished executing.

A common closure pattern has three parts:

1. A nested function is defined inside an outer function.
2. The nested function references a variable from the outer function's scope.
3. The outer function returns the nested function object.

```python
def outer(x):
    def inner(y):
        return x + y

    return inner


add_10 = outer(10)

print(add_10(5))  # 15
```

Here, `add_10` retains access to the value `x = 10`, even though the call to `outer(10)` has finished.

### Why closures are useful

Closures can retain state or configuration without relying on global variables. They are useful for:

- Creating functions with preset configuration.
- Implementing function factories.
- Maintaining small pieces of private state.
- Supporting decorators and callbacks.

A closure does not require a global variable. It uses variables captured from an enclosing scope.

---

## 4. Decorators

A **decorator** is a callable that takes a function (or another callable), adds or changes behavior, and returns a callable. Decorators let you reuse behavior without placing the same extra code inside every function.

For example, a timing decorator can measure how long a function takes to execute.

```python
import time
from functools import wraps


def timer(func):
    @wraps(func)
    def wrapper(*args, **kwargs):
        start_time = time.perf_counter()
        try:
            return func(*args, **kwargs)
        finally:
            elapsed = time.perf_counter() - start_time
            print(f"{func.__name__} took {elapsed:.4f} seconds")

    return wrapper


@timer
def brew_tea():
    time.sleep(1)
    print("Tea is ready!")


brew_tea()
```

The `@timer` syntax is equivalent to:

```python
def brew_tea():
    time.sleep(1)
    print("Tea is ready!")


brew_tea = timer(brew_tea)
```

### Understanding the wrapper

- `func` is the original function.
- `wrapper` runs the additional behavior around the original call.
- `*args` accepts positional arguments.
- `**kwargs` accepts keyword arguments.
- `@wraps(func)` preserves useful metadata from the original function, such as its name and docstring.
- `time.perf_counter()` is appropriate for measuring elapsed time.

The `try/finally` structure ensures elapsed time is printed even if the wrapped function raises an exception. The exception still propagates to the caller.

### When decorators are useful

Decorators can centralize cross-cutting behavior such as logging, timing, caching, access checks, retries, or instrumentation. Use them when they make code clearer; avoid adding layers of wrapping that make program flow difficult to follow.

---

## 5. Reading and Writing CSV Files

Python's built-in `csv` module provides tools for reading and writing comma-separated tabular data. CSV is a text format; values read from a CSV file are typically returned as strings, so convert values to numeric or other types when required.

### Read rows with `csv.reader`

Use `csv.reader` when working with rows by column position.

```python
import csv

with open("people.csv", "r", newline="", encoding="utf-8") as file:
    reader = csv.reader(file)

    for row in reader:
        print(row)
```

Each row is returned as a list of strings. For example, if a row contains `Priya,24,Bengaluru`, then `row[1]` is the string `"24"`.

```python
age = int(row[1])
```

Only perform this conversion when the value is valid for the expected type.

### Read rows with `csv.DictReader`

Use `csv.DictReader` when accessing values by column name rather than position. By default, it uses the first row as the field names.

```python
import csv

with open("people.csv", "r", newline="", encoding="utf-8") as file:
    reader = csv.DictReader(file)

    for row in reader:
        print(row["name"], row["age"], row["city"])
```

Each row behaves like a dictionary, which can make code more readable and less dependent on column order. Values are still generally strings.

### Write rows with `csv.writer`

Use `csv.writer` to write rows represented as sequences, such as lists or tuples.

```python
import csv

rows = [
    ["name", "age", "city"],
    ["Priya", 24, "Bengaluru"],
    ["Anu", 23, "Pune"],
]

with open("people.csv", "w", newline="", encoding="utf-8") as file:
    writer = csv.writer(file)
    writer.writerows(rows)
```

- `writerow()` writes one row.
- `writerows()` writes multiple rows.
- `newline=""` is the recommended setting when opening CSV files for use with Python's `csv` module.

### Write dictionaries with `csv.DictWriter`

Use `csv.DictWriter` when your records are dictionaries keyed by column names.

```python
import csv

fieldnames = ["name", "age", "city"]
people = [
    {"name": "Priya", "age": 24, "city": "Bengaluru"},
    {"name": "Anu", "age": 23, "city": "Pune"},
]

with open("people.csv", "w", newline="", encoding="utf-8") as file:
    writer = csv.DictWriter(file, fieldnames=fieldnames)
    writer.writeheader()
    writer.writerows(people)
```

- `writeheader()` writes the column names.
- `writerow()` writes one dictionary row.
- `writerows()` writes multiple dictionary rows.

Opening a file in `"w"` mode overwrites its existing contents. Use append mode (`"a"`) only when appending is intended, and consider whether a header should be written.

### Which CSV tool should you use?

| Tool | Best suited for |
|---|---|
| `csv.reader` | Reading rows by column position |
| `csv.DictReader` | Reading rows by column name |
| `csv.writer` | Writing rows from lists or other sequences |
| `csv.DictWriter` | Writing rows from dictionaries |

---

## Practical Takeaways

- Use iterators to consume values sequentially.
- Use generators when values can be produced on demand rather than stored all at once.
- Use closures when a function needs to retain access to values from an enclosing scope.
- Use decorators to add reusable behavior around function calls.
- Use `csv.DictReader` and `csv.DictWriter` when column names make tabular data easier to understand.
- Handle exhausted iterators, invalid data, file paths, and exceptions deliberately.

## Suggested Practice

1. Create an iterator from a list and call `next()` until it is exhausted.
2. Write a generator that yields even numbers up to a limit.
3. Compare building a list with producing the same values through a generator expression.
4. Write a closure that creates a multiplier function, such as `multiply_by(3)`.
5. Create a decorator that logs a function's name before calling it.
6. Read a CSV file with both `csv.reader` and `csv.DictReader`.
7. Write a list of dictionaries to a CSV file using `csv.DictWriter`.

## References

- [Python documentation: Iterator Types](https://docs.python.org/3/library/stdtypes.html#iterator-types)
- [Python documentation: Generators](https://docs.python.org/3/howto/functional.html#generators)
- [Python documentation: `functools.wraps`](https://docs.python.org/3/library/functools.html#functools.wraps)
- [Python documentation: `time.perf_counter`](https://docs.python.org/3/library/time.html#time.perf_counter)
- [Python documentation: `csv`](https://docs.python.org/3/library/csv.html)

---

*These notes are a learning reference based on practical study examples. Run the examples in your own Python environment and adapt them to the requirements of each project.*

