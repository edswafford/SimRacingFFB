# First Iteration — Bare-bones Skeleton

This working document captures the minimal scope for the first development iteration. It is intentionally small: a vertical slice that proves the core FFB processing loop, the processing rate, and basic state handling. This file is a working artifact and may be archived after the project is complete.

## Selected Use Cases

- **Use Case 1: Driver feels resistance when turning into a corner**
    - Source: Marvin's UC1 - Driver feels resistance when turning into a corner
    - Goal: Implement an end-to-end vertical slice: read game FFB/telemetry → process algorithm → output to wheel
    - Success criteria:
        - The app reads FFB/telemetry from at least one game source or simulator stub
        - The app produces a processed torque value and writes it to a wheel API (or a device stub)
- **Use Case 2: FFB output updates at a consistent, configurable rate**
    - Source: Marvin's UC13
    - Goal: Ensure the driver experiences smooth and stable force feedback by processing FFB at a consistent target rate.
    - Success criteria:
        - The application produces FFB output at a configured target rate (e.g., 360Hz or a simulated equivalent).
        - When telemetry arrives at a lower rate, values are interpolated or held such that output timing remains stable.
        - Output timing behavior can be observed or verified via a test harness or instrumentation.
- **Use Case 17: FFB output is suppressed when driving context is invalid**
    - Source: Marvin's Behaviors B7.1-B7.3
    - Goal: Prevent unintended or unsafe force feedback when the driver is not in a valid driving context.
    - Success criteria:
        - When the game reports an invalid driving state (e.g., not on track, replay mode, or native FFB active):
          - No force feedback is sent to the wheel.
        - When the state returns to valid:
          - FFB output resumes automatically.
        - State transitions are observable via logs, events, or test assertions.

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