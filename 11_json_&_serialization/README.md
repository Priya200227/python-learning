# JSON & Serialization

A practical collection of Python examples covering **JSON, serialization, deserialization, nested data, custom objects, and Pickle**.

These notes build on Python file handling and demonstrate how structured data can be saved, loaded, and exchanged between programs.

## Learning Objectives

- Explain serialization and deserialization.
- Understand JSON objects, arrays, keys, and values.
- Read JSON files into Python objects and write Python data to JSON files.
- Work with nested JSON structures safely.
- Use `json.dump()`, `json.load()`, `json.dumps()`, and `json.loads()`.
- Understand why some Python objects need custom conversion before JSON serialization.
- Compare JSON with Python's `pickle` module and understand the security implications.

## Topics Covered

- Serialization vs. deserialization
- JSON syntax and supported value types
- JSON objects and arrays
- Reading and writing JSON files
- `json.load()`, `json.dump()`, `json.loads()`, and `json.dumps()`
- Indentation and readable output
- Accessing keys and nested values
- Safe dictionary access with `.get()`
- JSON-compatible Python data types
- Custom object conversion
- Pickling and unpickling
- JSON vs. Pickle
- Security considerations

## 1. What Is Serialization?

**Serialization** converts an in-memory data structure into a format that can be stored or transferred.

**Deserialization** reconstructs usable data from that serialized representation.

```text
Serialization:     Python data  → JSON text
Deserialization:   JSON text    → Python data
```

Serialization is useful when an application needs to save structured data in a file, send data through an API, or exchange information with another system.

## 2. What Is JSON?

**JSON (JavaScript Object Notation)** is a text-based data format commonly used for configuration files, APIs, and data exchange. Despite its name, JSON is language-independent and is supported by many programming languages.

```json
{
  "name": "Matcha",
  "price": 12.99,
  "in_stock": true,
  "tags": ["tea", "green"]
}
```

### JSON value types

| JSON type | Example | Python equivalent after decoding |
|---|---|---|
| Object | `{"name": "Matcha"}` | `dict` |
| Array | `["tea", "coffee"]` | `list` |
| String | `"Matcha"` | `str` |
| Number | `12.99` or `3` | `float` or `int` |
| Boolean | `true` / `false` | `True` / `False` |
| Null | `null` | `None` |

JSON syntax uses **double quotes** for strings and object keys. JSON booleans are lowercase (`true`, `false`), and JSON null is written as `null`.

JSON does not directly represent every Python type. For example, tuples are normally encoded as JSON arrays and decode as lists; custom class instances require an explicit conversion strategy.

## 3. Python's `json` Module

Python includes a standard-library module named `json`; no third-party installation is needed.

```python
import json
```

| Function | Purpose | Input → Output |
|---|---|---|
| `json.dump()` | Write JSON to a file object | Python object → file |
| `json.load()` | Read JSON from a file object | file → Python object |
| `json.dumps()` | Convert data to a JSON string | Python object → `str` |
| `json.loads()` | Parse a JSON string | `str` → Python object |

**Memory aid:** `dump`/`load` work with file objects; `dumps`/`loads` work with strings. The `s` stands for string.

## 4. Writing Python Data to a JSON File

Use `json.dump()` when writing structured data directly to a file.

```python
import json

catalog = {
    "products": [
        {"name": "Matcha", "price": 12.99, "in_stock": True},
        {"name": "Black Tea", "price": 9.99, "in_stock": False}
    ]
}

with open("catalog.json", "w", encoding="utf-8") as file:
    json.dump(catalog, file, indent=4)
```

- `"w"` opens the file for writing and replaces existing contents.
- `encoding="utf-8"` makes the text encoding explicit.
- `indent=4` formats JSON for readability; it is optional.

Use a context manager (`with`) so the file is closed reliably, including if an error occurs.

## 5. Reading a JSON File

Use `json.load()` to parse JSON from an open file and convert it into Python data structures.

```python
import json

with open("catalog.json", "r", encoding="utf-8") as file:
    catalog = json.load(file)

print(catalog)
print(type(catalog))  # dict
```

Once loaded, access the data as a normal Python dictionary and list.

```python
print(catalog["products"][0]["name"])   # Matcha
print(catalog["products"][0]["price"])  # 12.99
```

## 6. Safe Access to Dictionary Values

Direct indexing raises `KeyError` if a key is absent:

```python
name = catalog["name"]
```

Use `.get()` when a key may be missing:

```python
name = catalog.get("name")
price = catalog.get("price", 0)
```

- `catalog.get("name")` returns `None` if `"name"` is missing.
- `catalog.get("price", 0)` returns `0` if `"price"` is missing.

`.get()` also returns `None` when a key exists with a value of `None`, unless you provide a different default or separately check key membership.

For nested structures, guard each level rather than assuming every key exists:

```python
product = catalog.get("product", {})
details = product.get("details", {})
description = details.get("description", "No description available")
```

This example assumes each intermediate value is a dictionary. For untrusted or irregular input, validate the structure before accessing deeper levels.

## 7. JSON Strings: `dumps()` and `loads()`

Use `json.dumps()` to serialize Python data into a string.

```python
import json

product = {"name": "Matcha", "price": 12.99, "in_stock": True}
json_text = json.dumps(product, indent=2)

print(json_text)
print(type(json_text))  # str
```

