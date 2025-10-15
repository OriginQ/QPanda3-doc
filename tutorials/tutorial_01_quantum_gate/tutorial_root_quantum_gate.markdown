Quantum Gate {#tutorial_quantum_gate}
============

[TOC]

@prev_tutorial{tutorial_root}
@next_tutorial{tutorial_circuit_and_program}


# Quantum Logic Gates

In classical computing, the most basic unit is the bit, and the fundamental control mechanism is the logic gate. We can achieve our control of circuits through combinations of logic gates. Similarly, the way we manipulate qubits is through quantum logic gates. By using quantum logic gates, we consciously induce the evolution of quantum states. Thus, quantum logic gates form the foundation of quantum algorithms.

Quantum logic gates are represented by unitary matrices. The most common quantum gates operate in the space of one or two qubits, just as common classical logic gates operate on one or two bits.

## Common Quantum Logic Gates in Matrix Form

| Gate | Name |          Matrix                                                                                                                                                                                                                    |
| :--------: | :------------: | :--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------: |
| I  | `I`        | \f[\begin{bmatrix} 1 & 0 \\ 0 & 1 \end{bmatrix}\f]                                                                                                                                                                                 |
| H  | `Hadamard` | \f[\begin{bmatrix} \frac{1}{\sqrt{2}} & \frac{1}{\sqrt{2}} \\ \frac{1}{\sqrt{2}} & -\frac{1}{\sqrt{2}} \end{bmatrix}\f]                                                                                                            |
| T  | `T`        | \f[\begin{bmatrix} 1 & 0 \\ 0 & \exp\left(\frac{i\pi}{4}\right) \end{bmatrix}\f]                                                                                                                                                   |
| S  | `S`        | \f[\begin{bmatrix} 1 & 0 \\ 0 & i \end{bmatrix}\f]                                                                                                                                                                                 |
| X  | `Pauli-X`  | \f[\begin{bmatrix} 0 & 1 \\ 1 & 0 \end{bmatrix}\f]                                                                                                                                                                                 |
| Y  | `Pauli-Y`  | \f[\begin{bmatrix} 0 & -i \\ i & 0 \end{bmatrix}\f]                                                                                                                                                                                |
| Z  | `Pauli-Z`  | \f[\begin{bmatrix} 1 & 0 \\ 0 & -1 \end{bmatrix}\f]                                                                                                                                                                                |
| X1 | `X1`       | \f[\begin{bmatrix} \frac{1}{\sqrt{2}} & -\frac{i}{\sqrt{2}} \\ -\frac{i}{\sqrt{2}} & \frac{1}{\sqrt{2}} \end{bmatrix}\f]                                                                                                           |
| Y1 | `Y1`       | \f[\begin{bmatrix} \frac{1}{\sqrt{2}} & -\frac{1}{\sqrt{2}} \\ \frac{1}{\sqrt{2}} & \frac{1}{\sqrt{2}} \end{bmatrix}\f]                                                                                                            |
| Z1 | `Z1`       | \f[\begin{bmatrix} \exp\left(-\frac{i\pi}{4}\right) & 0 \\ 0 & \exp\left(\frac{i\pi}{4}\right) \end{bmatrix}\f]                                                                                                                    |
| RX | `RX`       | \f[\begin{bmatrix} \cos\left(\frac{\theta}{2}\right) & -i\sin\left(\frac{\theta}{2}\right) \\ -i\sin\left(\frac{\theta}{2}\right) & \cos\left(\frac{\theta}{2}\right) \end{bmatrix}\f]                                             |
| RY | `RY`       | \f[\begin{bmatrix} \cos\left(\frac{\theta}{2}\right) & -\sin\left(\frac{\theta}{2}\right) \\ \sin\left(\frac{\theta}{2}\right) & \cos\left(\frac{\theta}{2}\right) \end{bmatrix}\f]                                                |
| RZ | `RZ`       | \f[\begin{bmatrix} \exp\left(-\frac{i\theta}{2}\right) & 0 \\ 0 & \exp\left(\frac{i\theta}{2}\right) \end{bmatrix}\f]                                                                                                              |
| U1 | `U1`       | \f[\begin{bmatrix} 1 & 0 \\ 0 & \exp(i\theta) \end{bmatrix}\f]                                                                                                                                                                     |
| U2 | `U2`       | \f[\begin{bmatrix} \frac{1}{\sqrt{2}} & -\frac{\exp(i\lambda)}{\sqrt{2}} \\ \frac{\exp(i\phi)}{\sqrt{2}} & \frac{\exp(i\lambda + i\phi)}{\sqrt{2}} \end{bmatrix}\f]                                                                |
| U3 | `U3`       | \f[\begin{bmatrix} \cos\left(\frac{\theta}{2}\right) & -\exp(i\lambda)\sin\left(\frac{\theta}{2}\right) \\ \exp(i\phi)\sin\left(\frac{\theta}{2}\right) & \exp(i\lambda + i\phi)\cos\left(\frac{\theta}{2}\right) \end{bmatrix}\f] |
| U4 | `U4`       | \f[\begin{bmatrix} u_0 & u_1 \\ u_2 & u_3 \end{bmatrix}\f]                                                                                            

## Multi-qubit Quantum Gates                         

| Gate    | Name | Matrix                                                                                              |
|:---------:|:--------:|:-----------------------------------------------------------------------------------------------------:|
| CNOT    | `CNOT` | \f[\begin{bmatrix} 1 & 0 & 0 & 0  \\ 0 & 1 & 0 & 0 \\ 0 & 0 & 0 & 1 \\ 0 & 0 & 1 & 0 \end{bmatrix}\f] |
| MS      | `MS`   | \f[\begin{bmatrix} \frac{1}{\sqrt{2}} & 0 & 0 & -i*\frac{1}{\sqrt{2}}  \\ 0 & \frac{1}{\sqrt{2}} & -i*\frac{1}{\sqrt{2}} & 0 \\ 0 & -i*\frac{1}{\sqrt{2}} & \frac{1}{\sqrt{2}} & 0 \\ -i*\frac{1}{\sqrt{2}} & 0 & 0 & \frac{1}{\sqrt{2}} \end{bmatrix}\f] |
| CR      | `CR`   | \f[\begin{bmatrix} 1 & 0 & 0 & 0  \\ 0 & 1 & 0 & 0 \\ 0 & 0 & 1 & 0 \\ 0 & 0 & 0 & \exp(i\theta) \end{bmatrix}\f] |
| iSWAP   | `iSWAP`| \f[\begin{bmatrix} 1 & 0 & 0 & 0  \\ 0 & \cos(\theta) & i\sin(\theta) & 0 \\ 0 & i\sin(\theta) & \cos(\theta) & 0 \\ 0 & 0 & 0 & 1 \end{bmatrix}\f] |
| SWAP    | `SWAP` | \f[\begin{bmatrix} 1 & 0 & 0 & 0  \\ 0 & 0 & 1 & 0 \\ 0 & 1 & 0 & 0 \\ 0 & 0 & 0 & 1 \end{bmatrix}\f] |
| CZ      | `CZ`   | \f[\begin{bmatrix} 1 & 0 & 0 & 0  \\ 0 & 1 & 0 & 0 \\ 0 & 0 & 1 & 0 \\ 0 & 0 & 0 & -1 \end{bmatrix}\f] |
| CU      | `CU`   | \f[\begin{bmatrix} 1 & 0 & 0 & 0  \\ 0 & 1 & 0 & 0 \\ 0 & 0 & u_0 & u_1 \\ 0 & 0 & u_2 & u_3 \end{bmatrix}\f] |
| RXX     | `RXX`  | \f[\begin{bmatrix} \cos(\theta/2) & 0 & 0 & -i\sin(\theta/2)  \\ 0 & \cos(\theta/2) & -i\sin(\theta/2) & 0 \\ 0 & -i\sin(\theta/2) & \cos(\theta/2) & 0 \\ -i\sin(\theta/2) & 0 & 0 & \cos(\theta/2) \end{bmatrix}\f] |
| RYY     | `RYY`  | \f[\begin{bmatrix} \cos(\theta/2) & 0 & 0 & i\sin(\theta/2)  \\ 0 & \cos(\theta/2) & -i\sin(\theta/2) & 0 \\ 0 & -i\sin(\theta/2) & \cos(\theta/2) & 0 \\ i\sin(\theta/2) & 0 & 0 & \cos(\theta/2) \end{bmatrix}\f] |
| RZZ     | `RZZ`  | \f[\begin{bmatrix} \exp(-i\theta/2) & 0 & 0 & 0  \\ 0 & \exp(i\theta/2) & 0 & 0 \\ 0 & 0 & \exp(i\theta/2) & 0 \\ 0 & 0 & 0 & \exp(-i\theta/2) \end{bmatrix}\f] |
| RZX     | `RZX`  | \f[\begin{bmatrix} \cos(\theta/2) & 0 & -i\sin(\theta/2) & 0  \\ 0 & \cos(\theta/2) & 0 & i\sin(\theta/2) \\ -i\sin(\theta/2) & 0 & \cos(\theta/2) & 0 \\ 0 & i\sin(\theta/2) & 0 & \cos(\theta/2) \end{bmatrix}\f] |
| Toffoli | `Toffoli` | \f[\begin{bmatrix} 1 & 0 & 0 & 0 & 0 & 0 & 0 & 0 \\ 0 & 1 & 0 & 0 & 0 & 0 & 0 & 0 \\ 0 & 0 & 1 & 0 & 0 & 0 & 0 & 0 \\ 0 & 0 & 0 & 1 & 0 & 0 & 0 & 0 \\ 0 & 0 & 0 & 0 & 1 & 0 & 0 & 0  \\ 0 & 0 & 0 & 0 & 0 & 1 & 0 & 0 \\ 0 & 0 & 0 & 0 & 0 & 0 & 0 & 1  \\ 0 & 0 & 0 & 0 & 0 & 0 & 1 & 0 \\ \end{bmatrix}\f] |



# API Introduction

QPanda3 encapsulates all quantum logic gates as APIs for user access, returning values of type [QGate](@ref pyqpanda3.core.core.QGate). 
For example, if you want to use the Hadamard gate, you can obtain it as follows:

@add_toggle_python
```python
h = H(0)
```
@end_toggle

As you can see, the `H` function only takes one qubit. Similarly, if you want to use the RX gate, you can obtain it like this:

@add_toggle_python
```python
rx = RX(0, 3.14)
```
@end_toggle

As shown, the RX gate takes two parameters: the first is the target qubit, and the second is the rotation angle. You can also use the same method for the RY and RZ gates.

The usage of two-qubit quantum logic gates is similar to that of single-qubit gates, but the input parameters differ. For example, using the CNOT gate:

@add_toggle_python
```python
cnot = CNOT(0, 1)
```
@end_toggle

The CNOT gate takes two parameters: the first is the control qubit, and the second is the target qubit.

## Interface Overview
Here we will introduce all the interfaces related to the [QGate](@ref pyqpanda3.core.core.QGate) class. Firstly, we need to know where to import the QGate class from

@add_toggle_python
```python
from pyqpanda3.core import *
```
@end_toggle

## Querying Qubits

- **Get Number of Qubits**

    Using the [qubits_num](@ref pyqpanda3.core.core.QGate.qubits_num) function to get this gate num of qubits .
    Example:
    @add_toggle_python

    ```python
    print("Number of qubits:", gate.qubits_num())
    ```
    @end_toggle

- **Get Qubits**

    Using the [qubits](@ref pyqpanda3.core.core.QGate.qubits) function to get this gate all qubits.
    Example:
    @add_toggle_python

    ```python
    print("Qubits:", gate.qubits())
    ```
    @end_toggle

- **Get Control Qubits**

    Using the [control_qubits](@ref pyqpanda3.core.core.QGate.control_qubits) function to get this gate control_qubits.
    Example:

    @add_toggle_python

    ```python
    print("Control qubits:", gate.control_qubits())
    ```
    @end_toggle

- **Get Target Qubits**

    Using the [target_qubits](@ref pyqpanda3.core.core.QGate.target_qubits) function to get this gate target_qubits.
    Example:
 
    @add_toggle_python

    ```python
    print("Target qubits:", gate.target_qubits())
    ```
    @end_toggle

## Gate Properties

- **Dagger Operation**

    As mentioned at the beginning of this chapter, all quantum logic gates are unitary matrices, and you can also perform conjugate transpose operations on quantum logic gates. The `QGate` type has two member functions that can perform conjugate transpose operations: [dagger](@ref pyqpanda3.core.core.QGate.dagger).

    The [dagger](@ref pyqpanda3.core.core.QGate.dagger) function creates a copy of the current quantum logic gate and updates the copied gate's dagger flag. For example:
    @add_toggle_python
    ```python
    rx_dagger = RX(0, 3.14).dagger()
    ```
    @end_toggle


- **Raise Gate to a Power**
    
    Using the [power](@ref pyqpanda3.core.core.QGate.power) function to get this gate raised to a power.
    Example:
  
    @add_toggle_python

    ```python
    powered_gate = gate.power(2)
    ```
    @end_toggle

- **Control Operations** 
    
    In addition to the conjugate transpose operation, you can also add control qubits to the quantum logic gates.
    After adding control qubits, whether the current quantum logic gate executes depends on the quantum state of the control qubits.
    If the control qubit's state is \f$|1\rangle\f$, the current quantum logic gate can be executed; if the state is \f$|0\rangle\f$, the gate will not execute.
    The `QGate` type has two member functions to help you add control qubits: [control](@ref pyqpanda3.core.core.QGate.control).

    The [control](@ref pyqpanda3.core.core.QGate.control) function creates a copy of the current quantum logic gate and set control qubits to the copied gate. For example:
    @add_toggle_python
    ```python
    rx_control = RX(0, 3.14).control(1)
    ```
    @end_toggle

- **Clear Control Qubits**

    The [clear_control](@ref pyqpanda3.core.core.QGate.clear_control) function clear  control qubits of gate. For example:

    @add_toggle_python

    ```python
    gate.clear_control()
    ```
    @end_toggle

## Matrix Operations

- **Get Matrix Representation**

    Using the [matrix](@ref pyqpanda3.core.core.QGate.matrix) function to obtain a matrix.

    @note
    The meaning of the expanded parameter is whether the matrix is obtained based on the order of qubits,for example, the matrix of CNOT (1,0) is equivalent to CNOT (0,1) when the parameter is False.
    
    Example:
    @add_toggle_python
    
    ```python
    gate = CNOT(1,0)
    maxtrix = gate.matrix(False)
    matrix_expanded = gate.matrix(True)
    ```
    @end_toggle

## Gate Type and Parameters

- **Get Gate Type**

    Using the [gate_type](@ref pyqpanda3.core.core.QGate.gate_type) function to get gate type.
    Example:
  
    @add_toggle_python

    ```python
    print("Gate type:", CNOT(0,1).gate_type())
    ```
    @end_toggle

- **Get and Set Parameters**
    
    Using the [parameters](@ref pyqpanda3.core.core.QGate.parameters) function to get gate parameters.
    Using the [set_parameters](@ref pyqpanda3.core.core.QGate.set_parameters) function to set gate parameters.
    Example:
  
    @add_toggle_python

    ```python
    gate = U3(0,3.14,0,0)
    gate.set_parameters([0.5, 1.0,2.0])
    print("Gate parameters:", gate.parameters())
    ```
    @end_toggle

- **Get Gate Name**

    Using the [name](@ref pyqpanda3.core.core.QGate.name) function to get gate name.
    
    Example:
  
    @add_toggle_python

    ```python
    print("Gate name:", CNOT(0,1).name())
    ```
    @end_toggle




