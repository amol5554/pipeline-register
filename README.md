# Single-Stage Pipeline Register (Valid/Ready Handshake)

## Overview
This repository implements a **single-stage pipeline register** in **SystemVerilog** using a standard **valid/ready handshake protocol**.  
The design sits between an upstream producer and downstream consumer and safely transfers data while handling **backpressure** without data loss or duplication.

---

## Design Features
- One-deep pipeline register (single-cycle storage)
- Standard valid/ready flow control
- Correct backpressure handling
- No data loss or duplication
- Fully synthesizable RTL
- Clean reset to empty state

---

## Handshake Behavior
- Data is accepted when `in_valid && in_ready`
- Data is presented on the output with `out_valid`
- The module stalls the upstream interface when the output is not ready
- Stored data remains stable during downstream backpressure

The ready logic is:
```verilog
in_ready = ~out_valid || out_ready;
```



## Waveform

The waveform below demonstrates correct operation of the single-stage pipeline register,
including normal data transfer and backpressure handling.

![Pipeline Register Waveform](Simulation Results/waveform.png)

### Waveform Explanation
- During reset, `out_valid` is deasserted, indicating an empty pipeline
- When `in_valid` and `in_ready` are high, data is captured into the pipeline register
- The data appears at the output with `out_valid` asserted after one clock cycle
- When `out_ready` is deasserted, the pipeline holds `out_data` stable
- `in_ready` deasserts during backpressure to prevent data overwrite
- Once backpressure is released, the stored data transfers correctly without loss or duplication


---
