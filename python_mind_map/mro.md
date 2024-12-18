# Markdown Mind Map: Python Method Resolution Order (MRO)

## 🌀 **Core Concepts of Method Resolution Order (MRO)**
### 1️⃣ **What is MRO?**
   - **Definition**: MRO determines the order in which base classes are searched for a method or attribute in a class hierarchy during inheritance.
   - **Purpose**: Ensures consistent and predictable method resolution when working with multiple inheritance.
   - **Example**:
     ```python
     class A:
         def process(self):
             print("A")

     class B(A):
         def process(self):
             print("B")

     class C(A):
         def process(self):
             print("C")

     class D(B, C):
         pass

     d = D()
     d.process()  # Output: B
     print(D.mro())
     ```
     - Output: `[<class '__main__.D'>, <class '__main__.B'>, <class '__main__.C'>, <class '__main__.A'>, <class 'object'>]`

---

## 🧮 **Algorithm Used: C3 Linearization**
### **Overview of C3 Linearization**
   - Python uses the **C3 Linearization Algorithm** for MRO in new-style classes.
   - **Rules**:
     1. A class comes before its parents.
     2. Parents are maintained in the order they are inherited.
     3. No class is repeated.
   - Introduced in **Python 2.3** and applicable for new-style classes in Python 3.

### **Example of C3 Linearization**:
   - For a diamond-shaped inheritance:
     ```python
     class A: pass
     class B(A): pass
     class C(A): pass
     class D(B, C): pass
     print(D.mro())
     ```
     - Output:
       ```
       [<class '__main__.D'>, <class '__main__.B'>, <class '__main__.C'>, <class '__main__.A'>, <class 'object'>]
       ```

---

## 🪜 **Key Subtopics**
### **1️⃣ Understanding `super()` with MRO**
   - **Definition**: `super()` helps you call the next method in the MRO chain.
   - Example:
     ```python
     class A:
         def process(self):
             print("A")

     class B(A):
         def process(self):
             print("B")
             super().process()

     class C(B):
         def process(self):
             print("C")
             super().process()

     obj = C()
     obj.process()
     ```
     - Output:
       ```
       C
       B
       A
       ```

### **2️⃣ MRO and Diamond Problem**
   - **Diamond Problem**:
     - Occurs when a class inherits from two classes that share a common parent.
     - MRO solves the problem by ensuring each class in the hierarchy is only called once.

   - Example:
     ```python
     class A:
         def process(self):
             print("A")

     class B(A):
         def process(self):
             print("B")

     class C(A):
         def process(self):
             print("C")

     class D(B, C):
         def process(self):
             print("D")
             super().process()

     d = D()
     d.process()
     ```
     - Output:
       ```
       D
       B
       C
       A
       ```

---

## 🛠 **How to Use and Validate MRO**
### **Using `mro()`**
   - Example:
     ```python
     print(D.mro())
     ```
     - Output:
       ```
       [<class '__main__.D'>, <class '__main__.B'>, <class '__main__.C'>, <class '__main__.A'>, <class 'object'>]
       ```

### **Using `__mro__` Attribute**
   - Example:
     ```python
     print(D.__mro__)
     ```

---

## ⚠️ **Cautions and Issues**
### **1️⃣ Avoiding Deadlocks**
   - **When Deadlocks May Happen**: Cyclic dependencies in the inheritance hierarchy.
   - **How Python Handles It**: Python raises a `TypeError` if MRO cannot be constructed.

### **2️⃣ Understanding Order of Method Resolution**
   - If the MRO is misunderstood:
     - Methods might not behave as intended.
     - Can lead to subtle bugs, especially in deep or multiple inheritance trees.

---

## 📚 **Tricky Concepts**
### **1️⃣ Overriding `__mro__`**
   - The MRO for a class is **calculated automatically** by Python and **cannot be modified** directly.

### **2️⃣ Combining `super()` and MRO**
   - **Use of `super()`** ensures consistent use of MRO in deep hierarchies.

---

## 📘 **Example Use-Cases**
### **1️⃣ Framework Development**
   - Frameworks like **Django** and **Flask** use MRO to determine method overrides in class-based views.
   - Example:
     ```python
     from django.views import View

     class BaseView(View):
         def get(self, request):
             print("Base GET")

     class CustomView(BaseView):
         def get(self, request):
             print("Custom GET")
             super().get(request)
     ```

### **2️⃣ Component-Based Design**
   - MRO is crucial when combining multiple components with shared parents.

---

## 🛠 **Tools to Analyze MRO**
### **1️⃣ Built-in Tools**
   - `Class.mro()` and `Class.__mro__`.

### **2️⃣ Third-Party Libraries**
   - **`inspect` module**:
     ```python
     import inspect
     print(inspect.getmro(D))
     ```

---

## 🔍 **Comparison to Other Languages**
### **MRO in Python vs. Other Languages**
   - Python: Uses C3 Linearization.
   - C++: Follows a depth-first approach.
   - Java: Only supports single inheritance, avoiding the diamond problem.

---

## 🛡 **Best Practices**
1. Keep inheritance hierarchies **shallow** to simplify MRO.
2. Use **composition over inheritance** to avoid complexity.
3. Always test for MRO with `Class.mro()` when working with multiple inheritance.

---

## **🌐 References**
1. [Understanding Method Resolution Order in Python](https://codefinity.com/blog/Understanding-Method-Resolution-Order-in-Python)
2. [Python Method Resolution Order (MRO) Documentation](https://docs.python.org/3/howto/mro.html)