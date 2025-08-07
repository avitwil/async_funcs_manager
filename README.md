



````markdown
# Async Function Container with Named Execution

This Python module provides a framework for encapsulating and managing regular (blocking) Python functions and executing them asynchronously using `asyncio`.

It is especially useful for applications that need to manage and execute multiple functions concurrently, while also offering error handling, introspection, and flexibility for adding new functions at runtime.

---

## Features

- Encapsulate any callable with arguments using `CapsFunc`.
- Store and manage multiple functions via `FuncsToAsync`.
- Run any stored function asynchronously using `await funcs("func_name")`.
- Execute all registered functions concurrently with `AsyncRunner`.
- Supports forced overwrites and name-based access.
- Full error handling and input validation.

---

## Requirements

- Python 3.9+
- No external dependencies.

---

## Installation

Simply copy the module into your project or import as a submodule.

```bash
# If saved as asyncfuncs.py
from asyncfuncs import CapsFunc, FuncsToAsync, AsyncRunner
````

---

## Usage

### 1. Define Blocking Functions

```python
import time

def slow_add(a: int, b: int) -> int:
    time.sleep(1)
    return a + b

def say_hello(name: str) -> str:
    time.sleep(1)
    return f"Hello, {name}!"
```

### 2. Wrap with `CapsFunc`

```python
f1 = CapsFunc("add", slow_add, 3, 4)
f2 = CapsFunc("hello", say_hello, name="Twill")
```

### 3. Store in `FuncsToAsync`

```python
funcs = FuncsToAsync(f1, f2)
```

### 4. Run Asynchronously

```python
import asyncio

async def main():
    result1 = await funcs("add")
    result2 = await funcs("hello")
    print(result1)  # 7
    print(result2)  # Hello, Twill

asyncio.run(main())
```

---

## Using `AsyncRunner` to Run All at Once

```python
async def main():
    async with AsyncRunner(funcs) as runner:
        results = await runner.run_all()
        print(results)

# Output example:
# {'add': 7, 'hello': 'Hello, Twill'}
```

---

## API Overview

### `CapsFunc(name, func, *args, **kwargs)`

Encapsulates a function with its name and arguments.

### `FuncsToAsync(*CapsFunc)`

Stores multiple `CapsFunc` objects.

#### Methods:

* `await funcs("name")` – Run function asynchronously.
* `funcs.add(cfunc, force=False)` – Add a new function.
* `name in funcs` – Check if a name exists.
* `funcs.names()` – List all function names.

### `AsyncRunner(funcs: FuncsToAsync)`

Async context manager to run all functions concurrently.

---

## Error Handling Examples

```python
try:
    await funcs("non_existent")
except KeyError as e:
    print(e)

try:
    funcs.add("not a CapsFunc")  # Raises TypeError
except TypeError as e:
    print(e)

try:
    await funcs(123)  # Invalid name type
except TypeError as e:
    print(e)
```

---

## Test and Debug

Run the module directly to test all functionality:

```bash
python asyncfuncs.py
```

It will demonstrate:

* Normal execution
* Duplicate handling
* Forced overwrite
* Error handling

---

## License

This project is licensed under the terms of the [MIT License](LICENSE).

---

## Author

Created by Avi Twil, 2025.

```

