# Candidate Use Cases for SimRacingFFB

This document contains a candidate list of use cases derived from Marvin's AIRA analysis, generalized to work with all racing games (not just iRacing).

## Generalization Notes

The following iRacing-specific concepts have been generalized:
- **60Hz/360Hz telemetry**: Generalized to "read telemetry at game's native rate" and "process FFB at target rate"
- **SteeringWheelTorque_ST array**: Generalized to "read force feedback data from game"
- **iRacing SDK specifics**: Generalized to "game telemetry interface"

---

## Candidate Use Cases

### Core FFB Processing

1. **Driver feels resistance when turning into a corner**
   - Read force feedback data from game
   - Process through selected algorithm
   - Apply output adjustments
   - Send to steering wheel
   - *Status: Already exists in Use Cases folder*
   - *Source: Marvin's UC1 - Driver feels resistance when turning into a corner*

2. **System processes FFB at target rate (e.g., 360Hz)**
   - Read telemetry at game's native rate (varies by game)
   - Interpolate/upsample to target processing rate
   - Process algorithm at target rate
   - Apply all adjustments at target rate
   - Output to wheel at target rate
   - *Note: Generalizes iRacing's 60Hz→360Hz processing*
   - *Source: Marvin's UC13 - System processes FFB at 360Hz*

3. **System fades FFB in/out smoothly**
   - Fade in over configurable duration when FFB is enabled
   - Fade out over configurable duration when FFB is disabled
   - Prevent sudden jumps in force feedback
   - *Source: Marvin's UC14 - System fades FFB in/out smoothly*

### Protection and Safety

4. **Driver feels reduced force after a crash**
   - Detect high G-forces (longitudinal/lateral)
   - Activate crash protection timer
   - Scale down force feedback for configurable duration
   - Prevent excessive forces during crashes
   - *Source: Marvin's UC2 - Driver feels reduced force after a crash*

5. **Driver feels reduced force when hitting curbs**
   - Detect curb contact (via telemetry or track surface data)
   - Activate curb protection timer
   - Reduce detail/harsh effects temporarily
   - Smooth out harsh force spikes
   - *Source: Marvin's UC7 - Driver feels reduced force when hitting curbs*

6. **System auto-configures max force**
   - Track peak torque values during normal driving
   - Automatically set max force to peak + safety margin
   - Help users find optimal force settings
   - *Source: Marvin's UC15 - System auto-configures max force*

### Vibration Effects

7. **Driver feels vibration when understeering**
   - Calculate understeer condition (yaw rate vs expected)
   - Generate vibration using configurable pattern (sine, square, triangle, sawtooth)
   - Apply vibration at configurable frequency and strength
   - Optionally apply constant force effect
   - *Source: Marvin's UC3 - Driver feels vibration when understeering*

8. **Driver feels vibration when oversteering**
   - Calculate oversteer condition (yaw rate vs expected)
   - Generate vibration using configurable pattern
   - Apply vibration at configurable frequency and strength
   - Optionally apply constant force effect
   - *Source: Marvin's UC4 - Driver feels vibration when oversteering*

9. **Driver feels vibration when changing gears**
   - Detect gear changes from telemetry
   - Generate brief vibration pulse
   - Provide tactile feedback for gear shifts
   - *Source: Marvin's UC5 - Driver feels vibration when changing gears*

10. **Driver feels vibration when ABS activates**
    - Detect ABS activation from telemetry
    - Generate vibration pattern
    - Indicate brake lock-up prevention
    - *Source: Marvin's UC6 - Driver feels vibration when ABS activates*

11. **Driver feels seat-of-pants vibration**
    - Calculate seat-of-pants effect from vehicle dynamics
    - Generate vibration using configurable pattern
    - Provide additional tactile feedback
    - *Source: Marvin's Behaviors B5.3 (seat-of-pants vibration) - part of core FFB processing*

### Force Effects

12. **Driver feels soft lock at steering limits**
    - Read steering angle and maximum angle from telemetry
    - Apply increasing resistance as angle approaches maximum
    - Simulate mechanical steering limits
    - *Source: Marvin's UC8 - Driver feels soft lock at steering limits*

13. **Driver feels friction resistance**
    - Read wheel velocity from steering wheel device
    - Apply friction force proportional to velocity
    - Simulate mechanical resistance
    - *Source: Marvin's UC9 - Driver feels friction resistance*

