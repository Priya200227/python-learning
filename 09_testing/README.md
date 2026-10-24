# Python Testing with Pytest

A practical introduction to unit testing in Python using `pytest`, covering test fundamentals, assertions, test discovery, edge cases, and exception testing.

## 🎯 Learning Objectives

- Understand software testing and unit testing.
- Install and configure `pytest`.
- Write and execute test functions.
- Use assertions to validate expected behavior.
- Test multiple inputs and boundary conditions.
- Verify that functions raise expected exceptions.
- Interpret test results and diagnose failures.

## 📚 Table of Contents

1. [Introduction to Testing](#1-introduction-to-testing)
2. [Why Testing Matters](#2-why-testing-matters)
3. [Introduction to Pytest](#3-introduction-to-pytest)
4. [Installation](#4-installation)
5. [Project Structure](#5-project-structure)
6. [Writing Your First Test](#6-writing-your-first-test)
7. [Running Tests](#7-running-tests)
8. [Testing Multiple Inputs and Edge Cases](#8-testing-multiple-inputs-and-edge-cases)
9. [Testing Exceptions](#9-testing-exceptions)
10. [Understanding Test Results](#10-understanding-test-results)
11. [Best Practices](#11-best-practices)
12. [Key Takeaways](#12-key-takeaways)
13. [References](#13-references)

---

## 1. Introduction to Testing

Software testing is the process of verifying that a program behaves as expected under specified conditions.

As applications grow, manually checking every output becomes inefficient and error-prone. Automated tests allow us to validate behavior repeatedly whenever code changes.

**Unit testing** focuses on testing small, individual units of code, such as functions or methods, to verify that they produce the expected results.

For example, a function that adds two numbers should return the correct sum for valid inputs.

```python
def add(a, b):
    return a + b
```

A test can verify its behavior:

```python
assert add(2, 3) == 5
```

If the returned value differs from the expected value, the assertion fails.

## 2. Why Testing Matters

Testing helps developers build reliable and maintainable software.

Key benefits include:

- **Correctness:** Verifies that functions produce expected results.
- **Early error detection:** Identifies defects before they affect other parts of an application.
- **Regression prevention:** Detects when code changes break existing functionality.
- **Edge-case validation:** Checks behavior for boundary values and unusual inputs.
- **Maintainability:** Makes it easier to modify code with confidence.

Tests do not guarantee that software is completely error-free, but they provide repeatable evidence that specific behaviors work as intended.

## 3. Introduction to Pytest

`pytest` is a Python testing framework that makes writing and running tests straightforward.

Python also provides the built-in `unittest` module. This section focuses on pytest because of its concise syntax and automatic test discovery.

### Key Features

- Simple assertions using Python's `assert` statement.
- Automatic discovery of test files and functions.
- Detailed failure reports.
- Support for testing expected exceptions.
- Compatibility with small scripts and larger Python projects.

## 4. Installation

Install pytest using the terminal:

```bash
python -m pip install pytest
```

Verify the installation:

```bash
python -m pytest --version
```

**Note:** Run these commands in the Python environment selected for your project in VS Code.

## 5. Project Structure

Create the following files inside the `09_testing` directory:

```text
09_testing/
├── README.md
├── main.py
└── test_main.py
```

| File | Purpose |
|---|---|
| `main.py` | Contains the application functions being tested. |
| `test_main.py` | Contains test functions that verify application behavior. |
| `README.md` | Documents the concepts, examples, and commands. |

Pytest conventionally discovers Python files named `test_*.py` or `*_test.py`. Test functions typically begin with `test_`.

## 6. Writing Your First Test

Consider a function that classifies a temperature as hot or cold.

### `main.py`

```python
def get_weather(temp):
    if temp > 20:
        return "hot"
    return "cold"
```

### `test_main.py`

```python
from main import get_weather


def test_get_weather():
    assert get_weather(25) == "hot"
    assert get_weather(10) == "cold"
    assert get_weather(20) == "cold"
```

### Understanding Assertions

An assertion checks whether a condition is true.

```python
assert actual_result == expected_result
```

- If the condition is true, execution continues.
- If the condition is false, Python raises an `AssertionError`.
- Pytest captures assertion failures and reports useful diagnostic information.

For example:

```python
assert get_weather(25) == "hot"
```

The function returns `"hot"`, so the assertion passes.

However, if the expected result is incorrectly written as `"cold"`, the assertion fails.

## 7. Running Tests

Open the terminal in the `09_testing` directory.

### Run all discovered tests

```bash
python -m pytest
```

### Display detailed test results

```bash
python -m pytest -v
```

### Run a specific test file

```bash
python -m pytest test_main.py
```

### Run a specific test function

```bash
python -m pytest test_main.py::test_get_weather
```

The `-v` option enables verbose output, making it easier to identify individual tests and their results.

## 8. Testing Multiple Inputs and Edge Cases

Testing only one typical input is insufficient to establish that a function behaves correctly across its intended input range.

Consider these test cases for `get_weather()`:

| Input | Expected result | Reason |
|---:|---|---|
| `25` | `"hot"` | Typical hot temperature |
| `21` | `"hot"` | Just above the threshold |
| `20` | `"cold"` | Boundary condition |
| `10` | `"cold"` | Typical cold temperature |
| `0` | `"cold"` | Additional input condition |

The boundary value `20` is particularly important because the function uses `temp > 20`, not `temp >= 20`.

Testing boundary values helps identify errors in comparison operators and conditional logic.

## 9. Testing Exceptions

Some functions should raise exceptions when they receive invalid inputs or encounter prohibited operations.

Consider a division function that explicitly rejects division by zero.

### `main.py`

```python
def divide(a, b):
    if b == 0:
        raise ValueError("Cannot divide by zero")

    return a / b
```

### `test_main.py`

```python
import pytest
from main import divide


def test_divide():
    assert divide(10, 2) == 5
    assert divide(9, 3) == 3


def test_divide_by_zero():
    with pytest.raises(
        ValueError,
        match="Cannot divide by zero"
    ):
        divide(10, 0)
```

### Understanding `pytest.raises()`

`pytest.raises()` verifies that a particular exception occurs during a test.

In this example:

- `ValueError` is the expected exception type.
- `match` checks that the exception message matches the specified regular expression.
- If the expected exception is not raised, the test fails.

Exception testing helps verify that error-handling behavior is implemented correctly.

## 10. Understanding Test Results

When pytest runs your tests, it reports whether each test passed or failed.

### Example: Successful test run

```text
============================= test session starts =============================
collected 2 items

test_main.py::test_get_weather PASSED
test_main.py::test_divide PASSED

============================== 2 passed ======================================
```

The output above is illustrative. Your exact output may differ depending on your environment and the number of tests.

### Common outcomes

| Result | Meaning |
|---|---|
| `PASSED` | The test completed successfully. |
| `FAILED` | An assertion or another test condition failed. |
| `ERROR` | An error occurred while setting up or executing a test. |
| `collected` | The number of tests pytest discovered for the run. |

When a test fails, inspect the failure message, the expected value, the actual value, and the relevant application code.

## 11. Best Practices

- Give test functions descriptive names that begin with `test_`.
- Test one clear behavior per test function where practical.
- Check both typical inputs and meaningful boundary conditions.
- Test expected exceptions when error handling is part of the function's contract.
- Keep application code separate from test code.
- Use meaningful assertions rather than checking only whether code executes.
- Run tests after modifying application logic.
- Keep tests deterministic and independent of unnecessary external services.

## 12. Key Takeaways

- Testing verifies whether code behaves as expected.
- Unit tests focus on small, individual units of functionality.
- Pytest provides a simple framework for writing and executing tests.
- Test discovery relies on conventional file and function naming patterns.
- Assertions compare actual behavior against expected results.
- Edge-case testing helps uncover boundary-related defects.
- `pytest.raises()` verifies expected exceptions.
- Automated tests help detect regressions as code evolves.

## 13. References

- [Official Pytest Documentation](https://docs.pytest.org/en/stable/)
- [Pytest — Getting Started](https://docs.pytest.org/en/stable/getting-started.html)
- [Pytest — How to Invoke Pytest](https://docs.pytest.org/en/stable/how-to/usage.html)
- [Python — `unittest` Documentation](https://docs.python.org/3/library/unittest.html)

---

*This section documents the fundamentals of Python testing with pytest, with a focus on writing, running, and understanding basic unit tests.*
