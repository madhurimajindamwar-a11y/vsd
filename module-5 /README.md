# IF, CASE and Looping Constructs

## Overview

This module covers important **Verilog RTL coding constructs** such as `if`, `case`, `for`, and `generate-for`.

The designs are simulated using **Icarus Verilog**, waveforms are viewed using **GTKWave**, and synthesis is performed using **Yosys**.

---

## Objectives

- Understand `if-else` statements.
- Study incomplete `if` and latch inference.
- Understand complete and incomplete `case` statements.
- Study `for` and `generate-for` loops.
- Implement MUX and DEMUX designs.
- Design and simulate a Full Adder.
- Design and simulate a Ripple Carry Adder.
- Perform RTL simulation and synthesis.
- Generate and view simulation waveforms.

---

## Table of Contents

1. [IF Construct](#1-if-construct)
2. [CASE Construct](#2-case-construct)
3. [Looping Constructs](#3-looping-constructs)
4. [MUX and DEMUX](#4-mux-and-demux)
5. [Full Adder](#5-full-adder)
6. [Ripple Carry Adder](#6-ripple-carry-adder)
7. [Simulation and Synthesis](#7-simulation-and-synthesis)
8. [Summary](#8-summary)
9. [Conclusion](#9-conclusion)

---

# 1. IF Construct

The `if-else` statement is used to implement conditional logic.

### Example

```verilog
always @(*) begin
    if (sel)
        y = a;
    else
        y = b;
end
```
When sel = 1, y = a.
When sel = 0, y = b.

---
## **Incomplete IF**
An incomplete if does not assign an output for every condition.
```text
always @(*) begin
    if (sel)
        y = a;
end
```
When sel = 0, y is not assigned.
This can cause latch inference during synthesis.

---
## **2. CASE Construct**

The case statement is useful when one signal needs to be compared with multiple values.

**Complete CASE**
```text 
always @(*) begin
    case (sel)
        2'b00: y = a;
        2'b01: y = b;
        2'b10: y = c;
        2'b11: y = d;
        default: y = 1'b0;
    endcase
end
```
A complete case provides an output for all possible conditions.

**Bad / Incomplete CASE**
```text 
always @(*) begin
    case (sel)
        2'b00: y = a;
        2'b01: y = b;
        2'b10: y = c;
    endcase
end
```
There is no assignment for:
sel = 2'b11
This can result in latch inference.

**Complete CASE vs Incomplete CASE**
```text

| Complete CASE           | Incomplete CASE             |
| ----------------------- | --------------------------- |
| All conditions handled  | Some conditions missing     |
| Predictable output      | Output may retain old value |
| Avoids unintended latch | May infer latch             |
| Recommended             | Avoid when unintended       |
```

---
## **3. Looping Constructs**
Loops are used to repeat operations in Verilog.

**Common loops include:**
for
while
repeat
forever

This module mainly focuses on for and generate-for loops.

**For Loop**
A for loop can be used inside an always block.

**Example**
``  text
integer i;

always @(*) begin
    for (i = 0; i < 4; i = i + 1)
        y[i] = a[i] & b[i];
end
```

During synthesis, a fixed-size for loop is generally converted into repeated hardware.
```text
for loop
   ↓
Synthesis
   ↓
Repeated Logic
```

**Generate-For Loop**

A generate-for loop is used to create multiple hardware instances.

**Example**
```text
genvar i;

generate
    for (i = 0; i < 4; i = i + 1) begin
        and_gate u (
            .a(a[i]),
            .b(b[i]),
            .y(y[i])
        );
    end
endgenerate
```

**Difference**
for loop
→ Repeats statements inside a procedural block

generate-for
→ Creates multiple hardware instances

---
## **4. Full Adder**

A Full Adder adds three inputs:

A + B + Cin

**It produces:**
Sum
Carry

**Simulation Commands**
```text 
iverilog -o fa.out full_adder.v tb_full_adder.v
vvp fa.out
gtkwave fa.vcd
```
---
## **5. Ripple Carry Adder**

A Ripple Carry Adder is built by connecting multiple Full Adders.

**For a 4-bit RCA:**

FA0 → FA1 → FA2 → FA3

The carry output of one Full Adder becomes the carry input of the next.
**Simulation Commands**
```text
iverilog -o rca.out full_adder.v ripple_carry_adder.v tb_rca.v
vvp rca.out
gtkwave rca.vcd
```
---
## **6. Summary**

- Studied if-else statements.
- Understood incomplete if and latch inference.
- Studied complete and incomplete case.
- Learned the importance of default.
- Learned for loops.
- Learned generate-for loops.
- Implemented MUX and DEMUX.
- Designed and simulated a Full Adder.
- Designed and simulated a Ripple Carry Adder.
- Performed simulation using Icarus Verilog.
- Viewed waveforms using GTKWave.
- Performed synthesis using Yosys.
---
## **7. Conclusion**

This module provided a basic understanding of conditional and looping constructs in Verilog.

The experiments showed how incomplete if and case statements can lead to unintended latches. The for and generate-for constructs were used to describe repeated hardware.

MUX, DEMUX, Full Adder, and Ripple Carry Adder designs were also implemented, simulated, and synthesized.



