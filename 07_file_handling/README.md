# 🐍 Python File Handling

A practical guide to working with files in Python, covering file input/output (I/O), reading and writing data, file modes, context managers, file pointers, binary files, and serialization using JSON and Pickle.

File handling is an essential Python skill for working with datasets, configuration files, logs, API responses, and persistent application data.

---

## 📚 Table of Contents

- [Learning Objectives](#-learning-objectives)
- [1. Introduction to File Handling](#1-introduction-to-file-handling)
- [2. Text Files vs Binary Files](#2-text-files-vs-binary-files)
- [3. Opening a File](#3-opening-a-file)
- [4. File Modes](#4-file-modes)
- [5. Writing to Files](#5-writing-to-files)
- [6. Reading from Files](#6-reading-from-files)
- [7. Context Managers — `with`](#7-context-managers--with)
- [8. File Pointers — `seek()` and `tell()`](#8-file-pointers--seek-and-tell)
- [9. Reading Large Files in Chunks](#9-reading-large-files-in-chunks)
- [10. Working with Binary Files](#10-working-with-binary-files)
- [11. Working with Different Python Data Types](#11-working-with-different-python-data-types)
- [12. Serialization and Deserialization](#12-serialization-and-deserialization)
- [13. Working with JSON](#13-working-with-json)
- [14. Handling Nested JSON Data](#14-handling-nested-json-data)
- [15. JSON and Tuples](#15-json-and-tuples)
- [16. Serializing Custom Python Objects to JSON](#16-serializing-custom-python-objects-to-json)
- [17. Pickle — Python Object Serialization](#17-pickle--python-object-serialization)
- [18. JSON vs Pickle](#18-json-vs-pickle)
- [19. Working with File Paths](#19-working-with-file-paths)
- [20. Common File Handling Methods](#20-common-file-handling-methods)
- [21. Best Practices](#21-best-practices)
- [22. Common Mistakes](#22-common-mistakes)
- [Key Takeaways](#-key-takeaways)
- [Learning Resources](#-learning-resources)

---

## 🎯 Learning Objectives

By the end of this section, you should understand:

- What file handling is and why it is important.
- The difference between text files and binary files.
- How to open, read, write, append, and close files.
- How file modes affect file operations.
- Why context managers are preferred for resource management.
- How file pointers work with `seek()` and `tell()`.
- How to process large files without loading everything into memory.
- How to read and write binary data.
- What serialization and deserialization mean.
- How to store and retrieve structured data using JSON.
- How Pickle serializes Python objects.
- The differences between JSON and Pickle.
- How to work with Windows file paths safely.

---

## 1. Introduction to File Handling

**File handling** is the process of creating, reading, writing, and managing data stored in files.

Variables store data in memory while a program runs. Files allow data to persist beyond the lifetime of a program.

For example, a Python program might need to:

- Read a CSV file containing sales data.
- Save the results of a data analysis.
- Read configuration settings from a JSON file.
- Write application logs to a file.
- Store structured Python data for later use.
- Read or copy image files.

### Basic File I/O Workflow

```text
Open a file
     ↓
Read or write data
     ↓
Close the file
```

Python provides built-in functions and modules for these operations.

### A Simple Example

```python
with open("sample.txt", "w", encoding="utf-8") as file:
    file.write("Hello, Python!")
```

This code:

1. Opens `sample.txt` in write mode.
2. Writes the specified text.
3. Closes the file automatically when execution leaves the `with` block.

If the file does not exist, Python creates it. If it already exists, write mode replaces its previous contents.

**Key idea:** File handling allows Python programs to work with persistent data stored outside the program's memory.

---

## 2. Text Files vs Binary Files

Python works with files in two broad modes: text mode and binary mode.

### 2.1 Text Files

Text files store data that can be interpreted as characters using a character encoding.

Examples include:

- `.txt`
- `.csv`
- `.json`
- `.py`
- `.log`

Example:

```python
with open("message.txt", "w", encoding="utf-8") as file:
    file.write("Hello, Python!")
```

When a text file is opened in text mode, Python converts between text strings and the underlying bytes using a character encoding.

### 2.2 Binary Files

Binary files are read and written as sequences of bytes.

Examples include:

- Images
- Audio files
- Video files
- Executable files
- Other binary data formats

Example:

```python
with open("image.png", "rb") as file:
    data = file.read()
```

The returned data is a `bytes` object rather than a string.

### Comparison

| Feature | Text Mode | Binary Mode |
|---|---|---|
| Data handled | Strings (`str`) | Bytes (`bytes`) |
| Common modes | `"r"`, `"w"`, `"a"` | `"rb"`, `"wb"`, `"ab"` |
| Character encoding | Applies | Not applied by the text I/O layer |
| Typical use cases | Text, CSV, JSON, logs | Images, audio, video, binary data |

**Important:** A file extension does not determine how Python must open a file. Choose the mode based on the data you intend to process.

---

## 3. Opening a File

Python's built-in `open()` function opens a file and returns a file object.

### Syntax

```python
open(file, mode="r", encoding=None)
```

The available arguments include:

- `file` — The file path or name.
- `mode` — Specifies how the file should be opened.
- `encoding` — Specifies the text encoding when using text mode.

### Example

```python
file = open("sample.txt", "w", encoding="utf-8")

file.write("Hello, world!")

file.close()
```

Here:

- `"sample.txt"` is the file path.
- `"w"` specifies write mode.
- `file` is the file object.
- `write()` writes text to the file.
- `close()` closes the file.

### Why Is Closing a File Important?

Closing a file releases its associated resources and ensures that buffered output is properly finalized.

However, manually closing files can be error-prone if an exception occurs before `close()` is reached.

That is why Python generally recommends using a context manager.

---

## 4. File Modes

The file mode determines how Python opens and interacts with a file.

### Common File Modes

| Mode | Purpose | Behavior |
|---|---|---|
| `"r"` | Read text | Requires the file to exist |
| `"w"` | Write text | Creates the file or truncates an existing file |
| `"a"` | Append text | Creates the file if necessary and writes at the end |
| `"x"` | Exclusive creation | Creates a new file and fails if it already exists |
| `"rb"` | Read binary | Reads bytes |
| `"wb"` | Write binary | Creates or truncates a file and writes bytes |
| `"ab"` | Append binary | Appends bytes to a file |
| `"r+"` | Read and write | Requires the file to exist; does not initially truncate it |
| `"w+"` | Read and write | Creates or truncates the file |
| `"a+"` | Read and append | Creates the file if necessary and supports reading and appending |

### Write Mode — `"w"`

```python
with open("sample.txt", "w", encoding="utf-8") as file:
    file.write("First version")
```

If `sample.txt` already exists, its previous contents are truncated.

Use write mode when replacing the contents is intentional.

### Append Mode — `"a"`

```python
with open("sample.txt", "a", encoding="utf-8") as file:
    file.write("\nNew entry")
```

Append mode adds the new content at the end of the file instead of replacing its existing contents.

This is useful for logs and files that accumulate records over time.

### Read Mode — `"r"`

```python
with open("sample.txt", "r", encoding="utf-8") as file:
    content = file.read()

print(content)
```

If the file does not exist, Python raises `FileNotFoundError`.

### Important Distinction

```text
"r"  → Read existing content
"w"  → Replace existing content
"a"  → Add content at the end
"x"  → Create a new file only
"rb" → Read bytes
"wb" → Write bytes
```

**Best practice:** Be especially careful with `"w"` because it can permanently replace existing file contents.

---

## 5. Writing to Files

Python provides `write()` and `writelines()` for writing data to files.

### 5.1 The `write()` Method

The `write()` method writes a string to a text file.

```python
with open("sample.txt", "w", encoding="utf-8") as file:
    file.write("Hello, world!")
```

The file contains:

```text
Hello, world!
```

### Writing Multiple Strings

```python
with open("sample.txt", "w", encoding="utf-8") as file:
    file.write("Hello, world!\n")
    file.write("Welcome to Python.\n")
    file.write("Learning file handling.")
```

**File contents:**

```text
Hello, world!
Welcome to Python.
Learning file handling.
```

The `\n` escape sequence represents a newline.

Without a newline or another separator, consecutive `write()` calls do not automatically insert spaces or line breaks.

### 5.2 The `writelines()` Method

`writelines()` writes multiple strings from an iterable to a file.

```python
lines = [
    "Python\n",
    "SQL\n",
    "Data Analytics\n",
    "Data Engineering\n",
]

with open("skills.txt", "w", encoding="utf-8") as file:
    file.writelines(lines)
```

**File contents:**

```text
Python
SQL
Data Analytics
Data Engineering
```

### Important: `writelines()` Does Not Add Newlines

This example:

```python
lines = ["Python", "SQL", "Excel"]

with open("skills.txt", "w", encoding="utf-8") as file:
    file.writelines(lines)
```

Produces:

```text
PythonSQLExcel
```

If separate lines are required, include newline characters in the strings or add them explicitly.

For example:

```python
lines = ["Python", "SQL", "Excel"]

with open("skills.txt", "w", encoding="utf-8") as file:
    file.writelines(line + "\n" for line in lines)
```

**Key idea:** `write()` writes a string; `writelines()` writes multiple strings without automatically adding separators.

---

## 6. Reading from Files

Python provides several methods for reading file contents.

The appropriate method depends on the size of the file and how the data should be processed.

### 6.1 The `read()` Method

`read()` reads the remaining contents of a file and returns them as a string in text mode.

```python
with open("sample.txt", "r", encoding="utf-8") as file:
    content = file.read()

print(content)
```

You can also specify the number of characters to read.

```python
with open("sample.txt", "r", encoding="utf-8") as file:
    content = file.read(10)

print(content)
```

This reads up to 10 characters from the current file position.

**Important:** The number refers to characters in text mode, not necessarily the number of bytes in the underlying file.

### 6.2 The `readline()` Method

`readline()` reads one line from the current file position.

```python
with open("sample.txt", "r", encoding="utf-8") as file:
    first_line = file.readline()
    second_line = file.readline()

print(first_line, end="")
print(second_line, end="")
```

The method normally includes the newline character when one is present.

Using `end=""` avoids adding another newline when printing the returned strings.

### 6.3 Iterating Through a File

For many text-processing tasks, iterating over the file object is simpler and more memory-efficient than reading the entire file at once.

```python
with open("sample.txt", "r", encoding="utf-8") as file:
    for line in file:
        print(line, end="")
```

Python reads and processes the file line by line.

### 6.4 Reading Until the End of a File

You can also use `readline()` in a loop.

```python
with open("sample.txt", "r", encoding="utf-8") as file:
    while True:
        line = file.readline()

        if line == "":
            break

        print(line, end="")
```

When no more text is available, `readline()` returns an empty string.

### Reading Methods Compared

| Method | Behavior | Suitable For |
|---|---|---|
| `read()` | Reads the remaining contents | Small files or full-content processing |
| `read(size)` | Reads up to a specified number of characters in text mode | Partial reads |
| `readline()` | Reads one line | Processing individual lines |
| `for line in file` | Iterates over lines | Efficient line-by-line processing |

**Best practice:** Avoid using `read()` on very large files unless you know the contents will fit comfortably in memory.

---

## 7. Context Managers — `with`

A **context manager** manages the setup and cleanup of a resource.

For file handling, Python's `with` statement ensures that the file is closed when execution leaves the block.

### Without a Context Manager

```python
file = open("sample.txt", "w", encoding="utf-8")

try:
    file.write("Hello, Python!")
finally:
    file.close()
```

The `finally` block ensures that the file is closed even if an exception occurs while writing.

### With a Context Manager

```python
with open("sample.txt", "w", encoding="utf-8") as file:
    file.write("Hello, Python!")
```

This is shorter and easier to maintain.

When execution leaves the `with` block, the file is closed automatically, including when an exception occurs inside the block.

### Mental Model

```text
Without a context manager:

Open file
    ↓
Perform operations
    ↓
Remember to close the file


With a context manager:

Enter with block
    ↓
Perform operations
    ↓
Automatic cleanup on exit
```

### Why Prefer `with`?

- Reduces the risk of leaving files open.
- Makes resource management easier to understand.
- Handles cleanup when exceptions occur.
- Keeps code concise and readable.

**Best practice:** Use `with open(...)` for ordinary file operations unless there is a specific reason to manage the file's lifecycle manually.

---

## 8. File Pointers — `seek()` and `tell()`

When Python reads or writes a file, the file object maintains a current position.

This position determines where the next read or write operation occurs, subject to the file mode and its buffering behavior.

Two useful methods are `tell()` and `seek()`.

### 8.1 The `tell()` Method

`tell()` returns the current file position.

```python
with open("sample.txt", "rb") as file:
    print(file.tell())

    file.read(5)

    print(file.tell())
```

For a binary file, the position is measured in bytes.

If the file contains at least five bytes, the output is:

```text
0
5
```

### 8.2 The `seek()` Method

`seek()` changes the file position.

```python
with open("sample.txt", "rb") as file:
    file.seek(5)

    data = file.read(10)

    print(data)
```

This moves to byte position `5` and reads up to the next 10 bytes.

The example uses binary mode because file positions and offsets are easier to reason about as byte positions.

### Checking File Positions

```python
with open("sample.txt", "rb") as file:
    print("Initial position:", file.tell())

    file.read(10)
    print("After reading:", file.tell())

    file.seek(0)
    print("After seeking to the beginning:", file.tell())
```

**Output, assuming the file contains at least 10 bytes:**

```text
Initial position: 0
After reading: 10
After seeking to the beginning: 0
```

### Common Uses

- Re-reading file contents.
- Moving to a known position in a binary file.
- Processing portions of a file.
- Understanding how sequential reads advance through data.

**Important:** In text mode, file positions are not always simple byte offsets. For reliable byte-oriented positioning, use binary mode. Text-mode `seek()` has additional restrictions, so arbitrary offsets should not be assumed to work like binary offsets.

---

## 9. Reading Large Files in Chunks

Reading an entire file into memory can be inefficient when the file is very large.

Instead, you can read and process smaller chunks.

### Example

```python
chunk_size = 1024

with open("large_file.txt", "r", encoding="utf-8") as file:
    while True:
        chunk = file.read(chunk_size)

        if chunk == "":
            break

        # Process the chunk
        print(chunk, end="")
```

This reads up to 1,024 characters per iteration.

The loop stops when `read()` returns an empty string, indicating the end of the file.

### Processing Workflow

```text
Large File
    ↓
Read a chunk
    ↓
Process the chunk
    ↓
Read the next chunk
    ↓
Repeat until EOF
```

### Why Is Chunking Useful?

- Limits how much data is read into memory at one time.
- Supports incremental processing.
- Can be useful for large logs, text files, and data streams.

### Alternative: Process Line by Line

For line-oriented files, this approach is often simpler:

```python
with open("large_file.txt", "r", encoding="utf-8") as file:
    for line in file:
        # Process one line at a time
        process_line = line.strip()
```

Here, `process_line` stores the stripped line; a real application would replace this assignment with its actual processing logic.

**Key idea:** Choose the reading strategy based on the file format and the processing requirements.

---

## 10. Working with Binary Files

Binary mode is designed for data that should be read or written as bytes rather than decoded into text.

Common examples include images, audio files, and other binary formats.

### 10.1 Reading a Binary File

```python
with open("image.png", "rb") as file:
    data = file.read()

print(type(data))
```

**Output:**

```text
<class 'bytes'>
```

The `b` in `"rb"` indicates binary mode.

### 10.2 Copying a Binary File

```python
with open("image.png", "rb") as source:
    with open("image_copy.png", "wb") as destination:
        destination.write(source.read())
```

This reads the source file as bytes and writes those bytes to a new file.

For a large file, it is better to copy in chunks rather than load the entire file into memory.

### Copying a Large Binary File in Chunks

```python
chunk_size = 1024 * 1024

with open("image.png", "rb") as source:
    with open("image_copy.png", "wb") as destination:
        while True:
            chunk = source.read(chunk_size)

            if not chunk:
                break

            destination.write(chunk)
```

This reads up to 1 MiB of data per iteration.

### Important Binary Modes

| Mode | Purpose |
|---|---|
| `"rb"` | Read bytes |
| `"wb"` | Write bytes, creating or truncating the file |
| `"ab"` | Append bytes |

**Key idea:** Binary mode preserves data as bytes without applying text encoding or newline translation.

---

## 11. Working with Different Python Data Types

Text files store characters. When you write a number to a text file, you generally need to convert it to a string first.

### Example: Writing a Number

```python
with open("number.txt", "w", encoding="utf-8") as file:
    file.write(str(5))
```

The file contains the text:

```text
5
```

### Reading the Value Back

```python
with open("number.txt", "r", encoding="utf-8") as file:
    value = int(file.read())

print(value + 5)
```

**Output:**

```text
10
```

The text `"5"` is converted into the integer `5` before arithmetic is performed.

### Why Does This Matter?

Different Python types need different representations when stored.

For simple text, strings may be sufficient. For structured data such as dictionaries and nested lists, formats such as JSON may be more suitable.

For Python-specific object serialization, Pickle is another option, with important security limitations discussed later.

---

## 12. Serialization and Deserialization

**Serialization** converts an in-memory object into a representation that can be stored or transmitted.

**Deserialization** reconstructs data from that representation.

### Serialization

```text
Python object
     ↓
Serialization
     ↓
Stored or transmitted representation
```

### Deserialization

```text
Stored or transmitted representation
     ↓
Deserialization
     ↓
Reconstructed data
```

### Why Is Serialization Useful?

Serialization allows programs to:

- Save structured data to files.
- Exchange data between applications.
- Store configuration settings.
- Transfer information between services.
- Preserve selected data between program executions.

Python supports different serialization formats. Two important examples are JSON and Pickle.

### JSON

- Text-based format.
- Widely supported across programming languages.
- Suitable for many data exchange and configuration tasks.
- Supports a limited set of data types.

### Pickle

- Python-specific object serialization mechanism.
- Can serialize many Python objects.
- Produces a binary representation by default.
- Must not be used to load untrusted data.

**Key idea:** Choose a serialization format based on interoperability, supported data types, and security requirements.

---

## 13. Working with JSON

**JSON** stands for JavaScript Object Notation.

It is a text-based format commonly used to store and exchange structured data.

Python provides the built-in `json` module, so no separate package installation is needed.

```python
import json
```

Two important functions are:

- `json.dump()` — Serializes a Python object and writes the JSON representation to a file-like object.
- `json.load()` — Reads JSON from a file-like object and deserializes it into Python data.

### 13.1 Serializing a List

```python
import json

numbers = [1, 2, 3, 4, 5]

with open("numbers.json", "w", encoding="utf-8") as file:
    json.dump(numbers, file)
```

The file contains:

```json
[1, 2, 3, 4, 5]
```

### 13.2 Serializing a Dictionary

```python
import json

employee = {
    "name": "Priya",
    "age": 24,
    "department": "Analytics"
}

with open("employee.json", "w", encoding="utf-8") as file:
    json.dump(employee, file, indent=4)
```

The resulting JSON is formatted for readability:

```json
{
    "name": "Priya",
    "age": 24,
    "department": "Analytics"
}
```

The `indent=4` argument adds indentation to the JSON output.

### 13.3 Deserializing JSON

```python
import json

with open("employee.json", "r", encoding="utf-8") as file:
    employee = json.load(file)

print(employee)
print(type(employee))
```

**Output:**

```text
{'name': 'Priya', 'age': 24, 'department': 'Analytics'}
<class 'dict'>
```

The JSON object is converted into a Python dictionary.

### 13.4 `dump()` vs `load()`

| Function | Purpose |
|---|---|
| `json.dump(obj, file)` | Writes a Python object as JSON |
| `json.load(file)` | Reads JSON from a file and returns the corresponding Python object |

Python also provides `json.dumps()` and `json.loads()` for working with JSON strings rather than file-like objects.

**Key idea:** JSON is particularly useful when data needs to be readable and exchanged between different systems.

---

## 14. Handling Nested JSON Data

JSON supports nested objects and arrays, making it useful for representing structured data.

### Example

```python
import json

student = {
    "name": "Priya",
    "marks": [85, 90, 78],
    "address": {
        "city": "Kurnool",
        "state": "Andhra Pradesh"
    }
}

with open("student.json", "w", encoding="utf-8") as file:
    json.dump(student, file, indent=4)
```

The file represents a student record containing a list and a nested object.

### Reading Nested Data

```python
import json

with open("student.json", "r", encoding="utf-8") as file:
    student = json.load(file)

print(student["name"])
print(student["marks"])
print(student["address"]["city"])
```

**Output:**

```text
Priya
[85, 90, 78]
Kurnool
```

### Why Is This Useful?

Nested JSON is common in:

- API responses.
- Application configuration.
- Structured records.
- Data exchange between services.
- Metadata and event payloads.

**Key idea:** JSON can represent nested dictionaries and lists, but its data types are more limited than Python's complete type system.

---

## 15. JSON and Tuples

Python tuples can be serialized into JSON, but JSON does not have a dedicated tuple data type.

### Example

```python
import json

numbers = (1, 2, 3, 4, 5)

with open("numbers.json", "w", encoding="utf-8") as file:
    json.dump(numbers, file)
```

The JSON representation is:

```json
[1, 2, 3, 4, 5]
```

### What Happens During Deserialization?

```python
import json

with open("numbers.json", "r", encoding="utf-8") as file:
    numbers = json.load(file)

print(numbers)
print(type(numbers))
```

**Output:**

```text
[1, 2, 3, 4, 5]
<class 'list'>
```

The original tuple is reconstructed as a list.

### Why?

JSON arrays are mapped to Python lists by the standard `json` module.

This means JSON serialization does not automatically preserve every detail of the original Python object's type.

**Key idea:** JSON preserves supported data values, but it does not guarantee that every original Python type will be reconstructed exactly.

---

## 16. Serializing Custom Python Objects to JSON

JSON supports standard data types such as objects, arrays, strings, numbers, booleans, and `null`.

It does not automatically serialize arbitrary custom Python objects.

To serialize such an object, convert it into a JSON-compatible representation, such as a dictionary.

### Example: A Custom Class

```python
import json


class Person:
    def __init__(self, first_name, last_name, age, gender):
        self.first_name = first_name
        self.last_name = last_name
        self.age = age
        self.gender = gender
```

Create an object:

```python
person = Person("Priya", "Varra", 24, "female")
```

Passing this object directly to `json.dump()` normally raises a `TypeError`, because the default encoder does not know how to serialize it.

### Approach: Provide a Conversion Function

```python
def person_to_dict(obj):
    if isinstance(obj, Person):
        return {
            "name": f"{obj.first_name} {obj.last_name}",
            "age": obj.age,
            "gender": obj.gender
        }

    raise TypeError(
        f"Object of type {type(obj).__name__} is not JSON serializable"
    )
```

Use the function with the `default` argument:

```python
with open("person.json", "w", encoding="utf-8") as file:
    json.dump(person, file, default=person_to_dict, indent=4)
```

The resulting JSON is:

```json
{
    "name": "Priya Varra",
    "age": 24,
    "gender": "female"
}
```

### Important Limitation

This example converts the object into a dictionary, but it does not automatically reconstruct the original `Person` instance when the JSON is read back.

Reconstructing a custom object requires an additional step, such as passing the loaded dictionary to the class constructor.

For example:

```python
with open("person.json", "r", encoding="utf-8") as file:
    data = json.load(file)

person = Person(
    first_name=data["name"].split(" ")[0],
    last_name=data["name"].split(" ")[1],
    age=data["age"],
    gender=data["gender"]
)
```

This reconstruction works for the simple example, but splitting a full name on spaces is not a reliable general-purpose way to store first and last names. A better design would preserve the fields separately in JSON.

**Key idea:** JSON serialization of custom objects requires an explicit conversion strategy.

---

## 17. Pickle — Python Object Serialization

`pickle` is a Python module that serializes and deserializes Python objects.

It can preserve many Python-specific types that JSON does not directly represent, including instances of custom classes under appropriate conditions.

### Importing Pickle

```python
import pickle
```

### 17.1 Creating a Custom Class

```python
class Person:
    def __init__(self, name, age):
        self.name = name
        self.age = age

    def display_info(self):
        print(f"My name is {self.name}, and I am {self.age} years old.")
```

Create an instance:

```python
person = Person("Priya", 24)
```

### 17.2 Serializing with `pickle.dump()`

```python
import pickle

with open("person.pkl", "wb") as file:
    pickle.dump(person, file)
```

The object is serialized and written to a binary file.

The `"wb"` mode is used because Pickle produces a byte stream.

### 17.3 Deserializing with `pickle.load()`

```python
import pickle

with open("person.pkl", "rb") as file:
    person = pickle.load(file)

person.display_info()
```

**Output:**

```text
My name is Priya, and I am 24 years old.
```

### Important Requirements

For many custom class instances, the class must be importable from the same module and available under the expected name when the object is unpickled.

For example, a class defined only inside a temporary interactive session may not be available when the serialized object is loaded by another program.

Pickle is most suitable for controlled Python environments where the producer and consumer can maintain compatible code and dependencies.

### Security Warning: Never Unpickle Untrusted Data

**Loading untrusted Pickle data can execute arbitrary code.**

Do not use `pickle.load()` on files, downloads, or data received from other people or systems unless you fully trust the source.

Pickle is not a safe general-purpose format for exchanging data with untrusted systems.

**Key idea:** Pickle supports Python-specific object serialization, but it requires careful attention to compatibility and security.

---

## 18. JSON vs Pickle

JSON and Pickle both support serialization, but they serve different purposes.

| Feature | JSON | Pickle |
|---|---|---|
| Representation | Text | Binary byte stream |
| Human-readable | Usually | Generally no |
| Cross-language support | Broad | Primarily Python-specific |
| Standard Python types | Supported types | Many Python object types |
| Custom objects | Require conversion logic | Supported for many objects under appropriate conditions |
| Typical use cases | APIs, configuration, data exchange | Trusted Python-specific serialization |
| Security consideration | Validate data and handle parsing errors | Never load untrusted data |

### Mental Model

```text
JSON

Python data
     ↓
JSON text
     ↓
Store or transmit
     ↓
Load JSON
     ↓
Python data
```

```text
Pickle

Python object
     ↓
Pickle byte stream
     ↓
Store in a trusted environment
     ↓
Load Pickle
     ↓
Reconstructed Python object
```

### Which Should You Use?

Use **JSON** when:

- Data must be exchanged between different programming languages.
- You need a human-readable file format.
- You are storing configuration or structured data.
- You are communicating through APIs that use JSON.

Consider **Pickle** when:

- You need to serialize Python-specific objects.
- Both the producer and consumer operate in a trusted Python environment.
- You can manage compatibility between the serialized data and the Python code that loads it.

**Important:** JSON parsing is not a substitute for input validation. Even with JSON, validate untrusted data before using it in application logic.

---

## 19. Working with File Paths

A file path identifies the location of a file or directory.

Paths can be absolute or relative.

### 19.1 Absolute Paths

An absolute path specifies a location from the root of a filesystem or drive.

Example on Windows:

```python
path = "C:\\Users\\Priya\\Desktop\\notes.txt"
```

### 19.2 Raw Strings

Backslashes have special meaning in ordinary Python strings because they introduce escape sequences.

For example:

```python
message = "Hello\nWorld"
print(message)
```

The `\n` sequence creates a newline.

For Windows paths, a raw string can make the path easier to read:

```python
path = r"C:\Users\Priya\Desktop\notes.txt"
```

The `r` prefix prevents most backslash escape sequences from being interpreted in the string.

**Important:** A raw string literal cannot end with an odd number of backslashes because the final backslash would escape the closing quote.

### 19.3 Relative Paths

A relative path is interpreted relative to the program's current working directory.

```python
with open("data.txt", "r", encoding="utf-8") as file:
    content = file.read()
```

This attempts to open `data.txt` relative to the current working directory.

The current working directory is not necessarily the same directory as the Python script.

### 19.4 Using `pathlib`

For more readable and cross-platform path handling, Python provides `pathlib`.

```python
from pathlib import Path

path = Path("data") / "sample.txt"

print(path)
```

The `/` operator joins path components appropriately for the current operating system.

You can also use `Path` when opening files:

```python
from pathlib import Path

path = Path("data") / "sample.txt"

with path.open("r", encoding="utf-8") as file:
    content = file.read()
```

**Best practice:** Prefer `pathlib` when building applications that work with file paths across different operating systems.

---

## 20. Common File Handling Methods

| Method | Purpose |
|---|---|
| `open()` | Opens a file and returns a file object |
| `close()` | Closes a file |
| `read()` | Reads the remaining contents |
| `readline()` | Reads one line |
| `readlines()` | Reads the remaining lines into a list |
| `write()` | Writes a string |
| `writelines()` | Writes an iterable of strings without adding separators |
| `seek()` | Changes the file position |
| `tell()` | Returns the current file position |
| `json.dump()` | Serializes a Python object to a JSON file-like object |
| `json.load()` | Deserializes JSON from a file-like object |
| `json.dumps()` | Serializes a Python object into a JSON string |
| `json.loads()` | Deserializes a JSON string |
| `pickle.dump()` | Serializes a Python object to a binary file-like object |
| `pickle.load()` | Deserializes a Pickle stream |

---

## 21. Best Practices

Follow these practices when working with files in Python.

### Use Context Managers

Prefer:

```python
with open("data.txt", "r", encoding="utf-8") as file:
    content = file.read()
```

This ensures the file is closed when execution leaves the block.

### Specify an Encoding for Text Files

For predictable text-file behavior, specify the encoding explicitly:

```python
with open("data.txt", "r", encoding="utf-8") as file:
    content = file.read()
```

### Be Careful with Write Mode

Opening a file using `"w"` truncates its existing contents.

Use `"a"` when you intend to append rather than replace.

### Avoid Loading Huge Files Entirely into Memory

Use line-by-line iteration or chunked reads when processing large files.

### Use the Correct Mode

- Text data: `"r"`, `"w"`, `"a"`.
- Binary data: `"rb"`, `"wb"`, `"ab"`.

### Handle Expected File Errors

A file may be missing or inaccessible.

```python
try:
    with open("data.txt", "r", encoding="utf-8") as file:
        content = file.read()

except FileNotFoundError:
    print("The requested file was not found.")

except PermissionError:
    print("Permission denied when accessing the file.")
```

Handle only the errors for which your program has a meaningful response.

### Validate Data After Reading It

A file may contain missing, malformed, or unexpected data. Validate it before using it in calculations or business logic.

### Choose an Appropriate Serialization Format

- Use JSON for portable structured data.
- Use Pickle only when Python-specific serialization is appropriate and the data source is trusted.

### Never Unpickle Untrusted Data

A malicious Pickle stream can execute arbitrary code during deserialization.

### Use `pathlib` for Path Management

It provides a convenient, cross-platform approach to handling file and directory paths.

---

## 22. Common Mistakes

### Mistake 1: Forgetting to Close a File

Manually opening a file without reliably closing it can leave resources open.

**Better approach:** Use a context manager.

### Mistake 2: Using `"w"` When You Want to Append

```python
with open("log.txt", "w", encoding="utf-8") as file:
    file.write("New log entry")
```

This replaces existing contents.

If the goal is to add a new entry, use append mode:

```python
with open("log.txt", "a", encoding="utf-8") as file:
    file.write("New log entry\n")
```

### Mistake 3: Expecting `writelines()` to Add Newlines

`writelines()` does not automatically separate strings.

Include newline characters or add separators explicitly.

### Mistake 4: Writing an Integer Directly to a Text File

```python
with open("number.txt", "w", encoding="utf-8") as file:
    file.write(5)
```

This raises a `TypeError` because text-mode `write()` expects a string.

**Correct version:**

```python
with open("number.txt", "w", encoding="utf-8") as file:
    file.write(str(5))
```

### Mistake 5: Reading a Large File All at Once

Calling `read()` without a size limit reads the remaining contents into memory.

For large files, consider processing them line by line or in chunks.

### Mistake 6: Confusing JSON and Python Types

A Python tuple becomes a JSON array and is normally reconstructed as a list.

JSON does not preserve every Python-specific type automatically.

### Mistake 7: Assuming Pickle Is Safe for Any File

Pickle is not safe for untrusted input.

Only unpickle data from sources you trust.

### Mistake 8: Assuming Relative Paths Are Relative to the Script

Relative paths are generally interpreted from the current working directory.

Check the working directory or use `pathlib` to construct paths deliberately.

---

## ✅ Key Takeaways

| Concept | Key Idea |
|---|---|
| File Handling | Allows programs to read and write persistent data |
| Text File | Handles character data through an encoding |
| Binary File | Handles raw bytes |
| `open()` | Opens a file |
| `"r"` | Reads an existing file |
| `"w"` | Creates or truncates a file for writing |
| `"a"` | Appends data |
| `write()` | Writes a string |
| `writelines()` | Writes multiple strings without automatically adding separators |
| `read()` | Reads remaining contents |
| `readline()` | Reads one line |
| `with` | Manages resource cleanup |
| `seek()` | Changes the file position |
| `tell()` | Reports the current file position |
| Chunked Reading | Processes large files incrementally |
| Serialization | Converts an object into a storable or transferable representation |
| JSON | Human-readable, portable structured-data format |
| Pickle | Python-specific serialization format that must only be loaded from trusted sources |
| `pathlib` | Provides cross-platform filesystem path handling |

---

## 🚀 Practical Applications

File handling becomes especially useful when working with:

- **Data Analytics:** Reading CSV files and saving analysis results.
- **Data Engineering:** Processing large files and managing data pipelines.
- **Automation:** Reading input files and generating reports.
- **APIs:** Working with JSON data.
- **Configuration Management:** Loading application settings.
- **Logging:** Recording application events.
- **Machine Learning:** Saving configuration and selected Python objects.
- **Application Development:** Managing persistent data and resources.

### Recommended Practice Exercises

1. Create a text file and write five lines to it.
2. Read the file using `read()`, `readline()`, and a `for` loop.
3. Append a new line without deleting existing content.
4. Read a large text file in chunks.
5. Copy an image using binary mode.
6. Store a Python dictionary in JSON and load it back.
7. Serialize a tuple to JSON and examine its reconstructed type.
8. Convert a custom class instance into a JSON-compatible dictionary.
9. Create and load a Pickle file in a trusted local environment.
10. Use `pathlib` to construct and access a file path.

---

## 📖 Learning Resources

### Video Tutorials

- [CampusX — Python File Handling](https://www.youtube.com/live/o-TAYRMQzIQ?si=mDrmmhh3bJ5g5aRm)
- [File Handling — Visually Explained](https://youtu.be/PXySDMxOPnI?si=T__Wu9oe_XnhEMZl)
- [JSON — Visually Explained](https://youtu.be/4rmBOxn0PdI?si=paZhPM8vHTqHB_PQ)
- [Context Managers — Visually Explained](https://youtu.be/LffQVBq3P9o?si=IpkVFJ3VKqg1ZKnV)

### Articles

- [Mimo — Python File Handling](https://mimo.org/glossary/python/file-handling)

### Official Documentation

- [Python Tutorial — Reading and Writing Files](https://docs.python.org/3/tutorial/inputoutput.html#reading-and-writing-files)
- [Python `open()` Documentation](https://docs.python.org/3/library/functions.html#open)
- [Python `pathlib` Documentation](https://docs.python.org/3/library/pathlib.html)
- [Python JSON Documentation](https://docs.python.org/3/library/json.html)
- [Python Pickle Documentation](https://docs.python.org/3/library/pickle.html)

---

**Next Step:** Practise reading and writing files, then build a small Python program that reads structured data, processes it, and saves the results to a JSON file.