Use `json.loads()` to parse a JSON string:

```python
restored_product = json.loads(json_text)

print(restored_product["name"])  # Matcha
print(type(restored_product))    # dict
```

The input to `json.loads()` must be valid JSON text. Invalid JSON raises `json.JSONDecodeError`.

## 8. Working with Nested JSON

JSON objects can contain other objects and arrays, which makes them useful for representing real-world data.

```python
catalog = {
    "store": "Tea Corner",
    "products": [
        {
            "name": "Matcha",
            "price": 12.99,
            "sizes": ["small", "medium", "large"]
        },
        {
            "name": "Black Tea",
            "price": 9.99,
            "sizes": ["small", "medium"]
        }
    ]
}

first_product = catalog["products"][0]
print(first_product["name"])       # Matcha
print(first_product["sizes"][1])   # medium
```

- Dictionary keys access object values.
- List indexes are zero-based.
- The Python data structure reflects the JSON document's structure.

## 9. JSON-Compatible Types and Custom Objects

The JSON encoder handles common types such as dictionaries, lists, strings, numbers, booleans, and `None`. It does not automatically know how to encode arbitrary custom class instances.

```python
class Person:
    def __init__(self, first_name, last_name, age):
        self.first_name = first_name
        self.last_name = last_name
        self.age = age

person = Person("Asha", "Rao", 24)
```

Convert the object into a JSON-compatible dictionary explicitly:

```python
person_data = {
    "name": f"{person.first_name} {person.last_name}",
    "age": person.age
}

json_text = json.dumps(person_data, indent=2)
```

For reusable custom conversion, `json.dump()` and `json.dumps()` accept a `default` function. It must return a JSON-serializable value for the object it handles.

```python
def to_json_compatible(value):
    if isinstance(value, Person):
        return {
            "name": f"{value.first_name} {value.last_name}",
            "age": value.age
        }
    raise TypeError(f"Object of type {type(value).__name__} is not JSON serializable")

json_text = json.dumps(person, default=to_json_compatible, indent=2)
```

Converting an object to a dictionary does not automatically reconstruct the original class when loading it. If reconstruction is required, define and validate that conversion explicitly.

## 10. Pickle: Python Object Serialization

Python's `pickle` module serializes Python objects into a binary representation. It supports many Python-specific objects that JSON cannot represent directly.

```python
import pickle

data = {"name": "Asha", "scores": [85, 92, 88]}

with open("data.pkl", "wb") as file:
    pickle.dump(data, file)

with open("data.pkl", "rb") as file:
    restored_data = pickle.load(file)
```

- `"wb"` means write binary.
- `"rb"` means read binary.
- Pickle files are not designed to be human-readable or language-independent.

### JSON vs. Pickle

| Feature | JSON | Pickle |
|---|---|---|
| Representation | Text | Binary |
| Human-readable | Usually | No |
| Language-independent | Yes | No; Python-specific |
| Common use | APIs, configuration, data exchange | Python-specific object persistence |
| Arbitrary Python objects | Not directly | Supports many Python objects |
| Typical extension | `.json` | `.pkl` / `.pickle` |
| Safe to load from untrusted sources | Parse carefully; validate content | **No — never unpickle untrusted data** |

**Security warning:** Loading a malicious Pickle file can execute arbitrary code. Only unpickle data from a source you trust. Do not use Pickle as a general interchange format for untrusted external data.

## 11. Common Mistakes

- Using `json.load()` on a string instead of a file object; use `json.loads()` for a string.
- Using `json.loads()` on a file object instead of JSON text; use `json.load()` for a file.
- Expecting `json.dump()` to return the serialized JSON string; it writes to the file object.
- Expecting tuples to round-trip as tuples; JSON arrays decode to Python lists.
- Expecting JSON to serialize any custom Python object automatically.
- Assuming `.get()` validates the structure of nested data; it only retrieves a dictionary value.
- Loading malformed JSON without handling `json.JSONDecodeError`.
- Loading Pickle data from an untrusted source.

## 12. Quick Reference

| Task | Example |
|---|---|
| Import JSON support | `import json` |
| Write Python data to file | `json.dump(data, file, indent=4)` |
| Read JSON from file | `data = json.load(file)` |
| Convert Python data to string | `text = json.dumps(data)` |
| Convert JSON string to Python data | `data = json.loads(text)` |
| Read optional dictionary key | `value = data.get("key", default)` |
| Write a Pickle object | `pickle.dump(obj, file)` |
| Read a Pickle object | `pickle.load(file)` — trusted data only |

## Practice Checklist

- [ ] Write a dictionary containing strings, numbers, booleans, lists, and nested dictionaries to `catalog.json`.
- [ ] Load the file and access values at multiple nesting levels.
- [ ] Compare direct key access with `.get()`.
- [ ] Convert Python data with `json.dumps()` and parse it with `json.loads()`.
- [ ] Handle malformed JSON using `try`/`except json.JSONDecodeError`.
- [ ] Write a custom class conversion function and serialize an instance.
- [ ] Explain when JSON is a better choice than Pickle and why untrusted Pickle data is dangerous.

---

**Key takeaway:** JSON is a readable, language-independent format for structured data exchange. Python's `json` module handles conversion between JSON and common Python data structures; custom objects require an explicit conversion strategy. Pickle serves a different, Python-specific purpose and must never be loaded from untrusted sources.
