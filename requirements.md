# Requirements Document for RV32I Python Simulator Lab

## Objective
Develop a basic RISC-V RV32I instruction-level simulator in Python. The simulator will execute RISC-V programs provided in hexadecimal format, updating the architectural state accordingly. This lab introduces students to the RISC-V ISA and provides a tool for future assignments.

---

## Requirements

### Simulator Core Features
- **Fetch-Decode-Execute Loop**:
  - Implement a loop that fetches, decodes, and executes instructions sequentially.
- **Registers and Program Counter**:
  - 32 general-purpose registers (`x0` to `x31`), with `x0` always equal to zero.
  - A Program Counter (`PC`) that tracks the address of the next instruction.
- **Memory Model**:
  - Simulate byte-addressable memory with word-aligned access.
  - Support for loading and storing data.

### Instruction Set to Implement
- **Arithmetic and Logical Instructions**:  
  `ADD`, `ADDI`, `SUB`, `AND`, `ANDI`, `OR`, `ORI`, `XOR`, `XORI`, `SLT`, `SLTI`, `SLTU`, `SLTIU`, `SLL`, `SLLI`, `SRL`, `SRLI`, `SRA`, `SRAI`
- **Control Flow Instructions**:  
  `BEQ`, `BNE`, `BLT`, `BGE`, `BLTU`, `BGEU`, `JAL`, `JALR`, `AUIPC`, `LUI`
- **Memory Access Instructions**:  
  `LW`, `SW`, `LB`, `LBU`, `LH`, `LHU`, `SB`, `SH`
- **System Instructions**:  
  `ECALL` (used only to halt the simulator)

### Input Format
- The simulator accepts a text file containing hexadecimal representations of RISC-V instructions, one per line.
- **Example**:
    ```HEX
    00208F33 
    00100513
    ```

- Programs start execution at address `0x0`.

### Output Requirements
- After execution, output the final state:
  - Values of all registers (`x0` to `x31`).
  - Final `PC` value.
  - Contents of memory locations that were read or written.
  - Output format should be structured (e.g., JSON or plain text) to facilitate auto-grading.

### Execution Control
- The simulator halts when an `ECALL` instruction is executed.
- No need to implement system call handling beyond halting execution.

### Code Structure and Submission
- **File Structure**:
  - Implement the simulator in a single Python file named `rv32i_simulator.py`.
  - Use functions or classes to organize code (e.g., `fetch()`, `decode()`, `execute()`).
- **Coding Standards**:
  - Write clear, commented, and readable code.
  - Do not use external libraries beyond Python's standard library.
- **Deliverables**:
  - Submit `rv32i_simulator.py`.
  - Include a README with instructions on running the simulator and any assumptions made.

### Testing and Validation
- Ensure the simulator passes provided test cases.
- Students are encouraged to create additional test cases for thorough testing.
- The simulator's outputs must match expected results for given inputs to support auto-grading.

---

## Summary
Complete an RV32I simulator in Python that accurately models specified RISC-V instructions. The simulator should read instructions from a file, execute them while updating the architectural state, and produce outputs suitable for automated testing.
