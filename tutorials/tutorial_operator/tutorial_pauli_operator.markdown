PauliOperator  {#tutorial_table_of_content_PauliOperator}
=============================================================================

[TOC]

@prev_tutorial{tutorial_quantum_visualization}
@next_tutorial{tutorial_variational_quantum_circuit}

-------------------------------------------------------------------------------------------------------------------------------

# Introduction

Pauli operator is an important concept in quantum mechanics, with a wide range of application prospects. The mathematical form of the Pauli operator is a matrix, usually represented as S = (Sx, Sy, Sz), where Sx, Sy, Sz represent the components of spin in the x, y, and z directions, respectively. These four components, including a unit vector, can be easily applied to calculations and derivations in quantum mechanics. Specifically, the Pauli operators are a set of three 2×2 unitary matrices
The Hermitian matrix is also known as the unitary matrix. These four components correspond to the quantum logic gate X (x-direction component), the quantum logic gate Y (y-direction component), the quantum logic gate Z (z-direction component), and the quantum logic gate I unit vector.

To distinguish between the X, Y, Z gates and the combination of these four gates, we refer to these four gates as the basic Pauli operators, and the combination of these four gates as the Pauli operator combination. The combination of Pauli operators can express complex physical concepts. For example, the Hamiltonian can be simulated using a combination of basic Pauli operators.

The lower part uses \f$\sigma_X\f$ \f$\sigma_Y\f$ \f$\sigma_Z\f$ \f$\sigma_I\f$ to represent the unitary matrices corresponding to X, Y, Z, and I, respectively.

# Unitary matrix
The unitary matrix of the Pauli operator in the basis



\f[
\sigma_X=
\begin{bmatrix}
0 & 1 \\
1 & 0
\end{bmatrix}
\f]


\f[
\sigma_Y=
\begin{bmatrix}
0 & -i \\
i & 0
\end{bmatrix}
\f]

\f[
\sigma_Z=
\begin{bmatrix}
1 & 0 \\
0 & -1
\end{bmatrix}
\f]

\f[
\sigma_I=
\begin{bmatrix}
1 & 0 \\
0 & 1
\end{bmatrix}
\f]

# Matrix multiplication
Matrix multiplication operation between basic Pauli operators

The \f$=\f$ below represents the equal sign in mathematical formulas

## XX,YY,ZZ

The Pauli operator multiplied by itself yields the identity matrix

\f[
\sigma_X\sigma_X=\sigma_I
\f]

\f[
\sigma_Y\sigma_Y=\sigma_I
\f]

\f[
\sigma_Z\sigma_Z=\sigma_I
\f]

## X/Y/Z and I
The value of the Pauli operator multiplied by the identity matrix remains unchanged, whether it is multiplied by the left or right

\f[
\sigma_X\sigma_I=\sigma_I\sigma_X=\sigma_X
\f]

\f[
\sigma_Y\sigma_I=\sigma_I\sigma_Y=\sigma_Y
\f]

\f[
\sigma_Z\sigma_I=\sigma_I\sigma_Z=\sigma_Z
\f]

## X/Y/Z and X/Y/Z
Multiplication between X, Y, Z

\f[
\sigma_X\sigma_Y=i\sigma_Z
\f]

\f[
\sigma_Y\sigma_X=-i\sigma_Z
\f]

\f[
\sigma_X\sigma_Z=-i\sigma_Y
\f]

\f[
\sigma_Z\sigma_X=i\sigma_Y
\f]

\f[
\sigma_Y\sigma_Z=i\sigma_X
\f]

\f[
\sigma_Z\sigma_Y=-i\sigma_X
\f]
 


# Simulation of Pauli Operator 

QPanda3 uses the PauliOperator class to simulate basic Pauli operators and their combinations. To simulate the way Pauli operators act on quantum systems, the PauliOperator class assigns a qubit index to each of the basic Pauli operators. Each PauliOperator object can be expressed using a quantum circuit such as QCircuit in QPanda3.

## For X/Y/Z/I
Simulation of the Pauli operator for the foundation

[Here is API doc](@ref pyqpanda3.hamiltonian.hamiltonian.PauliOperator.__init__)

Simulation of Pauli X

@add_toggle_python
    @snippet samples\python\Operator\PauliOperator.py Example of testing simulation of puali X using PauliOperator
@end_toggle

 Output
 ```bash
op:
 { qbit_total = 1, pauli_with_coef_s = { 'X0 ':1 + 0j, } }
 ```



Simulation of Pauli Y

@add_toggle_python
    @snippet samples\python\Operator\PauliOperator.py Example of testing simulation of puali Y using PauliOperator
@end_toggle

Output

```bash
op:
 { qbit_total = 1, pauli_with_coef_s = { 'Y0 ':1 + 0j, } }
```

Simulation of Pauli Z

@add_toggle_python
    @snippet samples\python\Operator\PauliOperator.py Example of testing simulation of puali Z using PauliOperator
@end_toggle

Output
```bash
op:
 { qbit_total = 1, pauli_with_coef_s = { 'Z0 ':1 + 0j, } }
```

Simulation of Pauli I

@add_toggle_python
    @snippet samples\python\Operator\PauliOperator.py Example of testing simulation of puali I using PauliOperator
@end_toggle

Output
```bash
op:
 { qbit_totql = 1, paulis = [ 'I', ], coefs = [ 1 + 0j, ] }
```


## For combination
Simulation of the combination of Pauli operators

[Here is API doc](@ref pyqpanda3.hamiltonian.hamiltonian.PauliOperator.__init__)

### From a dict like {"X0 Z1":2,"X1 Y2":3}
Constructs a PauliOperator from a dict containing operations and coefficients.

@add_toggle_python
    @snippet samples\python\Operator\PauliOperator.py Example of testing Method 1 of constructing PauliOperator
@end_toggle

Output
```bash
op:
 { qbit_totql = 3, paulis = [ 'XZI', 'IXY', ], coefs = [ 2 + 0j, 3 + 0j, ] }
```
### From a list like [("XXZ", [0, 1, 4], 1 + 2j), ("ZZ", [1, 2], -1 + 1j)]
Constructs a PauliOperator from a list containing operators, indices, and coefficients.

@add_toggle_python
    @snippet samples\python\Operator\PauliOperator.py Example of testing Method 2 of constructing PauliOperator
@end_toggle

Output
```bash
op:
 { qbit_totql = 5, paulis = [ 'XXIIZ', 'IZZII', ], coefs = [ 1 + 2j, -1 + 1j, ] }
```

### From a real matrix
Generate a PauliOperator object from a real matrix

@add_toggle_python
    @snippet samples\python\Operator\PauliOperator.py Generate a PauliOperator object from a real matrix
@end_toggle

Output
```bash
op:
 { qbit_total = 2, pauli_with_coef_s = { 'X0 ':0.373 + 0j, 'Y0 ':-0 -0.24026j, 'X0 Z1 ':-0.176981 + 0j, 'Y0 Z1 ':0 + 0.139393j, 'X1 ':0.678529 + 0j, 'Z0 X1 ':0.052562 + 0j, 'Y1 ':-0 -0.164005j, 'Z0 Y1 ':0 + 0.153926j, 'X0 X1 ':0.413656 + 0j, 'Y0 X1 ':-0 -0.134299j, 'X0 Y1 ':0 + 0.0292082j, 'Y0 Y1 ':-0.157953 + 0j, '':0.590014 + 0j, 'Z0 ':0.0761358 + 0j, 'Z1 ':0.142981 + 0j, 'Z0 Z1 ':-0.0352695 + 0j, } }
```


# Operations
Addition, subtraction, multiplication, division, and tensor product operations

## Describing

For the convenience of description, some expressions are agreed upon here. These expressions will be used for explanation in the following.

### Terms of Pauli Operator
The __"X0 Z1"__ and __"X1 Y2"__ in __PauliOperator({"X0 Z1":2+1.j,"X1 Y2":3+1.j})__ are called terms of the PauliOperator object;

### Equivalence

The equivalence of Pauli operator terms

* Essential condition 1: The quantum systems corresponding to the two Pauli operator terms are identical, which is manifested as the qbits in the quantum system being completely identical
* Essential condition 2: The two Pauli operator terms have the same effect on the quantum system

For examples

* "X0 Z1" is equal to "X0 Z1" provided that the corresponding quantum systems are the same, both containing qbits with indices 0 and 1, and that the other qbits are also the same
* "X1 Y2" is equal to "I0 X1 Y2" if the corresponding quantum systems are the same, both containing qbits with indices 0, 1, and 2, and other qbits are also the same

### Coefficient
Coefficients of Pauli operator terms

For __PauliOperator({“X0 Z1”:2+1.j,”X1 Y2”:3+1.j})__ , we call __2+1.j__ the coefficient of term __“X0 Z1”__ and __3+1.j__ the coefficient of term __“X1 Y2”__ .

## Addition

[Here is API doc](@ref pyqpanda3.hamiltonian.hamiltonian.PauliOperator.__add__)

### Using rules

```
PauliOperator + PauliOperator
```

@add_toggle_python
    @snippet samples\python\Operator\PauliOperator.py Example of testing Addition of PauliOperator
@end_toggle

Output
```bash
H3: { qbit_total = 2, pauli_with_coef_s = { 'X0 Y1 ':3 + 0j, } }
```

### Rules of operation

* If the items are the same, the coefficients of the corresponding items are added
* Different items, different items and their coefficients are retained
* If the coefficient of an item is 0, delete the item


## Subtraction

[Here is API doc](@ref pyqpanda3.hamiltonian.hamiltonian.PauliOperator.__sub__)

### Using rules

```
PauliOperator - PauliOperator
```

@add_toggle_python
    @snippet samples\python\Operator\PauliOperator.py Example of testing Subtraction of PauliOperator
@end_toggle

Output
```bash
H3: { qbit_total = 2, pauli_with_coef_s = { 'X0 Y1 ':-1 + 0j, } }
```
### Rules of operation

* If the items are the same, the coefficients of the corresponding items are subtracted
* If the items are different, different items and their coefficients will be retained
* If the coefficient of an item is 0, delete the item
  
## Scalar multiplication

[Here is API doc](@ref pyqpanda3.hamiltonian.hamiltonian.PauliOperator.__mul__)

### Using rules

```
PauliOperator * Complex
```
```
Complex * PauliOperator
```

@add_toggle_python
    @snippet samples\python\Operator\PauliOperator.py Example of testing Scalar multiplication of PauliOperator
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

[Here is API doc](@ref pyqpanda3.hamiltonian.hamiltonian.PauliOperator.__mul__)

### Using rules

```
PauliOperator * PauliOperator
```

@add_toggle_python
    @snippet samples\python\Operator\PauliOperator.py Example of testing Multiplication of PauliOperator
@end_toggle

Output
```bash
H3:
 { qbit_total = 2, pauli_with_coef_s = { '':1 + 0j, } }
```

### Rules of operation

#### Single-term and single-term
Multiply a PauliOperator object containing only a single term with another PauliOperator object containing only a single term

(1) Consolidated into 1 item：

@add_toggle_python
    @snippet samples\python\Operator\PauliOperator.py Example of testing Multiplication of PauliOperator(Consolidated into 1 item)
@end_toggle

Output
```bash
H3:
 { qbit_total = 4, pauli_with_coef_s = { 'X0 Y1 X2 Y3 ':1 + 0j, } }
```
(2) Pauli operators on the same qubit can evolve and the coefficients of the terms may be changed during the evolution

@add_toggle_python
    @snippet samples\python\Operator\PauliOperator.py Example of testing Multiplication of PauliOperator(Pauli operators on the same qubit can evolve and the coefficients of the terms may be changed during the evolution)
@end_toggle

Output
```bash
H3:
 { qbit_total = 2, pauli_with_coef_s = { 'Y0 Y1 ':0 -1j, } }
```
(3) If the coefficient of an item is 0, delete the item

#### Multi-term and Multi-term
Multi-term PauliOperator object multiply Multi-term PauliOperator object

(1) Apply the allocation rate in a manner akin to polynomial multiplication


@add_toggle_python
    @snippet samples\python\Operator\PauliOperator.py Example of testing Multi-term PauliOperator object multiply Multi-term PauliOperator object(Apply the allocation rate in a manner akin to polynomial multiplication)
@end_toggle

Output
```bash
H3:
 { qbit_total = 1, pauli_with_coef_s = { 'Y0 ':2.64 -2.31j, 'X0 ':2.42 + 2.52j, } }
```

(2) Apply the rules of addition, multiplication of single terms, and tensor product operations

## Tensor product

[Here is API doc](@ref pyqpanda3.hamiltonian.hamiltonian.PauliOperator.tensor)

### Using rules

```
PauliOperator tensor PauliOperator
```

@add_toggle_python
    @snippet samples\python\Operator\PauliOperator.py Example of testing Tensor product
@end_toggle

Output
```bash
H3:
 { qbit_total = 4, pauli_with_coef_s = { 'X0 Y1 X2 Y3 ':1 + 0j, } }
```

### Rules of operation
   
The qbit numbers of the quantum system Q1 corresponding to the Pauli operator string H1 are 0...n-1; There is a Pauli operator A1 acting on the k1 qbit of Q1.

The qbit numbers of the Q2 quantum system corresponding to the Pauli string H2 are 0...m-1; There is a Pauli operator A2 acting on the k2 qbit of Q2.

After the execution of H3=H1.tensor(H2), the qbit number of the quantum system Q3 corresponding to H3 is 0...(m+n-1),where A1 acts on qbit k1 of Q3 and A2 acts on the (n+k2) qbit of Q3

Continue to apply the rules of addition, multiplication, and division

@add_toggle_python
    @snippet samples\python\Operator\PauliOperator.py Example of testing Tensor product example
@end_toggle

Output
```bash
H3:
 { qbit_total = 7, pauli_with_coef_s = { 'Z0 Y3 Y6 ':1 + 0j, } }
```


# Application

## Calculate expectation

### On CPUQVM

Calculate the expectation of running quantum programs on quantum systems with Pauli operators

@add_toggle_python
    @snippet samples\python\Operator\PauliOperator.py Example of testing alculate the expectation of running quantum programs on quantum systems with Pauli operators
@end_toggle

Output
```bash
without I:
 { qbit_total = 3, pauli_with_coef_s = { 'X0 Z1 ':2 + 0j, 'X1 Y2 ':3 + 0j, } }
res: 0.0
res2: 0.0
Warrning! This interface will be deprecated in the future. Please use the new interface expval_pauli_operator (a global interface exported by pyqpanda3.core).
```

### On GPUQVM

Calculate the expectation of running quantum programs on quantum systems with Pauli operators,using GPUQVM

@add_toggle_python
    @snippet samples\python\Operator\PauliOperator.py Calculate expectation using GPUQVM
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

[API doc for QCloudBackend.expval_pauli_operator](@ref pyqpanda3.qcloud.QCloudBackend.expval_pauli_operator)

@add_toggle_python
    @snippet samples\python\Operator\PauliOperator.py Calculate the expectation on QPU
@end_toggle

Output

@note The numerical values of the results will fluctuate.

```bash
res: -0.6217999999999999
```

### On Full Amplitude Simulator in originqc's cloud

Calculate the expectation of running quantum programs on QPU with Hamiltonian

Please watch [Quantum Cloud Service](#tutorial_qcloud_service) to learn more ablout originqc's cloud service

[API doc for QCloudBackend.expval_pauli_operator](@ref pyqpanda3.qcloud.QCloudBackend.expval_pauli_operator)

@add_toggle_python
    @snippet samples\python\Operator\PauliOperator.py Calculate the expectation on Full Amplitude Simulator in originqc's cloud
@end_toggle

Output

```bash
res: -3.1
```

# Other

## Obtain the corresponding matrix.

[API doc for pyqpanda3.hamiltonian.PauliOperator.matrix](@ref pyqpanda3.hamiltonian.hamiltonian.PauliOperator.matrix)

```python
from pyqpanda3.hamiltonian import PauliOperator
 
# prepare PauliOperator object
op = PauliOperator(pauli_with_coef_s = {
    'X0 ':0.373 + 0j, 'Y0 ':-0 -0.24026j, 'X0 Z1 ':-0.176981 + 0j, 'Y0 Z1 ':0 + 0.139393j,
    'X1 ':0.678529 + 0j, 'Z0 X1 ':0.052562 + 0j, 'Y1 ':-0 -0.164005j, 'Z0 Y1 ':0 + 0.153926j,
    'X0 X1 ':0.413656 + 0j, 'Y0 X1 ':-0 -0.134299j, 'X0 Y1 ':0 + 0.0292082j, 'Y0 Y1 ':-0.157953 + 0j,
    '':0.590014 + 0j, 'Z0 ':0.0761358 + 0j, 'Z1 ':0.142981 + 0j, 'Z0 Z1 ':-0.0352695 + 0j, }
)
# Generate a matrix from a PauliOperator object.
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

## Retrieve internal data{#pauli_operator_reveal_internal_data}

### Method 1
```python
from pyqpanda3.hamiltonian import PauliOperator, Hamiltonian
from pyqpanda3.core import QCircuit
pauli_op = PauliOperator([("XYZ", [0, 1, 4], 1 + 2j), ("ZZ", [1, 2], -1 + 1j)])

def retrieve_internal_data_level_by_level(op: PauliOperator):
    for term in op.terms():
        # `term` corresponds to either ("XXZ", [0, 1, 4], 1 + 2j) or ("ZZ", [1, 2], -1 + 1j)
        # When `term` corresponds to ("XXZ", [0, 1, 4], 1 + 2j), `coef` corresponds to 1+2j, and `paulis` corresponds to {"XXZ", [0, 1, 4]}
        coef = term.coef()
        print('coef:', coef)
        paulis = term.paulis()
        for pauli_ in paulis:
            # When `pauli_` corresponds to Z and 4, `qbit` is 4, `pauli_char` is 'Z', `is_Z()` returns True, and `gate` is a Z gate acting on index 4
            qbit = pauli_.qbit()
            print('qbit:', qbit)
            pauli_char = pauli_.pauli_char()
            print('pauli_char:', pauli_char)
            if pauli_.is_I():
                print('is I')
            elif pauli_.is_X():
                print('is pauli X')
            elif pauli_.is_Y():
                print('is pauli Y')
            else:
                print('is pauli Z')
            gate = pauli_.to_qgate()
            cir = QCircuit()
            cir << gate
            print('cir ir:\n', cir.originir())

retrieve_internal_data_level_by_level(pauli_op)
```
Output is
```bash
coef: (1+2j)
qbit: 0
pauli_char: X
is pauli X
cir ir:
 QINIT 1
CREG 1
X q[0]

qbit: 1
pauli_char: Y
is pauli Y
cir ir:
 QINIT 2
CREG 1
Y q[1]

qbit: 4
pauli_char: Z
is pauli Z
cir ir:
 QINIT 5
CREG 1
Z q[4]

coef: (-1+1j)
qbit: 1
pauli_char: Z
is pauli Z
cir ir:
 QINIT 2
CREG 1
Z q[1]

qbit: 2
pauli_char: Z
is pauli Z
cir ir:
 QINIT 3
CREG 1
Z q[2]
```

### Method 2
```python
from pyqpanda3.hamiltonian import PauliOperator,Hamiltonian
from pyqpanda3.core import QCircuit
pauli_op = PauliOperator([("XYZ", [0, 1, 4], 1 + 2j), ("ZZ", [1, 2], -1 + 1j)] )

def to_qcircuit(op:PauliOperator):
    for term in op.terms():
        coef = term.coef()
        print('coef:', coef)
        cir = term.to_qcircuit()
        print('cir ir:\n',cir.originir())

to_qcircuit(pauli_op)
```
Output is
```bash
coef: (1+2j)
cir ir:
 QINIT 5
CREG 1
X q[0]
Y q[1]
I q[2]
I q[3]
Z q[4]

coef: (-1+1j)
cir ir:
 QINIT 5
CREG 1
I q[0]
Z q[1]
Z q[2]
I q[3]
I q[4]

```

### Method 3
```python
from pyqpanda3.hamiltonian import PauliOperator,Hamiltonian
from pyqpanda3.core import QCircuit
pauli_op = PauliOperator([("XYZ", [0, 1, 4], 1 + 2j), ("ZZ", [1, 2], -1 + 1j)] )

def to_qcircuits(op:PauliOperator):
    cir_with_coef_s = op.to_qcircuits()
    for cir,coef in cir_with_coef_s:
        print('coef:',coef)
        print('cir ir:\n',cir.originir())

to_qcircuits(pauli_op)
```
Output is
```bash
coef: (1+2j)
cir ir:
 QINIT 5
CREG 1
X q[0]
Y q[1]
I q[2]
I q[3]
Z q[4]

coef: (-1+1j)
cir ir:
 QINIT 5
CREG 1
I q[0]
Z q[1]
Z q[2]
I q[3]
I q[4]
```

### Method 4
```python
from pyqpanda3.hamiltonian import PauliOperator,Hamiltonian
from pyqpanda3.core import QCircuit
pauli_op = PauliOperator([("XYZ", [0, 1, 4], 1 + 2j), ("ZZ", [1, 2], -1 + 1j)] )

def to_hamiltonian_pq2(op:PauliOperator):
    data = op.to_hamiltonian_pq2()
    print(data)

to_hamiltonian_pq2(pauli_op)
```
Output is
```bash
[({0: 'X', 1: 'Y', 2: 'I', 3: 'I', 4: 'Z'}, (1+2j)), ({0: 'I', 1: 'Z', 2: 'Z', 3: 'I', 4: 'I'}, (-1+1j))]
```