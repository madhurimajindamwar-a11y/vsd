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

| State | Matched Sequence |
| ----- | ---------------- |
| S0    | Nothing matched  |
| S1    | `0`              |
| S2    | `00`             |
| S3    | `001`            |
| S4    | `0010`           |
| S5    | `00101`          |
| S6    | `001010`         |

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
```bash
tb/sequence_detector_tb.v
```
The testbench should generate:
Clock
Reset
Serial input sequence
VCD waveform

The target sequence should be tested:
0010100

For waveform generation, use:
```bash
$dumpfile("waveform/rtl.vcd");
$dumpvars(0, sequence_detector_tb);
```
## **5. Pre-Synthesis Simulation**

Pre-synthesis simulation checks whether the original RTL works correctly before synthesis.
Compile RTL

From the project directory:
```bash
iverilog -o sim/rtl_sim rtl/sequence_detector.v tb/sequence_detector_tb.v
Run RTL Simulation
vvp sim/rtl_sim
```
Check the waveform file:
```bash
ls waveform/
```
Expected:
```bash
rtl.vcd
```
## **6. View RTL Waveform**
Open the RTL waveform using GTKWave:
```bash
gtkwave waveform/rtl.vcd
```
Observe:
clk
reset
din
detected

## **7. RTL Waveform Analysis**
The target sequence is:
0010100

The input bits are applied serially on clock cycles.

**Example:**

Cycle:      1  2  3  4  5  6  7
din:        0  0  1  0  1  0  0

After receiving the complete sequence:
0010100

the detector output becomes:
detected = 1

**Verify in GTKWave**

Check:
reset initializes the FSM
din receives the required sequence
FSM progresses through the expected states
detected becomes 1 after sequence detection
No unexpected detection occurs

## **8. Logic Synthesis**
After RTL verification, the design is synthesized using Yosys.
Synthesis converts:
```
RTL Code
   ↓
Gate-Level Logic
```
## **9. Yosys Synthesis Script**

Create:
```bash
scripts/synth.ys
```
Use:
```bash
read_verilog rtl/sequence_detector.v
hierarchy -top sequence_detector
proc
opt
fsm
opt
techmap
opt
write_verilog netlist/sequence_detector_netlist.v
stat
```
## **10. Run Synthesis**
Run Yosys using:
```bash
yosys -s scripts/synth.ys
```
To save the synthesis report:
``` bash
yosys -s scripts/synth.ys | tee reports/synthesis_report.txt
```
## **11. Synthesis Report**
View the synthesis report:

cat reports/synthesis_report.txt

The report contains information such as:
```bash 
Number of wires
Number of wire bits
Number of cells
Flip-flops
Logic elements
Design statistics
```
## **12. Synthesized Netlist**

After synthesis, Yosys generates:
```bash
netlist/sequence_detector_netlist.v
```
Check the netlist:

ls netlist/

Expected:
```bash
sequence_detector_netlist.v
```
View the netlist:
```bash
cat netlist/sequence_detector_netlist.v
```
The synthesized netlist represents the RTL design using lower-level hardware logic.

## **13. Post-Synthesis Gate-Level Simulation**
Post-synthesis simulation verifies the synthesized netlist.
The simulation changes from:

RTL + Testbench

to:

Synthesized Netlist + Testbench

## **14. Compile GLS**
Compile the synthesized netlist with the testbench:
```bash
iverilog -o sim/gls_sim netlist/sequence_detector_netlist.v tb/sequence_detector_tb.v
```
If technology-library cells are used, include the required library file:
```bash
iverilog -o sim/gls_sim \
netlist/sequence_detector_netlist.v \
<technology_library>.v \
tb/sequence_detector_tb.v
```
## **15. Run GLS**
Run the gate-level simulation:
```bash
vvp sim/gls_sim
```
Check the waveform:

ls waveform/

Expected:
```bash
rtl.vcd
gls.vcd
```
## **16. RTL vs GLS Comparison**

| Parameter    | RTL              | GLS                  |
| ------------ | ---------------- | -------------------- |
| Input Design | RTL              | Synthesized Netlist  |
| Logic Level  | RTL              | Gate Level           |
| Synthesis    | No               | Yes                  |
| Simulation   | Functional       | Gate-Level           |
| Main Purpose | RTL Verification | Netlist Verification |
| Timing       | Ideal            | Timing Dependent     |
| Waveform     | RTL VCD          | GLS VCD              |

**17. Complete RTL-to-GLS Flow**
```text
             Sequence Detector RTL
                      │
                      ▼
             Pre-Synthesis Simulation
                      │
                      ▼
                 RTL Waveform
                      │
                      ▼
                Yosys Synthesis
                      │
                      ▼
              Synthesized Netlist
                      │
                      ▼
            Post-Synthesis GLS
                      │
                      ▼
                 GLS Waveform
                      │
                      ▼
              RTL vs GLS Analysis
                      │
                      ▼
                Final Verification
```
## **18. Complete Linux Commands**
Create Directories
```bash
mkdir -p rtl tb netlist scripts sim waveform reports
```
Check Project Files
```bash
ls
```
Compile RTL
```bash
iverilog -o sim/rtl_sim rtl/sequence_detector.v tb/sequence_detector_tb.v
```
Run RTL Simulation
```bash
vvp sim/rtl_sim
```
Open RTL Waveform
```bash
gtkwave waveform/rtl.vcd
```
Run Synthesis
```bash
yosys -s scripts/synth.ys
```
Generate Synthesis Report
```bash
yosys -s scripts/synth.ys | tee reports/synthesis_report.txt
```
Check Netlist
```bash
ls netlist/
```
View Netlist
```bash
cat netlist/sequence_detector_netlist.v
```
Compile GLS
```bash
iverilog -o sim/gls_sim netlist/sequence_detector_netlist.v tb/sequence_detector_tb.v
```
Run GLS
```bash
vvp sim/gls_sim
```
Open GLS Waveform
```bash
gtkwave waveform/gls.vcd
```

## **19. Conclusion**

The Sequence Detector was successfully verified through the complete RTL-to-GLS flow.

The RTL simulation verified the functional behavior for the target sequence 0010100. Yosys synthesized the RTL into a gate-level netlist, and post-synthesis GLS verified that the synthesized design maintains the expected sequence-detection functionality.


## **👩‍💻 Author**
```text
J. Madhurima

B.Tech – Electronics and Communication Engineering
```
