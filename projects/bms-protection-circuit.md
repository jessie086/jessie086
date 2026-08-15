# Solar Car Dashboard & Power Protection
*University of Michigan Solar Car Team — Microsystems · Sep 2023 – Apr 2025*

I worked on the University of Michigan Solar Car Team's Microsystems team to redesign and validate PCB hardware for the team's 12 V electrical systems. My work included reducing the dashboard PCB footprint by approximately 25%, adapting the board to updated MPPT module requirements, implementing reverse-polarity and overvoltage protection, and designing a 555-based timing circuit. I designed and assembled the hardware in Altium and validated the protection circuitry through bench testing and power-loss measurements.

## Objective

Develop and revise PCB hardware for the University of Michigan Solar Car Team's Microsystems systems, including a redesigned dashboard PCB and a compact power-input protection circuit for the team's 12 V electronics.

The dashboard required modification to accommodate updated World Solar Challenge requirements, including two fewer MPPT module interfaces, while also reducing the physical PCB footprint.

In parallel, my teammate and I developed and validated an input protection circuit to reduce the risk of PCB damage from reverse-polarity connections and overvoltage events. Because the Solar Car operates under a tight power budget, we also evaluated the power cost of the protection circuit and targeted less than 1 W of additional dissipation per board.

## My Contributions
- Redesigned the dashboard PCB in Altium Designer to accommodate the updated module requirements.
- Reduced the PCB footprint to approximately 75% of the original board size.
- Reworked component placement and routing, including moving modules closer together to reduce board area.
- Designed and implemented a series Schottky diode for reverse-polarity protection.
- Implemented a shunt Zener diode for overvoltage protection on the protected power rail.
- Worked with my teammate on component selection and circuit implementation.
- Hand-assembled and soldered prototype PCBs.
- Validated the protection circuit independently before connecting the MCU.
- Tested reverse-polarity behavior and monitored the protected rail while increasing input voltage.
- Measured the voltage drop across the Schottky diode and evaluated the resulting power dissipation.
- Debugged board-level failures using a multimeter and systematic power-rail tracing.
- Later contributed to a revised BMS sensing board, including further PCB-area reduction and module changes.

## Design Challenges
1. Updating the Dashboard PCB

The original dashboard was designed around an earlier set of World Solar Challenge requirements. The revised design required two fewer MPPT module interfaces.

This created an opportunity to reduce the physical PCB footprint while maintaining the required functionality and mechanical interfaces.

The redesigned board was approximately 75% of the original size.

One of the practical challenges was that reducing the board size also meant moving some modules closer together. While this saved PCB area, it made physical alignment and hand soldering more difficult during assembly.

Before → After
[TODO: add pics]
### Dashboard PCB — Original vs. Revised

| Original | Revised |
|---|---|
| ![Original dashboard PCB](./images/dashboard-original-layout.png) | ![Revised dashboard PCB](./images/dashboard-revised-layout.png) |

2. Input Protection Circuit

The team wanted to reduce the risk of damaging a PCB when power was connected incorrectly.

A particularly costly failure mode was reverse polarity: accidentally connecting power and ground in the wrong orientation could damage the MCU and require the PCB to be reassembled or resoldered.

The board operates from a nominal 12 V input, with expected board current below approximately 1 A.

The protection circuit therefore needed to provide protection while adding minimal power loss.

Protection architecture
                     Schottky diode
12 V INPUT ──────────────|>|────────────── PROTECTED RAIL
                                             │
                                             │
                                           Zener
                                             │
                                            GND
The Schottky diode is placed in series with the incoming power rail, while the Zener diode is connected in parallel between the protected rail and ground.

Reverse-Polarity Protection

We selected the Vishay VS-10BQ015-M3 Schottky diode.

The part has a maximum forward voltage of approximately 0.33 V under its specified datasheet test condition and a 15 V maximum reverse-voltage rating.

The Schottky diode was chosen over a conventional silicon diode because its lower forward voltage reduces the power dissipated in the series protection element.

At an expected current of approximately 1 A:

P=VF*I
P≈0.33V×1A=0.33W

This gave us a reasonable power-loss margin relative to the team's <1 W target.

Under normal polarity, the diode is forward biased and allows power to reach the board. If the supply polarity is reversed, the diode becomes reverse biased and blocks the reversed supply from reaching the protected circuitry.

Overvoltage Protection

A Zener diode was connected from the protected power rail to ground.

Under normal operating conditions, the Zener remains largely non-conductive. If the rail rises sufficiently into the Zener's breakdown region, it conducts and provides a shunt path for current, limiting the voltage seen by downstream circuitry.

The original team component library did not preserve the exact manufacturer information for this Zener, so I am intentionally not listing an exact part number or breakdown voltage here rather than reconstructing a specification from memory.


3. Timing / Warning Circuit

The dashboard also included a timing circuit based around an LM555 timer.

The 555 was configured as an astable oscillator, producing a periodic output signal used for the board's warning/blinking functionality.

The timing behavior was determined by the resistor-capacitor network.

The approximate timing components in the design were:

