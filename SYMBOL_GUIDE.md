# Viewing Symbol Labels in MIPS Assembly

## Quick Reference

### Method 1: Using the Symbol Viewer Script (Recommended)
```bash
# Basic usage - shows all symbols organized by segment
./show_symbols.sh task1_corrected.asm

# With MARS verification - also checks if code assembles
./show_symbols.sh task1_corrected.asm --mars
```

### Method 2: Using MARS GUI
```bash
# Open your file in MARS
java -jar Mars.jar task1_corrected.asm

# Then in the GUI:
# 1. Go to: Tools → Symbol Table
# 2. This shows all labels with their memory addresses
```

### Method 3: Simple grep (Quick & Dirty)
```bash
# Show all labels
grep "^[[:space:]]*[a-zA-Z_][a-zA-Z0-9_]*:" task1_corrected.asm

# Show only label names
grep -o "^[[:space:]]*[a-zA-Z_][a-zA-Z0-9_]*:" task1_corrected.asm | sed 's/://g'
```

---

## Example Output

### Your `task1_corrected.asm` Symbol Table:

```
==========================================
SYMBOL TABLE: task1_corrected.asm
==========================================

DATA SEGMENT SYMBOLS:
----------------------------------------
  arr1            │ DATA         │ .word 5, 20
  arr2            │ DATA         │ .half 7, -2, 8, -6
  arr3            │ DATA         │ .space 100
  str1            │ DATA         │ .asciiz "This is a message"
  str2            │ DATA         │ .asciiz "Another important string"

TEXT SEGMENT SYMBOLS:
----------------------------------------
  main            │ TEXT         │ Code label

GLOBAL SYMBOLS:
----------------------------------------
  main (globally visible)

SUMMARY:
----------------------------------------
  Data labels: 5
  Text labels: 1
  Total:       6
```

---

## Understanding Symbol Types

### Data Segment Labels
These are variables and constants in your `.data` section:
- **arr1, arr2, arr3** - Arrays
- **str1, str2** - Strings

### Text Segment Labels
These are code labels (functions, loops, etc.) in your `.text` section:
- **main** - Entry point of your program

### Global Symbols
Labels declared with `.globl` directive:
- Visible to other assembly files
- Required for the entry point (main)

---

## Advanced: Getting Memory Addresses

### Option 1: MARS GUI (Most Detailed)
```bash
java -jar Mars.jar task1_corrected.asm
```
Then:
1. Click **Run → Assemble** (or press F3)
2. Go to **Tools → Symbol Table**
3. You'll see addresses like:
   ```
   Symbol       Address
   arr1         0x10010000
   arr2         0x10010008
   main         0x00400000
   ```

### Option 2: Memory Dump After Assembly
```bash
# Assemble and dump data segment
java -jar Mars.jar a nc dump .data HexText data_dump.txt task1_corrected.asm

# Assemble and dump text segment
java -jar Mars.jar a nc dump .text HexText text_dump.txt task1_corrected.asm
```

---

## Common Symbol Naming Rules

### Valid Label Names:
- `main`
- `loop1`
- `_start`
- `my_function`
- `Array_2`

### Invalid Label Names:
- `2loop` (can't start with number)
- `my-function` (no hyphens)
- `my.label` (no dots, except for directives)
- `$register` ($ is for registers)

---

## Quick Commands Cheat Sheet

```bash
# View symbols in your file
./show_symbols.sh task1_corrected.asm

# View symbols with assembly check
./show_symbols.sh task1_corrected.asm --mars

# Compare original vs corrected
./show_symbols.sh task1.asm
./show_symbols.sh task1_corrected.asm

# Open MARS GUI to see addresses
java -jar Mars.jar task1_corrected.asm
# Then: Tools → Symbol Table

# Just list label names
grep -o "^[[:space:]]*[a-zA-Z_][a-zA-Z0-9_]*:" task1_corrected.asm | sed 's/://g' | sed 's/^[[:space:]]*//'
```

---

## Tips

1. **Always check symbols before running** - Helps catch typos in label names
2. **Use meaningful names** - `student_count` is better than `sc`
3. **Follow conventions**:
   - Data labels: lowercase (e.g., `array1`, `message`)
   - Function labels: lowercase (e.g., `main`, `calculate`)
   - Constants: UPPERCASE (e.g., `MAX_SIZE`)
4. **Global symbols** - Use `.globl main` for the entry point
5. **Check for duplicates** - Each label must be unique

---

## Troubleshooting

### "No symbols found"
- Make sure your labels end with a colon `:`
- Check that labels are at the start of the line (or with leading whitespace)

### "Assembly errors"
- Use `./show_symbols.sh file.asm --mars` to check
- Fix syntax errors before checking symbols

### "Can't find label"
- Labels are case-sensitive: `Main` ≠ `main`
- Check spelling carefully

---

## Related Commands

```bash
# Run your code
./run_mars.sh task1_corrected.asm

# View symbols
./show_symbols.sh task1_corrected.asm

# Open MARS GUI
java -jar Mars.jar

# Assemble only (check for errors)
java -jar Mars.jar a nc task1_corrected.asm
```

---

**Created:** December 2, 2025  
**For:** COAL Lab - Lab 5
