---
layout: default
title: Home
---


# Welcome to my Technical Page

I am a new Ham Radio operator, as of December 2025. I am in the process of tinkering, making mistakes, and trying to rediscover a lot of information that experienced hams throughout the ages already know. I am doing this as a learning exercise and will attempt to keep decent documentation along the way.

---

# Basic Unun Design Considerations

This section outlines the material properties and design principles required to build a functional 49:1 unun (unbalanced to unbalanced) transformer for an end-fed half-wave antenna.

## 1. Core Material and Permeability
The selection of the ferrite toroid is based on its initial permeability ($\mu_i$). This value determines how much inductance is generated per turn of wire.

| Material Mix | Permeability ($\mu_i$) | Typical Use Case |
| :--- | :--- | :--- |
| **Mix 43** | 850 | Standard for 80m to 10m. Provides high inductance with fewer turns. |
| **Mix 52** | 250 | Better for high power and high frequencies. Resistant to thermal drift. |
| **Mix 61** | 125 | Used for high HF and VHF. Requires more turns for low band operation. |

---

## 2. Inductance and the Inductance Factor ($A_L$)
Each toroid has an **Inductance Factor ($A_L$)** measured in nanohenries per turn squared ($nH/N^2$). This value is a constant provided by the manufacturer that accounts for the specific material mix and the physical dimensions of the core. 

### Reference Table for Common Core Sizes
To assist in calculations, the following table provides the Inductance Factor ($A_L$) for the two most common core sizes used in 100W EFHW builds.

| Core Size | Mix 43 ($A_L$) | Mix 52 ($A_L$) | Mix 61 ($A_L$) |
| :--- | :--- | :--- | :--- |
| **FT140** (1.40 inch OD) | 885 $nH/N^2$ | 362 $nH/N^2$ | 140 $nH/N^2$ |
| **FT240** (2.40 inch OD) | 1075 $nH/N^2$ | 440 $nH/N^2$ | 170 $nH/N^2$ |

### Finding Inductance Factor ($A_L$) Values
To find the correct Inductance Factor for a core, you must cross-reference the physical size (e.g., FT240) with the material mix (e.g., 43). Larger cores have a larger magnetic cross-section and therefore higher Inductance Factor values for the same material. You can find these data tables at the following manufacturer sites:

