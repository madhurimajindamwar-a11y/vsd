# Day 1: Verilog RTL Design and Simulation

> **Introduction to Verilog RTL design, simulation, testbench development, and synthesis.**

---

## 📌 Overview

Day 1 covers the basic workflow of **Verilog RTL Design and Simulation**.

A **2-to-1 Multiplexer** is designed, tested, simulated using **Icarus Verilog**, and introduced to **Yosys** for RTL synthesis.

### Design Flow
Write the Verilog RTL code.
Create the testbench.
Compile the design using Icarus Verilog.
Run the simulation.
Verify the output using waveforms.
Synthesize the RTL using Yosys.

## 📑 Table of Contents

1. [Simulator](#1-simulator)
2. [Design and Testbench](#2-design-and-testbench)
3. [Getting Started with Icarus Verilog](#3-getting-started-with-icarus-verilog)
4. [Lab: Simulating a 2-to-1 Multiplexer](#4-lab-simulating-a-2-to-1-multiplexer)
5. [Verilog Code Analysis](#5-verilog-code-analysis)
6. [Introduction to Yosys](#6-introduction-to-yosys)
7. [Summary](#7-summary)

## 1. Simulator

### Icarus Verilog

**Icarus Verilog** is an open-source tool used to compile and simulate Verilog designs.

**Tools used:**

| Tool           | Purpose           |
| -------------- | ----------------- |
| Verilog        | RTL Design        |
| Icarus Verilog | Simulation        |
| GTKWave        | Waveform Analysis |
| Yosys          | RTL Synthesis     |

---

## 2. Design and Testbench

### 2-to-1 Multiplexer
A 2-to-1 Multiplexer selects one of two inputs based on the select signal.

I0 → Input 0
I1 → Input 1
S → Select signal
Y → Output

Operation:
When S = 0, Y = I0
When S = 1, Y = I1

### Testbench

The testbench applies different input combinations and verifies the MUX output.

```
┌────────────┐
│ RTL Design │
└─────┬──────┘
      ↓
┌────────────┐
│ Testbench  │
└─────┬──────┘
      ↓
┌────────────┐
│ Simulation │
└────────────┘
```

## 3. Getting Started with Icarus Verilog

### Check Installation

```bash
iverilog -V
```

### Compile

```bash
iverilog -o mux_sim mux.v mux_tb.v


### Run
```bash
vvp mux_sim
```

### View Waveform

```bash
gtkwave dump.vcd
```

### Simulation Flow

RTL → Testbench → Compile → Simulate → Waveform

## 4. Lab: Simulating a 2-to-1 Multiplexer

### Objective

To design and simulate a **2-to-1 Multiplexer using Verilog HDL**.

### Truth Table

| Select (S) | Output (Y) |
| :--------: | :--------: |
|      0     |     I0     |
|      1     |     I1     |

### Expected Result

The output follows the selected input correctly for all test cases.


## 5. Verilog Code Analysis

The experiment introduces the following Verilog concepts:

* `module` – Defines the design
* `input` / `output` – Defines signals
* `assign` – Implements combinational logic
* `initial` – Generates testbench stimulus
* `$dumpfile` – Creates waveform file
* `$dumpvars` – Records signals

### Verification Flow
Apply different input combinations.
Change the select signal.
Observe the output.
Compare the actual output with the expected output.
Confirm the correct operation of the MUX.

## 6. Introduction to Yosys

**Yosys** is an open-source tool used for **RTL synthesis**.

### Synthesis Flow
Basic Steps
Provide the Verilog RTL file.
Read the design in Yosys.
Process and optimize the RTL.
Perform synthesis.
Generate the synthesized hardware representation.
Yosys converts the RTL description into a synthesized hardware representation.

## 7. Summary
Day 1 introduced the basic **Verilog RTL design and simulation flow**.

### Key Takeaways
* Designed a 2-to-1 Multiplexer
* Created a Verilog testbench
* Simulated the design using Icarus Verilog
* Viewed waveforms using GTKWave
* Learned the basics of RTL synthesis using Yosys

### Final Flow

```text
Design → Testbench → Simulation → Verification → Synthesis
```


### 🛠️ Tools

**Verilog HDL • Icarus Verilog • GTKWave • Yosys**

---
## **Author**
```text

Name: J.Madhurima
College: Anurag University
Branch: Electronics and Communication Engineering (ECE)
```

