# BabySoC Physical Design Flow

## Overview

This project demonstrates the complete digital IC design flow for **BabySoC**, starting from RTL design and functional verification and progressing through logic synthesis and post-synthesis gate-level simulation.

The main objective is to verify that the synthesized hardware preserves the functionality of the original RTL design.

### Design Flow

1. RTL Design
2. Pre-Synthesis RTL Simulation
3. Logic Synthesis
4. Gate-Level Netlist Generation
5. Post-Synthesis Gate-Level Simulation (GLS)
6. RTL vs GLS Comparison
7. Timing and Functional Analysis
8. Final Verification

---

## 1. Requirements

### Hardware

- Linux/Ubuntu system
- Minimum 4 GB RAM recommended
- At least 10 GB free disk space

### Software

The following tools are required:

- Linux / Ubuntu
- Git
- Icarus Verilog
- GTKWave
- Yosys
- Standard Unix utilities

### Check Installed Tools

```bash
git --version
iverilog -V
vvp -V
yosys -V
gtkwave --version
```
## **2. Pre-Synthesis / RTL Simulation**

**What is RTL Simulation?**
RTL simulation verifies the functionality of the original Verilog RTL before synthesis.

At this stage:
```text 
RTL Code
   ↓
RTL Testbench
   ↓
RTL Simulator
   ↓
Waveform
   ↓
Functional Verification

```

The RTL simulation confirms that the design behaves as expected.

## **2.1 Compile the RTL**

Navigate to the project directory:
```bash
cd BabySoC
```

Compile the RTL and testbench:
```bash
iverilog -o sim/rtl_sim.vvp \
rtl/babysoc.v \
rtl/cpu.v \
rtl/memory.v \
tb/babysoc_tb.v
```
If the design contains more RTL files, include them in the command.

## **2.2 Run RTL Simulation**
```bash
vvp sim/rtl_sim.vvp
```
If the testbench generates a VCD waveform:
```bash
waveforms/rtl.vcd
```
will be created.

## **2.3 View RTL Waveform**

Open the waveform using GTKWave:
```bash
gtkwave waveforms/rtl.vcd
```

Observe important signals such as:

Clock
Reset
Inputs
Outputs
FSM states
Control signals
Data signals

## **2.4 RTL Simulation Checklist**
Verify:

Reset operates correctly
Clock is generated correctly
Inputs are applied correctly
Outputs match expected values
No unexpected X or Z values
Functional sequence is correct
Testbench completes successfully

## **3. Logic Synthesis**

**What is Logic Synthesis?**

Logic synthesis converts the RTL description into a gate-level netlist.
``` text
RTL
 ↓
Logic Synthesis
 ↓
Gate-Level Netlist
```

The synthesized netlist contains hardware elements such as:
AND gates
OR gates
NOT gates
Multiplexers
Flip-flops
Logic cells

## **4. Yosys Synthesis**
Yosys can be used as an open-source synthesis tool.

Create a synthesis script:
```bash
synth/synth.ys
```
**Example:**
```bash
read_verilog rtl/babysoc.v
read_verilog rtl/cpu.v
read_verilog rtl/memory.v

hierarchy -top babysoc

proc
opt
fsm
opt
memory
opt
techmap
opt
abc -g simple
clean
write_verilog synth/gate_netlist.v
stat
```
Modify the RTL file names and top module name according to your BabySoC design.

## **4.1 Run Synthesis**

From the project root:
```bash
yosys synth/synth.ys
```
## **4.2 Save Synthesis Report**

To save the Yosys output:
```bash
yosys -s synth/synth.ys | tee reports/synthesis_report.txt
```
## **4.3 Check Synthesis Statistics**

Yosys reports information such as:
Number of wires
Number of wire bits
Number of memories
Number of processes
Number of cells

These values help understand the hardware generated from the RTL.

## **5. Gate-Level Netlist**
After synthesis, the file:
```bash
synth/gate_netlist.v
```
contains the synthesized gate-level representation.

The flow becomes:
```text 
RTL
 ↓
Yosys
 ↓
Synthesis
 ↓
Gate-Level Netlist
```
The gate-level netlist should be functionally equivalent to the RTL for the tested behavior.

## **6. Gate-Level Simulation (GLS)**

**What is GLS?**
Gate-Level Simulation (GLS) verifies the synthesized gate-level netlist using a testbench.

Instead of simulating RTL:
RTL + Testbench

GLS simulates:
Gate-Level Netlist + Testbench

This helps verify that synthesis has not changed the intended functionality.

## **7. Post-Synthesis GLS**

Post-Synthesis Simulation Flow
```text 
Synthesized Netlist
        +
    Testbench
        ↓
    GLS Simulation
        ↓
     Waveform
        ↓
Functional Comparison
```

