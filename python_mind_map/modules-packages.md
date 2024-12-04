# Python Modules Mind Map

## 1. **What Are Modules?**
   - **Definition**: A module is a file containing Python definitions and statements.
     - Example: `example.py` can be a module if it contains reusable code.
   - **Purpose**:
     - Organize Python code logically.
     - Reuse functionality across scripts and projects.
   - **How to Use**:
     - Import with `import module_name`.
       ```python
       import example
       example.function()
       ```

## 2. **Creating and Using Modules**
   - **Custom Module**:
     - Create a `.py` file with functions, variables, or classes.
     - Import it to another script.
       ```python
       # example.py
       def greet(name):
           return f"Hello, {name}!"

       # main.py
       import example
       print(example.greet("Alice"))
       ```

   - **Standard Library**:
     - Python includes a rich library of built-in modules (e.g., `os`, `math`).
       ```python
       import math
       print(math.sqrt(16))  # Outputs 4.0
       ```

   - **Accessing Module Contents**:
     - Use `module_name.attribute` to access variables, functions, or classes.
       ```python
       import math
       print(math.pi)  # Outputs 3.141592653589793
       ```

## 3. **Namespaces and `import` Statement**
   - **Namespaces**:
     - Each module has its own namespace, isolating variables from other modules.
     - Example:
       ```python
       # module1.py
       value = 42

       # module2.py
       value = 7

       # main.py
       import module1, module2
       print(module1.value)  # Outputs 42
       print(module2.value)  # Outputs 7
       ```

   - **Import Variants**:
     - `import module`: Imports the entire module.
     - `from module import name`: Imports specific attributes.
       ```python
       from math import sqrt
       print(sqrt(25))  # Outputs 5.0
       ```
     - `from module import *`: Imports all attributes (not recommended for large modules).
     - Aliasing with `as`:
       ```python
       import math as m
       print(m.pi)
       ```

   - **Reloading Modules**:
     - Use `importlib.reload(module)` to reload a modified module without restarting Python.

## 4. **Module Search Path**
   - **How Python Locates Modules**:
     - Searches directories listed in `sys.path`.
       - Current directory.
       - Standard library directories.
       - Directories in `PYTHONPATH` environment variable.
     ```python
     import sys
     print(sys.path)
     ```

   - **Custom Paths**:
     - Add a directory to `sys.path` dynamically.
       ```python
       import sys
       sys.path.append("/path/to/directory")
       ```

## 5. **Packages**
   - **Definition**: A package is a collection of modules in a directory containing an `__init__.py` file.
   - **Structure**:
     ```
     mypackage/
       __init__.py
       module1.py
       module2.py
     ```
   - **Using Packages**:
     - Import modules from a package:
       ```python
       from mypackage import module1
       ```

   - **Relative Imports**:
     - Use dots to refer to parent/sibling modules.
       ```python
       from . import sibling
       from ..parent import parent_module
       ```

## 6. **Built-in Functions for Modules**
   - **`dir()`**:
     - Lists all attributes in a module.
       ```python
       import math
       print(dir(math))
       ```

   - **`__name__`**:
     - Contains the name of the module.
       ```python
       if __name__ == "__main__":
           print("Executed directly!")
       ```

   - **`help()`**:
     - Displays documentation for a module.
       ```python
       import math
       help(math)
       ```

## 7. **Compiling Modules**
   - **`.pyc` Files**:
     - Compiled bytecode files stored in `__pycache__` for faster execution.
     - Automatically generated when a module is imported.

   - **Manual Compilation**:
     - Use `compileall` module to compile all Python files in a directory.
       ```bash
       python -m compileall .
       ```

## 8. **Differences from Pure OOP Languages**
   - **Dynamic Typing**:
     - No explicit data types required for variables in modules.
   - **Namespace Flexibility**:
     - Multiple ways to access attributes (`import`, `from ... import`, aliasing).
   - **Ease of Use**:
     - Modules are lightweight compared to OOP-based language packages.
   - **No Strong Encapsulation**:
     - All module attributes are accessible by default unless explicitly made private with `_`.

---

### Example Recap:
```python
# module_example.py
def add(a, b):
    return a + b

def subtract(a, b):
    return a - b

# main.py
import module_example as mod
print(mod.add(10, 5))       # Outputs 15
print(mod.subtract(10, 5))  # Outputs 5