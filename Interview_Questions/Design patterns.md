# Design Pattern - Most Important MCQs

---

# 1. What is a Design Pattern?

**Answer:**

A reusable solution to a commonly occurring software design problem.

---

# 2. Why are Design Patterns used?

Answer:

- Reusable solutions
- Better code organization
- Easier maintenance
- Loose coupling
- Improved scalability

---

# 3. Design Patterns are invented or discovered?

✅ Answer:

Discovered.

---

# 4. Who wrote the Design Patterns book?

Answer:

Gang of Four (GoF)

- Erich Gamma
- Richard Helm
- Ralph Johnson
- John Vlissides

---

# 5. How many GoF Design Patterns are there?

✅ Answer:

23

---

# 6. Three Categories of Design Patterns

| Category | Purpose |
|----------|---------|
| Creational | Object Creation |
| Structural | Object Composition |
| Behavioral | Object Communication |

---

# 7. Which category does Singleton belong to?

✅ Answer:

Creational

---

# 8. Which category does Factory Method belong to?

✅ Answer:

Creational

---

# 9. Which category does Adapter belong to?

✅ Answer:

Structural

---

# 10. Which category does Decorator belong to?

✅ Answer:

Structural

---

# 11. Which category does Observer belong to?

✅ Answer:

Behavioral

---

# 12. Which category does Strategy belong to?

✅ Answer:

Behavioral

---

# Singleton Pattern

## What is Singleton?

Ensures only one instance of a class exists.

---

### Examples

- Logger
- Database Connection
- Configuration Manager
- Cache

---

### MCQ

Only one object should exist throughout the application.

A. Factory

B. Observer

C. Singleton

D. Adapter

✅ Answer: Singleton

---

# Factory Pattern

## Purpose

Creates objects without exposing creation logic.

---

### Example

Instead of

```text
new Car()
new Bike()
```

Use

```text
VehicleFactory.create()
```

---

### MCQ

Which pattern hides object creation?

A. Observer

B. Factory

C. Decorator

D. Strategy

✅ Answer: Factory

---

# Observer Pattern

## Purpose

One object notifies multiple dependent objects automatically.

---

Examples

- YouTube Notifications
- Facebook Notifications
- Stock Market Updates

---

### MCQ

Subscribers automatically receive updates.

A. Factory

B. Strategy

C. Observer

D. Singleton

✅ Answer: Observer

---

# Strategy Pattern

## Purpose

Allows changing an algorithm at runtime.

---

Examples

Payment

- Credit Card
- PayPal
- Stripe
- Cash

---

### MCQ

Changing algorithms without modifying client code.

A. Singleton

B. Factory

C. Strategy

D. Adapter

✅ Answer: Strategy

---

# Adapter Pattern

Purpose

Makes incompatible interfaces work together.

---

Example

USB-C to HDMI Adapter

---

### MCQ

Allows incompatible classes to work together.

A. Adapter

B. Factory

C. Observer

D. Singleton

✅ Answer: Adapter

---

# Decorator Pattern

Purpose

Adds new functionality without modifying existing code.

---

Example

Coffee

Basic Coffee

↓

Milk

↓

Chocolate

↓

Whipped Cream

---

### MCQ

Adds behavior dynamically.

A. Singleton

B. Decorator

C. Factory

D. Strategy

✅ Answer: Decorator

---

# MVC Pattern

MVC stands for

Model

View

Controller

---

Responsibilities

Model → Data

View → UI

Controller → Business Logic

---

### MCQ

Which component handles user interface?

A. Controller

B. View

C. Model

D. Service

✅ Answer: View

---

# SOLID Related

Which principle is achieved using Strategy Pattern?

Answer

Open/Closed Principle

---

# Dependency Injection

Dependency Injection helps achieve

✅ Loose Coupling

---

# Frequently Asked Theory Questions

## Difference between Factory and Singleton

| Factory | Singleton |
|----------|------------|
| Creates many objects | Only one object |
| Creation logic hidden | Single shared instance |

---

## Difference between Strategy and Observer

| Strategy | Observer |
|-----------|----------|
| Changes algorithm | Sends notifications |
| One active strategy | Multiple subscribers |

---

## Difference between Adapter and Decorator

| Adapter | Decorator |
|----------|-----------|
| Changes interface | Adds behavior |

---

# Quick Revision

- Singleton → One Instance
- Factory → Create Objects
- Observer → Notify Subscribers
- Strategy → Change Algorithm
- Adapter → Connect Incompatible Interfaces
- Decorator → Add Features
- MVC → Model, View, Controller
- DI → Loose Coupling
- GoF → 23 Patterns
- Categories → Creational, Structural, Behavioral