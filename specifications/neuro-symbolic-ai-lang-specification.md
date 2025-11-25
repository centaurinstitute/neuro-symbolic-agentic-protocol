# Neuro-Symbolic AI Language Specification

## 1. Introduction

The Neuro-Symbolic AI Protocol (NSAIP) defines a unified framework for expressing logic, structure, and object semantics directly within Python. It interprets standard Python syntax as a declarative logical language, allowing rules, constraints, relationships, and object definitions to be represented symbolically while remaining fully compliant with Python’s grammar and execution model.

NSAIP provides a mechanism for transforming patterns learned by Large Language Models (LLMs) into explicit symbolic logic expressed entirely within standard Python syntax. Rather than maintaining a strict separation between neural inference and symbolic reasoning, the protocol embeds pattern-derived correlations, dependency structures, and abstractions directly into executable Python expressions. This allows LLM-generated insights to be represented as transparent, interpretable logical constructs, enabling Python to function simultaneously as a procedural language and a declarative logic formalism suitable for neuro-symbolic integration.

### 1.1 Declarative Runtime Model

The Neuro-Symbolic AI Protocol operates as a declarative runtime that interprets Python statements in-flight, constructing a Logic Graph that captures relationships between both logic and data statements. Unlike traditional imperative execution models where statements are executed sequentially and forgotten, NSAIP maintains a persistent representation of all statements and their interdependencies. This allows the runtime to automatically propagate changes through the dependency graph, ensuring that all derived values remain consistent with their declarations.

The runtime tracks each statement as it is received and dynamically creates dependency edges between variables, object properties, and logical expressions. When a value changes, the system traverses the graph to update all dependent computations automatically, providing reactive behavior without explicit event handling or manual synchronization code.

### 1.2 Integration with Neural Systems

NSAIP provides a mechanism for transforming patterns learned by Large Language Models into explicit symbolic logic. Rather than maintaining a strict separation between neural inference and symbolic reasoning, the protocol enables LLM-generated insights to be represented as transparent, interpretable logical constructs expressed in standard Python syntax. This bridges the gap between statistical pattern recognition and formal logical deduction, allowing systems to combine the flexibility of neural learning with the rigor and explainability of symbolic AI.

The protocol's declarative nature makes it particularly suitable for neuro-symbolic integration, as neural networks can generate or modify logical rules that are immediately integrated into the reasoning system without requiring recompilation or system restarts.

## Lexical Analysis

Lexical analysis in NSAIP follows Python's lexical conventions with minimal extensions. Since NSAIP interprets standard Python syntax declaratively, the lexical structure remains fully compatible with Python's grammar, allowing NSAIP code to be parsed by standard Python parsers. The lexical elements—identifiers, literals, operators, comments, and whitespace—adhere to Python Language Reference specifications, with the addition of reserved symbols for future declarative constructs.

### Identifiers

Identifiers follow Python naming rules:
- Must begin with a letter (`a-z`, `A-Z`) or underscore (`_`)
- May contain letters, digits (`0-9`), and underscores
- Are case-sensitive (`myVar` and `myvar` are distinct identifiers)
- Cannot be Python keywords (`if`, `class`, `def`, etc.)

**Examples:**
```python
user_count = 10
_internal_state = True
Account = class_definition
price2024 = 99.99
```

### Literals

NSAIP supports all standard Python literal forms:

**Numeric Literals:**
- Integers: `42`, `-17`, `1_000_000` (underscores for readability)
- Floating-point: `3.14`, `-0.001`, `1e-10`, `2.5e3`
- Complex: `3+4j`, `1.5-2.5j`

**String Literals:**
- Single quotes: `'hello'`
- Double quotes: `"world"`
- Triple quotes: `'''multi-line'''` or `"""text"""`
- Raw strings: `r'\n is literal'`
- F-strings: `f"Value: {x}"`

**Boolean and None:**
- Boolean values: `True`, `False`
- Null value: `None`

### Comments

Comments follow Python conventions:
- Single-line comments begin with `#` and extend to end of line
- Encoding declarations are supported: `# -*- coding: utf-8 -*-`
- Docstrings use triple-quoted strings and serve as documentation

**Examples:**
```python
# This is a single-line comment
x = 5  # Inline comment

"""
This is a multi-line docstring
that documents a module or function.
"""
```

### Operators and Delimiters

NSAIP uses standard Python operators for arithmetic, comparison, logical operations, and structural delimiters:

**Arithmetic:** `+`, `-`, `*`, `/`, `//`, `%`, `**`
**Comparison:** `==`, `!=`, `<`, `>`, `<=`, `>=`
**Logical:** `and`, `or`, `not`
**Bitwise:** `&`, `|`, `^`, `~`, `<<`, `>>`
**Assignment:** `=`, `+=`, `-=`, `*=`, `/=`, etc.
**Delimiters:** `(`, `)`, `[`, `]`, `{`, `}`, `,`, `:`, `.`, `;`, `@`

