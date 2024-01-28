# Pipelined SIMD Multimedia Unit

## Objective
To use VHDL hardware description language and modern CAD tools for the structural and behavioral design of a four-stage pipelined multimedia unit with a reduced set of multimedia instructions similar to those in the Sony Cell SPU and Intel SSE architectures.

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

## Instruction Format and Opcode Description
### Load Immediate
![LI format figure](https://github.com/HaoRanYuan0/Pipelined-SIMD-Multimedia-Unit/assets/121404407/98d7acc4-d9f1-4bdb-a5c3-e48699135943)

li: Load a 16-bit Immediate value from the [20:5] instruction field into the 16-bit field specified by the Load Index field [23:21] of the 128-bit register rd. Other fields of register rd are not changed. A li instruction first reads register rd and then (after inserting an immediate value into one of its fields) writes it back to register rd.

### Multiply-Add and Multiply-Subtract R4-Instruction Format
![R4 format figure](https://github.com/HaoRanYuan0/Pipelined-SIMD-Multimedia-Unit/assets/121404407/20e61e8a-2615-4e30-a9f3-44f0e8cfdac5)

Signed operations are performed with saturated rounding that takes the result, and sets a floor and ceiling corresponding to the max range for that data size. This means that instead of over/underflow wrapping, the max/min values are used.
| Data Type | Min | Max |
| --------- | --- | --- |
| Long (64-bit) | -2^-63 | 2^63 - 1 |
| Int (32-bit) | -2^-31 | 2^31 - 1 |

The tables below show the description for each operation:
![image](https://github.com/HaoRanYuan0/Pipelined-SIMD-Multimedia-Unit/assets/121404407/d76b0872-cf38-43cf-ab9c-39f4f7a0de44)

### R3 Instruction Format
![R3 instruction format](https://github.com/HaoRanYuan0/Pipelined-SIMD-Multimedia-Unit/assets/121404407/875285c5-43d7-41da-9533-51bc8805cff2)
In the table below, 16-bit signed integer add (AHS), and subtract (SFHS) operations are performed with saturation to signed halfword rounding that takes a 16-bit signed integer X, and converts it to -32768 (the most negative 16-bit signed value) if it is less than -32768, to +32767 (the highest positive 16-bit signed value) if it is greater than 32767, and leaves it unchanged otherwise.
![image](https://github.com/HaoRanYuan0/Pipelined-SIMD-Multimedia-Unit/assets/121404407/36929fed-b538-4961-94b3-9d0a7827e38f)

