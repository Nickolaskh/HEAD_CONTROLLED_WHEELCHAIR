# Head-Controlled Smart Wheelchair with Vision-Based Safety System

## Project Overview

This project presents a smart electric wheelchair controlled via head movements using an IMU sensor, with an integrated vision-based obstacle detection system for safety.

All system processing — including motion interpretation, PID motor control, and camera-based obstacle detection — was executed on a Raspberry Pi, as required by project constraints. Although the system is hardware-heavy in nature, the Raspberry Pi handled both high-level logic and low-level motor control.

The platform was fully built and tested using dual hoverboard motors with closed-loop speed regulation.

---

## System Architecture

### Main Components

- IMU sensor (head orientation tracking)
- Raspberry Pi (Python-based processing and control)
- 2 × Hoverboard BLDC motors
- 2 × Motor drivers (with integrated Hall sensors)
- CSI camera module
- Custom PID motor control implementation (no external libraries)

### High-Level Control Flow

1. IMU measures head tilt angles.
2. Raspberry Pi converts orientation into motion commands.
3. Differential drive logic generates left/right wheel references.
4. Independent PID controllers regulate each motor.
5. Camera continuously processes frames for obstacle detection.
6. If obstacle detected → safety override disables motor motion.

---

## Head Motion Control Strategy

Head orientation (pitch and roll) was mapped to motion commands using threshold-based logic:

- Forward tilt → Forward motion  
- Backward tilt → Reverse motion  
- Left tilt → Turn left  
- Right tilt → Turn right  

Small-angle thresholds were implemented to prevent unintended motion due to minor head movements or sensor noise.

Basic signal filtering was applied to IMU readings to improve stability and reduce jitter in control commands.

---

## Motor Control Implementation

Each hoverboard wheel was controlled independently using a custom PID controller written entirely in Python.

### Closed-Loop Speed Control

- Feedback source: Hall sensors integrated within motor drivers
- Controlled variable: Wheel angular velocity
- Control input: Motor driver voltage command
- Controller type: PID (implemented manually)

The PID controller was coded from first principles without relying on external control libraries.

A filtering stage was incorporated within the control loop to reduce noise sensitivity, particularly in the derivative component.

### Key Features

- Independent left/right wheel control (differential drive)
- Real-time speed regulation
- Manual PID gain tuning
- Stable operation under varying rider load
- Software-level safety shutdown logic

---

## Vision-Based Safety System

Obstacle detection was implemented using:

- Raspberry Pi Camera Module (CSI interface)
- Python with OpenCV

### Image Processing Pipeline

1. Frame acquisition
2. Grayscale conversion
3. Gaussian filtering
4. Edge detection
5. Contour extraction
6. Area-based thresholding

If a detected object exceeded a predefined area within a forward safety region, a stop command was issued to the motor control logic.

This created a real-time collision prevention layer independent of user input.

---

## Practical Engineering Challenges

### Sensor Noise
IMU data required filtering to prevent jitter-induced motion commands.

### Wheel Synchronization
Independent PID loops required careful tuning to maintain straight-line motion without drift.

### Load Variability
Motor dynamics varied depending on rider weight and surface friction.

### Processing Constraints
All control and vision tasks were executed on a Raspberry Pi due to project requirements, requiring efficient implementation to maintain responsiveness.

### Model-Free Tuning
PID gains were tuned empirically on hardware to account for non-ideal effects not captured analytically.

---

## Results

- Stable and responsive head-controlled navigation
- Smooth closed-loop speed regulation
- Reliable obstacle detection and safety override
- Fully functional hardware prototype validated experimentally

---

## Media

- Build process documentation
- Hardware testing demonstrations
- Final system demo
- Project presentation post

---

## Future Improvements

- Dedicated microcontroller for real-time motor control
- Advanced sensor fusion for improved head tracking
- Adaptive PID tuning
- Lightweight deep-learning-based obstacle detection
- Semi-autonomous navigation mode
