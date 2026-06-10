# Day 2 – OpenFPGA, VPR, and VTR Workflow Exploration
## Introduction

Day 2 focused on exploring the open-source FPGA CAD workflow using **OpenFPGA**, **VPR**, and **VTR**. The complete flow from Verilog RTL to FPGA placement, routing, and timing analysis was studied, along with report generation and design evaluation.

---

# OpenFPGA

OpenFPGA is an open-source framework for FPGA architecture research and fabric generation.

## Key Applications

* FPGA Architecture Exploration
* FPGA Fabric Generation
* Bitstream Generation
* CAD Flow Automation
* Design Verification

---

# VPR (Versatile Place and Route)

VPR is an open-source FPGA CAD tool responsible for implementing a design on a target FPGA architecture.

## Main Functions

* Packing
* Placement
* Routing
* Timing Analysis

## VPR Flow Command

```bash
$VTR_ROOT/vpr/vpr \
$VTR_ROOT/vtr_flow/arch/timing/EArch.xml \
<blif-file-path> \
--route_chan_width 100 \
--disp on
```
## VPR Flow

1. Packing of logic primitives
2. Placement on the FPGA grid
3. Routing of interconnections
4. Timing analysis and evaluation

## VPR GUI Visualization

The VPR graphical interface was used to analyze:

* FPGA Grid Structure
* Logic Block Placement
* Routing Resources
* Nets and Critical Paths

### VPR Visualization

<img width="699" height="557" alt="image" src="https://github.com/user-attachments/assets/c1e4a5b7-21a7-4dd5-bd3e-510d9f765af2" />

---

# EArch FPGA Architecture Analysis

The **EArch.xml** architecture was analyzed using the VPR flow to study FPGA placement, routing, timing, and routing resources.

## VPR Command

```bash
$VTR_ROOT/vpr/vpr \
$VTR_ROOT/vtr_flow/arch/timing/EArch.xml \
$VTR_ROOT/vtr_flow/benchmarks/blif/tseng.blif \
--route_chan_width 100 \
--disp on
```

## Outputs

* FPGA Architecture Visualization
* Logic Placement
* Routing Analysis
* Net Connectivity
* Timing Reports

---

# FPGA Architecture Visualization

The EArch FPGA architecture visualization was used to examine:

* Logic Blocks
* Routing Channels
* Switch Blocks
* Interconnect Resources
* FPGA Grid Structure

# Nets Analysis

The nets report provides information about:

* Source and Destination Nodes
* Net Routing Paths
* Interconnect Connectivity
* Routing Resource Utilization

## Nets Reports

<img width="1399" height="1116" alt="image" src="https://github.com/user-attachments/assets/067b7348-504f-45b6-9634-2d74c4e3c0fd" />

---

# Logical Connections
Logical connections show:

* Signal interconnections
* Routing connectivity
* Logic block communication

## Logical Connections Report

<img width="1401" height="1117" alt="image" src="https://github.com/user-attachments/assets/593c0ce8-41c3-438d-b913-4a6485bd2c26" />

---

# Critical Path Analysis

Critical path analysis identifies the longest timing path in the design and determines the maximum propagation delay and achievable operating frequency.

## Key Metrics

* Longest Timing Path
* Maximum Propagation Delay
* Maximum Operating Frequency

The critical path is a key factor affecting overall FPGA performance.

## Critical Path Report

<img width="698" height="572" alt="image" src="https://github.com/user-attachments/assets/056d48e9-ceaa-4989-a890-48e15a0e6d19" />


---

# Routing Utilization

Routing utilization analysis evaluates how efficiently the FPGA routing resources are used throughout the design.

## Key Metrics

* Channel Occupancy
* Routing Efficiency
* Congestion Distribution
* Resource Utilization

## Routing Utilization Report

<img width="794" height="595" alt="image" src="https://github.com/user-attachments/assets/90c0e711-2f37-4f85-ac2a-61089284c0dc" />

---

# Timing Analysis using Constraints
Timing constraints were added using an SDC file.

The constraints file defines:

* Clock period
* Input delays
* Output delays

---

# SDC Constraint File

## Constraint File

```tcl
create_clock -period 10 -name pclk
set_input_delay -clock pclk -max 0 [get_ports {*}]
set_output_delay -clock pclk -max 0 [get_ports {*}]
```

---

# Running VPR with Constraints

```bash
$VTR_ROOT/vpr/vpr \
$VTR_ROOT/vtr_flow/arch/timing/EArch.xml \
$VTR_ROOT/vtr_flow/benchmarks/blif/tseng.blif \
--route_chan_width 100 \
--sdc_file tseng.sdc \
--disp on
```

---

# Setup Timing Analysis

Setup timing analysis verifies that data arrives at the destination register before the required clock edge.

## Observations

* Improved Setup Slack
* Reduced Timing Violations
* Successful Timing Closure