**Reserved Symbols:**
- `$` — Reserved for future fact assertion syntax
- `?` — Not used in current specification
- Backtick — Not used in current specification

### Whitespace and Indentation

Whitespace handling follows Python's indentation-based block structure:

**Indentation:**
- Leading whitespace (spaces or tabs) determines code block nesting
- Consistent indentation is required within a block
- Standard convention: 4 spaces per indentation level

**Line Continuation:**
- Explicit continuation: Backslash `\` at end of line
- Implicit continuation: Inside parentheses `()`, brackets `[]`, or braces `{}`

**Examples:**
```python
# Indentation defines block structure
if condition:
    statement1
    statement2

# Explicit line continuation
long_expression = value1 + value2 + \
                  value3 + value4

# Implicit line continuation
result = function_call(
    arg1,
    arg2,
    arg3
)
```

### Encoding

NSAIP source files are UTF-8 encoded by default, supporting international characters in strings and comments. Encoding declarations can be specified using:
```python
# -*- coding: utf-8 -*-
``` 

## Data Model

The Data Model defines how NSAIP represents and manages program entities within the Logic Graph. It establishes the fundamental building blocks for declarative computation: variables that store values and maintain dependencies, classes that define object templates, and instances that represent individual entities tracked by the runtime. The data model supports both global and local scopes, enables property-based object composition, and provides query mechanisms for retrieving instances based on logical predicates. All entities in the data model are integrated into the Logic Graph, allowing automatic dependency tracking and reactive updates across the entire system.

### Variable

Variables in NSAIP serve as named references to values within the Logic Graph. Unlike traditional imperative variables that simply hold values, NSAIP variables maintain dependency relationships with other variables and expressions. When a variable is assigned an expression containing other variables, the runtime creates dependency edges that enable automatic recomputation when source values change. Variables can be declared at global scope (accessible throughout the program) or local scope (limited to a specific context such as a function body).

#### Global Variable

Global variables are declared at the module level and remain accessible throughout the program execution. When a global variable is assigned an expression containing other variables, the runtime establishes dependency relationships that enable automatic propagation of changes. The following example demonstrates a simple global variable declaration and a dependent variable that automatically updates when its dependency changes.

```python
pi = 3.14
```

```python
area_of_circle = 4 * pi ** 2
```

```python
> pi
3.14
> area_of_circle
39.4384
```

#### Local Variable

Local variables are scoped to a specific context, such as within a function body or conditional block. They can reference global variables while maintaining their own local scope. Local variables participate in the same dependency tracking mechanism as global variables, but their lifetime and visibility are limited to their enclosing scope.

```python
radius = 10
volume = None

    area = (radius ** 2) * 3.14
    global volume
    volume = area * 5
```

```python
> radius
10
> volume
1570
```

### Classes

Classes in NSAIP define templates for creating objects that are tracked within the Logic Graph. Each class declaration establishes both a type definition and a storage collection for instances. The `__init__` method specifies how instances are initialized with their properties. When a class is defined, it becomes a first-class entity in the runtime, enabling class-level queries, computed properties, and declarative rules that apply to all instances. Classes serve as the foundation for object-oriented modeling within the neuro-symbolic framework.

```python
class Person:
    def __init__(self, name, age):
        self.name = name
        self.age = age
```

```python
person1 = Person("Alice", 25)
```

```python
> student1
{ "name": "Alice", "age": 25 }
```

### Instance

#### Properties

Instance properties can be assigned both during initialization (via `__init__`) and after instance creation through direct property assignment. Each instance is registered in the class collection and stored as a node in the Logic Graph, making it queryable and subject to class-level declarative rules. Properties added dynamically become part of the instance's state and participate in dependency tracking.

```python
class Vehicle:
    def __init__(self, model):
        self.model = model

vehicle1 = Vehicle("Honda")
vehicle1.color = "red"
```

#### Query Expressions

NSAIP provides built-in query methods for retrieving instances from the Logic Graph based on logical predicates. The `.where()` method returns all instances matching a given lambda predicate, while `.find()` returns a single instance that satisfies the condition. These queries execute directly against the in-memory graph without requiring external database systems or query languages.

```python
class User:
    def __init__(self, email, password):
        self.email = email
        self.password = md5(password)
```

```python
user1 = User("jordan.miller@testmail.local", "jmiller_2024!")
user2 = User("sarah.chen@dev-example.net", "Sc!dev4392")
user3 = User("michael.roberts@samplemail.org", "MRoberts#92")
user4 = User("anna.keller@mockserver.app", "AnnaKeller_88")
```

```python
> User.where(lambda user: user.email == "jordan.miller@testmail.local")
[{ "email": "jordan.miller@testmail.local", "password": "5188d1a5278b980296ed0af914682507" }]

