# First Iteration — Bare-bones Skeleton

This working document captures the minimal scope for the first development iteration. It is intentionally small: a vertical slice that proves the core FFB processing loop, the processing rate, and basic state handling. This file is a working artifact and may be archived after the project is complete.

## Selected Use Cases

- **Use Case 1: Driver feels resistance when turning into a corner**
  - Source: Marvin's UC1 - Driver feels resistance when turning into a corner
  - Goal: Implement an end-to-end vertical slice: read game FFB/telemetry → process algorithm → output to wheel
  - Success criteria:
    - The app reads FFB/telemetry from at least one game source or simulator stub
    - The app produces a processed torque value and writes it to a wheel API (or a device stub)

- **Use Case 2: System processes FFB at target rate (e.g., 360Hz)**
  - Source: Marvin's UC13 - System processes FFB at 360Hz
  - Goal: Establish a timed processing loop that consumes game telemetry and produces FFB at a target rate (with interpolation/upsampling as needed)
  - Success criteria:
    - A processing loop runs at the configured target rate (or a reasonable simulated rate for the prototype)
    - Telemetry arriving at a lower native rate is accepted and interpolated into the processing loop

- **Use Case 17: System suspends FFB when appropriate**
  - Source: Marvin's Behaviors B7.1-B7.3 (state management)
  - Goal: Implement basic state checks to avoid sending FFB when inappropriate (e.g., not on track, in replay, or when game FFB is already active and configured to disable external FFB)
  - Success criteria:
    - The system honors a simple set of state flags and suspends/resumes output accordingly

## Implementation Notes (first iteration)

- Use a simulator stub or a single supported game interface for telemetry input to keep the prototype small.
- Output to a device stub (or existing wheel API) to validate end-to-end behavior without needing full device drivers.
- Keep the algorithm simple (e.g., normalization and optional curve) to exercise the pipeline; complex algorithms can be added in later iterations.
- Document any assumptions and interfaces in this file so future iterations can extend them.

---

## Next Actions

1. Create a minimal telemetry source (simulator stub) that emits FFB/telemetry at a configurable native rate.
2. Implement the processing loop with configurable target rate and a simple interpolation strategy.
3. Implement a basic output adapter that writes processed torque to a device stub.
4. Add state flags and logic to suspend/resume output.

---

*This is a working document intended to be updated as the first iteration progresses.*