## Setup Timing Report

<img width="673" height="556" alt="image" src="https://github.com/user-attachments/assets/cdce5e5c-b845-494c-89c5-1af881948050" />

---
# Hold Timing Analysis

Hold timing ensures data remains stable after the active clock edge.

## Hold Timing Report

<img width="1348" height="493" alt="image" src="https://github.com/user-attachments/assets/12b43a6f-9303-4e1c-826a-e886a83c1443" />


---

# Introduction to VTR

**VTR (Verilog-to-Routing)** is an open-source FPGA CAD framework that provides a complete flow from RTL design to FPGA implementation and timing analysis.

## VTR Flow

* RTL Elaboration
* Synthesis
* Technology Mapping
* Packing
* Placement
* Routing
* Timing Analysis

## Core Tools

* **ODIN II** – RTL Elaboration and Synthesis
* **ABC** – Logic Optimization and Technology Mapping
* **VPR** – Packing, Placement, Routing, and Timing Analysis

---
# VTR Flow Command

```bash
$VTR_ROOT/vtr_flow/scripts/run_vtr_flow.py \
counter.v \
$VTR_ROOT/vtr_flow/arch/timing/EArch.xml \
-temp_dir . \
-route_chan_width 100
```

---

# Counter for VTR Flow

The following counter design was used for VTR implementation.

## Counter Verilog Code

```verilog
/*Important: Once you run ./a.out, it will keep running infinitely, because it is in an always block. You need to hit Ctrl + Z to stop it, else, the vcd will become a large file and will never end.
*/

module up_counter (
out      ,
enable   ,
clk      ,
reset
);

output [3:0] out;

input enable, clk, reset;

reg [3:0] out;

always @(posedge clk)

if (reset) begin
    out = 4'b0 ;
end

else if (enable) begin
    out = out + 1;
end

endmodule
```

---

# VTR Flow Stages

The VTR flow performed the following stages:

1. Elaboration and Synthesis using ODIN II  
2. Logic Optimization using ABC  
3. Packing using VPR  
4. Placement using VPR  
5. Routing using VPR  
6. Timing Analysis  

---

# Generated VTR Outputs

The VTR flow generated:
- `.net`
- `.place`
- `.route`
- `.blif`
- Timing reports
- Routing reports
- Placement reports

---

# Nets and Logical Connections

The generated reports were analyzed to study the connectivity and routing structure of the implemented design.

## Analysis Performed

* Net Connectivity
* Routing Utilization
* Critical Path Analysis
* Logical Interconnections

These reports provide insight into signal flow, routing efficiency, and communication between FPGA logic elements.

## Nets Report

<img width="626" height="510" alt="image" src="https://github.com/user-attachments/assets/c0d67427-fdb8-429c-97d6-9d1d549d69fd" />

## Logical Connections

<img width="551" height="506" alt="image" src="https://github.com/user-attachments/assets/a6a479b2-5fe4-42c6-8265-8975bcd71492" />

---

# Critical Path Analysis

Critical path analysis was performed after routing to identify the longest timing path in the design.

## Key Metrics

* Maximum Operating Frequency
* Worst-Case Delay Path
* Timing Performance Evaluation

The critical path directly impacts the achievable clock frequency and overall performance of the FPGA implementation.

## Critical Path Report

<img width="700" height="521" alt="image" src="https://github.com/user-attachments/assets/169cd326-3dec-436b-846c-7c82083c046e" />

---

# Timing Analysis using Constraints

Timing constraints were applied using an **SDC (Synopsys Design Constraints)** file to guide the placement, routing, and timing optimization process.

## Constraints Defined

* Clock Period
* Input Delay Constraints
* Output Delay Constraints

These constraints enabled accurate timing analysis and improved timing closure during implementation.

## Setup Timing Analysis

Setup timing analysis verifies that data arrives at the destination register before the required clock edge.

### Results

* Setup Timing Requirements Met
* Positive Setup Slack Achieved
* No Critical Setup Violations

### Setup Timing Report

<img width="607" height="309" alt="image" src="https://github.com/user-attachments/assets/b4ca4ad3-35b5-455a-ac27-e39c7b2eaa33" />

### Hold Timing Report

<img width="649" height="368" alt="image" src="https://github.com/user-attachments/assets/c7507a8d-3251-4507-b00a-58a79daba72d" />

---

# SDC Constraint File

Timing constraints were defined using an SDC (Synopsys Design Constraints) file to guide timing analysis during FPGA implementation.

## Constraint File

```
create_clock -period 10 up_counter_clk
set_input_delay -clock up_counter_clk -max 0 [get_ports {*}]
set_output_delay -clock up_counter_clk -max 0 [get_ports {*}]
```
---

# Running VPR with Constraints

