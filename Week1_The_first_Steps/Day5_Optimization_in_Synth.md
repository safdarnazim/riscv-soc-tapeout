# Day 5 - If/Case Constructs and Looping in Verilog  

---

## Session 1 - If / Case Constructs

### If Statement
- The **`if` statement** implements **priority logic**.  
- **Danger:** Incomplete or missing assignments may cause **inferred latches**.  
- Always ensure **all outputs are assigned in every path**.  

### Case Statement
- The **`case` statement** allows multiple-choice selection logic.  
- **Dangers with `case`:**
  - **Incomplete case:** Some input combinations not covered → may infer latches.  
    - **Solution:** Always include a `default` case.  
  - **Partial assignment:** Not assigning all outputs in each case branch → may infer latches.  

---

## Session 2 - Lab: Incomplete `if` Statements

### Incomplete IF 1
```bash
# Simulate RTL
iverilog incomp_if.v tb_incomp_if.v
./a.out
gtkwave tb_incomp_if.vcd

# Synthesize using Yosys
yosys
read_liberty -lib ./lib/sky130
read_verilog incomp_if.v
synth -top incomp_if
abc -liberty ./lib/sky130
show
````

### Incomplete IF 2

```bash
# Simulate RTL
iverilog incomp_if2.v tb_incomp_if2.v
./a.out
gtkwave tb_incomp_if2.vcd

# Synthesize using Yosys
yosys
read_liberty -lib ./lib/sky130
read_verilog incomp_if2.v
synth -top incomp_if2
abc -liberty ./lib/sky130
show
```

**Theory:** Always assign all outputs in **every branch of `if`** to avoid unintended latch inference.

---

## Session 3 - Lab: Incomplete / Overlapping Case Statements

### Incomplete Case 1

```bash
iverilog incomp_case.v tb_incomp_case.v
./a.out
gtkwave tb_incomp_case.vcd

yosys
read_liberty -lib ./lib/sky130
read_verilog incomp_case.v
synth -top incomp_case
abc -liberty ./lib/sky130
show
```

### Incomplete Case 2

```bash
iverilog incomp_case2.v tb_incomp_case2.v
./a.out
gtkwave tb_incomp_case2.vcd

yosys
read_liberty -lib ./lib/sky130
read_verilog incomp_case2.v
synth -top incomp_case2
abc -liberty ./lib/sky130
show
```

### Complete Case

```bash
iverilog comp_case.v tb_comp_case.v
./a.out
gtkwave tb_comp_case.vcd

yosys
read_liberty -lib ./lib/sky130
read_verilog comp_case.v
synth -top comp_case
abc -liberty ./lib/sky130
show
```

### Partial Case Assignment

```bash
iverilog partial_case_assign.v tb_partial_case_assign.v
./a.out
gtkwave tb_partial_case_assign.vcd

yosys
read_liberty -lib ./lib/sky130
read_verilog partial_case_assign.v
synth -top partial_case_assign
abc -liberty ./lib/sky130
show
```

### Bad Case Assignment

```bash
iverilog bad_case.v tb_bad_case.v
./a.out
gtkwave tb_bad_case.vcd

yosys
read_liberty -lib ./lib/sky130
read_verilog bad_case.v
synth -top bad_case
abc -liberty ./lib/sky130
show
```

**Theory:** Always include **all outputs assigned in every case branch** and a `default` to avoid latches.

---

## Session 4 - Loops and Generate Constructs

### For Loop

* Used **inside `always` blocks** for repeated **behavioral operations**.
* Evaluates expressions **at runtime**.

### Generate + For Loop

* Used for **hardware instantiation** (structural generation).
* Cannot be used inside `always` blocks.
* Evaluates **during elaboration**, instantiates repeated hardware blocks.

---

## Session 5 - Lab: For / Generate Loops

### MUX using Generate

```bash
iverilog mux_generate.v tb_mux_generate.v
./a.out
gtkwave tb_mux_generate.vcd

yosys
read_liberty -lib ./lib/sky130
read_verilog mux_generate.v
synth -top mux_generate
abc -liberty ./lib/sky130
show
```

### DEMUX using Case

```bash
iverilog demux_case.v tb_demux_case.v
./a.out
gtkwave tb_demux_case.vcd

yosys
read_liberty -lib ./lib/sky130
read_verilog demux_case.v
synth -top demux_case
abc -liberty ./lib/sky130
show
```

### DEMUX using Generate

```bash
iverilog demux_generate.v tb_demux_generate.v
./a.out
gtkwave tb_demux_generate.vcd

yosys
read_liberty -lib ./lib/sky130
read_verilog demux_generate.v
synth -top demux_generate
abc -liberty ./lib/sky130
show
```

### RCA (Ripple Carry Adder) using Full Adders

```bash
iverilog fa.v rca.v tb_rca.v
./a.out
gtkwave tb_rca.vcd

yosys
read_liberty -lib ./lib/sky130
read_verilog rca.v  # fa inside rca
synth -top rca
abc -liberty ./lib/sky130
show
```

**Theory:**

* Behavioral loops (`for`) are used for **testbench or repeated logic inside always blocks**.
* Generate loops are used for **structural replication of hardware modules**.
* Always synthesize and perform GLS to verify correctness.

---

#  Summary - Day 5

1. **If/Case Constructs**:

   * Always assign **all outputs in all paths**.
   * Use `default` in case statements to avoid inferred latches.
2. **Loops in Verilog**:

   * **For loop** → inside always, evaluates expressions, behavioral.
   * **Generate + for** → hardware instantiation, structural.
3. Always **simulate RTL** and **GLS of synthesized netlist** to ensure correctness.

---
