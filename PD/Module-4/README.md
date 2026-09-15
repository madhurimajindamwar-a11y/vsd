# Module 4: Pre-Layout Timing Analysis and Importance of a Good Clock Tree

## Overview

This module focuses on understanding **timing analysis, delay modelling, clock-tree synthesis, and signal integrity** in the physical design flow.

The module covers timing libraries, delay tables, OpenSTA timing analysis, TritonCTS, clock-tree design, and setup/hold analysis with real clocks.

---

## Topics Covered

### SKY130_D4_SK1 — Timing Modelling Using Delay Tables

- Converting grid information into track information
- Converting Magic layout into standard-cell LEF
- Understanding timing libraries
- Adding a new cell to the synthesis flow
- Introduction to delay tables
- Understanding delay table usage
- Configuring synthesis settings to improve slack
- Including `vsdinv` in the synthesis flow

---

### SKY130_D4_SK2 — Timing Analysis with Ideal Clocks Using OpenSTA

- Introduction to timing analysis
- Understanding flip-flop setup time
- Introduction to clock jitter and uncertainty
- Configuring OpenSTA for post-synthesis timing analysis
- Checking setup timing violations
- Optimizing synthesis to improve timing
- Performing basic timing analysis using OpenSTA

---

### SKY130_D4_SK3 — Clock Tree Synthesis Using TritonCTS

- Introduction to Clock Tree Synthesis (CTS)
- Understanding clock routing and buffering
- H-Tree based clock distribution
- Introduction to clock skew
- Understanding crosstalk
- Clock-net shielding
- Running CTS using TritonCTS
- Checking the generated clock tree
- Understanding the effect of CTS on timing

---

### SKY130_D4_SK4 — Timing Analysis with Real Clocks Using OpenSTA

- Timing analysis using real clocks
- Setup timing analysis
- Hold timing analysis
- Running OpenSTA after CTS
- Using correct timing libraries
- Assigning CTS results for timing analysis
- Observing the effect of CTS buffer size
- Comparing setup and hold timing

---

## Practical Work

The practical work in this module includes:

- Working with timing libraries and delay tables
- Converting layout information into LEF
- Performing post-synthesis timing analysis
- Using OpenSTA for setup and hold analysis
- Running Clock Tree Synthesis using TritonCTS
- Studying clock buffering and routing
- Understanding clock skew and signal integrity
- Performing timing analysis with real clocks

---

## Key Takeaways

- Timing libraries provide important delay and timing information for design analysis.
- Delay tables help understand the timing behavior of standard cells.
- **OpenSTA** is used for setup and hold timing analysis.
- Clock jitter and uncertainty can affect the overall timing of a design.
- **TritonCTS** is used to build and buffer the clock tree.
- A properly designed clock tree helps reduce clock-related timing problems.
- Crosstalk and clock shielding are important for maintaining signal integrity.
- Real-clock analysis helps understand actual setup and hold behavior after CTS.
- The size of CTS buffers can affect both setup and hold timing.

---

## Tools Used

- OpenSTA
- TritonCTS
- Magic
- Sky130 PDK
- Standard Cell Libraries
- LEF / Timing Libraries

---

## Module Outcome

After completing this module, I gained practical knowledge of timing modelling, OpenSTA analysis, clock-tree synthesis, signal integrity, and setup/hold timing analysis with real clocks.
