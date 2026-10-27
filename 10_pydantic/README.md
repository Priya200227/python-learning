# Pydantic — Practical Notes for Python & AI Engineering

> **Scope:** Pydantic v2  
> **Purpose:** A concise reference for validating application data, configuring Python applications, and validating structured LLM outputs.  
> **Sources:** CampusX Pydantic examples, Dave Ebbelaar's Pydantic Crash Course, Sheryians AI School's Pydantic course notes, and the official Pydantic documentation.

These notes consolidate the concepts in my own words. The original course materials remain the property of their respective creators.

---

## 1. What is Pydantic?

**Pydantic is a Python library that validates and transforms data using type annotations.** It helps ensure that incoming data conforms to a schema you define.

Common uses include API requests and responses, JSON payloads, application configuration, external service data, and LLM-generated output.

### Why use it?

Without a validation library, applications often need repetitive checks for types, missing fields, formats, ranges, and nested structures. Pydantic centralizes those rules in reusable models.

Pydantic v2 uses `pydantic-core` for its core validation logic, which is implemented in Rust.

### Basic workflow

1. Define a model describing the expected data.
2. Supply incoming data to the model.
3. Pydantic validates it and may coerce compatible values.
4. Use the resulting model instance if validation succeeds.
5. Handle `ValidationError` if validation fails.

**Important:** Pydantic checks the rules you define. It does not prove that data is factually true, authorized, safe, or semantically correct in every business context.

---

## 2. Installation and imports

```bash
python -m pip install pydantic email-validator
```

`email-validator` is needed for the `EmailStr` type. Settings management is a separate package:

```bash
python -m pip install pydantic-settings
```

```python
from pydantic import BaseModel, Field, ValidationError
```

Use a virtual environment and record dependencies in `requirements.txt` or your project configuration.

---

## 3. Your first model

A Pydantic model is a class that inherits from `BaseModel`. Annotated attributes define its fields.

```python
from pydantic import BaseModel

class User(BaseModel):
    name: str
    age: int

user = User(name="Priya", age="24")

print(user.name)  # Priya
print(user.age)   # 24: converted from a compatible string
```

### Creating a model from a dictionary

```python
user_data = {"name": "Priya", "age": "24"}
user = User(**user_data)  # Unpack dictionary keys as keyword arguments

# Alternative:
user = User.model_validate(user_data)
```

### Validation and coercion

By default, Pydantic is generally permissive when a value can be converted to the declared type. For example, `"24"` may become `24`. Invalid values raise `ValidationError`.

```python
try:
    User(name="Priya", age="twenty-four")
except ValidationError as exc:
    print(exc)
```

Coercion depends on the type and validation mode. Do not assume every input can be converted.

---

## 4. Type annotations and field definitions

```python
from pydantic import BaseModel

class Example(BaseModel):
    name: str
    age: int
    score: float
    active: bool
    tags: list[str]
    metadata: dict[str, str]
```

Type annotations describe the expected shape of each field. In modern Python, prefer built-in generics such as `list[str]` and `dict[str, str]` where supported.

### Required, nullable, and defaulted fields

```python
class Profile(BaseModel):
    name: str                    # Required
    nickname: str | None         # Required; None is an allowed value
    bio: str | None = None       # Not required; defaults to None
    active: bool = True           # Not required; defaults to True
```

**Key distinction:** `T | None` (or `Optional[T]`) means `None` is an allowed value. It does **not** by itself make the field optional to supply. A default value makes the field non-required.

For mutable defaults, prefer a factory when appropriate:

```python
from pydantic import BaseModel, Field

class TagList(BaseModel):
    tags: list[str] = Field(default_factory=list)
```

---

## 5. Constraints and metadata with `Field`

```python
from typing import Annotated
from pydantic import BaseModel, Field

ShortName = Annotated[
    str,
    Field(
        min_length=2,
        max_length=50,
        description="A person's display name",
        examples=["Priya", "Amit"],
    ),
]

class Product(BaseModel):
    name: ShortName
    price: float = Field(gt=0)
    stock: int = Field(ge=0)
    rating: float = Field(ge=0, le=5)
```

