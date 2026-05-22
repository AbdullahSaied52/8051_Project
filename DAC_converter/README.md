# 8051 Sine Wave Generation Project 🌊

This project demonstrates how to generate a smooth **Sine Wave** using an **8051 Microcontroller (AT89C51/52)**. It utilizes **Timer 0 in Mode 2 (8-bit Auto-Reload)** for precise timing control and outputs digital values to a Look-up Table (LUT) mapped to **Port 2**, which can be connected to a DAC (Digital-to-Analog Converter) or an R-2R resistor ladder network.

---

## 📌 Project Overview

The microcontroller continuously loops through a predefined array of **32 digital values** that mathematically represent a full cycle ($360^\circ$) of a Sine Wave. By sending these values sequentially to `P2` with a fixed time delay controlled by hardware timers, a periodic analog-like wave is synthesized.

### Look-up Table (LUT) Mechanics
The array `x[]` contains values scaled between `0` (minimum voltage) and `255` (maximum voltage, 5V), with `128` acting as the reference center line (0V AC component / 2.5V DC offset).

---

## 🛠️ Hardware & Architecture Configuration

### 1. Peripherals Configured
* **Port 2 (`P2`)**: Configured as the digital output bus connected to the DAC.
* **Timer 0**: Used in **Mode 2 (8-bit Auto-reload)**.
  * `TMOD = 0x02;` sets Timer 0 to 8-bit Auto-reload mode.
  * `TH0 = -5;` (or `0xFB`) defines the timer overflow rate, directly controlling the frequency of the generated sine wave.

### 2. Included Libraries
* `<reg51.h>`: Standard 8051 register definitions.
* `"lcd_kp.h"`: Kept for potential LCD and Keypad expansion (currently unused in the core wave generation loop).

---

## 💻 Code Structure & Functions

### `void main(void)`
* Initializes Port 2 values to `0`.
* Sets up Timer 0 configuration (`TMOD`).
* Loads the reload register `TH0` with `-5`.
* Enters an infinite `while(1)` loop that steps through the `sizeof(x)` array elements, pushes the byte to `P2`, and calls the hardware delay.

### `void Delay(void)`
* Uses hardware polling instead of software loops for precise, reliable timing.
* Starts Timer 0 (`TR0 = 1`).
* Waits for the Timer 0 Overflow Flag (`TF0 == 1`).
* Stops the timer and clears the flag to prepare for the next step.

---

## 🚀 How to Run & Simulate

### 1. Prerequisites
* **IDE**: Keil uVision (C51 Compiler).
* **Simulation**: Proteus Design Suite.

### 2. Circuit Setup in Proteus
1. Place an **AT89C51** microcontroller.
2. Connect **Port 2 (P2.0 - P2.7)** to an **DAC0808** IC or build an **R-2R Resistor Ladder** network.
3. Connect the analog output of the DAC/R-2R network to channel A of an **Oscilloscope**.
4. Load the compiled `.hex` file into the 8051 microcontroller.
5. Run the simulation to monitor the synthesized Sine Wave.

---

## 📊 Tuning the Frequency
You can change the output frequency of the sine wave by modifying the value of `TH0` in the `main` function:
* `TH0 = -5;` (Current - Fastest frequency)
* `TH0 = -29;` (Slower frequency)
* `TH0 = -71;` (Lowest frequency)