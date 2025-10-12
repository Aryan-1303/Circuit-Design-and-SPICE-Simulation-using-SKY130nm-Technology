# Circuit-Design-and-SPICE-Simulation-using-SKY130nm-Technology

## Table Of Contents

- [NgspiceSky130-Day1-Basics of NMOS Drain Current(Id) vs Drain-to-source Voltage(Vds)](#ngspicesky130-day1-basics-of-nmos-drain-currentid-vs-drain-to-source-voltagevds)
  - [Introduction to Circuit Design and Spice Simulations](#introduction-to-circuit-design-and-spice-simulations)

  - [NMOS resistive region and Saturation region of operation](#nmos-resistive-region-and-saturation-region-of-operation)

  - [Introduction to SPICE](#introduction-to-spice)


- [NgspiceSky130-Day2-Velocity saturation and basics of CMOS inverter VTC](#ngspicesky130-day2-velocity-saturation-and-basics-of-cmos-inverter-vtc)
  - [SPICE simulation for lower nodes and velocity saturation effect](#spice-simulation-for-lower-nodes-and-velocity-saturation-effect)

  - [CMOS voltage transfer characteristics (VTC)](#cmos-voltage-transfer-characteristics-vtc)


- [NgspiceSky130-Day3-CMOS switching threshold and dynamic simulations](#ngspicesky130-day3-cmos-switching-threshold-and-dynamic-simulations)

  - [Static behaviour evaluation-CMOS inverter robustness-Switching Threshold](#static-behaviour-evaluation-cmos-inverter-robustness-switching-threshold)


- [NgspiceSky130-Day4-CMOS Noise Margin robustness evaluation](#ngspicesky130-day4-cmos-noise-margin-robustness-evaluation)
  - [Static behaviour evaluation-CMOS inverter robustness-Noise Margin](#static-behaviour-evaluation-cmos-inverter-robustness-noise-margin)


- [NgspiceSky130-Day5-CMOS power supply and device variation robustness evaluation](#ngspicesky130-day5-cmos-power-supply-and-device-variation-robustness-evaluation)
  - [Static behaviour evaluation-CMOS inverter robustness-Power supply variation](#static-behaviour-evaluation-cmos-inverter-robustness-power-supply-variation)

  - [Static behaviour evaluation-CMOS inverter robustness-Device variation](#static-behaviour-evaluation-cmos-inverter-robustness-device-variation)

# NgspiceSky130 – Day 1  
## **Basics of NMOS Drain Current (Id) vs Drain-to-Source Voltage (Vds)**  

### Introduction to Circuit Design and SPICE Simulations  

Circuit design involves connecting **PMOS** and **NMOS** transistors in specific configurations to form logic gates such as **NAND**, **NOR**, **OR**, and **AND**.  

 **SPICE simulations** help us analyze transistor behavior, determine delay, and extract meaningful design parameters like the **W/L ratio**.

---

### **Why Do We Need SPICE Simulations?**

- Circuit design involves connecting **PMOS and NMOS transistors** in required configurations to achieve desired logic functions such as **NAND, NOR, OR, AND**, etc.  

- **SPICE (Simulation Program with Integrated Circuit Emphasis)** is used to simulate these transistor-level circuits by feeding **input waveforms** and observing **output characteristics** like voltage, current, and delay.

- **Delay** in a circuit depends primarily on the **W/L ratio (width/length)** of transistors:  
  - W/L controls **drive current**  
  - Drive current controls **charging/discharging rate of capacitors**  
  - Charging/discharging rate controls **waveform transition**  
  - Waveform transition defines **propagation delay**

- To **select the appropriate W/L ratio** for a required delay or performance, SPICE simulations are used to analyze and tune circuit behavior.

- SPICE simulations help **generate delay tables**:  
  - For different **buffers, gates, and cells**, simulations are performed under varying **input slews** and **output loads**.  
  - The **delay value** corresponding to each (input slew, output load) pair is recorded.  
  - These values form the **cell delay tables** used later in **Static Timing Analysis (STA)** and **physical design tools**.

- These tables serve as the **foundation for timing libraries (.lib files)**, which are essential for:  
  - **Logic synthesis**  
  - **STA verification**  
  - **Placement and routing optimization**

- SPICE simulations also **validate transistor-level models**, ensuring that the **theoretical models match real circuit behavior** and that **STA-predicted delays** align with actual silicon performance.

---
 
- Accurate SPICE simulations are critical for **timing, reliability**, and **design optimization**.

---






