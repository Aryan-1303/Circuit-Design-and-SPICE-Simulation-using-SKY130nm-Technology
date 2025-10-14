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

- Accurate SPICE simulations are critical for **timing, reliability**, and **design optimization**.

---
### **Fundamentals of NMOS Transistor Structure and Operation**

The **NMOS transistor** is a 4-terminal device built on a **p-type substrate**. Its structure and terminals are fundamental to its operation:

**Structure and Components:**
* **Substrate:** p-type silicon body.
* **Source (S) and Drain (D):** Formed by **n+ diffusion regions** embedded in the p-substrate.
* **Gate (G):** A **poly-Si or metal layer** placed above a **thin gate oxide** (SiO₂). The gate terminal **drives the NMOS**.
* **Isolation:** **SiO₂ isolation regions** (e.g., shallow trench isolation) are used to differentiate between adjacent transistors.
* **Body/Bulk (B):** The p-type substrate itself.

**Four Terminals:**
1.  **G** (Gate)
2.  **S** (Source)
3.  **D** (Drain)
4.  **B** (Body/Bulk) – Typically grounded, but any applied potential ($V_{sb}$) can **tune the threshold voltage** (Body Effect).

**PMOS** has an inverted structure compared to NMOS (built on an n-substrate with p+ Source/Drain regions).

---

### **NMOS Operation: From Cut-off to Strong Inversion**

The operation of the NMOS transistor is centered on creating a conductive channel between the Source and Drain:

1.  **Initial Condition (Cut-off):**
    * $V_{gs} = 0$, Drain, Source, and Bulk are grounded.
    * The **p-n junction diodes** formed by Substrate-Source (B-S) and Substrate-Drain (B-D) are **OFF** due to 0V bias.
    * The resistance between Source and Drain is very high, and the transistor is **OFF**.

2.  **Applying Positive $V_{gs}$:**
    * The **Gate terminal** acts like a positive capacitor plate.
    * This positive charge **repels positive charges** (holes) from the p-substrate surface and **attracts negative charges** (electrons) towards the $\text{Si-SiO}_2$ interface.
    * A **depletion region** (devoid of carriers) forms under the gate. As $V_{gs}$ increases, the depletion width increases.

3.  **Strong Inversion and Threshold Voltage ($V_t$):**
    * As $V_{gs}$ continues to increase, the top portion of the p-substrate surface inverts to behave like an **N-type channel**. This condition is called **strong inversion**.
    * The **Threshold Voltage ($V_t$)** is the **gate-to-source voltage ($V_{gs}$)** at which strong inversion occurs.

4.  **Conduction:**
    * A further increase in $V_{gs}$ beyond $V_t$ strengthens the channel.
    * Electrons are drawn from the n+ **Source** region into the continuous **n-channel** formed between Source and Drain.
    * The **channel conductivity is modulated by $V_{gs}$**, facilitating the **cut-off to conduction transition**.

---

### **The Body Effect**

The **Body Effect** describes how the potential of the bulk/body terminal ($V_b$) affects the threshold voltage.

* **Case 1: $V_{sb} = 0$** $\to$ Normal inversion and the minimum $V_t$.
* **Case 2: $V_{sb} > 0$** (Substrate is positively biased relative to the Source):
    * The p-n junction between the Source and Substrate is **further reverse-biased**.
    * This **increases the depletion region width** near the Source.
    * A greater gate field is required to attract the necessary charges to achieve strong inversion.
    * Result: **Inversion occurs later**, meaning a **higher $V_{gs}$ (increased $V_t$) is required** compared to Case 1.

---
### Threshold Voltage ($V_t$) and Fermi Potential ($\Phi_f$)

The Threshold Voltage is influenced by the **Fermi Potential ($\Phi_f$)**, which is a function of the substrate material properties:

$$\Phi_f = \frac{kT}{q} \ln \left( \frac{N_A}{n_i} \right)$$

Where:
* $k$ = Boltzmann constant
* $T$ = Absolute temperature (K)
* $q$ = Elementary charge
* $N_A$ = Doping concentration (Acceptor concentration for p-type substrate)
* $n_i$ = Intrinsic carrier concentration of silicon
---

## **Part 2: NMOS Resistive region and Saturation region of operation**

The operation of the NMOS transistor, once the channel is formed ($\boldsymbol{V_{GS} > V_T}$), is governed by the drain-to-source voltage ($\boldsymbol{V_{DS}}$), defining the **Linear** and **Saturation** regions.

### **Gate Oxide Capacitance and Induced Charge**

When $V_{GS} > V_T$, a channel forms, and the induced charge density $Q_i(x)$ depends on the effective gate-to-channel voltage:
$$Q_i(x) = -C_{ox}[V_{GS} - V_{T} - V(x)]$$
Where $V(x)$ is the channel potential at distance $x$ from the source.

