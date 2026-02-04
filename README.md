# PIC16F84 Keypad Security System

A lightweight, assembly-based security controller for the **Microchip PIC16F84** microcontroller. This project interfaces with a 3x4 matrix keypad to trigger a 1-second pulse on an output pin when a specific four-digit passcode is entered.

## Features
* **Low Overhead:** Written entirely in MPASM (Assembly) for maximum efficiency.
* **Fixed Code Security:** Hard-coded 4-digit sequence (Default: 1-2-3-4).
* **Timed Output:** Precise 1-second logic high pulse on `RA0` upon successful entry.
* **Auto-Reset:** Incorrect entries immediately reset the sequence progress.

## Hardware Requirements
1.  **Microcontroller:** PIC16F84 or PIC16F84A.
2.  **Clock:** 4MHz Crystal Oscillator.
3.  **Input:** 3x4 Matrix Keypad.
4.  **Output:** LED, Relay, or Solenoid (connected to `RA0` via a transistor).
5.  **Resistors:** 10k Ohm for MCLR reset pin; internal pull-ups enabled on PORTB.

## Pin Configuration

| Component       | PIC16F84 Pin | Function                   |
| :-------------- | :----------- | :------------------------- |
| **Keypad Rows** | RB0 - RB3    | Inputs (Internal Pull-ups) |
| **Keypad Cols** | RB4 - RB6    | Scanning Outputs           |
| **Action Out** | RA0          | 1-Second Logic High Pulse  |
| **MCLR** | Pin 4        | Tied to VDD (10k Resistor) |



## Technical Logic

The system operates as a simple state machine. Each key press is captured via a scanning routine that pulls columns low and reads the row state.

### Passcode Verification
Each correct key press advances the logic to the next "check" state. If a key is pressed that does not match the expected digit in the sequence, the logic jumps to a `RESET_CODE` label, requiring the user to start the sequence from the first digit again.

### Timing Calculation
The 1-second delay is calculated based on the instruction cycle of a 4MHz clock. Since each instruction cycle takes $4 / f_{osc}$, or $1\mu s$, the nested loops in the code are calibrated to consume approximately $1,000,000$ cycles.

$$T_{delay} \approx (COUNT1 \times COUNT2 \times COUNT3) \times 1\mu s$$



## Installation & Compilation
1.  **Clone the repository:**
    ```bash
    git clone [https://github.com/nepryoon/PIC16F84-Keypad-Security-System.git](https://github.com/nepryoon/PIC16F84-Keypad-Security-System.git)
    ```
2.  **Open the Source:** Load the `.asm` file in **MPLAB X IDE**.
3.  **Build:** Ensure the toolchain is set to MPASM and the device is PIC16F84. Build to generate the `.hex` file.
4.  **Flash:** Use a programmer (e.g., PICkit 3) to load the hex file onto the microcontroller.

## License
This project is open-source. Feel free to modify the passcode and timing for your own security applications.
