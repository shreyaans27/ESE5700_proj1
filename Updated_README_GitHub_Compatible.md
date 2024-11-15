<div align="center" style="font-size: 18px;">

# **University of Pennsylvania**

## **Department of Electrical and System Engineering**

## **Digital Integrated Circuits AND VLSI Fundamentals**

## **ESE 5700, Fall 2024**

**Group members: Shreyaans Singh – 42835021, Ananya Murali Kashyap – 40590283**

Documentation also on 

## **ESE5700 Project 1: Optimization of Adder**

</div>

---

---

### 1. Baseline Design

#### 1.1 Baseline Design Description, Schematics, and Test Schematics

##### Description

The baseline design is centered around implementing a full adder circuit using complementary static CMOS technology. The design adheres to fundamental logic equations for sum and carry outputs, specifically:

Carry-out Boolean Function = $C_{out} = A \cdot B + B \cdot C_{in} + A \cdot C_{in}$

Sum Boolean Function = $S = A \cdot B \cdot C_{in} + \overline{C_{out}} \cdot (A + B + C_{in}) $

##### Overview

The full adder circuit consists of interconnected logic gates that collectively compute the sum ($ S $) and carry-out ($ C_{out} $) signals. The schematic highlights the use of CMOS logic to create robust pull-up and pull-down networks that ensure reliable logic transitions. The Full Adder circuit utilizes 28 transistors in total.

##### Design Features

1. **Inverter Optimization**: To reduce the number of logic stages in the carry path, the design leverages the inverting property. This helps minimize delays by sharing common inverting nodes and strategically placing inverters where necessary.
2. **PMOS and NMOS Stacking**: The layout incorporates tall PMOS stacks for both carry and sum generation circuits, which impacts capacitance and delay. The circuit uses 28 transistors to achieve this full adder configuration. The NMOS and PMOS transistors are arranged strategically along the critical path.
3. **Placement of Inputs**: The carry-generation circuit design, while slow, uses optimization techniques to reduce delays. By placing the control signal $ C_{in} $ on a smaller PMOS stack and positioning transistors close to the output, the design minimizes logical effort and delay. Pre-charging or discharging internal node capacitances in advance allows only a single node to be charged or discharged when $ C_{in, k} $ arrives, enhancing the circuit's efficiency.

![alt text](images/28t_CMOS/32t_CMOS_schem_baseline.png)

<p align="center" style="font-size:14px; text-decoration: underline;">
1-bit Full adder Schematic  
All transistors are min sized: (W/L)<sub>n,p</sub> = (120 nm/45 nm)
</p>

![alt text](images/28t_CMOS/32t_CMOS_FA_test.png)

<p align="center" style="font-size:14px; text-decoration: underline;">
1-bit Full adder Test Schematic
</p>

##### 1-bit Full Adder Verification

- **Cin** is connected to a Vpulse with:

  - Period = 8 ns
  - Pulse width = 4 ns
  - $ V_1 = 0 \, V $
  - $ V_2 = V_{dd} $
- **B** is connected to a Vpulse with:

  - Period = 16 ns
  - Pulse width = 8 ns
  - $ V_1 = 0 \, V $
  - $ V_2 = V_{dd} $
- **A** is connected to a Vpulse with:

  - Period = 32 ns
  - Pulse width = 16 ns
  - $ V_1 = 0 \, V $
  - $ V_2 = V_{dd} $
- **Rise time** and **Fall time** = 1 ps, **delay** = 0 s

  - Additional details about Vpulse
- **$ V_{dd} $** = Vdc = 1.1 V

##### Truth Table for 1-bit Full Adder

| A | B | Cin | Sum | Cout |
| - | - | --- | --- | ---- |
| 0 | 0 | 0   | 0   | 0    |
| 0 | 0 | 1   | 1   | 0    |
| 0 | 1 | 0   | 1   | 0    |
| 0 | 1 | 1   | 0   | 1    |
| 1 | 0 | 0   | 1   | 0    |
| 1 | 0 | 1   | 0   | 1    |
| 1 | 1 | 0   | 0   | 1    |
| 1 | 1 | 1   | 1   | 1    |

![alt text](images/28t_CMOS/32t_FA_verif_plot.bmp)

<p align="center" style="font-size:14px; text-decoration: underline;">
Transient analysis of FA adder verification for 32n
</p>

Red = Input A, Green = Input B, Purple = Input Cin, Light Blue = Output Cout, Violet = Output S

The transient curve can be seen and mapped to the truth table and verified.

##### Output: Verification of 1-bit Full Adder

Analyzing the Timing Diagram for a 1-bit Full Adder Logical Correctness

###### Taking 1 case for verification:

- **Inputs:** A = 1, B = 0, Cin = 1
- **Sum Calculation (S):** For a 1-bit full adder, the sum output is determined using the equation:

  $ S = A \cdot B \cdot C_{in} + \overline{C_{out}} \cdot (A + B + C_{in}) $

  Simplified as:

  $ S = A \oplus B \oplus C_{in} $
