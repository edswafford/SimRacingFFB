<h1 align="center">Core Force Feedback Behaviors</h1>
<h2 align="center">Driver Feels Resistance When Turning Into a Corner</h2>



## 1. Steering Input Produces Resistance

**Given** a non-zero steering angle

**And** non-zero lateral force

**When** the system processes input

**Then** the output torque is non-zero

> Establishes that resistance exists when turning under load.

---

## 2. Resistance Increases with Lateral Force

**Given** steering angle remains constant

**And** lateral force increases

**When** the system processes input

**Then** output torque increases

> Models increasing tire load producing stronger self-aligning torque.

---

## 3. Resistance Increases with Steering Angle

**Given** lateral force remains constant

**And** steering angle increases

**When** the system processes input

**Then** output torque increases

> Represents increased driver effort as steering input increases.

---

## 4. No Lateral Force Produces Minimal Resistance

**Given** lateral force is zero

**When** the system processes input

**Then** output torque is minimal

> Straight-line motion produces little or no self-aligning torque.

---

## 5. No Steering Input Produces No Resistance

**Given** steering angle is zero

**When** the system processes input

**Then** output torque is zero

> No steering input means no steering resistance.

---

## 6. Speed Scales Resistance

**Given** steering angle and lateral force remain constant

**And** vehicle speed increases

**When** the system processes input

**Then** output torque increases

> Higher speed amplifies steering forces.

---

## 7. Stable Input Produces Stable Output

**Given** steering angle, lateral force, and speed remain constant

**When** the system processes input repeatedly

**Then** output torque remains stable

> Prevents oscillation, jitter, or unintended variation.

---

## 8. Direction Is Preserved

**Given** lateral force direction reverses

**When** the system processes input

**Then** output torque reverses direction

> Ensures left/right steering feedback matches physical reality.

---

## Notes

* These behaviors are **deliberately independent of implementation**.
* They are **observable**, **testable**, and **domain-meaningful**.
* They define *what must happen*, not *how it happens*.
* They form a stable foundation for incremental TDD.