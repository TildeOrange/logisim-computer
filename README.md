# logisim-computer
An implementation of SAP-1 in Logisim | Boyd Thomas - Dec. 2020

## Summary
This circuit is a loose implementation of Albert Paul Malvino's SAP-1 computer. It uses 4-bit addresses, 8-bit registers, 8-bit instructions, and has eight instructions.

## Memory
The memory in this computer is very limited (only 15 bytes). This memory holds the program and any memory that is written to or read from by that program.

## Instructions
Each instruction is eight bits. The four most significant bits are auxiliary bits. These are used by the computer to execute the instruction. Currently, the aux bits are used as an address for accessing memory or jumping to a location in memory.
| Name  | Lower 4 bits  | Upper 4 bits  | Description                                                                           |
| ----- | ------------- | ------------  | ------------------------------------------------------------------------------------- |
| OUT   | 0x0           | Address       | Puts memory at address on the output register (which displays its value)              |
| LDA   | 0x1           | Address       | Loads memory at address into A register                                               |
| STA   | 0x2           | Address       | Stores A register in memory at address                                                |
| ADD   | 0x3           | Address       | Adds memory at address to value in A register and stores result in A register         |
| JMP   | 0x4           | Address       | Sets program counter to address in memory                                             |
| SUB   | 0x5           | Address       | Subtracts memory at address from value in A register and stores result in A register  |
| MAB   | 0x6           | N/A           | Moves value in A register to B register                                               |
| MBA   | 0x7           | N/A           | Moves value in B register to A register                                               |

## Sample Programs
### Counter Program

This program starts counting at zero. Then, increments twice and decrements once while displaying the count.
```
LDA 0xF
ADD 0xE
STA 0xD
OUT 0xD
ADD 0xE
STA 0xD
OUT 0xD
SUB 0xE
STA 0xD
OUT 0xD
JMP 0x1
```
RAM: `0xF1 0xE3 0xD2 0xD0 0xE2 0xD2 0xD0 0xE5 0xD2 0xD0 0x14 0x00 0x00 0x00 0x01 0x00`
### Quine 

This program's only output is its source.

RAM: `0x00 0x10 0x20 0x30 0x40 0x50 0x60 0x70 0x80 0x90 0xA0 0xB0 0xC0 0xD0 0xE0 0xF0`

---

# Supplemental Information

## Block Diagram
![Block Diagram](https://i.imgur.com/hEdhBsG.png)

## Control Logic
The control logic has a 3-bit counter, a decoder, and a ROM. The microinstruction counter steps through each step of an instruction. The decoder is used to simplify the loading of the next instruction. Finally, the ROM holds the control signals that should be set for each step in an instruction. This ROM is split into 8-byte divisions. Only the first five bytes of these divisions are usable. These bits are incremented through until the 16th bit is set - which resets the counter - or the counter overflows.

## Control Signals
These signals control each module and are read from a ROM in the control logic that holds microinstructions.
| Abbrev. | Name                        | Description                                                                                                   |
| ------- | --------------------------- | ------------------------------------------------------------------------------------------------------------- |
| J       | Jump                        | Loads address into program counter                                                                            |
| CO      | Counter Out                 | Puts program counter value onto bus                                                                           |
| CE      | Counter Enable              | Increments counter                                                                                            |
| MAI     | Memory Address Register In  | Reads bus value into memory address register                                                                  |
| MI      | Memory In                   | Stores value on bus at current memory address                                                                 |
| MO      | Memory Out                  | Puts value onto bus from current memory address                                                               |
| SO      | Sum Out                     | Puts ALU result onto bus                                                                                      |
| SS      | Sum Subtract                | Puts ALU into subtract mode                                                                                   |
| II      | Instruction Register In     | Reads bus value into instruction register                                                                     |
| IO      | Instruction Register Out    | Writes four most significant bits in the instruction register into the four least significant bits on the bus |
| AI      | A Register In               | Reads bus value into A register                                                                               |
| AO      | A Register Out              | Writes A register value onto bus                                                                              |
| BI      | B Register In               | Reads bus value into B register                                                                               |
| BO      | B Register Out              | Writes B register value onto bus                                                                              |
| OI      | Output Register In          | Reads bus value into output register                                                                          |
