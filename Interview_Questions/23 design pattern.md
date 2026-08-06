# Design Patterns - Complete Last-Minute Notes (MCQ + Interview)

> **Definition**
>
> A **Design Pattern** is a **reusable solution to a commonly occurring software design problem.**
>
> It is **not code**, but a **template or blueprint** for solving design problems.

---

# Why Use Design Patterns?

- Reusable solutions
- Reduce code duplication
- Improve code readability
- Loose coupling
- High maintainability
- Better scalability
- Follow best software engineering practices

---

# Gang of Four (GoF)

**GoF = Gang of Four**

The GoF introduced **23 Design Patterns** in their famous book.

### Authors

- Erich Gamma
- Richard Helm
- Ralph Johnson
- John Vlissides

---

# Categories of Design Patterns

There are **23 GoF Design Patterns**, divided into **3 categories**.

| Category | Number | Purpose |
|-----------|---------|----------|
| Creational | 5 | Object Creation |
| Structural | 7 | Organizing Classes & Objects |
| Behavioral | 11 | Communication Between Objects |

---

# 1. Creational Design Patterns (5)

**Purpose**

> Deal with **how objects are created.**

Instead of creating objects directly using `new`, these patterns provide flexible ways to create them.

---

## 1. Singleton

### Definition

Ensures **only one instance** of a class exists throughout the application.

### Real-Life Example

- Database Connection
- Logger
- Configuration Manager
- Cache

### Key Features

- Only one object
- Global access point
- Saves memory
- Prevents multiple instances

### Interview Keyword

> One Instance

---

## 2. Factory Method

### Definition

Creates objects **without exposing the object creation logic**.

### Real-Life Example

Vehicle Factory

Instead of

```text
new Car()
new Bike()
```

Use

```text
VehicleFactory.createVehicle()
```

### Key Features

- Hides object creation
- Loose coupling
- Easy to extend
- Object created based on input

### Interview Keyword

> Object Creation

---

## 3. Abstract Factory

### Definition

Creates **families of related objects** without specifying their concrete classes.

### Example

Windows UI Factory

Creates

- Windows Button
- Windows Checkbox

Linux UI Factory

Creates

- Linux Button
- Linux Checkbox

### Key Features

- Creates related objects
- Factory of factories
- Easy to switch product families

### Interview Keyword

> Family of Objects

---

## 4. Builder

### Definition

Builds **complex objects step by step**.

### Example

Building a House

- Foundation
- Walls
- Roof

Or

Building a Computer

- CPU
- RAM
- SSD

### Key Features

- Step-by-step construction
- Same process, different products
- Cleaner object creation

### Interview Keyword

> Step-by-Step Object Creation

---

## 5. Prototype

### Definition

Creates new objects by **copying an existing object**.

### Example

Duplicate an existing document.

### Key Features

- Uses cloning
- Faster than creating from scratch
- Saves initialization time

### Interview Keyword

> Clone Object

---

# Creational Pattern Summary

| Pattern | Easy Keyword |
|----------|--------------|
| Singleton | One Instance |
| Factory | Create Object |
| Abstract Factory | Family of Objects |
| Builder | Step-by-Step Build |
| Prototype | Clone Object |

---

# 2. Structural Design Patterns (7)

**Purpose**

> Organize classes and objects into larger structures.

---

## 1. Adapter

### Definition

Allows **incompatible interfaces** to work together.

### Example

USB-C to HDMI Adapter

### Key Features

- Converts one interface into another
- Makes incompatible classes compatible

### Interview Keyword

> Compatibility

---

## 2. Bridge

### Definition

Separates **abstraction** from **implementation**.

### Example

Remote Control

Remote

↓

TV / Radio

### Key Features

- Independent development
- Reduce dependency
- Flexible design

### Interview Keyword

> Separate Abstraction

---

## 3. Composite

### Definition

Treats **individual objects and groups of objects** the same way.

### Example

Folder

Folder

↓

Files

↓

Folders

### Key Features

- Tree structure
- Parent-child relationship

### Interview Keyword

> Tree Structure

---

## 4. Decorator

### Definition

Adds **new behavior dynamically** without modifying existing code.

### Example

Coffee

↓

Milk

↓

Chocolate

↓

Whipped Cream

### Key Features

- Adds functionality
- Does not modify original class
- Flexible alternative to inheritance

### Interview Keyword

> Add Features

---

## 5. Facade

### Definition

Provides **one simple interface** to a complex system.

### Example

Computer Start Button

Internally

- CPU
- Memory
- Disk
- BIOS

### Key Features

- Simplifies complex systems
- Hides internal complexity

### Interview Keyword

> Simple Interface

---

## 6. Flyweight

### Definition

Shares objects to **save memory**.

### Example

Game

Thousands of trees share one tree model.

### Key Features

- Shared objects
- Reduces memory usage

### Interview Keyword

> Memory Optimization

---

## 7. Proxy

### Definition

Acts as a **placeholder or representative** for another object.

### Example

Loading large image

Image loaded only when needed.

### Key Features

- Controls access
- Lazy loading
- Security

### Interview Keyword

> Control Access

---

# Structural Pattern Summary

