# Day 4 - GLS and Synthesis-Simulation Mismatch  

---

## Session 1 - GLS, Blocking and Non-Blocking Statements

### Gate-Level Simulation (GLS)
- **GLS** = Gate-Level Simulation.  
- Purpose:
  - Simulate and verify the **technology-mapped gate-level netlist** obtained after synthesis.  
  - Ensure **logical correctness** and **timing requirements** are met (run with delay annotation).  
- **GLS Flow:**  
```

Design + Gate-Level Verilog Models + Testbench --> iverilog --> .vcd --> GTKWave

````
- If gate-level models are **delay-annotated**, we can perform **timing GLS**.

### Synthesis-Simulation Mismatch
- **Causes of mismatch:**
  - **Missing sensitivity list** → simulator does not see input changes.  
  - **Blocking vs Non-blocking assignments**
    - **Blocking (`=`)** → Executes sequentially in the order written.  
    - **Non-blocking (`<=`)** → RHS evaluated first, LHS updated simultaneously at the end (parallel behavior).  
  - **Non-standard Verilog coding** → may not synthesize as intended.

---

## Session 2 - Lab: GLS and Synthesis Simulation Mismatch

### 1. Ternary Operator MUX
```bash
# Simulate RTL
iverilog ternary_operator_mux.v tb_ternary_operator_mux.v
./a.out
gtkwave tb_ternary_operator_mux.vcd

# Synthesize using Yosys
yosys
read_liberty -lib ./lib/sky130
read_verilog ternary_operator_mux.v
synth -top ternary_operator_mux
abc -liberty ./lib/sky130
write_verilog -noattr ternary_operator_mux_net.v
show

# GLS of synthesized netlist
iverilog ./verilog_models/primitives.v ./verilog_model/sky130_fd_sc_hd.v ternary_operator_mux_net.v tb_ternary_operator_mux.v
./a.out
gtkwave tb_ternary_operator_mux.vcd
````

---

### 2. Bad MUX Example

```bash
# Simulate RTL
iverilog bad_mux.v tb_bad_mux.v
./a.out
gtkwave tb_bad_mux.vcd

# Synthesize using Yosys
yosys
read_liberty -lib ./lib/sky130
read_verilog bad_mux.v
synth -top bad_mux
abc -liberty ./lib/sky130
write_verilog -noattr bad_mux_net.v
show

# GLS of synthesized netlist
iverilog ./verilog_models/primitives.v ./verilog_model/sky130_fd_sc_hd.v bad_mux_net.v tb_bad_mux.v
./a.out
gtkwave tb_bad_mux.vcd
```
---

### 3. Blocking Caveat Example

```bash
# Simulate RTL
iverilog blocking_caveat.v tb_blocking_caveat.v
./a.out
gtkwave tb_blocking_caveat.vcd

# Synthesize using Yosys
yosys
read_liberty -lib ./lib/sky130
read_verilog blocking_caveat.v
synth -top blocking_caveat
abc -liberty ./lib/sky130
write_verilog -noattr blocking_caveat_net.v
show

# GLS of synthesized netlist
iverilog ./verilog_models/primitives.v ./verilog_model/sky130_fd_sc_hd.v blocking_caveat_net.v tb_blocking_caveat.v
./a.out
gtkwave tb_blocking_caveat.vcd
```

---

## Session 3 - Lab: Synthesis-Simulation Mismatch for Blocking Statements

**Theory:**

* Blocking assignments (`=`) in sequential logic may cause **simulation-synthesis mismatches**.
* Non-blocking assignments (`<=`) mimic actual hardware behavior for registers.
* Best practice: Use **non-blocking assignments** in sequential `always` blocks.
* Verify using **GLS** that the synthesized netlist behaves identically to the RTL.

**Simulation and GLS workflow:**

```
RTL Design + Testbench --> Simulation (iverilog + GTKWave)
Synthesized Netlist + Testbench --> GLS (iverilog + GTKWave)
Compare waveforms to verify correctness
```
---

#  Summary

* GLS ensures **gate-level netlist correctness** and timing verification.
* Mismatches can arise from:

  * Missing sensitivity lists.
  * Improper blocking/non-blocking usage.
  * Non-standard Verilog coding practices.
* Always compare **RTL simulation** and **GLS** to catch issues early.
---
