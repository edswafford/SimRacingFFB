# Marvin's AIRA FFB Analysis

This document analyzes Marvin's AIRA refactored codebase to extract FFB behaviors and synthesize use cases for the SimRacingFFB project.

## Core FFB Processing Flow

The FFB processing follows this high-level flow:

1. **Telemetry Input** (60Hz from iRacing SDK)

   - Reads `SteeringWheelTorque_ST` array (6 samples per frame = 360Hz equivalent)
   - Reads other telemetry: steering angle, speed, accelerations, gear, ABS, etc.
1. **Torque Buffer Update** (when new data arrives)

   - Copies 6 samples into internal 360Hz buffer
   - Interpolates between samples for smooth 360Hz+ processing
1. **Algorithm Processing** (360Hz+)

   - Applies selected algorithm to transform input torque
   - Algorithms: Native60Hz, Native360Hz, DetailBooster, DeltaLimiter, SlewAndTotalCompression, MultiAdjustmentToolkit
1. **Output Adjustments** (360Hz+)

   - Output curve
   - Soft limiter
   - Output maximum/minimum
   - Crash protection scaling
   - Curb protection scaling
   - Parked strength reduction
   - LFE addition
   - Soft lock
   - Friction
   - Centering force
1. **Vibration Effects** (360Hz+)

   - Understeer vibration
   - Oversteer vibration
   - Seat-of-pants vibration
   - Gear change vibration
   - ABS vibration
1. **Fade In/Out** (when starting/stopping)

   - Smooth transitions when FFB is enabled/disabled
1. **Output to Wheel** (360Hz+)

   - Sends final torque value to DirectInput force feedback device

## Synthesized Use Cases

### Use Case 1: Driver feels resistance when turning into a corner

**Behaviors:**

- B1.1, B1.2: Read steering wheel torque and supporting telemetry
- B2.1, B2.2: Interpolate torque for smooth processing
- B3.1-B3.6: Apply selected algorithm to process torque
- B4.1-B4.4: Apply output adjustments (curve, limiter, max/min)
- B7.1-B7.3: Ensure FFB is active (not suspended)
- B7.4: Fade in if just starting
- B8.3: Track peak torque for reference

**Description:** When the driver turns the steering wheel, the system reads telemetry, processes it through the selected algorithm, applies output adjustments, and sends force feedback to the wheel.

### Use Case 2: Driver feels reduced force after a crash

**Behaviors:**

- B8.1: Detect high G-forces
- B4.5: Apply crash protection scaling
- B7.1-B7.3: Maintain FFB state

**Description:** When the vehicle experiences high G-forces (crash), the system activates crash protection and reduces force feedback for a configurable duration.

### Use Case 3: Driver feels vibration when understeering

**Behaviors:**

- B1.2: Read telemetry (steering angle, yaw rate, speed)
- B5.1: Generate understeer vibration
- B6.1: Apply understeer constant force (if configured)
- B4.1-B4.4: Apply output adjustments

**Description:** When the vehicle is understeering (yaw rate below expected), the system generates vibration and optionally applies constant force effects.

### Use Case 4: Driver feels vibration when oversteering

**Behaviors:**

- B1.2: Read telemetry
- B5.2: Generate oversteer vibration
- B6.2: Apply oversteer constant force (if configured)
- B4.1-B4.4: Apply output adjustments

**Description:** When the vehicle is oversteering (yaw rate above expected), the system generates vibration and optionally applies constant force effects.

### Use Case 5: Driver feels vibration when changing gears

**Behaviors:**

- B1.2: Read gear telemetry
- B5.4: Generate gear change vibration
- B4.1-B4.4: Apply output adjustments

**Description:** When the driver changes gears, the system generates a brief vibration pulse to provide tactile feedback.

### Use Case 6: Driver feels vibration when ABS activates

**Behaviors:**

- B1.2: Read ABS status
- B5.5: Generate ABS vibration
- B4.1-B4.4: Apply output adjustments

