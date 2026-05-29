# Moore FSM - Sequence Detector (1010)

A Moore Finite State Machine implemented in Verilog that detects the sequence **1010** in a serial input stream. Designed and simulated using EDA Playground, with netlist synthesis and visualization done via Yosys and netlistsvg.

---

## What It Does

The FSM reads one bit at a time on each clock edge. When the input sequence `1010` is detected, the output goes high (`out = 1`). In a Moore machine, the output depends only on the current state, not the input.

---

## States

| State | Meaning                        | Output |
|-------|--------------------------------|--------|
| A     | Reset / No progress            | 0      |
| B     | Received `1`                   | 0      |
| C     | Received `10`                  | 0      |
| D     | Received `101`                 | 0      |
| E     | Received `1010` (sequence hit) | 1      |

---

## Files

| File          | Description                          |
|---------------|--------------------------------------|
| `moore.v`     | Moore FSM and testbench              |
| `dump.vcd`    | Waveform output from simulation      |
| `moore.json`  | Synthesized netlist in JSON format   |
| `moore.svg`   | Netlist schematic as SVG image       |

---

## Tools Used

- **EDA Playground** - Online Verilog simulation environment
- **Yosys** - Open-source synthesis tool (via EDA Playground terminal)
- **netlistsvg** - Converts Yosys JSON netlist to SVG schematic

---

## Simulation

The testbench applies the following input sequence after reset:

```
1 0 1 0 0 1 1 0 0
```

The output goes high when the FSM reaches state E after detecting `1010`.

The testbench uses `$monitor` to print state transitions and `$dumpvars` to generate a `.vcd` waveform file viewable in GTKWave or EDA Playground's built-in viewer.

---

## Netlist Generation (Yosys + netlistsvg)

Run the following commands in the EDA Playground Yosys terminal to synthesize the design and generate a schematic:

```bash
cd ~/Desktop
ls
yosys
```

Inside Yosys:

```
read_verilog moore.v
prep -top moore
write_json moore.json
exit
```

Then generate and open the SVG:

```bash
netlistsvg moore.json -o moore.svg
open moore.svg
```

This produces a gate-level schematic of the synthesized Moore FSM.

---

## How to Run on EDA Playground

1. Open [EDA Playground](https://www.edaplayground.com)
2. Paste the `moore` module and `tb` module into the editor
3. Select a simulator (e.g., Icarus Verilog)
4. Enable EPWave to view the waveform
5. Click **Run**

---

## Sample Output

```
Time = 0  Seq_In = 0 || Output = 0
Time = 20 Seq_In = 1 || Output = 0
Time = 40 Seq_In = 0 || Output = 0
Time = 50 Seq_In = 1 || Output = 0
Time = 60 Seq_In = 0 || Output = 0
Time = 65 Seq_In = 0 || Output = 1
Time = 70 Seq_In = 1 || Output = 1
Time = 75 Seq_In = 1 || Output = 0
Time = 80 Seq_In = 0 || Output = 0
Time = 85 Seq_In = 0 || Output = 1
Time = 95 Seq_In = 0 || Output = 0
```

Output goes high at Time = 65 and Time = 85 when the FSM enters state E after detecting `1010`.
