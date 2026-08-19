# Python Error Handling

Error handling allows a Python program to deal with problems that occur during execution without crashing unexpectedly.

---

## 1. Syntax Errors vs Exceptions

### Syntax Error

A syntax error occurs when the code does not follow Python's grammar rules.

Examples:

- Missing `:`
- Incorrect indentation
- Misspelled keywords
- Missing brackets or parentheses

Syntax errors must be fixed in the code before the program can run.

### Exceptions

Exceptions occur during program execution when something unexpected happens.

Examples:

- Accessing an invalid list index
- Using a missing dictionary key
- Dividing by zero
- Opening a file that does not exist
- Using an inappropriate type or value

---

## 2. Common Python Exceptions

Some common exceptions are:

| Exception | Meaning |
|---|---|
| `IndexError` | Accessing an invalid index |
| `KeyError` | Accessing a dictionary key that does not exist |
| `TypeError` | Performing an operation on an inappropriate type |
| `ValueError` | A function receives an inappropriate value |
| `NameError` | Using a name/variable that is not defined |
| `AttributeError` | Accessing an attribute or method that an object does not have |
| `ModuleNotFoundError` | Importing a module that cannot be found |
| `FileNotFoundError` | Trying to access a file that does not exist |
| `ZeroDivisionError` | Attempting to divide by zero |

---

## 3. Stack Trace

When an exception is not handled, Python displays a **stack trace**.

A stack trace helps identify:

- Where the error occurred
- Which function calls led to the error
- The type of exception
- The error message

Reading the traceback is an important part of debugging Python programs.

---

## 4. Handling Exceptions

Python uses `try` and `except` to handle exceptions.

```python
try:
    with open("sample.txt", "r") as f:
        print(f.read())
except FileNotFoundError:
    print("File not found")