## **7.1 Compile the Gate-Level Netlist**
Use Icarus Verilog:
```bash 
iverilog -o sim/gls_sim.vvp \
synth/gate_netlist.v \
tb/babysoc_tb.v
```
If additional library or support files are required, include them:
```bash 
iverilog -o sim/gls_sim.vvp \
synth/gate_netlist.v \
rtl/*.v \
tb/babysoc_tb.v
```
Use only the files required by the synthesized netlist.

## **7.2 Run GLS**
```bash
vvp sim/gls_sim.vvp
```
If the testbench generates a waveform:
```bash 
waveforms/gls.vcd
```
will be generated.

## **7.3 View GLS Waveform**
```bash 
gtkwave waveforms/gls.vcd
```
Compare the important signals with the RTL waveform.

## **8. RTL vs Post-Synthesis Comparison**
The main objective of GLS is to determine whether the synthesized design preserves the behavior of the RTL.

**Comparison Parameters**
```text 
| Parameter      | RTL Simulation       | Post-Synthesis GLS                |
| -------------- | -------------------- | --------------------------------- |
| Design         | RTL                  | Gate-level netlist                |
| Simulation     | Functional           | Gate-level                        |
| Logic          | Behavioral/RTL       | Synthesized gates                 |
| Timing         | Ideal RTL timing     | Gate-level delays if modeled      |
| Purpose        | Verify functionality | Verify synthesized implementation |
| Waveform       | RTL waveform         | GLS waveform                      |
| Expected Logic | Correct              | Same functional behavior          |
```

## **9. Complete Command Flow**
The complete basic flow can be executed as follows:

**Step 1:** Create Directories
```bash
mkdir -p sim waveforms reports synth
```
**Step 2:** Run RTL Simulation
```bash
iverilog -o sim/rtl_sim.vvp rtl/*.v tb/babysoc_tb.v
vvp sim/rtl_sim.vvp
```
**Step 3:** Open RTL Waveform
```bash
gtkwave waveforms/rtl.vcd
```
**Step 4:** Run Logic Synthesis
```bash 
yosys -s synth/synth.ys | tee reports/synthesis_report.txt
```
**Step 5:** Check Generated Netlist
```bash
ls -l synth/gate_netlist.v
```
**Step 6:** Run Post-Synthesis GLS
```bash 
iverilog -o sim/gls_sim.vvp \
synth/gate_netlist.v \
tb/babysoc_tb.v
vvp sim/gls_sim.vvp
```
**Step 7:** Open GLS Waveform
```bash
gtkwave waveforms/gls.vcd
```
**Step 8:** Compare RTL and GLS
Compare:
Inputs
Clock
Reset
Outputs
State transitions
Detection sequence
Timing

## **10. Pre-Synthesis vs Post-Synthesis**
```text

| Feature               | Pre-Synthesis RTL       | Post-Synthesis GLS           |
| --------------------- | ----------------------- | ---------------------------- |
| Design Representation | RTL                     | Gate-level netlist           |
| Tool                  | Icarus Verilog          | Icarus Verilog               |
| Source                | Verilog RTL             | Synthesized Verilog          |
| Logic                 | Abstract RTL            | Gates/cells                  |
| Main Purpose          | Functional verification | Synthesis verification       |
| Timing                | Mostly ideal            | Gate delays may be present   |
| Optimization          | Not applied             | Applied by synthesis         |
| Waveform              | RTL waveform            | GLS waveform                 |
| Hardware Structure    | Not visible directly    | Visible as synthesized logic |
| Expected Function     | Correct                 | Same as RTL                  |

```
## **11.Final Conclusion**

The BabySoC design was verified through both pre-synthesis RTL simulation and post-synthesis gate-level simulation. The RTL simulation confirmed the intended functional behavior, while synthesis converted the RTL into a gate-level netlist for further verification.

The RTL and GLS waveforms are compared using the same testbench and input sequence. If the logical output and detection sequence remain the same, the synthesized implementation preserves the functional behavior of the RTL. Any small timing difference observed in GLS can be attributed to gate-level propagation delays when timing information is modeled.

## **12. Overall BabySoC Flow**
```text 
             BabySoC RTL
                  │
                  ▼
        Pre-Synthesis Simulation
                  │
                  ▼
            RTL Waveform
                  │
                  ▼
          Functional Check
                  │
                  ▼
           Logic Synthesis
              (Yosys)
                  │
                  ▼
       Gate-Level Netlist
                  │
                  ▼
      Post-Synthesis GLS
                  │
                  ▼
            GLS Waveform
                  │
                  ▼
        RTL vs GLS Comparison
                  │
                  ▼
       Functional Verification
                  │
                  ▼
             Final Result
```
---
**Author**
J. Madhurima
B.Tech – Electronics and Communication Engineering
