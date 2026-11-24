# Python, FastAPI and Coding – 63 Interview Questions and Answers

*Arranged from fundamentals to advanced topics, with practical examples and scenario-based approaches.*

**Label format:** (V.Imp) = Very Important · (Imp) = Important · [Scenario Based] = scenario question

---

## 1. Python Fundamentals and Core Concepts

### 1. What is Python? (V.Imp)

**Answer:**

Python is a high-level, general-purpose programming language known for its readable syntax and large ecosystem of libraries. It supports procedural, object-oriented, and functional programming styles.

**Key features:**
- Easy-to-read syntax.
- Dynamically typed.
- Automatic memory management.
- Large standard library and third-party ecosystem.
- Supports scripting, web development, automation, data science, and AI.

**Example:**

```python
name = "Arbaaz"
print(f"Hello, {name}")
```

Python is commonly used for AI/ML applications, APIs with FastAPI or Django, automation, and data processing.

### 2. What is the difference between a Python list and a tuple? (V.Imp)

**Answer:**

Both lists and tuples are ordered collections that can store multiple values. The main difference is that a list is mutable, while a tuple is immutable.

| List | Tuple |
|---|---|
| Mutable | Immutable |
| Uses `[]` | Uses `()` |
| Supports item assignment | Items cannot be reassigned |
| Suitable for changing collections | Suitable for fixed collections |

```python
numbers = [10, 20, 30]
numbers[0] = 100

coordinates = (10, 20)
# coordinates[0] = 100  # TypeError
```

Use a list when the collection changes. Use a tuple when the collection should remain fixed. A tuple is only deeply immutable if its elements are immutable too.

### 3. What is a mutable object in Python? (V.Imp)

**Answer:**

A mutable object can be changed after it is created, without creating a new object.

Examples:
- `list`
- `dict`
- `set`
- Most custom class instances

```python
items = [1, 2, 3]
items.append(4)
print(items)  # [1, 2, 3, 4]
```

The list is modified in place.

### 4. What is an immutable object?

**Answer:**

An immutable object cannot be changed after creation. If an operation appears to modify it, Python creates or returns another object instead.

Common examples:
- `int`
- `float`
- `bool`
- `str`
- `tuple` (provided its contents are also immutable)
- `frozenset`
- `bytes`

```python
name = "Python"
name = name + " language"
```

The original string is not modified; the expression creates a new string and assigns it to `name`.

### 5. What is variable scoping in Python? (V.Imp)

**Answer:**

Variable scope determines where a variable can be accessed. Python follows the LEGB lookup rule:

1. **Local:** Inside the current function.
2. **Enclosing:** In an enclosing function.
3. **Global:** At module level.
4. **Built-in:** Names provided by Python, such as `len()`.

```python
x = "global"

def outer():
    x = "enclosing"

    def inner():
        x = "local"
        print(x)

    inner()

outer()  # local
```

Use `global` to assign to a module-level variable from a function, and `nonlocal` to assign to a variable in an enclosing function.

### 6. What are `*args` and `**kwargs` in Python? (V.Imp)

**Answer:**

They allow a function to accept a variable number of arguments.

- `*args` collects extra positional arguments into a tuple.
- `**kwargs` collects extra keyword arguments into a dictionary.

```python
def show_details(*args, **kwargs):
    print(args)
    print(kwargs)

show_details("Arbaaz", 5, city="Delhi", role="Developer")
```

Output:

```python
("Arbaaz", 5)
{"city": "Delhi", "role": "Developer"}
```

The names `args` and `kwargs` are conventions; the `*` and `**` syntax is what matters.

### 7. What is the difference between `__new__` and `__init__` in Python? (V.Imp)

**Answer:**

Both methods are involved in object creation, but they have different responsibilities.

- `__new__()` creates and returns a new instance. It is a static method in behavior and receives the class as its first argument, conventionally `cls`.
- `__init__()` initializes the already-created instance. It receives the instance as its first argument, conventionally `self`, and must return `None`.

```python
class Employee:
    def __new__(cls, name):
        print("Creating object")
        return super().__new__(cls)

    def __init__(self, name):
        print("Initializing object")
        self.name = name

employee = Employee("Arbaaz")
```

**Execution order:** `__new__()` runs first; if it returns an instance of the class, `__init__()` initializes it.

### 8. What is a Python compiler? (V.Imp)

**Answer:**

Python implementations commonly compile source code into an intermediate bytecode representation before executing it.

In CPython, the typical process is:

```text
Python source (.py)
       |
   Parsing / Compilation
       |
   Bytecode
       |
 Python Virtual Machine
       |
    Execution
```

The bytecode may be cached in `__pycache__` as `.pyc` files. Python is often described as interpreted, but compilation to bytecode is part of the normal CPython execution process. Other Python implementations may work differently.

### 9. What is FastAPI built on in Python? (V.Imp)

**Answer:**

FastAPI is a modern Python web framework built on:
- **Starlette:** Provides the ASGI web framework capabilities, routing, middleware, and request/response handling.
- **Pydantic:** Provides data validation and serialization using Python type hints.
- **ASGI:** The asynchronous server interface used by FastAPI applications.

FastAPI also uses type hints to generate OpenAPI documentation and interactive API documentation.

**Example:**

```python
from fastapi import FastAPI
from pydantic import BaseModel

app = FastAPI()

class Employee(BaseModel):
    name: str
    age: int

@app.post("/employees")
def create_employee(employee: Employee):
    return employee
```

