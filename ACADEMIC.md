# Academic Resources for MIPSduino

MIPSduino is designed to be the perfect tool for **Computer Architecture** and **Assembly Language** courses.

## Why use MIPSduino in your course?

1.  **Modern Tooling**: Students prefer VS Code over outdated Java GUIs.
2.  **Cross-Platform**: Works identically on Windows, macOS, and Linux.
3.  **Hardware Connection**: Allows students to see their code run on real hardware (Arduino/FPGA), bridging the gap between theory and practice.
4.  **Automated Grading**: The CLI allows you to write scripts to automatically grade student submissions.

## Sample Assignments

### Lab 1: Introduction to MIPS
**Objective**: Familiarize with registers and basic arithmetic.
**Task**: Write a program to calculate the Fibonacci sequence.
**Tools**: VS Code + MIPSduino Extension.

### Lab 2: System Calls
**Objective**: Understand I/O operations.
**Task**: Create a simple calculator that takes user input.
**Key Concept**: Difference between MARS syscalls and Linux syscalls (using Dual Mode).

### Lab 3: Memory Operations
**Objective**: Learn load/store instructions.
**Task**: Implement a bubble sort algorithm on an array in memory.
**Visualization**: Use `MIPSduino memory` command to inspect the sorted array.

### Lab 4: Hardware Interfacing
**Objective**: Run code on a microcontroller.
**Task**: Blink an LED using MIPS assembly logic exported to Arduino.
**Tools**: MIPSduino Build (`-f arduino`).

## Automated Grading Script Example

You can use a simple Python script to grade submissions:

```python
import subprocess

def grade_submission(file_path):
    # Run the student's code
    result = subprocess.run(
        ["MIPSduino", "run", file_path], 
        capture_output=True, text=True
    )
    
    expected_output = "Fibonacci: 0, 1, 1, 2, 3, 5, 8, 13"
    
    if expected_output in result.stdout:
        return 100
    else:
        return 0
```
