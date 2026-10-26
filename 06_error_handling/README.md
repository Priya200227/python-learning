# 🐍 Python Error Handling

Learn how to identify Python errors, handle exceptions, understand tracebacks, raise exceptions intentionally, and create custom exceptions for application-specific situations.

This section focuses on writing Python programs that handle expected failures gracefully, provide meaningful error messages, and remain easier to debug and maintain.

---

## 📚 Table of Contents

- [Learning Objectives](#-learning-objectives)
- [1. Understanding Python Errors](#1-understanding-python-errors)
- [2. Common Python Exceptions](#2-common-python-exceptions)
- [3. What Is Exception Handling?](#3-what-is-exception-handling)
- [4. Handling Exceptions with `try` and `except`](#4-handling-exceptions-with-try-and-except)
- [5. Catching Specific Exceptions](#5-catching-specific-exceptions)
- [6. Accessing Exception Information](#6-accessing-exception-information)
- [7. Understanding Tracebacks](#7-understanding-tracebacks)
- [8. Python Exception Hierarchy](#8-python-exception-hierarchy)
- [9. The `else` Block](#9-the-else-block)
- [10. The `finally` Block](#10-the-finally-block)
- [11. Combining `try`, `except`, `else`, and `finally`](#11-combining-try-except-else-and-finally)
- [12. Raising Exceptions with `raise`](#12-raising-exceptions-with-raise)
- [13. `raise` vs `except`](#13-raise-vs-except)
- [14. Creating Custom Exceptions](#14-creating-custom-exceptions)
- [15. When to Use Custom Exceptions](#15-when-to-use-custom-exceptions)
- [16. Error Handling vs Error Prevention](#16-error-handling-vs-error-prevention)
- [17. Exception Handling Flow](#17-exception-handling-flow)
- [18. Best Practices](#18-best-practices)
- [19. Common Mistakes](#19-common-mistakes)
- [Key Concepts](#-key-concepts)
- [Learning Takeaway](#-learning-takeaway)
- [Learning Resources](#-learning-resources)

---

## 🎯 Learning Objectives

By the end of this section, you should understand:

- The difference between syntax errors and exceptions.
- Common built-in Python exceptions and when they occur.
- How to handle exceptions using `try` and `except`.
- When to catch specific exceptions instead of using a broad handler.
- How to access exception messages and understand tracebacks.
- The purpose of `else` and `finally`.
- How to raise exceptions using `raise`.
- How to create and handle custom exceptions.
- How to write reliable, maintainable error-handling code.

---

## 1. Understanding Python Errors

Errors can occur at different stages of a program.

Two important categories to understand are **syntax errors** and **exceptions**.

### 1.1 Syntax Errors

A syntax error occurs when Python cannot parse code because it violates the language's syntax rules.

Example:

```python
if True
    print("Hello")
```

The colon after `True` is missing.

Python reports a `SyntaxError`, and the code cannot execute as written.

**Correct version:**

```python
if True:
    print("Hello")
```

### 1.2 Exceptions

An exception occurs when a problem arises during program execution.

The code may be syntactically valid, but an operation fails at runtime.

Example:

```python
a = 10
b = 0

print(a / b)
```

The syntax is valid, but dividing a number by zero raises:

```text
ZeroDivisionError: division by zero
```

### Syntax Errors vs Exceptions

| Aspect | Syntax Error | Exception |
|---|---|---|
| When it occurs | During parsing or syntax analysis | During execution |
| Cause | Invalid Python syntax | An operation or condition causes a runtime failure |
| Example | Missing colon after an `if` statement | Dividing by zero |
| Can `try` handle it at runtime? | Not when the syntax error prevents the containing code from being parsed | Yes, if the exception is raised within the protected execution and a matching handler exists |

**Key idea:** Syntax errors prevent Python from interpreting the code correctly. Exceptions occur when an operation fails during execution.

---

## 2. Common Python Exceptions

Python provides built-in exception types for different kinds of errors.

Understanding them helps you identify problems and choose appropriate handling strategies.

### 2.1 `IndexError`

Occurs when a sequence is accessed using an index that does not exist.

```python
numbers = [10, 20, 30]

print(numbers[100])
```

**Output:**

```text
IndexError: list index out of range
```

The list contains only three elements, so index `100` is invalid.

### 2.2 `KeyError`

Occurs when a dictionary lookup requests a key that does not exist.

```python
employee = {
    "name": "Priya",
    "age": 24
}

print(employee["salary"])
```

**Output:**

```text
KeyError: 'salary'
```

### 2.3 `TypeError`

Occurs when an operation or function receives an inappropriate type of object.

```python
result = "hello" + 10
```

**Output:**

```text
TypeError
```

Python cannot concatenate a string and an integer using this operation.

### 2.4 `ValueError`

Occurs when an operation receives an argument of an appropriate general type but an invalid value.

```python
number = int("abc")
```

**Output:**

```text
ValueError: invalid literal for int() with base 10: 'abc'
```

The string is a valid argument type for `int()`, but its contents cannot be converted into an integer.

### 2.5 `NameError`

Occurs when Python cannot find the name being referenced.

```python
print(customer_name)
```

If `customer_name` has not been defined in the accessible scope, Python raises a `NameError`.

### 2.6 `AttributeError`

Occurs when an object does not provide the requested attribute or method.

```python
numbers = [1, 2, 3]

numbers.upper()
```

**Output:**

```text
AttributeError
```

Lists do not provide an `upper()` method.

### 2.7 `ZeroDivisionError`

Occurs when an arithmetic division operation uses zero as its divisor.

```python
result = 10 / 0
```

**Output:**

```text
ZeroDivisionError: division by zero
```

### 2.8 `FileNotFoundError`

Occurs when an operation attempts to access a file that cannot be found at the specified path.

```python
with open("missing_file.txt", "r") as file:
    content = file.read()
```

If the file does not exist at the specified location, Python raises a `FileNotFoundError`.

### 2.9 `ModuleNotFoundError`

Occurs when Python cannot locate a module being imported.

```python
import nonexistent_module
```

Python raises a `ModuleNotFoundError`.

This can happen because the module does not exist, is unavailable in the active environment, or cannot be found through Python's import system.

### Quick Reference

| Exception | Typical Cause |
|---|---|
| `SyntaxError` | Invalid Python syntax |
| `IndexError` | Invalid sequence index |
| `KeyError` | Missing dictionary key |
| `TypeError` | Operation used with an inappropriate type |
| `ValueError` | Invalid value supplied to an operation |
| `NameError` | Undefined or inaccessible name |
| `AttributeError` | Missing attribute or method |
| `ZeroDivisionError` | Division by zero |
| `FileNotFoundError` | File cannot be found |
| `ModuleNotFoundError` | Module cannot be located |

---

## 3. What Is Exception Handling?

**Exception handling** is the process of responding to runtime errors so that a program can recover, report a problem, clean up resources, or terminate in a controlled way.

Consider this example:

```python
numerator = 10
denominator = 0

print(numerator / denominator)
```

Without exception handling, the unhandled `ZeroDivisionError` stops normal execution of the current operation and produces a traceback.

Exception handling allows us to define what should happen when an expected failure occurs.

For example:

```python
numerator = 10
denominator = 0

try:
    result = numerator / denominator

except ZeroDivisionError:
    print("Cannot divide by zero.")
```

**Output:**

```text
Cannot divide by zero.
```

Instead of allowing the exception to remain unhandled, the program catches it and displays a meaningful message.

**Key idea:** Exception handling helps a program respond to failures deliberately instead of allowing every runtime error to terminate normal execution unexpectedly.

---

## 4. Handling Exceptions with `try` and `except`

Python uses `try` and `except` to handle exceptions.

### Basic Structure

```python
try:
    # Code that may raise an exception

except SomeException:
    # Code that handles the exception
```

- **`try`** — Contains code that may raise an exception.
- **`except`** — Handles a matching exception raised while executing the `try` block.

### Example

```python
try:
    numerator = 10
    denominator = 0

    result = numerator / denominator

    print(result)

except ZeroDivisionError:
    print("Cannot divide by zero.")
```

**Output:**

```text
Cannot divide by zero.
```

When Python encounters the division by zero, it skips the remaining statements in the `try` block and looks for a matching `except` handler.

Because the handler matches `ZeroDivisionError`, the program executes its code.

### Important Behavior

If an exception occurs in the `try` block:

1. Python stops executing the remaining statements in that block.
2. Python looks for a matching exception handler.
3. If a matching handler is found, its code executes.
4. If no matching handler exists, the exception propagates to the calling code.

If no exception occurs, the `except` block is skipped.

---

## 5. Catching Specific Exceptions

It is generally better to catch the exceptions you expect rather than catch every possible exception indiscriminately.

### Avoid Catching Everything Blindly

```python
try:
    result = 10 / 0

except:
    print("Something went wrong.")
```

This catches a broad range of exceptions, including exceptions that you may not intend to handle.

It can hide programming errors and make debugging difficult.

### Prefer a Specific Exception

```python
try:
    result = 10 / 0

except ZeroDivisionError:
    print("Cannot divide by zero.")
```

This communicates exactly which problem the program expects to handle.

### Handling Multiple Exception Types

A program can handle different exceptions separately.

```python
try:
    number = int(input("Enter a number: "))
    result = 100 / number

except ValueError:
    print("Please enter a valid integer.")

except ZeroDivisionError:
    print("The number cannot be zero.")
```

If the input is not a valid integer, Python raises `ValueError`.

If the input is `0`, Python raises `ZeroDivisionError`.

Each error receives a different response.

**Key idea:** Catch exceptions at the point where you can respond to them meaningfully.

---

## 6. Accessing Exception Information

You can use `as` to bind an exception object to a variable.

This makes its information available inside the handler.

### Example

```python
try:
    number = int("abc")

except ValueError as error:
    print("Conversion failed:", error)
```

**Output:**

```text
Conversion failed: invalid literal for int() with base 10: 'abc'
```

Here, `error` refers to the exception object.

You can use it to inspect or report the error message.

### Why Is This Useful?

Exception information can help you:

- Understand why an operation failed.
- Display an informative error message.
- Record diagnostic information in logs.
- Investigate problems during debugging.

However, avoid exposing sensitive internal details to end users. Detailed diagnostic information is often better suited to application logs.

---

## 7. Understanding Tracebacks

A **traceback** shows the sequence of calls that led to an unhandled exception.

It commonly includes:

- The file in which the problem occurred.
- The line number associated with the failure.
- The sequence of function calls leading to the failure.
- The exception type and message.

Example:

```python
def calculate_total():
    return 10 / 0


def main():
    calculate_total()


main()
```

When this code runs without exception handling, Python displays a traceback showing the calls from `main()` to `calculate_total()` and the resulting `ZeroDivisionError`.

### How to Read a Traceback

When debugging:

1. Read the exception type and message at the end.
2. Identify the line where the exception occurred.
3. Trace the function calls to understand how execution reached that line.
4. Inspect the relevant values and assumptions.
5. Fix the underlying problem rather than merely hiding the error.

### Traceback vs Exception Message

| Term | Meaning |
|---|---|
| Exception | The object representing a runtime failure |
| Exception message | A description of the specific failure |
| Traceback | Information about the call sequence and location associated with the exception |

**Key idea:** A traceback is a debugging tool, not just an error message.

---

## 8. Python Exception Hierarchy

Python organizes exceptions into a class hierarchy.

At a simplified level:

```text
BaseException
│
├── SystemExit
├── KeyboardInterrupt
├── GeneratorExit
└── Exception
    │
    ├── ArithmeticError
    │   └── ZeroDivisionError
    │
    ├── LookupError
    │   ├── IndexError
    │   └── KeyError
    │
    ├── TypeError
    ├── ValueError
    ├── NameError
    ├── AttributeError
    ├── OSError
    │   └── FileNotFoundError
    └── ImportError
        └── ModuleNotFoundError
```

This diagram illustrates selected relationships rather than every exception in Python.

### Why Does the Hierarchy Matter?

Exception inheritance allows you to handle related exceptions using a common base class.

For example:

```python
try:
    result = 10 / 0

except ArithmeticError:
    print("An arithmetic error occurred.")
```

`ZeroDivisionError` is a subclass of `ArithmeticError`, so this handler can catch it.

### Why Prefer `Exception` Over `BaseException`?

Most application-level exceptions inherit from `Exception`.

A handler such as:

```python
except Exception:
    ...
```

can catch many ordinary runtime exceptions.

By contrast, `BaseException` also includes special exceptions such as `KeyboardInterrupt` and `SystemExit`, which are generally not meant to be swallowed by ordinary application error handlers.

**Best practice:** Catch specific exception types whenever practical. Use broader handlers only when their behavior is intentional.

---

## 9. The `else` Block

Python provides an optional `else` block for `try` statements.

The `else` block executes **only when the `try` block completes without raising an exception**.

### Example

```python
try:
    result = 10 / 2

except ZeroDivisionError:
    print("Cannot divide by zero.")

else:
    print("Calculation successful.")
    print("Result:", result)
```

**Output:**

```text
Calculation successful.
Result: 5.0
```

If the division raises `ZeroDivisionError`, the `except` block executes instead, and the `else` block is skipped.

### Why Use `else`?

The `else` block separates successful-operation logic from the code being monitored for exceptions.

This can make it easier to identify which operation is expected to fail.

For example:

```python
try:
    number = int("25")

except ValueError:
    print("Invalid number.")

else:
    print("Converted number:", number)
```

**Key idea:** Use `else` for code that should execute only after the `try` block succeeds.

---

## 10. The `finally` Block

The `finally` block contains code that normally executes whether the `try` block succeeds or an exception occurs.

It is commonly used for cleanup operations.

### Example

```python
try:
    result = 10 / 2
    print("Result:", result)

except ZeroDivisionError:
    print("Cannot divide by zero.")

finally:
    print("Execution finished.")
```

**Output:**

```text
Result: 5.0
Execution finished.
```

If the division is changed to `10 / 0`, the `except` block handles the error, and the `finally` block still executes.

### Common Uses

The `finally` block can be useful for:

- Releasing resources.
- Closing connections.
- Cleaning up temporary state.
- Performing final operations before control leaves the construct.

### Important Note

For file handling, Python's `with` statement is usually preferable because it manages the file's lifecycle automatically.

For example:

```python
with open("data.txt", "r") as file:
    content = file.read()
```

The file is closed automatically when execution leaves the `with` block, including when an exception occurs.

**Key idea:** Use `finally` when cleanup logic must run after the `try`/`except` process, regardless of its normal outcome.

---

## 11. Combining `try`, `except`, `else`, and `finally`

All four blocks can be used together.

### Structure

```python
try:
    # Code that may raise an exception

except SomeException:
    # Handle the exception

else:
    # Execute when try succeeds

finally:
    # Execute during normal exit from the construct
```

### Example

```python
try:
    numerator = 10
    denominator = 2

    result = numerator / denominator

except ZeroDivisionError:
    print("Cannot divide by zero.")

else:
    print("Calculation successful.")
    print("Result:", result)

finally:
    print("Execution finished.")
```

**Output:**

```text
Calculation successful.
Result: 5.0
Execution finished.
```

### Role of Each Block

| Block | Purpose |
|---|---|
| `try` | Executes the operation that may fail |
| `except` | Handles a matching exception |
| `else` | Executes if the `try` block completes without an exception |
| `finally` | Executes during normal control flow leaving the construct, whether an exception occurred or not |

**Remember:** An exception raised inside an `except` or `else` block is not caught by another handler belonging to the same `try` statement. The `finally` block still executes during normal exception propagation.

---

## 12. Raising Exceptions with `raise`

Python allows you to raise an exception explicitly using the `raise` keyword.

This is useful when a program detects an invalid condition that should not be allowed to continue.

### Example: Validating an Age

```python
age = -5

if age < 0:
    raise ValueError("Age cannot be negative.")
```

Python raises:

```text
ValueError: Age cannot be negative.
```

The exception communicates that the supplied value violates the expected rule.

### Example: Validating a Withdrawal

```python
balance = 500
withdrawal = 1000

if withdrawal > balance:
    raise ValueError("Insufficient balance.")
```

The arithmetic comparison is valid, but the withdrawal violates the application's business rule.

A more domain-specific exception could also be appropriate, as discussed in the next section.

### Re-raising an Exception

Inside an exception handler, `raise` without an argument re-raises the currently handled exception.

```python
try:
    result = 10 / 0

except ZeroDivisionError:
    print("An arithmetic error occurred.")
    raise
```

The message is printed, but the exception is raised again and can propagate to a higher-level handler.

This is useful when you want to perform local logging or cleanup without suppressing the original failure.

**Key idea:** Use `raise` when a condition should be communicated as an exception rather than treated as a successful result.

---

## 13. `raise` vs `except`

These keywords serve different purposes.

### `raise`

Explicitly raises an exception.

```python
raise ValueError("Invalid amount.")
```

### `except`

Handles a matching exception.

```python
try:
    raise ValueError("Invalid amount.")

except ValueError as error:
    print("Handled:", error)
```

**Output:**

```text
Handled: Invalid amount.
```

### Comparison

| `raise` | `except` |
|---|---|
| Triggers an exception | Handles a matching exception |
| Can communicate an invalid condition | Defines how to respond to a failure |
| Can cause execution to leave the current code path | Executes when a matching exception is caught |

### Mental Model

```text
raise
  ↓
An exception is triggered
  ↓
Python searches for a matching handler
  ↓
except handles the exception
```

If no matching handler is found, the exception propagates up the call stack.

---

## 14. Creating Custom Exceptions

Python provides many built-in exceptions, but applications sometimes need exception types that describe their own business rules.

A **custom exception** is an exception class defined by the developer.

Custom exceptions normally inherit from `Exception`.

### 14.1 Creating a Basic Custom Exception

```python
class MyException(Exception):
    pass
```

Here:

- `MyException` is the custom exception class.
- `Exception` is its parent class.
- `pass` indicates that the class has no additional implementation.

### 14.2 Raising a Custom Exception

```python
raise MyException("Something went wrong.")
```

Python raises an instance of `MyException`.

### 14.3 Handling a Custom Exception

```python
class MyException(Exception):
    pass


try:
    raise MyException("Something went wrong.")

except MyException as error:
    print(error)
```

**Output:**

```text
Something went wrong.
```

Because the exception inherits from `Exception`, it can be handled like other ordinary application-level exceptions.

### 14.4 Creating a Domain-Specific Exception

Imagine a banking application that needs to report a withdrawal exceeding the available balance.

```python
class InsufficientBalanceError(Exception):
    pass
```

The application can raise this exception when the rule is violated:

```python
balance = 500
withdrawal = 1000

if withdrawal > balance:
    raise InsufficientBalanceError(
        "Insufficient balance for this withdrawal."
    )
```

The exception name communicates the problem more precisely than a generic error name.

### 14.5 Handling the Custom Exception

```python
class InsufficientBalanceError(Exception):
    pass


def withdraw(balance, amount):
    if amount > balance:
        raise InsufficientBalanceError(
            "Insufficient balance."
        )

    return balance - amount


try:
    remaining_balance = withdraw(500, 1000)

except InsufficientBalanceError as error:
    print(error)

else:
    print("Remaining balance:", remaining_balance)
```

**Output:**

```text
Insufficient balance.
```

This example demonstrates how a function can signal a business-rule violation and allow its caller to decide how to respond.

### Do Custom Exceptions Need a Constructor?

Not always.

A simple custom exception can inherit its constructor and behavior from `Exception`:

```python
class InvalidPINError(Exception):
    pass
```

You can then supply a message when raising it:

```python
raise InvalidPINError("The PIN is invalid.")
```

You generally do not need to implement `__init__()` or `__str__()` merely to provide a custom error message.

If an exception needs additional structured information, you can define extra attributes or customize its constructor.

**Key idea:** Create custom exceptions when they make application-specific failures easier to recognize and handle.

---

## 15. When to Use Custom Exceptions

Custom exceptions are especially useful when an application has domain-specific rules.

Examples include:

- `InsufficientBalanceError`
- `InvalidPINError`
- `InvalidTransactionError`
- `DataValidationError`

These names help communicate what went wrong.

### Built-in vs Custom Exceptions

| Situation | Possible Exception |
|---|---|
| A string cannot be converted to an integer | `ValueError` |
| A dictionary key is missing | `KeyError` |
| An operation uses an inappropriate type | `TypeError` |
| A withdrawal exceeds the available balance | `InsufficientBalanceError` |
| A business-specific transaction rule is violated | `InvalidTransactionError` |

The custom names in the table are examples that you would define yourself.

### Benefits of Custom Exceptions

- **Meaningful names:** Communicate the problem clearly.
- **Specific handling:** Allow callers to catch a particular application error.
- **Better organization:** Separate business-rule failures from unrelated programming errors.
- **Maintainability:** Make error-handling behavior easier to understand as an application grows.

### Why Inherit from `Exception`?

A custom exception should normally inherit from `Exception` so that it participates in Python's standard exception-handling mechanisms.

```python
class DataValidationError(Exception):
    pass
```

This allows code to raise and catch it:

```python
try:
    raise DataValidationError("Invalid dataset.")

except DataValidationError as error:
    print(error)
```

**Best practice:** Do not create a custom exception when an existing built-in exception already describes the problem adequately.

---

## 16. Error Handling vs Error Prevention

Exception handling is not a reason to place every line of code inside a `try` block.

A good program prevents invalid operations when practical and handles expected failures when they occur.

### Example: Handling a Missing File

```python
try:
    with open("data.txt", "r") as file:
        content = file.read()

except FileNotFoundError:
    print("The requested file does not exist.")
```

A file might genuinely be missing, so catching `FileNotFoundError` is a reasonable response.

### Avoid Catching Every Possible Error Without a Plan

```python
try:
    # Entire application
    ...

except Exception:
    print("Something went wrong.")
```

This broad handler can hide the original problem and make debugging difficult.

It may also prevent higher-level code from responding appropriately to a failure.

### Better Approach

1. Identify the operation that can fail.
2. Determine which exceptions are expected.
3. Catch those exceptions where the program can respond meaningfully.
4. Allow unexpected exceptions to surface during development or propagate to an appropriate higher-level handler.
5. Log useful diagnostic details when appropriate.

**Key idea:** Good exception handling makes failures understandable and manageable; it should not conceal defects.

---

## 17. Exception Handling Flow

A simplified view of a `try` statement is:

```text
             Start
               |
               v
              try
               |
        Did try raise an
          exception?
          /       \
        No         Yes
        |            |
        v            v
       else     Find a matching
                except handler
                     |
              Matching handler?
                /        \
              Yes         No
               |           |
               v           v
            Handle      Propagate
            exception   exception
                \          /
                 \        /
                  v      v
                   finally
                      |
                      v
                     Exit
```

This diagram is a simplified model of normal exception flow.

A few important details:

- The `else` block executes only when the `try` block finishes without an exception.
- A matching `except` block handles the exception.
- If no matching handler is found, the exception propagates.
- The `finally` block executes during normal control flow leaving the construct, including when an exception propagates.

**Important:** If an exception is raised inside an `else` or `except` block, it is not handled by another `except` belonging to the same `try` statement.

---

## 18. Best Practices

Follow these practices when writing production-quality Python code.

### Catch Specific Exceptions

Prefer:

```python
try:
    number = int(user_input)

except ValueError:
    print("Enter a valid integer.")
```

over a bare `except` that catches everything without distinguishing the failure.

### Keep `try` Blocks Focused

Place the operation that may fail inside the `try` block rather than wrapping unrelated application logic around it.

### Avoid Silencing Exceptions

Do not catch an exception and do nothing unless suppressing it is explicitly intended and safe.

### Use Meaningful Error Messages

Prefer:

```python
raise ValueError("Age cannot be negative.")
```

over an ambiguous message such as:

```python
raise ValueError("Error.")
```

### Use Custom Exceptions for Domain-Specific Problems

Create custom exceptions when callers need to distinguish application-specific failures.

### Use `with` for Resource Management

For files and other supported resources, prefer context managers when available.

```python
with open("data.txt", "r") as file:
    content = file.read()
```

### Preserve Unexpected Failures

Do not convert every unexpected error into a generic success or failure message that hides the underlying issue.

### Use Logging When Appropriate

For applications, logging can preserve diagnostic information for investigation.

```python
import logging

logging.basicConfig(level=logging.ERROR)

try:
    result = 10 / 0

except ZeroDivisionError:
    logging.exception("The calculation failed.")
```

`logging.exception()` includes exception traceback information when called inside an exception handler.

Avoid logging sensitive data such as passwords, access tokens, or personal information.

---

## 19. Common Mistakes

### Mistake 1: Using a Bare `except`

```python
try:
    result = 10 / 0

except:
    print("Error.")
```

**Why it is problematic:** It can catch exceptions you did not intend to handle, including special exceptions such as `KeyboardInterrupt`.

**Better approach:**

```python
try:
    result = 10 / 0

except ZeroDivisionError:
    print("Cannot divide by zero.")
```

### Mistake 2: Using `raise` as a Replacement for Printing

`raise` does not simply display an error message. It raises an exception and changes the program's control flow.

```python
raise ValueError("Invalid input.")
```

Use it when an operation should signal failure to the caller.

### Mistake 3: Creating Custom Exceptions for Everything

Custom exceptions are useful when they express meaningful application-specific conditions.

Do not create a new exception class if a built-in exception already describes the problem adequately.

### Mistake 4: Putting Too Much Code Inside `try`

A large `try` block can make it difficult to determine which operation caused an exception.

Keep exception-handling boundaries as focused as practical.

### Mistake 5: Hiding the Original Error

```python
try:
    result = perform_calculation()

except Exception:
    print("Something went wrong.")
```

This might be appropriate for a carefully designed user-facing boundary, but on its own it discards useful diagnostic context and can hide unexpected bugs.

During development, preserve the traceback and investigate the underlying cause.

### Mistake 6: Confusing `else` and `finally`

- `else` runs only when the `try` block completes without an exception.
- `finally` runs during normal control flow leaving the construct, whether the `try` block succeeds or an exception occurs.

They serve different purposes.

---

## ✅ Key Concepts

| Concept | Purpose |
|---|---|
| `SyntaxError` | Indicates invalid Python syntax |
| Exception | Represents a failure or exceptional condition during execution |
| `try` | Contains code whose exceptions may be handled |
| `except` | Handles a matching exception |
| `else` | Runs when the `try` block completes without an exception |
| `finally` | Performs final operations during normal exit from the construct |
| `raise` | Explicitly raises an exception |
| `Exception` | Base class for most ordinary application-level exceptions |
| Custom Exception | Represents an application-specific failure |
| `as error` | Binds an exception object to a variable |
| Traceback | Shows the call sequence and location associated with an exception |
| Exception Hierarchy | Organizes exception types through inheritance |

---

## 🎓 Learning Takeaway

The central idea behind Python error handling is to identify failures, decide which ones can be handled meaningfully, and ensure that unexpected problems are not silently hidden.

### Basic Exception Handling

```text
Operation
    |
    v
Exception occurs?
    |
    +---- No ----> Continue execution
    |
    +---- Yes ---> Find a matching handler
                       |
                       +---- Found ----> Handle exception
                       |
                       +---- Not found -> Propagate exception
```

### Using `else` and `finally`

```text
try
 |
 +---- Completes successfully ----> else
 |
 +---- Raises exception ----------> except (if matched)
                                      |
                                      v
                                   finally
```

### Handling Business Rules

```text
Business rule violated
        |
        v
raise a meaningful exception
        |
        v
Caller receives the exception
        |
        v
Appropriate handler responds
```

Once you understand these patterns, you can write Python programs that are easier to debug, more predictable, and more maintainable.

---

## 📖 Learning Resources

### Video Tutorials

- [CampusX — Python Error Handling](https://www.youtube.com/live/rvKR6tciJ2Q?si=5HHG3aI2xFnWqa4m)
- [Visually Explained — YouTube](https://youtu.be/XN3udSTGAOM?si=iDcfOnVJ_WGdphHT)

### Articles and Practice

- [GeeksforGeeks — Python Exception Handling](https://www.geeksforgeeks.org/python/python-exception-handling/)
- [30 Days of Python — Exception Handling](https://github.com/Asabeneh/30-Days-Of-Python/blob/master/17_Day_Exception_handling/17_exception_handling.md)
- [Coddy — Error Handling Practice](https://coddy.tech/learn/python/logic_and_flow/what_is_error_handling)

### Official Documentation

- [Python Tutorial — Errors and Exceptions](https://docs.python.org/3/tutorial/errors.html)
- [Python Built-in Exceptions](https://docs.python.org/3/library/exceptions.html)
- [Python Logging Documentation](https://docs.python.org/3/library/logging.html)
- [Python `with` Statement and Context Managers](https://docs.python.org/3/reference/compound_stmts.html#the-with-statement)

---

**Next Step:** Practise handling `ValueError`, `ZeroDivisionError`, and `FileNotFoundError`. Then create a custom exception for a business rule and write a function that raises it when the rule is violated.