### 10. What is a context manager in Python? (V.Imp)

**Answer:**

A context manager manages setup and cleanup around a block of code. It is commonly used with the `with` statement.

For example, when opening a file, Python automatically closes it when the block exits, even if an exception occurs.

```python
with open("data.txt", "r", encoding="utf-8") as file:
    content = file.read()
```

A context manager generally implements `__enter__()` and `__exit__()`, or is created using `contextlib`.

### 11. What are Python context managers? (V.Imp)

**Answer:**

This is the same core concept as Question 10. A context manager ensures resources are acquired and released safely.

**Custom example:**

```python
class DemoContext:
    def __enter__(self):
        print("Entering block")
        return self

    def __exit__(self, exc_type, exc_value, traceback):
        print("Leaving block")
        return False  # Do not suppress exceptions

with DemoContext():
    print("Inside block")
```

Use context managers for files, database transactions, locks, and other resources that need reliable cleanup.

### 12. What is a decorator in Python? (V.Imp)

**Answer:**

A decorator is a callable that takes another function or class and returns a modified or extended callable, without requiring changes to the original function's implementation.

Decorators are commonly used for logging, authentication, authorization, caching, timing, and retries.

```python
def my_decorator(func):
    def wrapper(*args, **kwargs):
        print("Before function")
        result = func(*args, **kwargs)
        print("After function")
        return result
    return wrapper

@my_decorator
def greet():
    print("Hello")

greet()
```

The syntax `@my_decorator` is equivalent to `greet = my_decorator(greet)`.

### 13. What is a decorator in Python? Can you provide a practical example, such as logging? (V.Imp)

**Answer:**

A decorator can add logging around a function without changing the function itself.

```python
from functools import wraps

def log_calls(func):
    @wraps(func)
    def wrapper(*args, **kwargs):
        print(f"Calling {func.__name__}")
        result = func(*args, **kwargs)
        print(f"{func.__name__} completed")
        return result
    return wrapper

@log_calls
def add(a, b):
    return a + b

print(add(10, 20))
```

`functools.wraps` preserves useful metadata such as the original function's name and docstring.

For production logging, use Python's `logging` module rather than relying on `print()`.

### 14. What is the difference between `yield` and `return` in Python? (V.Imp)

**Answer:**

- `return` ends a function and sends back a result.
- `yield` produces a value from a generator and pauses execution. The function can continue from that point when the next value is requested.

```python
def normal_function():
    return [1, 2, 3]

def generator_function():
    yield 1
    yield 2
    yield 3

print(normal_function())

for value in generator_function():
    print(value)
```

A generator produces values lazily, so it can be more memory-efficient for large sequences.

### 15. What is a Python generator, and how does it differ from a normal function? (V.Imp)

**Answer:**

A generator is a special iterator created by a function that uses `yield`.

A normal function generally executes and returns its result. A generator function returns a generator object; its body runs as values are requested.

```python
def generate_numbers(limit):
    for number in range(limit):
        yield number

for number in generate_numbers(3):
    print(number)
```

**Advantages:**
- Lazy evaluation.
- Lower memory usage for large sequences.
- Useful for streaming files, records, or API results.

### 16. Does Python support lazy loading with generators? (V.Imp)

**Answer:**

Yes. Generators support lazy evaluation: values are produced only when requested, rather than building the entire result in memory first.

```python
def read_numbers():
    for number in range(1_000_000):
        yield number

numbers = read_numbers()
print(next(numbers))  # 0
print(next(numbers))  # 1
```

This is useful for processing large files, database results, and data pipelines. A generator does not automatically make every operation lazy; the surrounding code and libraries must also avoid eagerly materializing all data.

### 17. What is the difference between `asyncio` and threading in Python? (V.Imp)

**Answer:**

Both can support concurrency, but they use different execution models.

| asyncio | threading |
|---|---|
| Usually uses a single event loop thread | Uses multiple OS threads |
| Tasks cooperate by awaiting | Threads are scheduled by the OS |
| Excellent for many async I/O operations | Useful for blocking I/O libraries |
| Requires async-compatible code | Can work with synchronous blocking code |
| Uses `async` and `await` | Uses `threading.Thread`, locks, etc. |

**Example of asyncio:**

```python
import asyncio

async def fetch_data():
    await asyncio.sleep(1)
    return "Done"

async def main():
    result = await fetch_data()
    print(result)

asyncio.run(main())
```

For CPU-heavy work, consider multiprocessing or native libraries that release the GIL. `asyncio` does not make CPU-bound Python code run in parallel by itself.

### 18. What is multithreading in Python and FastAPI? (V.Imp)

**Answer:**

Multithreading allows multiple threads to run within one process. It is useful for overlapping I/O operations such as network requests or file access.

FastAPI is based on ASGI and supports both asynchronous endpoints (`async def`) and normal synchronous endpoints (`def`). In common ASGI deployments, synchronous endpoints are run in a thread pool so they do not block the event loop. Exact execution behavior depends on the server and framework integration.

```python
from fastapi import FastAPI
import time

app = FastAPI()

@app.get("/sync")
def sync_endpoint():
    time.sleep(1)
    return {"message": "Completed"}
```

For async I/O, use async libraries and `await`. Do not call long blocking operations directly inside an `async def` endpoint, because that can block the event loop.

### 19. Can you explain the Global Interpreter Lock (GIL) in Python and its impact on multithreaded and multiprocessing applications? (V.Imp)

