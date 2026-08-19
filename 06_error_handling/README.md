# Python Error Handling

This repository contains my notes and practice code for Exception Handling in Python.

The topic covers how Python errors occur, how to identify different error types, how exceptions differ from syntax errors, and how to handle, raise, and create custom exceptions.

## 1. Why Do Errors Happen?

Errors can occur at different stages of a Python program.

### A. During Compilation / Parsing

These are generally syntax errors.

They happen when Python cannot understand the structure of the program.

Example:

```python
if True
    print("Hello")
```

Python cannot parse this correctly, so the program does not execute.

### B. During Execution

These are exceptions.

The code is syntactically valid, but something goes wrong while the program is running.

Examples:

- IndexError
- KeyError
- ModuleNotFoundError
- TypeError
- ValueError
- NameError
- AttributeError
- ZeroDivisionError
- FileNotFoundError

## 2. Common Python Exceptions

**IndexError**

Occurs when trying to access an invalid index.

```python
a = [1, 2, 3]

print(a[100])
```

Output:

```
IndexError: list index out of range
```

**ModuleNotFoundError**

Occurs when Python cannot find the module being imported.

```python
import something_that_does_not_exist
```

**KeyError**

Occurs when a dictionary key does not exist.

```python
d = {
    "name": "Priya",
    "age": 24
}

print(d["salary"])
```

**TypeError**

Occurs when an operation or function is used with an inappropriate type.

```python
"hello" + 10
```

**ValueError**

Occurs when a function receives a value of the correct general type but an inappropriate value.

```python
int("abc")
```

**NameError**

Occurs when Python cannot find a variable or name.

```python
print(k)
```

Output:

```
NameError: name 'k' is not defined
```

**AttributeError**

Occurs when an object does not have the requested attribute or method.

```python
l = [1, 2, 3]

l.upper()
```

## 3. What Is an Exception?

An exception is an error that occurs during program execution.

Unlike a syntax error, the program can be syntactically correct and still encounter an exception while running.

Example:

```python
a = 10
b = 0

print(a / b)
```

The code is valid Python syntax, but execution raises:

```
ZeroDivisionError
```

Exception handling allows us to decide what the program should do when such situations occur.

## 4. How to Handle Exceptions

Python provides the `try` / `except` structure.

**Basic structure**

```python
try:
    # code that may raise an exception

except:
    # code that handles the exception
```

Example:

```python
try:
    a = 10
    b = 0
    print(a / b)

except:
    print("Something went wrong")
```

The basic idea is:

- `try` → contains code that may cause an exception
- `except` → handles the exception

## 5. Catching a Specific Exception

It is better to catch the specific exception when possible.

```python
try:
    a = 10
    b = 0
    print(a / b)

except ZeroDivisionError:
    print("Cannot divide by zero")
```

This is preferable to blindly catching every exception because it makes the program's intention clearer.

## 6. Accessing the Exception Object

We can store the exception object using `as`.

```python
try:
    print(k)

except Exception as e:
    print(e)
```

The variable `e` contains the exception object.

We can also use:

```python
print(e.with_traceback(e.__traceback__))
```

to inspect traceback information.

## 7. Exception Traceback

A traceback helps identify:

- where the error happened
- what type of error occurred
- the sequence of function calls that led to the error

This is useful when debugging a program.

The traceback provides information such as:

```
File location
Line number
Error type
Error message
```

## 8. Exception Hierarchy

Python exceptions are organized into a class hierarchy.

At a high level:

```
BaseException
    |
    +-- Exception
          |
          +-- ValueError
          +-- TypeError
          +-- NameError
          +-- IndexError
          +-- KeyError
          +-- AttributeError
          +-- FileNotFoundError
          +-- ...
```

Most application-level exceptions that we normally handle inherit from `Exception`.

This is why a broad handler such as:

```python
except Exception:
```

can catch many ordinary exceptions.

## 9. `else` Block

Python also provides an `else` block with `try`/`except`.

The `else` block executes when the `try` block completes without raising an exception.

```python
try:
    result = 10 / 2

except ZeroDivisionError:
    print("Cannot divide by zero")

else:
    print("Operation successful")
```

Output:

```
Operation successful
```

