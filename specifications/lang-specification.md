# Language Specification

## Overview

This document defines a declarative logic language that expresses logical truths, relationships, and computation rules through class-based declarative syntax. It extends classical logic semantics with computed attributes and conditional axioms, enabling rule-driven evaluation of business or natural-world logic.

Example:

```python
// There is a human
class Human:
    name: str

    def __init__(self, name: str) -> None:
        self.name = name

// All humans are mortal
Human.mortal = true

// Socrate is human
socrates = Human("Socrates")

> socrates.mortal == true
true

```

---

## 1. Core Concepts

### 1.1 Entities and Classes

- `class` declarations define categories of entities.
- Classes introduce logical types (predicates) and can define attributes, computed expressions, and universal rules.
- Syntax: `class <Name>:` defines a logical type.

Example:

```python
class Human:
    name: str

```

### 1.2 Instances

Instances represent particular facts. Instantiation uses Python-like syntax and automatically satisfies the class predicate.

```python
socrates = Human("Socrates")

```

Declares the fact `Human(Socrates)`.

### 1.3 Attributes and Computed Properties

Attributes define data or logical properties. Computed attributes are declared using expressions that derive values based on other attributes or axioms.

```python
Order.total = Order.item.price * Order.qty

```

---

## 2. Class-Level Rules (Axioms)

### 2.1 Universal Assignments

`Class.property = value` assigns a rule or value that applies universally to all instances.

Example:

```python
Human.mortal = true

```

Translates to ∀x (Human(x) → mortal(x)).

### 2.2 Conditional Rules

Conditions can be declared with `if` statements at the class level. They define logical or arithmetic implications.

Example:

```python
if Order.total > 25:
    Order.shipping_fee = 0

```

Means: ∀x (Order(x) ∧ total(x) > 25 → shipping_fee(x) = 0)

---

## 3. Logical Evaluation Semantics

### 3.1 Facts

Facts are direct truths such as instance creation or constant assignments.

### 3.2 Rule Propagation

The evaluator resolves class-level rules onto all instances by substitution.

### 3.3 Derived Attributes

When an expression references other attributes, the engine computes derived values lazily or upon query.

Example:

```python
Order.final = Order.total + Order.shipping_fee

```

### 3.4 Conditionals

Conditional expressions represent logical implications in the rulebase.

```python
if Order.final > 1000:
    Order.status = "AUDIT"

```

---

## 4. Syntax Summary

| Construct | Meaning |
| --- | --- |
| `class <Name>:` | Declare a new logical type |
| `<instance> = <Class>(args)` | Instantiate a new entity |
| `<Class>.<attr> = <expr>` | Declare a universal rule or derived attribute |
| `if <expr>:` | Declare a conditional rule |
| `<instance>.<attr>` | Retrieve or infer value |
| `> <expr>` | Evaluate logical query |

---

## 5. Example: Order Pricing Logic

### Definition

```python
class Item:
    sku: str
    price: float

    def __init__(self, sku, price):
        self.sku = sku
        self.price = price

class Order:
    item: Item
    qty: int
    status: str
    shipping_fee: float

    def __init__(self, qty: int, item: Item, status: str, shipping_fee: float):
        self.item = item
        self.qty = qty
        self.status = status
        self.shipping_fee = shipping_fee

# The total price of an order is calculated by multiplying the price of each item by its quantity.
Order.total = Order.item.price * Order.qty

# If order is more than $25, shipping is free
if Order.total > 25:
    Order.shipping_fee = 0

# Final price of order is total price of order plus shipping fee
Order.final = Order.total + Order.shipping_fee

# If the final price of order is more than $1000, order will be audited
if Order.final > 1000:
    Order.status = "AUDIT"

# If order is cancelled, final price is 0
if Order.status == "CANCELLED":
    Order.final = 0

```

### Semantics

1. **Total Computation:** ∀x (Order(x) → total(x) = item(x).price × qty(x))
2. **Shipping Rule:** ∀x (Order(x) ∧ total(x) > 25 → shipping_fee(x) = 0)
3. **Final Price:** ∀x (Order(x) → final(x) = total(x) + shipping_fee(x))
4. **Audit Trigger:** ∀x (Order(x) ∧ final(x) > 1000 → status(x) = "AUDIT")
5. **Cancellation Override:** ∀x (Order(x) ∧ status(x) = "CANCELLED" → final(x) = 0)

### Example Instances

```python
item1 = Item("100001", 100)
item2 = Item("100002", 25)
item3 = Item("100003", 5)

order1 = Order(item1, 1)
order2 = Order(item2, 10)
order3 = Order(item3, 4)
order4 = Order(item1, 100)

```

Expected results:

```python
order1 = {"item": {"sku": "100001", "price": 100}, "qty": 1, "total": 100, "shipping_fee": 0, "final": 100, "status": "NORMAL"}
order2 = {"item": {"sku": "100002", "price": 25}, "qty": 10, "total": 250, "shipping_fee": 0, "final": 250, "status": "NORMAL"}
order3 = {"item": {"sku": "100003", "price": 5}, "qty": 4, "total": 20, "shipping_fee": 5.0, "final": 25.0, "status": "NORMAL"}
order4 = {"item": {"sku": "100001", "price": 100}, "qty": 100, "total": 10000, "shipping_fee": 0, "final": 10000, "status": "AUDIT"}

```

---

## 6. Type System

Supports primitive types (`str`, `int`, `float`, `bool`) and composite user-defined types. Computed attributes are type-inferred based on arithmetic expressions.

---

## 7. Evaluation Model

- Evaluator maintains **class environment** (rules and axioms) and **instance environment** (facts).
- On query or instance creation, all relevant rules are applied.
- Conditional rules evaluated top-down; later rules may override earlier derived facts.

---

## 8. Future Extensions

- Rule chaining and dependency graphs.
- Support for negation-as-failure.
- Support for declarative imports and modules.

---
