# Two-Stage CMOS Operational Amplifier | Cadence Virtuoso | gpdk90

Designed and optimized a two-stage CMOS operational amplifier in Cadence Virtuoso using gpdk90 technology. The amplifier was systematically sized from hand calculations and characterized using DC, AC and transient analyses to optimize gain, bandwidth, stability, power consumption and slew rate.

---

## Design Specifications

| Parameter | Value |
|-----------|-------|
| Technology | gpdk90 |
| Supply Voltage | 1.8 V |
| Target GBW | 10 MHz |
| Compensation Capacitor | 1 pF |
| Load Capacitor | 1 pF |
| Design Methodology | Moderate Inversion (gm/Id) |

---

## Initial Design Methodology

Instead of selecting transistor dimensions by trial and error, the amplifier was designed using the **gm/Id methodology**.

The design flow followed was

```
Target GBW
      ↓
Calculate gm
      ↓
Select gm/Id = 10 V⁻¹
      ↓
Calculate Drain Current
      ↓
Select VOV ≈ 200 mV
      ↓
Fix L = 200 nm
      ↓
Adjust Width until required VOV is obtained
      ↓
Build Complete Op-Amp
      ↓
Optimize Individual Transistors
```

### Current Calculation

Using

\[
GBW=\frac{g_m}{2\pi C_C}
\]

Target

- GBW = **10 MHz**
- CC = **1 pF**

Calculated

- gm ≈ **62.8 μS**

For moderate inversion,

\[
\frac{g_m}{I_D}=10\;V^{-1}
\]

Therefore,
- M1 = M2 = **6.3 μA**
- Tail Current = **12.6 μA**

These currents were used as the design currents throughout the amplifier.

## Transistor-Level Optimization

The initial design achieved an open-loop gain of **35 dB**. To improve the performance, the output resistance (`ro = 1/gds`) of every transistor was analyzed. At each stage, the transistor with the lowest output resistance was identified as the bottleneck and optimized by increasing its channel length while slightly adjusting the width to maintain the required drain current.

### Stage 1 – M7 Optimization
- M7 exhibited the lowest output resistance in the second stage and limited the overall gain.
- Increased **L: 200 → 270 nm** and **W: 275 → 310 nm**, maintaining **ID ≈ 12.1 µA**.
- Gain improved from **35 dB → 37.8 dB**, while the output bias shifted closer to mid-supply.

### Stage 2 – M6 Optimization
- After optimizing M7, M6 became the new bottleneck.
- Increased **L: 200 → 225 nm** and **W: 450 → 480 nm** to recover the drain current.
- Overall gain improved from **37.8 dB → 40.1 dB**.

### Stage 3 – M1 & M2 Optimization
- The first-stage differential pair was optimized by increasing **L: 200 → 300 nm** and **W: 150 → 160 nm**.
- This significantly increased the intrinsic gain by improving the output resistance while maintaining **ID ≈ 6.1 µA**.
- The DC operating point remained nearly unchanged.

### Stage 4 – M3 & M4 Optimization
- The PMOS active load was optimized by increasing **L: 200 → 275 nm**, while **W = 275 nm** was maintained.
- The output resistance increased from **≈440 kΩ → ≈780 kΩ**.
- Final open-loop gain reached **42.5 dB**.
- 
## M5 Optimization
Initially, **M5** was intentionally left unchanged because its output resistance contributes only a second-order effect to the differential gain.
Later, during the **CMRR characterization**, M5 was revisited. Its channel length was increased to improve the output resistance of the tail current source while maintaining approximately the same drain current.
This modification had **negligible effect on the differential gain**, but reduced the common-mode gain and improved the overall CMRR.

### Bias Circuit
- During transistor optimization, an ideal **458 mV** bias source was used for M5.
- After completing the sizing, the ideal bias was replaced with a self-biased current reference that generated approximately the same bias voltage and operating currents.

# Performance Characterization

After completing the transistor sizing and bias optimization, the operational amplifier was characterized using DC, AC and transient analyses.

---

## DC Operating Point

The DC operating point verifies that all transistors operate in the saturation region with the desired bias currents and output voltages.

📷 **Insert DC Operating Point Screenshot Here**

**Final DC Operating Point**

