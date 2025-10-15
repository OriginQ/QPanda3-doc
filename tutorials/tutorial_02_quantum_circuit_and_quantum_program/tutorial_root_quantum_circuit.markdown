Quantum Circuit {#tutorial_quantum_circuit}
=============================================================

[TOC]

@prev_tutorial{tutorial_circuit_and_program}
@next_tutorial{tutorial_quantum_program}

-------------------------------------------------------------------------------------------------------------------------------

# QCircuit

Quantum circuits, also known as quantum logic circuits, are the most commonly used general quantum computing models. They represent circuits that operate on quantum bits in abstract concepts and are a collection of various logic gates. Finally, quantum measurements are often required to read out the results.
Unlike traditional circuits that use metal wires to transmit voltage or current signals, in quantum circuits, the wires are connected by time, meaning that the state of quantum bits naturally evolves over time, following the instructions of the Hamiltonian operator, until they encounter a logic gate and are operated upon.
Since each quantum logic gate that makes up a quantum circuit is a 'unitary matrix', the entire quantum circuit is also a large unitary matrix.

## Class Initialization

You can initialize a [QCircuit](@ref pyqpanda3.core.core.QCircuit) in three different ways:

1. **Default Constructor**:	

	@add_toggle_python
	
	```python
	circuit = QCircuit()
	```
	@end_toggle

2. **With a Specified Number of Qubits**:

	@add_toggle_python
	
	```python
	circuit = QCircuit(3)  # Creates a circuit with 3 qubits.
	```
	@end_toggle

3. **Copy Constructor**:

	@add_toggle_python
	
	```python
	circuit_copy = QCircuit(circuit)
	```
	@end_toggle

## Basic Methods

- **Get the Size of the Circuit**:
	
	Using the [size](@ref pyqpanda3.core.core.QCircuit.size) function to get circuit size.
	Example:
	
	@add_toggle_python
	
	```python
	size = circuit.size()
	```
	@end_toggle

- **Append Operations**:

	You can append both gates and other circuits using the `append` method or the `<<` operator.	

	@add_toggle_python
	
	```python
	circuit.append(H(0))  # Appending a gate
	circuit << H(0)        # Using operator overload
	```
	@end_toggle

## Circuit Management

- **Clear the Circuit**:

	Using the [clear](@ref pyqpanda3.core.core.QCircuit.clear) function to clear circuit.
	Example:	
	
	@add_toggle_python
	
	```python
	circuit.clear()	
	```
	@end_toggle
			
- **Get Qubits**:	

	Using the [qubits](@ref pyqpanda3.core.core.QCircuit.qubits) function to retrieve qubits involved in the circuit:
	Example:
	
	@add_toggle_python
	
	```python
	qubits = circuit.qubits()
	```
	@end_toggle

- **Target and Control Qubits**:

	Using the [target_qubits](@ref pyqpanda3.core.core.QCircuit.target_qubits) function to get target qubits.
	Using the [control_qubits](@ref pyqpanda3.core.core.QCircuit.control_qubits) function to get control qubits.
	Example:
	
	@add_toggle_python
	
	```python
	target_qubits = circuit.target_qubits()
	control_qubits = circuit.control_qubits()
	```
	@end_toggle

## Circuit Properties

- **Set Dagger Property**:

	Using the [dagger](@ref pyqpanda3.core.core.QCircuit.dagger) function to set circuit dagger.
	If the current circuit is dagger, the dagger will be cancelled.Example:
	
	@add_toggle_python
	
	```python
	dagger_circuit = circuit.dagger()  # Set the dagger version of the circuit
	```
	@end_toggle

## Control Operations

- **Control Qubits**:

	Using the [control](@ref pyqpanda3.core.core.QCircuit.control) and [clear_control](@ref pyqpanda3.core.core.QCircuit.clear_control)function to set and clear control qubits:

	@add_toggle_python
	
	```python
	circuit.control([1, 2])  # Set control qubits
	circuit.clear_control()       # Clear control qubits
	```
	@end_toggle

## Circuit Name

- **Set and Get Circuit Name**:

	Using the [set_name](@ref pyqpanda3.core.core.QCircuit.set_control) and [name](@ref pyqpanda3.core.core.QCircuit.clear_control)function to set and get circuit's name:
	
	@add_toggle_python
	
	```python
	circuit.set_name("Circuit_888")
	name = circuit.name()
	```
	@end_toggle

## Operations and Layers

- **List Operations**:

	Using the [gate_operations](@ref pyqpanda3.core.core.QCircuit.gate_operations) function to get circuit's operations:

	@add_toggle_python
	
	```python
	operations = circuit.gate_operations()
	```
	@end_toggle

- **Count Operations**:

	Using the [count_ops](@ref pyqpanda3.core.core.QCircuit.count_ops) function to count the number of gates in the circuit:

	@add_toggle_python
	
	```python
	operation_count = circuit.count_ops()
	```
	@end_toggle

- **Get Circuit Depth**:

	Using the [depth](@ref pyqpanda3.core.core.QCircuit.depth) function to get circuit's depth:

	@add_toggle_python
	
	```python
	depth = circuit.depth()
	```
	@end_toggle

- **Expand Circuit**:

	Using the [expand](@ref pyqpanda3.core.core.QCircuit.expand) function to get a copy of the expanded circuit:

	@add_toggle_python
	
	```python
	expanded_circuit = circuit.expand()
	```
	@end_toggle

## Example Usage

Here is a simple example demonstrating the creation of a `QCircuit`, appending operations, and retrieving properties:

@add_toggle_python
@snippet samples/python/circuit_and_program.py QCircuit Example
@end_toggle

Run result:
```python
Circuit Size: 2
Qubits: [0, 1]
Operations: {'CNOT': 1, 'H': 1}
Circuit Depth: 2
```
