# Sequence Detector – RTL to GLS

## 📌 Overview

This project implements a **Sequence Detector using Verilog HDL**.

The complete flow covers:

- RTL Design
- Pre-Synthesis Simulation
- Logic Synthesis
- Synthesized Netlist
- Post-Synthesis Gate-Level Simulation (GLS)
- RTL vs GLS Waveform Analysis

---

## 🎯 Objective

To design and verify a Sequence Detector and understand the complete:

**RTL → Synthesis → Netlist → GLS**

flow using open-source EDA tools.

---

## 🛠️ Tools Required

| Tool | Purpose |
|------|---------|
| Icarus Verilog | RTL and GLS Simulation |
| Yosys | Logic Synthesis |
| GTKWave | Waveform Analysis |
| Linux/Ubuntu | EDA Environment |
| Git | Version Control |

### Install Tools

```bash
sudo apt update
sudo apt install iverilog yosys gtkwave git
```
---

## **1. RTL Design**
RTL File
```bash
rtl/sequence_detector.v
```
The design contains the following ports:
##

| Signal     | Type   | Description               |
| ---------- | ------ | ------------------------- |
| `clk`      | Input  | Clock signal              |
| `reset`    | Input  | Active-high reset         |
| `din`      | Input  | Serial input data         |
| `detected` | Output | Sequence detection output |
##
**RTL Module**
``` bash
module sequence_detector (
    input  wire clk,
    input  wire reset,
    input  wire din,
    output reg  detected
);
```
**2. FSM Design**

The detector uses 7 states.
```bash
STATE_W   = 3
NUM_STATES = 7
```
Each state represents the amount of the target sequence already matched.

Target:
```bash 
0010100
```
**State Representation**
```text
| State | Matched Sequence |
| ----- | ---------------- |
| S0    | Nothing matched  |
| S1    | `0`              |
| S2    | `00`             |
| S3    | `001`            |
| S4    | `0010`           |
| S5    | `00101`          |
| S6    | `001010`         |
```
When the FSM is in S6 and receives 0:

001010 + 0 = 0010100
The sequence is detected.

Therefore:
detected = 1

After detection, the FSM moves to State 2 to allow overlapping sequence detection.

## **3. RTL Logic**
The design contains two main sequential/combinational blocks.

Next-State Logic
```bash 
always @(*) begin
    next_state = 'd0;
    next_detected = 1'b0;

    case (state)
        ...
    endcase
end
```
This block determines:

Next FSM state
Detection output
**State Register**
```bash 
always @(posedge clk) begin
    if (reset) begin
        state <= 'd0;
        detected <= 1'b0;
    end else begin
        state <= next_state;
        detected <= next_detected;
    end
end
```

This block updates the state and output on the rising edge of the clock.

## ** 4. Testbench **

Testbench File
tb/sequence_detector_tb.v

The testbench should generate:

Clock
Reset
Serial input sequence
VCD waveform

The target sequence should be tested:

0010100

For waveform generation, use:

$dumpfile("waveform/rtl.vcd");
$dumpvars(0, sequence_detector_tb);

