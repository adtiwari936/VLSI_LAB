# CMOS Inverter VTC Using LTspice

## 1. Aim

To design a CMOS inverter in LTspice and obtain its DC voltage-transfer characteristic (VTC).

## 2. Theory

A CMOS inverter uses one PMOS transistor and one NMOS transistor.

- The PMOS is connected between `VDD` and `OUT`.
- The NMOS is connected between `OUT` and ground.
- Both gates are connected to the input `IN`.
- Both drains are connected to the output `OUT`.

The inverter performs the NOT operation:

```text
IN = 0  →  OUT = 1
IN = 1  →  OUT = 0
```

### Operation

| Input condition | PMOS | NMOS | Output |
|---|---|---|---|
| `IN = 0 V` | ON | OFF | Approximately `VDD` |
| `IN = VDD` | OFF | ON | Approximately `0 V` |
| Intermediate input | ON | ON | Rapid transition |

The VTC is a graph of output voltage against input voltage:

```text
VTC = VOUT versus VIN
```

The switching voltage is the point where the input and output voltages are approximately equal.

## 3. Circuit Connections

| Component terminal | Connection |
|---|---|
| PMOS source | `VDD` |
| PMOS drain | `OUT` |
| PMOS gate | `IN` |
| NMOS drain | `OUT` |
| NMOS source | Ground |
| NMOS gate | `IN` |

Use:

```text
VDD = 5 V
PMOS: W = 2 µm, L = 180 nm
NMOS: W = 1 µm, L = 180 nm
```

## 4. LTspice Directives

Place the following directives using the `S` shortcut:

```spice
.model NM NMOS (VTO=0.7 KP=200u LAMBDA=0.02)
.model PM PMOS (VTO=-0.7 KP=100u LAMBDA=0.02)
.dc VIN 0 5 1m
```

The command below sweeps the input source named `VIN` from 0 V to 5 V in 1 mV steps:

```spice
.dc VIN 0 5 1m
```

> Draw the voltage sources and MOSFETs graphically. Do not paste their complete netlist as another SPICE directive, or LTspice may report duplicate-instance errors.

## 5. Procedure

1. Open LTspice and create a new schematic.
2. Place one PMOS, one NMOS, two voltage sources, and ground.
3. Connect the PMOS source to `VDD`.
4. Connect the NMOS source to ground.
5. Connect both drains to `OUT`.
6. Connect both gates to `IN`.
7. Set the supply voltage to 5 V.
8. Name the input voltage source `VIN`.
9. Assign the models `PM` and `NM` to the transistors.
10. Add the model and DC-sweep directives given above.
11. Click **Run**.
12. Click the `OUT` node to plot `V(OUT)`.
13. Save a screenshot of the circuit and the VTC plot.

## 6. Expected VTC

The VTC should decrease from approximately 5 V to 0 V:

```text
VOUT
 5 V |───────────────
     |              \
     |               \
     |                \
 0 V |                 ─────────
     +-------------------------- VIN
     0 V                      5 V
```

Expected observations:

- At `VIN = 0 V`, `VOUT` is approximately 5 V.
- At an intermediate input voltage, the output changes rapidly.
- At `VIN = 5 V`, `VOUT` is approximately 0 V.

## 7. Screenshot

Add the LTspice screenshots below this heading.

### Screenshot 1: CMOS inverter schematic

_Insert or paste the screenshot here._

Suggested caption:

> Figure 1: CMOS inverter schematic designed in LTspice.

### Screenshot 2: DC voltage-transfer characteristic

_Insert or paste the VTC plot here._

Suggested caption:

> Figure 2: DC voltage-transfer characteristic of the CMOS inverter.

## 8. Observation Table

| Input voltage | Output voltage |
|---:|---:|
| 0 V | ____ V |
| 1 V | ____ V |
| 2 V | ____ V |
| 2.5 V | ____ V |
| 3 V | ____ V |
| 4 V | ____ V |
| 5 V | ____ V |

## 9. Result

The CMOS inverter was designed and simulated successfully in LTspice. The DC sweep produced the expected VTC. The circuit gives a HIGH output for a LOW input and a LOW output for a HIGH input.

## 10. Precautions

- Check the PMOS and NMOS source connections.
- Ensure both gates are connected to `IN`.
- Ensure both drains are connected to `OUT`.
- Use the correct source name in the `.dc` command.
- Use a DC source, not a pulse source, for VTC analysis.
- Do not duplicate the graphical circuit using a text netlist.
