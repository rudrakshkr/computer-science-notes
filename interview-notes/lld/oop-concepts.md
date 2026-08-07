# OOP Concepts (LLD)

## 1. Encapsulation

### Definition

Keep an object's data private and control access through methods.

### Goal

- Protect object state
- Enforce business rules
- Prevent invalid updates

### Example

```java
account.deposit(100);
account.withdraw(50);
```

Instead of:

```java
account.balance += 100;   ❌
```

### Interview Tip

Hide state, expose behavior.

---

## 2. Abstraction

### Definition

Expose **what** an object does, hide **how** it does it.

Usually achieved using:

- Interface
- Abstract class

### Example

```java
interface PaymentMethod {
    pay();
}
```

Implementations:

- CreditCard
- UPI
- PayPal

The caller only knows:

```java
payment.pay();
```

---

## 3. Polymorphism

### Definition

Different objects respond differently to the same method call.

### Example

```java
payment.pay();
```

Could execute:

- CreditCard.pay()
- UPI.pay()
- PayPal.pay()

without changing caller code.

### Avoid

```java
if(paymentType == "UPI") ...

else if(paymentType == "Card") ...
```

Use interfaces instead.

---

## 4. Inheritance

### Definition

One class extends another to reuse implementation.

```text
SavingsAccount
        ↑
BankAccount
```

### Good Use Case

When multiple classes genuinely share stable implementation.

Example:

- balance
- deposit()
- withdraw()

shared by all bank accounts.

---

## Problems with Inheritance

- Tight coupling
- Fragile base class
- Changes in parent affect children
- Hard to extend

---

## Prefer Composition Over Inheritance

Instead of:

```text
ElectricCar extends Car
```

Use:

```text
Car
 └── Drivetrain
        ├── GasEngine
        └── ElectricMotor
```

Behavior becomes replaceable.

---

# Interview Rules

✅ Encapsulation

- Private fields
- Public methods

---

✅ Abstraction

Use interfaces for varying behavior.

---

✅ Polymorphism

Program to interfaces.

Avoid type checks and switch statements.

---

✅ Inheritance

Use only for stable shared implementation.

Default choice:

> Composition + Interfaces

---