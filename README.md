# Day 1 — Verilog RTL Design & Simulation
 **Learning Verilog RTL design, simulation, testbench development, and basic synthesis using Icarus Verilog and Yosys.*
 
## 📌 Overview
Day 1 focuses on the basics of **Verilog RTL Design and Simulation**.
In this lab, a **2-to-1 Multiplexer** is designed using Verilog, verified using a testbench, simulated with **Icarus Verilog**, and introduced to **Yosys** for RTL synthesis.

### 🔄 Design Flow

┌─────────────────────┐
│    Verilog RTL      │
│      mux.v          │
└──────────┬──────────┘
           │
           ▼
┌─────────────────────┐
│     Testbench       │
│      mux_tb.v       │
└──────────┬──────────┘
           │
           ▼
┌─────────────────────┐
│   Icarus Verilog    │
│      Simulator      │
└──────────┬──────────┘
           │
           ▼
┌─────────────────────┐
│     Simulation      │
│       Output        │
└──────────┬──────────┘
           │
           ▼
┌─────────────────────┐
│      GTKWave        │
│  Waveform Analysis  │
└─────────────────────┘

## 📑 Table of Contents

* [1. Simulator](#1-simulator)
* [2. Design and Testbench](#2-design-and-testbench)
* [3. Getting Started with Icarus Verilog](#3-getting-started-with-icarus-verilog)
* [4. Lab: 2-to-1 Multiplexer](#4-lab-2-to-1-multiplexer)
* [5. Verilog Code Analysis](#5-verilog-code-analysis)
* [6. Introduction to Yosys](#6-introduction-to-yosys)
* [7. Summary](#7-summary)

## 1. Simulator
### Icarus Verilog
**Icarus Verilog** is an open-source Verilog simulator used to compile and simulate RTL designs.
### Main Uses
* Verilog compilation
* RTL simulation
* Testbench verification
* Waveform generation

### Simulation Flow

┌──────────────┐
│ Verilog Code │
└──────┬───────┘
       ▼
┌──────────────┐
│ Testbench    │
└──────┬───────┘
       ▼
┌──────────────┐
│ Icarus       │
│ Verilog      │
└──────┬───────┘
       ▼
┌──────────────┐
│ Simulation   │
└──────┬───────┘
       ▼
┌──────────────┐
│ Waveform     │
└──────────────┘

## 2. Design and Testbench
The main design for Day 1 is a **2-to-1 Multiplexer**.

### 🧩 2-to-1 MUX

             ┌─────────────┐
 I0 ────────►│             │
             │    2:1 MUX  ├──────► Y
 I1 ────────►│             │
             │             │
 S  ────────►│   Select    │
             └─────────────┘

### Logic
S = 0  →  Y = I0
S = 1  →  Y = I1

### Design + Testbench

       ┌──────────────┐
       │  mux.v       │
       │  RTL Design  │
       └──────┬───────┘
              │
              ▼
       ┌──────────────┐
       │  mux_tb.v    │
       │  Testbench   │
       └──────┬───────┘
              │
              ▼
       ┌──────────────┐
       │  Simulation  │
       └──────────────┘

## 3. Getting Started with Icarus Verilog
### Check Installation
bash
iverilog -V

### Compile
bash
iverilog -o mux_sim mux.v mux_tb.v

### Run
bash
vvp mux_sim

If a waveform file is generated:
bash
gtkwave dump.vcd

### 📁 Project Structure
┌──────────────────────────┐
│         Day-1/           │
└────────────┬─────────────┘
             │
     ┌───────┼────────┬──────────┐
     ▼       ▼        ▼          ▼
 ┌───────┐ ┌───────┐ ┌────────┐ ┌──────────┐
 │mux.v  │ │mux_tb │ │dump.vcd│ │README.md │
 │  RTL  │ │  Test │ │Waveform│ │   Docs   │
 └───────┘ └───────┘ └────────┘ └──────────┘
## 4. Lab: 2-to-1 Multiplexer

### 🎯 Objective

To design and simulate a **2-to-1 Multiplexer using Verilog HDL** and verify its operation using a testbench.

### Truth Table

|  S  |  I0 |  I1 |  Y  |
| :-: | :-: | :-: | :-: |
|  0  |  0  |  X  |  0  |
|  0  |  1  |  X  |  1  |
|  1  |  X  |  0  |  0  |
|  1  |  X  |  1  |  1  |

### Expected Result
The output `Y` follows:
S = 0  →  I0 selected
S = 1  →  I1 selected

## 5. Verilog Code Analysis
The MUX design introduces some basic Verilog concepts:

| Concept     | Purpose                              |
| ----------- | ------------------------------------ |
| `module`    | Defines the circuit                  |
| `input`     | Declares input signals               |
| `output`    | Declares output signals              |
| `assign`    | Implements combinational logic       |
| `initial`   | Provides testbench stimulus          |
| `$display`  | Displays simulation results          |
| `$dumpfile` | Creates waveform file                |
| `$dumpvars` | Records signals for waveform viewing |

### RTL Verification
┌──────────────────┐
│   Input Signals  │
│   I0, I1, S      │
└────────┬─────────┘
         │
         ▼
┌──────────────────┐
│    2:1 MUX       │
│   RTL Design     │
└────────┬─────────┘
         │
         ▼
┌──────────────────┐
│   Output: Y      │
└────────┬─────────┘
         │
         ▼
┌──────────────────┐
│    Testbench     │
│   Verification   │
└────────┬─────────┘
         │
         ▼
┌──────────────────┐
│  Pass / Fail     │
│     Result       │
└──────────────────┘

## 6. Introduction to Yosys
**Yosys** is an open-source tool used for **RTL synthesis**.
It converts Verilog RTL into a hardware-level representation.

### Synthesis Flow
┌──────────────────┐
│   Verilog RTL    │
└────────┬─────────┘
         │
         ▼
┌──────────────────┐
│      Yosys       │
└────────┬─────────┘
         │
         ▼
┌──────────────────┐
│    Synthesis     │
└────────┬─────────┘
         │
         ▼
┌──────────────────┐
│    Logic Gates   │
└──────────────────┘
Basic command:
bash
yosys

## 7. Summary
Day 1 covered the basic workflow of **Verilog RTL design and simulation**.

### ✅ What I Learned
* Basics of Verilog RTL design
* Creating a 2-to-1 Multiplexer
* Writing a Verilog testbench
* Simulating designs using Icarus Verilog
* Generating and viewing waveforms
* Introduction to RTL synthesis using Yosys

## 🛠️ Tools Used

| Tool               | Purpose           |
| ------------------ | ----------------- |
| **Verilog HDL**    | RTL Design        |
| **Icarus Verilog** | Simulation        |
| **GTKWave**        | Waveform Analysis |
| **Yosys**          | RTL Synthesis     |

### 🚀 Day 1 Completed


> *Building the fundamentals of digital design, one day at a time.*
