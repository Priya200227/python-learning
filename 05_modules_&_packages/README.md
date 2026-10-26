# 🐍 Python Modules & Packages

Learn how to organize Python code using modules and packages, work with libraries, manage dependencies using `pip`, and explore modern Python project management with `uv`.

This section covers the tools and concepts that help transform individual Python scripts into organized, reusable, and maintainable projects.

---

## 📚 Table of Contents

- [Learning Objectives](#-learning-objectives)
- [1. Modules](#1-modules)
  - [Creating a Module](#creating-a-module)
  - [Importing Modules](#importing-modules)
- [2. Packages](#2-packages)
- [3. Libraries](#3-libraries)
- [4. Standard Library vs External Libraries](#4-standard-library-vs-external-libraries)
- [5. pip — Python Package Installer](#5-pip--python-package-installer)
- [6. uv — Modern Python Project Manager](#6-uv--modern-python-project-manager)
  - [Traditional Workflow](#traditional-workflow)
  - [Workflow with uv](#workflow-with-uv)
- [7. pip vs uv](#7-pip-vs-uv)
- [8. Project Structure with uv](#8-project-structure-with-uv)
- [9. Module vs Package vs Library](#9-module-vs-package-vs-library)
- [10. Best Practices](#10-best-practices)
- [Key Takeaways](#-key-takeaways)
- [Learning Resources](#-learning-resources)

---

## 🎯 Learning Objectives

By the end of this section, you should understand:

- What modules, packages, and libraries are.
- How to create and import Python modules.
- How packages help organize larger projects.
- The difference between the Python Standard Library and external libraries.
- How to install and manage packages using `pip`.
- What `uv` does and how it simplifies Python project management.
- The purpose of `pyproject.toml` and virtual environments.
- How dependencies fit into a maintainable Python project.

---

## 1. Modules

A **module** is a Python file (`.py`) containing reusable code, such as functions, classes, and variables.

Modules help you:

- Organize code into smaller, manageable files.
- Reuse functionality across multiple programs.
- Avoid duplicating code.
- Keep projects easier to maintain and understand.

### Creating a Module

Suppose you want to create reusable mathematical functions.

Create a file named `math_utils.py`:

```python
# math_utils.py

def add(a, b):
    return a + b


def subtract(a, b):
    return a - b
```

This file is a module named `math_utils`.

### Importing Modules

Create another file named `main.py`:

```python
# main.py

import math_utils

print(math_utils.add(10, 20))
print(math_utils.subtract(20, 5))
```

**Output:**

```text
30
15
```

When you import `math_utils`, you can access its functions using the module name followed by a dot.

```python
math_utils.add(10, 20)
```

### Importing Specific Functions

You can import individual functions directly:

```python
from math_utils import add

print(add(10, 20))
```

You can also import multiple functions:

```python
from math_utils import add, subtract

print(add(10, 20))
print(subtract(20, 5))
```

### Importing with an Alias

An alias provides an alternative name for an imported module or function.

```python
import math_utils as mu

print(mu.add(10, 20))
```

Aliases are particularly common when importing third-party libraries:

```python
import pandas as pd
import numpy as np
```

**Key idea:** Modules allow you to separate reusable functionality from the code that uses it.

---

## 2. Packages

A **package** organizes related Python modules into a directory structure.

Packages become useful when a project grows beyond a few files and its functionality needs to be divided into logical components.

### Example Package Structure

```text
my_project/
│
├── main.py
│
└── utilities/
    ├── __init__.py
    ├── math_utils.py
    └── string_utils.py
```

Here:

- `utilities/` is the package directory.
- `math_utils.py` contains mathematical functions.
- `string_utils.py` contains string-related functions.
- `__init__.py` can mark the directory as a regular Python package and optionally define package initialization behavior.

Modern Python also supports **namespace packages**, which can work without an `__init__.py` file.

### Importing from a Package

Suppose `math_utils.py` contains:

```python
# utilities/math_utils.py

def add(a, b):
    return a + b
```

You can import the function from `main.py`:

```python
# main.py

from utilities.math_utils import add

print(add(10, 20))
```

**Output:**

```text
30
```

As a project grows, packages help group related functionality together.

For example, a data analytics application might organize code into packages for data loading, cleaning, analysis, and visualization.

**Key idea:** A package helps organize related modules into a structured project.

---

## 3. Libraries

A **library** is a collection of reusable functionality designed to help developers perform particular tasks without implementing everything from scratch.

Libraries can contain modules, packages, classes, functions, and other components.

### Common Python Libraries

| Library | Primary Purpose |
|---|---|
| **Pandas** | Data manipulation and analysis |
| **NumPy** | Numerical computing and array operations |
| **Matplotlib** | Data visualization |
| **Requests** | Making HTTP requests |
| **scikit-learn** | Machine learning |
| **Seaborn** | Statistical data visualization |

### Example: Using Pandas

After installing Pandas, you can import and use it:

```python
import pandas as pd

data = {
    "Name": ["Priya", "Anu", "Rahul"],
    "Score": [85, 90, 78]
}

df = pd.DataFrame(data)

print(df)
```

Pandas provides the `DataFrame` structure, allowing you to work with tabular data without building that functionality yourself.

**Key idea:** Libraries provide reusable functionality for common programming tasks.

> **Note:** The terms *module*, *package*, and *library* are related, but they are not interchangeable. A module is a Python code unit, a package organizes modules, and a library is a broader term for reusable functionality.

---

## 4. Standard Library vs External Libraries

Python code can use functionality from the Standard Library or from separately installed packages.

### 4.1 Python Standard Library

The **Python Standard Library** is distributed with Python and provides functionality for common programming tasks.

Examples include:

```python
import math
import os
import datetime
import json
```

Some examples of what these modules provide:

| Module | Purpose |
|---|---|
| `math` | Mathematical functions |
| `os` | Operating-system interfaces |
| `datetime` | Working with dates and times |
| `json` | Encoding and decoding JSON |
| `pathlib` | Working with filesystem paths |
| `csv` | Reading and writing CSV files |

For example:

```python
import math

print(math.sqrt(25))
print(math.pi)
```

**Output:**

```text
5.0
3.141592653589793
```

These modules do not require a separate `pip install` command.

### 4.2 External Libraries

External libraries are distributed separately from Python itself and generally need to be installed before they can be imported.

For example, to install Pandas:

```bash
python -m pip install pandas
```

Then import it:

```python
import pandas as pd
```

External libraries extend Python's capabilities for tasks such as data analysis, web development, automation, and machine learning.

**Important distinction:**

- **Standard Library:** Distributed with Python.
- **External Library:** Installed separately when required.

---

## 5. pip — Python Package Installer

`pip` is Python's commonly used package installer. It installs packages from package indexes and can manage installed package versions.

### Common pip Commands

**Install a package:**

```bash
python -m pip install pandas
```

**Upgrade a package:**

```bash
python -m pip install --upgrade pandas
```

**Uninstall a package:**

```bash
python -m pip uninstall pandas
```

**Display installed packages:**

```bash
python -m pip list
```

**Display information about a package:**

```bash
python -m pip show pandas
```

**Save installed package information to a requirements file:**

```bash
python -m pip freeze > requirements.txt
```

**Install dependencies listed in a requirements file:**

```bash
python -m pip install -r requirements.txt
```

### Why Use `python -m pip`?

Using:

```bash
python -m pip install pandas
```

runs `pip` through the selected Python interpreter.

This can help avoid accidentally installing a package into a different Python installation than the one used to run your program.

### Mental Model

> `pip` installs and manages Python packages.

**Important:** A virtual environment helps isolate project dependencies. When working on a project, activate its virtual environment before installing packages, or explicitly use that environment's Python interpreter.

---

## 6. uv — Modern Python Project Manager

[`uv`](https://docs.astral.sh/uv/) is a fast Python package and project management tool written in Rust.

It provides workflows for tasks that might otherwise require several separate commands or tools, including:

- Creating Python projects.
- Managing Python versions.
- Creating virtual environments.
- Installing packages.
- Managing project dependencies.
- Running Python scripts and project commands.

### Traditional Workflow

A traditional workflow might begin by creating a virtual environment:

```bash
python -m venv .venv
```

Activate it before installing dependencies.

**Windows — Command Prompt:**

```bash
.venv\Scripts\activate
```

**Windows — PowerShell:**

```powershell
.venv\Scripts\Activate.ps1
```

**macOS / Linux:**

```bash
source .venv/bin/activate
```

Then install a package:

```bash
python -m pip install pandas
```

You would also create your project files and manage dependency information as needed.

### Workflow with uv

`uv` can simplify several of these steps.

#### Step 1: Create a Project

```bash
uv init my-project
```

This initializes a new project with a basic structure and project configuration.

#### Step 2: Navigate into the Project

```bash
cd my-project
```

#### Step 3: Add a Dependency

```bash
uv add pandas
```

This adds Pandas as a project dependency and updates the project's dependency configuration and lockfile.

#### Step 4: Run the Project

```bash
uv run main.py
```

`uv run` runs the script within the project's managed environment and ensures the project dependencies are available.

The exact files generated by `uv init` can vary depending on the project type and options used.

### Why Use uv?

Compared with a workflow involving separate tools and manual steps, `uv` can bring project setup, dependency management, environment management, and execution into one consistent workflow.

**Key idea:** `uv` is more than a package installer; it also provides project and environment management features.

---

## 7. pip vs uv

Both tools can install and manage Python dependencies, but their scope differs.

| Feature | `pip` | `uv` |
|---|---|---|
| Install Python packages | Yes | Yes |
| Upgrade packages | Yes | Yes |
| Uninstall packages | Yes | Yes |
| Create virtual environments | Not its primary role; use `venv` | Yes |
| Initialize Python projects | No | Yes |
| Manage project dependencies | Commonly used with additional project configuration | Yes |
| Support lockfiles for reproducible project environments | Usually used with additional tools or workflows | Yes |
| Manage Python versions | No | Yes |
| Typical workflow | Package installation and dependency management | Integrated project, environment, and dependency management |

### Does uv Replace pip?

Not entirely in the conceptual sense.

`pip` remains a widely used package installer, and understanding it is valuable when working with existing Python projects.

`uv` offers a broader, integrated workflow and can perform many package-management tasks that developers traditionally handle with `pip` and other tools.

### Which Should You Learn?

For a beginner learning Python:

1. Understand how `pip` installs packages.
2. Learn why virtual environments are useful.
3. Understand how project dependencies are recorded.
4. Learn `uv` to simplify project setup and dependency management.

**Recommendation:** Understand the underlying concepts first, then choose the workflow that best fits your project.

---

## 8. Project Structure with uv

A project initialized with `uv` may have a structure similar to this:

```text
my-project/
│
├── .git/
├── .gitignore
├── .python-version
├── .venv/
├── main.py
├── pyproject.toml
├── uv.lock
└── README.md
```

The exact structure depends on how the project is initialized and configured.

### Important Files and Directories

| File / Directory | Purpose |
|---|---|
| `.git/` | Stores Git repository metadata, if the directory is a Git repository |
| `.gitignore` | Specifies files Git should ignore |
| `.python-version` | Can specify the Python version used by the project |
| `.venv/` | Contains the project's virtual environment |
| `main.py` | An example Python entry-point script |
| `pyproject.toml` | Stores project metadata, configuration, and dependency declarations |
| `uv.lock` | Records resolved dependency versions for reproducible environments |
| `README.md` | Describes the project and explains how to use it |

### Understanding `pyproject.toml`

The `pyproject.toml` file is a standard configuration file used by Python projects.

It can contain:

- Project name and version.
- Project metadata.
- Declared dependencies.
- Build-system configuration.
- Tool-specific settings.

For example, a simplified project configuration might look like this:

```toml
[project]
name = "my-project"
version = "0.1.0"
requires-python = ">=3.12"
dependencies = [
    "pandas",
]
```

This example illustrates the general structure of project metadata and dependencies. Actual projects may contain additional configuration.

When using `uv add pandas`, `uv` updates the project configuration to record the dependency.

### Understanding `uv.lock`

The `uv.lock` file records the resolved dependency versions and related information for the project.

This helps different environments install compatible dependency versions and makes project setup more reproducible.

### Should You Commit `.venv/` to Git?

**Generally, no.**

The virtual environment contains locally installed packages and environment-specific files. It can usually be recreated from the project's dependency configuration and lockfile.

A typical `.gitignore` includes:

```gitignore
.venv/
__pycache__/
*.py[cod]
```

For projects using `uv`, committing `pyproject.toml` and `uv.lock` is generally useful so other developers can understand and recreate the project's dependency environment.

---

## 9. Module vs Package vs Library

These terms describe related but different concepts.

| Concept | Meaning | Example |
|---|---|---|
| **Module** | A Python file containing reusable code | `math_utils.py` |
| **Package** | A structure that organizes related modules | `utilities/` |
| **Library** | A collection of reusable functionality | Pandas, NumPy |
| **Standard Library** | Modules distributed with Python | `math`, `json` |
| **External Library** | Functionality distributed separately from Python | Pandas, Requests |
| **Package Manager** | A tool used to install and manage packages | `pip`, `uv` |
| **Virtual Environment** | An isolated Python environment for a project | `.venv/` |
| **Project Configuration** | A file containing project metadata and dependencies | `pyproject.toml` |

### Mental Model

```text
Module
  ↓
A Python file containing reusable code

Package
  ↓
Organizes related modules

Library
  ↓
Provides reusable functionality

Package Manager
  ↓
Installs and manages packages

Virtual Environment
  ↓
Isolates a project's Python dependencies
```

Remember that this is a simplified conceptual model. In practice, the terms *package* and *library* can overlap, and a library does not have to correspond to one particular package structure.

---

## 10. Best Practices

When working with modules, packages, and dependencies, follow these practices:

- **Keep modules focused:** Give each module a clear responsibility.
- **Use descriptive names:** Prefer names such as `data_cleaning.py` over vague names such as `helper.py` when the purpose is known.
- **Avoid unnecessary duplication:** Put reusable logic in functions or modules.
- **Use clear imports:** Import only what you need when that improves readability.
- **Use virtual environments:** Isolate dependencies between projects.
- **Track dependencies:** Record the packages a project needs.
- **Use lockfiles where appropriate:** Help make dependency installation reproducible.
- **Avoid committing `.venv/`:** Recreate the environment instead of storing it in Git.
- **Understand your tools:** Learn what `pip`, `venv`, `pyproject.toml`, and `uv` each do.
- **Document setup instructions:** Explain how another developer can install dependencies and run the project.

---

## ✅ Key Takeaways

- **Module:** A Python file containing reusable code.
- **Package:** A way to organize related Python modules.
- **Library:** Reusable functionality that helps solve particular problems.
- **Standard Library:** Python functionality distributed with the language.
- **External Library:** Functionality distributed separately and generally installed when needed.
- **`pip`:** A widely used Python package installer.
- **`uv`:** A tool for managing Python projects, packages, environments, and dependencies.
- **Virtual Environment:** An isolated environment for a project's Python dependencies.
- **`pyproject.toml`:** Stores project metadata, dependencies, and configuration.
- **`uv.lock`:** Records resolved dependency versions to support reproducible environments.

## 🚀 Why This Matters

Understanding modules, packages, libraries, and dependency management is an important step toward writing maintainable Python applications.

These concepts become especially useful when working with:

- Data analytics and data processing.
- Python automation.
- APIs and web applications.
- Data engineering pipelines.
- AI and machine learning projects.
- Larger, multi-module applications.
- Collaborative projects using Git and GitHub.

The goal is not just to write Python code, but to organize it so that it can be reused, maintained, tested, and shared with other developers.

---

## 📖 Learning Resources

### YouTube Tutorials

1. [Modules, Packages, and Libraries](https://youtu.be/6mw_lWlHCYk?si=MUPtmNk1u4wlLBh4)
2. [Libraries and Modules — Visual Explanation](https://youtu.be/7MOzepKojbw?si=YmlfqZz3HZyIFbsc)
3. [Virtual Environments — Chai aur Code](https://youtu.be/nwN27ORTUXg?si=BKEcvgaYMBtxiOaN)
4. [uv Package Management — Chai aur Code](https://youtu.be/8mk85fyzevc?si=vLZ9g2lLWyOipdzR)

### Official Documentation

- [Python Modules](https://docs.python.org/3/tutorial/modules.html)
- [Python Packages](https://docs.python.org/3/tutorial/modules.html#packages)
- [Python Standard Library](https://docs.python.org/3/library/)
- [Python Packaging User Guide](https://packaging.python.org/)
- [pip Documentation](https://pip.pypa.io/en/stable/)
- [uv Documentation](https://docs.astral.sh/uv/)
- [Python `venv` Documentation](https://docs.python.org/3/library/venv.html)

---

**Next Step:** Practice creating a small Python project with multiple modules, organize them into a package, and manage its dependencies using a virtual environment and `uv`.
