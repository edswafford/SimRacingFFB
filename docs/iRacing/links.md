https://github.com/NickThissen/iRacingSdkWrapper/blob/master/iRacingSdkWrapperTutorial.pdf

– – – SteeringWheelTorque_ST = Steering Wheel Torque

– – This data is output as a 6 element array, with a new flag variable header indicating that ist should be treated as a subdivision of time. You will need a new version of the ATLAS plugin to properly view the data, however the old plugin, and any older telemetry tool, should continue to read the data just fine.

– A new McLaren ATLAS importer version 1.05 is available for download from the member site. This adds some new unit conversion parameters and adds support for properly viewing 360 Hz telemetry.
In iRacing,
SteeringWheelTorque_ST refers to the instantaneous, high-frequency output torque on the steering shaft (in Nm), sampled at 360 Hz, providing incredibly detailed force feedback data for advanced wheelbases and telemetry, allowing users to feel tiny bumps and road details that the standard SteeringWheelTorque (sampled slower) misses. It's part of iRacing's SDK (Software Development Kit) for deep telemetry analysis, helping users and software fine-tune force feedback effects beyond basic settings. 
Key Aspects:

    ST (Sampled at 360 Hz): The "_ST" suffix denotes "Sampled at 360 Hz," a very high update rate for force feedback data, far faster than typical updates.
    High-Detail Feedback: This high frequency captures subtle vibrations and forces that might be lost in lower-frequency data, crucial for realistic feel.
    Use Case: Primarily for advanced users, telemetry software (like SimHub), and custom setups that need very precise data to create nuanced effects.
    Distinction from SteeringWheelTorque: SteeringWheelTorque gives the standard torque (Nm) at the normal update rate, while SteeringWheelTorque_ST provides the same data but sampled much faster for enhanced detail. 

In simpler terms: If SteeringWheelTorque is like a standard video, SteeringWheelTorque_ST is a slow-motion, high-definition video of the forces on your wheel, giving you the ultimate detail for tuning

--
Another link
https://github.com/LeoAdamek/iracing.rs/blob/master/sample.yaml