# CHIP DESIGN – PHYSICAL DESIGN (PD)

## Module 2: Floorplanning, Library Binding & Cell Characterization

---

## 1. Overview

Module 2 introduces the fundamentals of **ASIC floorplanning**, **library cells**, **placement**, and **cell characterization**.

### Topics Covered

- Good Floorplan vs Bad Floorplan
- Utilization Factor
- Aspect Ratio
- Pre-placed Cells
- Decoupling Capacitors
- Power Planning
- Pin Placement
- Placement Blockages
- OpenLane Floorplan
- Library Binding
- Placement Optimization
- Standard Cell Libraries
- Cell Characterization
- NLDM, CCS
- Cell Layout Design
- Euler's Path
- Stick Diagram
- Timing Characterization
- Propagation Delay
- Transition Time

---

# 2. Good Floorplan vs Bad Floorplan

## 2.1 What is Floorplanning?

**Floorplanning** is the process of deciding the physical arrangement of the major components of a chip before placement and routing.

It defines:

- Core area
- Die area
- Cell placement area
- Macro locations
- I/O pin locations
- Power distribution
- Placement blockages

---

## 2.2 Good Floorplan

A good floorplan should have:

- Proper utilization
- Suitable aspect ratio
- Short interconnects
- Low congestion
- Proper power distribution
- Good pin placement
- Adequate spacing between macros
- Easy routing

```text
+--------------------------------+
|          I/O PINS              |
|                                |
|   +------------------------+   |
|   |                        |   |
|   |     STANDARD CELLS     |   |
|   |                        |   |
|   |   MACRO       MACRO    |   |
|   |                        |   |
|   +------------------------+   |
|                                |
|          I/O PINS              |
+--------------------------------+
```
**2.3 Bad Floorplan**
A bad floorplan may have:
```text
Very high utilization
Poor aspect ratio
Large routing congestion
Poor pin placement
Long interconnects
Insufficient power distribution
Macros placed too close together
Difficult routing paths
```
```text
+--------------------------------+
| MACRO |MACRO| MACRO |         |
|   \      |      /              |
|    \     |     /               |
|     CONGESTED AREA             |
|        XXXXXXXX                |
|     XXXXXXXX                   |
|                                |
+--------------------------------+
```
**Good vs Bad**
| Good Floorplan          | Bad Floorplan           |
| ----------------------- | ----------------------- |
| Low congestion          | High congestion         |
| Proper utilization      | Excessive utilization   |
| Short connections       | Long connections        |
| Good power distribution | Poor power distribution |
| Easy routing            | Difficult routing       |
| Proper macro placement  | Poor macro placement    |

---
## **3. Chip Floorplanning Considerations**
**3.1 Utilization Factor and Aspect Ratio**
Before creating a floorplan, the core dimensions and die dimensions must be decided.

**Core**
The core is the main area where standard cells and other logic are placed.

**Die**
The die is the complete silicon area.
```text
+----------------------------------+
|              DIE                 |
|   +--------------------------+   |
|   |          CORE            |   |
|   |                          |   |
|   |     Standard Cells       |   |
|   |                          |   |
|   +--------------------------+   |
|                                  |
+----------------------------------+
```
**3.2 Core Height and Width**
The dimensions of the core depend on:
```text
Number of cells
Cell area
Target utilization
Aspect ratio
Basic Relationship
Core Area = Core Width × Core Height
```
**3.3 Utilization Factor**
Utilization tells us how much of the core area is occupied by cells.

**Formula**
```text
Utilization Factor =

Area occupied by cells
---------------------- × 100
       Core Area
```
**Example**

If:

Cell Area = 60
Core Area = 100

Then:

Utilization = (60 / 100) × 100
            = 60%
            
**Important Point**
**Higher utilization:**
Saves area
But can increase congestion

**Lower utilization:**
Provides more routing space
But increases chip area
---
## **4. Aspect Ratio**
Aspect ratio defines the shape of the core.
**Formula**
```text
Aspect Ratio = Core Width / Core Height
```
**For example:**
Width  = 100
Height = 100
Aspect Ratio = 100 / 100
             = 1

This gives a square core.

---
## **5. Pre-Placed Cells**
Some cells or blocks must be placed at specific locations before standard-cell placement.
These are called pre-placed cells/blocks.

**Examples:**
```text
Memories
Analog blocks
IP blocks
Large macros
Interface blocks
```
**Example**
```text
+--------------------------------+
|                                |
|    +---------+                 |
|    |  MACRO  |                 |
|    +---------+                 |
|                                |
|                   +---------+  |
|                   |  MACRO  |  |
|                   +---------+  |
|                                |
+--------------------------------+
```
**Why Pre-Placement?**
Pre-placement helps:
```text
Reduce routing problems
Reserve space for large blocks
Control critical connections
Improve floorplan quality
```
---
## **6. Decoupling Capacitors**
Decoupling capacitors (Decaps) help stabilize the local power supply.
They provide temporary current when a circuit needs a sudden amount of current.

**Basic Concept**
```text
Power Network
     │
     ▼
+------------+
|   Decap    |
+------------+
     │
     ▼
Logic Cells
```
Surrounding Pre-Placed Cells

