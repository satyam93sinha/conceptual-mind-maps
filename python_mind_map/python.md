# Python Mind Map

## 1. Python Basics
- **Variables**
  - Variables store data values and are dynamically typed in Python.
  - **Example:**
    ```python
    x = 10  # integer
    y = "Hello"  # string
    z = 3.14  # float
    ```
  - No explicit declaration of types is required.
- **Data Types**
  - **Primitive Types**: int, float, str, bool
  - **Collection Types**: list, tuple, dict, set
  - **NoneType**: Represents the absence of value.
  - **Example:**
    ```python
    my_list = [1, 2, 3]  # list
    my_tuple = (1, 2, 3)  # tuple
    my_dict = {"key": "value"}  # dictionary
    my_set = {1, 2, 3}  # set
    ```

## 2. Control Flow
- **Conditionals**
  - Use `if`, `elif`, and `else` to execute code based on conditions.
  - **Example:**
    ```python
    x = 10
    if x > 5:
        print("x is greater than 5")
    elif x == 5:
        print("x is equal to 5")
    else:
        print("x is less than 5")
    ```
- **Loops**
  - `for`: Iterates over a sequence.
  - `while`: Repeats as long as the condition is `True`.
  - **Example:**
    ```python
    for i in range(5):
        print(i)
    while x > 0:
        print(x)
        x -= 1
    ```
- **Comprehensions**
  - A concise way to create lists, dictionaries, or sets.
  - **Example:**
    ```python
    squares = [x**2 for x in range(10)]
    ```

## 3. Functions
- **Definition**
  - Defined using the `def` keyword.
  - **Example:**
    ```python
    def greet(name):
        return f"Hello, {name}"
    ```
- **Function Arguments**
  - Positional, keyword, default values, `*args`, `**kwargs`.
  - **Example:**
    ```python
    def example(a, b=2, *args, **kwargs):
        print(a, b, args, kwargs)
    ```
- **Anonymous Functions**
  - `lambda` functions are short, inline functions.
  - **Example:**
    ```python
    square = lambda x: x**2
    ```
- **Decorators**
  - Used to modify the behavior of functions.
  - **Example:**
    ```python
    def decorator(func):
        def wrapper():
            print("Before")
            func()
            print("After")
        return wrapper

    @decorator
    def say_hello():
        print("Hello")
    ```
- **Recursion**
  - Functions that call themselves.
  - **Example:**
    ```python
    def factorial(n):
        if n == 1:
            return 1
        return n * factorial(n-1)
    ```

## 4. Object-Oriented Programming (OOP)
- **Classes**
  - **Definition:**
    ```python
    class Person:
        def __init__(self, name, age):
            self.name = name
            self.age = age
    ```
- **Inheritance**
  - Enables classes to inherit attributes and methods from other classes.
  - **Example:**
    ```python
    class Parent:
        def greet(self):
            print("Hello from Parent")

    class Child(Parent):
        pass
    ```
- **Polymorphism**
  - **Overriding:**
    ```python
    class Parent:
        def greet(self):
            print("Hello from Parent")

    class Child(Parent):
        def greet(self):
            print("Hello from Child")
    ```
- **Encapsulation**
  - Control access using private and public attributes (`_` and `__`).
- **Special Methods**
  - Define custom behavior for operators (`__add__`, `__str__`).
  - **Example:**
    ```python
    class MyClass:
        def __str__(self):
            return "Custom String"
    ```

## 5. Modules and Packages
- **Modules**
  - Python files (`.py`) containing functions, classes, or variables.
  - **Example:**
    ```python
    import math
    print(math.sqrt(16))
    ```
- **Packages**
  - Directories containing multiple modules and an `__init__.py` file.
  - **Example:**
    ```
    mypackage/
        __init__.py
        module1.py
        module2.py
    ```

## 6. Data Structures
- **Lists**
  - Ordered, mutable collections.
  - Methods: `append()`, `pop()`, `extend()`, `remove()`.
- **Tuples**
  - Ordered, immutable collections.
- **Dictionaries**
  - Key-value pairs, mutable.
  - Methods: `get()`, `keys()`, `values()`, `items()`.
- **Sets**
  - Unordered collections of unique elements.
  - Operations: union, intersection, difference.

## 7. Exception Handling
- **Syntax**
  ```python
  try:
      # code that may raise an exception
  except ExceptionType:
      # handle exception
  finally:
      # cleanup code