* [Amidon Ferrite Toroid Specifications](https://www.amidoncorp.com/product_images/specifications/1-02.pdf)
* [Fair-Rite Material Data Sheets](https://www.fair-rite.com/materials/)

### Inductance Calculation
You calculate the total inductance ($L$) using the following formula:

$$L = N^2 \times A_L$$

In this equation, $N$ is the number of turns. Because the turn count is squared, doubling the number of turns results in four times the inductance.



---

### Technical Note on the Values Used
The values in the table above reflect standard industry specifications. Specifically, the value for the **FT240-52** is listed at **440 $nH/N^2$**, which accounts for the specific American-standard permeability found in high quality cores. Using these verified constants ensures that your **Inductive Reactance ($X_L$)** calculations in the following sections remain accurate.

---

## 3. Inductive Reactance ($X_L$)
A transformer must have enough **Inductive Reactance ($X_L$)** to avoid acting as a load on the transmitter. Inductive Reactance is a measure of the transformer's "resistance" to alternating current at a specific frequency. If the reactance is too low, the transformer will dissipate power as heat.

The standard design goal is for the Inductive Reactance to be 4 to 5 times the system impedance. In a 50 $\Omega$ system, the target is $200 \Omega$ to $250 \Omega$ at the lowest operating frequency.

$$X_L = 2\pi f L$$

In this formula, $f$ is the frequency in Hertz and $L$ is the inductance in Henries.

---

## 4. Turns Ratio and Impedance Matching
An end-fed half-wave antenna typically has a feed point impedance near $2450 \Omega$. To match this to $50 \Omega$ coax, a 49:1 impedance ratio is used. Impedance transformation is determined by the square of the turns ratio:

$$\text{Impedance Ratio} = \left(\frac{N_{sec}}{N_{pri}}\right)^2$$

For a 49:1 ratio, the transformer requires a 7:1 turns ratio. Common examples include:
* **2 Primary turns to 14 Secondary turns**
* **3 Primary turns to 21 Secondary turns**

---

## 5. High Frequency Parasitics
At higher frequencies like the 10m band, physical variables called parasitics interfere with the transformer ratio.

* **Leakage Inductance:** This occurs when magnetic flux does not couple perfectly between the primary and secondary windings. It causes SWR to rise on high bands.
* **Inter-winding Capacitance:** This is stray capacitance between the wires. It is caused by turns being placed too close together.

### Common Corrections
* **Winding Spacing:** Spreading the secondary turns around the core reduces capacitance.
* **Bifilar Winding:** Twisting the primary turns with the first few turns of the secondary improves magnetic coupling.
* **Compensation Capacitors:** A high voltage capacitor (often 100pF) placed across the primary can cancel out leakage inductance on 10m.

---

## 6. Core Size and Power
The physical size of the core (such as the FT140 or FT240) determines power handling and heat dissipation. Larger cores or stacks of multiple cores have more surface area. This prevents the material from reaching its Curie temperature, which is the point where the ferrite loses its magnetic properties and the SWR increases rapidly.

---
# Verifying Performance with a Vector Network Analyzer (VNA)
A Vector Network Analyzer, such as the NanoVNA, is used to verify that the unun is performing as designed before it is attached to an antenna. This allows for bench testing where variables can be controlled.

## Using a Dummy Load for Ratio Testing
To test a 49:1 unun, you must terminate the secondary side with a resistance that represents the antenna wire at resonance. Since the unun is designed to match $50 \Omega$ to approximately $2450 \Omega$ ($50 \times 49$), you use a $2450 \Omega$ or $2500 \Omega$ resistor. 

The resistor should be a non-inductive type, such as a metal film or carbon film resistor. Wire-wound resistors have internal inductance that will provide inaccurate readings at high frequencies. When you connect the VNA to the input and the resistor across the output, the VNA should show an SWR near 1.0:1 across the bands where the transformer is efficient.

## Bench SWR vs. Field SWR
It is important to understand that a dummy load test only verifies the transformer ratio. A low SWR on the bench does not guarantee a low SWR when the unun is attached to a real antenna for several reasons:

* **Lack of Counterpoise:** On the bench, there is usually no counterpoise or coax shield acting as a return path. In the field, the coax shield or a dedicated counterpoise wire changes the complex impedance of the system.
* **Parasitics in the Environment:** Nearby objects, height above ground, and the wire insulation itself introduce capacitive and inductive reactances that a simple resistor cannot simulate.
* **Complex Impedance:** An antenna wire is only purely resistive at its resonant points. On other frequencies, it presents a complex impedance that the unun must handle.

If the bench test shows a high SWR on the 10m band with a dummy load, it may actually improve once the antenna is deployed and has a proper counterpoise. Conversely, a perfect bench test might show a higher SWR in the field due to environmental factors. The dummy load test should be treated as a verification of the unun build quality, but final tuning must be done with the antenna wire in its operating position.

---

# Designing a Wideband 49:1 Unun for 80m to 10m Performance

The End-Fed Half-Wave (EFHW) antenna is a common choice for amateur radio because it allows for multi-band operation with one wire. The 49:1 impedance transformer is the critical component of this system. It transforms the feedpoint impedance of approximately 2500 $\Omega$ to the 50 $\Omega$ impedance expected by the radio. The goal of this project is to design a transformer that covers the 80 meter phone band through 10 meters with acceptable efficiency for 100W or under.

## 1. Antenna System Overview and Goals
This analysis focuses on an antenna design covering 75 meters (the 80m phone band) through 10 meters. The system uses a single wire approximately 130 feet long. To manage resonance and impedance across this range, two specific components are integrated into the radiator.

* **Compensation Coil:** This is a loading coil consisting of a few turns on a 1 inch OD form. My current understanding is that its purpose is to pull the resonance frequencies of the upper bands downward into more preferable locations. This understanding is supported by experiments placing coils of various sizes at distances between 6 and 12 feet from the feedpoint.
* **Parallel RC Network:** My understanding of this network is that it is placed in the wire span to improve antenna performance in the phone portion of the 80 meter band, or to stabilize antenna behavior across multiple bands.

I am qualifying these statements with the fact that this is my current understanding. As a beginner in antenna design, I may be misinterpreting the specific physics of how these elements interact with the radiator. My objective is to create an antenna for 100W operation that works across multiple bands with minimal tuning. I intend to use this antenna frequently on 40, 20, 17, and 15 meters, and occasionally on 80, 12, and 10 meters.



## 2. The Engineering Conflict
Designing a high power transformer for this frequency range involves competing technical requirements. To maintain efficiency on the lower bands, the **Inductive Reactance ($X_L$)** must be high to prevent the transformer from acting as a shunt load. This is achieved by increasing turns or using high permeability material.

Higher frequencies involve different challenges. Every additional turn increases leakage inductance and capacitance between the windings. At 28 MHz, these factors can cause high SWR even when the wire length is correct.

## 3. Quantitative Analysis of Hybrid Core Stacks
In this analysis, I examine unun configurations to see how core size and hybrid material stacks (mixing Type 43 and Type 52 ferrites) affect performance.

* **The 4x Rule:** Verifying that Inductive Reactance remains between $200 \Omega$ and $250 \Omega$.
* **Geometry:** Testing if an FT140 core can outperform an FT240 on high bands by using less wire.
* **Hybrid Stacks:** Evaluating if mixing material types can improve frequency response.

### Verified Inductance Factor ($A_L$) Values
The following values are used for these calculations. They have been verified against Amidon and Fair-Rite data sheets:
* **FT240-43 Inductance Factor ($A_L$):** $1075 \text{ nH/N}^2$
* **FT240-52 Inductance Factor ($A_L$):** $440 \text{ nH/N}^2$

---

## 4. Inductive Reactance Results for Hybrid Configurations

### Configuration 1: 2x Mix 43 + 1x Mix 52
**Total Stack Inductance Factor ($A_L$):** $(2 \times 1075) + 440 = 2590 \text{ nH/N}^2$

| Band | Frequency | 2-Turn Inductive Reactance ($L = 10.36 \mu\text{H}$) | 3-Turn Inductive Reactance ($L = 23.31 \mu\text{H}$) |
| :--- | :--- | :--- | :--- |
| **80m** | 3.5 MHz | $227.8 \Omega$ | $512.6 \Omega$ |
| **40m** | 7.1 MHz | $462.1 \Omega$ | $1040.3 \Omega$ |
| **20m** | 14.1 MHz | $918.0 \Omega$ | $2066.0 \Omega$ |
| **15m** | 21.2 MHz | $1380.2 \Omega$ | $3106.8 \Omega$ |
| **10m** | 28.4 MHz | $1848.7 \Omega$ | $4161.4 \Omega$ |

### Configuration 2: 2x Mix 52 + 1x Mix 43
**Total Stack Inductance Factor ($A_L$):** $(2 \times 440) + 1075 = 1955 \text{ nH/N}^2$

| Band | Frequency | 2-Turn Inductive Reactance ($L = 7.82 \mu\text{H}$) | 3-Turn Inductive Reactance ($L = 17.59 \mu\text{H}$) |
| :--- | :--- | :--- | :--- |
| **80m** | 3.5 MHz | $171.9 \Omega$ | $386.9 \Omega$ |
| **40m** | 7.1 MHz | $349.0 \Omega$ | $785.4 \Omega$ |
| **20m** | 14.1 MHz | $693.3 \Omega$ | $1560.1 \Omega$ |
| **15m** | 21.2 MHz | $1041.9 \Omega$ | $2344.6 \Omega$ |
| **10m** | 28.4 MHz | $1395.7 \Omega$ | $3141.2 \Omega$ |

---

## 5. Technical Observations
The target for Inductive Reactance is $200 \Omega$ to $250 \Omega$ at the lowest frequency. In Configuration 1, a 2-turn primary meets this requirement with $227.8 \Omega$. In Configuration 2, the 2-turn primary is too low at $171.9 \Omega$, which can result in efficiency losses and heating on 80 meters.

While 3 turns provide high Inductive Reactance on all bands, the additional wire length often creates SWR issues on 10 meters. Configuration 1 with 2 turns appears to be the most balanced option for this multi-band requirement.