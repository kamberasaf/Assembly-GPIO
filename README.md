# Assembly-GPIO

This repository contains my implementation of GPIO, Keypad, and LCD control using MSP430 assembly language for Lab 3 of the "Introduction to Computers" course.

## Repository Contents

### Code Files
- [Main Program (main_lab3.s43)](./code/main_lab3.s43): Contains task implementations for LED patterns using GPIO.
- [Routines (routines_lab3.s43)](./code/routines_lab3.s43): Provides routines for GPIO, Keypad, and LCD operations.

### Documentation
- [Pre-lab Assignment](./docs/lab3_pre_assignment.pdf): Initial preparation for GPIO, Keypad, and LCD control.
- [Preparation Report](./docs/lab3_preparation_report.pdf): Detailed documentation of lab preparation.
- [Final Lab Report](./docs/lab3_final_report.pdf): Complete analysis and results of the implementation.

## Features Implemented

### 1. GPIO Control
The code demonstrates various methods of controlling the MSP430's General Purpose Input/Output (GPIO) ports:
- Direct port manipulation
- LED pattern generation
- Input detection through switches
- Timed output sequences

### 2. Task Implementations

The program includes four tasks that demonstrate different LED patterns:

**Task 1: Increment Pattern**
```assembly
task1         CLR    R14               ; Clear counter
next_num      MOV.B  R14,&P2OUT        ; Output value to LEDs
              CMP.B  #0x01,&P1IN       ; Check if switch 1 is pressed
              JNE    mainloop          ; If not, return to main loop
              Delay  #58250,#6         ; 1 second delay
              INC.B  R14               ; Increment counter
              JMP    next_num          ; Continue pattern
```
*This task displays an incrementing binary pattern on the LEDs when switch 1 is pressed.*

**Task 2: Decrement Pattern**
```assembly
task2         CLR    R14               ; Clear register
              MOV.B  0xff,R14          ; Set all bits to 1
prev_num      MOV.B  R14,&P2OUT        ; Output value to LEDs
              CMP.B  #0x02,&P1IN       ; Check if switch 2 is pressed
              JNE    mainloop          ; If not, return to main loop
              Delay  #58250,#6         ; 1 second delay
              DEC    R14               ; Decrement counter
              JMP    prev_num          ; Continue pattern
```
*This task displays a decrementing binary pattern on the LEDs when switch 2 is pressed.*

**Task 3: Alternating ID Digits**
```assembly
task3         MOV     #id1,R14         ; Point to first ID
              MOV     #id2,R12         ; Point to second ID
              MOV.B   #8,R13           ; Set counter to ID length
```
*This task alternately displays digits from two ID numbers when switch 3 is pressed.*

**Task 4: Mirrored Display**
```assembly
task4         MOV     #id1_id2,R10     ; End pointer
              MOV     #id1_id2,R11     ; Start pointer
              MOV.B   #7, R7           ; Set iteration counter
```
*This task displays digits from both ends of the combined ID array when switch 4 is pressed.*

### 3. Delay Macro

The code uses a macro for generating precise delays:
```assembly
Delay         MACRO  time,mul          ; time*mul = presumed clock cycles
              LOCAL  L1,L2             ; Local labels for the macro
              MOV.W  mul,R4            ; Outer loop counter
L1            MOV.W  time,R5           ; Inner loop counter
L2            DEC.W  R5                ; Decrement inner counter
              JNZ    L2                ; Loop if not zero
              DEC.W  R4                ; Decrement outer counter
              JNZ    L1                ; Loop if not zero
              ENDM
```
*This macro creates a configurable delay by executing nested loops.*

## Hardware Requirements

- MSP430G2553 microcontroller
- Development board with LEDs connected to P2 port
- Switches connected to P1 port
- Optional: LCD display (for extended functionality)
- Optional: Keypad (for extended functionality)

## Usage

1. Connect the MSP430 development board to your computer
2. Build and flash the code using IAR Embedded Workbench or Code Composer Studio
3. Use the switches to activate different LED patterns:
   - Switch 1 (P1.0): Incrementing pattern (Task 1)
   - Switch 2 (P1.1): Decrementing pattern (Task 2)
   - Switch 3 (P1.2): Alternating ID digits (Task 3)
   - Switch 4 (P1.3): Mirrored ID display (Task 4)

## Author

Created by Asaf Kamber and Aviv Primor

## Course Information

- Course: Introduction to Computers
- Lab: GPIO, Keypad and LCD (Lab 3)
