# CMOS Inverter Power Dissipation Analysis

## Objective

To analyze the transient power dissipation of a CMOS inverter using LTspice.

The analysis focuses on:

- Supply current.
- Instantaneous power.
- Average power.
- Switching-power behavior.
- Energy consumed during one input cycle.

## Circuit Description

The CMOS inverter consists of:

- One PMOS transistor connected between `VDD` and `VOUT`.
- One NMOS transistor connected between `VOUT` and ground.
- A pulse input connected to both transistor gates.
- A `50 fF` load capacitor connected between `VOUT` and ground.

The circuit operates as follows:

- When `VIN = 0 V`, the PMOS is ON and the NMOS is OFF. The output is charged to approximately 5 V.
- When `VIN = 5 V`, the PMOS is OFF and the NMOS is ON. The output is discharged toward 0 V.

The inverter logic function is:

$$
V_{OUT}=\overline{V_{IN}}
$$

## Simulation Waveform

![CMOS inverter power waveform](./Images/power_waveform.png)

_Figure 1: Input voltage, inverter output voltage, instantaneous power, and supply current._

## Simulation Parameters

| Parameter             |     Value |
| --------------------- | --------: |
| Supply voltage, `VDD` |       5 V |
| Input voltage range   |     0–5 V |
| NMOS width            |     10 µm |
| PMOS width            |     20 µm |
| Channel length        |      1 µm |
| Load capacitance      |     50 fF |
| Pulse rise time       |      1 ns |
| Pulse fall time       |      1 ns |
| Pulse width           |     30 ns |
| Pulse period          |     60 ns |
| Input frequency       | 16.67 MHz |
| Duty cycle            |       50% |
| Simulation duration   |    120 ns |
| Maximum time step     |   0.05 ns |

## Input Signal

The input source was defined as:

```spice
PULSE(0 5 0 1n 1n 30n 60n)
```

The input frequency is:

$$
f=\frac{1}{T}
$$

$$
f=\frac{1}{60\text{ ns}}\approx16.67\text{ MHz}
$$

The duty cycle is:

$$
D=\frac{T_{ON}}{T}\times100
$$

$$
D=\frac{30\text{ ns}}{60\text{ ns}}\times100=50\%
$$

## Transient Analysis

The transient simulation command was:

```spice
.tran 0 120n 0 0.05n
```

The simulation was performed from 0 ns to 120 ns. This includes two complete input cycles.

The measurement interval used for average power was:

```text
60 ns to 120 ns
```

This interval contains one complete input cycle and avoids including the initial startup transition.

## Supply Current

The current through the supply voltage source was plotted using:

```spice
-I(VDD)
```

The negative sign is used because of LTspice's voltage-source current reference direction.

The actual current drawn from the supply is therefore:

$$
I_{DD}(t)=-I(VDD)
$$

## Instantaneous Power

The instantaneous supply power was calculated using:

```spice
V(VDD)*(-I(VDD))
```

The mathematical expression is:

$$
P_{DD}(t)=V_{DD}(t)I_{DD}(t)
$$

Since the supply voltage is constant:

$$
P_{DD}(t)=5\text{ V}\times I_{DD}(t)
$$

The power is concentrated near the switching transitions because the load capacitor is charged and discharged during those intervals.

## LTspice Measurement Commands

The following commands were used:

```spice
.meas tran avg_current AVG (-I(VDD)) FROM=60n TO=120n

.meas tran avg_power AVG (V(VDD)*(-I(VDD))) FROM=60n TO=120n

.meas tran max_power MAX (V(VDD)*(-I(VDD))) FROM=60n TO=120n

.meas tran energy_cycle INTEG (V(VDD)*(-I(VDD))) FROM=60n TO=120n

.meas tran avg_power_calc PARAM='energy_cycle/60n'
```

## Power Measurement Formulas

The average supply current is:

$$
I_{\text{avg}}
=
\frac{1}{T_m}
\int_{t_1}^{t_2}I_{DD}(t)\,dt
$$

The average power is:

$$
P_{\text{avg}}
=
\frac{1}{T_m}
\int_{t_1}^{t_2}P_{DD}(t)\,dt
$$

