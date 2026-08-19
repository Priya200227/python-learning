# Python File Handling

A practical collection of Python examples covering file handling and file I/O, based on handwritten learning notes and a Jupyter Notebook.

The notebook progresses from basic text-file operations to context managers, file pointers, binary files, serialization/deserialization, JSON, custom Python objects, and Pickle.

## 📌 Topics Covered

- Text files vs. binary files
- Opening and closing files
- File modes
- Writing to files
- Appending to files
- Reading files
  - `read()`
  - `readline()`
  - `writelines()`
- Reading data in chunks
- File pointers
  - `seek()`
  - `tell()`
- Context managers with `with`
- Working with binary files
- Serialization and deserialization
  - JSON
    - `json.dump()`
    - `json.load()`
  - Serializing lists, dictionaries, tuples, and nested data
  - Serializing custom Python objects
  - Pickling and unpickling
- JSON vs Pickle
- Windows file paths and raw strings

## 1. What is File Handling?

File handling is the process of allowing a Python program to create, read, write, modify, and manage data stored in files.

A typical file I/O workflow is:

```
Open → Read/Write → Close
```

Python provides built-in tools for working with files without requiring an external library for basic file I/O.

## 2. Text Files vs Binary Files

There are two broad types of data handled during file I/O.

**Text**

Text data is represented as a sequence of characters.

Examples: `.txt`, `.csv`, `.py`, `.json`

**Binary**

Binary data is represented as bytes.

Examples: Images, Audio, Video, Executable files, Other non-text files

The file mode determines whether Python treats the file as text or binary.

## 3. Opening a File

The basic syntax is:

```python
f = open("filename", "mode")
```

Example:

```python
f = open("sample.txt", "w")
```

Here:
- `sample.txt` → file path/name
- `"w"` → write mode
- `f` → file object / file handle

The file object provides methods such as:

```python
f.write()
f.read()
f.readline()
f.seek()
f.tell()
f.close()
```

## 4. File Modes

Common modes used in the notebook:

| Mode | Purpose |
|------|---------|
| r    | Read |
| w    | Write |
| a    | Append |
| rb   | Read binary |
| wb   | Write binary |

**Write mode**

```python
with open("sample.txt", "w") as f:
    f.write("Hello world")
```

- If the file does not exist, it can be created.
- If the file already exists, `w` mode replaces its existing contents.

**Append mode**

```python
with open("sample.txt", "a") as f:
    f.write("\nI am fine")
```

Append mode adds new content to the end instead of replacing the existing content.

## 5. Writing to a File

**`write()`**

```python
f = open("sample.txt", "w")

f.write("Hello world")

f.close()
```

Multiple `write()` calls can be used:

```python
f = open("sample.txt", "w")

f.write("hello world")
f.write("\nhow are you?")

f.close()
```

The newline character `\n` moves the following text to a new line.

**`writelines()`**

`writelines()` can write multiple strings from an iterable.

```python
lines = [
    "hello\n",
    "hi\n",
    "how are you\n",
    "I am fine"
]

with open("sample.txt", "w") as f:
    f.writelines(lines)
```

Important: `writelines()` does not automatically add newline characters. If separate lines are required, include `\n` in the strings.

## 6. Reading from a File

**`read()`**

Reads the file contents.

```python
with open("sample.txt", "r") as f:
    data = f.read()

print(data)
```

You can also specify the number of characters to read:

```python
with open("sample.txt", "r") as f:
    data = f.read(10)

print(data)
```

**`readline()`**

Reads one line at a time.

```python
with open("sample.txt", "r") as f:
    print(f.readline(), end="")
    print(f.readline(), end="")
```

A loop can be used to process a file line by line:

```python
with open("sample.txt", "r") as f:
    while True:
        data = f.readline()

        if data == "":
            break

        print(data, end="")
```

## 7. Context Manager: `with`

Instead of manually calling `close()`, Python provides the `with` statement.

```python
with open("sample.txt", "w") as f:
    f.write("Hello")
```

Once execution leaves the `with` block, the file is automatically closed.

This is generally preferred because it ensures cleanup even when an exception occurs inside the block.

**Why use `with`?**

```
Without with:
open → work → remember to close

With with:
open → work → automatic cleanup
```

## 8. File Pointer

When a file is being read or written, Python maintains a current position in the file.

Two useful methods are:

**`tell()`**

Returns the current file position.

```python
with open("sample.txt", "r") as f:
    print(f.tell())
```

**`seek()`**

Moves the file position to a specified location.

```python
with open("sample.txt", "r") as f:
    f.seek(15)
    print(f.read(10))
```

Example of checking the position:

```python
with open("sample.txt", "r") as f:
    print(f.read(10))
    print(f.tell())

    print(f.read(10))
    print(f.tell())
```

Conceptually:

