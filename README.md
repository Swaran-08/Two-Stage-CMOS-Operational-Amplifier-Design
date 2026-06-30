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