> User.find(lambda user: user.email == "sarah.chen@dev-example.net")
{ "email": "sarah.chen@dev-example.net", "password": "22497336687c6bbc110f64762b10e2ce" }
```

## Declarative Statements

Declarative statements form the core of NSAIP's reactive computation model. Unlike imperative statements that execute once and are forgotten, declarative statements establish persistent relationships in the Logic Graph that automatically maintain consistency as values change. When a variable, property, or conditional statement is declared, the runtime creates dependency edges and ensures that all dependent computations are automatically updated when their source values change. This section demonstrates declarative behavior at various levels: variable-level dependencies, instance-level properties, class-level rules, and conditional constraints.

#### Variable Level

Variable-level declarative statements demonstrate the core reactive behavior of NSAIP. When variable `b` is defined as an expression involving `a`, the runtime creates a dependency edge from `a` to `b`. Subsequently, any reassignment to `a` triggers automatic recalculation of `b`, ensuring the relationship `b = a + 2` remains valid throughout execution.

```python
a = 1
b = a + 2
```

```python
> a
1
> b
3
```

```python
a = 2
```

```python
> a
2
> b
4
```

#### Instance Level

Instance-level declarative statements define conditional rules that apply to specific object instances. These rules are stored in the Logic Graph and automatically re-evaluate when the properties they depend on change. In this example, the `archive` property is conditionally set based on the `status` property, creating a declarative constraint on the instance.

```python
class Project:
    def __init__(self, name, status):
        self.name = name
        self.status = status
        
library_project = Project("Library Project", "started")

if library_project.status == "completed"
    library_project.archive = True
```

#### Class Level

Class-level declarative statements define properties or rules that apply universally to all instances of a class. When a property is assigned at the class level (e.g., `Human.mortal = True`), it becomes accessible through all instances of that class. This enables the expression of universal facts and shared characteristics across an entire class of objects.

```python
class Human:
    def __init__(self, name):
        self.name = name        
```

```python
Human.mortal = True
```

```python
human1 = Human("socrates")
```

```python
> human1.mortal
true
```

#### `value` Property of Variable

The `.value` accessor provides a mechanism to retrieve the current value of a variable without establishing a dependency relationship in the Logic Graph. This is useful when you want to capture a snapshot of a value at a specific moment without creating reactive behavior. In the example below, `balance1.amount` uses `rate.value` to get the current rate without making the amount dependent on future changes to `rate`.

```python
rate = 1.15

class Balance:
    def __init__(self):
        self.amount = 0

balance1 = Balance()
balance1.amount = 1000 * rate.value
```

#### `value` Property of Instance

Similar to variables, instance properties can also be accessed using the `.value` suffix to obtain snapshot values without creating dependencies. This is particularly useful when combining instance properties with class-level properties, allowing selective control over which relationships should be reactive and which should be static.

```python
class Stock:
    rate = 0.05
    def __init__(amount):
        self.amount = 0
```
	
```python
stock1 = Stock(5)
stock1 = 22.5 * stock1.amount * stock1.rate.value
```

#### If Statement

Conditional statements at the class level define declarative rules that automatically re-evaluate when their predicate conditions change. The if-else structure creates branching logic nodes in the Logic Graph, and the appropriate branch is activated based on the current state. When an instance property changes and affects the condition, the runtime automatically updates the dependent properties according to the active branch.

```python
class Account:
    def __init__(balance):
        self.balance = balance

if Account.balance < 1500
    Account.status = "low"
else
    Account.status = "normal"
```

```python
account1 = Account(2000)
```

```python
> account1.status
"normal"
```

```python
account1.balance = 1000
```

```python
> account1.status
"low"
```

## Imperative Statements

While NSAIP operates primarily in a declarative mode, certain situations require traditional imperative execution where statements execute once without establishing persistent dependencies. The `@Imperative` decorator marks statements for one-time execution, preventing the runtime from creating dependency edges or automatically updating values when dependencies change. This is useful for initialization code, side-effect operations, or scenarios where reactive behavior would be inappropriate. Imperative statements provide an escape hatch from the declarative model when explicit control over execution is needed.

### Variable

```python
a = 1
@imperative
b = a + 2
```

or

```python
a = 1
b = a.value + 2
```

```python
> a
1
> b
3
```

```python
a = 2
```

```python
> a
2
> b
3
```

## Compound Class Statements

Compound class statements express relationships and computed properties that span multiple classes. When one class references properties of another class through object composition, the runtime creates transitive dependency chains through the Logic Graph. These cross-class dependencies enable complex reasoning patterns where changes to one object automatically propagate through related objects. Class-level computed properties can access nested properties through reference chains, establishing declarative rules that maintain consistency across compositional relationships.

```python
class Item:
    def __init__(self, sku, price)
        self.sku = sku
        self.price = price
```

```python
class Order:
    def __init__(self, qty, item):
        self.item = item
        self.qty = qty