- **Carry-out (Cout):** The carry-out is given by:

  $ C_{out} = A \cdot B + B \cdot C_{in} + A \cdot C_{in} $
- **Waveform Verification:** In the timing diagram, during the time interval when A = 1, B = 0, and Cin = 1, the output sum $ S $ is 0, and the carry-out $ C_{out} $ is 1, which matches the expected result.

![alt text](images/28t_CMOS/32t_8b_RCA_schem.png)

<p align="center" style="font-size:14px; text-decoration: underline;">
8-bit ripple carry adder schematic
</p>

**8-bit Ripple Carry Adder**

To construct an 8-bit ripple carry adder, we connected eight 1-bit full adder circuits in series. The configuration begins with the first full adder receiving **$ C_{in} = 0 $**, along with the input bits **$ A_{0} $** and **$ B_{0} $**. This initial stage produces the sum **$ S_{0} $** and a carry-out (**$ C_{out1} $**).

The carry-out of each preceding full adder is then connected to the carry-in of the subsequent full adder to propagate the carry signal through the chain. The last carry-out, **$ C_{out} $**, serves as the overall carry output of the 8-bit adder, while the individual sums from **$ S_{0} $** to **$ S_{7} $** represent the 8-bit sum output.

This cascading structure forms the complete ripple carry adder, capable of adding two 8-bit binary numbers and generating a sum (**$ S_{0} – S_{7} $**) and carry-out, resulting in a 9-bit output.

---

#### 1.2 Verification of 8bit baseline adder for logical correctness

![alt text](images/28t_CMOS/8b_RCA_32t_func_test.png)

<p align="center" style="font-size:14px; text-decoration: underline;">
Test Schematic of 8-bit baseline to check for logical correctness
</p>

![alt text](images/28t_CMOS/8b_RCA_32t_FA_verif_plot.bmp)

<p align="center" style="font-size:14px; text-decoration: underline;">
8-bit ripple carry adder verification graph (Transient Analysis 32n)
</p>

##### Explanation

All 16 inputs, **$ A_{0} $** to **$ A_{7} $** and **$ B_{0} $** to **$ B_{7} $**, are toggled, with **$ C_{in} $** set to 0 (always set to 0).

- When all the inputs are 0, the outputs **$ S_{0} $** to **$ S_{7} $** and **$ C_{out} $** remain 0, as seen in the transient analysis graph below.
- Conversely, when all inputs are 1, **$ S_{0} $** is 0 due to the initial **$ C_{in} $** being 0. This only propagates a carry **$ C_{out0} = 1 $** (carry-out of the first full adder).
- For consequent stages, all three inputs are 1, resulting in the final **$ C_{out} $** being 1, while all other 7 outputs, **$ S_{1} $** to **$ S_{7} $**, are set to 1.

---

#### 1.3 Delay test case description and measurement

![alt text](images/28t_CMOS/32t_8b_RCA_delay_test.png)

<p align="center" style="font-size:14px; text-decoration: underline;">
8-bit Ripple Carry Adder Delay Test Schematic
</p>

![alt text](images/28t_CMOS/inv_32t_drive.png)

<p align="center" style="font-size:14px; text-decoration: underline;">
Input Drive Inverter Schematic
</p>

![alt text](images/28t_CMOS/16inv_32t_drive.png)

<p align="center" style="font-size:14px; text-decoration: underline; font-size:12px;">
16 input inverter driver schematic
</p>

##### Load Design

The maximum load of the 28-transistor Full Adder is **8C`<sub>`g`</sub>`** (for inputs **A** and **B**). To simulate a load of **8C`<sub>`g`</sub>`**, an NMOS transistor with a width **W = 960 nm** (calculated as 120 nm * 8 = 960 nm) and a length **L = 45 nm** is added. This NMOS configuration was applied to each sum output from **S`<sub>`0`</sub>`** to **S`<sub>`7`</sub>`**, with one NMOS transistor connected to each output. This setup helps emulate realistic self-loading conditions for evaluating the performance of the 8-bit ripple carry adder.

##### Test Setup

The input configuration to the 8-bit ripple carry adder for the delay test is as follows:

- **A`<sub>`0`</sub>`** is switched, **A`<sub>`1`</sub>`**, **A`<sub>`2`</sub>`**, **A`<sub>`3`</sub>`**, **A`<sub>`4`</sub>`**, **A`<sub>`5`</sub>`**, **A`<sub>`6`</sub>`**, **A`<sub>`7`</sub>`** = LOW = **vdc = 0V**
- **B`<sub>`0`</sub>`**, **B`<sub>`1`</sub>`**, **B`<sub>`2`</sub>`**, **B`<sub>`3`</sub>`**, **B`<sub>`4`</sub>`**, **B`<sub>`5`</sub>`**, **B`<sub>`6`</sub>`**, **B`<sub>`7`</sub>`** = HIGH = **vdc = 1.1V**

  *(These inputs arrive through the load inverters which provide a realistic input.)*

