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
read_liberty -lib ./lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog ./verilog_files/opt_check.v  
synth -top opt_check  
opt_clean -purge  
abc -liberty ./lib/sky130_fd_sc_hd__tt_025C_1v80.lib  
show
```
<img width="601" height="165" alt="opt_check" src="https://github.com/user-attachments/assets/b3249f52-6daa-4c0e-8b43-8be8d7b1ac1b" />

```bash
read_verilog ./verilog_files/opt_check2.v  
synth -top opt_check2  
opt_clean -purge  
abc -liberty ./lib/sky130_fd_sc_hd__tt_025C_1v80.lib  
show
```
<img width="599" height="153" alt="opt_check2" src="https://github.com/user-attachments/assets/39b92d2c-0c1d-4b12-8a96-a3e373fa8846" />

```bash
read_verilog ./verilog_files/opt_check3.v  
synth -top opt_check3  
opt_clean -purge  
abc -liberty ./lib/sky130_fd_sc_hd__tt_025C_1v80.lib  
show
```
<img width="594" height="216" alt="opt_check3" src="https://github.com/user-attachments/assets/843e2726-c0ab-4f93-9f2e-4c06fa2b2c2e" />

```bash
read_verilog ./verilog_files/opt_check4.v  
synth -top opt_check4  
opt_clean -purge  
abc -liberty ./lib/sky130_fd_sc_hd__tt_025C_1v80.lib  
show
```
<img width="598" height="202" alt="opt_check4" src="https://github.com/user-attachments/assets/d99f5177-1ae6-4d90-8bf4-65afbcd65f62" />

```bash
read_verilog ./verilog_files/multiple_module_opt.v  
synth -top multiple_module_opt  
flatten  
opt_clean -purge  
abc -liberty ./lib/sky130_fd_sc_hd__tt_025C_1v80.lib  
show
```
<img width="594" height="305" alt="multiple_module" src="https://github.com/user-attachments/assets/27315123-f358-4638-bb03-4637d5d6bb51" />

---

## Session 3 - Sequential Logic Optimizations  

### Simulation of Constant Flops  
```bash
iverilog dff_const1.v tb_dff_const1.v  
./a.out  
gtkwave tb_dff_const1.vcd
```  
<img width="1360" height="335" alt="tb_dff_const1" src="https://github.com/user-attachments/assets/7adc6d28-ca06-484f-a5bb-cd4226e67d38" />

```bash
iverilog dff_const2.v tb_dff_const2.v  
./a.out  
gtkwave tb_dff_const2.vcd  
```
<img width="1365" height="332" alt="tb_dff_const2" src="https://github.com/user-attachments/assets/f3da816a-cb35-4dfe-8946-464eee6046ad" />

```bash
iverilog dff_const3.v tb_dff_const3.v  
./a.out  
gtkwave tb_dff_const3.vcd  
```
<img width="1364" height="345" alt="tb_dff_const3" src="https://github.com/user-attachments/assets/00cfde13-8eee-4978-94bb-18ca75c37508" />

```bash
iverilog dff_const4.v tb_dff_const4.v  
./a.out  
gtkwave tb_dff_const4.vcd  
```
<img width="1366" height="344" alt="tb_dff_const4" src="https://github.com/user-attachments/assets/0e59b360-f31d-49bc-81e8-eaa5a43fb59c" />

```bash
iverilog dff_const5.v tb_dff_const5.v  
./a.out  
gtkwave tb_dff_const5.vcd  
```
<img width="1366" height="351" alt="tb_dff_const5" src="https://github.com/user-attachments/assets/24f2c6bf-8a69-412a-b519-3819a4298f91" />

### Synthesis of Constant Flops  
```bash
yosys  
read_liberty -lib ./lib/sky130_fd_sc_hd__tt_025C_1v80.lib  
read_verilog dff_const1.v  
synth -top dff_const1  
dfflibmap -liberty ./lib/sky130_fd_sc_hd__tt_025C_1v80.lib  
abc -liberty ./lib/sky130_fd_sc_hd__tt_025C_1v80.lib  
show  
```
<img width="596" height="119" alt="dff_const1" src="https://github.com/user-attachments/assets/beb50794-65db-418e-9fd8-f3e4bf049749" />

```bash
read_verilog dff_const2.v  
synth -top dff_const2  
abc -liberty ./lib/sky130_fd_sc_hd__tt_025C_1v80.lib  
show  
```
<img width="502" height="412" alt="dff_const2" src="https://github.com/user-attachments/assets/b8b10fe6-2cd1-44b4-859e-1621b2aeeb26" />

```bash
read_verilog dff_const3.v  
synth -top dff_const3  
abc -liberty ./lib/sky130_fd_sc_hd__tt_025C_1v80.lib  
show  
```
<img width="592" height="169" alt="dff_const3" src="https://github.com/user-attachments/assets/f729e0ea-10ae-4b5d-b0f1-04969080eab1" />

```bash
read_verilog dff_const4.v  
synth -top dff_const4  
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib  
show  
```
<img width="496" height="500" alt="dff_const4" src="https://github.com/user-attachments/assets/cf4920e7-d8ac-43fb-af44-ca2d7808b99b" />

```bash
read_verilog dff_const5.v  
synth -top dff_const5  
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib  
show  
```
<img width="597" height="175" alt="dff_const5" src="https://github.com/user-attachments/assets/b95a0bda-6ae9-4a6d-a606-241e15e1fea3" />
---

## Session 4 - Sequential Optimization for Unused Ports  

### Unused Output Optimization  
```bash
yosys  
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib  
read_verilog counter_opt.v  
synth -top counter_opt  
dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib  
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib  
show  
```
<img width="1122" height="162" alt="counter_opt" src="https://github.com/user-attachments/assets/35923bd1-45b7-4aca-871a-aad3daf2fff6" />

 **Observation:** Synthesized into a **T-flop** using a single DFF since unused bits were optimized away.  

### Example 2 - Modified Design with More Utilized Bits  
```bash
cp counter_opt.v counter_opt2.v  
gedit counter_opt2.v   # Modify to use all 3 bits  

yosys  
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib  
read_verilog counter_opt2.v  
synth -top counter_opt2  
dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib  
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib  
show  
```
<img width="1356" height="258" alt="counter_opt2" src="https://github.com/user-attachments/assets/e1543c68-b18f-43b9-9708-44227503a3b9" />

 **Observation:** Outputs we don’t use are optimized away by synthesis. Only the essential logic remains.But here as we use all the ouptut bits , no optimizations , complete counter is formed with 3 dff and counter logic.  

---

#  Summary  

- **Combinational optimization** → Removes redundant gates, propagates constants, simplifies Boolean logic.  
- **Sequential optimization** → Removes constant flops, unused FSM states, retimes registers.  
- **Unused outputs** → Any logic not contributing to final outputs gets eliminated.  
- **Takeaway:** Synthesis tools are *smart optimizers*. Always check post-synthesis netlists to verify what got optimized.  