```

```python
Order.total = Order.item.price * Order.qty
```

```python
item1 = Item("1000001", 100)
order1 = Order(2, item1)
```

```python
> order1.total
200
```

## Functions

Functions in NSAIP define reusable computational procedures that can be invoked with arguments. While functions themselves are defined imperatively, their invocations within declarative expressions can create dependencies on function parameters. However, function definitions themselves do not automatically trigger recomputation of existing call sites when redefined—variables that depend on function calls retain their computed values unless explicitly reassigned. This behavior distinguishes function definitions from declarative variable assignments, as functions encapsulate imperative logic rather than establishing persistent relationships.

```python
@declarative
def mass(volume):
    ratio = 1.2
    return volume * ratio

air_mass = mass(10) + 1
```

```python
> air_mass
13
```

```python
def mass(volume):
    ratio = 1.3
    return volume * ratio
```

```python
> air_mass
13
```

## MCP

The Model Context Protocol (MCP) integration enables NSAIP to incorporate external data sources and services directly into declarative computations. MCP calls can be embedded within class-level property definitions, allowing computed properties to depend on external APIs, databases, or real-time data feeds. When an MCP service is invoked as part of a declarative expression, the runtime treats it as a dependency source, enabling seamless integration of external information into the Logic Graph. This bridges the gap between internal symbolic reasoning and external data systems, allowing the protocol to reason over both local state and remote resources.

```python
class Stock:
    def __init__(self, ticker, qty):
        self.ticker = ticker
        self.qty = qty

Stock.final_price = stock_mcp.get_price(Stock.ticker) * Stock.qty
```

```python
stock1 = Stock("MM", 3)
```

```python
> stock1
{ "ticker": "MM", "qty": 3 }
```



## Data Persistence

NSAIP includes a built-in data persistence mechanism that eliminates the need for external databases. The runtime manages object state and stores each transaction in the built-in data store by declaratively maintaining the Logic Graph structure.

### Built-in Storage Layer

Rather than requiring external database systems, the runtime maintains its own persistent storage that:

- **Stores Statements**: Each statement is persisted as part of the graph structure
- **Preserves Dependencies**: Relationships between entities are stored along with values
- **Maintains History**: Transaction log enables replay and recovery
- **Supports Transactions**: ACID properties ensure consistency

### On-Chain Data Model

Data and logic are stored together "on-chain" within the same runtime environment:

```python
class User:
    def __init__(self, email, password):
        self.email = email
        self.password = md5(password)

user1 = User("jordan.miller@testmail.local", "jmiller_2024!")
# Both the User class definition and user1 instance are persisted
```

When this code executes:
1. The `User` class definition is added to the graph and persisted
2. The `user1` instance is created as a graph node
3. Property values are stored as node attributes
4. References between user1 and the User class are maintained
5. All state is immediately available for querying without database round-trips

### Declarative Persistence

Persistence occurs automatically as a side effect of statement execution. Developers do not need to:
- Write explicit save/commit operations
- Define database schemas separately from code
- Manage connections or transactions manually
- Synchronize between memory and storage

The runtime automatically:
- Persists each statement as it is executed
- Updates storage when values change through dependency propagation
- Maintains referential integrity across all relationships
- Rolls back failed transactions to ensure consistency

### Query Model

The built-in storage supports declarative queries through class methods:

```python
# Find all matching instances
User.where(lambda user: user.email.endswith("@testmail.local"))

# Find single instance
User.find(lambda user: user.email == "sarah.chen@dev-example.net")
```

These queries execute directly against the in-memory Logic Graph without SQL translation or external database queries, providing near-instantaneous results.

### Performance Characteristics

The integrated persistence model provides:

- **Reduced I/O**: Data resides in memory with asynchronous persistence
- **Linear Scaling**: For average-complexity applications, close to O(n) performance
- **No Network Latency**: Eliminates database connection overhead
- **Consistent State**: No synchronization delays between compute and storage layers

This architecture is particularly suited for applications where logic and data are tightly coupled and where the overhead of traditional database systems would exceed the benefits of separate storage.
## Top-Level Components

The NSAIP runtime consists of several interconnected subsystems that work together to provide declarative execution with automatic reasoning:

### 1. Parser and Statement Analyzer

Receives Python statements and analyzes them to determine:
- Variable dependencies and references
- Class and instance relationships
- Conditional logic branches
- Function calls and their side effects

The parser operates incrementally, processing statements as they arrive without requiring complete program files.

### 2. Logic Graph Manager

Maintains the central graph data structure representing all program state:
- Creates and updates nodes for variables, objects, and classes
- Establishes dependency edges based on expression analysis
- Manages graph topology for efficient traversal
- Handles graph modifications during plasticity operations

### 3. Dependency Tracker

Monitors relationships between entities to enable automatic propagation:
- Builds dependency trees from expression analysis
- Identifies affected nodes when values change
- Determines topological execution order for updates
- Detects circular dependencies and handles them appropriately

### 4. Inference Engine

Executes logical reasoning over the graph:
- Evaluates expressions in the context of current graph state
- Applies conditional logic based on predicate evaluation
- Propagates changes through dependent relationships
- Performs query resolution for `.where()` and `.find()` operations

### 5. State Manager

Coordinates the in-memory state model:
- Maintains consistent object state across updates
- Manages instance collections for each class
- Tracks property values and computed attributes
- Ensures transactional integrity during updates

### 6. Persistence Layer

Handles storage and recovery:
- Persists statements as they are executed
- Maintains transaction log for recovery
- Provides durability guarantees
- Supports state snapshots and replay

### 7. Type System

Manages class definitions and type relationships:
- Stores class templates with their methods and properties
- Handles inheritance and class-level declarations
- Provides type checking for operations
- Supports dynamic type extension

### Component Interaction

These components work together in the statement execution pipeline:

```
Statement Input
      ↓
