# Week 2 – BabySoC Fundamentals & Functional Modelling

## 1. What is a System-on-Chip (SoC)?

A **System-on-Chip (SoC)** is an integrated circuit that consolidates multiple essential computing system components into a single chip.  
It enables:
- **High performance:** Shorter communication paths and parallelism.
- **Low power consumption:** Reduced off-chip data transfers.
- **Compact design:** Saves board space.
- **Cost efficiency:** Cheaper for mass production.

SoCs are widely used in smartphones, IoT devices, embedded systems, and high-performance computing.

---

## 2. Components of a Typical SoC

A typical SoC integrates:

- **CPU (Central Processing Unit):** Executes instructions; can be simple or multi-core.
- **Memory (SRAM/DRAM/Flash):** Stores instructions and data; internal memory is faster, external memory provides larger storage.
- **Peripherals:** Interfaces like UART, GPIO, I2C, SPI, timers, ADC/DAC for interaction with external devices.
- **Interconnect/Bus System:** Connects CPU, memory, and peripherals. Examples include AMBA (AXI, AHB, APB).

---

## 3. Why BabySoC is a Simplified Model

The **BabySoC** is an educational, simplified SoC model that focuses on **core concepts**:

- Contains only basic components: CPU, memory, minimal peripherals, and lightweight interconnect.
- Enables learners to **trace signals, instructions, and bus transactions** easily.
- Serves as a practical introduction to SoC design without overwhelming complexity.

---

## 4. Role of Functional Modelling

Functional modelling is used to **validate architecture and interactions** before detailed RTL and physical design:

- Describes **component behavior and interaction** without gate-level or timing details.
- Supports **early verification**: CPU instruction fetch, memory response, peripheral communication.
- Uses **simulation tools** like Icarus Verilog (logic simulation) and GTKWave (waveform viewing).
- Helps catch errors early, saving time during RTL and physical design stages.

---

## 5. How BabySoC Fits into the Learning Journey

- Introduces **fundamental SoC concepts** in a simplified, manageable environment.
- Allows learners to simulate and understand CPU-memory interaction, bus transactions, and instruction execution.
- Builds foundation for advanced stages: RTL design, synthesis, physical design, and tapeout.
- Mirrors real-world SoC design flow in an **educational, hands-on approach**.

---

## 6. BabySoC Block Diagram (Simplified)

```bash
   +------------------+
   |       CPU        |
   +------------------+
            |
   +------------------+
   |    Interconnect  |
   +------------------+
    |       |       |
+--------+ +--------+ +--------+
| Memory | | UART   | | GPIO   |
+--------+ +--------+ +--------+

```
---

