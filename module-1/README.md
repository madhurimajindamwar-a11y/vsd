# Verilog RTL Design and Simulation

## Overview

This module introduces the fundamentals of **Verilog RTL design, simulation, verification, and synthesis**.

The objective is to understand how a digital circuit is described using Verilog HDL, verified using a testbench and simulator, analyzed through waveforms, and synthesized into a gate-level representation using **Yosys** and a standard-cell library.

### Topics Covered

- Simulator, Design and Testbench
- Icarus Verilog
- 2-to-1 Multiplexer Simulation
- Verilog RTL Code Analysis
- Yosys RTL Synthesis
- Liberty Standard-Cell Library
- Technology Mapping

---

## Table of Contents

1. [Simulator, Design and Testbench](#simulator-design-and-testbench)
2. [Getting Started with Icarus Verilog](#1-getting-started-with-icarus-verilog)
3. [Lab: Simulating a 2-to-1 Multiplexer](#2-lab-simulating-a-2-to-1-multiplexer)
4. [Verilog Code Analysis](#3-verilog-code-analysis)
5. [Introduction to Yosys](#4-introduction-to-yosys)
6. [Summary](#5-summary)

---

# Simulator, Design and Testbench

## Simulator

A **simulator** is a software tool used to execute Verilog code and verify the behavior of a digital circuit before implementing it on hardware.

It helps identify design errors and allows the functionality of the RTL design to be verified using different input conditions.

## Design

The **design** is the Verilog module that represents the required digital circuit.

It defines:

- Inputs
- Outputs
- Internal signals
- Logic functionality

## Testbench

A **testbench** is a separate Verilog module used to verify the design.

It:

- Generates input stimulus
- Applies different test conditions
- Observes the outputs
- Verifies the expected functionality

### Basic Verification Flow

```text
Verilog Design
      ↓
   Testbench
      ↓
   Simulator
      ↓
Waveform Analysis
      ↓
   Verification