Parser/Analyzer
      ↓
Logic Graph Manager ←→ Dependency Tracker
      ↓
Inference Engine ←→ State Manager
      ↓
Persistence Layer
      ↓
Result/Side Effects
```

The architecture allows for extensibility where additional components (such as MCP integrations or neural reasoning modules) can be inserted into the pipeline to enhance capabilities while maintaining the core declarative execution model.
## Grammar

NSAIP uses standard Python grammar as defined in the Python Language Reference, with additional semantic interpretations for declarative behavior. The grammar remains fully compatible with Python syntax, but the runtime assigns declarative meanings to certain constructs.

### Statement Categories

**Assignment Statements** (Declarative by default)
```python
variable = expression
```
Creates a dependency relationship where `variable` is automatically updated when `expression` dependencies change.

**Class Definitions**
```python
class ClassName:
    def __init__(self, params):
        self.property = value
```
Defines both a type template and a storage collection for instances.

**Class-Level Assignments** (Declarative rules)
```python
ClassName.property = expression
```
Establishes a computed property that applies to all instances of the class, creating a rule in the Logic Graph.

**Instance Creation**
```python
instance = ClassName(args)
```
Creates a node in the Logic Graph, registers it with the class, and makes it queryable.

**Conditional Statements** (Declarative constraints)
```python
if condition:
    statement
else:
    statement
```
Creates branching logic nodes where branches are re-evaluated when condition dependencies change.

**Function Definitions** (Imperative by default unless dependencies are tracked)
```python
def function_name(params):
    return expression
```
Defines callable operations. When used in declarative contexts, may create dependencies.

**Query Expressions** (Special class methods)
```python
ClassName.where(lambda obj: predicate)
ClassName.find(lambda obj: predicate)
```
Executes queries over all instances of the class.

**Decorator Annotations**
```python
@Imperative
statement
```
Marks statements as imperative, preventing automatic dependency tracking and propagation.

### Extended Syntax Elements

**Property Access Chains** (Dependency paths)
```python
Order.item.price
```
Creates transitive dependencies through object references.

**Special Properties**
```python
variable.value    # Access snapshot value without creating dependency
instance.value    # Access instance state without triggering updates
```

**Reserved Operators**
```python
$  # Reserved for future fact assertion syntax
```

### Grammar Production Rules (BNF Extension)

```
declarative_assignment ::= target "=" expression
class_rule            ::= classname "." identifier "=" expression
conditional_rule      ::= "if" expression ":" suite ["else" ":" suite]
instance_query        ::= classname ".where(" lambda_expression ")"
                        | classname ".find(" lambda_expression ")"
imperative_statement  ::= "@Imperative" NEWLINE statement
```

### Semantic Interpretation

The grammar is interpreted with declarative semantics by default:

1. **Assignments** create persistent relationships, not one-time value transfers
2. **Conditionals** establish rules that re-evaluate automatically
3. **Class-level statements** define templates applied to all instances
4. **Property access** establishes dependency tracking

This allows standard Python code to function as a declarative logic specification while remaining syntactically valid Python.
## Data Types

NSAIP supports all standard Python data types with additional semantic behavior for tracking in the Logic Graph. Types are classified into primitive types, collection types, and symbolic types.

### Primitive Types

**Numeric Types**
- `int`: Integer values (arbitrary precision)
- `float`: Floating-point numbers (IEEE 754 double precision)
- `complex`: Complex numbers with real and imaginary parts

All numeric types support standard arithmetic operations and participate in dependency tracking.

**Boolean Type**
- `bool`: Logical values `True` and `False`
- Used in conditional expressions to control rule activation

**None Type**
- `None`: Represents absence of value or undefined state

**String Type**
- `str`: Unicode text sequences
- Immutable and hashable for use as keys

### Collection Types

**List**
```python
items = [1, 2, 3]
```
Ordered, mutable sequences. Changes to list elements can trigger dependency updates if tracked.

**Tuple**
```python
coordinates = (x, y, z)
```
Ordered, immutable sequences. Often used for compound keys or return values.

**Dictionary**
```python
mapping = {"key": "value"}
```
Unordered key-value associations. Can be used to represent structured data.

**Set**
```python
unique_items = {1, 2, 3}
```
Unordered collections of unique elements.

### Object Types

**Class Instances**
```python
class Person:
    def __init__(self, name):
        self.name = name