**Answer:**

In traditional GIL-enabled CPython builds, the Global Interpreter Lock is a mechanism that allows only one thread at a time to execute Python bytecode within a process.

**Impact:**
- **I/O-bound tasks:** Threads can still be useful because a thread waiting for I/O can release the GIL, allowing another thread to run.
- **CPU-bound Python tasks:** Threads generally do not provide parallel execution of Python bytecode in a standard GIL-enabled build.
- **Multiprocessing:** Separate processes have separate interpreters and can use multiple CPU cores, at the cost of process and data-transfer overhead.

**Version note:** CPython also has optional free-threaded builds in newer versions. Therefore, avoid claiming that every Python build always has a GIL; describe the interpreter/build being used.

### 20. Can you explain shallow copy and deep copy in Python? (V.Imp)

**Answer:**

A shallow copy creates a new outer object but keeps references to nested objects. A deep copy recursively copies nested objects too.

```python
import copy

original = [[1, 2], [3, 4]]

shallow = copy.copy(original)
deep = copy.deepcopy(original)

original[0].append(99)

print(shallow)  # [[1, 2, 99], [3, 4]]
print(deep)     # [[1, 2], [3, 4]]
```

Use shallow copy when sharing nested objects is acceptable. Use deep copy when nested mutable objects must be independent. Deep copying can be expensive and may not be appropriate for every object.

### 21. When do you use the `nonlocal` keyword for variables in Python? (V.Imp)

**Answer:**

Use `nonlocal` inside a nested function when you want to reassign a variable defined in the nearest enclosing function scope.

```python
def counter():
    count = 0

    def increment():
        nonlocal count
        count += 1
        return count

    return increment

next_count = counter()
print(next_count())  # 1
print(next_count())  # 2
```

Without `nonlocal`, assigning to `count` inside `increment()` would create a local variable and could cause an `UnboundLocalError` when trying to read it first.

### 22. What methods and practices are used to create an immutable object?

**Answer:**

Python does not provide a universal `final` or `immutable` keyword for arbitrary objects. Immutability is usually enforced through design.

Common approaches:
- Use immutable built-in types such as tuples, strings, and frozensets.
- Use `@dataclass(frozen=True)` for read-only dataclass fields.
- Avoid exposing mutable internal collections.
- Return copies or immutable views where appropriate.
- Validate that nested fields are immutable too.

```python
from dataclasses import dataclass

@dataclass(frozen=True)
class Employee:
    employee_id: int
    name: str

employee = Employee(1, "Arbaaz")
# employee.name = "Ali"  # FrozenInstanceError
```

A frozen dataclass prevents normal attribute reassignment, but deep immutability still depends on the field values.

---

## 2. Python Coding and Problem-Solving

### 23. How do you reverse a string in Python? (V.Imp)

**Answer:**

The simplest approach is slicing with a step of `-1`.

```python
def reverse_string(text):
    return text[::-1]

print(reverse_string("python"))  # nohtyp
```

**Complexity:** O(n) time and O(n) additional space for the resulting string.

### 24. How do you find duplicate elements in a list in Python? (V.Imp)

**Answer:**

Use a set to track values already seen and another set to collect duplicates.

```python
def find_duplicates(items):
    seen = set()
    duplicates = set()

    for item in items:
        if item in seen:
            duplicates.add(item)
        else:
            seen.add(item)

    return list(duplicates)

print(find_duplicates([1, 2, 3, 2, 4, 1]))
```

Output contains `1` and `2` (the order is not guaranteed because the result is a set).

**Complexity:** O(n) average time and O(n) extra space, assuming hashable elements.

### 25. How to implement a compose function that composes multiple functions?

**Answer:**

Function composition combines functions so that the output of one function becomes the input of the next. In the conventional mathematical `compose(f, g)`, the result is `f(g(x))`.

```python
def compose(*functions):
    def composed(value):
        for function in reversed(functions):
            value = function(value)
        return value
    return composed

def add_two(x):
    return x + 2

def multiply_three(x):
    return x * 3

combined = compose(add_two, multiply_three)
print(combined(4))  # 14: add_two(multiply_three(4))
```

The functions are applied from right to left. If you want left-to-right composition, iterate through `functions` without reversing them.

### 26. Identify and fix syntax errors in the provided Python code for this function. (V.Imp)

**Answer:**

The actual code is not included in the question, so the exact syntax errors cannot be identified without seeing the snippet.

When debugging, check:
1. Missing or extra colons after `if`, `for`, `while`, `def`, and `class`.
2. Mismatched parentheses, brackets, or quotation marks.
3. Incorrect indentation.
4. Missing commas between dictionary or function arguments.
5. Invalid assignment or comparison syntax.
6. Incorrect use of reserved keywords.

Share the original code to identify the exact errors and corrected version.

### 27. Can you explain the approach you are using to merge overlapping intervals? [Scenario Based]

**Answer:**

A common approach is sorting followed by a single pass.

1. Sort intervals by their start value.
2. Put the first interval in the result.
3. For each next interval, compare its start with the end of the last result interval.
4. If they overlap, merge by taking the maximum end.
5. Otherwise, append the new interval.

```python
def merge_intervals(intervals):
    if not intervals:
        return []

    intervals.sort(key=lambda interval: interval[0])
    merged = [intervals[0][:]]

    for start, end in intervals[1:]:
        if start <= merged[-1][1]:
            merged[-1][1] = max(merged[-1][1], end)
        else:
            merged.append([start, end])

    return merged

print(merge_intervals([[1, 3], [2, 6], [8, 10], [9, 12]]))
# [[1, 6], [8, 12]]
```

