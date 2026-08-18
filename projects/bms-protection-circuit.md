# Solar Car Dashboard & Power Protection Circuit
**University of Michigan Solar Car Team — Microsystems Team** · Sep 2023 – Apr 2025

> Redesigned the dashboard PCB and led development of a reverse-polarity / overvoltage protection circuit for the team's 12V electrical system — later adopted as a standard feature on future team PCBs.

**Skills demonstrated:** PCB design (Altium Designer) · protection circuit design (Schottky/Zener) · 555 timer & RC design · hand SMD assembly · bench validation & fault injection · systematic hardware debugging

| | |
|---|---|
| 🔻 **25%** smaller | Dashboard PCB footprint |
| ⚡ **<1W** overhead | Added by the protection circuit |
| 🛡️ Dual protection | Reverse-polarity + overvoltage in one circuit |
| ♻️ Reused | Approach extended to the team's BMS sensing board |

---

## Problem

The dashboard needed to meet an updated World Solar Challenge spec (two fewer MPPT module interfaces), which opened room to shrink the board. Separately, the team kept losing PCBs to a preventable failure: reverse-polarity power connections frying the MCU and forcing a full resolder. I redesigned the dashboard and, with a teammate, designed a protection circuit against both reverse polarity and overvoltage — while staying under the team's strict <1W power budget.

## Design & Implementation

**Dashboard PCB.** Reworked component placement and routing in Altium Designer, moving modules closer together to cut board area while meeting the updated interface requirements.

**Protection circuit.**

```
                     Schottky diode 
12V INPUT ──────────────|>|────────────── PROTECTED RAIL
                                             │
                                           Zener
                                             │
                                            GND
```

- **Reverse polarity:** series Schottky diode (Vishay VS-10BQ015-M3), chosen for its low forward-voltage drop (~0.33V) to minimize dissipation — `P = V_F × I ≈ 0.33W` at 1A, well under budget. Forward-biased under normal polarity; blocks a reversed supply.
- **Overvoltage:** shunt Zener diode from the protected rail to ground — non-conductive normally, clamps the rail if voltage rises into its breakdown region.

**Timing circuit.** Designed a 555-timer astable oscillator for the dashboard's warning indicator, sizing the RC network (3.6kΩ / 46.3kΩ, 10µF, 10nF) to hit the target blink frequency: `f ≈ 1.44 / ((R_A + 2R_B) × C)`.

| Original | Revised |
|---|---|
| ![Original dashboard PCB](./images/dashboard-original-layout.png) | ![Revised dashboard PCB](./images/dashboard-revised-layout.png) |

## Build & Debug

Hand-assembled and soldered the prototypes myself — the tighter component spacing from the smaller footprint made alignment harder than expected. Early bring-up surfaced real failures: an accidental MCU short, a reversed diode, boards that didn't power up correctly. Rather than swapping parts on a guess, I traced the power path stage by stage with a multimeter to isolate each fault:

```
Input → Protection → Regulator → Power Rail → MCU → Peripherals
```

## Validation

Tested the protection circuit standalone, before it ever touched the MCU:

- **Reverse polarity:** intentionally reversed the input and confirmed the protected rail stayed clean
- **Overvoltage:** ramped input voltage on a bench supply and monitored the protected rail for correct clamping
- **Power loss:** measured the actual voltage drop across the Schottky diode to confirm real-world dissipation, not just the datasheet number

## Results

- Dashboard PCB footprint cut to **~75%** of the original size, meeting the updated MPPT interface spec
- Protection circuit added **<1W** dissipation with no observed thermal issue, and held under both reverse-polarity and overvoltage fault tests
- Design adopted as a **standard for future team PCBs**
- Extended the same approach to a follow-on **BMS sensing board** redesign

## Tradeoffs & Lessons

**Smaller board vs. assembly difficulty.** Cutting PCB area saved space but packed components tighter, making hand-soldering and probing harder — a tradeoff I felt directly during assembly.

**Protection vs. power cost.** Since `P = V_F × I`, minimizing the Schottky's forward voltage was the lever that kept the circuit in budget. The real question wasn't just *"does it protect the board?"* — it was *"what does that protection cost us?"*

**A correct schematic isn't a working board.** Footprints, orientation, placement, and soldering quality all determine whether a design actually functions — and debugging systematically (inspect → measure → isolate → verify → fix → retest) beats guessing.

---

[← Back to main portfolio](../README.md)