This input configuration tests for the worst-case delay, as it ensures that the carry signal must propagate through all stages of the 8-bit ripple carry adder.

##### Delay Measurement (Transient Analysis 60n)

- Delay is measured between **IN** and **C`<sub>`out`</sub>`**.
- **Input** = Red
- **C`<sub>`out`</sub>`** = Green

![alt text](images/28t_CMOS/8b_RCA_32t_prop_delay1.bmp)

<p align="center" style="font-size:14px; text-decoration: underline; font-size:12px;">
Transient Curve of input A0 switching and subsequently the output C<sub>out</sub>  switching.  
</p>

![alt text](images/28t_CMOS/8b_RCA_32t_prop_delay2.bmp)

<p align="center" style="font-size:14px; text-decoration: underline;">
Delay measurement from transient curve
</p>

| Parameter        | Calculation                             | Result    |
| ---------------- | --------------------------------------- | --------- |
| $ \tau_{plh} $ | 20.34269 ns - 20.01335 ns               | 329.19 ps |
| $ \tau_{phl} $ | 24.36127 ns - 24.00896 ns               | 352.32 ps |
| $ \tau_p $     | $ \frac{\tau_{plh} + \tau_{phl}}{2} $ | 340.83 ps |

---

#### 1.4 Baseline maximum and average active energy measurements

- To evaluate the active energy consumption of the 8-bit ripple carry adder during addition operations, measurements were set up for two specific scenarios:

  - **Maximum Switching Energy Case**: This scenario involves measuring the energy when all inputs transition from 0 to 1 simultaneously (noted as TRANS in the test schematic). This case represents the highest possible energy consumption due to the simultaneous switching of all input bits, resulting in maximum capacitive charging and discharging through the adder circuit.
  - **Average Switching Energy Case**: This scenario measures the energy when only half of the inputs transition from 0 to 1. It simulates a more typical operation where only a portion of the bits switch, representing an average use case.
- For both cases, the switching energy is calculated by integrating the power drawn from the power supply over time, starting from the moment the inputs begin switching until the outputs have fully transitioned. This integration captures the energy consumed during the dynamic operation of the adder as it processes the input changes and generates the output results.

![alt text](images/28t_CMOS/32t_8b_RCA_max_e_test.png)

<p align="center" style="font-size:14px; text-decoration: underline;">
Maximum energy test schematic for 8-bit ripple carry adder
</p>

![alt text](images/28t_CMOS/8b_RCA_32t_max_e_plot.bmp)

<p align="center" style="font-size:14px; text-decoration: underline;">
Maximum energy plot (Shows the Input, Output (C<sub>out</sub>), Current at V<sub>dd</sub>)
</p>

![alt text](images/28t_CMOS/32t_8b_RCA_max_e.png)

<p align="center" style="font-size:14px; text-decoration: underline;">
Maximum energy calculation (absolute and integrating)
</p>

The current is integrated from the point when all inputs start switching from 0 to when all outputs stop switching at 1.

Maximum active energy: **17.08 fJ**

![alt text](images/28t_CMOS/32t_8b_RCA_avg_e_test.png)

<p align="center" style="font-size:14px; text-decoration: underline;">
Average energy test schematic for 8-bit ripple carry adder
</p>

![alt text](images/28t_CMOS/8b_RCA_32t_avg_e_plot.bmp)

<p align="center" style="font-size:14px; text-decoration: underline;">
Average energy plot (Shows the Input, Output (Cout), Current at Vdd)
</p>

![alt text](images/28t_CMOS/32t_8b_RCA_avg_e.png)

<p align="center" style="font-size:14px; text-decoration: underline;">
Average energy calculation (absolute and integrating)
</p>

The current is integrated from the point when all inputs start switching from 0 to when all outputs stop switching at 1.

Average active energy: **10.27 fJ**

---

#### 1.5 Baseline Leakage energy maximum and minimum cases and measurements

For the leakage energy analysis of the 8-bit ripple carry adder, we considered eight key input test cases involving different configurations of **A**, **B**, and **C`<sub>`in`</sub>`** as mentioned in the truth table. The integration for measuring the leakage energy was conducted over **340.83 ps**, matching the delay period of the 8-bit adder to capture steady-state leakage when no switching occurred.

Transient response plots confirmed steady current behavior during these periods and showed energy spikes during active switching, allowing us to thoroughly assess the adder’s energy profile under different input conditions.

![alt text](images/28t_CMOS/32t_1b_FA_leakage_e_test.png)

<p align="center" style="font-size:14px; text-decoration: underline;">
Leakage energy test schematic
</p>

![alt text](images/28t_CMOS/8b_RCA_32t_leakage_e_plot.bmp)

<p align="center" style="font-size:14px; text-decoration: underline;">
Leakage energy plot
</p>

![alt text](images/28t_CMOS/32t_1b_FA_leakage_energy.png)

<p align="center" style="font-size:14px; text-decoration: underline;">
Leakage energy calculation
</p>

##### Leakage Energy Table for All Inputs

