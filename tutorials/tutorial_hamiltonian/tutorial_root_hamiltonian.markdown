Hamiltonian  {#tutorial_hamiltonian}
=============================================================================

[TOC]

@prev_tutorial{tutorial_quantum_information}
@next_tutorial{tutorial_quantum_profiling}

------------------------------------------------------------------------------------------------------------------------------

# Introduction

The Hamiltonian, denoted as H, is a fundamental concept in quantum mechanics and classical mechanics. It represents the total energy of a system, and it is a function of the coordinates and momenta (or velocities) of the particles in the system.

In classical mechanics, the Hamiltonian is derived from the Lagrangian function through a Legendre transformation, and it is used to describe the dynamics of the system through the Hamilton's equations of motion. These equations provide a more general and powerful description of the motion of particles than Newton's laws of motion.

In quantum mechanics, the Hamiltonian corresponds to the energy operator, and its eigenvalues represent the possible energy levels of the system. The Schrödinger equation, which describes the evolution of a quantum system in time, can be derived from the Hamiltonian.

# Simulate
Simulate the Hamiltonian using Pauli operators

[Here is API doc](@ref pyqpanda3.hamiltonian.hamiltonian.Hamiltonian.__init__)

## From a dict like { 'X0 Z1 ':2 + 0j, 'X1 Y2 ':3 + 0j, }
Constructs a Hamiltonian from a dict containing operators and coefficients.

@add_toggle_python
    @snippet samples\python\Hamiltonian.py Method 1 of constructing Hamiltonian
@end_toggle

Output
```bash
Ham
 { qbit_total = 3, pauli_with_coef_s = { 'X0 Z1 ':2 + 0j, 'X1 Y2 ':3 + 0j, } }
```
## From a list like [("XXZ", [0, 1, 4], 1 + 2j), ("ZZ", [1, 2], -1 + 1j)]
Constructs a Hamiltonian from a list containing operators, indices, and coefficients.

@add_toggle_python
    @snippet samples\python\Hamiltonian.py Method 2 of constructing Hamiltonian
@end_toggle

Output
```bash
Ham:
 { qbit_total = 5, pauli_with_coef_s = { 'X0 X1 Z4 ':1 + 2j, 'Z1 Z2 ':-1 + 1j, } }
```

## From a PauliOperator object
Constructs a Hamiltonian from a PauliOperator

@add_toggle_python
    @snippet samples\python\Hamiltonian.py Method 3 of constructing Hamiltonian
@end_toggle

Output
```bash
Ham:
 { qbit_total = 5, pauli_with_coef_s = { 'X0 X1 Z4 ':1 + 2j, 'Z1 Z2 ':-1 + 1j, } }
```

## From a real matrix
Generate a Hamiltonian object from a real matrix.

@add_toggle_python
    @snippet samples\python\Hamiltonian.py Generate a Hamiltonian object from a real matrix
@end_toggle

Output
```bash
op:
 { qbit_total = 2, pauli_with_coef_s = { 'X0 ':0.373 + 0j, 'Y0 ':-0 -0.24026j, 'X0 Z1 ':-0.176981 + 0j, 'Y0 Z1 ':0 + 0.139393j, 'X1 ':0.678529 + 0j, 'Z0 X1 ':0.052562 + 0j, 'Y1 ':-0 -0.164005j, 'Z0 Y1 ':0 + 0.153926j, 'X0 X1 ':0.413656 + 0j, 'Y0 X1 ':-0 -0.134299j, 'X0 Y1 ':0 + 0.0292082j, 'Y0 Y1 ':-0.157953 + 0j, '':0.590014 + 0j, 'Z0 ':0.0761358 + 0j, 'Z1 ':0.142981 + 0j, 'Z0 Z1 ':-0.0352695 + 0j, } }
```

# Operations

Addition, subtraction, multiplication, division, and tensor product operations

## Describing

For the convenience of description, some expressions are agreed upon here. These expressions will be used for explanation in the following. These expressions are same with PauliOperator's. 
Please see [PauliOperator](#tutorial_table_of_content_PauliOperator).



## Addition

[Here is API doc](@ref pyqpanda3.hamiltonian.hamiltonian.Hamiltonian.__add__)

### Using rules

```
Hamiltonian + Hamiltonian
```

@add_toggle_python
    @snippet samples\python\Hamiltonian.py Hamiltonian + Hamiltonian
@end_toggle

Output
```bash
H3:
 { qbit_total = 2, pauli_with_coef_s = { 'X0 Y1 ':3 + 0j, } }
```
### Rules of operation

* If the items are the same, the coefficients of the corresponding items are added
* Different items, different items and their coefficients are retained
* If the coefficient of an item is 0, delete the item

## Subtraction

[Here is API doc](@ref pyqpanda3.hamiltonian.hamiltonian.Hamiltonian.__sub__)

### Using rules

```
Hamiltonian - Hamiltonian
```

@add_toggle_python
    @snippet samples\python\Hamiltonian.py Hamiltonian - Hamiltonian
@end_toggle

Output
```bash
H3:
 { qbit_total = 2, pauli_with_coef_s = { 'X0 Y1 ':3 + 0j, } }
```
### Rules of operation

* If the items are the same, the coefficients of the corresponding items are subtracted
* If the items are different, different items and their coefficients will be retained
* If the coefficient of an item is 0, delete the item
  
## Scalar multiplication

[Here is API doc](@ref pyqpanda3.hamiltonian.hamiltonian.Hamiltonian.__mul__)

### Using rules

```
Hamiltonian * Complex
```
```
Complex * Hamiltonian
```

@add_toggle_python
    @snippet samples\python\Hamiltonian.py Hamiltonian * Hamiltonian
@end_toggle

Output
```bash
H4:
 { qbit_total = 2, pauli_with_coef_s = { 'X0 Y1 ':4.4 + 1j, } }
```

### Rules of operation

* Multiply each coefficient by a complex number
* If the coefficient of an item is 0, delete the item
## Multiplication

[Here is API doc](@ref pyqpanda3.hamiltonian.hamiltonian.Hamiltonian.__matmul__)

### Using rules

```
Hamiltonian * Hamiltonian
```

@add_toggle_python
    @snippet samples\python\Hamiltonian.py eg2 Hamiltonian * Hamiltonian
@end_toggle

Output
```bash
H3:
 { qbit_total = 2, pauli_with_coef_s = { 'X1 ':0 -1j, } }
```

### Rules of operation


#### Single-term and Single-term
Multiply a Hamiltonian object containing only a single term with another Hamiltonian object containing only a single term

(1) Consolidated into 1 item：

@add_toggle_python
    @snippet samples\python\Hamiltonian.py Consolidated into 1 item Hamiltonian * Hamiltonian
@end_toggle

Output
```bash
H3:
 { qbit_total = 4, pauli_with_coef_s = { 'X0 Y1 X2 Y3 ':1 + 0j, } }
```

(2) Pauli operators on the same qubit can evolve and the coefficients of the terms may be changed during the evolution

@add_toggle_python
    @snippet samples\python\Hamiltonian.py Hamiltonian * Hamiltonian evolution
@end_toggle

Output
```bash
H3:
 { qbit_total = 2, pauli_with_coef_s = { 'Y0 Y1 ':0 -1j, } }
```

(3) If the coefficient of an item is 0, delete the item

#### Multi-term and Multi-term
Multi-term Hamiltonian object multiply Multi-term Hamiltonian object

(1) Apply the allocation rate in a manner akin to polynomial multiplication

@add_toggle_python
    @snippet samples\python\Hamiltonian.py Apply the allocation rate in a manner akin to polynomial multiplication
@end_toggle

Output

```bash
H3:
 { qbit_total = 1, pauli_with_coef_s = { 'Y0 ':2.64 -2.31j, 'X0 ':2.42 + 2.52j, } }
```


(2) Apply the rules of addition, multiplication of single terms, and tensor product operations

## Tensor product

[Here is API doc](@ref pyqpanda3.hamiltonian.hamiltonian.Hamiltonian.tensor)

### Using rules

```
Hamiltonian tensor Hamiltonian
```

@add_toggle_python
    @snippet samples\python\Hamiltonian.py Hamiltonian tensor Hamiltonian
@end_toggle

Output
```bash
H3:
 { qbit_total = 4, pauli_with_coef_s = { 'X0 Y1 X2 Y3 ':1 + 0j, } }
```
### Rules of operation
   
The qbit numbers of the quantum system Q1 corresponding to the Pauli operator string H1 are 0...n-1; There is a Pauli operator A1 acting on the k1 qbit of Q1.

The qbit numbers of the Q2 quantum system corresponding to the Pauli string H2 are 0...m-1; There is a Pauli operator A2 acting on the k2 qbit of Q2.

After the execution of H3=H1.tensor(H2), the qbit number of the quantum system Q3 corresponding to H3 is 0...(m+n-1), where A1 acts on qbit k1 of Q3 and A2 acts on the (n+k2) qbit of Q3

Continue to apply the rules of addition, multiplication, and division

@add_toggle_python
    @snippet samples\python\Hamiltonian.py Hamiltonian tensor Hamiltonian Rules of operation
@end_toggle

Output
```bash
H3:
 { qbit_total = 7, pauli_with_coef_s = { 'Z0 Y3 Y6 ':1 + 0j, } }
```


# Application

## Calculate the expectation
Calculate the expectation of running quantum programs on quantum systems with Hamiltonian

### On CPUQVM

Calculate the expectation of running quantum programs on CPUQVM with Hamiltonian

@add_toggle_python
    @snippet samples\python\Hamiltonian.py Calculate the expectation on CPUQVM
@end_toggle

Output
```bash
without I:
 { qbit_total = 3, pauli_with_coef_s = { 'X0 Z1 ':2 + 0j, 'X1 Y2 ':3 + 0j, } }
res: 0.0
res2: 0.0
Warrning! This interface will be deprecated in the future. Please use the new interface expval_hamiltonian (a global interface exported by pyqpanda3.core).
```


### On GPUQVM

Calculate the expectation of running quantum programs on GPUQVM with Hamiltonian

@add_toggle_python
    @snippet samples\python\Hamiltonian.py Calculate the expectation on GPUQVM
@end_toggle

Output
```bash
without I:
 { qbit_total = 3, pauli_with_coef_s = { 'X0 Z1 ':2 + 0j, 'X1 Y2 ':3 + 0j, } }
res2: 0.0
```

### On QPU

Calculate the expectation of running quantum programs on QPU with Hamiltonian

Please watch [Quantum Cloud Service](#tutorial_qcloud_service) to learn more ablout originqc's cloud service

[API doc for QCloudBackend.expval_hamiltonian](@ref pyqpanda3.qcloud.QCloudBackend.expval_hamiltonian)

@add_toggle_python
    @snippet samples\python\Hamiltonian.py Calculate the expectation on QPU
@end_toggle

Output

@note The numerical values of the results will fluctuate.

```bash
res: -0.6217999999999999
```

### On Full Amplitude Simulator in originqc's cloud

Calculate the expectation of running quantum programs on QPU with Hamiltonian

Please watch [Quantum Cloud Service](#tutorial_qcloud_service) to learn more ablout originqc's cloud service

[API doc for QCloudBackend.expval_hamiltonian](@ref pyqpanda3.qcloud.QCloudBackend.expval_hamiltonian)

@add_toggle_python
    @snippet samples\python\Hamiltonian.py Calculate the expectation on Full Amplitude Simulator in originqc's cloud
@end_toggle

Output

```bash
res: -3.1
```

# Other

## Obtain the corresponding matrix.

[API doc for pyqpanda3.hamiltonian.Hamiltonian.matrix](@ref pyqpanda3.hamiltonian.hamiltonian.Hamiltonian.matrix)

```python
from pyqpanda3.hamiltonian import Hamiltonian
 
# prepare Hamiltonian object
op = Hamiltonian(pauli_with_coef_s = {
    'X0 ':0.373 + 0j, 'Y0 ':-0 -0.24026j, 'X0 Z1 ':-0.176981 + 0j, 'Y0 Z1 ':0 + 0.139393j,
    'X1 ':0.678529 + 0j, 'Z0 X1 ':0.052562 + 0j, 'Y1 ':-0 -0.164005j, 'Z0 Y1 ':0 + 0.153926j,
    'X0 X1 ':0.413656 + 0j, 'Y0 X1 ':-0 -0.134299j, 'X0 Y1 ':0 + 0.0292082j, 'Y0 Y1 ':-0.157953 + 0j,
    '':0.590014 + 0j, 'Z0 ':0.0761358 + 0j, 'Z1 ':0.142981 + 0j, 'Z0 Z1 ':-0.0352695 + 0j, }
)
# Generate a matrix from a Hamiltonian object.
mat = op.matrix()
print('mat:\n',mat)
```

Output is
```bash
mat:
[[0.7738613+0.j 0.296886 +0.j 0.74117  +0.j 0.6766998+0.j]
[0.095152 +0.j 0.6921287+0.j 0.0921958+0.j 0.943898 +0.j]
[0.721012 +0.j 0.4192102+0.j 0.5584383+0.j 0.929634 +0.j]
[0.4665182+0.j 0.308036 +0.j 0.170328 +0.j 0.3356277+0.j]]
```

## Obtain PauliOperator object for simulating the Hamiltonian{#hamiltonian_pauli_operator}

[API doc for pyqpanda3.hamiltonian.Hamiltonian.pauli_operator](@ref pyqpanda3.hamiltonian.hamiltonian.Hamiltonian.pauli_operator)

```python
from pyqpanda3.hamiltonian import Hamiltonian

ham = Hamiltonian({ "X0 Z1 ":2 + 0j, "X1 Y2 ":3 + 0j, })
pauli = ham.pauli_operator()
cir_coef_s = pauli.to_qcircuits()
for cir,coef in cir_coef_s:
    print('coef:',coef)
    print('cir:',cir.originir())
```
Output is
```bash
coef: (2+0j)
cir: QINIT 3
CREG 1
X q[0]
Z q[1]
I q[2]

coef: (3+0j)
cir: QINIT 3
CREG 1
I q[0]
X q[1]
Y q[2]
```