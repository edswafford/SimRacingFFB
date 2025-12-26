# Simulator Racing Force Feedback Racing – Requirements

This document defines the **requirements** for a Force Feedback (FFB) application used with racing simulator games.  

It is intentionally written to support **Kent Beck–style Test-Driven Development (TDD)** by ensuring behaviors are clear, testable, and decomposable.

---

## 1. Functional Requirements

### 1.1 Multi-Game Support

- The application shall support multiple racing simulator games (e.g., iRacing, Assetto Corsa, Automobilista 2).
- The core force feedback logic shall not depend on which game is providing telemetry data.
- The core logic shall operate identically regardless of the source game.
- Adding support for a new game shall not require changes to the core force feedback logic.
- Modifying support for one game shall not affect other games.

---

### 1.2 Multi-Wheel Support

- The application shall support multiple force feedback steering wheels (e.g., Simucube, Logitech G29, Moza, Simagic).
- Each wheel implementation shall be isolated behind a common interface.
- The application shall not depend on vendor-specific APIs outside of the wheel adapter layer.

---

### 1.3 Force Feedback Configuration

- The application shall allow users to configure force feedback parameters.
- Configuration values shall be readable and changeable at runtime.
- Configuration shall be independent of both game and wheel implementations.

---

### 1.4 Telemetry Input

- The application shall determine which racing game to use as the telemetry source.
- The application shall read telemetry data from the active racing game.
- Telemetry data shall be represented in a normalized internal format.
- Telemetry reading shall not depend on force feedback output or wheel state.

---

### 1.5 Force Feedback Signal Generation

- The application shall generate a force feedback signal using:
    - Telemetry data
    - User-defined configuration
- The force feedback calculation shall be deterministic for a given telemetry and configuration input.
- Force feedback generation shall not depend on wheel-specific implementation details.

---

### 1.6 Force Feedback Output

- The application shall determine which steering wheel to use for force feedback output.
- The application shall send the generated force feedback signal to the active steering wheel.
- The output process shall not modify the force feedback signal.
- Failures in sending force feedback shall be detectable and reportable.

---

### 1.7 Processing Loop (Core Behavior)

- The application shall operate in a continuous loop executing the following steps in order:

  1. Read telemetry data from the active game  
  2. Generate a force feedback signal  
  3. Send the force feedback signal to the active wheel  
- The loop shall be controllable (start, stop, pause).
- The loop shall operate at a predictable and configurable update rate.

---

## 2. Non-Functional Requirements

### 2.1 Determinism & Testability

- Core logic shall be deterministic and testable without hardware or games present.
- External dependencies shall be replaceable with test doubles.

---

### 2.2 Performance Boundaries

- The system shall support high-frequency update loops (e.g., ≥ 360 Hz) without architectural changes.
- Performance considerations shall not leak into domain logic.

---

### 2.3 Error Handling

- The system shall fail gracefully when telemetry data is unavailable or invalid.
- The system shall continue running even if force feedback output fails.

---

### 2.4 Platform Scope

- The application shall initially target Windows only.
- Cross-platform support is explicitly out of scope.

---

## 3. Explicit Non-Requirements

The following are intentionally excluded from the current scope:

- User interface or user experience design
- Persistence (profiles, presets, or saved configurations)
- Networking or remote control
- Multiplayer or session awareness
- Game-specific tuning logic
- Hardware discovery or auto-detection

---

## 4. System Boundaries

### The system:

- Transforms telemetry into force feedback signals
- Coordinates timing and control flow
- Abstracts game and hardware integrations

### The system does NOT:

- Decide what “feels good”
- Optimize performance prematurely
- Manage device drivers
- Render graphics or UI

---