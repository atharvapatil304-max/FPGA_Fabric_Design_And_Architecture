# Day 3 - Mythcore Processor Implementation and FPGA Analysis
## Objective

Day 3 focused on implementing and analyzing a more complex FPGA design using the **Mythcore RISC-V processor**. The objective was to study the complete FPGA implementation flow for a processor-based system and evaluate its performance on FPGA hardware.

### Activities Performed

* RTL Simulation
* FPGA Synthesis
* RTL and Technology Schematic Analysis
* Package and Pin Mapping
* Integrated Logic Analyzer (ILA) Integration
* Timing Analysis
* Resource Utilization Analysis
* Power Analysis
* Functional Verification

This exercise provided practical exposure to implementing processor-based designs and analyzing timing, area, routing, and power characteristics on FPGA platforms.

---

# Design Source

## Files Used

* `mythcore_test.v`
* `mythcore_test_gn.v`

## Reference Repository

*https://github.com/shivanishah269/risc-v-core/tree/master*

---

# FPGA Design Flow

The Mythcore processor was implemented using the standard FPGA design flow:

1. RTL Design
2. Functional Simulation
3. Synthesis
4. Netlist Generation
5. Placement and Routing
6. Timing Analysis
7. Bitstream Generation
8. Hardware Debugging using ILA

---

# RTL Simulation

RTL simulation was performed to verify the functionality of the Mythcore processor before synthesis and implementation.

## Observations

* Correct Clock Operation
* Successful Reset Initialization
* Expected Processor Execution
* Functional Behavior Verification

## RTL Simulation Output

<img width="1273" height="178" alt="image" src="https://github.com/user-attachments/assets/21bee600-16ab-4e50-8145-89322cd94957" />

---

# Schematic Generation

After synthesis, Vivado generated an RTL schematic of the Mythcore processor, providing a graphical representation of the design architecture and logic connectivity.

## Key Components

* LUT Structures
* Flip-Flops
* Internal Routing
* Processor Datapath
* Logic Interconnections

Due to the complexity of the Mythcore processor, the generated schematic contained a large number of interconnected logic elements.

---

# Package View

The package view provides a physical representation of FPGA resources and I/O pin assignments after implementation.

### Key Insights

* Physical FPGA Layout
* Resource Mapping
* Pin Assignments
* Routing Regions

The package view helps visualize how the design is mapped onto the target FPGA device.

## Package View Output

<img width="695" height="379" alt="image" src="https://github.com/user-attachments/assets/f6a83d2e-9399-48fa-8da3-1bb168ffa4fd" />

---

# Integrated Logic Analyzer (ILA)

## Objective

The Integrated Logic Analyzer (ILA) was used to monitor and debug internal FPGA signals in real time through the Vivado Hardware Manager.

Unlike external I/O monitoring, ILA enables direct observation of internal design signals during FPGA operation.

---

# ILA Instantiation

```verilog id="xifv6o"
ila_0 your_instance_name (
    .clk(clk),
    .probe0(reset),
    .probe1(out)
);
```
---

# ILA Connections

| Signal  | Purpose      |
| ------- | ------------ |
| `clk`   | Clock Input  |
| `reset` | Reset Signal |
| `out`   | Output Probe |

---

# Constraint File

The XDC constraint file was used to define:

* Clock Pin Mapping
* Reset Pin Mapping
* Timing Constraints
* Debug Hub Configuration
* Clock Frequency

## Constraints Used

<img width="817" height="361" alt="image" src="https://github.com/user-attachments/assets/5761c6b6-5987-417c-8057-7d3e74e3bdad" />

---

# Utilization Analysis

The utilization report was analyzed to evaluate FPGA resource consumption by the Mythcore processor implementation.

## Resources Utilized

* LUTs
* LUTRAM
* Flip-Flops
* BRAM
* I/O Blocks

Compared to the counter design, the Mythcore processor required significantly more FPGA resources due to its increased architectural complexity and functionality.

## Utilization Report
<img width="577" height="275" alt="image" src="https://github.com/user-attachments/assets/1ea17881-7391-4e2e-bf28-41720aeeb35f" />

---

# Timing Analysis

Timing analysis was performed after placement and routing to verify the correct operation of the Mythcore processor on the FPGA.

## Objectives

* Verify Setup Timing
* Verify Hold Timing
* Ensure Correct Clock Synchronization
* Validate Timing Constraints

The timing reports confirmed that the design met the required timing specifications for reliable FPGA operation.

## Timing Analysis Report
<img width="892" height="169" alt="image" src="https://github.com/user-attachments/assets/741a9817-82ad-4681-80e9-a32bd54edd1a" />

---

# Power Analysis

Power analysis was performed after implementation to evaluate the power consumption characteristics of the Mythcore processor on the FPGA.

## Power Components Analyzed

* Dynamic Power
* Static Power
* Clock Power
* Logic Power
* Signal Power

## Observations

* The clock network contributed significantly to dynamic power consumption.
* Logic switching activity increased overall power usage.
* Static FPGA power remained relatively constant throughout operation.

The power report provides insights into the energy efficiency and operational characteristics of the implemented design.

## Power Analysis Output
<img width="633" height="266" alt="image" src="https://github.com/user-attachments/assets/279e864f-4c05-4809-aa34-cc52d37a3a37" />

---

# Important Observations

## Design Complexity

Compared to the counter design, the Mythcore processor exhibited significantly higher design complexity, resulting in:

* Increased Routing Complexity
* Higher Resource Utilization
* More Critical Timing Paths

## FPGA Resource Usage

The processor implementation required:

* A Large Number of LUTs
* Multiple Flip-Flops
* Additional Routing Resources
* Increased Clock Network Usage

## Importance of Timing Constraints

Proper timing constraints were essential for successful implementation. Without constraints:

* Timing Violations May Occur
* Placement Efficiency Decreases
* Routing Delays Increase
* Timing Closure Becomes Difficult

Applying appropriate constraints enabled the FPGA tools to optimize placement, routing, and timing performance.

---

# Conclusion

The Mythcore RISC-V processor was successfully implemented and analyzed on the FPGA platform. The workflow included RTL simulation, synthesis, schematic analysis, package mapping, ILA-based debugging, timing analysis, utilization analysis, and power analysis.

This experiment provided practical insight into the implementation, optimization, and verification of complex processor-based designs on FPGA architectures.

---
