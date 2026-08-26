# Gate-Level Simulation, Synthesis Mismatch and Verilog Coding Styles

## Overview

This module focuses on **Gate-Level Simulation, synthesis and simulation mismatch, blocking and non-blocking assignments, and blocking statement caveats**.

The main goal is to understand how Verilog RTL behaves during simulation, how synthesis converts RTL into hardware, and how coding styles can affect the final synthesized design.

---

## Objectives

- Understand **Gate-Level Simulation (GLS)**.
- Understand RTL simulation and Gate-Level Simulation.
- Learn about synthesis and simulation mismatches.
- Understand **blocking (`=`)** and **non-blocking (`<=`)** assignments.
- Learn the correct use of blocking and non-blocking statements.
- Understand common problems with blocking assignments.
- Perform RTL simulation using **Icarus Verilog**.
- View waveforms using **GTKWave**.
- Synthesize RTL using **Yosys**.
- Generate and simulate a synthesized netlist.

---

## Table of Contents

1. [Gate-Level Simulation](#1-gate-level-simulation)
2. [Synthesis and Simulation Mismatch](#2-synthesis-and-simulation-mismatch)
3. [Blocking and Non-Blocking Statements](#3-blocking-and-non-blocking-statements)
4. [Caveats with Blocking Statements](#4-caveats-with-blocking-statements)
5. [Simulation and Synthesis Commands](#5-simulation-and-synthesis-commands)
6. [Overall Flow](#6-overall-flow)
7. [Summary](#7-summary)
8. [Conclusion](#8-conclusion)

---

# 1. Gate-Level Simulation

## What is Gate-Level Simulation?

Gate-Level Simulation (GLS) is the simulation of a **synthesized gate-level netlist**.

RTL simulation checks the original Verilog design, while GLS checks the design after synthesis.

### Basic Flow

```text
RTL Code
   ↓
RTL Simulation
   ↓
Synthesis
   ↓
Gate-Level Netlist
   ↓
Gate-Level Simulation
```
---

## **RTL Simulation vs Gate-Level Simulation**
``` text

| RTL Simulation             | Gate-Level Simulation          |
| -------------------------- | ------------------------------ |
| Uses RTL Verilog           | Uses synthesized netlist       |
| Faster                     | Slower                         |
| Checks functional behavior | Checks post-synthesis behavior |
| Before synthesis           | After synthesis                |
```
---
## **2. Synthesis and Simulation Mismatch**
A synthesis mismatch occurs when the behavior of the RTL simulation is different from the synthesized hardware.

Ideally:
RTL Simulation = Synthesized Hardware Behavior

**Common Causes**
Incorrect blocking assignments.
Incorrect non-blocking assignments.
Incomplete sensitivity lists.
Incomplete assignments.
Unintended latch inference.
Incorrect reset coding.
Multiple drivers.
Race conditions.
Simulation-only constructs.

---
## **3. Blocking and Non-Blocking Statements**

Verilog mainly uses two procedural assignment types:

Blocking assignment: =
Non-blocking assignment: <=

**3.1 Blocking Assignment**

A blocking assignment executes immediately.

Example
always @(*) begin
    a = b;
    y = a;
end

Here, a is updated before y is assigned.
Blocking assignments are generally used for combinational logic.

**3.2 Non-Blocking Assignment**
A non-blocking assignment schedules the update for the end of the current simulation step.

Example
always @(posedge clk) begin
    q <= d;
end

Non-blocking assignments are generally used for sequential logic such as flip-flops and registers.

**3.3 Comparison**
```
| Feature     | Blocking `=`        | Non-Blocking `<=`       |
| ----------- | ------------------- | ----------------------- |
| Update      | Immediate           | Scheduled               |
| Common use  | Combinational logic | Sequential logic        |
| Example     | `always @(*)`       | `always @(posedge clk)` |
| Recommended | Combinational logic | Flip-flops/registers    |
```
---

## **4. Caveats with Blocking Statements**

Blocking assignments can cause unexpected results when used in sequential logic.

**Example**
always @(posedge clk) begin
    q1 = d;
    q2 = q1;
end

Since blocking assignments execute immediately, q2 receives the updated value of q1.

**For sequential logic, the preferred coding style is:**
always @(posedge clk) begin
    q1 <= d;
    q2 <= q1;
end

Now both flip-flops update together at the clock edge.

**Possible Problems with Blocking Assignments**
Unexpected simulation results.
Race conditions.
Order-dependent behavior.
Difficulty matching RTL and hardware behavior.
Possible synthesis and simulation mismatches.

---
## **5. Summary**
- Learned the basics of Gate-Level Simulation.
- Understood RTL simulation and Gate-Level Simulation.
- Studied synthesis and simulation mismatch.
- Learned blocking and non-blocking assignments.
- Understood the correct use of = and <=.
- Studied the caveats of blocking assignments.
- Simulated RTL using Icarus Verilog.
- Viewed waveforms using GTKWave.
- Synthesized RTL using Yosys.
- Generated a synthesized Verilog netlist.
- Understood the basic RTL → Synthesis → GLS flow.
---

## **6.Conclusion**

- This module provided a basic understanding of Gate-Level Simulation, synthesis mismatch, and Verilog coding styles.
- The difference between blocking and non-blocking assignments was studied, along with the problems that can occur when blocking assignments are used incorrectly in sequential logic.
- The complete flow from RTL simulation to synthesis and Gate-Level Simulation was also understood.
