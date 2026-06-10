# Day 5 – RISC-V Implementation on Custom SOFA Fabric
## RISC-V Core Implementation on SOFA FPGA Fabric

The **RVMYTH RISC-V core** was implemented on a custom **SOFA FPGA fabric** using the OpenFPGA and VTR toolchain. The complete flow, from synthesis to routing and verification, was executed to validate the processor implementation on the generated FPGA architecture.

### Analysis Performed

* Timing Analysis
* Resource Utilization Analysis
* Routing Analysis
* Post-Implementation Simulation
* FPGA Mapping Verification

The generated reports were used to evaluate design correctness, timing performance, routing efficiency, and successful mapping of the processor onto the custom FPGA fabric.

---

# SOFA RVMYTH Utilization Report

The utilization report summarizes the FPGA resources consumed by the **RVMYTH RISC-V core** after synthesis and mapping onto the **SOFA FPGA architecture**. It provides insight into the hardware resources required to implement the processor on the custom FPGA fabric.

## Circuit Statistics

* Total Blocks: 5526
* Inputs: 2
* Outputs: 8
* Latches: 1807
* 0-LUTs: 4
* 4-LUTs: 3705

## Net Statistics

* Total Nets: 5518
* Average Fanout: 3.1
* Maximum Fanout: 1807
* Minimum Fanout: 1.0

## Timing Graph Statistics

* Timing Graph Nodes: 22705
* Netlist Clocks: 1

<img width="422" height="380" alt="image" src="https://github.com/user-attachments/assets/d01d1760-9ebb-4900-8cce-c453065493b4" />

---

# Constraints File

An **SDC (Synopsys Design Constraints)** file was used to define the timing requirements for the RVMYTH design.

## Constraint File

```tcl id="szon5n"
create_clock -period 200 clk
set_input_delay -clock clk -max 0 [get_ports {*}]
set_output_delay -clock clk -max 0 [get_ports {*}]
```

## Defined Constraints

* Clock Period
* Input Delay Constraints
* Output Delay Constraints

These constraints guide the timing analysis engine during placement, routing, and timing verification.

<img width="566" height="137" alt="image" src="https://github.com/user-attachments/assets/30042811-0389-445f-a4a4-798bdb3de97b" />

---

# Timing Analysis

Timing analysis was performed after placement and routing to verify that the design meets the specified setup and hold timing requirements.

## Observations

* Positive Setup Slack
* Timing Constraints Satisfied
* No Critical Timing Violations
* Successful Timing Closure

The timing report indicates that data arrival times are within the required limits, ensuring reliable operation of the RVMYTH processor on the SOFA FPGA fabric.

## Timing Analysis Report

<img width="460" height="389" alt="image" src="https://github.com/user-attachments/assets/8d9b388b-a960-4b43-add9-12c8d316610f" />

---

# Hold Timing Analysis

Hold timing analysis verifies that data remains stable for the required duration after the active clock edge.

## Observations

* Hold Timing Requirements Satisfied
* Positive Hold Slack
* No Hold Violations Detected
* Stable Data Transfer Between Registers

The hold timing report confirms that the design operates reliably without hold-time violations.

## Hold Timing Report
<img width="1044" height="786" alt="image" src="https://github.com/user-attachments/assets/c8789c88-7991-430e-9a7a-af503cbc5362" />

---

# Post-Implementation Simulation

Post-implementation simulation was performed after synthesis, placement, and routing of the **RVMYTH RISC-V core** to verify the functionality of the final implemented design.

## Observations

* Correct Processor Execution
* Stable Clock and Reset Operation
* Expected Output Transitions
* Successful Netlist Verification

The simulation results confirm that the implemented design behaves as expected after FPGA mapping and routing.

## Post-Implementation Simulation Output

<img width="723" height="355" alt="image" src="https://github.com/user-attachments/assets/482e956e-1993-4003-8018-e1a16975b896" />

---

# Observations

* The RVMYTH RISC-V processor was successfully mapped onto the custom SOFA FPGA fabric.
* Resource utilization increased significantly compared to simple counter-based designs.
* Timing requirements were satisfied with positive setup and hold slack values.
* The OpenFPGA and VTR flow successfully generated all implementation and analysis reports.
* Post-implementation simulation verified the correctness of the synthesized and routed design.

---

# Conclusion

The RVMYTH RISC-V core was successfully implemented on a custom SOFA FPGA architecture using the OpenFPGA and VTR toolchain. Analysis of utilization, timing, routing, and simulation results confirmed correct processor operation and successful FPGA mapping.

This experiment demonstrated the complete open-source FPGA implementation flow, from RTL design and synthesis to placement, routing, timing verification, and post-implementation simulation.

