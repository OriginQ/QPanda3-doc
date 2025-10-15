Density Matrix  {#tutorial_table_of_content_QI_DensityMatrix}
=============================================================================

[TOC]

@prev_tutorial{tutorial_variational_quantum_circuit}
@next_tutorial{tutorial_table_of_content_QI_StateVector}

-------------------------------------------------------------------------------------------------------------------------------

# Introduction

The density matrix is a crucial concept in quantum mechanics, which is used to describe the state of a quantum system, especially when there is interaction between the system and the environment. The following is a detailed introduction to the density matrix in the quantum field:


The density matrix, also known as the density operator, is a Hermitian matrix with a trace of 1 and a squared trace of less than or equal to 1. In quantum mechanics, the density matrix is used to represent the superposition of probabilities of a system being in multiple quantum states. For the state of a quantum system, its density matrix can be expressed as \f$
\rho=\sum_i p_i|\psi_i\rangle\langle\psi_i|\f$, where \f$p_i\f$ is the probability that the system is in the state \f$|\psi_i\rangle\f$, satisfying \f$p_i \geq 0\f$  and \f$\sum_i\f$ \f$p_i=1 \f$ , which is the normalization condition.

# In QPanda3 Quantum Information

QPanda3 uses DensityMatrix in Quantum Informaton to abstract and simulate density matrix

## Constructing a DensityMatrix object

[Here is API doc](@ref pyqpanda3.quantum_info.quantum_info.DensityMatrix.__init__)

### Default constructor

The default is to construct a density matrix for a quantum system with only one qubit and a current state of all 0s

For internal tensor product operations, the qbit index with the larger value is used as the left operand. For example, when performing a tensor product operation on a quantum system consisting of \f$q[0]\f$, \f$q[1]\f$, and \f$q[2]\f$, the result corresponds to \f$\left | q[2]  \right \rangle \otimes \left | q[1]  \right \rangle \otimes \left | q[0]  \right \rangle \f$ 

@add_toggle_python
    @snippet samples\python\QuantumInformation\quantum_state\density_matrix\construct_a_density_matrix_object_1.py Example of Construct A Density Matrix Object
@end_toggle
Output
```bash
[[1.+0.j 0.+0.j]
 [0.+0.j 0.+0.j]]
```

### From qubit total 

Specify the total number of qubits in the quantum system, and generate a density matrix where the state of each quantum is currently 0

For internal tensor product operations, the qbit index with the larger value is used as the left operand. For example, when performing a tensor product operation on a quantum system consisting of \f$q[0]\f$, \f$q[1]\f$, and \f$q[2]\f$, the result corresponds to \f$\left | q[2]  \right \rangle \otimes \left | q[1]  \right \rangle \otimes \left | q[0]  \right \rangle \f$ 

@add_toggle_python
    @snippet samples\python\QuantumInformation\quantum_state\density_matrix\construct_a_density_matrix_object_2.py Example of Construct A Density Matrix Object
@end_toggle
Output
```bash
[[1.+0.j 0.+0.j 0.+0.j 0.+0.j 0.+0.j 0.+0.j 0.+0.j 0.+0.j]
 [0.+0.j 0.+0.j 0.+0.j 0.+0.j 0.+0.j 0.+0.j 0.+0.j 0.+0.j]
 [0.+0.j 0.+0.j 0.+0.j 0.+0.j 0.+0.j 0.+0.j 0.+0.j 0.+0.j]
 [0.+0.j 0.+0.j 0.+0.j 0.+0.j 0.+0.j 0.+0.j 0.+0.j 0.+0.j]
 [0.+0.j 0.+0.j 0.+0.j 0.+0.j 0.+0.j 0.+0.j 0.+0.j 0.+0.j]
 [0.+0.j 0.+0.j 0.+0.j 0.+0.j 0.+0.j 0.+0.j 0.+0.j 0.+0.j]
 [0.+0.j 0.+0.j 0.+0.j 0.+0.j 0.+0.j 0.+0.j 0.+0.j 0.+0.j]
 [0.+0.j 0.+0.j 0.+0.j 0.+0.j 0.+0.j 0.+0.j 0.+0.j 0.+0.j]]

```

### From a complex array

Construct a density matrix based on the input two-dimensional complex array.If the matrix corresponding to the two-dimensional complex array does not satisfy the condition of trace 1 and positive definiteness, an exception is thrown. If the number of elements in a given complex array is not a power of 2, it will be automatically expanded to a power of 2. This expansion operation will expand the number of elements as little as possible

For internal tensor product operations, the qbit index with the larger value is used as the left operand. For example, when performing a tensor product operation on a quantum system consisting of \f$q[0]\f$, \f$q[1]\f$, and \f$q[2]\f$, the result corresponds to \f$\left | q[2]  \right \rangle \otimes \left | q[1]  \right \rangle \otimes \left | q[0]  \right \rangle \f$ 

@add_toggle_python
    @snippet samples\python\QuantumInformation\quantum_state\density_matrix\construct_a_density_matrix_object_3.py Example of Construct A Density Matrix Object
@end_toggle
Output
```bash
[[ 0.18056413+0.j          0.12215589+0.06893592j -0.05337882-0.06077268j
  -0.06164719-0.00112235j]
 [ 0.12215589-0.06893592j  0.38670196+0.j         -0.251278  -0.09109501j
   0.11033526+0.04016816j]
 [-0.05337882+0.06077268j -0.251278  +0.09109501j  0.26018725+0.j
  -0.14253993+0.02027215j]
 [-0.06164719+0.00112235j  0.11033526-0.04016816j -0.14253993-0.02027215j
   0.17254666+0.j        ]]
```
### From DensityMatrix
Construct a new DensityMatrix object based on an existing DensityMatrix object

@add_toggle_python
    @snippet samples\python\QuantumInformation\quantum_state\density_matrix\construct_a_density_matrix_object_4.py Example of Construct A Density Matrix Object
@end_toggle

Output
```bash
[[ 0.18056413+0.j          0.12215589+0.06893592j -0.05337882-0.06077268j
  -0.06164719-0.00112235j]
 [ 0.12215589-0.06893592j  0.38670196+0.j         -0.251278  -0.09109501j
   0.11033526+0.04016816j]
 [-0.05337882+0.06077268j -0.251278  +0.09109501j  0.26018725+0.j
  -0.14253993+0.02027215j]
 [-0.06164719+0.00112235j  0.11033526-0.04016816j -0.14253993-0.02027215j
   0.17254666+0.j        ]]
```

### From StateVector
Construct a new DensityMatrix object from an existing StateVector object

@add_toggle_python
    @snippet samples\python\QuantumInformation\quantum_state\density_matrix\construct_a_density_matrix_object_5.py Example of Construct A Density Matrix Object
@end_toggle
Output
```bash
dm: {
dm: [[0.00490196+0.j 0.00980392+0.j 0.01470588+0.j 0.01960784+0.j
  0.0245098 +0.j 0.02941176+0.j 0.03431373+0.j 0.03921569+0.j]
 [0.00980392+0.j 0.01960784+0.j 0.02941176+0.j 0.03921569+0.j
  0.04901961+0.j 0.05882353+0.j 0.06862745+0.j 0.07843137+0.j]
 [0.01470588+0.j 0.02941176+0.j 0.04411765+0.j 0.05882353+0.j
  0.07352941+0.j 0.08823529+0.j 0.10294118+0.j 0.11764706+0.j]
 [0.01960784+0.j 0.03921569+0.j 0.05882353+0.j 0.07843137+0.j
  0.09803922+0.j 0.11764706+0.j 0.1372549 +0.j 0.15686275+0.j]
 [0.0245098 +0.j 0.04901961+0.j 0.07352941+0.j 0.09803922+0.j
  0.12254902+0.j 0.14705882+0.j 0.17156863+0.j 0.19607843+0.j]
 [0.02941176+0.j 0.05882353+0.j 0.08823529+0.j 0.11764706+0.j
  0.14705882+0.j 0.17647059+0.j 0.20588235+0.j 0.23529412+0.j]
 [0.03431373+0.j 0.06862745+0.j 0.10294118+0.j 0.1372549 +0.j
  0.17156863+0.j 0.20588235+0.j 0.24019608+0.j 0.2745098 +0.j]
 [0.03921569+0.j 0.07843137+0.j 0.11764706+0.j 0.15686275+0.j
  0.19607843+0.j 0.23529412+0.j 0.2745098 +0.j 0.31372549+0.j]]
```

## Obtain internal data

### Get numpy.ndarray
Get the internal data of the density matrix and return it in the form of numpy.ndarray

[Here is API doc for DensityMatrix.ndarray](@ref pyqpanda3.quantum_info.quantum_info.DensityMatrix.ndarray)

@add_toggle_python
    @snippet samples\python\QuantumInformation\quantum_state\density_matrix\obtain_internal_data_1.py Example of Obtain Internal Data
@end_toggle
Output
```bash
data: [[ 0.18056413+0.j          0.12215589+0.06893592j -0.05337882-0.06077268j
  -0.06164719-0.00112235j]
 [ 0.12215589-0.06893592j  0.38670196+0.j         -0.251278  -0.09109501j
   0.11033526+0.04016816j]
 [-0.05337882+0.06077268j -0.251278  +0.09109501j  0.26018725+0.j
  -0.14253993+0.02027215j]
 [-0.06164719+0.00112235j  0.11033526-0.04016816j -0.14253993-0.02027215j
   0.17254666+0.j        ]]
```

### Get element

Get an element of internal data by matrix row and column index

[Here is API doc for DensityMatrix.dim](@ref pyqpanda3.quantum_info.quantum_info.DensityMatrix.dim)

[Here is API doc for DensityMatrix.at](@ref pyqpanda3.quantum_info.quantum_info.DensityMatrix.at)

@add_toggle_python
    @snippet samples\python\QuantumInformation\quantum_state\density_matrix\obtain_internal_data_2.py Example of Obtain Internal Data
@end_toggle
Output
```bash
element in dm:
(0.18056413+0j)
(0.12215589+0.06893592j)
(-0.05337882-0.06077268j)
(-0.06164719-0.00112235j)
(0.12215589-0.06893592j)
(0.38670196+0j)
(-0.251278-0.09109501j)
(0.11033526+0.04016816j)
(-0.05337882+0.06077268j)
(-0.251278+0.09109501j)
(0.26018725+0j)
(-0.14253993+0.02027215j)
(-0.06164719+0.00112235j)
(0.11033526-0.04016816j)
(-0.14253993-0.02027215j)
(0.17254666+0j)
```

## Purity
Obtain the purity of the density matrix

[Here is API doc for DensityMatrix.purity](@ref pyqpanda3.quantum_info.quantum_info.DensityMatrix.purity)

@add_toggle_python
    @snippet samples\python\QuantumInformation\quantum_state\density_matrix\obtain_purity_of_density_matrix.py Example of Obtain Purity Of Density Matrix
@end_toggle
Output
```bash
dm: [[ 0.18056413+0.j          0.12215589+0.06893592j -0.05337882-0.06077268j
  -0.06164719-0.00112235j]
 [ 0.12215589-0.06893592j  0.38670196+0.j         -0.251278  -0.09109501j
   0.11033526+0.04016816j]
 [-0.05337882+0.06077268j -0.251278  +0.09109501j  0.26018725+0.j
  -0.14253993+0.02027215j]
 [-0.06164719+0.00112235j  0.11033526-0.04016816j -0.14253993-0.02027215j
   0.17254666+0.j        ]]
purity: (0.5515582694688418+0j)
```

## StateVector
Obtain the corresponding state vector

[Here is API doc for DensityMatrix.to_statevector](@ref pyqpanda3.quantum_info.quantum_info.DensityMatrix.to_statevector)

@add_toggle_python
    @snippet samples\python\QuantumInformation\quantum_state\density_matrix\obtain_corresponding_state_vector.py Example of Obtain Corresponding State Vector
@end_toggle
Output
```bash
stv: [(1+0j), 0j, 0j, 0j, 0j, 0j, 0j, 0j]
```

## Evolution with QCircuit
Use quantum circuit QCircuit to evolve

### Evolve without updating
Use the quantum circuit QCircuit to evolve the density matrix without updating the internal data of the original DensityMatrix object; The result of evolution is returned as a new DensityMatrix object

[Here is API doc for DensityMatrix.evolve](@ref pyqpanda3.quantum_info.quantum_info.DensityMatrix.evolve)

@add_toggle_python
    @snippet samples\python\QuantumInformation\quantum_state\density_matrix\use_quantum_circuit_to_evolve_1.py Example of Use Quantum Circuit QCircuit To Evolve
@end_toggle
Output
```bash
dm2: [[0.+0.j 0.+0.j 0.+0.j 0.+0.j 0.+0.j 0.+0.j 0.+0.j 0.+0.j]
 [0.+0.j 1.+0.j 0.+0.j 0.+0.j 0.+0.j 0.+0.j 0.+0.j 0.+0.j]
 [0.+0.j 0.+0.j 0.+0.j 0.+0.j 0.+0.j 0.+0.j 0.+0.j 0.+0.j]
 [0.+0.j 0.+0.j 0.+0.j 0.+0.j 0.+0.j 0.+0.j 0.+0.j 0.+0.j]
 [0.+0.j 0.+0.j 0.+0.j 0.+0.j 0.+0.j 0.+0.j 0.+0.j 0.+0.j]
 [0.+0.j 0.+0.j 0.+0.j 0.+0.j 0.+0.j 0.+0.j 0.+0.j 0.+0.j]
 [0.+0.j 0.+0.j 0.+0.j 0.+0.j 0.+0.j 0.+0.j 0.+0.j 0.+0.j]
 [0.+0.j 0.+0.j 0.+0.j 0.+0.j 0.+0.j 0.+0.j 0.+0.j 0.+0.j]]
```

### Evolve with updating
Use the quantum circuit QCircuit to evolve the density matrix and update the internal data of the original DensityMatrix object

[Here is API doc for DensityMatrix.update_by_evolve](@ref pyqpanda3.quantum_info.quantum_info.DensityMatrix.update_by_evolve)

@add_toggle_python
    @snippet samples\python\QuantumInformation\quantum_state\density_matrix\use_quantum_circuit_to_evolve_2.py Example of Use Quantum Circuit QCircuit To Evolve
@end_toggle
Output
```bash
dm: [[0.+0.j 0.+0.j 0.+0.j 0.+0.j 0.+0.j 0.+0.j 0.+0.j 0.+0.j]
 [0.+0.j 1.+0.j 0.+0.j 0.+0.j 0.+0.j 0.+0.j 0.+0.j 0.+0.j]
 [0.+0.j 0.+0.j 0.+0.j 0.+0.j 0.+0.j 0.+0.j 0.+0.j 0.+0.j]
 [0.+0.j 0.+0.j 0.+0.j 0.+0.j 0.+0.j 0.+0.j 0.+0.j 0.+0.j]
 [0.+0.j 0.+0.j 0.+0.j 0.+0.j 0.+0.j 0.+0.j 0.+0.j 0.+0.j]
 [0.+0.j 0.+0.j 0.+0.j 0.+0.j 0.+0.j 0.+0.j 0.+0.j 0.+0.j]
 [0.+0.j 0.+0.j 0.+0.j 0.+0.j 0.+0.j 0.+0.j 0.+0.j 0.+0.j]
 [0.+0.j 0.+0.j 0.+0.j 0.+0.j 0.+0.j 0.+0.j 0.+0.j 0.+0.j]]
```

## Boolean function

### Valid
Verify whether the internal data of the density matrix is legal
The term "legal" here refers to whether it meets the requirements of being semi-positive definite and having a trace of 1

[Here is API doc for DensityMatrix.is_valid](@ref pyqpanda3.quantum_info.quantum_info.DensityMatrix.is_valid)

@add_toggle_python
    @snippet samples\python\QuantumInformation\quantum_state\density_matrix\boolean_function_1.py Example of Boolean function
@end_toggle
Output
```bash
is_valid: True
```

### Equal
Determine whether the internal data of two DensityMatrix are the same

[Here is API doc](@ref pyqpanda3.quantum_info.quantum_info.DensityMatrix.__eq__)

@add_toggle_python
    @snippet samples\python\QuantumInformation\quantum_state\density_matrix\boolean_function_2.py Example of Boolean function
@end_toggle
Output
```bash
dm==dm2: True
```