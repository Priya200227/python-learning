# 🐍 Python Basics

## Introduction

This section covers the fundamental building blocks of Python that I learned before moving into more structured programming concepts.

These fundamentals form the base for working with Python in data analytics, automation, and AI-related applications.

## Concepts Covered

### Variables

Variables are used to store references to values.

```python
name = "Priya"
age = 24
```

### Basic Data Types

Python provides several commonly used built-in data types:

```python
name = "Priya"       # str
age = 24             # int
height = 5.4         # float
is_learning = True   # bool
```

### Strings

Strings represent text and support operations such as indexing, slicing, formatting, and common string methods.

```python
name = "Python"

print(name[0])
print(name[0:3])
```

### Type Checking

The `type()` function is used to inspect the type of a value.

```python
value = 100

print(type(value))
```

### Type Conversion

Python allows values to be converted between compatible data types.

```python
number = "100"

number = int(number)

print(number)
```

### Input and Output

`input()` is used to accept user input, while `print()` is used to display output.

```python
name = input("Enter your name: ")

print(f"Hello, {name}")
```

### Comments

Comments are used to explain code and are ignored during execution.

```python
# This is a comment
```

## Key Takeaways

* Variables are used to work with values in a program.
* Python provides built-in data types for different kinds of values.
* Strings support indexing, slicing, formatting, and various methods.
* `type()` can be used to inspect the type of a value.
* Type conversion allows compatible values to be converted between data types.
* `input()` accepts user input and `print()` displays output.
* Comments help explain code without affecting program execution.

## Why This Matters

These concepts are the foundation for everything that follows in Python.

I use these fundamentals throughout later topics such as conditions, loops, data structures, functions, file handling, automation, and eventually AI-related development.
