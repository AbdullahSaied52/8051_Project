
# 🛠️ 8051 Motor Control System (Assembly)

This project implements a basic motor control system using the **8051 microcontroller** and **Assembly language**. It monitors three input switches to control the direction of a DC motor (Clockwise, Counter-Clockwise, or Stop) via two output pins, which are typically connected to an H-Bridge driver or status LEDs.

---

## 📌 Features

* **Directional Control:** Full support for Clockwise (CW), Counter-Clockwise (CCW), and hard Stop operations.
* **Input Initialization:** Properly configures input pins with internal pull-ups to handle Active-Low button presses.
* **Embedded Delay Function:** Includes a nested-loop delay routine (DELAY_ON) structured for future time-based control or speed modulation.

---

## 🔌 Pin Mapping

### 📥 Input Pins (Switches)

| Code Alias | Microcontroller Pin | Function |
| --- | --- | --- |
| CW | P1.0 | Rotates the motor Clockwise (Right) |
| STOP | P1.1 | Stops the motor immediately |
| CCW | P1.2 | Rotates the motor Counter-Clockwise (Left) |
| INCR | P1.3 | Reserved for incrementing delay/speed (Not used in current logic) |
| DECR | P1.4 | Reserved for decrementing delay/speed (Not used in current logic) |

### 📤 Output Pins (Motor Control / LEDs)

| Microcontroller Pin | Clockwise State | Counter-Clockwise State | Stopped State |
| --- | --- | --- | --- |
| P2.0 | **HIGH (1)** | **LOW (0)** | **LOW (0)** |
| P2.1 | **LOW (0)** | **HIGH (1)** | **LOW (0)** |

---

## ⚙️ Program Workflow

1. **Initialization:** The program sets P1.0, P1.1, and P1.2 to 1 to configure them as inputs. It also pre-loads registers R1 and R2 with values 10 and 90 respectively for the delay loop.
2. **Polling Loop (CHECK):** The microcontroller continuously polls the input pins using JNB (Jump if Not Bit) instructions:
* If CW drops to 0 ➡️ Jumps to TURN_RIGHT.
* If CCW drops to 0 ➡️ Jumps to TURN_LEFT.
* If STOP drops to 0 ➡️ Jumps to OFF.


3. **Execution Blocks:**
* **TURN_RIGHT / TURN_LEFT:** Directly toggles the state of P2.0 and P2.1 to change the H-bridge polarity.
* **OFF:** Clears both output pins to cut off power to the motor.



---

## 🛠️ Tools Required

* **IDE / Compiler:** [Keil uVision] to compile the Assembly code and generate the .HEX file.
* **Simulation:** [Proteus Design Suite] to visually test the circuit using an 8051 IC, push buttons, and a DC motor or LEDs.

---

💡 **Developer Note:** The DELAY_ON sub-routine at the bottom uses nested loops with registers R3, R4, and R6 to create a timed delay based on the values of R1 and R2. However, this routine is currently unlinked (not called via ACALL or LCALL) in the main polling routine. If you want to implement timed cycles, you can call this routine directly right after the direction changes.
