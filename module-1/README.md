Module 1 – Verilog RTL Design and Simulation
Overview

This module introduces the fundamentals of Verilog HDL, RTL design, simulation, verification, and synthesis.

The objective is to understand how a digital circuit is described using Verilog, verified through simulation, and converted into a gate-level representation using Yosys and a standard-cell library.

Learning Objectives
Understand the basics of Verilog HDL.
Learn the roles of a Design, Testbench, and Simulator.
Simulate a simple digital circuit using Icarus Verilog.
Analyze simulation waveforms using GTKWave.
Understand the basic RTL-to-gate synthesis flow.
Learn the purpose of Liberty (.lib) files.
Perform technology mapping using Yosys and the Sky130 standard-cell library.

1. Verilog RTL Fundamentals
Simulator
A simulator is a software tool used to execute Verilog code and verify the behavior of a digital design before hardware implementation.

Design
The design is the Verilog module that describes the required digital circuit. It defines the inputs, outputs, and logic functionality.

Testbench
A testbench is a separate Verilog module used to apply test inputs to the design and observe its outputs.

Basic Verification Flow

Design → Testbench → Simulation → Waveform Analysis

2. Icarus Verilog

What is Icarus Verilog?
Icarus Verilog is an open-source Verilog compiler and simulator used to compile RTL code, execute simulations, and generate waveform files.

Installation
sudo apt install iverilog

Install GTKWave for waveform analysis:

sudo apt install gtkwave
3. Lab – 2-to-1 Multiplexer

A 2-to-1 multiplexer selects one of two inputs based on a select signal.

Inputs and Output
Signal	Description
i0	Input 0
i1	Input 1
sel	Select signal
y	Output
Operation
When sel = 0, y = i0
When sel = 1, y = i1
Simulation

Compile the design and testbench:

iverilog good_mux.v tb_good_mux.v

Run the simulation:

./a.out

Open the generated waveform:
gtkwave tb_good_mux.vcd

Result
The waveform confirms that the multiplexer correctly selects the required input according to the select signal.
![good mux](<img width="1280" height="835" alt="Image" src="https://github.com/user-attachments/assets/f8f507f5-6c23-43ab-b41f-330fc0155dcd" />)

4. Verilog Code Analysis

The multiplexer uses an always @(*) procedural block.

always @(*) begin
    if (sel)
        y = i1;
    else
        y = i0;
end
Working
always @(*) responds to changes in the input signals.
The sel signal determines which input is connected to the output.
The output continuously follows the selected input.
This coding style represents combinational logic.
5. RTL Synthesis with Yosys
What is Yosys?

Yosys is an open-source RTL synthesis tool that converts Verilog RTL into a gate-level netlist.

The basic synthesis process is:

RTL → Optimization → Technology Mapping → Gate-Level Netlist

Liberty Standard-Cell Library

A Liberty (.lib) file contains information about standard cells, including:

Cell functionality
Timing characteristics
Area
Power
Drive strength

Yosys uses this information during technology mapping.

6. Yosys Synthesis Lab
Start Yosys
yosys
Load the Standard-Cell Library
read_liberty -lib /home/vsduser/VLSI/sky130RTLDesignAndSynthesisWorkshop/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
Load the Verilog Design
read_verilog /home/vsduser/VLSI/sky130RTLDesignAndSynthesisWorkshop/verilog_files/good_mux.v
Run RTL Synthesis
synth -top good_mux
Perform Technology Mapping
abc -liberty /home/vsduser/VLSI/sky130RTLDesignAndSynthesisWorkshop/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
View the Synthesized Circuit
show
7. Synthesis Result

The good_mux design was successfully synthesized using Yosys.

RTL was analyzed and optimized.
The Sky130 standard-cell library was loaded.
Logic was mapped to standard cells.
A gate-level representation of the multiplexer was generated.
The synthesized circuit was viewed using Yosys.
8. Tools Used
Tool	Purpose
Verilog HDL	RTL design
Icarus Verilog	Compilation and simulation
GTKWave	Waveform analysis
Yosys	RTL synthesis
Sky130	Standard-cell library
9. Key Takeaways
Verilog HDL can be used to describe digital hardware at the RTL level.
A testbench is essential for verifying RTL functionality.
Icarus Verilog can compile and simulate Verilog designs.
GTKWave helps analyze simulation waveforms.
Yosys converts RTL into a synthesized gate-level representation.
Liberty files provide standard-cell information for technology mapping.
The 2-to-1 multiplexer demonstrates the complete RTL → Simulation → Synthesis workflow.
