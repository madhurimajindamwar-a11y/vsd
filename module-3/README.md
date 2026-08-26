# Flip-Flop Coding Styles and RTL Optimization

## Overview

This module focuses on **flip-flop coding styles, synthesis, RTL optimization, and netlist generation**.

The objective is to understand different ways of coding flip-flops, verify their behavior through simulation, synthesize them using Yosys, and observe how synthesis tools optimize RTL into an efficient hardware representation.

---

## Objectives

The main objectives of this module are:

- Understand different **flip-flop coding styles**.
- Study **asynchronous and synchronous D flip-flops**.
- Understand asynchronous reset and set operations.
- Verify flip-flop behavior through simulation.
- Synthesize flip-flop designs using **Yosys**.
- Understand how RTL is mapped to **SKY130 standard cells**.
- Study RTL optimization using constant multiplication.
- Observe how Yosys optimizes arithmetic operations.
- Generate and examine synthesized Verilog netlists.

---

## Table of Contents

1. [Flip-Flop Coding Styles and Optimization](#1-flip-flop-coding-styles-and-optimization)
   - [Asynchronous Reset D Flip-Flop](#11-asynchronous-reset-d-flip-flop)
   - [Asynchronous Set D Flip-Flop](#12-asynchronous-set-d-flip-flop)
   - [Synchronous Reset D Flip-Flop](#13-synchronous-reset-d-flip-flop)
2. [Interesting Optimization](#2-interesting-optimization)
   - [Constant Multiplication](#21-constant-multiplication)
   - [Synthesis and Optimization](#22-synthesis-and-optimization)
   - [Generated Synthesized Netlist](#23-generated-synthesized-netlist)
3. [Overall Results](#3-overall-results)
4. [Conclusion](#4-conclusion)

---

# 1. Flip-Flop Coding Styles and Optimization

Flip-flops are sequential elements used to store data.

The behavior of a flip-flop depends on the clock and control signals such as **reset** and **set**.

Different coding styles can represent different hardware behaviors.

---

## 1.1 Asynchronous Reset D Flip-Flop

An asynchronous reset changes the output immediately when the reset is asserted, without waiting for the clock edge.

**Working**
The flip-flop captures d on the rising edge of clk.
When set = 1, q immediately becomes 1.
The set operation does not wait for the clock.

**1.2  Synchronous Reset D Flip-Flop**

A synchronous reset changes the output only at the active clock edge.
**Working**
The flip-flop is triggered by the rising edge of clk.
If reset = 1 at the clock edge, q becomes 0.
The reset operation depends on the clock.

** Asynchronous vs Synchronous Reset**
| Feature                  | Asynchronous Reset             | Synchronous Reset |
| ------------------------ | ------------------------------ | ----------------- |
| Reset response           | Immediate                      | At clock edge     |
| Clock required for reset | No                             | Yes               |
| Sensitivity list         | Includes reset                 | Clock only        |
| Example                  | `posedge clk or posedge reset` | `posedge clk`     |


## **2. Interesting Optimization**

RTL synthesis tools optimize a design while preserving its required functionality.

Yosys can simplify arithmetic operations and generate an optimized hardware representation.

In this experiment, constant multiplication is used to observe RTL optimization.

## **2.2 Synthesis and Optimization**

## Start Yosys:

yosys

## Read the RTL:

read_verilog mult_constant.v

## Select the top module:

hierarchy -top mult_constant

## Run synthesis:

synth

## Optimize the design:

opt

## Display the synthesized design:

show

## **2.3 Technology Mapping**

## Load the SKY130 Liberty library:

read_liberty -lib sky130_fd_sc_hd__tt_025C_1v80.lib

 ## Perform technology mapping:

abc -liberty sky130_fd_sc_hd__tt_025C_1v80.lib

## Display the mapped design:

show

The RTL is now represented using standard cells from the selected SKY130 library.

## **3. Generated Synthesized Netlist**

A synthesized Verilog netlist can be generated using the write_verilog command.

## Generate Netlist
write_verilog -noattr synthesized_netlist.v

The generated file contains the synthesized hardware representation.

## View the Netlist
less synthesized_netlist.v

or:

cat synthesized_netlist.v

**Netlist Flow**

RTL Code
   ↓
Yosys Synthesis
   ↓
Optimization
   ↓
Technology Mapping
   ↓
Synthesized Netlist

The synthesized netlist can be examined to understand how the original RTL is converted into hardware cells and logic.

## **4. Overall Results**

The following results were obtained from the experiments:

- Different D flip-flop coding styles were studied.
- Asynchronous reset behavior was verified through simulation.
- Asynchronous set behavior was studied.
- Synchronous reset behavior was verified through simulation.
- Flip-flop designs were synthesized using Yosys.
- Synthesized designs were mapped to SKY130 standard cells.
- Constant multiplication operations were synthesized and optimized.
- Yosys generated optimized hardware representations from the RTL.
- Synthesized Verilog netlists were generated and examined.

## **5. Conclusion**

This module provided an understanding of flip-flop coding styles and RTL optimization.

Different reset and set techniques were implemented and verified through simulation. The designs were then synthesized using Yosys to understand how RTL code is converted into hardware.

The constant multiplication experiment demonstrated how synthesis tools optimize arithmetic operations and generate efficient hardware.

Overall, this module strengthened the understanding of the RTL → Optimization → Technology Mapping → Synthesized Netlist flow.
