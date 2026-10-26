# 08 - Object-Oriented Programming (OOP)

## 📌 Overview

**Object-Oriented Programming (OOP)** is a programming paradigm that organizes code around objects, which combine data and behavior.

In Python, **classes** define the structure and behavior of objects, while **objects** are individual instances of those classes. OOP helps organize code into reusable, maintainable, and modular components.

This section documents my learning of Python's OOP principles and related concepts through notes, code examples, experiments, and Jupyter notebooks.

### 🎯 Learning Objectives

By working through this section, I aim to understand how to:

- Design classes and create objects.
- Work with attributes, methods, constructors, and special methods.
- Encapsulate data and understand attribute access conventions.
- Implement inheritance, polymorphism, and abstraction.
- Understand Python's method resolution order and namespaces.
- Use decorators, iterators, dataclasses, and closures.
- Write more structured, reusable, and maintainable Python code.

---

## 📚 Table of Contents

- [Topics Covered](#-topics-covered)
- [Repository Structure](#-repository-structure)
- [Notebook Guide](#-notebook-guide)
  - [1. Classes and Objects](#1-classes-and-objects)
  - [2. Encapsulation](#2-encapsulation)
  - [3. Inheritance, Polymorphism, and Abstraction](#3-inheritance-polymorphism-and-abstraction)
  - [4. Namespaces and Decorators](#4-namespaces-and-decorators)
  - [5. Iterators](#5-iterators)
  - [6. Dataclasses and Closures](#6-dataclasses-and-closures)
- [Core OOP Principles](#-core-oop-principles)
- [Related Python Concepts](#-related-python-concepts)
- [Learning Approach](#-learning-approach)
- [Key Takeaways](#-key-takeaways)
- [Learning Resources](#-learning-resources)

---

## 🧩 Topics Covered

### 1. Classes and Objects

- Classes and objects
- Classes as blueprints
- Object creation and initialization
- Constructors and `__init__()`
- The `self` parameter
- Attributes and methods
- Instance variables and class variables
- Instance methods, class methods, and static methods
- Special methods
- Operator overloading

### 2. Encapsulation

- Encapsulation and data organization
- Public, protected, and private attributes
- Name mangling
- Getters and setters
- Instance variables and class variables
- Instance methods, class methods, and static methods
- Aggregation

### 3. Inheritance, Polymorphism, and Abstraction

**Inheritance**
- Parent and child classes
- Inheriting attributes and methods
- Method overriding
- The `super()` function
- Types of inheritance
- Method Resolution Order (MRO)

**Polymorphism**
- A common interface with different implementations
- Method overriding
- The concept of method overloading
- Operator overloading

**Abstraction**
- Abstract classes
- Abstract methods
- Abstract Base Classes (ABC)
- The `@abstractmethod` decorator
- Implementing abstract methods in subclasses

### 4. Namespaces and Decorators

**Namespaces**
- Local namespace
- Enclosing namespace
- Global namespace
- Built-in namespace
- The LEGB rule
- Variable scope and name resolution

**Decorators**
- Functions as first-class objects
- Passing functions as arguments
- Returning functions
- Nested functions
- Function decoration
- The `@` decorator syntax
- Using `*args` and `**kwargs` in decorators

### 5. Iterators

- Iterables and iterators
- The `iter()` and `next()` functions
- The `__iter__()` and `__next__()` methods
- Implementing custom iterators
- The `StopIteration` exception
- Retrieving values one at a time

### 6. Dataclasses and Closures

**Dataclasses**
- Creating dataclasses
- The `@dataclass` decorator
- Defining fields to store object data
- Reducing repetitive class code

**Closures**
- Nested functions
- Enclosing variables
- Returning inner functions
- Retaining access to variables from an enclosing scope
- Understanding how closures work after the outer function has finished executing

---

## 🗂️ Repository Structure

```text
08_oop/
│
├── oop_class.ipynb
├── oop_encapsulation.ipynb
├── oop_inher_poly_abs.ipynb
├── oop_namespaces_decorators.ipynb
├── Iterators.ipynb
├── data_classes_&_closures.ipynb
└── README.md
```

*Note: The directory name above is illustrative. Keep the actual directory name consistent with your repository.*

---

## 📓 Notebook Guide

### 1. Classes and Objects

**File:** `oop_class.ipynb`

Introduces the fundamentals of object-oriented programming in Python, focusing on how classes define object structure and behavior.

**Topics covered:**
- Classes and objects
- Classes as blueprints
- Creating and initializing objects
- Constructors and `__init__()`
- The `self` parameter
- Attributes and methods
- Instance variables and class variables
- Instance methods, class methods, and static methods
- Special methods
- Operator overloading

**Learning focus:** Understand how objects store data, expose behavior through methods, and use special methods to customize their behavior.

### 2. Encapsulation

**File:** `oop_encapsulation.ipynb`

Explores encapsulation and the conventions Python uses to organize object data and control access to attributes.

**Topics covered:**
- Encapsulation
- Public, protected, and private attributes
- Name mangling
- Getters and setters
- Instance variables and class variables
- Class methods and static methods
- Aggregation

**Learning focus:** Understand how data and behavior can be grouped within a class, how attribute-access conventions work, and how objects can be related through aggregation.

### 3. Inheritance, Polymorphism, and Abstraction

**File:** `oop_inher_poly_abs.ipynb`

Explores three major OOP principles and how they support code reuse, flexible interfaces, and structured class design.

**Topics covered:**

**Inheritance**
- Parent and child classes
- Inheriting attributes and methods
- Method overriding
- `super()`
- Types of inheritance
- Method Resolution Order (MRO)

**Polymorphism**
- Common method names with different implementations
- Method overriding
- The concept of method overloading
- Operator overloading

**Abstraction**
- Abstract classes and abstract methods
- Abstract Base Classes (ABC)
- `@abstractmethod`
- Implementing abstract methods in subclasses

**Learning focus:** Understand how classes can extend existing behavior, provide different implementations of a common interface, and define interfaces that subclasses are expected to implement.

### 4. Namespaces and Decorators

**File:** `oop_namespaces_decorators.ipynb`

Covers Python's name-resolution mechanism and the use of decorators to modify or extend function behavior.

**Namespaces topics:**
- Local, enclosing, global, and built-in namespaces
- The LEGB rule
- Variable scope and name resolution

**Decorators topics:**
- Functions as objects
- Passing functions to other functions
- Returning functions
- Nested functions
- Applying decorators
- The `@` syntax
- Using `*args` and `**kwargs` in decorators

**Learning focus:** Understand how Python resolves variable names and how functions can be passed around, wrapped, and extended through decorators.

### 5. Iterators

**File:** `Iterators.ipynb`

Explores Python's iteration protocol and how values are retrieved sequentially from an iterator.

**Topics covered:**
- Iterable vs. iterator
- `iter()` and `next()`
- `__iter__()` and `__next__()`
- Creating custom iterators
- `StopIteration`

**Learning focus:** Understand the mechanism behind iteration and how to implement custom objects that produce values one at a time.

### 6. Dataclasses and Closures

**File:** `data_classes_&_closures.ipynb`

Covers two related Python concepts: dataclasses, which simplify classes primarily used to store data, and closures, which allow inner functions to retain access to variables from their enclosing scope.

**Dataclasses topics:**
- Creating dataclasses
- The `@dataclass` decorator
- Defining fields
- Reducing repetitive class code

**Closures topics:**
- Nested functions
- Enclosing variables
- Returning inner functions
- Retaining access to enclosing-scope variables
- Closure behavior after the outer function returns

**Learning focus:** Understand how dataclasses reduce boilerplate in data-focused classes and how closures preserve access to variables from an enclosing function.

---

## 🏛️ Core OOP Principles

The notebooks cover the three major OOP principles below, along with encapsulation-related concepts and object composition.

| Principle | Description | Python Concepts |
|---|---|---|
| **Encapsulation** | Groups data and behavior together and establishes conventions for attribute access. | Classes, attributes, name mangling, properties or getters and setters |
| **Inheritance** | Allows a class to derive and extend behavior from another class. | Parent classes, child classes, `super()`, MRO |
| **Polymorphism** | Allows different objects to provide different implementations of a common interface. | Method overriding, operator overloading, duck typing |
| **Abstraction** | Defines essential behavior while hiding implementation details behind an interface. | Abstract classes, ABC, `@abstractmethod` |

These principles help structure larger programs, reduce unnecessary duplication, and make code easier to extend and maintain.

---

## ⚙️ Related Python Concepts

Alongside the core OOP principles, this section covers several Python features that are useful for understanding how the language works.

| Concept | Purpose |
|---|---|
| **Special methods** | Define how objects behave with built-in operations and language features. |
| **Operator overloading** | Customizes operators for user-defined objects through special methods. |
| **Namespaces and LEGB** | Explain where names are stored and how Python resolves them. |
| **Decorators** | Wrap functions or classes to extend or modify their behavior. |
| **Iterators** | Provide a protocol for retrieving values one at a time. |
| **Dataclasses** | Reduce boilerplate when defining classes primarily used to store data. |
| **Closures** | Preserve access to variables from an enclosing scope. |
| **Aggregation** | Represents a relationship in which one object contains or refers to another object. |

These concepts complement OOP and contribute to writing idiomatic, reusable Python code.

---

## 🧪 Learning Approach

The notebooks are organized to build understanding progressively:

1. **Learn the fundamentals** — Start with classes, objects, attributes, and methods.
2. **Understand object design** — Explore encapsulation and relationships between objects.
3. **Study the core principles** — Practice inheritance, polymorphism, and abstraction.
4. **Explore Python internals and function behavior** — Learn namespaces, name resolution, and decorators.
5. **Understand iteration** — Work with iterables, iterators, and the iteration protocol.
6. **Explore additional language features** — Practice dataclasses and closures.

The emphasis is on understanding concepts through code examples, experimentation, and practical implementation.

---

## ✅ Key Takeaways

After completing this section, the main concepts to understand are:

- Classes define object structure and behavior; objects are instances of classes.
- `self` refers to the current instance, while `cls` is conventionally used as the first parameter of a class method.
- Instance variables belong to individual instances, while class variables are associated with the class.
- Python uses naming conventions and name mangling rather than strict access modifiers for public, protected, and private attributes.
- Inheritance supports code reuse and specialization.
- Polymorphism allows different objects to respond to a common interface.
- Abstract Base Classes can define methods that concrete subclasses must implement.
- MRO determines the order in which Python searches classes for attributes and methods.
- Namespaces and the LEGB rule explain how Python resolves names.
- Decorators can extend function behavior without changing the original function's core implementation.
- Iterators follow Python's iteration protocol.
- Dataclasses simplify classes that primarily store data.
- Closures retain access to variables from their enclosing scope.

---

## 📖 Learning Resources

The following resources supported my study of OOP and related Python concepts.

### Classes, Objects, and Encapsulation

- [CampusX — Classes and Objects](https://www.youtube.com/live/Trvqq5w7-0Q?si=e9sc_yo-C6GazH6c)
- [CampusX — Encapsulation, Static Keyword, and Reference Variables](https://www.youtube.com/live/P4xizq3CiJg?si=4W6tBXwI_5DcmoTk)

### Inheritance, Polymorphism, and Abstraction

- [CampusX — Inheritance and Polymorphism](https://www.youtube.com/live/bEWwM4hXZg8?si=lfPWD6JF5KryBr0m)
- [CampusX — Abstraction](https://youtu.be/O-NrQvtorp0?si=l5bO_cCtBAdPWsU0)

### Namespaces, Decorators, and Iterators

- [CampusX — Namespaces and Decorators](https://www.youtube.com/live/aMARZGTbULc?si=7eMI7kb3DRtKCwx7)
- [CampusX — Iterators](https://youtu.be/pH7YVRhnpUI?si=bX51y1JkruZ87X8m)

### Dataclasses and Closures

- [CampusX — Dataclasses and Closures](https://www.youtube.com/live/Trvqq5w7-0Q?si=e9sc_yo-C6GazH6c)
- [GeeksforGeeks — Python Closures](https://www.geeksforgeeks.org/python/python-closures/)

---

## 🚀 Why These Concepts Matter

A solid understanding of OOP and related Python features provides a foundation for building larger, more maintainable applications.

These concepts are useful when working with:

- Data analysis and data-processing pipelines
- Automation scripts
- API clients and integrations
- Machine learning and AI applications
- Reusable utility modules and libraries
- Larger Python applications with multiple interacting components

**The goal is not just to learn OOP syntax, but to understand how and when these concepts help organize real Python programs.**
