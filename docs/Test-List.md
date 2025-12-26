# Canon TDD – Key Alignment and Test List

This document captures the **alignment principles** and **behavioral test list** for the Force Feedback Racing system, following **Kent Beck’s Canon TDD**.

It is intentionally free of implementation details, architectural assumptions, and premature abstractions.

---

## 1. Key Alignment with Canon TDD

### 1.1 What We Are Optimizing For

* Behavior-first development
* Confidence through executable tests
* Incremental discovery of design
* Minimal assumptions about structure
* Freedom to refactor without fear

---

### 1.2 What the Test List Is (and Is Not)

#### The Test List **is**:

* A list of **behavioral expectations**
* A tool for thinking about **what should happen**
* A guide for selecting the *next test to write*
* Independent of implementation choices

#### The Test List **is not**:

* A design document
* A list of interfaces or classes
* A decomposition of the system architecture
* A checklist of methods or modules

---

### 1.3 Isolation and Abstraction

* Tests are isolated; production code does **not** have to be.
* Isolation exists to make behavior observable and repeatable.
* Abstractions emerge only when tests demand them.
* Dependency injection is a *tool*, not a goal.

---

### 1.4 What We Are Avoiding

* Premature abstraction
* Mock-heavy tests that mirror implementation
* Designing architecture before behavior exists
* Treating interfaces as requirements

---

### 1.5 Guiding Question

> **“What behavior should exist that does not exist yet?”**

Not:

> “What classes or interfaces should I create?”

---

## 2. Revised Test List (Canon TDD Style)

This list represents **behavioral expectations**, not implementation structure.
Items are unordered and intentionally coarse-grained.

---

### 2.1 Core Behavior

* When telemetry data is provided, the system produces force feedback output.
* When telemetry data changes, the output changes accordingly.
* When telemetry data does not change, the output remains stable.
* When no telemetry data is available, the system produces no force feedback.

---

### 2.2 Continuous Operation

* The system can process input repeatedly over time.
* The system can perform a single processing step.
* The system can start processing.
* The system can stop processing.
* The system can pause processing.
* The system does not produce output when stopped.
* The system does not produce output when paused.

---

### 2.3 Behavior Under Change

* The system responds when telemetry values change.
* The system responds when configuration values change.
* The system stabilizes when inputs stop changing.

---

### 2.4 Failure and Recovery

* The system does not crash when telemetry becomes unavailable.
* The system resumes normal operation when telemetry becomes available again.
* The system does not crash when output delivery fails.
* The system continues operating after a failure.

---

### 2.5 Predictability

* Given the same sequence of inputs, the system produces the same outputs.
* The system does not depend on hidden global state.
* The system behaves consistently across multiple runs.
* The system operates at a predictable update rate.
* The system operates at a configurable update rate.

---

### 2.6 Game and Wheel Independence

* The system determines which game to use as the telemetry source.
* The system determines which wheel to use for force feedback output.
* The core logic operates identically regardless of which game provides telemetry.
* The core logic operates identically regardless of which wheel receives output.
* Telemetry from different games produces the same behavior when normalized.
* Adding support for a new game does not require changes to core logic.
* Adding support for a new wheel does not require changes to core logic.

---

### 2.7 Explicit Non-Responsibilities

* The system does not require physical hardware.
* The system does not require a user interface.
* The system does not persist state between runs.
* The system does not manage hardware lifecycle.

---

## 3. Next Step (Canon TDD)

The next step is **not** to design or refactor.

The next step is to choose **one test** that:

* Forces the system to exist
* Is small enough to finish quickly
* Teaches something meaningful about the problem

> *“Given telemetry input, the system produces output.”*
> is often a good starting point — but the choice should be deliberate.

---

