# 🐍 Python Functions

Functions are reusable blocks of code designed to perform a specific task.

They help make programs:

* Modular
* Readable
* Reusable
* Easier to maintain

---

# Types of Functions

Python functions can be categorized into several types.

### Built-in Functions

Functions provided directly by Python.

Examples:

```python
print()
len()
sum()
```

### Standard Library Functions

Functions available through Python's standard library modules.

Examples include functionality from modules such as:

```python
math
datetime
```

### External Library Functions

Functions provided by external packages that are installed separately.

### User-defined Functions

Functions created by the programmer using the `def` keyword.

```python
def greet():
    print("Hi")
```

---

# Defining and Calling a Function

A function is defined using the `def` keyword.

```python
def greet():
    print("Hi")

greet()
```

A function can contain:

* Function name
* Parameters (optional)
* Function body
* `return` statement (optional)

The function body executes only when the function is called.

---

# Parameters and Arguments

**Parameters** are variables defined in the function.

**Arguments** are the actual values passed during the function call.

```python
def multiply(a, b):   # parameters
    return a * b

multiply(3, 5)        # arguments
```

## Positional Arguments

Values are assigned according to their position.

```python
multiply(3, 5)
```

Here:

```text
a → 3
b → 5
```

## Keyword Arguments

Values are passed using parameter names.

```python
multiply(a=3, b=5)
```

Keyword arguments make function calls more explicit and readable.

---

# Default Parameters

A parameter can have a default value.

```python
def greet(name, country="India"):
    print(name, country)
```

If no value is provided for `country`, the default value is used.

For example:

```python
greet("Priya")
```

The default value `"India"` is used.

### Important

Parameters with default values should come after non-default parameters.

```python
# Correct
def greet(name, country="India"):
    ...
```

---

# `return`

`return` sends a value back to the caller and ends the function.

```python
def add(a, b):
    return a + b

result = add(2, 3)
```

The returned value can be stored in a variable:

```python
result = add(2, 3)

print(result)
```

If a function doesn't explicitly return a value, Python returns `None`.

---

# `*args` and `**kwargs`

## `*args`

`*args` is used when a function needs to accept an unknown number of positional arguments.

```python
def total(*args):
    return sum(args)

total(1, 2, 3, 4)
```

Here, the values are collected into `args`.

```text
1, 2, 3, 4 → args
```

### Key Idea

```text
*args → multiple positional arguments
```

---

## `**kwargs`

`**kwargs` is used when a function needs to accept an unknown number of keyword arguments.

```python
def create_user(**kwargs):
    print(kwargs)

create_user(name="Priya", age=24)
```

The keyword arguments are collected into a dictionary.

### Key Idea

```text
**kwargs → multiple keyword arguments
```

---

# Scope: Local vs Global Variables

## Local Variable

A local variable is created inside a function and is normally accessible only inside that function.

```python
def test():
    y = 20
```

Here, `y` is a local variable.

## Global Variable

A global variable is created outside a function and can be accessed from different parts of the program.

```python
x = 10

def test():
    y = 20
```

Here, `x` is a global variable.

### Best Practice

Avoid unnecessary global variables because they can make program behavior harder to understand.

---

# Multiple Input and Output

Functions can accept multiple parameters and return multiple values.

```python
def calculate(a, b):
    return a + b, a * b

result = calculate(2, 3)
```

Python actually returns the multiple values as a **tuple**.

Conceptually:

```python
result = (5, 6)
```

---

# Function Design

A good function should generally:

* Do one clear task
* Have a meaningful name
* Be easy to read and reuse
* Accept appropriate inputs
* Return useful output when required

## Naming Functions

Python convention uses **snake_case** for function names.

```python
def clean_customer_data():
    ...
```

A function name should clearly communicate what the function does.

---

# Functions in a Program

Functions can be divided based on their purpose.

### Input/Action Functions

Receive input or perform an action.

### Validation Functions

Check whether data is valid.

```python
def is_valid_email(email):
    # validation
    ...
```

### Transformation Functions

Modify or process data.

```python
def clean_email(email):
    # transformation
    ...
```

### Orchestration Functions

Control the overall program flow by calling other functions.

```python
def process_user():
    # orchestration
    ...
```

---

# 11. Functions Working Together

A larger problem can be broken into smaller, focused functions.

For example:

```python
def is_valid_email(email):
    # validation
    ...

def clean_email(email):
    # transformation
    ...

def process_user():
    # orchestration
    ...
```

The main idea is:

```text
Large Problem
      ↓
Small Functions
      ↓
Each Function Performs One Task
      ↓
Functions Work Together
      ↓
Complete Program
```

This makes code easier to understand, test, reuse, and maintain.

---

# 12. Key Takeaways

* Functions are reusable blocks of code designed to perform specific tasks.
* `def` is used to define a function.
* A function runs when it is called.
* Parameters are variables defined by the function.
* Arguments are values passed to the function.
* Arguments can be positional or keyword-based.
* Default parameters provide fallback values.
* `return` sends a value back to the caller and ends the function.
* Functions without an explicit `return` statement return `None`.
* `*args` accepts multiple positional arguments.
* `**kwargs` accepts multiple keyword arguments.
* Local variables belong to the function's local scope.
* Global variables are defined outside functions.
* A good function should generally perform one clear task.
* Python convention uses `snake_case` for function names.
* Functions can be organized around validation, transformation, actions, and orchestration.
* Breaking a large problem into small functions makes programs more modular and maintainable.

---

# Why This Matters

Functions are one of the most important building blocks of structured Python programming.

They allow complex programs to be broken into **small, focused, reusable pieces of logic**.

These concepts form the foundation for later topics such as:

* File handling
* Exception handling
* APIs
* Automation
* Data processing
* Pandas
* ETL workflows
* AI and application development

