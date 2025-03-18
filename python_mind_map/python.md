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

    adults = list(filter(lambda person: person['age'] >= 18, people))
    print(adults)

    fruits = ["apple", "banana", "cherry", "date"]
    fruits_len_grt_5 = list(filter(lambda fruit: len(fruit) > 5, fruits))
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
- **Closure**
  - Nested functions can access the outer scope of the enclosing function - Closure.
  - A function that remembers the environment it was created in, even after the environment is no longer active.
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
  ```
- ***Exception Chanining***
  ```python
  try:
    v = {}['a']
  except KeyError as e:
      raise ValueError('failed') from e
  
  Output: from e can also be skipped
  Traceback (most recent call last):
  File "t.py", line 2, in <module>
    v = {}['a']
  KeyError: 'a'

  The above exception was the direct cause of the following exception:

  Traceback (most recent call last):
    File "t.py", line 4, in <module>
      raise ValueError('failed') from e
  ValueError: failed
  ```

## 8. Garbage Collection
- **Reference Counting**
  - keeps track of an object's reference
  - reference is incremented when object is used, decremented on reference removal
  - object is garbage collected if reference count decrements to 0
- **Generational Garbage Collection**
  - Garbage collects cyclic referenced objects
  - Gen-0 (younger): Python v3.13 - threshold: 2000 objects to begin collection process.
  - Mostly, objects are cleaned from Gen-0.
  - Gen-1 (adult): Python v3.13 - threshold: 10 objects to begin collection
  - Gen-2 (older): Python v3.13 - threshold: 10 objects
  - Garbage collector triggers collection process when threshold is reached in each Generation. The object surviving this collection is moved to older generation.

  ```python
  import gc
  gc.get_threshold()
  > 2000, 10, 10

  gc.get_count()  # gives current count of objects in each generation
  gc.collect()  # collects objects

  gc.set_threshold(<GEN-0 threshold>, <GEN-1 threshold>, <GEN-2 threshold>)
  # sets threshold values for 0, 1, 2 generations.
  ```
## 9. Custom Context Managers
- **Class-Based**
```python
  class FileManager:
    def __init__(self, filename, mode):
        self.filename = filename
        self.mode = mode
        self.file = None

    def __enter__(self):
        self.file = open(self.filename, self.mode)
        return self.file

    def __exit__(self, exc_type, exc_value, traceback):
        if self.file:
            self.file.close()


  # Example usage
  if __name__ == "__main__":
    with FileManager("example.txt", "w") as file:
        file.write("Hello, world!\n")
  
  import time


  class Timer:
    def __enter__(self):
        self.start_time = time.time()
        return self

    def __exit__(self, exc_type, exc_value, traceback):
        self.end_time = time.time()
        elapsed_time = self.end_time - self.start_time
        print(f"Elapsed time: {elapsed_time} seconds")


  # Example usage
  if __name__ == "__main__":
    with Timer() as timer:
        # Code block to measure the execution time
        time.sleep(2)  # Simulate some time-consuming operation
  Elapsed time: 2.002082347869873 seconds
  ```
- **Function-Based**
  - The @contextmanager decorator transforms the timer function into a context manager. Inside the function, start_time is captured, and the yield statement pauses execution, allowing code within the with block to run.
  Finally, __exit__ functionality is achieved by capturing the end time and printing the elapsed time.
  Essentially, you write the logic for the __enter__ before the yield keyword whereas the logic for __exit__ comes after. Both approaches achieve the same outcome, but the choice depends on your preference for structure and readability.
  ```python
  import time
  from contextlib import contextmanager


  @contextmanager
  def timer():
    start_time = time.time()
    yield
    end_time = time.time()
    elapsed_time = end_time - start_time
    print(f"Elapsed time: {elapsed_time} seconds")


  # Example usage
  if __name__ == "__main__":
    with timer():
        time.sleep(2)
  Elapsed time: 2.0020740032196045 seconds
  ```
## 10. __new__ vs __init__
  - __new__ is a static method, while __init__ is an instance method.
  - __new__ is responsible for creating and returning a new instance, while __init__ is responsible for initializing the attributes of the newly created object.
  - __new__ is called before __init__.
  - __new__ happens first, then __init__.
  - __new__ can return any object, while __init__ must return None.
  - **Syntax**
    ```python
    class Person:
    def __new__(cls, name, age):
        print("Creating a new Person object")
        instance = super().__new__(cls)
        return instance

    def __init__(self, name, age):
        print("Initializing the Person object")
        self.name = name
        self.age = age

    person = Person("John Doe", 30)
    print(f"Person's name: {person.name}, age: {person.age}")

    # Creating a new Person object
    # Initializing the Person object
    # Person's name: John Doe, age: 30
    ```


References:
1. [CustomContextManagers] https://www.datacamp.com/tutorial/writing-custom-context-managers-in-python
2. [__new__vs__init__] https://builtin.com/data-science/new-python 