| Pattern | Easy Keyword |
|----------|--------------|
| Adapter | Compatibility |
| Bridge | Separate Implementation |
| Composite | Tree Structure |
| Decorator | Add Features |
| Facade | Simple Interface |
| Flyweight | Save Memory |
| Proxy | Control Access |

---

# 3. Behavioral Design Patterns (11)

**Purpose**

> Define **how objects communicate and interact**.

---

## 1. Chain of Responsibility

### Definition

Passes a request through multiple handlers.

### Example

Customer Support Levels

Level 1

↓

Level 2

↓

Manager

### Keyword

Pass Request

---

## 2. Command

### Definition

Encapsulates a request as an object.

### Example

Undo / Redo

### Keyword

Request Object

---

## 3. Interpreter

### Definition

Defines grammar for interpreting language.

### Example

SQL Parser

Calculator

### Keyword

Language Parser

---

## 4. Iterator

### Definition

Access collection elements one by one.

### Example

Array Traversal

### Keyword

Sequential Access

---

## 5. Mediator

### Definition

Objects communicate through a mediator instead of directly.

### Example

Air Traffic Controller

### Keyword

Central Communication

---

## 6. Memento

### Definition

Stores previous state to restore later.

### Example

Undo in Text Editor

### Keyword

Save State

---

## 7. Observer

### Definition

One object automatically notifies many dependent objects.

### Example

- YouTube Notifications
- Facebook Notifications
- Stock Updates

### Key Features

- One-to-many relationship
- Automatic notification
- Event-driven programming

### Interview Keyword

> Notification

---

## 8. State

### Definition

Changes behavior when object's state changes.

### Example

Traffic Light

Red

↓

Green

↓

Yellow

### Keyword

State-Based Behavior

---

## 9. Strategy

### Definition

Allows changing an algorithm at runtime.

### Example

Payment Methods

- Cash
- Card
- PayPal

### Key Features

- Replace algorithms easily
- Open for extension
- No if-else chains

### Interview Keyword

> Change Algorithm

---

## 10. Template Method

### Definition

Defines algorithm structure while allowing subclasses to customize some steps.

### Example

Tea and Coffee preparation.

### Keyword

Algorithm Template

---

## 11. Visitor

### Definition

Adds new operations without changing object classes.

### Example

Tax calculation on different products.

### Keyword

Add Operations

---

# Behavioral Pattern Summary

| Pattern | Easy Keyword |
|----------|--------------|
| Chain of Responsibility | Pass Request |
| Command | Request Object |
| Interpreter | Language Parser |
| Iterator | Sequential Access |
| Mediator | Central Communication |
| Memento | Save State |
| Observer | Notification |
| State | Change Behavior |
| Strategy | Change Algorithm |
| Template Method | Algorithm Template |
| Visitor | Add Operations |

---

# ⭐ Most Important Patterns for Freshers

| Pattern | Importance |
|----------|------------|
| Singleton | ⭐⭐⭐⭐⭐ |
| Factory Method | ⭐⭐⭐⭐⭐ |
| Strategy | ⭐⭐⭐⭐⭐ |
| Observer | ⭐⭐⭐⭐⭐ |
| Dependency Injection *(not GoF)* | ⭐⭐⭐⭐⭐ |
| Facade | ⭐⭐⭐⭐ |
| Adapter | ⭐⭐⭐⭐ |
| Decorator | ⭐⭐⭐⭐ |
| MVC *(Architectural Pattern)* | ⭐⭐⭐⭐ |

---

# Common MCQs

### Q1. How many GoF Design Patterns are there?

✅ **23**

---

### Q2. What are the three categories?

- Creational
- Structural
- Behavioral

---

### Q3. Which pattern ensures only one instance?

✅ **Singleton**

---

### Q4. Which pattern hides object creation?

✅ **Factory Method**

---

### Q5. Which pattern clones an object?

✅ **Prototype**

---

### Q6. Which pattern adds functionality without modifying the original class?

✅ **Decorator**

---

### Q7. Which pattern makes incompatible interfaces work together?

✅ **Adapter**

---

### Q8. Which pattern provides one simple interface to a complex subsystem?

✅ **Facade**

---

### Q9. Which pattern sends notifications to multiple objects?

✅ **Observer**

---

### Q10. Which pattern changes an algorithm at runtime?

✅ **Strategy**

---

### Q11. Which pattern controls access to another object?

✅ **Proxy**

---

### Q12. Which pattern builds a complex object step by step?

✅ **Builder**

---

### Q13. Which pattern creates families of related objects?

✅ **Abstract Factory**

---

# One-Minute Revision

| Pattern | Remember This |
|----------|---------------|
| Singleton | One instance |
| Factory Method | Create object |
| Abstract Factory | Family of objects |
| Builder | Step-by-step build |
| Prototype | Clone object |
| Adapter | Compatibility |
| Bridge | Separate abstraction and implementation |
| Composite | Tree structure |
| Decorator | Add features dynamically |
| Facade | Simple interface |
| Flyweight | Save memory |
| Proxy | Control access |
| Chain of Responsibility | Pass request |
| Command | Request as object |
| Interpreter | Parse language |
| Iterator | Traverse collection |
| Mediator | Central communication |
| Memento | Save previous state |
| Observer | Notify subscribers |
| State | Behavior depends on state |
| Strategy | Change algorithm |
| Template Method | Algorithm skeleton |
| Visitor | Add new operations |