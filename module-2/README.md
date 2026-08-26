# Timing Libraries, Synthesis Approaches, and Flip-Flop Coding

## Overview

This module focuses on understanding **timing libraries, the SKY130 PDK, synthesis approaches, flip-flop coding styles, and the RTL-to-gate-level design flow**.

The objective is to understand how technology libraries provide timing information, how RTL designs are synthesized using different approaches, and how sequential elements such as flip-flops are correctly described and verified using Verilog.

---

## Objectives

The main objectives of this module are:

- Understand the basics of the **SKY130 PDK**.
- Understand the purpose of **Liberty (`.lib`) timing libraries**.
- Learn the meaning of the **`tt_025C_1v80`** process corner.
- Explore timing, power, and cell information available in a `.lib` file.
- Understand **hierarchical and flattened synthesis**.
- Compare hierarchical and flattened synthesis approaches.
- Understand different **flip-flop coding styles**.
- Simulate RTL using **Icarus Verilog**.
- Analyze simulation waveforms using **GTKWave**.
- Synthesize RTL using **Yosys**.
- Understand the complete **RTL simulation and synthesis flow**.

---

## Table of Contents

1. [SKY130 PDK](#1-sky130-pdk)
   - [Understanding `tt_025C_1v80`](#11-understanding-tt_025c_1v80)
   - [Exploring the `.lib` File](#12-exploring-the-lib-file)
2. [Synthesis Approaches](#2-synthesis-approaches)
   - [Hierarchical Synthesis](#21-hierarchical-synthesis)
   - [Flattened Synthesis](#22-flattened-synthesis)
   - [Comparison](#23-comparison-of-hierarchical-and-flattened-synthesis)
3. [Flip-Flop Coding](#3-flip-flop-coding)
4. [RTL Simulation and Synthesis Flow](#4-rtl-simulation-and-synthesis-flow)
5. [Summary](#5-summary)

---

# 1. SKY130 PDK

## What is SKY130?

**SKY130** is an open-source semiconductor process design kit (PDK) based on a **130 nm CMOS technology**.

It provides the files and information required for designing and analyzing circuits using the SKY130 technology.

The PDK includes:

- Standard-cell libraries
- Timing libraries
- Technology files
- Design rules
- SPICE models
- Process information

In this module, the SKY130 standard-cell library is used with **Yosys** for RTL synthesis and technology mapping.

---

## 1.1 Understanding `tt_025C_1v80`

A SKY130 timing library filename may contain information about the **process, voltage, and temperature corner**.

Example:

```text
sky130_fd_sc_hd__tt_025C_1v80.lib
The name represents the operating conditions of the library.
| Part   | Meaning                        |
| ------ | ------------------------------ |
| `tt`   | Typical-Typical process corner |
| `025C` | Temperature = 25°C             |
| `1v80` | Supply Voltage = 1.80 V        |
| `.lib` | Liberty timing library         |

Therefore:

tt_025C_1v80

represents a Typical-Typical process corner at 25°C and 1.80 V.

## 1.2 Exploring the .lib File

A Liberty (.lib) file contains information about standard cells.

It includes:

Cell names
Input and output pins
Logic functions
Timing information
Area information
Power information
View the Library File
less sky130_fd_sc_hd__tt_025C_1v80.lib
Search for Cells
grep -n "cell (" sky130_fd_sc_hd__tt_025C_1v80.lib
Search for Timing Information
grep -n "timing" sky130_fd_sc_hd__tt_025C_1v80.lib

The .lib file helps synthesis tools understand the functionality, timing, area, and power characteristics of standard cells.

# 2. Synthesis Approaches

Synthesis converts a Verilog RTL design into a gate-level representation.

Two basic synthesis approaches are:
Hierarchical Synthesis
Flattened Synthesis

## 2.1 Hierarchical Synthesis
In hierarchical synthesis, the module structure of the RTL design is preserved.

Example
Top Module
├── Module A
├── Module B
└── Module C
Advantages
Preserves module structure.
Easier to understand.
Easier to debug.
Useful for large designs.

## Yosys Commands

Start Yosys:

yosys

Read the Verilog design:

read_verilog design.v

## Set the top module:

hierarchy -top top

## Run synthesis:

synth

## Display the synthesized design:

show

## 2.2 Flattened Synthesis

In flattened synthesis, the module hierarchy is removed and the complete design is combined into one logic structure.

Example
Top Module
└── Combined Logic

## Advantages
Allows optimization across module boundaries.
Removes unnecessary hierarchy.
Provides a combined logic representation.

## Yosys Commands

## Start Yosys:

yosys

## Read the Verilog design:

read_verilog design.v

 ## Set the top module:

hierarchy -top top

 ## Flatten the design:

flatten

## Run synthesis:

synth

 ## Display the synthesized design:

show
# 2.3 Comparison of Hierarchical and Flattened Synthesis
Feature	Hierarchical Synthesis	Flattened Synthesis
Module hierarchy	Preserved	Removed
Design structure	Modular	Combined
Debugging	Easier	More difficult
Optimization	Within hierarchy	Across hierarchy
Design understanding	Easier	More complex

Key Difference

Hierarchical → Module structure is preserved

Flattened → Module structure is combined

# 4. RTL Simulation and Synthesis Flow

## Start Yosys:

yosys

## Read the Verilog design:

read_verilog design.v

## Run synthesis:

synth -top dff

## Load the Liberty library:

read_liberty -lib sky130_fd_sc_hd__tt_025C_1v80.lib

## Perform technology mapping:

abc -liberty sky130_fd_sc_hd__tt_025C_1v80.lib

## Display the synthesized design:

show

## Complete RTL Flow

Verilog RTL
     ↓
 Testbench
     ↓
Icarus Verilog
     ↓
 Simulation
     ↓
 VCD Waveform
     ↓
 GTKWave
     ↓
Verification
     ↓
   Yosys
     ↓
 RTL Synthesis
     ↓
Technology Mapping
     ↓
Gate-Level Design

# 5. Summary

Key Learnings
Learned the basics of the SKY130 PDK.
Understood the tt_025C_1v80 timing corner.
Explored the Liberty (.lib) file.
Understood standard-cell information.
Learned hierarchical synthesis.
Learned flattened synthesis.
Compared both synthesis approaches.
Learned basic D flip-flop coding.
Understood the use of non-blocking assignments.
Simulated Verilog using Icarus Verilog.
Analyzed waveforms using GTKWave.
Synthesized RTL using Yosys.
Understood the complete RTL-to-gate-level flow.

## Conclusion

This module introduced timing libraries, the SKY130 PDK, synthesis approaches, and flip-flop coding.

The SKY130 Liberty library provided an understanding of technology-specific cell information, while hierarchical and flattened synthesis demonstrated different ways of processing RTL designs.

The flip-flop experiment connected Verilog coding, simulation, waveform analysis, and synthesis, providing a foundation for advanced RTL and VLSI design.