person1 = Person("Alice")
```
User-defined types that become nodes in the Logic Graph. Each instance is:
- Tracked by the runtime
- Queryable through class methods
- Subject to class-level rules

**Functions**
```python
def compute(x):
    return x * 2
```
Callable objects that can be invoked. Functions may create dependencies if used in declarative expressions.

### Symbolic Types (Logic Graph Specific)

**Dependency References**
When a variable appears in an expression, it creates a dependency reference rather than just copying the value:
```python
a = 5
b = a + 2  # b maintains a dependency on a, not just the value 7
```

**Computed Properties**
Class-level property definitions create computed types that dynamically calculate values:
```python
Order.total = Order.item.price * Order.qty
# total is a computed property type
```

**Rule Predicates**
Lambda expressions used in conditionals and queries represent predicate types:
```python
if Account.balance < 1500:  # Predicate: Account → bool
    Account.status = "low"
```

### Type Behavior in Logic Graph

**Value Types vs. Reference Types**
- Primitive types (int, float, str, bool) are stored as values
- Objects and collections are stored as references with dependency edges

**Type Preservation**
The Logic Graph preserves type information across updates:
```python
a = 5        # int
b = a + 2    # int (preserves numeric type)
a = 5.5      # float
# b automatically becomes 7.5 (float), preserving type consistency
```

**Dynamic Typing**
Variables can change types through reassignment:
```python
x = 5        # x is int
x = "hello"  # x becomes str, dependencies are updated accordingly
```

**Type Checking**
The runtime performs dynamic type checking during expression evaluation:
- Ensures operations are valid for operand types
- Raises type errors for incompatible operations
- Maintains type safety across dependency propagation

### Special Type Behaviors

**The `.value` Accessor**
Accessing `.value` on a variable or instance property returns the current value without creating a dependency:
```python
rate = 1.15
amount = 1000 * rate.value  # amount does not depend on rate
```

This is useful for creating snapshot values that should not automatically update.

**Type Coercion**
Standard Python type coercion rules apply:
- Numeric operations follow standard promotion rules (int → float → complex)
- String concatenation requires explicit conversion
- Boolean contexts follow Python truthiness rules

## Exceptions

NSAIP extends Python's exception handling model to work with the declarative runtime and transactional execution model. Exceptions can occur during statement execution, dependency propagation, or constraint validation.

### Logic Graph Exceptions

NSAIP introduces additional exception types specific to declarative reasoning:

**CircularDependencyError**
```python
a = b + 1
b = a + 1  # CircularDependencyError: Circular dependency detected
```
Raised when the dependency graph contains cycles that prevent deterministic evaluation.

**ConstraintViolationError**
```python
class Account:
    def __init__(self, balance):
        self.balance = balance


if Account.balance < 0:
    raise ConstraintViolationError("Balance cannot be negative")
```

```python
account1 = Account(100)
account1.balance = -50  # ConstraintViolationError: Balance cannot be negative
```

Raised when declarative constraints or assertions are violated during updates.

**PropagationError**
```python
PropagationError: Failed to propagate changes through dependency graph
```
Raised when dependency updates fail due to runtime errors during recalculation.

**GraphConsistencyError**
```python
GraphConsistencyError: Logic Graph entered inconsistent state
```
Raised when internal graph invariants are violated (indicates runtime bug).

### Transaction Rollback

When an exception occurs during statement execution, the transaction model ensures atomicity:

**Rollback Process**:
1. Exception is caught by the transaction handler
2. All graph modifications from the current transaction are reverted
3. Dependent values are restored to pre-transaction state
4. Persistence layer discards uncommitted changes
5. Exception is raised to the caller

**Example**:
```python
a = 5
b = a + 2  # b = 7

try:
    a = 10 / 0  # Raises ZeroDivisionError
except ZeroDivisionError:
    pass

# Graph state is unchanged: a = 5, b = 7
```

The Logic Graph remains in its pre-exception state, ensuring consistency even when errors occur.

### Exception Propagation

Exceptions propagate through the dependency graph during updates:

```python
def risky_operation(x):
    if x < 0:
        raise ValueError("x must be non-negative")
    return x * 2

a = 5
b = risky_operation(a)  # b = 10

a = -3  # Triggers recalculation of b
# ValueError is raised during propagation, transaction rolls back
# a remains 5, b remains 10
```

### Error Recovery

The runtime provides mechanisms for graceful error recovery:

**Try-Except in Declarative Context**
```python
try:
    risky_value = compute_risky()
