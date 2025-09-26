# RTL Design and Synthesis Workshop Documentation

## Table of Contents
- [Introduction to Digital Simulation](#introduction-to-digital-simulation)
- [Icarus Verilog & Testbenches](#icarus-verilog--testbenches)
- [Logic Synthesis with Yosys](#logic-synthesis-with-yosys)
- [Workshop Labs](#workshop-labs)

---

## Introduction to Digital Simulation

### What is a Simulator?
A **simulator** is a software tool that mimics the behavior of digital hardware before actual physical implementation. It serves as a virtual testing environment where we can verify our designs without the need for expensive fabrication.

**Simulation Process:**
- **Input:** Design files (`.v`) + Testbench files (`.v`)
- **Output:** Value Change Dump file (`.vcd`) containing waveform data
- **Purpose:** Functional verification and debugging

---

## Icarus Verilog & Testbenches

### Understanding Design vs Testbench

#### Design
- The **design** represents the actual digital circuit we want to implement
- Contains the functional logic with defined inputs and outputs
- Example: An AND gate module with inputs `a`, `b` and output `y`

#### Testbench
- A **testbench (TB)** is a simulation environment used to verify the design
- Unlike designs, testbenches have **no primary inputs/outputs**
- Key responsibilities:
  - Apply **stimulus** (test vectors) to the design under test (DUT)
  - Capture and record output behavior
  - Provide comprehensive test coverage

**Analogy:** Design = Circuit, Testbench = Tester

### Simulation Mechanics
The simulator operates on an **event-driven** model:
1. Continuously monitors for **input signal changes**
2. When inputs change → recalculates and updates outputs
3. When inputs are stable → outputs remain constant
4. Records all signal transitions in the VCD file

### Icarus Verilog Workflow
```
Design.v + Testbench.v → [iverilog] → a.out → [execute] → waveform.vcd → [GTKWave] → Analysis
```

1. **Compile:** Use `iverilog` to compile design and testbench
2. **Execute:** Run the compiled output to generate VCD file
3. **Visualize:** Open VCD file in GTKWave for waveform analysis
4. **Verify:** Analyze waveforms to confirm correct functionality
   
---

## Logic Synthesis with Yosys

### What is Logic Synthesis?
**Logic Synthesis** is the process of converting high-level RTL (Register Transfer Level) description into a technology-mapped gate-level netlist using a specific standard cell library.

### Synthesis Flow
```
RTL Design + Standard Cell Library → [Yosys Synthesizer] → Gate-level Netlist
```

**Key Components:**
- **Input:** RTL design (`.v` file)
- **Library:** Technology library (`.lib` file) - contains standard cells
- **Output:** Synthesized netlist (`.v` file with gate-level implementation)

### Standard Cell Library Characteristics

#### Why Different Cell Variants Exist?
Standard cell libraries contain multiple variants of the same logic function with different drive strengths:

- **Fast (ff) Cells:** 
  - Wider transistors for higher current drive
  - Lower propagation delay
  - Higher power consumption and area
  - Used to meet setup time requirements

- **Slow (ss) Cells:**
  - Narrower transistors for lower current drive  
  - Higher propagation delay
  - Lower power consumption and area
  - Used to meet hold time requirements

### Post-Synthesis Verification
After synthesis, it's crucial to perform **post-synthesis simulation** to ensure:
- Functional equivalence between RTL and gate-level netlist
- Timing requirements are met
- No synthesis-induced bugs

---

## Workshop Labs

### Lab Setup
**Repository:** [Sky130 RTL Design and Synthesis Workshop](https://github.com/kunalg123/sky130RTLDesignAndSynthesisWorkshop)

```bash
# Clone the workshop repository
git clone https://github.com/kunalg123/sky130RTLDesignAndSynthesisWorkshop.git
cd sky130RTLDesignAndSynthesisWorkshop
```

### Lab 1: Simulation with Icarus Verilog

#### Objective
Simulate and verify the `good_mux.v` module using Icarus Verilog and GTKWave.

#### Steps
```bash
# Navigate to verilog files directory
cd verilog_files

# Compile design and testbench
iverilog good_mux.v tb_good_mux.v

# Execute simulation
./a.out

# View waveforms
gtkwave tb_good_mux.vcd
```

#### Expected Results
- Successful compilation without errors
- Generation of VCD file
- Waveform verification in GTKWave showing correct multiplexer behavior

<img width="1362" height="737" alt="gtk_wave_output" src="https://github.com/user-attachments/assets/9f4d330d-7ace-4910-bda2-13fe54419f1e" />

### Lab 2: Logic Synthesis with Yosys

#### Objective
Synthesize the `good_mux.v` design using Yosys synthesizer with Sky130 technology library.

#### Synthesis Commands
```bash
# Launch Yosys
yosys

# Read the technology library
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

# Read the RTL design
read_verilog good_mux.v

# Perform synthesis
synth -top good_mux

# Technology mapping
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

# Display schematic
show

# Write synthesized netlist
write_verilog -noattr good_mux_netlist.v
```

#### Key Yosys Commands Explained
- `read_liberty`: Loads the standard cell library
- `read_verilog`: Loads the RTL design
- `synth -top`: Performs synthesis with specified top module
- `abc`: Performs technology mapping using ABC tool
- `show`: Displays the synthesized schematic
- `write_verilog -noattr`: Writes netlist without attributes

<img width="724" height="351" alt="schematic" src="https://github.com/user-attachments/assets/9142ac97-7b23-4d87-a480-bf53a6294067" />

---