| Input Case | Energy over$ \tau_p $ | Energy for 8-bit RC Adder     |
| ---------- | ----------------------- | ----------------------------- |
| 000        | 4.534 fJ                | 36.272 fJ                     |
| 001        | 5.203 fJ                | 41.624 fJ                     |
| 010        | 3.664 fJ                | **29.312 fJ** (Minimum) |
| 011        | 4.319 fJ                | 34.552 fJ                     |
| 100        | 4.76 fJ                 | 38.08 fJ                      |
| 101        | 3.741 fJ                | 29.928 fJ                     |
| 110        | 3.709 fJ                | 29.672 fJ                     |
| 111        | 8.498 fJ                | **67.984 fJ** (Maximum) |

- **Minimum leakage energy** for input case **010**: 29.312 fJ
- **Maximum leakage energy** for input case **111**: 67.984 fJ

##### Metric Summary:

| Metric                   | Value          |
| ------------------------ | -------------- |
| $ \tau_{plh} $         | 329.19 ps      |
| $ \tau_{phl} $         | 352.32 ps      |
| $ \tau_{p} $           | 340.83 ps      |
| Maximum Switching Energy | 18.09 fJ       |
| Average Switching Energy | 10.27 fJ       |
| Minimum Leakage Energy   | 29.672 fJ      |
| Maximum Leakage Energy   | 67.984 fJ      |
| $ W/L_p $              | 120 nm / 45 nm |
| $ W/L_n $              | 120 nm / 45 nm |
| Maximum Input Load       | 8$C_g$       |
| Maximum Drive            | $R_{on}$     |

---

---

### 2. Optimized Design and Methodology

#### 2.0 Topological Considerations

For optimizing the 8-bit Ripple Carry Adder, we took two new topologies into consideration:

- Ratioed XOR2-NAND2 implementation
- Mirror Full Adder

In the next section, we will describe why we chose one design over the other.

** *Note we have carried out active and leakage energy calculations only for 1 design which we felt was better. As one of the design have a lower delay but doesn't restore the output.* **

---

#### 2.1 Optimized Design Description, Schematics, and Test Schematics

#### i.**Ratioed Full Adder:**

##### Description

![alt text](images/Ratiod_Adder/topology_diag.png)

<p align="center" style="font-size:14px; text-decoration: underline;">
Topology Diagram
</p>

Using the above topology, we can see that the sum of the full adder is generated by using 2 XOR2 gates in a chain topology. This simply implements the Boolean function:

$ S = A \oplus B \oplus C_{in} $

Further, in the topology, the carry output is obtained by utilizing 3 NAND2 gates in a tree topology.

Where:
$ X = A \oplus B $

Thus:
$ C_{out} = \overline{\left(\overline{\left((A \oplus B) \cdot C_{in}\right)} \cdot \overline{(A \cdot B)}\right)} $

which simplifies to:
$ C_{out} = A \cdot B + A \cdot C_{in} + B \cdot C_{in} $

In this topology, each XOR2 gate uses 12 transistors (4 for 2 inverters and 8 for XOR2 implementation), and each ratio’d NAND2 gate uses 3 transistors (with a Pull-down Logic function).

In total, the topology has $12 \times 2 + 3 \times 3 = 33$ transistors (15 PMOS, 18 NMOS).

The critical path (path propagating the carry-out) is the tree topology of the three NAND2 gates. Therefore, we decided to use ratio’d logic to decrease the delay.

##### Schematics

![alt text](images/Ratiod_Adder/ratiod_nand2_schem.png)

<p align="center" style="font-size:14px; text-decoration: underline;">
Ratioed nand2 schematic
</p>

![alt text](images/Ratiod_Adder/ratiod_1b_FA_schem.png)

<p align="center" style="font-size:14px; text-decoration: underline;">
Ratioed 1bit Full Adder Schematic
</p>

![alt text](images/Ratiod_Adder/ratiod_1b_FA_test.png)

<p align="center" style="font-size:14px; text-decoration: underline;">
Ratioed 1bit Full Adder Test Schematic
</p>

![alt text](images/Ratiod_Adder/ratiod_1b_FA_verif.bmp)

<p align="center" style="font-size:14px; text-decoration: underline;">
Ratioed 1bit Full Adder Test verification plot
</p>

The above transient plot maps completely with the truth table to verify its logical functionality.

![alt text](images/Ratiod_Adder/ratiod_8b_RCA_schem.png)

<p align="center" style="font-size:14px; text-decoration: underline;">
Ratioed 8-bit Ripple Carry Adder Schematic
</p>

##### Propagation Delay Calculation

![alt text](images/Ratiod_Adder/ratiod_8b_RCA_delay_test.png)

<p align="center" style="font-size:14px; text-decoration: underline;">
Ratioed 8-bit Ripple Carry Adder-Delay test schematic
</p>

![alt text](images/Ratiod_Adder/ratiod_8b_RCA_delay_plot.bmp)

<p align="center" style="font-size:14px; text-decoration: underline;">
Delay Plot from transient analysis
</p>

