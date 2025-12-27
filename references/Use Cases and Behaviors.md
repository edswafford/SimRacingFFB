# Use Cases and Behaviors

This document captures the reasoning and clarification around **use cases and behaviors** in the context of **Kent Beck–style TDD**, specifically as it applies to building a Force Feedback (FFB) system.  They operate at **different levels of abstraction** and serve **different purposes**.

---

## Big Picture Comparison

| Concept      | Purpose                     | Scope          | Stability          |
| ------------ | --------------------------- | -------------- | ------------------ |
| **Use Case** | Describes *user intent*     | Coarse-grained | Relatively stable  |
| **Behavior** | Describes *system response* | Fine-grained   | Evolves during TDD |
| **Test**     | Verifies behavior           | Very narrow    | Highly volatile    |

---

## What a Use Case Really Is

A **use case** answers:

> *“What does the user want to accomplish?”*

Characteristics:

* User-facing
* Describes intent, not mechanics
* Independent of technology
* Often long-lived

### Example:

> “The driver experiences force feedback that reflects the car’s interaction with the track.”

This does **not** imply:

* telemetry formats
* update loops
* algorithms
* internal data structures

It is a *story*, not a design.

---

## What a Behavior Is

A **behavior** answers:

> *“How does the system respond to a stimulus?”*

Characteristics:

* Observable
* Testable
* Specific
* Mechanical rather than conceptual

### Example:

> “Given lateral acceleration (the primary force) increases, the output force feedback increases torque increases.”

This is something that can be tested without understanding *why* it happens.

---

## TDD operates **below the use-case level**.

Kent Beck’s TDD operates **below the use-case level**.

You do **not** test:

> “The driver feels realistic steering.”

You **do** test:

> “When lateral force increases, output torque increases.”

Behavior is the bridge between:

* Human intent (use cases)
* Executable tests (code)

---

## What a Requirement Is

The common mistake is treating **use cases as tests**.

For example:

> “The system supports force feedback.”

That’s a requirement — not a testable behavior.

Tests must be:

* Concrete
* Observable
* Repeatable

---

## Example Mapping (Domain → Behavior)

### Use Case:

> “The driver feels resistance when turning into a corner.”

### Core FFB Parameters

- Steering angle — wheel position
- Lateral force/acceleration — side forces during cornering (main steering resistance)
- Speed — scales force magnitude

The primary force is self-aligning torque from lateral forces, not just steering angle.

### Derived Behaviors:

* Given steering angle increases and laterial force, output torque increases.
* Given vehicle speed is zero, output torque is minimal.
* Given steering angle is zero, output torque is zero.

Each of these can become a test.