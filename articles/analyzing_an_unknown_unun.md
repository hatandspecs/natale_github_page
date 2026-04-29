---
layout: default
title: Analyzing an Commercial 75m to 10m EFHW Unun
category: article
date: 2026-04-29
---


## Part 1: The FT240 Stack. Reverse Engineering a Commercial Unun

In the world of amateur radio, commercial "black box" ununs often hide their best secrets under layers of heat shrink and epoxy. When a device features an unconventional winding scheme, understanding its true performance, specifically the **primary inductive reactance ($X_L$)**, requires moving beyond simple SWR readings into the realm of slightly more complex admittance modeling.

### The Anatomy of the Device
The EFHW transformer in question promises useable SWR on amateur bands from 75m to 10m.  It is built on two stacked **FT240** toroids of an unknown ferrite mix. These are stacked directly with no spacing to maximize the magnetic cross-sectional area. The winding geometry is a variation of the hybrid Ruthroff/Guanella design, likely intended to function as a 56:1 impedance transformer with a 2:15 turns ratio.

* **Winding Ratio:** 2 Primary turns to 15 Secondary turns (1:7.5 voltage ratio).
* **Winding Style:** A "twisted pair" start for the first 2 turns to maximize coupling, followed by a split-secondary winding. The first 5 secondary turns are wound clockwise (around the circumference of the core) and spaced tightly together (touching). After a crossover, the final 8 turns are wound counter-clockwise (around the circumference of the core) and spaced slightly apart. The entire winding occupies less than half of the core circumference.
* **Compensation:** A **120 pF** ceramic capacitor labeled 121 across the primary.

---

### Measurements with a 2500 Ohm Dummy Load

To evaluate how the unun performs in a real-world scenario, the device was tested using a 2500 Ohm non-inductive dummy load. This value was chosen to simulate the typical feedpoint impedance of an End-Fed Half-Wave (EFHW) antenna, representing an approximate 50:1 transformation ratio.

The following table details the SWR and complex impedance measured at the primary input side with the NanoVNA:

| Frequency (MHz) | SWR | Complex Impedance ($R + jX$) |
| :--- | :--- | :--- |
| **3.803** | 1.21 | 40.3 + 4.0j $\Omega$ |
| **7.186** | 1.49 | 34.6 - 5.9j $\Omega$ |
| **14.243** | 2.07 | 24.3 - 3.9j $\Omega$ |
| **28.453** | 1.59 | 70.9 + 18.3j $\Omega$ |

These loaded measurements provide the baseline for our performance analysis. While the SWR remains largely usable, being under 2.1:1 for most of the range, the shifting real and imaginary components suggest that the transformation mechanism is evolving as frequency increases.

---

### Decoding the Primary Inductive Reactance ($X_L$)
To find the $X_L$ of the windings alone, we must mathematically remove the 120 pF capacitor. Since the capacitor and the primary winding are in parallel, we convert the series impedance measurements ($Z$) into parallel admittance ($Y$).  I measured the no-load complex impedance at each of the frequencies I was interested in.

#### The Mathematical Model
We calculate the capacitive susceptance ($B_C$) of the 120 pF capacitor at each frequency and subtract it from the measured net susceptance ($B_{net}$) to isolate the inductive susceptance ($B_L$).

$$B_C = 2 \pi f C$$
$$X_L = \frac{1}{|B_{net} - B_C|}$$

| Frequency (MHz) | No-Load Impedance ($Z$) | Net Susceptance ($B_{net}$) | Cap. Susceptance ($B_C$) | **Primary $X_L$** |
| :--- | :--- | :--- | :--- | :--- |
| **3.803** | 335 + 70.2j | -0.00060 S | 0.00287 S | **288.4 $\Omega$** |
| **7.186** | 36.1 - 92.4j | +0.00939 S | 0.00542 S | **251.9 $\Omega$** |
| **14.243** | 6.74 - 32.5j | +0.02948 S | 0.01074 S | **53.4 $\Omega$** |
| **28.435** | 8.03 + 10.1j | -0.06065 S | 0.02144 S | **12.2 $\Omega$** |

---

