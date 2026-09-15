# Module 5: Final Steps for RTL2GDS Using TritonRoute and OpenSTA

## Overview

This module focuses on the final stages of the **RTL to GDSII flow**, mainly routing, power distribution, design rule checking, and post-route analysis.

It covers maze routing, DRC, power distribution network (PDN), global and detailed routing, and the important features of TritonRoute.

---

## Topics Covered

### SKY130_D5_SK1 — Routing and Design Rule Check (DRC)

- Introduction to Maze Routing
- Understanding Lee's Algorithm
- Finding suitable routing paths
- Understanding the purpose of Design Rule Check (DRC)
- Identifying DRC violations
- Checking whether the layout follows design rules

---

### SKY130_D5_SK2 — Power Distribution Network and Routing

- Introduction to Power Distribution Network (PDN)
- Building the power distribution network
- Connecting power straps to standard-cell power
- Understanding global routing
- Understanding detailed routing
- Configuring TritonRoute for routing
- Understanding the connection between power planning and routing

---

### SKY130_D5_SK3 — TritonRoute Features

- Understanding TritonRoute and its routing process
- Using pre-processed route guides
- Handling connectivity between route guides
- Understanding intra-layer routing
- Understanding inter-layer routing
- Routing topology and connectivity handling
- Understanding routing optimization
- Checking final post-route files and results

---

## Practical Work

The practical work in this module includes:

- Studying Maze Routing and Lee's Algorithm
- Performing Design Rule Checks
- Creating and understanding the Power Distribution Network
- Connecting power straps to standard cells
- Running global and detailed routing
- Configuring and using TritonRoute
- Studying route guides and connectivity
- Understanding intra-layer and inter-layer routing
- Observing final post-route results

---

## Key Takeaways

- Maze routing helps in understanding how routing paths are created.
- **DRC** is used to check whether the layout follows manufacturing design rules.
- The **Power Distribution Network (PDN)** supplies power to different parts of the chip.
- TritonRoute is used for global and detailed routing.
- Routing can use different metal layers for connecting different cells.
- Intra-layer and inter-layer connectivity are important during routing.
- Route guides help TritonRoute follow the required routing paths.
- Post-route files provide the final routing and physical design information.
- This module completes the major routing steps in the **RTL2GDS flow**.

---

## Tools Used

- TritonRoute
- OpenSTA
- OpenLane
- Sky130 PDK
- Magic
- LEF / DEF Files

---

## Module Outcome

After completing this module, I gained practical knowledge of routing, DRC, power distribution networks, TritonRoute, route guides, and post-route analysis, which are important final steps in the RTL to GDSII physical design flow.
