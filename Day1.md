# Day 1 – FPGA Architecture Fundamentals and Vivado Workflow

## Objective

Day 1 focused on understanding FPGA fundamentals, FPGA architecture, and the complete Vivado design flow through the implementation of a simple digital design on the Basys 3 FPGA board.

### Topics Covered

* Introduction to FPGA and Programmable Logic
* FPGA vs ASIC Comparison
* FPGA Architecture Overview
* LUTs, CLBs, Flip-Flops, and Routing Resources
* Vivado RTL-to-Bitstream Flow
* Behavioral Simulation
* Synthesis and Implementation
* Timing, Power, and Utilization Analysis
* Constraints and Pin Mapping
* Virtual I/O (VIO)
---
# Introduction to FPGA

A **Field Programmable Gate Array (FPGA)** is a reconfigurable integrated circuit that can be programmed by the user after manufacturing. Unlike ASICs, which are designed for a fixed function, FPGAs can be reprogrammed to implement different digital systems and applications.

## Common Applications

* Hardware Acceleration
* Digital Signal Processing (DSP)
* Embedded Systems
* Machine Learning and AI
* Aerospace and Defense Systems
* High-Performance Computing (HPC)
  
## FPGA vs ASIC

| FPGA | ASIC |
|---|---|
| Reprogrammable | Fixed after fabrication |
| Faster prototyping | Long fabrication cycle |
| Lower initial cost | High initial fabrication cost |
| Higher flexibility | Optimized performance |
| RTL to Bitstream | RTL to Layout |

---

# FPGA Architecture

An FPGA is composed of several programmable resources that work together to implement digital designs:

* Configurable Logic Blocks (CLBs)
* Look-Up Tables (LUTs)
* Flip-Flops (FFs)
* Programmable Interconnects
* Block RAM (BRAM)
* DSP Blocks
* I/O Blocks

## Configurable Logic Blocks (CLBs)

CLBs are the primary logic elements within an FPGA and are responsible for implementing digital circuits. A typical CLB contains:

* LUTs for combinational logic
* Flip-flops for sequential logic
* Carry chains for arithmetic operations
* Multiplexers and routing resources

## Look-Up Tables (LUTs)

LUTs implement combinational logic by storing output values in memory. An N-input LUT contains **2ⁿ** memory locations, allowing it to realize any logic function with N inputs.

For example, a **3-input LUT** can implement any Boolean function of three variables.

---

# FPGA Design Flow

The FPGA implementation process in Vivado typically follows these stages:

1. RTL Design
2. Testbench Creation
3. Behavioral Simulation
4. Synthesis
5. Constraint Definition
6. Placement and Routing
7. Timing Analysis
8. Bitstream Generation
9. FPGA Programming

---

# Basys 3 FPGA Board

The **Basys 3** is an FPGA development board based on the **Xilinx Artix-7** FPGA. It was used throughout the workshop for design implementation, simulation, and hardware validation.

## Key Components

| Component         | Function                       |
| ----------------- | ------------------------------ |
| Push Buttons      | User input                     |
| Slide Switches    | Logic input selection          |
| LEDs              | Visual output indication       |
| 7-Segment Display | Numeric data display           |
| VGA Port          | Video output interface         |
| USB/JTAG Port     | FPGA programming               |
| Clock Oscillator  | System clock generation        |

---

# Vivado Counter Design

A 4-bit up counter was designed and implemented using Verilog HDL on the Basys 3 FPGA board.

## Design Features

* Counts upward on each rising edge of the clock
* Supports synchronous reset functionality
* Displays the counter value through onboard LEDs
* Verified using simulation and hardware implementation in Vivado

---

