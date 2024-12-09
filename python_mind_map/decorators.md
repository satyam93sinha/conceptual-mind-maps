# Python Decorators Mind Map

## 1. **What Are Decorators?**
   - **Definition**: Functions that modify the behavior of another function or class.
   - **Purpose**:
     - Add functionality to existing code without modifying it directly.
     - Maintain cleaner, more modular code.
   - **Basic Syntax**:
     ```python
     @decorator_function
     def my_function():
         pass
     ```
     Equivalent to:
     ```python
     def my_function():
         pass
     my_function = decorator_function(my_function)
     ```

## 2. **How Decorators Work**
   - **Higher-Order Functions**:
     - A decorator takes a function as input, wraps it in additional functionality, and returns it.
       ```python
       def decorator(func):
           def wrapper():
               print("Before the function is called")
               func()
               print("After the function is called")
           return wrapper

       @decorator
       def say_hello():
           print("Hello!")

       say_hello()
       ```
       **Output**:
       ```
       Before the function is called
       Hello!
       After the function is called
       ```

   - **Internal Working**:
     - When a decorated function is called, it executes the `wrapper` function created inside the decorator. This `wrapper` function waits to be executed.
     - `@decorator` is syntactic sugar for function transformation.

## 3. **Functionality Added by Decorators**
   - **Logging**:
     ```python
     def log(func):
         def wrapper(*args, **kwargs):
             print(f"Function {func.__name__} is called")
             return func(*args, **kwargs)
         return wrapper

     @log
     def add(a, b):
         return a + b
     ```
   - **Access Control** (e.g., authentication):
     ```python
     def requires_authentication(func):
         def wrapper(user):
             if not user.get("authenticated", False):
                 raise PermissionError("User not authenticated")
             return func(user)
         return wrapper

     @requires_authentication
     def view_profile(user):
         print("Profile details")
     ```

   - **Timing Functions**:
     ```python
     import time

     def timer(func):
         def wrapper(*args, **kwargs):
             start = time.time()
             result = func(*args, **kwargs)
             end = time.time()
             print(f"Execution time: {end - start:.2f} seconds")
             return result
         return wrapper

     @timer
     def slow_function():
         time.sleep(2)
         print("Finished!")

     slow_function()
     ```

## 4. **Decorator with Arguments**
   - **When Decorators Need Parameters**:
     - Use a nested function to pass arguments to the decorator.
       ```python
       def repeat(num):
           def decorator(func):
               def wrapper(*args, **kwargs):
                   for _ in range(num):
                       func(*args, **kwargs)
               return wrapper
           return decorator

       @repeat(3)
       def say_hello():
           print("Hello!")
       ```

## 5. **Built-in Decorators**
   - **`@staticmethod`**:
     - Defines a method that doesn’t depend on instance attributes.
       ```python
       class MyClass:
           @staticmethod
           def greet():
               print("Hello, world!")
       ```
   - **`@classmethod`**:
     - A method that receives the class as the first argument (`cls`).
       ```python
       class MyClass:
           @classmethod
           def create_instance(cls):
               return cls()
       ```

   - **`@property`**:
     - Converts a method into a readable attribute.
       ```python
       class Circle:
           def __init__(self, radius):
               self.radius = radius

           @property
           def area(self):
               return 3.14 * self.radius ** 2
       ```

## 6. **Chaining Decorators**
   - Apply multiple decorators to a single function.
     ```python
     @decorator1
     @decorator2
     def my_function():
         pass
     ```
     - Execution order: `decorator2` wraps `my_function` first, followed by `decorator1`.

## 7. **Common Use Cases**
   - **Memoization**:
     - Store results of expensive function calls for re-use.
       ```python
       def memoize(func):
           cache = {}

           def wrapper(*args):
               if args not in cache:
                   cache[args] = func(*args)
               return cache[args]
           return wrapper

       @memoize
       def fib(n):
           if n <= 1:
               return n
           return fib(n - 1) + fib(n - 2)
       ```

   - **Validation**:
     ```python
     def validate_positive(func):
         def wrapper(x):
             if x < 0:
                 raise ValueError("Argument must be positive")
             return func(x)
         return wrapper

     @validate_positive
     def square_root(x):
         return x ** 0.5
     ```

## 8. **Tricky Concepts**
   - **Decorators on Methods**:
     - Decorators need to handle `self` for instance methods.
   - **Preserving Metadata**:
     - Wrapping functions with decorators can lose function metadata like the name.
       - Use `functools.wraps` to preserve it.
         ```python
         import functools

         def decorator(func):
             @functools.wraps(func)
             def wrapper(*args, **kwargs):
                 return func(*args, **kwargs)
             return wrapper
         ```

   - **Order of Execution**:
     - Multiple decorators execute in the order they are applied (from bottom to top).

## 9. **Internal Workings**
   - **Closures**:
     - Decorators rely heavily on closures (functions inside functions).
   - **Function Objects**:
     - Python treats functions as first-class objects, enabling them to be passed as arguments.

---

### Links:
- [DataCamp: Decorators in Python](https://www.datacamp.com/tutorial/decorators-python)
- [FreeCodeCamp: The Python Decorator Handbook](https://www.freecodecamp.org/news/the-python-decorator-handbook/)

---

This mind map outlines everything you need to know about decorators, from syntax and use cases to advanced concepts and internal workings. Let me know if you need further clarification!