**Description:** When ABS activates, the system generates vibration to indicate brake lock-up prevention.

### Use Case 7: Driver feels reduced force when hitting curbs

**Behaviors:**

- B1.2: Read track surface telemetry
- B8.2: Activate curb protection
- B4.6: Apply curb protection (reduces detail effects)
- B4.1-B4.4: Apply output adjustments

**Description:** When the vehicle contacts curbs, the system activates curb protection to reduce harsh force feedback spikes.

### Use Case 8: Driver feels soft lock at steering limits

**Behaviors:**

- B1.2: Read steering angle and max angle
- B4.9: Apply soft lock force
- B4.1-B4.4: Apply output adjustments

**Description:** When the steering wheel approaches its maximum angle, the system applies increasing resistance to simulate mechanical limits.

### Use Case 9: Driver feels friction resistance

**Behaviors:**

- B1.2: Read wheel velocity (from DirectInput)
- B4.10: Apply friction force
- B4.11: Apply parked friction (if parked)
- B4.1-B4.4: Apply output adjustments

**Description:** The system applies friction force proportional to wheel velocity to simulate mechanical resistance.

### Use Case 10: Driver feels centering force

**Behaviors:**

- B1.2: Read wheel position and velocity
- B4.12: Apply centering force
- B4.1-B4.4: Apply output adjustments

**Description:** When enabled, the system applies a centering force to help return the wheel to center position.

### Use Case 11: Driver feels reduced force when parked

**Behaviors:**

- B1.2: Read speed telemetry
- B4.7: Apply parked strength reduction
- B4.1-B4.4: Apply output adjustments

**Description:** When the vehicle is moving slowly (below 5 MPH), the system reduces force feedback strength to prevent unnecessary resistance.

### Use Case 12: Driver feels LFE effects

**Behaviors:**

- B1.2: Read LFE magnitude (from LFE component)
- B4.8: Add LFE to torque
- B4.1-B4.4: Apply output adjustments

**Description:** The system adds low-frequency effects (e.g., engine rumble, road texture) to the force feedback output.

### Use Case 13: System processes FFB at 360Hz

**Behaviors:**

- B1.1: Read 60Hz telemetry
- B1.3: Update torque buffer
- B2.1: Interpolate to 360Hz+
- B3.1-B3.6: Process algorithm
- B4.1-B4.12: Apply all adjustments
- B5.1-B5.6: Add vibration effects
- B6.1-B6.3: Add constant force effects
- Output to wheel at 360Hz+

**Description:** The system maintains a 360Hz+ processing loop, reading 60Hz telemetry, interpolating to higher rates, and outputting smooth force feedback.

### Use Case 14: System fades FFB in/out smoothly

**Behaviors:**

- B7.4: Fade in over 2 seconds
- B7.5: Fade out over 0.5 seconds
- B4.1-B4.4: Apply output adjustments

**Description:** When FFB is enabled or disabled, the system smoothly fades the force feedback to prevent sudden jumps.

### Use Case 15: System auto-configures max force

**Behaviors:**

- B8.3: Track peak torque
- B7.7: Auto-set max force to peak + margin

**Description:** The system can automatically determine and set the maximum force setting based on observed peak torque values.

## Extracted Behaviors

Most behaviors are in RacingWheel.cs in the Update or ProcessAlgorithm methods. A few reference other files like DirectInput.cs for device initialization and reset operations.

### Behavior Group 1: Telemetry Reading

**B1.1: Read steering wheel torque telemetry**

Source: `RacingWheel.cs`, `Update` method (accessed via `app.Simulator.SteeringWheelTorque_ST`)

Given: iRacing is connected and providing telemetry

When: New telemetry frame arrives (60Hz)

Then: System reads `SteeringWheelTorque_ST` array containing 6 samples

---

**B1.2: Read supporting telemetry data**

Source: `RacingWheel.cs`, `Update` method (accessed via `app.Simulator.*` properties)