Common constraints:

| Argument | Meaning |
|---|---|
| `gt` / `ge` | Greater than / greater than or equal to |
| `lt` / `le` | Less than / less than or equal to |
| `min_length` / `max_length` | Minimum / maximum string or collection length |
| `pattern` | Require a string to match a regular expression |
| `strict=True` | Disable normal coercion for the field where supported |
| `description`, `title`, `examples` | Documentation and schema metadata |

`Annotated[T, Field(...)]` attaches metadata to a type. For a simple field, `name: str = Field(max_length=50)` is also clear.

### Strict validation

```python
class Measurement(BaseModel):
    weight: float = Field(gt=0, strict=True)
```

Strictness can behave differently depending on the type and whether validation is performed in Python or JSON mode. Use it when implicit conversion is undesirable.

---

## 6. Useful built-in types

```python
from pydantic import BaseModel, EmailStr, AnyUrl

class Contact(BaseModel):
    email: EmailStr
    website: AnyUrl
```

Useful types include:
- `EmailStr` — validates email-address syntax; requires `email-validator`.
- `AnyUrl` / `HttpUrl` — validates URL structure according to the type's rules.
- `UUID` from Python's `uuid` module — for UUID values.
- `datetime`, `date`, and `time` from `datetime`.
- `Decimal` from `decimal` — useful when decimal precision matters, such as monetary values.
- `Literal` and `Enum` — restrict values to a known set.
- `SecretStr` — helps avoid displaying a secret plainly in common representations; it is not a secret-storage solution.

A syntactically valid email does not prove that the inbox exists, and a valid URL does not prove that the destination is reachable. Business rules may need separate checks.

---

## 7. Custom field validation

Use `@field_validator` for rules that apply to one field, including custom checks and transformations.

```python
from pydantic import BaseModel, EmailStr, field_validator

ALLOWED_DOMAINS = {"example.com", "company.org"}

class Account(BaseModel):
    name: str
    email: EmailStr

    @field_validator("name")
    @classmethod
    def normalize_name(cls, value: str) -> str:
        return value.strip().title()

    @field_validator("email")
    @classmethod
    def check_company_domain(cls, value: EmailStr) -> EmailStr:
        domain = str(value).rsplit("@", 1)[-1].lower()
        if domain not in ALLOWED_DOMAINS:
            raise ValueError("Email domain is not allowed")
        return value
```

### Validator modes

- **`mode="after"`** (default): runs after normal validation for the field; receives the parsed value.
- **`mode="before"`**: runs on raw input before normal validation. The input may be any type, so handle it defensively.
- **`mode="plain"`**: takes over field validation, bypassing Pydantic's normal validation for that field.
- **`mode="wrap"`**: wraps the normal validation handler for greater control.

Example:

```python
from pydantic import BaseModel, field_validator

class Quantity(BaseModel):
    count: int

    @field_validator("count", mode="before")
    @classmethod
    def strip_string_input(cls, value):
        if isinstance(value, str):
            return value.strip()
        return value
```

Raise `ValueError` when a value is invalid. Do not return an error message as if it were a valid field value.

---

## 8. Model-level validation

Use `@model_validator` when a rule depends on multiple fields or the whole model.

```python
from typing import Self
from pydantic import BaseModel, model_validator

class Booking(BaseModel):
    start_hour: int
    end_hour: int

    @model_validator(mode="after")
    def validate_time_range(self) -> Self:
        if self.end_hour <= self.start_hour:
            raise ValueError("end_hour must be later than start_hour")
        return self
```

Modes:
- **`mode="after"`:** runs after field validation. In Pydantic v2, the method receives the model instance (`self`) and should return it.
- **`mode="before"`:** receives raw input, often a dictionary, before model construction. Return data for subsequent validation and handle unexpected input shapes defensively.
- **`mode="wrap"`:** wraps the model-validation handler.

