# Module 3: Design Library Cell Using Magic Layout and ngspice Characterization

## Overview

This module focuses on designing and analyzing a **CMOS inverter** using **ngspice, Magic, and Sky130 PDK**.

The module covers CMOS inverter simulation, standard-cell layout, CMOS fabrication steps, Sky130 technology files, layout extraction, and DRC checking.

---

## Topics Covered

### SKY130_D3_SK1 — CMOS Inverter ngspice Simulation

- Introduction to CMOS inverter simulation using ngspice
- IO placer revision
- Creating a SPICE deck for CMOS inverter
- Running CMOS inverter SPICE simulations
- Finding the switching threshold voltage (Vm)
- Static and dynamic simulation of CMOS inverter
- Cloning the `vsdstdcelldesign` repository
- Observing inverter input and output waveforms

---

### SKY130_D3_SK2 — Introduction to CMOS Layout

- Basic CMOS fabrication process
- Creating active regions
- Formation of N-well and P-well
- Formation of the gate terminal
- Lightly Doped Drain (LDD) formation
- Source and drain formation
- Local interconnect formation
- Higher-level metal formation
- Introduction to Sky130 basic layout layers
- Understanding LEF using a CMOS inverter
- Creating a standard-cell layout
- Extracting the SPICE netlist from the layout

---

### SKY130_D3_SK3 — Sky130 Technology File Labs

- Creating the final SPICE deck using Sky130 technology
- Characterizing the CMOS inverter using Sky130 model files
- Introduction to Magic layout tool
- Understanding Magic DRC rules
- Introduction to Sky130 PDK and technology files
- Loading Sky130 technology rules in Magic
- Fixing `poly.9` DRC error
- Implementing poly resistor spacing with diffusion and tap
- Understanding DRC errors as geometrical structures
- Finding and fixing incorrect or missing DRC rules

---

## Practical Work

The practical work in this module includes:

- CMOS inverter SPICE deck creation
- ngspice simulation
- Analysis of inverter waveforms
- Switching threshold analysis
- CMOS layout creation using Magic
- Understanding CMOS fabrication layers
- Standard-cell layout and extraction
- Sky130 model file usage
- Magic DRC checking and error fixing

---

## Key Takeaways

- **ngspice** is used to simulate and characterize CMOS circuits.
- A CMOS inverter can be analyzed using static and dynamic simulations.
- **Magic** is used to create, inspect, and verify IC layouts.
- **Sky130 PDK** provides the required technology and model files.
- CMOS fabrication is represented through different physical layers.
- Layout extraction converts the physical layout into a **SPICE netlist**.
- DRC checks help identify and correct layout design-rule violations.
- The complete flow connects **circuit simulation → layout → extraction → verification**.

---

## Tools Used

- ngspice
- Magic
- Sky130 PDK
- SPICE
- Git / GitHub

---

## Module-3 Outcome

After completing this module, I gained practical knowledge of CMOS inverter simulation, Sky130 layout design, Magic DRC, SPICE characterization, and standard-cell layout extraction.