Given: iRacing is connected

When: New telemetry frame arrives

Then: System reads steering angle, speed, accelerations, gear, ABS status, track surface, on-track status

---

**B1.3: Update torque buffer when new data arrives**

Source: `RacingWheel.cs`, `Update` method, lines 809-837

Given: New telemetry data is available

When: `UpdateSteeringWheelTorqueBuffer` flag is set

Then: System copies 6 samples into internal 360Hz buffer array

### Behavior Group 2: Torque Interpolation

**B2.1: Interpolate torque samples for 360Hz processing**

Source: `RacingWheel.cs`, `Update` method, lines 841-856

Given: Torque buffer contains samples at 60Hz intervals

When: System needs torque value between samples

Then: System uses Hermite interpolation to calculate smooth 360Hz+ values

---

**B2.2: Extract 60Hz and 500Hz equivalent torque values**

Source: `RacingWheel.cs`, `Update` method, lines 855-856

Given: Interpolated 360Hz buffer

When: Processing algorithm

Then: System provides both 60Hz sample (index 6) and interpolated 500Hz value

### Behavior Group 3: Algorithm Processing

**B3.1: Apply native 60Hz algorithm**

Source: `RacingWheel.cs`, `ProcessAlgorithm` method, lines 152-157

Given: Input torque at 60Hz

When: Native60Hz algorithm is selected

Then: System normalizes torque by dividing by max force setting

---

**B3.2: Apply native 360Hz algorithm**

Source: `RacingWheel.cs`, `ProcessAlgorithm` method, lines 159-164

Given: Input torque at 500Hz (interpolated)

When: Native360Hz algorithm is selected

Then: System normalizes torque by dividing by max force setting

---

**B3.3: Apply detail booster algorithm**

Source: `RacingWheel.cs`, `ProcessAlgorithm` method, lines 166-176

Given: Input torque at 500Hz, detail boost setting, bias setting

When: DetailBooster algorithm is selected

Then: System amplifies torque changes by detail boost factor, applies bias toward smoothed value

---

**B3.4: Apply delta limiter algorithm**

Source: `RacingWheel.cs`, `ProcessAlgorithm` method, lines 178-190

Given: Input torque at 500Hz, delta limit setting, bias setting

When: DeltaLimiter algorithm is selected

Then: System limits rate of change of torque, applies bias toward smoothed value

---

**B3.5: Apply slew and total compression algorithm**

Source: `RacingWheel.cs`, `ProcessAlgorithm` method, lines 218-258

Given: Input torque at 500Hz, slew and compression settings

When: SlewAndTotalCompression algorithm is selected

Then: System applies slew rate limiting and total compression to torque signal

---

**B3.6: Apply multi-adjustment toolkit algorithm**

Source: `RacingWheel.cs`, `ProcessAlgorithm` method, lines 260-356

Given: Input torque at 60Hz and 500Hz, multiple adjustment settings

When: MultiAdjustmentToolkit algorithm is selected

Then: System applies compression, slew reduction, detail gain, and output smoothing

### Behavior Group 4: Output Adjustments

**B4.1: Apply output curve**

Source: `RacingWheel.cs`, `ProcessAlgorithm` method, lines 359-366

Given: Algorithm output torque, curve setting

When: Output curve is non-zero

Then: System applies power curve transformation to torque

---

**B4.2: Apply soft limiter**

Source: `RacingWheel.cs`, `ProcessAlgorithm` method, lines 368-373

Given: Algorithm output torque

When: Soft limiter is enabled

Then: System applies soft clipping to prevent hard saturation

---

**B4.3: Apply output maximum**

Source: `RacingWheel.cs`, `ProcessAlgorithm` method, lines 375-380

Given: Processed torque

When: Output maximum is less than 1.0

Then: System clamps torque to maximum value

---

**B4.4: Apply output minimum**

