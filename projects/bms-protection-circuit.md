# Solar Car BMS Protection Circuit
*University of Michigan Solar Car Team — Microsystems · Sep 2023 – Apr 2025*

## Objective

Designed and validated a compact power-input protection circuit to prevent PCB failures from reverse-polarity wiring and overcurrent/overvoltage events across the solar car's 12V systems. The circuit was adopted as a standard feature on all future team PCBs after validation showed it added less than 1W of dissipation per board — protection with negligible performance cost.

## Skills Learned

- Power electronics component selection under real tradeoffs (protection vs. efficiency vs. cost)
- Reverse-polarity protection using series Schottky diodes — reasoning through forward-voltage drop as a design constraint
- Overcurrent/overvoltage protection using a TVS diode + resettable fuse combination, and why that pairing beats either component alone
- Schematic capture and PCB design in Altium Designer
- Hand assembly and bench-testing fault-tolerant circuits (reverse-supply injection, surge/current injection)
- Quantifying real-world tradeoffs — measuring power dissipation and thermal rise to justify a design decision with data, not just simulation

## Tools Used

- **Altium Designer** — schematic capture and PCB layout
- Bench power supply, oscilloscope — fault injection and validation testing
- Hand soldering / SMD assembly

## Design Process

**1. Identifying the Failure Modes**
Before designing anything, I identified the two real failure scenarios worth protecting against: reverse-polarity connection (accidentally swapping power/ground) frying the MCU, and overcurrent/overvoltage events from wiring mistakes or faulty adapters — both of which meant reassembling and resoldering a PCB from scratch if unprotected.

**2. Component Selection & Justification**
*[Diagram: simple circuit schematic — Schottky diode in series, TVS diode + fuse to GND]*
For reverse-polarity protection, I chose a **series Schottky diode** over a standard diode specifically for its lower forward-voltage drop — meaning less power loss as current flows through it at the system's 12V operating point. For overcurrent/overvoltage, I used a **TVS diode + resettable fuse** in combination rather than either alone: the fuse limits sustained overcurrent by heating and increasing resistance, while the TVS diode clamps short voltage spikes — pairing them means the TVS diode isn't left conducting continuously during a sustained overvoltage fault (which would cause it to overheat on its own).
- Selected a TVS diode with standoff voltage (V_RM) above the 12V system maximum (chose 13V) and a clamping voltage of 20V at surge current

**3. Schematic & PCB Design**
Built the schematic and PCB layout in Altium Designer, then fabricated and hand-assembled prototype boards.

**4. Validation Testing**
Bench-tested the circuit against both failure modes directly: applying reverse supply voltage and injecting surge/overcurrent conditions to confirm the protection held under fault.

**5. Measuring Real-World Cost**
Measured power dissipation before and after adding the protection circuit to quantify its actual cost to the system — this mattered because "protection" that silently costs significant efficiency wouldn't be worth adopting team-wide.

## Results

- **<1W added dissipation per board**, with acceptable thermal rise
- Adopted as a **standard feature on all future team PCBs**
- Reduced BMS sensing board footprint by **25%** in a follow-on design iteration
- Validated fault tolerance against both reverse-polarity and overcurrent/overvoltage conditions via direct bench testing

## Reflection

*[Optional — draft below, edit freely]*

*Draft:* This project taught me that good protection design isn't just "does it work" — it's proving the cost is low enough that a team will actually standardize on it. Measuring the dissipation and thermal impact, not just confirming the circuit survived fault testing, is what made the case for adoption across every future board.

---
[← Back to main portfolio](../README.md)
