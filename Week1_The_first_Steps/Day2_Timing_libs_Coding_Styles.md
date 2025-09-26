# Sky130 RTL Design and Synthesis Workshop – Notes  

---

## Session 1 – Introduction to Timing and .lib Files  

In this session, we explored the **timing library (.lib)** files of the Sky130nm PDK.  

### Key Learnings  
- Understood the **.lib file structure** for Sky130nm PDK.  
- Learned about **PVT corners**:  
  - **P**rocess variations  
  - **V**oltage variations  
  - **T**emperature variations  
- Explored **standard cell naming conventions** and how names indicate functionality.  
- Identified key cell parameters:  
  - **Leakage power**  
  - **Cell size**  
  - **Delay**  

---

## Session 2 – Hierarchical vs Flattened Synthesis  

We studied the differences between **hierarchical synthesis** and **flattened synthesis**, and why **sub-module synthesis** is important.  

### Key Learnings  
- **Hierarchical Synthesis**:  
  - Maintains sub-module boundaries.  
  - Easier to debug and reuse.  
- **Flattened Synthesis**:  
  - Breaks down hierarchy into a single-level netlist.  
  - May result in **better optimization** but harder debugging.  

### Visualization  

**Hierarchical Design:**  
<img width="605" height="555" alt="Hierarchical Netlist" src="https://github.com/user-attachments/assets/44a42903-7486-4264-84bf-529700934ce9" />  

**Flattened Design:**  
<img width="604" height="311" alt="Flattened Netlist" src="https://github.com/user-attachments/assets/7c379219-4505-4782-8623-aa7b87f28c1e" />  

---

## Session 3 – Various Flip-Flop Coding Styles and Optimizations  

We explored **flip-flop implementations** in Verilog and their impact on synthesis.  

### Key Learnings  
- Understood **why flip-flops are used** → to handle **glitches** and synchronize signals.  
- Compared different coding styles:  
  - **Asynchronous Reset**  
  - **Asynchronous Set**  
  - **Synchronous Reset**  
- Simulated and synthesized these coding styles to observe behavior differences.  

---

## Summary  

- `.lib` files provide **timing, power, and functional information** of standard cells.  
- **Hierarchical vs Flattened synthesis** affects **readability, optimization, and debugging**.  
- **Flip-flop styles** must be chosen carefully depending on **reset/set requirements** and **glitch handling**.  

---