| Parameter        | Calculation                             | Result    |
| ---------------- | --------------------------------------- | --------- |
| $ \tau_{plh} $ | 20.22951 ns - 20.00881 ns               | 220.7 ps  |
| $ \tau_{phl} $ | 24.22086 ns - 24.00658 ns               | 214.28 ps |
| $ \tau_p $     | $ \frac{\tau_{plh} + \tau_{phl}}{2} $ | 217.49 ps |

**Even though this topology and sizing does give us very good delay optimization, we were unable to restore the output for this design with the use of buffers. Hence, we moved on to another topology that had a more robust output**

#### ii. **Mirror Adder**

##### Description:

- The following topology used is a special kind of a CMOS implementation which occurs when the number of parallel branches are equal to the number of branches in series. This leads to a circuit where the pull-up and pull-down network are identical mirror images of each other.
- The NMOS and PMOS chains are completely symmetrical, which still yields correct operation due to *self-duality* of both the sum and carry functions. As a result, a maximum of two series transistors can be found in the carry-generation circuitry.
- The transistors connected to $C_{i}$ are placed closest to the output of the gate.
- Only the transistors in the carry stage have to be optimized for speed.
- Meaning a Full adder will have correct operation if all inputs are inverted the output with still be $S$ and $C_{out}$.
- Hence, we can remove inverting stages from the critical path and utilize the inverted $C_{out}$ of the previous and invert the input $A$, $B$ to get non-inverted output. By doing so we are adding inverters on the sum path (non - critical) that does not affect the worst-case delay. This process is called **polarity optimization**.

##### Schematics

All ($L_{n,p}$) = 45 nm, width = $W$, $V_{dd}$ = 1.1 V

![alt text](images/Mirror_adder/24t_Mirror_FA_schem.png)

<p align="center" style="font-size:14px; text-decoration: underline;">
1-bit Mirror Adder Schematic
</p>

![alt text](images/Mirror_adder/24t_8b_RCA_schem.png)

<p align="center" style="font-size:14px; text-decoration: underline;">
8-bit Ripple Carry Adder using mirror adder schematic
</p>

![alt text](images/Mirror_adder/24t_8b_RCA_schem_inv.png)

<p align="center" style="font-size:14px; text-decoration: underline;">
8-bit RCA inverter schematic
</p>

These inverters are used to invert the input and output of alternative stages to maintain proper functionality. As these inverters are added on the non critical path they do not contribute in the worst case propagation delay of the carry-out while also allowing us to exploit the self-duality nature of the Full adder to reduce the delay of the critical path.

We can see from the schematic:

We invert the input at  all even bits stages $ a_{1}, b_{1}, a_{3}, b_{3}, a_{5}, b_{5}, a_{7}, b_{7} $

While we also have to invert the output received from adder to get the following output $ s_{0}, s_{2}, s_{4}, s_{6} $

![alt text](images/Mirror_adder/24t_1b_FA_test.png)

<p align="center" style="font-size:14px; text-decoration: underline;">
1-bit Mirror Adder Test Schematic
</p>

##### 1-bit full adder verification:

- **Cin** is connected to a Vpulse with:

  - Period = 8 ns
  - Pulse width = 4 ns
  - $ V_1 = 0 \, V $
  - $ V_2 = V_{dd} $
- **B** is connected to a Vpulse with:

  - Period = 16 ns
  - Pulse width = 8 ns
  - $ V_1 = 0 \, V $
  - $ V_2 = V_{dd} $
- **A** is connected to a Vpulse with:

  - Period = 32 ns
  - Pulse width = 16 ns
  - $ V_1 = 0 \, V $
  - $ V_2 = V_{dd} $
- **Rise time** and **Fall time** = 1 ps, **delay** = 0 s

  - Additional details about Vpulse
- **$ V_{dd} $** = Vdc = 1.1 V

![alt text](images/Mirror_adder/1b_24t_FA_verif_plot.bmp)

<p align="center" style="font-size:14px; text-decoration: underline;">
1bit full adder verification plot
</p>

---

##### 2.2 Verified optimized 8-bit adder logical verification:

![alt text](images/Mirror_adder/24t_8b_RCA_func_test.png)

<p align="center" style="font-size:14px; text-decoration: underline;">
8-bit Ripple carry adder functional verification schematic
</p>

All inputs from A0 to A7 and B0 to B7 are toggled, with Cin set to 0. When all the inputs are 0, the outputs S0 to S7 and Cout remain 0 as seen in the transient analysis graph below. Conversely, when all inputs are 1, S0 is 0 due to the initial Cin being 1. The carry is propagated through the adder, resulting in Cout being 1, while all other outputs S1 to S7 are set to 1.

![alt text](images/Mirror_adder/8b_RCA_24t_verif_plot.bmp)

<p align="center" style="font-size:14px; text-decoration: underline;">
8bit mirror ripple carry adder verification plot
</p>

---

#### 2.3 Optimized Delay test case description and measurements:

##### 16 input inverter driver