**Complexity:** O(n log n) time due to sorting and O(n) space for the output.

This example treats intervals that touch at an endpoint as overlapping (`start <= end`). Change the comparison to `<` if the problem defines them as non-overlapping.

### 28. How can you use a decorator to convert a function’s output to uppercase without modifying the original function? (V.Imp)

**Answer:**

Wrap the original function and convert its returned string to uppercase.

```python
from functools import wraps

def uppercase_output(func):
    @wraps(func)
    def wrapper(*args, **kwargs):
        result = func(*args, **kwargs)
        return result.upper()
    return wrapper

@uppercase_output
def get_message():
    return "hello world"

print(get_message())  # HELLO WORLD
```

This assumes the wrapped function returns a string. For other return types, add an explicit type check or conversion policy.

### 29. How would you return the most similar document for a given query from a list of documents? [Scenario Based]

**Answer:**

For a semantic search problem, I would:
1. Convert the query into an embedding.
2. Convert each document into an embedding, ideally once and cache/store it.
3. Calculate similarity between the query vector and document vectors.
4. Return the document with the highest similarity score.

For cosine similarity:

\[
\operatorname{cosine}(q,d)=
\frac{q \cdot d}{\|q\|\|d\|}
\]

**Simple Python example:**

```python
import numpy as np

def cosine_similarity(a, b):
    a = np.asarray(a, dtype=float)
    b = np.asarray(b, dtype=float)

    denominator = np.linalg.norm(a) * np.linalg.norm(b)
    if denominator == 0:
        return 0.0

    return float(np.dot(a, b) / denominator)

def most_similar_document(query_vector, documents):
    if not documents:
        return None

    return max(
        documents,
        key=lambda doc: cosine_similarity(query_vector, doc["embedding"])
    )

documents = [
    {"text": "Java programming", "embedding": [0.9, 0.1]},
    {"text": "Cooking recipes", "embedding": [0.1, 0.9]},
]

print(most_similar_document([0.8, 0.2], documents)["text"])
```

In production, use a consistent embedding model and a vector index such as Qdrant when the document collection is large. Similarity scores are model-dependent and should be evaluated with representative queries.

### 30. When would you override the `__new__` and `__init__` methods? [Scenario Based]

**Answer:**

Override `__new__()` when you need to control instance creation itself, especially for immutable built-in types, singleton-like patterns, or specialized object construction.

Override `__init__()` when you need to initialize instance attributes after the object has been created. This is the common case for normal classes.

```python
class User:
    def __init__(self, name):
        self.name = name
```

For an immutable type subclass, `__new__()` may be needed because the value must be created before initialization:

```python
class UpperString(str):
    def __new__(cls, value):
        return super().__new__(cls, value.upper())

print(UpperString("hello"))  # HELLO
```

Do not override `__new__()` unless object creation itself needs customization.

### 31. Can you explain the logic you used to count the number of ones in the range from 0 to N?

**Answer:**

One straightforward approach is to convert each number to a string and count the character `"1"`.

```python
def count_ones(n):
    total = 0
    for number in range(n + 1):
        total += str(number).count("1")
    return total

print(count_ones(13))  # 6
```

From 0 through 13, the digit `1` occurs in 1, 10, 11, 12, and 13; the number 11 contributes two occurrences.

This simple solution takes roughly O(N log N) character-processing time.

### 32. Can you use a mathematical approach instead of string conversion to count the number of ones in all numbers up to N?

**Answer:**

Yes. Count how many times digit `1` appears at each decimal position.

For each position, define:
- `higher = n // (position * 10)`
- `current = (n // position) % 10`
- `lower = n % position`

Then:
- If `current == 0`, contribution is `higher * position`.
- If `current == 1`, contribution is `higher * position + lower + 1`.
- If `current > 1`, contribution is `(higher + 1) * position`.

```python
def count_ones_math(n):
    if n < 0:
        return 0

    total = 0
    position = 1

    while position <= n:
        higher = n // (position * 10)
        current = (n // position) % 10
        lower = n % position

        if current == 0:
            total += higher * position
        elif current == 1:
            total += higher * position + lower + 1
        else:
            total += (higher + 1) * position

        position *= 10

    return total

print(count_ones_math(13))  # 6
```

**Complexity:** O(log N) time and O(1) extra space.

### 33. How would you process a very large log file line by line in Python without loading it all into memory? [Scenario Based] (V.Imp)

**Answer:**

Use file iteration. A file object is an iterator, so Python reads lines progressively instead of loading the entire file.

```python
def process_log_file(file_path):
    error_count = 0

    with open(file_path, "r", encoding="utf-8") as file:
        for line in file:
            if "ERROR" in line:
                error_count += 1
                process_error(line)

    return error_count

def process_error(line):
    print(line.rstrip())

print(process_log_file("application.log"))
```

For very large files:
- Process each line and discard it when no longer needed.
- Avoid `read()` or `readlines()` for the entire file.
- Use buffered I/O (the default file buffering is usually suitable).
- Handle malformed lines and decoding errors deliberately.
- If processing is CPU-heavy, consider batching and multiprocessing.

### 34. What causes a SyntaxError in dictionary definitions and how to fix it?

**Answer:**