Decap cells can be placed around macros or sensitive logic to improve local power stability.
```text
+--------------------------+
| D  D  D  D  D  D        |
| D +-------------+ D      |
| D |    MACRO    | D      |
| D +-------------+ D      |
| D  D  D  D  D  D        |
+--------------------------+
```
D = Decoupling Capacitor

---
## **7. Noise Margin Summary**
Noise margin indicates how much unwanted noise a digital signal can tolerate without being interpreted incorrectly.

**Main Types**
```text
High-level noise margin
Low-level noise margin
```
**Formula**
```text
NMH = VOH(min) - VIH(min)

NML = VIL(max) - VOL(max)
```
Higher noise margin generally means better noise immunity.

---
## **8. Power Planning**
Power planning creates a reliable power distribution network for the chip.

**Main Power Signals**
```text
VDD → Power
VSS → Ground
Power Planning Includes
Power rings
Power straps
Power rails
Standard-cell power connections
Decoupling capacitors
```
**Basic Structure**
```text
        VDD
  =================
  ||             ||
  ||   POWER     ||
  ||   STRAPS    ||
  ||             ||
  =================
        VSS
```
**Good power planning helps reduce:**
IR drop
Electromigration
Supply noise

---
**9. Pin Placement**
I/O pins are placed around the boundary of the core/die.

**Good pin placement should:**
Reduce wire length
Reduce congestion
Improve timing
Simplify routing
```
      INPUT PINS
  ↓  ↓  ↓  ↓  ↓

+----------------------+
|                      |
|       CORE           |
|                      |
|                      |
+----------------------+

  ↑  ↑  ↑  ↑
     OUTPUT PINS
```
---
**10. Logical Cell Placement Blockage**
A placement blockage prevents cells from being placed in a particular region.

**Why Use Blockages?**
Reserve space for routing
Reserve space for macros
Reduce congestion
Protect special areas
```text
+----------------------+
| Standard Cells       |
|                      |
|    BLOCKAGE          |
|   XXXXXXXXX          |
|   XXXXXXXXX          |
|                      |
| Standard Cells       |
+----------------------+
```
---
**11. Running Floorplan Using OpenLane**
OpenLane automates the floorplanning process.

**Basic Steps**
Prepare RTL
Create configuration
Select PDK
Set floorplan parameters
Run OpenLane
Check generated floorplan
View layout

Navigate to OpenLane
```bash
cd ~/OpenLane
```
Check the directory:
```bash
ls
```
Run the design:
```bash
./flow.tcl -design my_design
```
The exact command may vary depending on the OpenLane version.

---
## **12. Review Floorplan in Magic**
Magic is an open-source VLSI layout tool.

A typical command is:
```text
magic -T <technology_file> <layout_file>
```
For a generated layout, use the technology and layout files corresponding to your OpenLane/SKY130 setup.

**Example form:**
magic -T sky130A.tech <design>.mag
Magic Can Be Used To
View layout
Inspect layers
Check cell placement
Inspect routing
Perform DRC

---
## **13. Library Binding and Placement**
**13.1 Netlist Binding**
After synthesis, the design contains logic that must be mapped to available physical library cells.

**Example**
```text
RTL
 ↓
Synthesis
 ↓
Gate-Level Netlist
 ↓
Library Binding
 ↓
Physical Standard Cells
```
**Examples of library cells:**
AND
OR
NAND
NOR
INV
Buffer
Flip-Flop

---
## **14. Initial Placement**
Placement determines the physical location of standard cells.
```text
Gate-Level Netlist
        ↓
     Placement
        ↓
Physical Cell Locations
```
Placement Goals
Minimize wire length
Reduce congestion
Meet timing
Optimize power
Maintain routability

---
## **15. Placement Optimization**
Placement is optimized using estimated:

Wire length
Capacitance
Delay
Congestion

**Basic Concept**
```text
Poor Placement
      ↓
High Wire Length
      ↓
High Capacitance
      ↓
Higher Delay
```
Therefore:
```text
Better Placement
      ↓
Shorter Connections
      ↓
Lower Capacitance
      ↓
Better Timing
```
---
**16. Final Placement Optimization**
After initial placement, further optimization is performed.

**Objectives**
Improve timing
Reduce congestion
Reduce wire length
Fix placement issues
Improve routability
```text
Initial Placement
       ↓
Optimization
       ↓
Timing Check
       ↓
Congestion Check
       ↓
Final Placement
```
---
**17. Need for Libraries and Characterization**
Standard-cell libraries provide information required by synthesis and physical design tools.

A library contains information about:

Cell functionality
Cell area
Timing
Power
Physical dimensions
Pin information
Noise characteristics

---
## **18. Library Characterization and Modelling**

Characterization determines how a cell behaves under different conditions.
**For example:**
```
Input Slew
     +
Output Load
     ↓
Cell Delay
     +
Output Slew
     +
Power
```
The results are stored in library models.

**19. NLDM**
NLDM = Non-Linear Delay Model
NLDM represents cell timing using lookup tables.

