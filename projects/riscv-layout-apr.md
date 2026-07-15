# RISC-V Processor — Layout & APR Design
*EECS 427: VLSI Design · Jan 2026 – Present · Team of 6*

## Objective

Designed and physically implemented a two-stage pipelined RISC-V processor — register file, ALU, barrel/log shifter, program counter, and control unit — from schematic through fabricatable layout, using the TSMC standard-cell library in Cadence. My focus areas were **manual layout of the ALU** (built around a Kogge-Stone adder for speed) and **automated place-and-route** for the PC and control unit, closing timing with zero setup/hold violations at 90% core density.

This was my first deep dive into the area/timing/power tradeoff that hand layout lives in, and it's what convinced me I want to do this professionally.

## Skills Learned

- Manual layout and floorplanning via stick diagrams, translating a schematic into a legal, dense physical layout
- Transistor sizing and speedpath optimization using HSPICE simulation
- High-speed adder architecture (Kogge-Stone) — logarithmic carry propagation and its wiring-complexity tradeoff
- Automated Place & Route (Cadence Innovus), including .tcl scripting for synthesis and APR flow control
- DRC/LVS verification and physical design closure
- I/O pin placement strategy for routability

## Tools Used

- **Cadence Virtuoso** — schematic capture and manual layout
- **Cadence Innovus** — synthesis, automated place & route
- **HSPICE** — analog simulation for speedpath/transistor sizing analysis
- **TSMC standard-cell library** — cell-level building blocks
- Tcl scripting for APR flow automation

## Design Process

**1. Architecture & Datapath Planning**
*[Sketch: block diagram of the two-stage pipeline — register file, ALU, shifter, PC, control unit]*
Before any layout work, I mapped out the datapath at a block level to understand how data would flow through the two pipeline stages and where the critical timing path was likely to live.

**2. Manual Layout — Kogge-Stone ALU**
*[Sketch: stick diagram / hand floorplan of the ALU]*
I hand-floorplanned the ALU around a Kogge-Stone adder, chosen for its logarithmic (rather than linear) carry-propagation delay. The tradeoff: Kogge-Stone is fast but wiring-dense — getting the layout to actually realize that speed advantage without the routing overhead eating the gains was the core challenge here. I used stick diagrams to plan transistor placement and wire routing before committing to layout.

**3. Transistor Sizing & Speedpath Analysis**
*[Sketch or chart: speedpath annotated on your ALU diagram, or a sizing-vs-delay chart if you have the data]*
Using HSPICE, I ran speedpath analysis across the ALU to identify the critical path and iteratively resized transistors to balance power, performance, and area (PPA) — this is the part of layout I found most engaging: every sizing decision trades against another.

**4. Automated APR — PC & Control Unit**
*[Sketch: floorplan sketch of PC/controller block, or a diagram of your I/O pin placement strategy]*
For the program counter and controller, I drove synthesis and place-and-route in Cadence Innovus, writing and editing .tcl scripts to control the flow. I planned I/O pin placement to minimize wire crossings, achieved 90% core density, and closed timing with **zero setup/hold violations** post-route.

*This project also includes a 16×16 multiplier built with a hierarchical APR flow — [see it as its own project →](/projects/multiplier-hierarchical-apr.md)*

## Results

- **90% core density** achieved in automated APR (PC/controller)
- **Zero setup/hold timing violations** post-route
- Clean DRC/LVS across all hand-laid and auto-routed blocks
- Functional Kogge-Stone ALU, verified in simulation

## Reflection
This project sharpened my preference for manual layout specifically — the Kogge-Stone ALU took the most iteration, and I found the process of tracing a speedpath back to a transistor sizing decision more satisfying than the automated APR steps, even though both were necessary. Going forward, I want to get faster at manual floorplanning for larger blocks while keeping the same level of area discipline.

---
[← Back to main portfolio](../README.md)
