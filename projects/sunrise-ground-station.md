# NASA SunRISE Ground Radio Lab — Electrical Team Lead

*HelioPhysics Ground Lab, University of Michigan · Jan 2025 – Present*

I lead a 10-person electrical subteam (grown from 5) building a ground-based radio telescope network that supports NASA's SunRISE CubeSat mission. My work spans RF hardware bring-up, field deployment, GNSS-based interferometry, and technical leadership across a 30-person interdisciplinary lab.

**Skills demonstrated:** RF signal chain debugging · antenna hardware · connector/cable fabrication · GNSS positioning & timing · systems-level troubleshooting · technical documentation · cross-functional leadership · root-cause analysis

---

## Impact at a Glance

| | |
|---|---|
| **Team growth** | Grew electrical subteam from 5 → 10 engineers |
| **Hardware deployed** | 2 crossed-dipole RF antenna systems, fully assembled and field-verified at Peach Mountain Observatory |
| **Reach** | 26 partner high schools running antenna kits I helped document |
| **Diagnostic win** | Root-caused an intermittent RF failure to a connector-fabrication process; fixed it system-wide with a standardized tool and procedure |
| **R&D in progress** | Leading GNSS positioning work and evaluating a new SDR-based receiver architecture for polarization/interferometry measurements |

---

## The Project

NASA's SunRISE mission uses a CubeSat constellation to study solar radio bursts from space. Our lab builds the ground-based complement: a network of antennas observing the same phenomena from 5–85 MHz, a band the ionosphere makes hard to see from space but that ground stations can access directly.

The long-term goal isn't just detecting bursts — it's localizing where they originate, tying them to solar energetic events, and eventually combining geographically distributed antennas into an interferometric array.

**Signal chain:** Solar emission → crossed-dipole antenna → LWA front-end/active balun → coax → RF filtering/up-conversion → CALLISTO receiver → acquisition software → dynamic spectrum

---

## What I Own

- **Team leadership** — run weekly execution meetings, track workstreams across 10 engineers, review purchase requests and trade studies, coordinate with mechanical/software/science teams
- **Field deployment** — antenna assembly, RF cable routing/termination, front-end and receiver integration, on-site bring-up and debugging at Peach Mountain Observatory
- **Interferometry R&D** — researching antenna geometry, timing synchronization, and calibration requirements to combine multiple ground stations into a single measurement system
- **GNSS positioning** — leading the effort to precisely geolocate each antenna (required to compute interferometric baselines) using a Trimble Zephyr Base-3 / NetRS setup
- **Documentation & outreach** — writing installation procedures that non-RF-background high school students can follow correctly, supporting 26 partner schools

---

## Case Study: Root-Causing an Intermittent RF Failure

**Problem:** A fully assembled antenna system — antenna, cables, front end, receiver, software — produced no usable signal. All components had been independently validated by their manufacturers, so the fault had to be in our own assembly.

**Approach:**
1. Ruled out software and known-good hardware through component swaps and repeated bring-up across multiple field tests
2. Narrowed scope to the one thing we built ourselves: cable/connector terminations
3. Used a multimeter in continuity mode to test the cable-to-adapter interface — expected near-zero resistance, measured an open circuit
4. Formed and tested a hypothesis: re-exposed more conductor and reassembled the connector — continuity restored

**Root cause:** We weren't using the standardized cable-preparation tool for that connector type, so exposed-conductor length varied by whoever assembled it — an inconsistent, unrepeatable process.

**Fix:** Rather than patch the one antenna, I submitted a purchase request for the proper prep tool and updated our build procedure, turning a one-off repair into a repeatable process that reduces field-debugging time on every future build.

This is the debugging pattern I default to: **observe → isolate → measure → test hypothesis → find root cause → fix the process, not just the instance.**

---

## Current R&D

**GNSS Positioning for Interferometry**
Interferometry requires knowing exact antenna positions to compute baseline geometry between sites. I'm leading this effort with a Trimble Zephyr Base-3 antenna and NetRS receiver. When the receiver's config interface conflicted with the university network, I set up a dedicated hotspot and worked through configuration via terminal tools — now collecting raw `.T00` GNSS data at 15-second intervals, with position/timing post-processing in progress.

**Next-Gen Receiver Architecture**
Our current receiver outputs intensity-only dynamic spectra. I'm evaluating an SDRplay RSPdx-R2-based architecture to determine whether it can preserve the richer channel information (phase, polarization) needed for future interferometric and polarization measurements — currently under project-level review.

---

## Status

| Area | Status |
|---|---|
| Crossed-dipole antennas | 2 deployed at Peach Mountain Observatory |
| RF data acquisition | Operational |
| GNSS positioning | Data collection established; post-processing in progress |
| Interferometry research | Active |
| New receiver architecture | Under review |
| High-school network | 26 partner schools |
| Next deployment | Marquette High School field trip |

---

## Why This Project

This project taught me that hardware systems fail at the *intersection* of disciplines, not in isolation — the connector failure looked electrical but was really a manufacturing process problem. Good engineering isn't just getting something to work once; it's making it repeatable, testable, and deployable by someone else.

## Links

- [SunRISE Mission](https://sunrise.umich.edu/)
- [University of Michigan HelioPhysics Ground Lab](https://heliophysics.engin.umich.edu/)
- [Long Wavelength Array](https://lwa.unm.edu/)

---
*[← Back to main portfolio](../README.md)*
