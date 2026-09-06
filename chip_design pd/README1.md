# CHIP DESIGN – PHYSICAL DESIGN (PD)

## Module 1: Inception of Open-Source EDA, OpenLane & SKY130 PDK

---

## 1. Overview

This module introduces the basics of:

- Open-source EDA
- OpenLane
- SKY130 PDK
- QFN-48 package
- Chip, die, core, pads and IPs
- RISC-V ISA
- Software to hardware flow
- SoC design
- RTL-to-GDS flow
- Open-source EDA tools
- Design preparation
- Synthesis
- Synthesis result characterization

---

# 2. How to Talk to Computers

## 2.1 Introduction to QFN-48 Package

**QFN** stands for **Quad Flat No-lead**.

A QFN-48 package has 48 external electrical connections and contains the semiconductor die.

### Basic Structure

```text
             QFN-48 PACKAGE
        ┌─────────────────────┐
        │                     │
        │       CHIP/DIE      │
        │                     │
        │      ┌───────┐      │
        │      │ CORE  │      │
        │      └───────┘      │
        │                     │
        └─────────────────────┘
          │ │ │ │ │ │ │ │
         PADS / CONNECTIONS
```
---
## **2.2 Important Chip Terms**

| Term              | Meaning                                                |
| ----------------- | ------------------------------------------------------ |
| **Package**       | Physical outer body containing the chip                |
| **Die**           | Silicon piece containing the circuit                   |
| **Core**          | Main area where digital logic is placed                |
| **Pad**           | Electrical connection between die and package          |
| **IP**            | Reusable Intellectual Property block                   |
| **Standard Cell** | Pre-designed logic cell such as NAND, NOR or Flip-Flop |
| **Macro**         | Large predefined block such as memory or processor     |

**Relationship**
``` text
Package
   ↓
Die
   ↓
Core
   ↓
Standard Cells + Macros + IPs
```
---
## **3. Introduction to RISC-V ISA**

**What is ISA?**
ISA (Instruction Set Architecture) defines the interface between software and processor hardware.

It specifies:
```
Instructions
Registers
Data types
Memory operations
Arithmetic operations
Control operations
```
**RISC-V**
RISC-V is an open standard Instruction Set Architecture based on the RISC concept.

**Basic Flow**
```text
Software
   ↓
RISC-V Instructions
   ↓
RISC-V Processor
   ↓
Digital Hardware        
```
---
## **4. From Software Application to Hardware**

Software is converted into machine instructions that are executed by hardware.
**Software to Hardware Flow**
```text
Application
     ↓
High-Level Language
     ↓
Compiler
     ↓
Assembly
     ↓
Machine Code
     ↓
RISC-V Processor
     ↓
RTL
     ↓
Logic Gates
     ↓
Physical Layout
     ↓
GDSII
     ↓
Fabricated Chip
```
---
## **5. SoC Design and OpenLane**
**5.1 What is SoC?**
SoC = System on Chip
An SoC integrates multiple components into a single chip.

**Typical SoC Components**
```
CPU
Memory
GPIO
UART
SPI
I2C
Timers
Bus
Clock circuits
Other IP blocks
```
**Basic SoC Structure**
                 SoC
    ┌──────────────────────────┐
    │                          │
    │     CPU      Memory      │
    │      │          │        │
    │      └──── Bus ─┘        │
    │           │              │
    │     ┌─────┼─────┐        │
    │    GPIO  UART  SPI       │
    │                          │
    │         Timer            │
    │                          │
    └──────────────────────────┘
---
## **6. Components of Open-Source Digital ASIC Design**
A digital ASIC design flow requires several tools and components.

| Component             | Purpose                      |
| --------------------- | ---------------------------- |
| RTL                   | Describes digital hardware   |
| Simulator             | Verifies RTL functionality   |
| Synthesis Tool        | Converts RTL into gates      |
| Standard Cell Library | Provides logic cells         |
| Floorplanning Tool    | Defines chip organization    |
| Placement Tool        | Places standard cells        |
| CTS Tool              | Builds clock network         |
| Routing Tool          | Connects cells               |
| STA Tool              | Checks timing                |
| DRC                   | Checks design rules          |
| LVS                   | Compares layout with netlist |
| GDSII                 | Final physical layout        |

