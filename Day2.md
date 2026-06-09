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