- The input driver comprises a 16-input inverter driver schematic incorporating 16 minimum-sized inverters, each designed with a width W = 120 nm and length L = 135 nm. These dimensions ensure that each inverter delivers the necessary drive strength while maintaining minimal size. The choice of this configuration considers the resistance seen at the output stage, specified as 3Run, to replicate realistic input conditions. This setup is crucial for accurate testing, as it delivers practical and realistic input signals to the 8-bit ripple carry adder.
- W n,p=120nm, L n,p = 135nm

![alt text](images/Mirror_adder/inv_24t_drive.png)

<p align="center" style="font-size:14px; text-decoration: underline;">
Input inverter driver schematic
</p>

![alt text](images/Mirror_adder/16inv_24t_drive.png)

<p align="center" style="font-size:14px; text-decoration: underline;">
16 input inverter driver
</p>

###### Equivalent Load

The maximum load of the 24-transistor Mirror Full Adder is 8Cg (for inputs A and B). To simulate a load of 8Cg, an NMOS transistor with a width W = 960 nm (calculated as 120 nm * 8 = 960 nm) and a length L = 45 nm is added. This NMOS configuration was applied to each sum output from S0 to S7, with one NMOS transistor connected to each output. This setup helps emulate realistic self-loading conditions for evaluating the performance of the 8-bit ripple carry adder.

Further, while sizing the circuit the equivalent load would increase according to the transistor size.

As we only size half the input transistors that are in the critical path for the carry-out generation the maximum load capacitance scales in this way:

As we assume the gate capacitance of the a min sized device to be Cg. For a device of width W we take the input gate capacitance to be $(W/120nm)*C_{g}$.

Hence, for the following Full adder implementation we get the following Load value:

$ \text{Maximum Load} = (4 \times \frac{W}{120} + 4) \times C_{g} $

Hence, for our optimal size of 505nm we get equivalent load to be 20.8*Cg that is provided by a single NMOS transistor of width = 2.5um

##### Test Setup

The input configuration to the 8-bit ripple carry adder for the delay test is as follows:

- **A`<sub>`0`</sub>`** is switched, **A`<sub>`1`</sub>`**, **A`<sub>`2`</sub>`**, **A`<sub>`3`</sub>`**, **A`<sub>`4`</sub>`**, **A`<sub>`5`</sub>`**, **A`<sub>`6`</sub>`**, **A`<sub>`7`</sub>`** = LOW = **vdc = 0V**
- **B`<sub>`0`</sub>`**, **B`<sub>`1`</sub>`**, **B`<sub>`2`</sub>`**, **B`<sub>`3`</sub>`**, **B`<sub>`4`</sub>`**, **B`<sub>`5`</sub>`**, **B`<sub>`6`</sub>`**, **B`<sub>`7`</sub>`** = HIGH = **vdc = 1.1V**

  *(These inputs arrive through the load inverters which provide a realistic input.)*

This input configuration tests for the worst-case delay, as it ensures that the carry signal must propagate through all stages of the 8-bit ripple carry adder.

###### Delay measurement: (Transient analysis 60n) – 1st iteration

![alt text](images/Mirror_adder/24t_delay_test_new.png)

<p align="center" style="font-size:14px; text-decoration: underline;">
8-bit Ripple carry adder delay test schematic
</p>

###### 1) All min size, W/L n,p = 120nm/45nm: Output plot for delay:

![alt text](images/Mirror_adder/opt_un_24t_prop_delay.bmp)

| Parameter        | Calculation                             | Result    |
| ---------------- | --------------------------------------- | --------- |
| $ \tau_{plh} $ | 20.33686 ns - 20.01897 ns               | 317.89 ps |
| $ \tau_{phl} $ | 24.32253 ns - 24.0149 ns                | 307.63 ps |
| $ \tau_p $     | $ \frac{\tau_{plh} + \tau_{phl}}{2} $ | 312.76 ps |

###### 2) Parametric analysis:

From: 120nm, To: 10um, Steps: 40

![alt text](images/Mirror_adder/opt_parametric_tphl_1.bmp)

<p align="center" style="font-size:14px; text-decoration: underline;">
Tphl of 8bit mirror adder
</p>

![alt text](images/Mirror_adder/opt_parametric_tplh_1.bmp)

<p align="center" style="font-size:14px; text-decoration: underline;">
Tplh of 8bit mirror adder
</p>

$ \tau_{plh} $: Min: 417nm, max 10um
$ \tau_{phl} $: Min: 508nm, max: 10um

###### 3) Parametric analysis:

From: 300nm, To: 650nm, Steps : 20

![alt text](images/Mirror_adder/opt_parametric_tphl_3.bmp)

<p align="center" style="font-size:14px; text-decoration: underline;">
Tphl of 8-bit mirror adder
</p>

![alt text](images/Mirror_adder/opt_parametric_tplh_3.bmp)

<p align="center" style="font-size:14px; text-decoration: underline;">
Tplh of 8bit mirror adder
</p>

$ \tau_{plh} $: Min: 429nm, max: 300nm
$ \tau_{phl} $: Min: 594nm, max: 300nm