Source: `RacingWheel.cs`, `ProcessAlgorithm` method, lines 382-400

Given: Processed torque

When: Output minimum is greater than 0.0

Then: System ensures torque magnitude is at least minimum value

---

**B4.5: Apply crash protection**

Source: `RacingWheel.cs`, `Update` method, lines 876-900, 1006

Given: Processed torque, crash protection active

When: Crash protection timer is active

Then: System scales down torque by protection force reduction factor

---

**B4.6: Apply curb protection**

Source: `RacingWheel.cs`, `ProcessAlgorithm` method (curbProtectionLerpFactor parameter), `Update` method, lines 902-926

Given: Processed torque, curb protection active

When: Curb protection timer is active

Then: System reduces detail boost and delta limit effects

---

**B4.7: Apply parked strength reduction**

Source: `RacingWheel.cs`, `Update` method, lines 1008-1017

Given: Processed torque, vehicle speed

When: Speed is below 5 MPH and parked strength is less than 1.0

Then: System reduces torque proportionally to parked strength setting

---

**B4.8: Add LFE (Low Frequency Effects)**

Source: `RacingWheel.cs`, `Update` method, lines 928-930, 1019-1024

Given: Processed torque, LFE magnitude

When: LFE strength is greater than 0

Then: System adds LFE magnitude scaled by strength to torque

---

**B4.9: Apply soft lock**

Source: `RacingWheel.cs`, `Update` method, lines 1026-1043

Given: Processed torque, steering wheel angle, max angle

When: Soft lock strength is greater than 0 and angle exceeds max

Then: System adds restoring force proportional to angle beyond maximum

---

**B4.10: Apply friction**

Source: `RacingWheel.cs`, `Update` method, lines 1045-1050

Given: Processed torque, wheel velocity

When: Friction setting is greater than 0

Then: System adds friction force proportional to wheel velocity

---

**B4.11: Apply parked friction**

Source: `RacingWheel.cs`, `Update` method, lines 1052-1057

Given: Processed torque, wheel velocity, vehicle speed

When: Parked friction is greater than 0 and speed is low

Then: System adds additional friction force when parked

---

**B4.12: Apply wheel centering**

Source: `RacingWheel.cs`, `Update` method, lines 1059-1069

Given: Processed torque, wheel position, wheel velocity

When: Centering is enabled and vehicle is on track

Then: System adds centering force proportional to position and velocity

### Behavior Group 5: Vibration Effects

**B5.1: Generate understeer vibration**

Source: `RacingWheel.cs`, `Update` method, lines 440-502

Given: Understeer effect value, vibration pattern, frequency, strength

When: Understeer effect is greater than 0

Then: System generates vibration torque using selected pattern (sine, square, triangle, sawtooth)

---

**B5.2: Generate oversteer vibration**

Source: `RacingWheel.cs`, `Update` method, lines 504-566

Given: Oversteer effect value, vibration pattern, frequency, strength

When: Oversteer effect is greater than 0

Then: System generates vibration torque using selected pattern

---

**B5.3: Generate seat-of-pants vibration**

Source: `RacingWheel.cs`, `Update` method, lines 568-632

Given: Seat-of-pants effect value, vibration pattern, frequency, strength

When: Seat-of-pants effect is non-zero

Then: System generates vibration torque using selected pattern

---

**B5.4: Generate gear change vibration**

Source: `RacingWheel.cs`, `Update` method, lines 634-658

Given: Gear value, previous gear value

When: Gear changes and vibrate on gear change is enabled

Then: System generates brief square wave vibration at 40Hz

---

**B5.5: Generate ABS vibration**

Source: `RacingWheel.cs`, `Update` method, lines 660-681

Given: ABS active status

When: ABS is active and vibrate on ABS is enabled

Then: System generates triangle wave vibration at 50Hz

---

**B5.6: Generate test signal vibration**

Source: `RacingWheel.cs`, `Update` method, lines 422-438

Given: Test signal requested