Use field validators for single-field rules and model validators for cross-field rules. Prefer built-in constraints when they express the requirement clearly.

---

## 9. Nested models and collections

Use one model as a field type inside another when data contains structured objects.

```python
from pydantic import BaseModel, Field

class Address(BaseModel):
    city: str
    state: str
    postal_code: str

class Customer(BaseModel):
    name: str
    address: Address
    tags: list[str] = Field(default_factory=list)

payload = {
    "name": "Priya",
    "address": {
        "city": "Kurnool",
        "state": "Andhra Pradesh",
        "postal_code": "518001",
    },
    "tags": ["newsletter", "customer"],
}

customer = Customer.model_validate(payload)
print(customer.address.city)
```

Pydantic validates nested models recursively. Use `list[Model]` for lists of records and `dict[str, Model]` for mappings of keys to structured records.

Nested models are useful for addresses, orders, API payloads, and LLM output schemas.

---

## 10. Computed fields

A computed field is derived from other model attributes rather than supplied as an input field.

```python
from pydantic import BaseModel, computed_field

class Rectangle(BaseModel):
    width: float
    height: float

    @computed_field
    @property
    def area(self) -> float:
        return self.width * self.height

rectangle = Rectangle(width=4, height=3)
print(rectangle.area)        # 12.0
print(rectangle.model_dump())  # Includes area
```

Computed fields are included in serialization by default. They calculate a value from existing fields; they are not a replacement for input validation.

---

## 11. Serialization: dictionaries and JSON

Serialization converts a model into a representation that can be stored or sent elsewhere.

```python
customer_dict = customer.model_dump()
customer_json = customer.model_dump_json()

print(type(customer_dict))  # dict
print(type(customer_json))  # str
```

Useful options:

```python
customer.model_dump(include={"name"})
customer.model_dump(exclude={"address"})
customer.model_dump(exclude_unset=True)
customer.model_dump(exclude_none=True)
customer.model_dump(exclude_defaults=True)
```

- `include` / `exclude`: choose fields to include or omit; nested selection is supported.
- `exclude_unset=True`: omits fields not explicitly set during model creation.
- `exclude_none=True`: omits fields whose values are `None`.
- `exclude_defaults=True`: omits fields equal to their default values.

These options are not interchangeable. A defaulted field can be omitted by `exclude_unset=True` if the caller did not provide it.

Use `model_dump(mode="json")` for a Python dictionary containing JSON-compatible values. Use `model_dump_json()` when you need a JSON string.

---

## 12. Model configuration

Use `ConfigDict` and `model_config` to define model-wide behavior.

```python
from pydantic import BaseModel, ConfigDict

class InputPayload(BaseModel):
    model_config = ConfigDict(
        extra="forbid",
        str_strip_whitespace=True,
    )

    username: str
```

| Setting | Purpose |
|---|---|
| `extra="ignore"` | Ignore undeclared input fields (default) |
| `extra="forbid"` | Reject undeclared input fields |
| `extra="allow"` | Preserve extra fields |
| `strict=True` | Enable strict validation at model level |
| `str_strip_whitespace=True` | Strip leading/trailing whitespace from strings |
| `validate_assignment=True` | Validate field assignment after model creation |
| `frozen=True` | Prevent normal field reassignment |

Do not assume fields remain valid after mutation unless assignment validation or an appropriate immutable design is used. `frozen=True` prevents normal reassignment but should not be treated as deep immutability for every nested mutable object.

---

## 13. Aliases

Aliases map external field names to names used internally. This is useful when an API uses camelCase but Python code uses snake_case.

```python
from pydantic import BaseModel, Field, ConfigDict

class APIUser(BaseModel):
    model_config = ConfigDict(populate_by_name=True)

    user_id: int = Field(alias="userId")
    full_name: str = Field(alias="fullName")

user = APIUser.model_validate({"userId": 10, "fullName": "Priya"})
print(user.user_id)
print(user.model_dump(by_alias=True))
```

