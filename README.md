# Pipelined SIMD Multimedia Unit

## Objective
To use VHDL/Verilog hardware description language and modern CAD tools for the structural and behavioral design of a four-stage pipelined multimedia unit with a reduced set of multimedia instructions similar to those in the Sony Cell SPU and Intel SSE architectures.

![image002](https://github.com/HaoRanYuan0/Pipelined-SIMD-Multimedia-Unit/assets/121404407/38ef0a7f-161d-4880-9bf4-7afec1fc9130)

## Components
The complete 4-stage pipelined design is developed in a structural/RTL manner with several modules operating simultaneously. Each module represents a pipelined stage with its interstage register. The major units inside those stages modules are described below.

### Multimedia ALU
Takes up to three inputs from the Register File, and calculates the result based on the current instruction to be performed.
### Register File
The register file has 32 128-bit registers. On any cycle, there can be 3 reads and 1 write. When executing instructions, each cycle two/three 128-bit register values are read, and one 128-bit result can be written if a write signal is valid.
### Instruction Buffer
The instruction buffer can store 64 25-bit instructions. The contents of the buffer are loaded from a test file at the start of simulation. On each cycle, the instruction specified by the Program Counter (PC) is fetched, and the value of PC is incremented by 1.
### Forwarding Unit
Every instruction uses the most recent value of a register, even if this value has not yet been written to the Register File. 
### Four-Stage Pipelined Multimedia Unit
Clock edge-sensitive pipeline registers separate the IF, ID, EXE, and WB stages. Data are written to the Register File after WB Stage. All instructions (including li) take four cycles to complete. This pipeline is implemented as a structural model with modules for each corresponding pipeline stages and their interstage registers. Four instructions can be at different stages of the pipeline at every cycle.
### Assembler
This is a separate program written in Python. Its purpose is to convert an assembly file to the binary format for the Instruction Buffer.
### Results File
This file shows the outputs of the pipeline.