```
File
↓
0 1 2 3 4 5 6 7 8 9 10 ...

        ↑
    file pointer
```

## 9. Reading Large Files in Chunks

Large files should not always be loaded completely into memory.

The notebook explores reading data in smaller pieces:

```python
with open("big.txt", "r") as f:
    chunk_size = 10

    data = f.read(chunk_size)

    while data:
        print(data, end="***")
        data = f.read(chunk_size)
```

The idea is:

```
Large File
   ↓
Read small chunk
   ↓
Process chunk
   ↓
Read next chunk
   ↓
Repeat
```

This approach is useful when working with files that are too large to comfortably load into memory at once.

## 10. Binary Files

Text mode is designed for character-based data.

Binary files such as images need binary modes.

**Reading binary data**

```python
with open("screenshot1.png", "rb") as f:
    data = f.read()
```

**Copying a binary file**

```python
with open("screenshot1.png", "rb") as f:
    with open("screenshot_copy.png", "wb") as wf:
        wf.write(f.read())
```

The important modes are:

```
rb → read binary
wb → write binary
```

## 11. Working with Other Python Data Types

A text file stores text.

For example:

```python
with open("sample.txt", "w") as f:
    f.write("5")
```

When reading it:

```python
with open("sample.txt", "r") as f:
    value = int(f.read())

print(value + 5)
```

The string `"5"` has to be converted back into an integer.

This becomes more important with complex Python objects such as dictionaries and custom classes.

## 12. Serialization and Deserialization

**Serialization**

Serialization means converting an in-memory Python object into a format that can be stored or transmitted.

```
Python object
     ↓
Serialization
     ↓
Storable / transferable representation
```

**Deserialization**

Deserialization is the reverse process.

```
Stored representation
     ↓
Deserialization
     ↓
Python object
```

The notebook explores JSON and Pickle as two different approaches.

## 13. JSON

JSON (JavaScript Object Notation) is a text-based, human-readable data format commonly used for storing and exchanging structured data.

Python provides the built-in `json` module.

```python
import json
```

**Serialize a List**

```python
import json

numbers = [1, 2, 3, 4]

with open("demo.json", "w") as f:
    json.dump(numbers, f)
```

**Serialize a Dictionary**

```python
import json

data = {
    "name": "nitish",
    "age": 33,
    "gender": "male"
}

with open("demo.json", "w") as f:
    json.dump(data, f, indent=4)
```

`indent=4` makes the JSON easier for humans to read.

**Deserialize JSON**

```python
import json

with open("demo.json", "r") as f:
    data = json.load(f)

print(data)
print(type(data))
```

The JSON representation is converted back into corresponding Python data structures.

## 14. Nested JSON Data

JSON can represent nested structures.

Example:

```python
data = {
    "student": "nitish",
    "marks": [23, 14, 34, 45, 56]
}

with open("demo.json", "w") as f:
    json.dump(data, f)
```

This is useful for representing structured real-world data.

## 15. JSON and Tuples

The notebook also demonstrates serializing a tuple:

```python
import json

t = (1, 2, 3, 4, 5)

with open("demo.json", "w") as f:
    json.dump(t, f)
```

JSON does not have a separate tuple data type. Python therefore maps the tuple into a JSON array representation.

When deserialized, it comes back as a Python list rather than preserving the original tuple type.

This illustrates an important limitation of JSON: not every Python type has a one-to-one representation in JSON.

## 16. Serializing Custom Python Objects

JSON does not automatically know how to serialize arbitrary custom Python objects.

Example:

```python
class Person:

    def __init__(self, fname, lname, age, gender):
        self.fname = fname
        self.lname = lname
        self.age = age
        self.gender = gender
```

Create an object:

```python
person = Person("Nitish", "Singh", 33, "male")
```

A custom conversion function can be supplied to `json.dump()` through `default`.

Example:

```python
def show_object(person):
    if isinstance(person, Person):
        return {
            "name": person.fname + " " + person.lname,
            "age": person.age,
            "gender": person.gender
        }

with open("demo.json", "w") as f:
    json.dump(person, f, default=show_object, indent=4)
```

The custom object is converted into a JSON-compatible dictionary before being written.

## 17. Pickling

Pickle is Python's mechanism for serializing Python objects into a byte stream.

```python
import pickle
```

Unlike JSON, Pickle is designed specifically around Python object serialization.

**Pickle Dump**

```python
class Person:

    def __init__(self, name, age):
        self.name = name
        self.age = age

    def display_info(self):
        print(
            "Hi my name is",
            self.name,
            "and I am",
            self.age,
            "years old"
        )
```

Create an object:

```python
p = Person("priya", 24)
```

Serialize it:

```python
import pickle

with open("person.pkl", "wb") as f:
    pickle.dump(p, f)
```

**Pickle Load**

