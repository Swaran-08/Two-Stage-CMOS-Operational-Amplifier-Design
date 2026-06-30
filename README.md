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