### Estimating the $A_L$ and Ferrite Mix
The $A_L$ value, representing inductance per square turn, is the fingerprint of the ferrite core. The relationship between the inductance, the material permeability, and the physical dimensions of the core is expressed by the following equation:

$$A_L = \frac{L}{N^2} = \frac{\mu_0 \mu_i A_e}{l_e}$$

Where:
* $\mu_0$ is the permeability of free space ($4\pi \times 10^{-7}$ H/m).
* $\mu_i$ is the initial relative permeability of the ferrite material.
* $A_e$ is the effective cross-sectional area of the core.
* $l_e$ is the effective magnetic path length.

Using the 3.8 MHz data, where the inductive component is most stable, we can estimate the inductance ($L$) and the $A_L$ for the stack.

1.  **Inductance ($L$) at 3.8 MHz:** $$L = \frac{X_L}{2 \pi f} \approx 12.07 \text{ }\mu\text{H}$$
2.  **$A_L$ Calculation:** Since $L = A_L \cdot N^2$ (with $N=2$):
    $$A_L = \frac{12,070 \text{ nH}}{4} \approx 3,017 \text{ nH/turn}^2$$

For a stack of two FT240 cores, where the total $A_e$ is doubled while $l_e$ remains constant, an $A_L$ of 3,017 points toward **Type 43 Ferrite**. While nominal single-core values are approximately 1075 nH/turn$^2$, permeability often rises at the lower end of the HF spectrum before the core becomes predominantly resistive.

---

### Technical Observations
* **Core Loss:** The resistance at 3.8 MHz (335 $\Omega$) is relatively low for an unloaded transformer. This confirms the core is operating in its lossy region, which is common for Type 43 at low frequencies. This unun will convert a measurable portion of power into heat on the 80m band.
* **The Crossover Winding:** The use of a crossover, reversing the winding direction, is a sophisticated touch designed to cancel out phase accumulation and push the self-resonant frequency (SRF) higher.
* **Efficiency:** With the 2500 Ohm load, the 40.3 + 4j reading at 3.8 MHz is excellent. It shows that even though the core is lossy, the transformation ratio is spot-on and the 120 pF capacitor is neutralizing the leakage inductance of the secondary.

---

## Part 2: The Complex Permeability Paradox

We discovered a counter-intuitive phenomenon where the primary inductive reactance ($X_L$) plummeted as frequency increased, dropping from 288.4 $\Omega$ at 3.8 MHz to a mere 12.2 $\Omega$ at 28 MHz. In a perfect air-core inductor, reactance should climb linearly with frequency. However, in ferrite-core transformers, we are at the mercy of **complex permeability**.

### The Ferrite Wall: $\mu'$ vs. $\mu''$
Ferrite materials like Type 43 are defined by $\mu'$ (Mu-prime), the inductive component, and $\mu''$ (Mu-double prime), the resistive component. As frequency increases across the HF spectrum, the $\mu'$ craters. The core stops acting like a storage device for magnetic energy and starts acting like a high-frequency resistor. This explains the collapse of $X_L$ because the value of $L$ is dropping faster than the frequency is rising.



### From Induction to Transmission Line Mode
If the inductive reactance is only 12.2 $\Omega$ at 10m, why is the SWR only 1.59? The secret is the twisted-pair start.

* **At 80m (3.8 MHz):** The device operates primarily via **magnetic induction**. The core does the work.
* **At 10m (28 MHz):** Induction has failed. However, the device shifts into **Transmission Line Transformer (TLT)** mode. Energy is coupled directly between the tightly twisted wires rather than through the ferrite itself.

### The Appropriateness of the Design
Is an unun with 12.2 $\Omega$ of $X_L$ on 10m appropriate?
1.  **For SWR:** It is highly effective. The design cheats the physics of the ferrite to provide a usable match.
2.  **For Efficiency:** It is less than ideal. Because $X_L$ is so low, significant current flows through a core that has become resistive. This leads to **thermal dissipation**. In high-duty cycle modes, this energy becomes heat.

### Final Technical Summary
This unun is a classic example of broadband engineering trade-offs. It prioritizes a low SWR across the entire HF spectrum by utilizing a hybrid design consisting of a magnetic transformer on the low bands and a transmission line transformer on the high bands. It remains a functional solution, provided the user respects the thermal limits of the core.