**Why use `else`?**

It allows us to keep the code that should execute only after successful execution separate from the code that is being monitored for exceptions.

For readability, code that is not expected to raise the handled exception does not need to be unnecessarily placed inside `try`.

## 10. `finally` Block

The `finally` block is used for code that should execute regardless of whether an exception occurs or not.

**Structure:**

```python
try:
    # code

except:
    # exception handling

finally:
    # cleanup code
```

Example:

```python
try:
    print("Opening resource")
    result = 10 / 2

except ZeroDivisionError:
    print("Error occurred")

finally:
    print("Cleanup completed")
```

The `finally` block runs whether the `try` succeeds or an exception is handled.

**Why is `finally` useful?**

It is commonly used for cleanup operations such as:

- closing files
- releasing resources
- closing connections
- cleaning up temporary resources

Conceptually:

```
try
 |
 |-- success --> else
 |
 |-- exception --> except
 |
 +--------------> finally
```

The important idea is: `finally` is for code that must run after the try/except process.

## 11. `try` + `except` + `else` + `finally`

All four blocks can be combined:

```python
try:
    result = 10 / 2

except ZeroDivisionError:
    print("Cannot divide by zero")

else:
    print("Calculation successful")

finally:
    print("Execution finished")
```

The roles are:

| Block | Purpose |
|---|---|
| try | Code that may raise an exception |
| except | Handle an exception |
| else | Run when no exception occurs |
| finally | Run regardless of success/failure |

## 12. Raising an Exception Manually

Python allows us to intentionally raise an exception using `raise`.

Example:

```python
age = -5

if age < 0:
    raise ValueError("Age cannot be negative")
```

Here, the program is deliberately raising a `ValueError`.

**Why use `raise`?**

Sometimes Python's built-in exceptions do not automatically express the business rule we want to enforce.

For example:

```python
balance = 500

withdrawal = 1000

if withdrawal > balance:
    raise ValueError("Insufficient balance")
```

The calculation itself may be valid, but the application rule is violated.

## 13. `raise` vs `except`

These two have different purposes.

**`raise`**

Used to create/trigger an exception.

```python
raise ValueError("Invalid amount")
```

**`except`**

Used to handle an exception.

```python
try:
    raise ValueError("Invalid amount")

except ValueError:
    print("Invalid amount handled")
```

Mental model:

```
raise   → "Something is wrong. Stop/transfer control."
except  → "I know how to handle that problem."
```

## 14. Custom Exceptions

Python provides many built-in exceptions, but applications sometimes need their own exception types.

A custom exception is created by defining a class that inherits from `Exception`.

Example:

```python
class MyException(Exception):
    pass
```

Now it can be raised:

```python
raise MyException("Something went wrong")
```

And handled:

```python
try:
    raise MyException("Something went wrong")

except MyException as e:
    print(e)
```

## 15. Custom Exception with a Constructor

A custom exception can define its own constructor.

```python
class MyException(Exception):

    def __init__(self, message):
        self.message = message

    def __str__(self):
        return self.message
```

Now:

```python
raise MyException("This is my custom error")
```

The custom exception can carry an application-specific message.

## 16. Why Create a Custom Exception Class?

Custom exceptions are useful when an application has domain-specific or business-specific error conditions.

For example, imagine a banking application.

Python already provides:

```
ValueError
TypeError
ZeroDivisionError
```

But the application may have a specific rule: *Withdrawal exceeds available balance*

Instead of using a generic exception everywhere:

```python
raise ValueError("Insufficient balance")
```

we can define:

```python
class InsufficientBalanceError(Exception):
    pass
```

Then:

```python
raise InsufficientBalanceError("Insufficient balance")
```

**Benefits**

Custom exceptions provide:

- **Meaningful error names** — `InsufficientBalanceError` is more descriptive than `ValueError`
- **Application-specific error handling**

```python
try:
    withdraw(10000)

except InsufficientBalanceError:
    print("You do not have enough balance.")
```

- **Separation of business errors from generic Python errors** — the program can distinguish between *Invalid input* and *Insufficient balance*
- **Better readability** — the exception name communicates what went wrong
- **Better maintainability** — as an application grows, custom exception classes make error handling more organized