# Counter Code
```verilog
module counter_clk_div(clk,rst,counter_out);

input clk,rst;
reg div_clk;
reg [25:0] delay_count;
output reg [3:0] counter_out;

always @(posedge clk)
begin
    if(rst)
    begin
        delay_count <= 26'd0;
        div_clk <= 1'b0;
        counter_out <= 4'b0000;
    end
    else
    begin
        if(delay_count == 26'd212)
        begin
            delay_count <= 26'd0;
            div_clk <= ~div_clk;   
        end
        else
        begin
            delay_count <= delay_count + 1;
        end
    end
end

always @(posedge div_clk)
begin
    if(rst)
    begin
        counter_out <= 4'b0000;
    end
    else
    begin
        counter_out <= counter_out + 1;
    end
end

endmodule

```
---
# Behavioral Simulation

Behavioral simulation was carried out in Vivado to verify the counter design before synthesis and hardware implementation.

## Verification Results

* Correct clock operation
* Proper counter incrementing
* Successful reset functionality
* Expected waveform behavior

## Simulation Output

The generated waveform confirmed that the design operated as intended under all test conditions.

<img width="2461" height="865" alt="image" src="https://github.com/user-attachments/assets/fd99253a-6d8f-4b2d-8926-ccff40a5bb9c" />

---

# RTL Elaboration
RTL elaboration converts Verilog HDL into an RTL schematic representation.

This stage verifies:

* Logical connectivity
* Module hierarchy
* Register structure
* Signal flow

## RTL Schematic
The generated RTL schematic was used to verify the correctness of the design structure and signal interconnections.

<img width="1034" height="433" alt="image" src="https://github.com/user-attachments/assets/d466bb00-a87c-4b8e-95ec-9e8ecca5523d" />

---

# Constraints and Pin Mapping

Constraints were added using the `.xdc` file.

The constraints file maps:
- Clock input pin
- Reset input pin
- Output LEDs

## Constraint.xdc:

<img width="530" height="274" alt="image" src="https://github.com/user-attachments/assets/9ee6aa88-8040-4833-a337-30e7b6d7b953" />

## Package Mapping

<img width="878" height="433" alt="image" src="https://github.com/user-attachments/assets/966cef19-4665-4424-a337-1f88398cdab5" />

---

# Synthesis

Synthesis transforms the RTL design into FPGA hardware primitives such as:

* Look-Up Tables (LUTs)
* Flip-Flops (FFs)
* Carry Chains

During this stage, logic optimization is performed, resource usage is estimated, and preliminary timing information is generated.

---

# Timing Analysis

Timing analysis verifies that all signals propagate through the design within the required clock period.

## Setup Timing

Setup timing ensures that data arrives at the destination register before the active clock edge.

T_{cq}+T_{logic}<T_{clock}-T_{setup}

## Hold Timing

Hold timing ensures that data remains stable for a sufficient duration after the clock edge to guarantee correct operation.

## Slack

Slack represents the timing margin between the required arrival time and the actual arrival time.

Slack = Required\ Time - Arrival\ Time

A positive slack value indicates that the timing requirements have been successfully met.

## Design Timing Summary

<img width="1498" height="334" alt="image" src="https://github.com/user-attachments/assets/63f32b0e-f1b2-444c-8870-ff97372ce6cb" />

---
# Device Utilization
Vivado generates utilization reports showing FPGA resource consumption.

Resources analyzed:

* LUTs
* Flip-Flops
* I/O pins
* BRAM
  
## Utilization Report

<img width="1204" height="543" alt="image" src="https://github.com/user-attachments/assets/372f1176-a096-45b1-beef-57d3fdd53e4f" />

---

# Power Analysis
Power analysis estimates:

* Dynamic power
* Static power
* Clock power
* Signal power
## Power Report

<img width="1539" height="658" alt="image" src="https://github.com/user-attachments/assets/bb3fdfe1-18c0-411a-8039-3d9dc2c985da" />

---
# Virtual Input/Output (VIO)

Virtual Input/Output (VIO) is a Vivado debugging IP that enables real-time monitoring and control of internal FPGA signals through the Hardware Manager without requiring external I/O pins.

## Applications

* Internal signal debugging
* Real-time signal monitoring
* Runtime testing and verification
* Design validation on hardware