Aliases are useful when integrating with third-party APIs and JSON schemas. Alias-related configuration has evolved across Pydantic v2 minor versions, so check the documentation for the version pinned in your project.

---

## 14. Settings and environment variables

Use `pydantic-settings` to load configuration from environment variables and optionally `.env` files.

```python
from pydantic_settings import BaseSettings, SettingsConfigDict

class Settings(BaseSettings):
    model_config = SettingsConfigDict(
        env_file=".env",
        env_file_encoding="utf-8",
        extra="ignore",
    )

    app_name: str = "AI Service"
    debug: bool = False
    api_key: str

settings = Settings()
```

Example local `.env` file:

```dotenv
DEBUG=false
API_KEY=replace-with-a-local-secret
```

Security practices:
- Add `.env` to `.gitignore`.
- Never commit real API keys or credentials.
- Provide a safe `.env.example` containing placeholders if needed.
- In production, use the platform's environment configuration or secret manager.

`BaseSettings` is imported from `pydantic_settings`, not `pydantic`.

---

## 15. Understanding validation errors

Invalid input raises `pydantic.ValidationError`. Errors include locations, types, messages, and input details.

```python
from pydantic import ValidationError

try:
    Product(name="X", price=-10, stock=-1, rating=8)
except ValidationError as exc:
    print(exc)
    print(exc.errors())
```

When debugging:
1. Read `loc` to identify the failing field or nested path.
2. Read the error type and message.
3. Correct the input or model rule.
4. Avoid exposing sensitive input values in user-facing errors or logs.

---

## 16. Pydantic in LLM applications

LLM responses can be incomplete, malformed, or inconsistent. A Pydantic model defines the structure your application expects and validates parsed output before use.

```python
from typing import Literal
from pydantic import BaseModel, Field

class TaskExtraction(BaseModel):
    task: str = Field(description="The action to perform")
    priority: Literal["low", "medium", "high"]
    due_date: str | None = Field(
        default=None,
        description="Due date if explicitly stated; otherwise null",
    )
```

Typical workflow:

1. Define the output schema.
2. Request structured output from the LLM provider when supported, or request JSON.
3. Validate the result using the provider's integration or Pydantic.
4. Handle failures with a controlled retry, repair, or fallback strategy.
5. Apply business rules and authorization checks before taking action.

For JSON text:

```python
from pydantic import ValidationError

raw_json = '''
{
  "task": "Send the project update",
  "priority": "high",
  "due_date": null
}
'''

try:
    task = TaskExtraction.model_validate_json(raw_json)
except ValidationError:
    print("The output did not match the expected schema.")
else:
    print(task.task)
```

**Important:**
- Schema validation checks shape and declared constraints, not factual correctness.
- A field description alone does not restrict values; use `Literal` or `Enum` for fixed choices.
- Validated output is still untrusted. Do not execute generated code, SQL, shell commands, or consequential tool actions merely because output passed validation.
- Prefer provider-native structured-output modes when available, while still handling refusals, incomplete responses, and validation failures.

---

## 17. Common mistakes

1. **Confusing nullable with non-required:** `str | None` without a default is still required.
2. **Assuming all inputs are strict:** Pydantic may coerce compatible values unless strict behavior is enabled.
3. **Using old validator syntax:** Pydantic v2 uses `@field_validator` and `@model_validator`; v1 tutorials may show older APIs.
4. **Using the wrong validator scope:** field validators handle individual fields; model validators handle relationships across fields.
5. **Forgetting to return a value/model:** after validators should return the validated value or model instance.
6. **Confusing serialization outputs:** `model_dump()` returns a dictionary; `model_dump_json()` returns a JSON string.
7. **Treating serialization flags as equivalent:** `exclude_unset`, `exclude_none`, and `exclude_defaults` mean different things.
8. **Assuming validation guarantees truth or safety:** it checks defined rules, not real-world facts.
9. **Committing secrets:** keep `.env` files and credentials out of Git.
10. **Mixing Pydantic v1 and v2:** follow one version's docs and pin the dependency.