###### 4) Parametric analysis:

From: 400nm, To: 600nm, Steps : 20

![alt text](images/Mirror_adder/opt_parametric_tplh_4.bmp)

<p align="center" style="font-size:14px; text-decoration: underline;">
Tplh of  mirror adder
</p>

After conducting many iterations of parametric analysis for optimizing the 8-bit ripple carry adder, we finalized a transistor width $W = 505$ nm. The analysis included evaluating propagation delays $ \tau_{plh} $ and $ \tau_{phl} $ over various width ranges, from 120 nm to 10 µm, with 40 steps to identify the optimal $W$. The final results show a propagation delay $ \tau_{p} = \mathbf{240.57}$ ps, with $ \tau_{plh} = \mathbf{250.24}$ ps and $ \tau_{phl} = \mathbf{231.14}$ ps. This configuration achieved a performance improvement, optimizing the delay by **29.43%**. The integration of this analysis ensures that the 8-bit ripple carry adder operates efficiently.

**Final delay measurement for $W = 505nm$ optimized chosen design**

Delay is measured between IN and Cout.

- IN - Red
- Cout - Green

![alt text](images/Mirror_adder/8b_RCA_24t_prop_delay_plot_1.bmp)

<p align="center" style="font-size:14px; text-decoration: underline;">
Transient waveform of mirror adder
</p>

![alt text](images/Mirror_adder/8b_RCA_24t_prop_delay_plot_2.bmp)

| Parameter        | Calculation                             | Result    |
| ---------------- | --------------------------------------- | --------- |
| \( \tau_{plh} \) | 20.28648 ns - 20.03624 ns               | 250.24 ps |
| \( \tau_{phl} \) | 24.26029 ns - 24.02914 ns               | 231.14 ps |
| \( \tau_p \)     | \( \frac{\tau_{plh} + \tau_{phl}}{2} \) | 240.57 ps |

---

#### 2.4 Optimized maximum and average active energy  measurements

To evaluate the active energy consumption of the 8-bit ripple carry adder during addition operations, measurements were set up for two specific scenarios:

1. **Maximum Switching Energy Case**: This scenario involves measuring the energy when all inputs transition from 0 to 1 simultaneously (noted as TRANS). This case represents the highest possible energy consumption due to the simultaneous switching of all input bits, resulting in maximum capacitive charging and discharging through the adder circuit.
2. **Average Switching Energy Case**: This scenario measures the energy when only half of the inputs transition from 0 to 1. It simulates a more typical operation where only a portion of the bits switch, representing an average use case.

For both cases, the switching energy is calculated by integrating the power drawn from the power supply over time, from when the inputs begin switching until the outputs have fully transitioned.

##### Max energy:

![alt text](images/Mirror_adder/24t_max_e_test.png)

<p align="center" style="font-size:14px; text-decoration: underline;">
Mirror adder maximum energy test schematic
</p>

![alt text](images/Mirror_adder/8b_RCA_24t_max_e_plot.bmp)

<p align="center" style="font-size:14px; text-decoration: underline;">
Mirror adder maximum energy plot
</p>

![alt text](images/Mirror_adder/24t_8b_RCA_max_e.png)

<p align="center" style="font-size:14px; text-decoration: underline;">
Mirror adder maximum energy calculation
</p>

The current is integrated from the point when all inputs start switching from 0 to when all outputs stop switching at 1.

Maximum active energy: **28.39 fJ**

##### Average energy

![alt text](images/Mirror_adder/24t_8b_RCA_avg_e_test.png)

<p align="center" style="font-size:14px; text-decoration: underline;">
Mirror adder average energy test schematic
</p>

![alt text](images/Mirror_adder/8b_RCA_24t_avg_e_plot.bmp)

<p align="center" style="font-size:14px; text-decoration: underline;">
Mirror adder average energy plot
</p>

![alt text](images/Mirror_adder/24t_8b_RCA_avg_e.png)

<p align="center" style="font-size:14px; text-decoration: underline;">
Mirror adder average energy calculation
</p>
The current is integrated from the point when all inputs start switching from 0 to when all outputs stop switching at 1.

Average active energy: **15.20 fJ**

---

#### 2.5 Optimized Leakage energy maximum and minimum cases and measurements

![alt text](images/Mirror_adder/24t_1b_FA_leakage_e_test.png)

<p align="center" style="font-size:14px; text-decoration: underline;">
1-bit Mirror adder leakage energy test schematic
</p>

![alt text](images/Mirror_adder/8b_RCA_24t_leakage_e_plot.bmp)

<p align="center" style="font-size:14px; text-decoration: underline;">
1-bit Mirror adder leakage energy plot
</p>

![alt text](images/Mirror_adder/24t_1b_FA_leakage_energy.png)

<p align="center" style="font-size:14px; text-decoration: underline;">
1-bit Mirror adder leakage energy calculation
</p>

### Leakage Energy Table for All Inputs

