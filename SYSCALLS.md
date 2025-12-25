# MIPS vs Linux Syscall Reference

This document outlines the differences between MARS/SPIM system calls (Academic) and standard Linux MIPS system calls (Real).

## Overview

- **MARS/SPIM**: Uses a simplified set of syscalls for educational purposes. These are handled by the simulator.
- **Linux MIPS (O32 ABI)**: Uses standard kernel system calls. These are handled by the Linux kernel.

## Syscall Numbers

| Operation | MARS ($v0) | Linux ($v0) | Arguments (MARS) | Arguments (Linux) |
|-----------|------------|-------------|------------------|-------------------|
| Print Int | 1 | - | $a0 = int | Use `write` (4004) |
| Print String | 4 | - | $a0 = address | Use `write` (4004) |
| Read Int | 5 | - | - | Use `read` (4003) |
| Exit | 10 | 4001 | - | $a0 = status |
| Print Char | 11 | - | $a0 = char | Use `write` (4004) |
| Read Char | 12 | - | - | Use `read` (4003) |
| Open File | 13 | 4005 | $a0=name, $a1=flags | $a0=name, $a1=flags, $a2=mode |
| Read File | 14 | 4003 | $a0=fd, $a1=buf, $a2=len | $a0=fd, $a1=buf, $a2=len |
| Write File | 15 | 4004 | $a0=fd, $a1=buf, $a2=len | $a0=fd, $a1=buf, $a2=len |
| Close File | 16 | 4006 | $a0=fd | $a0=fd |

## Writing Portable Code

To write code that runs on both MARS and Linux, you can use conditional assembly or macros (if supported), or stick to the common subset (which is very small).

For **Real Mode** development, it is recommended to use the Linux syscall numbers.

### Example: Hello World (Linux MIPS)

```asm
.data
    msg: .asciiz "Hello, Linux!\n"

.text
.globl main
main:
    # write(1, msg, 14)
    li $v0, 4004    # Syscall: write
    li $a0, 1       # File descriptor: 1 (stdout)
    la $a1, msg     # Buffer
    li $a2, 14      # Length
    syscall

    # exit(0)
    li $v0, 4001    # Syscall: exit
    li $a0, 0       # Status
    syscall
```

### Example: Hello World (MARS)

```asm
.data
    msg: .asciiz "Hello, MARS!\n"

.text
main:
    # print_string(msg)
    li $v0, 4
    la $a0, msg
    syscall

    # exit
    li $v0, 10
    syscall
```