---
## **7. Simplified RTL-to-GDS Flow**
```text
RTL
 ↓
RTL Simulation
 ↓
Synthesis
 ↓
Floorplanning
 ↓
Power Planning
 ↓
Placement
 ↓
Clock Tree Synthesis
 ↓
Routing
 ↓
STA
 ↓
DRC / LVS
 ↓
GDSII
```
---
## **8. Introduction to OpenLane**
OpenLane is an open-source RTL-to-GDS ASIC design flow.

It automates major stages of digital ASIC physical design.
```text
OpenLane Uses
RTL synthesis
Floorplanning
Placement
Clock Tree Synthesis
Routing
Timing analysis
Physical verification
GDSII generation
```
**Basic OpenLane Flow**
```
RTL
 ↓
Yosys
 ↓
Synthesis
 ↓
OpenROAD
 ↓
Floorplan
 ↓
Placement
 ↓
CTS
 ↓
Routing
 ↓
Magic / KLayout
 ↓
GDSII
```
## **9. OpenLane Detailed ASIC Design Flow**

**9.1 Design Preparation**
**Inputs:**
``` text
RTL files
Configuration files
Timing constraints
PDK
Standard cell libraries
```
**Output:**
```text
Prepared design
Configuration
Required libraries
```
**9.2 Synthesis**
Synthesis converts RTL into a gate-level netlist.
```text
RTL
 ↓
Yosys
 ↓
Gate-Level Netlist
```
**Main Objectives**
```
Logic optimization
Technology mapping
Cell selection
Area estimation
Timing estimation
```
**9.3 Floorplanning**
**Floorplanning defines:**
```text
Die area
Core area
I/O locations
Macro locations
Power structure
```
**9.4 Placement**
Placement determines the physical locations of standard cells.
```text
Gate-Level Netlist
        ↓
     Placement
        ↓
Physical Cell Locations
```
**9.5 Clock Tree Synthesis**
CTS creates the clock distribution network.

**Objectives**
```text
Reduce clock skew
Control clock latency
Meet timing requirements
```
**9.6 Routing**
Routing creates physical connections between cells.
**Types**
Global Routing
Detailed Routing

**9.7 Signoff**
Important checks include:
```text
Static Timing Analysis
DRC
LVS
Antenna Checks
Power Analysis
```
**9.8 GDSII**
GDSII is the final layout database used for chip fabrication.
```text
RTL
 ↓
Netlist
 ↓
Physical Layout
 ↓
GDSII
 ↓
Fabrication
```
---
## **10. Open-Source EDA Tools**

| Tool         | Purpose                         |
| ------------ | ------------------------------- |
| **Yosys**    | RTL synthesis                   |
| **OpenROAD** | Physical design                 |
| **OpenSTA**  | Static Timing Analysis          |
| **Magic**    | Layout and DRC                  |
| **Netgen**   | LVS                             |
| **KLayout**  | Layout viewing and verification |
| **GTKWave**  | Waveform viewing                |
| **OpenLane** | Complete RTL-to-GDS flow        |

---
## **11. SKY130 PDK**
**What is PDK?**
PDK = Process Design Kit

A PDK provides technology-specific information required for chip design.

**PDK Contains**
```
Design rules
Technology files
Standard cell libraries
SPICE models
Layer information
Timing information
Physical abstracts
```
**SKY130**
SKY130 is an open-source PDK for the SkyWater 130 nm process.

**Relationship**
```text
RTL Design
     +
EDA Tools
     +
SKY130 PDK
     ↓
ASIC Physical Design
```
---
## **12. Design Preparation**
Before running OpenLane, prepare the design.