| Input Case | Energy over$ \tau_p $ | Energy for 8-bit RC Adder     |
| ---------- | ----------------------- | ----------------------------- |
| 000        | 0.6182 fJ               | 4.9456 fJ                     |
| 001        | 0.7309 fJ               | 5.8472 fJ                     |
| 010        | 0.6758 fJ               | 5.4064 fJ                     |
| 011        | 1.265 fJ                | 10.218 fJ                     |
| 100        | 0.5349 fJ               | **4.2792 fJ** (Minimum) |
| 101        | 1.160fJ                 | 9.28 fJ                       |
| 110        | 1.564 fJ                | 12.512 fJ                     |
| 111        | 4.130 fJ                | **33.04 fJ** (Maximum)  |

- **Minimum leakage energy** for input case **100**:  4.2792 fJ
- **Maximum leakage energy** for input case **111**:  33.04 fJ

### Metric Summary

| Metric                   | Value            |
| ------------------------ | ---------------- |
| $ \tau_{plh} $         | 250.24 ps        |
| $ \tau_{phl} $         | 231.14 ps        |
| $ \tau_{p} $           | 240.57 ps        |
| Maximum Switching Energy | 28.39 fJ         |
| Average Switching Energy | 15.20 fJ         |
| Minimum Leakage Energy   | 4.2792 fJ        |
| Maximum Leakage Energy   | 33.04 fJ         |
| $ W/L_p $              | 505 nm / 45 nm   |
| $ W/L_n $              | 505 nm / 45 nm   |
| Maximum Input Load       | $20.8 * C_{g}$ |
| Maximum Drive            | $3 * R_{on}$   |

*Note some transistors that our not in the critical path are left to be minimum sized*

---

---

### 3. Optimization process description

We chose a baseline adder schematic utilizing 28 transistors, with a propagation delay of 340.83 ps.

1. **Topological Optimization**

   - This circuit contained a few redundancies that could be removed to improve delay. Firstly, the circuit inverts the sum and carry. Inverting the carry at every stage adds unnecessary delay to the critical path.
   - By using the mirror adder topology, we removed these redundancies, specifically the inversion of the carry-out at every stage, which helps to reduce delay. This optimization is possible due to the self-duality nature of the full adder.

   | No. | A | B | Cin | Sum | Cout |
   | --- | - | - | --- | --- | ---- |
   | 1   | 0 | 0 | 0   | 0   | 0    |
   | 2   | 0 | 0 | 1   | 1   | 0    |
   | 3   | 0 | 1 | 0   | 1   | 0    |
   | 4   | 0 | 1 | 1   | 0   | 1    |
   | 5   | 1 | 0 | 0   | 1   | 0    |
   | 6   | 1 | 0 | 1   | 0   | 1    |
   | 7   | 1 | 1 | 0   | 0   | 1    |
   | 8   | 1 | 1 | 1   | 1   | 1    |

   From the truth table, we can verify this self-dual nature: row 1 is the complement of row 8, row 2 is the complement of row 7, and so on. This observation forms the basis for implementing the mirror adder schematic.
2. **Sizing Optimization**

   - After finalizing the preferred topology, which provides a robust output and removes inversion redundancies from the critical path, we optimized the sizes of the transistors in the critical path to further reduce delay.
   - To achieve this, we conducted parametric analysis (documented above) to identify a transistor size that minimizes the worst-case propagation delay. After several iterations, we arrived at an approximate width of 505 nm.

Through these two steps, we improved the delay of our full adder by **29.43%** compared to our 28-transistor baseline.

---

---

### 4. Design Metric summary table

| Metric                           | Baseline Value | Optimized Value | Percentage Change/Improvement        |
| -------------------------------- | -------------- | --------------- | ------------------------------------ |
| $ \tau_{p} $ Propagation Delay | 340.83 ps      | 240.57 ps       | 29.43% reduction or 1.4167x Speed-up |
| Maximum Switching Energy         | 18.09 fJ       | 28.39 fJ        | 36.28% increase                      |
| Average Switching Energy         | 10.27 fJ       | 15.20 fJ        | 32.43% increase                      |
| Minimum Leakage Energy           | 29.672 fJ      | 4.2792 fJ       | 85.57% reduction                     |
| Maximum Leakage Energy           | 67.984 fJ      | 33.04 fJ        | 51.45% reduction                     |
| $ W/L_p $                      | 120 nm / 45 nm | 505 nm / 45 nm  | -                                    |
| $ W/L_n $                      | 120 nm / 45 nm | 505 nm / 45 nm  | -                                    |
| Maximum Input Load               | 8$C_g$       | 20.8$C_g$     |                                      |
| Maximum Drive                    | $R_{on}$     | 3$R_{on}$     |                                      |

From the above summary table we can draw the conclusions that the worst-case propagation delay $ \tau_{p} $ of the optimized design decreased by **29.43%**. Due to the increased number of transistor (inverters added in non-critical path) the maximum and average active energy increased, whereas due to the reduction of worst-case propagation delay $ \tau_{p} $ the maximum and minimum leakage energy also decreased.
