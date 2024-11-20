# Verification Plan

| **Test Type**        | **Example**                                         | **Purpose**                                                             |
|-----------------------|-----------------------------------------------------|--------------------------------------------------------------------------|
| Unit Tests            | `test_register_initialization`                     | Verify isolated components like registers, PC, memory, decoding.         |
| Directed Tests        | `test_directed_add`, `test_beq`                    | Validate simulator behavior for specific instructions and scenarios.      |
| Constrained Random    | `test_random_instructions`                         | Explore unexpected combinations of instructions and states.               |
| Reference Comparison  | `validate_against_reference`                       | Verify outputs against a reference implementation or known outputs.       |
| Functional Coverage   | `test_instruction_coverage`                        | Ensure all instructions are tested and edge cases are explored.           |
| Checker Assertions    | `assert sim.pc % 4 == 0`                           | Verify simulator invariants at runtime.                                   |

---
### Unit Tests
#### Registers and Program Counter
```python
def test_register_initialization():
    sim = RV32ISimulator()
    assert sim.registers == [0] * 32, "Registers not initialized to zero."
    assert sim.registers[0] == 0, "Register x0 must always be zero."

def test_program_counter_initialization():
    sim = RV32ISimulator()
    assert sim.pc == 0, "Program counter not initialized to zero."

def test_program_counter_update():
    sim = RV32ISimulator()
    sim.pc = 0
    sim.pc += 4
    assert sim.pc == 4, "Program counter not incrementing correctly."


```

#### **Memory Operations**

```python
def test_memory_read_write():
    sim = RV32ISimulator()
    sim.memory[0x1000] = 42
    assert sim.memory[0x1000] == 42, "Memory write or read failed."

def test_memory_alignment():
    sim = RV32ISimulator()
    try:
        sim.memory[0x1001] = 10
        assert False, "Memory should enforce word alignment."
    except KeyError:
        pass  # Expected behavior
```
---
### **Directed Tests**

#### **Arithmetic and Logical Instructions**
```python
def test_add():
    sim = RV32ISimulator()
    sim.registers[1] = 10
    sim.registers[2] = 20
    sim.execute(0x002080B3)  # ADD x1, x1, x2
    assert sim.registers[1] == 30, "ADD instruction failed."

def test_sub():
    sim = RV32ISimulator()
    sim.registers[1] = 30
    sim.registers[2] = 10
    sim.execute(0x402080B3)  # SUB x1, x1, x2
    assert sim.registers[1] == 20, "SUB instruction failed."
```



#### **Memory Instructions**

```python
def test_lw_sw():
    sim = RV32ISimulator()
    sim.memory[0x1000] = 42
    sim.registers[1] = 0x1000
    sim.execute(0x00012003)  # LW x4, 0(x1)
    assert sim.registers[4] == 42, "LW instruction failed."

    sim.registers[5] = 99
    sim.execute(0x00512023)  # SW x5, 0(x1)
    assert sim.memory[0x1000] == 99, "SW instruction failed."
```

#### **Control Flow Instructions**
```python
def test_beq():
    sim = RV32ISimulator()
    sim.registers[1] = 5
    sim.registers[2] = 5
    sim.pc = 0
    sim.execute(0x00208663)  # BEQ x1, x2, offset=12
    assert sim.pc == 12, "BEQ instruction failed."

def test_bne():
    sim = RV32ISimulator()
    sim.registers[1] = 5
    sim.registers[2] = 10
    sim.pc = 0
    sim.execute(0x00209663)  # BNE x1, x2, offset=12
    assert sim.pc == 12, "BNE instruction failed."

```
---
### **Verification Tests**

#### **Test Hexadecimal Program**
```python
def test_hexadecimal_program():
    sim = RV32ISimulator()
    program = [0x002081B3, 0x00108093]  # ADD x3, x1, x2 and ADDI x1, x1, 1
    sim.load_program(program)
    sim.run()
    assert sim.registers[3] == sim.registers[1] + sim.registers[2], "ADD failed in program."
    assert sim.registers[1] == 1, "ADDI failed in program."

```
#### **Branch Program**
```python
def test_branch_program():
    sim = RV32ISimulator()
    sim.registers[1] = 5
    sim.registers[2] = 5
    program = [0x00208663, 0x00008067]  # BEQ x1, x2, offset=12 and JALR x0, x0, 0
    sim.load_program(program)
    sim.run()
    assert sim.pc == 12, "BEQ failed in branch program."
```

---


### **Validation Tests**

#### **Validation with Known Output**
```python
def validate_against_reference():
    sim = RV32ISimulator()
    reference_output = {
        "registers": [0] * 32,
        "pc": 8,
    }
    reference_output["registers"][1] = 1
    reference_output["registers"][3] = reference_output["registers"][1] + reference_output["registers"][2]

    program = [0x00108093, 0x002081B3]  # ADDI x1, x1, 1 and ADD x3, x1, x2
    sim.load_program(program)
    sim.run()

    assert sim.registers == reference_output["registers"], "Registers do not match reference output."
    assert sim.pc == reference_output["pc"], "PC does not match reference output."

```
---
