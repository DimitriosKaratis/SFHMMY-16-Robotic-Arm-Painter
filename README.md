# Robotic Arm Painter using Arduino Uno 🤖🎨

A 2-DOF robotic arm project developed for ΣΦΗΜΜΥ 16 workshop that combines robotics and art by creating an autonomous painting robot. This project demonstrates fundamental robotics concepts including inverse kinematics, servo control, and serial communication using Arduino.

<img src="https://i.imgur.com/7CiEBBA.jpg" alt="Robotic Arm Setup" width="600">

## Overview

This project was created for the **"Autonomous Robotic Arm Motion: From Theory to Practice"** workshop, presented by the Icarus Project subteam of RAS (Robotics and Automation Society). The goal was to design and implement a robot painter that can receive coordinate data and autonomously draw continuous lines.

## Features

- **2-DOF Robotic Arm Control**: Controls two servo motors to achieve precise positioning
- **Inverse Kinematics**: Calculates joint angles from desired end-effector positions
- **Serial Communication**: Receives drawing coordinates from a Python GUI application
- **Smooth Line Drawing**: Interpolates between waypoints for continuous motion
- **Real-time Control**: Responds to coordinate data in real-time

## Hardware Requirements

- Arduino UNO
- 2x Servo Motors
- Robotic arm mechanical structure
- Power supply for servos
- USB cable for Arduino communication

## Software Components

### Arduino Code (`robotic_arm.ino`)
The main Arduino sketch that handles:
- Servo motor control (pins 9 and 10)
- Inverse kinematics calculations
- Serial communication protocol
- Line drawing interpolation

### Python GUI Application
- `painter_app.py`: Source code for the GUI interface

The GUI application allows users to:
- Draw paths by clicking points
- Send coordinate data to the Arduino via serial
- Control the robotic arm's drawing sequence

## Mathematical Foundation

### Inverse Kinematics
The project implements a 2-DOF inverse kinematics solution for a planar robotic arm:

```
Given: Target position (px, py)
Link lengths: l1 = 10, l2 = 9.5

cos(q2) = (px² + py² - l1² - l2²) / (2*l1*l2)
sin(q2) = √(1 - cos²(q2))
q2 = atan2(sin(q2), cos(q2))

d1 = l1 + l2*cos(q2)
d2 = l2*sin(q2)
a = atan2(d2, d1)
q1 = atan2(py, px) - a
```

### Line Interpolation
Smooth line drawing is achieved through linear interpolation between waypoints:
```
For parameter s ∈ [0,1]:
px = x_start + s * (x_end - x_start)
py = y_start + s * (y_end - y_start)
```

## Code Structure

### Main Functions

- `Inverse_Kin(float px, float py)`: Calculates and applies inverse kinematics
- `line(float x_start, float y_start, float x_end, float y_end, float u)`: Draws smooth lines between points
- Serial communication protocol for receiving coordinate arrays

### Communication Protocol

The Arduino expects coordinate data in the format:
```
x1,y1\n
x2,y2\n
...
stop\n
```

Each coordinate pair is acknowledged with "ACK" before the drawing sequence begins.

## Project Evolution

The repository contains multiple solution iterations:

1. **Initial Template** (`robotic_arm.ino`): Basic structure with empty function implementations
2. **Solution 1**: Implemented inverse kinematics
3. **Solution 2**: Added line drawing functionality
4. **Solution 3**: Complete implementation with full drawing capabilities

## Getting Started

1. **Hardware Setup**:
   - Connect servo motors to Arduino pins 9 and 10
   - Assemble the robotic arm mechanism
   - Ensure proper power supply for servos

2. **Software Setup**:
   - Upload `solution3.ino` to Arduino UNO
   - Run `python painter_app.py`
   - Select correct serial port in the GUI

3. **Operation**:
   - Draw desired path in the GUI by clicking points
   - Send coordinates to Arduino
   - Watch the robotic arm execute the drawing

## Technical Specifications

- **Workspace**: Determined by link lengths (l1=10cm, l2=9.5cm)
- **Control Frequency**: ~10Hz update rate
- **Communication**: 9600 baud serial
- **Positioning Accuracy**: Limited by servo resolution (±1°)
