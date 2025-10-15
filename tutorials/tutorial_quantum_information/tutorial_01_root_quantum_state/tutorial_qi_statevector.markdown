State Vector  {#tutorial_table_of_content_QI_StateVector}
=============================================================================

[TOC]

@prev_tutorial{tutorial_table_of_content_QI_DensityMatrix}
@next_tutorial{tutorial_table_of_content_QI_Kraus}

-------------------------------------------------------------------------------------------------------------------------------
# Introduction

The state vector is a core concept in quantum mechanics, used to abstractly represent the quantum state of a quantum system. The following is a detailed introduction to state vectors in the quantum field:

## Definition and Existence Space

### Definition

Quantum states are represented by vectors in Hilbert space, also known as state vector space or state space, and are called state vectors.

### Existence space 

The state vector exists in the inner product space. Inner product space is a vector space with an additional inner product structure. The state vector satisfies all the axioms of the vector space and allows for the operation of inner product.

## Nature and characteristics

### Unit vector

The norm of a state vector is 1, making it a unit vector.
Linear combination: Every inner product space has a single norm-orthogonal basis, and the state vector is a linear combination of all the basis vectors of the single norm-orthogonal basis.
### Superposition principle of state

The state vector satisfies the linear property, which corresponds to the superposition principle of state in quantum mechanics. That is, if two quantum states are represented by state vectors  \f$\mathcal{ ψ } _1\f$ and \f$\mathcal{ ψ } _2\f$, then their superposition state can be represented by a linear combination of these two state vectors.

### Probability distribution

The state vector can be seen as a probability distribution that describes all possible measurement results of a quantum system. For each measurement, there is a certain probability of obtaining different results, and the probability distribution of these results depends on the state vector of the system.

# In QPanda3 Quantum Information

QPanda3 uses StateVector in Quantum Informaton to abstract and simulate state vectors

## Constructing a StateVector object

[Here is API doc](@ref pyqpanda3.quantum_info.quantum_info.StateVector.__init__)

### Default constructor
  
The default is to construct a state vector for a quantum system with only one qubit and a current state of all 0s

For internal tensor product operations, the qbit index with the larger value is used as the left operand. For example, when performing a tensor product operation on a quantum system consisting of \f$q[0]\f$, \f$q[1]\f$, and \f$q[2]\f$, the result is \f$\left | q[2]  \right \rangle \otimes \left | q[1]  \right \rangle \otimes \left | q[0]  \right \rangle \f$  

@add_toggle_python
    @snippet samples\python\QuantumInformation\quantum_state\state_vector\construction_state_vector_1.py Example of Construction State Vector
@end_toggle
Output
```bash
[1.+0.j 0.+0.j]
```

### From qubit total

Specify the total number of qubits in the quantum system, and generate a state vector where the current state of each quantum is 0

For internal tensor product operations, the qbit index with the larger value is used as the left operand. For example, when performing a tensor product operation on a quantum system consisting of \f$q[0]\f$, \f$q[1]\f$, and \f$q[2]\f$, the result is \f$\left | q[2]  \right \rangle \otimes \left | q[1]  \right \rangle \otimes \left | q[0]  \right \rangle \f$ 

@add_toggle_python
    @snippet samples\python\QuantumInformation\quantum_state\state_vector\construction_state_vector_2.py Example of Construction State Vector
@end_toggle
Output
```bash
[1.+0.j 0.+0.j 0.+0.j 0.+0.j 0.+0.j 0.+0.j 0.+0.j 0.+0.j]
```

## From a complex array

Construct a state vector based on a given complex array. If the given complex array does not meet the normalization constraint, it will be automatically normalized. If the number of elements in a given complex array is not a power of 2, it will be automatically expanded to a power of 2. This expansion operation will expand the number of elements as little as possible

For internal tensor product operations, the qbit index with the larger value is used as the left operand. For example, when performing a tensor product operation on a quantum system consisting of \f$q[0]\f$, \f$q[1]\f$, and \f$q[2]\f$, the result is \f$\left | q[2]  \right \rangle \otimes \left | q[1]  \right \rangle \otimes \left | q[0]  \right \rangle \f$ 

@add_toggle_python
    @snippet samples\python\QuantumInformation\quantum_state\state_vector\construction_state_vector_3.py Example of Construction State Vector
@end_toggle
Output
```bash
[0.04950738+0.04950738j 0.09901475+0.09901475j 0.14852213+0.14852213j
 0.19802951+0.19802951j 0.24753689+0.24753689j 0.29704426+0.29704426j
 0.34655164+0.34655164j 0.39605902+0.39605902j]
[0.04950738+0.04950738j 0.09901475+0.09901475j 0.14852213+0.14852213j
 0.19802951+0.19802951j 0.24753689+0.24753689j 0.29704426+0.29704426j
 0.34655164+0.34655164j 0.39605902+0.39605902j]
```

### From a dictionary
Construct a state vector based on the input dictionary


The keys of the dictionary are positive integers, representing the index of the quantum state; The value of the dictionary is the corresponding amplitude. If the normalization constraint is not met, automatic normalization processing will be performed. If the maximum value of the key of a given dictionary element is not a power of 2, it will be automatically expanded to a power of 2. This expansion operation will expand as few elements as possible

For internal tensor product operations, the qbit index with the larger value is used as the left operand. For example, when performing a tensor product operation on a quantum system consisting of \f$q[0]\f$, \f$q[1]\f$, and \f$q[2]\f$, the result is \f$\left | q[2]  \right \rangle \otimes \left | q[1]  \right \rangle \otimes \left | q[0]  \right \rangle \f$ 

@add_toggle_python
    @snippet samples\python\QuantumInformation\quantum_state\state_vector\construction_state_vector_4.py Example of Construction State Vector
@end_toggle
Output
```bash
[0.        +0.j         0.13867505+0.13867505j 0.        +0.j
 0.41602515+0.41602515j 0.5547002 +0.5547002j  0.        +0.j
 0.        +0.j         0.        +0.j        ]
```



## Obtain internal data

### Get numpy.ndarray
Get all the internal data in the form of numpy.ndarray

[Here is API doc for StateVector.ndarray](@ref pyqpanda3.quantum_info.quantum_info.StateVector.ndarray)

@add_toggle_python
    @snippet samples\python\QuantumInformation\quantum_state\state_vector\obtain_internal_data_1.py Example of Obtain Internal Data
@end_toggle
Output
```bash
data: [0.        +0.j         0.13867505+0.13867505j 0.        +0.j
 0.41602515+0.41602515j 0.5547002 +0.5547002j  0.        +0.j
 1.        +0.j         0.        +0.j        ]
```

### Get element
Get the value corresponding to a certain state by indexing

[Here is API doc for StateVector.dim](@ref pyqpanda3.quantum_info.quantum_info.StateVector.dim)

[Here is API doc for StateVector.at](@ref pyqpanda3.quantum_info.quantum_info.StateVector.at)
  
@add_toggle_python
    @snippet samples\python\QuantumInformation\quantum_state\state_vector\obtain_internal_data_2.py Example of Obtain Internal Data
@end_toggle
Output
```bash
element in stv:
0j
(0.1386750490563073+0.1386750490563073j)
0j
(0.41602514716892186+0.41602514716892186j)
(0.5547001962252291+0.5547001962252291j)
0j
0j
0j
```

## Evolution with QCircuit

### Evolve without updating
Use the quantum circuit QCircuit to evolve the quantum state without updating the internal data of the original StateVector object, and the result is returned as a new StateVector object

[Here is API doc for StateVector.evolve](@ref pyqpanda3.quantum_info.quantum_info.StateVector.evolve)
  
@add_toggle_python
    @snippet samples\python\QuantumInformation\quantum_state\state_vector\evolution_with_qcircuit_1.py Example of Evolution With QCircuit
@end_toggle
Output
```bash
stv2: [0.+0.j 1.+0.j 0.+0.j 0.+0.j 0.+0.j 0.+0.j 0.+0.j 0.+0.j]
```

### Evolve with updating

Use the quantum circuit QCircuit to evolve the quantum state and update the internal data of the original StateVector object

[Here is API doc for StateVector.update_by_evolve](@ref pyqpanda3.quantum_info.quantum_info.StateVector.update_by_evolve)

@add_toggle_python
    @snippet samples\python\QuantumInformation\quantum_state\state_vector\evolution_with_qcircuit_2.py Example of Evolution With QCircuit
@end_toggle
Output
```bash
stv2: [0.+0.j 1.+0.j 0.+0.j 0.+0.j 0.+0.j 0.+0.j 0.+0.j 0.+0.j]
```

## Density Matrix
Obtain the corresponding density matrix representation

[Here is API doc for StateVector.get_density_matrix](@ref pyqpanda3.quantum_info.quantum_info.StateVector.get_density_matrix)

@add_toggle_python
    @snippet samples\python\QuantumInformation\quantum_state\state_vector\obtain_corresponding_density_matrix_represent.py Example of Obtain Corresponding Density Matrix Representation
@end_toggle
Output
```bash
dm: [[0.        +0.j 0.        +0.j 0.        +0.j 0.        +0.j
  0.        +0.j 0.        +0.j 0.        +0.j 0.        +0.j]
 [0.        +0.j 0.03846154+0.j 0.        +0.j 0.11538462+0.j
  0.15384615+0.j 0.        +0.j 0.        +0.j 0.        +0.j]
 [0.        +0.j 0.        +0.j 0.        +0.j 0.        +0.j
  0.        +0.j 0.        +0.j 0.        +0.j 0.        +0.j]
 [0.        +0.j 0.11538462+0.j 0.        +0.j 0.34615385+0.j
  0.46153846+0.j 0.        +0.j 0.        +0.j 0.        +0.j]
 [0.        +0.j 0.15384615+0.j 0.        +0.j 0.46153846+0.j
  0.61538462+0.j 0.        +0.j 0.        +0.j 0.        +0.j]
 [0.        +0.j 0.        +0.j 0.        +0.j 0.        +0.j
  0.        +0.j 0.        +0.j 0.        +0.j 0.        +0.j]
 [0.        +0.j 0.        +0.j 0.        +0.j 0.        +0.j
  0.        +0.j 0.        +0.j 0.        +0.j 0.        +0.j]
 [0.        +0.j 0.        +0.j 0.        +0.j 0.        +0.j
  0.        +0.j 0.        +0.j 0.        +0.j 0.        +0.j]]
```
## Purity
Obtain the corresponding purity

[Here is API doc for StateVector.purity](@ref pyqpanda3.quantum_info.quantum_info.StateVector.purity)

@add_toggle_python
    @snippet samples\python\QuantumInformation\quantum_state\state_vector\obtain_corresponding_purity.py Example of Obtain Corresponding Purity
@end_toggle
Output
```bash
purity: (1+0j)
```

## Boolean function

### Valid
Determine whether the internal data is legal

[Here is API doc for StateVector.is_valid](@ref pyqpanda3.quantum_info.quantum_info.StateVector.is_valid)

@add_toggle_python
    @snippet samples\python\QuantumInformation\quantum_state\state_vector\boolean_function_1.py Example of Boolean function
@end_toggle
Output
```bash
res: True
```

### Equal
Determine whether the internal data of two state vectors are equal within the default tolerance

[Here is API doc](@ref pyqpanda3.quantum_info.quantum_info.StateVector.__eq__)
  
@add_toggle_python
    @snippet samples\python\QuantumInformation\quantum_state\state_vector\boolean_function_2.py Example of Boolean function
@end_toggle
Output
```bash
res: True
```
