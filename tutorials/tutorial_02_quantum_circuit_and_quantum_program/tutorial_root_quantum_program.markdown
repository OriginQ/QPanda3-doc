Quantum Program {#tutorial_quantum_program}
=============================================================

[TOC]

@prev_tutorial{tutorial_quantum_circuit}
@next_tutorial{tutorial_dynamic_circuit}

-------------------------------------------------------------------------------------------------------------------------------
# QProg

Quantum programming is used for writing and constructing quantum programs, generally understood as a sequence of operations. Due to the fact that classical computing is also included in quantum algorithms, the industry envisions that the quantum computers that will emerge in the near future will be hybrid structures, consisting of two main parts, one of which is a classical computer responsible for performing classical computing and control; The other part is quantum devices, responsible for performing quantum computing. QPanda3 regards the programming process of quantum programs as a part of classical program execution, and the entire peripheral host program must include the part that creates quantum programs.

## Constructors

You can create a [QProg](@ref pyqpanda3.core.core.QProg) instance using several constructors:

1. **Default Constructor**
@add_toggle_python
   ```python
   prog = QProg()
   ```
@end_toggle

2. **Constructor with Qubits Number**
@add_toggle_python
   ```python
   prog = QProg(qubits_num)
   ```
@end_toggle

3. **Copy Constructor**
@add_toggle_python
   ```python
   prog_copy = QProg(prog)
   ```
@end_toggle

4. **Constructor with a Circuit**
@add_toggle_python
   ```python
   circuit = QCircuit()
   prog = QProg(circuit)
   ```
@end_toggle

## Member Functions

The following functions can be used to manipulate and query the `QProg` instance:

- **Append Operations**

  You can append gates, circuits, or measurements:
@add_toggle_python
  ```python
  prog.append(H(0))
  prog.append(QCircuit())
  prog.append(measure(0,0))
  ```
@end_toggle

- **Operator Overloading**

  You can use the `<<` operator to append nodes:
@add_toggle_python
  ```python
  prog << H(0) << QCircuit() << measure(0,0)
  ```
@end_toggle


- **Querying Properties**

  Using the [qubits_num](@ref pyqpanda3.core.core.QProg.qubits_num) and [cbits_num](@ref pyqpanda3.core.core.QProg.cbits_num) function to get the number of qubits and classical bits:
@add_toggle_python
  ```python
  num_qubits = prog.qubits_num()
  num_cbits = prog.cbits_num()
  ```
@end_toggle

- **Get Qubits and Classical Bits**

  Using the [qubits](@ref pyqpanda3.core.core.QProg.qubits) and [cbits](@ref pyqpanda3.core.core.QProg.cbits) function to retrieve the lists of qubits and classical bits:
@add_toggle_python
  ```python
  qubits = prog.qubits()
  cbits = prog.cbits()
  ```
@end_toggle

- **Operations**

    Using the [operations](@ref pyqpanda3.core.core.QProg.operations) function to retrieve the operations:
@add_toggle_python
  ```python
  operations = prog.operations()
  ```
@end_toggle

- **Gate Operations**
  
    Using the [gate_operations](@ref pyqpanda3.core.core.QProg.gate_operations) function to get the list of gate operations,The meaning of the parameter is to only obtain two qubit gates:
@add_toggle_python
  ```python
  gates = prog.gate_operations(only_q2=False)
  ```
@end_toggle

- **Count Operations**
  
    Using the [count_ops](@ref pyqpanda3.core.core.QProg.count_ops) function to count the operations in the program,The meaning of the parameter is to only count two qubit gates:
@add_toggle_python
  ```python
  count = prog.count_ops(only_q2=False)
  ```
@end_toggle

- **Depth of the Program**

    Using the [depth](@ref pyqpanda3.core.core.QProg.depth) function to get the depth of the program,The meaning of the parameter is to only count two qubit gates:
@add_toggle_python
  ```python
  depth = prog.depth(only_q2=False)
  ```
@end_toggle

- **Flatten and Convert to Circuit**

     Using the [flatten](@ref pyqpanda3.core.core.QProg.flatten) and [to_circuit](@ref pyqpanda3.core.core.QProg.to_circuit) function to flatten the program or convert it to a circuit:
@add_toggle_python
  ```python
  flat_prog = prog.flatten()
  circuit = prog.to_circuit()
  ```
@end_toggle

- **Clearing the Program**

    Using the [clear](@ref pyqpanda3.core.core.QProg.clear) function to clear all operations from the program:
@add_toggle_python
```python
prog.clear()
```
@end_toggle


- **Convert to Instruction File**

    Use the [to_instruction](@ref pyqpanda3.core.core.QProg.to_instruction) function to convert the program into an instruction file supported by the Origin Quantum Cloud Chip.
Using this interface requires that the quantum program already consists entirely of the chip's basic instructions (RPhi, CZ, Measure, and ECHO).
`to_instruction` has three parameters. The first parameter is the specified chip backend. The second parameter is the offset of the qubit during the instruction conversion process. The third parameter is whether to use pattern to layer the instructions.
  @add_toggle_python
    ```python
    from pyqpanda3.core import *
    all_qbits = list(range(10))
    circuit = random_qcircuit(all_qbits, 10, ["RPHI", "CZ"])

    prog_1 = QProg()
    prog_1 << circuit
    for i in all_qbits:
        prog_1 << measure(i, i)

    json = prog_1.to_instruction(backend,0,False)
    print(json)
    ```
  @end_toggle


## Example Usage

Here’s a simple example of how to create a quantum program, run quantum program and measure results:

@add_toggle_python
@snippet samples/python/circuit_and_program.py QProg Example
@end_toggle

Run result:
@add_toggle_python
```python

          ┌─┐               ┌─┐
q_0:  |0>─┤H├ ───*── ───────┤M├ ────────── ────────── ───────
          └─┘ ┌──┴─┐        └╥┘        ┌─┐
q_1:  |0>──── ┤CNOT├ ───*────╫─ ───────┤M├ ────────── ───────
              └────┘ ┌──┴─┐  ║         └╥┘        ┌─┐
q_2:  |0>──── ────── ┤CNOT├──╫─ ───*────╫─ ───────┤M├ ───────
                     └────┘  ║  ┌──┴─┐  ║         └╥┘  ┌─┐
q_3:  |0>──── ────── ────────╫─ ┤CNOT├──╫─ ───*────╫─ ─┤M├───
                             ║  └────┘  ║  ┌──┴─┐  ║   └╥┘┌─┐
q_4:  |0>──── ────── ────────╫─ ────────╫─ ┤CNOT├──╫─ ──╫─┤M├
                             ║          ║  └────┘  ║    ║ └╥┘
 c :   / ════════════════════╩══════════╩══════════╩════╩══╩═
                              0          1          2    3  4


{'00000': 0.478515625, '11111': 0.521484375}
```
@end_toggle