except Exception:
    risky_value = default_value
```

**Conditional Guards**
```python
if safe_condition:
    result = potentially_failing_operation()
else:
    result = safe_fallback()
```

**Assertion-Based Validation**
```python
class ValidatedAccount:
    def __init__(self, balance):
        assert balance >= 0
        self.balance = balance
```

### Exception Handling Best Practices

1. **Validate at Boundaries**: Use assertions in `__init__` to validate instance creation
2. **Guard Dependencies**: Ensure dependent expressions cannot produce invalid states
3. **Use Conditional Logic**: Prevent exceptions through declarative conditions
4. **Handle External Calls**: Wrap external service calls (MCP, etc.) in try-except blocks
5. **Design for Rollback**: Ensure operations are atomic and can be safely reverted

### Debugging Support

When exceptions occur, the runtime provides:

- **Stack Traces**: Full call stack including dependency chain
- **Graph Context**: Information about graph state at time of error
- **Transaction Log**: Sequence of operations leading to the exception
- **Dependency Path**: Which dependencies triggered the failing update

This helps developers identify not just where the error occurred, but which declarative relationships caused the problematic update.
## Logic Graph

The Logic Graph is the central data structure that represents the complete state of the program, including all relationships between variables, objects, properties, and logical constraints. It is a directed graph where:

- **Nodes** represent program entities: variables, object instances, class definitions, functions, and conditional blocks
- **Edges** represent dependencies and data flow relationships between entities

### Graph Construction

The runtime constructs the Logic Graph incrementally as statements are received:

1. Each variable assignment creates a node for the variable
2. Dependencies in expressions create directed edges from referenced variables to the result variable
3. Object instantiations create nodes linked to their class definitions
4. Property assignments create edges from the object to its properties
5. Conditional statements create branching nodes with separate subgraphs for each branch
6. Class-level declarations create template edges that are inherited by all instances

Example:
```python
a = 1
b = a + 2
c = b * 3
```

This creates a graph: `a → b → c` where edges indicate dependency relationships.

### Automatic Propagation

When any node's value changes, the runtime:

1. Identifies all dependent nodes by traversing outgoing edges
2. Recalculates values in topological order to maintain consistency
3. Recursively updates all transitive dependencies
4. Applies conditional logic to determine which branches are active

This ensures that the entire system state remains consistent with all declared relationships without requiring explicit update logic.

### Graph Plasticity

The Logic Graph exhibits **plasticity**—the ability to modify its structure in response to new information. New statements can:

- Add new nodes and edges to the graph
- Redefine existing relationships by updating edge connections
- Introduce new constraints that affect multiple existing nodes
- Be integrated without requiring system restarts or recompilation

This adaptive capability is essential for neuro-symbolic systems where learned patterns must be dynamically incorporated into the reasoning framework.
## Logic Representation

The Logic Graph represents logical relationships using both first-order and higher-order logic constructs, allowing expression of complex reasoning patterns within standard Python syntax.

### First-Order Logic

First-order logic statements express relationships between specific objects and their properties. The NSAIP runtime interprets standard Python expressions as first-order logical assertions:

**Atomic Facts**: Direct property assignments represent atomic predicates
```python
human1.mortal = True  # mortal(human1)
```

**Relations**: Object references express binary and n-ary relations
```python
order1.item = item1  # item_of(order1, item1)
```

**Quantification**: The `.where()` and `.find()` methods express existential and universal quantification
```python
# ∃ user ∈ User : user.email = "jordan.miller@testmail.local"
User.where(lambda user: user.email == "jordan.miller@testmail.local")

# ∃! user ∈ User : user.email = "sarah.chen@dev-example.net"
User.find(lambda user: user.email == "sarah.chen@dev-example.net")
```

**Conditional Rules**: If-statements express logical implications
```python
if Account.balance < 1500:
    Account.status = "low"
# ∀ account ∈ Account : balance(account) < 1500 → status(account) = "low"
```

### High-Order Logic

Higher-order logic allows reasoning about properties, relations, and functions as first-class entities. NSAIP supports higher-order constructs through class-level declarations and computed properties:

**Properties as Functions**: Class-level computed properties define functions over all instances
```python
Order.total = Order.item.price * Order.qty
# total: Order → ℝ, where total(o) = price(item(o)) × qty(o)
```

**Abstraction over Relations**: Class-level conditionals define predicates that abstract over instance properties
```python
if Account.balance < 1500:
    Account.status = "low"