A dictionary syntax error often comes from missing colons, missing commas, mismatched braces, or invalid key-value expressions.

Incorrect:

```python
# person = {"name" "Arbaaz", "age": 30}
```

Correct:

```python
person = {
    "name": "Arbaaz",
    "age": 30
}
```

Dictionary entries use the format `key: value`, and entries are separated by commas. Keys must be hashable.

### 35. Explain Python decorators with an example. (V.Imp)

**Answer:**

This is another version of Question 12. A decorator wraps a function to add behavior while keeping the original function body unchanged.

For example, a timing decorator:

```python
from functools import wraps
from time import perf_counter

def measure_time(func):
    @wraps(func)
    def wrapper(*args, **kwargs):
        start = perf_counter()
        try:
            return func(*args, **kwargs)
        finally:
            elapsed = perf_counter() - start
            print(f"{func.__name__} took {elapsed:.4f} seconds")
    return wrapper

@measure_time
def calculate():
    return sum(range(100_000))

calculate()
```

Use `functools.wraps` so introspection and debugging retain the wrapped function's metadata.

### 36. How would you flatten and rename nested dictionary keys in Python? [Scenario Based] (V.Imp)

**Answer:**

A recursive function can traverse nested dictionaries and join keys into a flattened key.

```python
def flatten_dict(data, parent_key="", separator="_"):
    flattened = {}

    for key, value in data.items():
        new_key = f"{parent_key}{separator}{key}" if parent_key else str(key)

        if isinstance(value, dict):
            flattened.update(flatten_dict(value, new_key, separator))
        else:
            flattened[new_key] = value

    return flattened

data = {
    "employee": {
        "name": "Arbaaz",
        "address": {
            "city": "Delhi"
        }
    }
}

print(flatten_dict(data))
```

Output:

```python
{
    "employee_name": "Arbaaz",
    "employee_address_city": "Delhi"
}
```

For production, define how lists, key collisions, and keys containing the separator should be handled.

### 37. Why does `AttributeError: 'dict' object has no attribute 'value'` occur, and how can it be fixed?

**Answer:**

It occurs when code tries to access a dictionary key as an object attribute.

Incorrect:

```python
employee = {"name": "Arbaaz"}
print(employee.value)
```

Correct dictionary access:

```python
print(employee["name"])
```

Or use `.get()` when the key may be missing:

```python
print(employee.get("name"))
```

If the value is actually a Pydantic model or custom object, attribute access may be correct. First inspect the actual type of the variable.

### 38. How would you group words by their starting character into a dictionary? [Scenario Based]

**Answer:**

Use a dictionary where each first character maps to a list of words.

```python
from collections import defaultdict

def group_by_first_character(words):
    groups = defaultdict(list)

    for word in words:
        if word:
            groups[word[0].lower()].append(word)

    return dict(groups)

words = ["apple", "ant", "banana", "ball", "Cat"]
print(group_by_first_character(words))
```

Output:

```python
{
    "a": ["apple", "ant"],
    "b": ["banana", "ball"],
    "c": ["Cat"]
}
```

This version is case-insensitive and skips empty strings.

### 39. How would you group items from a dictionary by their first letter? [Scenario Based]

**Answer:**

If the requirement is to group dictionary entries by the first letter of their keys, use `defaultdict`.

```python
from collections import defaultdict

def group_dict_by_key_initial(data):
    grouped = defaultdict(dict)

    for key, value in data.items():
        if key:
            grouped[str(key)[0].lower()][key] = value

    return dict(grouped)

data = {
    "apple": 10,
    "ant": 5,
    "banana": 20,
    "berry": 7
}

print(group_dict_by_key_initial(data))
```

This preserves each original key-value pair inside its group. If the interviewer instead means grouping the dictionary's values by their first letter, clarify the requirement and group the values.

### 40. Given a list of strings, group the anagrams together.

**Answer:**

Two words are anagrams if they contain the same characters with the same frequencies, possibly in a different order.

A simple key is the sorted characters of each word.

```python
from collections import defaultdict

def group_anagrams(words):
    groups = defaultdict(list)

    for word in words:
        key = tuple(sorted(word.lower()))
        groups[key].append(word)

    return list(groups.values())

words = ["eat", "tea", "tan", "ate", "nat", "bat"]
print(group_anagrams(words))
```

Possible output:

```python
[["eat", "tea", "ate"], ["tan", "nat"], ["bat"]]
```

For a word of length k, sorting costs O(k log k). For a fixed alphabet, a character-frequency tuple can be used as the key instead.

### 41. Write a program to check if a number is a palindrome.

**Answer:**

A palindrome reads the same forward and backward.

```python
def is_palindrome(number):
    if number < 0:
        return False

    original = number
    reversed_number = 0

    while number > 0:
        digit = number % 10
        reversed_number = reversed_number * 10 + digit
        number //= 10

    return original == reversed_number

print(is_palindrome(121))  # True
print(is_palindrome(123))  # False
```

This uses arithmetic rather than converting the number to a string.

### 42. Write a function that, given an array of integers, returns the smallest positive integer missing from the array.

**Answer:**

The smallest missing positive integer must be between 1 and n+1 for an array of length n.

Use an in-place placement strategy: place each value x in position x-1 when x is in the range 1..n.

