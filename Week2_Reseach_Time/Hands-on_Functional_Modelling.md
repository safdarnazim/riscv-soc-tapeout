# Week 2 Task – BabySoC Fundamentals & Functional Modelling

## Objective
To build a solid understanding of SoC fundamentals and practice functional modelling of the BabySoC using simulation tools (Icarus Verilog & GTKWave).

---

## Part 1 – Setup
1. Cloned the BabySoC project repository.  
2. Created folder structure and made necessary edits.  
3. Installed tools:
   - **Icarus Verilog (iverilog)** for simulation.
   - **GTKWave** for waveform analysis.
## Project Directory Structure

```text
.
├── compiled_tlv
├── lib
│   ├── avsddac.lib
│   ├── avsdpll.lib
│   └── sky130_fd_sc_hd__tt_025C_1v80.lib
├── output
│   ├── post_synth_sim
│   │   ├── post_synth_sim.out
│   │   └── post_synth_sim.vcd
│   ├── pre_synth_sim
│   │   ├── pre_synth_sim.out
│   │   └── pre_synth_sim.vcd
│   └── synth
│       └── vsdbabysoc.synth.v
├── src
│   ├── include
│   │   ├── sandpiper.vh
│   │   ├── sandpiper_gen.vh
│   │   ├── sp_default.vh
│   │   └── sp_verilog.vh
│   ├── module
│   │   ├── avsddac.v
│   │   ├── avsdpll.v
│   │   ├── clk_gate.v
│   │   ├── mythcore_test.v
│   │   ├── mythcore_test_gen.v
│   │   ├── primitives.v
│   │   ├── pseudo_rand.sv
│   │   ├── pseudo_rand_gen.sv
│   │   ├── rvmyth.tlv
│   │   ├── rvmyth.v
│   │   ├── sandpiper.vh
│   │   ├── sandpiper_gen.vh
│   │   ├── sky130_fd_sc_hd.v
│   │   ├── sp_default.vh
│   │   ├── sp_verilog.vh
│   │   ├── tb_mythcore_test.v
│   │   ├── testbench.rvmyth.post-routing.v
│   │   ├── testbench.v
│   │   └── vsdbabysoc.v
│   ├── primitives.v
│   └── sky130_fd_sc_hd.v
└── yosys.ys
```
---

## Part 2 – Pre-Synthesis Functional Simulation

### Compilation
```bash
iverilog -o output/pre_synth_sim/pre_synth_sim.out -DPRE_SYNTH_SIM \
    -I src/include -I src/module \
    src/module/testbench.v src/module/vsdbabysoc.v
````

### Running Simulation

```bash
cd output/pre_synth_sim
./pre_synth_sim.out
```

### Generated Files

* `pre_synth_sim.vcd` – waveform dump file
* Simulation log (see below)

---

## Simulation Logs

```text
The simulation executed successfully without console logs. The results were captured in the pre_synth_sim.vcd file and analyzed using GTKWave.
```
<img width="1366" height="551" alt="Screenshot 2025-10-03 213903" src="https://github.com/user-attachments/assets/587c809a-f61b-41d2-99a0-5f2d63db05fa" />

---

## Part 3 – Waveform Analysis (GTKWave)

### Testbench / Simulation setup (assumptions)

* DUT: `uut` (BabySoC).
* Testbench: `vsdbabysoc_tb`.
* Observed VCD capture spans 0 → ~85 µs in the screenshot.
* Clock (`CLK`) is the simulation reference clock.
* `reset` is active-low (observed low in the capture → SoC released from reset).
* `RV_TO_DAC[9:0]` is the 10-bit digital bus intended for an external DAC (MSB = bit9, LSB = bit0).
* Additional signals present relate to a PLL block (ENb_CP, ENb_VCO, VCO_IN, REF, VREFH).

---

### High-level summary / verdict

The BabySoC successfully generates a periodic 10-bit counter pattern on RV_TO_DAC[9:0], creating a ramp waveform. The OUT signal functions as a threshold comparator, toggling high when the DAC output exceeds VREFH. Binary weighting is evident (LSBs toggle fastest). No reset is active during steady-state operation. The system demonstrates successful integration of RISC-V core, PLL, and DAC subsystems.

---

### Signals 

#### `CLK`

* Stable periodic clock, driving sequential logic.
* No jitter observed in the displayed window.

#### `reset`

* Held low (system not in reset) for the displayed interval — SoC is in normal operation mode.

#### `OUT`

* OUT is actively toggling between 0 and 1 throughout the waveform
* It shows a periodic pulse pattern synchronized with the RV_TO_DAC bus
* OUT appears to be a comparator output that goes high when the DAC value exceeds a reference threshold (VREFH)

#### `RV_TO_DAC[9:0]` (10-bit bus)

* At simulation start the bus is `XXX` (unknown) — expected during initialization or before valid writes occur.
* After initialization the bus assumes valid logic values and exhibits periodic pulses across the 10 bits.
* Pattern characteristics:

  * Lower bits (0..3) toggle at highest frequency (shortest periods).
  * Middle bits (4..6) toggle at intermediate rates.
  * Higher bits (7..9) toggle slowest and produce the gross envelope of the waveform.
* When the bits are viewed as weighted contributions to a DAC, the combined bus forms a **staircase waveform** with a smooth periodic envelope — visually similar to a sampled sine or amplitude-modulated ramp.
* This indicates a CPU/software routine or hardware module is writing sequential amplitude values to the DAC register (e.g., a lookup table or counter feeding a DAC).

#### PLL-related signals (`ENb_CP`, `ENb_VCO`, `REF`, `VCO_IN`, `VREFH`)

---

## Observations & Conclusion

## Observation

**System Behavior:**
- The RISC-V core is generating a repeating 10-bit counter sequence on the `RV_TO_DAC[9:0]` bus, incrementing from 0 to 1023 (full 10-bit range)
- Each complete cycle takes approximately 30-40 µs, indicating the core is executing a tight loop at a consistent rate
- The **OUT** signal functions as a digital comparator output, transitioning to logic '1' when the DAC's analog output crosses above the reference voltage (VREFH), and back to '0' when it falls below
- Binary weighting is clearly visible: LSBs (bits 0-3) toggle at the highest frequency, while MSBs (bits 7-9) toggle at progressively slower rates, confirming proper bit-width operation
- No reset activity is observed during the captured window, indicating stable steady-state operation
- The PLL-derived clock (CLK) maintains stable periodicity with no visible jitter

---

## Conclusion

The BabySoC demonstrates **successful end-to-end integration** of all three subsystems:

1. **RISC-V Core (RVMyth)**: Executing program code correctly, generating sequential data, and interfacing with peripherals
2. **PLL**: Providing stable clock generation for synchronous operation
3. **DAC + Comparator**: Converting digital values to analog and performing threshold detection

---
