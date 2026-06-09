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

# Design Source

## Files Used

* `mythcore_test.v`
* `mythcore_test_gn.v`

## Reference Repository

*https://github.com/shivanishah269/risc-v-core/tree/master*

---

## FPGA Design Flow

The Mythcore processor was implemented using the standard FPGA design flow:

1. RTL Design
2. Functional Simulation
3. Synthesis
4. Netlist Generation
5. Placement and Routing
6. Timing Analysis
7. Bitstream Generation
8. Hardware Debugging using ILA

## RTL Simulation

RTL simulation was performed to verify the functionality of the Mythcore processor before synthesis and implementation.

### Observations

* Correct Clock Operation
* Successful Reset Initialization
* Expected Processor Execution
* Functional Behavior Verification

### RTL Simulation Output