```python
def first_missing_positive(nums):
    n = len(nums)

    for i in range(n):
        while 1 <= nums[i] <= n and nums[nums[i] - 1] != nums[i]:
            correct_index = nums[i] - 1
            nums[i], nums[correct_index] = nums[correct_index], nums[i]

    for i in range(n):
        if nums[i] != i + 1:
            return i + 1

    return n + 1

print(first_missing_positive([3, 4, -1, 1]))  # 2
```

**Complexity:** O(n) time and O(1) extra space, excluding the input array. The input list is modified.

### 43. Write a program to find the second highest salary from a list of employee salaries.

**Answer:**

If “second highest” means the second distinct salary, remove duplicates and sort.

```python
def second_highest_salary(salaries):
    unique_salaries = sorted(set(salaries), reverse=True)

    if len(unique_salaries) < 2:
        return None

    return unique_salaries[1]

print(second_highest_salary([50000, 70000, 60000, 70000]))
# 60000
```

For a large list, a one-pass solution can track the highest and second-highest distinct values without sorting.

### 44. Write a Python function that returns "hello world" in lowercase. (V.Imp)

**Answer:**

```python
def get_message():
    return "HELLO WORLD".lower()

print(get_message())  # hello world
```

If the input is already known to be lowercase, simply return `"hello world"`.

### 45. What is the output of the provided Python/MySQL code? (V.Imp)

**Answer:**

The Python/MySQL code is not included, so its exact output cannot be determined from the question alone.

To solve it, inspect:
1. The SQL query and table data.
2. The database engine's behavior and column types.
3. The Python connector's return format.
4. Whether results are fetched with `fetchone()`, `fetchall()`, or an iterator.
5. Any Python transformations, loops, or print statements.

Share the code and sample table rows to determine the exact output.

### 46. What should replace XX and YY to produce the shown Python output? (V.Imp)

**Answer:**

The code containing `XX` and `YY`, along with the expected output, is missing. The correct replacements cannot be determined without those details.

In an interview, first identify the required output, then trace the expression's types, operator precedence, scope, and evaluation order. Share the original snippet and output for an exact solution.

### 47. What would be your approach to completing the coding solution?

**Answer:**

I follow a structured problem-solving process:

1. Clarify the input, output, and constraints.
2. Work through a small example manually.
3. Identify edge cases.
4. Choose a suitable data structure and algorithm.
5. Explain the approach before coding.
6. Write readable, modular code.
7. Test normal, boundary, and invalid inputs.
8. Analyze time and space complexity.
9. Improve the solution if constraints require it.

If a requirement is ambiguous, ask a focused clarification rather than making an unsafe assumption.

---

## 3. Python Data Processing, Pandas and Performance

### 48. How would you redesign a heavily used Python UDF to use built-in expressions and columnar execution? [Scenario Based] (V.Imp)

**Answer:**

This question commonly applies to Spark or another distributed data-processing engine. A Python UDF can be slower because data may need to cross the Python/JVM boundary and may not benefit from the engine's expression optimizer.

I would:
1. Inspect what the UDF calculates.
2. Check whether the logic can be expressed using built-in functions.
3. Replace row-by-row Python logic with native column expressions.
4. Use vectorized operations where supported if native expressions are not sufficient.
5. Avoid unnecessary data conversions and repeated actions.
6. Compare execution plans and benchmark on representative data.

**Illustrative PySpark example:**

Instead of a Python UDF that uppercases a column:

```python
from pyspark.sql import functions as F

df = df.withColumn("name_upper", F.upper(F.col("name")))
```

Built-in expressions can be optimized by Spark's execution engine. The exact best replacement depends on the original UDF and its semantics.

### 49. How would you use Pandas to find the employee or employees with the highest salary? [Scenario Based]

**Answer:**

Use `max()` to find the highest salary, then filter all rows with that value. This returns every employee tied for the highest salary.

```python
import pandas as pd

df = pd.DataFrame({
    "name": ["Amit", "Sara", "John", "Neha"],
    "salary": [50000, 80000, 70000, 80000]
})

highest_salary = df["salary"].max()
top_employees = df[df["salary"] == highest_salary]

print(top_employees)
```

To return only one employee, use `idxmax()`, but note that it returns the first occurrence of the maximum.

### 50. Write a Python function to impute missing values in a Pandas DataFrame using median for numerical columns and mode for categorical columns. (V.Imp)

**Answer:**

Use the median for numerical columns and the mode for categorical or other non-numeric columns.

```python
import pandas as pd

def impute_missing_values(df):
    result = df.copy()

    for column in result.columns:
        if result[column].isna().all():
            # No value is available to calculate a median or mode.
            continue

        if pd.api.types.is_numeric_dtype(result[column]):
            fill_value = result[column].median()
        else:
            modes = result[column].mode(dropna=True)
            if modes.empty:
                continue
            fill_value = modes.iloc[0]

        result[column] = result[column].fillna(fill_value)

    return result
```

**Important considerations:**
- This function returns a copy and does not modify the original DataFrame.
- For an all-null column, median or mode cannot be calculated, so it remains null.
- For categorical columns with multiple modes, this example selects the first mode.
- In machine-learning workflows, calculate imputation values on the training set only, then apply the same values to validation/test data to avoid data leakage.

### 51. How do you optimize Python code handling large document processing? [Scenario Based] (V.Imp)

**Answer:**

I optimize the pipeline by reducing memory use, avoiding repeated work, and controlling concurrency.