**Steps**
```text
Create the design directory
Add RTL files
Create configuration file
Select PDK
Define clock
Define timing constraints
Run the design
Check generated files
Run synthesis
```
**12.1 Basic Linux Commands**

Go to OpenLane:
```bash
cd ~/OpenLane
```
Check files:
```bash
ls
```
Go to the design:
```bash
cd designs/my_design
```
Check design files:
```bash
ls
```
Create a directory:
```bash
mkdir src
```
---
## **13. Review Files After Design Preparation**

After running the flow, OpenLane creates a run directory.
```bash
cd designs/my_design/runs
```
List the available runs:
```bash
ls
```
A typical run contains:
```bash
runs/
└── RUN_TAG/
    ├── config.tcl
    ├── logs/
    ├── reports/
    ├── results/
    └── tmp/
```
**Important Directories**

| Directory  | Contents                              |
| ---------- | ------------------------------------- |
| `logs/`    | Tool execution logs                   |
| `reports/` | Timing and synthesis reports          |
| `results/` | Generated netlists and physical files |
| `tmp/`     | Temporary/intermediate files          |

---
## **14. Running Synthesis**
The exact command depends on the installed OpenLane version.

For a traditional OpenLane setup:
```bash
cd ~/OpenLane
```
Run the design:
```bash
./flow.tcl -design my_design
```
For a tagged run:
```bash
./flow.tcl -design my_design -tag synthesis_run
```
Always use the command recommended by the OpenLane version installed on your system.

---
## **15. Characterizing Synthesis Results**
After synthesis, important parameters should be analyzed.

**15.1 Cell Count**

**Check:**
```text
Total number of cells
Combinational cells
Sequential cells
Buffers
Inverters
```
**15.2 Area**
**Check:**
```text
Cell area
Core area
Die area
```
A smaller area generally improves chip area efficiency.

**15.3 Timing**

Important timing parameters:
```text
Clock period
Setup timing
Hold timing
Worst Slack
Critical path
```
**Slack**
```text
Positive Slack
     ↓
Timing Met
Negative Slack
     ↓
Timing Violation
```
**15.4 Power**
Important power components:
```text
Dynamic Power
Leakage Power
Total Power
```
Power depends on:
```text
Switching activity
Frequency
Cell count
Capacitance
Technology
```
---
## **16. Complete Module 1 Flow**
```text

              MODULE 1
                  │
                  ▼
       Open-Source EDA
                  │
                  ▼
        QFN / Chip / Die
                  │
                  ▼
             RISC-V ISA
                  │
                  ▼
        Software → Hardware
                  │
                  ▼
             SoC Design
                  │
                  ▼
          OpenLane + SKY130
                  │
                  ▼
       Open-Source EDA Tools
                  │
                  ▼
            RTL-to-GDS
                  │
                  ▼
        Design Preparation
                  │
                  ▼
             Synthesis
                  │
                  ▼
       Synthesis Characterization
```
---
**17. Key Takeaways**
```text
ISA defines the interface between software and processor hardware.
RISC-V is an open standard ISA.
SoC integrates multiple hardware components on one chip.
PDK provides technology-specific information for chip design.
SKY130 is an open-source 130 nm PDK.
OpenLane automates the RTL-to-GDS ASIC flow.
Yosys is used for RTL synthesis.
OpenROAD is used for physical design.
The RTL-to-GDS flow converts RTL into a physical chip layout.
Synthesis results are mainly characterized using cell count, area, timing and power.
```
---
# 18. Conclusion

This module provided a clear understanding of the **ASIC chip design process and open-source physical design ecosystem**. It covered QFN packages, chip architecture, RISC-V ISA, SoC design, SKY130 PDK, OpenLane, open-source EDA tools, and the complete **RTL-to-GDS flow**.

The practical study of **design preparation, synthesis, OpenLane directory structure, and synthesis result characterization** provides the foundation for understanding the later stages of physical design such as floorplanning, placement, CTS, routing, and physical verification.

Overall, this module establishes the basic knowledge required to work with **open-source ASIC design tools and technologies**.
