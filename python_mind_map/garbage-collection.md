Markdown Mind Map: Python Garbage Collection

# Python Garbage Collection 🗑️

## Overview
- **Definition**: Automatic memory management mechanism in Python.
- **Purpose**: Reclaims memory occupied by objects no longer in use, avoiding memory leaks.
- **Based on**: Reference counting & cyclic garbage collection.
- **Key Modules**: `gc` (garbage collection module).

---

## Memory Management in Python
- **Dynamic Memory Allocation**:
  - Python objects are stored in private heap space.
  - Managed by the Python Memory Manager.
- **Memory Allocators**:
  - **Object-Specific Allocators**: Manage storage for lists, dictionaries, etc.
  - **Low-Level Allocators**: Interface with the operating system.
- **Tricky Statement**: Python does not allow explicit memory deallocation (like `free()` in C).

---

## Reference Counting
- **Definition**: Keeps track of the number of references to an object.
- **Mechanism**:
  - Object's **reference count** increases when assigned or passed to a function.
  - Reference count decreases when references are deleted or go out of scope.
- **Example**:
  ```python
  a = [1, 2, 3]  # Reference count = 1
  b = a          # Reference count = 2
  del a          # Reference count = 1

	•	When Collected: If reference count reaches 0.
	•	Limitations:
	•	Cannot handle reference cycles (e.g., two objects referencing each other).

Cyclic Garbage Collection
	•	Purpose: Handles reference cycles.
	•	Reference Cycles:
	•	Objects that reference each other but are unreachable.
	•	Example:

class Node:
    def __init__(self):
        self.reference = None

node1 = Node()
node2 = Node()
node1.reference = node2
node2.reference = node1
del node1
del node2  # Both objects remain in memory without cyclic GC.


	•	How It Works:
	•	Collects objects that form unreachable cycles.
	•	Uses generational collection.

Generational Garbage Collection
	•	Definition: Groups objects into generations based on their lifespan.
	•	Generations:
	•	Generation 0 (Youngest):
	•	Short-lived objects.
	•	Collected frequently.
	•	Generation 1:
	•	Objects surviving one cycle.
	•	Collected less frequently.
	•	Generation 2 (Oldest):
	•	Long-lived objects.
	•	Collected rarely.
	•	Process:
	•	Promotion: Objects surviving collection move to older generations.
	•	Performance:
	•	Optimized for short-lived objects (common in Python).

Garbage Collection with gc Module
	•	Importing the Module:

import gc


	•	Key Methods:
	•	gc.collect(): Triggers manual garbage collection.
	•	gc.isenabled(): Checks if garbage collection is enabled.
	•	gc.disable() / gc.enable(): Disable or enable garbage collection.
	•	gc.get_objects(): Returns all tracked objects.
	•	gc.get_stats(): Returns garbage collection statistics.
	•	Example:

import gc
gc.disable()  # Disable automatic garbage collection
# Perform operations...
gc.collect()  # Manually trigger garbage collection

Performance Considerations
	•	Pros:
	•	Simplifies memory management.
	•	Avoids manual deallocation errors.
	•	Cons:
	•	Garbage collection adds overhead.
	•	Cyclic collection may delay memory deallocation.
	•	Best Practices:
	•	Use local variables to minimize reference cycles.
	•	Use weak references for large objects (via weakref module).
	•	Tricky Statement: Manual garbage collection with gc.collect() may pause execution.

Internal Working
	•	Garbage Collection Process:
	1.	Identify unreachable objects.
	2.	Collect cyclic references.
	3.	Free memory.
	•	Implementation:
	•	Built into CPython (reference implementation of Python).
	•	Tied to Python’s private heap space.

Differences from Pure OOP Languages
	•	Python vs. Java:
	•	Python uses reference counting + cyclic GC.
	•	Java relies entirely on garbage collection.
	•	Python vs. C++:
	•	Python: No explicit deallocation (automatic).
	•	C++: Manual memory management with delete.
	•	Unique Python Feature: Ability to interact with garbage collector via gc module.

Relevant Examples
	1.	Cyclic References:

import gc

class A:
    def __init__(self):
        self.ref = None

obj1 = A()
obj2 = A()
obj1.ref = obj2
obj2.ref = obj1

del obj1
del obj2

gc.collect()  # Collects cyclic references


	2.	Manual Collection:

import gc

gc.disable()  # Disable automatic GC
# Create and delete large objects
gc.collect()  # Manually trigger GC

Debugging Memory Issues
	•	Tools:
	•	gc module for inspecting tracked objects.
	•	objgraph library for visualizing object references.
	•	Tips:
	•	Monitor reference counts.
	•	Use weak references to avoid cycles.

Resources
	•	Stackify: Python Garbage Collection
	•	Python Docs: Garbage Collection

References:
1. https://stackify.com/python-garbage-collection/
2. https://www.honeybadger.io/blog/memory-management-in-python/#generational-garbage-collection-in-python 
3. https://instagram-engineering.com/dismissing-python-garbage-collection-at-instagram-4dca40b29172