# Defines status: Account → String as a function of balance
```

**Composition**: Chained property access represents function composition
```python
Order.item.price  # Composition: Order → Item → Price
```

**Second-Order Quantification**: Lambda expressions in query methods allow quantification over predicates
```python
# Filter accepts a predicate (user → bool) as an argument
User.where(lambda user: user.email.endswith("@testmail.local"))
```

These higher-order constructs enable the system to express meta-level reasoning patterns where rules themselves can be parameterized, composed, and reasoned about within the Logic Graph.
## State Management

The NSAIP runtime employs an in-memory computing model where the complete program state resides in the Logic Graph. State management operates through a declarative model where the runtime automatically maintains consistency rather than requiring explicit state updates.

### In-Memory State Model

All program entities—variables, objects, class definitions, and logical relationships—exist as nodes in the Logic Graph maintained in memory. This provides:

- **Fast Access**: No I/O operations required for state queries
- **Consistent View**: All components see the same unified state
- **Automatic Synchronization**: Changes propagate immediately through the graph
- **Transactional Integrity**: Updates either complete fully or roll back entirely

### Declarative State Updates

State changes occur through declarative reassignment rather than imperative mutation:

```python
a = 1        # Initial state
b = a + 2    # Dependent state: b = 3

a = 2        # State update triggers propagation
             # b automatically becomes 4
```

The runtime:
1. Detects the assignment to `a`
2. Traverses the dependency graph to find `b`
3. Recalculates `b` using the new value of `a`
4. Recursively updates any further dependencies

### Instance State Tracking

Each object instance is tracked in the Logic Graph with edges to:
- Its class definition (for inherited properties)
- Its own property values
- Other objects it references
- Computed properties derived from its state

When an instance property changes, class-level rules automatically recompute affected derived properties:

```python
if Account.balance < 1500:
    Account.status = "low"

account1.balance = 2000  # status becomes "normal"
account1.balance = 1000  # status becomes "low"
```

### Transaction Model

Each statement execution constitutes a transaction:

1. **Parse**: Statement is analyzed for dependencies
2. **Execute**: Statement evaluates in current graph state
3. **Update**: Graph structure and values are modified
4. **Propagate**: Dependent values are recalculated
5. **Persist**: Transaction is committed to storage
6. **Rollback**: On error, all changes are reverted

This ensures the Logic Graph never enters an inconsistent state, maintaining logical integrity across all declarative constraints.

### Plasticity and Adaptation

The state model supports **dynamic reconfiguration** where new statements can modify the structure of the state space itself:

- Adding new classes extends the type system
- New class-level rules create additional computed properties on all instances
- Conditional logic can introduce context-dependent state transformations
- The dependency graph restructures itself to accommodate new relationships

This plasticity allows the system to learn and incorporate new patterns without requiring predefined schemas or restart procedures.

## Use Cases

### Socrates' Syllogism 

### Order Management

### Location

```python
x = 0
y = 0
```

<img width="542" height="542" alt="image" src="https://github.com/user-attachments/assets/7fa4345d-b3ac-4602-a29e-f057ea8dab30" />


```python
@declarative
if x > 2 or x < -2 or y > 2 or y < -2:
    raise Exception("Out of Boundary")
```

```python
@imperative
def move(direction):
    if direction is "up":
        y += 1
    elif direction is "down":
        y -= 1
    elif direction is "left":
        x -= 1
    elif direction is "right":
        x += 1
```

```python
move("up")
```

<img width="540" height="541" alt="image" src="https://github.com/user-attachments/assets/39f8815f-da07-4a2a-98cc-9f35c07c267d" />

```python
move("right")
```

<img width="541" height="540" alt="image" src="https://github.com/user-attachments/assets/36f1f235-0bfe-41a3-8f37-c86ee431d746" />

```python
move("up")
```

<img width="541" height="538" alt="image" src="https://github.com/user-attachments/assets/8aed1d43-c46a-4cb6-bf47-14bac7df4ce8" />


```python
move("up")
# Exception: Out of Boundary
```

### Location with Direction

```python
class Object:
    facing_direction = "up"

    def __init__(self):
        self.x = 0
        self.y = 0

    @imperative
    def turn(self, facing_direction):
        if facing_direction not in ["up", "down", "left", "right"]
            raise Exception("Wrong facing direction")
        self.facing_direction = facing_direction

    @imperative
    def move(self, heading_direction):
        if heading_direction is not self.facing_direction:
            raise Exception("Wrong heading direction")

        if heading_direction is "up":
            self.y += 1
        elif heading_direction is "down":
            self.y -= 1
        elif heading_direction is "left":
            self.x -= 1
        elif heading_direction is "right":
            self.x += 1
```

```python
object1 = Object()
object2 = Object()
```

<img width="546" height="541" alt="image" src="https://github.com/user-attachments/assets/35fce86e-748f-40b2-b1ba-73ad17bec689" />

```python
object1.turn("right")
```

<img width="542" height="541" alt="image" src="https://github.com/user-attachments/assets/12be6b64-30f6-469a-8dfc-97c849ae1314" />


```python
object1.move("right")
```

<img width="544" height="540" alt="image" src="https://github.com/user-attachments/assets/bd867a61-6f61-46c9-b143-150e32b1cabc" />

```python
object1.move("up")
# Exception: Wrong heading direction
```

