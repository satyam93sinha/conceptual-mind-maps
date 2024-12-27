MARKDOWN MIND MAP: SOLID Principles in Python

Source: ArjanCodes GitHub Repository on SOLID Principles

# SOLID Principles in Python

## Introduction
- **Definition**: SOLID is a set of design principles in software development that aims to create robust, maintainable, and scalable systems.
- **Core Acronym**:
  - **S**: Single Responsibility Principle (SRP)
  - **O**: Open/Closed Principle (OCP)
  - **L**: Liskov Substitution Principle (LSP)
  - **I**: Interface Segregation Principle (ISP)
  - **D**: Dependency Inversion Principle (DIP)
- **Goal**:
  - Improve code readability and maintainability.
  - Reduce coupling.
  - Encourage reusability.

---

## S - Single Responsibility Principle (SRP)
- **Concept**: A class should have only one reason to change.
- **Objective**: Ensure a class has a single responsibility.
- **Benefits**:
  - Easier debugging.
  - Improved testing.

### Example: Before SRP
```python
class Invoice:
    def __init__(self, items):
        self.items = items

    def calculate_total(self):
        return sum(item.price for item in self.items)

    def print_invoice(self):
        for item in self.items:
            print(f"{item.name}: {item.price}")

	•	Issue: The Invoice class has two responsibilities:
	1.	Calculating totals.
	2.	Printing invoices.

Example: After SRP

class Invoice:
    def __init__(self, items):
        self.items = items

    def calculate_total(self):
        return sum(item.price for item in self.items)

class InvoicePrinter:
    def print_invoice(self, invoice):
        for item in invoice.items:
            print(f"{item.name}: {item.price}")

	•	Transformation:
	•	Extract printing logic into InvoicePrinter.
	•	Each class now has a single responsibility.

O - Open/Closed Principle (OCP)
	•	Concept: Software entities should be open for extension but closed for modification.
	•	Objective: Allow new functionality without changing existing code.
	•	Benefits:
	•	Promotes extensibility.
	•	Reduces risk of breaking existing functionality.

Example: Before OCP

class Discount:
    def apply_discount(self, price, discount_type):
        if discount_type == "percentage":
            return price * 0.9
        elif discount_type == "fixed":
            return price - 10

	•	Issue: Adding new discount types requires modifying the method.

Example: After OCP

class Discount:
    def apply_discount(self, price):
        raise NotImplementedError()

class PercentageDiscount(Discount):
    def apply_discount(self, price):
        return price * 0.9

class FixedDiscount(Discount):
    def apply_discount(self, price):
        return price - 10

	•	Transformation:
	•	Use inheritance to add new discount types without changing the Discount base class.

L - Liskov Substitution Principle (LSP)
	•	Concept: Subtypes must be substitutable for their base types.
	•	Objective: Avoid breaking functionality when using derived classes.
	•	Benefits:
	•	Ensures consistency.
	•	Prevents unexpected behavior.

Example: Before LSP

class Bird:
    def fly(self):
        pass

class Penguin(Bird):
    def fly(self):
        raise Exception("Penguins cannot fly")

	•	Issue: The Penguin class violates the LSP because it cannot fly.

Example: After LSP

class Bird:
    def move(self):
        pass

class FlyingBird(Bird):
    def move(self):
        print("I am flying!")

class Penguin(Bird):
    def move(self):
        print("I am swimming!")

	•	Transformation:
	•	Use composition or redefine methods to ensure substitutability.

I - Interface Segregation Principle (ISP)
	•	Concept: Clients should not be forced to depend on methods they do not use.
	•	Objective: Avoid bloated interfaces.
	•	Benefits:
	•	Reduces unnecessary dependencies.
	•	Improves flexibility.

Example: Before ISP

class Machine:
    def print(self):
        pass

    def scan(self):
        pass

    def fax(self):
        pass

	•	Issue: A simple printer does not need the scan or fax methods.

Example: After ISP

class Printer:
    def print(self):
        pass

class Scanner:
    def scan(self):
        pass

	•	Transformation:
	•	Separate interfaces for specific functionalities.

D - Dependency Inversion Principle (DIP)
	•	Concept: High-level modules should not depend on low-level modules. Both should depend on abstractions.
	•	Objective: Reduce coupling between components.
	•	Benefits:
	•	Promotes modularity.
	•	Simplifies testing.

Example: Before DIP

class Backend:
    def connect(self):
        return "Connected to backend"

class Application:
    def run(self):
        backend = Backend()
        print(backend.connect())

	•	Issue: The Application class directly depends on the Backend class.

Example: After DIP

class Backend:
    def connect(self):
        raise NotImplementedError()

class DatabaseBackend(Backend):
    def connect(self):
        return "Connected to database backend"

class Application:
    def __init__(self, backend):
        self.backend = backend

    def run(self):
        print(self.backend.connect())

	•	Transformation:
	•	Use dependency injection to decouple Application and Backend.

Transforming a Codebase to Follow SOLID
	1.	Analyze Current Architecture:
	•	Identify violations of SOLID principles.
	2.	Refactor Incrementally:
	•	Apply one principle at a time.
	3.	Leverage Design Patterns:
	•	Use patterns like Strategy, Factory, and Adapter.
	4.	Testing:
	•	Ensure new components are well-tested.

Tools/Libraries to Apply SOLID in Python
	•	Dependency Injection:
	•	injector library.
	•	dependency-injector library.
	•	Static Analysis:
	•	pylint for detecting code smells.
	•	Code Organization:
	•	black for code formatting.
	•	mypy for type checking.

Use-Cases for SOLID Principles
	•	When to Use:
	•	Large, scalable systems.
	•	Collaborative projects.
	•	When Not to Use:
	•	Small scripts or one-off projects.
	•	Overhead outweighs benefits.

Constraints to Keep in Mind
	•	Over-engineering is a risk.
	•	Understand the problem before applying SOLID principles.
	•	Balance flexibility with simplicity.

Visual Representation

mindmap
  root((SOLID Principles))
    S(Single Responsibility)
      - Benefits
      - Before Example
      - After Example
    O(Open/Closed)
      - Benefits
      - Before Example
      - After Example
    L(Liskov Substitution)
      - Benefits
      - Before Example
      - After Example
    I(Interface Segregation)
      - Benefits
      - Before Example
      - After Example
    D(Dependency Inversion)
      - Benefits
      - Before Example
      - After Example
    Transforming Codebase
      - Analyze Current Architecture
      - Incremental Refactoring
    Tools
      - Injector Libraries
      - Static Analysis Tools
    Use Cases
      - When to Use
      - When Not to Use
    Constraints
      - Over-Engineering Risks

References:
1. https://www.youtube.com/watch?v=pTB30aXS77U
2. https://github.com/ArjanCodes/betterpython/tree/main/9%20-%20solid