14. **Driver feels centering force**
    - Read wheel position and velocity from steering wheel device
    - Apply centering force proportional to position and velocity
    - Help return wheel to center (when enabled)
    - *Source: Marvin's UC10 - Driver feels centering force*

15. **Driver feels reduced force when parked/moving slowly**
    - Detect low vehicle speed from telemetry
    - Reduce force feedback strength below threshold speed
    - Prevent unnecessary resistance when stationary
    - *Source: Marvin's UC11 - Driver feels reduced force when parked*

16. **Driver feels LFE (Low Frequency Effects)**
    - Read or calculate LFE magnitude (engine rumble, road texture, etc.)
    - Add LFE to force feedback output
    - Enhance immersion with low-frequency effects
    - *Source: Marvin's UC12 - Driver feels LFE effects*

### State Management

17. **System suspends FFB when appropriate**
    - Suspend when simulator's built-in FFB is enabled (if configured)
    - Suspend when vehicle is not on track
    - Suspend during replay/spectator mode
    - Prevent FFB when it shouldn't be active
    - *Source: Marvin's Behaviors B7.1-B7.3 (state management) - part of core FFB processing*

18. **System resets FFB device**
    - Reinitialize force feedback device on demand
    - Recover from device errors
    - Reset device state
    - *Source: Marvin's Behavior B7.6 (reset FFB device) - part of state management*

### Algorithm Processing

19. **System applies selected FFB algorithm**
    - Support multiple processing algorithms (native, detail booster, delta limiter, etc.)
    - Allow user to select algorithm
    - Process force feedback through selected algorithm
    - *Note: Specific algorithms can be separate use cases if needed*
    - *Source: Marvin's Behaviors B3.1-B3.6 (algorithm processing) - part of UC1 core FFB processing*

### Output Adjustments

20. **System applies output curve transformation**
    - Apply power curve to force feedback output
    - Allow user to adjust response curve
    - Customize force feedback feel
    - *Source: Marvin's Behavior B4.1 (output curve) - part of UC1 core FFB processing*

21. **System applies soft limiter**
    - Apply soft clipping to prevent hard saturation
    - Smooth out maximum force transitions
    - Prevent harsh cutoffs
    - *Source: Marvin's Behavior B4.2 (soft limiter) - part of UC1 core FFB processing*

22. **System applies output maximum/minimum limits**
    - Clamp output to maximum value (if less than 1.0)
    - Ensure minimum output magnitude (if greater than 0.0)
    - Provide fine-grained output control
    - *Source: Marvin's Behaviors B4.3-B4.4 (output max/min) - part of UC1 core FFB processing*

---

## Use Case Categories

### Must-Have (Core Functionality)
- Use Case 1: Driver feels resistance when turning into a corner
- Use Case 2: System processes FFB at target rate
- Use Case 17: System suspends FFB when appropriate

### Should-Have (Important Features)
- Use Case 3: System fades FFB in/out smoothly
- Use Case 4: Driver feels reduced force after a crash
- Use Case 19: System applies selected FFB algorithm
- Use Case 20: System applies output curve transformation

### Nice-to-Have (Enhancements)
- Use Case 5: Driver feels reduced force when hitting curbs
- Use Case 6: System auto-configures max force
- Use Case 7-11: Vibration effects (understeer, oversteer, gear change, ABS, seat-of-pants)
- Use Case 12-16: Force effects (soft lock, friction, centering, parked reduction, LFE)
- Use Case 18: System resets FFB device
- Use Case 21-22: Additional output adjustments

---

## Next Steps

1. Review and prioritize these candidate use cases
2. Select the simplest use case to start with (per Workflow.md)
3. Move selected use cases to individual folders under `docs/Use Cases/`
4. Update `docs/Use Cases/README.md` with selected use cases
5. Begin TDD workflow with first use case

---

## Notes

- These use cases are derived from Marvin's AIRA analysis but generalized for all racing games
- Game-specific implementations (iRacing SDK, Assetto Corsa shared memory, etc.) will be handled in the Infrastructure layer
- The core behaviors should be game-agnostic
- Processing rate (360Hz) is a target - actual rate may vary based on game capabilities and hardware