When: Test signal button is pressed

Then: System generates 2-second test signal with combined cosine and sine waves

### Behavior Group 6: Constant Force Effects

**B6.1: Apply understeer constant force**

Source: `RacingWheel.cs`, `Update` method, lines 936-956

Given: Understeer effect value, constant force direction, strength

When: Understeer effect is greater than 0

Then: System applies constant force in specified direction (increase or decrease)

---

**B6.2: Apply oversteer constant force**

Source: `RacingWheel.cs`, `Update` method, lines 958-978

Given: Oversteer effect value, constant force direction, strength

When: Oversteer effect is greater than 0

Then: System applies constant force in specified direction

---

**B6.3: Apply seat-of-pants constant force**

Source: `RacingWheel.cs`, `Update` method, lines 980-1002

Given: Seat-of-pants effect value, constant force direction, strength

When: Seat-of-pants effect is non-zero

Then: System applies constant force in specified direction

### Behavior Group 7: State Management

**B7.1: Suspend FFB when simulator FFB is enabled**

Source: `RacingWheel.cs`, `Update` method, lines 685-701, 746

Given: Simulator FFB enabled status

When: Simulator FFB is enabled and "always enable" setting is off

Then: System suspends FFB output

---

**B7.2: Suspend FFB when not on track**

Source: `RacingWheel.cs`, `Update` method, lines 705-726, 867

Given: On-track status

When: Vehicle is not on track

Then: System does not use steering wheel torque data

---

**B7.3: Suspend FFB during replay**

Source: `RacingWheel.cs`, `Update` method, line 746

Given: Replay playing status

When: Replay is playing

Then: System suspends FFB output

---

**B7.4: Fade in FFB when starting**

Source: `RacingWheel.cs`, `Update` method, lines 711-718, 1073-1080

Given: FFB is being enabled, fade is enabled

When: FFB transitions from suspended to active

Then: System fades in torque over 2 seconds

---

**B7.5: Fade out FFB when stopping**

Source: `RacingWheel.cs`, `Update` method, lines 719-724, 1081-1084

Given: FFB is being disabled, fade is enabled

When: FFB transitions from active to suspended

Then: System fades out torque over 0.5 seconds

---

**B7.6: Reset FFB device**

Source: `RacingWheel.cs`, `Update` method, lines 728-742; `DirectInput.cs`, `InitializeForceFeedback` method, lines 100-183

Given: Reset requested

When: Reset button is pressed

Then: System reinitializes force feedback device

---

**B7.7: Auto-set max force**

Source: `RacingWheel.cs`, `Update` method, lines 791-801, 872-874

Given: Peak torque tracking

When: Auto-set max force is requested

Then: System sets max force to peak torque plus margin

### Behavior Group 8: Protection Systems

**B8.1: Activate crash protection on high G-force**

Source: `RacingWheel.cs`, `Update` method, lines 878-891 (triggered externally via `ActivateCrashProtection` flag)

Given: Longitudinal and lateral G-forces, protection thresholds

When: G-force exceeds threshold

Then: System activates crash protection timer

---

**B8.2: Activate curb protection on curb contact**

Source: `RacingWheel.cs`, `Update` method, lines 902-917 (triggered externally via `ActivateCurbProtection` flag)

Given: Track surface detection

When: Curb contact is detected

Then: System activates curb protection timer

---

**B8.3: Track peak torque**

Source: `RacingWheel.cs`, `Update` method, lines 858-870

Given: Processed torque, on-track status

When: Vehicle is on track

Then: System tracks maximum absolute torque value

## Notes

- The code processes FFB at 360Hz+ even though telemetry arrives at 60Hz
- Multiple algorithms are available for different driving preferences
- Protection systems (crash, curb) prevent excessive forces
- Vibration effects provide additional tactile feedback beyond raw torque
- State management ensures FFB only runs when appropriate (on track, not in replay, etc.)
- All effects are additive and configurable through settings