The **Gate Oxide Capacitance** per unit area is defined as:
$$C_{ox} = \frac{\epsilon_{ox}}{t_{ox}}$$
* $\epsilon_{ox} = 3.97\epsilon_0 \approx 3.51 \times 10^{-11} \text{ F/m}$ (Permittivity of $\text{SiO}_2$)
* $t_{ox}$ = Oxide thickness.

### **Current Mechanism**

MOSFET current is primarily **drift current** ($\boldsymbol{I_D}$), caused by the potential difference ($V_{DS}$) accelerating carriers (electrons) through the channel. Diffusion current (due to carrier concentration gradient) is generally ignored in first-order models.

The drift current is derived by integrating the carrier velocity and induced charge along the channel length ($L$):
$$I_D = \mu_n C_{ox} \frac{W}{L} \left[ (V_{GS} - V_{T})V_{DS} - \frac{1}{2}V_{DS}^2 \right]$$

This equation can be simplified using the **process transconductance** ($\boldsymbol{k'_n}$) and the **gain factor** ($\boldsymbol{k_n}$):
* $k'_n = \mu_n C_{ox}$ (Process Transconductance Parameter)
* $k_n = k'_n \frac{W}{L}$ (Gain Factor, depending on device geometry)

$$I_D = k_n \left[ (V_{GS} - V_{T})V_{DS} - \frac{1}{2}V_{DS}^2 \right]$$

### **Region Conditions and Drain Current Equations**

The relationship between $V_{DS}$ and the effective voltage $\boldsymbol{(V_{GS} - V_{T})}$ determines the operating region:

#### 1. Resistive / Linear Region
* **Condition:** $V_{DS} < (V_{GS} - V_{T})$
* **Channel State:** The channel is continuous and strong across the entire length.
* **Current:** The quadratic term ($V_{DS}^2/2$) is small, and the current is approximately linear with $V_{DS}$:
$$\boldsymbol{I_D \approx k_n (V_{GS} - V_{T})V_{DS}}$$

#### 2. Saturation (Pinch-off) Region
* **Condition:** $V_{DS} \ge (V_{GS} - V_{T})$
* **Pinch-off:** The effective gate-to-channel voltage near the drain, $V_{GS} - V_{DS}$, drops to $V_T$ or below. The channel effectively **vanishes (pinches off)** near the drain.
* **Current:** Once saturation is reached, the drain current becomes relatively constant (independent of $V_{DS}$ in ideal models):
$$\boldsymbol{I_{Dsat} = \frac{1}{2} k_n (V_{GS} - V_{T})^2}$$

### **Channel Length Modulation ($\boldsymbol{\lambda}$)**

In reality, the drain current does not stay perfectly flat in the saturation region. As $V_{DS}$ increases above the pinch-off voltage, the pinch-off point moves closer to the source, **decreasing the effective channel length ($L$)**.

This physical effect, known as **Channel Length Modulation (CLM)**, is modeled by multiplying the saturation current equation by the factor $\boldsymbol{(1 + \lambda V_{DS})}$:

$$I_D = \frac{1}{2} k_n (V_{GS} - V_{T})^2 (1 + \lambda V_{DS})$$
* **$\lambda$** = Channel-length modulation parameter (units: $\text{V}^{-1}$).
* **Impact:** A **larger $\lambda$** means the drain current has a **stronger dependence on $V_{DS}$** in saturation.
    * For **long-channel** devices, $\lambda \approx 0$ (current is flat).
    * For **short-channel** devices (common in modern technology nodes), $\lambda$ is significant.
---
## **Part 3: Introduction to SPICE and Device Simulation**

SPICE (Simulation Program with Integrated Circuit Emphasis) is a crucial tool used to simulate the transistor-level circuits, deriving practical device characteristics and calculating delays. SPICE simulations replace complex manual calculations for modern MOSFET models.

### **SPICE Setup and Simulation Flow**

The SPICE process provides a systematic approach from writing netlists to analyzing results. It involves three main steps: **Netlist**, **Model Definition**, and **Simulation Commands**.

#### 1. SPICE Netlist: Circuit Description

The netlist is a text file that describes the circuit topology, components, and connections.

* **Node Definition:** Terminals in the circuit (Source, Drain, Gate, Substrate, VDD, etc.) are assigned numerical or alphanumeric names (nodes).
* **Transistor Syntax:** For a MOSFET (e.g., NMOS $M_1$), the standard syntax is:
    `MX name Node_D Node_G Node_S Node_B Model_Name W=... L=...`
    The order of terminals is typically **Drain, Gate, Source, Substrate (DGSS)**.

#### 2. Model Parameters and Technology Inclusion

Device behavior is defined by **model parameters** ($V_T$, $\gamma$, $\mu_n$, etc.), which are determined by the foundry's manufacturing process (technology).

* These parameters are packaged within a technology file (often a `.lib` or `.mod` file).
* The top-level netlist includes this packaged file using the **`.lib`** command.

**Process Corners:**
The fundamental device parameters, specifically the **Threshold Voltage ($\boldsymbol{V_t}$)** and the **Drive Current ($\boldsymbol{I_D}$)**, are highly sensitive to manufacturing variations. The process corners simulated represent this range:

* **tt (Typical):** Nominal or expected performance.
* **ff (Fast):** Often associated with **lower $V_t$** and **higher drive current**.
* **ss (Slow):** Often associated with **higher $V_t$** and **lower drive current**.

When including the library, a process corner is specified to account for these manufacturing variations:

` .lib "path/to/library/file.spice" tt`

#### 3. Simulation Commands

Commands like `.op`, `.dc`, and `.tran` define the type of analysis to be performed on the circuit.

* **`.op`**: Calculates the **DC operating point** (static voltages and currents).
* **`.dc`**: Performs a **DC sweep**, varying one or two voltage/current sources over a range (e.g., sweeping `Vds` and `Vgs` to generate `Id` vs `Vds` curves).
* **`.tran`**: Performs a **transient analysis** (time-domain simulation) to see how voltages and currents change over time, which is used for **delay calculation**.

### **SPICE Simulation Flow Summary**

The overall systematic approach is:

Write Netlist -> Include Model Libraries -> Set Process Corner -> Run Simulation Commands -> Analyze Waveforms & Results

The resulting SPICE waveforms are used to calculate the **delay** of a cell, providing results that are very close to practical silicon delays.
---

## Lab Activity – Day 1

The day's lab focused on observing the practical characteristics of the NMOS transistor using **SPICE simulation** with the following key setup and observations:

### **Simulation Setup and Execution**

* **Process Corner:** The **tt (typical corner)** was initially used for simulation, representing nominal device performance.
* **Analysis:** The **`.dc` analysis** command was used to sweep both the drain voltage ($V_{ds}$) and the gate voltage ($V_{gs}$) to observe the NMOS drain current ($I_D$) behavior across the linear and saturation regions.
* **Process Variation:** It was noted that **process corner variation** ($tt \to ff \to ss$) significantly changes fundamental device parameters, namely **drive current** and **Threshold Voltage ($V_t$)**.

### **Output Observation and Data Extraction**

* **Practical Characteristics:** The simulation curves provided a direct observation of the practical transistor characteristics ($I_D$ vs $V_{DS}$ curves for different $V_{GS}$ values).
* **Node Definition:** Nodes in SPICE are defined by the connection points specified in the netlist.
* **Data Reading in GUI:** To check the value of the drain current ($I_D$) at any specific point on the curve:
    * **Left-click** on the desired point in the SPICE output GUI (Figure 3).
    * The terminal output will display the coordinates $x_0$ and $y_0$.
    * The **$y_0$ value** corresponds directly to the **drain current ($I_D$)** in Amperes at that specific operating point.
# NgspiceSky130 – Day 2
## **Velocity saturation and basics of CMOS inverter VTC**

On the second day of the workshop, we explored advanced concepts of MOSFET device physics and their impact on circuit behavior. Through SPICE simulations, we observed the characteristics of long-channel and short-channel devices, focusing on the effect of velocity saturation at different electric fields. In addition, the operation of MOSFETs as switches and the fundamentals of CMOS inverters were introduced, including their voltage transfer characteristics (VTC).

---

### **Part 1: SPICE Simulation for Lower Nodes and Velocity Saturation Effect**

This section explores how scaling down the transistor dimensions (lower nodes) affects the $\boldsymbol{I_D}$ vs $\boldsymbol{V_{DS}}$ characteristics, specifically due to the **Velocity Saturation effect**.

#### **1. Long Channel vs. Short Channel Operation Modes**

The regions of operation change with channel length, requiring a unified model that incorporates the **Velocity Saturation** regime for modern devices. We define the effective gate voltage as $\boldsymbol{V_{GT} = V_{GS} - V_T}$.

| Operation Modes | Long Channel ($>250\text{nm}$) | Short Channel ($<250\text{nm}$) |
| :--- | :---: | :---: |
| **Cutoff** | Cutoff | Cutoff |
| **Resistive (Linear)** | Resistive | Resistive |
| **Velocity Saturation** | - | Velocity Saturation |
| **Saturation** | Saturation | Saturation |

* **Cutoff Mode:** $\boldsymbol{I_D = 0}$ for $V_{GT} < 0$.

#### **2. Unified Drain Current Model for Short Channels**

A single model can be used to describe the current in the Resistive, Velocity Saturation, and Saturation modes by defining a minimum voltage term ($\boldsymbol{V_{\min}}$). This model inherently accounts for velocity saturation through the technology parameter $\boldsymbol{V_{dsat}}$ (Saturation voltage).

The general drain current equation for the conducting modes is:

$$I_D = k_n \cdot \left[ (V_{GT} \cdot V_{\min}) - \frac{V_{\min}^2}{2} \right] \cdot [1 + \lambda V_{DS}]$$

Where:
* $k_n = \mu_n C_{ox} (W/L)$ (Gain factor)
* $V_{GT} = V_{GS} - V_T$
* $V_{dsat}$ is a **Technology Parameter** (Saturation voltage where device velocity saturates, independent of $V_{GS}$ or $V_{DS}$).

The term $\boldsymbol{V_{\min}}$ is the minimum of the three voltages, which dictates the effective voltage drop across the conducting part of the channel:

$$\boldsymbol{V_{\min} = \min(V_{GT}, V_{DS}, V_{dsat})}$$

**Example using $V_{\min}$:**
* **Resistive Mode:** If $V_{DS}$ is small (i.e., $V_{\min} \approx V_{DS}$), the equation simplifies toward the classic quadratic linear current equation.
* **Velocity Saturation Mode:** If $V_{dsat}$ is the minimum value (i.e., $V_{\min} = V_{dsat}$), the equation models the saturated current limit:
    $$I_D = k_n \cdot \left[ (V_{GT} \cdot V_{dsat}) - \frac{V_{dsat}^2}{2} \right] \cdot [1 + \lambda V_{DS}]$$
* **Saturation Mode (Ideal Long Channel):** If $V_{GT}$ is the minimum value (i.e., $V_{\min} = V_{GT}$), and we ignore $\lambda$ and $V_{dsat}$, the equation simplifies toward the ideal quadratic saturation current equation.

#### **3. Velocity Saturation Mechanism**

As channel lengths decrease (becoming **short channel** devices), the electric field ($E$) in the channel increases significantly, leading to the **Velocity Saturation effect**.

* **Relationship:** Velocity ($v$) and electric field ($E$) are related by the equation $v = \mu E$.
* **Saturation:** Carrier velocity increases linearly with $E$ only up to a certain critical electric field ($\mathcal{E}_c$). Beyond $\mathcal{E}_c$, the velocity ceases to increase linearly and **saturates** due to **scattering effects**, causing the carrier mobility ($\mu$) to decrease.
* **Impact:** Velocity saturation tends to limit the peak current early. Consequently, the **saturation current for lower nodes is often lower** than predicted by the long-channel model.

---

### **Part 2: Day 2 Lab Activities – SPICE Simulation for Short-Channel Devices**

This lab focused on using SPICE to characterize a short-channel NMOS device (Sky130 $\boldsymbol{L=0.15\mu}$, $\boldsymbol{W=0.39\mu}$) to demonstrate velocity saturation and extract the threshold voltage ($V_T$).

#### **1. Lab 2A: $\boldsymbol{I_{DS}}$ vs $\boldsymbol{V_{DS}}$ Sweep (Velocity Saturation)**

**Purpose:** To generate the characteristic $I_{DS}$ vs $V_{DS}$ curves for various gate voltages and observe the transition from quadratic to linear behavior due to velocity saturation.

### **Lab 2B: I_DS vs V_GS Sweep (Threshold Voltage Extraction)**

**Purpose:** To calculate the **Threshold Voltage (V_T)** of a short-channel NMOS transistor (L=0.15u, W=0.39u).

**Procedure (SPICE Netlist):**
This code holds the drain voltage (V_DS) constant at 1.8V (forcing saturation) and sweeps the gate voltage (V_GS) to obtain the I_DS vs V_GS curve.

### **CMOS Inverter & Voltage Transfer Characteristic (VTC)**

The final part of Day 2 introduces the fundamental building block of digital logic: the CMOS inverter, and analyzes its **Voltage Transfer Characteristic (VTC)**.

#### **1. MOSFET as a Switch**

A MOSFET acts as a voltage-controlled switch:
* **OFF State (Open Circuit):** Occurs when $|\boldsymbol{V_{GS}}| < |\boldsymbol{V_T}|$.
* **ON State (Closed Circuit):** Occurs when $|\boldsymbol{V_{GS}}| > |\boldsymbol{V_T}|$.

#### **2. CMOS Inverter Operation**

The CMOS inverter uses an NMOS (pull-down) and a PMOS (pull-up) transistor to ensure one is always ON while the other is OFF in the static state:

| Case | Input ($V_{IN}$) | PMOS (Pull-up) | NMOS (Pull-down) | Output ($V_{OUT}$) |
| :--- | :---: | :---: | :---: | :---: |
| **Case 1** | $V_{DD}$ (High) | OFF | ON (Closed) | $\approx 0\text{V}$ |
| **Case 2** | $0\text{V}$ (Low) | ON (Closed) | OFF | $\approx V_{DD}$ |

**Voltage Relationships:**
* $V_{gsN} = V_{IN}$
* $V_{gsP} = V_{IN} - V_{DD}$
* $V_{dsN} = V_{OUT}$
* $V_{dsP} = V_{OUT} - V_{DD}$
* $I_{dsP} = -I_{dsN}$

#### **3. Five Regions of the CMOS VTC**

The VTC plot shows $V_{OUT}$ vs $V_{IN}$. It is derived by finding the operating points where the NMOS load curve and the PMOS load curve intersect ($|I_{dsN}| = |I_{dsP}|$).

| Region | Input ($V_{IN}$) | NMOS Mode | PMOS Mode | Output ($V_{OUT}$) |
| :--- | :---: | :---: | :---: | :---: |
| **1** | $V_{IN} \approx 0$ | Cut-Off (OFF) | Linear | $V_{OUT} = V_{DD}$ |
| **2** | Low | Saturation | Linear | High |
| **3 (Transition)**| $V_{IN} \approx V_M$ | Saturation | Saturation | Switching |
| **4** | High | Linear | Saturation | Low |
| **5** | $V_{IN} \approx V_{DD}$ | Linear | Cut-Off (OFF) | $V_{OUT} = 0\text{V}$ |

***

### **Explanation of CMOS Inverter Operation and VTC Regions**

* **Regions 1 & 5 (Static States):** These are the stable output states where static power consumption is minimized because only one transistor is **ON**.
    * **Region 1** ($V_{IN} \approx 0$): PMOS is strongly **ON (Linear)**, pulling $V_{OUT}$ to $V_{DD}$. NMOS is **OFF**.
    * **Region 5** ($V_{IN} \approx V_{DD}$): NMOS is strongly **ON (Linear)**, pulling $V_{OUT}$ to $0\text{V}$. PMOS is **OFF**.

* **Regions 2 & 4 (Transition Sides):** These are the high-current transition periods.
    * In **Region 2**, NMOS is in **Saturation** (max current), actively pulling down, while PMOS is still in the **Linear** region (resistive).
    * In **Region 4**, PMOS is in **Saturation** (max current), actively pulling up, while NMOS is in the **Linear** region.

* **Region 3 (Switching Threshold – $\boldsymbol{V_M}$):** This is the high-gain region where the output switches rapidly.
    * At this point, the input voltage is $\boldsymbol{V_M}$.
    * **Both NMOS and PMOS are simultaneously in the Saturation region** ($\boldsymbol{I_{DSN} = -I_{DSP}}$).
    * The steep slope confirms the **robust logic levels** and excellent **noise margin** of the CMOS inverter.

# NgspiceSky130 – Day 3
## **CMOS Switching Threshold and Dynamic Simulations**

Day 3 focuses on the **CMOS Inverter** characterization, covering static behavior (VTC and Switching Threshold $V_M$) and dynamic behavior (Rise and Fall Delay) through SPICE simulation and analytical modeling.

---

### **Voltage Transfer Characteristics – SPICE Simulations**

The **SPICE Deck** is the crucial input file for simulating the inverter's static response.

#### **1. SPICE Deck Creation for CMOS Inverter (VTC)**

The circuit defines a CMOS inverter with an output load capacitor ($C_{load}$), driven by $V_{DD}$ and swept by $V_{IN}$.

**Netlist Breakdown for VTC Simulation (DC Sweep):**

```spice
* MODEL Descriptions - Load technology file
.LIB "path/to/models.mod" CMOS_MODELS

* Voltage Sources (Example: VDD = 2.5V)
Vdd vdd 0 2.5
Vin in 0 2.5

* Netlist Description (DGSS syntax: Drain, Gate, Source, Substrate)
* M1: PMOS (Example Sizing)
M1 out in vdd vdd pmos W=0.375u L=0.25u
* M2: NMOS
M2 out in 0 0 nmos W=0.375u L=0.25u
* Load Capacitor
Cload out 0 10f

* Simulation Commands
.op
* DC Sweep: Vin sweeps from 0V to 2.5V in 0.05V steps
.dc Vin 0 2.5 0.05
.end
```

VTC Simulation:
The .dc command sweeps the input voltage ($\boldsymbol{V_{IN}}$) to generate the Voltage Transfer Characteristic (VTC) plot ($\boldsymbol{V_{OUT}}$ vs $\boldsymbol{V_{IN}}$).

**2. Effect of Transistor Sizing on VTC (Lab Observations)**

Simulating inverters with different $\boldsymbol{W/L}$ ratios confirms the robustness of the CMOS structure.
The following was observed in the lab when varying PMOS width:

**Observation 1 (Robustness):**
The fundamental shape of the VTC remains the same, demonstrating the inverter's robustness against fabrication variations in size.

**Observation 2 (Shifting $V_M$):**
Increasing the PMOS width makes the PMOS stronger, causing the $\boldsymbol{V_M}$ (Switching Threshold) to shift toward $V_{DD}$ (higher $V_{IN}$).

---

### Static Behavior Evaluation – CMOS Inverter Robustness – Switching Threshold ($\boldsymbol{V_M}$)

**1. Definition of Switching Threshold ($\boldsymbol{V_M}$)**

The switching threshold $V_M$ is the input voltage where the inverter switches, defined by the condition:

```
V_{IN} = V_{OUT} = V_M
```

**Critical State:**  
At $V_M$, both NMOS and PMOS are simultaneously in the Saturation region ($\boldsymbol{I_{DSN} = -I_{DSP}}$).

**2. Analytical Expression and Design**

$V_M$ is analytically determined by equating the saturation currents:

```
V_M = f\left(\frac{W}{L}_p, \frac{W}{L}_n, \mu_n, \mu_p, V_{TN}, V_{TP}\right)
```

**Designing for Symmetry:**  
To achieve ideal Symmetry ($V_M \approx V_{DD}/2$), the $\boldsymbol{PMOS}$ must be made wider than the $\boldsymbol{NMOS}$ to compensate for the lower mobility of holes ($\mu_p$).

---

### Transient Analysis and Dynamic Simulation

Transient Analysis (.tran) is a time-domain simulation used to find the dynamic characteristics of the inverter, specifically its delay times.

**1. SPICE Deck for Transient Analysis**

The input source ($V_{IN}$) is defined as a PULSE function to simulate a switching event.

```spice
* Vdd is a static source
Vdd vdd 0 1.8V
* Vin is a transient pulse (Example: 0V to 1.8V, Trise/Tfall = 0.1ns)
Vin in 0 PULSE(0 1.8 0n 0.1n 0.1n 2n 4n) 

* ... (Netlist remains the same) ...

* Simulation Commands
.tran 0.01n 10n * Run simulation for 10ns with a 0.01n step
.end
```

**2. Delay Measurement and Calculation**

Delays are measured at the 50% transition point (e.g., $0.9\text{V}$).

- **Rise Delay ($\boldsymbol{t_{r}}$):**  
  Time required to charge the output capacitor ($C_{LOAD}$), primarily determined by PMOS width.

  ```
  Rise Delay = t_{output,50\%}^{rising} - t_{input,50\%}^{falling}
  ```

  **Lab Value:**  
  $\text{Rise Delay} = \text{[FILL IN RISE END TIME]} - \text{[FILL IN RISE START TIME]} = \text{[FILL IN RISE DELAY] ns}$

- **Fall Delay ($\boldsymbol{t_{f}}$):**  
  Time required to discharge the output capacitor ($C_{LOAD}$), primarily determined by NMOS width.

  ```
  Fall Delay = t_{output,50\%}^{falling} - t_{input,50\%}^{rising}
  ```

  **Lab Value:**  
  $\text{Fall Delay} = \text{[FILL IN FALL END TIME]} - \text{[FILL IN FALL START TIME]} = \text{[FILL IN FALL DELAY] ns}$

**3. Key Conclusion: Delay Symmetry**

Symmetry is Achieved when the $\boldsymbol{W_P/L_P}$ ratio is tuned such that Rise Delay ≈ Fall Delay.

This balanced characteristic is critical for Clock Inverters/Buffers and is a fundamental requirement in Static Timing Analysis (STA).

# NgspiceSky130 – Day 4
## CMOS Noise Margin Robustness Evaluation

Day 4 focuses on evaluating the static robustness of the CMOS inverter, primarily through the calculation and analysis of Noise Margins ($NM_H$ and $NM_L$).

---

### Static Behaviour Evaluation – CMOS Inverter Robustness – Noise Margin

#### 1. Introduction to Noise Margin

Noise Margin is a measure of how much unwanted electrical noise a logic circuit can tolerate on its input without causing an incorrect output state. It is a critical metric for the reliability of digital systems.

While an ideal inverter has an infinite slope, practical inverters have a finite slope due to parasitics, which necessitates defining precise voltage boundaries.

#### 2. Noise Margin Voltage Parameters

The VTC curve defines four critical voltage points that demarcate valid logic regions and noise tolerance:

| Parameter | Definition | Logic State |
|-----------|------------|-------------|
| $V_{OH}$  | Output High Voltage: Minimum output voltage guaranteed to be a logic HIGH (ideally $V_{DD}$). | Valid Logic '1' |
| $V_{OL}$  | Output Low Voltage: Maximum output voltage guaranteed to be a logic LOW (ideally 0V). | Valid Logic '0' |
| $V_{IH}$  | Input High Voltage: Minimum input voltage recognized as a logic HIGH. | Start of Transition |
| $V_{IL}$  | Input Low Voltage: Maximum input voltage recognized as a logic LOW. | End of Transition |

**Critical Points:** $V_{IL}$ and $V_{IH}$ are typically defined as the points on the VTC where the slope ($\frac{dV_{OUT}}{dV_{IN}}$) equals $-1$.

#### 3. Noise Margin Equations

Noise margin is the buffer zone between the output voltage of one gate and the valid input voltage required by the next gate.

- **Noise Margin High ($NM_H$):** The noise tolerance when the input is meant to be a logic '1'.

  $$
  NM_H = V_{OH} - V_{IH}
  $$

- **Noise Margin Low ($NM_L$):** The noise tolerance when the input is meant to be a logic '0'.

  $$
  NM_L = V_{IL} - V_{OL}
  $$

**Conclusion:**  
A larger noise margin implies a more robust CMOS inverter that is more immune to noise disturbances. Any signal outside these margins is considered "Undefined."

---

#### 4. SPICE Simulation for Noise Margin Calculation (Lab Activity)

The VTC curve is generated using a DC sweep, and the critical points ($V_{IL}$, $V_{IH}$) are extracted where the slope is $-1$.

**SPICE Netlist for Noise Margin Simulation:**
```spice
* Model Description
.param temp=27

* Including sky130 library files
.lib "sky130_fd_pr/models/sky130.lib.spice" tt

* Netlist Description (Example W/L ratio used for noise margin testing)
* XM1 (PMOS) W=1u, L=0.15u
XM1 out in vdd vdd sky130_fd_pr__pfet_01v8 w=1u l=0.15u
* XM2 (NMOS) W=0.36u, L=0.15u
XM2 out in 0 0 sky130_fd_pr__nfet_01v8 w=0.36u l=0.15u

Cload out 0 50fF

Vdd vdd 0 1.8V
Vin in 0 1.8V

* Simulation Commands
.op
* DC Sweep Vin from 0 to 1.8V with a fine step of 0.01V
.dc Vin 0 1.8 0.01

.control
run
setplot dc1
display
.endc

.end
```

---

#### 5. Noise Margin Calculation (Lab Results)

After running the simulation and extracting the critical points ($V_{OH}$, $V_{OL}$, $V_{IH}$, $V_{IL}$):

| Parameter | Extracted Value |
|-----------|----------------|
| $V_{OH}$  | [FILL IN V_{OH} VALUE] V |
| $V_{OL}$  | [FILL IN V_{OL} VALUE] V |
| $V_{IH}$  | [FILL IN V_{IH} VALUE] V |
| $V_{IL}$  | [FILL IN V_{IL} VALUE] V |

- **Noise Margin High ($NM_H$):**

  $$
  NM_H = V_{OH} - V_{IH} = \text{[CALCULATED NM_H VALUE]} \text{ V}
  $$

- **Noise Margin Low ($NM_L$):**

  $$
  NM_L = V_{IL} - V_{OL} = \text{[CALCULATED NM_L VALUE]} \text{ V}
  $$

---

#### 6. Conclusion on Robustness

**CMOS Robustness:**  
The CMOS inverter is highly robust to noise. As demonstrated in earlier labs, even when PMOS sizing is significantly increased, the noise margins tend to remain static or change only minimally, preserving the integrity of the digital signal.

**Balance:**  
When $NM_H \approx NM_L$ (achieved through proper PMOS/NMOS sizing, typically $W_P/L_P \approx 2-3 \times W_N/L_N$), the inverter exhibits optimal balance and noise immunity.
# NgspiceSky130 – Day 5
## CMOS Power Supply and Device Variation Robustness Evaluation

Day 5 addresses two critical sources of non-ideality in manufacturing and operation: the impact of scaling the supply voltage ($V_{DD}$) and the effect of process-induced device variations on CMOS inverter robustness.

---

### Static Behavior Evaluation – CMOS Inverter Robustness: Power Supply Variation

As technology scales (e.g., from 250nm to 20nm), the supply voltage is reduced (e.g., 1.8V→0.7V) to reduce power consumption. This trade-off significantly impacts the VTC.

#### 1. Supply Voltage Scaling Trade-offs

| Scaling Effect | Advantages (Low $V_{DD}$, e.g., 0.5V) | Disadvantages (Low $V_{DD}$) |
|----------------|--------------------------------------|-------------------------------|
| Gain           | Gain increases (e.g., ∼50% increase observed). | Transition slope becomes gentler. |
| Power          | Energy consumption reduces significantly (∼90% reduction). | Performance suffers: Rising/falling edges are slower. |
| Robustness     | - | Noise Margins ($NM_H$, $NM_L$) shrink, reducing robustness. |

#### 2. SPICE Simulation for Supply Variation (Lab Code)

A smart SPICE simulation uses a dowhile loop in the `.control` block to sweep the supply voltage ($V_{DD}$) across several values for a single netlist definition.

**SPICE Netlist for $V_{DD}$ Sweep:**
```spice
* Model Description
.param temp=27

* Including sky130 library files
.lib "sky130_fd_pr/models/sky130.lib.spice" tt

* Netlist Description
XM1 out in vdd vdd sky130_fd_pr__pfet_01v8 w=1u l=0.15u
XM2 out in 0 0 sky130_fd_pr__nfet_01v8 w=0.36u l=0.15u

Cload out 0 50fF

* Initial Supply Voltage (Altered in loop)
Vdd vdd 0 1.8V
Vin in 0 1.8V

.control
let powersupply = 1.8
alter Vdd = powersupply
        let voltagesupplyvariation = 0
        dowhile voltagesupplyvariation < 6
        dc Vin 0 1.8 0.01
        let powersupply = powersupply - 0.2
        alter Vdd = powersupply
        let voltagesupplyvariation = voltagesupplyvariation + 1
      end

plot dc1.out vs in dc2.out vs in dc3.out vs in dc4.out vs in dc5.out vs in dc6.out vs in xlabel "input voltage(V)" ylabel "output voltage(V)" title "Inveter dc characteristics as a function of supply voltage"

.endc

.end
```
*Figure 21: Snapshot of output window to observe power supply variations*

#### 3. Inverter Gain Calculation (Lab Procedure)

Inverter gain ($g$) is calculated near the switching point ($V_{M}$), where the slope is steepest:

$$
\text{Gain (g)} = \frac{\Delta V_{IN}}{\Delta V_{OUT}}
$$

**Observation:**  
As $V_{DD}$ decreases, the VTC transition becomes smoother, and the overall robustness diminishes, showing the trade-off between power efficiency and circuit stability.

---

### Static Behavior Evaluation – CMOS Inverter Robustness: Device Variation

In real fabrication, device parameters are never perfectly ideal due to manufacturing processes.

#### 1. Sources of Device Variation

- **Etching Process Variation:** Errors in the photolithography and etching steps lead to variations in the drawn Width (W) and Length (L) of the transistor gate. Since $I_D \propto W/L$, this directly impacts drive current and delay.
- **Oxide Thickness Variation ($t_{ox}$):** Small variations in the gate oxide thickness during fabrication significantly affect the gate capacitance ($C_{ox} = \epsilon_{ox} / t_{ox}$), which in turn alters the Threshold Voltage ($V_T$) and drive current.

#### 2. Imbalanced Device Strengths

Device variations or intentional non-symmetrical sizing create imbalances that shift the VTC:

| Device Imbalance             | $V_M$ Shift                 | Performance Effect                     | Robustness Effect                  |
|------------------------------|-----------------------------|----------------------------------------|------------------------------------|
| Strong NMOS / Weak PMOS      | Shifts Downward (towards 0V) | Output falls faster than it rises.     | $NM_H$ reduces; $NM_L$ improves.   |
| Strong PMOS / Weak NMOS      | Shifts Upward (towards $V_{DD}$) | Output rises faster than it falls.     | $NM_L$ reduces; $NM_H$ improves.   |

#### 3. SPICE Simulation for Device Variation (Lab Code)

The following simulation tests the Strong PMOS / Weak NMOS case by setting a large $W_P/W_N$ ratio (e.g., 7/0.42≈16.7).

**SPICE Netlist for Device Variation Sweep:**
```spice
* Model Description
.param temp=27

* Including sky130 library files
.lib "sky130_fd_pr/models/sky130.lib.spice" tt

* Netlist Description (Strong PMOS: W=7u, Weak NMOS: W=0.42u)
XM1 out in vdd vdd sky130_fd_pr__pfet_01v8 w=7u l=0.15u
XM2 out in 0 0 sky130_fd_pr__nfet_01v8 w=0.42u l=0.15u

Cload out 0 50fF

Vdd vdd 0 1.8V
Vin in 0 1.8V

* Simulation Commands
.op
.dc Vin 0 1.8 0.01

.control
run
setplot dc1
display
.endc

.end
```
*Figure 22: Snapshot of output window to observe device variations*

#### 4. Switching Threshold Observation (Lab Result)

The VTC plot for the strong PMOS case shifts right. By finding the point where $V_{IN} \approx V_{OUT}$ (zooming in the GUI):

**Example Result:**  
$V_M \approx$ [FILL IN Vm VALUE] V.

---

### Conclusion

The shifting $V_M$ confirms the strength imbalance, where the stronger device (PMOS) requires the input to be closer to $V_{DD}$ to be overcome by the weaker device (NMOS).