```python
import pickle

with open("person.pkl", "rb") as f:
    p = pickle.load(f)

p.display_info()
```

The Python object can be reconstructed from the serialized data.

## 18. JSON vs Pickle

| Feature | JSON | Pickle |
|---|---|---|
| Format | Text | Binary |
| Human-readable | Yes | No |
| Python-specific | No | Yes |
| Good for data exchange | Yes | Generally not its purpose |
| Supports arbitrary Python objects | Not directly | Yes |
| Common extension | `.json` | `.pkl` / `.pickle` |

**Simple mental model**

```
JSON
Python data → JSON text → Python data

Pickle
Python object → byte stream → Python object
```

**Security warning:** Only unpickle data that you trust. Loading malicious pickle data can execute arbitrary code.

## 19. File Paths

File paths tell Python where a file is located.

Example Windows path:

```python
"C:\\Users\\Priya\\Desktop\\notes.txt"
```

Backslashes have special meaning in Python strings because they can introduce escape sequences.

For example, `"\n"` represents a newline.

A raw string can be used for Windows paths:

```python
r"C:\Users\Priya\Desktop\notes.txt"
```

The `r` prefix tells Python to treat backslashes as literal characters for most path-related use cases.

## 20. Key Methods to Remember

| Method | Purpose |
|---|---|
| `open()` | Opens a file |
| `close()` | Closes a file |
| `write()` | Writes a string |
| `writelines()` | Writes multiple strings |
| `read()` | Reads file contents |
| `readline()` | Reads one line |
| `seek()` | Moves the file pointer |
| `tell()` | Returns the current file position |
| `json.dump()` | Serializes Python data to a JSON file |
| `json.load()` | Deserializes JSON from a file |
| `pickle.dump()` | Serializes a Python object |
| `pickle.load()` | Deserializes a Python object |

## 21. Practical Mental Model

Think of file handling as interacting with stored data:

```
                 FILE HANDLING
                      │
          ┌───────────┴───────────┐
          │                       │
       TEXT                     BINARY
          │                       │
     .txt / .json            images / video
          │                       │
     read / write             rb / wb
          │
      ┌───┴────┐
      │        │
   Raw text  Structured data
               │
          ┌────┴────┐
          │         │
         JSON     Pickle
          │         │
     human-readable Python objects
```

The main progression is:

```
Basic File I/O
      ↓
Read / Write / Append
      ↓
Context Manager
      ↓
File Pointer
      ↓
Large File Processing
      ↓
Binary Files
      ↓
Serialization
      ↓
JSON
      ↓
Pickle
```

## 22. What This Notebook Demonstrates

This notebook is intentionally example-driven. It does not just list file-handling syntax; it demonstrates the behavior of the operations through small experiments.

Examples include:

- Creating a file
- Overwriting an existing file
- Appending content
- Reading complete files
- Reading a fixed number of characters
- Reading line by line
- Using `with`
- Moving the file pointer
- Working with binary files
- Attempting to store non-string data in text mode
- Converting text back into Python data
- Serializing structured data with JSON
- Handling custom objects
- Serializing and reconstructing Python objects with Pickle

## 23. Files in This Repository

```
File-Handling/
│
├── file_handling.ipynb
└── README.md
```

The Jupyter Notebook contains the actual experiments and code examples covered while learning the topic.

## 24. Learning Outcome

After completing these examples, the goal is to be comfortable with:

- Opening files using the appropriate mode
- Reading and writing text files
- Appending data
- Using `with` for safe resource management
- Understanding the file pointer
- Using `seek()` and `tell()`
- Processing large files in chunks
- Reading and writing binary files
- Understanding serialization and deserialization
- Using JSON for structured, human-readable data
- Understanding the basic purpose of Pickle
- Recognizing when data needs conversion between text and Python types

## ⚠️ Notes on the Learning Examples

Some notebook cells are deliberately written to demonstrate what does not work and why.

For example:

```python
with open("sample.txt", "w") as f:
    f.write(5)
```

raises a `TypeError` because `write()` expects a string in text mode.

The notebook also contains exploratory examples that are intended for learning rather than production-ready implementation.

A few handwritten explanations simplify internal details for learning purposes. In particular, file storage should be thought of as persistent storage/disk, rather than literally "ROM", and the `with` statement is best understood as deterministic resource cleanup rather than relying on the garbage collector.

---
# Learning Resources

- [CampusX File Handling](https://www.youtube.com/live/o-TAYRMQzIQ?si=mDrmmhh3bJ5g5aRm)
- [Open GitHub Page](https://github.com/Asabeneh/30-Days-Of-Python/blob/master/19_Day_File_handling/19_file_handling.md)
- [Chroma Campus Website](https://www.cromacampus.com/blogs/what-is-file-handling-in-python-programming/)
- [Mimo Webpage](https://mimo.org/glossary/python/file-handling)
