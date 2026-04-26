---
layout: default
title: Designing Multiband 75m to 10m EFHW and Wide-band 49:1 Unun
category: article
date: 2026-04-26
---


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

# Applied Engineering

I'm going to start with a hybrid stack of 2x FT240-43 and 1x FT240-52.  I'm using inexpensive 18 AWG lacquered magnet wire