# Environment Variables & Secrets

A practical introduction to configuring Python applications through **environment variables**, managing local configuration with `.env` files, and protecting credentials such as API keys and database passwords.

The goal is to keep configuration separate from source code and reduce the risk of accidentally exposing sensitive values in a repository.

## Learning Objectives

- Explain what environment variables are and why applications use them.
- Read environment variables with `os.environ` and `os.getenv()`.
- Understand that environment-variable values are strings.
- Create and load a local `.env` file for development.
- Keep secrets out of Git and public repositories.
- Handle missing configuration explicitly.
- Distinguish local development configuration from production secret management.

## Topics Covered

- Configuration vs. application code
- Environment variables
- `os.environ` and `os.getenv()`
- String values and type conversion
- `.env` files and `python-dotenv`
- `.gitignore` and secret protection
- Required vs. optional configuration
- Safe defaults and validation
- Development vs. production configuration
- Common mistakes and a practical checklist

## 1. What Are Environment Variables?

Environment variables are named values provided to a running process by its environment. Applications use them for configuration that may change between machines or deployment environments.

Examples include:

- Application mode (`development`, `testing`, `production`)
- API keys and access tokens
- Database connection details
- Service URLs
- Feature flags and configuration values

Instead of hard-coding a value into a Python file, an application can read it from its environment.

```python
import os

app_mode = os.getenv("APP_MODE", "development")
print(app_mode)
```

The second argument to `os.getenv()` is a default value used when the variable is not set.

## 2. Reading Environment Variables

Python's standard-library `os` module provides two common approaches.

### `os.getenv()`

```python
import os

api_key = os.getenv("SERVICE_API_KEY")
app_mode = os.getenv("APP_MODE", "development")
```

- Returns the variable's value when it exists.
- Returns `None` if it is missing and no default is provided.
- Returns the supplied default when the variable is missing.

### `os.environ`

```python
import os

app_mode = os.environ.get("APP_MODE", "development")
```

You can also use direct indexing:

```python
api_key = os.environ["SERVICE_API_KEY"]
```

Direct indexing raises `KeyError` if the variable is missing. This can be useful for required configuration because it fails immediately, but a clearer error message is often better.

```python
import os

api_key = os.getenv("SERVICE_API_KEY")

if not api_key:
    raise RuntimeError("Required environment variable SERVICE_API_KEY is not set")
```

Do not print or log the actual secret value.

## 3. Environment Variable Values Are Strings

Environment variables are read as strings, even when the value looks like a number or boolean.

```python
import os

# Suppose MAX_RETRIES=3 is set in the environment.
max_retries_text = os.getenv("MAX_RETRIES", "3")
max_retries = int(max_retries_text)

print(max_retries)        # 3
print(type(max_retries))  # <class 'int'>
```

Convert values to the type the application needs, and handle invalid values:

```python
import os

raw_port = os.getenv("APP_PORT", "8000")

try:
    app_port = int(raw_port)
except ValueError as exc:
    raise RuntimeError("APP_PORT must be an integer") from exc
```

Do not assume a string such as `"false"` automatically behaves like the Boolean `False`—non-empty strings are truthy in Python. Parse boolean configuration deliberately.

## 4. Setting an Environment Variable

### In Python (for the current process)

```python
import os

os.environ["APP_MODE"] = "development"
print(os.getenv("APP_MODE"))
```

This changes the environment mapping for the current Python process and its child processes. It does not permanently configure the operating system or other unrelated terminal sessions.

### In a terminal

Commands depend on the shell and operating system. For example, in PowerShell:

```powershell
$env:APP_MODE = "development"
python main.py
```

This sets the variable for commands run in that PowerShell session. Avoid putting real secrets into commands that may be saved in shell history or exposed in logs.

## 5. Using a `.env` File for Local Development

A `.env` file is a convenient plain-text file containing environment-style key/value pairs. Python does not automatically load `.env` files by default; a package such as `python-dotenv` can load them into the process environment.

Install it:

```bash
python -m pip install python-dotenv
```

Example local `.env` file:

```dotenv
APP_MODE=development
APP_PORT=8000
SERVICE_API_KEY=replace_with_your_local_key
```

Load it near application startup:

```python
from dotenv import load_dotenv
import os

load_dotenv()

app_mode = os.getenv("APP_MODE", "development")
api_key = os.getenv("SERVICE_API_KEY")

if not api_key:
    raise RuntimeError("SERVICE_API_KEY is required")
```

`load_dotenv()` normally looks for a `.env` file and loads its values into the process environment. Existing environment variables are not overwritten by default. You can use an explicit path if your project layout requires it.

**Important:** A `.env` file is not encrypted or inherently secure. Treat it as a file containing secrets.