1. **Streaming:** Read large files in chunks or line by line where possible.
2. **Efficient parsing:** Use suitable PDF, DOCX, or HTML parsers and avoid unnecessary conversions.
3. **Batching:** Process documents in manageable batches.
4. **Parallelism:** Use concurrency for I/O-bound tasks and multiprocessing or native code for CPU-bound work when appropriate.
5. **Caching:** Cache reusable extraction or embedding results using safe keys.
6. **Database batching:** Use bulk inserts/upserts rather than one request per chunk.
7. **Embedding optimization:** Batch embedding requests, respect provider limits, and retry transient errors.
8. **Profiling:** Measure memory, CPU, latency, and throughput before changing code.

For a RAG ingestion pipeline, I would also track document IDs and content hashes so unchanged documents do not need to be processed again.

### 52. How do you optimize Python code handling large document processing? [Scenario Based] (V.Imp)

**Answer:**

This repeats Question 51. The core approach is to stream input, batch expensive operations, avoid duplicate extraction/embedding, profile bottlenecks, and use suitable concurrency.

For document-to-RAG processing, a practical pipeline is:

```text
Upload
  |
Validate File
  |
Extract Text
  |
Chunk in Batches
  |
Generate Embeddings in Batches
  |
Bulk Upsert to Vector Store
  |
Record Processing Status
```

Add retry handling, idempotency, file-size limits, and monitoring so a failed document can be reprocessed without duplicating all previously completed work.

---

## 4. APIs, React and System Design

### 53. How do you use the map function in JavaScript to render multiple items in a list? (Imp)

**Answer:**

JavaScript's `map()` creates a new array by applying a function to each item. In React, it is commonly used to render a list of components.

```jsx
function EmployeeList() {
  const employees = [
    { id: 1, name: "Amit" },
    { id: 2, name: "Sara" },
    { id: 3, name: "John" }
  ];

  return (
    <ul>
      {employees.map((employee) => (
        <li key={employee.id}>{employee.name}</li>
      ))}
    </ul>
  );
}
```

The `key` prop helps React identify list items efficiently. Use a stable unique identifier rather than an array index when the list can change.

### 54. Can you create a React component that fetches tasks from a URL, displays them in a list, and shows a loading state? (Imp)

**Answer:**

Use `useEffect` to fetch data when the component mounts, and `useState` to track loading, data, and errors.

```jsx
import { useEffect, useState } from "react";

function TaskList() {
  const [tasks, setTasks] = useState([]);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState("");

  useEffect(() => {
    const controller = new AbortController();

    async function fetchTasks() {
      try {
        const response = await fetch("/api/tasks", {
          signal: controller.signal
        });

        if (!response.ok) {
          throw new Error("Failed to fetch tasks");
        }

        const data = await response.json();
        setTasks(data);
      } catch (err) {
        if (err.name !== "AbortError") {
          setError(err.message);
        }
      } finally {
        if (!controller.signal.aborted) {
          setLoading(false);
        }
      }
    }

    fetchTasks();
    return () => controller.abort();
  }, []);

  if (loading) return <p>Loading tasks...</p>;
  if (error) return <p role="alert">{error}</p>;

  return (
    <ul>
      {tasks.map((task) => (
        <li key={task.id}>{task.title}</li>
      ))}
    </ul>
  );
}

export default TaskList;
```

The abort controller prevents an outdated request from updating state after the component unmounts.

### 55. How would you design a Python API to handle time-consuming video processing tasks? [Scenario Based] (V.Imp)

**Answer:**

I would use an asynchronous job-processing architecture rather than keeping the HTTP request open until video processing finishes.

**Architecture:**

```text
Client
  |
FastAPI
  |
Validate Request and Store Video
  |
Create Job Record
  |
Queue (Celery / RQ / Similar)
  |
Worker Processes Video
  |
Store Output and Job Status
  |
Client Polls Status or Receives Callback
```

**API design:**
- `POST /videos`: Upload or register a video and create a job.
- `GET /jobs/{job_id}`: Return queued, processing, completed, or failed status.
- `GET /jobs/{job_id}/result`: Return the result when available.

**Important practices:**
- Store large videos in object storage rather than passing them through the queue.
- Queue only the job ID and required metadata.
- Validate file type, size, and authorization.
- Make job processing idempotent.
- Track progress and errors.
- Limit worker concurrency based on CPU/GPU and memory.
- Secure temporary files and clean them up.
- Use retries carefully to avoid duplicate outputs.

### 56. How would you design a Python module for managing a restaurant ordering system? [Scenario Based] (V.Imp)

**Answer:**

I would separate the system into clear models and services.

**Core entities:**
- `MenuItem`: ID, name, price, availability.
- `OrderItem`: Menu item, quantity, item price.
- `Order`: ID, customer, items, status, total.
- `Customer`: Customer details.
- `Payment`: Amount, payment status, transaction reference.

**Example structure:**

```text
restaurant/
  models.py
  menu_service.py
  order_service.py
  payment_service.py
  repository.py
  api.py
  tests/
```

**Simple implementation:**

```python
from dataclasses import dataclass, field
from decimal import Decimal

@dataclass
class MenuItem:
    item_id: int
    name: str
    price: Decimal
    available: bool = True

@dataclass
class OrderLine:
    item: MenuItem
    quantity: int

@dataclass
class Order:
    order_id: int
    items: list[OrderLine] = field(default_factory=list)

    def total(self):
        return sum(
            (line.item.price * line.quantity for line in self.items),
            start=Decimal("0.00")
        )

    def add_item(self, item, quantity):
        if not item.available:
            raise ValueError("Item is not available")
        if quantity <= 0:
            raise ValueError("Quantity must be positive")
        self.items.append(OrderLine(item, quantity))
```

