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
  - Dynamic Typing: No explicit declaration of types is required.
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
- **Switch/Match case**
  - Use `match` and `case`
  - **Example:**
  ```python
  x = 10
  match x:
    case 10:
      print(10)
    case 15:
      print(15)
    case _:
      print("default")
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
  - Every function returns None by default.
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
    print(square(2))
    ```
  - `filter` functions
  - **Example:**
    ```python
    people = [
    {'name': 'Alice', 'age': 30},
    {'name': 'Bob', 'age': 25},
    {'name': 'Charlie', 'age': 35}
    ]

    adults = list(functools.filter(lambda person: person['age'] >= 18, people))
    print(adults)

    fruits = ["apple", "banana", "cherry", "date"]
    fruits_len_grt_5 = list(functools.filter(lambda fruit: len(fruit) > 5, fruits))
    print(fruits_len_grt_5)  # Output: ['banana', 'cherry']
    ```
  -  `reduce` functions
  - **Example:**
    ```python
    from functools import reduce

    numbers = [1, 2, 3, 4, 5, 6]
    product_of_even_numbers = reduce(lambda x, y: x * y if y % 2 == 0 else x, numbers, 1)
    print(product_of_even_numbers)  # Output: 48

    strings = ["apple", "banana", "cherry", "date"]
    longest_string = reduce(lambda x, y: x if len(x) > len(y) else y, strings)
    print(longest_string)  # Output: 'banana'

    lists = [[1, 2], [3, 4], [5, 6]]
    flattened_list = reduce(lambda x, y: x + y, lists)
    print(flattened_list)  # Output: [1, 2, 3, 4, 5, 6]
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
  - Private attributes are stored through name mangling thus, weak Encapsulation.
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