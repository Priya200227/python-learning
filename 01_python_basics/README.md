# 🐍 Python Basics

A practical introduction to Python's fundamental concepts, including variables, built-in data types, strings, type checking, type conversion, input/output, and comments.

These concepts establish the foundation for writing Python programs and progressing toward data analytics, automation, and AI engineering.

## 📚 Table of Contents

1. [Introduction](#1-introduction)
2. [Variables](#2-variables)
3. [Basic Data Types](#3-basic-data-types)
4. [Strings](#4-strings)
5. [Type Checking](#5-type-checking)
6. [Type Conversion](#6-type-conversion)
7. [Input and Output](#7-input-and-output)
8. [Comments](#8-comments)
9. [Key Takeaways](#9-key-takeaways)
10. [References](#10-references)

---

## 1. Introduction

Python is a high-level, general-purpose programming language known for its readable syntax and extensive ecosystem of libraries.

Before working with more complex programming concepts, it is important to understand how Python represents values, stores references to objects, handles user input, and produces output.

This section covers the fundamental building blocks used throughout Python development.

## 2. Variables

Variables are names that refer to objects in Python. They allow us to store and access values throughout a program.

```python
name = "Priya"
age = 24
```

In this example:

- `name` refers to a string object.
- `age` refers to an integer object.
- The `=` operator assigns an object to a variable name.

Python is dynamically typed, which means a variable name can be reassigned to an object of a different type.

```python
value = 100
value = "Python"

print(value)
# Output: Python
```

### Naming Conventions

- Use descriptive names that communicate the purpose of a variable.
- Use `snake_case` for variable names.
- Names cannot begin with a digit.
- Avoid Python keywords such as `class`, `if`, and `for` as variable names.

```python
student_name = "Priya"
total_marks = 450
is_enrolled = True
```

## 3. Basic Data Types

Python provides built-in data types for representing different kinds of information.

| Data Type | Description | Example |
|---|---|---|
| `int` | Integer numbers | `24` |
| `float` | Floating-point numbers | `5.4` |
| `str` | Text | `"Python"` |
| `bool` | Boolean values | `True`, `False` |
| `NoneType` | Represents the absence of a value | `None` |

### Example

```python
name = "Priya"       # str
age = 24             # int
height = 5.4         # float
is_learning = True   # bool
result = None        # NoneType
```

Python determines the type of an object automatically when an assignment is made.

Understanding data types is important because they influence which operations are supported and how values behave in a program.

## 4. Strings

A string (`str`) is an immutable sequence of Unicode characters used to represent text.

```python
language = "Python"
```

### Indexing

Indexing accesses an individual character. Python uses zero-based indexing, so the first character is at index `0`.

```python
language = "Python"

print(language[0])
# Output: P

print(language[-1])
# Output: n
```

Negative indices access characters from the end of the string.

### Slicing

Slicing extracts a portion of a sequence using the syntax `sequence[start:stop:step]`. The stop index is excluded.

```python
language = "Python"

print(language[0:3])
# Output: Pyt

print(language[2:])
# Output: thon
```

### String Formatting

F-strings allow variables and expressions to be embedded directly into strings.

```python
name = "Priya"
age = 24

print(f"My name is {name} and I am {age} years old.")
```

Output:

```text
My name is Priya and I am 24 years old.
```

### Common String Methods

```python
text = "  Python Programming  "

print(text.strip())       # Remove surrounding whitespace
print(text.lower())       # Convert to lowercase
print(text.upper())       # Convert to uppercase
print(text.replace("Python", "Data"))
```

**Important:** Strings are immutable. Methods such as `replace()` return a new string rather than modifying the original string in place.

## 5. Type Checking

The built-in `type()` function returns the type of an object.

```python
value = 100

print(type(value))
# Output: <class 'int'>
```

Additional examples:

```python
print(type("Python"))   # <class 'str'>
print(type(5.4))        # <class 'float'>
print(type(True))       # <class 'bool'>
```

When checking whether an object belongs to a particular type or class, `isinstance()` is often more appropriate.

```python
value = 100

print(isinstance(value, int))
# Output: True
```

Type checking is useful when inspecting values, debugging programs, and validating assumptions about data.

## 6. Type Conversion

Type conversion changes a value from one data type to another when the conversion is supported.

### Explicit Conversion

Python provides built-in functions such as `int()`, `float()`, `str()`, and `bool()` for common conversions.

```python
number = "100"

converted_number = int(number)

print(converted_number)
# Output: 100

print(type(converted_number))
# Output: <class 'int'>
```

Additional examples:

```python
print(int(5.9))       # 5
print(float("3.14"))  # 3.14
print(str(100))       # "100"
print(bool(1))        # True
print(bool(0))        # False
```

### Handling Invalid Conversions

Not every value can be converted successfully.

```python
number = "hello"

# Raises ValueError:
# int(number)
```

When conversion depends on external input, consider handling conversion errors using `try` and `except`.

```python
user_input = "abc"

try:
    number = int(user_input)
except ValueError:
    print("Please enter a valid integer.")
```

**Note:** Conversion does not always preserve the original value exactly. For example, `int(5.9)` truncates the fractional part rather than rounding it.

## 7. Input and Output

Python provides `input()` for accepting user input and `print()` for displaying information.

### Taking Input

```python
name = input("Enter your name: ")

print(f"Hello, {name}!")
```

The `input()` function returns a string, even when the user enters digits.

### Converting User Input

```python
age = int(input("Enter your age: "))

print(f"Next year, you will be {age + 1}.")
```

If the input cannot be converted to an integer, `int()` raises a `ValueError`. Real applications should handle such cases when invalid input is possible.

### Displaying Output

```python
name = "Priya"
age = 24

print("Name:", name)
print("Age:", age)
print(f"{name} is {age} years old.")
```

The `print()` function can display multiple values and supports formatted output.

## 8. Comments

Comments explain code to readers and are ignored by Python during normal execution.

### Single-Line Comments

Use `#` to write a comment.

```python
# Store the student's age
age = 24
```

### Practical Use

Comments are most useful when they explain why a decision was made or clarify logic that is not immediately obvious.

```python
# Convert input to an integer before performing arithmetic
age = int(input("Enter your age: "))
```

Avoid comments that merely repeat what the code already makes obvious.

## 9. Key Takeaways

- Variables are names that refer to objects.
- Python provides built-in types such as `int`, `float`, `str`, `bool`, and `NoneType`.
- Strings support indexing, slicing, formatting, and useful methods.
- Strings are immutable.
- `type()` inspects an object's type, while `isinstance()` checks type membership.
- Type conversion can change a value's representation and may raise exceptions.
- `input()` returns a string, so numeric input often requires explicit conversion.
- `print()` displays values and formatted messages.
- Comments improve code readability when used meaningfully.

## 10. References

- [Python Official Tutorial](https://docs.python.org/3/tutorial/)
- [Python — Built-in Types](https://docs.python.org/3/library/stdtypes.html)
- [Python — Built-in Functions](https://docs.python.org/3/library/functions.html)
- [Python — Lexical Analysis and Identifiers](https://docs.python.org/3/reference/lexical_analysis.html)

---

*This section establishes the Python fundamentals required for more structured programming, data processing, automation, and subsequent topics in this repository.*
