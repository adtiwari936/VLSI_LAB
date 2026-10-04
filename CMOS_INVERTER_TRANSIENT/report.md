# CMOS Inverter Transient Analysis

## Objective

To design and simulate a CMOS inverter in LTspice and measure its transient switching characteristics, including rise time, fall time, and propagation delay.

## Circuit Description

The CMOS inverter consists of:

- One PMOS transistor connected between `VDD` and the output.
- One NMOS transistor connected between the output and ground.
- A pulse voltage source connected to both transistor gates.
- A `20 fF` load capacitor connected between the output and ground.

The inverter operation is:

- When `VIN = 0 V`, the PMOS is ON and the NMOS is OFF. Therefore, `VOUT` is high.
- When `VIN = 5 V`, the PMOS is OFF and the NMOS is ON. Therefore, `VOUT` is low.

The logic function is:

$$
V_{OUT}=\overline{V_{IN}}
$$

## Simulation Screenshot

![](/CMOS_INVERTER_TRANSIENT/Images/simulation_ss.png)

> Figure : CMOS Inverter Transient analysis and Waveform

## Simulation Parameters

| Parameter                   |   Value |
| --------------------------- | ------: |
| Supply voltage, \(V\_{DD}\) |     5 V |
| Input voltage range         |   0–5 V |
| NMOS width                  |   10 µm |
| PMOS width                  |   20 µm |
| Channel length              |    1 µm |
| Load capacitance            |   20 fF |
| Input pulse period          |   20 ns |
| Simulation duration         |  100 ns |
| Maximum time step           | 0.05 ns |

## Input Signal

The input voltage source was defined as:

```spice
PULSE(0 5 0 1n 1n 10n 20n)
```

This produces a periodic signal that switches between 0 V and 5 V.

## Transient Analysis

The transient simulation command was:

```spice
.tran 0.05n 100n 0 0.05n
```

The simulation calculates the circuit response from 0 ns to 100 ns.

## Measurement Definitions

For a 0–5 V output signal:

$$
V_{10\%}=0.1V_{DD}=0.5\text{ V}
$$

$$
V_{50\%}=0.5V_{DD}=2.5\text{ V}
$$

$$
V_{90\%}=0.9V_{DD}=4.5\text{ V}
$$

The LTspice measurement commands were:

```spice
.meas tran tr TRIG V(VOUT) VAL=0.5 RISE=1 TARG V(VOUT) VAL=4.5 RISE=1

.meas tran tf TRIG V(VOUT) VAL=4.5 FALL=1 TARG V(VOUT) VAL=0.5 FALL=1

.meas tran tphl TRIG V(VIN) VAL=2.5 RISE=1 TARG V(VOUT) VAL=2.5 FALL=1

.meas tran tplh TRIG V(VIN) VAL=2.5 FALL=1 TARG V(VOUT) VAL=2.5 RISE=1

.meas tran tpd PARAM='(tphl+tplh)/2'
```

## Measurement Formulas

The output rise time is measured from 10% to 90% of \(V\_{DD}\):

$$
t_r=t_{90\%}-t_{10\%}
$$

The output fall time is measured from 90% to 10% of \(V\_{DD}\):

$$
t_f=t_{10\%}-t_{90\%}
$$

The high-to-low propagation delay is:

$$
t_{PHL}=t_{\text{output falling at }50\%}
-t_{\text{input rising at }50\%}
$$

The low-to-high propagation delay is:

$$
t_{PLH}=t_{\text{output rising at }50\%}
-t_{\text{input falling at }50\%}
$$

The average propagation delay is:

$$
t_{pd}=\frac{t_{PHL}+t_{PLH}}{2}
$$

## Simulation Results

| Parameter                |      LTspice Result | Converted Result |
| ------------------------ | ------------------: | ---------------: |
| Rise time (tr)           | 6.97979150211e-10 s |         0.698 ns |
| Fall time (tf)           | 6.97283937220e-10 s |         0.697 ns |
| High-to-low delay (tphl) | 4.63730437516e-10 s |         0.464 ns |
| Low-to-high delay (tplh) | 4.63754630472e-10 s |         0.464 ns |
| Average delay (tpd)      | 4.63742533994e-10 s |         0.464 ns |

The average propagation delay is:

$$
t_{pd}=\frac{t_{PHL}+t_{PLH}}{2}
$$

$$
t_{pd}
=
\frac{0.4637304375+0.4637546305}{2}
=
0.4637425340\text{ ns}
$$

Therefore:

$$
\boxed{t_{pd}\approx0.464\text{ ns}}
$$