The energy consumed during one 60 ns cycle is:

$$
E_{\text{cycle}}
=
\int_{t_1}^{t_1+60\text{ ns}}P_{DD}(t)\,dt
$$

The average power calculated from the cycle energy is:

$$
P_{\text{avg}}
=
\frac{E_{\text{cycle}}}{60\text{ ns}}
$$

## Why Power Is Almost Zero Most of the Time

The instantaneous power remains close to zero during the steady logic states. This is expected for a CMOS inverter.

When `VIN = 0 V`:

- The PMOS is ON.
- The NMOS is OFF.
- The output capacitor charges to approximately `VDD`.
- After charging is complete, the supply current becomes very small.

When `VIN = 5 V`:

- The PMOS is OFF.
- The NMOS is ON.
- The output capacitor discharges to ground.
- After discharging is complete, the supply current again becomes very small.

Therefore:

$$
P_{\text{static}}\approx0
$$

Most of the power is consumed during the switching transitions.

## Capacitive Energy

The energy stored in the load capacitor is:

$$
E_C=\frac{1}{2}C_LV_{DD}^{2}
$$

For this circuit:

$$
E_C
=
\frac{1}{2}
\times50\text{ fF}
\times(5\text{ V})^2
$$

$$
E_C=625\text{ fJ}
$$

This small energy value explains why the power spikes are narrow and why the power remains close to zero for most of the simulation.

## Theoretical Dynamic Power

The approximate dynamic-power equation is:

$$
P_{\text{dynamic}}=\alpha C_LV_{DD}^{2}f
$$

where:

- $$\alpha$$ is the switching activity factor.
- $$C_{L}$$ is the load capacitance.
- $$V_{DD}$$ is the supply voltage.
- $$f$$ is the input frequency.

For this simulation:

$$
\alpha=0.5
$$

$$
C_L=50\text{ fF}
$$

$$
V_{DD}=5\text{ V}
$$

$$
f=16.67\text{ MHz}
$$

Therefore:

$$
P_{\text{dynamic}}
=
0.5
\times50\text{ fF}
\times(5\text{ V})^2
\times16.67\text{ MHz}
$$

$$
P_{\text{dynamic}}\approx10.42\ \mu\text{W}
$$

This is an approximate theoretical value. The LTspice result may differ because the simulation includes transistor model capacitances, short-circuit current, and the actual voltage and current waveforms.

## Simulation Results

Replace the placeholders with the values from the LTspice SPICE Error Log.

| Quantity                    |      LTspice result | Converted value |
| --------------------------- | ------------------: | --------------: |
| Average supply current      | 9.99694475179e-05 A |       99.969 µA |
| Average power               | 4.99872088390e-04 W |      499.872 µW |
| Maximum instantaneous power | 2.54163499922e-02 W |       25.416 mW |
| Energy per 60 ns cycle      | 2.99923253034e-11 J |       29.992 pJ |
| Average power from energy   | 4.99872088389e-04 W |      499.872 µW |

## Observations

- The output voltage is the logical complement of the input voltage.
- The input frequency is approximately 16.67 MHz.
- The input duty cycle is 50%.
- Supply-current spikes occur near the input transitions.
- Instantaneous power is concentrated around switching events.
- Power remains almost zero during steady-state logic levels.
- The PMOS charges the output capacitor during a low-to-high output transition.
- The NMOS discharges the output capacitor during a high-to-low output transition.
- The simplified CMOS inverter has very low static power dissipation.
- Increasing the load capacitance, supply voltage, or switching frequency increases dynamic power.

## Conclusion

The transient power dissipation of a CMOS inverter was successfully analyzed using LTspice.

The instantaneous power was calculated from the supply voltage and supply current:

$$
P_{DD}(t)=V_{DD}(t)I_{DD}(t)
$$

The simulation shows that power is almost zero during steady-state operation and appears mainly as short pulses during switching transitions. This occurs because the load capacitor consumes energy while charging and discharging.

The approximate theoretical dynamic power for the selected parameters is:

$$
\boxed{P_{\text{dynamic}}\approx10.42\ \mu\text{W}}
$$

The measured LTspice average power should be compared with this theoretical value after entering the results from the SPICE Error Log.
