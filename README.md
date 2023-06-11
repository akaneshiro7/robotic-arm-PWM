# Robotic Arm Control Using Pulse Width Modulation on DE1SoC FPGA

## Description
This project demonstrates how to use Pulse Width Modulation (PWM) to control a robotic arm using the DE1SoC and FPGA utilizing Intel Quartus Prime. The robotic arm simulates an assembly line by moving bottles from position 1 to position 2. The user can use push buttons and switches on the DE1SoC board to manually control individual motors on the robotic arm, save positions, and initiate automated movement.
An IEEE Paper for this project can be found [here](Robotic-Arm-PWM.pdf)
### Schematic of Completed Circuit
![](https://github.com/akaneshiro7/robotic-arm-PWM/blob/master/final.png)

## Features
- Control of the robotic arm's base, bicep, elbow, wrist, and grip through the switches and push buttons on the DE1SoC board.
- Utilization of PWM for precise control of the servo motors.
- Ability to save two positions (Position 1 and Position 2) using the switches SW7 and SW8.
- Automated movement of bottles from Position 1 to Position 2 initiated by SW9.
- Implementation of a 4-second delay after releasing the bottle at position 2 before returning to position 1.
- The robotic arm initializes in an upwards position.

## How it Works
1. **Pulse Width Modulation (PWM):** This is a technique that involves generating pulses of varying widths to transfer an analog signal. In this project, PWM is generated using a counter and comparator block in Quartus. A duty cycle, which is the ratio of time a circuit is ON compared to the time it is OFF, is utilized to control the PWM.
### The duty cycle and corresponding comparator values can be calculated using the equation below.
![](https://github.com/akaneshiro7/robotic-arm-PWM/blob/master/duty_cycle.png)

### Quartus Schematic for PWM Generation
![](https://github.com/akaneshiro7/robotic-arm-PWM/blob/master/pwm.png)

2. **Servo Motor Control:** The servo motors operate within a duty cycle range of 3% to 12%, corresponding to a movement range of 0 to 180 degrees. This translates to a duty cycle of 0.6ms to 2.4ms, considering the board's clock frequency of 50 MHz.
### Quartus Schematic for Motor Control
![](https://github.com/akaneshiro7/robotic-arm-PWM/blob/master/motor_control.png)

3. **Motor Positions:** These are controlled by changing the comparator value. An updown counter controlled by a switch and a pushbutton initializes with a value of 9, corresponding to the upwards position. A multiplier block then multiplies the counter value by 5,000 and a subtractor subtracts the value from 120,000 to allow for rotation between 0 and 180 degrees.

4. **Position Saving:** A counter with synchronous load input is used to save the comparator values for Position 1 and Position 2 using switches SW7 and SW8. These values are then passed through a multiplexer (mux) controlled by a timing circuit.
### Quartus Schematic Saving Positions
![](https://github.com/akaneshiro7/robotic-arm-PWM/blob/master/saving_positions.png)

5. **Timing of the Circuit:** Moving from Position 1 to Position 2 is capped at 10 seconds. After releasing the bottle at Position 2, the arm waits for 4 seconds before moving back to Position 1.
### Timing Diagram
![](https://github.com/akaneshiro7/robotic-arm-PWM/blob/master/timing.png)
### Quartus Implementation of Timing Diagram
![](https://github.com/akaneshiro7/robotic-arm-PWM/blob/master/timing_implementation.png)

## Final Circuit Controls
- SW0, KEY0: Control base of the robotic arm.
- SW1, KEY1: Control bicep.
- SW2, KEY2: Control elbow.
- SW3, KEY3: Control wrist.
- SW4: Control grip.
- SW5: Sets arm to stand straight up.
- SW7, SW8: Save Position 1 and Position 2.
- SW9: Begins automated motion.

## Usage
To operate the robotic arm:
1. Manually control the base, bicep, elbow, wrist, and grip of the robotic arm using the switches and push buttons (SW0-4 and KEY0-4).
2. Save Position 1 and Position 2 using switches SW7 and SW8.
3. Start the automated motion by toggling SW9.
Remember that the arm should not knock over the bottle at any point in the cycle. It will wait for 4 seconds after releasing the bottle at Position 2 before returning to Position 1.

## Implementation
The implementation of this project is divided into three parts:
1. **Pulse Width Modulation (PWM) Generation:** PWM signals are generated using a counter and comparator in Quartus Prime. If the counter value is less than the compared value, output is 1; otherwise, it's 0. For a 90-degree movement, the duty cycle is 1.5ms and the comparator value is set to 75,000.
2. **Saving Positions:** A counter with a synchronous load input is used to save the comparator values for Position 1 and Position 2 using switches SW7 and SW8. The saved values are passed through a mux controlled by a timing circuit.
3. **Controlling Motor Positions:** The motor positions are controlled by changing the comparator value using Counter, Subtractor, and Multiplicator blocks. The Updown counter's input is controlled by a switch and the count enable is controlled by a pushbutton. The counter is initialized with a value of 9 to assume the upwards position.
The project is assembled in Intel Quartus Prime. 

## Circuit Details
The circuit includes the following key components:
- **Switches (SW0-SW9):** Used for controlling different aspects of the robotic arm's movement and grip, and initiating automated motion.
- **Keys (KEY0-KEY3):** Used in conjunction with the switches for controlling the robotic arm.
- **Counter:** Used for PWM generation and for saving positions.
- **Subtractor:** Used for controlling motor positions.
- **Multiplicator:** Also used for controlling motor positions.
- **Multiplexers (Mux):** Used for selecting between saved positions and for controlling timing.
- **Comparator:** Used in PWM generation.

## Specific Project Limitations
1. The robotic arm must not knock over the bottle at any point in the cycle.
2. The movement from Position 1 to Position 2 must not take more than 10 seconds.
3. The arm waits for exactly 4 seconds after releasing the bottle at Position 2 before returning to Position 1.

## Future Work
Future modifications could include a feedback system to dynamically adjust the motor positions and the implementation of more complex movements or tasks. 

## Conclusion
This project provides a foundation for understanding how to implement Pulse Width Modulation on an FPGA to control a robotic arm. The use of PWM allows for precise control of the servo motors, enabling the robotic arm to perform tasks such as moving an object from one position to another. 

## Support
For more information or assistance, please reach out to the project support team.