```bash
$VTR_ROOT/vpr/vpr \
$VTR_ROOT/vtr_flow/arch/timing/EArch.xml \
counter.pre-vpr.blif \
--route_chan_width 100 \
--sdc_file counter.sdc
```
---
# Setup Timing Analysis

After applying the timing constraints, the implementation achieved improved timing performance.

## Results

* Improved Setup Slack
* Timing Requirements Satisfied
* Successful Timing Closure

## Setup Timing Report

<img width="576" height="362" alt="image" src="https://github.com/user-attachments/assets/00fb5675-1d2c-47fe-b844-b9a06fbdc064" />

---

# Post Synthesis Simulation

Post synthesis simulation was performed using:
- Generated post synthesis netlist
- SDF timing file
- Vivado simulator

The generated files:
- `up_counter_post_synthesis.v`
- `up_counter_post_synthesis.sdf`

---

# Generating Post Synthesis Netlist

```bash
$VTR_ROOT/vpr/vpr \
$VTR_ROOT/vtr_flow/arch/timing/EArch.xml \
counter.pre-vpr.blif \
--gen_post_synthesis_netlist on
```

---

# Testbench for Post Synthesis Simulation

## Counter Testbench

```verilog
`timescale 1ns/1ps

module upcounter_testbench();

reg clk, reset, enable;
wire [3:0] out;

up_counter dut(
    .\up_counter^enable (enable),
    .\up_counter^clk (clk),
    .\up_counter^reset (reset),
    .\up_counter^out~0 (out[0]),
    .\up_counter^out~1 (out[1]),
    .\up_counter^out~2 (out[2]),
    .\up_counter^out~3 (out[3])
);

initial $sdf_annotate("up_counter_post_synthesis.sdf", dut);

initial begin

clk=0;
enable=0;
reset=1;

#20;

reset=0;
enable=1;

end

always
#5 clk=~clk;

endmodule
```

---

# Post-Synthesis Simulation Results

Post-synthesis simulation was performed to verify the functionality of the synthesized design and validate its timing characteristics.

## Verification Results

* Timing Behavior Validation
* Routing Delay Verification
* Gate-Level Functional Verification

The generated waveform confirmed that the synthesized design operated correctly after FPGA implementation.

## Post-Synthesis Waveform
<img width="1819" height="922" alt="image" src="https://github.com/user-attachments/assets/514cee3c-8969-4171-8ddb-69e914febd05" />

---

# Power Analysis using VTR

Power estimation was performed using VTR power analysis flow.

The flow estimated:
- Dynamic power
- Routing power
- Clock power
- Leakage power

---

# Power Analysis Command

```bash
$VTR_ROOT/vtr_flow/scripts/run_vtr_flow.py \
counter.v \
$VTR_ROOT/vtr_flow/arch/timing/k6_frac_N10_mem32K_40nm.xml \
-power \
-temp_dir . \
-route_chan_width 100
```
---

# stdout.log report

<img width="488" height="290" alt="image" src="https://github.com/user-attachments/assets/f66e8337-2d76-4e90-95ae-bd94f81225e9" />

---

# Power Report

The power report summarizes the estimated power consumption of the design after FPGA implementation and routing.

## Power Analysis

The analysis provides insights into:

* Dynamic Power Consumption
* Clock Network Power
* Routing Power
* Logic Power
* Leakage Power

The generated report helps evaluate the overall energy efficiency of the implemented design.

## Power Report Output
 <img width="473" height="301" alt="image" src="https://github.com/user-attachments/assets/8f60808e-4894-4e3e-859f-1f0f6bdc3a29" />

---

# VTR Generated Reports

The VTR flow generated several reports and implementation files used for design analysis and verification.

## Generated Reports

* Timing Reports
* Routing Reports
* Power Reports
* Placement Reports
* Post-Synthesis Netlists

---

# Important Commands Used

## Running VTR Flow

```bash
$VTR_ROOT/vtr_flow/scripts/run_vtr_flow.py \
counter.v \
$VTR_ROOT/vtr_flow/arch/timing/EArch.xml \
-temp_dir . \
-route_chan_width 100
```

---

## Running VPR GUI

```bash
$VTR_ROOT/vpr/vpr \
$VTR_ROOT/vtr_flow/arch/timing/EArch.xml \
counter.pre-vpr.blif \
--route_chan_width 100 \
--disp on
```
---

## Running VPR with Constraints

```bash
$VTR_ROOT/vpr/vpr \
$VTR_ROOT/vtr_flow/arch/timing/EArch.xml \
counter.pre-vpr.blif \
--route_chan_width 100 \
--sdc_file counter.sdc
```
---

## Generating Post-Synthesis Netlist

```bash
$VTR_ROOT/vpr/vpr \
$VTR_ROOT/vtr_flow/arch/timing/EArch.xml \
counter.pre-vpr.blif \
--gen_post_synthesis_netlist on
```
