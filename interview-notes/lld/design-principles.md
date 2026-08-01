# LLD - Design Principles

> Design principles help you make clean, maintainable, and extensible design decisions during LLD interviews.

---

# General Design Principles

## KISS (Keep It Simple, Stupid)

**Idea**

Choose the simplest solution that solves the current problem.

### Don't

- Introduce design patterns too early.
- Create unnecessary abstractions.
- Over-engineer.

### Do

- Start simple.
- Add complexity only when the design actually needs it.

> ⭐ The most commonly violated principle in LLD interviews.

---

## DRY (Don't Repeat Yourself)

**Idea**

Avoid duplicating the same logic in multiple places.

### Benefits

- Easier maintenance
- One place to fix bugs
- Better readability

### Don't Overdo It

Sometimes duplicating a few lines is simpler than introducing an unnecessary abstraction.

Balance **DRY** with **KISS**.

---

## YAGNI (You Aren't Gonna Need It)

**Idea**

Build for today's requirements, not tomorrow's guesses.

### Don't

- Add features "just in case."
- Build for imaginary requirements.

### Do

- Implement only what's required.
- Extend the design when new requirements actually arrive.

---

## Separation of Concerns

Each part of the system should have one clear responsibility.

Example:

- UI → Display
- Business Logic → Rules
- Database Layer → Data storage

Keep these responsibilities separate.

---

## Law of Demeter

Each object should communicate only with its **direct dependencies**.

Avoid chaining multiple method calls.

❌ Bad

```java
order.getCustomer().getAddress().getCity();
```

✅ Better

```java
order.getCustomerCity();
```

**Benefits**

- Reduces coupling
- Better encapsulation
- Easier to maintain

---

# SOLID Principles

## S — Single Responsibility Principle (SRP)

A class should have **one reason to change**.

❌ Bad

```text
Report
├── Generate Report
├── Convert to PDF
└── Save to Database
```

✅ Better

```text
ReportGenerator
PdfFormatter
ReportRepository
```

---

## O — Open/Closed Principle (OCP)

Software should be:

- Open for extension
- Closed for modification

Add new behavior by extending existing code instead of changing working code.

Example:

Add a new payment method by creating another implementation instead of modifying existing payment logic.

---

## L — Liskov Substitution Principle (LSP)

A child class should be usable anywhere its parent class is expected.

If replacing a parent with a child breaks the program, LSP is violated.

---

## I — Interface Segregation Principle (ISP)

Prefer many small, focused interfaces over one large interface.

Clients should not depend on methods they don't use.

---

## D — Dependency Inversion Principle (DIP)

Depend on **abstractions**, not concrete implementations.

Instead of:

```text
OrderService
    ↓
StripePayment
```

Prefer:

```text
OrderService
    ↓
PaymentProcessor
       ↑
StripePayment
PayPalPayment
```

This makes the system easier to extend and test.

---

# Interview Mindset

Don't try to apply every principle.

Ask yourself:

- Is this making the design simpler?
- Am I solving a real problem?
- Am I adding unnecessary complexity?

Good design is about **trade-offs**, not blindly following rules.

---

# 1-Minute Revision

- ✅ KISS → Start simple.
- ✅ DRY → Avoid duplicate logic.
- ✅ YAGNI → Build only what's needed.
- ✅ Separation of Concerns → Keep responsibilities separate.
- ✅ SRP → One reason to change.
- ✅ OCP → Extend, don't modify.
- ✅ LSP → Child should replace parent safely.
- ✅ ISP → Small focused interfaces.
- ✅ DIP → Depend on abstractions.