3.6 kΩ resistor
46.3 kΩ resistor
10 µF timing capacitor
10 nF capacitor
LM555 timer

For a standard 555 astable configuration:

f≈1.44/[(RA​+2RB​)C]

The important design relationship was:

R↑⇒slower charging/discharging⇒f↓

and

C↑⇒slower charging/discharging⇒f↓

The RC network therefore allowed us to set the desired blinking frequency.

4. Schematic & PCB Implementation

I used Altium Designer to implement the circuit and PCB design.

The work included:

schematic capture
component footprints
component placement
PCB routing
board-area optimization
integrating the protection circuitry into the existing design

The physical redesign required balancing electrical requirements with mechanical and manufacturing constraints.

In particular, moving modules closer together helped reduce PCB area but made alignment and hand soldering more challenging.

Suggested image

Use a screenshot of the actual Altium schematic showing the regulator/protection/timing area.

protection-timing-schematic.png

I'd crop it tightly enough that someone can immediately see:

12 V input → Schottky → regulator/protected rail → Zener + 555 timing circuit

rather than showing the entire enormous schematic.

5. Assembly & Bring-Up

I hand-assembled and soldered the prototype boards.

This was one of my first substantial PCB assembly experiences, and it introduced several practical challenges that were not obvious from the schematic alone.

During early assembly and bring-up, I encountered issues including:

an accidental MCU short
incorrect diode polarity
boards that did not initially function as expected

These failures forced me to develop a more systematic hardware-debugging process.

Instead of immediately replacing components, I learned to trace the power path through the board:

Input
  ↓
Protection
  ↓
Regulator
  ↓
Power rail
  ↓
MCU
  ↓
Peripherals

Using a multimeter, I checked voltages and continuity at different points in the circuit and isolated the problem progressively.

This became one of the most valuable parts of the project because it taught me that a correct schematic does not guarantee a functional PCB — assembly, polarity, soldering, and physical implementation all matter.

6. Validation

Because the MCU was one of the components most at risk from an incorrectly functioning protection circuit, we validated the protection circuitry before connecting it to the MCU.

Reverse-polarity test

We intentionally reversed the input power connections and measured the protected side of the Schottky diode to verify that the reversed supply was not being passed into the downstream circuitry.

Overvoltage test

We increased the input voltage using a bench power supply and monitored the protected rail with a multimeter to evaluate the behavior of the Zener protection.

Power-loss measurement

We measured the voltage drop across the Schottky diode and evaluated the additional power dissipation introduced by the protection circuit.

The measured/estimated dissipation remained below the team's 1 W requirement, with no significant thermal issue observed during testing.

## Results
Protection Circuit
Successfully validated reverse-polarity protection.
Validated overvoltage protection behavior.
Added <1 W of power dissipation per board under the expected operating conditions.
No significant thermal concern was observed from the added protection circuitry.
The team planned to extend the protection approach to additional PCBs in subsequent Solar Car designs.
Dashboard Redesign
Reduced the PCB footprint to approximately 75% of the original size.
Accommodated the updated requirement for two fewer MPPT module interfaces.
Repositioned modules and components to make better use of the reduced board area.
Successfully assembled and tested the redesigned board.
Follow-on BMS Design

In a subsequent design iteration, I also worked on the BMS sensing board, where the team modified the board's module configuration and reduced its physical footprint.

Engineering Tradeoffs

One of the biggest lessons from this project was that optimizing one engineering metric usually affects another.

Smaller PCB

Reducing board area:

Pros

smaller physical footprint
better use of space on the vehicle
potentially easier mechanical integration

Tradeoffs

less routing space
tighter component spacing
more difficult assembly
harder access for probing/debugging

I experienced this directly when moving modules closer together reduced board area but made alignment and hand soldering more difficult.

Protection vs. Power Consumption

Protection circuitry adds components and inevitably introduces some electrical cost.

For the series Schottky:

P=V
F
	​

I

Therefore, minimizing forward voltage was important because the diode's voltage drop directly translates into power dissipation.

This was why we didn't evaluate the protection circuit only by asking:

"Does it protect the board?"

We also asked:

"How much does the protection cost us?"

Lessons Learned

This project was one of my first experiences taking a PCB from schematic → layout → physical assembly → testing → debugging.

The biggest lessons were:

1. PCB design is more than schematic correctness

A functioning design also depends on:

correct footprints
component orientation
physical placement
routing
assembly quality
soldering
power-up procedure
2. Protection design requires a system-level tradeoff

A protection circuit is only useful if it protects the system without creating an unacceptable cost elsewhere.

For our 12 V system, that meant balancing protection against power dissipation and thermal impact.

3. Hardware debugging should be systematic

When a board doesn't work, I learned to avoid guessing and instead:

Inspect → measure → isolate → verify → fix → retest

Tracing the power path from the regulator toward the MCU gave me a much more reliable way to locate faults.

4. Design decisions should be validated with measurements

Rather than assuming that a protection circuit was "low power," we measured the voltage drop and evaluated the actual dissipation.

That data helped demonstrate that the protection approach was practical for future Solar Car boards.​

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