## 17. Why Must a Custom Exception Inherit from `Exception`?

A custom exception should normally inherit from Python's exception hierarchy.

```python
class MyException(Exception):
    pass
```

This makes it an exception type that Python can raise and handle.

Conceptually:

```
Exception
   |
   +-- MyException
```

Without inheriting from an appropriate exception base class, the class does not behave as a normal custom exception type.

## 18. Custom Exceptions for Application-Level Errors

A useful mental model is:

```
Python errors
      |
      +-- Built-in exceptions
      |      ├── ValueError
      |      ├── TypeError
      |      ├── KeyError
      |      └── ...
      |
      +-- Custom application exceptions
             ├── InsufficientBalanceError
             ├── InvalidPINError
             └── ...
```

Built-in exceptions describe common programming/runtime problems.

Custom exceptions allow the application to describe its own rules.

## 19. Error Handling vs Error Prevention

Exception handling does not mean: *"Put every line of code inside try."*

The purpose is to handle situations that are genuinely expected or recoverable.

For example:

```python
try:
    with open("data.txt") as f:
        data = f.read()

except FileNotFoundError:
    print("File does not exist")
```

This is meaningful because a missing file is a realistic runtime condition.

Overusing:

```python
try:
    # entire program
except Exception:
    print("Something went wrong")
```

can hide bugs and make debugging harder.

## 20. Exception Handling Flow

A useful mental model:

```
             try
              |
        Does exception occur?
          /            \
        No              Yes
        |                |
      else            except
        \                /
         \              /
             finally
                |
              End
```

If an exception occurs and no matching `except` handles it, the exception continues propagating upward through the call stack.

## 21. Key Concepts Learned

| Concept | Purpose |
|---|---|
| Syntax Error | Problem detected while parsing code |
| Exception | Problem occurring during execution |
| try | Contains code that may raise an exception |
| except | Handles an exception |
| else | Runs when try succeeds without an exception |
| finally | Runs regardless of success or exception |
| raise | Explicitly raises an exception |
| Exception | Base class for most application-level exceptions |
| Custom Exception | Application-specific exception type |
| Traceback | Shows where/how an exception occurred |
| as e | Gives access to the exception object |

## 22. Common Mistakes

**Mistake 1 — Catching everything blindly**

```python
try:
    ...
except:
    print("Error")
```

This can hide the actual problem. Prefer specific exceptions when practical.

**Mistake 2 — Using `raise` without understanding why**

`raise` is not just another way to print an error. It changes control flow by raising an exception.

**Mistake 3 — Creating custom exceptions for everything**

Custom exceptions are useful for meaningful application/domain-specific conditions.

Do not create a new exception class simply because a built-in exception already describes the problem adequately.

**Mistake 4 — Putting unrelated code inside try**

A large `try` block makes it harder to identify which operation actually failed.

Keep the risky operation reasonably focused.

## 23. Practice Code

The accompanying Python practice file demonstrates:

- common exception types
- try/except
- exception objects
- traceback concepts
- else
- finally
- raise
- custom exception classes
- custom exception constructors
- exception hierarchy

The examples are intentionally small so that each concept can be isolated and understood.

## 24. Learning Takeaway

The most important mental model from this topic is:

```
Something can go wrong
        ↓
Python raises an exception
        ↓
try detects the risky operation
        ↓
except handles a matching exception
        ↓
else runs after successful try
        ↓
finally performs guaranteed cleanup
```

And when the application's own rules need to trigger an error:

```
Application rule violated
        ↓
raise
        ↓
Built-in or custom exception
        ↓
except handles it
```

For custom business rules:

```
Business-specific problem
        ↓
Create custom Exception class
        ↓
raise CustomException(...)
        ↓
Handle it explicitly
```
# Learning Resources

- [CampusX Error Handling](https://www.youtube.com/live/rvKR6tciJ2Q?si=5HHG3aI2xFnWqa4m)
- [GeekforGeeks](https://www.geeksforgeeks.org/python/python-exception-handling/)
- [Python Official Document](https://docs.python.org/3/tutorial/errors.html)
- [For Practice](https://coddy.tech/learn/python/logic_and_flow/what_is_error_handling)
