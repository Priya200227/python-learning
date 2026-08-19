# 🐍 Python Modules & Packages

## Modules

A module is a Python file (`.py`) containing reusable code such as functions, classes, and variables.

Modules help us:

- Organize code into smaller files
- Reuse code across programs
- Keep projects clean and maintainable

### Creating and Using a Module

```python
# math_utils.py

def add(a, b):
    return a + b
```

```python
# main.py

import math_utils

print(math_utils.add(10, 20))
```

You can also import specific functions:

```python
from math_utils import add
```

---

## Packages

A package is a collection of related Python modules organized together.

Think of it as:

```
Package
├── Module 1
├── Module 2
└── Module 3
```

Packages help organize larger Python projects into logical sections.

---

## Libraries

A library is a collection of reusable modules and packages that provide functionality without having to build everything from scratch.

Examples:

- **Pandas** → Data manipulation and analysis
- **NumPy** → Numerical computing
- **Matplotlib** → Data visualization
- **Requests** → Working with HTTP/API requests

---

## Standard vs Community Libraries

### Standard Library

The Python Standard Library comes built into Python.

Examples:

```python
import math
import os
import datetime
```

No separate installation is required.

---

### Community / External Libraries

Community or external libraries are created by the Python community and need to be installed separately.

For example:

```bash
pip install pandas
```

---

## pip

`pip` is Python's package installer and package manager.

It is commonly used to:

- Install packages
- Update packages
- Remove packages
- Manage Python dependencies

Example:

```bash
pip install pandas
```

**Mental Model**

> pip = a package manager for Python

---

## uv

`uv` is a modern, high-performance Python package and project manager written in Rust.

It can handle several tasks that traditionally required separate tools, including:

- Creating Python projects
- Creating virtual environments
- Installing packages
- Managing dependencies
- Managing Python versions

---

## Traditional Workflow

A traditional Python workflow might look like:

```bash
python -m venv .venv
```

Activate the environment and then install packages:

```bash
pip install pandas
```

You would also manually create files such as:

```
README.md
.gitignore
main.py
```

---

## Modern Workflow with uv

**Create a Project**

```bash
uv init my-project
```

**Add a Package**

```bash
uv add pandas
```

`uv` manages the project environment and dependencies as part of the workflow.

---

## pip vs uv

| pip | uv |
|---|---|
| Traditional Python package manager | Modern Python project/package manager |
| Mainly manages packages | Manages packages, environments, and projects |
| Written in Python | Written in Rust |
| Widely used and familiar | Designed for speed and modern workflows |

**Key Idea**

> uv is not simply a replacement for pip.

It combines package management with project and environment management into a faster workflow.

---

## Project Structure with uv

A typical project initialized with `uv` can contain:

```
my-project/
│
├── .git/
├── .gitignore
├── .python-version
├── .venv/
├── main.py
├── pyproject.toml
└── README.md
```

**`pyproject.toml`**

The `pyproject.toml` file contains important project configuration and dependencies.

---

## Module → Package → Library

A simple mental model:

```
Module
   ↓
A Python file containing reusable code

Package
   ↓
A collection of related modules

Library
   ↓
Reusable functionality built from modules/packages
```

---

## Key Takeaways

- **Module** → A Python file containing reusable code.
- **Package** → A collection of related modules.
- **Library** → Reusable functionality built from modules and packages.
- **Standard Library** → Comes built into Python.
- **External Library** → Usually needs to be installed separately.
- **pip** → Traditional Python package installer and package manager.
- **uv** → Modern tool for Python projects, packages, environments, and dependencies.
- **`pyproject.toml`** → Stores important project configuration and dependency information.

## Why This Matters

Understanding modules, packages, libraries, and package managers is important for building organized Python projects.

These concepts become especially useful when working with:

- Virtual environments
- External libraries
- Dependencies
- APIs
- Automation
- Data analytics
- AI/ML projects
- Larger Python applications

--- 

# Learning Resources

- [Modules&Packages](https://youtu.be/6mw_lWlHCYk?si=MUPtmNk1u4wlLBh4)
- [Visually Explained YouTube Channel](https://youtu.be/7MOzepKojbw?si=YmlfqZz3HZyIFbsc)
- [Virtual Environment](https://youtu.be/nwN27ORTUXg?si=BKEcvgaYMBtxiOaN)
- [UV package explanation](https://youtu.be/8mk85fyzevc?si=vLZ9g2lLWyOipdzR)