**The tables generally depend on:**

Input transition/slew
Output load capacitance
**Basic Model**
```
Input Slew
     +
Output Load
     ↓
Lookup Table
     ↓
Cell Delay / Output Slew
```
---
**20. CCS Timing, Power and Noise Characterization**

CCS = Composite Current Source
CCS provides a more detailed model of cell behavior than traditional NLDM.

**It can model:**
Timing
Power
Signal transitions

---
**Simplified Comparison**
| Model       | Main Use                              |
| ----------- | ------------------------------------- |
| NLDM        | Delay and transition lookup tables    |
| CCS         | More detailed timing/current behavior |
| Power Model | Dynamic and leakage power             |
| Noise Model | Noise behavior                        |

---
**21. Art of Layout**
The transistor schematic must be converted into physical layout.

Important concepts:

Euler's Path
Stick Diagram
Diffusion sharing
Minimum area
Minimum parasitics
Design rules

---
## **22. Euler's Path**
Euler's path is used to find an efficient transistor ordering for CMOS layout.

It helps:
Reduce diffusion breaks
Reduce layout area
Simplify connections
Basic Idea
```text
Transistor Network
       ↓
Find Euler Path
       ↓
Optimize Transistor Ordering
       ↓
Efficient Layout
```
---
## **23. Stick Diagram**
A stick diagram is a simplified representation of a layout.

**It shows:**
PMOS
NMOS
Metal
Poly
Diffusion

It does not represent exact dimensions.

---
## **24. Layout Design Step**
The transistor circuit is converted into an actual physical layout.

**Layout Process**
```text
Circuit Schematic
       ↓
Transistor Placement
       ↓
Diffusion / Poly
       ↓
Metal Connections
       ↓
DRC
       ↓
Layout
```
**Important Considerations**
Design rules
Area
Parasitic capacitance
Routing
Power connections
Input/output pins

---
## **25. General Timing Characterization Parameters**
Timing characterization requires standard definitions for signal transitions.

**Important parameters include:**
Rise thresholds
Fall thresholds
Input rise/fall transition
Output rise/fall transition
Propagation delay

---
## **26. Timing Threshold Definitions**
Typical voltage thresholds are defined as percentages of the supply voltage.

**For example:**
```text
LOW threshold  = 20%
HIGH threshold = 80%
```
The exact values depend on the characterization methodology/library.

**26.1 Slew Low Rise Threshold**
slew_low_rise_threshold

Defines the lower voltage threshold used for measuring an output rising transition.

**26.2 Slew High Rise Threshold**
slew_high_rise_threshold

Defines the upper voltage threshold used for measuring an output rising transition.

**26.3 Slew Low Fall Threshold**
slew_low_fall_threshold

Defines the lower voltage threshold used for measuring an output falling transition.

**26.4 Slew High Fall Threshold**
slew_high_fall_threshold

Defines the upper voltage threshold used for measuring an output falling transition.

---
## **27. Input Rise Threshold**
```text
in_rise_threshold
```
Defines the voltage threshold used to determine the timing of an input rising transition.

**Input Rising Signal**
```text
80% ───────────────
                  /
                 /
20% ────────────
```
---
## **28. Input Fall Threshold**
```text
in_fall_threshold
```
Defines the voltage threshold used to determine the timing of an input falling transition.

**Input Falling Signal**
```text
80% ────────────
                \
                 \
20% ─────────────
```
---
**29. Output Rise**
```text
out_rise
```
Defines the rising transition of the output signal.
```text
Voltage
  │
80%│             ______
  │           /
  │         /
20%│_______/
  │
  └────────────────── Time
```
---
**30. Output Fall**
```text
out_fall
```
Defines the falling transition of the output signal.
```text
Voltage
  │
80%│───────
  │       \
  │        \
20%│         \_______
  │
  └────────────────── Time
```
---
## **31. Propagation Delay**
Propagation delay is the time taken for a change at the input to produce the corresponding change at the output.

**Formula**
```text
Propagation Delay = Output Transition Time - Input Transition Time
```
**For example:**
```text
Input crosses threshold  →  5 ns
Output crosses threshold →  7 ns
Delay = 7 - 5
      = 2 ns
```
---
## **32. Transition Time**
Transition time is the time taken by a signal to change between two defined voltage thresholds.

**Rise Transition**
```text
Rise Time = Time at high threshold
            -
            Time at low threshold
```
**Fall Transition**
```text
Fall Time = Time at low threshold
            -
            Time at high threshold
```
**Example**
```text
20% ────────
           /
          /
         /
80% ────

Transition Time
= T80% - T20%
```
---
## **33. Conclusion**

Module 2 provides the foundation for understanding ASIC floorplanning, placement, standard-cell libraries, and timing characterization. The concepts of utilization, aspect ratio, pre-placement, power planning, and congestion are essential for creating a good physical design.

The study of library binding, RePlAce placement, CMOS cell layout, Euler's path, stick diagrams, NLDM, CCS, propagation delay, and transition time provides the knowledge required for the next stages of physical design such as CTS, routing, timing analysis, and signoff.