In production, validate prices and availability on the server, use database transactions, and integrate payment processing through a secure provider. Do not trust prices sent by the client.

### 57. Can you walk me through a time when you designed and built a production-quality backend service using a Python framework such as FastAPI or Flask? [Scenario Based] (V.Imp)

**Answer:**

Use the following structure and replace the example details with your actual project experience.

**Situation:** “We needed a backend service to expose application functionality through secure APIs.”

**Task:** “I was responsible for designing endpoints, validating input, integrating the database, and making the service reliable.”

**Action:**
1. Designed REST endpoints using FastAPI.
2. Defined request and response schemas using Pydantic.
3. Implemented service and repository layers.
4. Integrated the database using appropriate transactions.
5. Added JWT authentication and role-based authorization.
6. Added structured logging, exception handling, and validation.
7. Wrote unit and integration tests.
8. Containerized and deployed the service with environment-based configuration.
9. Added health checks and monitoring.

**Result:** Explain measurable outcomes that are true for your project, such as reduced response time, fewer invalid requests, improved reliability, or easier maintenance.

Do not invent production metrics or claim tools you did not use. Interviewers may ask you to explain the architecture, database schema, security decisions, deployment, and a difficult bug you solved.

### 58. Which programming language do you prefer: Python or Java? (V.Imp)

**Answer:**

A balanced answer is:

“I am comfortable with both Python and Java. I prefer Java for large enterprise backend applications where strong typing, mature tooling, and structured object-oriented design are important. I prefer Python for AI/ML integration, automation, data processing, and rapid prototyping because of its ecosystem and concise syntax. I choose the language based on project requirements rather than saying one is universally better.”

If the role is primarily Python-focused, emphasize your Python experience and give a concrete example.

---

## 5. Additional Coding and Interview Questions

### 59. How would you group words by their starting character into a dictionary? [Scenario Based]

**Answer:**

This is the same problem as Question 38. Use a dictionary of lists, with the first character as the key.

```python
from collections import defaultdict

def group_words(words):
    result = defaultdict(list)

    for word in words:
        if word:
            result[word[0].lower()].append(word)

    return dict(result)
```

### 60. How do you find the employee or employees with the highest salary using Pandas? [Scenario Based]

**Answer:**

This repeats Question 49. Use the maximum salary and filter rows equal to that value:

```python
max_salary = df["salary"].max()
result = df.loc[df["salary"].eq(max_salary)]
```

This returns all employees tied for the highest salary.

### 61. How comfortable are you with Python on a scale of 1 to 10? (V.Imp)

**Answer:**

This is a self-assessment question. Give an honest rating and support it with evidence.

Example answer:

“I would rate myself 7 out of 10. I am comfortable with Python fundamentals, functions, OOP, decorators, generators, exception handling, and building APIs with FastAPI. I have also worked with data processing and AI-related libraries. I am continuing to improve my knowledge of advanced concurrency, performance optimization, and Python internals.”

Adjust the rating and topics to match your actual ability. Be prepared for the interviewer to test the areas you mention.

### 62. How would you approach completing the coding solution?

**Answer:**

This is a general problem-solving question. Explain your process:
1. Understand the requirements and constraints.
2. Clarify ambiguous input or output.
3. Identify edge cases.
4. Select the right data structure.
5. Explain the algorithm.
6. Implement a readable solution.
7. Test examples and boundary cases.
8. Analyze time and space complexity.

If you cannot solve it immediately, explain your reasoning and work through a smaller example.

### 63. How would you write a Python decorator that adds retry logic? [Scenario Based] (V.Imp)

**Answer:**

A retry decorator can call a function again when it raises a configured exception. It should have a maximum retry count and a delay between attempts.

```python
import time
from functools import wraps

def retry(max_attempts=3, delay=1, exceptions=(Exception,)):
    def decorator(func):
        @wraps(func)
        def wrapper(*args, **kwargs):
            for attempt in range(1, max_attempts + 1):
                try:
                    return func(*args, **kwargs)
                except exceptions:
                    if attempt == max_attempts:
                        raise
                    time.sleep(delay)
        return wrapper
    return decorator

@retry(max_attempts=3, delay=1, exceptions=(TimeoutError,))
def call_service():
    print("Calling service")
    raise TimeoutError("Service timed out")

# call_service()
```

**Production considerations:**
- Retry only transient failures.
- Use exponential backoff and jitter.
- Set an overall timeout.
- Avoid retrying non-idempotent operations unless they use idempotency protection.
- Log each attempt.
- Consider a circuit breaker when a dependency remains unavailable.

---

## Quick Interview Preparation Tips

1. Learn Python fundamentals first: data types, mutability, scope, functions, OOP, and exceptions.
2. Understand decorators, generators, context managers, and `asyncio` with small runnable examples.
3. Be able to explain the GIL accurately, including the difference between threading and multiprocessing.
4. Practice coding questions with edge cases and time/space complexity.
5. For Pandas and large-data questions, explain memory usage, vectorization, batching, and data leakage.
6. For FastAPI scenarios, cover validation, authentication, authorization, database transactions, background jobs, and observability.
7. The original questions 23, 40, and 41 refer to code or output that was not supplied. Exact answers require the missing snippets or expected output.
8. Several questions repeat the same concept. The repeated entries are retained for reference and point back to the main explanation.
