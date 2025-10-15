OriginIR  {#tutorial_quantum_program_of_content_OriginIR}
=========================================================

[TOC]

@prev_tutorial{tutorial_quantum_circuit_transpiler}
@next_tutorial{tutorial_quantum_program_of_content_QASM}

-------------------------------------------------------------------------------------------------------------------------------

### Overview

OriginIR is an intermediate representation of quantum programs based on QPanda3, playing a crucial role in supporting various features of QPanda3. 

The main components of OriginIR include quantum bits, classical registers, quantum logic gates, transpose conjugate operations, control qubit operations, QIf, QWhile, and classical expressions.

@note
Currently, the new release of QPanda3 has the following updates compared to the older version of QPanda2 regarding the OriginIR module:
- First, QPanda3's OriginIR interface for translating QProg supports program comment parsing.
- Second, the OriginIR part of QPanda3 only supports operations such as applying quantum bit and register, quantum logic gate, transposing conjugate (DAGGER), adding control bit (CONTROL), BARRIER, and measurement, but not operations such as quantum conditional judgment (QIF), quantum cyclic control (QWHILE), classical expressions, RESET, and QGATE.
- Finally, when executing large quantum program translations, compared to QPanda2's translation interface, QPanda3's translation interface is executed about 4.5 times more efficiently (located in Ubuntu 22.04 environment).
@code{.c}
// QPanda2's translation interface
def convert_originir_to_qprog(file_path: str, machine: QuantumMachine) -> list
def convert_originir_str_to_qprog(ir_str: str, machine: QuantumMachine) -> list

// QPanda3's translation interface
def convert_originir_file_to_qprog(file_path: str) -> QProg
def convert_originir_string_to_qprog(ir_str: str) -> QProg
@endcode


### Quantum Bit

OriginIR uses `QINIT` to allocate quantum bits, with the format being `QINIT` followed by a space and the total number of classical registers. 
An example is as follows:
@code{.c}
    QINIT 6
@endcode

@note
Except for comments, `QINIT` must appear on the first line of the OriginIR program. 

When using quantum bits, OriginIR represents a specific quantum bit as `q[i]`, where `i` is the quantum bit index. The `i` can be an unsigned integer constant, a variable, or an expression using `c[i]` as a substitute. An example is as follows:
@code{.c}
    q[1], q[c[0]], q[c[1] + c[2] + c[3]]
@endcode


### Classical Register

OriginIR uses `CREG` to allocate classical registers, with the format being `CREG` followed by a space and the total number of classical registers. An example is as follows:
@code{.c}
    CREG 6
@endcode

When using classical registers, OriginIR represents a specific classical register as `c[i]`, where `i` is the classical register index. Here, `i` must be an unsigned integer constant. An example is as follows:
@code{.c}
    c[1]
@endcode


### Quantum Logic Gate

OriginIR categorizes quantum logic gates into the following types:

- Single-gate without parameters: The keywords include `H`, `T`, `S`, `X`, `Y`, `Z`, `X1`, `Y1`, `Z1`, and `I`, representing single quantum logic gates with no parameters. The format for usage is "quantum logic gate keyword + space + target quantum bit". An example is as follows:
@code{.c}
    H q[0]
@endcode

- Single-gate with one parameter: The keywords include `RX`, `RY`, `RZ`, `U1`, and `P`, representing single quantum logic gates with one parameter. The format for usage is "quantum logic gate keyword + space + target quantum bit + comma + (rotation angle)". An example is as follows:
@code{.c}
    RX q[0],(1.570796)
@endcode

- Single-gate with two parameters: The keywords include `U2` and `RPHI`, representing single quantum logic gates with two parameters. The format for usage is "quantum logic gate keyword + space + target quantum bit + comma + (two rotation angles)". An example is as follows:
@code{.c}
    U2 q[0],(1.570796,-3.141593)
@endcode

- Single-gate with three parameters: The keyword includes `U3`, representing a single quantum logic gate with three parameters. The format for usage is "quantum logic gate keyword + space + target quantum bit + comma + (three rotation angles)". An example is as follows:
@code{.c}
    U3 q[0],(1.570796,4.712389,1.570796)
@endcode

- Single-gate with four parameters: The keyword includes `U4`, representing a single quantum logic gate with four parameters. The format for usage is "quantum logic gate keyword + space + target quantum bit + comma + (four rotation angles)". An example is as follows:
@code{.c}
    U4 q[1],(3.141593,4.712389,1.570796,-3.141593)
@endcode

- Two-gate without parameters: The keywords include `CNOT`, `CZ`, `ISWAP`, and `SWAP`, representing two quantum logic gates with no parameters. The format for usage is "quantum logic gate keyword + space + control qubit + comma + target qubit". An example is as follows:
@code{.c}
    CNOT q[0],q[1]
@endcode

- Two-gate with one parameter: The keywords include `CP`,`CR`,`RXX`, `RYY`, `RZZ`, and `RZX`, representing two quantum logic gates with one parameter. The format for usage is "quantum logic gate keyword + space + control qubit + comma + target qubit + comma + (rotation angle)". An example is as follows:
@code{.c}
    CR q[0],q[1],(1.570796)
@endcode

- Two-gate with four parameters: The keyword includes `CU`, representing a two quantum logic gate with four parameters. The format for usage is "quantum logic gate keyword + space + control qubit + comma + target qubit + comma + (four rotation angles)". An example is as follows:
@code{.c}
    CU q[1],q[3],(3.141593,4.712389,1.570796,-3.141593)
@endcode

- Three-gate without parameters: The keyword includes `TOFFOLI`, representing a three quantum logic gate with no parameters. The format for usage is "quantum logic gate keyword + space + control qubit 1 + comma + control qubit 2 + comma + target qubit". An example is as follows:
@code{.c}
    TOFFOLI q[0],q[1],q[2]
@endcode

@note
It is important to note that for all single-gate operations, the target quantum bit can be either an entire array of quantum bits or a single quantum bit. When it is an entire array of quantum bits, for example:
@code{.c}
    H q
@endcode
When the size of the quantum bit array is 3, it is equivalent to:
@code{.c}
    H q[0]
    H q[1]
    H q[2]
@endcode


### Transpose Conjugate Operation

In OriginIR, you can perform a transpose conjugate operation on one or more quantum logic gates. OriginIR uses the `DAGGER` and `ENDDAGGER` keywords to define the scope of the transpose conjugate operation. Each `DAGGER` must have a matching `ENDDAGGER`. The example is as follows:
@code{.c}
    DAGGER
    H q[0]
    CNOT q[0],q[1]
    ENDDAGGER
@endcode


### Adding Control Bit Operation

OriginIR can add control qubits to one or more quantum logic gates using the `CONTROL` and `ENDCONTROL` keywords to define the range for adding control qubits. The `CONTROL` is followed by a space and the list of control qubits. Here's an example:
@code{.c}
    CONTROL q[2],q[3]
    H q[0]
    CNOT q[0],q[1]
    ENDCONTROL
@endcode


### Custom Gate

OriginIR supports custom gate operation, allowing multiple logic gates to be combined into a new gate for use. It uses the keywords `QGATE` and `ENDQGATE` to define the scope of the custom gate. Note that the parameter names of the custom gate must not conflict with the aforementioned keywords. An example is provided below:
@code{.c}
    QGATE new_H a
    H a
    X a
    ENDQGATE
    new_H q[1]
    QGATE new_RX a,(b)
    RX a,(PI/2+b)
    X a
    ENDQGATE
    new_RX q[1],(PI/4)
@endcode


### QIF

In OriginIR, quantum conditional branching can be expressed using `QIF`, `QELSEIF`, `QELSE`, and `QENDIF` to define the scope of different branches. Note that every `QIF` must be matched with a `QENDIF`. If a `QIF` has multiple conditional branches, they must be paired with `QELSEIF` or `QELSE`. An example is provided below:
@code{.c}
    QINIT 4
    CREG 4

    X q[0]

    QIF c[0],c[1],c[2]
    Y q[0]
    MEASURE q[3],c[3]
    QELSEIF c[0]                                                                                                                           
    X q[2]
    MEASURE q[3],c[3]
    QELSEIF c[1]
    I q[3]
    MEASURE q[3],c[3]
    QELSE
    H q[1]
    MEASURE q[3],c[3]
    QENDIF
@endcode


### QWHILE

OriginIR supports quantum loop operations, which are defined using `QWHILE` and `QENDWHILE` to delineate the loop scope. Note that each `QWHILE` must be paired with a corresponding `QENDWHILE`. An example is shown below:
@code{.c}
    QINIT 2
    CREG 2

    X q[1]

    QWHILE c[1]
    X q[0]
    X q[1]
    MEASURE q[1],c[1]
    QENDWHILE
@endcode


### MEASURE

In OriginIR, `MEASURE` represents a measurement operation on the specified quantum bit, with the result stored in the specified classical register. `MEASURE` is followed by a space, the target quantum bit, a comma, and the target classical register. Here's an example:
@code{.c}
    MEASURE q[0],c[0]
@endcode

If the number of requested quantum qubits and classical registers is the same, `q` can be used to represent all quantum qubits, and `c` can be used to represent all classical bits. Here's an example:
@code{.c}
    MEAUSRE q,c
@endcode

If the number of quantum qubits and classical bits is both 3, it is equivalent to:
@code{.c}
    MEAUSRE q[0],c[0]
    MEAUSRE q[1],c[1]
    MEAUSRE q[2],c[2]
@endcode


### BARRIER

The `BARRIER` operation in OriginIR is used to block the qubits involved in the operation, preventing optimization and execution during the process. The format is `BARRIER` followed by a space and the target qubits. The target qubits can be a whole array of qubits or individual/multiple qubits. Here’s an example:
@code{.c}
    BARRIER q
    BARRIER q[0]
    BARRIER q[0],q[1],q[2]
@endcode


### OriginIR Program Example

Here is an example of an OriginIR quantum program that characterizes the OPE algorithm:
@code{.c}
    QINIT 3
    CREG 2
    H q[2]
    H q[0]
    H q[1]
    CONTROL q[1]
    RX q[2],(-3.141593)
    ENDCONTROL
    CONTROL q[0]
    RX q[2],(-3.141593)
    RX q[2],(-3.141593)
    ENDCONTROL
    DAGGER
    H q[1]
    CR q[0],q[1],(1.570796)
    H q[0]
    ENDDAGGER
    MEASURE q[0],c[0]
    MEASURE q[1],c[1]
@endcode


### OriginIR Conversion Tool

QPanda3 provides a set of OriginIR conversion tools, mainly used to translate quantum programs between QProg and OriginIR. OriginIR is an intermediate representation (IR) used to represent the information of quantum programs. QProg is a container class for quantum programming and represents the highest-level unit of a quantum program. Below, we will provide a detailed introduction to the interface definitions and usage of these conversion tools.

- Translate the instruction set string in OriginIR format into a [QProg](@ref pyqpanda3.core.core.QProg) object.
- Translate the instruction set file in OriginIR format into a [QProg](@ref pyqpanda3.core.core.QProg) object.
- Translate the [QProg](@ref pyqpanda3.core.core.QProg) object into an instruction set string in OriginIR format.


#### OriginIR String Translated to QProg

The compilation module of QPanda3 defines [convert_originir_string_to_qprog](@ref pyqpanda3.intermediate_compiler.intermediate_compiler.convert_originir_string_to_qprog) for translating the OriginIR instruction set string into a QProg object.

Below is a simple interface call example to demonstrate the process of converting an OriginIR instruction set string into a quantum program (QProg):

@add_toggle_python
    @snippet samples/python/convert_originir_string_to_qprog.py  Example of testing OriginIR String to Qprog
@end_toggle

The output is as follows:
@code{.bash}
Result(stv) of running qprog:

( 3.6742350925920025e-07 , -0.7071067811864525 )
( 0.0 , 0.0 )
( 0.0 , 0.0 )
( 0.0 , 0.0 )
( 3.6742350925920025e-07 , -0.7071067811864525 )
( 0.0 , 0.0 )
( 0.0 , 0.0 )
( 0.0 , 0.0 )

          ┌─┐                  ┌──────────────┐ ┌──────────────┐  ┌─┐    
q_0:  |0>─┤H├ ──────────────── ┤0             ├ ┤0             ├ ─┤M├─── 
          ├─┤ ┌──────────────┐ │              │ │  QCircuit_2  │  └╥┘┌─┐ 
q_1:  |0>─┤H├ ┤0             ├ ┤  QCircuit_1  ├ ┤1             ├ ──╫─┤M├ 
          ├─┤ │  QCircuit_0  │ │              │ └──────────────┘   ║ └╥┘ 
q_2:  |0>─┤H├ ┤1             ├ ┤1             ├ ──────────────── ──╫──╫─ 
          └─┘ └──────────────┘ └──────────────┘                    ║  ║  
 c :   / ══════════════════════════════════════════════════════════╩══╩═
                                                                    0  1
@endcode

@note
For operation types that are not supported, errors may occur during the conversion of OriginIR into a quantum program.


#### OriginIR File Translated to QProg

The compilation module of QPanda3 defines [convert_originir_file_to_qprog](@ref pyqpanda3.intermediate_compiler.intermediate_compiler.convert_originir_file_to_qprog) for translating the OriginIR instruction set file into a QProg object.

Below is a simple interface call example to demonstrate the process of converting an OriginIR instruction set file into a quantum program (QProg):

@add_toggle_python
    @snippet samples/python/convert_originir_file_to_qprog.py  Example of testing OriginIR File to Qprog
@end_toggle

The output is as follows:
@code{.bash}
Result(stv) of running qprog:

( 3.6742350925920025e-07 , -0.7071067811864525 )
( 0.0 , 0.0 )
( 0.0 , 0.0 )
( 0.0 , 0.0 )
( 3.6742350925920025e-07 , -0.7071067811864525 )
( 0.0 , 0.0 )
( 0.0 , 0.0 )
( 0.0 , 0.0 )

          ┌─┐                  ┌──────────────┐ ┌──────────────┐  ┌─┐    
q_0:  |0>─┤H├ ──────────────── ┤0             ├ ┤0             ├ ─┤M├─── 
          ├─┤ ┌──────────────┐ │              │ │  QCircuit_2  │  └╥┘┌─┐ 
q_1:  |0>─┤H├ ┤0             ├ ┤  QCircuit_1  ├ ┤1             ├ ──╫─┤M├ 
          ├─┤ │  QCircuit_0  │ │              │ └──────────────┘   ║ └╥┘ 
q_2:  |0>─┤H├ ┤1             ├ ┤1             ├ ──────────────── ──╫──╫─ 
          └─┘ └──────────────┘ └──────────────┘                    ║  ║  
 c :   / ══════════════════════════════════════════════════════════╩══╩═
                                                                    0  1
@endcode

@note
For operation types that are not supported, errors may occur during the conversion of OriginIR into a quantum program.


#### QProg Translated to OriginIR String

The compilation module of QPanda3 defines [convert_qprog_to_originir](@ref pyqpanda3.intermediate_compiler.intermediate_compiler.convert_qprog_to_originir) for translating a QProg object into an OriginIR instruction set string.

Below is a simple interface call example to demonstrate the process of converting a quantum program (QProg) into an OriginIR instruction set string:

@add_toggle_python
    @snippet samples/python/convert_qprog_to_originir_string.py  Example of testing Qprog to OriginIR String
@end_toggle

The output is as follows:
@code{.bash}
Result(stv) of running qprog:

( 3.6742350925920025e-07 , -0.7071067811864525 )
( 0.0 , 0.0 )
( 0.0 , 0.0 )
( 0.0 , 0.0 )
( 3.6742350925920025e-07 , -0.7071067811864525 )
( 0.0 , 0.0 )
( 0.0 , 0.0 )
( 0.0 , 0.0 )

          ┌─┐                  ┌──────────────┐ ┌──────────────┐  ┌─┐    
q_0:  |0>─┤H├ ──────────────── ┤0             ├ ┤0             ├ ─┤M├─── 
          ├─┤ ┌──────────────┐ │              │ │  QCircuit_2  │  └╥┘┌─┐ 
q_1:  |0>─┤H├ ┤0             ├ ┤  QCircuit_1  ├ ┤1             ├ ──╫─┤M├ 
          ├─┤ │  QCircuit_0  │ │              │ └──────────────┘   ║ └╥┘ 
q_2:  |0>─┤H├ ┤1             ├ ┤1             ├ ──────────────── ──╫──╫─ 
          └─┘ └──────────────┘ └──────────────┘                    ║  ║  
 c :   / ══════════════════════════════════════════════════════════╩══╩═
                                                                    0  1

QINIT 3
CREG 2
H q[2]
H q[0]
H q[1]
CONTROL q[1]
RX q[2],(-3.14159)
ENDCONTROL

CONTROL q[0]
RX q[2],(-3.14159)
RX q[2],(-3.14159)
ENDCONTROL

DAGGER
H q[1]
H q[0]
ENDDAGGER

MEASURE q[0],c[0]
MEASURE q[1],c[1]
@endcode