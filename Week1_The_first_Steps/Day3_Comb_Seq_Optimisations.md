# Day 3 - Logic Optimizations in Synthesis  

---

## Session 1 - Introduction to Optimizations  

### Combinational Logic Optimization  
- **Squeezing logic** → Simplifying redundant gates.  
- **Constant propagation** → If inputs are constant, tool removes unnecessary logic.  
- **Boolean logic optimization** → Algebraic simplifications like `A + A'B = A + B`.  

### Sequential Logic Optimization  
- **Sequential constant propagation**  
  - If a flip-flop always drives a constant, synthesis removes it.  
- **State optimization**  
  - Unused FSM states are optimized away.  
- **Retiming**  
  - Moving flip-flops across logic for better timing without changing functionality.  
- **Floorplan aware synthesis (cloning)**  
  - Achieved by *physical synthesis*.  
  - Tool knows the physical layout and duplicates/registers signals near their consumers for performance.  

---

## Session 2 - Combinational Logic Optimization  
```bash
yosys  
read_liberty -lib ./lib/sky130  
read_verilog opt_check.v  
synth -top opt_check  
opt_clean -purge  
abc -liberty ./lib/sky130  
show  

read_verilog opt_check2.v  
synth -top opt_check2  
opt_clean -purge  
abc -liberty ./lib/sky130  
show  

read_verilog opt_check3.v  
synth -top opt_check3  
opt_clean -purge  
abc -liberty ./lib/sky130  
show  

read_verilog opt_check4.v  
synth -top opt_check4  
opt_clean -purge  
abc -liberty ./lib/sky130  
show  

read_verilog multiple_modules_opt.v  
synth -top multiple_modules_opt  
flatten  
opt_clean -purge  
abc -liberty ./lib/sky130  
show  
```
---

## Session 3 - Sequential Logic Optimizations  

### Simulation of Constant Flops  
```bash
iverilog dff_const1.v tb_dff_const1.v  
./a.out  
gtkwave tb_dff_const1.vcd  

iverilog dff_const2.v tb_dff_const2.v  
./a.out  
gtkwave tb_dff_const2.vcd  

iverilog dff_const3.v tb_dff_const3.v  
./a.out  
gtkwave tb_dff_const3.vcd  

iverilog dff_const4.v tb_dff_const4.v  
./a.out  
gtkwave tb_dff_const4.vcd  

iverilog dff_const5.v tb_dff_const5.v  
./a.out  
gtkwave tb_dff_const5.vcd  
```
### Synthesis of Constant Flops  
```bash
yosys  
read_liberty -lib ./lib/sky130  
read_verilog dff_const1.v  
synth -top dff_const1  
dfflibmap -liberty ./lib/sky130  
abc -liberty ./lib/sky130  
show  

read_verilog dff_const2.v  
synth -top dff_const2  
abc -liberty ./lib/sky130  
show  

read_verilog dff_const3.v  
synth -top dff_const3  
abc -liberty ./lib/sky130  
show  

read_verilog dff_const4.v  
synth -top dff_const4  
abc -liberty ./lib/sky130  
show  

read_verilog dff_const5.v  
synth -top dff_const5  
abc -liberty ./lib/sky130  
show  
```
---

## Session 4 - Sequential Optimization for Unused Ports  

### Unused Output Optimization  
```bash
yosys  
read_liberty -lib ./lib/sky130  
read_verilog counter_opt.v  
synth -top counter_opt  
dfflibmap -liberty ./lib/sky130  
abc -liberty ./lib/sky130  
show  
```
 **Observation:** Synthesized into a **T-flop** using a single DFF since unused bits were optimized away.  

### Example 2 - Modified Design with More Utilized Bits  
```bash
cp counter_opt.v counter_opt2.v  
gedit counter_opt2.v   # Modify to use all 3 bits  

yosys  
read_liberty -lib ./lib/sky130  
read_verilog counter_opt2.v  
synth -top counter_opt2  
dfflibmap -liberty ./lib/sky130  
abc -liberty ./lib/sky130  
show  
```
 **Observation:** Outputs we don’t use are optimized away by synthesis. Only the essential logic remains.  

---

#  Summary  

- **Combinational optimization** → Removes redundant gates, propagates constants, simplifies Boolean logic.  
- **Sequential optimization** → Removes constant flops, unused FSM states, retimes registers.  
- **Unused outputs** → Any logic not contributing to final outputs gets eliminated.  
- **Takeaway:** Synthesis tools are *smart optimizers*. Always check post-synthesis netlists to verify what got optimized.  