## 6. Keep Secrets Out of Git

Never commit real API keys, access tokens, private keys, or passwords to a repository—public or private.

Add local secret files and other sensitive artifacts to `.gitignore`:

```gitignore
# Local environment configuration and secrets
.env
.env.*
!.env.example

# Python virtual environment
.venv/
venv/

# Python cache
__pycache__/
*.py[cod]
```

This pattern ignores `.env` and similarly named environment files while allowing a deliberately safe `.env.example` template. Review the pattern if your project uses other environment-file naming conventions.

### Use `.env.example` as a template

Commit a template containing variable names and non-sensitive placeholders, not real credentials:

```dotenv
APP_MODE=development
APP_PORT=8000
SERVICE_API_KEY=your_api_key_here
```

Document how to copy the template to `.env` and fill in local values. Do not commit a real key in `.env.example`.

### If a secret is committed accidentally

1. Revoke or rotate the exposed credential immediately.
2. Remove the secret from the current source files and prevent further commits.
3. Review repository history and access logs as appropriate; deleting the latest line does not erase previous Git history.
4. Follow the hosting provider's guidance for cleaning sensitive history if necessary.

Treat a committed credential as exposed, even if the repository was private or the commit was quickly deleted.

## 7. A Small Configuration Pattern

Keep configuration checks close to application startup so the program fails early with a clear message.

```python
import os
from dotenv import load_dotenv

load_dotenv()

def get_required_env(name: str) -> str:
    value = os.getenv(name)
    if not value:
        raise RuntimeError(f"Required environment variable {name} is not set")
    return value

APP_MODE = os.getenv("APP_MODE", "development")
SERVICE_API_KEY = get_required_env("SERVICE_API_KEY")
```

This is a simple pattern for learning projects. Larger applications may benefit from a dedicated configuration module or a settings library that validates types and required fields.

Avoid including secret values in exception messages, logs, screenshots, issue reports, or example output.

## 8. Development vs. Production

| Concern | Local development | Production |
|---|---|---|
| Configuration source | Local environment or ignored `.env` file | Deployment environment or managed configuration |
| Secret storage | Local file with restricted access, or OS credential store | Secret manager/platform-provided secret storage |
| Repository | Commit `.env.example`, not `.env` | Never commit production secrets |
| Defaults | Safe, non-sensitive development defaults may be appropriate | Avoid unsafe or misleading defaults for required settings |
| Rotation | Replace local credentials when needed | Use a defined rotation and access-control process |

Environment variables help separate configuration from code, but they do not by themselves guarantee that a secret is secure. Access permissions, deployment practices, logging, and secret rotation also matter.

## 9. Common Mistakes

- Hard-coding credentials in source code.
- Committing `.env` files or real secrets to Git.
- Assuming Python automatically reads `.env` files.
- Forgetting to install or call `python-dotenv` when relying on a `.env` file.
- Assuming environment-variable values are integers or booleans rather than strings.
- Using `os.getenv()` without checking whether required configuration is missing.
- Printing secrets while debugging.
- Believing `.gitignore` removes a file that is already tracked; it does not untrack files or erase history.
- Believing a private repository makes committed secrets safe.
- Using production credentials in learning projects or public examples.

## 10. Quick Reference

| Task | Example |
|---|---|
| Read optional variable | `os.getenv("APP_MODE", "development")` |
| Read variable with mapping API | `os.environ.get("APP_MODE")` |
| Require a variable via indexing | `os.environ["SERVICE_API_KEY"]` |
| Convert a string to integer | `int(os.getenv("APP_PORT", "8000"))` |
| Install `.env` support | `python -m pip install python-dotenv` |
| Load local `.env` file | `from dotenv import load_dotenv; load_dotenv()` |
| Ignore local secrets | Add `.env` to `.gitignore` |
| Share required variable names | Commit a safe `.env.example` |
| Handle a leaked credential | Revoke/rotate it immediately |

## Practice Checklist

- [ ] Read an optional variable using `os.getenv()` and a default.
- [ ] Read a required variable and raise a clear error when it is missing.
- [ ] Convert an environment variable to an integer and handle invalid input.
- [ ] Create a local `.env` file and load it using `python-dotenv`.
- [ ] Add `.env` to `.gitignore`.
- [ ] Create a safe `.env.example` with placeholders only.
- [ ] Verify that no real credentials appear in code, output, screenshots, or Git history.
- [ ] Explain why `.gitignore` does not remove a file that Git already tracks.

---

**Key takeaway:** Environment variables let an application receive configuration from its runtime environment instead of hard-coding it into source code. Keep secrets out of Git, validate required configuration, and remember that a `.env` file is a convenience—not a security boundary.