---

## 18. Practice checklist

- [ ] Create a `User` model with required and defaulted fields.
- [ ] Test valid, invalid, and coercible values.
- [ ] Add numeric and string constraints using `Field`.
- [ ] Validate an email and URL using specialized types.
- [ ] Write a field validator that normalizes a string.
- [ ] Write a model validator that compares two fields.
- [ ] Create nested `Customer`, `Address`, and `Order` models.
- [ ] Add a computed field and inspect serialized output.
- [ ] Compare `model_dump()` with `model_dump_json()`.
- [ ] Test `include`, `exclude`, `exclude_unset`, and `exclude_none`.
- [ ] Configure `extra="forbid"` and test unexpected input.
- [ ] Load settings without committing secrets.
- [ ] Define an LLM output schema and handle `ValidationError`.
- [ ] Write tests for valid and invalid cases using `pytest`.

---

## 19. Quick reference

| Need | Use |
|---|---|
| Define a schema | `BaseModel` |
| Validate a dictionary/object | `Model.model_validate(data)` |
| Validate JSON text | `Model.model_validate_json(json_text)` |
| Add constraints or metadata | `Field(...)` |
| Validate one field | `@field_validator(...)` |
| Validate across fields | `@model_validator(...)` |
| Define nested structures | Another model as a field type |
| Calculate a derived value | `@computed_field` |
| Export a dictionary | `model.model_dump()` |
| Export JSON text | `model.model_dump_json()` |
| Generate JSON Schema | `Model.model_json_schema()` |
| Configure model behavior | `ConfigDict` / `model_config` |
| Load environment settings | `pydantic-settings` / `BaseSettings` |
| Handle invalid input | `ValidationError` |

---

## 20. Learning resources

- **Dave Ebbelaar — video:** [Pydantic Crash Course: Build Reliable Python & AI Applications](https://www.youtube.com/watch?v=PkQIREapb9o)
- **Dave Ebbelaar — repository:** [pydantic-crash-course](https://github.com/daveebbelaar/pydantic-crash-course)
- **Course docs:** [Introduction](https://github.com/daveebbelaar/pydantic-crash-course/blob/master/docs/01-introduction.md) · [Type hints](https://github.com/daveebbelaar/pydantic-crash-course/blob/master/docs/02-type-hints.md) · [First model](https://github.com/daveebbelaar/pydantic-crash-course/blob/master/docs/03-your-first-model.md) · [Validation and fields](https://github.com/daveebbelaar/pydantic-crash-course/blob/master/docs/04-validation-and-fields.md) · [Nested models](https://github.com/daveebbelaar/pydantic-crash-course/blob/master/docs/05-nested-models.md) · [Settings](https://github.com/daveebbelaar/pydantic-crash-course/blob/master/docs/06-pydantic-settings.md) · [Structured LLM output](https://github.com/daveebbelaar/pydantic-crash-course/blob/master/docs/07-structured-llm-output.md) · [Summary](https://github.com/daveebbelaar/pydantic-crash-course/blob/master/docs/08-summary.md)
- **Official docs:** [Pydantic](https://docs.pydantic.dev/latest/) · [Models](https://docs.pydantic.dev/latest/concepts/models/) · [Validators](https://docs.pydantic.dev/latest/concepts/validators/) · [Fields](https://docs.pydantic.dev/latest/concepts/fields/) · [Serialization](https://docs.pydantic.dev/latest/concepts/serialization/) · [Settings](https://docs.pydantic.dev/latest/concepts/pydantic_settings/)

---

*Last reviewed: October 2026. Check the documentation for the exact Pydantic version installed in your project.*
