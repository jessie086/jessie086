# 16×16 Multiplier — Hierarchical APR
*EECS 427: VLSI Design · Jan 2026 – Apr 2026 · Team of 6*

*RTL implementation by a teammate; I owned floorplanning, hierarchical APR, and DRC/LVS verification. I'm also familiar with the RTL and happy to walk through it in interviews.*

## Objective

Built a 16×16 multiplier combining a systolic array architecture with Wallace-tree compression, implemented through a hierarchical automated place-and-route (APR) flow. Unlike the ALU on this project's companion RISC-V datapath — which I hand-floorplanned — this block was large and regular enough to benefit from automation, so my focus here was structuring an APR flow that could handle scale: treating sub-modules as black boxes to keep synthesis and routing tractable, then verifying the final layout was physically correct.

## Skills Learned

- Systolic array architecture and Wallace-tree compression for multiplication
- Hierarchical APR — decomposing a large design into black-boxed sub-modules to make automated place-and-route tractable at scale
- DRC/LVS verification for physical design correctness
- Layout area minimization within an automated flow (vs. the manual optimization used on the ALU)
- Judgment on when to hand-layout vs. when to lean on automation — a large regular structure like this benefits more from a well-structured APR flow than from hand placement

## Tools Used

- **Cadence Innovus** — hierarchical synthesis and automated place & route
- **Cadence Virtuoso** — DRC/LVS verification
- TSMC standard-cell library

## Design Process

*[NDA note for yourself — remove before publishing: use hand sketches of the systolic array structure/hierarchy instead of tool screenshots, same approach as the RISC-V page.]*

**1. Architecture Selection**
*[Sketch: systolic array + Wallace-tree structure diagram]*
Chose a systolic array for the multiplier core, paired with Wallace-tree compression to reduce the partial-product summation stage — a standard high-throughput multiplier architecture, but one that produces a large, regular, repeating structure well-suited to hierarchical automation rather than manual layout.

**2. Hierarchical Decomposition**
*[Sketch: block diagram showing how sub-modules were black-boxed]*
Given the size of the design, I structured the APR flow hierarchically — treating repeated sub-modules as black boxes during floorplanning and routing rather than flattening the entire design at once. This kept synthesis and routing runtimes manageable and made the flow easier to debug when issues arose in one sub-block without needing to re-run the entire design.

**3. Place & Route**
Drove synthesis and automated place-and-route across the hierarchical structure in Cadence Innovus, with area minimization as the primary optimization target given the multiplier's size relative to the rest of the datapath.

**4. Verification**
Verified physical correctness across the full hierarchical layout with DRC (design rule checking) and LVS (layout-vs-schematic) to confirm the automated flow produced a fabricatable, functionally correct result.

## Results

- Functional 16×16 multiplier verified via systolic array + Wallace-tree architecture
- Clean DRC/LVS across the full hierarchical design
- Minimized layout area within the constraints of an automated hierarchical flow

## Reflection

*[Optional — draft below, edit freely]*

*Draft:* This project was a useful contrast to the RISC-V ALU I hand-laid out on the same team: the multiplier's size and regularity made automation the right call, whereas the ALU's smaller, timing-critical structure rewarded manual attention. Working both taught me that "hand layout vs. APR" isn't really a philosophical preference — it's a decision that depends on the structure you're laying out, and I want to keep building judgment on when each approach actually pays off.

---
[← Back to main portfolio](../README.md) · [See the RISC-V datapath project →](/projects/riscv-layout-apr.md)