| Parameter | Value |
|-----------|-------|
| Vbias | **458.56 mV** |
| I1 = I2 = I3 = I4 | **6.12 µA** |
| I5 | **12.25 µA** |
| I6 | **≈12.9 µA** |
| I7 | **12.93 µA** |
| VOUT1 | **1.275 V** |
| VOUT | **1.02 V** |

---

## Output Swing

The output swing was measured by configuring the op-amp as a unity-gain buffer and sweeping the input voltage from **0 V to 1.8 V**.

📷 **Insert Output Swing Screenshot Here**

| Parameter | Value |
|-----------|-------|
| Linear Output Swing | **0 – 1.56 V** |
| Maximum Output Swing | **0 – 1.64 V** |

---

## Slew Rate

The slew rate was measured using transient analysis by applying a pulse input and calculating the slope of the output waveform.

📷 **Insert Positive Slew Rate Screenshot Here**

Positive Slew Rate

\[
SR_+=\frac{\Delta V}{\Delta t}
=\frac{115.13mV}{20.51ns}
=5.61V/\mu s
\]

---

📷 **Insert Negative Slew Rate Screenshot Here**

Negative Slew Rate

\[
SR_-=\frac{\Delta V}{\Delta t}
=\frac{151.81mV}{26.48ns}
=5.73V/\mu s
\]

The measured slew rate closely matches the theoretical approximation

\[
SR\approx\frac{I}{C_C+C_L}
\]

where

\[
\frac{12.6\mu A}{1pF+1pF}
=
6.3V/\mu s
\]

which is close to the measured value.

---

## Input Common Mode Range (ICMR)

ICMR was determined by sweeping the common-mode input voltage while monitoring the operating region of every transistor.

📷 **Insert ICMR Screenshot Here**

| Common Mode Voltage | Observation |
|---------------------|-------------|
| **< 190 mV** | Multiple transistors leave saturation |
| **190 mV** | M5, M7 enter Linear Region |
| **550 mV** | M5 in Linear, M7 in Saturation |
| **650 mV** | All transistors in Saturation ✅ |
| **650 mV – 1.545 V** | Normal Operating Region |
| **>1.545 V** | M1 and M2 enter Linear Region |

### Final ICMR

| Parameter | Value |
|-----------|-------|
| **ICMR** | **650 mV – 1.545 V** |

Within this range, all MOSFETs operate in saturation and the amplifier functions correctly.
---
## Common-Mode Rejection Ratio (CMRR)

The common-mode gain was measured by applying the same AC signal to both input terminals while keeping the amplifier in open-loop configuration.

📷 **Insert Initial Common-Mode Gain Screenshot Here**

Initially, the measured common-mode gain was **-1.71 dB**. To improve CMRR, the output resistance of the tail current source (**M5**) was increased by modifying its dimensions while maintaining nearly the same drain current.

| M5 Parameter | Initial | Optimized |
|:------------|:-------:|:---------:|
| Width | 275 nm | 325 nm |
| Length | 200 nm | 245 nm |

📷 **Insert Optimized Common-Mode Gain Screenshot Here**

The optimization reduced the common-mode gain from **-1.71 dB** to **-4 dB**, while the differential gain remained unchanged at **42.5 dB**.

> **Final CMRR = 42.5 − (−4) = 46.5 dB**
---

## Power Supply Rejection Ratio (PSRR)

PSRR measures the ability of the op-amp to reject variations in the supply voltage. A higher PSRR indicates better immunity to supply noise.

📷 **Insert PSRR Analysis Screenshot Here**

The PSRR was calculated from the measured supply-to-output gain (`Avdd`) using:

`PSRR = 20 log10(1 / Avdd)`

**Final PSRR:** **6.15 dB**

---

## Final Performance Summary

| Parameter | Result |
|-----------|--------|
| Open Loop Gain | **42.5 dB** |
| Unity Gain Bandwidth | **5 MHz** |
| Phase Margin | **46°** |
| Power Dissipation | **64 µW** |
| Positive Slew Rate | **5.61 V/µs** |
| Negative Slew Rate | **5.73 V/µs** |
| Linear Output Swing | **0 – 1.56 V** |
| Maximum Output Swing | **0 – 1.64 V** |
| ICMR | **0.65 – 1.545 V** |
| CMRR | **46.5 dB** |
| PSRR | **6.15 dB** |
