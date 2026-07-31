# Low-Level Design - Delivery Framework

> Source: Hello Interview - Low-Level Design in a Hurry

---

## Interview Flow

```text
Requirements
      ↓
Entities & Relationships
      ↓
Class Design
      ↓
API Design
      ↓
Workflow
      ↓
Deep Dive
```

The goal is to progressively refine the design instead of jumping straight into code.

---

# 1. Requirements (~5 min)

## Goal

Turn a vague prompt into a clear problem statement.

### Ask Questions About

- Functional requirements
- Out-of-scope features
- Constraints
- Assumptions
- Edge cases (only if important)

### Example

**Prompt:** Design a Parking Lot

Possible questions:

- Should it support multiple floors?
- What vehicle types are supported?
- Can a spot fit different vehicle sizes?
- Is payment in scope?

> 💡 Never start designing before clarifying the requirements.

---

# 2. Entities & Relationships (~3 min)

## Goal

Identify the main objects in the system and how they relate.

### Identify Entities

Extract the important nouns from the requirements.

Examples:

- ParkingLot
- Floor
- ParkingSpot
- Vehicle
- Ticket

An entity usually:

- Has state
- Has behavior
- Represents an important concept

---

### Define Relationships

Determine how entities interact.

Think about:

- Ownership
- Composition
- Association
- Dependencies

Example:

```text
ParkingLot
 ├── Floors
 │     └── ParkingSpots
 │
 └── Tickets
```

> 💡 Don't over-model. Focus on the core relationships first.

---

# 3. Class Design (~10–15 min)

## Goal

Design each class with clear responsibilities.

For every class, define:

### State

What information does it store?

Example:

```text
Vehicle
--------
licensePlate
vehicleType
```

---

### Behavior

What actions can it perform?

Example:

```text
park()
unpark()
isAvailable()
assignVehicle()
```

---

### Responsibility

Ask yourself:

- What should this class do?
- What should it **not** do?

Follow the **Single Responsibility Principle (SRP)** whenever possible.

---

# 4. API Design

Define how objects communicate.

Focus on meaningful methods.

Example:

```java
Ticket parkVehicle(Vehicle vehicle);

void removeVehicle(Ticket ticket);

List<ParkingSpot> getAvailableSpots();
```

Keep APIs:

- Small
- Clear
- Intentional

---

# 5. Workflow

Walk through a complete use case.

Example:

```text
Vehicle arrives
        ↓
ParkingLot receives request
        ↓
Find available spot
        ↓
Assign spot
        ↓
Generate ticket
        ↓
Return ticket
```

Walking through the flow helps identify missing classes or responsibilities.

---

# 6. Deep Dive

Interviewers may ask follow-up questions.

Examples:

- How would you make it thread-safe?
- How would you add reservations?
- How would you support electric vehicles?
- How would you improve extensibility?

Don't redesign everything.

Extend the existing design.

---

# Common Interview Tips

- Clarify before coding.
- Keep responsibilities separate.
- Prefer composition over inheritance when appropriate.
- Explain trade-offs.
- Design for future extension.

---

# Quick Revision Checklist

## ✅ Requirements

- Clarify requirements
- Define scope
- Make assumptions explicit

---

## ✅ Entities & Relationships

- Extract entities
- Define ownership
- Keep relationships simple

---

## ✅ Class Design

- Define state
- Define behavior
- Assign responsibilities

---

## ✅ API Design

- Small, meaningful methods
- Clear naming

---

## ✅ Workflow

- Walk through one complete scenario
- Validate the design

---

## ✅ Deep Dive

- Handle follow-up questions
- Extend instead of redesigning

---

# Interview Mantra

```text
Clarify
    ↓
Identify Entities
    ↓
Define Relationships
    ↓
Design Classes
    ↓
Design APIs
    ↓
Walk Through Workflow
    ↓
Handle Follow-ups
```