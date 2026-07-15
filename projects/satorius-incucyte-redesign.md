# Sartorius Incucyte Redesign — Airflow & Controls
*Student Engineering Project, Sponsored by Sartorius · Jan 2026 – Present · Team of 6*

*[Confidentiality check: verify with your team lead/sponsor contact before publishing — sponsored projects sometimes carry informal expectations around what's shareable, even without a signed NDA. Remove this note once confirmed.]*

## Objective

Contributing to a redesign of Sartorius's Incucyte live-cell imaging system, which currently requires a separate, bulky external incubator. The goal: combine both functions — live-cell imaging and incubation — into a single, more compact unit, while holding the cell-culture environment at 37°C ± 0.5°C and 95% relative humidity. My focus within the six-person team is the **airflow subsystem**: understanding how air circulation affects both temperature/humidity uniformity and the risk of drying out cell cultures.

## Skills Learned

- Requirements-driven subsystem design under tight environmental tolerances (±0.5°C, 95% RH)
- Trade-off analysis between competing physical effects — airflow improves thermal/humidity uniformity but risks evaporating cell culture media
- Sensor grid design for spatial environmental characterization (temperature + humidity across an 8-point grid at the cell-tray plane)
- Controls architecture planning: I2C sensor acquisition → Arduino/Raspberry Pi control logic → PWM-driven MOSFET actuation
- Rapid prototyping methodology — building a simplified physical testbed ("Dumb Box") to isolate and test subsystem behavior before committing to full-system design
- Cross-subsystem collaboration (airflow decisions directly affect the heating, cooling, and humidity subsystems' requirements)

## Tools Used

- Arduino / Raspberry Pi
- PWM-driven MOSFET actuation
- I2C sensor communication
- Temperature/humidity sensor grid
- Custom PCB (in progress) for sensor-to-controller communication

## Design Process

**1. Requirement Definition & Brainstorming**
The team's six initial redesign concepts converged on one central open question: should the design use active airflow at all? Camera and stepper-motor components inside the unit generate heat and create local hot spots that hurt imaging quality — airflow could even that out, but risked drying the cell culture. This uncertainty is what drove the team to split into subsystem-focused testing groups (heating, cooling, humidity, and airflow) rather than committing to one architecture upfront.

**2. Prototype Testbed — "Dumb Box"**
*[Diagram: simple sketch of the Dumb Box setup with sensor grid locations]*
Built a simplified insulated prototype (cardboard + aluminum sheeting) at a fraction of a real incubator's size, specifically to test subsystem behavior in isolation before committing to a full design. A smaller volume also directly tests the airflow hypothesis: less air mass means faster heat-up/humidify time, but only if circulation is handled well.

**3. Airflow Subsystem Testing**
My focus: running tests on fan configurations and their effect on evaporation around the cell tray, and how different setups circulate heated, humid air without excessive drying.

**4. Sensor Grid Design**
Instrumented the Dumb Box with combined temperature/humidity sensors at eight corner points plus the cell-tray height, to characterize spatial uniformity — this data is what will ultimately drive PID tuning for each subsystem.

**5. Controls Architecture Planning**
Defined the intended controls path: sensors report over I2C to an Arduino/Raspberry Pi, which will run PID control logic and output PWM signals to MOSFETs actuating the physical components (fans, heaters, humidifiers). A custom PCB is planned to carry sensor data back to the controller board.

## Results

- Defined and tested the core open question (airflow vs. no airflow) via isolated subsystem testing rather than committing early
- Built a functional low-cost prototype testbed (Dumb Box) enabling rapid iteration ahead of full-system design
- Instrumented an 8-point + cell-tray-height sensor grid for spatial environmental characterization
- Established the controls architecture (I2C → Arduino/RPi → PWM → MOSFET) as the path toward PID-based environmental control

*[Update this section as the project progresses — e.g. once the testing PCB and PID controllers are built]*

## Reflection

*[Optional — draft below, edit freely]*

*Draft:* This project pushed me to think about controls and environmental engineering together rather than separately — the airflow subsystem doesn't just need to hit a temperature target, it has to do so without breaking a completely different requirement (cell culture viability) that another subsystem depends on. It's given me a much better appreciation for how much upfront testing on a simplified prototype can save downstream, rather than debugging a fully integrated system.

---
[← Back to main portfolio](../README.md)
